# 🏥 Abstergo: Arquitetura de Serviços AWS

> Projeto desenvolvido como parte do desafio do bootcamp  
> **Santander 2025 – Ciência de Dados com Python**, promovido pela **DIO em parceria com o Banco Santander**.

---

![Image](https://github.com/user-attachments/assets/a3104ada-f20b-49a4-bcc1-edd588a2b9ea)

![AWS](https://img.shields.io/badge/AWS-Cloud-orange?style=for-the-badge)
![EC2](https://img.shields.io/badge/Amazon%20EC2-Compute-blue?style=for-the-badge)
![RDS](https://img.shields.io/badge/Amazon%20RDS-Database-blue?style=for-the-badge)
![S3](https://img.shields.io/badge/Amazon%20S3-Storage-green?style=for-the-badge)
![Bootcamp](https://img.shields.io/badge/DIO-Santander%202025-red?style=for-the-badge)

---

## 📌 Sobre o Projeto

Este projeto apresenta a proposta de implementação de uma **arquitetura AWS para um e-commerce farmacêutico**, desenvolvido para a empresa fictícia **Abstergo**, empresa esta, que quer começar a atuar com a redistribuição de produtos farmacêuticos para farmácias da região.

O objetivo é demonstrar, de forma prática e aplicada, como a computação em nuvem pode reduzir custos, aumentar a escalabilidade e garantir maior segurança e disponibilidade da aplicação.


---

## 🎯 Objetivo do Desafio

Implementar uma solução em nuvem utilizando serviços da AWS para uma empresa do setor farmacêutico, com foco em:

- Redução de custos operacionais  
- Escalabilidade automática  
- Alta disponibilidade  
- Segurança da informação  
- Processamento eficiente de pedidos  


---

# RELATÓRIO DE IMPLEMENTAÇÃO DE SERVIÇOS AWS

Data: 07/12/2025  
Empresa: Abstergo Industries  
Responsável: Lucas Pimenta Barretto

---

## Introdução

Este relatório apresenta o processo de implementação de ferramentas na empresa Abstergo Industries, realizado por Lucas Pimenta Barretto. O objetivo do projeto foi selecionar e aplicar serviços da AWS com foco principal na redução imediata de custos operacionais, escalabilidade automática da aplicação e modernização da infraestrutura, considerando o interesse na empresa em se tornar uma redistribuidora de produtos farmacêuticos.

Embora a atividade solicite o uso de três serviços principais, a arquitetura proposta utiliza serviços complementares essenciais para garantir segurança, processamento assíncrono, autenticação e monitoramento da aplicação.

---

## Descrição do Projeto

### Etapa 1:
- **Nome da ferramenta:** Amazon EC2 com Auto Scaling  
- **Foco da ferramenta:** Hospedagem da aplicação e escalabilidade automática  
- **Descrição de caso de uso:**  
  O Amazon EC2 é utilizado para hospedar o frontend e o backend da aplicação da Abstergo, ambos executados em contêineres Docker. O Auto Scaling permite que novas instâncias sejam criadas automaticamente conforme o aumento da demanda. O tráfego é distribuído por meio do Elastic Load Balancer (ELB), garantindo alta disponibilidade.

---

### Etapa 2:
- **Nome da ferramenta:** Amazon RDS  
- **Foco da ferramenta:** Banco de dados relacional gerenciado  
- **Descrição de caso de uso:**  
  O Amazon RDS armazena os dados de clientes, pedidos e estoque. O serviço gerencia backups, atualizações e falhas automaticamente, aumentando a confiabilidade e reduzindo custos operacionais.

---

### Etapa 3:
- **Nome da ferramenta:** Amazon S3  
- **Foco da ferramenta:** Armazenamento de arquivos estáticos  
- **Descrição de caso de uso:**  
  O Amazon S3 é utilizado para armazenar as imagens dos produtos, reduzindo o consumo de processamento nas instâncias EC2 e garantindo alta durabilidade com baixo custo.

---

## Serviços AWS Utilizados e Justificativa

| Serviço AWS | Função no Projeto | Motivo da Utilização |
|-------------|-------------------|-----------------------|
| Amazon EC2 | Hospedagem do frontend e backend da aplicação | Permite executar a aplicação em containers Docker com flexibilidade, controle do ambiente e alta capacidade de processamento. |
| Auto Scaling | Escalabilidade automática das instâncias EC2 | Garante aumento ou redução automática de servidores conforme a demanda, mantendo performance e reduzindo custos. |
| Elastic Load Balancer (ELB) | Distribuição de tráfego entre as instâncias | Evita sobrecarga em um único servidor e garante alta disponibilidade da aplicação. |
| Amazon RDS | Banco de dados relacional da aplicação | Armazena dados de clientes, pedidos e estoque com gerenciamento automático de backups, segurança e alta disponibilidade. |
| Amazon S3 | Armazenamento de imagens dos produtos | Reduz o consumo de recursos das instâncias EC2 e oferece alta durabilidade com baixo custo. |
| Amazon Cognito | Autenticação e cadastro de usuários | Garante segurança no login, controle de acesso e proteção dos dados dos clientes. |
| AWS Lambda | Validação automática dos pagamentos | Processa a validação sem necessidade de servidores dedicados, reduzindo custos e aumentando a eficiência. |
| Amazon SQS | Fila de processamento de pedidos | Organiza os pedidos e garante que sejam processados corretamente mesmo em picos de acesso. |
| Amazon SNS | Envio de notificações ao cliente | Informa automaticamente o status do pedido ao cliente em caso de sucesso ou falha. |
| Amazon CloudWatch | Monitoramento da aplicação | Permite acompanhamento de métricas, desempenho, logs e criação de alertas em tempo real. |

---

## 🔄 Fluxo de Funcionamento da Aplicação

1. O cliente acessa o site da Abstergo pelo navegador.  

2. A requisição passa inicialmente pelo **Elastic Load Balancer (ELB)**.  

3. O ELB direciona o acesso para uma **instância Amazon EC2 em Auto Scaling**, onde estão:
   - O frontend da aplicação  
   - O backend da aplicação  
   - Containers Docker  

4. O cliente realiza login ou cadastro por meio do **Amazon Cognito**.  

5. Após autenticado, o cliente navega pelo catálogo de produtos.  

6. As imagens dos produtos são carregadas a partir do **Amazon S3**.  

7. O cliente adiciona os produtos ao carrinho e clica em **"Pagar"**.  

8. **Validação do Pagamento com AWS Lambda (Fluxo Detalhado):**
   - O cliente clica em **"Pagar"** e o site envia os dados do cartão de forma criptografada.  
   - A instância **EC2 chama a função AWS Lambda**, solicitando a validação do pagamento.  
   - A **Lambda se conecta a uma API externa de pagamento** (como Cielo ou Stripe) e consulta se há saldo disponível.  
   - A API retorna uma resposta (**Sim ou Não**).  
   - A Lambda devolve essa resposta para o fluxo da aplicação:  
     - Se for **"Sim"**, o fluxo segue normalmente para o SQS.  
     - Se for **"Não"**, o fluxo é direcionado para o SNS para notificação ao cliente.  

9. Se o pagamento for aprovado:
   - O pedido é enviado para o **Amazon SQS**.  
   - O **Amazon RDS atualiza o estoque e registra o pedido**.  
   - O **Amazon SNS envia a confirmação ao cliente**.  

10. Se o pagamento for reprovado:
   - O **Amazon SNS envia uma notificação informando a falha**.  

11. Toda a aplicação é monitorada por meio do **Amazon CloudWatch**.

---

## 🗺️ Diagrama da Arquitetura

Abaixo, o diagrama visualiza a interação entre os componentes descritos nos passos 1 a 11:

```mermaid
graph TD
    classDef green fill:#2ecc71,stroke:#27ae60,stroke-width:2px,color:#fff
    classDef darkBlue fill:#34495e,stroke:#2c3e50,stroke-width:2px,color:#fff
    classDef lightBlue fill:#3498db,stroke:#2980b9,stroke-width:2px,color:#fff
    classDef orange fill:#e67e22,stroke:#d35400,stroke-width:2px,color:#fff
    classDef purple fill:#9b59b6,stroke:#8e44ad,stroke-width:2px,color:#fff
    classDef yellow fill:#f1c40f,stroke:#f39c12,stroke-width:2px,color:#2c3e50
    classDef red fill:#e74c3c,stroke:#c0392b,stroke-width:2px,color:#fff
    classDef database fill:#2980b9,stroke:#34495e,stroke-width:2px,color:#fff

    Client([1. Cliente: Acesso Navegador]):::green
    ELB[2. Elastic Load Balancer]:::darkBlue
    
    subgraph ASG [3. EC2 Auto Scaling Group]
        direction TB
        EC2[Frontend / Backend]:::lightBlue
    end

    Cognito[4. Amazon Cognito: Login]:::purple
    S3[/6. Amazon S3: Imagens/]:::orange
    
    Client --> ELB
    ELB --> EC2
    EC2 <--> Cognito
    EC2 -- 5. Navega Catálogo --> S3
    
    Lambda{{8. AWS Lambda: Valida Pagamento}}:::yellow
    EC2 -- 7. Finaliza Pedido --> Lambda

    Decision{9. Aprovado?}:::lightBlue
    Lambda --> Decision

    subgraph Success [Fluxo de Aprovação]
        direction TB
        SQS[Amazon SQS: Fila]:::darkBlue
        RDS[(Amazon RDS: Estoque)]:::database
        SNS_OK[Amazon SNS: Confirmação]:::purple
    end

    SNS_Fail[10. Amazon SNS: Falha]:::red

    Decision -->|Sim| SQS
    Decision -->|Sim| RDS
    Decision -->|Sim| SNS_OK
    Decision -->|Não| SNS_Fail

    CW((11. CloudWatch)):::purple
    CW -.- EC2
    CW -.- Lambda
    CW -.- RDS
```

## Conclusão

A implementação da arquitetura em nuvem para a Abstergo Industries proporciona redução de custos, aumento da escalabilidade, maior segurança e melhor desempenho da aplicação. A solução está preparada para crescimento e atende com eficiência um cenário real de e-commerce farmacêutico.

---

## 👨‍💻 Autor

**Lucas Pimenta**  
Bootcamp Santander 2025 – Ciência de Dados com Python  
DIO + Banco Santander  

