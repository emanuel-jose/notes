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

## 01 - Fundamentos

- Iniciar Projeto: `npm init -y`, cria o arquivo `package.json`
- Importação `CommonJS => require`, padrão novo: `ESModules => import/export`
- Para usar as importação ESModules precisa adicionar no `package.json`: `"type": "module"`
- Por padrão módulos internos do node devem ser importados com a sigla node na frente do nome da lib, por exemplo: `import http from "node:http"`
- Para manter o server atualizando novas informações do código adicione o `--watch`, como por exemplo: `node --watch src/server.js`
- Para facilitar adicione nos scripts do `package.json` e rode `npm run dev`:

```
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

```
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
