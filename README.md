# 📊 Projeto de Pipeline de Dados: Análise de Suicídios no Brasil

Este projeto foi desenvolvido como parte de um trabalho acadêmico com o objetivo de construir um pipeline de dados completo na nuvem utilizando a plataforma Databricks Community Edition. O foco é a análise de dados públicos de óbitos por suicídio no Brasil, com base nos registros do SIM (Sistema de Informação sobre Mortalidade) do OpenDataSUS.

## 🎯 1. Objetivo

Investigar padrões de suicídio no Brasil com base em atributos geográficos, temporais e sociodemográficos, respondendo às seguintes perguntas:

- Quais estados possuem maior incidência de suicídios?
- Existe diferença significativa entre os sexos?
- Qual faixa etária é mais acometida?
- Escolaridade ou estado civil influenciam nos casos?
- Há uma distribuição por horário?
- Onde ocorrem com mais frequência (domicílio, hospital etc)?

## 🌐 2. Coleta dos Dados

Os dados foram obtidos a partir do OpenDataSUS:

- DO21OPEN.csv
- DO22OPEN.csv
- DO23OPEN.csv

Os arquivos foram enviados para o armazenamento da plataforma Databricks (/FileStore/tables) e tratados via notebooks em PySpark/SQL.

## 🧱 3. Modelagem

Adotou-se o modelo Esquema Estrela com a seguinte estrutura:

### 📌 Tabela fato:
- FATO_SUICIDIOS: contém os registros de óbitos classificados com CID-10 entre X60 e X84 (suicídio).

### 📌 Tabelas dimensão:
- DIM_SEXO
- DIM_RACACOR
- DIM_ESTCIV
- DIM_ESC
- DIM_LOCOCOR
- DIM_ASSISTMED
- DIM_UF

## 🔄 4. Pipeline de Carga

A carga foi realizada com:

- Leitura de arquivos CSV via spark.read.csv
- Padronização e unificação de colunas
- Conversão de campos como data (DTOBITO → DATA_OBITO) e hora
- Criação de colunas calculadas (FAIXA_ETARIA, ANO, MES)
- Armazenamento em tabelas persistentes com saveAsTable

## 📈 5. Análise e Visualização

### a. Qualidade dos dados:
- Presença de dados ausentes e valores inválidos em colunas como ESC, ESTCIV, SEXO.
- Aplicação de tratamentos com valores null ou códigos NA.
- Converçao de strings para dates e times

### b. DMs Criadas:
- DM_TEMPO: volume por ano, mês e dia da semana
- DM_UF: cruzamento de UF × sexo × raça/cor
- DM_FAIXA_ETARIA: faixa etária × sexo × local
- DM_ESCOLARIDADE_ESTCIV: escolaridade × estado civil
- DM_HORA: distribuição por hora do óbito





