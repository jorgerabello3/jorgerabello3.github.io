# Method References

Method references no Java 8 são uma forma concisa de referenciar métodos existentes (ou construtores) em vez de escrever uma expressão lambda para invocar esses métodos. Eles permitem que você passe um método como argumento para outro método ou utilize-o em situações onde se espera uma expressão lambda. As method references tornam o código mais legível, especialmente quando se está apenas delegando uma chamada de método existente.

Digamos que queremos tornar todos os nossos pilotos campeões do mundo, a princípio poderíamos pensar em um código mais ou menos assim:

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        pilotos.forEach(piloto -> piloto.tornarCampeaoMundial());

        pilotos.forEach(piloto -> {
            System.out.println("\nNOME: " + piloto.getNome());
            System.out.println("CAMPEÃO MUNDIAL: " + piloto.isCampeaoMundial());
        });

    }
}
```

Utilizando method references podemos abreviar um pouco às chamadas aos métodos da classe `Piloto`, você pode utilizar method references para fazer referências à:

- métodos estáticos
- métodos de instância
- contrutores

Nesse exemplo podemos reescrever a parte que torna um piloto campeão mundial assim:

```java
pilotos.forEach(Piloto::tornarCampeaoMundial);
```

Repare que a sintaxe é simples, basta utilizar um tipo, nesse caso `Piloto` e o operador `::` seguido do método que se deseja invocar, nese caso:

Então nosso código todo fica assim:

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        pilotos.forEach(Piloto::tornarCampeaoMundial);

        pilotos.forEach(piloto -> {
            System.out.println("\nNOME: " + piloto.getNome());
            System.out.println("CAMPEÃO MUNDIAL: " + piloto.isCampeaoMundial());
        });

    }
}
```

Isso funciona pois o method reference é traduzido pelo compilador como uma interface funcional. Sabemos que o `forEach()` espera receber um consumidor, o código acima é tratado pelo compilador da mesma forma como o seguinte consumidor:

```java
public interface Iterable<T> {
    default void forEach(Consumer<? super T> action) {
        Objects.requireNonNull(action);
        for (T t : this) {
            action.accept(t);
        }
    }
}
```

Esse mesmo código por baixo dos panos seria algo mais ou menos assim:

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        Consumer<Piloto> tornarCampeaoMundial = piloto -> piloto.tornarCampeaoMundial();

        pilotos.forEach(tornarCampeaoMundial);

    }
}
```

### Method refrences com argumentos

Até agora invocamos métodos sem argumentos, mas e por exemplo quando temos um `System.out.println("alguma coisa")` ? Por exemplo nesse caso:

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 100);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        pilotos.forEach(piloto -> System.out.println(piloto));
    }
}
```

Nesse caso pdemos fazer assim:

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 100);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        pilotos.forEach(System.out::println);
    }
}
```

Esse código terá o mesmo resultado do anterior, isso por que quando escrevemos `System.out::printl` temos um código equivalente ao `pilotos.forEach(piloto -> System.out.println(piloto));`.

Isso funciona por que o compilador saber que ao iterar sob a lista de pilotos, a cada iteração do `forEach()` temos um objeto do tipo `Piloto` e infere que esse parâmetro deve ser passado ao method reference como argumento ou parâmetro.

### Referenciando construtores

Para referenciar construtores precisamos utilizar uma interface funcional chamada `Supplier` presente no pacote `java.util.function`.

```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```

Essa interface tem um tipo de factory, sendo assim é possível guardar a expressão `Piloto::new` em um `Supplier<Piloto>`.

Em outras palavras, podemos utilizar o supplier para criarmos objetos a partir do seu construtor default.

Então para exemplificar vamos criar um construtor que recebe apenas o nome na classe `Piloto`, deixando ela da seguinte forma:

```java
public class Piloto {
    private String nome;
    private int pontuacao;
    private boolean campeaoMundial;

    public Piloto(String nome) {
        this.nome = nome;
    }

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
        return this.nome;
    }
}
```

Agora vamos criar novos pilotos

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {

        Function<String, Piloto> criadorDePilotos = Piloto::new;

        List<Piloto> pilotos = new ArrayList<>();

        pilotos.add(criadorDePilotos.apply("Senna"));
        pilotos.add(criadorDePilotos.apply("Gasly"));
        pilotos.add(criadorDePilotos.apply("Prost"));
        pilotos.add(criadorDePilotos.apply("Leclerc"));

        System.out.println(pilotos);
    }
}
```

Agora que você entendeu como isso funciona, vamos dizer que tenhamos uma situação onde queremos impementar um construtor que receba o nome do piloto e sua pontuação.

Para essa finalidade a API do java conta com uma interface funcional chamada `BiFunction`.

Assim vamos considerar o seguinte construtor da classe `Piloto`

```java
public class Piloto {
    // atributos

    // construtor
    public Piloto(String nome, int pontuacao) {
        this.nome = nome;
        this.pontuacao = pontuacao;
    }

    // getters & setters

    // toString
}
```

E utilizando a `BiFunction` criar a nossa "factory"

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.BiFunction;

public class Main {
    public static void main(String[] args) {

        BiFunction<String, Integer, Piloto> criadorDePilotos = Piloto::new;

        List<Piloto> pilotos = new ArrayList<>();

        pilotos.add(criadorDePilotos.apply("Senna", 1000));
        pilotos.add(criadorDePilotos.apply("Gasly", 980));
        pilotos.add(criadorDePilotos.apply("Prost", 100));
        pilotos.add(criadorDePilotos.apply("Leclerc", 678));

        System.out.println(pilotos);
    }
}
```

## Comparators e Method References

Podemos aplicar method references a fim de simplificar a escrita de comparações, como por exemplo, comparar para ordenar os pilotos pelos seus nomes.

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.function.Consumer;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        pilotos.sort(Comparator.comparing(Piloto::getNome));

        System.out.println(pilotos);

    }
}
```

Podemos deixar esse código ainda mais fluente, fazendo um import estático de `Comparator.comparing()` e criando um `Function<Piloto, String>` chamada `byName`, da seguinte forma:

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.function.Function;

import static java.util.Comparator.comparing;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        Function<Piloto, String> byName = Piloto::getNome;

        pilotos.sort(comparing(byName));

        System.out.println(pilotos);

    }
}
```

## Ordenações Complexas

Agora digamos que desejamos ordernar a nossa lista de pilotos por pontos e em caso de empate ordenar pelo nome.

Para essa finalidade vamos criar um comparator composto, utilizando o método `thenComparing()`

A implementação seria algo mais ou menos assim:

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 100);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        Comparator<Piloto> comparator = Comparator.comparingInt(Piloto::getPontuacao)
                .thenComparing(Piloto::getNome);

        pilotos.sort(comparator);

        System.out.println(pilotos);
    }
}
```

Claro que podemos simplificar ainda mais passando o comparator diretamente para o sort eliminando assim a variável intermediária:

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 100);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        pilotos.sort(Comparator.comparingInt(Piloto::getPontuacao)
                .thenComparing(Piloto::getNome));

        System.out.println(pilotos);
    }
}
```

Agora que conhecemos bem as funcionalidades de ordenação e aprendemos sobre method references, vamos conhecer a nova API de stream no próximo artigo.
