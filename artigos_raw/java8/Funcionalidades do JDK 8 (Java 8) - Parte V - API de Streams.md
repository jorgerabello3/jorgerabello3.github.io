# API de Streams - Parte I

## Começando com streams

Para esses exemplos, vamos continuar considerando a nossa classe `Piloto` dos artigos anteriores:

```java
public class Piloto {
    private String nome;
    private int pontuacao;
    private boolean campeaoMundial;

    public Piloto(String nome, int pontuacao) {
        this.nome = nome;
        this.pontuacao = pontuacao;
    }

    public Piloto(String nome, int pontuacao, boolean campeaoMundial) {
        this.nome = nome;
        this.pontuacao = pontuacao;
        this.campeaoMundial = campeaoMundial;
    }

    public String getNome() {
        return nome;
    }

    public int getPontuacao() {
        return pontuacao;
    }

    public boolean isCampeaoMundial() {
        return campeaoMundial;
    }

    public void tornarCampeaoMundial() {
        this.campeaoMundial = true;
    }

    @Override
    public String toString() {
        return "Piloto{" +
                "nome='" + nome + '\'' +
                ", pontuacao=" + pontuacao +
                ", campeaoMundial=" + campeaoMundial +
                '}';
    }
}
```

Agora imagine um cenário onde precisamos tornar campeões mundiais os 5 pilotos com mais pontos. Então vamos ter algo mais ou menos assim:

```java
package br.com.jorgerabellodev.ordenacao;

import br.com.jorgerabellodev.lambda.model.Piloto;

import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;
import java.util.function.BiFunction;

public class Main {
    public static void main(String[] args) {

        BiFunction<String, Integer, Piloto> criadorDePilotos = Piloto::new;

        List<Piloto> pilotos = new ArrayList<>();

        pilotos.add(criadorDePilotos.apply("Senna", 1000));
        pilotos.add(criadorDePilotos.apply("Prost", 122));
        pilotos.add(criadorDePilotos.apply("Gasly", 980));
        pilotos.add(criadorDePilotos.apply("Leclerc", 789));
        pilotos.add(criadorDePilotos.apply("Albon", 562));
        pilotos.add(criadorDePilotos.apply("Tsunoda", 967));
        pilotos.add(criadorDePilotos.apply("Bottas", 609));
        pilotos.add(criadorDePilotos.apply("Hamilton", 967));
        pilotos.add(criadorDePilotos.apply("Verstappen", 890));

        pilotos.sort(Comparator.comparing(Piloto::getPontuacao).reversed());

        pilotos.subList(0, 5).forEach(Piloto::tornarCampeaoMundial);

        pilotos.forEach(System.out::println);
    }
}
```

OK ! Isso funciona bem e já é bem melhor do que se precisava fazer antes do Java 8, mas agora como podemos filtrar os usuário com mais de 900 pontos ? Para essa finalidade vamos utilizar o stream.

Um stream nada mais é do que um novo default method que foi adicionado a interface `Collection`.

```java
public interface Collection<E> extends Iterable<E> {

    default Stream<E> stream() {
        return StreamSupport.stream(spliterator(), false);
    }

}
```

Assim teríamos um stream da seguinte forma:

```java
Stream<Piloto> stream = pilotos.stream();
```

E a partir desse stream é possível aplicar filtros com o método `filter()`

```java
Stream<Piloto> stream = pilotos.stream();

stream.filter(piloto -> piloto.getPontos() > 900);
```

Podemos simplificar essas duas linhas da seguinte forma:

```java
pilotos.stream().filter(piloto -> piloto.getPontos > 900);
```

Sendo assim vamos aplicar streams ao nosso código:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.BiFunction;
import java.util.stream.Stream;

public class Main {
    public static void main(String[] args) {

        BiFunction<String, Integer, Piloto> criadorDePilotos = Piloto::new;

        List<Piloto> pilotos = new ArrayList<>();

        pilotos.add(criadorDePilotos.apply("Senna", 1000));
        pilotos.add(criadorDePilotos.apply("Prost", 122));
        pilotos.add(criadorDePilotos.apply("Gasly", 980));
        pilotos.add(criadorDePilotos.apply("Leclerc", 789));
        pilotos.add(criadorDePilotos.apply("Albon", 562));
        pilotos.add(criadorDePilotos.apply("Tsunoda", 967));
        pilotos.add(criadorDePilotos.apply("Bottas", 609));
        pilotos.add(criadorDePilotos.apply("Hamilton", 967));
        pilotos.add(criadorDePilotos.apply("Verstappen", 890));

        Stream<Piloto> pilotosComMaisDe900Pontos = pilotos.stream()
                .filter(piloto -> piloto.getPontuacao() > 900);

        System.out.println("Coleção Original");
        pilotos.forEach(System.out::println);

        System.out.println("\nColeção alterada pelo stream");
        pilotosComMaisDe900Pontos.forEach(System.out::println);
    }
}
```

Veja que para a exibição foi criado um stream, e que os dados originais da coleção que originou o stream permaneceram imutáveis, isso acontece por que o stream não tem efeito colateral sobre a coleção que o original.

Outra coisa importante que você deve saber é que **o stream não é uma coleção**, ele não tem capacidade para guardar dados, **ele apenas utiliza uma coleção** ou alguma outra fonte para trabalhar os dados dessa fonte, esse trabalho pode ou não ser feito encadeando ações no que podemos chamar de **pipeline de operações**. Por exemplo:

## Obtendo uma lista de stream

Já que o `forEach()` é um método que retorna void e `filter()` retorna um `Stream<T>` como obter uma lista ? Para essa finalidade vamos utilizar os collectors.O método `collect()` pode ser utilizado para resgatar elementos do nosso `Stream<T>` para uma lista. Vamos dar uma olhada na assinatura desse método:

```java
<R> R collect(Supplier<R> supplier,
              BiConsumer<R, ? super T> accumulator,
              BiConsumer<R, R> combiner);
```

Vale lembrar que esse mesmo método possui diversas sobrecargas, podendo alterar seus parâmetros. Observando essa assinatura note que o método recebe três argumentos, que são interfaces funcionais, onde:

`Supplier<R> supplier`
É um factory que vai criar o objeto que será retornado ao final da coleta.

`BiConsumer<R, ? super T> accumulator`
Esse segundo parâmetro representa o método que será invocado para adicionar cada elemento.

`BiConsumer<R, R> combiner`
Por fim o terceiro parâmetro pode ser invocado se por exemplo formos usar uma entratégia de coletar elementos paralelamente.

Explicado o código vamos a sua aplicação prática:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.BiConsumer;
import java.util.function.BiFunction;
import java.util.function.Supplier;

public class Main {
    public static void main(String[] args) {

        BiFunction<String, Integer, Piloto> criadorDePilotos = Piloto::new;

        List<Piloto> pilotos = new ArrayList<>();

        pilotos.add(criadorDePilotos.apply("Senna", 1000));
        pilotos.add(criadorDePilotos.apply("Prost", 122));
        pilotos.add(criadorDePilotos.apply("Gasly", 980));
        pilotos.add(criadorDePilotos.apply("Leclerc", 789));
        pilotos.add(criadorDePilotos.apply("Albon", 562));
        pilotos.add(criadorDePilotos.apply("Tsunoda", 967));
        pilotos.add(criadorDePilotos.apply("Bottas", 609));
        pilotos.add(criadorDePilotos.apply("Hamilton", 967));
        pilotos.add(criadorDePilotos.apply("Verstappen", 890));

        Supplier<ArrayList<Piloto>> supplier = ArrayList::new;

        BiConsumer<ArrayList<Piloto>, Piloto> accumulator = ArrayList::add;

        BiConsumer<ArrayList<Piloto>, ArrayList<Piloto>> combiner = ArrayList::addAll;

        ArrayList<Piloto> pilotosComMaisDe900Pontos = pilotos.stream()
                .filter(piloto -> piloto.getPontuacao() > 900)
                .collect(supplier, accumulator, combiner);


        pilotosComMaisDe900Pontos.forEach(System.out::println);
    }
}
```

Agora você deve estar pensando, poxa mas isso ficou bem mais complicado, e poderia tentar resumir o código da seguinte forma:

```java
ArrayList<Usuario> usuariosComMaisDeCemPontos = usuarios.stream()
        .filter(usuario -> usuario.getPontos() > 100)
        .collect(ArrayList::new, ArrayList::add, ArrayList::addAll);
```

O que além de apenas mover o problema de lugar ainda faz com que haja perda de legilibilidade.

A boa notícia é que a API simplificou o trabalho pra nós com uma das sobrecargas que comentei acima:

Pra ser mais específico essa aqui:

```java
<R, A> R collect(Collector<? super T, A, R> collector);
```

Note que a interface `Collector` é alguém que tem um supplier, um accumulator e um combiner, e sendo assim podemos ter vários tipos de coletores prontos, dito isso, podemos simplificar o código anterior apenas fazendo o seguinte:

```java
List<Usuario> usuariosComMaisDeCemPontos = usuarios.stream()
        .filter(usuario -> usuario.getPontos() > 100)
        .collect(Collectors.toList());
```

Sendo assim nosso código ficaria assim na íntegra:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.BiFunction;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {

        BiFunction<String, Integer, Piloto> criadorDePilotos = Piloto::new;

        List<Piloto> pilotos = new ArrayList<>();

        pilotos.add(criadorDePilotos.apply("Senna", 1000));
        pilotos.add(criadorDePilotos.apply("Prost", 122));
        pilotos.add(criadorDePilotos.apply("Gasly", 980));
        pilotos.add(criadorDePilotos.apply("Leclerc", 789));
        pilotos.add(criadorDePilotos.apply("Albon", 562));
        pilotos.add(criadorDePilotos.apply("Tsunoda", 967));
        pilotos.add(criadorDePilotos.apply("Bottas", 609));
        pilotos.add(criadorDePilotos.apply("Hamilton", 967));
        pilotos.add(criadorDePilotos.apply("Verstappen", 890));

        List<Piloto> pilotosComMaisDe900Pontos = pilotos.stream()
                .filter(piloto -> piloto.getPontuacao() > 900)
                .collect(Collectors.toList());

        pilotosComMaisDe900Pontos.forEach(System.out::println);
    }
}
```

No que `Collector.toList()` nos devolve um coletor bem parecido com o que tinhamos antes, porém ele devolve uma lista a qual não sabemos se é ou não thread-safe, se é ou não mutável ou qual a sua implementação e esses são detalhes aos quais devemos sempre nos atentar.

```java
public static <T>
Collector<T, ?, List<T>> toList() {
    return new CollectorImpl<>((Supplier<List<T>>) ArrayList::new, List::add,
                                (left, right) -> { left.addAll(right); return left; },
                                CH_ID);
}
```

Se você quiser pode ainda fazer um import estático deixando o código um pouco mais enxuto

```java
List<Piloto> pilotosComMaisDe900Pontos = pilotos.stream()
                .filter(piloto -> piloto.getPontuacao() > 900)
                .collect(toList());
```

Além do `toList()`, também temos outros métodos como o `toSet()` para coletar as informações de um `Stream<Piloto>` em um `Set<Piloto>`

```java
Set<Piloto> pilotosComMaisDe900Pontos = pilotos.stream()
        .filter(piloto -> piloto.getPontuacao() > 900)
        .collect(Collectors.toSet());
```

E há ainda o método `toCollection()` que permite que se escolha a implementação que será devolvida após a coleta, por exemplo:

```java
Set<Piloto> pilotosComMaisDe900Pontos = pilotos.stream()
        .filter(piloto -> piloto.getPontuacao() > 900)
        .collect(Collectors.toCollection(HashSet::new));
```

Note que o método `toCollection()` rebe um `Supplier<T>` que como já vimos é uma interface que funciona como uma factory e tem apenas um único método chamado `get()`, esse método não recebe argumento e devolve `T`.

```java
public static <T, C extends Collection<T>> Collector<T, ?, C> toCollection(Supplier<C> collectionFactory) {
    return new CollectorImpl<>(collectionFactory,
                               Collection<T>::add,(r1, r2) -> { r1.addAll(r2); return r1; },
                               CH_ID);
}
```

Além desses você ainda poderia invocar o método `toArray()` se assim o desejar

```java
Piloto[] pilotosComMaisDe900Pontos = pilotos.stream()
        .filter(piloto -> piloto.getPontuacao() > 900)
        .toArray(Piloto[]::new);
```

## Utilizando map

Vamos imaginar que queremos listar apenas os pontos dos pilotos, podemos fazer isso da seguinte forma:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.BiFunction;

public class Main {
    public static void main(String[] args) {

        BiFunction<String, Integer, Piloto> criadorDePilotos = Piloto::new;

        List<Piloto> pilotos = new ArrayList<>();

        pilotos.add(criadorDePilotos.apply("Senna", 1000));
        pilotos.add(criadorDePilotos.apply("Prost", 122));
        pilotos.add(criadorDePilotos.apply("Gasly", 980));
        pilotos.add(criadorDePilotos.apply("Leclerc", 789));
        pilotos.add(criadorDePilotos.apply("Albon", 562));
        pilotos.add(criadorDePilotos.apply("Tsunoda", 967));
        pilotos.add(criadorDePilotos.apply("Bottas", 609));
        pilotos.add(criadorDePilotos.apply("Hamilton", 967));
        pilotos.add(criadorDePilotos.apply("Verstappen", 890));

        List<Integer> pontos = new ArrayList<>();

        pilotos.forEach(piloto -> pontos.add(piloto.getPontuacao()));

        System.out.println(pontos);
    }
}
```

Porém nessa abordagem precisamos criar uma variável chamada `pontos` para intermediar a operação, além disso esse código causa efeitos colaterais alterando o estado das variáveis e objetos fora do escopo da lambda. Então como escrever esse código sob uma melhor abordagem ? Para essa finalidade vamos utilizar o meptodo `map()` da API de stream, podemos obter o mesmo resultado da seguinte forma:

```java
List<Integer> pontos = pilotos.stream()
        .map(piloto -> piloto.getPontuacao())
        .collect(Collectors.toList());
```

Além disso, se você deuma olhada na implementação do método `map()` viu que ele recebe uma `Function` que é uma interface funcional

```java
public interface Stream<T> extends BaseStream<T, Stream<T>> {
    <R> Stream<R> map(Function<? super T, ? extends R> mapper);
}
```

Sendo assim, por tanto, utilizando o método `map()` podemos utilizar `method reference` da seguinte forma:

```java
List<Integer> pontos = pilotos.stream()
        .map(Piloto::getPontuacao)
        .collect(Collectors.toList());
```

Nosso código completo fica da seguinte forma:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.BiFunction;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {

        BiFunction<String, Integer, Piloto> criadorDePilotos = Piloto::new;

        List<Piloto> pilotos = new ArrayList<>();

        pilotos.add(criadorDePilotos.apply("Senna", 1000));
        pilotos.add(criadorDePilotos.apply("Prost", 122));
        pilotos.add(criadorDePilotos.apply("Gasly", 980));
        pilotos.add(criadorDePilotos.apply("Leclerc", 789));
        pilotos.add(criadorDePilotos.apply("Albon", 562));
        pilotos.add(criadorDePilotos.apply("Tsunoda", 967));
        pilotos.add(criadorDePilotos.apply("Bottas", 609));
        pilotos.add(criadorDePilotos.apply("Hamilton", 967));
        pilotos.add(criadorDePilotos.apply("Verstappen", 890));

        List<Integer> pontos = pilotos.stream()
                .map(Piloto::getPontuacao)
                .collect(Collectors.toList());

        System.out.println(pontos);
    }
}
```