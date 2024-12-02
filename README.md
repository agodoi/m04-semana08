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

Os pinos são:

* T0 (GPIO4)
* T1 (GPIO0)
* T2 (GPIO2)
* T3 (GPIO15)
* T4 (GPIO13)
* T5 (GPIO12)
* T6 (GPIO14)
* T7 (GPIO27)
* T8 (GPIO33)
* T9 (GPIO32)

---
# Fazendo seu ESP32 Dormir


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
