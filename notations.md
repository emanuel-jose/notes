# Estudos NodeJS

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
