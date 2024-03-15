## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente sobre a J.V.M. (Java Virtual Machine)  ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## J.V.M. (Java Virtual Machine)

### **O que é bytecode ?**

Bytecode é um código de instruções que é gerado a partir do código-fonte de um programa, e é projetado para ser executado em uma máquina virtual. Ele é composto por instruções de baixo nível que são interpretadas e executadas pela máquina virtual. O bytecode é um intermediário entre o código-fonte e o código de máquina nativo, o que permite que o mesmo código bytecode possa ser executado em diferentes plataformas que suportem a mesma máquina virtual. O bytecode é amplamente utilizado em linguagens de programação como Java, Python e Ruby.

### **Qual a diferença entre JDK, JRE e JVM ?**

**JDK, JRE e JVM** são siglas comuns no universo do desenvolvimento em Java. Veja abaixo a definição de cada um deles:

**JDK (Java Development Kit):** É um conjunto de ferramentas para desenvolvimento de software em Java. Ele inclui o compilador Java, que é responsável por transformar o código-fonte em bytecode, e outras ferramentas, como o javadoc e o debugger.

**JRE (Java Runtime Environment):** É o ambiente de execução para aplicações Java. Ele inclui a JVM e as bibliotecas necessárias para executar programas em Java.

**JVM (Java Virtual Machine):** É a máquina virtual que executa o bytecode gerado pelo compilador Java. Ela é responsável por carregar as classes, verificar o bytecode, interpretá-lo e convertê-lo em código de máquina.

Em resumo, o JDK é utilizado para desenvolvimento de software em Java, o JRE é utilizado para executar aplicações em Java e a JVM é responsável por executar o bytecode gerado pelo compilador Java.

### **Como você explicaria o funcionamento da JVM pra uma criança ?**

A JVM (Java Virtual Machine) é como se fosse uma casa de brinquedo para os programas escritos em Java. Assim como uma casa de bonecas pode ser colocada em diferentes lugares, como na sala ou no quarto, a JVM pode ser colocada em diferentes tipos de computadores, como no seu notebook ou no celular.

Quando você escreve um programa em Java, ele precisa ser traduzido para uma linguagem que o computador entenda. A JVM é como um tradutor que entende a linguagem em que o programa foi escrito (chamada de bytecode) e o transforma em instruções que o computador pode executar.

Então, a JVM garante que programas escritos em Java possam ser executados em diferentes tipos de computadores, sem a necessidade de reescrever o programa para cada um deles. É como se a JVM fosse um super-herói que salva o dia quando precisamos executar programas em Java!

### **Quais as vantagens em se utilizar uma JVM comparado a outras linguagens ?**

Uma das principais vantagens de se utilizar uma JVM (Java Virtual Machine) é a portabilidade de código. Como a JVM é capaz de executar o bytecode gerado por compiladores de diversas linguagens, o código escrito em uma plataforma pode ser executado em qualquer outra plataforma que tenha uma JVM compatível. Além disso, a JVM oferece um gerenciamento de memória automático, o que ajuda a evitar erros comuns de alocação de memória. Outra vantagem é a grande quantidade de bibliotecas disponíveis na plataforma Java, que permitem desenvolver rapidamente aplicações complexas e robustas.

A JVM também é conhecida por ter um bom desempenho e segurança, graças à sua estrutura e aos diversos mecanismos de segurança que possui. Por fim, a JVM é uma plataforma de desenvolvimento madura e amplamente adotada, o que significa que há uma grande comunidade de desenvolvedores, ferramentas e recursos disponíveis para ajudar no desenvolvimento de aplicativos.

### **Você saberia dizer o que é e como funciona o Garbage Collector ?**

O Garbage Collector é um mecanismo de gerenciamento de memória que é responsável por liberar automaticamente a memória utilizada por objetos que não são mais referenciados pelo programa em execução.

Basicamente, quando um objeto é criado na memória heap da JVM, um ponteiro é criado para que o programa possa acessá-lo. Quando o objeto não é mais referenciado, ele se torna elegível para ser coletado pelo Garbage Collector. O Garbage Collector é executado periodicamente em segundo plano, analisando a memória heap e identificando os objetos que não são mais referenciados. Em seguida, o Garbage Collector libera a memória alocada por esses objetos, tornando-a disponível para ser reutilizada por outros objetos.

O Garbage Collector oferece a vantagem de permitir que os desenvolvedores escrevam código sem se preocupar em liberar a memória alocada para objetos que não são mais necessários. Isso ajuda a evitar erros de vazamento de memória, que podem ser difíceis de detectar e corrigir. Além disso, o Garbage Collector permite que os programas Java executem de forma mais eficiente, pois a memória é alocada de forma dinâmica e otimizada pelo Garbage Collector.

### **Pra que serve o parâmetro -Xprof na JVM ?**

O parâmetro `-Xprof` é utilizado para ativar o profiler da JVM, que é uma ferramenta que coleta informações sobre a execução de um programa Java e gera relatórios de análise de desempenho. Esses relatórios podem ser usados para identificar gargalos no código, otimizar a performance e melhorar a eficiência do programa.

Ao usar o parâmetro `-Xprof`, a JVM coleta informações sobre o número de vezes que cada método é chamado, o tempo de execução de cada método, o número de alocações de memória, entre outros dados. Essas informações são salvas em um arquivo de saída que pode ser analisado posteriormente.

É importante notar que o uso do parâmetro `-Xprof` pode afetar o desempenho do programa, pois a coleta de informações adicionais pode consumir recursos do sistema. Portanto, é recomendável usá-lo apenas em ambiente de desenvolvimento ou em situações em que a análise de desempenho é necessária.

Uma vez habilitado, os dados de profiling são gravados em um arquivo de saída. Para acessar esses dados, você precisa analisar o arquivo gerado. O formato do arquivo de saída é específico para o `-Xprof` e não é fácil de ser lido diretamente.

Existem diversas ferramentas que podem ser utilizadas para analisar esses dados e gerar relatórios mais fáceis de serem interpretados. Algumas dessas ferramentas incluem o VisualVM, o JProfiler e o Java Mission Control. Cada ferramenta tem suas próprias características e recursos, mas todas elas permitem que você analise os dados coletados pelo `-Xprof` de uma maneira mais fácil e eficiente.

### **Pra que serve o parâmetro -Xss na JVM ?**

O parâmetro `-Xss` é utilizado para definir o tamanho máximo da pilha de execução de uma thread na JVM. A pilha de execução é usada para armazenar informações sobre os métodos que estão sendo executados em uma thread, incluindo os parâmetros, variáveis locais e valores de retorno.

Quando um método é chamado, um novo quadro (frame) é adicionado ao topo da pilha de execução da thread para armazenar as informações do método. Quando o método retorna, o quadro é removido da pilha.

O valor padrão do parâmetro -Xss é geralmente definido pelo sistema operacional e pode variar dependendo do ambiente de execução. Em algumas situações, pode ser necessário aumentar ou diminuir o tamanho da pilha de execução para evitar erros como StackOverflowError.

Por exemplo, se você estiver enfrentando o erro StackOverflowError em sua aplicação Java, pode tentar aumentar o valor do parâmetro `-Xss` para fornecer mais espaço para a pilha de execução e evitar o erro. No entanto, é importante ter cuidado ao ajustar esse parâmetro, pois um valor muito alto pode levar a problemas de desempenho ou até mesmo esgotar a memória disponível.

### **Pra que serve o parâmetro -Xmx na JVM ?**

O parâmetro `-Xmx` é usado para especificar o tamanho máximo da memória heap disponível para a JVM. A memória heap é a região da memória onde os objetos Java são alocados durante a execução do programa. Quando a memória heap atinge seu limite máximo, a JVM pode lançar um erro de OutOfMemoryError.

O uso desse parâmetro é importante para garantir que a JVM tenha memória suficiente para executar o programa sem que ocorram erros de falta de memória. É comum que programas maiores ou que precisem lidar com grandes quantidades de dados requeiram mais memória heap, e nesses casos é necessário aumentar o valor do parâmetro `-Xmx`.

O valor do parâmetro é especificado em bytes, mas pode ser abreviado usando sufixos como k para kilobytes, m para megabytes e g para gigabytes. Por exemplo, para especificar um limite de memória heap de 2 gigabytes, o valor do parâmetro seria -`Xmx2g`.

### **Pra que serve o parâmetro -XX:+UseParallelGC na jvm ?**

O parâmetro `-XX:+UseParallelGC` é utilizado para habilitar o coletor de lixo paralelo na JVM. Esse coletor de lixo é responsável por coletar objetos não utilizados na memória e liberar espaço para alocar novos objetos. Ele trabalha de forma paralela, utilizando várias threads para realizar a coleta de forma mais eficiente.

Ao habilitar esse parâmetro, a JVM utilizará o coletor de lixo paralelo como padrão, que é indicado para sistemas que necessitam de alta taxa de processamento e de grande quantidade de memória. Porém, em alguns casos, pode ser necessário ajustar outros parâmetros da JVM para otimizar o desempenho do coletor de lixo paralelo ou utilizar outro tipo de coletor, dependendo das características da aplicação e do ambiente em que ela está sendo executada.

### **Pra que serve o parâmetro -XX:+UseConcMarkSweepGC na jvm ?**

O parâmetro `-XX:+UseConcMarkSweepGC` é usado para habilitar o coletor de lixo de marcação de varredura simultânea concorrente (Concurrent Mark Sweep - CMS) na JVM. Este coletor de lixo é projetado para minimizar o tempo de pausa da JVM para a coleta de lixo, permitindo que o aplicativo continue executando enquanto o coletor de lixo trabalha nos bastidores.

O CMS usa um algoritmo de duas fases que marca os objetos usados e, em seguida, os varre em segundo plano para liberar a memória alocada para os objetos inutilizados. Esse processo pode ser feito em paralelo para melhorar ainda mais o desempenho.

Essa opção é especialmente útil em aplicativos que precisam manter tempos de resposta curtos e não podem tolerar longos tempos de pausa da JVM durante a coleta de lixo.

### **O que é heap memory ?**

Heap memory (ou memória heap) é uma região de memória do processo que é usada para alocação dinâmica de objetos em tempo de execução em linguagens de programação como Java e C#. É chamada de heap porque, ao contrário da pilha (stack), que tem um tamanho fixo, a heap pode crescer e diminuir conforme necessário. A heap memory é gerenciada automaticamente pela JVM (Java Virtual Machine) ou pelo CLR (Common Language Runtime) do .NET Framework, através do mecanismo de coleta de lixo (garbage collector), que libera automaticamente a memória não utilizada pelos objetos, permitindo que novos objetos sejam alocados.