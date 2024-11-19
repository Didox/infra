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

### **Proposta com Audiência e Carga**
1. **Histórico**:
   - Você inicialmente mencionou uma carga de **100 transações por minuto**.
   - Isso equivale a **144.000 transações por dia** ou **4,32 milhões de transações por mês**.

2. **Carga no Banco de Dados**:
   - Cada transação pode envolver múltiplas consultas ao banco de dados (leitura/escrita).
   - Supondo que cada transação gere, em média, **5 consultas**, o banco de dados precisa lidar com **21,6 milhões de operações mensais**.

3. **Servidores de Aplicação**:
   - Os servidores devem processar **100 requisições por minuto**, incluindo validações e chamadas ao banco.

4. **Transferência de Dados**:
   - Supondo que cada transação envolva a transferência de 5 KB (banco + resposta), o total seria:
     - **100/min × 5 KB × 60 min × 24 horas × 30 dias ≈ 1 TB/mês**.

---

### **Impacto na Configuração**
Com base nesses dados, os custos podem aumentar para atender à carga:

#### 1. **Banco de Dados: Amazon Aurora Serverless v2**
- Aurora Serverless escalonará conforme a demanda de leitura e escrita.
- Para **21,6 milhões de operações por mês**, é razoável esperar que o banco funcione com uma média de **8 ACUs** (em vez de 4).
  - **Custo Atualizado**:
    - 8 ACUs x $0,12/ACU/hora x 730 horas ≈ **$700/mês**.
  - Armazenamento e backups permanecem os mesmos: **$50/mês**.

**Total Banco de Dados**: **$750/mês**.

---

#### 2. **Servidores de Aplicação: EC2 Auto Scaling Group**
- Para atender 100 transações por minuto:
  - **Configuração Base**: 2 instâncias `t3.medium`.
  - **Escalabilidade**: Até 6 instâncias durante picos.

- **Custo Atualizado**:
  - Base (2 instâncias): $0,0416 x 730 x 2 ≈ **$61/mês**.
  - Escaláveis (4 instâncias): $0,0416 x 730 x 0,5 (50% do tempo) ≈ **$61/mês**.

**Total Servidores**: **$122/mês**.

---

#### 3. **Redis (ElastiCache)**
Redis lidará com consultas repetitivas, ajudando a aliviar o banco de dados.

- Redis pode ser mantido em `cache.t3.medium` para suportar o volume.
- **Custo Estimado**:
  - $0,052/hora x 730 horas ≈ **$38/mês**.

---

#### 4. **Elastic Load Balancer (ALB)**
O ALB distribuirá as requisições para as instâncias de aplicação.

- **Custo Atualizado**:
  - Base: $18/mês.
  - Transferência: $0,008/GB x 1 TB ≈ **$26/mês**.

**Total ALB**: **$44/mês**.

---

#### 5. **Filas (Amazon SQS)**
Com o volume de 4,32 milhões de mensagens mensais:
- $0,40/milhão x 4,32 ≈ **$1,73/mês**.

---

#### 6. **Transferência de Dados**
- **Interno (entre serviços)**: $0,01/GB para 1 TB ≈ **$10/mês**.
- **Saída (para a internet)**: $0,09/GB x 1 TB ≈ **$90/mês**.

---

### **Resumo Atualizado dos Custos Mensais**

| Serviço                      | Custo Estimado |
|------------------------------|----------------|
| Aurora Serverless            | $750           |
| EC2 Auto Scaling (2-6 nós)   | $122           |
| Elastic Load Balancer (ALB)  | $44            |
| Redis (ElastiCache)          | $38            |
| SQS                          | $1,73          |
| Transferência Interna        | $10            |
| Transferência Saída (1 TB)   | $90            |

**Total Mensal Atualizado**: **$1.055,73/mês**

---
