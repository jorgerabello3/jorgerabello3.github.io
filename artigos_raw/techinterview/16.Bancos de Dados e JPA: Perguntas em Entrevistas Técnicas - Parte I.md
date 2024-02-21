## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente sobre Modelagem em bancos de dados relacional, JPA e persistência em geral ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Bancos de Dados e JPA

### **O que é cardinalidade ?**

Em bancos de dados, a cardinalidade refere-se à relação entre duas tabelas, ou seja, quantos registros de uma tabela correspondem a quantos registros de outra tabela. Ela define como os registros em uma tabela estão relacionados aos registros em outra tabela, estabelecendo assim a natureza do relacionamento entre as tabelas. A cardinalidade pode ser de três tipos principais: um para um (1:1), um para muitos (1:N) e muitos para muitos.

### **O que é uma entidade fraca ?**

Uma entidade fraca é uma entidade de um modelo de dados que depende de outra entidade para existir. Em outras palavras, ela não pode existir independentemente e precisa de uma entidade forte para ser identificada.

Por exemplo, considere um modelo de dados para um sistema de reservas de quartos de hotel. A entidade "Reserva" pode depender da entidade "Cliente" para existir, pois uma reserva só pode ser feita se houver um cliente associado a ela. Nesse caso, "Cliente" seria a entidade forte e "Reserva" seria a entidade fraca.

Geralmente, as entidades fracas são identificadas por um conjunto de chaves primárias que incluem a chave primária da entidade forte, a fim de garantir a integridade referencial entre as duas entidades.

### **O que é chave primária e chave estrangeira e chave composta ?**

**Chave primária (Primary Key)** é um atributo ou conjunto de atributos de uma tabela de banco de dados que identifica exclusivamente cada registro da tabela. É a chave pela qual os registros são indexados e é utilizada como referência em outras tabelas do banco de dados.

**Chave estrangeira (Foreign Key)** é uma coluna ou conjunto de colunas de uma tabela de banco de dados que se refere à chave primária de outra tabela, estabelecendo uma relação entre as duas tabelas. É utilizada para manter a integridade referencial do banco de dados e garantir que os dados relacionados estejam consistentes.

**Chave composta (Composite Key)** é um conjunto de duas ou mais colunas que juntas formam uma chave primária em uma tabela de banco de dados. É utilizada quando uma única coluna não é suficiente para identificar unicamente cada registro da tabela.

### **Qual a importância em se definir indicies em tabelas de bancos de dados ?**

Os índices são estruturas de dados que permitem acelerar a busca e a recuperação de informações em uma tabela do banco de dados. Quando uma consulta é executada em uma tabela sem índice, o banco de dados precisa fazer uma varredura completa na tabela, o que pode ser um processo lento, especialmente em tabelas com um grande volume de dados. Ao definir índices nas colunas relevantes de uma tabela, o banco de dados pode usar esses índices para localizar rapidamente os registros correspondentes às consultas e reduzir o tempo de resposta da consulta.

A criação de índices é importante para melhorar a performance de consultas em bancos de dados com grandes volumes de dados, já que permite que as consultas sejam executadas de forma mais rápida e eficiente. No entanto, é importante ter em mente que a criação de índices também pode afetar negativamente a performance de atualização e inserção de dados na tabela, já que o banco de dados precisa atualizar os índices sempre que um registro é inserido, atualizado ou excluído. Portanto, é importante avaliar cuidadosamente a necessidade de criar índices em uma tabela e definir os índices de forma estratégica para garantir o melhor desempenho geral do banco de dados.

### **Quais as diferenças entre bancos de dados relacionais e não relacionais ?**

Os bancos de dados relacionais e não relacionais diferem na forma como armazenam e gerenciam dados.

Os bancos de dados relacionais armazenam dados em tabelas com linhas e colunas, usando chaves primárias e estrangeiras para relacionar os dados entre as tabelas. Eles são projetados para lidar com dados altamente estruturados, com esquemas predefinidos, e são usados principalmente em sistemas corporativos para gerenciar grandes volumes de dados transacionais.

Já os bancos de dados não relacionais, também conhecidos como bancos de dados NoSQL, armazenam dados em formatos mais flexíveis, como documentos, grafos ou pares chave-valor. Eles foram desenvolvidos para lidar com dados sem uma estrutura fixa ou com esquemas dinâmicos que podem mudar facilmente ao longo do tempo. Eles são usados principalmente em aplicativos modernos, como aplicativos da web, mobile e IoT, que exigem escalabilidade e flexibilidade para lidar com grandes volumes de dados não estruturados.

A seguir estão algumas diferenças adicionais entre bancos de dados relacionais e não relacionais:

**Linguagem de consulta:** Bancos de dados relacionais usam SQL (Structured Query Language) para fazer consultas, enquanto bancos de dados NoSQL podem usar várias linguagens, incluindo JSON, XML e outras linguagens específicas do fornecedor.

**Escalabilidade:** Os bancos de dados relacionais geralmente não são escaláveis horizontalmente, o que significa que, quando precisTDD (Test Driven Development) é uma metodologia de desenvolvimento de software em que os testes são escritos antes mesmo do código do programa. O objetivo é garantir que o software desenvolvido atenda aos requisitos do cliente e seja mais fácil de manter e evoluir ao longo do tempo.

**Consistência:** Bancos de dados relacionais geralmente oferecem consistência forte, o que significa que os dados sempre estarão em um estado consistente após uma transação ser concluída. Os bancos de dados NoSQL oferecem diferentes níveis de consistência, desde a consistência forte até a eventual consistência, onde pode haver um pequeno atraso na propagação dos dados atualizados.

**Flexibilidade de esquema:** Bancos de dados relacionais são altamente estruturados, o que significa que eles têm um esquema predefinido que deve ser seguido. Os bancos de dados NoSQL são mais flexíveis em termos de esquema, permitindo que os usuários armazenem dados sem uma estrutura predefinida ou que evolua ao longo do tempo.

### **Em quais situações é recomendável utilizar bancos de dados relacionais ?**

**TL;DR:** Bancos de dados relacionais são recomendados quando a estrutura dos dados é bem definida, é necessário garantir a integridade e a consistência dos dados, é necessário suportar consultas complexas e alta concorrência.

Bancos de dados relacionais são recomendados em diversas situações, como:

**Quando a estrutura dos dados é bem definida e não tende a mudar com frequência:** bancos de dados relacionais têm uma estrutura bem definida e exigem que os dados sejam organizados em tabelas, com colunas e tipos de dados específicos. Se a estrutura dos dados é bem definida e não tende a mudar com frequência, um banco de dados relacional pode ser uma boa opção.

**Quando é necessário garantir a integridade dos dados:** bancos de dados relacionais são projetados para garantir a integridade dos dados, ou seja, para evitar que dados inválidos ou inconsistentes sejam armazenados no banco de dados. Isso é feito através de mecanismos como chaves primárias, chaves estrangeiras e restrições de integridade referencial.

**Quando é necessário garantir a consistência dos dados:** bancos de dados relacionais também são projetados para garantir a consistência dos dados, ou seja, para evitar que dados inconsistentes sejam lidos do banco de dados. Isso é feito através de mecanismos como transações e isolamento de transações.

**Quando é necessário suportar consultas complexas:** bancos de dados relacionais são projetados para suportar consultas complexas, envolvendo várias tabelas e relacionamentos entre elas. Isso é feito através da linguagem SQL e de recursos como junções, subconsultas e funções de agregação.

**Quando é necessário suportar alta concorrência:** bancos de dados relacionais são projetados para suportar alta concorrência, ou seja, muitos usuários acessando o banco de dados ao mesmo tempo. Isso é feito através de recursos como controle de transações, bloqueio de registros e gerenciamento de memória e disco.

### **Em quais situações é recomendável utilizar bancos de dados não relacionais ?**

**TL;DR:** Bancos de dados não relacionais são recomendados em situações em que a flexibilidade de esquema, a escalabilidade horizontal e o desempenho são mais importantes do que a consistência de dados e a capacidade de consultas complexas fornecida por bancos de dados relacionais.

Bancos de dados não relacionais são recomendados em diversas situações, incluindo:

**Escalabilidade horizontal:** bancos de dados não relacionais são projetados para serem escaláveis horizontalmente, permitindo que você aumente a capacidade de armazenamento e processamento adicionando mais servidores, o que os torna uma escolha natural para aplicações que precisam lidar com grande volume de dados e/ou alta demanda.

**Flexibilidade de esquema:** ao contrário dos bancos de dados relacionais, bancos de dados não relacionais permitem que você armazene dados com diferentes estruturas e formatos em um mesmo banco de dados, sem a necessidade de um esquema rígido. Isso pode ser muito útil em aplicações em que o modelo de dados pode mudar com frequência.

**Performance:** bancos de dados não relacionais são geralmente mais rápidos do que bancos de dados relacionais em situações em que é necessário fazer muitas leituras ou gravações de dados em grande escala. Isso ocorre porque eles foram projetados para suportar operações em paralelo e em grandes volumes.

**Dados não estruturados:** bancos de dados não relacionais são ideais para armazenar dados não estruturados, como documentos, imagens e vídeos.

**Tecnologias modernas:** bancos de dados não relacionais geralmente utilizam tecnologias modernas, como armazenamento em memória e processamento em tempo real, que os tornam adequados para aplicações que precisam lidar com dados em tempo real, como sistemas de IoT e processamento de dados em streaming.

### **Quais as vantagens e desvantagens de se utilizar bancos de dados relacionais ?**

*As vantagens de se utilizar bancos de dados relacionais incluem:*

1. **Estrutura de dados organizada:** Os bancos de dados relacionais utilizam uma estrutura organizada que garante a consistência dos dados, o que é especialmente importante para aplicações que trabalham com transações financeiras ou informações críticas.

2. **Linguagem SQL:** A linguagem SQL (Structured Query Language) é amplamente utilizada e facilita a manipulação e extração de informações de bancos de dados relacionais.

3. **Suporte a transações:** Bancos de dados relacionais suportam transações, permitindo que as aplicações realizem ações complexas que requerem várias operações de banco de dados em um único processo.

4. **Consistência de dados:** Bancos de dados relacionais são projetados para garantir a consistência dos dados e evitar problemas de redundância e inconsistência, o que pode ser especialmente importante para aplicações que exigem alta disponibilidade e confiabilidade.

*As desvantagens de se utilizar bancos de dados relacionais incluem:*

1. **Dificuldade de escalar horizontalmente:** Bancos de dados relacionais podem ter dificuldades para escalar horizontalmente, o que pode afetar o desempenho de aplicações que exigem alta escalabilidade.

2. **Flexibilidade limitada:** Bancos de dados relacionais são projetados com uma estrutura rígida, o que pode limitar a flexibilidade da aplicação em relação à manipulação de dados.

3. **Alto custo:** Bancos de dados relacionais podem ter um alto custo em termos de hardware, software e licenças.

4. **Dificuldade de lidar com dados não estruturados:** Bancos de dados relacionais são projetados para lidar com dados estruturados, e podem ter dificuldades para lidar com dados não estruturados, como imagens, vídeos e áudio.

É importante ressaltar que as vantagens e desvantagens dos bancos de dados relacionais podem variar dependendo das necessidades específicas da aplicação. Por exemplo, se a aplicação requer alta disponibilidade e confiabilidade, um banco de dados relacional pode ser a escolha ideal, enquanto se a aplicação requer alta escalabilidade e flexibilidade, um banco de dados não relacional pode ser a melhor escolha.

### **Quais as vantagens e desvantagens de se utilizar bancos de dados não relacionais ?**


*Algumas vantagens do uso de bancos de dados não relacionais são:*

1. **Escalabilidade horizontal mais fácil:** bancos de dados não relacionais, como os bancos de dados orientados a documentos, são mais fáceis de escalar horizontalmente, o que significa que você pode adicionar mais servidores ao cluster para lidar com o aumento do tráfego sem afetar o desempenho do sistema.

2. **Flexibilidade:** os bancos de dados não relacionais podem lidar com dados não estruturados e semiestruturados com mais facilidade do que os bancos de dados relacionais. Eles permitem adicionar campos e coleções de documentos com diferentes campos sem a necessidade de reestruturar a base de dados.

3. **Alta disponibilidade:** muitos bancos de dados não relacionais são projetados para suportar redundância de dados e recuperação de desastres, garantindo alta disponibilidade para o sistema.

4. **Escalabilidade vertical:** alguns bancos de dados não relacionais, como os bancos de dados de chave-valor, são altamente escaláveis verticalmente, o que significa que podem lidar com grandes volumes de dados em um único servidor.

*Algumas desvantagens do uso de bancos de dados não relacionais são:*

1. **Menor suporte de ferramentas:** o ecossistema de ferramentas e bibliotecas disponíveis para bancos de dados não relacionais pode ser menor em comparação com bancos de dados relacionais.

2. **Dificuldade na realização de consultas complexas:** muitas vezes, os bancos de dados não relacionais não oferecem recursos avançados de consulta, o que pode tornar difícil a realização de consultas complexas ou análises de dados.

3. **Menos maduros:** alguns bancos de dados não relacionais são relativamente novos no mercado e podem não ter passado pelo mesmo nível de testes e validação que os bancos de dados relacionais mais estabelecidos.

4. **Requerem uma abordagem de modelagem de dados diferente:** o modelo de dados não relacionais é diferente do modelo de dados relacional, o que pode exigir uma abordagem diferente na modelagem de dados e no design do esquema.

### **O que são migrations e qual a importância em se utilizar ?**

Migrations são uma forma de versionar e gerenciar as mudanças em um banco de dados. Geralmente, as migrations são arquivos que contêm instruções SQL para criar, alterar ou excluir tabelas, índices, chaves estrangeiras e outras entidades do banco de dados.

A importância de utilizar migrations é garantir que o banco de dados esteja sempre sincronizado com a versão da aplicação em execução. Dessa forma, é possível fazer alterações no esquema do banco de dados sem perder os dados existentes ou causar erros na aplicação. Além disso, as migrations permitem que várias pessoas trabalhem em conjunto no mesmo banco de dados, com um controle efetivo de versão e histórico de alterações.

As migrations também facilitam o processo de implantação da aplicação em ambientes de desenvolvimento, teste e produção, pois permitem a execução automática das alterações no banco de dados em cada ambiente.

### **Quais as vantagens e desvantagens em se utilizar migrations ?**

As migrations são uma forma de gerenciamento de mudanças de esquema de banco de dados de forma programática. Algumas vantagens de se utilizar migrations são:

1. **Controle de versão:** as migrations permitem que as mudanças de esquema sejam versionadas, o que torna mais fácil rastrear as alterações e reverter as alterações, caso necessário.

2. **Padronização:** as migrations são uma maneira padronizada de definir as alterações de esquema, o que pode ajudar a garantir que as alterações sejam feitas de maneira consistente.

3. **Facilidade de implantação:** as migrations podem ser aplicadas automaticamente quando um aplicativo é implantado, o que pode ajudar a evitar erros de implantação.

No entanto, também existem algumas desvantagens em se utilizar migrations, tais como:

1. **Curva de aprendizado:** pode levar algum tempo para entender como as migrations funcionam e como implementá-las corretamente.

2. **Complexidade:** o gerenciamento de alterações de esquema pode se tornar bastante complexo, especialmente em aplicativos maiores com muitas alterações.

3. **Conflitos:** podem ocorrer conflitos de alterações de esquema quando vários desenvolvedores trabalham na mesma base de código, o que pode ser difícil de resolver.

Apesar disso, a utilização de migrations é amplamente recomendada como uma prática essencial de desenvolvimento de banco de dados.

### **O que é ACID ?**

ACID é um acrônimo que representa as propriedades de um sistema de gerenciamento de banco de dados (SGBD) que garante a integridade dos dados. As letras "ACID" representam:

**Atomicidade (Atomicity):** garante que as transações são tratadas como uma unidade indivisível, ou seja, ou a transação é concluída com sucesso, ou nenhuma alteração é feita no banco de dados.

**Consistência (Consistency):** garante que o banco de dados está sempre em um estado válido, mesmo em caso de falhas ou interrupções inesperadas.

**Isolamento (Isolation):** garante que as transações em execução são isoladas umas das outras e não interferem umas com as outras. Isso garante que as transações sejam executadas em uma ordem que mantenha a consistência do banco de dados.

**Durabilidade (Durability):** garante que as alterações feitas no banco de dados são permanentes e não serão perdidas, mesmo em caso de falhas no sistema ou de hardware.

Juntas, essas propriedades garantem que o banco de dados seja confiável, consistente e durável. Os SGBDs que seguem essas propriedades são adequados para sistemas críticos onde a integridade dos dados é fundamental, como em sistemas bancários, financeiros, de comércio eletrônico e de gerenciamento de informações confidenciais.