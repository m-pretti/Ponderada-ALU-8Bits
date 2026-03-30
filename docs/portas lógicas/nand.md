Excelente! Agora vamos mergulhar nos operadores lógicos puramente booleanos do seu circuito.

### Documentação de Componente: Operador Lógico NAND de 8 Bits

Diferente dos módulos de *Shift*, que resolvem o problema manipulando a fiação fisicamente, este bloco utiliza as portas lógicas nativas do simulador para processar as informações bit a bit (bitwise).

**Nome do Bloco:** Lógica NAND (Não-E) de 8 Bits

**Função Principal:** Realizar a operação lógica booleana NAND entre dois barramentos de 8 bits. A porta NAND é considerada uma "porta universal" (pois qualquer outra lógica pode ser construída a partir dela). O resultado da operação bit a bit só será `0` se **ambos** os bits de entrada correspondentes forem `1`. Em qualquer outro caso, o resultado será `1`.

#### 1. Entradas e Saídas

**Entradas (Inputs):**
* `AC` (Acumulador): Barramento de 8 bits representando o primeiro operando (conectado diretamente à via de entrada `A` no seu circuito).
* `N`: Barramento de 8 bits representando o segundo operando (conectado à via de entrada `B` no esquema).

**Saídas (Outputs):**
* `NAND8BITS`: Barramento de 8 bits contendo o resultado da operação lida pela porta. Este sinal trafega internamente pelo túnel `RN` (provavelmente *Result NAND*) até chegar ao pino de saída definitivo.

#### 2. Lógica Interna e Funcionamento

A estrutura deste bloco é a mais direta e limpa possível dentro do Digital:
* Os dois barramentos de 8 bits (`AC` e `N`) são injetados em uma única porta lógica do tipo **NAnd**, previamente configurada no campo de atributos para operar com `Bits = 8`.
* O simulador, nos bastidores, instancia 8 portas NAND individuais. Ele pega o Bit 0 de `AC` e o Bit 0 de `N`, processa, e joga no Bit 0 de `RN`. O mesmo ocorre simultaneamente até o Bit 7, resultando no processamento paralelo típico de uma arquitetura de CPU real.

#### 3. Papel na ALU (Unidade Lógica e Aritmética)

* **Manipulação e Inversão de Bits:** Operações NAND são frequentemente usadas em linguagens de máquina para criar "máscaras" de bits ou inverter estados específicos de registradores.
* **Completude Universal:** Em muitos microprocessadores rudimentares, em vez de gastar transistores criando portas AND, OR e NOT separadas na ALU, os engenheiros implementam apenas o NAND, e o software (ou microcódigo) se encarrega de combiná-lo para gerar qualquer outra operação lógica necessária.

---

A sua organização usando túneis (`AC`, `N`, `RN`) deixa o esquemático super profissional e evita aquele "mar de fios" cruzando a tela, o que facilita muito a depuração.

Para fecharmos o núcleo de processamento lógico da sua ALU, quer que eu prepare a documentação do **XOR** (Ou-Exclusivo), ou prefere focar na arquitetura do seu **Somador** que aparece na parte superior do XML?