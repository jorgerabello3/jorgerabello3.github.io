## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente sobre spring e spring boot  ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Spring e Spring Boot

### **Como você definiria o Spring ?**
O Spring é um framework open-source para o desenvolvimento de aplicações empresariais em Java. Ele oferece uma infraestrutura completa para o desenvolvimento de aplicações web, incluindo recursos para a criação de APIs RESTful, injeção de dependências, segurança, persistência de dados, entre outros. O Spring também fornece uma abordagem modular, permitindo que os desenvolvedores utilizem apenas os componentes necessários para suas aplicações. Com sua ampla adoção e comunidade ativa, o Spring se tornou uma das ferramentas mais populares para o desenvolvimento de aplicações Java de alta qualidade e escaláveis.

### **Qual a diferença entre Spring Boot e Spring MVC ?**

Spring Boot e Spring MVC são duas tecnologias distintas do ecossistema Spring.

O Spring MVC é um framework MVC (Model-View-Controller) completo para construir aplicações web baseadas em Java. Ele oferece várias funcionalidades para trabalhar com requisições HTTP, views, modelos de dados, validação, segurança e muito mais. O Spring MVC é um dos frameworks mais populares para desenvolvimento de aplicações web em Java.

Já o Spring Boot é um framework que facilita a criação de aplicações Java autônomas, com a vantagem de minimizar a configuração manual. Ele é baseado no Spring Framework e utiliza a configuração por convenção, com o objetivo de acelerar o processo de desenvolvimento de aplicações. O Spring Boot inclui uma série de recursos prontos para uso, como servidores embutidos, configuração automática de componentes, autoconfiguração, monitoramento, entre outros.

Em resumo, o Spring MVC é um framework para desenvolvimento de aplicações web, enquanto o Spring Boot é um conjunto de ferramentas e convenções que tornam a criação de aplicações Java mais simples e rápida. Embora possam ser utilizados juntos, eles são tecnologias distintas.

### **Como você definiria o spring data ?**

O Spring Data é um subprojeto do Spring Framework que fornece abstrações de acesso a dados consistentes para diferentes tipos de banco de dados, incluindo bancos de dados relacionais, bancos de dados NoSQL, armazenamento de chave-valor e mais. Ele fornece uma maneira fácil de acessar, consultar e manipular dados em diferentes tipos de banco de dados, tornando o processo de acesso a dados mais simples e intuitivo para os desenvolvedores. O Spring Data também inclui funcionalidades como paginação, ordenação e cache de dados, além de suportar o desenvolvimento de aplicativos reativos e a integração com outros projetos do Spring.

### **Qual a finalidade do uso da annotation @Service ?**

**TL;DR:** a anotação @Service ajuda a tornar o código mais claro e legível, além de fornecer uma maneira fácil de gerenciar beans no Spring Framework.

A anotação `@Service` é usada em classes para indicar que ela possui uma função de serviço, ou seja, ela contém a lógica de negócios da aplicação. Essa anotação é uma especialização da anotação `@Component` e é usada para tornar mais clara a intenção do desenvolvedor.

Quando uma classe é anotada com `@Service`, o Spring a registra como um bean gerenciado pelo container e a torna disponível para ser injetada em outras classes que a utilizam.

Por exemplo, imagine que você esteja desenvolvendo uma aplicação de e-commerce e tenha uma classe chamada PedidoService que gerencia a lógica de negócios relacionada aos pedidos. Ao anotar a classe com `@Service`, você indica que essa classe é uma implementação de serviço e pode ser injetada em outras classes que precisam acessar ou manipular pedidos, como a classe de controle de pedidos ou a classe que manipula o carrinho de compras.

### **Qual a difenrença entre @Service e @Component ?**

Ambas as anotações, `@Service` e `@Component`, são usadas para indicar que uma classe é um componente gerenciado pelo Spring. A diferença é que `@Service` é uma especialização de `@Component` e é usada para identificar serviços na camada de negócios da aplicação.

Enquanto `@Component` pode ser usada para qualquer componente gerenciado pelo Spring, incluindo classes utilitárias, `@Service` é uma anotação mais semântica e deve ser usada para classes que implementam a lógica de negócios da aplicação.

Dessa forma, usar a anotação `@Service` ajuda a dar mais semântica ao código e torná-lo mais legível e fácil de entender, pois indica que a classe é responsável por implementar serviços da aplicação.

### **Qual a finalidade da annotation @Repository ?**

A annotation `@Repository` é uma anotação do Spring Framework que indica que a classe é um repositório, ou seja, é responsável pela persistência e recuperação de dados. Essa anotação é normalmente usada em conjunto com outras anotações de persistência, como `@Autowired`, `@Transactional`, `@PersistenceContext`, entre outras.

Ao marcar uma classe com a anotação `@Repository`, o Spring Framework automaticamente a trata como um bean gerenciado, permitindo que a classe seja injetada em outras classes que dependem dela usando a anotação `@Autowired`, por exemplo. Além disso, a anotação `@Repository` ajuda a simplificar o código de acesso a banco de dados, ao fornecer uma camada de abstração para as operações de persistência e consulta.

Em resumo, a annotation `@Repository` é usada para marcar uma classe como um repositório de dados, indicando ao Spring que a classe deve ser gerenciada como um bean e permitindo que outras classes dependam dela usando injeção de dependência.

### **Qual a finalidade da annotation @Autowired ?**

A annotation `@Autowired` é utilizada no Spring Framework para realizar a injeção de dependências automaticamente. Isso significa que o Spring procura por um bean correspondente ao tipo da variável a ser injetada e a instância automaticamente, evitando que o desenvolvedor tenha que fazer isso manualmente.

Ao usar a annotation `@Autowired`, o desenvolvedor indica ao Spring que determinado bean precisa de uma instância de outra classe para ser executado. Isso torna o código mais simples, legível e fácil de manter, além de permitir a troca de implementações de forma mais fácil, já que as dependências são desacopladas do código.

### **Qual a diferença entre as annotations @Controller e @RestController ?**

**TL;DR:** `@Controller` é utilizado para retornar views (normalmente HTML), enquanto `@RestController` é utilizado para retornar dados serializados em formatos como JSON e XML.

A diferença entre as annotations `@Controller` e `@RestController` está na maneira como os dados são retornados pelo controlador.

A annotation @Controller é utilizada em classes que implementam o padrão de projeto MVC (Model-View-Controller) no Spring Framework. Ela é responsável por receber requisições HTTP e processá-las, retornando uma view (normalmente uma página HTML) como resposta para o cliente.

Já a annotation `@RestController` é utilizada para criar controladores que retornam objetos diretamente no formato JSON, XML, entre outros. Ela combina as funcionalidades da annotation `@Controller` com a annotation `@ResponseBody`, que indica que o retorno do método deve ser serializado e enviado como resposta ao cliente.


### **Por que não devemos retornar as entidades nos rest controllers ?**

Retornar entidades nos REST Controllers pode expor informações sensíveis do sistema ou da aplicação, bem como pode limitar a escalabilidade da API. Além disso, pode ocorrer a exposição de informações extras que não são necessárias para o cliente e podem tornar a transferência de dados mais lenta e consumir mais recursos. Portanto, é uma boa prática utilizar DTOs (Data Transfer Objects) para representar as informações que serão enviadas e recebidas pelos clientes. Dessa forma, as informações podem ser modeladas de acordo com as necessidades do cliente e evita-se expor informações desnecessárias ou sensíveis.

### **Pra que serve a annottion @Bean ?**

A annotation `@Bean` é usada no Spring Framework para indicar que um método de um arquivo de configuração de Spring (por exemplo, uma classe com a anotação `@Configuration`) retorna um objeto que deve ser registrado como um bean gerenciado pelo Spring.

Essa annotation permite que o Spring saiba que o método deve ser invocado para criar o objeto e também pode permitir a configuração adicional do objeto (por exemplo, injetando dependências) antes que ele seja registrado no contexto do Spring.

Os beans gerenciados pelo Spring são objetos que são criados pelo container do Spring e gerenciados por ele. O container do Spring é responsável por gerenciar o ciclo de vida desses objetos e injetar as dependências necessárias.

### **Pra que serve a annottion @Configuration ?**

A annotation `@Configuration` é utilizada para indicar que uma classe contém definições de beans que devem ser processadas pelo contêiner Spring. Em outras palavras, é utilizada para marcar uma classe como uma classe de configuração para o Spring. Essa annotation pode ser usada em conjunto com outras annotations, como `@Bean`, `@ComponentScan` e `@Import`, para definir beans e outras configurações necessárias para a aplicação. Através da annotation @Configuration é possível definir e personalizar configurações, como banco de dados, segurança, cache, entre outros.

### **Quais os escopos de um bean ?**

No Spring, os escopos definem a vida útil dos objetos gerenciados pelo container do Spring. Os principais escopos de bean no Spring são:

* **Singleton:** É o escopo padrão do Spring. Quando um bean é definido como Singleton, o container cria apenas uma instância do objeto e a reutiliza sempre que o bean é injetado em outras classes.

* **Prototype:** Quando um bean é definido como Prototype, o container cria uma nova instância do objeto sempre que o bean é injetado em outras classes.

* **Request:** Quando um bean é definido com escopo Request, o container cria uma instância nova do objeto para cada requisição HTTP. Isso significa que cada thread que recebe a requisição HTTP terá sua própria instância do bean.

* **Session:** Quando um bean é definido com escopo Session, o container cria uma instância nova do objeto para cada sessão HTTP. Ou seja, cada usuário que acessa o sistema terá sua própria instância do bean.

* **Global Session:** O escopo Global Session é similar ao escopo Session, porém é utilizado em aplicações que utilizam portlets em vez de páginas web tradicionais. O container cria uma instância nova do objeto para cada sessão global do usuário.

* **Application:** O escopo Application é utilizado em aplicações que utilizam o ServletContext. O container cria uma instância nova do objeto para toda a aplicação.

* **Websocket:** O escopo Websocket é utilizado em aplicações que utilizam a tecnologia WebSocket. O container cria uma instância nova do objeto para cada conexão WebSocket.

Cada escopo tem suas próprias características e é importante escolher o escopo correto para cada bean, de acordo com o comportamento desejado da aplicação.

### **Quais são as core features do Spring Boot ?**

O Spring Boot possui diversas funcionalidades que simplificam o processo de desenvolvimento de aplicações Java. Algumas das core features do Spring Boot são:

* **Configuração automática:** O Spring Boot tem como objetivo minimizar a configuração manual, fornecendo configurações padrões para as bibliotecas mais utilizadas.

* **Tomcat embutido:** O Spring Boot traz um servidor Tomcat embutido, o que significa que não é necessário configurar um servidor externo para executar a aplicação.

* **Starter dependencies:** O Spring Boot oferece uma série de starter dependencies para tornar a configuração de projetos mais simples e fácil. Cada starter inclui todas as dependências necessárias para uma determinada funcionalidade.

* **Spring Actuator:** O Spring Boot Actuator oferece endpoints para monitorar e gerenciar a aplicação em tempo de execução. Ele permite que você verifique a saúde da aplicação, exiba informações de métricas, verifique os logs e muito mais.

* **Profiles:** O Spring Boot permite que você defina profiles para diferentes ambientes de execução. Por exemplo, você pode ter diferentes configurações para desenvolvimento, teste e produção.

* **Spring Data JPA:** O Spring Boot possui integração com o Spring Data JPA, que facilita o acesso a bancos de dados.

Essas são apenas algumas das core features do Spring Boot, mas existem muitas outras funcionalidades que tornam a construção de aplicativos Java mais fácil e rápida.

Além dessas temos a [documentação](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html) do próprio Spring Boot