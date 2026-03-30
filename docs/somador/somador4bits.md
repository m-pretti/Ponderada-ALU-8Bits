### Documentação de Componente: Somador de 4 Bits (4-bit Ripple-Carry Adder)

**Função Principal:** 

Realizar a soma aritmética de dois números binários de 4 bits, processando também um bit de transporte inicial (`Cin`) e gerando um resultado de 4 bits, além de um transporte de saída final (`Cout`) indicando se houve "estouro" de capacidade (overflow).

#### 1. Entradas e Saídas

**Entradas (Inputs):**

* `A`: Primeiro número a ser somado (barramento de 4 bits).
* `B`: Segundo número a ser somado (barramento de 4 bits).
* `Cin` (Carry In): Bit de transporte de entrada (geralmente mantido em `0` para somas normais, ou `1` quando usado em operações de subtração).

**Saídas (Outputs):**

* `S`: O resultado da soma (barramento de 4 bits).
* `Cout` (Carry Out): Bit de transporte de saída final. Se for `1`, significa que a soma extrapolou o limite máximo de 4 bits (maior que 15 em decimal).

#### 2. Lógica Interna e Funcionamento

Este circuito adota uma arquitetura em cascata conhecida como **Ripple-Carry Adder (Somador de Propagação de Transporte)**. Ele reaproveita o bloco `somador1bit.dig` criado anteriormente, instanciando-o quatro vezes.

* **Separação de Bits (Splitters de Entrada):**

    * Os barramentos de 4 bits das entradas `A` e `B` são divididos em fios individuais (bits do menos significativo ao mais significativo). 
    
* **Processamento em Cascata:**

    * **1º Estágio (Bit menos significativo):** Recebe o 1º bit de A, o 1º bit de B e o `Cin` geral. Calcula o primeiro bit da soma (`S1`) e gera um transporte (`C1`).

    * **2º Estágio:** Recebe o 2º bit de A e B, e utiliza o `C1` do estágio anterior como seu transporte de entrada. Gera a soma (`S2`) e o transporte (`C2`).

    * **3º e 4º Estágios:** O processo se repete consecutivamente. O "vai-um" propaga (ripple) de um bloco para o próximo até o último estágio gerar o bit final da soma (`S4`) e o transporte de saída geral (`Cout`).

* **Agrupamento de Bits (Splitter de Saída):**

    * Os fios individuais resultantes de cada bloco (`S1`, `S2`, `S3`, `S4`) são combinados novamente por um componente Splitter invertido, formando o barramento único de 4 bits na saída `S`.

#### 3. Papel na ALU (Unidade Lógica e Aritmética)

O Somador de 4 bits é um passo intermediário crucial no desenvolvimento de processadores. 

* **Modularidade em Ação:** Ele prova o conceito de que operações complexas de múltiplos bits não exigem portas lógicas gigantescas, mas sim a repetição inteligente de um bloco básico (o Somador de 1 bit).

* **Expansão para 8 bits:** A sua ULA final utiliza operandos de 8 bits. Este somador documentado ilustra exatamente a mesma lógica utilizada na sua arquitetura final: basta colocar 8 blocos em vez de 4.

* **Base para Subtração:** Assim como no bloco de 1 bit, se a ULA enviar o complemento de 1 no operando B e um sinal `1` no `Cin`, este bloco de 4 bits realizará uma subtração de 4 bits perfeitamente.