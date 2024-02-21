## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente de programação orientada a objeto no Java ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Programação Orientada a Objetos

### **Você conseguiria exemplificar um cenário onde você utilizaria classes e métodos abstratos ?**

**TL;DR:** O uso de classes e métodos abstratos pode ser muito útil em situações em que precisamos criar uma hierarquia de classes que compartilham comportamentos comuns, mas também têm comportamentos específicos. Isso torna o código mais flexível, expansível e fácil de manter.
Considere uma aplicação que lida com diferentes tipos de formas geométricas, como círculos, quadrados e triângulos. Para calcular a área de cada forma, podemos criar uma classe abstrata chamada "Forma" com um método abstrato "calcularÁrea". Essa classe pode ter subclasses para cada tipo de forma, como "Círculo", "Quadrado" e "Triângulo", que implementam o método "calcularÁrea" de maneira específica para cada forma.

Por exemplo, a classe "Círculo" pode ter um método "calcularÁrea" que usa a fórmula "pi * raio^2" para calcular a área, enquanto a classe "Quadrado" pode ter um método "calcularÁrea" que simplesmente multiplica o comprimento do lado pelo comprimento do lado.

Ao usar classes e métodos abstratos dessa maneira, podemos criar uma hierarquia de classes que permite que nossa aplicação seja flexível e expansível. Podemos facilmente adicionar novos tipos de formas, como "Retângulo" ou "Losango", criando uma nova subclasse da classe "Forma" e implementando o método "calcularÁrea" para essa forma específica.

Além disso, podemos criar uma lista de objetos do tipo "Forma" e chamar o método "calcularÁrea" em cada objeto, independentemente do tipo de forma. Isso torna o código mais limpo e fácil de manter.


### **Qual a função da palavra reservada static ?**

**TL;DR:** A palavra reservada static no Java é usada para criar membros de classe que pertencem à classe em si, em vez de pertencerem a instâncias individuais da classe. Esses membros estáticos podem ser acessados sem a necessidade de criar uma instância da classe e são compartilhados por todas as instâncias da classe.

A palavra reservada `static` no Java é usada para criar membros de classe que pertencem à classe em si, em vez de pertencerem a instâncias individuais da classe. Esses membros são chamados de membros estáticos e podem ser variáveis ou métodos.

Quando uma variável é declarada como estática, ela é compartilhada por todas as instâncias da classe. Isso significa que se uma instância alterar o valor da variável, essa alteração será refletida em todas as outras instâncias. Além disso, as variáveis estáticas podem ser acessadas sem a necessidade de criar uma instância da classe.

Quando um método é declarado como estático, ele pode ser chamado sem a necessidade de criar uma instância da classe. Isso torna o método acessível a partir da classe em si, em vez de depender de uma instância específica da classe. Além disso, os métodos estáticos não podem acessar variáveis de instância da classe, pois essas variáveis só existem em uma instância específica da classe.


### **Qual a função da palavra reservada final ?**

**TL;DR:** A palavra reservada final é usada para definir elementos que não podem ser modificados após a inicialização. Isso pode ser aplicado a variáveis, métodos ou classes para garantir a integridade do código e evitar que ele seja alterado inadvertidamente.

A palavra reservada `final` no Java é usada para definir elementos que não podem ser >modificados após a inicialização. Quando aplicado a variáveis, final indica que o v>alor atribuído à variável não pode ser alterado após a atribuição. Quando aplicado a mtodos, final indica que o método não pode ser sobrescrito nas subclasses. E quando aplicado a classes, "final" indica que a classe não pode ser estendida por outras classes.

Aqui estão alguns exemplos de como a palavra reservada final pode ser usada no Java:

```java
// Uso em variáveis

// Valor constante que não pode ser alterado.
final int NUMERO_MAXIMO = 100; 

// Referência constante para um objeto Pessoa que não pode ser modificada.
final Pessoa pessoa = new Pessoa(); 
```

```java
// uso em métodos
class Animal {
  // Método final que não pode ser sobrescrito pelas subclasses.    
  final void fazerBarulho() { 
    System.out.println("Barulho de um animal.");
  }
}

class Cachorro extends Animal {
  // Tentativa de sobrescrever o método final gerará um erro de compilação.
}
```

```java
// uso em classes

// Classe final que não pode ser estenso
Por outro lado, o método `hashCode()` é usado para obter um código hash que representa o objeto. O código hash é um número inteiro que pode ser usado para identificar de maneira rápida e eficiente se dois objetos são iguais ou diferentes. A implementação padrão do método `hashCode()` na classe Object retorna um código hash baseado no endereço de memória do objeto.

No entanto, se o método `equals()` é sobrescrito, então o método `hashCode()` também deve ser sobrescrito para garantir que objetos iguais tenham o mesmo código hash. Isso é importante porque muitas estruturas de dados em Java, como HashMaps e HashSets, dependem dos códigos hash para identificar se dois objetos são iguais ou diferentes. Se o código hash não for consistente com a implementação do método `equals()`, essas estruturas de dados podem funcionar de forma imprevisível.

Portanto, a sobrescrita correta dos métodos `equals()` e `hashCode()` é importante para garantir que objetos personalizados sejam comparados corretamente em aplicativos Java.

### **Pra que serve o método `toString()` em uma classe Java ?**

**TL;DR:** Em resumo, o método toString() é usado para fornecer uma representação em formato de string de um objeto. A sobrescrita desse método é útil para fornecer uma representação personalizada que inclui informações importantes sobre o objeto, o que pode ser útil para depurar código, exibir informações ao usuário, entre outras situações.

O método `toString()` em Java é usado para obter uma representação em formato de string de um objeto. A implementação padrão do método `toString()` na classe Object retorna uma string que consiste no nome da classe seguido pelo endereço de memória do objeto. Essa representação não é muito útil para entender o conteúdo do objeto e não é muito legível para humanos.

No entanto, é possível sobrescrever o método `toString()` em uma classe personalizada para fornecer uma representação em formato de string mais significativa e legível. Ao sobrescrever o método `toString()`, podemos fornecer uma representação personalizada que inclui informações importantes sobre o objeto. Isso pode ser útil para depurar código, log de mensagens, exibir informações ao usuário, entre outras situações.

Por exemplo, se tivermos uma classe chamada Pessoa que possui campos como nome, idade e endereço, podemos sobrescrever o método `toString()` para retornar uma representação em formato de string que inclua essas informações.

Aqui está um exemplo:

```java
public class Pessoa {
    private String nome;
    private int idade;
    private String endereco;

    // construtor e métodos da classe

    @Override
    public String toString() {
        return "Pessoa [nome=" + nome + ", idade=" + idade + ", endereco=" + endereco + "]";
    }
}
```
Ao chamar o método `toString()` em um objeto Pessoa, a string retornada incluirá os valores dos campos nome, idade e endereço. Isso pode ser útil para depurar código ou exibir informações ao usuário.Por exemplo:

```java
Pessoa p = new Pessoa("João", 25, "Rua A, 123");
// Imprime: Pessoa [nome=João, idade=25, endereco=Rua A, 123]
System.out.println(p.toString()); 
```

### **O que são Generics ?**

**TL;DR:** Em resumo, os Generics são um recurso poderoso da linguagem Java que permitem criar classes, interfaces e métodos que podem ser parametrizados por tipos. Isso torna o código mais flexível, seguro e fácil de reutilizar.

Generics é um recurso da linguagem Java que permite criar classes, interfaces e métodos que podem ser parametrizados por tipos. Em outras palavras, os Generics permitem que você crie componentes de software que possam ser reutilizados com diferentes tipos de dados.

Antes do Java 5, a linguagem não tinha suporte nativo para Generics. Isso significava que, para criar classes que manipulavam diferentes tipos de objetos, era necessário usar o tipo Object e fazer a conversão de tipos manualmente, o que podia levar a erros de compilação e tempo de execução.

Com o suporte para Generics, é possível criar classes, interfaces e métodos que são genéricos em relação aos tipos que manipulam. Isso permite que os programadores escrevam código que possa ser reutilizado com diferentes tipos de dados, aumentando a flexibilidade e a segurança do código.

Por exemplo, suponha que você queira criar uma classe que armazena uma lista de elementos de um determinado tipo. Com o suporte para Generics, é possível criar uma classe genérica que pode armazenar elementos de qualquer tipo. Aqui está um exemplo:

```java
public class Lista<T> {
    private List<T> elementos = new ArrayList<>();

    public void adicionar(T elemento) {
        elementos.add(elemento);
    }

    public T obter(int indice) {
        return elementos.get(indice);
    }
}
```

Nesse exemplo, a classe Lista é genérica em relação ao tipo T. Isso significa que o tipo de dados que a lista armazena pode ser definido no momento em que a classe é instanciada. Por exemplo:

```java
Lista<Integer> listaInteiros = new Lista<>();
listaInteiros.adicionar(1);
listaInteiros.adicionar(2);
System.out.println(listaInteiros.obter(0)); // Imprime: 1

Lista<String> listaStrings = new Lista<>();
listaStrings.adicionar("Hello");
listaStrings.adicionar("World");
System.out.println(listaStrings.obter(0)); // Imprime: Hello
```

### **Quais as diferenças entre o Java 7, 8 e 9?**

O Java 7, 8 e 9 são versões do Java que foram lançadas em anos diferentes e que apresentam diferenças significativas em relação a recursos e melhorias. Aqui estão algumas das diferenças mais notáveis entre essas versões:

**Java 7:**

* Suporte para literais binários e numéricos com sublinhado para melhorar a legibilidade do código; 
* Melhorias na biblioteca de concorrência, incluindo o pacote Fork/Join e o novo framework de concorrência para programação de paralelismo de dados; 
* Suporte para Strings em switch statements; 
* Suporte para multi-catch statements para facilitar o tratamento de exceções; 
* Melhorias na biblioteca de IO, incluindo a classe try-with-resources para facilitar a gestão de recursos; 
*  Melhorias na API de linguagem, incluindo a introdução de Type Inference para instanciar objetos genéricos.

**Java 8:** 

* Introdução de expressões lambda para melhorar a legibilidade do código; 
* Introdução de Streams e interfaces funcionais para simplificar a programação assíncrona; 
* Melhorias na API de data e hora, incluindo a introdução das classes LocalDate, LocalTime e LocalDateTime; 
* Introdução da interface de método padrão para fornecer uma maneira de adicionar métodos às interfaces sem quebrar a compatibilidade com versões anteriores; 
* Melhorias na biblioteca de IO, incluindo o novo pacote de IO NIO2; 
* Introdução de anotações repetidas e tipos anotados para melhorar a legibilidade e organização do código.

**Java 9:** 

* Introdução do módulo de sistema para fornecer uma maneira de encapsular bibliotecas e aplicativos; 
* Introdução de JShell, um shell de linha de comando interativo para testar e experimentar o código Java; 
* Melhorias na API de Streams, incluindo a introdução dos métodos takeWhile() e dropWhile(); 
* Introdução da coleta de lixo G1 como padrão; 
* Suporte para o uso de variáveis privadas em interfaces; 
* Suporte para a classe ProcessHandle para controlar processos em execução.

Essas são apenas algumas das diferenças entre as versões Java 7, 8 e 9. Cada versão apresenta melhorias significativas em relação à anterior, o que torna o Java uma linguagem em constante evolução e aprimoramento.

### **Quais as novidades do Java 11 e 17 ?**

Java 11 e 17 são duas versões recentes do Java, que apresentam diversas novidades e melhorias. Aqui estão algumas das principais mudanças de cada uma delas:

**Java 11:**

* Introdução do método String::repeat para repetir strings;
* Introdução do método Files::readString para ler arquivos como strings;
* Suporte ao HTTP/2 e TLS 1.3 na classe HttpClient;
* Introdução do garbage collector Epsilon, que não executa a coleta de lixo, mas ajuda a identificar pontos de alto uso de memória;
* Remoção do Java EE e do CORBA;
* Melhorias na classe Optional e na API de stream, incluindo a introdução dos métodos takeWhile() e dropWhile().

**Java 17:**

* Introdução do recurso de padrões de correspondência (pattern matching) para instâncias de classe;
* Introdução do tipo de dado primitivo "sealed" para controlar a hierarquia de classes;
* Introdução do recurso de "preview features", que permite que recursos experimentais sejam testados sem habilitá-los completamente;
* Melhorias na API de stream, incluindo a introdução do método Stream.toList() e tream.toUnmodifiableList();
* Suporte para a versão mais recente do HTTP/2;
* Suporte para a migração de código-fonte do JDK para o Git e GitHub.

Essas são apenas algumas das novidades do Java 11 e 17. Cada versão apresenta melhorias significativas em relação à anterior, o que torna o Java uma linguagem em constante evolução e aprimoramento.

### **O que são default methods ?**

Os default methods do Java são uma funcionalidade introduzida na versão 8 da linguagem Java que permite adicionar métodos a uma interface sem quebrar a compatibilidade com as implementações existentes dessa interface.

Antes da introdução dos default methods, as interfaces Java só poderiam definir métodos que não tinham implementação, ou seja, métodos abstratos. Quando uma nova versão de uma interface Java adicionava um novo método abstrato, todas as classes que implementavam essa interface precisavam ser atualizadas para implementar esse novo método.

Com os default methods, é possível adicionar novos métodos a uma interface que têm uma implementação padrão. As classes que implementam essa interface podem optar por implementar ou não o método default. Se uma classe não implementar o método, a implementação padrão será usada. Se uma classe implementar o método, a implementação da classe será usada.

Os default methods são úteis para adicionar novas funcionalidades às interfaces Java sem quebrar a compatibilidade com as implementações existentes. Eles são usados principalmente em interfaces funcionais que definem um único método abstrato, como a interface java.util.function.Function. Com os default methods, é possível adicionar novos métodos auxiliares a essas interfaces sem quebrar a compatibilidade com as implementações existentes.

### **Considerando a API de streams do java 8 pra que serve respectivamente, filter, sorted, map e collect ?**

A API de streams do Java 8 é uma das principais novidades introduzidas na linguagem, permitindo manipular e processar coleções de objetos de forma mais expressiva e eficiente. As operações de stream são divididas em duas categorias: operações intermediárias e operações terminais.

As operações intermediárias transformam ou filtram o stream e retornam outro stream como resultado, permitindo encadear várias operações para criar um pipeline de processamento de dados. Já as operações terminais executam o pipeline e retornam um resultado final.

*Dentre as operações intermediárias, temos as seguintes:*

* **Filter:** recebe um predicado (uma função que retorna um valor booleano) e retorna um stream contendo apenas os elementos que satisfazem o predicado. Exemplo: filtrar uma lista de números e retornar apenas os números pares.

* **Sorted:** retorna um stream ordenado de acordo com um critério especificado. Exemplo: ordenar uma lista de strings em ordem alfabética.

* **Map:** recebe uma função que transforma um elemento em outro e retorna um stream contendo os elementos resultantes da aplicação da função a cada elemento do stream original. Exemplo: mapear uma lista de objetos para uma lista contendo apenas um atributo desses objetos.

* **FlatMap:** é uma variação do Map que retorna um stream plano (flattened), ou seja, que combina os resultados dos streams resultantes da aplicação da função em um único stream. É muito útil para transformar streams de coleções aninhadas em um único stream de elementos. Exemplo: transformar uma lista de listas em uma lista plana contendo todos os elementos das listas aninhadas.

* **Peek:** é uma operação intermediária que permite executar uma ação (um Consumer) em cada elemento do stream sem alterar o stream em si. É útil para fins de debug ou para observar o comportamento do pipeline.

* **Limit:** retorna um stream contendo apenas os primeiros n elementos do stream original. É útil quando não se deseja processar a coleção inteira.

* **Skip:** retorna um stream contendo todos os elementos do stream original, exceto os primeiros n elementos. É útil para descartar elementos indesejados.

*Dentre as operações terminais, temos as seguintes:*

* **Collect:** coleta os elementos do stream em uma coleção ou objeto específico. É a operação terminal mais geral, permitindo especificar uma função de agregação personalizada. Exemplo: coletar os elementos de um stream em uma lista ou conjunto.

* **Reduce:** combina todos os elementos do stream em um único elemento usando uma operação de redução. É útil para calcular estatísticas ou valores agregados. Exemplo: calcular a soma ou média dos elementos de um stream.

* **ForEach:** executa uma ação (um Consumer) em cada elemento do stream, sem retornar nenhum resultado. É útil para efeitos colaterais, como imprimir valores na tela ou atualizar objetos externos.

* **Count:** retorna o número de elementos do stream.

* **AnyMatch:** retorna verdadeiro se pelo menos um elemento do stream satisfaz um predicado.

* **AllMatch:** retorna verdadeiro se todos os elementos do stream satisfazem um predicado.

* **NoneMatch:** retorna verdadeiro se nenhum elemento do stream satisfaz um predicado.