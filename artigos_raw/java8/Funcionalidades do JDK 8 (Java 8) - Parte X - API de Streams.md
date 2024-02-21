# Aplicação da API de Streams Parte II

# Gerando mapas com coletores

Podemos gerar um stream com todas as linhas dos arquivos que lemos de um determinado diretório:

```java
Stream<String> stream = Files.list(Paths.get(ROOT_PATH))
        .filter(path -> path.toString().endsWith(".txt"))
        .flatMap(path -> lines(path));
```

Podemos ainda ter um stream com a quantidade de linhas de cada arquivo, basta em vez de usar o `flatMap()` utilizar um `map()` para a quantidade de linhas, usando o `count()`, veja:

```java
List<Long> linhas = Files.list(Paths.get(ROOT_PATH))
        .filter(path -> path.toString().endsWith(".txt"))
        .map(path -> lines(path).count())
        .collect(Collectors.toList());
```

Agora e se desejarmos saber quantas linhas tem cada arquivo nesse diretório ? Podemos fazer um `forEach()` e popular um `Map<Path, Long>` no qual a chave é o arquivo e o valor é a quantidade de linhas dele:

```java
Map<Path, Long> linesInFile = new HashMap<>();

Files.list(Paths.get(ROOT_PATH))
        .filter(path -> path.toString().endsWith(".txt"))
        .forEach(path -> linesInFile.put(path, lines(path).count()));
```

Esse código vai produzir uma saída mais ou menos assim:

```bash
{/home/seujorge/meu_arquivo.txt=52}
```

Isso já é bom, já que sob esse approach nos livramos de coisas com o`BufferedReaders` e loops, porém essa solução não é muito funcional, note que a lambda passada para o `forEach()` utiliza uma variável que está declarada fora do seu escopo, assim a lambda altera o estado dessa variável gerando efeitos colaterais, o que afetaria coisas como paralelismo, para resolver esse problema podemos fazer da seguinte forma:

```java
Map<Path, Long> linesInFile = Files.list(Paths.get(ROOT_PATH))
        .filter(path -> path.toString().endsWith(".txt"))
        .collect(Collectors.toMap(path -> path, path -> lines(path).count()));
```

O `toMap()` recebe duas `Functions`. A primeira produzirá a chave (o path) e a segunda o valor (quantidade de linhas), é bem comum precisarmos de uma lambda que retorna o próprio argumento, como fazemos em `path -> path,`, nesse caso podemos utilizar `Function.identity()` para deixar mais claro:

```java
Map<Path, Long> linesInFile = Files.list(Paths.get(ROOT_PATH))
        .filter(path -> path.toString().endsWith(".txt"))
        .collect(Collectors.toMap(Function.identity(), path -> lines(path).count()));
```

Sendo assim saiba que sempre que uma lambda tiver um arquivo tipo `algumaCoisa -> algumaCoisa`, você pode utilizar `Function.identity()`.

Outro exemplo de uso para `Function.identity()`, imagina que queremos mapear todos os pilotos utilizando seus nomes como chave:

A princípio teríamos o seguinte código:

```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

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

        Map<String, Piloto> nomes = pilotos.stream()
                .collect(Collectors.toMap(Piloto::getNome, piloto -> piloto));

        System.out.println(nomes);
    }
}
```

Note que passamos ao `toMap()` um argumento `piloto -> piloto`, podemos alterar esse argumento para `Function.identity()`

```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

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

        Map<String, Piloto> nomes = pilotos.stream()
                .collect(Collectors.toMap(Piloto::getNome, Function.identity()));

        System.out.println(nomes);
    }
}
```

Se a classe `Piloto` fosse uma entidade JPA poderíamos mapear o id e o nome, assim:

```java
pilotos.stream()
        .collect(Collectors.toMap(Piloto::getId, Function.identity()));
```

# Agrupando e particionando

Temos na classe `Piloto` um booleano para dizer se o piloto é ou não um campeão mundial, vamos revistar a classe para lembrar

```java
package br.com.jorgerabellodev.streams.model;

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

Agora vamos criar alguns pilotos

```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.function.Function;
import java.util.stream.Collectors;

public class Main {

    public static void main(String[] args) {

        Piloto prost = new Piloto("Prost", 100, false);
        Piloto gasly = new Piloto("Gasly", 100, false);
        Piloto leclerc = new Piloto("Leclerc", 289, false);
        Piloto senna = new Piloto("Senna", 1000, true);
        Piloto albon = new Piloto("Albon", 562, false);
        Piloto tsunoda = new Piloto("Tsunoda", 967, false);
        Piloto bottas = new Piloto("Bottas", 609, false);
        Piloto hamilton = new Piloto("Hamilton", 1000, true);
        Piloto verstappen = new Piloto("Verstappen", 890, true);

        List<Piloto> pilotos = Arrays.asList(prost, gasly, leclerc, senna, albon, tsunoda, bottas, hamilton, verstappen);

        System.out.println(pilotos);
    }
}
```

Agora queremos um mapa em que a chave seja a pontuação do piloto e o valor seja uma lista de pilotos que possuem aquela pontuação, ou seja queremos um `Map<Integer, List<Piloto>`. Com Java 8 nossa implementação vai ficar mais ou menos assim:

```java
Map<Integer, List<Piloto>> pontuacoesPorPilotos = new HashMap<>();

for (Piloto piloto : pilotos) {
    pontuacoesPorPilotos.computeIfAbsent(piloto.getPontuacao(), driver -> new ArrayList<>()).add(piloto);
}
```

O código todo fica assim:

```java
import java.util.*;

public class Main {

    public static void main(String[] args) {

        Piloto prost = new Piloto("Prost", 100, false);
        Piloto gasly = new Piloto("Gasly", 100, false);
        Piloto leclerc = new Piloto("Leclerc", 289, false);
        Piloto senna = new Piloto("Senna", 1000, true);
        Piloto albon = new Piloto("Albon", 562, false);
        Piloto tsunoda = new Piloto("Tsunoda", 967, false);
        Piloto bottas = new Piloto("Bottas", 609, false);
        Piloto hamilton = new Piloto("Hamilton", 1000, true);
        Piloto verstappen = new Piloto("Verstappen", 890, true);

        List<Piloto> pilotos = Arrays.asList(prost, gasly, leclerc, senna, albon, tsunoda, bottas, hamilton, verstappen);

        Map<Integer, List<Piloto>> pontuacoesPorPilotos = new HashMap<>();

        for (Piloto piloto : pilotos) {
            pontuacoesPorPilotos.computeIfAbsent(piloto.getPontuacao(), driver -> new ArrayList<>()).add(piloto);
        }

        System.out.println(pontuacoesPorPilotos);
    }
}
```

Até o Java 7 precisaríamos de muito mais código para executar essa mesma ação.

O método `computeIfAbsent()` chama a `Function` da lambda no caso de não encontrar um valor para a chave `piloto.getPontuacao()` e associar o resultado (a nova ArrayList) a essa mesma chave, essa chamada para `computeIfAbsent()` faz o papel de um if que seria necessário nesse fluxo.

Esse código funciona, mas queremos trabalhar com streams, então poderíamos pensar em escrever um `Collector` ou trabalhar com o `reduce()` como já vimos, porém existe um Collector que faz exatamente isso:

Assim teríamos um mapa com as pontuações

```java
Map<Integer, List<Piloto>> pontuacoesPorPilotos = pilotos.stream()
        .collect(Collectors.groupingBy(Piloto::getPontuacao));
```

Veja que temos o mesmo resultado do código anterior mas com menos código.

Agora imagina que queremos particionar os pilotos em campeões mundiais e não campeões mundiais. Para essa finalidade existe o `partitioningBy()`

```java
Map<Boolean, List<String>> campeoesENaoCampeoes = pilotos.stream()
        .collect(Collectors.partitioningBy(Piloto::isCampeaoMundial, Collectors.mapping(Piloto::getNome, Collectors.toList())));
```

O código todo seria assim

```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class Main {

    public static void main(String[] args) {

        Piloto prost = new Piloto("Prost", 100, false);
        Piloto gasly = new Piloto("Gasly", 100, false);
        Piloto leclerc = new Piloto("Leclerc", 289, false);
        Piloto senna = new Piloto("Senna", 1000, true);
        Piloto albon = new Piloto("Albon", 562, false);
        Piloto tsunoda = new Piloto("Tsunoda", 967, false);
        Piloto bottas = new Piloto("Bottas", 609, false);
        Piloto hamilton = new Piloto("Hamilton", 1000, true);
        Piloto verstappen = new Piloto("Verstappen", 890, true);

        List<Piloto> pilotos = Arrays.asList(prost, gasly, leclerc, senna, albon, tsunoda, bottas, hamilton, verstappen);

        Map<Boolean, List<String>> campeoesENaoCampeoes = pilotos.stream()
                .collect(Collectors.partitioningBy(Piloto::isCampeaoMundial, Collectors.mapping(Piloto::getNome, Collectors.toList())));

        System.out.println(campeoesENaoCampeoes);
    }
}
```

Vamos tentar somar todas as pontuações dos campeões mundiais e dos campeões mundiais, separadamente, podemos fazer isso facilmente da seguinte forma:

```java
Map<Boolean, Integer> pontuacoes = pilotos.stream()
        .collect(Collectors.partitioningBy(Piloto::isCampeaoMundial, Collectors.summingInt(Piloto::getPontuacao)));
```

Note que até o momento não utilizamos mais loops e mesmo se precisarmos concatenar os nomes dos usuários também seria fácil:

Imagina que queremos a seguinte saída

```bash
Prost, Gasly, Leclerc, Senna, Albon, Tsunoda, Bottas, Hamilton, Verstappen
```

Poderíamos obte-la facilmente assim:

```java
String nomesDosPilotos = pilotos.stream()
        .map(Piloto::getNome)
        .collect(Collectors.joining(", "));
```

Com streams e collectors podemos escrever código mais enxuto em estilo funcional e consequentemente mais expressivo. Além disso produzimos menos efeitos colaterais favorecendo a imutabilidade das coleções e facilitando a paralelização.

# Executando o pipeline em paralelo

Vamos voltar ao exemplo em que precisamos filtrar os pilotos com mais de 900 pontos

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

public class Main {

    public static void main(String[] args) {

        Piloto prost = new Piloto("Prost", 100, false);
        Piloto gasly = new Piloto("Gasly", 100, false);
        Piloto leclerc = new Piloto("Leclerc", 289, false);
        Piloto senna = new Piloto("Senna", 1000, true);
        Piloto albon = new Piloto("Albon", 562, false);
        Piloto tsunoda = new Piloto("Tsunoda", 967, false);
        Piloto bottas = new Piloto("Bottas", 609, false);
        Piloto hamilton = new Piloto("Hamilton", 1000, true);
        Piloto verstappen = new Piloto("Verstappen", 890, true);

        List<Piloto> pilotos = Arrays.asList(prost, gasly, leclerc, senna, albon, tsunoda, bottas, hamilton, verstappen);


        List<Piloto> pilotosComMaisDe900Pontos = pilotos.stream()
                .filter(piloto -> piloto.getPontuacao() > 900)
                .sorted(Comparator.comparing(Piloto::getNome))
                .collect(Collectors.toList());

        System.out.println(pilotosComMaisDe900Pontos);
    }
}
```

Nesse exemplo tudo acontece na mesma thread, se tivermos uma lista muito grande o processo poderá levar muito tempo travando essa thread até que termine sua execução. Seria interessante paralelizar esse processo, visto que atualmente a maioria dos dispositivos, se não todos eles, possuem mais de um processador, ou núcleo de processador, porém escrever código que use Thread para filtrar, ordenar e coletar pode ser trabalhoso. Poderíamos trabalhar com `Fork/Join` mas mesmo assim teríamos bastante trabalho.

A boa notícia é que as collections oferecem uma implementação de stream diferente, o stream paralelo. Esse stream tem capacidade para decidir quantas threads deve utilizar, como deve quebrar o processamento de dados e qual será a forma de unir o resultado.

Para utilizar basta no lugar de invocar o método `stream()` invocar `parallelStream()`

```java
List<Piloto> pilotosComMaisDe900Pontos = pilotos.parallelStream()
        .filter(piloto -> piloto.getPontuacao() > 900)
        .sorted(Comparator.comparing(Piloto::getNome))
        .collect(Collectors.toList());
```

Sendo assim nosso código ficaria assim:

```java
import java.util.Arrays;
import java.util.Comparator;
import java.util.List;
import java.util.stream.Collectors;

public class Main {

    public static void main(String[] args) {

        Piloto prost = new Piloto("Prost", 100, false);
        Piloto gasly = new Piloto("Gasly", 100, false);
        Piloto leclerc = new Piloto("Leclerc", 289, false);
        Piloto senna = new Piloto("Senna", 1000, true);
        Piloto albon = new Piloto("Albon", 562, false);
        Piloto tsunoda = new Piloto("Tsunoda", 967, false);
        Piloto bottas = new Piloto("Bottas", 609, false);
        Piloto hamilton = new Piloto("Hamilton", 1000, true);
        Piloto verstappen = new Piloto("Verstappen", 890, true);

        List<Piloto> pilotos = Arrays.asList(prost, gasly, leclerc, senna, albon, tsunoda, bottas, hamilton, verstappen);


        List<Piloto> pilotosComMaisDe900Pontos = pilotos.parallelStream()
                .filter(piloto -> piloto.getPontuacao() > 900)
                .sorted(Comparator.comparing(Piloto::getNome))
                .collect(Collectors.toList());

        System.out.println(pilotosComMaisDe900Pontos);
    }
}
```

Para esse pequeno exemplo, não faz muita diferença mas em sistemas distribuídos grandes, com uma alta volumetria de dados a performance agradeceria !

Espero que este artigo tenha sido útil e que quem leu tenha, assim como eu, compreendido melhor a API de streams. 

A nossa última parte vai falar sobre a API de datas !