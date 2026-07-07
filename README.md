<div align="center">
    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRwS5nxYo2PkOltT5AWWknYKxSWoS0_W7OHdoNnPN7YaQ&s=10">
</div>

A **Amazon Web Services (AWS)** é a plataforma de computação em nuvem da Amazon. Ela oferece centenas de serviços para que empresas e desenvolvedores possam criar, hospedar e gerenciar aplicações sem precisar manter uma infraestrutura física própria.

## O que é computação em nuvem?

Computação em nuvem é o fornecimento de recursos de TI pela internet, como servidores, armazenamento, bancos de dados e redes. Em vez de comprar e manter equipamentos, você utiliza os recursos sob demanda e paga apenas pelo que consome.

## Principais serviços da AWS

* [Amazon Athena](./docs/Athena.md): Serviço de consulta de dados (query).
  - Documentação: https://docs.aws.amazon.com/athena/
  - AWS CLI Commands: https://docs.aws.amazon.com/cli/latest/reference/athena/

* [Amazon EC2](./docs/ec2.md): Cria e gerencia servidores virtuais.
  - Documentação: https://docs.aws.amazon.com/ec2/
  - AWS CLI Commands: https://docs.aws.amazon.com/cli/latest/reference/ec2/

* [Amazon S3](./docs/s3.md): Armazena arquivos, imagens, vídeos e backups.
  - Documentação: https://docs.aws.amazon.com/s3/
  - AWS CLI Commands: https://docs.aws.amazon.com/cli/latest/reference/s3/

* [Amazon RDS](./docs/rds.md): Banco de dados relacional gerenciado.
  - Documentação: https://docs.aws.amazon.com/rds/
  - AWS CLI Commands: https://docs.aws.amazon.com/cli/latest/reference/rds/

* [Amazon DynamoDB](./docs/DynamoDB.md): Banco de dados NoSQL de alta disponibilidade.
  - Documentação: https://docs.aws.amazon.com/dynamodb/
  - AWS CLI Commands: https://docs.aws.amazon.com/cli/latest/reference/dynamodb/

* [Amazon Lambda](./docs/Lambda.md): Executa código sem gerenciar servidores.
  - Documentação: https://docs.aws.amazon.com/lambda/
  - AWS CLI Commands: https://docs.aws.amazon.com/cli/latest/reference/lambda/

* [Amazon VPC](./docs/VPC.md): Cria redes virtuais isoladas.
  - Documentação: https://docs.aws.amazon.com/vpc/
  - AWS CLI Commands: https://docs.aws.amazon.com/cli/latest/reference/ec2/

* [Amazon CloudWatch](./docs/CloudWatch.md): Monitora aplicações e infraestrutura.
  - Documentação: https://docs.aws.amazon.com/cloudwatch/
  - AWS CLI Commands:
    - CloudWatch: https://docs.aws.amazon.com/cli/latest/reference/cloudwatch/
    - CloudWatch Logs: https://docs.aws.amazon.com/cli/latest/reference/logs/

* [Amazon IAM](./docs/IAM.md): Controla usuários, permissões e acessos.
  - Documentação: https://docs.aws.amazon.com/iam/
  - AWS CLI Commands: https://docs.aws.amazon.com/cli/latest/reference/iam/

* [Amazon CloudFront](./docs/CloudFront.md): Distribui conteúdo com baixa latência.
  - Documentação: https://docs.aws.amazon.com/cloudfront/
  - AWS CLI Commands: https://docs.aws.amazon.com/cli/latest/reference/cloudfront/

* [Amazon API Gateway](./docs/gateway.md): Cria e gerencia APIs.
  - Documentação: https://docs.aws.amazon.com/apigateway/
  - AWS CLI Commands: https://docs.aws.amazon.com/cli/latest/reference/apigateway/

## Modelos de serviço

A AWS oferece diferentes modelos de computação em nuvem:

* **IaaS (Infrastructure as a Service):** fornece infraestrutura, como servidores, armazenamento e redes. Exemplo: Amazon EC2.
* **PaaS (Platform as a Service):** oferece uma plataforma para desenvolver e implantar aplicações sem gerenciar a infraestrutura. Exemplo: AWS Elastic Beanstalk.
* **Serverless:** permite executar aplicações sem administrar servidores. Exemplo: AWS Lambda.

## Vantagens da AWS

* Escalabilidade automática para acompanhar a demanda.
* Alta disponibilidade e tolerância a falhas.
* Segurança com criptografia e controle de acesso.
* Modelo de pagamento conforme o uso.
* Presença global com data centers em diversas regiões.

## Exemplos de uso

Empresas utilizam a AWS para:

* Hospedar sites e aplicações web.
* Desenvolver aplicativos para dispositivos móveis.
* Armazenar e analisar grandes volumes de dados.
* Executar soluções de inteligência artificial e aprendizado de máquina.
* Realizar backups e recuperação de desastres.
* Implementar arquiteturas de microsserviços e aplicações serverless.

## Exemplo de arquitetura simples

```mermaid
flowchart TD
    A[Usuário] --> B[CloudFront]
    B --> C[API Gateway]
    C --> D[AWS Lambda]
    D --> E[DynamoDB<br/>dados]
    D --> F[Amazon S3<br/>arquivos]
```

## Resumo

A AWS é uma das principais plataformas de computação em nuvem do mundo, oferecendo serviços para infraestrutura, armazenamento, bancos de dados, redes, segurança, monitoramento e desenvolvimento de aplicações. Seu principal diferencial é permitir que organizações utilizem recursos de TI de forma escalável, segura e com pagamento baseado no consumo, reduzindo a necessidade de investir em infraestrutura própria.

