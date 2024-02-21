# Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente de containers  ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Containers

### **Qual a diferença entre docker e docker-compose ?**

Docker é uma plataforma de código aberto para criar, implantar e gerenciar aplicativos em contêineres. Ele permite que os desenvolvedores empacotem seus aplicativos junto com as dependências necessárias em um contêiner, garantindo que o aplicativo funcione da mesma forma em qualquer ambiente.

Já o Docker Compose é uma ferramenta que permite definir e executar aplicativos Docker compostos por vários contêineres. Ele permite que os usuários definam e gerenciem aplicativos Docker complexos com vários contêineres, redes e volumes em um único arquivo YAML.

Em resumo, o Docker é uma plataforma para criar e gerenciar contêineres, enquanto o Docker Compose é uma ferramenta para gerenciar aplicativos compostos por vários contêineres.

### **Você poderia descrever a diferença entre um container do docker e uma máquina virtual ?**

**TL;DR:** A principal diferença entre containers Docker e máquinas virtuais é que os containers compartilham o sistema operacional host, enquanto as máquinas virtuais têm seu próprio sistema operacional virtualizado. Os containers são mais leves e têm um tempo de inicialização mais rápido do que as máquinas virtuais, mas podem ter menos isolamento do sistema host. As máquinas virtuais são mais pesadas e têm um tempo de inicialização mais lento, mas oferecem isolamento completo do sistema host.

Um container Docker é uma unidade de software que inclui todos os elementos necessários para executar um aplicativo de forma isolada do sistema operacional host, como bibliotecas, dependências e binários. Ele compartilha o kernel do sistema operacional host, permitindo que os containers sejam mais leves e tenham um tempo de inicialização mais rápido. Cada container é executado em sua própria namespace, que o isola do restante do sistema.

Por outro lado, uma máquina virtual é uma cópia completa de um sistema operacional, incluindo o kernel, a memória e o hardware virtualizado. Isso permite que um sistema operacional seja executado em cima de outro sistema operacional como se estivesse sendo executado em hardware físico dedicado. Uma máquina virtual é mais pesada que um container, pois cada máquina virtual precisa de seu próprio sistema operacional, kernel e recursos de hardware virtualizado.

### **O que você entende pelos comandos docker ps, docker images, docker network ls, docker volume ls e docker build ?**

* **docker ps:** é um comando que lista todos os containers em execução no momento.

* **docker images:** é um comando que lista todas as imagens disponíveis no host Docker local.

* **docker network ls:** é um comando que lista todas as redes disponíveis no host Docker local.

* **docker volume ls:** é um comando que lista todos os volumes disponíveis no host Docker local.

* **docker build:** é um comando utilizado para criar uma nova imagem a partir de um Dockerfile. O Dockerfile é um arquivo de configuração que descreve como a imagem deve ser construída, contendo instruções para instalar dependências, copiar arquivos, definir variáveis de ambiente, entre outras configurações. Ao executar o comando docker build, o Docker irá ler o Dockerfile e criar uma nova imagem com base nas configurações descritas.

### **Como você descreveria o processo de multi stage build do docker ?**

**TL;DR:** O processo de multi-stage build é uma técnica poderosa que ajuda a melhorar a eficiência e a segurança do processo de construção de imagens Docker, permitindo a criação de imagens menores, mais seguras e mais rápidas.

O processo de multi-stage build no Docker é uma técnica para criar imagens Docker mais leves e eficientes. Ele permite que o desenvolvedor defina múltiplos estágios de compilação dentro de um único Dockerfile, e cada estágio pode ser baseado em diferentes imagens base, com diferentes dependências e bibliotecas.

A principal vantagem do processo de multi-stage build é que ele ajuda a reduzir o tamanho final da imagem Docker, evitando a inclusão de arquivos e dependências desnecessárias. Além disso, ele também torna o processo de construção mais eficiente, pois permite que os estágios intermediários sejam armazenados em cache e reutilizados em builds subsequentes, acelerando a compilação.

Por exemplo, em uma aplicação Java, é comum ter um estágio de compilação, um estágio de teste e um estágio de produção. Cada estágio tem requisitos diferentes em termos de bibliotecas e dependências, então o processo de multi-stage build permite que essas etapas sejam definidas e executadas em sequência, evitando a necessidade de criar várias imagens intermediárias.

Eis aqui um exemplo de Dockerfile com multi stage building:

```Dockerfile
FROM gradle:7.1.1-jdk11 AS TEMP_BUILD_IMAGE

ENV APP_HOME=/usr/app

WORKDIR $APP_HOME

COPY build.gradle settings.gradle gradlew $APP_HOME/

COPY gradle $APP_HOME/gradle

COPY . .

RUN ./gradlew clean build

FROM openjdk:11

ENV ARTIFACT_NAME=todolist-0.0.1-SNAPSHOT.jar

ENV APP_HOME=/usr/app

WORKDIR $APP_HOME

COPY --from=TEMP_BUILD_IMAGE $APP_HOME/build/libs/todolist-0.0.1-SNAPSHOT.jar .

EXPOSE 8000

ENTRYPOINT ["java", "-jar", "todolist-0.0.1-SNAPSHOT.jar"]
```

### **Em um docker file o que fazem os comandos: FROM, ENV, WORKDIR, COPY, RUN, EXPOSE e ENTRYPOINT ?**

**FROM:** define a imagem base a ser utilizada para a construção da imagem atual.

**ENV:** define uma variável de ambiente com um valor específico.

**WORKDIR:** define o diretório de trabalho para qualquer comando RUN, CMD, ENTRYPOINT, COPY e ADD que aparecerem no Dockerfile.

**COPY:** copia arquivos ou diretórios do host para o container.

**RUN:** executa um comando no momento da construção da imagem.

**EXPOSE:** informa ao Docker que o container irá "escutar" em uma determinada porta.

**ENTRYPOINT:** define o comando que será executado quando o container for iniciado, sobrescrevendo qualquer outro comando informado no momento da execução.

Eis aqui um exemplo de arquivo `Dockerfile` que utiliza esses comandos:

```Dockerfile
FROM gradle:7.1.1-jdk11 AS TEMP_BUILD_IMAGE

ENV APP_HOME=/usr/app

WORKDIR $APP_HOME

COPY build.gradle settings.gradle gradlew $APP_HOME/

COPY gradle $APP_HOME/gradle

COPY . .

RUN ./gradlew clean build

FROM openjdk:11

ENV ARTIFACT_NAME=todolist-0.0.1-SNAPSHOT.jar

ENV APP_HOME=/usr/app

WORKDIR $APP_HOME

COPY --from=TEMP_BUILD_IMAGE $APP_HOME/build/libs/todolist-0.0.1-SNAPSHOT.jar .

EXPOSE 8000

ENTRYPOINT ["java", "-jar", "todolist-0.0.1-SNAPSHOT.jar"]
```

### **Qual a finalidade de uso do Kubernetes ?**

**TL;DR:** O Kubernetes é uma ferramenta poderosa para ajudar a automatizar e gerenciar aplicativos em contêineres em um ambiente de produção, tornando-os mais escaláveis, resilientes e confiáveis.

O Kubernetes é uma plataforma de orquestração de contêineres, projetada para automatizar a implantação, o dimensionamento e o gerenciamento de aplicativos em contêineres. Ele ajuda a resolver problemas comuns em ambientes de produção, como a escalabilidade, a resiliência, a disponibilidade, a atualização e a implantação de novas versões de aplicativos.

O Kubernetes fornece recursos para gerenciar aplicativos em contêineres em escala, como o dimensionamento automático de contêineres, o balanceamento de carga, a recuperação de falhas, o gerenciamento de configurações e a implantação contínua. Ele também fornece APIs abertas para extensibilidade e integração com outras ferramentas e sistemas.