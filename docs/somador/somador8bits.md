### Documentação de Componente: Somador de 8 Bits (8-bit Ripple-Carry Adder)

**Função Principal:** 

Realizar a soma aritmética de dois barramentos binários de 8 bits (um byte completo), processando um bit de transporte inicial (`Cin`) e gerando um resultado unificado de 8 bits, além de um transporte de saída final (`Cout`) para indicar *overflow* (estouro da capacidade de 8 bits, ou seja, resultados maiores que 255 em decimal).

#### 1. Entradas e Saídas

**Entradas (Inputs):**

* `A`: Primeiro operando de 8 bits.
* `B`: Segundo operando de 8 bits.
* `Cin` (Carry In): Bit de transporte de entrada. Fundamental para operações de subtração (onde recebe `1`) ou para encadear este somador em arquiteturas de 16 bits.

**Saídas (Outputs):**

* `S`: O resultado da operação (barramento de 8 bits).
* `Cout` (Carry Out): Bit de transporte final indicando o estouro do bit mais significativo.

#### 2. Lógica Interna e Funcionamento

Este componente é um excelente exemplo de design hierárquico (construir blocos maiores a partir de blocos menores). Ele utiliza **duas instâncias** do `somador4bits.dig` documentado anteriormente.

* **Divisão dos Dados (Splitters de Entrada):**

    * Os barramentos de 8 bits `A` e `B` são divididos ao meio por componentes Splitters, gerando dois "nibbles" (pacotes de 4 bits) para cada entrada.
    * `AL` e `BL` (Low): Representam os 4 bits menos significativos (metade inferior).
    * `AM` e `BM` (Most/High): Representam os 4 bits mais significativos (metade superior).

* **Cascata de Blocos de 4 Bits:**

    * **Bloco Inferior (LSB):** Recebe `AL`, `BL` e o pino principal `Cin`. Ele calcula a primeira metade da soma (`S1`) e gera um "vai-um" intermediário, nomeado através do túnel `C1`.

    * **Bloco Superior (MSB):** Recebe `AM`, `BM` e utiliza o túnel `C1` como sua entrada de "vai-um". Ele calcula a segunda metade da soma (`S2`) e gera o transporte final para fora do circuito (`Cout`).
    
* **Reconstrução do Dado (Splitter de Saída):**
    * Os barramentos de 4 bits gerados (`S1` e `S2`) são roteados para um Splitter invertido (que atua como um unificador/combinador). Ele junta as duas partes na ordem correta, entregando o resultado final inteiro de 8 bits no pino `S`.

#### 3. Papel na ALU (Unidade Lógica e Aritmética)

O Somador de 8 bits é o "coração matemático" da sua ULA e o bloco mais pesado em termos de processamento aritmético.

* **Adequação à Arquitetura:** Como os registradores `AC` e `MQ` do seu projeto são de 8 bits, este é o componente exato acionado durante o **Opcode 0 (Soma)**. Ele processa a largura de dados nativa da sua máquina.

* **Múltiplas Funções:** É o bloco diretamente responsável por executar o **Opcode 1 (Subtração)** (recebendo a entrada B invertida e Cin em 1) e atua internamente nas engrenagens das operações de **Multiplicação (Opcode 2)** e **Divisão (Opcode 3)**, que dependem de somas/subtrações sucessivas de 8 bits para chegarem aos seus resultados.

* **Escalabilidade:** Demonstra como um processador real é construído. Para fazer uma ULA de 16 ou 32 bits no futuro, bastaria replicar este mesmo bloco e conectar o `Cout` de um no `Cin` do outro.