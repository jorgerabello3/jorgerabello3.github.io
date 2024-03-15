# Instalando um Framework em um container Docker

## Objetivo

Este artigo tem por objetivo demonstrar como realizar a instalação de um framework em um container e gerar uma imagem personalizada do zero.

Para este exemplo vamos utilizar o laravel, porém o demonstrado aqui pode ser aplicada a quaisquer outros cenários semelhantes.

## Planejamento

Para iniciar os nossos trabalhos vamos executar um container PHP, da seguinte forma:

```bash
docker run -it --name php php:7.4-cli bash

root@5eccc7905844:/#
```

Uma vez executado o container a primeira coisa que vamos fazer é atualizar o sources de pacotes dando um apt-get update

```bash
root@5eccc7905844:/# apt-get update

Get:1 http://deb.debian.org/debian bullseye InRelease [116 kB]
Get:2 http://deb.debian.org/debian-security bullseye-security InRelease [48.4 kB]
Get:3 http://deb.debian.org/debian bullseye-updates InRelease [44.1 kB]
Get:4 http://deb.debian.org/debian bullseye/main amd64 Packages [8183 kB]
Get:5 http://deb.debian.org/debian-security bullseye-security/main amd64 Packages [246 kB]
Get:6 http://deb.debian.org/debian bullseye-updates/main amd64 Packages [14.8 kB]
Fetched 8652 kB in 1s (7523 kB/s)
Reading package lists... Done
```

O próximo passo vai ser definir em qual diretório vamos instalar o laravel, no nosso exemplo vamos instalar em /var/www

```bash
root@ea8bb49926c4:/# cd /var/www/
```

Agora vamos partir para a instalação do laravel, baseado [nessa documentação](https://laravel.com/docs/10.x), para facilitar o desenvolvimento, vamos utilizar o compose que é um tipo de gerenciador de pacotes. Para fazer a instalação do compose vamos utilizar [essa documentação](https://getcomposer.org/download/).

Sendo assim vamos executar os comandos necessários no nosso container:

```bash
root@ea8bb49926c4:/var/www# php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
```

```bash
root@ea8bb49926c4:/var/www# php -r "if (hash_file('sha384', 'composer-setup.php') ===
'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
Installer verified
```

```bash
root@ea8bb49926c4:/var/www# php composer-setup.php
All settings correct for using Composer
Downloading...

Composer (version 2.5.8) successfully installed to: /var/www/composer.phar
Use it: php composer.phar
```

```bash
root@ea8bb49926c4:/var/www# php -r "unlink('composer-setup.php');"
```

Podemos notar que após a instalação temos um arquivo chamado `composer.phar`

```bash
root@ea8bb49926c4:/var/www# ls
composer.phar  html
```

Podemos então executar o composer da seguinte forma:

```bash
root@ea8bb49926c4:/var/www# php composer.phar
   ______
  / ____/___  ____ ___  ____  ____  ________  _____
 / /   / __ \/ __ `__ \/ __ \/ __ \/ ___/ _ \/ ___/
/ /___/ /_/ / / / / / / /_/ / /_/ (__  )  __/ /
\____/\____/_/ /_/ /_/ .___/\____/____/\___/_/
                    /_/
Composer version 2.5.8 2023-06-09 17:13:21

Usage:
  command [options] [arguments]
   ....
```

É sabido que o composer para funcionar bem, necessita de uma lib chamada libzip, sendo assim vamos fazer a instalação dessa dependência:

```bash
root@ea8bb49926c4:/var/www# apt-get install libzip-dev -y
```

Também será necessário instalar a extensão zip do php

```bash
root@ea8bb49926c4:/var/www# docker-php-ext-install zip
```

Agora que temos o composer podemos utilizar ele para criar um projeto utilizando o laravel como framework, conforme [essa documentação](https://laravel.com/docs/10.x#your-first-laravel-project). Sendo assim vamos executar:

```bash
root@ea8bb49926c4:/var/www# php composer.phar create-project --prefer-dist laravel/laravel laravel
```

Ao final da instalação podemos verificar que o laravel está instalado:

```bash
...
> @php artisan key:generate --ansi
Application key set successfully.
```

```bash
root@ea8bb49926c4:/var/www# ls
composer.phar  html  laravel

root@ea8bb49926c4:/var/www# ls laravel/
README.md  artisan    composer.json  config    package.json  public routes     storage  vendor
app    bootstrap  composer.lock  database  phpunit.xml   resources server.php  tests    webpack.mix.js
```

## Dockerfile

Note que tudo que executamos até o momento são ações que podemos traduzir para um `Dockerfile`, sendo assim, como nosso objetivo é criar uma imagem do zero, vamos criar um Dockerfile e começar a edita-lo:

```Dockerfile
FROM php:7.4-cli

WORKDIR /var/wwww

RUN apt-get update && \
    apt-get install libzip-dev -y && \
    docker-php-ext-install zip

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');"

RUN php composer.phar create-project --prefer-dist laravel/laravel laravel
```

Note que cada comando que executamos anteriormente foi traduzido para uma layer da nossa imagem no Dockerfile.

O próximo passo será executar o laravel, mas antes vamos tentar buildar a nossa imagem, para isso abra um novo terminal e entre no diretório onde está o Dockerfile que criamos acima.

Agora execute o docker build e veja se não ocorrem erros:

```bash
docker build -t jorgerabello/custom-laravel:latest .
```

Ao final do processo você deve ter uma imagem gerada, mais ou menos assim:

```bash
docker images

REPOSITORY                    TAG       IMAGE ID       CREATED         SIZE
jorgerabello/custom-laravel   latest    37ef640b9ad0   4 seconds ago   554MB
```

Voltando ao nosso container de base, o laravel tem um programa chamado artisan, esse artisan nos ajuda a criar componentes de aplicação, no caso vamos utilizar o artisan para subir a nossa aplicação, sendo assim:

Entre no diretório do laravel:

```bash
root@ea8bb49926c4:/var/www# cd laravel/
```

Veja que temos o artisan nesse diretório

```bash
root@ea8bb49926c4:/var/www/laravel# ls

README.md  artisan    composer.json  config    package.json  public routes     storage  vendor
app    bootstrap  composer.lock  database  phpunit.xml   resources server.php  tests    webpack.mix.js
```

Vamos exeutar o artisan

```bash
root@ea8bb49926c4:/var/www/laravel# php artisan serve
Starting Laravel development server: http://127.0.0.1:8000
[Thu Jun 29 03:53:19 2023] PHP 7.4.33 Development Server (http://127.0.0.1:8000) started
```

Note que subimos um servidor na porta 8000.

Sabendo disso, queremos que ao executar um container com a nossa imagem, queremos que isso seja executado, além disso queremos que o acesso a porta 8000 seja liberado então vamos editar novamente nosso Dockerfile:

```Dockerfile
FROM php:7.4-cli

WORKDIR /var/wwww

RUN apt-get update && \
    apt-get install libzip-dev -y && \
    docker-php-ext-install zip

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');"

RUN php composer.phar create-project --prefer-dist laravel/laravel laravel

ENTRYPOINT [ "php", "laravel/artisan", "serve" ]
```

## ENTRYPOINT vs CMD

Porém se fizermos dessa forma não vamos conseguir acessar o laravel, isso por que se você prestou atenção ao executar o laravel, ele fica liberado para acesso apenas dentro do container, veja só:

```bash
root@ea8bb49926c4:/var/www/laravel# php artisan serve
Starting Laravel development server: http://127.0.0.1:8000
[Thu Jun 29 03:53:19 2023] PHP 7.4.33 Development Server (http://127.0.0.1:8000) started
```

Ou seja se eu for na minha máquina e tentar acessar não será possível.

Olhando o help da artisan, podemos notar que é possível passar um host e uma porta como parâmetros.

```bash
root@ea8bb49926c4:/var/www/laravel# php artisan serve --help
Description:
  Serve the application on the PHP development server

Usage:
  serve [options]

Options:
      --host[=HOST]     The host address to serve the application on [default: "127.0.0.1"]
      --port[=PORT]     The port to serve the application on
      --tries[=TRIES]   The max number of ports to attempt to serve from [default: 10]
      --no-reload       Do not reload the development server on .env file changes
  -h, --help            Display help for the given command. When no command is given display help for the list command
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi|--no-ansi  Force (or disable --no-ansi) ANSI output
  -n, --no-interaction  Do not ask any interactive question
      --env[=ENV]       The environment the command should run under
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug
```

Porém queremos que o host e a porta possam ser modificados quando alguém executar um container com a nossa imagem, então além do ENTRYPOINT vamos utilizar o CMD, então nosso Dockerfile vai ficar assim

```Dockerfile
FROM php:7.4-cli

WORKDIR /var/wwww

RUN apt-get update && \
    apt-get install libzip-dev -y && \
    docker-php-ext-install zip

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('sha384', 'composer-setup.php') === 'e21205b207c3ff031906575712edab6f13eb0b361f2085f1f1237b7126d785e826a450292b6cfd1d64d92e6563bbde02') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');"

RUN php composer.phar create-project --prefer-dist laravel/laravel laravel

ENTRYPOINT [ "php", "laravel/artisan", "serve" ]

CMD [ "--host=0.0.0.0" ]
```

Por padrão será passado `--host=0.0.0.0` mas isso poderá ser alterado via parâmetro, na prática quando a nossa imagem for executada em um container, teremos um comando mais ou menos assim: `php laravel/artisan serve --host=0.0.0.0`.

Vamos fazer novamente o build da nossa imagem e tentar subir um container com ela:

```bash
docker build -t jorgerabello/custom-laravel:latest .
```

```bash
docker run --rm --name custom-laravel -p 8000:8000 jorgerabello/custom-laravel

Starting Laravel development server: http://0.0.0.0:8000
[Thu Jun 29 04:10:42 2023] PHP 7.4.33 Development Server (http://0.0.0.0:8000) started
```

Se você tentar acessar no seu navegador <http://localhost:8000/> verá que agora o acesso ao laravel vai ser realizado com sucesso.

Agora como utilizar CMD podemos por exemplo querer ao invés de utilizar a porta 8000 querer utilizar a porta 8001, sendo assim poderíamos executar

```bash
docker run --rm --name custom-laravel-2 -p 8001:8001 jorgerabello/custom-laravel --host=0.0.0.0 --port=8001
```

Note que você agora tem 2 containers sendo executados

```bash
docker ps
CONTAINER ID    IMAGE                         COMMAND                  CREATED          STATUS          PORTS                                       NAMES
3158a14d1f1e    jorgerabello/custom-laravel   "php laravel/artisan…"   29 seconds ago   Up 29 seconds   0.0.0.0:8001->8001/tcp, :::8001->8001/tcp   custom-laravel-2
70e92d27ac9a    jorgerabello/custom-laravel   "php laravel/artisan…"   6 minutes ago    Up 6 minutes    0.0.0.0:8000->8000/tcp, :::8000->8000/tcp   custom-laravel
```

E que você pode acessar o laravel tanto no container que está executando na porta 8000 <http://localhost:8000/> como na 8801 <http://localhost:8001>.

## Publicando a imagem

Agora que sabemos que nossa imagem está estável, vamos subir ela no dockerhub

```bash
docker push jorgerabello/custom-laravel:latest
```
