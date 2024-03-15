## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente de boas práticas, S.O.L.I.D., e Clean Code no Java ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## S.O.L.I.D.

### **Imagine que você está olhando pra um código de um projeto onde tem uma classe chamadaCartoriosController e nessa mesma classe você tem um método para cadastrar um novo cartório, porém nesse método você nota que além de cadastrar o cartório são feitas diversas validações e diversas consultas a api's de terceiros e você começa a perceber que o código desse controller está grande de mais com muitos if's e com muitas dependencias. Você vê algum problema nessa implementação ? E se sim como vc aplicaria um refactoring ?**

Há problemas nessa implementação. Um dos problemas é que a classe CartoriosController está violando o princípio de responsabilidade única (SRP), que diz que uma classe deve ter apenas uma responsabilidade. Além disso, o código com muitos if's e muitas dependências dificulta a manutenção e o teste do código.

Para refatorar esse código, uma opção seria separar as responsabilidades em outras classes. Por exemplo, poderíamos ter uma classe Cartorio para representar a entidade de cartório, uma classe CartorioService para lidar com as validações e regras de negócio relacionadas a cartórios e uma classe CartorioApiClient para lidar com as chamadas às APIs de terceiros relacionadas a cartórios.

O método cadastrarCartorio da classe CartorioController, em vez de lidar com todas as responsabilidades, poderia chamar o CartorioService para realizar as validações e regras de negócio, e o CartorioApiClient para realizar as chamadas às APIs de terceiros, se necessário. A classe CartorioController, assim, ficaria responsável apenas por receber as requisições HTTP e encaminhá-las para as classes responsáveis por lidar com as tarefas específicas.

Essa abordagem, além de melhorar a manutenção e testabilidade do código, também segue o princípio de separação de interesses (SoC), que diz que cada parte do código deve ter apenas uma responsabilidade específica.

### **Imagine que você tem o código de um jogo de fazendinhha - nesse código existe uma superclasse chamada Animal e todas as classes que representam um animal herdam dessa mesma classe. Você vê algum problema nessa implementação ? Se sim qual ?**

Dependendo da forma como a hierarquia de classes Animal e seus filhos foram projetados, pode haver problemas de design na implementação. Um problema comum é a violação do princípio de substituição de Liskov (LSP).

O princípio de substituição de Liskov afirma que, em um programa orientado a objetos, se S é um subtipo de T, então objetos do tipo T podem ser substituídos por objetos do tipo S sem alterar a corretude do programa. Em outras palavras, se uma classe B herda de uma classe A, a classe B deve ser utilizável em qualquer lugar onde a classe A é esperada, sem causar efeitos colaterais indesejados ou quebrar a lógica do programa.

No caso do exemplo dado, se a classe Animal tiver métodos ou comportamentos que não se aplicam a todos os seus filhos, ou se alguns filhos exigirem métodos ou comportamentos específicos que a classe Animal não suporta, então a hierarquia de classes Animal pode violar o princípio de substituição de Liskov. Isso pode levar a erros e dificuldades na manutenção do código.

Uma maneira de resolver esse problema é através da aplicação de interfaces. Por exemplo, poderíamos ter uma interface IAnimal com métodos que todos os animais compartilham, e então as classes de animais poderiam implementar essa interface e, se necessário, criar métodos específicos para seus comportamentos únicos. Dessa forma, não haveria a necessidade de herdar de uma classe com métodos desnecessários e, assim, o princípio de substituição de Liskov seria mantido.

### **Você poderia por favor descrever o que é S.O.L.I.D. e como você aplica no seu dia a dia ?**

S.O.L.I.D é um acrônimo para cinco princípios da programação orientada a objetos que foram definidos por [Robert C. Martin](https://pt.wikipedia.org/wiki/Robert_Cecil_Martin), também conhecido como Uncle Bob. Cada letra representa um princípio que deve ser seguido para criar um software de qualidade e fácil de manter.

**S (Single Responsibility Principle)** - Princípio da Responsabilidade Única: Uma classe deve ter apenas uma responsabilidade, ou seja, ela deve ter apenas um motivo para mudar.

**O (Open-Closed Principle)** - Princípio Aberto-Fechado: Uma classe deve estar aberta para extensão, mas fechada para modificação. Isso significa que você deve ser capaz de adicionar novas funcionalidades sem precisar modificar o código já existente.

**L (Liskov Substitution Principle)** - Princípio da Substituição de Liskov: As classes derivadas devem ser substituíveis por suas classes base. Isso significa que as classes filhas devem respeitar os contratos da classe pai.

**I (Interface Segregation Principle)** - Princípio da Segregação de Interfaces: Uma classe não deve ser forçada a implementar interfaces que ela não usa. Em vez disso, as interfaces devem ser segregadas em interfaces menores e mais específicas.

**D (Dependency Inversion Principle)** - Princípio da Inversão de Dependência: Módulos de alto nível não devem depender de módulos de baixo nível. Em vez disso, ambos devem depender de abstrações.

Na minha rotina de desenvolvimento, aplico esses princípios regularmente para escrever código mais limpo e mais fácil de manter. Por exemplo, utilizo a responsabilidade única para garantir que cada classe tenha apenas uma única responsabilidade, reduzindo a complexidade do código. Também aplico o princípio aberto-fechado para escrever código que possa ser facilmente estendido sem precisar modificar o código existente.

Além disso, utilizo o princípio da segregação de interfaces para garantir que as interfaces sejam menores e mais específicas, o que torna mais fácil para outras classes implementá-las. Também uso o princípio da inversão de dependência para garantir que os módulos de alto nível não dependam dos módulos de baixo nível, tornando mais fácil alterar a implementação sem precisar mudar a interface.

[Para saber mais](https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898)

## Clean Code e Boas Práticas

### **Você conseguiria descrever o que é KISS ?**

KISS é uma sigla que representa o princípio "Keep It Simple, Stupid!" ou "Mantenha Simples, Estúpido!", que é um princípio de design de software que incentiva a simplicidade e a clareza em vez de complexidade desnecessária. Esse princípio pode ser aplicado em todos os aspectos do desenvolvimento de software, incluindo arquitetura, codificação, testes e documentação.

Ao seguir o princípio KISS, os desenvolvedores tentam criar sistemas simples, elegantes e fáceis de entender. Eles evitam adicionar recursos desnecessários, funções complicadas e abstrações complexas. Em vez disso, eles tentam criar soluções simples e diretas para os problemas em questão.

A aplicação do princípio KISS pode levar a um código mais legível, menos propenso a erros e mais fácil de manter. Isso pode levar a um desenvolvimento mais eficiente e uma melhor experiência do usuário.

### **Você conseguiria descrever o que é YAGN ?**
YAGNI é uma sigla que representa o princípio "You Ain't Gonna Need It" ou "Você Não Vai Precisar Disso", que é um princípio de design de software que incentiva os desenvolvedores a evitar a implementação de funcionalidades que ainda não são necessárias ou que possam nunca ser necessárias.

Ao seguir o princípio YAGNI, os desenvolvedores tentam evitar a criação de funcionalidades complexas e desnecessárias, que possam adicionar complexidade e custo desnecessário ao software. Eles se concentram em implementar apenas o que é necessário para atender aos requisitos imediatos do projeto, deixando outras funcionalidades para serem implementadas apenas quando houver a necessidade real delas.

A aplicação do princípio YAGNI pode levar a um código mais simples, menos propenso a erros e mais fácil de manter. Além disso, pode ajudar a economizar tempo e recursos durante o desenvolvimento, permitindo que os desenvolvedores se concentrem no que é realmente importante e necessário.

### **Cite pelo menos 5 boas práticas na escrita de código limpo.**

1. **Nomes significativos:** Utilizar nomes significativos para variáveis, métodos e classes ajuda a tornar o código mais legível e fácil de entender.

2. **Funções pequenas e bem definidas:** É importante criar funções que tenham apenas uma responsabilidade, sejam fáceis de entender e possam ser facilmente reutilizáveis em outros lugares do código.

3. **Comentários claros e concisos:** Comentários devem ser usados apenas para explicar o que não é óbvio no código. É importante que sejam claros e concisos e não poluam o código com informações desnecessárias.

4. **Código bem formatado:** Código bem formatado é mais fácil de ler e entender. Utilizar espaços em branco, indentação e padrões de formatação ajuda a tornar o código mais limpo e organizado.

5. **Testes automatizados:** Testes automatizados são essenciais para garantir a qualidade do código e reduzir a ocorrência de bugs. Eles também ajudam na manutenção do código, pois permitem verificar se as alterações realizadas não impactaram outras partes do sistema.