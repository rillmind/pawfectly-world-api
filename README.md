# pawfectly-world-api

API da Pawfectly World, rede social de adoção de animais, em NestJS + MongoDB.

## O que faz

API REST (porta `2306`) com cadastro e login de usuários (JWT), perfis de animais, posts e
pedidos de adoção. As rotas ficam em `src/<módulo>/<módulo>.controller.ts`: `/user`, `/auth`,
`/animal`, `/post` e `/adoption`. Quase todas exigem o token devolvido no cabeçalho
`Authorization` do `POST /auth`; algumas exigem o papel `admin` ou ser o dono do recurso.

## Por que existe

Foi o TCC do curso técnico em Informática do IFPE (2023), feito em dupla: eu fiz esta API e um
colega fez o front-end que a consome. O protótipo das telas está em
[pawfectly-world-design](https://github.com/rillmind/pawfectly-world-design) e a suíte de testes
de integração em [pawfectly-world-api-tests](https://github.com/rillmind/pawfectly-world-api-tests).

## Instalação

Pré-requisitos: Podman + podman-compose (ou Docker Compose). Para rodar fora do contêiner,
Node.js 20: a API não sobe no Node 26 (`TypeError: Cannot read properties of undefined (reading
'prototype')` no pacote `jwa`).

O `.env` versionado aponta para um cluster do MongoDB Atlas. Para usar um MongoDB local, suba o
banco e a API pelo compose, que troca o `ATLAS_URI` pelo contêiner `mongodb`:

```bash
git clone https://github.com/rillmind/pawfectly-world-api.git
cd pawfectly-world-api
export JWT_SECRET=troque-isto ENCRYPT_JWT_SECRET=troque-isto-tambem JWT_EXPIRES=1d
podman-compose up -d --build   # MongoDB 7 em :27017 e a API em :2306
```

Sem contêiner para a API (Node 20):

```bash
npm ci
podman-compose up -d mongodb
ATLAS_URI=mongodb://localhost:27017/pawfectly_world npm run start:dev
```

## Como testar

Não há testes unitários (`npm run test` não encontra nenhum `*.spec.ts`), e o `test/app.e2e-spec.ts`
é o do template do Nest: espera `Hello World!` em `GET /`, rota que esta API não tem.
`npm run build` confere que o projeto compila.

Para conferir à mão, com a API rodando:

```bash
curl -X POST localhost:2306/user -H 'content-type: application/json' \
  -d '{"nome":"Admin","username":"admin","email":"admin@email.com","senha":"123456"}'
curl -i -X POST localhost:2306/auth -H 'content-type: application/json' \
  -d '{"email_ou_username":"admin","senha":"123456"}'   # 200 e o token em Authorization
```

Os testes de integração ficam em
[pawfectly-world-api-tests](https://github.com/rillmind/pawfectly-world-api-tests). Eles precisam
de um usuário `admin` e da rota `GET /health`, que ainda não está neste repositório (há
`GET /user/health`, que exige login). Por isso a suíte para no `TestMain` com
`status-code: 404, want 200`.

---

<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

[circleci-image]: https://img.shields.io/circleci/build/github/nestjs/nest/master?token=abc123def456
[circleci-url]: https://circleci.com/gh/nestjs/nest

  <p align="center">A progressive <a href="http://nodejs.org" target="_blank">Node.js</a> framework for building efficient and scalable server-side applications.</p>
    <p align="center">
<a href="https://www.npmjs.com/~nestjscore" target="_blank"><img src="https://img.shields.io/npm/v/@nestjs/core.svg" alt="NPM Version" /></a>
<a href="https://www.npmjs.com/~nestjscore" target="_blank"><img src="https://img.shields.io/npm/l/@nestjs/core.svg" alt="Package License" /></a>
<a href="https://www.npmjs.com/~nestjscore" target="_blank"><img src="https://img.shields.io/npm/dm/@nestjs/common.svg" alt="NPM Downloads" /></a>
<a href="https://circleci.com/gh/nestjs/nest" target="_blank"><img src="https://img.shields.io/circleci/build/github/nestjs/nest/master" alt="CircleCI" /></a>
<a href="https://coveralls.io/github/nestjs/nest?branch=master" target="_blank"><img src="https://coveralls.io/repos/github/nestjs/nest/badge.svg?branch=master#9" alt="Coverage" /></a>
<a href="https://discord.gg/G7Qnnhy" target="_blank"><img src="https://img.shields.io/badge/discord-online-brightgreen.svg" alt="Discord"/></a>
<a href="https://opencollective.com/nest#backer" target="_blank"><img src="https://opencollective.com/nest/backers/badge.svg" alt="Backers on Open Collective" /></a>
<a href="https://opencollective.com/nest#sponsor" target="_blank"><img src="https://opencollective.com/nest/sponsors/badge.svg" alt="Sponsors on Open Collective" /></a>
  <a href="https://paypal.me/kamilmysliwiec" target="_blank"><img src="https://img.shields.io/badge/Donate-PayPal-ff3f59.svg"/></a>
    <a href="https://opencollective.com/nest#sponsor"  target="_blank"><img src="https://img.shields.io/badge/Support%20us-Open%20Collective-41B883.svg" alt="Support us"></a>
  <a href="https://twitter.com/nestframework" target="_blank"><img src="https://img.shields.io/twitter/follow/nestframework.svg?style=social&label=Follow"></a>
</p>
  <!--[![Backers on Open Collective](https://opencollective.com/nest/backers/badge.svg)](https://opencollective.com/nest#backer)
  [![Sponsors on Open Collective](https://opencollective.com/nest/sponsors/badge.svg)](https://opencollective.com/nest#sponsor)-->

## Description

API back-end desenvolvida com NestJS e TypeScript como projeto de conclusão do curso técnico em Informática. Sistema voltado ao mundo pet, com módulos de animais, adoção, autenticaçã e posts.
Utilizei o Supabse para armazenamento de imagens.  Aplicou arquitetura modular, injeção de dependência, MongoDB como banco de dados e boas práticas de API REST. Estrutura com src/ e test/ separados.
O projeto de api é pensado para ser consumida por um front-end feito por um colega do curso técnico que fez dupla comigo.

## Installation

```bash
$ npm install
```

## Running the app

```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode
$ npm run start:prod
```

## Test

```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```

## Support

Nest is an MIT-licensed open source project. It can grow thanks to the sponsors and support by the amazing backers. If you'd like to join them, please [read more here](https://docs.nestjs.com/support).

## Stay in touch

- Author - [Kamil Myśliwiec](https://kamilmysliwiec.com)
- Website - [https://nestjs.com](https://nestjs.com/)
- Twitter - [@nestframework](https://twitter.com/nestframework)

## License

Nest is [MIT licensed](LICENSE).
