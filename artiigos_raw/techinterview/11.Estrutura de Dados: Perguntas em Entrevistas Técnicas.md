## Objetivo

O intuíto desse artigo é expor algumas perguntas técnicas que eu já fiz em entrevistas técnicas ou perguntas que me foram feitas.

Essa parte tratará exclusivamente sobre estrutura de dados  ok ;D ? Outras publicações serão escritas com mais conteúdo específico.

As respostas são apenas a minha visão sobre o assunto, por tanto, de forma alguma são verdade absoluta, e inclusive pode ser que haja espaço para melhorias, correções e até visões diferentes das apresentadas aqui, fique a vontade, use os comentários e enriqueça a discussão.

## Estrutura de Dados

### **Como você explicaria fila e pilha para um criança ?**
Uma fila é como uma fila de espera para brincar em um brinquedo. As crianças precisam esperar sua vez para brincar, e a primeira que chega é a primeira a brincar. Ninguém pode furar a fila e passar na frente dos outros, a menos que haja uma razão especial, como ser muito pequeno ou ter algum problema de saúde. É assim que uma fila funciona.

Uma pilha é como uma torre de blocos. Você empilha um bloco em cima do outro, sempre deixando o último bloco que foi colocado no topo da torre. Quando você quer remover um bloco, precisa remover o último bloco colocado na torre, o que faz com que os outros blocos acima dele caiam um pouco. É assim que uma pilha funciona.

### **O que é análise ciclomática ?**

**TL;DR:** Análise ciclomática é uma técnica de análise estática que pode ajudar os desenvolvedores a identificar e reduzir a complexidade do código, tornando-o mais fácil de manter e menos propenso a erros.

A análise ciclomática é uma técnica de análise estática de código que mede a complexidade de um programa de software. A técnica foi desenvolvida por Thomas McCabe em 1976 e utiliza o grafo de fluxo de controle para determinar o número mínimo de testes necessários para cobrir todas as possíveis trajetórias de execução de um programa.

O grafo de fluxo de controle é um modelo gráfico que representa o fluxo de controle de um programa de software. A partir desse grafo, é possível calcular um número chamado de "complexidade ciclomática", que é uma medida quantitativa da complexidade do programa.

A complexidade ciclomática é calculada contando o número de caminhos independentes através do grafo de fluxo de controle. Esse número representa o número mínimo de casos de teste necessários para garantir que todas as instruções do programa sejam executadas pelo menos uma vez.

A análise ciclomática é uma técnica útil para avaliar a complexidade do código e identificar possíveis pontos de falha. Ela pode ser usada para determinar a complexidade de um programa e ajudar na tomada de decisões de projeto, como refatorar o código para reduzir a complexidade ou adicionar mais testes para cobrir todas as possíveis trajetórias de execução.

### **O que é e pra que serve Big O Annotation ?**

A Big O notation é uma forma de medir a complexidade de um algoritmo, descrevendo o seu tempo de execução e a quantidade de memória necessária para executá-lo. A notação Big O é geralmente usada para descrever o pior caso de tempo de execução de um algoritmo, ou seja, o tempo de execução quando o algoritmo recebe a entrada mais difícil possível.

A Big O notation é representada por uma função matemática que descreve a taxa de crescimento do tempo de execução ou uso de memória em relação ao tamanho da entrada. A notação é escrita como O(f(n)), onde "f(n)" é uma função que representa a complexidade do algoritmo. Por exemplo, se um algoritmo tem uma complexidade de O(n), isso significa que seu tempo de execução aumenta linearmente com o tamanho da entrada.

A utilização da notação Big O é importante para que os desenvolvedores possam avaliar o desempenho dos algoritmos e escolher a melhor opção para cada situação. Isso ajuda a evitar gargalos e melhorar a eficiência do código. As Big O annotations são utilizadas para indicar a complexidade de um método ou função, permitindo que outros desenvolvedores saibam como ele se comporta em relação ao tempo e memória em diferentes situações.

Na Big O, existem diversas notações utilizadas para analisar o desempenho dos algoritmos. Algumas das principais notações são:

* **O(1):** Indica que o tempo de execução do algoritmo é constante, independentemente do tamanho da entrada.

* **O(log n):** Indica que o tempo de execução do algoritmo cresce de forma logarítmica em relação ao tamanho da entrada. Algoritmos com essa complexidade são considerados muito eficientes.

* **O(n):** Indica que o tempo de execução do algoritmo é proporcional ao tamanho da entrada. Algoritmos com essa complexidade são considerados razoavelmente eficientes.

* **O(n log n):** Indica que o tempo de execução do algoritmo cresce em função do produto entre o tamanho da entrada e o logaritmo desse tamanho. Algoritmos com essa complexidade são considerados eficientes.

* **O(n²):** Indica que o tempo de execução do algoritmo cresce de forma quadrática em relação ao tamanho da entrada. Algoritmos com essa complexidade são considerados ineficientes para entradas grandes.

* **O(2^n):** Indica que o tempo de execução do algoritmo cresce exponencialmente em relação ao tamanho da entrada. Algoritmos com essa complexidade são considerados extremamente ineficientes e geralmente inviáveis para entradas de tamanho significativo.

### **Cite 3 exemplos de algoritmos de ordenação**

1. **Bubble Sort:** um algoritmo simples que percorre a lista diversas vezes, comparando elementos adjacentes e trocando-os se estiverem na ordem errada.

2. **Merge Sort:** um algoritmo divide a lista em duas metades, ordena cada metade recursivamente e, em seguida, combina as duas metades em uma única lista ordenada.

3. **Quick Sort:** um algoritmo que seleciona um elemento como "pivot" e divide a lista em dois grupos - um grupo com valores menores que o pivot e outro grupo com valores maiores. Esse processo é realizado recursivamente para cada subgrupo, até que toda a lista esteja ordenada.

### **Entre Bubble Sort, Merge Sort e Quick Sort qual o mais e o menos eficiente ?**

O algoritmo mais eficiente entre os três citados é o **Quicksort**, que tem complexidade média O(n log n) e pior caso O(n^2), mas na prática tem um bom desempenho em muitos casos. Em seguida vem o **Mergesort**, que tem complexidade O(n log n) tanto no caso médio quanto no pior caso, sendo uma boa opção para dados em memória externa. Já o **Bubblesort** é o menos eficiente dos três, com complexidade O(n^2) tanto no caso médio quanto no pior caso.