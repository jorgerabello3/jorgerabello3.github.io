## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente de programação orientada a objeto no Java ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Programação Orientada a Objetos

### **Quais os benefícios do paradigma de programação orientada a objetos ?**

A programação orientada a objetos (POO) é um paradigma de programação que se baseia na ideia de que um programa pode ser modelado como um conjunto de objetos que interagem entre si. Cada objeto tem um estado (atributos) e um comportamento (métodos), e as interações entre objetos ocorrem através de mensagens trocadas entre eles.

Alguns dos principais benefícios do paradigma de programação orientada a objetos são:

1. **Reutilização de código:** POO permite a criação de classes genéricas e reutilizáveis que podem ser utilizadas em diversos contextos, diminuindo a necessidade de reescrever código repetitivo. Isso é possível graças ao conceito de herança, que permite a criação de classes que estendem o comportamento de outras classes já existentes.

2. **Encapsulamento:** POO permite ocultar os detalhes de implementação dos objetos e expor apenas uma interface pública para interagir com eles. Isso protege o objeto de ser manipulado de forma inadequada e torna a manutenção do código mais fácil, já que mudanças internas não afetarão a forma como os objetos são utilizados externamente.

3. **Modularidade:** POO incentiva a criação de classes independentes e autônomas, que podem ser combinadas em um programa maior. Essa modularidade torna o código mais organizado e fácil de entender e manter.

4. **Polimorfismo:** POO permite que objetos de diferentes classes possam ser tratados de forma uniforme através do conceito de polimorfismo. Isso permite a criação de código mais flexível e genérico, que pode lidar com objetos de diferentes tipos sem precisar de tratamento especial para cada um deles.

5. **Abstração:** POO permite a criação de classes abstratas que definem um conjunto de comportamentos comuns a diversas classes, sem definir a implementação desses comportamentos. Essa abstração permite uma maior flexibilidade e facilidade de adaptação do código às mudanças de requisitos.

Esses são apenas alguns dos benefícios da programação orientada a objetos. O paradigma também tem sido amplamente utilizado em desenvolvimento de software por permitir uma melhor organização do código, uma maior facilidade na manutenção e uma maior clareza no entendimento do funcionamento do programa.

### **Quais os pilares do paradigma de programação orientada a objetos ?**

Os pilares do paradigma de programação orientada a objetos são quatro:

1. **Abstração:** A abstração é a habilidade de identificar características essenciais de um objeto e representá-las em um modelo simplificado. Na POO, a abstração é alcançada através da criação de classes e objetos que representam entidades ou conceitos do mundo real. A abstração permite que um objeto seja representado de forma simples e coesa, sem detalhes desnecessários, tornando mais fácil a compreensão e o uso do objeto.

2. **Encapsulamento:** O encapsulamento é a prática de ocultar a implementação interna de um objeto e fornecer uma interface pública para acesso a ele. Através do encapsulamento, os detalhes internos de um objeto ficam ocultos e protegidos, impedindo o acesso direto a eles por outros objetos e garantindo que a manipulação do objeto seja feita de forma segura e consistente.

3. **Herança:** A herança é um mecanismo que permite que uma classe herde características e comportamentos de outra classe já existente, criando uma relação de "é um". Isso significa que uma classe derivada herda todos os atributos e métodos da classe base, além de poder adicionar novos atributos e métodos ou modificar os já existentes. A herança é usada para criar hierarquias de classes, evitando a duplicação de código e permitindo a criação de classes mais especializadas.

4. **Polimorfismo:** O polimorfismo é a capacidade de objetos de diferentes classes serem tratados de forma uniforme, permitindo que um método de uma classe possa ser utilizado por objetos de classes diferentes. Isso é possível porque as classes derivadas herdam os métodos da classe base, mas podem ter comportamentos diferentes. O polimorfismo é usado para criar código flexível e genérico, que pode lidar com diferentes tipos de objetos sem precisar de tratamento especial para cada um deles.

### **O que é e como você utiliza herança ?**

Herança é um dos conceitos fundamentais da programação orientada a objetos que permite que uma classe herde atributos e métodos de outra classe já existente. No Java, a herança é implementada através da utilização da palavra-chave "extends".

Para utilizar herança em Java, primeiro é necessário definir a classe base ou superclasse, que contém os atributos e métodos que serão herdados por outras classes. Em seguida, é possível criar uma ou mais classes derivadas ou subclasses, que herdam os atributos e métodos da superclasse.

A sintaxe para definir uma classe derivada é a seguinte:

```java
public class NomeDaClasseDerivada extends NomeDaSuperclasse {
  // atributos e métodos da classe derivada
}
```

Por exemplo, suponha que temos uma classe Pessoa que define atributos como nome, idade e sexo, além de métodos para obter e definir esses atributos. Podemos criar uma classe Aluno que herda os atributos e métodos de Pessoa, adicionando atributos como número de matrícula e método para calcular a média.

```java
public class Pessoa {
  private String nome;
  private int idade;
  private char sexo;

  public String getNome() {
    return nome;
  }

  public void setNome(String nome) {
    this.nome = nome;
  }

  public int getIdade() {
    return idade;
  }

  public void setIdade(int idade) {
    this.idade = idade;
  }

  public char getSexo() {
    return sexo;
  }

  public void setSexo(char sexo) {
    this.sexo = sexo;
  }
}
```

```java
public class Aluno extends Pessoa {
  private int matricula;
  private double[] notas;

  public int getMatricula() {
    return matricula;
  }

  public void setMatricula(int matricula) {
    this.matricula = matricula;
  }

  public double calcularMedia() {
    // código para calcular a média das notas
  }
}
```

A classe Aluno herda todos os atributos e métodos da classe Pessoa, além de ter seus próprios atributos e métodos específicos. Dessa forma, é possível reutilizar o código da classe Pessoa e estender seu comportamento de forma modular e organizada.

Além da herança simples, o Java também suporta herança múltipla por meio de interfaces, que permitem que uma classe implemente múltiplas interfaces que definem métodos a serem implementados pela classe.

### **O que é e como você utiliza encapsulamento ?**

Encapsulamento é um conceito fundamental da programação orientada a objetos que permite esconder os detalhes de implementação de uma classe e expor apenas uma interface pública para o usuário. Em Java, o encapsulamento é implementado através do uso de modificadores de acesso e dos métodos getters e setters.

Modificadores de acesso em Java:

* **public:** a classe, método ou atributo é acessível a partir de qualquer classe;

* **protected:** a classe, método ou atributo é acessível a partir de classes dentro do mesmo pacote ou subclasses fora do pacote;

* **private:** a classe, método ou atributo é acessível apenas dentro da mesma classe.

Os métodos getters e setters são usados para obter e definir os valores de atributos privados de uma classe. Eles geralmente têm o nome no padrão getXXX e setXXX, onde XXX é o nome do atributo.

Por exemplo, suponha que temos uma classe ContaBancaria que contém um saldo e métodos para depositar e sacar dinheiro da conta. Podemos encapsular o atributo saldo tornando-o privado e fornecendo métodos públicos para acessá-lo e modificá-lo.

```java
public class ContaBancaria {

  private double saldo;

  public double getSaldo() {
    return saldo;
  }

  public void depositar(double valor) {
    saldo += valor;
  }

  public void sacar(double valor) {
    saldo -= valor;
  }

}
```

Dessa forma, o usuário da classe ContaBancaria não tem acesso direto ao atributo saldo, mas pode usar os métodos `getSaldo()`, `depositar()` e `sacar()` para interagir com a conta bancária. Isso garante que o saldo da conta não seja modificado diretamente de fora da classe, mantendo a integridade dos dados.

O encapsulamento também ajuda a manter a coesão e o baixo acoplamento entre as classes, permitindo que elas evoluam independentemente umas das outras e facilitem a manutenção e o reuso de código.

### **O que é e como você utiliza polimorfismo ?**

Polimorfismo é um conceito fundamental da programação orientada a objetos que permite que objetos de diferentes classes possam ser tratados de forma uniforme, por meio do uso de métodos com o mesmo nome e a mesma assinatura, mas com implementações diferentes. Em outras palavras, é a capacidade de um objeto assumir várias formas.

Em Java, o polimorfismo é implementado por meio de dois mecanismos principais:

1. **Sobrescrita de métodos (Override):** Uma classe filha (subclasse) pode sobrescrever um método da classe pai (superclasse), fornecendo sua própria implementação. Quando um objeto da subclasse é criado, o método sobrescrito é chamado em vez do método da superclasse.

Por exemplo:

```java
public class Animal {
   public void fazerBarulho() {
      System.out.println("Um animal está fazendo barulho");
   }
}
```

```java
public class Cachorro extends Animal {
   public void fazerBarulho() {
      System.out.println("O cachorro está latindo");
   }
}
```

```java
public class Gato extends Animal {
   public void fazerBarulho() {
      System.out.println("O gato está miando");
   }
}
```

```java
// Exemplo de utilização do polimorfismo
Animal animal = new Cachorro();
animal.fazerBarulho(); // Output: O cachorro está latindo

animal = new Gato();
animal.fazerBarulho(); // Output: O gato está miando
```

2. **Sobrecarga de métodos (Overload):** Uma classe pode ter vários métodos com o mesmo nome, mas com parâmetros diferentes. Quando um método é chamado, o compilador escolhe o método correto com base nos parâmetros passados.

Por exemplo:

```java
public class Calculadora {
   public int soma(int a, int b) {
      return a + b;
   }

   public double soma(double a, double b) {
      return a + b;
   }
}
```

```java
// Exemplo de utilização do polimorfismo
Calculadora calc = new Calculadora();
int resultadoInt = calc.soma(2, 3); // Output: 5
double resultadoDouble = calc.soma(2.5, 3.5); // Output: 6.0
```

O polimorfismo é muito útil na criação de código reutilizável e de fácil manutenção, pois permite que as classes possam evoluir independentemente umas das outras, facilitando a adição de novas funcionalidades sem quebrar a compatibilidade com as outras classes.

### **O que é e como você utiliza abstração ?**

**TL;DR:** Abstração é um conceito fundamental da programação orientada a objetos que permite modelar objetos do mundo real em um nível de detalhe mais alto e mais próximo da realidade do problema em questão, sem precisar conhecer os detalhes da implementação.

Por exemplo, imagine que você esteja desenvolvendo um sistema para uma loja de livros. Ao modelar um livro como um objeto, você pode definir suas características essenciais, como o título, o autor, o preço, a editora, entre outros. Essas características são abstraídas do mundo real, pois você está se concentrando apenas nas informações mais relevantes para a loja de livros, sem se preocupar com detalhes irrelevantes, como a cor do papel ou a fonte utilizada.

Um exemplo claro do conceito de abstração seria o funcionamento de um carro. Quando acionamos ele para ligar, não precisamos saber quais passos ele faz para colocar o motor em funcionamento. Quando acionamos o freio, não precisamos saber todos os mecanismos que são acionados para fazer o carro frear. Apenas sabemos o que cada objeto ou função do carro produz como resultado.

### **Qual a diferença entre sobrecarga e sobrescrita de métodos ?**

**TL;DR:** A sobrecarga de métodos ocorre quando uma classe tem dois ou mais métodos com o mesmo nome, mas com diferentes parâmetros, enquanto a sobrescrita de métodos ocorre quando uma subclasse fornece uma implementação específica de um método que já é definido em sua superclasse.

Sobrecarga (Overloading) e sobrescrita (Overriding) de métodos são conceitos diferentes na programação orientada a objetos em Java.

A sobrecarga de métodos em Java ocorre quando uma classe tem dois ou mais métodos com o mesmo nome, mas com assinaturas de parâmetros diferentes. Nesse caso, o compilador Java determina qual método deve ser chamado com base nos argumentos fornecidos no momento da chamada do método. Ou seja, a sobrecarga de métodos permite que você tenha vários métodos com o mesmo nome, mas com diferentes parâmetros, para executar diferentes tarefas em uma classe.

Por exemplo, uma classe "Calculadora" pode ter dois métodos com o nome "soma", um que soma dois inteiros e outro que soma dois números de ponto flutuante (float ou double):

```java
public class Calculadora {
    public int soma(int a, int b) {
        return a + b;
    }

    public double soma(double a, double b) {
        return a + b;
    }
}
```

Já a sobrescrita de métodos em Java ocorre quando uma subclasse fornece uma implementação específica de um método que já é definido em sua superclasse. Nesse caso, a assinatura do método na subclasse deve ser exatamente a mesma que na superclasse (mesmo nome, mesma lista de parâmetros e mesmo tipo de retorno). O objetivo é permitir que a subclasse substitua o comportamento padrão do método em sua superclasse por um comportamento específico à subclasse.

Por exemplo, uma superclasse "Animal" pode ter um método "emitirSom" que retorna uma string "som desconhecido", enquanto uma subclasse "Cachorro" pode sobrescrever esse método para retornar "latido":

```java
public class Animal {
    public String emitirSom() {
        return "som desconhecido";
    }
}
```

```java
public class Cachorro extends Animal {
    @Override
    public String emitirSom() {
        return "latido";
    }
}
```

### **Pra que servem construtores ?**

**TL;DR:**  Construtores em Java são usados para inicializar objetos e garantir que eles tenham um estado inicial consistente antes de serem usados. Eles podem ter parâmetros e são invocados automaticamente quando um objeto é criado com o operador new.

Construtores em Java são métodos especiais que são usados para inicializar objetos. Eles são invocados quando um objeto é criado com o operador new. A principal finalidade de um construtor é garantir que os objetos tenham um estado inicial consistente antes de serem usados.

Os construtores podem ter parâmetros, assim como outros métodos, e podem ser usados para definir os valores iniciais dos atributos de um objeto. Quando nenhum construtor é explicitamente definido em uma classe, o compilador Java fornece um construtor padrão sem parâmetros. Esse construtor padrão é usado sempre que um objeto é criado sem especificar argumentos.

A seguir está um exemplo simples de uma classe Java que define um construtor:

```java
public class Pessoa {
    private String nome;
    private int idade;

    public Pessoa(String nome, int idade) {
        this.nome = nome;
        this.idade = idade;
    }

    // getters e setters omitidos para simplificar o exemplo
}
```

Nesse exemplo, o construtor Pessoa é definido com dois parâmetros: nome e idade. Ele inicializa os atributos correspondentes com os valores passados como argumentos.

Ao criar um objeto da classe Pessoa, um construtor é usado para inicializar seus atributos:

```java
Pessoa pessoa1 = new Pessoa("João", 30);
```

Esse código cria um novo objeto Pessoa e passa os valores "João" e 30 para o construtor, que os usa para inicializar os atributos nome e idade do objeto. Depois que o objeto é criado e inicializado, ele pode ser usado para acessar ou modificar seus atributos por meio de getters e setters, por exemplo:


```java
System.out.println(pessoa1.getNome()); // Imprime "João"
pessoa1.setIdade(31);
System.out.println(pessoa1.getIdade()); // Imprime 31
```