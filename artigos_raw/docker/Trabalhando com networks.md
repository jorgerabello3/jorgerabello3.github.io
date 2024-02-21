# Trabalhando com networks

Quando você está executando container docker, ele possui uma espécie de rede interna, própria dele. Essa rede pode ser utilizada para diversas finalidades, sendo uma delas permitir a comunicação entre containers.

Um cenário de exemplo seria um container executando uma aplicação java e um segundo container executando um banco de dados PostgreSQL por exemplo, para que esses dois containers se comuniquem será necessário uma rede, uma network.

## Tipos de Networks

O docker possui alguns tipos de redes, a saber:

- bridge

  - networks do tipo bridge são as mais utilizadas, comuns e é o padrão que será utilizado caso você não informe o tipo de rede que deseja criar.

- host

  - Networks do tipo host é utilizado para mesclar a network do docker com a network da máquina. Então imaginando que você subiu um container com uma aplicação web na porta 80, se o tipo de network que você utilizou foi o tipo host, o container poderá acessar a rede da sua máquina física, coisa que não é permitido por padrão.
    Em outras palavras significa que a máquina poderá acessar o container sem a necessidade de expor portas por meio do parâmetro -p.

- overlay

  - Não é um formato muito comum, é utilizado quando você tem container em máquinas diferentes mas você precisa que eles se comuniquem como se estivessem na mesma rede. Esse forma de rede é mais utilizado em clusters docker swarm.

- maclan

  - Nesse formato é possível setar um mac address no container simulando uma rede virtual (vlan) plugada na sua rede. Esse formato também não é muito comum de ser utilizado.

- none
  - O formato nenhum é utilizado quando não se declara nenhum tipo de rede, isso fará com que o container seja executado de forma isolada.

## O comando docker network

```bash
docker network

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

Run 'docker network COMMAND --help' for more information on a command.
```

Podemos por exemplo listar as nossas redes da seguinte forma:

```bash
docker network ls

NETWORK ID     NAME      DRIVER    SCOPE
af855cbaa3d2   bridge    bridge    local
07e3aa548d8c   host      host      local
559bb92696a2   none      null      local
```

Caso você tenha networks que não estejam sendo utilizadas, você pode executar o comando a seguir para remover redes não utilizadas:

```bash
docker network prune

WARNING! This will remove all custom networks not used by at least one container.
Are you sure you want to continue? [y/N] y
```

## Trabalhando com bridge

Para começar vamos executar um container com ubuntu da seguinte forma:

```bash
docker run -d -it --name first_ubuntu bash
```

Na sequencia vamos executar um segundo ubuntu:

```bash
docker run -d -it --name second_ubuntu bash
```

Veja que agora temos 2 containers:

```bash
 docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS     NAMES
a2af539d0c81   bash      "docker-entrypoint.s…"   43 seconds ago       Up 42 seconds                 second_ubuntu
d95d846752be   bash      "docker-entrypoint.s…"   About a minute ago   Up About a minute             first_ubuntu
```

Nesse momento vamos inspecionar as nossas redes

```bash
docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "af855cbaa3d2043ea5601c0e6c3e3737ba9d09b50a2101af7a610cb70f09f44e",
        "Created": "2023-06-27T01:06:57.345401748-03:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.17.0.0/16",
                    "Gateway": "172.17.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "a2af539d0c81d3787e623d19ff6ec7bcdf3d143cf3dc3ff6adc691fe8471d592": {
                "Name": "second_ubuntu",
                "EndpointID": "e5151012805fcfbe91b813c0a11cc36aba6d5c2129ad8081f0b04727615ae781",
                "MacAddress": "02:42:ac:11:00:03",
                "IPv4Address": "172.17.0.3/16",
                "IPv6Address": ""
            },
            "d95d846752be810aeccd19dcd391125396bf16e79d67fadf3d708d5894ad7f9c": {
                "Name": "first_ubuntu",
                "EndpointID": "5e3f2184047431ecfb1c112f69a7e5b74b41634846bdd35a4f7723e85cba7b18",
                "MacAddress": "02:42:ac:11:00:02",
                "IPv4Address": "172.17.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]
```

Aqui temos várias informações sobre as networks, e em determinado trecho da saída note o seguinte:

```bash
docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "af855cbaa3d2043ea5601c0e6c3e3737ba9d09b50a2101af7a610cb70f09f44e",
        "Created": "2023-06-27T01:06:57.345401748-03:00",
        "Scope": "local",
        "Driver": "bridge",
```

Aqui podemos ver que estamos em uma rede do tipo bridge.

Mais abaixo, temos o second_ubuntu e o first_ubuntu, inclusive podemos ver os seus respectivos endereços de IP entre outras informações.

```bash
"Containers": {
    "a2af539d0c81d3787e623d19ff6ec7bcdf3d143cf3dc3ff6adc691fe8471d592": {
        "Name": "second_ubuntu",
        "EndpointID": "e5151012805fcfbe91b813c0a11cc36aba6d5c2129ad8081f0b04727615ae781",
        "MacAddress": "02:42:ac:11:00:03",
        "IPv4Address": "172.17.0.3/16",
        "IPv6Address": ""
    },
    "d95d846752be810aeccd19dcd391125396bf16e79d67fadf3d708d5894ad7f9c": {
        "Name": "first_ubuntu",
        "EndpointID": "5e3f2184047431ecfb1c112f69a7e5b74b41634846bdd35a4f7723e85cba7b18",
        "MacAddress": "02:42:ac:11:00:02",
        "IPv4Address": "172.17.0.2/16",
        "IPv6Address": ""
    }
},
```

Se você leu os artigos anteriores, viu que executamos os containers com o parâmetro `-d` de `--detach`. Agora queremos conectar ao nosso container, existem formas de se fazer isso e uma delas é utilizando a opção `attach` da seguinte forma:

```bash
docker attach first_ubuntu
```

Ao acessar o container podemos ver nosso endereço de ip:

```bash
first_ubuntu# ip add show

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever

9: eth0@if10: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

first_ubuntu#
```

Sendo assim o endereço ip do container `first_ubuntu` é 172.17.0.2, vamos verificar o endereço up do outro container:

```bash
docker attach second_ubuntu

second_ubuntu# ip add show

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever

11: eth0@if12: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:11:00:03 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.3/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

second_ubuntu#
```

Podemos constatar então que o container `second_ubuntu` tem o ip 172.17.0.3.

Agora vamos tentar comunicar o `second_ubuntu` com o `first_ubuntu`:

```bash
second_ubuntu# ping  172.17.0.2

PING 172.17.0.2 (172.17.0.2): 56 data bytes
64 bytes from 172.17.0.2: seq=0 ttl=64 time=0.075 ms
64 bytes from 172.17.0.2: seq=1 ttl=64 time=0.048 ms
64 bytes from 172.17.0.2: seq=2 ttl=64 time=0.057 ms
```

Veja que obtivemos sucesso, e a partir do `first_ubuntu` também conseguimos comunicação:

```bash
first_ubuntu# ping 172.17.0.3

PING 172.17.0.3 (172.17.0.3): 56 data bytes
64 bytes from 172.17.0.3: seq=0 ttl=64 time=0.066 ms
64 bytes from 172.17.0.3: seq=1 ttl=64 time=0.048 ms
64 bytes from 172.17.0.3: seq=2 ttl=64 time=0.055 ms
```

Agora vamos sair dos containers com o comando exit e remover eles:

```bash
docker rm -f $(docker ps -qa)

a2af539d0c81
d95d846752be
```

## Criando uma network

Nosso objetivo agora vai ser demonstrar como criar uma rede:

```bash
docker network create --driver bridge minha_rede

51aee2e1b4639ea0ad8777457072359adf696af1aa6b06d7a1956548ef9199c0
```

Uma vez criada a nossa rede, podemos observar ela com o comando `docker network ls`

```bash
docker network ls

NETWORK ID     NAME         DRIVER    SCOPE
af855cbaa3d2   bridge       bridge    local
07e3aa548d8c   host         host      local
51aee2e1b463   minha_rede   bridge    local
559bb92696a2   none         null      local
```

Agora vamos executar um container utilizando a rede criada:

```bash
docker run -dit --name first_ubuntu --network minha_rede bash

043ca9b74596ebce5cc6b709b808ee48c8e5941affb77b14982b853b06da0d08
```

Vamos executar também um segundo container na mesma rede:

```bash
docker run -dit --name second_ubuntu --network minha_rede bash

043ca9b74596ebce5cc6b709b808ee48c8e5941affb77b14982b853b06da0d08
```

Agora temos os dois containers sendo executados:

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS     NAMES
7324978ccb21   bash      "docker-entrypoint.s…"   25 seconds ago       Up 24 seconds                 second_ubuntu
043ca9b74596   bash      "docker-entrypoint.s…"   About a minute ago   Up About a minute             first_ubuntu
```

Vamos tentar entrar neles:

Além do attach podemos utilizar o exec para se conectar ao container:

```bash
docker exec -it first_ubuntu bash

first_ubuntu# ip addr show

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever

14: eth0@if15: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:12:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.2/16 brd 172.18.255.255 scope global eth0
       valid_lft forever preferred_lft forever

first_ubuntu#
```

Veja que temos com o endereço ip 172.18.0.2 no container `first_ubuntu`, vamos verificar o segundo container:

```bash
❯ docker exec -it second_ubuntu bash
second_ubuntu# ip addr show

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever

16: eth0@if17: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:12:00:03 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.3/16 brd 172.18.255.255 scope global eth0
       valid_lft forever preferred_lft forever

second_ubuntu#
```

Veja que o endereço ip do segundo container é 172.18.0.3. Vamos tentar nos comunicar com o container de nome `first_ubuntu`:

```bash
second_ubuntu# ping 172.18.0.2

PING 172.18.0.2 (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.094 ms
64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.056 ms
64 bytes from 172.18.0.2: seq=2 ttl=64 time=0.058 ms
```

E veja que funciona perfeitamente.

Agora vamos criar um terceiro container, porém este não vamos colocar na rede que criamos:

```bash
docker run -dit --name third_ubuntu bash

fb657a9a9107d7899efdc4b2d92f8f7b7e7c1e0a9d8fa1ad3dfdd0ae6672107f
```

```bash
docker exec -it third_ubuntu bash

third_ubuntu# ip addr show

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever

18: eth0@if19: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

third_ubuntu#
```

Vamos tentar nos comunicar com os outros containers:

```bash
third_ubuntu# ping 172.18.0.2
PING 172.18.0.2 (172.18.0.2): 56 data bytes
^C
--- 172.18.0.2 ping statistics ---
2 packets transmitted, 0 packets received, 100% packet loss

third_ubuntu# ping 172.18.0.3
PING 172.18.0.3 (172.18.0.3): 56 data bytes
^C
--- 172.18.0.3 ping statistics ---
2 packets transmitted, 0 packets received, 100% packet loss

third_ubuntu#
```

Veja quem em ambos os casos houve erro, 100% de perda de pacotes.

Isso acontece por que nosso container de nome `third_ubuntu` está em um rede separada da que criamos, porém eu posso conecta-lo. Abra um novo terminal e execute o seguinte comando:

```bash
docker network connect minha_rede third_ubuntu
```

Feito isso tente testar a comunicação novamente

```bash
third_ubuntu# ping 172.18.0.2

PING 172.18.0.2 (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.090 ms
64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.057 ms
64 bytes from 172.18.0.2: seq=2 ttl=64 time=0.057 ms
^C
--- 172.18.0.2 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.057/0.068/0.090 ms

third_ubuntu# ping 172.18.0.3

PING 172.18.0.3 (172.18.0.3): 56 data bytes
64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.089 ms
64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.056 ms
64 bytes from 172.18.0.3: seq=2 ttl=64 time=0.056 ms
^C
--- 172.18.0.3 ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max = 0.056/0.067/0.089 ms
```

E veja que agora tudo funciona perfeitamente.

## Trabalhando com host

```bash
docker run --rm -d --name nginx --network host nginx
```

Note que dessa vez não foi mapeada a porta 80 do container, não utilizamos o parâmetro -p

```bash
docker ps

CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS     NAMES
c1ac2e47cde1   nginx     "/docker-entrypoint.…"   22 seconds ago   Up 22 seconds             nginx
```

Porém se tentarmos acessar o container vamos conseguir

```bash
curl http://localhost

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

Isso acontece porque estamos utilizando a network no modo host.

Bom por enquanto é isso, nessa etapa da nossa sequencia de artigos, exploramos algumas opções de trabalho com networks utilizando Docker, valeu e até a próxima.
