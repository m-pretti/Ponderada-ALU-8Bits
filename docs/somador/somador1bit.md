### Documentação de Componente: Somador Completo de 1 Bit (Full Adder)

**Função Principal:** 

Realizar a soma aritmética de dois bits de entrada, levando em consideração o transporte de entrada Cin ("vai-um" anterior) e gerando um bit de soma e um possível transporte de saída Cout ("vai-um" atual).

#### 1. Entradas e Saídas

**Entradas (Inputs):**

* `A`: Primeiro bit a ser somado.
* `B`: Segundo bit a ser somado.
* `Cin` (Carry In): Bit de transporte de entrada (o "vai-um" que veio de uma soma de bits menos significativos).

**Saídas (Outputs):**

* `S`: O bit de resultado da soma.
* `Cout` (Carry Out): Bit de transporte de saída (o "vai-um" gerado pela soma atual, que será enviado para a próxima casa decimal/bit mais significativo).

#### 2. Lógica Interna e Funcionamento

O circuito foi implementado utilizando portas lógicas básicas a partir das equações booleanas tradicionais de um Somador Completo:

* **Cálculo da Soma (`S`):** 

    * O resultado da soma é `1` apenas quando o número de bits de entrada com valor `1` é ímpar.

    * **Implementação no circuito:** 
    
    * Utiliza-se uma porta lógica `XOR` de 3 entradas. Os sinais `A`, `B` e `Cin` são ligados diretamente a esta porta. A saída desta porta representa o pino `S`.

* **Cálculo do Transporte de Saída (`Cout`):**

    * O "vai-um" ocorre quando pelo menos duas das três entradas (`A`, `B`, `Cin`) são iguais a `1`.

    * **Implementação no circuito:** O circuito realiza isso em algumas etapas lógicas combinacionais:

        1.  Verifica se `A` e `B` são ambos verdadeiros usando uma porta `AND`.
        2.  Verifica se as entradas `A` e `B` geram um valor verdadeiro exclusivo (usando uma porta `XOR`) e, em caso positivo, faz uma operação `AND` com o `Cin`.
        3.  Os resultados dessas operações `AND` são unidos por uma porta `OR`. Se qualquer uma das condições for verdadeira, o `Cout` será `1`.
  

#### 3. Papel na ALU (Unidade Lógica e Aritmética)

O Somador de 1 bit é a unidade fundamental de processamento matemático da ULA. 

* **Soma de Múltiplos Bits:** Ao conectar a saída `Cout` de um somador na entrada `Cin` do somador adjacente (em cascata), cria-se um **Somador Ripple-Carry (RCA)**. Este arranjo permite que a ALU some números binários de 8, 16 ou 32 bits.

* **Subtração:** Este mesmo componente é reaproveitado para realizar operações de subtração (Opcode 1). Para isso, a ALU inverte todos os bits do operando B (fazendo o complemento de 1) e injeta um `1` inicial no pino `Cin` do primeiro somador (gerando o complemento de 2). Dessa forma, a soma se transforma logicamente em uma subtração.

* **Multiplicação e Divisão:** Operações aritméticas mais complexas, como multiplicação e divisão, são essencialmente construídas através de sucessivos ciclos de soma (ou subtração) combinados com deslocamentos (shifts), dependendo fortemente da integridade deste bloco básico.