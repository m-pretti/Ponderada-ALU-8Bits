### Documentação de Componente: Deslocador à Direita de 8 Bits (Shift Right)

**Função Principal:** 

Realizar o deslocamento lógico de todos os bits do registrador Acumulador (`AC`) uma posição para a direita. Na prática, os bits "escorregam" para posições menores, o bit menos significativo (LSB) "cai" e é descartado, e um zero é inserido na posição mais significativa (MSB) que ficou vazia. Matematicamente, isso equivale a uma divisão inteira por 2, ignorando o resto.

#### 1. Entradas e Saídas

**Entradas (Inputs):**

* `AC` (Acumulador): Barramento de 8 bits contendo o valor original que será deslocado.
* `Constante 0`: Um pino fixo em `0` lógico para preencher a lacuna superior deixada pelo deslocamento.

**Saídas (Outputs):**

* `SHIFTRIGHT`: Barramento de 8 bits resultante do deslocamento, roteado pelo túnel `SR`.
* `A1` *(Saída implícita)*: Este túnel captura de forma isolada o Bit 0 original (o LSB) que "caiu" do número. Em arquiteturas completas, esse bit não se perde no vácuo; ele costuma ser enviado à *Flag de Carry* para alertar se a divisão por 2 deixou um resto ímpar.

#### 2. Lógica Interna e Funcionamento

* **Passo 1: Desmembramento da Base (Splitter 1):**

    * O barramento `AC` chega ao primeiro *Splitter* configurado como `8 -> 1,7`.
    * O **Bit 0** (LSB) é separado e enviado sozinho para o túnel `A1`.
    * Os **7 bits superiores** (Bits 1 a 7) são aglomerados e enviados para o túnel `A2`.

* **Passo 2: Injeção do MSB:**

    * O componente `Const 0` aguarda para ser costurado na nova posição mais alta do barramento.

* **Passo 3: Reconstrução Rebaixada (Splitter 2):**

    * Um segundo *Splitter* atua como montador, configurado para `7,1 -> 8`.
    * A entrada `0` (que vai preencher as posições de 0 a 6 do novo número) recebe o túnel `A2` (os antigos bits 1 a 7). Eles foram fisicamente "rebaixados" uma casa.
    * A entrada `1` (que vai preencher a posição 7, o novo MSB) recebe a `Constante 0`.

    * **Resultado Final:** O barramento reconstituído sai pelo túnel `SR` pronto para o painel principal.

#### 3. Papel na ALU (Unidade Lógica e Aritmética)

* **Divisão Ultrarrápida:** Assim como o *Shift Left* faz multiplicações instantâneas por 2, o *Shift Right* faz divisões por 2. Se o processador rodar um código que divide um número por 4, ele simplesmente chamará esse circuito duas vezes seguidas, economizando o uso de um divisor complexo.

* **Extração de Bits:** É a ferramenta perfeita para quando o programador precisa ler os bits individuais de um byte (ex: um protocolo de comunicação serial onde você lê um bit por vez, sempre "empurrando" o pacote para a direita).