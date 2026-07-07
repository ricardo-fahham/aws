# RDS Commands

Os comandos da **AWS CLI** para o serviço **Amazon RDS (Relational Database Service)** permitem criar, gerenciar e monitorar bancos de dados relacionais na AWS. Com eles, é possível iniciar, parar, reiniciar, excluir instâncias de banco de dados, além de criar snapshots e consultar grupos de parâmetros.

## 1. `aws rds describe-db-instances`

**Descrição:**

Lista todas as instâncias de banco de dados RDS da conta na região configurada.

Exibe informações como:

* Identificador da instância
* Engine (MySQL, PostgreSQL, MariaDB, Oracle, SQL Server)
* Classe da instância
* Status
* Endpoint
* Região e Zona de Disponibilidade

**Exemplo:**

```bash
aws rds describe-db-instances
```

**Exemplo de saída:**

```json
{
    "DBInstances": [
        {
            "DBInstanceIdentifier": "meu-banco",
            "Engine": "mysql",
            "DBInstanceStatus": "available",
            "DBInstanceClass": "db.t3.micro",
            "Endpoint": {
                "Address": "meu-banco.xxxxxxxxx.us-east-1.rds.amazonaws.com",
                "Port": 3306
            }
        }
    ]
}
```

Consultar uma instância específica:

```bash
aws rds describe-db-instances \
    --db-instance-identifier meu-banco
```

---

## 2. `aws rds create-db-instance`

**Descrição:**

Cria uma nova instância de banco de dados RDS.

É necessário informar parâmetros como:

* Identificador da instância
* Engine do banco
* Classe da instância
* Armazenamento
* Usuário administrador
* Senha

**Exemplo:**

```bash
aws rds create-db-instance \
    --db-instance-identifier meu-banco \
    --db-instance-class db.t3.micro \
    --engine mysql \
    --allocated-storage 20 \
    --master-username admin \
    --master-user-password MinhaSenha123
```

**Resultado:**

Uma nova instância RDS será criada e ficará disponível após alguns minutos.

---

## 3. `aws rds start-db-instance`

**Descrição:**

Inicia uma instância RDS que esteja parada.

> **Observação:** Nem todos os mecanismos (engines) do RDS permitem parar e iniciar instâncias. Além disso, uma instância pode permanecer parada por no máximo **7 dias**, após os quais a AWS a inicia automaticamente.

**Exemplo:**

```bash
aws rds start-db-instance \
    --db-instance-identifier meu-banco
```

---

## 4. `aws rds stop-db-instance`

**Descrição:**

Para uma instância RDS temporariamente.

Durante esse período:

* O armazenamento é mantido.
* O banco pode ser iniciado novamente.
* Não há cobrança pelo uso da instância, mas continuam sendo cobrados o armazenamento e os backups.

**Exemplo:**

```bash
aws rds stop-db-instance \
    --db-instance-identifier meu-banco
```

---

## 5. `aws rds reboot-db-instance`

**Descrição:**

Reinicia uma instância RDS.

Esse comando é útil para:

* Aplicar algumas alterações de configuração.
* Resolver problemas temporários.
* Atualizar conexões.

**Exemplo:**

```bash
aws rds reboot-db-instance \
    --db-instance-identifier meu-banco
```

Também é possível forçar um failover em implantações Multi-AZ:

```bash
aws rds reboot-db-instance \
    --db-instance-identifier meu-banco \
    --force-failover
```

---

## 6. `aws rds delete-db-instance`

**Descrição:**

Exclui permanentemente uma instância RDS.

Antes da exclusão, é possível criar um snapshot final para preservar os dados.

**Exemplo:**

```bash
aws rds delete-db-instance \
    --db-instance-identifier meu-banco \
    --skip-final-snapshot
```

Ou criando um snapshot final:

```bash
aws rds delete-db-instance \
    --db-instance-identifier meu-banco \
    --final-db-snapshot-identifier backup-final
```

> **Atenção:** Após a exclusão da instância, os dados não poderão ser recuperados, exceto se houver snapshots disponíveis.

---

## 7. `aws rds describe-db-snapshots`

**Descrição:**

Lista todos os snapshots de bancos RDS.

Mostra informações como:

* Nome do snapshot
* Banco de origem
* Data de criação
* Status

**Exemplo:**

```bash
aws rds describe-db-snapshots
```

Consultar snapshots de uma instância específica:

```bash
aws rds describe-db-snapshots \
    --db-instance-identifier meu-banco
```

---

## 8. `aws rds create-db-snapshot`

**Descrição:**

Cria manualmente um snapshot de uma instância RDS.

Snapshots permitem restaurar o banco para o estado em que ele se encontrava no momento da criação.

**Exemplo:**

```bash
aws rds create-db-snapshot \
    --db-instance-identifier meu-banco \
    --db-snapshot-identifier backup-julho
```

**Resultado:**

```json
{
    "DBSnapshot": {
        "DBSnapshotIdentifier": "backup-julho",
        "Status": "creating"
    }
}
```

---

## 9. `aws rds delete-db-snapshot`

**Descrição:**

Remove um snapshot manual do banco de dados.

**Exemplo:**

```bash
aws rds delete-db-snapshot \
    --db-snapshot-identifier backup-julho
```

> **Observação:** Snapshots automáticos são gerenciados pela AWS conforme a política de retenção configurada.

---

## 10. `aws rds describe-db-parameter-groups`

**Descrição:**

Lista os grupos de parâmetros (Parameter Groups) existentes.

Os grupos de parâmetros permitem personalizar configurações do mecanismo do banco, como:

* Número máximo de conexões
* Tamanho do cache
* Configurações de memória
* Logs
* Time zone

**Exemplo:**

```bash
aws rds describe-db-parameter-groups
```

**Exemplo de saída:**

```json
{
    "DBParameterGroups": [
        {
            "DBParameterGroupName": "default.mysql8.0",
            "Description": "Default parameter group"
        }
    ]
}
```

Consultar um grupo específico:

```bash
aws rds describe-db-parameters \
    --db-parameter-group-name default.mysql8.0
```

---

# Resumo dos comandos

| Comando                                | Finalidade                                 |
| -------------------------------------- | ------------------------------------------ |
| `aws rds describe-db-instances`        | Lista todas as instâncias RDS.             |
| `aws rds create-db-instance`           | Cria uma nova instância de banco de dados. |
| `aws rds start-db-instance`            | Inicia uma instância RDS parada.           |
| `aws rds stop-db-instance`             | Para temporariamente uma instância RDS.    |
| `aws rds reboot-db-instance`           | Reinicia uma instância RDS.                |
| `aws rds delete-db-instance`           | Exclui permanentemente uma instância RDS.  |
| `aws rds describe-db-snapshots`        | Lista os snapshots existentes.             |
| `aws rds create-db-snapshot`           | Cria um snapshot manual do banco de dados. |
| `aws rds delete-db-snapshot`           | Remove um snapshot manual.                 |
| `aws rds describe-db-parameter-groups` | Lista os grupos de parâmetros do RDS.      |

### Fluxo típico de gerenciamento de uma instância RDS

Um fluxo comum de administração de bancos de dados no Amazon RDS utilizando a AWS CLI é:

1. Criar uma instância com `create-db-instance`.
2. Verificar seu status usando `describe-db-instances`.
3. Parar a instância temporariamente com `stop-db-instance` (quando suportado pela engine).
4. Iniciá-la novamente com `start-db-instance`.
5. Reiniciá-la com `reboot-db-instance` para aplicar determinadas alterações.
6. Criar snapshots periódicos utilizando `create-db-snapshot`.
7. Consultar os snapshots disponíveis com `describe-db-snapshots`.
8. Excluir snapshots antigos usando `delete-db-snapshot`.
9. Verificar ou personalizar configurações por meio dos Parameter Groups.
10. Excluir a instância com `delete-db-instance`, opcionalmente criando um snapshot final para preservar os dados.
