# Criando imagens pequenas com Docker

Nosso desafio aqui será criar uma imagem pequena que rode uma aplicação em go lang.

Nossa imagem não deve ultrapssar 2 MB

## Começando

Pra começar vamos consultar o site da Go Lang e ver como se faz um Hello World simples, podemos dar uma olhada [aqui](https://go.dev/doc/tutorial/getting-started).

O primeiro passo será criar uma pasta vamos chamar de essa pasta com o nome de `goapp`

```bash
mkdir goapp

cd goapp
```

Dentro dessa pasta vamos inicializar uma aplicação go

```bash
go mod init hello
```

Agora vamos criar um arquivo chamado `hello.go` com o seguinte conteúdo

```go
package main

import "fmt"

func main() {
 fmt.Println("It Works !!!")
}
```

E podemos executar nossa aplicação:

```bash
go run .

It Works !!!
```

## Criando a imagem para o container

Agora queremos criar uma imagem docker para o nosso container, sendo assim vamos criar um `Dockerfile`

```Dockerfile
FROM golang:1.21

WORKDIR /usr/src/app

COPY . .

RUN go mod download && go mod verify

RUN go build -v -o /usr/local/bin/app ./...

CMD ["app"]
```

E vamos buildar a nossa imagem

```bash
docker build -t jorgerabello/goapp .
```

Podemos conferir se a imagem foi criada

```bash
docker images                      
REPOSITORY               TAG       IMAGE ID       CREATED             SIZE
jorgerabello/goapp       latest    6dc78128ffa6   41 seconds ago      842MB
```

E executar a nossa imagem

```bash
docker run jorgerabello/goapp    
It Works !!!
```

Porém nossa imagem está muito grande ainda 842 MB e queremos que ela não ultrapasse os 2 MB, pra isso vamos contar com uma ferramenta do Docker chamada multi stage building.

## Multi Staging Build

O multi stage build nos permite construir nossa imagem com multiplos estágios, nossa imagem ficou grande pois tudo está incluso nela, então o que vamos fazer é separar o build da aplicação do seu container de execução.

Na imagem de execução que vai ser nossa imagem final, vai ter apenas o executável da aplicação e mais nada a imagem de build será descartada automaticamente após buildar.

Também vamos utilizar na imagem de build uma versão mais enxuta chamada alpine da linguagem go e na imagem de execução uma imagem base bem enxuta chamada scratch.

Então pra começar vamos alterar o nosso `Dockerfile`

```Dockerfile
FROM golang:1.21-alpine as builder

WORKDIR /usr/src/app

COPY . .

RUN go mod download && go mod verify

RUN go build -v -o /usr/local/bin/app ./...

FROM scratch

COPY --from=builder /usr/local/bin/app /usr/local/bin/app

CMD ["/usr/local/bin/app"]
```

Após a alteração vamos buildar novamente

```bash
docker build -t jorgerabello/goapp .
```

E podemos ver que o tamanho da imagem foi significativamente reduzido

```bash
docker images

REPOSITORY               TAG       IMAGE ID       CREATED              SIZE
jorgerabello/goapp       latest    b5aa99298dee   About a minute ago   1.8MB
```

Agora podemos conferir a execução

```bash
docker run jorgerabello/goapp                    

It Works !!!
```

Agora vamos colocar nossa imagem no registry do [docker hub](https://hub.docker.com/).

## Subindo nossa imagem no dockerhub

Primeiramente se você não tem uma conta no [docker hub](https://hub.docker.com/) criee uma.

E faça login na sua máquina utilizando seu usuário e senha do docker hub

```bash
docker login

Username:
Password:
```

Digite seu username do docker hub e sua senha.

Se tudo deu certo você deve ver a mensagem `Login Succeeded`

Agora apenas execute um push da imagem

```bash
docker push jorgerabello/goapp 
```

Feito isso provavelmente você já verá sua imagem publicada no docker hub.

**ATENÇÃO:** Quando você for buildar sua imagem e fazer o `docker push` substitua `jorgerabello` pelo seu usuário do dockerhub blz ?

## Distribuindo a imagem

Agora por fim vamos criar um `docker-compose.yaml` para que qualquer um possa utilizar a nossa imagem

**ATENÇÃO:** Lembre-se de alterar  `jorgerabello/goapp` para seu nome de usuário do docker hub.

```yaml
version: "3"

services:
  app:
    image: jorgerabello/goapp
    container_name: goapp
```

Agora qualquer um que tenha esse `docker-compose.yaml` pode baixar a imagem e executar o container.

Para baixar a imagem basta no diretório onde está o `docker-comppose.yaml` executar o comando:

```bash
docker-compose up -d
```

Podemos verificar as imagens

```bash
docker images                

REPOSITORY           TAG       IMAGE ID       CREATED          SIZE
jorgerabello/goapp   latest    b5aa99298dee   15 minutes ago   1.8MB
```

E para executar o container

```bash
docker run jorgerabello/goapp

It Works !!!
```
