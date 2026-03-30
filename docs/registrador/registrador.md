### Documentação de Componente: Registrador de 8 Bits (Carga Paralela)

**Função Principal:** 

Armazenar um valor de 8 bits (`Din`) de forma estável. O valor só é atualizado quando o sinal de controle `Load` está ativo no momento da borda de subida do `CLOCK`. Se `Load` for 0, o registrador ignora a entrada e mantém o valor atual.

#### 1. Arquitetura Interna

O circuito é composto por 8 "células" idênticas. Cada célula contém:

* **Multiplexador (MUX):** Atua como o "porteiro" do dado.

    * **Entrada 0:** Conectada à própria saída do Flip-Flop (Realimentação).

    * **Entrada 1:** Conectada ao bit correspondente da entrada externa (`D1` a `D8`).

    * **Seletor:** Conectado ao túnel `L` (`Load`).
    
* **Flip-Flop D (D_FF):** O elemento de memória real que captura o estado na borda do clock.

#### 2. Lógica de Operação

* **Modo Hold (Manter):** Quando `Load = 0`, o MUX seleciona a entrada 0. Isso faz com que a saída do Flip-Flop seja enviada de volta para sua própria entrada. A cada pulso de clock, o FF "lê" o que ele já tinha e grava o mesmo valor. O dado fica preservado.

* **Modo Load (Carregar):** Quando `Load = 1`, o MUX seleciona a entrada 1. O dado novo vindo de `Din` flui para a entrada do Flip-Flop. Na próxima borda do clock, este novo valor é armazenado e aparece na saída `Q`.

#### 3. Organização de Dados (Splitters e Túnis)

Você usou uma estratégia muito organizada para lidar com os fios:

* **Entrada:** O sinal `Din` (8 bits) passa por um *Splitter* que o separa em 8 túnis individuais (`D1` a `D8`), enviando cada bit para sua respectiva célula.
* **Saída:** As saídas de cada Flip-Flop (`Q1` a `Q8`) são reunidas por outro *Splitter* para formar o barramento de saída único `Dout` de 8 bits.


### Por que isso é importante para o seu Multiplicador?

No multiplicador que vimos antes, você usou dois destes: o **AC** e o **MQ**.

1.  O sinal **`G`** (Gate) do seu multiplicador é o que se conecta ao pino **`Load`** deste registrador.

2.  Sem essa lógica de MUX + Realimentação, os registradores gravariam qualquer lixo que estivesse no barramento a cada batida do clock. 

3.  Com essa estrutura, o resultado da multiplicação só entra no AC e MQ quando o circuito de controle decide que a conta está pronta.