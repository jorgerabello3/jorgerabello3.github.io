## Construindo uma aplicação com banco de dados utilizando docker compose

### Parte I: O banco de dados

Para esse exemplo vamos subir uma imagem de um banco de dados e uma aplicação com node, sendo assim vamos partir de um arquivo docker-compose.yaml de base

```yaml
version: "3"

services:

networks:
  minharede:
    driver: bridge
```

A primeira coisa que vamos fazer é criar um container para a base de dados MySQL

```yaml
version: "3"

services:
  database:
    image: mysql:5.7
    command: --innodb-use-native-aio=0
    container_name: database
    restart: always
    tty: true
    volumes:
      - ./mysql:/var/lib/mysql
    environment:
      - MYSQL_DATABASE=appdb
      - MYSQL_ROOT_PASSWORD=dbpasswd
      - MYSQL_USER=dbuser
    networks:
      - minharede

networks:
  minharede:
    driver: bridge

volumes:
  mysql:
```

Antes de seguir vamos conhecer as opções novas:

`command: --innodb-use-native-aio=0`

- Equivalente ao CMD nesse caso seta esse parâmetro na execução do MySQL
- O InnoDB é um mecanismo de armazenamento utilizado no MySQL que utiliza o subsistema de I/O **assíncrono** (AIO nativo) no Linux para executar operações de leitura e gravação em páginas de arquivos de dados. Essa funcionalidade é ativada por padrão na opção de configuração innodb_use_native_aio e é aplicável apenas a sistemas Linux. Em sistemas Unix diferentes do Linux, o InnoDB utiliza apenas I/O **síncrona**. Anteriormente, o InnoDB utilizava apenas I/O assíncrona em sistemas Windows. No entanto, para usar o subsistema de I/O assíncrono no Linux, é necessário ter a biblioteca libaio instalada. Para saber mais a respeito visite [esse link](https://dev.mysql.com/doc/refman/8.0/en/innodb-linux-native-aio.html).

`restart: always`

- caso algo de errado nosso container vai ficar reiniciando sozinho até que ele consiga estar funcional, isso evita que o container morra em caso de erro.

`tty: true`

- caso seja preciso acessar o shell do sistema operacional desse container vamos habilitar o tty.

```yaml
volumes:
  - ./mysql:/var/lib/mysql
```

- Aqui estamos criando um volume, onde dizemos que tudo que for gravado no diretório `/var/lib/mysql` do container vai ser gravado no diretório `mysql` que está na nossa máquina. Isso serve para que ao desligar o container os dados gravados no mysql não sejam perdidos.

```yaml
environment:
  - MYSQL_DATABASE=appdb
  - MYSQL_ROOT_PASSWORD=dbpasswd
  - MYSQL_USER=dbuser
```

Usamos a opção `environment` para passar variáveis de ambiente para a execução do container, nesse caso estamos definindo o nome da base de dados, a senha do usuário root, e um usuário para acessar a base de dados.

_**PRO TIP:**_

No lugar de utilizar `environment` você pode utilizar `env_file` e armazenar suas variáveis de ambiente em um arquivo .env que não deverá ser comitado no git.

Uma das vantagens dessa abordagem é que você não expões nomes de usuários e senhas, a desvantagem é que cada vez que alguém precisar subir esse container vai ter de saber criar o arquivo .env e suas variáveis.

```yaml
env_file:
  - .env
```

Agora podemos executar

```shell
docker-compose up -d
```

E teremos um servidor MySQL sendo executado.

```bash
 docker ps

CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                 NAMES
43ce1b233080   mysql:5.7   "docker-entrypoint.s…"   14 minutes ago   Up 14 minutes   3306/tcp, 33060/tcp   database
```

Podemos também visualizar os logs do container

```bash
docker logs database

2023-07-11 04:05:23+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.42-1.el7 started.
2023-07-11 04:05:23+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
2023-07-11 04:05:23+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 5.7.42-1.el7 started.
2023-07-11 04:05:23+00:00 [Warn] [Entrypoint]: MYSQL_USER specified, but missing MYSQL_PASSWORD; MYSQL_USER will not be created
2023-07-11 04:05:23+00:00 [Note] [Entrypoint]: Initializing database files
...
```

### Parte II: Aplicação node

Pra começar vamos criar um diretório chamado nodeapp e dentro desse diretório um arquivo chamado Dockerfile, nossa estrutura deve ficar mais ou menos assim:

```shell
.
├── docker-compose.yml
├── mysql
└── nodeapp
    └── Dockerfile
```

O Dockerfile deve ter o seguinte conteúdo:

```Dockerfile
FROM node:18.16.1

WORKDIR /usr/src/app

EXPOSE 3000
```

Agora entre no diretório nodeapp

```shell
cd nodeapp
```

E crie uma aplicação chamada nodeapp

```shell
npm init
```

Veja que após o preenchimento dos dados foi criado um arquivo chamado `package.json`

Faça a instalação do express

```shell
npm install express --save
```

Crie um arquivo chamado index.js com o seguinte conteúdo:

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

Retornando ao docker-compose.yaml veja que ele agora tem o seguinte conteúdo:

```yaml
version: "3"

services:
  database:
    image: mysql:5.7
    command: --innodb-use-native-aio=0
    container_name: database
    restart: always
    tty: true
    volumes:
      - ./mysql:/var/lib/mysql
    environment:
      - MYSQL_DATABASE=appdb
      - MYSQL_ROOT_PASSWORD=dbpasswd
      - MYSQL_USER=dbuser
    env_file:
      - .env
    networks:
      - minharede

networks:
  minharede:
    driver: bridge

volumes:
  mysql:
```

Vamos adicionar nossa aplicação node

```yaml
version: "3"

services:
  app:
    build:
      context: ./nodeapp
    networks:
      - minharede
    volumes:
      - ./nodeapp:/usr/src/app
    tty: true
    ports:
      - "3000:3000"
    container_name: nodeapp

  database:
    image: mysql:5.7
    command: --innodb-use-native-aio=0
    container_name: database
    restart: always
    tty: true
    volumes:
      - ./mysql:/var/lib/mysql
    environment:
      - MYSQL_DATABASE=appdb
      - MYSQL_ROOT_PASSWORD=dbpasswd
      - MYSQL_USER=dbuser
    networks:
      - minharede

networks:
  minharede:
    driver: bridge

volumes:
  mysql:
```

Uma vez feito isso, agora podemos iniciar o container:

```shell
docker-compose up -d --build
```

E pronto, temos nossos containers sendo executados.

```shell
docker ps

CONTAINER ID   IMAGE                    COMMAND                  CREATED              STATUS              PORTS                 NAMES
bcc7c7d85ae0   exemplo_node_mysql_app   "docker-entrypoint.s…"   About a minute ago   Up About a minute   3000/tcp              nodeapp
43ce1b233080   mysql:5.7                "docker-entrypoint.s…"   33 minutes ago       Up 33 minutes       3306/tcp, 33060/tcp   database
```

Porém ainda não estamos conectados ao banco de dados MySQL, para isso vamos ter de editar nosso programa que está no arquivo index.js, sendo assim altere esse arquivo da seguinte forma:

Para isso a primeira coisa que vamos fazer é acessar o container da base de dados:

```bash
docker ps

CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                                       NAMES
551a29f83748   exemplo_node_mysql_app   "docker-entrypoint.s…"   4 minutes ago   Up 4 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   nodeapp
24b1ec7ea978   mysql:5.7                "docker-entrypoint.s…"   7 minutes ago   Up 7 minutes   3306/tcp, 33060/tcp                         database
```

Para isso vamos executar:

```bash
docker exec -it database bash

bash-4.2#
```

Uma vez dentro do container vamos acessar o shell do MySQL com o mysql-client

```bash
bash-4.2# mysql -uroot -p
Enter password:

Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.42 MySQL Community Server (GPL)

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

Uma vez dentro do MySQL vamos ver quais databases ele tem

```bash
mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| appdb              |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
5 rows in set (0.00 sec)

mysql>
```

Veja que foi criada uma database com o nome de `appdb` conforme especificado nas variáveis de ambiente do docker-compose.yml.

Agora vamos utilizar a database appdb para criar uma tabela para a nossa aplicação:

Primeiro selecione a database

```bash
mysql> use appdb;

Database changed
```

Agora crie a tabela

```bash
mysql> create table todo(id int not null auto_increment, title varchar(255), primary key(id));
Query OK, 0 rows affected (0.01 sec)
```

Podemos descrever a estrutura da nossa tabela com o comando desc

```bash
mysql> desc todo;

+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| title | varchar(255) | YES  |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)

mysql>
```

**ATENÇÃO:** O código mostrado a seguir é apenas um exemplo, para demonstrar o funcionamento de uma aplicação e uma base de dados se comunicando, em uma aplicação real, que iria para produção jamais teríamos a camada de apresentação e dados misturadas e muito menos iríamos expor os dados da conexão com o banco de dados no código fonte blz ?

Vamos fazer com que a nossa aplicação node crie um registro nessa tabela. Primeiramente vamos acessar o container que tem a aplicação nodejs

```bash
docker ps

CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS          PORTS                                       NAMES
551a29f83748   exemplo_node_mysql_app   "docker-entrypoint.s…"   13 minutes ago   Up 13 minutes   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp   nodeapp
24b1ec7ea978   mysql:5.7                "docker-entrypoint.s…"   16 minutes ago   Up 16 minutes   3306/tcp, 33060/tcp                         database
```

```bash
docker exec -it nodeapp bash

root@551a29f83748:/usr/src/app#
```

Vamos instalar o cliente mysql

```bash
root@551a29f83748:/usr/src/app# npm install mysql --save
```

Agora vamos alterar a nossa aplicação no arquivo index.js para que ela se conecte ao MySQL e cadastre um dado no banco de dados:

```javascript
const express = require("express");
const app = express();
const port = 3000;

const database_configuration = {
  host: "database",
  user: "root",
  password: "dbpasswd",
  database: "appdb",
};

const mysql = require("mysql");

const connection = mysql.createConnection(database_configuration);

const sql = `INSERT INTO todo(title) VALUES ('Estudar')`;

connection.query(sql);

connection.end();

app.get("/", (req, res) => {
  res.send("<h1>It Works !</h1>");
});

app.listen(port, () => {
  console.log("Server running on port " + port);
});
```

Agora volte ao shell do container da aplicação node e execute a aplicação

```bash
root@551a29f83748:/usr/src/app# node index.js

Server running on port 3000
```

E verifique no container do MySQL se ocorreu o registro do dado

```bash
mysql> select * from todo;
+----+---------+
| id | title   |
+----+---------+
|  1 | Estudar |
+----+---------+
1 row in set (0.00 sec)

mysql>
```

## Tratando a dependência entre containers

Nesse nosso exemplo temos 2 containers onde um depende do outro, então queremos garantir que a aplicação se conecte ao banco de dados. Para fazer isso podemos contar com algumas opções.

A primeira é utilizar no nosso docker-compose a opção depends on, essa opção fará com que a ordem de execução dos containers seja alterada, então vamos editar o docker compose, deixando da seguinte forma:

```yaml
version: "3"

services:
  app:
    build:
      context: ./nodeapp
    networks:
      - minharede
    volumes:
      - ./nodeapp:/usr/src/app
    tty: true
    ports:
      - "3000:3000"
    container_name: nodeapp
    depends_on:
      - database

  database:
    image: mysql:5.7
    command: --innodb-use-native-aio=0
    container_name: database
    restart: always
    tty: true
    volumes:
      - ./mysql:/var/lib/mysql
    environment:
      - MYSQL_DATABASE=appdb
      - MYSQL_ROOT_PASSWORD=dbpasswd
      - MYSQL_USER=dbuser
    networks:
      - minharede

networks:
  minharede:
    driver: bridge

volumes:
  mysql:
```

Agora garantimos que o container database vai subir primeiro. Porém o container da aplicação não vai ficar esperando o MySQL (banco de dados estar pronto), para essa finalidade temos algumas ferramentas, nesse exemplo vamos utilizar o [dockerize](https://github.com/jwilder/dockerize).

Sendo assim conforme manda a documentação, vamos editar o nosso Dockerfile para instalar na imagem do node o dockerize

```Dockerfile
FROM node:18.16.1

ENV DOCKERIZE_VERSION v0.7.0

RUN apt-get update \
    && apt-get install -y wget \
    && wget -O - https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz | tar xzf - -C /usr/local/bin \
    && apt-get autoremove -yqq --purge wget && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/src/app

EXPOSE 3000
```

Após editar o Dockerfile vamos buildar novamente a imagem

Então vamos parar os nosso containers

```bash
docker rm -f $(docker ps -qa)
```

Buildar novamente

```bash
docker-compose up -d --build
```

Por fim vamos alterar o entry point da nossa aplicação node, fazendo com que o dockerize seja executado, para isso altere o docker-compose da seguinte forma adicionando o entrypoint

```yaml
version: '3'

services:
  app:
    build:
      context: ./nodeapp
    networks:
      - minharede
    volumes:
      - ./nodeapp:/usr/src/app
    tty: true
    ports:
      - "3000:3000"
    container_name: nodeapp
    entrypoint: dockerize -wait tcp://database:3306 -timeout 20s docker-entrypoint.sh
    depends_on:
      - database

  database:
    image: mysql:5.7
    command: --innodb-use-native-aio=0
    container_name: database
    restart: always
    tty: true
    volumes:
      - ./mysql:/var/lib/mysql
    environment:
      - MYSQL_DATABASE=appdb
      - MYSQL_ROOT_PASSWORD=dbpasswd
      - MYSQL_USER=dbuser
    networks:
      - minharede

networks:
  minharede:
    driver: bridge

volumes:
  mysql:
```

Nesse caso estamos dizendo que quando esse container for executado o dockerize vai aguardar por 20 segundos uma conexão com o mysql.

Faça um novo build

```bash
docker-compose up -d --build
```

E repare que o container nodeapp subiu depois do mysql (database) e se inspecionarmos os logs

```bash
docker logs nodeapp

2023/07/12 07:34:40 Waiting for: tcp://database:3306
2023/07/12 07:34:40 Connected to tcp://database:3306
Welcome to Node.js v18.16.1.
Type ".help" for more information.
```

Veja que o container ficou aguardando (retentando) a conexão com a base de dados.
