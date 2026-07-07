# AWS CLI Configuration & Information Commands

Os comandos da **AWS CLI** para configuração e consulta de informações permitem configurar credenciais, verificar a identidade utilizada, consultar informações da conta e obter ajuda sobre os comandos disponíveis.

## 1. `aws configure`

**Descrição:**

Configura a AWS CLI para acesso à sua conta AWS.

Durante a configuração, serão solicitados:

* Access Key ID
* Secret Access Key
* Região padrão
* Formato de saída (json, yaml, table ou text)

**Exemplo:**

```bash
aws configure
```

**Saída:**

```text
AWS Access Key ID [None]: AKIAxxxxxxxxxxxxxxxx
AWS Secret Access Key [None]: ********************************
Default region name [None]: us-east-1
Default output format [None]: json
```

As informações são armazenadas em:

Linux/macOS:

```text
~/.aws/credentials
~/.aws/config
```

Windows:

```text
C:\Users\<usuario>\.aws\
```

---

## 2. `aws configure list`

**Descrição:**

Exibe a configuração atualmente utilizada pela AWS CLI.

Mostra:

* Access Key
* Secret Key (oculta)
* Região
* Método de autenticação

**Exemplo:**

```bash
aws configure list
```

**Exemplo de saída:**

```text
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile                <not set>             None
access_key     ****************ABCD      shared-credentials-file
secret_key     ****************WXYZ      shared-credentials-file
    region               us-east-1        config-file
```

---

## 3. `aws sts get-caller-identity`

**Descrição:**

Mostra qual identidade AWS está sendo utilizada pela AWS CLI.

É um dos comandos mais utilizados para verificar se as credenciais estão corretas.

Retorna:

* Account ID
* ARN
* User ID

**Exemplo:**

```bash
aws sts get-caller-identity
```

**Saída:**

```json
{
    "UserId": "AIDAXXXXXXXXXXXXX",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/admin"
}
```

---

## 4. `aws sts get-session-token`

**Descrição:**

Solicita credenciais temporárias do AWS Security Token Service (STS).

É bastante utilizado quando:

* MFA está habilitado.
* É necessário utilizar credenciais temporárias.
* Deseja-se aumentar a segurança dos acessos.

**Exemplo:**

```bash
aws sts get-session-token
```

Solicitando um token válido por 12 horas:

```bash
aws sts get-session-token \
    --duration-seconds 43200
```

**Resultado:**

```json
{
    "Credentials": {
        "AccessKeyId": "...",
        "SecretAccessKey": "...",
        "SessionToken": "...",
        "Expiration": "2026-07-08T02:00:00Z"
    }
}
```

---

## 5. `aws --version`

**Descrição:**

Exibe a versão instalada da AWS CLI.

**Exemplo:**

```bash
aws --version
```

**Saída:**

```text
aws-cli/2.27.0 Python/3.12 Linux/5.15 exe/x86_64
```

Esse comando é útil para verificar se a versão instalada suporta determinados recursos.

---

## 6. `aws help`

**Descrição:**

Exibe a documentação integrada da AWS CLI.

Permite consultar:

* Serviços
* Comandos
* Parâmetros
* Exemplos

**Exemplo:**

```bash
aws help
```

Ajuda de um serviço específico:

```bash
aws ec2 help
```

Ajuda de um comando específico:

```bash
aws s3 cp help
```

---

## 7. `aws iam list-users`

**Descrição:**

Lista todos os usuários IAM da conta.

Mostra informações como:

* Nome
* ARN
* Data de criação

**Exemplo:**

```bash
aws iam list-users
```

**Saída resumida:**

```json
{
    "Users": [
        {
            "UserName": "admin",
            "Arn": "arn:aws:iam::123456789012:user/admin"
        }
    ]
}
```

---

## 8. `aws iam get-user`

**Descrição:**

Obtém informações detalhadas de um usuário IAM.

Se nenhum nome for informado, retorna os dados do usuário correspondente às credenciais atualmente utilizadas.

**Exemplo:**

```bash
aws iam get-user
```

Consultando outro usuário:

```bash
aws iam get-user \
    --user-name desenvolvedor
```

**Exemplo de saída:**

```json
{
    "User": {
        "UserName": "desenvolvedor",
        "CreateDate": "2025-06-15T10:30:00Z"
    }
}
```

---

## 9. `aws account get-contact-information`

**Descrição:**

Obtém as informações de contato cadastradas na conta AWS.

Retorna dados como:

* Nome da empresa
* Endereço
* E-mail de contato
* Telefone

**Exemplo:**

```bash
aws account get-contact-information
```

**Exemplo de saída:**

```json
{
    "ContactInformation": {
        "FullName": "Empresa Exemplo",
        "EmailAddress": "contato@empresa.com"
    }
}
```

> **Observação:** Esse comando exige permissões para o serviço **AWS Account Management** e pode não estar disponível em contas pertencentes a uma **AWS Organization**, dependendo das políticas aplicadas.

---

## 10. `aws ec2 describe-regions`

**Descrição:**

Lista todas as regiões AWS disponíveis para a conta.

> **Correção:** Esse é o comando correto no lugar de `aws region list`.

**Exemplo:**

```bash
aws ec2 describe-regions
```

**Exemplo de saída:**

```json
{
    "Regions": [
        {
            "RegionName": "us-east-1"
        },
        {
            "RegionName": "us-west-2"
        },
        {
            "RegionName": "sa-east-1"
        }
    ]
}
```

Listando apenas os nomes das regiões:

```bash
aws ec2 describe-regions \
    --query "Regions[].RegionName" \
    --output text
```

---

# Resumo dos comandos

| Comando                               | Finalidade                                                               |
| ------------------------------------- | ------------------------------------------------------------------------ |
| `aws configure`                       | Configura as credenciais e a região padrão da AWS CLI.                   |
| `aws configure list`                  | Exibe a configuração atual da AWS CLI.                                   |
| `aws sts get-caller-identity`         | Mostra a identidade (usuário ou role) utilizada nas chamadas da AWS CLI. |
| `aws sts get-session-token`           | Gera credenciais temporárias por meio do AWS STS.                        |
| `aws --version`                       | Exibe a versão instalada da AWS CLI.                                     |
| `aws help`                            | Mostra a documentação e ajuda dos comandos da AWS CLI.                   |
| `aws iam list-users`                  | Lista os usuários IAM da conta.                                          |
| `aws iam get-user`                    | Exibe informações detalhadas de um usuário IAM.                          |
| `aws account get-contact-information` | Consulta as informações de contato da conta AWS.                         |
| `aws ec2 describe-regions`            | Lista todas as regiões AWS disponíveis.                                  |

### Fluxo típico de configuração da AWS CLI

Um fluxo comum para configurar e validar o ambiente da AWS CLI é:

1. Instalar a AWS CLI e verificar a versão com `aws --version`.
2. Configurar as credenciais utilizando `aws configure`.
3. Confirmar a configuração com `aws configure list`.
4. Verificar qual usuário ou role está sendo utilizado com `aws sts get-caller-identity`.
5. Gerar credenciais temporárias, quando necessário, usando `aws sts get-session-token`.
6. Consultar a documentação integrada por meio de `aws help` ou da ajuda de serviços específicos.
7. Verificar os usuários IAM disponíveis com `aws iam list-users` e consultar detalhes com `aws iam get-user`.
8. Consultar as informações de contato da conta utilizando `aws account get-contact-information` (quando suportado).
9. Listar as regiões disponíveis com `aws ec2 describe-regions` antes de criar recursos em uma região específica.
