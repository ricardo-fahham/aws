# EC2 Commands

Os comandos da **AWS CLI** para o serviço **EC2** permitem criar, gerenciar e consultar instâncias, volumes e grupos de segurança. Abaixo está uma descrição de cada comando com exemplos práticos.

## 1. `aws ec2 describe-instances`

**Descrição:**
Lista todas as instâncias EC2 da conta na região configurada, mostrando informações como:

* ID da instância
* Estado (running, stopped)
* Tipo
* IP público e privado
* Tags
* AMI utilizada

**Exemplo:**

```bash
aws ec2 describe-instances
```

**Exemplo de saída (resumida):**

```json
{
    "Reservations": [
        {
            "Instances": [
                {
                    "InstanceId": "i-0123456789abcdef0",
                    "InstanceType": "t3.micro",
                    "State": {
                        "Name": "running"
                    },
                    "PublicIpAddress": "54.10.20.30"
                }
            ]
        }
    ]
}
```

Consultar apenas instâncias em execução:

```bash
aws ec2 describe-instances \
    --filters Name=instance-state-name,Values=running
```

---

## 2. `aws ec2 run-instances`

**Descrição:**

Cria uma nova instância EC2.

É necessário informar pelo menos:

* AMI
* Tipo da instância

Normalmente também se informa:

* Security Group
* Key Pair
* Subnet
* Quantidade

**Exemplo:**

```bash
aws ec2 run-instances \
    --image-id ami-0abcdef1234567890 \
    --instance-type t2.micro \
    --key-name MinhaChave \
    --security-group-ids sg-0123456789abcdef0 \
    --count 1
```

Resultado:

Uma nova máquina virtual será criada.

---

## 3. `aws ec2 start-instances`

**Descrição:**

Liga uma instância que esteja parada (stopped).

**Exemplo:**

```bash
aws ec2 start-instances \
    --instance-ids i-0123456789abcdef0
```

Saída:

```json
{
    "StartingInstances": [
        {
            "CurrentState": {
                "Name": "pending"
            }
        }
    ]
}
```

---

## 4. `aws ec2 stop-instances`

**Descrição:**

Desliga uma instância.

A instância permanece existente e poderá ser ligada novamente.

**Exemplo:**

```bash
aws ec2 stop-instances \
    --instance-ids i-0123456789abcdef0
```

Estado:

```
running
↓
stopping
↓
stopped
```

---

## 5. `aws ec2 reboot-instances`

**Descrição:**

Reinicia uma instância.

Equivale ao reboot do sistema operacional.

**Exemplo:**

```bash
aws ec2 reboot-instances \
    --instance-ids i-0123456789abcdef0
```

Não altera:

* IP privado
* Disco EBS
* Configurações

---

## 6. `aws ec2 terminate-instances`

**Descrição:**

Remove definitivamente uma instância.

Após terminar:

* Não poderá ser ligada novamente.
* O volume raiz será apagado se estiver configurado para isso.

**Exemplo:**

```bash
aws ec2 terminate-instances \
    --instance-ids i-0123456789abcdef0
```

Fluxo:

```
running
↓
shutting-down
↓
terminated
```

---

## 7. `aws ec2 describe-instances`

Este comando aparece novamente na lista.

Sua função continua sendo consultar informações das instâncias.

Exemplo filtrando por ID:

```bash
aws ec2 describe-instances \
    --instance-ids i-0123456789abcdef0
```

---

## 8. `aws ec2 create-volume`

**Descrição:**

Cria um volume EBS.

Esse disco poderá ser anexado posteriormente a uma instância.

**Exemplo:**

```bash
aws ec2 create-volume \
    --availability-zone us-east-1a \
    --size 20 \
    --volume-type gp3
```

Parâmetros:

* `--size`: tamanho em GB
* `--volume-type`: tipo do disco
* `--availability-zone`: zona onde será criado

Saída:

```json
{
    "VolumeId": "vol-0123456789abcdef0",
    "State": "creating"
}
```

---

## 9. `aws ec2 attach-volume`

**Descrição:**

Anexa um volume EBS a uma instância.

**Exemplo:**

```bash
aws ec2 attach-volume \
    --volume-id vol-0123456789abcdef0 \
    --instance-id i-0123456789abcdef0 \
    --device /dev/sdf
```

Resultado:

O novo disco ficará disponível dentro da máquina.

No Linux normalmente aparecerá como:

```
/dev/xvdf
```

ou

```
/dev/nvme1n1
```

dependendo do tipo de instância.

---

## 10. `aws ec2 describe-security-groups`

**Descrição:**

Lista todos os Security Groups.

Mostra:

* Nome
* ID
* Regras de entrada (Inbound)
* Regras de saída (Outbound)
* VPC

**Exemplo:**

```bash
aws ec2 describe-security-groups
```

Exemplo resumido:

```json
{
    "SecurityGroups": [
        {
            "GroupName": "default",
            "GroupId": "sg-0123456789abcdef0",
            "Description": "Default VPC security group"
        }
    ]
}
```

Consultar um grupo específico:

```bash
aws ec2 describe-security-groups \
    --group-ids sg-0123456789abcdef0
```

---

# Resumo dos comandos

| Comando                            | Finalidade                                  |
| ---------------------------------- | ------------------------------------------- |
| `aws ec2 describe-instances`       | Lista informações das instâncias EC2.       |
| `aws ec2 run-instances`            | Cria uma nova instância EC2.                |
| `aws ec2 start-instances`          | Liga uma instância parada.                  |
| `aws ec2 stop-instances`           | Desliga uma instância em execução.          |
| `aws ec2 reboot-instances`         | Reinicia uma instância sem removê-la.       |
| `aws ec2 terminate-instances`      | Exclui definitivamente uma instância.       |
| `aws ec2 create-volume`            | Cria um volume EBS para armazenamento.      |
| `aws ec2 attach-volume`            | Anexa um volume EBS a uma instância EC2.    |
| `aws ec2 describe-security-groups` | Lista os grupos de segurança e suas regras. |

### Fluxo típico de uso

Um fluxo comum de administração de instâncias EC2 usando a AWS CLI é:

1. Criar uma instância com `run-instances`.
2. Consultar seu status com `describe-instances`.
3. Parar a instância quando não estiver em uso com `stop-instances`.
4. Iniciá-la novamente com `start-instances`.
5. Reiniciá-la, se necessário, com `reboot-instances`.
6. Criar e anexar um disco adicional usando `create-volume` e `attach-volume`.
7. Verificar ou ajustar as regras de acesso consultando os `describe-security-groups`.
8. Encerrar a instância definitivamente com `terminate-instances` quando ela não for mais necessária.
