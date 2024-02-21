# API de Streams - Parte III

# Ordenacao

Se precisarmos ordenar uma lista de pilotos por nome, sabemos como fazer:

```java
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


        pilotos.sort(Comparator.comparing(Piloto::getNome));

        System.out.println(pilotos);
    }
}
```

E quanto a um stream ? Vamos imaginar que queremos filtrar os pilotos com mais de 900 pontos e então orderná-los:

```java
Stream<Piloto> stream = pilotos.stream()
        .filter(piloto -> piloto.getPontuacao() > 900)
        .sorted(Comparator.comparing(Piloto::getNome));
```

No caso do stream o método de ordenação é o `sorted()`. Então qual a diferença entre ordernar uma lista com `sort()` e um stream com `sorted()` ? Se você respondeu que a diferença é que o stream não altera quem o gerou, você acertou ! No caso o stream não produz efeitos colaterais na lista de pilotos. Mas e se quisermos o resultado em uma lista ? Se você prestou atenção até aqui, já deve saber que precisaremos de um `Collector`, e que nosso código ficará assim:

```java
List<Piloto> pilotosComMaisDe900Pontos = pilotos.stream()
        .filter(piloto -> piloto.getPontuacao() > 900)
        .sorted(Comparator.comparing(Piloto::getNome))
        .collect(Collectors.toList());
```

Esse mesmo código no Java 7 seria mais ou menos assim:

```java
List<Piloto> pilotosComMaisDe900Pontos = new ArrayList<>();

for (Piloto piloto:pilotos) {
    if (piloto.getPontuacao() > 900) {
        pilotosComMaisDe900Pontos.add(piloto);
    }
}

Collections.sort(pilotosComMaisDe900Pontos, new Comparator<Piloto>() {
    @Override
    public int compare(Piloto o1, Piloto o2) {
        return o1.getNome().compareTo(o2.getNome());
    }
});
```

Veja que antes precisavamos de uma lista temporária, um laço e para esse mesmo filtro uma classe anônima para o `Comparator` até que finalmente teríamos a invocação para a ordenação. Lembre-se sempre, "Time is Money !"

# Operações Lazy

Várias operações em streams são lazy !

Geralmente ao manipular um stream encadeamos diversas operações, a esse conjunto de operações damos o nome de **pipeline** ou **pipeline de operacões**.

O Java consegue tirar proveito dessa estrutura evitando executar operações o máximo possível, pois essas operações apenas serão de fato executadas somente quando realmente for necessário obter seu resultado final.

Vamos a um exemplo, considere a operação a seguir:

```java
Stream<Piloto> stream = pilotos.stream()
        .filter(piloto -> piloto.getPontuacao() > 900)
        .sorted(Comparator.comparing(Piloto::getNome));
```

Note que os métodos `filter()` e `sorted()` devolvem um `Stream`, sendo assim no momento da invocação desses métodos eles nem filtram e nem ordenam, eles apenas devolvem novos streams em que essa informação (ordernar e filtrar) é marcada. Essas são chamadas de **operações intermediárias**.

Os streams retornados sabem que devem ser filtrados e ordenados no momento em que a última operação for invocada, nesse caso o `collect()` é essa última operação, ou **operação terminal**.

Agora você deve estar pensando em qual a vantagem em termos métodos lazy. Para entender vamos a um exemplo prático:

Imagina que precisamos encontrar um piloto com mais de 900 pontos, basta apenas um e qualquer um da lista serve, desde que tenha mais de 900 pontos (cumpra o critério do predicado).

Podemos pensar no seguinte código:

```java
Piloto vencedor = pilotos.stream()
        .filter(piloto -> piloto.getPontuacao() > 900)
        .collect(Collectors.toList())
        .get(0);
```

Tivemos muito trabalho para algo simples, veja que filtramos todos os pilotos, e criamos uma nova coleção com todos eles para pegar o primeiro da lista. Além disso, o que acontece no caso de não haver um piloto com mais de 900 pontos ? Teríamos uma exception ! Sendo assim, pensando nesse tipo de problea a API de streams possui o método `findAny()`, que devolve qualquer um dos elementos, considerando um predicado de um filtro.

```java
Optional<Piloto> vencedor = pilotos.stream()
        .filter(piloto -> piloto.getPontuacao() > 900)
        .findAny();
```

Esse segundo código apresenta duas vantagens:

1. O método `findAny()` devolve um `Optional<Piloto>` e com isso somos obrigados a fazer um `get()` ou usar os métodos de teste como `orElse()` ou `ifPresent()`.

2. Como todo o trabalho foi lazy, o stream **não foi inteiramente filtrado**

O método `findAny()` é uma operação terminal e força a execução do pipiline de oprações, pertence a interface `java.util.stream.Stream<T>` e possui a seguinte assinatura:

```java
Optional<T> findAny();
```

E observe uma de suas implementações:

```java
@Override
public final Optional<P_OUT> findAny() {
    return evaluate(FindOps.makeRef(false));
}
```

Veja que o retorno é um método chamado `evaluate()` que por sua vez recebe como parâmetro uma chamada para o método `makeRef()` da classe `java.util.stream.FindOps`, o `evaluate()` analisa as operações invocadas anteriormente e é inteligente o bastante para saber que não precisa filtrar todos os elementos da lista para pegar apenas um deles.

```java
final <R> R evaluate(TerminalOp<E_OUT, R> terminalOp) {
    assert getOutputShape() == terminalOp.inputShape();
    if (linkedOrConsumed)
        throw new IllegalStateException(MSG_STREAM_LINKED);
    linkedOrConsumed = true;

    return isParallel()
            ? terminalOp.evaluateParallel(this, sourceSpliterator(terminalOp.getOpFlags()))
            : terminalOp.evaluateSequential(this, sourceSpliterator(terminalOp.getOpFlags()));
}
```

Sendo assim o método `findAny()` executa o filtro e assim que encontrar um piloto que cumpra o predicado (nesse caso ter mais de 900 pontos) retorna-lo e termina a filtragem. Se você é curioso o bastante viu que além do `findAny()` existe o `findFirst()`, então qual a diferença entre usar um ou outro ? A diferença é que o `findFirst()` utiliza os elementos na ordem percorrida pelo stream.

Para demonstrar isso vamos utilizar um método chamado `peek()` que nos permite inspecionar elementos do stream.

# Inspecionando elementos do stream - peek()

O método `peek()`` na API de streams do Java 8 é uma operação intermediária que permite inspecionar cada elemento do stream à medida que eles são processados. Ele não modifica os elementos do stream, apenas os "espiões" (peeks) neles, permitindo que você execute ações adicionais sem afetar o fluxo da operação principal da stream.

Para demonstrar vamos alterar os getters para nome e pontuação da classe `Piloto` deixando eles assim:

```java
package br.com.jorgerabellodev.lambda.model;

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
        // adicione esse print
        System.out.println("getNome()");
        return nome;
    }

    public int getPontuacao() {
        // adicione esse print
        System.out.println("getPontuacao()");
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

Agora, imagina que queremos executar uma tarefa toda vez que processar um elemento, nesse exemplo vamos apenas imprimir o nome do primeiro piloto que for encontrado com mais de 900 pontos, para essa finalidade podemos utilizar o método `peek()`

```java
pilotos.stream()
        .filter(piloto -> piloto.getPontuacao() > 900)
        .peek(System.out::println)
        .findAny()
```

Considere o seguinte código:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.BiFunction;

public class Main {
    public static void main(String[] args) {

        BiFunction<String, Integer, Piloto> criadorDePilotos = Piloto::new;

        List<Piloto> pilotos = new ArrayList<>();

        pilotos.add(criadorDePilotos.apply("Prost", 122));
        pilotos.add(criadorDePilotos.apply("Gasly", 180));
        pilotos.add(criadorDePilotos.apply("Leclerc", 789));
        pilotos.add(criadorDePilotos.apply("Senna", 1000));
        pilotos.add(criadorDePilotos.apply("Albon", 562));
        pilotos.add(criadorDePilotos.apply("Tsunoda", 967));
        pilotos.add(criadorDePilotos.apply("Bottas", 609));
        pilotos.add(criadorDePilotos.apply("Hamilton", 967));
        pilotos.add(criadorDePilotos.apply("Verstappen", 890));

        pilotos.stream()
                .filter(piloto -> piloto.getPontuacao() > 900)
                .peek(System.out::println)
                .findAny();
    }
}

```

Execute ele e observe a saída ! Veja que o método `getPontuacao()` foi invovado 4 vezes até encontrar um piloto com mais de 900 pontos.

O método `peek()`, diferente do `forEach()` não devolve void e não é uma operação terminal, ao invés disso ele devolve um novo stream e por tanto somos capazes de realizar outras operações lazy.

Outro exemplo seria o código que ordena pelo nome e imprime o primeiro ordenado:

```java
import java.util.ArrayList;
import java.util.Comparator;
import java.util.List;
import java.util.function.BiFunction;

public class Main {
    public static void main(String[] args) {

        BiFunction<String, Integer, Piloto> criadorDePilotos = Piloto::new;

        List<Piloto> pilotos = new ArrayList<>();

        pilotos.add(criadorDePilotos.apply("Prost", 122));
        pilotos.add(criadorDePilotos.apply("Gasly", 180));
        pilotos.add(criadorDePilotos.apply("Leclerc", 789));
        pilotos.add(criadorDePilotos.apply("Senna", 1000));
        pilotos.add(criadorDePilotos.apply("Albon", 562));
        pilotos.add(criadorDePilotos.apply("Tsunoda", 967));
        pilotos.add(criadorDePilotos.apply("Bottas", 609));
        pilotos.add(criadorDePilotos.apply("Hamilton", 967));
        pilotos.add(criadorDePilotos.apply("Verstappen", 890));

        pilotos.stream()
                .sorted(Comparator.comparing(Piloto::getNome))
                .peek(System.out::println)
                .findAny();
    }
}
```

Note que nesse caso o método `getNome()` foi invocado para todos os elementos da lista.

Veja que o método `peek()` imprime todos os pilotos, mesmo se só quisermos fazer um `findAny()`, isso por que o método `sorted()` é o que se chama de _statefull_ e operações desse tipo podem precisar processar todo o stream, mesmo que sua operação terminal não demande isso.

# Operações de Redução

Uma operação de redução (reduction) é aquela que utiliza os elementos do stream para retornar um valor final, já usamos essas operações, um exemplo é o `average()`.

```java
double media = pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .average()
        .getAsDouble();
```

Há outros métodos de redução, como `count()`, `max()`, `min()` e `sum()`.

O método `sum()`, assim como `average()` encontra-se apenas nos streams primitivos.

Os métodos `min()` e `max()` precisam de um `Comparator` como argumento.

Todos esses métodos exceto `sum()` e `count()` retornam um `Optional`.

Por exemplo se desejarmos somar todos os pontos de todos os pilotos

```java
int total = pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .sum();
```

Por baixo dos panos, essa soma é obtida a partir de uma operação de redução que funciona da seguinte forma:

Primeiro é criado um valor inicial para o somatório:

```java
int valorInicial = 0;
IntBinaryOperator adicao = (a, b) -> a + b;
```

Na sequencia é executada uma lambda que faz a operação de adição `(a, b) -> a + b;` essa operação é o mesmo que escrever o seguinte método:

```java
public int soma(int a, int b) {
    return a + b;
}
```

A diferença é que como é uma lambda não retorna um int e sim um `IntBinaryOperator` que é uma interface funcional

```java
@FunctionalInterface
public interface IntBinaryOperator {
    int applyAsInt(int left, int right);
}
```

Daí com essas informações podemos fazer com que o stream processe a redução passo a passo:

```java
int total = pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .reduce(valorInicial, adicao);
```

Então em uma operação completa de redução teremos os seguintes componentes:

```java
// um valor inicial
int valorInicial = 0;

// uma lambda ou alguma operação - nesse caso uma adição
IntBinaryOperator adicao = (a, b) -> a + b;

// a redução propriamente dita feita com o método reduce que recebe como parâmetro o valor inicial e a operação de redução (nesse caso adição)
int total = pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .reduce(valorInicial, adicao);

// saída
System.out.println(total);
```

Esse código poderia ter sido escrito assim também:

```java
int total = pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .reduce(0, (a, b) -> a + b);
```

Apenas passando os valores para `reduce()`

E é assim que o método `sum()` funciona por baixo dos panos !

Qual a vantagem em utilizar `reduce()` dessa forma, no lugar de simplesmente utilizar `sum()` ? Nenhuma ! Porém é importante que você conheça esse método para poder executar determinadas operações em streams, como por exemplo multiplicar todos os pontos dos pilotos:

```java
int totalDePontosMultiplicado = pilotos.stream()
        .mapToInt(Piloto::getPontuacao)
        .reduce(1, (a, b) -> a * b);
```

Repare que a lógica é a mesma da soma, temos um valor inicial que é 1, dois argumentos (a, b) e pedimos para retornar a multiplicação entre esses argumentos.