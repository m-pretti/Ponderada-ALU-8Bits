# PONDERADA-ALU-8BITS: Unidade Lógica e Aritmética de 8 Bits

Este repositório contém a implementação de uma Unidade Lógica e Aritmética (ULA/ALU) de 8 bits desenvolvida no simulador Digital. O projeto integra componentes fundamentais da arquitetura de computadores, permitindo o processamento de operações matemáticas e lógicas complexas através de um sistema modular e um caminho de dados controlado por microinstruções.

## Estrutura do Repositório

O projeto está organizado para facilitar a navegação entre a documentação técnica e os arquivos de implementação lógica:

* **`docs/`**: Contém a documentação técnica específica, diagramas e explicações detalhadas de cada submódulo, como divisor, multiplicador, portas lógicas, registrador, seletor, shift lógico, somador e subtrator.
* **`src/`**: Arquivos fonte (.dig) contendo os circuitos funcionais de cada componente, organizados em pastas correspondentes aos submódulos.
* **`README.md`**: Guia principal do projeto.

## Operações e Funcionalidades

A ALU utiliza um Opcode de 3 bits para selecionar entre as operações disponíveis. Abaixo, detalhamos cada uma delas conforme a estrutura implementada:

### 1. Aritmética Base (Somador e Subtrator)

* **Soma (ADD)**: Realiza a adição binária de dois números de 8 bits (A e B).
* **Subtração (SUB)**: Executa a subtração utilizando a lógica de Complemento de 2, transformando A - B em A + (NOT B) + 1.
* **Flags**: O módulo monitora estados como Cout (Carry), N (Negative) e X (Overflow).

### 2. Divisão de 8 Bits (DIV)

Operação realizada pelo módulo divisor8bits.

* **Funcionamento**: Implementa o algoritmo de Divisão por Restauração através de uma matriz combinacional de 8 estágios.
* **Saídas**: O circuito entrega simultaneamente o Resto (R) na saída RAC e o Quociente (Q) na saída RMQ.

### 3. Multiplicação (MUL)

* **Funcionamento**: Implementa a multiplicação de 8 bits através de somas sucessivas ou matriz de multiplicadores.
* **Registro**: O resultado é armazenado e pode ser visualizado nos registros de saída.

### 4. Deslocamento Lógico (SHIFT)

Gerenciado pelos módulos shift_logico e shiftleftac8bits.

* **Shift Left (SL)**: Desloca os bits para a esquerda (multiplicação por 2), inserindo zero no LSB.
* **Shift Right (SR)**: Desloca os bits para a direita (divisão inteira por 2), inserindo zero no MSB.

### 5. Portas Lógicas (LOGIC)

Realiza operações bit-a-bit fundamentais entre os operandos A e B:

* **AND, OR, XOR e NAND**: Operações essenciais para mascaramento de bits e lógica booleana.

### 6. Sistema de Registradores e Seletor

* **Registrador (AC e MQ)**: Unidades de memória interna para manter resultados de operações anteriores ou operandos persistentes.
* **Seletor (Mux/Demux)**: Gerencia o fluxo de dados, decidindo qual componente terá acesso ao barramento de saída baseado no Opcode.

## Como Simular

1. Abra o simulador Digital (H. Neemann).
2. Carregue o circuito principal localizado em `src/`.
3. **Entradas**: Ajuste os valores nos inputs A e B.
4. **Controle**: Defina o Opcode desejado através do seletor.
5. **Execução**:
    * Para operações combinacionais (Soma, Divisão), o resultado é imediato.
    * Para operações que envolvem o Acumulador, utilize o sinal de Clock.
6. **Saídas**: Observe os displays RAC (Resultado Principal) e RMQ (Resultado Secundário).

# Vídeo de Demonstração

[Vídeo](https://drive.google.com/drive/folders/1JugWxQ_Q3C9oBb86Ny2e79adnWJ5CMp7?usp=sharing)  
