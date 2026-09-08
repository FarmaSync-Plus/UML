# 💊 FarmaSync Plus — Documentação e Modelagem UML

Repositório oficial da documentação arquitetural e de engenharia de software do **FarmaSync Plus**, sistema voltado para controle farmacêutico, conformidade regulatória (ANVISA/SNGPC) e vendas presenciais e online.

---

## 📌 Sumário
1. [Visão Geral da Arquitetura](#-visão-geral-da-arquitetura)
2. [Diagrama de Classes](#-diagrama-de-classes)
3. [Dicionário de Entidades](#-dicionário-de-entidades)
   - [Núcleo de Vendas e Balcão](#1-núcleo-de-vendas-e-balcão-pdv)
   - [Núcleo de Produtos e Medicamentos](#2-núcleo-de-produtos-e-medicamentos)
   - [Cadeia de Suprimentos e Rastreabilidade](#3-cadeia-de-suprimentos-e-rastreabilidade)
   - [E-commerce e Usuário](#4-e-commerce-e-usuário)
4. [Enumerações](#-enumerações-enumeration)
5. [Mapeamento de Relacionamentos](#-mapeamento-de-relacionamentos)

---

## 🏛️ Visão Geral da Arquitetura

A modelagem do **FarmaSync Plus** adota os fundamentos da Orientação a Objetos (POO):
* **Herança / Especialização (`extends`):** Centralização de atributos gerais na superclasse `Produto` e desdobramentos para `Medicamento`, `HigienePessoal` e `Cosmetico`.
* **Rastreabilidade Regulatória:** Vínculo obrigatório entre fármacos controlados (`MedicamentoControlado`), receituários médicos (`Receita`) e controle de lotes (`Lote`).
* **Segregação de Operações:** Suporte a vendas diretas em balcão (`Venda` e `ItemVenda`) e operações digitais de e-commerce (`Pedidos` e `Usuário`).

---

## 📐 Diagrama de Classes

<details>
  <img width="968" height="887" alt="image" src="https://github.com/user-attachments/assets/8ef57457-be27-42d6-9a8e-0ccd6783fd33" />
</details>

---

## 📚 Dicionário de Entidades

### 1. Núcleo de Vendas e Balcão (PDV)

<details>
<summary><b>Visualizar classes de Venda, ItemVenda e Receita</b></summary>

#### `Venda`
Representa a transação comercial finalizada no ponto de venda.
* `- id: int`: Identificador único da transação.
* `- dataHora: Date`: Registro de data e horário do faturamento.
* `- valorTotal: Decimal`: Valor financeiro total acumulado.
* `- formaPagamento: String`: Método utilizado (PIX, Dinheiro, Cartão).

#### `ItemVenda`
Item específico de uma transação, armazenando o valor histórico praticado.
* `- quantidade: int`: Unidades compradas.
* `- precoUnitario: Decimal`: Valor unitário no momento da venda.
* `- subtotal: Decimal`: Subtotal apurado (`quantidade * precoUnitario`).

#### `Receita`
Registro do receituário médico retido ou apresentado.
* `- id: int`: Chave do registro da receita.
* `- numero: String`: Código impresso na prescrição.
* `- dataEmissao: Date`: Data em que a receita foi expedida.
* `- medico: String`: Nome do médico responsável.
* `- crm: String`: Registro no conselho de classe.

</details>

---

### 2. Núcleo de Produtos e Medicamentos

<details>
<summary><b>Visualizar superclasse Produto e especializações</b></summary>

#### `Produto` *(Superclasse)*
* **Atributos:**
  * `- id: int`, `- nome: String`, `- codigoBarras: String`, `- descricao: String`
  * `- precoVenda: Decimal`, `- quantidadeEstoque: int`, `- validade: Date`, `- fabricante: String`
* **Métodos:**
  * `+ cadastrar(): void`
  * `+ atualizar(): void`
  * `+ consultarEstoque(): void`
  * `+ verificarValidade(): void`

#### `Medicamento` *(Herda de Produto)*
* **Atributos:**
  * `- principioAtivo: String`, `- dosagem: String`, `- formaFarmaceutica: String`, `- necessitaReceita: Boolean`
* **Métodos:**
  * `+ validarReceita(): void`

#### `MedicamentoControlado` *(Herda de Medicamento)*
* **Atributos:**
  * `- numeroRegistroControlado: String`, `- tipoControle: String`
* **Métodos:**
  * `+ registrarVendaControlada(): void`

#### `MedicamentoGenerico` *(Herda de Medicamento)*
* **Atributos:**
  * `- medicamentoReferencia: String`
* **Métodos:**
  * `+ informarEquivalencia(): void`

#### `HigienePessoal` & `Cosmetico` *(Herdam de Produto)*
* **HigienePessoal:** `- categoria: String`, `- marca: String`, `- publicoAlvo: String`
* **Cosmetico:** `- categoria: String`, `- marca: String`, `- tipoPele: String`

</details>

---

### 3. Cadeia de Suprimentos e Rastreabilidade

<details>
<summary><b>Visualizar Fornecedor e Lote</b></summary>

#### `Fornecedor`
* `- id: int`: Chave primária.
* `- razaoSocial: String`: Razão social cadastrada.
* `- cnpj: String`: Cadastro Nacional da Pessoa Jurídica.
* `- telefone: String` | `- email: String`: Canais de contato.

#### `Lote`
* `- id: int`: Identificador único do lote.
* `- numero: String`: Código do lote para fins de rastreio/recall.
* `- dataFabricacao: Date` | `- dataValidade: Date`: Datas de controle industrial.
* `- quantidade: int`: Unidades vinculadas ao lote.

</details>

---

### 4. E-commerce e Usuário

<details>
<summary><b>Visualizar Usuário e Pedidos</b></summary>

#### `Usuário`
Gerencia autenticação e perfil cadastral do cliente/operador.
* **Atributos:** `- usuario_id: int`, `- nome_usuario: String`, `- CPF: String`, `- logradouro: String`, `- bairro: String`, `- Cidade: String`, `- cep: String`, `- email_usuario: String`, `- senha_usuario: String`, `- data_de_nasc: LocalDate`
* **Métodos principais:**
  * `+ fazerLogin(...)`, `+ criarConta(...)`, `+ verConta()`, `+ atualizarConta()`, `+ removerConta()`
  * `+ procurarProduto()`, `+ adicionarProduto()`, `+ removerProduto()`, `+ realizarPagamento()`, `+ cancelarCompra()`

#### `Pedidos`
Gerencia ordens originadas na plataforma web/app.
* **Atributos:** `- pedidos_id: int`, `- usuario_id: int`, `- id_produto: int`, `- nome: String`, `- peso: decimal`, `- pagamento: String`, `- valor_total: decimal`, `- quantidade_pedido: int`, `- data_pedido: DateTime`, `- pedido_atualizacao: DateTime`
* **Métodos:** `+ criarPedido()`, `+ atualizarPedido()`, `+ cancelarPedido()`, `+ calcularValorTotal()`, `+ alterarQuantidade()`, `+ consultarPedido()`

</details>

---

## 🏷️ Enumerações (`<<enumeration>>`)

Padronizações aplicadas para manter a integridade dos dados cadastrais:

| Enumeração | Valores Aceitos | Aplicação |
| :--- | :--- | :--- |
| **`Estado`** | Todas as 27 UFs brasileiras (`Acre (AC)` até `Distrito Federal (DF)`) | Padronização fiscal e cálculo logístico de entrega. |
| **`Genero`** | `Cisgênero`, `Transgênero`, `Não-binário`, `Agênero`, `Gênero-fluido`, `Biogênero` | Perfil inclusivo e relatórios demográficos. |

---

## 🔗 Mapeamento de Relacionamentos

| Origem | Tipo de Relação | Cardinalidade | Destino | Regra de Negócio |
| :--- | :---: | :---: | :--- | :--- |
| `Venda` | Composição | `1` $\rightarrow$ `1..*` | `ItemVenda` | Uma venda não existe sem ao menos um item faturado. |
| `ItemVenda` | Associação | `0..*` $\rightarrow$ `1` | `Produto` | Cada item vendido aponta para um produto de referência. |
| `Venda` | Associação | `0..1` $\rightarrow$ `1` | `Receita` | Venda vincula opcionalmente uma receita médica regularizada. |
| `Receita` | Associação | `1` $\rightarrow$ `1..*` | `Medicamento` | A prescrição médica autoriza um ou mais medicamentos. |
| `Fornecedor` | Associação | `1` $\rightarrow$ `0..*` | `Produto` | Um fornecedor homologado pode fornecer múltiplos produtos. |
| `Produto` | Composição | `1` $\rightarrow$ `0..*` | `Lote` | Um produto tem seu estoque subdividido em vários lotes. |
| `Usuário` | Associação | `1` $\rightarrow$ `0..*` | `Pedidos` | Um cliente autenticado pode registrar múltiplos pedidos. |
| `Medicamento` | Herança | $\triangle$ | `Produto` | Especialização com regras sanitárias e posologia. |
| `HigienePessoal` | Herança | $\triangle$ | `Produto` | Especialização para itens de perfumaria e higiene. |
| `Cosmetico` | Herança | $\triangle$ | `Produto` | Especialização para dermocosméticos e cuidados pessoais. |
| `MedicamentoControlado`| Herança | $\triangle$ | `Medicamento` | Especialização para fármacos retidos com notificação da ANVISA. |
| `MedicamentoGenerico` | Herança | $\triangle$ | `Medicamento` | Especialização indicando produto intercambiável. |
