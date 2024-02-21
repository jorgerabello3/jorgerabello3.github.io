# Trabalhando com imagens

Tudo que toca o assunto container está sempre ligado a imagens.

## Container registry

Temos um repositório de imagens público chamado [docker hub](https://hub.docker.com/) esse repositório é o que chamamos de `container registry`.

Um container registry pode ser ser público ou privado.

Sendo assim podemos encontar imagens no container registry, como por exemplo:

[A imagem hello-world(]<https://hub.docker.com/_/hello-world>), bem como a [ubuntu](https://hub.docker.com/_/ubuntu) que executamos nos artigos anteriores.

## Imagens locais

Para que um container seja executado o docker necessita baixar uma imagem para aquele container.

Podemos observar quais imagens já temos utilizando o comando `docker images`

```bash
docker images

REPOSITORY                 TAG       IMAGE ID       CREATED       SIZE
nginx                      latest    f9c14fe76d50   6 days ago    143MB
hello-world                latest    9c7a54a9a43c   3 weeks ago   13.3kB
ubuntu                     latest    3b418d7b466a   5 weeks ago   77.8MB
```

Também podemos remover as imagens utilizando o comando `docker rmi`

Por exemplo:

```bash
docker rmi hello-world

Untagged: hello-world:latest
Untagged: hello-world@sha256:fc6cf906cbfa013e80938cdf0bb199fbdbb86d6e3e013783e5a766f50f5dbce0
Deleted: sha256:9c7a54a9a43cca047013b82af109fe963fde787f63f9e016fdc3384500c2823d
Deleted: sha256:01bb4fce3eb1b56b05adf99504dafd31907a5aadac736e36b27595c8b92f07f1
```

```bash
docker images
REPOSITORY                 TAG       IMAGE ID       CREATED       SIZE
jorgerabello/todolistapi   latest    8503cdc59217   4 days ago    702MB
nginx                      latest    f9c14fe76d50   6 days ago    143MB
postgres                   latest    0c88fbae765e   7 days ago    379MB
ubuntu                     latest    3b418d7b466a   5 weeks ago   77.8MB
```

É possível excluir todas as imagens de uma só vez a exemplo do que fizemos com containers.

Podemos listar apenas os ID's das imagens com o comando `docker images -qa` e passar como argumento para o comando `docker rmi -f` que vai forçar a remoção das imagens.

A opção `-f` serve para forçar a remoção da imagem, caso algum container que utilize essa imagem esteja ativo.

```bash
docker rmi -f $(docker images -qa)
Untagged: jorgerabello/todolistapi:latest
Untagged: jorgerabello/todolistapi@sha256:df6d31eb314575d9032e440655615f2b667352b004b0e586bce182693b816722
Deleted: sha256:8503cdc59217b1a3cf941138577fb5ae9e56ed1b2e1315bdf60dcb420387967b
Untagged: nginx:latest
Untagged: nginx@sha256:af296b188c7b7df99ba960ca614439c99cb7cf252ed7bbc23e90cfda59092305
Deleted: sha256:f9c14fe76d502861ba0939bc3189e642c02e257f06f4c0214b1f8ca329326cda
Deleted: sha256:419f8948c50c723f2a5ac74428af3d804b5d0079d6df8f7f827663cf10cbc366
Deleted: sha256:1030aac4f1a8096ed58d3d4a2df55dd1b1b27d919ad156d97ad1f68081d0051a
Deleted: sha256:7d90b49d96c3036539ef144ecc27c01de03902d8ea166a0f7b77d11d3779c4bd
Deleted: sha256:551acb210764654af31b6cd51adaa74edc9a202587c3395fe0e9f95a2e097f8b
Deleted: sha256:3c530958db4c75c6fb409f339367aaf9a1e163c84718c035d4b09bebc83f43e7
Untagged: postgres:latest
Untagged: postgres@sha256:31c9342603866f29206a06b77c8fed48b3c3f70d710a4be4e8216b134f92d0df
Deleted: sha256:0c88fbae765e8bcdb4c8974c73279e9c00b3bb5ab7e23131e40706430ef2bb2a
Deleted: sha256:18b526b5dcb36f8962ee736668dfb46647d7dc79b660ea9f0639afb11e53cc4f
Deleted: sha256:eb2e757a9d2bde0b17f95399d28d05db02d36bbd62e8b92e52980f8d0164fc6c
Deleted: sha256:c431d77a4b1b6777d1358d0357bba6d7e8de41349db60a772ad631bca2521dbb
Deleted: sha256:71b42321830a8136a386ab80d93211edb05109ccb5c7eb6ff2396c0fd92af59d
Deleted: sha256:9bc07483aa2aaae52c3c9a0ed7a3e63b4b11d8f1d7c9a181b3beba28c2c8b66a
Deleted: sha256:6924221877a441f8b2c551145f9606232b2b3b77b375b677a6375bb4db8aefc1
Deleted: sha256:d330f39e9e4fdfd86f7963238d82e6381e91bed0b4b249a32832a840b84e68bc
Deleted: sha256:75d34a65af7ea089b2e7afdcb5c7ffe2a018d5b07fbb3f30b2af0a1f06c92196
Deleted: sha256:69f968c7f08a036baf94c6b3b450e64b0159b7bc268d2193fa2686ac04b6bed0
Deleted: sha256:a0934333ea426c54ef8c58732541c08b72777249a8f4c1c6d74f9888507db8d1
Deleted: sha256:e3eb62bb6c049fde27e8f2bb4f8c49c6b33bcfa6db410465a807b5a9c80d0f2d
Deleted: sha256:7f74048287a78fdf193e31119dd5be9100aeba35b7321c674ec6f083e11a0ce3
Deleted: sha256:8cbe4b54fa88d8fc0198ea0cc3a5432aea41573e6a0ee26eca8c79f9fbfa40e3
Untagged: ubuntu:latest
Untagged: ubuntu@sha256:dfd64a3b4296d8c9b62aa3309984f8620b98d87e47492599ee20739e8eb54fbf
Deleted: sha256:3b418d7b466ac6275a6bfcb0c86fbe4422ff6ea0af444a294f82d3bf5173ce74
Deleted: sha256:b8a36d10656ac19ddb96ef3107f76820663717708fc37ce929925c36d1b1d157
```

```bash
docker images

REPOSITORY   TAG       IMAGE ID   CREATED   SIZE

```

## Baixando uma imagem - docker pull

Se você quer apenas baixar uma imagem, sem executar um container, você pode fazer isso por meio do comando `docker pull`. Por exemplo:

```bash
docker images

REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
```

```bash
docker pull ubuntu

Using default tag: latest
latest: Pulling from library/ubuntu
dbf6a9befcde: Pull complete
Digest: sha256:dfd64a3b4296d8c9b62aa3309984f8620b98d87e47492599ee20739e8eb54fbf
Status: Downloaded newer image for ubuntu:latest
docker.io/library/ubuntu:latest

```

```bash
docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
ubuntu       latest    3b418d7b466a   5 weeks ago   77.8MB
```

## Criando uma imagem

Vamos criar um arquivo chamado Dockerfile e editar esse arquivo.

Um Dockerfile é um arquivo declarativo, que serve para que possamos criar nossas próprias imagens.

Imagina que nós queremos trabalhar com que aquela imagem do nginx, porém já queremos que o vim venha instalado.

Então vamos criar um dockerfile que vai partir do nginx e por meio do comando `RUN` vamos instalar o vim.

```Dockerfile
FROM nginx:latest

RUN apt-get update && apt-get install vim -y
```

Após escrever e salvar o Dockerfile, devemos fazer um build dessa imagem. Para isso vamos utilizar o comando `docker build`

A opção `-t` é de tag e serve para especificar o nome da nossa imagem.

Sendo assim vamos executar no mesmo diretório onde está o nosso Dockerfile

```bash
ls

drwxrwxr-x⠀Jorge Rabello⠀seujorge⠀4096⠀May 31 04:55:20⠀ﱮ⠀..
drwxrwxr-x⠀Jorge Rabello⠀seujorge⠀4096⠀May 31 04:55:27⠀ﱮ⠀.
-rw-rw-r--⠀Jorge Rabello⠀seujorge⠀53  ⠀May 31 04:55:45⠀⠀Dockerfile
```

```bash
docker build -t jorgerabello/nginxcomvim:latest .

[+] Building 5.6s (6/6) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                                      0.0s
 => => transferring dockerfile: 102B                                                                                                                                      0.0s
 => [internal] load .dockerignore                                                                                                                                         0.0s
 => => transferring context: 2B                                                                                                                                           0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                                                                                           0.5s
 => CACHED [1/2] FROM docker.io/library/nginx:latest@sha256:af296b188c7b7df99ba960ca614439c99cb7cf252ed7bbc23e90cfda59092305                                              0.0s
 => [2/2] RUN apt-get update && apt-get install vim -y                                                                                                                    4.7s
 => exporting to image                                                                                                                                                    0.3s
 => => exporting layers                                                                                                                                                   0.3s
 => => writing image sha256:0fbb0a7085c385195d42694d2f2f26aeacb2e5654daa59b055df65c20a20b26d                                                                              0.0s
 => => naming to docker.io/jorgerabello/nginxcomvim:latest                                                                                                                0.0s
```

Vamos entender esse comando:

- **docker build:** indica que vamos criar uma imagem

- **-t:** tag para dar o nome a tag da nossa imagem

- **jorgerabello/nginxcomvim:latest:** nome da tag ou nome da imagem, nesse caso coloquei jorgerabello na frente pois esse é meu usuário no [docker hub](https://hub.docker.com/).
- Já o . (ponto) serve para indicar onde está o arquivo chamado `Dockerfile`, nesse caso o ponto . representa o diretório atual.

Uma vez feito o build podemos verificar que a imagem foi gerada

```bash
docker images

REPOSITORY                 TAG       IMAGE ID       CREATED              SIZE
jorgerabello/nginxcomvim   latest    0fbb0a7085c3   About a minute ago   197MB
```

Agora se eu executar esse um container baseado nessa imagem

```bash
docker run -it --name nginx_server -p 8082:80 jorgerabello/nginxcomvim bash

root@58991e7aa21a:/#
```

Perceba que temos o vim instalado

````bash
docker run -it --name nginx_server -p 8082:80 jorgerabello/nginxcomvim 

root@58991e7aa21a:/# vim
````
