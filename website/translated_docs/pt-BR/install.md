---
id: installation
title: "Instalação"
---

Verdaccio é uma aplicação web multiplataforma. Para instalá-lo, você precisa de alguns pré-requisitos.

#### Pré-requisitos

1. Node, acima da versão 
    - Para a versão `verdaccio@3.x`, o Node `v6.12` é a versão mínima suportada.
    - Para as versões `verdaccio@4.0.0-alpha.x` ou `verdaccio@4.x`, o Node `8.x` (LTS "Carbon") é a versão mínima suportada.
2. npm `>=5.x` or `yarn` > We highly recommend to use the latest Node Package Managers clients `> npm@6.x | yarn@1.x | pnpm@4.x`
3. A interface da web suporta os navegadores `Chrome, Firefox, Edge, and IE11`.

> Verdaccio suportará a versão mais recente do Node.js de acordo com as recomendações do [Node.js Release Working Group](https://github.com/nodejs/Release).

<div id="codefund">''</div>

## Instação

O `verdaccio` deve ser instalado globalmente usando um dos métodos a seguir:

Usando `npm`

```bash
npm install -g verdaccio
```

ou usando `yarn`

```bash
yarn global add verdaccio
```

![install verdaccio](assets/install_verdaccio.gif)

## Como Usar

Uma vez instalado, você só precisa executar o comando da CLI:

```bash
$> verdaccio
warn --- config file  - /home/.config/verdaccio/config.yaml
warn --- http address - http://localhost:4873/ - verdaccio/3.0.0
```

Para mais informações sobre a CLI, por favor [leia a seção sobre a cli](cli.md).

Você pode definir o registro usando o seguinte comando.

```bash
npm set registry http://localhost:4873/
```

ou você pode passar uma `--registry` flag quando necessário.

```bash
npm install --registry http://localhost:4873
```

## Imagem do Docker

`verdaccio` has an official docker image you can use, and in most cases, the default configuration is good enough. For more information about how to install the official image, [read the docker section](docker.md).

## Cloudron

`verdaccio` is also available as a 1-click install on [Cloudron](https://cloudron.io)

[![Instalação](https://cloudron.io/img/button.svg)](https://cloudron.io/button.html?app=org.eggertsson.verdaccio)