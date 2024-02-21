## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente sobre Modelagem em bancos de dados relacional, JPA e persistência em geral ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Bancos de Dados e JPA

### **O que são ORM's ?**

ORM (Object-Relational Mapping) é uma técnica de programação que permite mapear objetos em uma linguagem de programação orientada a objetos (como Java) para tabelas em um banco de dados relacional. Ele elimina a necessidade de escrever código SQL manualmente, fornecendo uma camada de abstração entre o banco de dados e o aplicativo. ORM geralmente envolve a criação de classes Java que representam tabelas no banco de dados e a utilização de um framework para realizar operações de banco de dados, como inserção, atualização, exclusão e consulta. Isso permite que os desenvolvedores trabalhem com objetos Java em vez de ter que lidar diretamente com o banco de dados e o SQL.

### **O que é o hibernate ?**

O Hibernate é um framework de mapeamento objeto-relacional (ORM) em Java, que permite aos desenvolvedores mapear objetos Java para tabelas de banco de dados relacionais, bem como executar operações de consulta e persistência de dados de forma transparente, sem a necessidade de escrever código SQL manualmente. Ele oferece uma camada de abstração que permite trabalhar com objetos Java em vez de lidar diretamente com o banco de dados, o que pode tornar o processo de desenvolvimento mais produtivo e menos propenso a erros. O Hibernate é uma das soluções mais populares de ORM para Java e é amplamente utilizado em projetos empresariais.

### **Qual a diferença entre JPA e Hibernate ?**

JPA (Java Persistence API) é uma especificação Java para mapeamento objeto-relacional (ORM), que define uma interface comum para frameworks ORM. Por outro lado, Hibernate é uma implementação popular de ORM que segue a especificação JPA.

Em outras palavras, JPA é uma especificação que define um conjunto de interfaces e anotações que os desenvolvedores podem usar para definir a estrutura do banco de dados e como as entidades Java se relacionam com essa estrutura. Hibernate, por outro lado, é uma implementação dessa especificação que fornece recursos adicionais que podem não estar incluídos na especificação JPA. Embora seja possível usar outras implementações de JPA, como EclipseLink, o Hibernate é amplamente utilizado na comunidade Java.

### Quais as vantagens e as desvantagens em se utilizar um ORM ?

ORM (Object-Relational Mapping) é uma técnica de mapeamento objeto-relacional que permite representar objetos de uma aplicação em um banco de dados relacional.

*As vantagens em se utilizar um ORM incluem:*

1. **Maior produtividade:** o desenvolvimento de aplicações com ORM pode ser mais rápido e fácil, uma vez que o ORM cuida de muitos detalhes da persistência de dados automaticamente.

2. **Redução de código:** o ORM pode ajudar a reduzir a quantidade de código necessário para a manipulação de dados, tornando o código mais simples e legível.

3. **Portabilidade:** ao utilizar um ORM, a aplicação fica menos dependente de um banco de dados específico, pois o ORM pode ser configurado para trabalhar com diferentes bancos de dados.

4. **Segurança:** o uso do ORM pode ajudar a prevenir ataques de SQL injection, uma vez que o ORM cuida da sanitização dos dados automaticamente.

*As desvantagens em se utilizar um ORM incluem:*

1. **Complexidade:** o ORM pode ser complexo e difícil de configurar e utilizar corretamente.

2. **Performance:** em algumas situações, o uso de ORM pode resultar em uma performance inferior a uma solução de persistência de dados mais manual e customizada.

3. **Limitações:** o ORM pode apresentar limitações em relação à funcionalidade que é possível implementar em SQL, o que pode ser um problema em casos mais avançados de persistência de dados.



### **Qual a finalidade da annotation @Transactional ?**

A annotation `@Transactional` é usada no Spring Framework para indicar que um método deve ser executado dentro de uma transação. A transação é uma sequência de operações de banco de dados que devem ser tratadas como uma única unidade de trabalho, de forma que se ocorrer um erro em qualquer uma das operações, todas as operações anteriores deverão ser revertidas (rollback) para garantir a consistência dos dados.

Ao adicionar a annotation `@Transactional` em um método de serviço, por exemplo, o Spring Framework cuida da criação e gerenciamento da transação automaticamente. É importante destacar que essa annotation deve ser adicionada em métodos que fazem operações de leitura ou escrita no banco de dados.

Dessa forma, a annotation `@Transactional` ajuda a manter a integridade dos dados e a garantir que as operações sejam executadas de maneira consistente.

### **Qual a finalidade da annotation @PersistenceContext ?**

A annotation `@PersistenceContext` é usada no contexto do JPA (Java Persistence API) e indica que o provedor de persistência deve injetar um EntityManager no bean onde a annotation está presente. O EntityManager gerencia o ciclo de vida das entidades do JPA e é usado para realizar operações de persistência (inserção, atualização e remoção) em um banco de dados. O uso dessa annotation é uma forma conveniente de injetar um EntityManager em um bean sem precisar criar o objeto manualmente, o que pode levar a uma maior produtividade e legibilidade do código.

### **Pra que servem as anotações @OneToMany, @ManyToOne, @OneToOne, @ManyToMany, @Entity, @Table, @ColumnDefinition e @Query ?**

As anotações mencionadas referem-se ao framework JPA (Java Persistence API), que é uma especificação do Java para mapeamento objeto-relacional (ORM) de bancos de dados relacionais.

**@Entity:** marca uma classe como uma entidade JPA, ou seja, uma classe que representa uma tabela do banco de dados.

**@Table:** especifica o nome da tabela correspondente à entidade no banco de dados.

**@Column:** mapeia um atributo de uma entidade para uma coluna do banco de dados.

**@ColumnDefinition:** define a definição da coluna do banco de dados, incluindo o tipo de dados, tamanho máximo e outras propriedades específicas do banco de dados.

**@OneToMany:** define uma associação de um para muitos entre duas entidades, onde uma instância da entidade pai pode estar associada a várias instâncias da entidade filho.

**@ManyToOne:** define uma associação de muitos para um entre duas entidades, onde várias instâncias da entidade filho podem estar associadas a uma instância da entidade pai.

**@ManyToMany:** define uma associação de muitos para muitos entre duas entidades, onde várias instâncias da entidade filho podem estar associadas a várias instância da entidade pai.

**@OneToOne:** define uma associação um para um entre duas entidades, onde uma instância da entidade pai está associada a uma única instância da entidade filho e vice-versa.

**@Query:** permite definir uma consulta personalizada em JPQL (Java Persistence Query Language) ou SQL nativo.

Essas anotações ajudam a definir o mapeamento entre as classes Java e as tabelas do banco de dados, permitindo que as operações de persistência sejam realizadas de maneira mais fácil e eficiente.

### **O que é JPQL ?**

JPQL (Java Persistence Query Language) é uma linguagem de consulta semelhante ao SQL (Structured Query Language) usada para consultar objetos em bancos de dados relacionais usando JPA (Java Persistence API).

A diferença principal entre JPQL e SQL é que JPQL trabalha com entidades gerenciadas pelo JPA, enquanto SQL trabalha diretamente com tabelas de bancos de dados. JPQL é uma linguagem orientada a objetos e usa as entidades e seus relacionamentos para definir consultas. É possível usar o JPQL em um aplicativo Java para executar consultas em bancos de dados relacionais.


### **Quais as vantagens e desvantagens em se utilizar jpql ?**

A JPQL (Java Persistence Query Language) é uma linguagem de consulta orientada a objetos, que permite a criação de consultas orientadas a objetos para dados armazenados em um banco de dados relacional usando o padrão JPA (Java Persistence API).

*Algumas vantagens de se utilizar a JPQL incluem:*

1. **Orientação a objetos:** permite que as consultas sejam escritas com base em classes Java e objetos em vez de tabelas e colunas do banco de dados;

2. **Portabilidade:** a JPQL é compatível com qualquer banco de dados relacional que suporte a JPA;

3. **Consultas Complexas:** Possibilidade de realizar consultas complexas e obter resultados personalizados.

*Algumas desvantagens de se utilizar a JPQL incluem:*

1. **Custo de aprendizado:** é necessário aprender a sintaxe da JPQL para escrever consultas eficientes;

2. **Limitações na execução de consultas:** consultas muito complexas ou com muitos resultados podem afetar negativamente o desempenho do aplicativo;

3. **Pode ser difícil de depurar:** como as consultas são criadas por meio de objetos Java, pode ser difícil depurar problemas nas consultas.

Em geral, a JPQL é uma boa escolha para consultas complexas e personalizadas, mas pode não ser a melhor opção para consultas simples ou com grandes volumes de dados.

### **O que é native query ?**

Uma native query é uma consulta SQL nativa escrita em uma linguagem de banco de dados específica, como SQL Server, Oracle ou MySQL. Essa consulta é executada diretamente no banco de dados, sem passar pelo JPA ou outros frameworks de mapeamento objeto-relacional.

As native queries são escritas usando o SQL padrão, permitindo que o desenvolvedor utilize recursos específicos de cada banco de dados. Essa abordagem pode ser útil em casos em que é necessário escrever uma consulta complexa que não pode ser expressa facilmente em JPQL ou quando se deseja aproveitar recursos específicos do banco de dados.

No entanto, o uso excessivo de native queries pode levar a problemas de portabilidade, já que o código SQL pode ser diferente em diferentes bancos de dados. Além disso, as native queries não são verificadas pelo compilador e podem introduzir vulnerabilidades de segurança ou erros de sintaxe.

### **Quais as vantagens e desvantagens em se utilizar native query ?**

Uma native query é uma consulta SQL padrão que é executada diretamente no banco de dados sem ser traduzida em um conjunto de instruções pelo ORM (Object-Relational Mapping). As principais vantagens e desvantagens em se utilizar native queries em vez de consultas JPQL são:

*Vantagens:*

1. **Desempenho:** como as consultas nativas são executadas diretamente no banco de dados, podem ser mais rápidas e eficientes do que as consultas JPQL, que precisam ser traduzidas pelo ORM em uma ou mais instruções SQL.

2. **Recursos de banco de dados:** consultas nativas permitem a utilização de recursos avançados de banco de dados, como funções, procedures e views, que podem não estar disponíveis ou serem difíceis de usar através de JPQL.

3. **Flexibilidade:** com as consultas nativas é possível realizar operações complexas e personalizadas que não são possíveis de realizar com JPQL.

*Desvantagens:*

1. **Portabilidade:** as consultas nativas são específicas para um banco de dados, o que pode tornar difícil portar o código para outro banco de dados sem modificar a consulta.

2. **Manutenção:** com as consultas nativas, as atualizações de esquema (schema) do banco de dados podem exigir atualizações adicionais na consulta, o que pode aumentar a complexidade e tornar a manutenção mais difícil.

3. **Perda de recursos ORM:** o uso de consultas nativas pode levar à perda de recursos importantes fornecidos pelo ORM, como o gerenciamento de cache, o mapeamento de entidades e a validação de dados.

Em resumo, as consultas nativas podem ser uma opção viável quando é necessário desempenho máximo ou recursos específicos do banco de dados, mas devem ser usadas com cuidado para garantir a portabilidade e a manutenção adequadas do código.

### **Como funcionam as queries do spring data jpa ?**

O Spring Data JPA é uma biblioteca que fornece um modelo de programação baseado em interfaces para a integração entre aplicativos Java e bancos de dados relacionais, simplificando o acesso e a manipulação de dados por meio da camada de persistência.

As queries do Spring Data JPA são baseadas em métodos declarativos, onde a partir da assinatura do método e de sua nomenclatura, o Spring Data JPA automaticamente cria a query necessária para executar a operação solicitada. Essas operações podem incluir operações CRUD (criação, leitura, atualização e exclusão) e outras consultas mais específicas.

Por exemplo, considere a seguinte interface de repositório usando Spring Data JPA:

```java
@Repository
public interface CustomerRepository extends JpaRepository<Customer, Long> {

    List<Customer> findByLastName(String lastName);

    @Query("SELECT c FROM Customer c WHERE c.age > :age")
    List<Customer> findByAgeGreaterThan(@Param("age") int age);

}
```

Nesse exemplo, a interface `CustomerRepository` estende a interface `JpaRepository` do Spring Data JPA, que fornece as operações padrão CRUD. Além disso, dois métodos personalizados são definidos:

**findByLastName:** esse método busca todos os clientes com um determinado sobrenome e retorna uma lista de objetos Customer. A implementação do Spring Data JPA cria a consulta SQL apropriada a partir do nome do método.

**findByAgeGreaterThan:** esse método usa a anotação `@Query` para definir uma consulta personalizada em JPQL. Nesse caso, a consulta busca todos os clientes com idade maior que um valor especificado pelo parâmetro age.

Ao utilizar esses métodos, o Spring Data JPA é responsável por criar e executar as consultas SQL necessárias para atender às solicitações do aplicativo. Isso pode simplificar significativamente o código necessário para acessar e manipular dados em um banco de dados relacional.