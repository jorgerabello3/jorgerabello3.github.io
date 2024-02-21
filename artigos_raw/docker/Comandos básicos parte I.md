# Comandos básicos

Nesse artigo vamos utilizar os comandos básicos pra que você comece a sua jornada no Docker.

## Listando containers - `docker ps`

Este comando nos mostra quais containers estão em execução em nossa máquina atualmente.

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

Nesse caso não temos nenhum container sendo executado.

## Criando e executando containers - `docker run`

Comando utilizado para executar um container em nossa máquina

```bash
docker run hello-world

Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete
Digest: sha256:fc6cf906cbfa013e80938cdf0bb199fbdbb86d6e3e013783e5a766f50f5dbce0
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

Neste caso pedimos para que fosse executado um container chamado `hello-world` se prestarmos atenção algumas coisas aconteceram:

Vamos a elas:

1. Em primeiro lugar o docker nos informa que não existe uma imagem chamada `hello-world:latest` localmente, então ele vai baixar essa imagem fazendo um pull.

```bash
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
```

Sempre que você não especificar a versão da imagem, será utilizada a latest (a mais recente).

2. Aqui o download (pull) foi completado e o docker nos exibe o digest sha1 que é simplesmente uma chave nos informando o checksum da imagem

```bash
719385e32844: Pull complete
Digest: sha256:fc6cf906cbfa013e80938cdf0bb199fbdbb86d6e3e013783e5a766f50f5dbce0
Status: Downloaded newer image for hello-world:latest
```

3. Essa parte é simplesmente uma mensagem enviada a saída padrão do container, exibindo esse texto.

```bash
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

Se você executar novamente o `docker ps` vai ver que não há mais nenhum container sendo executado.

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

Ao executar um container Docker a partir de uma imagem, é necessário configurar um command ou entrypoint, que podem ter diferenças entre si. O entrypoint permite a execução de um executável, como um shell script ou um binário. No exemplo dado, um shell script foi executado para imprimir uma mensagem de "hello world" e a execução do container foi encerrada, uma vez que não havia mais comandos a serem executados.

Se você executar um `docker ps` vai notar que não há containers ativos:

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

Porém se você acrescentar a opção -a, vamos ver todos os containers, ativos e inativos:

```bash
docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
3de2e5c68313   hello-world   "/hello"   9 minutes ago   Exited (0) 9 minutes ago             sleepy_wilson

```

Veja que temos algumas opções:

- CONTAINER ID 3de2e5c68313
  - Representa um identificados para esse container
- IMAGE hello-world
  - Representa o nome da imagem executado
- COMMAND "/hello"
  - Representa o comando executado no entrypoint
- CREATED 9 minutes ago
  - Representa há quanto o container foi criado
- STATUS Exited (0) 9 minutes ago
  - Representa o estado do container
- PORTS
  - Nesse caso não temos portas, mas representas as portas que estão acessíveis no container
- NAMES sleepy_wilson
  - Representa o nome do container

## Executando um ubuntu

Agora que você já entendeu como o docker executa uma imagem, vamos dar um passo a frente.

Se você esteve atento, e eu espero que esteja, percebeu que na mensagem que o hello-world imprimiu na mensagem o seguinte:

```bash
...
To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash
```

```bash
...
Para tentar algo mais ambicioso, você pode executar um container ubuntu com:
 $ docker run -it ubuntu bash
```

Então vamos fazer isso agora:

```bash
docker run -it ubuntu bash

Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
dbf6a9befcde: Pull complete
Digest: sha256:dfd64a3b4296d8c9b62aa3309984f8620b98d87e47492599ee20739e8eb54fbf
Status: Downloaded newer image for ubuntu:latest
root@12837bc0b9d3:/#

```

Ao tentar executar uma imagem Docker que não está presente localmente, o Docker faz o download da imagem. Agora, temos acesso a um shell (bash) dentro do container Docker.

A opção -i indica o modo interativo, o que mantém a entrada padrão (stdin) ativa o tempo todo no container.

A opção -t está relacionada ao tty, que permite a execução de comandos no terminal. Portanto, -i mantém o terminal ativo e -t nos permite interagir com o terminal, executando comandos nele.

Sendo assim podemos executar comandos no shell do nosso ubuntu (container), veja:

```bash
Status: Downloaded newer image for ubuntu:latest
root@12837bc0b9d3:/# ls
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

root@12837bc0b9d3:/# uname -a
Linux 12837bc0b9d3 5.19.0-42-generic #43~22.04.1-Ubuntu SMP PREEMPT_DYNAMIC Fri Apr 21 16:51:08 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
root@12837bc0b9d3:/#
```

Se você executar um `docker ps` em outra janela do terminal vai ver que temos um container sendo executado:

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
12837bc0b9d3   ubuntu    "bash"    5 minutes ago   Up 5 minutes             peaceful_cerf
```

Note também que o `COMMAND` que está sendo executado é o terminal (bash).

Sendo assim, se você sair do container

```bash
root@12837bc0b9d3:/# exit
exit

```

E der um docker ps

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

```

Vai ver que não tem mais nenhum container ativo.

Porém se der um `docker ps -a` vai ver que o container do ubuntu está inativo:

```bash
docker ps -a

CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                          PORTS     NAMES
12837bc0b9d3   ubuntu        "bash"     8 minutes ago    Exited (0) About a minute ago             peaceful_cerf
3de2e5c68313   hello-world   "/hello"   26 minutes ago   Exited (0) 26 minutes ago                 sleepy_wilson

```

Agora se você quiser fazer com que um container volte a ser executado, você pode fazer isso com o comando `docker start`

Por exemplo:

```bash
docker start peaceful_cerf

peaceful_cerf

```

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND   CREATED         STATUS         PORTS     NAMES
12837bc0b9d3   ubuntu    "bash"    9 minutes ago   Up 2 seconds             peaceful_cerf
```

Veja que eu utilizei o nome do container localizado na coluna `NAMES`.

Agora imagine um cenário onde você tenha um cotainer que você queira executar algo e sair dele, e você não quer deixar rastros, ou seja não quer que depois de terminar o processo que ele fique listado quando der um `docker ps -a`.

Nesse caso podemos executar o seguinte:

```bash
docker run -it --rm ubuntu bash
```

Faça isso e depois de um `docker ps -a` e você verá que esse container não será listado, pois foi removido pela opção `--rm` assim que você saiu do processo de entrypoint.