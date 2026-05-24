# SVDC_TEST
# Análise de Dados COVID-19 

O objetivo deste projeto é analisar a evolução da pandemia de COVID-19 através de um conjunto de dados real, identificando padrões temporais e geográficos, bem como o impacto de fatores como vacinação, testes e capacidade dos sistemas de saúde.

Pretende-se comunicar insights relevantes que possam apoiar a tomada de decisão em contextos de saúde pública.

---

## 🧹 Pré-processamento e Transformação de Dados

Durante a fase de análise exploratória, foram realizadas as seguintes etapas de limpeza e transformação:


### 1. Tratamento de valores nulos

* O ficheiro foi convertido em .xls dado a um erro no tableau quando usei a fonte csv.
* Foram identificadas colunas com valores em falta (null).
* Colunas completamente vazias foram removidas. (Human development index, Weekly icu admissions, Weekly icu admissions per mil)

---

### 2. Filtragem de dados inconsistentes

* Gostaria de ter aplicado filtros para lidar com dados com valores inválidos mas dado o tempo nao o consegui fazer mas da minha avaliação do dataset não me pareceram existir(ex: valores negativos em casos ou mortes).

---

### 3. Criação de métricas derivadas

Foram criadas novas variáveis para permitir uma análise mais aprofundada:

* **Taxa de Mortalidade (Case Fatality Rate)**
  `total_deaths / total_cases`

* **Crescimento de Casos (Growth Rate)**
  `new_cases / valor anterior`

* **Taxa de Vacinação**
  `people_fully_vaccinated / population`

---

## Objetivos da Visualização

As visualizações foram organizadas em dashboards com objetivos específicos, permitindo uma análise estruturada da pandemia.

---

### 1. Executive Summary

**Objetivo:**
Fornecer uma visão geral do estado da pandemia.

**Perguntas:**

* Qual a dimensão global da pandemia?
* Como evoluiu ao longo do tempo?
* Que países foram mais afetados?

**Visualizações:**

* KPIs (casos totais, mortes, vacinação)
* Evolução temporal dos casos
* Mapa global

![Visão Geral da Pandemia](/geral.png)

---

### 2. Evolução Temporal

**Objetivo:**
Analisar o comportamento da pandemia ao longo do tempo.

**Perguntas:**

* Existem padrões ou ondas?
* Quando ocorreram picos de infeção?


**Visualizações:**

* Novos casos ao longo do tempo
* Novas mortes

![Evolução Temporal](/evolucao.png)

---

### 3. Comparação entre Países e Vacinação e Impacto

**Objetivo:**
Identificar diferenças entre países e fatores explicativos.
Avaliar o efeito da vacinação na evolução da pandemia.

**Perguntas:**

* Que países foram mais afetados (relativamente)?
* Existem diferenças na mortalidade?
* O impacto foi uniforme?

* A vacinação contribuiu para reduzir mortes?
* Existe relação entre vacinação e impacto?

**Visualizações:**

* Casos por milhão
* Mortes por milhão
* Taxa de mortalidade

* Evolução da vacinação
* Scatter plot: vacinação vs mortes
* Comparação temporal antes/depois vacinação


![Impacto global](/impacto.png)

---

# SVDC_TEST_AFTER_HOUR

---

## Tratamento de Dados

O processo de preparação dos dados focou-se na limpeza de valores ausentes e na filtragem geográfica para garantir a integridade das análises. 
Este tratamento foi feito através de um notebook usando a biblioteca pandas.
As principais etapas foram:

### 1. Limpeza de Colunas Irrelevantes
*   **Remoção por Ausência de Dados**: Foram eliminadas **13 colunas** que apresentavam mais de **90% de valores em falta**, garantindo que apenas variáveis com relevância estatística fossem mantidas.

### 2. Tratamento de Valores Nulos
*   **Normalização de Métricas Cumulativas**: As colunas fundamentais (`total_cases`, `new_cases`, `total_deaths` e `new_deaths`) tiveram os seus valores nulos preenchidos com **0** para permitir cálculos matemáticos e visualizações contínuas.

### 3. Filtragem Geográfica e de Entidades
*   **Exclusão de Agregados**: Para evitar a duplicidade de dados em análises globais, foram removidas entidades que não representam países individuais, tais como:
    *   **Continentes**: África, Ásia, Europa, etc.
    *   **Indicadores Económicos**: High-income countries, Low-income countries, etc.
    *   **Eventos e Globais**: World, Summer/Winter Olympics.
*   **Identificação**: Foi criada uma nova coluna `is_country` para validar que o dataset final contém apenas registos nacionais.

### 4. Exportação
*   O dataset final tratado foi exportado para o ficheiro `covid_clean.csv`, pronto para ser utilizado em ferramentas de visualização como o Tableau.

---

##  Objetivos da Visualização e Insights

O objetivo principal é analisar a evolução da pandemia de COVID-19, cruzando dados epidemiológicos com indicadores socioeconómicos e demográficos para compreender não apenas a propagação, mas o impacto entre nações.

### Painel 1: Panorama Global e Disparidades Regionais
*   **Mensagem Central**: Identificar quais as nações que sofreram o maior impacto relativo, desmistificando a ideia de que o impacto absoluto (números totais) é o único indicador relevante.
*   **Insights Desejados**:
    *   **Métricas Globais**: Exibição dos totais acumulados para contextualização da escala da pandemia.
    *   **Impacto Proporcional**: Através do gráfico de barras "Países mais afetados proporcionalmente", destacamos nações como San Marino e Chipre, que apresentam uma densidade de casos perante a sua população muito superior à média global.
    *   **Correlação Casos/Mortes**: O gráfico de dispersão (*scatter plot*) permite identificar *outliers* como os Estados Unidos ou o Brasil, analisando a eficiência da resposta sanitária perante o volume de infeções.

![Visão Geral da Pandemia](/geral2.png)

### Painel 2: Dinâmica Temporal e Taxas de Crescimento
*   **Mensagem Central**: Demonstrar a ciclicidade da pandemia e a eficácia das medidas de contenção ao longo do tempo.
*   **Insights Desejados**:
    *   **Evolução de Médias Móveis**: Comparação entre o volume de novos casos e novas mortes (*smoothed*), revelando o desfasamento temporal entre o contágio e a fatalidade.
    *   **Taxa de Crescimento (Growth Rate)**: Visualização da aceleração e desaceleração da pandemia, evidenciando o declínio acentuado após os picos iniciais e a estabilização em 2024-2026.
    *   **Tendência Acumulada**: Gráfico de linhas comparativo que mostra o achatamento das curvas de casos totais versus mortes totais.

![Visão Geral da Pandemia](/evolucao2.png)

### Painel 3: Fatores Socioeconómicos e Demográficos
*   **Mensagem Central**: Analisar se a riqueza de um país (PIB), a idade da sua população ou a sua longevidade (Expectativa de Vida) são preditores diretos da taxa de mortalidade por COVID-19.
*   **Insights Desejados**:
    *   **Relação PIB vs Impacto**: Investigar se países com maior PIB per capita conseguiram mitigar melhor a mortalidade (Total Deaths Per Million).
    *   **Fator Demográfico**: O gráfico "Idade vs Mortes" cruza a média de idade (Median Age) com a taxa de mortalidade, testando a hipótese de que populações mais envelhecidas sofreram um impacto mais severo.
    *   **Saúde Pública**: Através da análise da Expectativa de Vida, identificar correlações entre a infraestrutura de saúde pré-existente e a resiliência à pandemia.

![Visão Geral da Pandemia](/social2.png)

---

## 🚀 Metodologia de Implementação
Para alcançar estes objetivos, o plano de execução envolveu:
1.  **Agregação e Filtragem**: Utilização de filtros de data e país para permitir uma exploração interativa.
2.  **Normalização de Dados**: Foco em métricas "Per Million" para permitir uma comparação justa entre países de diferentes dimensões populacionais.
3.  **Análise de Tendência**: Inclusão de linhas de tendência (regressão linear) nos gráficos de dispersão para identificar rapidamente países que estão acima ou abaixo da média esperada de impacto.

---


