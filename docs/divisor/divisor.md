### Documentação: Estágio Divisor (Célula de 1 bit do Quociente)

**Função:** 

Este estágio compara o valor atual (`Ain`) com o divisor (`Bin`). Se o divisor for menor ou igual ao dividendo, ele realiza a subtração e passa o resto adiante; caso contrário, ele mantém o dividendo original para o próximo estágio.

### 1. Componentes e Lógica

* **Subtrator de 8 bits (`subtrator8bits.dig`):** * Calcula $Ain - Bin$. 

    * A constante `1` no pino inferior (Carry In) indica que ele está configurado para operação de subtração (complemento de 2).

    * **Cout (Borrow):** O segredo está aqui. Na lógica de subtratores digitais, o `Cout` indica se o resultado foi positivo ou se houve um "empréstimo". Se $Ain \geq Bin$, o `Cout` será **1**.

* **Multiplexador (MUX) de 8 bits:**

    * **Entrada 0:** Recebe o `Ain` original (não coube a divisão).

    * **Entrada 1:** Recebe o resultado da subtração `S` (coube a divisão).

    * **Seletor:** Conectado ao `Cout`. 

* **Saídas:**

    * **Q (Quociente):** É o próprio `Cout`. Se o divisor coube no dividendo, este bit do quociente é `1`, caso contrário, `0`.
    
    * **R (Resto):** A saída do MUX. É o valor que será passado para o próximo estágio da cascata de divisão.

### 2. Funcionamento do Algoritmo

Para formar um divisor completo de 8 bits, você precisaria empilhar **8 instâncias** deste mesmo circuito (em cascata). O fluxo seria assim:

| Passo | Ação | Resultado |
| :--- | :--- | :--- |
| **Tentativa** | O estágio calcula $R = Ain - Bin$. | O subtrator trabalha o tempo todo. |
| **Decisão** | Se $Ain < Bin$, a subtração gera um "borrow" (Cout=0). | O MUX ignora a subtração e solta $R = Ain$. |
| **Confirmação** | Se $Ain \geq Bin$, não há borrow (Cout=1). | O MUX solta $R = S$ e o bit do quociente vira `1`. |

### 3. Exemplo Prático

Se você tiver $Ain = 10$ e $Bin = 3$:

1. O subtrator faz $10 - 3 = 7$.

2. Como $10 \geq 3$, o `Cout` é `1`.

3. A saída **Q** será `1` (primeiro bit do quociente encontrado).

4. A saída **R** será `7` (o que sobrou para o próximo estágio tentar subtrair 3 de novo).