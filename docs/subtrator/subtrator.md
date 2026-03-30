### Documentação de Componente: Somador/Subtrator de 8 Bits (Adder/Subtractor)


**Função Principal:** 

Realizar de forma dinâmica operações de adição matemática pura ou subtração (utilizando a lógica de complemento de 2) entre dois números de 8 bits. A operação é definida por um único pino de controle (`Op`).

#### 1. Entradas e Saídas

**Entradas (Inputs):**

* `A`: Primeiro operando (barramento de 8 bits).
* `B`: Segundo operando (barramento de 8 bits).
* `Op` (Operação): Bit de controle que define o modo do circuito. 
    * Se `Op = 0`: O circuito realiza uma **Soma** ($A + B$).
    * Se `Op = 1`: O circuito realiza uma **Subtração** ($A - B$).

**Saídas (Outputs):**

* `S` (Resultado): O resultado da operação de 8 bits, roteado diretamente para dois displays hexadecimais para fácil leitura visual.
* `Cout` (Carry Out): Bit de transporte final. Na soma, indica "estouro" (overflow). Na subtração, ele normalmente é ignorado em operações simples, mas pode ser usado para verificar se o resultado é positivo ou negativo dependendo da arquitetura.

#### 2. Lógica Interna e Funcionamento

Este circuito é uma evolução inteligente do somador de 8 bits. Em vez de criar um hardware totalmente novo para subtrair, ele "engana" os somadores utilizando a matemática binária (Complemento de 2).

* **Inversão de B (Porta NOT e Túnel `R`):**

    * O barramento `B` entra no circuito e imediatamente passa por um bloco lógico `NOT` de 8 bits. Esse bloco inverte todos os zeros para uns e uns para zeros (gerando o **Complemento de 1**). Esse sinal invertido é guardado no túnel `R`.

* **Seleção de Operação (Multiplexador):**

    * Um MUX de 8 bits atua como um "trilho de trem". Ele recebe o `B` original e o `B` invertido (`R`). 
    * O pino `Op` é conectado ao seletor desse MUX. 
    * Se `Op = 0`, o MUX deixa passar o `B` normal.
    * Se `Op = 1`, o MUX barra o `B` normal e deixa passar o `B` invertido.

* **O Truque do Complemento de 2 (Pino `Cin`):**
    * O grande segredo deste circuito é que o pino `Op` também está ligado diretamente na entrada de "vai-um" (`Cin`) do primeiro bloco somador inferior.
    * Quando `Op = 1` (Subtração), o somador recebe o sinal de `B` invertido *E* recebe um `1` no `Cin`. Inverter os bits e somar 1 é a definição matemática exata de transformar um número binário no seu negativo (**Complemento de 2**). O somador então calcula $A + (-B)$, que é igual a $A - B$.
    * Quando `Op = 0` (Soma), o somador recebe o `B` normal e um `0` no `Cin`, realizando a soma tradicional.

* **Processamento (Blocos de 4 bits em cascata):**

    * Assim como no somador puro, os dados que saem do MUX são divididos em "nibbles" (`BL` e `BM`) e somados com `AL` e `AM` usando dois blocos `somador4bits.dig` encadeados pelo túnel de transporte `C1`.

#### 3. Papel na ALU (Unidade Lógica e Aritmética)

Este é um dos componentes de hardware mais geniais e eficientes da computação clássica.

* **Otimização de Hardware:** Ao invés da sua ULA ter um chip gigante só para somar e outro chip gigante só para subtrair, ela usa um circuito só para as duas coisas. Isso economiza espaço, energia e custo na fabricação de processadores reais.

* **Atendimento aos Opcodes:** Na sua arquitetura final, é este o circuito responsável por resolver o **Opcode 0** e o **Opcode 1**. O bit que aciona o pino `Op` provavelmente sai do seu Demultiplexador de instruções lá no painel principal.

* **Base para Multiplicação/Divisão:** Como este bloco domina tanto a ida quanto a volta aritmética, ele é a ferramenta acionada pelo seu Multiplicador (que fará várias somas seguidas) e pelo Divisor (que fará várias subtrações seguidas).