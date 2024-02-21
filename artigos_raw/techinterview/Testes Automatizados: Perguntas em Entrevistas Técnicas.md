## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente sobre Testes Automatizados ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Testes Automatizados

### **Como garantir a qualidade de uma aplicação, sabendo que geralmente existe um time de pessoas fazendo diversas alterações simultâneas no código fonte ?**

Existem várias práticas e ferramentas que podem ser utilizadas para garantir a qualidade de uma aplicação em um ambiente de desenvolvimento colaborativo. Algumas delas são:

1. **Utilização de padrões de código e boas práticas de desenvolvimento:** é importante que toda equipe esteja alinhada quanto aos padrões de código e boas práticas de desenvolvimento para que a aplicação tenha uma estrutura coesa e de fácil manutenção.

2. **Uso de ferramentas de revisão de código:** ferramentas como o GitHub, GitLab, Bitbucket e outras oferecem recursos para revisão de código. Isso permite que cada alteração de código seja avaliada por um ou mais membros da equipe antes de ser integrada ao código-base, aumentando a chance de se detectar erros e problemas de qualidade.

3. **Integração Contínua:** a Integração Contínua (CI) é uma prática que consiste em automatizar a compilação, os testes e o empacotamento da aplicação sempre que uma alteração é feita no código. Isso garante que as alterações sejam validadas rapidamente e que erros sejam detectados o mais cedo possível.

4. **Testes automatizados:** o uso de testes automatizados é fundamental para garantir a qualidade de uma aplicação. É importante que sejam criados testes unitários, testes de integração e testes de aceitação para validar a funcionalidade e a integridade da aplicação.

5. **Análise de código estático:** ferramentas como o SonarQube, PMD e Checkstyle realizam análise estática do código-fonte para detectar possíveis problemas de qualidade, como vulnerabilidades de segurança, código duplicado, baixa cobertura de testes e outras questões que podem afetar a qualidade da aplicação.

6. **Revisão de arquitetura:** é importante que a equipe faça uma revisão periódica da arquitetura da aplicação para garantir que ela esteja adequada às necessidades do negócio e que esteja seguindo as melhores práticas de desenvolvimento.

Ao utilizar essas práticas e ferramentas, é possível garantir que a aplicação tenha uma qualidade adequada mesmo em um ambiente de desenvolvimento colaborativo.

### **Pra que serve o Junit ?**

O JUnit é um framework de teste de software para a linguagem de programação Java. Ele é usado para escrever e executar testes automatizados de unidades (unit tests) para garantir que o código esteja funcionando corretamente.

O framework fornece uma estrutura para escrever testes de forma organizada e automatizada, e também fornece recursos para assertivas (verificações) de resultados esperados, execução de testes em paralelo, entre outros recursos que facilitam a criação e manutenção de testes de software.

Com o JUnit, os desenvolvedores podem escrever testes para verificar se suas classes, métodos e outros componentes estão funcionando de acordo com o esperado, ajudando a garantir a qualidade do software e prevenindo problemas futuros.

### **Quais features o Junit 5 tem que não tem no Junit 4 ?**

O JUnit 5 possui várias novas funcionalidades em relação ao JUnit 4, algumas das quais são:

1. **Extensões:** o JUnit 5 introduziu o conceito de extensões, que permitem adicionar comportamentos personalizados aos testes, como injeção de dependência, interceptação de chamadas, manipulação de exceções, entre outros.

2. **Anotações de parâmetros:** agora é possível usar as anotações `@BeforeEach`, `@AfterEach`, `@BeforeAll` e `@AfterAll` para passar parâmetros para métodos.

3. **Asserções melhoradas:** o JUnit 5 inclui novas asserções, como assertAll(), assertTimeout() e assertThrows(), que tornam mais fácil escrever testes mais sofisticados e legíveis.

4. **Suporte para testes parametrizados aprimorado:** agora é possível usar métodos estáticos ou fábricas de objetos para gerar parâmetros de teste.

5. **Integração com outras ferramentas:** o JUnit 5 pode ser usado com outras ferramentas, como o Gradle e o Maven, para automatizar a execução dos testes.

6. **Anotações de tag:** o JUnit 5 permite marcar os testes com tags personalizadas para executar testes específicos em uma categoria.

7. **Suporte para testes dinâmicos:** com o JUnit 5, agora é possível gerar testes dinamicamente em tempo de execução, o que é útil quando os testes dependem de dados gerados aleatoriamente ou de outros fatores externos.

### **Pra que serve o Mockito ?**

O Mockito é uma biblioteca de mock para testes unitários em Java. Ele é usado para simular objetos e comportamentos em um ambiente de teste para verificar se as funcionalidades estão funcionando corretamente.

Com o Mockito, é possível criar objetos falsos (mocks) para as classes que estão sendo testadas e simular o comportamento de métodos específicos ou comportamentos complexos de objetos reais que não são facilmente reproduzidos em um ambiente de teste. Além disso, o Mockito também permite verificar se determinados métodos foram chamados ou não, bem como a ordem em que foram chamados.

Em resumo, o Mockito ajuda a isolar o código que está sendo testado de dependências externas e a testar comportamentos específicos sem precisar de um ambiente completo para reproduzir esses comportamentos. Isso torna os testes mais confiáveis e mais fáceis de escrever e manter.

### **Qual a finalidade da annotation @Mock ?**

A anotação `@Mock` é usada na biblioteca Mockito para criar um objeto simulado (mock) de uma classe ou interface em um teste. Ela permite simular o comportamento de um objeto real durante a execução do teste, sem a necessidade de criar uma instância real da classe.

Com o `@Mock`, é possível definir o comportamento do objeto simulado, especificando os valores que ele deve retornar para determinados métodos ou como deve se comportar diante de certas condições. Essa abordagem é muito útil para testar um comportamento específico de uma classe sem a necessidade de ter acesso a todos os componentes que ela usa ou interage.


### **Qual a finalidade da annotation @InjectMocks ?**

A annotation `@InjectMocks` é usada em testes de unidade com o framework Mockito para injetar automaticamente instâncias de objetos anotados com `@Mock` em objetos que são instâncias da classe sendo testada.

Quando uma classe é anotada com `@InjectMocks`, o Mockito procura todas as instâncias de objetos que foram anotadas com `@Mock` e as injeta automaticamente no objeto anotado com `@InjectMocks`. Dessa forma, o programador não precisa instanciar manualmente esses objetos para cada teste e pode se concentrar apenas na lógica de teste da classe.

### **Qual a finalidade das annotations @Before, @After, @BeforeAll, @AfterAll, @BeforeEach e @AfterEach ?**

As annotations `@Before`, `@After`, `@BeforeAll`, `@AfterAll`, `@BeforeEach` e `@AfterEach` são usadas no JUnit para definir métodos que são executados antes ou depois da execução de cada método de teste ou antes ou depois da execução de todos os métodos de teste de uma classe de teste.

**@BeforeAll:** é executado uma única vez, antes da execução de todos os métodos de teste da classe. Geralmente é usado para inicialização de recursos compartilhados entre os testes.

**@BeforeEach:** é executado antes da execução de cada método de teste da classe. Geralmente é usado para inicialização de recursos específicos de cada teste.

**@AfterEach:** é executado após a execução de cada método de teste da classe. Geralmente é usado para finalização de recursos específicos de cada teste.

**@AfterAll:** é executado uma única vez, depois da execução de todos os métodos de teste da classe. Geralmente é usado para finalização de recursos compartilhados entre os testes.

**@Before:** é executado antes da execução de cada método de teste da classe, permitindo a configuração de objetos antes da execução dos testes.

**@After:** é executado após a execução de cada método de teste da classe, permitindo a limpeza de objetos após a execução dos testes.

Essas annotations ajudam a garantir que o ambiente de teste seja configurado corretamente antes da execução dos testes e que os recursos sejam liberados após a conclusão dos testes, garantindo a confiabilidade e integridade dos testes.

### **Qual a finalidade de annotation @SpringBootTest ?**

A anotação `@SpringBootTest` é usada para testar o contexto de toda a aplicação Spring em um teste de integração. Ela permite que você crie um teste que carrega o contexto de toda a aplicação Spring, injetando todos os beans gerenciados pelo Spring. Dessa forma, você pode testar como a aplicação se comporta quando todos os componentes são integrados e executados juntos.

Essa anotação é geralmente usada para testes de integração que cobrem todo o fluxo de trabalho da aplicação, desde a camada de persistência até a camada de apresentação. Ela é particularmente útil quando você deseja testar o comportamento da aplicação com o banco de dados, configurações de segurança, configurações de cache, etc.

### **Qual a finalidade de annotation @ExtendWith ?**

A annotation `@ExtendWith` é usada no JUnit 5 para permitir a extensibilidade da plataforma de testes. Essa annotation permite a configuração de uma classe que estende o framework de testes do JUnit 5 para adicionar novos recursos ou modificar o comportamento padrão do JUnit. Isso é especialmente útil para configurações personalizadas, como a injeção de dependência de uma classe de teste. Alguns exemplos de extensões do JUnit 5 incluem o SpringExtension para integração com o Spring Framework e o MockitoExtension para a integração com o Mockito.

### **Qual a finalidade de annotation @RunWith ?**

A anotação `@RunWith` é usada para especificar o Runner que será usado para executar os testes em uma classe JUnit. O JUnit fornece alguns runners internos, como BlockJUnit4ClassRunner, que é o runner padrão para JUnit 4, e JUnitPlatform, que é o runner padrão para JUnit 5.

Além disso, o JUnit permite a criação de runners personalizados, que podem ser usados para estender o comportamento do framework e fornecer recursos adicionais para testes. A anotação `@RunWith` é usada para especificar o runner personalizado a ser usado para executar os testes em uma classe específica.

Por exemplo, o SpringRunner é um runner personalizado fornecido pelo Spring Framework que permite a integração do Spring com testes JUnit. Ao adicionar a anotação `@RunWith(SpringRunner.class)` a uma classe de teste, o Spring é inicializado antes dos testes serem executados, permitindo o uso de anotações do Spring, como `@Autowired` e `@MockBean` nos testes.

### **Qual a finalidade de annotation @MockBean ?**

**TL;DR:** A annotation `@MockBean` é uma poderosa ferramenta de teste que permite que o desenvolvedor substitua beans reais por mocks e crie testes unitários e de integração mais robustos e confiáveis.

A annotation `@MockBean` é uma anotação do Spring que é usada para criar objetos simulados (mocks) de classes ou interfaces e adicioná-los ao contexto de aplicação como beans gerenciados pelo Spring. Essa anotação é comumente usada em testes de unidade e integração para substituir beans reais por mocks e assim permitir que os testes sejam executados de forma isolada e controlada.

Ao contrário da anotação `@Mock` do Mockito, que é usada apenas para criar um mock objeto, a anotação `@MockBean` é usada em conjunto com o contexto de aplicação gerenciado pelo Spring para permitir a injeção de dependências e a configuração de comportamentos específicos para o mock objeto.

## **É possível fazer mock de métodos estáticos ?**

Em geral, não é possível fazer mock de métodos estáticos diretamente com o Mockito, já que a biblioteca trabalha com instâncias de objetos e métodos não-estáticos. No entanto, existem algumas alternativas, como o PowerMock, que possibilita o mock de métodos estáticos, mas pode trazer algumas desvantagens, como deixar o código mais complexo e mais difícil de manter. Além disso, essa prática pode ser vista como um indicativo de código mal projetado, já que métodos estáticos podem ser problemáticos para testar e podem indicar falta de abstração e coesão no design do código.

Porém o mockito core a partir da versão 3.4.0 adicionou a possibilidade de se fazer mocks de métodos estáticos como [nesse exemplo](https://www.baeldung.com/mockito-mock-static-methods) além disso você pode utilizar a biblioteca [PowerMock](https://www.baeldung.com/intro-to-powermock).

### **O que são testes unitários ?**

Testes unitários são uma técnica de teste de software que consiste em isolar e testar individualmente unidades de código, como métodos ou funções. Essas unidades são testadas para garantir que produzam o resultado esperado e funcionem corretamente em diferentes cenários. Os testes unitários são escritos pelos desenvolvedores durante o processo de desenvolvimento e são executados automaticamente para detectar erros e garantir que as mudanças de código não afetem negativamente o restante do sistema. Eles são importantes para garantir a qualidade do código, facilitar a manutenção e aumentar a confiança na entrega de software.


## **Quais as diferenças entre um teste de integração e um testes End To End (E2E) ?**

Um teste de integração verifica se os componentes do sistema funcionam juntos corretamente, ou seja, se as interfaces entre esses componentes funcionam conforme o esperado. Já um teste end-to-end (E2E) verifica o comportamento do sistema como um todo, incluindo todos os seus componentes e suas interações, como se fosse uma simulação de um cenário real.

Algumas diferenças entre esses tipos de testes são:

**Escopo:** o teste de integração tem como escopo verificar a integração entre dois ou mais componentes, enquanto o teste E2E tem como escopo verificar o comportamento do sistema como um todo;

**Profundidade:** os testes de integração podem ter diferentes níveis de profundidade, dependendo do objetivo, enquanto os testes E2E geralmente têm uma profundidade maior;

**Velocidade:** os testes de integração podem ser executados mais rapidamente, uma vez que envolvem apenas alguns componentes, enquanto os testes E2E podem levar mais tempo para serem executados, pois envolvem todo o sistema;

**Ambiente:** os testes de integração geralmente são executados em um ambiente controlado, enquanto os testes E2E geralmente são executados em um ambiente mais próximo do ambiente de produção.

Ambos os tipos de testes são importantes para garantir a qualidade do sistema como um todo e devem ser aplicados de acordo com a necessidade e os objetivos do projeto.


### **O que são Smoke Tests ?**

Smoke Tests (ou testes de fumaça) são testes rápidos que têm como objetivo verificar se as funcionalidades mais básicas e críticas de uma aplicação estão funcionando corretamente após uma alteração significativa ter sido feita no código ou no ambiente em que a aplicação está sendo executada.

Esses testes são geralmente executados após um deploy ou release, antes de prosseguir com testes mais aprofundados, como os testes de integração e testes de aceitação. O objetivo é garantir que a aplicação está minimamente estável e funcional antes de realizar testes mais extensos, economizando tempo e esforço da equipe de desenvolvimento.

### **O que são testes de contrato ?**

Testes de contrato, ou Contract Tests, são testes automatizados que verificam se a integração entre dois sistemas ou componentes está de acordo com um contrato previamente definido. O contrato pode ser um documento, um esquema de dados ou uma interface de programação de aplicativos (API).

Esses testes garantem que as comunicações entre as partes envolvidas estão ocorrendo conforme o esperado, independentemente de como cada componente foi implementado. Isso significa que, se o contrato mudar, esses testes irão alertar sobre eventuais problemas de integração, possibilitando correções antes que os problemas se tornem críticos.

Os testes de contrato são uma prática comum em arquiteturas orientadas a serviços (SOA) e arquiteturas de microsserviços, em que múltiplos componentes precisam se comunicar para formar uma aplicação completa.


### **O que é a pirâmide de testes ?**

A Pirâmide de Testes é um conceito utilizado em engenharia de software que descreve a proporção ideal de cada tipo de teste a ser executado em um projeto. A pirâmide é composta por três camadas principais:

**Testes Unitários:** a base da pirâmide, onde devem estar localizados a maior parte dos testes, são testes automatizados que verificam o comportamento de uma unidade de código (como uma classe ou um método) de forma isolada, sem depender de outros componentes do sistema.

**Testes de Integração:** a camada intermediária da pirâmide, são testes automatizados que verificam se os componentes do sistema funcionam juntos corretamente, detectando erros de integração entre eles.

**Testes de Interface do Usuário (UI):** o topo da pirâmide, onde devem estar localizados os testes mais especializados, são testes automatizados que verificam a interface do usuário e a interação do usuário com o sistema.

A pirâmide de testes sugere que a maioria dos testes deve ser realizada em níveis mais baixos (como os testes unitários) e que a quantidade de testes deve diminuir à medida que a complexidade aumenta (como os testes de interface do usuário). A ideia é que, dessa forma, a cobertura de testes seja ampla, mas o tempo e os recursos necessários para executá-los sejam razoáveis.

### **O que é TDD ?**

**TL;DR:** TDD é uma metodologia de desenvolvimento de software que enfatiza a importância da qualidade do código e da manutenção de testes automatizados. Ao escrever os testes antes do código, o desenvolvedor pode garantir que o software atenda aos requisitos do cliente e seja mais fácil de manter e evoluir.

TDD (Test Driven Development) é uma metodologia de desenvolvimento de software em que os testes são escritos antes mesmo do código do programa. O objetivo é garantir que o software desenvolvido atenda aos requisitos do cliente e seja mais fácil de manter e evoluir ao longo do tempo.

O processo do TDD geralmente começa escrevendo um teste automatizado que falha, em seguida, é escrito o código que faz o teste passar. Esse ciclo de teste/código é repetido várias vezes durante o processo de desenvolvimento, resultando em um conjunto completo de testes automatizados que garantem que o software funciona corretamente.

O TDD enfatiza a importância da qualidade do código e da manutenção de testes automatizados, resultando em software mais robusto e menos propenso a erros. Além disso, como os testes são escritos antes do código, eles servem como uma especificação para o que o software deve fazer, ajudando a garantir que o código atenda aos requisitos do cliente.

Embora o TDD possa parecer um processo mais lento no início, ele geralmente resulta em um desenvolvimento mais rápido e eficiente a longo prazo, pois ajuda a detectar problemas mais cedo no processo de desenvolvimento, economizando tempo e recursos no longo prazo.