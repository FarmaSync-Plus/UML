# Diagrama UML — Produtos de uma Farmácia


```mermaid
classDiagram
    class Produto {
        -int id
        -String nome
        -String codigoBarras
        -String descricao
        -Decimal precoVenda
        -int quantidadeEstoque
        -Date validade
        -String fabricante
        +cadastrar()
        +atualizar()
        +consultarEstoque()
        +verificarValidade()
    }

    class Medicamento {
        -String principioAtivo
        -String dosagem
        -String formaFarmaceutica
        -Boolean necessitaReceita
        +validarReceita()
    }

    class MedicamentoControlado {
        -String numeroRegistroControlado
        -String tipoControle
        +registrarVendaControlada()
    }

    class MedicamentoGenerico {
        -String medicamentoReferencia
        +informarEquivalencia()
    }

    class HigienePessoal {
        -String categoria
        -String marca
        -String publicoAlvo
    }

    class Cosmetico {
        -String categoria
        -String marca
        -String tipoPele
    }

    class Fornecedor {
        -int id
        -String razaoSocial
        -String cnpj
        -String telefone
        -String email
    }

    class Lote {
        -int id
        -String numero
        -Date dataFabricacao
        -Date dataValidade
        -int quantidade
    }

    class Venda {
        -int id
        -Date dataHora
        -Decimal valorTotal
        -String formaPagamento
    }

    class ItemVenda {
        -int quantidade
        -Decimal precoUnitario
        -Decimal subtotal
    }

    class Receita {
        -int id
        -String numero
        -Date dataEmissao
        -String medico
        -String crm
    }

    Produto <|-- Medicamento
    Produto <|-- HigienePessoal
    Produto <|-- Cosmetico
    Medicamento <|-- MedicamentoControlado
    Medicamento <|-- MedicamentoGenerico
    Fornecedor "1" --> "0..*" Produto : fornece
    Produto "1" --> "0..*" Lote : possui
    Venda "1" *-- "1..*" ItemVenda : contém
    ItemVenda "0..*" --> "1" Produto : refere-se a
    Venda "0..1" --> "1" Receita : utiliza
    Receita "1" --> "1..*" Medicamento : prescreve
