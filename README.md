# Demand-Forecasting-Safety-Stock-Optimization
Este repositório contém uma solução completa (end-to-end) de Supply Chain &amp; Data Science para planejamento de demanda e otimização de níveis de estoque no varejo. A partir de um conjunto de dados históricos contendo 4.565.000 registros de vendas diárias, período de 2019 a 2023, abrangendo 50 lojas e 50 SKUs.

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas">
  <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" alt="NumPy">
  <img src="https://img.shields.io/badge/scikit_learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white" alt="Scikit-Learn">
  <img src="https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white" alt="Git">
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white" alt="Jupyter">
  <img src="https://img.shields.io/badge/Microsoft_Excel-217346?style=for-the-badge&logo=microsoft-excel&logoColor=white" alt="Excel">
</p>

A partir de um conjunto de dados históricos contendo 4.565.000 registros de vendas diárias (período de 2019 a 2023, abrangendo 50 lojas e 50 SKUs), o projeto realiza a classificação estratégica de produtos, calcula os parâmetros dinâmicos de estoques de segurança e projeta a demanda semanal futura para 2024 utilizando modelagem temporal Holt-Winters (Exponential Smoothing).

-  Números-Chave:
  
 * Volume Histórico Vendido: 133.593.174 unidades
   
 * Faturamento Acumulado: R$ 7.107.707.527,76

 * Acurácia Preditiva do Modelo: 92,45% (MAPE de 7,55%)
   
 * Cobertura da Malha: 50 Lojas / 50 SKUs (2.500 combinações loja-produto)
   

 - Principais Funcionalidades:

 * Ingestão e Tratamento de Dados: Processamento de grande volume de vendas, validação de consistência e cálculo de faturamento acumulado por item e loja.
   
 * Classificação Matriz ABC/XYZ: Categorização multinível combinando a relevância financeira do item (ABC) com a volatilidade/previsibilidade da demanda (XYZ).
   
 * Cálculo de Estoque de Segurança (SS) e Ponto de Encomenda (ROP): Formulação estatística para mitigar rupturas de estoque respeitando metas diferenciadas de Nível de Serviço e Lead Times por classe.
   
 * Previsão de Demanda (Demand Forecasting):Treinamento e validação de modelo de séries temporais Holt-Winters Aditivo com componente sazonal de 52 semanas.Projeção para 2024: Geração das metas semanais de suprimento para os 50 SKUs.


   - Matriz ABC/XYZ e Parâmetros de Estoque:
     
    * 1. Critérios de ClassificaçãoCurva ABC (Receita):
         Classe A: SKUs acumulando até 80% do faturamento total.
         Classe B: SKUs acumulando de 80% a 95% do faturamento total.
         Classe C: SKUs acumulando acima de 95% do faturamento total.
         Curva XYZ (Volatilidade da Demanda via Coeficiente de Variação CV = sigma\mu
         Classe X: CV <= 0,20 (Demanda estável e previsível).
         Classe Y: 0,20 < CV <= 0,50 (Demanda com variabilidade moderada).
         Classe Z: CV > 0,50 (Demanda altamente oscilante/esporádica).

         - Categoria,Quantidade de SKUs,% dos SKUs,Característica Principal
           
- AY,33,66%,"Alto faturamento, variabilidade moderada"
 - BY,11,22%,"Médio faturamento, variabilidade moderada"
 - CY,6,12%,"Baixo faturamento, variabilidade moderada"
 - Total,50,100%,50 SKUs
