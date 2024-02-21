# Publicando imagens no docker hub

## Crie uma conta e faça login

O primeiro passo vai ser criar uma conta no [docker hub](https://hub.docker.com/) e fazer login.

## Faça login na sua máquina

O segundo passo vai ser fazer login com a sua conta do docker hub na sua máquina pra isso abra um terminal e insira os seguintes comandos

```bash
docker login
```

Digite seu username do dockerhub e sua senha.

Se tudo deu certo você deve ver a mensagem `Login Succeeded`

## Faça o build da sua imagem com o seu nome de usuario como prefixo

Entre no diretório onde está seu `Dockerfile` e execute o comando a seguir substituindo `jorgerabello` pelo seu username do docker hub

```bash
docker build -t jorgerabello/minhaimagem . 
```

Após buildar a imagem você pode conferir se realmente ela foi criada seu tamanho

```bash
docker images
REPOSITORY                  TAG       IMAGE ID       CREATED          SIZE
jorgerabello/minhaimagem    latest    d1b062735ecb   24 minutes ago   1.8MB
```

## Faça o upload para o docker hub

Por fim faça o upload da imagem

```bash
docker push jorgerabello/minhaimagem
```

## Testandooo

Para testar basta tentar usar a sua imagem normalmente

```bash
docker run jorgerabello/fullcycle
```

Caso a imagem não exista será feito o download dela e daí será executada
