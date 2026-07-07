# CloudFront Commands

Os comandos da **AWS CLI** para o serviço **Amazon CloudFront** permitem criar, configurar, consultar e remover distribuições de CDN (Content Delivery Network). O CloudFront distribui conteúdo globalmente através de pontos de presença (**Edge Locations**), reduzindo latência e melhorando a disponibilidade de aplicações, APIs e arquivos.

## 1. `aws cloudfront list-distributions`

**Descrição:**

Lista todas as distribuições CloudFront existentes na conta AWS.

Exibe informações como:

* ID da distribuição
* Domínio CloudFront
* Status
* Origem configurada
* Estado de habilitação

**Exemplo:**

```bash
aws cloudfront list-distributions
```

**Exemplo de saída:**

```json
{
    "DistributionList": {
        "Items": [
            {
                "Id": "E1234567890ABC",
                "DomainName": "d123abc.cloudfront.net",
                "Status": "Deployed"
            }
        ]
    }
}
```

---

# 2. `aws cloudfront get-distribution`

**Descrição:**

Obtém detalhes de uma distribuição CloudFront específica.

Mostra:

* Configuração da distribuição
* Origem (S3, Load Balancer, API Gateway)
* Cache behaviors
* Certificado SSL
* Domínio associado

**Exemplo:**

```bash
aws cloudfront get-distribution \
    --id E1234567890ABC
```

---

# 3. `aws cloudfront create-distribution`

**Descrição:**

Cria uma nova distribuição CloudFront.

Normalmente é utilizada para:

* Distribuir arquivos de um bucket S3
* Expor aplicações web
* Criar uma camada CDN para APIs

A configuração é enviada através de um arquivo JSON.

**Exemplo:**

```bash
aws cloudfront create-distribution \
    --distribution-config file://distribution-config.json
```

Exemplo simplificado de configuração:

```json
{
    "CallerReference": "20260707",
    "Origins": {
        "Quantity": 1,
        "Items": [
            {
                "Id": "S3-origin",
                "DomainName": "meu-bucket.s3.amazonaws.com"
            }
        ]
    },
    "Enabled": true
}
```

---

# 4. `aws cloudfront update-distribution`

**Descrição:**

Atualiza uma distribuição CloudFront existente.

É utilizado para alterar:

* Origem
* Cache
* Domínio
* Certificado SSL
* Configurações de acesso

**Exemplo:**

```bash
aws cloudfront update-distribution \
    --id E1234567890ABC \
    --if-match ETAG123456 \
    --distribution-config file://novo-config.json
```

> O parâmetro `--if-match` utiliza o ETag retornado pelo comando `get-distribution`.

---

# 5. `aws cloudfront delete-distribution`

**Descrição:**

Remove uma distribuição CloudFront.

Antes da exclusão:

1. A distribuição precisa estar desabilitada.
2. É necessário obter o ETag atual.

**Exemplo:**

```bash
aws cloudfront delete-distribution \
    --id E1234567890ABC \
    --if-match ETAG123456
```

---

# 6. `aws cloudfront get-distribution-config`

**Descrição:**

Obtém apenas a configuração de uma distribuição CloudFront.

É muito utilizado antes de realizar alterações.

Retorna:

* Configuração JSON
* ETag
* Origem
* Cache behaviors
* Configuração SSL

**Exemplo:**

```bash
aws cloudfront get-distribution-config \
    --id E1234567890ABC
```

---

# 7. `aws cloudfront create-invalidation`

**Descrição:**

Remove arquivos armazenados no cache do CloudFront.

É usado quando um arquivo foi atualizado no backend, mas ainda existe uma versão antiga armazenada nos Edge Locations.

Exemplos de uso:

* Atualização de site
* Nova versão de JavaScript
* Alteração de imagens

**Exemplo:**

Limpar um arquivo específico:

```bash
aws cloudfront create-invalidation \
    --distribution-id E1234567890ABC \
    --paths "/index.html"
```

Limpar todo o cache:

```bash
aws cloudfront create-invalidation \
    --distribution-id E1234567890ABC \
    --paths "/*"
```

---

# 8. `aws cloudfront list-invalidations`

**Descrição:**

Lista as invalidações de cache realizadas em uma distribuição.

Mostra:

* ID da invalidação
* Status
* Data de criação
* Arquivos invalidados

**Exemplo:**

```bash
aws cloudfront list-invalidations \
    --distribution-id E1234567890ABC
```

---

# 9. `aws cloudfront get-invalidation`

**Descrição:**

Obtém detalhes de uma invalidação específica.

**Exemplo:**

```bash
aws cloudfront get-invalidation \
    --distribution-id E1234567890ABC \
    --id I123456789ABC
```

Exemplo de saída:

```json
{
    "Invalidation": {
        "Id": "I123456789ABC",
        "Status": "Completed"
    }
}
```

---

# 10. `aws cloudfront list-tags-for-resource`

**Descrição:**

Lista as tags associadas a uma distribuição CloudFront.

Tags são utilizadas para:

* Organização
* Controle de custos
* Identificação por ambiente

**Exemplo:**

```bash
aws cloudfront list-tags-for-resource \
    --resource arn:aws:cloudfront::123456789012:distribution/E1234567890ABC
```

---

# 11. `aws cloudfront tag-resource`

**Descrição:**

Adiciona tags a uma distribuição CloudFront.

**Exemplo:**

```bash
aws cloudfront tag-resource \
    --resource arn:aws:cloudfront::123456789012:distribution/E1234567890ABC \
    --tags Items=[
        {
            "Key":"Environment",
            "Value":"Production"
        }
    ]
```

---

# 12. `aws cloudfront untag-resource`

**Descrição:**

Remove tags de uma distribuição.

**Exemplo:**

```bash
aws cloudfront untag-resource \
    --resource arn:aws:cloudfront::123456789012:distribution/E1234567890ABC \
    --tag-keys Environment
```

---

# Resumo dos comandos

| Comando                                  | Finalidade                                |
| ---------------------------------------- | ----------------------------------------- |
| `aws cloudfront list-distributions`      | Lista todas as distribuições CloudFront.  |
| `aws cloudfront get-distribution`        | Exibe detalhes de uma distribuição.       |
| `aws cloudfront create-distribution`     | Cria uma nova distribuição CDN.           |
| `aws cloudfront update-distribution`     | Atualiza uma distribuição existente.      |
| `aws cloudfront delete-distribution`     | Remove uma distribuição CloudFront.       |
| `aws cloudfront get-distribution-config` | Obtém a configuração de uma distribuição. |
| `aws cloudfront create-invalidation`     | Remove arquivos do cache do CloudFront.   |
| `aws cloudfront list-invalidations`      | Lista invalidações realizadas.            |
| `aws cloudfront get-invalidation`        | Consulta uma invalidação específica.      |
| `aws cloudfront list-tags-for-resource`  | Lista tags de uma distribuição.           |
| `aws cloudfront tag-resource`            | Adiciona tags a uma distribuição.         |
| `aws cloudfront untag-resource`          | Remove tags de uma distribuição.          |

---

# Fluxo típico de uso do CloudFront

Um fluxo comum para publicar uma aplicação usando CloudFront é:

1. Criar uma origem:

   * Bucket S3
   * Load Balancer
   * API Gateway
   * Servidor HTTP

2. Criar uma distribuição usando:

```bash
aws cloudfront create-distribution
```

3. Verificar a distribuição criada:

```bash
aws cloudfront list-distributions
```

4. Consultar detalhes:

```bash
aws cloudfront get-distribution
```

5. Publicar novas versões da aplicação.

6. Limpar arquivos antigos do cache:

```bash
aws cloudfront create-invalidation \
--paths "/*"
```

7. Monitorar invalidações:

```bash
aws cloudfront list-invalidations
```

8. Atualizar configurações quando necessário:

```bash
aws cloudfront update-distribution
```

9. Remover a distribuição quando não for mais utilizada:

```bash
aws cloudfront delete-distribution
```

> **Boas práticas:** Utilize CloudFront junto com **Amazon S3** para distribuição de arquivos estáticos, configure certificados SSL usando **AWS Certificate Manager (ACM)**, utilize políticas de cache adequadas, habilite logs quando necessário e proteja origens privadas usando **Origin Access Control (OAC)** para evitar acesso direto aos recursos de origem.
