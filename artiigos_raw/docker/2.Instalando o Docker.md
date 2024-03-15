## Instalando o Docker

O Docker oferece um formato de instalação chamado Docker Desktop, esse formato de instalação gera uma pseudo máquina virtual, essa máquina virtual é um linux e todos os comandos são executados nessa máquina virtual Linux.

Esse approach é utilizado tanto no Mac OS quanto no Windows, porém no Windows temos um recurso interessante chamado WSL (Windows Subsystem for Linux).

Este artigo tem por finalidade demonstrar a instalação do Docker no Linux.

## Pré requisitos

- Ubuntu Lunar 23.04
- Ubuntu Kinetic 22.10
- Ubuntu Jammy 22.04 (LTS)
- Ubuntu Focal 20.04 (LTS)
- Ubuntu Bionic 18.04 (LTS)

A primeira coisa que devemos fazer é desintalar possíveis versões antigas

## Desinstalando versões antigas

Versões anteriores do Docker eram conhecidas como `docker`, `docker.io` ou `docker-engine`. Você também pode ter instalações de `containerd` ou `runc`.

Desinstale todas essas versões anteriores antes de tentar instalar uma nova versão.

```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```

## Instalando por meio de um repositório apt

Antes de instalar o Docker Engine pela primeira vez em uma nova máquina hospedeira, você precisa configurar o repositório do Docker. Em seguida, você poderá instalar e atualizar o Docker a partir do repositório.

1. Atualize o índice de pacotes do apt e instale os pacotes necessários para permitir que o apt utilize um repositório via HTTPS:

```bash
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
```

2. Adicione a chave GPG oficial do Docker:

```bash
sudo install -m 0755 -d /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

3. Use o seguinte comando para configurar o repositório:

```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Atualize o índice de pacotes do apt com o seguinte comando:

```bash
sudo apt-get update
```

4. Instale o Docker Engine, o containerd e o Docker Compose.

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

sudo docker run hello-world

## Post Install

É importante em alguns sistemas operacionas proceder com o post install. O pós instalação do docker consiste basicamente em criar um grupo para gerenciar o docker e nos adicionar a esse grupo, assim sendo:

Para criar o grupo "docker" e adicionar o seu usuário:

1. Crie o grupo "docker".

```bash
sudo groupadd docker
```

2. Adicione o seu usuário ao grupo "docker".

```bash
sudo usermod -aG docker $USER
```

3. Faça o logout e em seguida faça login novamente para que a sua adesão ao grupo seja reavaliada.

Você também pode executar o seguinte comando para ativar as alterações nos grupos:

```bash
newgrp docker
```

4. Verifique se você pode executar comandos do Docker sem o uso do sudo.

```bash
 docker run hello-world
```
Este comando faz o download de uma imagem de teste e a executa em um contêiner. Quando o contêiner é executado, ele imprime uma mensagem e encerra.

## Corrigindo erros

Se você inicialmente executou comandos do Docker CLI usando sudo antes de adicionar seu usuário ao grupo "docker", você pode encontrar o seguinte erro:

```bash
WARNING: Error loading config file: /home/user/.docker/config.json -
stat /home/user/.docker/config.json: permission denied
```
Esse erro indica que as configurações de permissão para o diretório `~/.docker/` estão incorretas devido ao uso anterior do comando sudo.

Para corrigir esse problema, você pode remover o diretório `~/.docker/` (ele será recriado automaticamente, mas as configurações personalizadas serão perdidas) ou alterar a propriedade e as permissões usando os seguintes comandos:

```bash
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "$HOME/.docker" -R
```

## Configuirando o docker para iniciar com o systemd
Geralmente essa configuração é desnecessária pois na maioria das distribuições Linux modernas utilizam o systemd para gerenciar quais serviços iniciam quando o sistema é inicializado. No Debian e no Ubuntu, o serviço do Docker é iniciado automaticamente durante a inicialização por padrão. Para iniciar automaticamente o Docker e o containerd durante a inicialização em outras distribuições Linux que utilizam o systemd, execute os seguintes comandos:

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

Para interromper esse comportamento, utilize o comando "disable" em vez disso.

```bash
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```
# Instalando o docker-compose

O Docker Compose é uma ferramenta desenvolvida pela Docker, Inc. que permite definir e gerenciar aplicações compostas por vários contêineres Docker. Ele é utilizado para simplificar o processo de orquestração de múltiplos serviços Docker, permitindo que você descreva a infraestrutura necessária para sua aplicação em um arquivo YAML.

Com o Docker Compose, você pode definir a estrutura da sua aplicação, incluindo os serviços, redes e volumes necessários para o funcionamento correto da aplicação. Cada serviço é geralmente executado em seu próprio contêiner Docker isolado, mas eles podem se comunicar entre si por meio das redes criadas pelo Docker Compose.

O arquivo YAML do Docker Compose é chamado de "docker-compose.yml" e nele você pode especificar as seguintes informações:

Serviços: Cada serviço representa um contêiner Docker que compõe sua aplicação. Por exemplo, um serviço pode ser o servidor web, outro pode ser o banco de dados e assim por diante.

**Imagens:** Você especifica a imagem Docker a ser usada para cada serviço. Essas imagens geralmente são hospedadas no Docker Hub ou em um registro de contêiner privado.

**Redes:** Você pode definir redes personalizadas para seus serviços, permitindo que eles se comuniquem entre si de maneira isolada.

**Volumes:** É possível especificar volumes que serão montados dentro dos contêineres para persistência de dados ou compartilhamento de arquivos com o host.

Variáveis de ambiente: O Docker Compose permite definir variáveis de ambiente que podem ser injetadas nos contêineres, facilitando a configuração de cada serviço.

Com o arquivo "docker-compose.yml" definido, você pode usar os comandos do Docker Compose para iniciar, parar, reiniciar, construir e gerenciar seus serviços e contêineres de forma simples e automatizada. Essa abordagem facilita a configuração de ambientes de desenvolvimento e produção, bem como a implantação de aplicações em diferentes sistemas.

Em resumo, o Docker Compose é uma ferramenta poderosa para definir, executar e gerenciar aplicativos compostos por múltiplos serviços em contêineres Docker, tornando a orquestração de aplicações mais fácil e escalável.

Para instalar basta executar o seguinte comando

```bash
 curl -SL https://github.com/docker/compose/releases/download/v2.20.2/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
```
Basicamente isso faz download do script do docker-compose e coloca no diretório `/usr/local/bin/` 

Caso você tenha problemas de permissões, pode criar um link simbólico

```bash
 sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
``` 
Após a instalação, você pode checar se tudo correu bem:

```bash
docker-compose -v
docker-compose version 1.29.2, build unknown
```

## Fontes:

https://docs.docker.com/engine/install/ubuntu/
https://docs.docker.com/engine/install/linux-postinstall/
https://docs.docker.com/compose/install/standalone/