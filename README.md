
# Telecom X — Análise de Churn (ETL + EDA)

## 📌 Visão geral

Este projeto foi desenvolvido como parte de um desafio de **ETL e análise exploratória de dados (EDA)** com o objetivo de investigar **evasão de clientes (Churn)** em uma empresa fictícia de telecomunicações chamada **Telecom X**.

O foco do projeto é:

- Extrair dados a partir de uma **API/arquivo JSON hospedado no GitHub**
- Realizar **processos de transformação e limpeza (ETL)**
- Conduzir uma **análise exploratória de dados (EDA)** para identificar padrões relacionados ao churn
- Gerar **insights e recomendações para reduzir a evasão de clientes**

---

# 🎯 Objetivo do projeto

O objetivo principal é **identificar padrões comportamentais e características dos clientes que estão associadas ao churn**, permitindo que a empresa possa:

- antecipar cancelamentos
- entender perfis de clientes com maior risco de evasão
- criar estratégias para retenção

---

# 🧱 Estrutura do projeto

O projeto foi estruturado seguindo o fluxo clássico de **ETL + Análise de Dados**.

ETL
 ├── Extração
 ├── Transformação
 └── Preparação da base analítica

EDA
 ├── Análise exploratória
 ├── Visualização de dados
 └── Identificação de padrões de churn

---

# 🔧 Tecnologias utilizadas

Principais bibliotecas utilizadas no projeto:

- Python
- Pandas → manipulação e transformação de dados
- NumPy → suporte para operações numéricas
- Requests → extração de dados via API
- Matplotlib → visualizações
- Seaborn → visualizações estatísticas

---

# 📥 Extração dos dados

A extração dos dados foi realizada a partir de um **JSON hospedado em um repositório GitHub**.

### Estratégia adotada

A abordagem escolhida foi consumir diretamente a URL da API utilizando `requests`.

Passos realizados:

1. Requisição HTTP para o endpoint
2. Validação da resposta (`status code`)
3. Conversão da resposta JSON para estrutura Python
4. Normalização dos dados com `pandas.json_normalize`

Essa estratégia possui vantagens importantes:

- Processo reproduzível
- Facilita automação
- Evita downloads manuais
- Estrutura o processo como um ETL profissional

---

# 🔍 Diagnóstico inicial dos dados

Antes de qualquer transformação, foi realizada uma inspeção da base utilizando:

- `head()`
- `info()`
- `describe()`
- `value_counts()`
- verificação de duplicidade
- análise de valores ausentes

Esse passo é essencial para **entender a qualidade dos dados antes de aplicar correções**.

---

# 🧹 Tratamento de dados (Transformação)

Durante a etapa de transformação foram realizadas diversas tratativas.

## 1️⃣ Identificação de valores ausentes lógicos

Nem todo valor ausente aparece como `NaN`. Em bases reais podem existir valores como:

- `""`
- `" "`
- `"null"`
- `"none"`
- `"n/a"`

Para tratar esses casos foi criada uma função auxiliar:

`mask_ausente_logico(df, coluna)`

Essa função detecta valores ausentes considerando:

- `NaN`
- strings vazias
- textos que representam ausência

Essa abordagem torna o tratamento **reutilizável e robusto**.

---

## 2️⃣ Conversão da coluna `account_Charges_Total`

A coluna `account_Charges_Total` estava originalmente como **string**, o que impedia análises numéricas.

Problemas encontrados:

- valores vazios
- tipo de dado incorreto

### Investigação realizada

Foi identificado que:

- **11 registros estavam vazios**
- todos esses registros tinham **tenure = 0**

Isso significa que o cliente **ainda não acumulou cobranças**, o que é coerente com a regra de negócio.

### Tratamento aplicado

1. Conversão segura usando:

`pd.to_numeric(errors="coerce")`

2. Valores inválidos convertidos para `NaN`
3. `NaN` preenchidos com **0**, pois representam clientes recém cadastrados.

---

## 3️⃣ Tratamento da coluna `Churn`

Foram identificados **224 registros com churn vazio**.

Como o objetivo do projeto é analisar churn (variável alvo), esses registros **não possuem rótulo válido**.

### Decisão adotada

Esses registros **não foram utilizados na análise comparativa**, para evitar distorção dos resultados.

Isso é uma prática comum em **modelagem supervisionada**.

---

# 📊 Análise exploratória de dados (EDA)

Após a limpeza da base, foi realizada uma análise exploratória para investigar fatores relacionados ao churn.

Algumas das variáveis analisadas:

- tempo de contrato (`tenure`)
- tipo de contrato (`Contract`)
- tipo de internet (`InternetService`)
- valor mensal (`MonthlyCharges`)
- suporte técnico (`TechSupport`)
- perfil demográfico (`SeniorCitizen`)

Foram utilizados gráficos como:

- distribuição de churn
- churn por tipo de contrato
- churn por serviço contratado
- churn por faixa de tempo como cliente

---

# 🔎 Principais padrões observados

A análise revelou alguns padrões relevantes.

### Contratos mensais apresentam maior churn

Clientes com contrato **Month-to-month** apresentam maior probabilidade de cancelamento.

Isso sugere que contratos mais longos aumentam retenção.

---

### Clientes com menor tempo de casa cancelam mais

Clientes com **baixo tenure** possuem maior taxa de evasão.

Isso indica que os primeiros meses são críticos para retenção.

---

### Serviços adicionais influenciam retenção

Clientes que possuem:

- suporte técnico
- backup online
- proteção de dispositivo

tendem a cancelar menos.

---

# 💡 Recomendações para redução de churn

Com base na análise realizada, algumas estratégias podem ser sugeridas:

### Incentivar contratos de longo prazo

Oferecer benefícios para clientes migrarem de contratos mensais para contratos anuais.

---

### Programas de retenção para novos clientes

Criar ações específicas nos primeiros meses de relacionamento.

---

### Ofertas de serviços adicionais

Pacotes com:

- suporte técnico
- proteção de dispositivos
- backup

podem aumentar fidelização.

---

# ▶️ Como executar o projeto

1️⃣ Clone o repositório

git clone <repo>

2️⃣ Instale as dependências

pip install pandas numpy matplotlib seaborn requests

3️⃣ Execute o notebook

jupyter notebook

Abra o arquivo:

challenge_alura_ajustado.ipynb

---

# 📂 Estrutura do repositório

telecom-churn-analysis
│
├── challenge_alura_ajustado.ipynb
├── challenge_alura_ajustado.py
├── README.md
└── data/

---

# 👨‍💻 Autor

Projeto desenvolvido como parte de um desafio de **ETL e Análise de Dados em Python**.
