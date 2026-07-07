# VPC Commands

Os comandos da **AWS CLI** para o serviço **Amazon VPC (Virtual Private Cloud)** permitem criar e gerenciar redes virtuais, sub-redes, tabelas de rotas, gateways de internet e grupos de segurança. Abaixo está a descrição de cada comando com exemplos práticos.

## 1. `aws ec2 describe-vpcs`

**Descrição:**

Lista todas as VPCs existentes na conta e na região configurada.

Exibe informações como:

* ID da VPC
* Bloco CIDR
* Estado
* VPC padrão (Default)
* Tags

**Exemplo:**

```bash
aws ec2 describe-vpcs
```

**Exemplo de saída:**

```json
{
    "Vpcs": [
        {
            "VpcId": "vpc-0123456789abcdef0",
            "CidrBlock": "10.0.0.0/16",
            "State": "available",
            "IsDefault": false
        }
    ]
}
```

Consultar uma VPC específica:

```bash
aws ec2 describe-vpcs \
    --vpc-ids vpc-0123456789abcdef0
```

---

## 2. `aws ec2 create-vpc --cidr-block`

**Descrição:**

Cria uma nova VPC.

O bloco CIDR define o intervalo de endereços IP privados disponíveis para a rede.

**Exemplo:**

```bash
aws ec2 create-vpc \
    --cidr-block 10.0.0.0/16
```

**Resultado:**

```json
{
    "Vpc": {
        "VpcId": "vpc-0123456789abcdef0",
        "State": "pending"
    }
}
```

Neste exemplo:

* Rede: `10.0.0.0`
* Máscara: `/16`
* Total de IPs possíveis: 65.536

---

## 3. `aws ec2 describe-subnets`

**Descrição:**

Lista todas as sub-redes (Subnets) da conta.

Mostra informações como:

* ID da subnet
* VPC associada
* Zona de disponibilidade
* Bloco CIDR

**Exemplo:**

```bash
aws ec2 describe-subnets
```

**Saída resumida:**

```json
{
    "Subnets": [
        {
            "SubnetId": "subnet-0123456789abcdef0",
            "VpcId": "vpc-0123456789abcdef0",
            "CidrBlock": "10.0.1.0/24",
            "AvailabilityZone": "us-east-1a"
        }
    ]
}
```

---

## 4. `aws ec2 create-subnet --vpc-id`

**Descrição:**

Cria uma nova subnet dentro de uma VPC existente.

É necessário informar:

* VPC
* Zona de disponibilidade
* Bloco CIDR

**Exemplo:**

```bash
aws ec2 create-subnet \
    --vpc-id vpc-0123456789abcdef0 \
    --cidr-block 10.0.1.0/24 \
    --availability-zone us-east-1a
```

**Resultado:**

```json
{
    "Subnet": {
        "SubnetId": "subnet-0abcdef1234567890",
        "State": "available"
    }
}
```

---

## 5. `aws ec2 describe-route-tables`

**Descrição:**

Lista todas as tabelas de rotas (Route Tables).

Essas tabelas definem para onde o tráfego da rede será encaminhado.

**Exemplo:**

```bash
aws ec2 describe-route-tables
```

**Exemplo de saída:**

```json
{
    "RouteTables": [
        {
            "RouteTableId": "rtb-0123456789abcdef0",
            "VpcId": "vpc-0123456789abcdef0"
        }
    ]
}
```

---

## 6. `aws ec2 create-route-table`

**Descrição:**

Cria uma nova tabela de rotas para uma VPC.

**Exemplo:**

```bash
aws ec2 create-route-table \
    --vpc-id vpc-0123456789abcdef0
```

**Resultado:**

```json
{
    "RouteTable": {
        "RouteTableId": "rtb-0abcdef1234567890"
    }
}
```

Após criar a tabela, normalmente adicionam-se rotas utilizando o comando `create-route`.

---

## 7. `aws ec2 describe-internet-gateways`

**Descrição:**

Lista todos os Internet Gateways (IGWs) existentes.

O Internet Gateway permite que recursos da VPC tenham acesso à Internet.

**Exemplo:**

```bash
aws ec2 describe-internet-gateways
```

**Saída resumida:**

```json
{
    "InternetGateways": [
        {
            "InternetGatewayId": "igw-0123456789abcdef0"
        }
    ]
}
```

---

## 8. `aws ec2 create-internet-gateway`

**Descrição:**

Cria um novo Internet Gateway.

Após a criação, ele ainda não estará conectado a nenhuma VPC.

**Exemplo:**

```bash
aws ec2 create-internet-gateway
```

**Resultado:**

```json
{
    "InternetGateway": {
        "InternetGatewayId": "igw-0123456789abcdef0"
    }
}
```

---

## 9. `aws ec2 attach-internet-gateway`

**Descrição:**

Associa um Internet Gateway a uma VPC.

Sem essa associação, a VPC não terá acesso à Internet, mesmo que existam rotas configuradas.

**Exemplo:**

```bash
aws ec2 attach-internet-gateway \
    --internet-gateway-id igw-0123456789abcdef0 \
    --vpc-id vpc-0123456789abcdef0
```

Após esse comando, basta configurar uma rota para `0.0.0.0/0` apontando para o Internet Gateway para permitir acesso à Internet.

---

## 10. `aws ec2 describe-security-groups`

**Descrição:**

Lista todos os Security Groups.

Um Security Group funciona como um firewall virtual para controlar o tráfego de entrada e saída das instâncias EC2.

Mostra informações como:

* Nome
* ID
* VPC
* Regras de entrada (Inbound)
* Regras de saída (Outbound)

**Exemplo:**

```bash
aws ec2 describe-security-groups
```

Consultar um grupo específico:

```bash
aws ec2 describe-security-groups \
    --group-ids sg-0123456789abcdef0
```

---

# Resumo dos comandos

| Comando                              | Finalidade                                        |
| ------------------------------------ | ------------------------------------------------- |
| `aws ec2 describe-vpcs`              | Lista todas as VPCs da conta.                     |
| `aws ec2 create-vpc --cidr-block`    | Cria uma nova VPC com um bloco CIDR especificado. |
| `aws ec2 describe-subnets`           | Lista todas as sub-redes (Subnets).               |
| `aws ec2 create-subnet --vpc-id`     | Cria uma subnet dentro de uma VPC.                |
| `aws ec2 describe-route-tables`      | Lista as tabelas de rotas da VPC.                 |
| `aws ec2 create-route-table`         | Cria uma nova tabela de rotas.                    |
| `aws ec2 describe-internet-gateways` | Lista os Internet Gateways existentes.            |
| `aws ec2 create-internet-gateway`    | Cria um novo Internet Gateway.                    |
| `aws ec2 attach-internet-gateway`    | Associa um Internet Gateway a uma VPC.            |
| `aws ec2 describe-security-groups`   | Lista os grupos de segurança e suas regras.       |

### Fluxo típico de criação de uma VPC

Um fluxo comum para configurar uma VPC usando a AWS CLI é:

1. Criar a VPC com `create-vpc`.
2. Consultar a VPC criada usando `describe-vpcs`.
3. Criar uma ou mais subnets com `create-subnet`.
4. Criar uma tabela de rotas utilizando `create-route-table`.
5. Criar um Internet Gateway com `create-internet-gateway`.
6. Associar o Internet Gateway à VPC usando `attach-internet-gateway`.
7. Configurar rotas para permitir acesso à Internet (por exemplo, usando `create-route`).
8. Verificar ou configurar os Security Groups para controlar o acesso aos recursos da VPC.
