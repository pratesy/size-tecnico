# Antecipação de Recebível
Seu desafio será implementar um serviço que calcule a antecipação de recebível de uma empresa,

# O que é Antecipação de Recebível?
Uma antecipação de recebíveis, de forma bem simples, é quando uma empresa ou pessoa vende algo a prazo e, ao invés de esperar o pagamento no futuro, ela quer o dinheiro antes. Para conseguir esse dinheiro antecipado, ela pede para o banco ou uma financeira adiantar o valor das vendas que ainda vão ser pagas.

Imagine que uma empresa vende um serviço ou produto para outra empresa e emite uma nota fiscal de R$ 10.000,00. A empresa compradora tem um prazo de 30 dias para pagar essa nota. Ou seja, a empresa que vendeu só vai receber o dinheiro daqui a 30 dias.

Agora, suponha que essa empresa precisa do dinheiro antes e não pode esperar os 30 dias. Ela pode ir a um banco ou uma financeira e pedir para antecipar o valor dessa nota fiscal. O banco então "compra" essa nota, adiantando o dinheiro para a empresa. No entanto, o banco vai cobrar uma taxa de antecipação. Então, ao invés de receber os R$ 10.000,00, a empresa vai receber um valor menor, por exemplo, R$ 9.700,00, com o banco ficando com a diferença como pagamento pela antecipação.

Resumindo: a antecipação de recebíveis com nota fiscal é quando uma empresa vende a prazo, emite uma nota fiscal e, em vez de esperar o pagamento no vencimento, pede para um banco antecipar o valor. Isso dá à empresa o dinheiro agora, mas com um desconto pela taxa do banco.

# Objetivo
> Implementar um serviço onde uma empresa pode solicitar a antecipação de recebíveis, calculando o valor a ser antecipado com base nas notas fiscais cadastradas e no limite de crédito, que varia de acordo com o faturamento mensal e o ramo da empresa.

# Requisitos Funcionais

**Cadastro de Empresas**
> A empresa deve se cadastrar com os seguintes dados:
> - CNPJ (obrigatório)
> - Nome (obrigatório)
> - Faturamento Mensal (obrigatório)
> - Ramo (obrigatório, “Serviços” ou “Produtos”)

**Cálculo de Limite**
> O limite de antecipação varia conforme o faturamento mensal e o ramo da empresa:
> - Faturamento entre R$ 10.000,00 e R$ 50.000,00
>   - 50% do faturamento.
> - Faturamento entre R$ 50.001,00 e R$ 100.000,00
>   - Empresa de Serviços 55% do faturamento.
>   - Empresa de Produtos 60% do faturamento.
> - Faturamento acima de R$ 100.001,00
>   - Empresa de Serviços 60% do faturamento.
>   - Empresa de Produtos 65% do faturamento.

**Cadastro de Notas Fiscais**

> A empresa deve cadastrar NFs com os seguintes dados:
> - Número (obrigatório) 
> - Valor (obrigatório) 
> - Data de Vencimento (obrigatório, formato de data, vencimento maior que hoje)

**Carrinho de Antecipação**
> - A empresa pode adicionar e remover notas fiscais ao carrinho.
> - As notas fiscais no carrinho devem ser usadas para calcular o valor a ser antecipado.
> - O valor total das notas no carrinho não pode ultrapassar o limite de crédito da empresa.

**Cálculo de Antecipação (Checkout)**
> - Para calcular o valor a ser antecipado no carrinho, seguir a fórmula:
> - Prazo: Data Atual - Data de Vencimento da Nota Fiscal (em dias)
> - Taxa: 4,65% ao mês
> - Deságio: ValorNF / (1 + Taxa)^(Prazo / 30)
> - Valor Líquido: ValorNF - Deságio
> - **O sistema deve retornar um JSON com o valor final de antecipação para cada nota e o valor total do carrinho.**

``` json
{
  "empresa": "Nome da Empresa",
  "cnpj": "XXXXXXXXXXXXXX",
  "limite": 50000,
  "notas_fiscais": [
    {
      "numero": 1234,
      "valor_bruto": 10000,
      "valor_liquido": 9534.88
    },
    {
      "numero": 5678,
      "valor_bruto": 20000,
      "valor_liquido": 19069.75
    }
  ],
  "total_liquido": 28574.63
  "total_bruto": 30000
}
```

# Exemplos

Supondo que um usuario tenha as seguintes NFS

NF 10
- Vencimento 15/11/2024
- Valor R$ 5000,00
- Prazo: 30
  - Deságio: R$ 192,31
  - Valor Liquido: R$ 4.807,69



NF 11
- Vencimento 15/12/2024
- Valor Bruto R$ 7000,00
- Prazo: 60
  - Deságio: R$ 528,11
  - Valor Liquido: R$ 6471,89

NF 1
- Vencimento 31/10/2024
- Valor Bruto R$ 850,80
- Prazo 15
  - Deságio: R$ 16,52
  - Valor Liquido: R$ 834,28


``` json
{
  "empresa": "Nome da Empresa",
  "cnpj": "XXXXXXXXXXXXXX",
  "limite": 50000,
  "notas_fiscais": [
    {
      "numero": 1,
      "valor_bruto": 850.80,
      "valor_liquido": 834.28
    },
    {
      "numero": 10,
      "valor_bruto": 5000,
      "valor_liquido": 4807.69
    },
    {
      "numero": 11,
      "valor_bruto": 7000,
      "valor_liquido": 6471.89
    }
  ],
  "total_liquido": 12113.86
  "total_bruto": 12850.80
}
```







