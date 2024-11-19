# Custo Pagaso sistema atual na AWS

### 1. **Wireguard**
- **Configuração Atual**:
  - CPUs: 1
  - Armazenamento: 44 GB
  - Memória: 4 GB
- **Equivalente AWS**:
  - **Instância**: t3.medium (2 vCPUs, 4 GB RAM)
  - **Armazenamento**: 50 GB EBS General Purpose (gp3)
- **Custo Estimado**:
  - **Instância**: $0,0416/hora ≈ $30/mês
  - **Armazenamento**: $0,08/GB/mês x 50 GB = $4/mês
  - **Total Mensal**: **$34**

### 2. **Pagaso-aq-all**
- **Configuração Atual**:
  - CPUs: 1
  - Armazenamento: 404 GB
  - Memória: 4 GB
- **Equivalente AWS**:
  - **Instância**: t3.medium (2 vCPUs, 4 GB RAM)
  - **Armazenamento**: 410 GB EBS General Purpose (gp3)
- **Custo Estimado**:
  - **Instância**: $0,0416/hora ≈ $30/mês
  - **Armazenamento**: $0,08/GB/mês x 410 GB = $32,80/mês
  - **Total Mensal**: **$62,80**

### 3. **Pagaso-prd-nginx**
- **Configuração Atual**:
  - CPUs: 1
  - Armazenamento: 380 GB
  - Memória: 8 GB
- **Equivalente AWS**:
  - **Instância**: t3.large (2 vCPUs, 8 GB RAM)
  - **Armazenamento**: 400 GB EBS General Purpose (gp3)
- **Custo Estimado**:
  - **Instância**: $0,0832/hora ≈ $60/mês
  - **Armazenamento**: $0,08/GB/mês x 400 GB = $32/mês
  - **Total Mensal**: **$92**

### 4. **Pagaso-prd-db**
- **Configuração Atual**:
  - CPUs: 20
  - Armazenamento: 440 GB
  - Memória: 80 GB
- **Equivalente AWS**:
  - **Instância**: r5.4xlarge (16 vCPUs, 128 GB RAM)
  - **Armazenamento**: 450 GB EBS General Purpose (gp3)
- **Custo Estimado**:
  - **Instância**: $1,008/hora ≈ $730/mês
  - **Armazenamento**: $0,08/GB/mês x 450 GB = $36/mês
  - **Total Mensal**: **$766**

### 5. **Pagaso-prd-a1-app**
- **Configuração Atual**:
  - CPUs: 2
  - Armazenamento: 308 GB
  - Memória: 8 GB
- **Equivalente AWS**:
  - **Instância**: t3.large (2 vCPUs, 8 GB RAM)
  - **Armazenamento**: 310 GB EBS General Purpose (gp3)
- **Custo Estimado**:
  - **Instância**: $0,0832/hora ≈ $60/mês
  - **Armazenamento**: $0,08/GB/mês x 310 GB = $24,80/mês
  - **Total Mensal**: **$84,80**

### 6. **Pagaso-prd-b1-app**
- **Configuração Atual**:
  - CPUs: 2
  - Armazenamento: 308 GB
  - Memória: 8 GB
- **Equivalente AWS**:
  - **Instância**: t3.large (2 vCPUs, 8 GB RAM)
  - **Armazenamento**: 310 GB EBS General Purpose (gp3)
- **Custo Estimado**:
  - **Instância**: $0,0832/hora ≈ $60/mês
  - **Armazenamento**: $0,08/GB/mês x 310 GB = $24,80/mês
  - **Total Mensal**: **$84,80**

### **Resumo dos Custos Mensais**

| Serviço             | Custo Instância | Custo Armazenamento | Custo Total |
|---------------------|-----------------|---------------------|-------------|
| Wireguard           | $30             | $4                  | $34         |
| Pagaso-aq-all       | $30             | $32,80              | $62,80      |
| Pagaso-prd-nginx    | $60             | $32                 | $92         |
| Pagaso-prd-db       | $730            | $36                 | $766        |
| Pagaso-prd-a1-app   | $60             | $24,80              | $84,80      |
| Pagaso-prd-b1-app   | $60             | $24,80              | $84,80      |
| **Total Geral**     |                 |                     | **$1.124,40**|


---

## **Infraestrutura Escalável AWS com Base na Paratus Cloud**

### 1. **Banco de Dados: Amazon Aurora Serverless v2**
O Aurora Serverless v2 é ideal para atender à necessidade de escalabilidade automática, ajustando a capacidade com base na carga.

- **Configuração Proposta**:
  - Aurora Serverless v2.
  - **Capacidade**: Escala de 2 ACUs a 16 ACUs (4 a 32 vCPUs, 4 GB a 32 GB RAM).
  - **Armazenamento Inicial**: 500 GB (autoescalável até 128 TB).

- **Custo Estimado**:
  - **ACUs (2-16)**: $0,12/ACU/hora, considerando uso médio de 4 ACUs por 730 horas/mês ≈ **$350/mês**.
  - **Armazenamento**: $0,10/GB/mês x 500 GB = **$50/mês**.
  - **Total Banco de Dados**: **$400/mês**.

---

### 2. **Servidores de Aplicação: EC2 Auto Scaling Group**
Para os servidores de aplicação, configuramos um Auto Scaling Group com instâncias escaláveis conforme a carga.

- **Configuração Proposta**:
  - EC2 Auto Scaling com `t3.medium` (2 vCPUs, 4 GB RAM).
  - **Mínimo de Instâncias**: 2 (para alta disponibilidade).
  - **Máximo de Instâncias**: 4 (para atender picos).

- **Custo Estimado**:
  - **Instâncias Base (2)**: $0,0416/hora x 730 horas ≈ **$61/mês**.
  - **Instâncias Escaláveis (2 adicionais)**: Escaladas por 20% do tempo (146 horas/mês):
    - $0,0416/hora x 2 x 146 horas ≈ **$12/mês**.
  - **Total Servidores**: **$73/mês**.

---

### 3. **Balanceador de Carga: Elastic Load Balancer (ALB)**
Distribui o tráfego de forma automática entre os servidores de aplicação.

- **Configuração Proposta**:
  - Application Load Balancer (ALB).
  - **Custo Estimado**:
    - $18/mês + $0,008/GB transferido (estimando 1 TB de tráfego) ≈ **$26/mês**.

---

### 4. **Cache: Amazon ElastiCache com Redis**
O Redis gerenciado reduz a carga no banco de dados e aumenta o desempenho para consultas repetitivas.

- **Configuração Proposta**:
  - ElastiCache com `cache.t3.small` (1 vCPU, 2 GB RAM).
  - **Custo Estimado**:
    - $0,026/hora x 730 horas ≈ **$19/mês**.

---

### 5. **Filas de Processamento: Amazon SQS**
Gerencia tarefas assíncronas para comunicações internas e operações de longa duração.

- **Configuração Proposta**:
  - Amazon SQS.
  - **Custo Estimado**:
    - 1 milhão de mensagens por mês = $0,40/mês.

---

### 6. **Transferência de Dados**
- **Interna (entre serviços)**: $0,01/GB para 1 TB ≈ **$10/mês**.
- **Saída (para a internet)**: 1 TB x $0,09/GB ≈ **$90/mês**.

---

## **Resumo de Custos Mensais**

| Serviço                      | Custo Estimado |
|------------------------------|----------------|
| Aurora Serverless            | $400           |
| EC2 Auto Scaling (2-4 nós)   | $73            |
| Elastic Load Balancer (ALB)  | $26            |
| Redis (ElastiCache)          | $19            |
| SQS                          | $0,40          |
| Transferência Interna        | $10            |
| Transferência Saída (1 TB)   | $90            |

**Total Mensal (Infraestrutura Escalável)**: **$618,40/mês**

