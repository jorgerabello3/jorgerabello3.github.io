## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente sobre RestFull API's ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## RestFull API's

### **Qual a diferença entre rest e restfull ?**

REST e RESTful são conceitos relacionados a serviços web, mas há diferenças entre eles.

REST (Representational State Transfer) é uma arquitetura de software que define um conjunto de regras para a criação de serviços web. O objetivo é tornar os serviços escaláveis, flexíveis e fáceis de manter. Os serviços RESTful são baseados em recursos e são acessados usando os verbos HTTP (GET, POST, PUT, DELETE, etc.).

Por outro lado, RESTful é uma implementação de serviços web que segue as regras da arquitetura REST. Portanto, é uma abordagem específica para criar serviços web baseados em recursos e verbos HTTP.

Em resumo, REST é uma arquitetura de software e RESTful é uma implementação dessa arquitetura.

### **Quais verbos http você conhece ?**

Existem vários verbos HTTP, mas os principais são:

* **GET:** é usado para solicitar um recurso em um servidor. É um verbo seguro e idempotente, ou seja, não deve causar mudanças no servidor.

* **POST:** é usado para enviar dados a um servidor para criar ou atualizar um recurso. É um verbo não seguro e não idempotente, ou seja, pode causar mudanças no servidor a cada vez que é executado.

* **PUT:** é usado para enviar dados a um servidor para criar ou atualizar um recurso, mas diferentemente do POST, o PUT é um verbo seguro e idempotente.

* **DELETE:** é usado para solicitar que um recurso seja removido do servidor. É um verbo idempotente, mas não é seguro, pois pode causar mudanças no servidor.

* **PATCH:** é usado para enviar dados a um servidor para atualizar um recurso parcialmente. É um verbo seguro e idempotente. Além desses, existem outros verbos menos comuns, como `OPTIONS`, `HEAD`, `CONNECT` e `TRACE`, que possuem funções específicas em casos de uso mais avançados.

### **Qual status code você retornaria para um endepoint onde você faz um POST para cadastrar uma nova informação ?**

Para indicar que a criação da nova informação foi bem sucedida, é comum retornar o status code 201 (Created). Além disso, é recomendado incluir no header da resposta a URL do novo recurso criado, utilizando a chave Location.

### **Qual status code você retornaria para um endepoint onde você faz um GET para obter apenas registro ?**

Para um endpoint que realiza uma busca de um único registro através de uma requisição GET, o status code mais apropriado é o 200 OK. Isso significa que a solicitação foi bem sucedida e os dados solicitados foram encontrados e retornados no corpo da resposta. Se a solicitação não foi encontrada, deve ser utilizado o status code 404 Not Found para indicar que o recurso solicitado não existe.

### **Qual status code você retornaria para um endepoint onde você faz um GET para obter multiplos registro ?**

Para um endpoint onde é feita uma solicitação GET para obter múltiplos registros, o status code mais adequado é o 200 OK. Isso indica que a solicitação foi bem-sucedida e a resposta contém uma lista de recursos solicitados. É possível que haja outros códigos de status dependendo do contexto, por exemplo, 206 Partial Content para uma solicitação de conteúdo parcial ou 400 Bad Request para uma solicitação inválida.

### **Qual status code você retornaria para um endepoint onde você faz um PUT para atualizar um recurso ?**

O status code adequado para uma requisição PUT bem-sucedida que atualiza um recurso seria o 200 (OK) ou 204 (No Content). O 200 é usado para retornar a representação atualizada do recurso como resultado da operação, enquanto o 204 é usado quando não há nada para retornar na resposta. Além disso, é importante lembrar que o corpo da resposta deve ser vazio quando o status code for 204.

### **Qual status code você retornaria para um endepoint onde você faz um PATCH para atualizar um recurso ?**

O status code mais apropriado para retornar em um endpoint que recebe uma requisição PATCH para atualizar um recurso é o 200 (OK), indicando que a atualização foi bem-sucedida. No entanto, se a atualização parcial não for possível, o status code 422 (Unprocessable Entity) pode ser retornado para indicar que a requisição foi entendida, mas não pôde ser processada devido a erros no conteúdo da requisição.

### **Qual status code você retornaria para um endepoint onde você faz um DELETE para excluir um recurso ? E se for um delete virtual ?**

Para um endpoint que realiza exclusão de recurso, o status code mais adequado é o 204 No Content, que indica que a solicitação foi bem-sucedida e que não há conteúdo para retornar.

Já em um delete virtual, em que o recurso não é realmente excluído, mas marcado como inativo ou removido logicamente, o status code mais apropriado é o 200 OK, juntamente com uma mensagem de confirmação indicando que o recurso foi marcado como inativo ou removido com sucesso.

### **Qual a diferença entre PUT e PATCH ?**

**TL;DR:** O PUT é utilizado para atualizar um recurso inteiro, enquanto o PATCH é utilizado para atualizar apenas algumas partes do recurso.

O método PUT e PATCH são ambos utilizados para atualizar recursos em uma API RESTful, porém eles possuem algumas diferenças importantes:

* **PUT:** é utilizado para atualizar um recurso inteiro, ou seja, o corpo da requisição contém todas as informações do recurso, inclusive os campos não modificados. Quando um recurso é atualizado com PUT, ele é substituído integralmente pelo novo conteúdo. Se o recurso não existir, ele será criado.

* **PATCH:** é utilizado para atualizar parcialmente um recurso, ou seja, apenas os campos que foram modificados precisam ser enviados no corpo da requisição. Com o uso do método PATCH, é possível atualizar um único campo ou diversos campos de forma independente.


### **Quando você faz um get em uma api e o recurso não é encontrado qual status code você retornaria ?**

Se um recurso não for encontrado em uma API em uma solicitação GET, o status code que deve ser retornado é 404 (Not Found). Isso indica que o servidor não pôde encontrar o recurso solicitado, mas não indica se o recurso existiu no passado ou não. É importante fornecer uma mensagem de erro útil para ajudar o usuário a entender a razão pela qual o recurso não foi encontrado.

### **Quais os níveis de maturidade de uma api ?**

Existem diferentes modelos para avaliar o nível de maturidade de uma API, mas um dos mais conhecidos é o modelo [Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html), proposto por Leonard Richardson em sua dissertação de 2008. Esse modelo define três níveis de maturidade:

**Nível 0 - API sem recursos:** o acesso aos recursos é feito por meio de um único endpoint, geralmente utilizando o método POST, com diferentes valores para identificar o recurso desejado. Isso torna a API difícil de ser explorada e mantida.

**Nível 1 - API com recursos:** a API é organizada em recursos, cada um representando um objeto de negócio. O acesso aos recursos é feito por meio de diferentes endpoints, utilizando os métodos HTTP apropriados (GET, POST, PUT, DELETE).

**Nível 2 - API com verbos HTTP corretos:** além de utilizar recursos, a API utiliza os verbos HTTP corretos para acessá-los. O método GET é utilizado para recuperar informações, o método POST é utilizado para criar novos recursos, o método PUT é utilizado para atualizar recursos existentes e o método DELETE é utilizado para excluir recursos.

**Nível 3 - API com HATEOAS:** além de utilizar os recursos e os verbos HTTP corretos, a API utiliza o conceito de HATEOAS (Hypermedia as the Engine of Application State), permitindo que o cliente da API navegue pelos recursos e descubra novos endpoints dinamicamente, reduzindo a necessidade de documentação e tornando a API mais flexível e fácil de ser utilizada.

Cada nível de maturidade representa um avanço na qualidade da API, tornando-a mais fácil de ser utilizada, mantida e explorada. No entanto, atingir um nível mais alto de maturidade também requer mais esforço e planejamento por parte dos desenvolvedores da API

### **O que é HATEOAS ?**

HATEOAS significa "Hypermedia as the Engine of Application State" e é um conceito chave em arquiteturas RESTful. Basicamente, HATEOAS é a ideia de que um recurso na API deve incluir links para outros recursos relacionados, permitindo que o cliente navegue na API de forma mais intuitiva e sem depender de conhecimento prévio sobre a estrutura da API.

Por exemplo, imagine uma API de e-commerce que retorna os detalhes de um produto. Com HATEOAS, o resultado também incluiria links para as avaliações do produto, as categorias em que o produto está listado, produtos relacionados e assim por diante. Esses links permitem que o cliente da API navegue por toda a API sem precisar de informações adicionais sobre a estrutura e o fluxo de navegação.

### **O que é idempotência ?**

Idempotência é um conceito em que uma ação pode ser executada várias vezes sem alterar o resultado final. Ou seja, não importa quantas vezes você execute a mesma ação, o resultado será sempre o mesmo. Em outras palavras, se uma operação idempotente é executada várias vezes, o resultado final será o mesmo do que se ela fosse executada apenas uma vez. Na área de desenvolvimento de software, a idempotência é um conceito importante para garantir a consistência e integridade dos dados. Isso é especialmente importante em operações que podem ser executadas várias vezes, como em chamadas de API REST, por exemplo.


### **Em caso de erro qual status deve-se retornar ?**

Em caso de erro em uma requisição, o status code a ser retornado depende do tipo de erro e do contexto da aplicação. No entanto, alguns status codes comuns para indicar erros são:

**400 Bad Request:** quando a requisição enviada pelo cliente está incorreta ou incompleta.

**401 Unauthorized:** quando o cliente não está autorizado a acessar o recurso solicitado.

**403 Forbidden:** quando o servidor entende a requisição do cliente, mas se recusa a executá-la por motivos de permissões.

**404 Not Found:** quando o recurso solicitado não foi encontrado.

**405 Method Not Allowed:** quando o método HTTP utilizado na requisição não é permitido para o recurso solicitado.

**422 Unprocessable Entity:** quando existem falhas de validação no body enviado na requisição.

**500 Internal Server Error:** quando ocorre um erro interno do servidor durante o processamento da requisição. É importante retornar um status code adequado e uma mensagem de erro informativa para ajudar o cliente a entender o problema e a tomar as medidas necessárias para corrigi-lo.

### **Quais as diferenças entre graphql e rest api ?**

GraphQL e REST são duas abordagens diferentes para construir APIs (Application Programming Interfaces) para acesso a recursos em uma aplicação. Aqui estão algumas diferenças principais entre os dois:

**Estrutura de dados:** Em REST, o servidor define os recursos que estão disponíveis para o cliente e a estrutura de dados que o cliente recebe quando faz uma solicitação. Em GraphQL, o cliente define a estrutura de dados que deseja receber em sua solicitação.

**Número de chamadas:** Em REST, é necessário fazer várias chamadas para diferentes endpoints para obter os dados necessários. Em GraphQL, é possível obter todos os dados necessários em uma única chamada.

**Tamanho da resposta:** Em REST, o servidor geralmente envia uma grande quantidade de dados em resposta à solicitação, mesmo que o cliente precise de apenas uma parte desses dados. Em GraphQL, o servidor envia apenas os dados que o cliente solicitou.

**Evolução da API:** Em REST, qualquer mudança na estrutura de dados da API pode afetar o cliente. Em GraphQL, o cliente pode solicitar apenas os campos que precisa e não precisa se preocupar com a estrutura subjacente dos dados.

**Ferramentas de desenvolvimento:** REST tem um grande ecossistema de ferramentas e frameworks disponíveis. GraphQL, sendo relativamente novo, tem menos ferramentas disponíveis, mas sua popularidade está crescendo rapidamente.

**Cache:** Em REST, a cache é fácil de implementar, já que cada recurso tem sua própria URL. Em GraphQL, é mais difícil implementar a cache, já que uma única solicitação pode retornar vários recursos diferentes.

Essas são apenas algumas diferenças entre REST e GraphQL. A escolha entre uma abordagem ou outra dependerá das necessidades específicas do projeto e dos requisitos de performance e escalabilidade.

### **Quais as vantages e desvantagens de se utilizar rest apis ?**

*As vantagens de se utilizar REST APIs incluem:*

1. **Simplicidade:** O REST é baseado no protocolo HTTP e é simples e fácil de entender e implementar.

2. **Flexibilidade:** O REST permite que você use diferentes formatos de dados, como JSON, XML e texto simples.

3. **Escalabilidade:** O REST é altamente escalável, permitindo que você adicione mais servidores ou recursos conforme necessário.

4. **Desacoplamento:** O REST permite que os clientes e servidores sejam independentes, permitindo que você atualize e modifique os componentes sem afetar os outros.

5. **Cache:** O REST suporta cache, o que pode melhorar significativamente o desempenho da aplicação.

*As desvantagens de se utilizar REST APIs incluem:*

1. **Segurança:** O REST não tem mecanismos de segurança integrados, portanto, você precisa implementar a segurança manualmente.

2. **Tamanho dos dados:** Os dados podem ser maiores com REST, especialmente se você estiver enviando muitos recursos em uma única solicitação.

3. **Exposição:** O REST pode expor muitos detalhes da implementação, o que pode tornar mais fácil para um atacante explorar vulnerabilidades.

4. **Suporte a transações:** O REST não tem suporte integrado para transações distribuídas, o que pode ser um problema em certos casos de uso.

5. **Falta de padrões:** O REST é baseado em convenções e não há padrões definidos para cada aspecto da API, o que pode levar a diferentes implementações.

### **Quais as vantages e desvantagens de se utilizar Graphql ?**

*Algumas vantagens do uso de GraphQL são:*

1. **Flexibilidade na definição de consultas:** o cliente pode definir quais dados deseja receber e a estrutura da resposta de acordo com suas necessidades, o que pode reduzir o tamanho da resposta e melhorar o desempenho.

2. **Redução de chamadas desnecessárias:** com GraphQL, é possível solicitar várias informações em uma única chamada, em vez de várias chamadas a diferentes endpoints.

3. **Menos problemas de versão:** como as consultas são definidas pelo cliente, as mudanças na estrutura de dados do servidor não afetam os clientes existentes.

4. **Maior facilidade no desenvolvimento de aplicações móveis:** a redução do tamanho das respostas pode melhorar o desempenho e a experiência do usuário em conexões de internet mais lentas.

*Algumas desvantagens do uso de GraphQL são:*

1. **Curva de aprendizado:** GraphQL possui uma curva de aprendizado maior do que REST, especialmente se a equipe de desenvolvimento não estiver familiarizada com a tecnologia.

2. **Maior complexidade de implementação:** a implementação de GraphQL pode ser mais complexa do que REST, especialmente para sistemas mais simples e com menos recursos.

3. **Requer ferramentas específicas:** o uso de GraphQL pode exigir o uso de ferramentas específicas para criação de consultas, validação e execução, o que pode aumentar a complexidade do projeto.

4. **Maior consumo de recursos do servidor:** como as consultas GraphQL são mais flexíveis, elas podem exigir mais recursos do servidor para processar, o que pode afetar o desempenho em cargas de trabalho mais pesadas.