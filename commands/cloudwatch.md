# CloudWatch Commands

Os comandos da **AWS CLI** para os serviços **Amazon CloudWatch** e **Amazon CloudWatch Logs** permitem monitorar recursos da AWS, coletar métricas, configurar alarmes e consultar logs de aplicações e serviços.

## 1. `aws cloudwatch list-metrics`

**Descrição:**

Lista todas as métricas disponíveis no Amazon CloudWatch.

As métricas podem ser de diversos serviços, como:

* Amazon EC2
* Amazon RDS
* Amazon S3
* AWS Lambda
* Amazon ECS
* Aplicações personalizadas

**Exemplo:**

```bash id="9j2xgm"
aws cloudwatch list-metrics
```

Listando apenas métricas do EC2:

```bash id="n2y8ta"
aws cloudwatch list-metrics \
    --namespace AWS/EC2
```

**Exemplo de saída:**

```json id="r6o3ph"
{
    "Metrics": [
        {
            "Namespace": "AWS/EC2",
            "MetricName": "CPUUtilization"
        }
    ]
}
```

---

## 2. `aws cloudwatch get-metric-statistics`

**Descrição:**

Obtém estatísticas de uma métrica do CloudWatch durante um intervalo de tempo.

É possível consultar informações como:

* Média (Average)
* Máximo (Maximum)
* Mínimo (Minimum)
* Soma (Sum)

**Exemplo:**

Consultar a utilização média da CPU de uma instância EC2:

```bash id="ebtmrn"
aws cloudwatch get-metric-statistics \
    --namespace AWS/EC2 \
    --metric-name CPUUtilization \
    --dimensions Name=InstanceId,Value=i-0123456789abcdef0 \
    --start-time 2026-07-01T00:00:00Z \
    --end-time 2026-07-02T00:00:00Z \
    --period 3600 \
    --statistics Average
```

**Exemplo de saída:**

```json id="2r0o0o"
{
    "Datapoints": [
        {
            "Average": 18.7,
            "Timestamp": "2026-07-01T12:00:00Z"
        }
    ]
}
```

---

## 3. `aws cloudwatch put-metric-alarm`

**Descrição:**

Cria ou atualiza um alarme do CloudWatch.

Os alarmes podem ser usados para:

* Enviar notificações via Amazon SNS
* Acionar Auto Scaling
* Executar ações automatizadas

**Exemplo:**

Criando um alarme para CPU acima de 80%:

```bash id="vd54yu"
aws cloudwatch put-metric-alarm \
    --alarm-name CPUAlta \
    --metric-name CPUUtilization \
    --namespace AWS/EC2 \
    --statistic Average \
    --period 300 \
    --threshold 80 \
    --comparison-operator GreaterThanThreshold \
    --evaluation-periods 2
```

---

## 4. `aws cloudwatch describe-alarms`

**Descrição:**

Lista todos os alarmes configurados no CloudWatch.

Mostra informações como:

* Nome
* Estado
* Métrica monitorada
* Limiar configurado

**Exemplo:**

```bash id="m4o20i"
aws cloudwatch describe-alarms
```

Consultar um alarme específico:

```bash id="v7bdn3"
aws cloudwatch describe-alarms \
    --alarm-names CPUAlta
```

---

## 5. `aws cloudwatch delete-alarms`

**Descrição:**

Remove um ou mais alarmes do CloudWatch.

**Exemplo:**

```bash id="n3uhx2"
aws cloudwatch delete-alarms \
    --alarm-names CPUAlta
```

> **Atenção:** Após a exclusão, o CloudWatch deixará de monitorar a condição configurada nesse alarme.

---

# CloudWatch Logs Commands

## 6. `aws logs describe-log-groups`

**Descrição:**

Lista todos os grupos de logs (Log Groups) existentes.

Cada Log Group reúne logs relacionados a um serviço ou aplicação.

**Exemplo:**

```bash id="hnxyr4"
aws logs describe-log-groups
```

**Exemplo de saída:**

```json id="0f2teu"
{
    "logGroups": [
        {
            "logGroupName": "/aws/lambda/ProcessarPedidos"
        }
    ]
}
```

---

## 7. `aws logs create-log-group`

**Descrição:**

Cria um novo grupo de logs.

É utilizado para organizar logs de aplicações e serviços.

**Exemplo:**

```bash id="nzbexy"
aws logs create-log-group \
    --log-group-name MinhaAplicacao
```

Após a criação, será possível criar streams de logs dentro desse grupo.

---

## 8. `aws logs describe-log-streams`

**Descrição:**

Lista os Log Streams pertencentes a um Log Group.

Cada Log Stream representa uma sequência de eventos de log.

**Exemplo:**

```bash id="brdjnq"
aws logs describe-log-streams \
    --log-group-name MinhaAplicacao
```

**Exemplo de saída:**

```json id="7sdye4"
{
    "logStreams": [
        {
            "logStreamName": "2026/07/07"
        }
    ]
}
```

---

## 9. `aws logs get-log-events`

**Descrição:**

Obtém os eventos de um Log Stream específico.

É útil para visualizar os registros gerados por aplicações ou serviços.

**Exemplo:**

```bash id="5vm34s"
aws logs get-log-events \
    --log-group-name MinhaAplicacao \
    --log-stream-name 2026/07/07
```

**Exemplo de saída:**

```json id="5vjlwm"
{
    "events": [
        {
            "timestamp": 1751880000000,
            "message": "Aplicação iniciada."
        },
        {
            "timestamp": 1751880060000,
            "message": "Usuário autenticado."
        }
    ]
}
```

---

## 10. `aws logs filter-log-events`

**Descrição:**

Pesquisa eventos de log utilizando filtros.

Permite localizar mensagens específicas sem percorrer todos os registros.

**Exemplo:**

Pesquisar mensagens contendo "ERROR":

```bash id="nk1oz0"
aws logs filter-log-events \
    --log-group-name MinhaAplicacao \
    --filter-pattern "ERROR"
```

Pesquisar mensagens contendo "Timeout":

```bash id="8x8ghu"
aws logs filter-log-events \
    --log-group-name MinhaAplicacao \
    --filter-pattern "Timeout"
```

Também é possível filtrar por intervalo de tempo utilizando `--start-time` e `--end-time`.

---

# Resumo dos comandos

| Comando                                | Finalidade                                                  |
| -------------------------------------- | ----------------------------------------------------------- |
| `aws cloudwatch list-metrics`          | Lista as métricas disponíveis no CloudWatch.                |
| `aws cloudwatch get-metric-statistics` | Obtém estatísticas de uma métrica em um período específico. |
| `aws cloudwatch put-metric-alarm`      | Cria ou atualiza um alarme de monitoramento.                |
| `aws cloudwatch describe-alarms`       | Lista os alarmes configurados.                              |
| `aws cloudwatch delete-alarms`         | Remove um ou mais alarmes.                                  |
| `aws logs describe-log-groups`         | Lista todos os grupos de logs.                              |
| `aws logs create-log-group`            | Cria um novo grupo de logs.                                 |
| `aws logs describe-log-streams`        | Lista os streams de logs de um grupo.                       |
| `aws logs get-log-events`              | Exibe os eventos registrados em um Log Stream.              |
| `aws logs filter-log-events`           | Pesquisa eventos utilizando filtros.                        |

### Fluxo típico de monitoramento com CloudWatch

Um fluxo comum de monitoramento utilizando o Amazon CloudWatch e o CloudWatch Logs é:

1. Consultar as métricas disponíveis com `list-metrics`.
2. Obter estatísticas de uma métrica utilizando `get-metric-statistics`.
3. Criar alarmes para monitorar condições críticas com `put-metric-alarm`.
4. Verificar o estado dos alarmes usando `describe-alarms`.
5. Remover alarmes que não são mais necessários com `delete-alarms`.
6. Criar grupos de logs utilizando `create-log-group`.
7. Consultar os grupos existentes com `describe-log-groups`.
8. Listar os Log Streams através de `describe-log-streams`.
9. Visualizar eventos de log usando `get-log-events`.
10. Pesquisar mensagens específicas ou erros nos logs com `filter-log-events`.

> **Boas práticas:** Configure alarmes para métricas críticas (como utilização de CPU, memória por meio do CloudWatch Agent, latência e erros de aplicações), centralize os logs no CloudWatch Logs e utilize filtros para identificar rapidamente falhas, exceções e eventos relevantes. Isso facilita o monitoramento contínuo e a resposta a incidentes em ambientes AWS.
