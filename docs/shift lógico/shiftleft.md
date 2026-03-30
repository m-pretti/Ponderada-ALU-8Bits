### Documentação de Componente: Deslocador à Esquerda de 8 Bits (Shift Left)

**Função Principal:** 

Realizar o deslocamento lógico de todos os bits do registrador Acumulador (`AC`) uma posição para a esquerda. Essa operação desloca o barramento, insere um zero na posição menos significativa (LSB) e descarta o bit mais significativo (MSB). Matematicamente, equivale a multiplicar o valor por 2 de forma ultrarrápida (ignorando eventuais *overflows*).

#### 1. Entradas e Saídas

**Entradas (Inputs):**

* `AC` (Acumulador): Barramento de 8 bits que contém o valor atual a ser deslocado.
* `Constante 0`: Um valor de hardware fixo em `0` que será "injetado" no novo número.

**Saídas (Outputs):**

* `SHIFTLEFT`: Barramento de 8 bits com o resultado final do deslocamento (conectado via túnel `SL`).

#### 2. Lógica Interna e Funcionamento

O funcionamento se dá em três passos:

* **Passo 1: Separação (Splitter de Desmembramento):**

    * O barramento `AC` entra em um Splitter configurado com divisão `8 -> 7,1` (Lê-se: separe 7 bits de um lado e 1 bit do outro).
    * O túnel `A3` recebe os 7 bits menos significativos (bits de 0 a 6).
    * O túnel `A4` recebe isoladamente o 1 bit mais significativo (bit 7, o MSB).

* **Passo 2: Injeção do LSB:**

    * Um componente `Const` de valor `0` (1 bit) é posicionado para ser o novo "início" do barramento.

* **Passo 3: Reconstrução (Splitter Combinador):**

    * Um segundo Splitter invertido é usado com a configuração `1,7 -> 8`. Ele atua como um unificador.
    * O pino `0` deste Splitter (1 bit) recebe a **Constante 0**.
    * O pino `1` deste Splitter (7 bits) recebe o túnel `A3` (os 7 bits originais de baixo).

    * **Resultado:** Os bits que antes ocupavam as posições 0 a 6 foram "empurrados" para as posições 1 a 7. O espaço vazio na posição 0 foi preenchido com zero. O sinal resultante sai pelo túnel `SL` para o pino `SHIFTLEFT`.

#### 3. Papel na ALU (Unidade Lógica e Aritmética)

* **Eficiência:** Como mencionado, a maior vantagem dessa montagem é que ela não requer portas lógicas (AND, OR, etc.). É puramente organização de cabos.

* **Aplicações:** É o circuito invocado quando o processador precisa manipular bits específicos para máscaras lógicas, ou processar algoritmos de multiplicação via software (onde se faz *Shift Left* sucessivos somados).