# Ordenação

Como sabemos se uma classe implementa `Comparable` podemos passar uma lista para `Collections.sort()` e ordernar essa lista, seguem alguns exemplos:

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(10, 50, 23, 56, 100, 98, 1, 12, 2000);

        Collections.sort(numbers);
        System.out.println("Ordenação crescente: " + numbers);

        Collections.sort(numbers, Comparator.reverseOrder());
        System.out.println("Ordenação decrescente: " + numbers);
    }
}
```

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> fruits = Arrays.asList("maçã", "pera", "uva", "amora", "abacaxi");

        Collections.sort(fruits);
        System.out.println("Ordenação crescente: " + fruits);

        Collections.sort(fruits, Comparator.reverseOrder());
        System.out.println("Ordenação decrescente: " + fruits);
    }
```

Porém na vida real nem sempre as coisas são simples, vamos tomar como exemplo a classe Piloto que criamos no artigo anterior:

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
        return this.nome;
    }
}
```

Ela não implementa `Comparable` e nem deveria ! No entanto e se houver necessidade de ordenar os pilotos ? Para realizar tais ordenações vamos precisar de um `Comparator<Piloto>`. Poderíamos criar um comparator que ordenasse os pilotos pelos seus nomes, seria algo mais ou menos assim:

```java
Comparator<Piloto> comparator = new Comparator<Piloto>() {
    @Override
    public int compare(Piloto piloto1, Piloto piloto2) {
        return piloto1.getNome().compareTo(piloto2.getNome());
    }
};
```

Então vamos criar a nossa lista de pilotos

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);
```

Agora vamos adicionar o comparator

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        Comparator<Piloto> comparator = new Comparator<Piloto>() {
            @Override
            public int compare(Piloto piloto1, Piloto piloto2) {
                return piloto1.getNome().compareTo(piloto2.getNome());
            }
        };
```

E por fim aplicar o comparator a lista, fazendo a sua ordenacao

```java
import java.util.Arrays;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);us
        };

        Collections.sort(pilotos, comparator);

        System.out.println(pilotos);
    }
}
```

Essa implementação pode ficar ainda mais enxuta se aplicarmos lambda

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        Comparator<Piloto> comparator = Comparator.comparing(Piloto::getNome);

        pilotos.sort(comparator);

        System.out.println(pilotos);
    }
}
```

Repare que agora a interface `java.util.List` ganhou um novo método chamado `sort()` para o qual basta passar um comparator

```java
public interface List<E> extends Collection<E> {
        ...

        default void sort(Comparator<? super E> c) {
        Object[] a = this.toArray();
        Arrays.sort(a, (Comparator) c);
        ListIterator<E> i = this.listIterator();
        for (Object e : a) {
            i.next();
            i.set((E) e);
        }
    }

}
```

Note que o método `sort()` é um _default method_ assunto que vimos no artigo sobre lambdas. Veja que ele mesmo já tem um Comparator, eliminando assim a necessidade de construirmos um.

Agora se precisarmos ordernar os pilotos por pontos isso seria bem fácil, bastaria fazer assim:

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        Comparator<Piloto> comparator = Comparator.comparing(Piloto::getPontuacao);

        pilotos.sort(comparator);

        System.out.println(pilotos);
    }
}
```

Por baixo dos panos esse código de comparação na verdade está executando o seguinte:

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.function.Function;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        Function<Piloto, Integer> extraiPontos = piloto -> piloto.getPontuacao();

        Comparator<Piloto> comparator = Comparator.comparing(extraiPontos);

        pilotos.sort(comparator);

        System.out.println(pilotos);

    }
}
```

O problema aqui é que o `extraiPontos` gerado, é um método que terá um `apply()`, esse `apply()` recebe um `Piloto` e devolve um `Integer` em vez de um int. Isso significa que cada vez que esse método for invocado vai acontecer uma operação de boxing.

Para evitar esse problema em vez de utilizar a interface `Function` devemos utilizar o `ToIntFunction` e o método `comparingIint()`, sendo assim nosso código ficaria mais ou menos assim:

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.function.ToIntFunction;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        ToIntFunction<Piloto> extraiPontos = piloto -> piloto.getPontuacao();

        Comparator<Piloto> comparator = Comparator.comparingInt(extraiPontos);

        pilotos.sort(comparator);

        System.out.println(pilotos);

    }
}
```

Claro que podemos e devemos utilizar uma versão mais enxuta, melhorando a legibilidade e eliminando variáveis intermediárias:

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;

public class Main {
    public static void main(String[] args) {

        Piloto senna = new Piloto("Ayrton Senna", 100);
        Piloto prost = new Piloto("Alain Prost", 10);
        Piloto gasly = new Piloto("Pierre Gasly", 5);

        List<Piloto> pilotos = Arrays.asList(senna, prost, gasly);

        pilotos.sort(Comparator.comparingInt(Piloto::getPontuacao));

        System.out.println(pilotos);

    }
}
```
