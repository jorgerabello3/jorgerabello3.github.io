# Acessando arquivos e diretórios

## Acessando e alterando arquivos em um container

Para esse cenários vamos executar nosso container de nginx

```bash
docker run --name nginx_server -d -p 8080:80 nginx

ce127fe5bf09bc15faab2e98e9cd3eb101a916af9f125c2bd102ddb946658b8e
```

Note que dessa vez utilizamos o parâmetro `--name` que serve para especificarmos o nome do nosso container:

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED        STATUS        PORTS                                   NAMES
ce127fe5bf09   nginx     "/docker-entrypoint.…"   1 second ago   Up 1 second   0.0.0.0:8080->80/tcp, :::8080->80/tcp   nginx_server
```

Sendo assim, sabemos que podemos acessar uma página do nginx a partir da nossa máquina seja no navegador no endereço <http://localhost:8080/> ou por meio do curl:

```bash
curl localhost:8080

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

Bom, já sabemos que para executando comandos num container podemos utilizar o comando `docker exec`, por exemplo:

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
ce127fe5bf09   nginx     "/docker-entrypoint.…"   3 minutes ago   Up 3 minutes   0.0.0.0:8080->80/tcp, :::8080->80/tcp   nginx_server
```

```bash
docker exec nginx_server ls -la

total 88
drwxr-xr-x   1 root root 4096 May 31 04:56 .
drwxr-xr-x   1 root root 4096 May 31 04:56 ..
-rwxr-xr-x   1 root root    0 May 31 04:56 .dockerenv
drwxr-xr-x   2 root root 4096 May 22 00:00 bin
drwxr-xr-x   2 root root 4096 Apr  2 11:55 boot
drwxr-xr-x   5 root root  340 May 31 04:56 dev
drwxr-xr-x   1 root root 4096 May 24 22:43 docker-entrypoint.d
-rwxrwxr-x   1 root root 1616 May 24 22:43 docker-entrypoint.sh
drwxr-xr-x   1 root root 4096 May 31 04:56 etc
drwxr-xr-x   2 root root 4096 Apr  2 11:55 home
drwxr-xr-x   1 root root 4096 May 22 00:00 lib
drwxr-xr-x   2 root root 4096 May 22 00:00 lib64
drwxr-xr-x   2 root root 4096 May 22 00:00 media
drwxr-xr-x   2 root root 4096 May 22 00:00 mnt
drwxr-xr-x   2 root root 4096 May 22 00:00 opt
dr-xr-xr-x 423 root root    0 May 31 04:56 proc
drwx------   2 root root 4096 May 22 00:00 root
drwxr-xr-x   1 root root 4096 May 31 04:56 run
drwxr-xr-x   2 root root 4096 May 22 00:00 sbin
drwxr-xr-x   2 root root 4096 May 22 00:00 srv
dr-xr-xr-x  13 root root    0 May 31 04:56 sys
drwxrwxrwt   1 root root 4096 May 24 22:43 tmp
drwxr-xr-x   1 root root 4096 May 22 00:00 usr
drwxr-xr-x   1 root root 4096 May 22 00:00 var
```

Sendo assim vamos entrar no container acessando o seu shell

```bash
docker exec -it nginx_server bash

root@ce127fe5bf09:/#

```

Uma vez dentro do container (e notem como root) podemos fazer qualquer coisa.

Nesse caso vamos editar o html padrão que o nginx exibe, esse arquivo fica no diretório `/usr/share/nginx/html`

Então vamos navegar até esse diretório:

```bash
root@ce127fe5bf09:/# cd /usr/share/nginx/html/

root@ce127fe5bf09:/usr/share/nginx/html# pwd
/usr/share/nginx/html

root@ce127fe5bf09:/usr/share/nginx/html# ls
50x.html  index.html

root@ce127fe5bf09:/usr/share/nginx/html#
```

Repare que ao listar os aquivos temos `50x.html` e `index.html`. Vamos editar esse aquivo index !

Como o container é uma imagem muito leve, então ele vem sem algumas coisas, por exemplo sem um editor de textos simples como o vim ou vi.

Sendo assim vamos instalar o vim:

Primeiramente vamos atualizar a lista de repositórios:

```bash
root@ce127fe5bf09:/usr/share/nginx/html# apt-get update

Get:1 http://deb.debian.org/debian bullseye InRelease [116 kB]
Get:2 http://deb.debian.org/debian-security bullseye-security InRelease [48.4 kB]
Get:3 http://deb.debian.org/debian bullseye-updates InRelease [44.1 kB]
Get:4 http://deb.debian.org/debian bullseye/main amd64 Packages [8183 kB]
Get:5 http://deb.debian.org/debian-security bullseye-security/main amd64 Packages [245 kB]
Get:6 http://deb.debian.org/debian bullseye-updates/main amd64 Packages [14.8 kB]
Fetched 8650 kB in 6s (1555 kB/s)
Reading package lists... Done

root@ce127fe5bf09:/usr/share/nginx/html#
```

Agora vamos instalar o vim

```bash
root@ce127fe5bf09:/usr/share/nginx/html# apt-get install vim

Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libgpm2 vim-common vim-runtime xxd
Suggested packages:
  gpm ctags vim-doc vim-scripts
The following NEW packages will be installed:
  libgpm2 vim vim-common vim-runtime xxd
0 upgraded, 5 newly installed, 0 to remove and 0 not upgraded.
Need to get 8174 kB of archives.
After this operation, 36.9 MB of additional disk space will be used.
Do you want to continue? [Y/n] y

Get:1 http://deb.debian.org/debian bullseye/main amd64 xxd amd64 2:8.2.2434-3+deb11u1 [192 kB]
Get:2 http://deb.debian.org/debian bullseye/main amd64 vim-common all 2:8.2.2434-3+deb11u1 [226 kB]
Get:3 http://deb.debian.org/debian bullseye/main amd64 libgpm2 amd64 1.20.7-8 [35.6 kB]
Get:4 http://deb.debian.org/debian bullseye/main amd64 vim-runtime all 2:8.2.2434-3+deb11u1 [6226 kB]

....

update-alternatives: warning: skip creation of /usr/share/man/man1/editor.1.gz because associated file /usr/share/man/man1/vim.1.gz (of link group editor) doesn't exist
Processing triggers for libc-bin (2.31-13+deb11u6) ...

root@ce127fe5bf09:/usr/share/nginx/html#
```

Agora vamos editar o arquivo `index.html`

```bash
root@ce127fe5bf09:/usr/share/nginx/html# vim index.html

```

Para iniciar a edição do arquivo pressione a tecla I agora altere o arquivo, o meu ficou assim:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Meu Servidor !</title>
    <style>
      html {
        color-scheme: light dark;
      }
      body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
      }
    </style>
  </head>
  <body>
    <h1>It Works !</h1>
  </body>
</html>
```

Após a edição, pressione ESC para sair do modo de edição.

Pressione :wq e dê enter.

[Para saber mais sobre o editor de texto vim leia-me](https://www.alura.com.br/artigos/desvendando-o-editor-vim?gclid=CjwKCAjwvdajBhBEEiwAeMh1UyssV62GpgUdnkESmCfJocfyuq3wYFt3og6geAReg6dahptAN2XJwBoCqesQAvD_BwE)

Após editar o arquivo tente acessar o nginx novamente em <http://localhost:8080/>, seja no navegador ou no por meio de curl:

```bash
curl localhost:8080

<!DOCTYPE html>
<html>
<head>
<title>Meu Servidor !</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>It Works !</h1>
</body>
</html>
```

Agora vamos sair do container e remover ele:

```bash
root@ce127fe5bf09:/usr/share/nginx/html# exit
exit
```

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
ce127fe5bf09   nginx     "/docker-entrypoint.…"   19 minutes ago   Up 19 minutes   0.0.0.0:8080->80/tcp, :::8080->80/tcp   nginx_server
```

```bash
docker rm -f nginx_server

nginx_server
```

```bash
docker ps -a

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

Agora vamos subir um outro nginx

```bash
docker run --name nginx_server -d -p 8080:80 nginx

812ea61d0419b830b3235705cd6a8571852a8cbddc123bce7e8bf8f2ccfbd6cc
```

Agora tente acessar a página do nginx <http://localhost:8080/> via navegador ou curl

```bash
curl localhost:8080

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

Se você tentou acessar já viu que tudo aquilo que foi feito, todas as alterações feitas foram perdidas.

Isso por que os dados de um container morrem junto com ele, porém existe uma forma de persistir esses dados.

Vamos ver como resolver esse problema no próximo artigo onde vamos trabalhar com volumes.

## Trabalhando com volumes

Crie um diretório no seu computador, vamos chamar esse diretório de `html`

```bash
mkdir html
```

Entre nesse diretório e crie o arquivo `index.html`

```bash
cd html

touch index.html
```

Vamos colocar algum conteúdo nesse arquivo, pra evitar o uso do vim vamos simplesmente usar o echo

```bash
echo "<h1>It Works</h1>" >>  index.html

cat index.html

<h1>It Works</h1>

```

A ideia é que possamos mapear esse diretório html para o nosso container, sendo assim vamos executar o seguinte:

Vamos remover qualquer container que esteja em execução

```bash
docker rm -f $(docker ps -qa)

812ea61d0419
```

```bash
docker ps -a

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

Agora vamos subir novamente o nginx, porém dessa vez indicando pra ele um mapeamento de diretório com o parâmetro -v de `volume`.

```bash
docker run --name nginx_server -d -p 8080:80 -v ~/html:/usr/share/nginx/html nginx
```

Veja que estou apontando o diretório `/html` que está na minha home `/home/seujorge/html` e fazendo com que esse diretório esteja mapeado dentro do container no caminho `/usr/share/nginx/html`.

Assim sendo quando eu acessar o nginx <http://localhost:8080/> o html que vai ser exibido vai ser do arquivo `index.html` que criamos anteriormente.

```bash
curl localhost:8080

<h1>It Works</h1>
```

O mais legal é que se eu alterar esse arquivo na minha máquina local, imediatamente as alterações são refletidas no nginx.

A opção `-v` é capaz de criar diretório e arquivos, veja o seguinte:

Vamos remover os containers

```bash
docker rm -f $(docker ps -qa)
```

Agora certifique-se de que você está no diretório que contem o diretório html criado anteriormente

No meu caso eu crieei em `~/html`.

E vamos executar um container nginx

```bash
docker run --name nginx_server -d -p 8080:80 -v "$(pwd)"/html/x:/usr/share/nginx/html nginx
```

Veja que passamos na opção `-v` um diretório que não existia, porem se examinarmos o diretório html, vemos que o docker foi capaz de criar esse diretório x.

```bash
ls ~/html

-rw-rw-r--⠀Jorge Rabello⠀seujorge⠀61  ⠀May 31 02:37:34⠀⠀index.html
drwxr-xr-x⠀root         ⠀root    ⠀4096⠀May 31 02:46:57⠀⠀x/
```

Vamos remover nosso container

```bash
docker rm -f $(docker ps -qa)

8bde243dc020
```

E recria-lo, porém agora no lugar da opção `-v` vamos utilizar a opção `-bind`

```bash
docker run --name nginx_server -d -p 8080:80 --mount type=bind,source="$(pwd)"/html/x,target=/usr/share/nginx/html nginx

docker: Error response from daemon: invalid mount config for type "bind": bind source path does not exist: /home/seujorge/html/html/x.
See 'docker run --help'.
```

Veja que o docker reclama que o `bind source path` ou seja o caminho do source, no caso `$(pwd)"/html/x` não existe e o `--mount` é incapaz de criar.

Sendo assim, em se tratando de volumes em containers docker, há uma diferença entre utilizar `-v` e `--mount`, pois a segunda não é capaz de criar diretórios.

Até o momento trabalhamos com um tipo de volume chmado bind mount, onde podemos acessar documentos e diretórios mapeados entre a nossa máquina e o container. Apesar de ser uma forma de persistir dados, não é a única.

O docker conta com um sistema que chamamos de sistema de volumes, os volumes podem ser criados especificamente no docker.

Para acessar os volumes do docker podemos utilizar o comando `docker volume`, por exemplo:

```bash
docker volume ls
DRIVER    VOLUME NAME
```

Podemos criar um volume manualmente da seguinte forma:

```bash
docker volume create meuvolume

meuvolume
```

```bash
 docker volume ls

DRIVER    VOLUME NAME
local     meuvolume
```

Para saber mais detalhes sobre um volume podemos executar `docker volume inspect`

```bash
docker volume inspect meuvolume

[
    {
        "CreatedAt": "2023-05-31T02:58:01-03:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/meuvolume/_data",
        "Name": "meuvolume",
        "Options": null,
        "Scope": "local"
    }
]
```

Agora vamos criar um container utilizando o nosso volume:

```bash
docker run --name nginx_server -d -p 8080:80 --mount type=volume,source=meuvolume,target=/app nginx
```

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS                                   NAMES
481e6187fe52   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 1 second   0.0.0.0:8080->80/tcp, :::8080->80/tcp   nginx_server

```

Veja que se você acessar o container, agora você vai ver um diretório novo chamado `/app` que está sendo utilizado como volume.

```bash
docker exec -it nginx_server bash

root@481e6187fe52:/# ls
app  boot  docker-entrypoint.d  etc   lib    media  opt   root  sbin  sys  usr
bin  dev   docker-entrypoint.sh  home  lib64  mnt    proc  run  srv   tmp  var

root@481e6187fe52:/#

```

Vamos então criar um arquivo nesse diretório app

```bash
root@481e6187fe52:/# ls
app  boot  docker-entrypoint.d  etc   lib    media  opt   root  sbin  sys  usr
bin  dev   docker-entrypoint.sh  home  lib64  mnt    proc  run  srv   tmp  var

root@481e6187fe52:/# ls app/

root@481e6187fe52:/# touch app/oi.txt

root@481e6187fe52:/# ls app/
oi.txt

root@481e6187fe52:/#

```

Agora vamos sair do nosso container e criar um novo nginx

```bash
root@481e6187fe52:/# exit

exit
```

```bash
docker run --name segundo_nginx_server -d -p 8081:80 --mount type=volume,source=meuvolume,target=/app nginx

791660073bf619afb5e2611aea6a74188965b8015f0cd947719e9952d729155e
```

Note que eu mudei a porta para `8081` e o nome do container para `segundo_nginx_server`.

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
791660073bf6   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 1 second    0.0.0.0:8081->80/tcp, :::8081->80/tcp   segundo_nginx_server
481e6187fe52   nginx     "/docker-entrypoint.…"   5 minutes ago   Up 5 minutes   0.0.0.0:8080->80/tcp, :::8080->80/tcp   nginx_server
```

Vamos executar um bash no segundo nginx

```bash
docker exec -it segundo_nginx_server bash

root@791660073bf6:/# ls app/
oi.txt

root@791660073bf6:/#
```

Note que ao listar os arquivos em /app vemos o arquivo oi.txt. Mesmo estando em containers diferentes o volume continua sendo o mesmo.

Então estamos compartilhando o volume entre os nossos containers.

Ou seja as alterações no volume se refletem para ambos os containers.

Uma outra forma de acessar ou referenciar volumes é utilizando o opção `-v` e o nome do volume, observe:

```bash
docker run --name terceiro_nginx_server -d -p 8082:80 -v meuvolume:/app nginx

069f44f11069513f31fb8b4a108d23dafecff71df464aab41ca081da96f75c41
```

Veja que agora temos três containers compartilhando o mesmo volume

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
069f44f11069   nginx     "/docker-entrypoint.…"   5 seconds ago    Up 4 seconds    0.0.0.0:8082->80/tcp, :::8082->80/tcp   terceiro_nginx_server
791660073bf6   nginx     "/docker-entrypoint.…"   4 minutes ago    Up 4 minutes    0.0.0.0:8081->80/tcp, :::8081->80/tcp   segundo_nginx_server
481e6187fe52   nginx     "/docker-entrypoint.…"   10 minutes ago   Up 10 minutes   0.0.0.0:8080->80/tcp, :::8080->80/tcp   nginx_server
```

Note que mesmo no terceiro container temos o diretório app.

```bash
docker exec -it terceiro_nginx_server bash

root@069f44f11069:/# ls app/
oi.txt

root@069f44f11069:/#
```

Por fim podemos excluir os container

```bash
docker rm -f $(docker ps -qa)
069f44f11069
791660073bf6
481e6187fe52
```

E para os volumes podemos utilizar o comando `docker volume prune` para limpar os dados não utilizados em volumes

```bash
docker volume prune 

WARNING! This will remove anonymous local volumes not used by at least one container.
Are you sure you want to continue? [y/N] y
Total reclaimed space: 0B
```

Ou simplesmente remover o volume com o comando `docker volume rm`

```bash
docker volume ls

DRIVER    VOLUME NAME
local     meuvolume
```

```bash
docker volume rm meuvolume 

meuvolume
```

```bash
docker volume ls

DRIVER    VOLUME NAME
```
