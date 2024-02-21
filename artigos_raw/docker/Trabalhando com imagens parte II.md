# Trabalhando com imagens

## Comandos úteis do Dockerfile

### FROM

Já vimos que o comando `FROM` serve para selecionarmos uma imagem base.

### RUN

Também já vimos que o comando `RUN` nos permite fazer a execução de comandos no cotainer.

### WORKDIR

O comando `WORKDIR` serve para definir qual será nosso diretório de trabalho dentro do container.

Sendo assim podemos editar nosso dockerfile da seguinte maneira:

### COPY

O comando `COPY`serve para copiar um arquivo que está no meu computador para o container.

Sendo assim no mesmo diretório onde está o Dockerfile vamos criar um diretório novo chamado html e dentro dessa pasta vamos criar um arquivo chamado `index.html` com o seguinte conteúdo:

```html
<h2>It Works !</h2>
```

E então vamos editar o Dockerfile da seguinte forma:

```Dockerfile
FROM nginx:latest

WORKDIR /app

RUN apt-get update && \
    apt-get install vim -y

COPY html/ /usr/share/nginx/html
```

Após editar faça um novo build

```bash
docker build -t jorgerabello/nginxcomvim:latest .
```

E execute o container

```bash
docker run -it --name nginx_server -p 8080:80 jorgerabello/nginxcomvim bash

root@0a6b398e44a8:/app# cat /usr/share/nginx/html/index.html
<h2>It Works !</h2>

root@0a6b398e44a8:/app#
```

Note que por padrão estamos no diretório `/app` por causa do `WORKDIR`, a segunda coisa é que temos o vim instalado, a terceira coisa é que foi feita a cópia do arquivo index.html, conforme se pode ver.

### ENTRYPOINT e CMD

Todas as vezes em que executamos um container, é necessário executar algo para que o processo rode.

Vamos a um exemplo prático, crie um dockerfile com o seguinte conteúdo

```Dockerfile
FROM ubuntu:latest

CMD ["echo", "Hello World !"]
```

```bash
docker build -t jorgerabello/hello .

[+] Building 7.1s (6/6) FINISHED
....
```

E agora execute um container baseado nessa imagem

```bash
docker run --rm jorgerabello/hello

Hello World !
```

Veja que o `CMD` serviu para executar uma instrução ao rodar o nosso container.

No caso do nginx, provavelmente esse comando deve executar algo para deixar o servidor nginx rodando.

Porém o que o `CMD` vai executar pode ser substituído por meio de parâmetros, veja só:

```bash
docker run --rm jorgerabello/hello echo "mah oeeeeee"

mah oeeeeee
```

Veja que o comando de `echo "Hello World !"` foi substituído por `echo "mah oeeeeee"`.

Por isso que quando você executa

```bash
docker run -it --rm jorgerabello/hello bash

root@087780a5067b:/#
```

Você cai num terminal, pois o `CMD` substitui seja lá o que for pelo parâmetro, nesse caso `bash`.

Mas e se eu não quiser que o comando seja substituído ? Daí utilizaremos o `ENTRYPOINT`.

Edite o dockerfile da seguinte forma:

```Dockerfile
FROM ubuntu:latest

ENTRYPOINT [ "echo", "Hello " ]

CMD ["World !"]
```

E faça um build

```bash
docker build -t jorgerabello/hello .
```

Agora execute:

```bash
docker run --rm jorgerabello/hello

Hello  World !
```

```bash
docker run --rm jorgerabello/hello Jorge

Hello  Jorge
```

Note que o docker substituiu apenas a parte do `CMD`, a do `ENTRYPOINT` ficou fixa.

O `CMD` sempre vai ser um comando variável que entrará como parâmetro do `ENTRYPOINT`.

### ENTRYPOINT, CMD e EXEC

É possível ver como foi feito o dockerfile de uma imagem pronta.

Vamos tomar como base o [nginx](https://hub.docker.com/_/nginx)

Ao acessar uma imagem no docker hub e clicar sobre alguma tag, por exemplo na [latest](https://github.com/nginxinc/docker-nginx/blob/3591b5e431af710432bd4852d9ee26eb19992776/mainline/debian/Dockerfile) do nginx, você será redirecionado para o repositório onde está o Dockerfile dessa imagem.

No caso do nginx

```Dockerfile
#
# NOTE: THIS DOCKERFILE IS GENERATED VIA "update.sh"
#
# PLEASE DO NOT EDIT IT DIRECTLY.
#
FROM debian:bullseye-slim

LABEL maintainer="NGINX Docker Maintainers <docker-maint@nginx.com>"

ENV NGINX_VERSION   1.25.0
ENV NJS_VERSION     0.7.12
ENV PKG_RELEASE     1~bullseye

RUN set -x \
# create nginx user/group first, to be consistent throughout docker variants
    && addgroup --system --gid 101 nginx \
    && adduser --system --disabled-login --ingroup nginx --no-create-home --home /nonexistent --gecos "nginx user" --shell /bin/false --uid 101 nginx \
    && apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y gnupg1 ca-certificates \
    && \
    NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
    NGINX_GPGKEY_PATH=/usr/share/keyrings/nginx-archive-keyring.gpg; \
    export GNUPGHOME="$(mktemp -d)"; \
    found=''; \
    for server in \
        hkp://keyserver.ubuntu.com:80 \
        pgp.mit.edu \
    ; do \
        echo "Fetching GPG key $NGINX_GPGKEY from $server"; \
        gpg1 --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \
    done; \
    test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \
    gpg1 --export "$NGINX_GPGKEY" > "$NGINX_GPGKEY_PATH" ; \
    rm -rf "$GNUPGHOME"; \
    apt-get remove --purge --auto-remove -y gnupg1 && rm -rf /var/lib/apt/lists/* \
    && dpkgArch="$(dpkg --print-architecture)" \
    && nginxPackages=" \
        nginx=${NGINX_VERSION}-${PKG_RELEASE} \
        nginx-module-xslt=${NGINX_VERSION}-${PKG_RELEASE} \
        nginx-module-geoip=${NGINX_VERSION}-${PKG_RELEASE} \
        nginx-module-image-filter=${NGINX_VERSION}-${PKG_RELEASE} \
        nginx-module-njs=${NGINX_VERSION}+${NJS_VERSION}-${PKG_RELEASE} \
    " \
    && case "$dpkgArch" in \
        amd64|arm64) \
# arches officialy built by upstream
            echo "deb [signed-by=$NGINX_GPGKEY_PATH] https://nginx.org/packages/mainline/debian/ bullseye nginx" >> /etc/apt/sources.list.d/nginx.list \
            && apt-get update \
            ;; \
        *) \
# we're on an architecture upstream doesn't officially build for
# let's build binaries from the published source packages
            echo "deb-src [signed-by=$NGINX_GPGKEY_PATH] https://nginx.org/packages/mainline/debian/ bullseye nginx" >> /etc/apt/sources.list.d/nginx.list \
            \
# new directory for storing sources and .deb files
            && tempDir="$(mktemp -d)" \
            && chmod 777 "$tempDir" \
# (777 to ensure APT's "_apt" user can access it too)
            \
# save list of currently-installed packages so build dependencies can be cleanly removed later
            && savedAptMark="$(apt-mark showmanual)" \
            \
# build .deb files from upstream's source packages (which are verified by apt-get)
            && apt-get update \
            && apt-get build-dep -y $nginxPackages \
            && ( \
                cd "$tempDir" \
                && DEB_BUILD_OPTIONS="nocheck parallel=$(nproc)" \
                    apt-get source --compile $nginxPackages \
            ) \
# we don't remove APT lists here because they get re-downloaded and removed later
            \
# reset apt-mark's "manual" list so that "purge --auto-remove" will remove all build dependencies
# (which is done after we install the built packages so we don't have to redownload any overlapping dependencies)
            && apt-mark showmanual | xargs apt-mark auto > /dev/null \
            && { [ -z "$savedAptMark" ] || apt-mark manual $savedAptMark; } \
            \
# create a temporary local APT repo to install from (so that dependency resolution can be handled by APT, as it should be)
            && ls -lAFh "$tempDir" \
            && ( cd "$tempDir" && dpkg-scanpackages . > Packages ) \
            && grep '^Package: ' "$tempDir/Packages" \
            && echo "deb [ trusted=yes ] file://$tempDir ./" > /etc/apt/sources.list.d/temp.list \
# work around the following APT issue by using "Acquire::GzipIndexes=false" (overriding "/etc/apt/apt.conf.d/docker-gzip-indexes")
#   Could not open file /var/lib/apt/lists/partial/_tmp_tmp.ODWljpQfkE_._Packages - open (13: Permission denied)
#   ...
#   E: Failed to fetch store:/var/lib/apt/lists/partial/_tmp_tmp.ODWljpQfkE_._Packages  Could not open file /var/lib/apt/lists/partial/_tmp_tmp.ODWljpQfkE_._Packages - open (13: Permission denied)
            && apt-get -o Acquire::GzipIndexes=false update \
            ;; \
    esac \
    \
    && apt-get install --no-install-recommends --no-install-suggests -y \
                        $nginxPackages \
                        gettext-base \
                        curl \
    && apt-get remove --purge --auto-remove -y && rm -rf /var/lib/apt/lists/* /etc/apt/sources.list.d/nginx.list \
    \
# if we have leftovers from building, let's purge them (including extra, unnecessary build deps)
    && if [ -n "$tempDir" ]; then \
        apt-get purge -y --auto-remove \
        && rm -rf "$tempDir" /etc/apt/sources.list.d/temp.list; \
    fi \
# forward request and error logs to docker log collector
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log \
# create a docker-entrypoint.d directory
    && mkdir /docker-entrypoint.d

COPY docker-entrypoint.sh /
COPY 10-listen-on-ipv6-by-default.sh /docker-entrypoint.d
COPY 20-envsubst-on-templates.sh /docker-entrypoint.d
COPY 30-tune-worker-processes.sh /docker-entrypoint.d
ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 80

STOPSIGNAL SIGQUIT

CMD ["nginx", "-g", "daemon off;"]
```

Repare que o `ENTRYPOINT` é um shell script chamado
[docker-entrypoint.sh](https://github.com/nginxinc/docker-nginx/blob/3591b5e431af710432bd4852d9ee26eb19992776/mainline/debian/docker-entrypoint.sh)

E seu conteúdo é esse aqui:

```sh
#!/bin/sh
# vim:sw=4:ts=4:et

set -e

entrypoint_log() {
    if [ -z "${NGINX_ENTRYPOINT_QUIET_LOGS:-}" ]; then
        echo "$@"
    fi
}

if [ "$1" = "nginx" -o "$1" = "nginx-debug" ]; then
    if /usr/bin/find "/docker-entrypoint.d/" -mindepth 1 -maxdepth 1 -type f -print -quit 2>/dev/null | read v; then
        entrypoint_log "$0: /docker-entrypoint.d/ is not empty, will attempt to perform configuration"

        entrypoint_log "$0: Looking for shell scripts in /docker-entrypoint.d/"
        find "/docker-entrypoint.d/" -follow -type f -print | sort -V | while read -r f; do
            case "$f" in
                *.envsh)
                    if [ -x "$f" ]; then
                        entrypoint_log "$0: Sourcing $f";
                        . "$f"
                    else
                        # warn on shell scripts without exec bit
                        entrypoint_log "$0: Ignoring $f, not executable";
                    fi
                    ;;
                *.sh)
                    if [ -x "$f" ]; then
                        entrypoint_log "$0: Launching $f";
                        "$f"
                    else
                        # warn on shell scripts without exec bit
                        entrypoint_log "$0: Ignoring $f, not executable";
                    fi
                    ;;
                *) entrypoint_log "$0: Ignoring $f";;
            esac
        done

        entrypoint_log "$0: Configuration complete; ready for start up"
    else
        entrypoint_log "$0: No files found in /docker-entrypoint.d/, skipping configuration"
    fi
fi

exec "$@"
```

Repare que o `CMD` do nginx está assim:

```Dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

Ou seja os comandos necessários para subir o servidor nginx, porém todas as vezes em que executamos o container com exec e passamos `bash` por exemplo, esse comando que subiria o servidor é substituído pela execução de um shell bash.

### EXPOSE

Note que no dockerfile também tem uma instrução chamada `EXPOSE`

```Dockerfile
EXPOSE 80
```

Essa instrução serve para expor a porta 80 do nosso container, porém isso não quer dizer que há um bind da porta 80 do container para a porta 80 da minha máquina, somente significa que a porta 80 do container vai ficar exposta, sendo assim, ao executar esse container será necessário utilizar o parâmetro -p para realizar o bind.

## Exemplo Pŕatico

Para um exemplo prático vamos executar o seguinte

```bash
docker run --rm -it nginx bash

root@892ccb29d163:/# 
```

Veja que nesse caso aquele trecho do `CMD`  do Dockerfile do nginx

```Dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

Foi substituído pela execução do bash.

Agora se listarmos o conteúdo do diretório em que estamos, podemos ver que aquele shell script referenciado no `ENTRYPOINT` existe de fato no container, veja:

```bash
root@892ccb29d163:/# ls

bin   dev     docker-entrypoint.sh  home  lib32  libx32  mnt  proc  run   srv  tmp  var
boot  docker-entrypoint.d  etc    lib   lib64  media   opt  root  sbin  sys  usr
```

```Dockerfile
ENTRYPOINT ["/docker-entrypoint.sh"]
```

Sendo assim ao executar essa imagem do nginx o que acontece é a execução do shell script `docker-entrypoint.sh` recebendo os parâmetros `nginx -g daemon off;`, é mais ou menos como se você executasse o seguinte comando:

```bash
./docker-entrypoint.sh nginx -g daemon off;
```

Se você prestou atenção ou inspecionou o shell script `docker-entrypoint.sh`, notou que no final dele tem uma instrução, exatamente assim:

```sh
...
exec "$@"
```

Sempre que um shell script tem essa instrução significa que o shell script deverá aceitar os parâmetros passados depois da execução do shell script.

Vamos testar isso ! No shell do container execute o script assim:

```bash
root@892ccb29d163:/# ./docker-entrypoint.sh echo "HELLO"
HELLO

root@892ccb29d163:/# 
```

Veja que o parâmetro passado foi aceito, como era de se esperar.

## Publicando uma imagem no DockerHub

Para esse exemplo vamos customizar uma imagem do nginx

Para isso crie um diretório novo, e dentro dele crie um `Dockerfile` com o seguinte conteúdo:

```Dockerfile
FROM nginx:latest

COPY html /usr/share/nginx/html

ENTRYPOINT ["/docker-entrypoint.sh"]

CMD ["nginx", "-g", "daemon off;"]
```

Note que estamos usando a imagem base do nginx, e tmbm seu entrypoint e cmd.

Porém antes do entrypoint, estamos fazendo a cópia de um diretório chamado html.

Sendo assim vamos criar o diretório html e dentro dele vamos criar um arquivo index.html com o seguinte conteúdo:

```html
<h1>It Works !</h1>
```

Agora vamos fazer o build dessa imagem:

```bash
docker build -t jorgerabello/cnginx .   
```

Repare que a imagem foi construída:

```bash
❯ docker images         
REPOSITORY            TAG       IMAGE ID       CREATED              SIZE
jorgerabello/cnginx   latest    b1824e4c6baa   About a minute ago   187MB
```

Agora vamos executar essa imagem em um container:

```bash
docker run --rm -d -p 8080:80 jorgerabello/cnginx 

38b16e123dee14454a3e3edf3b85b8cb284a6cdf39509fe7ac8d54c917b83091
```

```bash
docker ps

CONTAINER ID   IMAGE                 COMMAND                  CREATED          STATUS          PORTS                                   NAMES
38b16e123dee   jorgerabello/cnginx   "/docker-entrypoint.…"   18 seconds ago   Up 18 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   sharp_rosalind
```

E se acessarmos o container num navegador ou via curl, agora temos uma imagem personalizada do nginx:

```bash
curl localhost:8080

<h1>It Works !</h1>

```

Agora, se você ainda não tiver crie uma conta no [DockerHub](https://hub.docker.com/).

Note que, na tag da minha imagem eu usei `jorgerabellodev/nome_da_imagem`, isso por que esse é meu usuário no dockerhub, no seu caso você deve utilizar o seu usuário.

O próximo passo é fazer login no docker hub, pra isso digite no seu terminal:

```bash
docker logout
```

```bash
docker login
```

Entre com seu usuário e senha do Docker Hub

Agora basta executar um `docker push` da seguinte forma:

```bash
docker push jorgerabello/cnginx:latest 
The push refers to repository [docker.io/jorgerabello/cnginx]
5e4e8fc78137: Pushed 
9e96226c58e7: Mounted from library/nginx 
12a568acc014: Mounted from library/nginx 
7757099e19d2: Mounted from library/nginx 
bf8b62fb2f13: Mounted from library/nginx 
4ca29ffc4a01: Mounted from library/nginx 
a83110139647: Mounted from library/nginx 
ac4d164fef90: Mounted from library/nginx 
latest: digest: sha256:89388bd2bd41c7354b1e173aec95daea06c1a47dc89c661b2e553ab46df8cad5 size: 1985
```

Ao terminar o push você poderá verificar a imagem no seu docker hub, no meu caso ficou [nesse endereço](https://hub.docker.com/repository/docker/jorgerabello/cnginx/general).
