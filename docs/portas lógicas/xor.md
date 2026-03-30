### Documentação de Componente: Operador Lógico XOR de 8 Bits

**Função Principal:** 

Realizar a operação lógica booleana XOR (Exclusive-OR) de forma paralela (bit a bit) entre dois barramentos. O XOR atua como um "detector de diferenças": o resultado será `1` se, e somente se, os bits de entrada comparados forem **diferentes** entre si (um for `0` e o outro `1`). Se os bits forem iguais (ambos `0` ou ambos `1`), o resultado será `0`.

#### 1. Entradas e Saídas

**Entradas (Inputs):**

* `AC` (Acumulador): Barramento de 8 bits representando o primeiro operando.
* `N`: Barramento de 8 bits representando o segundo operando.

**Saídas (Outputs):**

* `XOR8BITS`: Barramento de 8 bits com o resultado final da operação, trafegando internamente através do túnel `RX` (provavelmente *Result XOR*) até o terminal de saída.

#### 2. Lógica Interna e Funcionamento

Assim como no componente NAND, você aproveitou a robustez do simulador Digital para simplificar o esquemático:

* Os sinais `AC` e `N` alimentam diretamente uma porta lógica **XOr** configurada nativamente para `Bits = 8`.
* O processamento ocorre em paralelo: o Bit 0 de `AC` é comparado com o Bit 0 de `N`, gerando o Bit 0 de `RX`, e assim por diante até o Bit 7, sem atrasos complexos de propagação que ocorreriam se você montasse o XOR usando portas AND/OR/NOT discretas.

#### 3. Papel na ALU (Unidade Lógica e Aritmética)

O XOR é, sem dúvida, uma das ferramentas mais poderosas no repertório de um processador por conta de seus "truques" matemáticos:

* **Zerar Registradores Rápido:** Em Assembly, a forma mais rápida de definir um registrador como zero não é movendo o número `0` para ele, mas sim fazendo um XOR dele com ele mesmo (ex: `A XOR A = 0`).

* **Inversão Controlada (Toggle):** Se você faz um XOR de um número com um barramento cheio de `1`s, ele inverte todos os bits (atua como uma porta NOT). Se faz XOR com `0`s, ele mantém os bits originais.

* **Verificação de Igualdade:** Se você precisa saber se duas variáveis são idênticas, basta passá-las pelo XOR. Se o resultado for puramente `0`, significa que todos os bits eram perfeitamente iguais.

* **Criptografia Básica:** É a base para várias cifras simétricas de baixo nível.