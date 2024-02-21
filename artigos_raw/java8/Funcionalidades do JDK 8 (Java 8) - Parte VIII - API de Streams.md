# API de Streams - Parte IV

# Streams e Iterators

Agora já sabemos que graças ao método `collect()` é possível coletar o resultado de uma pipeline de operações de um stream em uma coleção, mas o que acontece se tentarmos percorrer o stream com um foreach por exemplo ?

Assim funcionaria

```java
for (Piloto piloto:pilotos.stream().collect(Collectors.toList())) {
    System.out.println(piloto.getNome());
}
```

Pois nesse caso estamos percorrendo a lista, mas e se tirar o `collect()` e tentar percorrer o stream ? Algo mais ou menos assim:

```java
for (Piloto piloto:pilotos.stream()) {
    System.out.println(piloto.getNome());
}
```

Aqui teremos um erro de compilação, pois o _enhaced for_ ou `foreach()` pros mais chegados, espera um `Iterable` ou um `Array`. Então você deve estar se perguntando, por que diabos fizeram com que `Stream` não fosse um `Iterable` ? Isso foi feito por que diversas operações terminais de um `Stream` o marcam como já utilizado e se você invocar o `Stream` novamente vai receber uma `IllegalStateException`.

**IMPORTANTE:** Invocar duas vezes `pilotos.stream()` não vai gerar problemas, pois abrimos dois streams diferentes a cada invocação, o problema irá ocorrer somente se você invocar uma operação em um desses streams e depois tentar invoca-lo de novo.

Mas e agora ? Como eu percorro os elementos de um `Stream` ? Podemos fazer isso por meio do método `iterator()` da interface `java.util.stream.BaseStream`

```java
public interface BaseStream<T, S extends BaseStream<T, S>>

    Iterator<T> iterator();

}
```

Podemos obter um `Iterator<Usuario>` da seguinte forma:

```java
Iterator<Piloto> iterator = pilotos.stream().iterator();
```

Logo podemos utilizar esse iterator e percorrer o stream assim

```java
pilotos.stream()
        .iterator()
        .forEachRemaining(System.out::println);
```

Se você prestou atenção e tem um senso crítico desperto, deve estar se questionando, por que diabos eu usaria um iterator de um stream para percorrer ele se eu tenho `forEach()` que já faz isso, tanto em `Collection` quanto no próprio `Stream` ?

Bom, pode ser que você queira, por algum motivo alterar os objetos do stream. Ao utilizar streams paralelos, veremos que não devemos mudar o estado dos objetos que estão nele, pois corremos o risco de ter race conditions.

Um outro motivo é a compatibilidade de API's, pois pode ser que precisemos invocar um método que recebe um `Iterator`.

# Testando Predicados

Vamos falar um pouco sobre o método `filter()`.

Sabemos que ele recebe uma lambda como argumento que é da interface `Predicate`.

```java
@FunctionalInterface
public interface Predicate<T> {
    boolean test(T t);
}
```

Há casos em que queremos testar predicados mas não precisamos da lista filtrada, por exemplo, como saber se há algum elemento daquela lista de pilotos que é campeão mundial ? Então considere o seguinte código inicial:

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {

        Piloto prost = new Piloto("Prost", 122, false);
        Piloto gasly = new Piloto("Gasly", 180, false);
        Piloto leclerc = new Piloto("Leclerc", 789, false);
        Piloto senna = new Piloto("Senna", 1000, true);
        Piloto albon = new Piloto("Albon", 562, false);
        Piloto tsunoda = new Piloto("Tsunoda", 967, false);
        Piloto bottas = new Piloto("Bottas", 609, false);
        Piloto hamilton = new Piloto("Hamilton", 967, true);
        Piloto verstappen = new Piloto("Verstappen", 890, true);

        List<Piloto> pilotos = Arrays.asList(prost, gasly, leclerc, senna, albon, tsunoda, bottas, hamilton, verstappen);

        boolean hasCampeoesMundiais = pilotos.stream()
                .anyMatch(Piloto::isCampeaoMundial);

        System.out.println(hasCampeoesMundiais);
    }
}
```

Nesse caso `Piloto::isCampeaoMundial` é interpretado da mesma forma que `piloto -> piloto.isCampeaoMundial()`, gerando um predicado que testa se um piloto é ou não campeão mundial e devolve um booleano, um detalhe é que o processamento dessa operação vai parar assim que for encontrado no stream algum piloto que seja campeão mundial.

Como vimos o `anyMatch()` retorna true se algum elemento atender o predicado, temos ainda o `allMatch()` que retorna true se todos atenderem o predicado e o `noneMatch()` que retorna true caso nenhum atenda o predicado.

Há ainda muitos outros métodos que você pode explorar, por exemplo `count()` retorna quantos elementos há em um stream, `skip()` que vai pular os n próximos elementos e `limit()` como o próprio nome já diz vai limitar (cortar) o número de elementos.

Na interface `Stream` ainda temos duas outras formas de criar streams sem a existência de uma coleção utilizando `empty()` ou `of()`, sendo que o primeiro cria um stream vazio e o segundo depende do que você passar como argumento.

Exemplo de uso do `Stream.empty()`

```java
Stream<Object> empty = Stream.empty();
```

Exemplo de uso do `Stream.of()`

```java
Piloto senna = new Piloto("Senna", 100);
Piloto hamilton = new Piloto("Hamilton", 10);
Piloto leclerc = new UsuaPilotorio("Leclerc", 21);

Stream<Piloto> stream = Stream.of(senna, hamilton, leclerc);
```

Além disso é possível concatenar streams utilizando `Stream.concat()`

Fora da interface `Stream` ainda é possível produzir streams com:

```java
// regex
Pattern.splitAsStream

// ao ler um arquivo
Files.lines

// ao trabalhar com arrays
Arrays.stream
```

# Streams primitivos

Vamos imaginar um cenário onde você precise definir um stream infinito para gerar por exemplo uma lista de números pseudo aleatórios.

Para essa finalidade podemos utilizar a interface de factory `Supplier`, bastando apenas dizer qual é a regra para a criação dos objetos do stream.

Por exemplo:

```java
Random random = new Random(0);
Supplier<Integer> supplier = () -> random.nextInt();
Stream<Integer> stream = Stream.generate(supplier);
```

Esse stream gerado pelo método `generate()` é lazy, logo não vai gerar infinitos números, eles serão gerados somente à medida que forem necessários. Porém aqui estamos gerando boxing o tempo todo por ter utilizado `Supplier`, como você já deve imaginar temos outros suppliers mais específicos como o `IntSupplier` e streams, como você já sabe como o `IntStream`, além disso podemos nos livrar da variável temporária `supplier`:

```java
Random random = new Random(0);

IntStream stream = IntStream.generate(() -> random.nextInt());
```

Contudo, tenha cuidado, pois qualquer operação que precise passar pelo stream nunca terminará de executar, se por exemplo você invocar `stream.sum()`

```java
Random random = new Random(0);
IntStream stream = IntStream.generate(() -> random.nextInt());
int soma = stream.sum();
```

Esse código nunca terminará de executar !

Para streams infinitos, você pode apenas utilizar o que chamamos de **operações de curto-circuito**.

# Operações de curto-circuito

Essas são operações que não precisam processar todos os elementos do stream. Por exemplo, imagina que você queira apenas os 100 primeiros elementos, daí você utilizaria o `limit()`

```java
Random random = new Random(0);

IntStream stream = IntStream.generate(() -> random.nextInt());

List<Integer> cemElementos = stream.limit(100)
        .boxed()
        .collect(Collectors.toList());
```

Note que fizemos a invocação do método `boxed()` da interace `java.util.stream.IntStream`

```java
public interface IntStream extends BaseStream<Integer, IntStream> {

    Stream<Integer> boxed();

}
```

Repare que ele retorna um `Stream<Integer>` e não um `IntStream`, possibilitando a invocação a `collect()`. Sem essa chamada a `boxed()` teríamos apenas a opção de fazer `IntStream.toArray()` ou então chamar o `collect()` onde passaríamos 3 argumentos, porém nesse caso não teríamos onde armazenar os números.

Esse mesmo código acima pode ser escrito com a interface fluente:

```java
Random random = new Random(0);

List<Integer> cemElementos = IntStream.generate(random::nextInt)
        .limit(100)
        .boxed()
        .collect(Collectors.toList());
```

Em alguns casos pode ser útil para um supplier manter o estado e nesse caso precisamos de uma classe ou classe anônima, pois não podemos declarar atributos dentro de uma lambda.

Para exemplificar esse caso, vamos gerar a sequencia de fibonacci de maneira lazy e imprimir seus 10 primeiros elementos. Para isso vamos criar uma representação de fibonacci com uma classe e fazer com que ela implemente `IntSupplier`:

```java
import java.util.function.IntSupplier;

public class Fibonacci implements IntSupplier {

    private int anterior = 0;
    private int proximo = 1;

    @Override
    public int getAsInt() {
        proximo = proximo + anterior;
        anterior = proximo - anterior;
        return anterior;
    }
}
```

Agora vamos utilizar a nossa classe de fato:

```java
import java.util.stream.IntStream;

public class Main {
    public static void main(String[] args) {

        IntStream.generate(new Fibonacci())
                .limit(10)
                .forEach(System.out::println);

    }
}
```

Além do método `limit()` há outras operações que são de curto-circuito como o `findFirst()`. Mas e se não quisermos ober o primeiro elemento de fibonacci e sim primeiro elemento maior que 100 ? Poderíamos filtrar antes de invocar o `findFirst()`

```java
int proximoDepoisDe100 = IntStream.generate(new Fibonacci())
        .filter(value -> value > 100)
        .findFirst()
        .getAsInt();
```

O `filter()` não é uma operação de curto-circuito pois ele não produz um stream finito dado um stream infinito, logo para que a execução não fique infinita invocamos o `findFirst()` e depois que retonar um optional e para obter o valor de fato utilizamos o método `getAsInt()`.

**ATENÇÃO:** Nesse código que escrevemos ainda existem chances de uma execução infinita, por exemplo se não houvesse na sequencia de Fibonacci um elemento maior que 100.

Os matchers são também de curto-circuito. Para fins de demonstração podemos por exemplo tentar obter todos os números pares da sequencia:

```java
IntStream.generate(new Fibonacci())
                .allMatch(value -> value % 2 == 0);
```

Se por um acaso na sequencia de fibonacci houvesse apenas números pares essa execução seria infinita, pois somente vai retornar false, quando a condição do `allMatch()` falhar e retornar false.

**LEMBRE-SE:** trabalhar com streams infinitos pode ser perigoso, mesmo que você utilize operações de curto-circuito.