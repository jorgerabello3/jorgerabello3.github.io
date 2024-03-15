# API de Streams - Parte II

# Famílias de streams

De uma olhada no código a seguir:

```java
Stream<Integer> stream = pilotos.stream().map(Piloto::getPontuacao);
```

O problema nesse código é que ele gera boxing dos valores inteiros, o que gera um overhead indesejado se formos operar esses dados, em caso de listas grandes isso pode causar diversos problemas e até parar a aplicação dependendo do caso.

A boa notícia é que o pacote `java.util.stream` tem diversas implementações equivalentes ao stream para os principais tipos como `IntStream`, `LongStream`, e `DoubleStream`.

Nesse caso podemos utilizar o `IntStream` a fim de evitar o autoboxing, para isso devemos utilizar no lugar do `map()` o `mapToInt()`, veja só:

```java
IntStream stream = pilotos.stream().mapToInt(Piloto::getPontuacao);
```

Note que `mapToInt()` recebe uma função mais específica chamada `ToIntFunction`

```java
IntStream mapToInt(ToIntFunction<? super T> mapper);
```

Outra coisa legal é que o `IntStream` possui métodos específicos que podem simplificar bastante o trabalho com inteiros, como `max()`, `sorted()` e `average()`.

Por exemplo para obter a pontuação média podemos invovar o método `average()`

```java
double mediaDaPontuacao = pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .average()
        .getAsDouble();
```

# Optional

Note que utilizamos o método `getAsDouble()`, se você foi curioso o bastante pra olhar a implementação desse método viu que ele, antes de retornar o valor faz alguma coisa:

```java
public double getAsDouble() {
    if (!isPresent) {
        throw new NoSuchElementException("No value present");
    }
    return value;
}
```

Essa validação do if tira proveito de uma funcionalidade que chamamos de `optional` e serve para que não seja necessário fazer validações extremas, como por exemplo, se preocupar se o número de pilotos for zero. A classe `java.util.Optional` permite que possamos cuidar desses casos de uma maneira simples.

Além disso há também outras versões primitivas como `OptionalDouble`, `OptionalInt`. O método `average()` do `IntStream`, por exemplo, devolve um `OptionalDouble`

```java
OptionalDouble average();
```

```java
OptionalDouble media = pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .average();
```

Assim sendo podemos utilizar uma interface fluente para cuidarmos de casos como uma lista vazia, podendo implementar dessa forma:

```java
double mediaDaPontuacao = pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .average()
        .orElse(0.0);
```

Nesse caso se a lista estiver vazia fica valendo o valor do método `orElse()` ou seja 0.0. como resultado pra média. Caso haja necessidade de lançar uma exceção, podemos utilizar o `orElseThrow()`, da seguinte forma:

```java
double mediaDaPontuacao = pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .average()
        .orElseThrow(IllegalStateException::new);
```

Ainda é possível verificar se dentro do `Optional` existe um valor e caso exista passamos um consumer, algo mais ou menos assim:

```java
pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .average()
        .ifPresent(value -> System.out.println(value));
```

Como o método `ifPresent()` recebe um `DoubleConsumer` e ele é uma interface funcional, logo podemos utilziar method references aqui:

```java
pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .average()
        .ifPresent(System.out::println);
```

O principal benefício do `Optional` é que dessa forma sempre evitamos um `NullPointerException`.

Em um exemplo prático, digamos que queremos o piloto com maior pontuação, para isso já vimos que podemos utilizar o método `max()` e esse método recebe um `Comparator`, veja a assinatura:

```java
Optional<T> max(Comparator<? super T> comparator);
```

Assim poderíamos ter a seguinte implementação:

```java
Optional<Piloto> vencedor = pilotos.stream()
        .max(Comparator.comparingInt(Piloto::getPontuacao));
```

Nesse caso se a lista for vazia não haverá um piloto para ser retornado, e o resultado será um `Optional` vazio, dito isso, você deve verificar se há ou não um piloto presente no resultado, para essa finalidade você pode utilizar o método `get()`, podendo receber um `null` ou algo mais elaborado e seguro como `orElse()` ou `ifPresent()`.

Podemos inclusive trabalhar de forma `lazy` com o `Optional`, digamos que você queira o nome do usuário com mais pontos, sendo assim você pode fazer algo da seguinte forma:

```java
Optional<String> vencedor = pilotos.stream()
        .max(Comparator.comparingInt(Piloto::getPontuacao))
        .map(piloto -> piloto.getNome());
```

Sabendo que `map()` recebe uma `Function` conforme a assinatura abaixo, podemos ser ainda mais resumidos

```java
public<U> Optional<U> map(Function<? super T, ? extends U> mapper) {
    Objects.requireNonNull(mapper);
    if (!isPresent())
        return empty();
    else {
        return Optional.ofNullable(mapper.apply(value));
    }
}
```

E escrever o mesmo código para obter o nome do usuário com mais pontos utilizar `method reference`

```java
Optional<String> vencedor = pilotos.stream()
        .max(Comparator.comparingInt(Piloto::getPontuacao))
        .map(Piloto::getNome);
```

Agora que conhecemos um pouco sobre a API de streams, vamos no próximo artigo saber quais operações são possíveis de serem feitas com essas estruturas.