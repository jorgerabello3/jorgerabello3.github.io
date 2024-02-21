## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente de arquitetura e design ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Arquitetura de Software

### **O que é arquitetura hexagonal ? E quais os seus princípios ?**

A arquitetura hexagonal, também conhecida como arquitetura ports and adapters, é um estilo arquitetural que visa separar as preocupações (concerns) do núcleo da aplicação das suas dependências externas, como frameworks, banco de dados, interfaces de usuário, etc.

Os princípios da arquitetura hexagonal incluem:

1. **Separação de responsabilidades:** a aplicação deve ser dividida em camadas que possuem responsabilidades bem definidas, como a camada de domínio, de aplicação e de infraestrutura.

2. **Inversão de dependências:** as dependências da aplicação devem ser definidas em termos de interfaces, e não de implementações concretas, para permitir que as mesmas possam ser facilmente trocadas.

3. **Testabilidade:** a arquitetura deve ser projetada de forma a permitir que os testes sejam executados facilmente, sem a necessidade de componentes externos ou infraestrutura complexa.

4. **Flexibilidade:** a arquitetura deve ser projetada de forma a permitir que a aplicação possa ser facilmente adaptada a novos requisitos e mudanças no ambiente em que ela é executada.

Independência de tecnologia: a arquitetura deve ser projetada de forma a minimizar o acoplamento com tecnologias específicas, permitindo que as mesmas possam ser facilmente substituídas ou atualizadas.

Esses princípios buscam criar uma arquitetura que permita o desenvolvimento de software flexível, testável e facilmente mantido, que possa se adaptar facilmente a mudanças nos requisitos ou no ambiente em que a aplicação é executada.

### **O que é arquitetura limpa ?**

A Arquitetura Limpa (Clean Architecture) é um padrão de arquitetura de software proposto pelo renomado engenheiro de software, Uncle Bob Martin. Ela se concentra na separação de responsabilidades entre os componentes de um sistema, para torná-lo mais modular, escalável e testável.

A ideia principal da Arquitetura Limpa é separar a lógica de negócios do restante da aplicação, criando camadas de abstração que permitam a fácil manutenção e evolução do sistema. Para isso, a arquitetura é dividida em camadas que são organizadas em forma de círculos concêntricos, sendo cada camada dependente apenas da camada interna a ela.

Os princípios da Arquitetura Limpa são:

1. **Independência de frameworks e bibliotecas externas:** as camadas mais externas da arquitetura não devem depender de frameworks ou bibliotecas externas, permitindo a fácil substituição ou atualização desses componentes.

2. **Independência de tecnologias:** as camadas mais internas da arquitetura, responsáveis pela lógica de negócios, devem ser independentes de tecnologias específicas, como banco de dados, interface do usuário ou protocolos de rede.

3. **Separação de preocupações:** cada camada da arquitetura deve ter uma única responsabilidade, evitando a mistura de lógica de negócios com a lógica de apresentação ou persistência de dados.

4. **Testabilidade:** a arquitetura deve ser projetada de forma a permitir a fácil criação de testes automatizados para todas as camadas.

5. **Manutenibilidade:** a arquitetura deve permitir a fácil manutenção e evolução do sistema ao longo do tempo, sem causar impactos em outras partes do código.

### **O que é e quais são os 12 fatores em se tratando de design de software ?**

Os 12 fatores são um conjunto de princípios que visam guiar a construção de sistemas escaláveis, resilientes e sustentáveis, desenvolvido por Adam Wiggins em 2011. São eles:

1. **Base de código:** mantenha um repositório de código único, rastreável e versionado;

2. **Dependências:** declare, isole e gerencie explicitamente as dependências de bibliotecas e ferramentas utilizadas;

3. **Configuração:** armazene a configuração do sistema em variáveis de ambiente;

4. **Serviços de suporte:** trate serviços de suporte como recursos, acessados via rede;

5. **Build, release, run:** separe os estágios de build, release e execução do sistema;

6. **Processos:** execute o sistema como um ou mais processos sem estado;

7. **Vinculação de porta:** exporte serviços por meio de portas atribuídas explicitamente;

8. **Concorrência:** dimensione por meio do modelo de processo, usando processos sem estado e compartilhando recursos;

9. **Descartabilidade:** maximize a robustez com inicialização rápida e finalização graciosa;

10. **Paridade de desenvolvimento/produção:** mantenha o ambiente de desenvolvimento, teste e produção tão semelhante quanto possível;

11. **Registros:** trate o registro como fluxo de eventos;

12. **Processos de administração:** execute tarefas administrativas/gerenciais como processos ad-hoc.

Esses fatores foram criados para orientar a construção de sistemas modernos, escaláveis e que possam ser facilmente mantidos. A aplicação desses princípios pode resultar em um software mais modular, flexível e com melhor capacidade de evolução.

### **O que são aplicações monolíticas ? Quais as vantagens e desvantagens ?**

Aplicações monolíticas são aplicações de software onde todo o código fonte é empacotado e implantado como um único artefato. Geralmente, esse artefato inclui todos os componentes da aplicação, como o servidor web, o banco de dados, a lógica de negócios, etc.

*Algumas vantagens de aplicações monolíticas incluem:*

1. **Fácil de desenvolver e testar:** como todo o código da aplicação é empacotado em um único artefato, é fácil de construir e testar a aplicação como um todo.

2. **Escalabilidade vertical:** como todos os componentes da aplicação estão no mesmo processo, a escalabilidade vertical é simples, ou seja, basta adicionar mais recursos (como CPU, memória, etc.) ao servidor.

3. **Gerenciamento mais simples:** como toda a aplicação é empacotada em um único artefato, é mais fácil de gerenciar a aplicação como um todo.

*Algumas desvantagens de aplicações monolíticas incluem:*

1. **Escalabilidade limitada:** em aplicações monolíticas, é difícil escalar componentes específicos da aplicação de forma independente.

2. **Acoplamento elevado:** em aplicações monolíticas, é comum que os diferentes componentes da aplicação estejam acoplados de forma intensa, o que pode tornar a manutenção e evolução da aplicação mais complexas.

3. **Dificuldade de atualização:** em aplicações monolíticas, a atualização de um único componente pode exigir a reimplantação de toda a aplicação, o que pode levar a um tempo de inatividade significativo.

### **O que são microserviços ? Quais as vantagens e desvantagens ?**

**TL;DR:** Microserviços oferecem vantagens em termos de escalabilidade, manutenibilidade, resiliência e flexibilidade, mas também apresentam desafios em termos de complexidade, overhead de comunicação, teste e gerenciamento de transações.

Microserviços é um estilo arquitetural que consiste em desenvolver e implantar uma aplicação como um conjunto de serviços independentes, cada um executando um processo separado e se comunicando com os outros através de mecanismos de comunicação bem definidos, como APIs RESTful ou mensageria assíncrona.

*Algumas vantagens dos microserviços incluem:*

1. **Escalabilidade:** cada serviço pode ser escalado independentemente, permitindo melhor desempenho e utilização de recursos.

2. **Manutenibilidade:** com cada serviço isolado e coeso, é mais fácil modificar e atualizar uma parte da aplicação sem afetar outras partes.

3. **Resiliência:** em caso de falhas em um serviço, os outros serviços ainda podem funcionar normalmente, aumentando a disponibilidade da aplicação como um todo.

4. **Tecnologias diferentes:** diferentes serviços podem ser desenvolvidos usando tecnologias diferentes, permitindo maior flexibilidade e escolha de tecnologias.

*Algumas desvantagens dos microserviços incluem:*

1. **Complexidade:** gerenciar uma aplicação com múltiplos serviços pode ser mais complexo do que gerenciar uma aplicação monolítica.

2. **Overhead de comunicação:** com a comunicação entre serviços, há um overhead de latência e tráfego de rede que pode afetar o desempenho da aplicação como um todo.

3. **Testes:** testar uma aplicação com múltiplos serviços pode ser mais complexo e exigir ferramentas e técnicas adicionais.

4. **Gerenciamento de transações:** coordenar transações que envolvem múltiplos serviços pode ser mais complexo do que gerenciar transações em uma aplicação monolítica.