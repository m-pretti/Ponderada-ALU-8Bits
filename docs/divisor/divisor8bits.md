### Documentação: Divisor 8 bits

**Função:** 

Realiza a divisão inteira de um **Dividendo** (8 bits) por um **Divisor** (8 bits), entregando o **Quociente** e o **Resto**.

### 1. A Lógica da Cascata (Shift-and-Subtract)

O circuito funciona replicando o processo de divisão longa que aprendemos na escola, mas em hardware:

* **O Splitter de Entrada:** 

Você pega o `Dividendo` e o separa em bits individuais (`D1` a `D8`). Isso é fundamental porque a divisão processa um bit por vez, do mais significativo (MSB) para o menos significativo (LSB).

* **Alinhamento de Bits:** 

Note como você usa os *Splitters* no meio do caminho (como o que gera `A1`, `A2`, etc.). Eles estão fazendo o papel de **Shift Left** (deslocamento para a esquerda). A cada estágio, você "abaixa" um novo bit do dividendo e o anexa ao resto do estágio anterior para tentar uma nova subtração.

* **Os 8 Estágios (`divisor.dig`):** 

Cada bloco desse decide um bit do quociente.

    * Se a subtração for possível (resultado positivo), o bit do quociente é `1` e o resto subtraído passa para baixo.
    * Se não for possível, o bit é `0` e o valor original "passa reto" para a próxima tentativa.

### 2. Mapeamento de Túneis

A organização dos túnis revela o fluxo de dados:

* **`d`:** O divisor original. Ele é distribuído para todos os 8 blocos simultaneamente (é o valor que tentamos subtrair em cada passo).

* **`R0` até `R7`:** São os restos intermediários de cada etapa.

* **`r1`, `r3`, `r5`...:** São as partes fracionárias sendo montadas para a próxima tentativa de subtração.

### 3. Resultados Finais

1. **Quociente (Q):** Formado pela união de todos os sinais de saída `Q` de cada um dos 8 blocos `divisor.dig`.

2. **Resto (Remainder):** O valor do túnel `R7` (a saída do último estágio) é o resto final da divisão.