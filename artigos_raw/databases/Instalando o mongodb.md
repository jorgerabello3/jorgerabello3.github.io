# Objetivo 

O objetivo deste artigo será demonstrar a instalação e operação básica do MongoDB Community

# Instalando as dependências

```bash
sudo apt-get install gnupg curl
```

# Obtendo a chave GPG do repositório

```bash
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg \
   --dearmor
```

# Criando o fonte de sources.list

Criar um sources.list, aqui tenha atenção você deve criar para a sua versão de distribuição, nesse exemplo, estou fazendo para o Ubuntu 22.04 (Jammy) consulte a documentação de referencia.

```bash
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list
```

# Atulizando a base de pacotes

```bash
sudo apt-get update
```

# Fazendo a instalação propriamente dita

```bash
sudo apt-get install -y mongodb-org
```

Esse comando vai instalar a última versão, para instalar uma versão específica, você pode fazer algo do tipo:

```bash
sudo apt-get install -y mongodb-org=6.0.8 mongodb-org-database=6.0.8 mongodb-org-server=6.0.8 mongodb-org-mongos=6.0.8 mongodb-org-tools=6.0.8
```

# Lidando com o serviço

Como eu estou em um Ubuntu 22.04 vou utilizar o `systemd (systemctl)` para controlar o serviço do MongoDB, em alguns casos pode ser que seja preciso utilizar `System V Init (service)`, consulte a documentação.

Os comandos do systemd são bem simples:

Isso inicia o serviço

```bash
sudo systemctl start mongod
```

Se você receber o erro `Failed to start mongod.service: Unit mongod.service not found.` então execute:

```bash
sudo systemctl daemon-reload
```

Esse comando exibe o status do servidor

```bash
sudo systemctl status mongod
```

E esse permite habilitar o servidor para que inicie junto ao host

```bash
sudo systemctl enable mongod
```

Se você precisar parar o serviço, utilize stop

```bash
sudo systemctl stop mongod
```

E pra reiniciar é quase igual

```bash
sudo systemctl restart mongod
```

# Testando

Agora vamos testar se tudo está ok ! Para acessar o mongodb utilizamos o comando `mongosh`

```bash
❯ mongosh
Current Mongosh Log ID:	64c361b46ad84ae07c01d0a1
Connecting to:		mongodb://127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&appName=mongosh+1.10.1
Using MongoDB:		6.0.8
Using Mongosh:		1.10.1

For mongosh info see: https://docs.mongodb.com/mongodb-shell/

------
   The server generated these startup warnings when booting
   2023-07-28T03:35:29.562-03:00: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine. See http://dochub.mongodb.org/core/prodnotes-filesystem
   2023-07-28T03:35:29.891-03:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted
   2023-07-28T03:35:29.891-03:00: vm.max_map_count is too low
------

test>
```

## Exibindo databases

Podemos exibir as databases com o comando `show dbs`

```bash
test> show dbs
admin   132.00 KiB
config   12.00 KiB
local    72.00 KiB
```

## Criando databases

Vamos criar uma database, fazemos isso com o comando `use`

```bash
test> use friends
switched to db friends
friends>
```

Caso você precise saber em qual databse você está basta utilizar o comando `db` que tem uma referência para a database, veja:

```bash
friends> db
friends
```

## Excluindo databases

Basicamente os comandos do mongodb se resumem a sintaxe `db.<database>.<comando>`.

Para excluir a database você pode fazer o seguinte:

```bash
friends> db.dropDatabase()
{ ok: 1, dropped: 'friends' }
```

# Controle de Acesso

## Criando um usuário

Vamos criar um usuário para garantir a segurança de acesso ao nosso servidor. Para isso selecione a database `admin`:

```bash
test> use admin
switched to db admin
```

Crie o usuário e uma senha

```bash
admin> db.createUser(
...   {
...     user: "dev",
...     pwd: passwordPrompt(),
...     roles: [ { role: "userAdminAnyDatabase", db: "admin" }, "readWriteAnyDatabase" ]
...  }
... )
Enter password
devadmin
********{ ok: 1 }
```

E encerre a conexão.

```bash
admin> exit
```

## Configurando o servidor

Agora precisamos dizer ao mongodb que todo acesso precisa ser autenticado pra isso vamos editar o arquivo `/etc/mongod.conf`

```bash
sudo nano /etc/mongod.conf
```

Esse arquivo tem esse formato:

```conf
# mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# Where and how to store data.
storage:
  dbPath: /var/lib/mongodb
#  engine:
#  wiredTiger:

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log

# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1


# how the process runs
processManagement:
  timeZoneInfo: /usr/share/zoneinfo

#security:

#operationProfiling:

#replication:

#sharding:

## Enterprise-Only Options:

#auditLog:

#snmp:
```

Vamos alterar aquela seção `#security` deixando ela assim

```conf
#security:
security:
  authorization: enabled
```

O arquivo completo fica assim:

```conf
# mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# Where and how to store data.
storage:
  dbPath: /var/lib/mongodb
#  engine:
#  wiredTiger:

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log

# network interfaces
net:
  port: 27017
  bindIp: 127.0.0.1


# how the process runs
processManagement:
  timeZoneInfo: /usr/share/zoneinfo

#security:
security:
  authorization: enabled

#operationProfiling:

#replication:

#sharding:

## Enterprise-Only Options:

#auditLog:

#snmp:
```

Agora vamos riniiciar o serviço

```bash
sudo systemctl restart mongod
```

Verificar seu status, para saber se não houve erros na configuração

```bash
 sudo systemctl status mongod
● mongod.service - MongoDB Database Server
     Loaded: loaded (/lib/systemd/system/mongod.service; disabled; vend>
     Active: active (running) since Fri 2023-07-28 03:44:56 -03; 19s ago
       Docs: https://docs.mongodb.org/manual
   Main PID: 289625 (mongod)
     Memory: 166.0M
        CPU: 398ms
     CGroup: /system.slice/mongod.service
             └─289625 /usr/bin/mongod --config /etc/mongod.conf

jul 28 03:44:56 machinewar systemd[1]: Started MongoDB Database Server.
jul 28 03:44:56 machinewar mongod[289625]: {"t":{"$date":"2023-07-28T06
```

## Fazendo login

E então podemos fazer login:

```bash
mongosh -u dev -p --authenticationDatabase admin

Enter password: ********
Current Mongosh Log ID:	64c36434a5483229182dd678
Connecting to:		mongodb://<credentials>@127.0.0.1:27017/?directConnection=true&serverSelectionTimeoutMS=2000&authSource=admin&appName=mongosh+1.10.1
Using MongoDB:		6.0.8
Using Mongosh:		1.10.1

For mongosh info see: https://docs.mongodb.com/mongodb-shell/

test>
```

Vamos encerrar a conexão

```bash
test> exit
```

# Desabilitando o serviço

Como pra mim interessa apenas ter apenas o client `mongosh` e eu não quero ficar rodando o servidor vou desabilitar ele:

Pra isso vou parar o serviço

```bash
sudo systemctl stop mongod
```

E desabilitar

```bash
sudo systemctl disable mongod
```

Se verificar seu status vai estar inativo

```bash
sudo systemctl status mongod
○ mongod.service - MongoDB Database Server
Loaded: loaded (/lib/systemd/system/mongod.service; disabled; vend>
Active: inactive (dead)
Docs: https://docs.mongodb.org/manual
```

Bom pessoal é isso aí ! Eu tive bastante trabalho para encontrar essa documentação e fazer essa instalação espero que esse artigo poupe o sofrimento de mais alguém.

Um abraço !