# Estudos NodeJS

## Índice

- [Fundamentos](#01---fundamentos)
  - [HTTP](#http)
  - [Memória](#memória)
  - [Streams](#streams)
  - [Async](#async)
  - [Buffers](#buffers)
  - [Middlewares](#middlewares)
  - [Route e Query Parameters](#route-e-query-parameters)
- [Rotas e HTTP](#02---rotas-e-http)
  - [Configurações iniciais](#configurações-iniciais)
  - [Banco de dados](#banco-de-dados)
  - [Migrations](#migrations)
  - [Env - Variáveis de Ambiente](#env---variáveis-de-ambiente)
  - [Planejamento de aplicações](#planejamento-de-aplicações)
  - [Cookies](#cookies)
  - [Testes Automatizados](#testes-automatizados)
  - [Deploy](#deploy)

## 01 - Fundamentos

- Iniciar Projeto: `npm init -y`, cria o arquivo `package.json`
- Importação `CommonJS => require`, padrão novo: `ESModules => import/export`
- Para usar as importação ESModules precisa adicionar no `package.json`: `"type": "module"`
- Por padrão módulos internos do node devem ser importados com a sigla node na frente do nome da lib, por exemplo: `import http from "node:http"`
- Para manter o server atualizando novas informações do código adicione o `--watch`, como por exemplo: `node --watch src/server.js`
- Para facilitar adicione nos scripts do `package.json` e rode `npm run dev`:

```json
"scripts": {
  "dev": "node --watch src/server.js "
},
```

### HTTP

- Rotas possuem dois fatores importantes: **Método HTTP** e **Url**
- **Métodos HTTP** tem cinco tipos semânticos e comumente usados:
  - **GET** : Buscar informações do back-end
  - **POST** : Criar uma nova informação no back-end
  - **PUT** : Atualizar um informação no back-end
  - **PATCH** : Atualizar uma informação especifica de um recurso no back-end
  - **DELETE** : Deletar uma informação do back-end

Exemplo prático de uso:

```
GET /users => Buscando usuário no back-end
POST /users => Criando novo usuário no back-end
```

- Cabeçalhos (Requisição/Resposta) => Metados: São informação para que tanto o backend e o front saiba lidar com a requisição da melhor forma.

- **HTTP Status Code**: Códigos númericos que servem de forma semântica para informar ao front o "status" da requisição, como por exemplo se faltou informação, se o servidor está fora do ar, se a rquisição foi bem sucedida.
- [HTTP Status Code - MDN]("https://developer.mozilla.org/en-US/docs/Web/HTTP/Status")

### Memória

- Stateful x Stateless:
  - Aplicações Stateful sempre tem informações guardadas em memória. (Depende de informações salvas na memória para que continue funcionando). Caso restarte a aplicação as informações seram perdidas (não aplicavel para projetos reais).
  - Stateless não guarda as informações em memória, é salvo informações e dispositivos externos como banco de dados, arquivos de texto, etc. Caso restarte a aplicação as informações salvas seram mantidas externamente então não tera alteração no funcionamento.

### Streams

> Ler pequenas partes e poder trabalhar com "aquilo" sem ter ele "carregado" por completo, assim como os streaming fazem como netflix e spotify.

- Exemplo prático: um usuário envia um arquivo csv de um 1gb contento 1 milhão de linhas, nesse caso o procedimento padrão seria enviar o arquivo para o servidor e o servidor leria todo o arquivo que demoraria muito tempo para depois ele adicionar esses dados a um tabela, no stream ele pode ler pequenas partes desse arquivo como por exemplo se caso o upload está a 10mb/s ele na prática le 10 mil linhas por segundo, durante esse leitura ao invés de ler todo o arquivo, no stream ele leria as 10 mil linhas iniciais e ja procesaria esses dados enquanto a leitura das demais linhas continuaria.

- **Redable Streams**: Lendo aos poucos. E nesse caso seria a leitura de um arquivo sendo enviado do front para o back como uma importação de um csv.

- **Writable Streams**: Enviando uma informação aos poucos. No caso de uma netflix aonde o backend está enviando aos poucos para o front dados.

- **Transform Streams**: Serve para "transformar" uma stream no meio do caminho, como alterar um valor númerico de positivo para negativo para depois ser escrito.

- **Duplex**: Basicamente as duas streams de `Redable` e `Writable` juntas.

> **Todas as portas de entrada e saida no node são streams**, assim como o `request` e o `response` do http.

- **req(Request)**: `ReadobleStream`
- **res(Response)**: `WritableStream`

### Async

> A forma no node para se trabalhar com arquivos completos, sem ler ele por partes é usando o `async:await`

- Exemplo: Leitura de uma resposta http que vem um json:

```json
{
  "name": "Fulano",
  "email": "fulano@example.com"
}
```

Na forma de leitura do `Stream` no node acabaria que um pedaço daquele arquivo já fosse escrito para ser processado, como por exemplo: `{ "name": "Fulano", "em`, dessa forma é impossivel trabalhar com esse arquivo, e por isso no caso de arquivos `JSON` é necessário usar o `async:await` para poder ler todo o arquivo e trabalhar com ele e guarda-lo em outro local se precisar antes de o restante do código ser executado, o código então executa de maneira **assíncrona**, ou seja sem ler por partes mas executar por completo um trecho de código antes de ir para outro.

### Buffers

> Buffer de forma simples, **_é uma representação de um espaço de memória do computaodr_**, usado especificamente para transitar dados de uma maneira muito rápida. Os dados armazendas para logo serem tratados ou enviados para algum lugar e depois removido de forma perfomatica.

- O `Buffer` no node é guardado de forma binária e por isso é mais perfomatico que um dado em `str` ou qualquer outro tipo de dado.

### Middlewares

> Interceptador: é uma função que interceptará uma requisição para tratar os dados tanto do request ou response.

### Route e Query Parameters

- **Query Parameters**: URL Stateful - Filtros, paginação, não-obrigatórios. Exemplo: `http://localhost:3333/users?userID=1&name=Diego`
- **Route Parameters**: Identificação de recurso. Exemplo: `http://localhost:3333/users/1`
- **Request Body**: Envio de informações de um formulário (HTTPs)

## 02 - Rotas e HTTP

### Configurações iniciais:

```bash
$ npm i -D typescript @types/node tsx
$ npx tsc --init
$
```

- Alterar o `target` do `ts.config` para uma das opções:

  - `ESNext`
  - `ES2023`

- Instalar o fastify:

```bash
$ npm i fastify
```

- Conversão do código com tsx, adicione no `package.json`:

```json
"dev": "tsx watch src/server.ts"
```

### Banco de dados

- Knex - Query Builder: facilita o uso de queries do sql para a linguagem usada,no caso do Knex faz a queries de acordo com métodos do js.

- Instalação Knex:

```bash
$ npm install knex sqlite3

```

- Configurando banco:

```ts
// database.ts
// criar pasta tmp

import { knex as setupKnex } from "knex";

export const knex = setupKnex({
  client: "sqlite",
  connection: {
    filename: "./tmp/app.db",
  },
});

// Uso inicial (teste), usando fastify
// server.ts:

import fastify from "fastify";
import { knex } from "./database";

const app = fastify();

app.get("/hello", async () => {
  const tables = await knex("sqlite_schema").select("*");

  return tables;
});

app
  .listen({
    port: 3333,
  })
  .then(() => {
    console.log("HTTP Server runing");
  });
```

### Migrations

> "Controle de versão" das tabelas. Histórico das mudanças feitas no banco de dados.

- Configuração para criar migrations com o knex:

```ts
// criar arquivo knexfile.ts/.js

import { config } from "./src/database";

export default config;

// alterar o database.ts para exportar as configurações:

import { knex as setupKnex } from "knex";

export const config = {
  client: "sqlite",
  connection: {
    filename: "./tmp/app.db",
  },
  useNullAsDefault: true,
};

export const knex = setupKnex(config);
```

- Adicionar script no package.json

```json
"knex": "node --import tsx ./node_modules/knex/bin/cli.js"

```

- Criação de migrations com o knex (com as configs usando ts e tsx):

```bash
$ npm run knex -- migrate:make migration-name
```

- A migrations possuem duas funções **`up`** e **`down`**, no caso da **`up`** ela indica o que a migration vai fazer no banco de dados, e **`down`** caso necessário dar um rollback, por exemplo na **`up`** ela cria uma tabela nova de `users` e na função **`down`** ela remove essa tabela `users`.

- Exemplo prático:

```ts
import type { Knex } from "knex";

export async function up(knex: Knex): Promise<void> {
  await knex.schema.createTable("transactions", (table) => {
    table.uuid("id").primary();
    table.text("title").notNullable();
  });
}

export async function down(knex: Knex): Promise<void> {
  await knex.schema.dropTable("transactions");
}
```

- Para executar todas as migrations:

```bash
$ npm run knex -- migrate:latest
```

- Para rverter a migration:

```bash
$ npm run knex -- migrate:rollback
```

### Env - Variáveis de Ambiente

> Usadas para guardar valores como chaves e urls para diferentes ambientes de desenvolvimento, como produção ou dev.

- É geralmente recomendado criar um arquivo `.env.example` para deixar exemplificada o que tem no `.env`, mas nem todas as informações são passadas, como senhas que não devem subir para o github.

- Para usar no node, baixe o pacote:

```bash
$ npm i dotenv
```

- Crie um arquivo `.env` para guardar as variaveis, para acessar pelo node use o `process`, como no exemplo:

```ts
import "dotenv/config";

const db = process.env.DATABASE_URL;
```

- Usando **`zod`** para validação das variaveis:

  - Crie um arquivo em `scr/env/index.ts`
  - Deixe a importação do dotenv nesse arquivo: `import "dotenv/config"`.
  - Configuração padrão:

  ```ts
  import "dotenv/config";
  import { z } from "zod";

  const envSchema = z.object({
    DATABASE_URL: z.string(),
    PORT: z.number().default(3333),
  });

  export const env = envSchema.parse(process.env);
  ```

  - Para tratar os erros de melhor forma, altere as linhas finais do código:

  ```ts
  // ...

  const _env = envSchema.safeParse(process.env);

  if (_env.success === false) {
    console.error("Invalid enviroment variable!", _env.error.format());

    throw new Error("Invalid enviroment variable!");
  }

  export const env = _env.data;
  ```

  ### Planejamento de aplicações

  - **RF (Requisitos Funcionais)**: São as funcionalidades que o sistema precisa ter para atender às necessidades do usuário.
    Exemplo:
    - O usuário deve poder criar uma conta.
    - O sistema deve enviar um e-mail de confirmação após o cadastro.
    - O cliente pode adicionar produtos ao carrinho e finalizar a compra.
  - **RN (Regras de Negócio)**: São as diretrizes ou condições específicas de como o sistema deve operar dentro do contexto do negócio.
    Exemplo:
    - Para pedidos acima de R$ 200,00, oferecer frete grátis.
    - Não permitir que menores de 18 anos comprem bebidas alcoólicas.
    - Se um cliente devolver um produto, o reembolso só será efetuado após a análise do item.
  - **RNF (Requisitos Não funcionais)**: São características de qualidade do sistema, como desempenho, segurança e usabilidade.
    Exemplo:
    - O sistema deve carregar a página inicial em até 2 segundos.
    - As informações do usuário devem ser criptografadas.
    - O site deve estar disponível 99,9% do tempo ao longo do ano.

### Cookies

> Formas de manter contexto entre requisições, no caso pode guardar informações de requisição de um usuário mesmo sem estar logado, gerando um id para a máquina e que caso ele logue na, esse histórico permaneça salvo na conta dele.

- Trabalhando com `cookies` com `fastify`:
- Instale:

```bash
$ npm i @fastify/cookie
```

- Adicione o plugin ao fastify, sendo o primeiro na ordem de registro:

```ts
import fastify from "fastify";
import cookie from "@fastify/cookie";
import { transactionsRoutes } from "./routes/transactions";

const app = fastify();

app.register(cookie);
app.register(transactionsRoutes, {
  prefix: "transactions",
});
```

- Criar um "`middleware`" global com `fastify` para ser executado em todas as rotas dentro do plugin:

```ts
app.addHook("preHandler", async (request, reply) => {
  console.log(`[${request.method}] ${request.url}`);
});

// ... resto das rotas

// app.get()
```

- Ou pode ser adiciona antes de registrar os puglins das rotas no `server.ts` para ser global para qualquer rota na aplicação.

### Testes Automatizados

- **Unitários**: Testam exclusivamente uma pequena parte isolada da aplicação, teste de uma função de soma por exemplo.
- **Integração**: Testa a comunicação de duas ou mais unidades, por exemplo fazer um testes de duas funções, uma função de soma que chama outra função que renderiza um gráfico com esses dados.
- **e2e/ponta a ponta**: Testes que simulam um usuário operando na aplicação.

- Utilizando Vitest, instalação:

```bash
$ npm i vitest -D
```

- Uso:

```ts
import { test } from "vitest";

test("o usuário consegue criar uma nova transação", () => {
  // fazer a chamada HTTP

  // validação
  expect(response.status).toEqual(201);
});
```

- Para rodas os testes:

```bash
$ npx vitest
```

- Ou criando um script para rodar `test`:

```bash
$ npm test
```

- Para poder testar uma rota sem usar o servidor real, instale:

```bash
$ npm i supertest -D
$ npm i @types/supertest -D
```

### Deploy

> Envio da aplicação para produção

- Serviço gerenciado usando `tsup`:

```bash
$ npm i tsup -D
```

- Crie o script no `package.json`, com a flag para criar a pasta `build` apm invés da padrão `dist`

```json
"build": "tsup src --out-dir build",
```
