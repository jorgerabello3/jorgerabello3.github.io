# Aplicação da API de Streams Parte I

Nessa parte final sobre a API de streams do Java 8, gostaria de fazer um exemplo prático utilizando o que vimos até o momento

Utilizando a classe `java.nio.file.Files` que entrou no Java 7 para facilitar a manipulação de arquivos e diretórios trabalhando com a interface `Path` vamos tentar listar todos os arquivos em um diretório.

Saiba que a classe `java.nio.file.Files` possui métodos para trabalhar com `Stream` e que basta apenas pegar o `Stream<Path>` e depois percorrer com um `forEach()`

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class Main {
    private static final String ROOT_PATH = "/home/seujorge";

    public static void main(String[] args) throws IOException {

        Files.list(Paths.get(ROOT_PATH))
                .forEach(System.out::println);
    }
}
```

Agora vamos tentar obter apenas os arquivos:

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class Main {
    private static final String ROOT_PATH = "/home/seujorge";

    public static void main(String[] args) throws IOException {

        Files.list(Paths.get(ROOT_PATH))
                .filter(path -> path.toString().contains(".") || path.toString().startsWith("."))
                .forEach(System.out::println);
    }
}
```

Vamos criar um arquivo e tentar ler os dados desse arquivo:

Para exemplo eu crieei esse arquivo aqui, e chamei de `sources.txt` mas você pode utilizar qualquer outro se quiser:

````txt
Agora crie o arquivo meu_arquivo.txt coloque um conteúdo nele e tente fazer a leitura do arquivo

No meu caso crie o seguinte arquivo:

```txt
seujorge
/etc/apt/sources.list

# deb cdrom:[Ubuntu 22.04.2 LTS _Jammy Jellyfish_ - Release amd64 (20230223)]/ jammy main restricted

# See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to

# newer versions of the distribution.

deb http://br.archive.ubuntu.com/ubuntu/ jammy main restricted

# deb-src http://br.archive.ubuntu.com/ubuntu/ jammy main restricted

## Major bug fix updates produced after the final release of the

## distribution.

deb http://br.archive.ubuntu.com/ubuntu/ jammy-updates main restricted

# deb-src http://br.archive.ubuntu.com/ubuntu/ jammy-updates main restricted

## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
`tu.com/ubuntu/ jammy-updates universe

# deb-src http://br.archive.ubuntu.com/ubuntu/ jammy-updates universe

````

Se você tentou fazer assim

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;

public class Main {
    private static final String ROOT_PATH = "/home/seujorge";

    public static void main(String[] args) throws IOException {

        Files.list(Paths.get(ROOT_PATH))
                .filter(path -> path.toString().endsWith(".txt"))
                .map(path -> Files.lines(path))
                .forEach(System.out::println);
    }
}
```

Saiba que esse código não vai compilar ! Isso por que `Files.lines()` lança uma `IOEXception` bem como `Files.list()`, o problema do `Files.list()` é fácil de resolver basta envolver o código em um `try...catch`, para manter esse exemplo simples, eu apenas adicionei a exception ao metodo `main()` mas obviamente isso não é recomendável.

Agora o problema do `Files.lines()` é complicado pois a implementação da lambda que lança a exception `IOException` e o método `map()` recebe uma `Function`, essa function, tem o método `apply()` e esse método não lança exception em sua assinatura, eis o problema !

Diante disso há duas soluções:

1. Escrever uma classe anônima ou uma lambda definida com as chaves e com o try/catch por dentro.

2. Escrever um método estático simples, que faz o wrap da chamada para evitar a checked exception.

Como eu sou preguiçoso vou optar pela segunda opção ! Sendo assim vamos escrever o seguinte método:

```java
static Stream<String> lines(Path path) {
    try {
        return Files.lines(path);
    } catch (IOException e) {
        throw new RuntimeException(e);
    }
}
```

E agora invocar ele no nosso código escrito anteriormente

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.stream.Stream;

public class Main {
    private static final String ROOT_PATH = "/home/seujorge";

    public static void main(String[] args) throws IOException {

        Files.list(Paths.get(ROOT_PATH))
                .filter(path -> path.toString().endsWith(".txt"))
                .map(path -> lines(path))
                .forEach(System.out::println);
    }

    static Stream<String> lines(Path path) {
        try {
            return Files.lines(path);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

Esse código até compila, mas se você tentou executar viu que a saí ficou um pouco estranha, algo mais ou menos assim:

```bash
java.util.stream.ReferencePipeline$Head@7ba4f24f
```

Isso acontece por que o nosso método `lines()` retorna um `Stream<String>` e o `map()` vai produzir um `Stream<String>`, logo teremos um `Stream<Stream<String>>`, você pode ver isso retirando o `forEach()` e atribuindo o resultado do `map()` a uma variável

```java
Stream<Stream<String>> streamStream = Files.list(Paths.get(ROOT_PATH))
        .filter(path -> path.toString().endsWith(".txt"))
        .map(path -> lines(path));
```

Sendo assim, como resolver esse problema ?

# Achatando um stream com flatMap

Um `flatMap()` vai "achatar" o nosso stream de streams, para utilizar basta apenas trocar a invocação no final do `Stream<String>`

```java
Files.list(Paths.get(ROOT_PATH))
        .filter(path -> path.toString().endsWith(".txt"))
        .flatMap(path -> lines(path))
        .forEach(System.out::println);
```

Então nosso código todo fica assim:

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.stream.Stream;

public class Main {
    private static final String ROOT_PATH = "/home/seujorge";

    public static void main(String[] args) throws IOException {

        Files.list(Paths.get(ROOT_PATH))
                .filter(path -> path.toString().endsWith(".txt"))
                .flatMap(path -> lines(path))
                .forEach(System.out::println);

    }

    static Stream<String> lines(Path path) {
        try {
            return Files.lines(path);
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

Pra que serve o `flatMap()` a final ?

Utilizamos o `flatMap()` sempre que precisamos trabalhar com coleções de coleções e quermos que o resultado da transformação seja reduzido a um stream simples, sem composição.

Vamos a um outro exemplo prático:

Voltando ao nosso exemplo com pilotos, vamos imaginar que temos grupos de pilotos, para isso vamos criar uma classe para representar essa abstração de grupo

```java
import java.util.Collections;
import java.util.HashSet;
import java.util.Set;

public class Grupo {
    private Set<Piloto> pilotos = new HashSet<>();

    public void add(Piloto piloto) {
        pilotos.add(piloto);
    }

    public Set<Piloto> getPilotos() {
        return Collections.unmodifiableSet(this.pilotos);
    }
}
```

Agora iamgina que tenhamos grupos, separando quem fala inglês e quem fala espanhol

```java
import java.io.IOException;

public class Main {

    public static void main(String[] args) throws IOException {

        Grupo englishSpeakers = new Grupo();
        englishSpeakers.add(new Piloto("Senna", 1000));
        englishSpeakers.add(new Piloto("Alesi", 200));

        Grupo spanishSpeakers = new Grupo();
        spanishSpeakers.add(new Piloto("Albon", 983));
        spanishSpeakers.add(new Piloto("Prost", 100));

    }

}
```

Vamos colocar esses grupos dentro de uma coleção

```java
import java.io.IOException;
import java.util.Arrays;
import java.util.List;

public class Main {

    public static void main(String[] args) throws IOException {

        Grupo englishSpeakers = new Grupo();
        englishSpeakers.add(new Piloto("Senna", 1000));
        englishSpeakers.add(new Piloto("Alesi", 200));

        Grupo spanishSpeakers = new Grupo();
        spanishSpeakers.add(new Piloto("Albon", 983));
        spanishSpeakers.add(new Piloto("Prost", 100));

        List<Grupo> grupos = Arrays.asList(englishSpeakers, spanishSpeakers);

    }

}
```

Se tentarmos obter todos os pilotos desses grupos com algo como

```java
grupos.stream()
        .map(grupo -> grupo.getPilotos().stream());
```

Vamos obter um indesejado `Stream<Stream<Piloto>>` daí o `flatMap()` pode ser utilizado para "desembrulhar" esses streams achantando eles:

```java
grupos.stream()
        .flatMap(grupo -> grupo.getPilotos().stream())
        .distinct()
        .forEach(System.out::println);
```

O código todo fica assim:

```java
import java.io.IOException;
import java.util.Arrays;
import java.util.List;

public class Main {

    public static void main(String[] args) throws IOException {

        Grupo englishSpeakers = new Grupo();
        englishSpeakers.add(new Piloto("Senna", 1000));
        englishSpeakers.add(new Piloto("Alesi", 200));

        Grupo spanishSpeakers = new Grupo();
        spanishSpeakers.add(new Piloto("Albon", 983));
        spanishSpeakers.add(new Piloto("Prost", 100));

        List<Grupo> grupos = Arrays.asList(englishSpeakers, spanishSpeakers);

        grupos.stream()
                .flatMap(grupo -> grupo.getPilotos().stream())
                .distinct()
                .forEach(System.out::println);
    }

}
```

Dessa forma obtemos todos os usuários de ambos os grupos, sem repetição, outra forma de fazer isso seria coletar o resultado da pipeline com em um `Set`, daí não seria preciso o `distinct()`.