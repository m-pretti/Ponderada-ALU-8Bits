### Documentação de Componente: Multiplicador de 4 Bits (Arranjo Combinacional)

**Função Principal:** 

Multiplicar dois valores binários de 4 bits (`A` e `B`) para gerar um produto de 8 bits. Ao contrário de processadores muito antigos que faziam multiplicação usando *loops* de soma em software (o que levava dezenas de ciclos de clock), este circuito resolve a multiplicação fisicamente, propagando os sinais em paralelo. Isso é conhecido na engenharia como um **Multiplicador em Arranjo (Array Multiplier)**.

#### 1. Entradas e Saídas

**Entradas (Inputs):**

* `A` (Multiplicando): Barramento de 4 bits.
* `B` (Multiplicador): Barramento de 4 bits.

**Saídas (Outputs):**

* A saída de um multiplicador de dois números de 4 bits exige um barramento de **8 bits** (geralmente coletando os fios `R1`, `R2`, `R3` e os bits restantes da última soma) para evitar *overflow*.

#### 2. Lógica Interna e Funcionamento

O design que você montou imita exatamente a forma como aprendemos a multiplicar no papel na escola primária, mas usando eletrônica! Ele se divide em duas grandes fases:

**Fase 1: Geração dos Produtos Parciais (PP)**

Quando multiplicamos no papel, multiplicamos o número de cima por cada dígito do número de baixo. Em binário, multiplicar por `0` resulta em `0`, e multiplicar por `1` resulta no próprio número. O seu circuito faz isso de forma brilhante:

1. Ele "quebra" a entrada `B` em 4 bits individuais usando um *Splitter* (`B1`, `B2`, `B3`, `B4`).

2. Ele duplica cada um desses bits 4 vezes para criar "máscaras" (`BR1`, `BR2`, etc.).

3. Ele usa portas **AND** de 4 bits para cruzar a entrada `A` com essas máscaras. Se o bit de B for `1`, a porta AND deixa `A` passar (criando o Produto Parcial `PP`). Se for `0`, o Produto Parcial zera.

4. O resultado são os barramentos `PP1`, `PP2`, `PP3` e `PP4`.

**Fase 2: Deslocamento e Soma (Shift & Add)**

Na escola, a cada nova linha da multiplicação, nós "pulamos uma casa" para a esquerda, certo?

1. O seu circuito pega o `PP1` e manda o bit menos significativo (LSB) direto para a saída como `R1` (Resultado 1).

2. Os 3 bits restantes do `PP1` são "empurrados" para baixo e um `0` é inserido no topo, simulando o salto de casa. Isso forma o barramento `P1`.

3. Aí entra o seu componente **`somador4bits.dig`**! Ele soma esse `P1` com a próxima linha da multiplicação, o `PP2`, gerando uma soma `S1`.

4. O LSB do `S1` vira o `R2` (Resultado 2), e o ciclo de quebrar, "empurrar", juntar com o *Carry Out* (vai-um) e somar com o próximo Produto Parcial (`PP3`, depois `PP4`) se repete até extrairmos todos os 8 bits da resposta.

#### 3. Papel na ALU (Unidade Lógica e Aritmética)

A presença deste bloco na sua ALU é um salto de desempenho enorme.

* **Custo vs. Benefício:** Multiplicadores combinacionais (como este) gastam muito silício (várias portas AND e vários Somadores em cascata), mas a velocidade de execução justifica o custo. O processador não precisa gastar ciclos de clock fazendo somas repetidas; o resultado flui quase instantaneamente.

* **Expansão Futura:** Se um dia você quiser transformar isso em um multiplicador de 8 bits, a lógica será exatamente a mesma, apenas com uma escada maior de Produtos Parciais e Somadores.