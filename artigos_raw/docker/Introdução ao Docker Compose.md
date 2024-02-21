# Introdução ao Docker Compose

## O que é o Docker Compose

O docker compose é uma ferramenta que baseado em um único script consegue automatizar a execução de vários containers.

Quando se trabalha com docker existe a necessidade de se executar muitos comandos, o que pode ser trabalhoso, o docker compose automatiza todo esse processo.

## Docker Compose YAML

Para se trabalhar com docker compose é necessário se ter um arquivo que deve obrigatoriamente se chamar `docker-compose.yaml` ou `docker-compose.yml` como queira.

A primeira coisa que deve ser declarada em nesse arquivo é a versão que estamos trabalhando, sendo assim vamos editar o `docker-compose.yml` e deixa-lo assim:

```yaml
version: "3"
```

Logo a seguir devemos declarar os nossos serviços e isso é feito com a palavra reservada `services`

```yaml
version: "3"

services:
```

Agora note que temos algumas imagens

```bash
docker images

REPOSITORY                    TAG       IMAGE ID       CREATED        SIZE
jorgerabello/custom-nginx     latest    c5d10cd2585b   47 hours ago   18MB
jorgerabello/custom-laravel   latest    570f005e37fc   47 hours ago   141MB
```

Sendo assim o primeiro serviço que vamos declarar vai se chamar `laravel`, vamos dizer que esse serviço utilizar a rede `minha_rede` e vamos dar um nome ao nosso container:

```yaml
version: "3"

services:
  laravel:
    image: jorgerabello/custom-laravel:latest
    container_name: laravel
    networks:
      - minha_rede
```

Também vamos declarar um outro serviço chamado nginx:

```yaml
version: "3"

services:
  laravel:
    image: jorgerabello/custom-laravel:latest
    container_name: laravel
    networks:
      - minha_rede

  nginx:
    image: jorgerabello/custom-nginx:latest
    container_name: nginx
    networks:
      - minha_rede
    ports:
      - 8080:80

networks:
  minha_rede:
    driver: bridge
```

Note que definimos também a rede que queremos utilizar.

Agora para executar o docker-compose no mesmo diretório onde se encontrar o arquivo execute `docker-compose up -d`

```bash
docker-compose up -d

Creating network "docker-compose_minha_rede" with driver "bridge"
Pulling laravel (jorgerabello/custom-laravel:latest)...
```

Assim como existe o comando docker ps

```bash
docker ps
CONTAINER ID   IMAGE                                COMMAND                  CREATED              STATUS              PORTS                                   NAMES
081ad406a2b5   jorgerabello/custom-laravel:latest   "docker-php-entrypoi…"   About a minute ago   Up About a minute   9000/tcp                                laravel
6582862295ad   jorgerabello/custom-nginx:latest     "nginx -g 'daemon of…"   About a minute ago   Up About a minute   0.0.0.0:8080->80/tcp, :::8080->80/tcp   nginx
```

Existe também o comando docker-compose -ps

```bash
docker-compose ps

 Name                Command              State                  Ports
--------------------------------------------------------------------------------------
laravel   docker-php-entrypoint php-fpm   Up      9000/tcp
nginx     nginx -g daemon off;            Up      0.0.0.0:8080->80/tcp,:::8080->80/tcp
```

Agora que vemos que os dois containers estão em execução podemos acessar <http://localhost:8080/>

Note como é mais fácil trabalhar com docker-compose no lugar de ficar digitando uma série de comandos do docker.

## Buildando imagens com docker-compose

Nesse momento estamos trabalhando com imagens fechadas, fixadas no docker-compose.yaml, porém nem sempre vamos trabalhar dessa forma, às vezes precisamos construir uma imagem conforme o código da aplicação avança e para essa finalidade podemos contar com o comando build.

O comando build aceita um parâmetro chamado `context` esse contexto simplesmente é o diretório onde se encontra o seu docker-file, devemos passar também para o contexto o nome do Dockerfile que será utilizado para construir essa imagem.

Considere que temos temos uma imagem do nginx e outra do laravel, mais ou menos o que já construímos [nesse artigo](https://www.tabnews.com.br/seujorge/otimizando-imagens-docker-com-multi-stage-building).

Então vamos construir o seguinte docker-compose.yaml:

```yaml
version: "3"

services:
  laravel:
    build:
      context: ./laravel
      dockerfile: Dockerfile
    image: jorgerabello/custom-laravel:latest
    container_name: laravel
    networks:
      - minha_rede

  nginx:
    build:
      context: ./nginx
      dockerfile: Dockerfile
    image: jorgerabello/custom-nginx:latest
    container_name: nginx
    networks:
      - minha_rede
    ports:
      - 8080:80

networks:
  minha_rede:
    driver: bridge
```

Nossa estrutura de diretórios deve estar mais ou menos assim:

```bash
.
├── docker-compose.yaml
├── laravel
│   └── Dockerfile
└── nginx
    ├── Dockerfile
    └── nginx.conf
```

Dessa forma estamos dizendo do docker-compose que ao fazer o build deve-se olhar dentro do diretório de contexto, procurar um Dockerfile e construir uma imagem com o nome especificado em `image:`

Para buildar uma nova versão da imagem, quando você alterar os arquivos podemos executar o comando `docker-compose up -d --build` essa opção `--build` deve buildar uma nova versão da imagem.

Para fins de exemplo, vamos olhar os containers que estão sendo executados

```bash
docker-compose ps

 Name                Command              State                  Ports
--------------------------------------------------------------------------------------
laravel   docker-php-entrypoint php-fpm   Up      9000/tcp
nginx     nginx -g daemon off;            Up      0.0.0.0:8080->80/tcp,:::8080->80/tc
```

Agora vamos parar todos os containers

```bash
docker-compose down

Stopping laravel ... done
Stopping nginx   ... done
Removing laravel ... done
Removing nginx   ... done
Removing network docker-compose_minha_rede
```

Agora vamos buildar as imagens, no mesmo diretório onde está o arquivo docker-compose.yml execute

```bash
docker-compose up -d --build

Creating network "docker-compose_minha_rede" with driver "bridge"
Building laravel
...
```

Agora podemos acessar o nginx <http://localhost:8080/> e ser redirecionados para a aplicação laravel.
