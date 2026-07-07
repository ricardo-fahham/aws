# API Gateway Commands

Os comandos da **AWS CLI** para o serviço **Amazon API Gateway** permitem criar, gerenciar e monitorar APIs REST, HTTP e WebSocket. Com eles, é possível criar APIs, configurar recursos e métodos, implantar versões (stages) e visualizar informações sobre as APIs existentes.

## 1. `aws apigateway get-rest-apis`

**Descrição:**

Lista todas as APIs REST existentes na conta AWS.

Exibe informações como:

* ID da API
* Nome
* Descrição
* Data de criação

**Exemplo:**

```bash
aws apigateway get-rest-apis
```

**Exemplo de saída:**

```json
{
    "items": [
        {
            "id": "a1b2c3d4",
            "name": "MinhaAPI",
            "description": "API de pedidos"
        }
    ]
}
```

---

## 2. `aws apigateway create-rest-api`

**Descrição:**

Cria uma nova API REST.

É necessário informar pelo menos o nome da API.

**Exemplo:**

```bash
aws apigateway create-rest-api \
    --name MinhaAPI
```

Criando uma API com descrição:

```bash
aws apigateway create-rest-api \
    --name MinhaAPI \
    --description "API para gerenciamento de pedidos"
```

---

## 3. `aws apigateway get-resources`

**Descrição:**

Lista todos os recursos (Resources) de uma API REST.

Os recursos representam os caminhos (paths) da API, como:

* `/`
* `/clientes`
* `/produtos`
* `/pedidos`

**Exemplo:**

```bash
aws apigateway get-resources \
    --rest-api-id a1b2c3d4
```

**Exemplo de saída:**

```json
{
    "items": [
        {
            "id": "abc123",
            "path": "/"
        },
        {
            "id": "def456",
            "path": "/pedidos"
        }
    ]
}
```

---

## 4. `aws apigateway create-resource`

**Descrição:**

Cria um novo recurso (endpoint) dentro de uma API.

É necessário informar:

* ID da API
* ID do recurso pai
* Nome do caminho

**Exemplo:**

```bash
aws apigateway create-resource \
    --rest-api-id a1b2c3d4 \
    --parent-id abc123 \
    --path-part pedidos
```

Resultado:

Será criado o endpoint:

```text
/pedidos
```

---

## 5. `aws apigateway put-method`

**Descrição:**

Cria um método HTTP para um recurso.

Métodos suportados:

* GET
* POST
* PUT
* DELETE
* PATCH
* OPTIONS

**Exemplo:**

Criando um método GET:

```bash
aws apigateway put-method \
    --rest-api-id a1b2c3d4 \
    --resource-id def456 \
    --http-method GET \
    --authorization-type NONE
```

Também é possível utilizar outros tipos de autenticação, como:

* AWS_IAM
* CUSTOM
* COGNITO_USER_POOLS

---

## 6. `aws apigateway put-integration`

**Descrição:**

Configura a integração entre um método da API e o backend.

O backend pode ser:

* AWS Lambda
* Amazon S3
* Serviço HTTP
* Mock Integration

**Exemplo:**

Integrando com uma função Lambda:

```bash
aws apigateway put-integration \
    --rest-api-id a1b2c3d4 \
    --resource-id def456 \
    --http-method GET \
    --type AWS_PROXY \
    --integration-http-method POST \
    --uri arn:aws:apigateway:us-east-1:lambda:path/2015-03-31/functions/arn:aws:lambda:us-east-1:123456789012:function:MinhaFuncao/invocations
```

---

## 7. `aws apigateway create-deployment`

**Descrição:**

Publica (deploy) uma API em um Stage.

Após qualquer alteração na API, é necessário criar uma nova implantação para que as mudanças fiquem disponíveis.

**Exemplo:**

```bash
aws apigateway create-deployment \
    --rest-api-id a1b2c3d4 \
    --stage-name prod
```

Também é possível utilizar outros ambientes:

* dev
* test
* homolog
* staging

---

## 8. `aws apigateway get-stages`

**Descrição:**

Lista todos os Stages (ambientes) de uma API.

Mostra informações como:

* Nome
* Data da implantação
* Configurações

**Exemplo:**

```bash
aws apigateway get-stages \
    --rest-api-id a1b2c3d4
```

**Exemplo de saída:**

```json
{
    "item": [
        {
            "stageName": "prod"
        },
        {
            "stageName": "dev"
        }
    ]
}
```

---

## 9. `aws apigateway delete-rest-api`

**Descrição:**

Remove permanentemente uma API REST.

**Exemplo:**

```bash
aws apigateway delete-rest-api \
    --rest-api-id a1b2c3d4
```

> **Atenção:** Após a exclusão, todos os recursos, métodos e implantações da API serão removidos.

---

## 10. `aws apigateway get-export`

**Descrição:**

Exporta a definição da API em formatos como:

* OpenAPI (Swagger)
* YAML
* JSON

Esse recurso é útil para backup, documentação ou migração da API.

**Exemplo:**

```bash
aws apigateway get-export \
    --rest-api-id a1b2c3d4 \
    --stage-name prod \
    --export-type swagger \
    api-swagger.json
```

---

# Resumo dos comandos

| Comando                            | Finalidade                                               |
| ---------------------------------- | -------------------------------------------------------- |
| `aws apigateway get-rest-apis`     | Lista todas as APIs REST.                                |
| `aws apigateway create-rest-api`   | Cria uma nova API REST.                                  |
| `aws apigateway get-resources`     | Lista os recursos (endpoints) de uma API.                |
| `aws apigateway create-resource`   | Cria um novo recurso (endpoint).                         |
| `aws apigateway put-method`        | Adiciona um método HTTP a um recurso.                    |
| `aws apigateway put-integration`   | Configura a integração entre a API e o backend.          |
| `aws apigateway create-deployment` | Publica a API em um Stage.                               |
| `aws apigateway get-stages`        | Lista os ambientes (Stages) da API.                      |
| `aws apigateway delete-rest-api`   | Remove uma API REST.                                     |
| `aws apigateway get-export`        | Exporta a definição da API nos formatos OpenAPI/Swagger. |

### Fluxo típico de criação de uma API REST

Um fluxo comum de desenvolvimento com o Amazon API Gateway utilizando a AWS CLI é:

1. Criar uma API com `create-rest-api`.
2. Consultar os recursos existentes usando `get-resources`.
3. Criar um novo endpoint com `create-resource`.
4. Adicionar métodos HTTP (`GET`, `POST`, `PUT`, `DELETE`) usando `put-method`.
5. Configurar a integração com um backend, como uma função Lambda, utilizando `put-integration`.
6. Publicar a API em um ambiente (`dev`, `test` ou `prod`) com `create-deployment`.
7. Verificar os Stages disponíveis usando `get-stages`.
8. Exportar a definição da API para documentação ou backup com `get-export`.
9. Excluir a API com `delete-rest-api` quando ela não for mais necessária.

> **Boas práticas:** Integre o API Gateway com **AWS Lambda** para arquiteturas serverless, utilize **Stages** separados para desenvolvimento, testes e produção, habilite autenticação (IAM, Amazon Cognito ou Lambda Authorizers) quando necessário e monitore a API utilizando **Amazon CloudWatch** para acompanhar métricas, logs e alarmes.
