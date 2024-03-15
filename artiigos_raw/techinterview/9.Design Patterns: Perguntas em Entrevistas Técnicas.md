# Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente de design patterns ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Design Patterns

### **Imagine uma classe a qual tem um construtor muito grande com mais de 15 parâmetros. Você vê algum problema nessa implementação ? E se sim como vc aplicaria um refactoring ?**

Um construtor com muitos parâmetros pode tornar o código difícil de entender e manter, além de dificultar a escrita de testes automatizados.

Para refatorar essa situação, podemos utilizar o padrão de design chamado Builder, que separa a construção do objeto em etapas, onde cada etapa define apenas um ou alguns poucos parâmetros. Dessa forma, o código fica mais legível e os valores dos parâmetros podem ser validados a cada etapa da construção.

Para implementar o padrão Builder, podemos criar uma classe Builder dentro da classe que está sendo construída, com métodos que definem os valores dos parâmetros. Esses métodos retornam a própria instância do Builder, permitindo que vários métodos sejam chamados em sequência. No final, o método build é chamado para construir o objeto final com os valores definidos.

### **Imagine que você tenha uma aplicação que faz cálculo de impostos para orçamentos. Sabendo que os impostos são ICC, ICMS, ISS e que cada imposto é calculado de uma determinada maneira, como você implementaria essa classe de cálculo de imposto ? Você conseguiria descrever alguma forma de implementar essa mesma funcionalidade sem fazer uso de if's ?**

Uma abordagem para implementar o cálculo de impostos seria criar uma interface comum, digamos "Imposto", que teria um método "calcular" que seria implementado por cada imposto específico (ICC, ICMS, ISS, etc). Em seguida, cada imposto específico seria implementado em sua própria classe que implementa a interface "Imposto".

Assim, em vez de usar if's para determinar qual cálculo de imposto executar, a classe que realiza o cálculo poderia receber uma lista de objetos "Imposto" e executar o método "calcular" em cada um deles. Dessa forma, cada imposto específico seria responsável por calcular seu próprio valor, sem a necessidade de condicionais no código principal.

Essa abordagem segue o padrão de design "Strategy", que permite a escolha de diferentes algoritmos de cálculo em tempo de execução, sem a necessidade de modificar o código principal.

### **Qual padrão de projeto as annotations do Java implementam ?**

As annotations do Java implementam o padrão de projeto "Decorator", que permite adicionar comportamentos ou informações adicionais a uma classe em tempo de execução sem modificar a classe em si. As annotations fornecem metadados para as classes e seus elementos, permitindo que outras partes do código (como frameworks e bibliotecas) possam processar esses metadados para fornecer comportamentos adicionais ou executar ações específicas.