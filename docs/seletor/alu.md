### Análise Estrutural da ALU

#### 1. Seleção de Operação (Opcode)

Você utiliza um **Opcode de 3 bits**, o que permite até 8 operações diferentes.

* **Decodificação:** O `Demultiplexer` (posicionado em `500,720`) funciona como um decodificador de instrução simples. Ele ativa uma das linhas `D0` a `D7` com base no valor de `Op`.

* **Controle de Fluxo:** As saídas da ALU são filtradas por dois grandes **Multiplexadores de 8 bits** (posicionados em `1300,80` e `1300,400`). Eles selecionam qual o resultado final que aparecerá nas saídas `RAC` e `RMQ`.

#### 2. Os Módulos de Processamento

O circuito integra os componentes que analisamos anteriormente:

* **`shiftleftac8bits.dig`:** Realiza deslocamentos (Shifts). Recebe `A` e `B` e gera `S`, `Cout`, `N`, `X`, `SR`, `SL`.

* **`registradores8bits.dig`:** Gerencia o armazenamento e transferência. Gera os sinais `AC` (Acumulador) e `MQ` (Multiplicador/Quociente).

* **`divisor8bits.dig`:** O divisor que acabamos de ver. Ele processa `A` e `B` para gerar `R` (Resto) e `Q` (Quociente).

#### 3. Sinais de Saída (Registradores de Interface)

* **RAC (Result Accumulator):** Geralmente mostra o resultado principal (Soma, Resto ou AC).

* **RMQ (Result Multiplier/Quotient):** Mostra o resultado secundário (como o Quociente da divisão ou o MQ).

* **Nota de Formatação:** Você configurou as saídas para `intFormat: dec`, o que é ótimo para testar o circuito e ler os resultados em decimal imediatamente.

### Tabela de Operações

Com base nas conexões dos MUXs de saída, aqui está como o seu "computador" se comporta:

| Opcode (Op) | RAC (Saída Superior) | RMQ (Saída Inferior) | Descrição Sugerida |
| :--- | :--- | :--- | :--- |
| **000 (0)** | S (Soma/Shift) | 0 | Operação Aritmética Simples |
| **001 (1)** | S | 0 | Variação de Shift/Aritmética |
| **010 (2)** | AC | MQ | Leitura de Registradores Internos |
| **011 (3)** | R (Resto) | Q (Quociente) | **Divisão Completa** |
| **100 (4)** | SR (Shift Right) | 0 | Deslocamento para Direita |

### Dicas para o "Stress Test" do Circuito

1.  **Sincronismo:** Note que o túnis `CLK` está conectado ao módulo de registradores. Isso significa que as operações que envolvem `AC` e `MQ` só serão atualizadas na transição do clock.

2.  **Flags:** Você tem túnis para `Cout` (Carry), `N` (Negative) e `X` (Overflow). Seria interessante puxar esses sinais para `Outs` de 1 bit para que você possa ver se uma conta "estourou" o limite de 8 bits.