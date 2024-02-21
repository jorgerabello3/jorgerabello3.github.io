# Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente sobre DevOps e Cloud ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## DevOps e Cloud

### **O que é integração contínua ? E quais as suas vantagens e desvantagens ?**

Integração contínua (CI, do inglês Continuous Integration) é uma prática de desenvolvimento de software que visa integrar, construir e testar o código-fonte frequentemente e de forma automatizada. Basicamente, o objetivo da integração contínua é detectar e corrigir erros de forma rápida e eficiente, garantindo que o código esteja sempre em um estado utilizável e entregável.

As principais vantagens da integração contínua são:

1. Identificação precoce de bugs e problemas de integração;
2. Redução de problemas com merges e conflitos de código;
3. Redução do tempo necessário para detectar e corrigir problemas;
4. Aumento da confiabilidade do software entregue;
5. Aumento da produtividade e qualidade do time de desenvolvimento.

Porém, a implementação da integração contínua também possui algumas desvantagens, tais como:

1. Necessidade de infraestrutura para realizar a automação;
2. Investimento em ferramentas de CI;
3. Demanda de tempo para escrever e manter os testes automatizados.
4. Apesar dessas desvantagens, muitas empresas têm adotado a prática da integração contínua para garantir a qualidade e a eficiência no desenvolvimento de software.

### **O que é deploy contínuo ? E quais as suas vantagens e desvantagens ?**

O deploy contínuo é uma prática de desenvolvimento de software que automatiza o processo de implantação de código em produção assim que uma alteração é feita e aprovada em um ambiente de testes. Isso significa que, assim que uma nova funcionalidade ou correção de bug é adicionada ao repositório de código, ela passa por testes automatizados e, se for aprovada, é automaticamente implantada em produção.

*As vantagens do deploy contínuo incluem:*

1. **Redução de riscos:** como as mudanças são implantadas em pequenos incrementos, há menos risco de erros graves ou falhas catastróficas.
2. **Velocidade:** com o deploy contínuo, as alterações são implantadas rapidamente, o que permite que a equipe de desenvolvimento responda mais rapidamente às necessidades do negócio.
3. **Feedback rápido:** como as mudanças são testadas automaticamente, os desenvolvedores recebem feedback rápido sobre a qualidade do código, o que ajuda a identificar e corrigir erros mais rapidamente.

*As desvantagens do deploy contínuo incluem:*

1. **Falta de controle:** com o deploy contínuo, o código pode ser implantado automaticamente sem passar por revisão e aprovação adequadas. Isso pode levar a bugs e problemas não detectados que podem afetar a estabilidade do sistema.
2. **Erros no pipeline de implantação:** a implantação contínua depende muito de um pipeline de implantação automatizado que funciona sem problemas. Qualquer erro no pipeline de implantação pode resultar em problemas na produção.
3. **Maior complexidade:** a implementação de um processo de deploy contínuo pode ser complexa, com muitas ferramentas e tecnologias envolvidas. Isso pode aumentar o tempo e o custo de implementação.
4. **Maior necessidade de testes automatizados:** para garantir que o código implantado não tenha problemas, é necessário ter testes automatizados rigorosos que cubram todos os casos de uso. Isso pode aumentar a carga de trabalho dos desenvolvedores e testadores.
5. **Integração com sistemas existentes:** integrar o deploy contínuo com sistemas existentes pode ser difícil, especialmente se houver uma mistura de tecnologias e processos em uso. Isso pode exigir mudanças significativas na arquitetura e nos processos existentes.

### **Como você definiria DevOps ?**

DevOps é uma cultura de colaboração, comunicação e integração entre as equipes de desenvolvimento de software e operações de TI. É um conjunto de práticas que visam a automação de processos, a entrega rápida e frequente de software de alta qualidade e a melhoria contínua dos processos de desenvolvimento e operações. O objetivo é criar um ambiente ágil e colaborativo que permita às empresas desenvolver, testar e implantar software de maneira rápida, confiável e segura, para atender às necessidades do negócio e dos usuários finais.

### **O que é IaC (Infra as Code) ?**

IaC (Infrastructure as Code) é uma abordagem para gerenciar a infraestrutura de TI de uma empresa usando código. Isso significa que, em vez de configurar manualmente cada componente de uma infraestrutura, como servidores, redes e bancos de dados, você escreve código para definir como eles devem ser configurados e implantados. O código é então usado para automatizar a criação e configuração da infraestrutura. Com a abordagem IaC, as alterações na infraestrutura podem ser feitas facilmente, permitindo que as equipes de TI respondam rapidamente às necessidades da empresa. Além disso, IaC permite que as empresas gerenciem a infraestrutura de forma consistente e repetível, reduzindo erros e aumentando a eficiência.

### **Você tem familiriadidade com algum provedor de núvem pública ?**

Alguns exemplos de provedores de núvem pública são: Amazon Web Services (AWS), Google Cloud Platform (GCP) e Microsoft Azure.

### **O que é uma pipeline de deploy ?**

Uma pipeline de deploy é um conjunto de processos automatizados que permitem a entrega contínua de uma aplicação desde a fase de desenvolvimento até a produção. É um conceito importante em DevOps, pois permite uma implementação mais rápida, segura e confiável de novas funcionalidades ou correções de bugs.

A pipeline de deploy é composta por várias etapas, como a compilação do código, a realização de testes de unidade, testes de integração, testes de aceitação, empacotamento da aplicação, provisionamento de recursos de infraestrutura, deploy da aplicação em ambientes de testes e produção, entre outras.

Ao utilizar uma pipeline de deploy, as mudanças são implementadas com mais frequência e rapidez, reduzindo o tempo entre a escrita do código e a entrega do produto final. Isso permite que as equipes de desenvolvimento e operações possam trabalhar juntas de forma mais colaborativa e eficiente, garantindo uma maior qualidade do software entregue aos usuários finais.

### **O que é um quality gate ?**

Um quality gate é um conjunto de regras e critérios que um software deve atender antes de ser considerado apto para ser entregue ao usuário final. Essas regras geralmente incluem a cobertura de testes, o nível de qualidade do código, a conformidade com as políticas de segurança e a disponibilidade de documentação adequada.

Os quality gates são usados em pipelines de entrega contínua para garantir que o software seja de alta qualidade e atenda aos requisitos de negócios. Eles são geralmente automatizados e integrados na pipeline de entrega, de forma que, se uma das regras do quality gate não for atendida, a entrega é interrompida e os desenvolvedores precisam corrigir o problema antes de tentar novamente. Isso ajuda a garantir que os problemas sejam identificados e corrigidos o mais cedo possível, antes que o software seja entregue aos usuários finais.

### **O que é e qual a importância da análise estática de código ?**

Análise estática de código é o processo automatizado de análise de código fonte sem a sua execução. Ela verifica o código-fonte em busca de possíveis problemas e vulnerabilidades, como erros de sintaxe, variáveis não utilizadas, código duplicado, violações de padrões de codificação e possíveis vulnerabilidades de segurança.

A análise estática de código é importante porque permite que os desenvolvedores detectem e corrijam erros no código-fonte antes de implantar a aplicação, reduzindo a chance de problemas em produção. Além disso, ajuda a garantir a qualidade do código, melhorar a manutenibilidade, reduzir o tempo de desenvolvimento e diminuir os custos de correção de erros.

Existem diversas ferramentas que realizam análise estática de código, algumas delas gratuitas e outras pagas. As ferramentas de análise estática de código devem ser configuradas de acordo com as regras de negócio da aplicação e os padrões de codificação adotados pela equipe de desenvolvimento.

### **O que é escalabilidade horizontal ?**

Escalabilidade horizontal (também conhecida como "escalabilidade de dimensionamento horizontal") é um conceito de arquitetura de computadores que se refere à capacidade de aumentar a capacidade de processamento de um sistema adicionando mais recursos (como servidores, nós de processamento, unidades de armazenamento, etc.) em paralelo, em vez de aumentar a capacidade de um único recurso.

Em outras palavras, escalabilidade horizontal é a capacidade de aumentar a capacidade de um sistema adicionando mais instâncias do mesmo componente ou serviço, distribuindo o processamento entre eles. Isso pode ser alcançado por meio de técnicas de balanceamento de carga, em que as requisições são distribuídas entre as instâncias disponíveis, aumentando a capacidade total de processamento e reduzindo a carga em cada instância individual.

Escalabilidade horizontal é especialmente importante em aplicações modernas e sistemas distribuídos que precisam lidar com grandes quantidades de dados e/ou tráfego. Ele permite que o sistema cresça e se adapte às necessidades de maneira dinâmica e escalável, sem depender de atualizações ou upgrades caros de hardware. Além disso, escalabilidade horizontal também pode melhorar a disponibilidade e a tolerância a falhas do sistema, já que as instâncias adicionais podem assumir a carga de outras instâncias em caso de falhas ou interrupções inesperadas.

### **O que é escalabilidade vertical ?**

Escalabilidade vertical (também conhecida como "escalabilidade de dimensionamento vertical") é um conceito de arquitetura de computadores que se refere à capacidade de aumentar a capacidade de processamento de um sistema adicionando mais recursos a um único componente ou servidor, em vez de adicionar mais instâncias do mesmo componente.

Em outras palavras, escalabilidade vertical é a capacidade de aumentar a capacidade de um sistema adicionando mais poder de processamento, memória, capacidade de armazenamento, ou outros recursos a um único servidor ou componente. Isso pode ser alcançado por meio de atualizações de hardware ou software que aumentam o poder de processamento, a capacidade de memória ou o armazenamento de um único servidor.

Escalabilidade vertical é especialmente importante para aplicações que exigem alto desempenho em tarefas intensivas em recursos, como bancos de dados, sistemas de processamento de dados, e aplicações de análise de dados. No entanto, a escalabilidade vertical pode ter limitações físicas, como limitações de capacidade do hardware ou de compatibilidade entre os componentes, o que pode levar a custos mais altos e a tempo de inatividade significativo em caso de falhas de hardware.

Por outro lado, a escalabilidade horizontal pode oferecer maior flexibilidade, já que novas instâncias podem ser adicionadas de forma mais fácil e escalável, com custos mais baixos e menor tempo de inatividade. Cada abordagem tem seus prós e contras, e a escolha entre escalabilidade vertical e horizontal dependerá das necessidades específicas de cada aplicação e do sistema como um todo.

### **O que é observabilidade ?**

Observabilidade é uma prática de engenharia de software que se concentra em permitir que as equipes de desenvolvimento e operações monitorem, depurem e gerenciem sistemas de maneira eficiente e eficaz. A observabilidade é uma extensão da monitorização, mas em vez de se concentrar apenas nos dados de desempenho e disponibilidade, ela se concentra em todo o ecossistema do sistema, incluindo métricas, registros e rastreamentos.

A observabilidade ajuda as equipes de desenvolvimento a entender melhor o que está acontecendo em seus sistemas, incluindo possíveis pontos de falha ou gargalos. Isso pode ser alcançado por meio de recursos como instrumentação de código, rastreamento distribuído, análise de logs e análise de métricas de desempenho em tempo real.

Com a observabilidade, as equipes podem ter uma visão mais completa do sistema e responder rapidamente a problemas e anomalias, reduzindo o tempo de inatividade e melhorando a experiência do usuário. Além disso, a observabilidade pode ajudar a equipe a detectar e corrigir problemas de desempenho antes que eles se tornem críticos.

Em resumo, a observabilidade é uma prática importante em engenharia de software moderna e pode ajudar as equipes a desenvolver e manter sistemas escaláveis, confiáveis e seguros.