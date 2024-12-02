# Atendimento do Professor

## Terças e quintas. Professor de horário parcial. Nesses 2 dias, podem contar comigo!

# Semama 08 - Sistema de disparo de atuadores

## Impactos no seu Projeto

* Usar o botões minimalistas e multifunções;
* Usar botões touch ao invés de push bottons;
* Entender a importância do UML;
* Entendimento do sono profundo do ESP32.

# Problema:

#### Seu projeto pode ser conectado numa bateria ao invés da energia de uma tomada? Como fazer isso?

#### O cliente deseja conhecer o fluxo de comunicação durante o funcionamento do seu projeto. Como explicar de forma lúdica? UML!

---
# Usando Botões Touch

Você sabia que o ESP32 possui pinos GPIO (General Propouse Input Output) que atuam como botões **touch**?

E você pode assim substituir os seus botões da caixinha e colocar um papel alumínio de fora ou uma chapinha de refrigerante recortada. Passe Bombril para remover a tinta da lata;

Os 10 pinos capacitivos que detectam o toque no ESP32 são:

1) T0 (GPIO4)
2) T1 (GPIO0)
3) T2 (GPIO2)
4) T3 (GPIO15)
5) T4 (GPIO13)
6) T5 (GPIO12)
7) T6 (GPIO14)
8) T7 (GPIO27)
9) T8 (GPIO33)
10) T9 (GPIO32)

Você só precisa usar o comando **touchRead(TOUCH_PIN)** para ler o sensibilidade do toque. Valor de **touchRead** próximo de 10 estão detectando o toque. Próximos de 80, estão sem toque.

## Exemplo de Código:

```
#define TOUCH_PIN T8  // GPIO33

int threshold = 20;      // Limiar para detecção de toque
unsigned long previousMillis = 0; // Armazena o último tempo em que o touch foi lido
const long interval = 100; // Intervalo em milissegundos para leituras

void setup() {
  Serial.begin(115200);
  Serial.println("Inicializando Touch no ESP32...");
}

void loop() {
  // Verifica o tempo atual
  unsigned long currentMillis = millis();

  // Se o intervalo tiver passado, faz a leitura do sensor
  if (currentMillis - previousMillis >= interval) {
    previousMillis = currentMillis; // Atualiza o último tempo de execução

    // Ler o valor do sensor capacitivo
    int touchValue = touchRead(TOUCH_PIN);

    // Imprimir o valor no monitor serial
    Serial.print("Touch Value: ");
    Serial.println(touchValue);

    // Verificar se o valor está abaixo do limiar
    if (touchValue < threshold) {
      Serial.println("Toque detectado!");
    }
  }
}

```

---
# Fazendo seu ESP32 Dormir

O ESP32 pode dormir dormir teoricamente por 136 anos, porque seu relógio interno pode contar até 2^64 microsegundos.

Claro que não poderemos testar isso, porque uma bateria não duraria todo esse tempo.

Esse código exemplo faz um pisca-pisca dormir por 10s.

Note que você nunca usará o **void loop()** novamente se usar o recurso do sono profundo, porque o ESP32 se reinicia todas as vezes que ele acorda. Portanto, seu código do projeto deverá estar todo dentro do **void setup()**.

No modo **sono profundo**, as varáveis da RAM são reiniciada a cada vez que o ESP32 acordar. Portanto, prepare seu código para "perder" dados no modo sono profundo.

No modo profundo, o ESP32 consome 10uA (microAmpères). **Isso é 2000x menor que a corrente de um LED vermelho**.


## Código 1: Faz um LED acender por 3s e hibernar por 7s

```
#define LED_PIN 2 // GPIO onde o LED está conectado

void setup() {
  Serial.begin(115200);

  // Configuração do LED como saída
  pinMode(LED_PIN, OUTPUT);

  // Acender o LED para indicar atividade
  digitalWrite(LED_PIN, HIGH);
  Serial.println("ESP32 despertou! LED ligado por 3 segundos.");

  // Variáveis para controlar o tempo usando millis()
  unsigned long startMillis = millis(); // Registrar o momento inicial
  unsigned long currentMillis;

  // Manter o LED aceso por 3 segundos
  while (true) {
    currentMillis = millis();
    if (currentMillis - startMillis >= 3000) { // 3 segundos (3000 ms)
      break; // Sai do loop após 3 segundos
    }
  }

  // Apagar o LED
  digitalWrite(LED_PIN, LOW);
  Serial.println("LED desligado. Indo para sono profundo...");

  // Configurar sono profundo por 7 segundos
  esp_sleep_enable_timer_wakeup(7 * 1000000); // 7 segundos em microsegundos
  esp_deep_sleep_start();
}

void loop() {
  // Nunca será utilizado no modo sono profundo.
}

```

---
# Entendo o UML



### Problemas no Diagrama
1. **Ausência de visibilidade dos atributos e métodos** (`+`, `-`, `#`).
2. **Relações incorretas ou faltantes** (ex.: agregação, composição, herança).
3. **Nomes genéricos ou ambíguos** para classes e métodos.
4. **Faltam associações claras entre as classes**.
5. **Inconsistência no uso de tipos de dados dos atributos**.

---

**Descrição Textual do Sistema**
- O sistema gerencia um e-commerce com produtos, clientes e pedidos.
- Cada pedido está associado a um cliente e pode conter múltiplos produtos.
- Os clientes têm informações como nome e e-mail.
- Os produtos possuem nome, preço e estoque.
- O pedido registra a data do pedido e o total a pagar.

---

**Diagrama UML Incompleto**
```
+-----------------+              +----------------+
|     Cliente     |              |    Produto     |
|-----------------|              |----------------|
| nome            |              | nome           |
| e-mail          |              | preço          |
|-----------------|              |----------------|
|                 |              |                |
+-----------------+              +----------------+
         |                                |
         |                                |
         +--------------------------------+
                     |
                     |
             +---------------+
             |    Pedido     |
             |---------------|
             | data          |
             | total         |
             |---------------|
             |               |
             +---------------+
```

---

### Problemas para os Alunos Corrigirem
1. Adicionar visibilidade (`+`, `-`) aos atributos.
2. Definir os tipos de dados dos atributos (ex.: `String`, `float`, `Date`).
3. Inserir métodos nas classes apropriadas (ex.: `adicionarProduto()`, `calcularTotal()`).
4. Corrigir as relações:
   - O `Cliente` deve ter uma **associação 1 para muitos** com `Pedido`.
   - O `Pedido` deve ter uma **associação muitos para muitos** com `Produto` (com uma classe intermediária, se necessário).
5. Adicionar o nome das relações nos conectores (ex.: "realiza", "contém").
6. Identificar possíveis agregações ou composições (ex.: `Pedido` e `Produto`).

---

**Atividade**
- "Corrija o diagrama acima com base na descrição textual fornecida, garantindo que todos os problemas sejam resolvidos e que o diagrama UML esteja completo e correto." 

Após os ajustes, peça que os alunos expliquem como corrigiram os erros e o impacto das mudanças no design do sistema.





Segue a solução corrigida do **diagrama de classes UML**, com as correções necessárias aplicadas.

---

### **Diagrama UML Corrigido**
```
+-----------------+                +----------------+
|     Cliente     |                |    Produto     |
|-----------------|                |----------------|
| - nome: String  |                | - nome: String |
| - email: String |                | - preco: float |
|-----------------|                | - estoque: int |
| + getNome()     |                |----------------|
| + getEmail()    |                | + atualizarEstoque(qtd: int) |
|-----------------|                +----------------+
         |                                  |
         | 1                              * |
         |                                  |
         +----------------------------------+
                      realiza
                      1..*  
         +-----------------+
         |     Pedido      |
         |-----------------|
         | - data: Date    |
         | - total: float  |
         |-----------------|
         | + calcularTotal()|
         | + adicionarProduto(p: Produto) |
         | + getData()                    |
         |-----------------|
                  |
                  | *
                  |
     +----------------------+
     |    ItemPedido        |
     |----------------------|
     | - quantidade: int    |
     |----------------------|
     | + getSubtotal():float|
     +----------------------+
             *
             |
             |
        +---------+
        | Produto |
        +---------+
```

---

### **Explicação das Correções**

1. **Visibilidade dos Atributos e Métodos**
   - Todos os atributos agora possuem visibilidade: `-` para privado e `+` para público.
   - Métodos públicos foram adicionados com base nas necessidades do sistema (ex.: `getNome`, `calcularTotal`).

2. **Definição de Tipos de Dados**
   - Foram especificados os tipos de dados para cada atributo (ex.: `String` para nome, `float` para preço e total, `Date` para a data do pedido).

3. **Relações entre Classes**
   - Adicionada uma relação **1 para muitos** entre `Cliente` e `Pedido` (um cliente pode fazer vários pedidos).
   - Adicionada uma relação **muitos para muitos** entre `Pedido` e `Produto` usando uma classe intermediária chamada `ItemPedido`.
     - `ItemPedido` inclui o atributo `quantidade` para indicar quantas unidades do produto estão associadas ao pedido.
     - Esta classe resolve a necessidade de múltiplas instâncias do mesmo produto em um único pedido.

4. **Agregação**
   - A classe `ItemPedido` está agregada à classe `Pedido` e à classe `Produto`.

5. **Métodos Úteis**
   - `Pedido` recebeu métodos como `calcularTotal` (que calcula o valor total do pedido) e `adicionarProduto`.
   - `Produto` recebeu o método `atualizarEstoque` para gerenciar a quantidade disponível.

6. **Relações Nomeadas**
   - Foi adicionado o nome da relação "realiza" entre `Cliente` e `Pedido` para descrever a interação.


# Dinâmica em grupo

Seu grupo foi encarregado de evoluir o projeto para ser energizado por uma bateria recarregável de 9V
