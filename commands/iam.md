# IAM Commands

Os comandos da **AWS CLI** para o serviço **AWS IAM (Identity and Access Management)** permitem criar e gerenciar usuários, funções (roles) e políticas (policies), além de controlar as permissões de acesso aos recursos da AWS.

## 1. `aws iam list-policies`

**Descrição:**

Lista as políticas (Policies) disponíveis na conta AWS.

As políticas definem quais ações um usuário, grupo ou função pode executar sobre os recursos da AWS.

É possível listar tanto políticas gerenciadas pela AWS quanto políticas personalizadas da conta.

**Exemplo:**

```bash
aws iam list-policies
```

**Exemplo de saída:**

```json
{
    "Policies": [
        {
            "PolicyName": "AdministratorAccess",
            "Arn": "arn:aws:iam::aws:policy/AdministratorAccess"
        },
        {
            "PolicyName": "AmazonS3ReadOnlyAccess",
            "Arn": "arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess"
        }
    ]
}
```

---

## 2. `aws iam list-roles`

**Descrição:**

Lista todas as funções (Roles) existentes na conta.

As Roles são utilizadas para conceder permissões temporárias a serviços AWS, aplicações ou usuários.

**Exemplo:**

```bash
aws iam list-roles
```

**Exemplo de saída:**

```json
{
    "Roles": [
        {
            "RoleName": "LambdaExecutionRole",
            "Arn": "arn:aws:iam::123456789012:role/LambdaExecutionRole"
        }
    ]
}
```

---

## 3. `aws iam list-users`

**Descrição:**

Lista todos os usuários IAM existentes na conta.

Mostra informações como:

* Nome do usuário
* ARN
* Data de criação
* ID do usuário

**Exemplo:**

```bash
aws iam list-users
```

**Saída resumida:**

```json
{
    "Users": [
        {
            "UserName": "joao",
            "Arn": "arn:aws:iam::123456789012:user/joao"
        }
    ]
}
```

---

## 4. `aws iam create-user --user-name`

**Descrição:**

Cria um novo usuário IAM.

O usuário poderá receber permissões por meio de políticas ou grupos.

**Exemplo:**

```bash
aws iam create-user \
    --user-name desenvolvedor
```

**Resultado:**

```json
{
    "User": {
        "UserName": "desenvolvedor",
        "Arn": "arn:aws:iam::123456789012:user/desenvolvedor"
    }
}
```

---

## 5. `aws iam create-role --role-name`

**Descrição:**

Cria uma nova Role IAM.

Além do nome, é necessário fornecer um documento de confiança (*Trust Policy*), que define quem poderá assumir essa função.

**Exemplo:**

```bash
aws iam create-role \
    --role-name LambdaExecutionRole \
    --assume-role-policy-document file://trust-policy.json
```

**Exemplo de um arquivo `trust-policy.json`:**

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "Service": "lambda.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

---

## 6. `aws iam attach-user-policy`

**Descrição:**

Associa uma política gerenciada a um usuário IAM.

Essa política define as permissões concedidas ao usuário.

**Exemplo:**

```bash
aws iam attach-user-policy \
    --user-name desenvolvedor \
    --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```

Nesse exemplo, o usuário receberá permissão somente para leitura no Amazon S3.

---

## 7. `aws iam attach-role-policy`

**Descrição:**

Associa uma política gerenciada a uma Role IAM.

É muito utilizado para conceder permissões a serviços como:

* AWS Lambda
* Amazon EC2
* Amazon ECS
* AWS CodeBuild

**Exemplo:**

```bash
aws iam attach-role-policy \
    --role-name LambdaExecutionRole \
    --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
```

Após a associação, qualquer serviço que assumir essa Role herdará as permissões da política.

---

## 8. `aws iam list-attached-user-policies`

**Descrição:**

Lista todas as políticas anexadas a um usuário IAM.

**Exemplo:**

```bash
aws iam list-attached-user-policies \
    --user-name desenvolvedor
```

**Exemplo de saída:**

```json
{
    "AttachedPolicies": [
        {
            "PolicyName": "AmazonS3ReadOnlyAccess"
        }
    ]
}
```

---

## 9. `aws iam list-attached-role-policies`

**Descrição:**

Lista todas as políticas anexadas a uma Role IAM.

**Exemplo:**

```bash
aws iam list-attached-role-policies \
    --role-name LambdaExecutionRole
```

**Exemplo de saída:**

```json
{
    "AttachedPolicies": [
        {
            "PolicyName": "AWSLambdaBasicExecutionRole"
        }
    ]
}
```

---

## 10. `aws iam delete-user --user-name`

**Descrição:**

Remove permanentemente um usuário IAM.

Antes da exclusão, é necessário remover:

* Políticas anexadas
* Chaves de acesso (Access Keys)
* Certificados
* Login Profile (caso exista)
* Associação a grupos

**Exemplo:**

```bash
aws iam delete-user \
    --user-name desenvolvedor
```

> **Atenção:** Caso o usuário ainda possua recursos associados, a exclusão falhará até que esses recursos sejam removidos.

---

# Resumo dos comandos

| Comando                               | Finalidade                                |
| ------------------------------------- | ----------------------------------------- |
| `aws iam list-policies`               | Lista as políticas IAM disponíveis.       |
| `aws iam list-roles`                  | Lista todas as Roles da conta.            |
| `aws iam list-users`                  | Lista todos os usuários IAM.              |
| `aws iam create-user --user-name`     | Cria um novo usuário IAM.                 |
| `aws iam create-role --role-name`     | Cria uma nova Role IAM.                   |
| `aws iam attach-user-policy`          | Associa uma política a um usuário.        |
| `aws iam attach-role-policy`          | Associa uma política a uma Role.          |
| `aws iam list-attached-user-policies` | Lista as políticas anexadas a um usuário. |
| `aws iam list-attached-role-policies` | Lista as políticas anexadas a uma Role.   |
| `aws iam delete-user --user-name`     | Exclui um usuário IAM.                    |

### Fluxo típico de gerenciamento de identidades no IAM

Um fluxo comum de administração de identidades e permissões utilizando a AWS CLI é:

1. Criar um usuário com `create-user`.
2. Verificar os usuários existentes usando `list-users`.
3. Criar uma Role com `create-role` para ser utilizada por serviços AWS.
4. Consultar as políticas disponíveis com `list-policies`.
5. Conceder permissões ao usuário usando `attach-user-policy`.
6. Conceder permissões à Role usando `attach-role-policy`.
7. Verificar as políticas associadas ao usuário com `list-attached-user-policies`.
8. Verificar as políticas associadas à Role com `list-attached-role-policies`.
9. Remover usuários que não são mais necessários utilizando `delete-user`.

> **Boas práticas:** Sempre siga o princípio do **menor privilégio (Least Privilege)**, concedendo apenas as permissões estritamente necessárias para que usuários, aplicações e serviços executem suas funções. Isso reduz riscos de segurança e facilita a governança dos acessos na AWS.
