### Documentação de Componente: Multiplicador de 8 Bits com Registradores AC/MQ

**Função Principal:** 

Receber dois operandos de 8 bits, realizar a multiplicação combinacional através de um submódulo e, ao receber o sinal de Clock, armazenar o resultado de 16 bits dividido em dois registradores de 8 bits clássicos da arquitetura de computadores: o Acumulador (`AC`) e o *Multiplier Quotient* (`MQ`).

#### 1. Entradas e Saídas

**Entradas (Inputs):**

* `A` (Multiplicando): Barramento de 8 bits.
* `B` (Multiplicador): Barramento de 8 bits.
* `CLK` (Clock): Sinal de relógio que sincroniza a gravação do resultado.
* `G` (Gate/Enable): Sinal de controle (1 bit). Quando ativo, permite que os registradores gravem o novo resultado no pulso do clock.

**Saídas (Outputs):**

* `AC` (Acumulador): Barramento de 8 bits contendo a **parte alta** (High Byte) do resultado da multiplicação.
* `MQ` (Quociente Multiplicador): Barramento de 8 bits contendo a **parte baixa** (Low Byte) do resultado.

#### 2. Lógica Interna e Funcionamento

* **O "Cérebro" Combinacional (`multiplicador8bits-v2.dig`):** Os barramentos `A` e `B` entram diretamente neste submódulo. Assim como no seu multiplicador de 4 bits, este bloco resolve a matemática pura instantaneamente.

* **A Matemática dos Bits:** Quando você multiplica dois números de 8 bits (ex: $255 \times 255$), o resultado máximo precisa de 16 bits para ser representado (ex: $65025$). Portanto, o túnel de saída `R` carrega 16 bits.

* **O Separador (Splitter):** Como a sua arquitetura (barramentos e registradores) é projetada para 8 bits, você não pode transitar um número de 16 bits inteiro. O Splitter pega o barramento `R` de 16 bits e o "fatia" ao meio:

    * `R1` (Bits 0 a 7): A metade menos significativa do número.
    * `R2` (Bits 8 a 15): A metade mais significativa do número.

* **A Lógica Sequencial (Registradores):** O valor calculado precisa ser guardado para o próximo ciclo do processador.

    * O componente `registrador8bits.dig` (superior) recebe `R2` e é rotulado como **AC**.
    * O componente `registrador8bits.dig` (inferior) recebe `R1` e é rotulado como **MQ**.
    * Ambos os registradores só "engolem" esses dados quando o sinal `G` (Enable) estiver ativado e ocorrer a borda do Clock (`CLK`).

#### 3. Papel na Arquitetura

Dividir a resposta de uma multiplicação em registradores altos e baixos é um padrão da indústria (processadores x86, por exemplo, usam os registradores `AX` e `DX` em conjunto para multiplicar números de 16 bits gerando respostas de 32 bits).

Isso garante que o seu processador não perca dados (overflow) ao fazer contas grandes. O programador da sua CPU saberá que, após uma instrução de `MUL`, ele deve olhar para o MQ para respostas pequenas, e verificar o AC caso a multiplicação tenha gerado um número maior que 255.