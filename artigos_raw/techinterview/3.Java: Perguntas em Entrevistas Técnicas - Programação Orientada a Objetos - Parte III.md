## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente de programação orientada a objeto no Java ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Programação Orientada a Objetos

### **Defina acoplamento e coesão.**

Acoplamento e coesão são dois conceitos importantes no paradigma de programação orientada a objetos.

Acoplamento refere-se ao grau de dependência entre duas classes. Quanto maior o acoplamento, mais as classes estão interconectadas e dependentes uma da outra. Isso pode tornar o código mais difícil de entender, modificar e manter.

Por outro lado, coesão refere-se à medida em que os elementos de uma classe estão relacionados entre si e trabalham juntos para atingir um objetivo comum. Quanto maior a coesão, mais focada e clara é a responsabilidade da classe e mais fácil é entender e modificar o código.

Em geral, o objetivo é maximizar a coesão e minimizar o acoplamento no design de um sistema orientado a objetos. Isso é alcançado por meio de boas práticas de design, como o uso de padrões de projeto, interfaces bem definidas, injeção de dependência e outros conceitos relacionados.

Um exemplo de baixa coesão e alto acoplamento pode ser visto em uma classe que realiza várias tarefas diferentes e depende de muitas outras classes para realizar essas tarefas. Isso pode tornar a classe difícil de entender e modificar. Por outro lado, uma classe com alta coesão e baixo acoplamento terá responsabilidades bem definidas e terá pouca ou nenhuma dependência de outras classes. Isso torna o código mais fácil de entender, modificar e manter.

### **Qual a finalidade do finally em um try...catch ?**

O bloco finally é um bloco de código opcional que pode ser adicionado a um bloco try...catch. Ele é executado sempre, independentemente de ter ocorrido ou não uma exceção durante a execução do bloco try. A finalidade do bloco finally é permitir que o programador especifique um código que deve ser executado independentemente do resultado do bloco try.

Em outras palavras, o bloco finally é usado para garantir que um determinado conjunto de instruções seja executado, independentemente do resultado da execução do bloco try. Isso é útil em situações onde é importante garantir que alguns recursos, como arquivos ou conexões de banco de dados, sejam liberados ou fechados, independentemente de ocorrer uma exceção ou não.

A sintaxe básica para um bloco try...catch...finally é a seguinte:

```java
try {
    // código que pode lançar exceções
} catch (Exception e) {
    // tratamento da exceção
} finally {
    // código a ser executado sempre
}
```

O bloco finally pode ser omitido se não houver necessidade de executar um código específico nesse caso. Porém, caso seja necessário garantir que um conjunto específico de instruções seja executado independentemente do resultado do bloco try, o bloco finally é uma opção útil e importante a ser considerada.

### **O que é composição ?**

Composição em programação orientada a objetos é uma técnica que permite que um objeto contenha outros objetos como parte de sua estrutura interna. É uma forma de relacionamento entre classes em que uma classe é composta por instâncias de outras classes, em vez de herdar seus membros.

Em outras palavras, a composição permite que as classes sejam compostas por outras classes, que são tratadas como componentes do objeto principal. Esses componentes podem ser instanciados dentro da classe principal e, a partir disso, a classe principal pode acessar as funcionalidades e propriedades desses componentes.

A composição é geralmente usada para representar relacionamentos do tipo "tem-um". Por exemplo, um carro tem um motor, uma pessoa tem uma casa, etc. Em vez de herdar as propriedades de outra classe, o objeto principal é composto por uma instância da classe relacionada.

A composição é uma alternativa à herança, e pode ser mais flexível e modular em alguns casos. A utilização de composição em vez de herança pode tornar o código mais fácil de manter e modificar, pois a estrutura da classe principal não depende diretamente da estrutura da classe relacionada. Além disso, a composição pode ajudar a evitar problemas como a herança múltipla, que pode ser complexa e difícil de gerenciar.

### **Você perefe usar herança ou composição ? E por que ?**

Composição pode ser mais flexível e modular em muitos casos. Usar a composição em vez da herança pode tornar o código mais fácil de manter e modificar, pois a estrutura da classe principal não depende diretamente da estrutura da classe relacionada. Isso permite que as classes sejam modificadas independentemente umas das outras.

Porém vale lembrar que a escolha entre herança e composição depende do contexto específico de cada problema. O importante é entender as vantagens e desvantagens de cada abordagem e escolher a que melhor se adapta às necessidades do projeto.

*Vantagens da herança:*

1. **Reutilização de código:** A herança permite que uma classe herde as propriedades e métodos de outra classe. Isso significa que as classes filhas podem reutilizar o código das classes pai, o que pode reduzir a quantidade de código repetitivo e facilitar a manutenção do código.

2. **Hierarquia de classes:** A herança permite a criação de uma hierarquia de classes, na qual as classes filhas herdam as propriedades e métodos das classes pai. Isso pode tornar o código mais organizado e fácil de entender.

3. **Polimorfismo:** A herança permite o polimorfismo, que é a capacidade de um objeto de se comportar de diferentes maneiras. Isso pode melhorar a flexibilidade e a modularidade do código.

*Desvantagens da herança:*

1. **Acoplamento:** A herança pode levar a um alto acoplamento entre as classes, o que significa que as mudanças em uma classe podem afetar as outras classes na hierarquia de herança. Isso pode tornar o código mais difícil de manter e modificar.

2. **Complexidade:** A herança pode tornar a estrutura do código mais complexa e difícil de entender, especialmente em hierarquias de classes profundas.

3. **Rigidez:** A herança pode levar a uma rigidez na estrutura do código, pois as mudanças na classe pai podem afetar as classes filhas. Isso pode tornar o código menos flexível e adaptável a mudanças futuras.

*Vantagens da composição:*

1. **Reutilização de código:** A composição permite a criação de objetos reutilizáveis que podem ser usados em diferentes contextos, o que pode reduzir a quantidade de código duplicado e facilitar a manutenção do código.

2. **Flexibilidade:** A composição permite que os objetos sejam compostos de maneiras diferentes para atender a diferentes requisitos. Isso torna o código mais flexível e adaptável a mudanças futuras.

3. **Encapsulamento:** A composição incentiva o encapsulamento, que é a prática de ocultar a complexidade do código dentro de objetos, o que pode tornar o código mais fácil de entender e manter.

*Desvantagens da composição:*

1. **Complexidade:** A composição pode levar a uma complexidade adicional no código, especialmente quando os objetos compostos têm muitas dependências. Isso pode tornar o código mais difícil de entender e manter.

2. **Desempenho:** A composição pode ter um impacto negativo no desempenho, especialmente quando os objetos compostos são grandes e têm muitas dependências.

3. **Dificuldade de modelagem:** A composição pode ser difícil de modelar corretamente em alguns casos, especialmente quando as dependências entre os objetos são muito complexas.

### **Qual a diferença entre checked e unchecked exceptions ?**

**TL;DR:** A diferença entre checked e unchecked exceptions é que as checked exceptions devem ser tratadas ou propagadas, enquanto as unchecked exceptions não precisam ser tratadas ou propagadas. As checked exceptions são usadas para lidar com situações que podem ser previstas e tratadas pelo programador, enquanto as unchecked exceptions são usadas para lidar com situações imprevistas ou incorrigíveis durante a execução do programa.

Em Java, as exceções são divididas em duas categorias: checked exceptions e unchecked exceptions. A principal diferença entre elas é que as checked exceptions são obrigatórias e devem ser tratadas ou propagadas, enquanto as unchecked exceptions não precisam ser tratadas ou propagadas.

*Checked exceptions:*

As checked exceptions são exceções que podem ocorrer durante a execução de um programa, mas que podem ser previstas e tratadas pelo programador. Essas exceções devem ser declaradas na assinatura do método que as lança, usando a palavra-chave "throws". Além disso, se um método lança uma checked exception, qualquer método que o chame deve lidar com a exceção, seja capturando-a com um bloco try-catch ou propagando-a com a palavra-chave "throws".

Exemplos de checked exceptions incluem IOException, FileNotFoundException e SQLException.

*Unchecked exceptions:*

As unchecked exceptions são exceções que ocorrem durante a execução de um programa, mas que não podem ser previstas ou tratadas pelo programador. Essas exceções não precisam ser declaradas na assinatura do método que as lança e não precisam ser tratadas ou propagadas.

Exemplos de unchecked exceptions incluem NullPointerException, ArrayIndexOutOfBoundsException e IllegalArgumentException.

### **Por que utilizar a classe Exception pode ser considerado uma má prática?**

Não é necessariamente uma má prática utilizar a classe Exception em Java. Na verdade, a classe Exception é uma classe abstrata que serve como superclasse para muitas outras classes de exceção que podem ser usadas em Java.

No entanto, a má prática pode ocorrer se você utilizar a classe Exception de maneira inadequada, por exemplo, criando suas próprias subclasses de Exception que não possuem uma finalidade clara ou tratando todas as exceções de forma genérica, sem considerar a origem ou a gravidade da exceção.

Em vez disso, é uma boa prática criar classes de exceção personalizadas e específicas para cada situação em que uma exceção possa ocorrer em seu código. Isso ajuda a tornar o código mais legível, manutenível e escalável, pois permite que você trate as exceções de maneira mais específica e clara.

Além disso, é importante considerar o uso de outras subclasses de exceção mais específicas, como RuntimeException e IOException, dependendo do contexto em que a exceção ocorre. Essas subclasses fornecem mais informações e recursos específicos para tratar diferentes tipos de exceções em Java.

### **Qual a diferença entre utilizar Exception e RuntimeException ?**

Em Java, Exception e RuntimeException são subclasses da classe Throwable, que representa qualquer objeto que possa ser lançado como exceção em um método. Ambas são usadas para sinalizar situações excepcionais que ocorrem durante a execução de um programa.

A principal diferença entre Exception e RuntimeException é que a primeira é uma classe de exceção verificada (checked exception) e a segunda é uma classe de exceção não verificada (unchecked exception).

As exceções verificadas são aquelas que o compilador obriga o programador a lidar com elas, exigindo que sejam declaradas no cabeçalho do método ou tratadas com um bloco try-catch. Isso é feito para garantir que o programador tenha conhecimento de quais exceções podem ocorrer em um método e tome as devidas medidas para lidar com elas.

Já as exceções não verificadas são aquelas que o compilador não verifica se o código está tratando-as ou não. O compilador permite que essas exceções sejam ignoradas, mas isso pode resultar em erros de tempo de execução.

Assim, ao criar uma exceção personalizada, é importante decidir se ela deve ser verificada ou não verificada, com base na gravidade e frequência com que ela pode ocorrer, bem como na possibilidade de lidar com ela adequadamente no código que a invoca.

Em geral, as exceções verificadas são usadas para sinalizar erros que são previsíveis e recuperáveis, enquanto as exceções não verificadas são usadas para sinalizar erros que podem ocorrer de forma imprevisível e que geralmente requerem ação imediata do usuário ou do desenvolvedor para corrigir.

### **Qual a diferença entre utilizar Exception e RuntimeException ?**

Em Java, Exception e RuntimeException são subclasses da classe Throwable, que representa qualquer objeto que possa ser lançado como exceção em um método. Ambas são usadas para sinalizar situações excepcionais que ocorrem durante a execução de um programa.

A principal diferença entre Exception e RuntimeException é que a primeira é uma classe de exceção verificada (checked exception) e a segunda é uma classe de exceção não verificada (unchecked exception).

As exceções verificadas são aquelas que o compilador obriga o programador a lidar com elas, exigindo que sejam declaradas no cabeçalho do método ou tratadas com um bloco try-catch. Isso é feito para garantir que o programador tenha conhecimento de quais exceções podem ocorrer em um método e tome as devidas medidas para lidar com elas.

Já as exceções não verificadas são aquelas que o compilador não verifica se o código está tratando-as ou não. O compilador permite que essas exceções sejam ignoradas, mas isso pode resultar em erros de tempo de execução.

Assim, ao criar uma exceção personalizada, é importante decidir se ela deve ser verificada ou não verificada, com base na gravidade e frequência com que ela pode ocorrer, bem como na possibilidade de lidar com ela adequadamente no código que a invoca.

Em geral, as exceções verificadas são usadas para sinalizar erros que são previsíveis e recuperáveis, enquanto as exceções não verificadas são usadas para sinalizar erros que podem ocorrer de forma imprevisível e que geralmente requerem ação imediata do usuário ou do desenvolvedor para corrigir.