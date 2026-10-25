# Demand-Forecasting-Safety-Stock-Optimization
Este repositório contém uma solução completa (end-to-end) de Supply Chain &amp; Data Science para planejamento de demanda e otimização de níveis de estoque no varejo. A partir de um conjunto de dados históricos contendo 4.565.000 registros de vendas diárias, período de 2019 a 2023, abrangendo 50 lojas e 50 SKUs.

<h1 align="center">
   Demand Forecasting & Safety Stock Optimization
</h1>

<p align="center">
  <b>Planejamento Preditivo de Demanda, Otimização Estatística de Estoque de Segurança e Matriz Estratégica ABC/XYZ para o Varejo</b>
</p>

<p align="center">
 <p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas">
  <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" alt="NumPy">
  <img src="https://img.shields.io/badge/scikit_learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white" alt="Scikit-Learn">
  <img src="https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white" alt="Git">
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter">
  <img src="https://img.shields.io/badge/Microsoft_Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white" alt="Excel">
</p>

<p align="center">
  <a href="#-visão-geral-e-contexto-de-negócio">Visão Geral</a> •
  <a href="#-indicadores-chave-kpis">KPIs</a> •
  <a href="#-classificação-estratégica-matriz-abcxyz">Matriz ABC/XYZ</a> •
  <a href="#-dimensionamento-de-estoque-ss--rop">Estoque de Segurança</a> •
  <a href="#-modelagem-preditiva-demand-forecasting">Forecasting</a> •
  <a href="#-arquitetura-da-solução-e-pipeline">Arquitetura</a> •
  <a href="#-estrutura-do-repositório">Estrutura</a> •
  <a href="#-como-executar">Como Executar</a> •
  <a href="#-autor">Autor</a>
</p>

---

##  Visão Geral e Contexto de Negócio

No setor varejista multi-loja, o equilíbrio entre **disponibilidade de produto** e **eficiência do capital de giro** é um dos maiores desafios operacionais. A ocorrência de rupturas de estoque (*stockouts*) gera perda direta de receita e insatisfação do cliente, enquanto o excesso de estoque (*overstocking*) imobiliza capital financeiro e aumenta os custos de armazenagem e obsolescência.

Este projeto implementa uma solução **End-to-End de Supply Chain & Data Science** projetada para otimizar o planejamento de vendas e operações (S&OP). Utilizando um histórico transacional de **4.565.000 registros diários** (2019 a 2023) abrangendo 50 lojas e 50 SKUs, a solução realiza:

1. **Categorização Financeira e de Volatilidade**: Classificação dinâmica via Matriz **ABC/XYZ**.
2. **Dimensionamento Estatístico do Buffer**: Cálculo dinâmico do **Estoque de Segurança (SS)** e **Ponto de Encomenda (ROP)** ajustado ao nível de serviço e tempo de ressuprimento (*lead time*).
3. **Projeção de Demanda Futura**: Algoritmo de séries temporais **Holt-Winters (Exponential Smoothing)** com sazonalidade semanal de 52 semanas para prever as vendas de 2024.

---

##  Indicadores Chave (KPIs)

<table align="center">
  <tr>
    <td align="center" width="25%">
      <b>Volume Total Vendido</b><br>
      <sub>(2019 - 2023)</sub><br>
      <h2> 133,59M</h2>
      <sub>Unidades comercializadas</sub>
    </td>
    <td align="center" width="25%">
      <b>Faturamento Acumulado</b><br>
      <sub>(2019 - 2023)</sub><br>
      <h2> R$ 7,107B</h2>
      <sub>Receita bruta total</sub>
    </td>
    <td align="center" width="25%">
      <b>Acurácia Preditiva</b><br>
      <sub>Validação 2023</sub><br>
      <h2> 92,45%</h2>
      <sub>Modelo Holt-Winters</sub>
    </td>
    <td align="center" width="25%">
      <b>Erro Médio (MAPE)</b><br>
      <sub>Desvio Percentual</sub><br>
      <h2> 7,55%</h2>
      <sub>Erro percentual absoluto</sub>
    </td>
  </tr>
</table>

---

##  Classificação Estratégica: Matriz ABC/XYZ

A matriz **ABC/XYZ** cruza a **representatividade no faturamento** com a **predictibilidade/volatilidade da demanda** por SKU.

### 1. Critérios de Corte

* **Curva ABC (Financeiro)**:
  * **Classe A**: Representa até **80%** do faturamento acumulado.
  * **Classe B**: Representa de **80% a 95%** do faturamento acumulado.
  * **Classe C**: Representa os **5% finais** do faturamento acumulado.
* **Curva XYZ (Volatilidade via $CV = \frac{\sigma}{\mu}$)**:
  * **Classe X**: $CV \le 0{,}20$ (Demanda estável e altamente previsível).
  * **Classe Y**: $0{,}20 < CV \le 0{,}50$ (Demanda moderada com sazonalidade).
  * **Classe Z**: $CV > 0{,}50$ (Demanda altamente instável/esporádica).

### 2. Distribuição da Carteira de Produtos

| Categoria | SKUs | % Carteira | Perfil de Negócio e Estratégia de Suprimento |
| :---: | :---: | :---: | :--- |
| **AY** | **33** | **66%** | Alto faturamento, demanda moderada. Exige monitoramento contínuo e alto nível de serviço. |
| **BY** | **11** | **22%** | Médio faturamento. Política de reposição intermediária para otimização de giro. |
| **CY** | **6** | **12%** | Baixo faturamento. Reposição consolidada para evitar acúmulo de capital. |
| **Total** | **50** | **100%** | **50 SKUs ativos na malha** |

---

##  Dimensionamento de Estoque (SS & ROP)

Para mitigar rupturas operacionais sem sobrecarregar os armazéns, foi aplicada a modelagem estatística de **Estoque de Segurança (Safety Stock)** e **Ponto de Encomenda (Reorder Point)**:

### Formulário Estatístico

$$\text{Safety Stock (SS)} = \left\lceil Z \times \sigma_{\text{diário}} \times \sqrt{\text{Lead Time}} \right\rceil$$

$$\text{Reorder Point (ROP)} = \left\lceil (\mu_{\text{diário}} \times \text{Lead Time}) + \text{Safety Stock} \right\rceil$$

Onde:
* $\mu_{\text{diário}}$: Média de vendas diárias do SKU.
* $\sigma_{\text{diário}}$: Desvio padrão das vendas diárias.
* $Z$: Fator de Nível de Serviço (distribuição normal padrão).
* $\text{Lead Time}$: Tempo de atendimento do fornecedor (em dias).

### Parâmetros por Classe de Produto

| Classe | Nível de Serviço Alvo ($NS$) | Fator $Z$ | Lead Time ($LT$) | Diretriz Estratégica |
| :---: | :---: | :---: | :---: | :--- |
| **A** | **95%** | **1,65** | **7 dias** | Proteção prioritária; foco em evitar perda de vendas de alto valor. |
| **B** | **90%** | **1,28** | **10 dias** | Balanceamento entre risco de ruptura e custo de estocagem. |
| **C** | **85%** | **1,04** | **14 dias** | Nível de serviço reduzido; tolerância controlada a pequenas falhas. |

---

##  Modelagem Preditiva (Demand Forecasting)

Para a projeção temporal da demanda semanal, utilizou-se o modelo **Holt-Winters Aditivo** (`ExponentialSmoothing`), ideal para séries com tendência linear e padrões sazonais marcados.

```python 
from statsmodels.tsa.holtwinters import ExponentialSmoothing

# Treinamento do Modelo Holt-Winters
model = ExponentialSmoothing(
    train_data['sales'],
    trend='add',
    seasonal='add',
    seasonal_periods=52
).fit()

# Geração das previsões
forecast = model.forecast(steps=52)

Avaliação de Desempenho (Período de Validação 2023)

- Métrica de Avaliação,Resultado Obtido,Significado Prático
- Acurácia Global,"92,45%",Precisão agregada do modelo sobre a demanda real de 2023.
- MAPE (Mean Absolute Percentage Error),"7,55%",Erro relativo médio extremamente baixo para o varejo.
- MAE (Mean Absolute Error),35.243 un/sem,Desvio absoluto médio na demanda semanal agregada.
- RMSE (Root Mean Squared Error),47.175 un/sem,Penalização ajustada para desvios mais acentuados.

Arquitetura da Solução e Pipeline
flowchart LR
    A[ Base de Dados Bruta \ retail_sales.csv] --> B[ Processamento & Agregação \ Pandas / Python]
    B --> C[ Classificação Matriz \ ABC / XYZ]
    B --> D[ Cálculo de SS & ROP \ Estoque de Segurança]
    B --> E[ Modelagem Holt-Winters \ Statsmodels]
    C --> F[ abc_xyz_classification.csv]
    D --> G[ dim_products_inventory.csv]
    E --> H[ fact_weekly_demand_forecast.csv]
    F --> I[ Dashboard Interativo Excel]
    G --> I
    H --> I
```
Estrutura do Repositório

```
Demand-Forecasting-Safety-Stock/
├── 01_data/
│   ├── raw/
│   │   └── retail_sales.csv                         # Dado bruto (4.56M linhas)
│   └── processed/
│       ├── abc_xyz_classification.csv               # Tabela de curva ABC/XYZ por SKU
│       ├── dim_products_inventory.csv               # Dimensão de produto com SS e ROP
│       └── fact_weekly_demand_forecast.csv          # Tabela de fatos com projeções 2024
│
├── 02_notebooks/
│   ├── 01_eda_abc_xyz.ipynb                         # EDA e Curva ABC/XYZ
│   └── 02_forecasting_safety_stock.ipynb           # Modelagem temporal e parâmetros de estoque
│
├── 03_src/
│   ├── data_processing.py                           # Scripts de ingestão e limpeza
│   ├── inventory.py                                 # Módulo de cálculo de SS e ROP
│   └── forecasting.py                               # Módulo do algoritmo Holt-Winters
│
└── 04_reports_dashboard/
    ├── media/
    │   ├── Validacao_modelo_previsao_e_demanda.png  # Gráfico da validação de demanda
    │   ├── matriz_abc_xyz_distribuicao.png          # Visualização da matriz ABC/XYZ
    │   └── dashboard_overview.png                   # Capturas de tela do painel
    ├── dashboard_demand_safety_stock.pbix           # Relatório executivo interativo
    └── README.md                                    # Documentação do projeto

```

##  Dashboard Interativo S&OP

O painel analítico permite o controle estratégico dos indicadores de inventário e planejamento preditivo por meio de visões consolidadas e segmentadas por curva de produto.

### 1. Visão Geral Consolidada
Apresenta o balanço geral do portfólio, integrando a receita acumulada por categoria da matriz ABC/XYZ, a tendência sazonal de vendas projetadas para 2024 e o comparativo dos parâmetros de estoque (SS, ROP e Lead Time).

<p align="center">
  <img src="https://github.com/user-attachments/assets/6c8595e2-ecc1-4e39-bc2e-5ba5feb0e73e" alt="S&OP & Supply Chain Analytics Dashboard - Visão Geral" width="95%">
  <br>
  <em>Figura 1: Visão Geral do S&OP & Supply Chain Analytics Dashboard apresentando KPIs, Matriz ABC/XYZ, Curva Preditiva para 2024 e Níveis de Estoque por Classe.</em>
</p>

---

### 2. Análise Detalhada por Categoria de Produto (Drill-down)

#### 🔹 Classe A / Categoria AY (Produtos de Altíssimo Faturamento)
Composta por **33 SKUs** que concentram **R$ 5,68 Bilhões** (80% da receita total). Exigem nível de serviço rigoroso de **95%**, mantendo um Estoque de Segurança médio de **52 unidades** e Ponto de Encomenda de **302 unidades** para cobrir um Lead Time curto de **7 dias**.

<p align="center">
  <img src="https://github.com/user-attachments/assets/cd255624-48f6-4cef-a382-e3ee79b13c71" alt="Dashboard Filtrado na Categoria AY (Classe A)" width="90%">
  <br>
  <em>Figura 2: Filtro analítico aplicado aos SKUs Classe AY (Alto Faturamento / Variabilidade Moderada).</em>
</p>

---

#### 🔹 Classes B e C / Categorias BY e CY (Médio e Baixo Impacto)
Compreendem os **17 SKUs** restantes (11 BY e 6 CY) responsáveis por **R$ 1,42 Bilhão** em vendas. Com Lead Times maiores (10 e 14 dias), os ROPs médios são ajustados para **215 unidades (Classe B)** e **217 unidades (Classe C)**, otimizando o custo de estocagem sem comprometer a demanda secundária.

<p align="center">
  <img src="https://github.com/user-attachments/assets/d3d17057-7de1-47fd-aa09-998313a9843c" alt="Dashboard Filtrado nas Categorias BY e CY (Classes B e C)" width="90%">
  <br>
  <em>Figura 3: Filtro analítico aplicado aos SKUs das Categorias BY e CY (Médio e Baixo Impacto Financeiro).</em>
</p>

---

##  Modelagem de Dados Relacional

A arquitetura do modelo de dados adota uma estrutura otimizada conectando tabelas dimensionais e de fatos operacionais:

<p align="center">
  <img src="https://github.com/user-attachments/assets/5a0ffc1b-d451-4e2d-9941-b1939ae73056" alt="Esquema Relacional do Modelo de Dados" width="85%">
  <br>
  <em>Figura 4: Diagrama de Relacionamentos entre as tabelas `abc_xyz_classification`, `dim_products_inventory` e a fato de previsão temporal `fact_weekly_demand_forecast`.</em>
</p>


Como Executar
1. Clonar o Repositório
git clone [https://github.com/LuisAvila-MPY/Demand-Forecasting-Safety-Stock.git](https://github.com/LuisAvila-MPY/Demand-Forecasting-Safety-Stock.git)
cd Demand-Forecasting-Safety-Stock

2. Configurar o Ambiente Virtual
- Criar o ambiente
python -m venv venv

- Ativar no Linux/macOS:
source venv/bin/activate

- Ativar no Windows:
venv\Scripts\activate

3. Instalar as Dependências
pip install -r requirements.txt

4. Executar os Scripts da Aplicação
- Executa a limpeza de dados e gera a classificação ABC/XYZ
python 03_src/data_processing.py

- Calcula o Estoque de Segurança e ROP
python 03_src/inventory.py

- Treina o modelo Holt-Winters e exporta as previsões de 2024
python 03_src/forecasting.py


Autor
  - Luis Gustavo Ávila da Silva

[Linkedin] https://www.linkedin.com/in/luis-%C3%A1vila-25687134b/ | [Github] LuisAvila-MPY
 | [Currículo Lattes](https://lattes.cnpq.br/9470197971678821)
