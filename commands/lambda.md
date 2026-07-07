# Lambda Commands

Os comandos da **AWS CLI** para o serviço **AWS Lambda** permitem criar, gerenciar e executar funções serverless, além de configurar permissões, gatilhos (triggers) e aliases. Abaixo está a descrição de cada comando com exemplos práticos.

## 1. `aws lambda list-functions`

**Descrição:**

Lista todas as funções Lambda existentes na conta e na região configurada.

Exibe informações como:

* Nome da função
* Runtime
* ARN
* Última modificação
* Memória configurada
* Timeout

**Exemplo:**

```bash
aws lambda list-functions
```

**Exemplo de saída:**

```json
{
    "Functions": [
        {
            "FunctionName": "ProcessarPedidos",
            "Runtime": "python3.12",
            "MemorySize": 256,
            "Timeout": 30,
            "LastModified": "2025-06-15T12:45:30.000+0000"
        }
    ]
}
```

---

## 2. `aws lambda create-function`

**Descrição:**

Cria uma nova função Lambda.

Para criar uma função é necessário informar:

* Nome da função
* Runtime
* Função (Handler)
* Role IAM
* Código da função (arquivo ZIP ou imagem de contêiner)

**Exemplo:**

```bash
aws lambda create-function \
    --function-name ProcessarPedidos \
    --runtime python3.12 \
    --role arn:aws:iam::123456789012:role/LambdaExecutionRole \
    --handler lambda_function.lambda_handler \
    --zip-file fileb://function.zip
```

**Resultado:**

Uma nova função Lambda será criada e ficará disponível para execução.

---

## 3. `aws lambda invoke --function-name`

**Descrição:**

Executa uma função Lambda manualmente.

O resultado da execução é gravado em um arquivo especificado.

**Exemplo:**

```bash
aws lambda invoke \
    --function-name ProcessarPedidos \
    response.json
```

Executando com um evento em formato JSON:

```bash
aws lambda invoke \
    --function-name ProcessarPedidos \
    --payload '{"pedido":"12345"}' \
    response.json
```

**Resultado:**

```json
{
    "StatusCode": 200
}
```

O retorno da função será salvo em `response.json`.

---

## 4. `aws lambda update-function-code`

**Descrição:**

Atualiza o código de uma função Lambda existente.

É útil para publicar uma nova versão da aplicação sem recriar a função.

**Exemplo:**

```bash
aws lambda update-function-code \
    --function-name ProcessarPedidos \
    --zip-file fileb://nova-versao.zip
```

Também é possível atualizar utilizando um arquivo armazenado no Amazon S3:

```bash
aws lambda update-function-code \
    --function-name ProcessarPedidos \
    --s3-bucket lambda-deploy \
    --s3-key function.zip
```

---

## 5. `aws lambda delete-function`

**Descrição:**

Exclui permanentemente uma função Lambda.

**Exemplo:**

```bash
aws lambda delete-function \
    --function-name ProcessarPedidos
```

> **Atenção:** Após a exclusão, a função não poderá mais ser executada, a menos que seja recriada.

---

## 6. `aws lambda get-function`

**Descrição:**

Obtém informações detalhadas sobre uma função Lambda específica.

Mostra dados como:

* Configuração
* Runtime
* Handler
* Variáveis de ambiente
* Código
* ARN
* Última modificação

**Exemplo:**

```bash
aws lambda get-function \
    --function-name ProcessarPedidos
```

**Exemplo de saída:**

```json
{
    "Configuration": {
        "FunctionName": "ProcessarPedidos",
        "Runtime": "python3.12",
        "Handler": "lambda_function.lambda_handler",
        "MemorySize": 256,
        "Timeout": 30
    }
}
```

---

## 7. `aws lambda list-event-source-mappings`

**Descrição:**

Lista os gatilhos (Event Source Mappings) configurados para funções Lambda.

Esses gatilhos podem ser provenientes de serviços como:

* Amazon SQS
* Amazon Kinesis
* Amazon DynamoDB Streams
* Amazon MSK

**Exemplo:**

```bash
aws lambda list-event-source-mappings
```

Consultar os gatilhos de uma função específica:

```bash
aws lambda list-event-source-mappings \
    --function-name ProcessarPedidos
```

---

## 8. `aws lambda add-permission`

**Descrição:**

Adiciona uma permissão para que outro serviço AWS possa invocar a função Lambda.

É bastante utilizado para permitir chamadas provenientes de:

* Amazon API Gateway
* Amazon S3
* Amazon EventBridge
* Amazon SNS

**Exemplo:**

```bash
aws lambda add-permission \
    --function-name ProcessarPedidos \
    --statement-id s3invoke \
    --action lambda:InvokeFunction \
    --principal s3.amazonaws.com
```

Após essa configuração, o serviço autorizado poderá invocar a função, conforme as demais restrições definidas.

---

## 9. `aws lambda remove-permission`

**Descrição:**

Remove uma permissão previamente adicionada à função Lambda.

**Exemplo:**

```bash
aws lambda remove-permission \
    --function-name ProcessarPedidos \
    --statement-id s3invoke
```

Isso revoga a permissão identificada pelo `statement-id`.

---

## 10. `aws lambda list-aliases`

**Descrição:**

Lista os aliases configurados para uma função Lambda.

Os aliases são nomes amigáveis que apontam para versões específicas da função, como:

* `dev`
* `test`
* `staging`
* `prod`

Isso facilita implantações e atualizações sem alterar o nome da função.

**Exemplo:**

```bash
aws lambda list-aliases \
    --function-name ProcessarPedidos
```

**Exemplo de saída:**

```json
{
    "Aliases": [
        {
            "Name": "prod",
            "FunctionVersion": "15"
        },
        {
            "Name": "dev",
            "FunctionVersion": "$LATEST"
        }
    ]
}
```

---

# Resumo dos comandos

| Comando                                 | Finalidade                                             |
| --------------------------------------- | ------------------------------------------------------ |
| `aws lambda list-functions`             | Lista todas as funções Lambda.                         |
| `aws lambda create-function`            | Cria uma nova função Lambda.                           |
| `aws lambda invoke --function-name`     | Executa uma função Lambda manualmente.                 |
| `aws lambda update-function-code`       | Atualiza o código de uma função existente.             |
| `aws lambda delete-function`            | Exclui uma função Lambda.                              |
| `aws lambda get-function`               | Exibe informações detalhadas sobre uma função.         |
| `aws lambda list-event-source-mappings` | Lista os gatilhos configurados para as funções.        |
| `aws lambda add-permission`             | Concede permissão para outro serviço invocar a função. |
| `aws lambda remove-permission`          | Remove uma permissão previamente concedida.            |
| `aws lambda list-aliases`               | Lista os aliases associados à função Lambda.           |

### Fluxo típico de gerenciamento de uma função Lambda

Um fluxo comum de desenvolvimento e administração de funções no AWS Lambda utilizando a AWS CLI é:

1. Criar a função com `create-function`.
2. Verificar sua configuração usando `get-function`.
3. Executar testes manuais com `invoke`.
4. Atualizar o código da função utilizando `update-function-code`.
5. Configurar permissões para integração com outros serviços usando `add-permission`.
6. Verificar os gatilhos associados com `list-event-source-mappings`.
7. Utilizar aliases (`dev`, `test`, `prod`) para controlar versões da função com `list-aliases`.
8. Remover permissões desnecessárias utilizando `remove-permission`.
9. Excluir a função quando ela não for mais necessária com `delete-function`.
