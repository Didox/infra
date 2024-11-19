# Custo pagasó sistema atual na AWS

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

