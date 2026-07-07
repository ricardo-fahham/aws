# CloudFormation Commands

Os comandos da **AWS CLI** para **AWS CloudFormation**, **AWS Resource Groups Tagging API**, **AWS Cost Explorer** e **AWS Budgets** permitem gerenciar infraestrutura como código, consultar recursos da conta, monitorar custos e acompanhar orçamentos.

## 1. `aws cloudformation list-stacks`

**Descrição:**

Lista todas as stacks do AWS CloudFormation na região configurada.

Mostra informações como:

* Nome da stack
* Status
* Data de criação
* Última atualização

**Exemplo:**

```bash
aws cloudformation list-stacks
```

**Exemplo de saída:**

```json
{
    "StackSummaries": [
        {
            "StackName": "MinhaAplicacao",
            "StackStatus": "CREATE_COMPLETE"
        }
    ]
}
```

---

## 2. `aws cloudformation describe-stacks`

**Descrição:**

Exibe informações detalhadas sobre uma stack específica.

Mostra:

* Recursos
* Parâmetros
* Outputs
* Status
* Tags

**Exemplo:**

```bash
aws cloudformation describe-stacks \
    --stack-name MinhaAplicacao
```

---

## 3. `aws cloudformation create-stack`

**Descrição:**

Cria uma nova stack utilizando um template CloudFormation.

O template pode estar armazenado:

* Localmente
* No Amazon S3

**Exemplo:**

```bash
aws cloudformation create-stack \
    --stack-name MinhaAplicacao \
    --template-body file://template.yaml
```

Utilizando um template no S3:

```bash
aws cloudformation create-stack \
    --stack-name MinhaAplicacao \
    --template-url https://meu-bucket.s3.amazonaws.com/template.yaml
```

Após a criação, o CloudFormation provisionará automaticamente todos os recursos definidos no template.

---

## 4. `aws cloudformation delete-stack`

**Descrição:**

Remove uma stack e todos os recursos criados por ela (exceto aqueles protegidos por políticas de retenção).

**Exemplo:**

```bash
aws cloudformation delete-stack \
    --stack-name MinhaAplicacao
```

> **Atenção:** A exclusão pode remover recursos como EC2, VPCs, bancos RDS e buckets S3, dependendo do template.

---

# Resource Groups Tagging API Commands

## 5. `aws resourcegroupstaggingapi get-resources`

**Descrição:**

Lista os recursos da AWS que possuem tags.

É muito útil para localizar recursos por ambiente, projeto, equipe ou centro de custo.

**Exemplo:**

```bash
aws resourcegroupstaggingapi get-resources
```

Filtrando por uma tag:

```bash
aws resourcegroupstaggingapi get-resources \
    --tag-filters Key=Environment,Values=Production
```

**Exemplo de saída:**

```json
{
    "ResourceTagMappingList": [
        {
            "ResourceARN": "arn:aws:ec2:...",
            "Tags": [
                {
                    "Key": "Environment",
                    "Value": "Production"
                }
            ]
        }
    ]
}
```

---

# Cost Explorer Commands

## 6. `aws ce get-cost-and-usage`

**Descrição:**

Consulta os custos e o uso dos serviços AWS durante um período específico.

É possível agrupar os resultados por:

* Serviço
* Conta
* Região
* Tag

**Exemplo:**

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-07-01,End=2026-07-31 \
    --granularity MONTHLY \
    --metrics BlendedCost
```

Também é possível agrupar por serviço:

```bash
aws ce get-cost-and-usage \
    --time-period Start=2026-07-01,End=2026-07-31 \
    --granularity MONTHLY \
    --metrics UnblendedCost \
    --group-by Type=DIMENSION,Key=SERVICE
```

---

## 7. `aws ce get-cost-forecast`

**Descrição:**

Estima os custos futuros da conta AWS com base no consumo atual.

É útil para prever gastos antes do fechamento da fatura.

**Exemplo:**

```bash
aws ce get-cost-forecast \
    --time-period Start=2026-08-01,End=2026-08-31 \
    --metric UNBLENDED_COST \
    --granularity MONTHLY
```

**Exemplo de saída:**

```json
{
    "Total": {
        "Amount": "128.45",
        "Unit": "USD"
    }
}
```

---

## 8. `aws ce get-dimension-values`

**Descrição:**

Lista os valores disponíveis para uma determinada dimensão do Cost Explorer.

Algumas dimensões disponíveis são:

* SERVICE
* REGION
* LINKED_ACCOUNT
* USAGE_TYPE

**Exemplo:**

```bash
aws ce get-dimension-values \
    --dimension SERVICE \
    --time-period Start=2026-07-01,End=2026-07-31
```

**Exemplo de saída:**

```json
{
    "DimensionValues": [
        {
            "Value": "Amazon EC2"
        },
        {
            "Value": "Amazon S3"
        }
    ]
}
```

---

# AWS Budgets Commands

## 9. `aws budgets describe-budgets`

**Descrição:**

Lista todos os orçamentos (Budgets) configurados para uma conta AWS.

Os orçamentos podem controlar:

* Custos
* Uso
* Instâncias Reservadas
* Savings Plans

**Exemplo:**

```bash
aws budgets describe-budgets \
    --account-id 123456789012
```

**Exemplo de saída:**

```json
{
    "Budgets": [
        {
            "BudgetName": "OrcamentoMensal",
            "BudgetLimit": {
                "Amount": "500"
            }
        }
    ]
}
```

---

## 10. `aws budgets describe-budget`

**Descrição:**

Obtém informações detalhadas de um orçamento específico.

**Exemplo:**

```bash
aws budgets describe-budget \
    --account-id 123456789012 \
    --budget-name OrcamentoMensal
```

Mostra informações como:

* Valor do orçamento
* Tipo
* Período
* Limites
* Configuração de alertas

---

# Resumo dos comandos

| Comando                                      | Finalidade                                                 |
| -------------------------------------------- | ---------------------------------------------------------- |
| `aws cloudformation list-stacks`             | Lista todas as stacks do CloudFormation.                   |
| `aws cloudformation describe-stacks`         | Exibe detalhes de uma stack.                               |
| `aws cloudformation create-stack`            | Cria uma nova stack a partir de um template.               |
| `aws cloudformation delete-stack`            | Remove uma stack e seus recursos.                          |
| `aws resourcegroupstaggingapi get-resources` | Lista recursos AWS e suas tags.                            |
| `aws ce get-cost-and-usage`                  | Consulta custos e uso da conta AWS.                        |
| `aws ce get-cost-forecast`                   | Estima os custos futuros da conta.                         |
| `aws ce get-dimension-values`                | Lista valores disponíveis para dimensões do Cost Explorer. |
| `aws budgets describe-budgets`               | Lista todos os orçamentos configurados.                    |
| `aws budgets describe-budget`                | Exibe os detalhes de um orçamento específico.              |

### Fluxo típico de gerenciamento de infraestrutura e custos

Um fluxo comum de administração utilizando esses serviços da AWS CLI é:

1. Criar uma infraestrutura com `cloudformation create-stack`.
2. Acompanhar a implantação utilizando `describe-stacks`.
3. Listar todas as stacks com `list-stacks`.
4. Consultar recursos da conta e suas tags usando `resourcegroupstaggingapi get-resources`.
5. Monitorar os custos mensais com `ce get-cost-and-usage`.
6. Estimar gastos futuros utilizando `ce get-cost-forecast`.
7. Consultar os serviços e dimensões disponíveis com `ce get-dimension-values`.
8. Verificar os orçamentos configurados através de `budgets describe-budgets`.
9. Consultar os detalhes de um orçamento específico com `budgets describe-budget`.
10. Remover uma infraestrutura quando ela não for mais necessária usando `cloudformation delete-stack`.

> **Boas práticas:** Utilize **AWS CloudFormation** para provisionar infraestrutura de forma consistente e reproduzível, aplique **tags** em todos os recursos para facilitar a organização e a alocação de custos, acompanhe regularmente os gastos com o **Cost Explorer** e configure **AWS Budgets** com alertas para evitar surpresas na fatura da AWS.
