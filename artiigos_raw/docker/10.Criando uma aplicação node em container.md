# Criando uma aplicação node em container

## Objetivo

Criar uma aplicação node utilizando o docker

Nessa parte da nossa série de artigos sobre docker, vamos ver como desenvolver um software utilizando uma tecnologia, sem a necessidade de ter essa tecnologia instalada em nossa máquina, no nosso caso vamos utilizar como exemplo uma aplicação node.

## Disclaimer

Neste artigo **a aplicação é apenas um exemplo**, sendo assim não considerei fatores como separar responsabilidades e etc... outro ponto importante é que ao utilizar o COPY no Dockerfile abaixo, vamos copiar tudo inclusive o diretório `node_modules` que é enorme, em produção não fazemos dessa forma, porém o objetivo do artigo é simplesmente demonstrar como desenvolver aplicações utilizando um container, sobre otimização de imagens falaremos em outro artigo.

## Começando os trabalhos

Para inicio de conversa vamos criar ou escolher um diretório para trabalhar na nossa máquina, no meu caso eu escolhi `/~/Documents/artigos/aplicacao_node`

Agora vamos executar um container com node, porém vamos mapear um volume para o diretório acima, é importante que ao executar esse comando, você esteja dentro desse diretório:

```bash
docker run --rm -it -v $(pwd):/usr/src/app -p 3000:3000 node bash
root@8cab4b572964:/#
```

Vou também abrir o diretório `/~/Documents/artigos/aplicacao_node` no vscode (pode utilizar o editor que você preferir).

```bash
cd /~/Documents/artigos/aplicacao_node
```

```bash
code .
```

No container, vamos acessar o diretório do nosso volume

```bash
root@8cab4b572964:/# cd /usr/src/app/
```

Note que se você criar um arquivo aí dentro, esse arquivo deve aparecer no vscode

```bash
root@8cab4b572964:/usr/src/app# touch oi.txt

root@8cab4b572964:/usr/src/app# ls
oi.txt

root@8cab4b572964:/usr/src/app# rm oi.txt
```

## Iniciando o desenvolvimento

Para iniciar nossa aplicação node vamos executar um `npm init` e aceitar todas as opções

```bash
root@8cab4b572964:/usr/src/app# npm init
```

Note que foi criado um arquivo `package.json` que pode ser acessado pelo vscode

```bash
root@8cab4b572964:/usr/src/app# ls
package.json
```

Agora vamos instalar o express

```bash
root@8cab4b572964:/usr/src/app# npm install express --save
```

No vscode no diretório raiz crie um arquivo chamado `index.js` com o seguinte conteúdo:

```javascript
const express = require("express");
const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("<h1>It Works !</h1>");
});

app.listen(port, () => {
  console.log("Server running on port " + port);
});
```

Vamos testar se o nosso programa consegue ser executado

```bash
root@8cab4b572964:/usr/src/app# node index.js
Server running on port 3000
```

Se você abrir um navegador e testar a url <http://localhost:3000/> ou mesmo via curl verá que teremos nossa aplicação sendo executada

```bash
curl http://localhost:3000/

<h1>ItWorks!<h1>
```

Note que você está desenvolvendo dentro do container por tanto nesse caso nem precisaria ter o node instalado na sua máquina.

## Criando uma imagem

Agora que sabemos que nossa aplicação está estável, podemos criar uma imagem pra ela, então vamos criar um Dockerfile

```Dockerfile
FROM node:latest

WORKDIR /usr/src/app

COPY . .

EXPOSE 3000

CMD [ "node", "index.js" ]
```

Agora vamos fazer o build dessa imagem

```bash
docker build -t jorgerabello/sample-node-app:latest .
```

Agora vamos executar o um container utilizando essa imagem

```bash
docker run -p 3000:3000 jorgerabello/sample-node-app:latest
Server running on port 3000
```

Agora basta acessar ou no navegador ou via curl.

Por fim vamos fazer push da nossa imagem

```bash
docker push jorgerabello/sample-node-app:latest
```
