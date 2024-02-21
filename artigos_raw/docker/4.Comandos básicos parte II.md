# Comandos básicos

## Executando comandos em um container - `docker exec`

Agora temos um terceiro cenário.

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS         PORTS     NAMES
12837bc0b9d3   ubuntu    "bash"    14 minutes ago   Up 4 minutes             peaceful_cerf
```

Nesse caso, temos um container sendo executado, e se eu quiser acessar o shell dele ?

Então podemos executar o seguinte comando:

```bash
docker exec -it peaceful_cerf bash
```

E pronto temos acesso ao shell novamente.

```bash
root@12837bc0b9d3:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

root@12837bc0b9d3:/# uname -a
Linux 12837bc0b9d3 5.19.0-42-generic #43~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Fri Apr 21 16:51:08 UTC 2 x86_64 x86_64 x86_64 GNU/Linux

root@12837bc0b9d3:/#

```

## Publicando portas em um container

Nesse exemplo vamos utilizar uma imagem de container chamada nginx (Engine X).

Então vamos executar o nosso docker run:

```bash
docker run nginx

Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
f03b40093957: Already exists
eed12bbd6494: Pull complete
fa7eb8c8eee8: Pull complete
7ff3b2b12318: Pull complete
0f67c7de5f2c: Pull complete
831f51541d38: Pull complete
Digest: sha256:af296b188c7b7df99ba960ca614439c99cb7cf252ed7bbc23e90cfda59092305
Status: Downloaded newer image for nginx:latest
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/05/31 03:20:27 [notice] 1#1: using the "epoll" event method
2023/05/31 03:20:27 [notice] 1#1: nginx/1.25.0
2023/05/31 03:20:27 [notice] 1#1: built by gcc 10.2.1 20210110 (Debian 10.2.1-6)
2023/05/31 03:20:27 [notice] 1#1: OS: Linux 5.19.0-42-generic
2023/05/31 03:20:27 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/05/31 03:20:27 [notice] 1#1: start worker processes
2023/05/31 03:20:27 [notice] 1#1: start worker process 29
2023/05/31 03:20:27 [notice] 1#1: start worker process 30
2023/05/31 03:20:27 [notice] 1#1: start worker process 31
2023/05/31 03:20:27 [notice] 1#1: start worker process 32
2023/05/31 03:20:27 [notice] 1#1: start worker process 33
2023/05/31 03:20:27 [notice] 1#1: start worker process 34
2023/05/31 03:20:27 [notice] 1#1: start worker process 35
2023/05/31 03:20:27 [notice] 1#1: start worker process 36

```

Nesse caso nosso terminal ficará parado, então abra um novo terminal para seguirmos:

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS     NAMES
bb067c1ce9bd   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   80/tcp    friendly_ishizaka
```

Veja que agora temos uma informação nova, ali e `PORTS` podemos ver que temos `80/tcp`, o que significa que nosso container está acessível nessa porta.

Porém se você tentar acessar <http://localhost> ou <http://localhost:80> no seu navegador, vai ver que não será possível acessar o container.

Isso acontece por que o container (sendo ele um processo), pra ele a porta 80 está ativa, o que não significa que eu consigo acessar essa porta.

Sendo assim vamos remover esse container utilizando o comando `docker rm` com a opção `-f` para forçar a remoção:

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
bb067c1ce9bd   nginx     "/docker-entrypoint.…"   12 minutes ago   Up 12 minutes   80/tcp    friendly_ishizaka

```

```bash
docker rm -f friendly_ishizaka

friendly_ishizaka
```

Note mais uma vez que eu utilizei o nome do container localizado em `NAMES`.

```
docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

Agora vamos executar nosso container novamente, porém dessa vez vamos expor as portas dele, para que essas sejam acessíveis.

O docker faz isso utilzando a opção `-p` do comando `docker run`, essa opção faz com que aconteça um redirecionamento de portas permitindo que eu consiga acessar as portas do container, então vamos executar o seguinte:

```bash
docker run -p 8080:80 nginx

/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
....
2023/05/31 03:36:16 [notice] 1#1: start worker processes
2023/05/31 03:36:16 [notice] 1#1: start worker process 28
2023/05/31 03:36:16 [notice] 1#1: start worker process 29
2023/05/31 03:36:16 [notice] 1#1: start worker process 30
2023/05/31 03:36:16 [notice] 1#1: start worker process 31
2023/05/31 03:36:16 [notice] 1#1: start worker process 32
2023/05/31 03:36:16 [notice] 1#1: start worker process 33
2023/05/31 03:36:16 [notice] 1#1: start worker process 34
2023/05/31 03:36:16 [notice] 1#1: start worker process 35
```

Abra um novo terminal e execute um `docker ps`

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
b49e90588e47   nginx     "/docker-entrypoint.…"   34 seconds ago   Up 33 seconds   0.0.0.0:8080->80/tcp, :::8080->80/tcp   happy_moser

```

Veja que a informação disponível em `PORTS` agora é diferente, pois temos `0.0.0.0:8080->80/tcp, :::8080->80/tcp` o que significa que a porta 8080 da nossa máquina local está sendo redirecionada para a porta 80 do container.

Sendo assim agora se você acessar <http://localhost:8080> no seu navegador vai conseguir ter acesso ao nginx no container.

Inclusive, você pode testar isso tanto no navegador quanto via curl:

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

Outra coisa legal é que no terminal que ficou preso na execução do nginx (container) podemos ver os logs de acesso, veja:

```bash
docker run -p 8080:80 nginx
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
...
2023/05/31 03:36:16 [notice] 1#1: start worker process 28
2023/05/31 03:36:16 [notice] 1#1: start worker process 29
2023/05/31 03:36:16 [notice] 1#1: start worker process 30
2023/05/31 03:36:16 [notice] 1#1: start worker process 31
2023/05/31 03:36:16 [notice] 1#1: start worker process 32
2023/05/31 03:36:16 [notice] 1#1: start worker process 33
2023/05/31 03:36:16 [notice] 1#1: start worker process 34
2023/05/31 03:36:16 [notice] 1#1: start worker process 35

172.17.0.1 - - [31/May/2023:03:38:48 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36" "-"

...

172.17.0.1 - - [31/May/2023:03:39:13 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.81.0" "-"

```

Experimente dar uns Enters aí e tente acessar localhost:8080 e você verá que será registrado um log acesso.

No meu caso veja que foi identificado os dois acessos realizados, um com curl e outro com o firefox.

```bash
....
2023/05/31 03:36:16 [notice] 1#1: start worker process 35
172.17.0.1 - - [31/May/2023:03:38:48 +0000] "GET / HTTP/1.1" 200 615 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) 

172.17.0.1 - - [31/May/2023:03:41:16 +0000] "GET / HTTP/1.1" 200 615 "-" "curl/7.81.0" "-"

172.17.0.1 - - [31/May/2023:03:41:25 +0000] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/113.0.0.0 Safari/537.36" "-"

```

Nesse exemplo note que a execução do container prende o nosso terminal, o que não faz o menor sentido.

Sendo assim vamos matar esse container e subir ele novamente de uma outra forma.

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
b49e90588e47   nginx     "/docker-entrypoint.…"   8 minutes ago   Up 8 minutes   0.0.0.0:8080->80/tcp, :::8080->80/tcp   happy_moser
```

```bash
docker rm -f happy_moser

happy_moser
```

```bash
docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

## Liberando o terminal

Executando o comando `docker run` com a opção `-d` liberamos o terminal, esse `-d` significa detach, ou seja separar ou separado, significa que queremos executar esse container separado, a parte, do nosso terminal.

```bash
docker run -d -p 80:80 nginx

f59f72aee1ec7c498280fedcefb258e59a13957e853da0aa25638bf775a1693e

```

Note que o container está sendo executado normalmente em segundo plano

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                               NAMES
f59f72aee1ec   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 2 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp   nice_ptolemy
```

E que podemos acessa-lo na porta 80

```bash
curl localhost:80

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

## Parando e Removendo container

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                               NAMES
f59f72aee1ec   nginx     "/docker-entrypoint.…"   14 minutes ago   Up 14 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   nice_ptolemy
```

Note que estamos executando um container.

Se nesse momento eu der um docker stop passando o id ou o nome do do container

```bash
docker stop f59f72aee1ec

f59
```

E der um docker ps

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

Vai notar que não há nenhum container ativo, por tanto, se tentarmos acessar o nginx, não vamos conseguir

```bash
curl localhost:80

curl: (7) Failed to connect to localhost port 80 after 0 ms: Connection refused
```

Porém se você executar um `docker ps -a` vai ver que o container está lá, inativo

```bash
docker ps -a

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                          PORTS     NAMES
f59f72aee1ec   nginx     "/docker-entrypoint.…"   17 minutes ago   Exited (0) About a minute ago             nice_ptolemy

```

Note que é possível startar o container com o comando `docker start`, passando o id ou nome do container, veja:

```bash
docker start f59f72aee1ec

f59f72aee1ec

```

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS         PORTS                               NAMES
f59f72aee1ec   nginx     "/docker-entrypoint.…"   18 minutes ago   Up 3 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp   nice_ptolemy
```

```bash
curl localhost:80

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

Agora vamos parar novamente nosso container

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS         PORTS                               NAMES
f59f72aee1ec   nginx     "/docker-entrypoint.…"   27 minutes ago   Up 8 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   nice_ptolemy
```

```bash
docker stop f59f72aee1ec

f59f72aee1ec
```

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED

```

```bash
docker ps -a

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS                      PORTS     NAMES
f59f72aee1ec   nginx     "/docker-entrypoint.…"   28 minutes ago   Exited (0) 30 seconds ago             nice_ptolemy

## Sumário dos comandos utilizados
```

Nesse momento vamos imaginar que o container do nginx não é mais necessário, então queremos remover da lista do `-a`.

Para isso podemos executar o comando `docker rm` passando o id do container ou seu nome.

```bash
docker rm f59f72aee1ec

f59f72aee1ec
```

```
docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

Vamos executar nosso container de nginx novamente

```bash
docker run -d -p 80:80 nginx

9b25ffb5c1c15a41d7e8a064fd545442cb087ee301947de1a8e29e01c15a914a
```

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS        PORTS                               NAMES
9b25ffb5c1c1   nginx     "/docker-entrypoint.…"   2 seconds ago   Up 1 second   0.0.0.0:80->80/tcp, :::80->80/tcp   sweet_babbage
```

Se nesse momento tentarmos remover o container sem dar um `docker stop` veja o que acontece:

```bash
docker rm 9b25ffb5c1c1

Error response from daemon: You cannot remove a running container 9b25ffb5c1c15a41d7e8a064fd545442cb087ee301947de1a8e29e01c15a914a. Stop the container before attempting removal or force remove
```

Veja que o docker nos exibe uma mensagem de erro informando que não é possível remover um container que esteja rodando.

Na mensagem ainda diz que você tem duas opções, parar o container ou forçar a sua remoção.

Para forçar a remoção de um container, podemos utilizar o comando `docker rm` com a opção `-f`, da seguinte forma:

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                               NAMES
9b25ffb5c1c1   nginx     "/docker-entrypoint.…"   14 minutes ago   Up 14 minutes   0.0.0.0:80->80/tcp, :::80->80/tcp   sweet_babbage
```

```bash
docker rm -f 9b25ffb5c1c1

9b25ffb5c1c1
```

Veja que após a execução, não há mais containers, nem ativos nem inativos

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

```bash
docker ps -a

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

Nesse caso foi fácil, pois precisamos fazer a remoção de apenas um container, mas e se o cenário fosse de remover vários containers ? Poderíamos utilizar o comando `docker rm -f` para cada um passando seu respectivo id, porém isso seria bastante trabalhoso, sendo assim, podemos tirar proveito do shell do linux.

Para esse cenário vamos executar alguns containers:

```bash
docker run -itd ubuntu:latest

41537b8d11282b427cfe1889446234d597d2a3711d5fec55459828ae646e2c30
```

```bash
docker run -d -p 80:80 nginx

199c3817eaf522f6e4dad9ff121ad620fa64deeabd111a054f9cd68d85b301ef
```

```bash
 docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.
....
```

Vamos listar os container ativos

```bash
docker ps

CONTAINER ID   IMAGE           COMMAND                  CREATED          STATUS          PORTS                               NAMES
41537b8d1128   ubuntu:latest   "/bin/bash"              39 seconds ago   Up 38 seconds                                       fervent_rhodes
199c3817eaf5   nginx           "/docker-entrypoint.…"   48 seconds ago   Up 48 seconds   0.0.0.0:80->80/tcp, :::80->80/tcp   wonderful_elbakyan
```

Perceba que estamos executando 2 containers um ubuntu e um nginx e que temos um terceiro container inativo, veja:

```bash
docker ps  -a

CONTAINER ID   IMAGE           COMMAND                  CREATED              STATUS                          PORTS                               NAMES
41537b8d1128   ubuntu:latest   "/bin/bash"              About a minute ago   Up About a minute                                                   fervent_rhodes
199c3817eaf5   nginx           "/docker-entrypoint.…"   About a minute ago   Up About a minute               0.0.0.0:80->80/tcp, :::80->80/tcp   wonderful_elbakyan
3d84a60434f8   hello-world     "/hello"                 About a minute ago   Exited (0) About a minute ago                                       intelligent_bardeen
```

Agora quero forçar a remoção de todos os 3 de uma vez, para isso vamos entender um pouco mais sobre o comando `docker ps`.

Execute um `docker ps -a` e ele lista os containers ativos e inativos

```bash
docker ps  -a

CONTAINER ID   IMAGE           COMMAND                  CREATED              STATUS                          PORTS                               NAMES
41537b8d1128   ubuntu:latest   "/bin/bash"              About a minute ago   Up About a minute                                                   fervent_rhodes
199c3817eaf5   nginx           "/docker-entrypoint.…"   About a minute ago   Up About a minute               0.0.0.0:80->80/tcp, :::80->80/tcp   wonderful_elbakyan
3d84a60434f8   hello-world     "/hello"                 About a minute ago   Exited (0) About a minute ago                                       intelligent_bardeen
```

Se você executar um `docker ps -q` ele vai listar apenas os id's dos containers ativos

```bash
docker ps -q

41537b8d1128
199c3817eaf5
```

Sendo assim podemos combinar esses comandos para obter os id's dos container ativos e inativos:

```bash
docker ps -qa

41537b8d1128
199c3817eaf5
3d84a60434f8
```

E tendo esses id's podemos passar eles como parâmetro para o comando `docker rm -f`, da seguinte forma:

```bash
docker rm -f $(docker ps -qa)

41537b8d1128
199c3817eaf5
3d84a60434f8
```

Veja que não temos mais containers sendo executados.

```bash
docker ps -a

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```
