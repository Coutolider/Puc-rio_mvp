# 📊 Projeto de Pipeline de Dados: Análise de Suicídios no Brasil

Este projeto foi desenvolvido como parte de um trabalho acadêmico com o objetivo de construir um pipeline de dados completo na nuvem utilizando a plataforma Databricks Community Edition. O foco é a análise de dados públicos de óbitos por suicídio no Brasil, com base nos registros do SIM (Sistema de Informação sobre Mortalidade) do OpenDataSUS.

## 🎯 1. Objetivo

Investigar padrões de suicídio no Brasil com base em atributos geográficos, temporais e sociodemográficos, respondendo às seguintes perguntas:

- Quais estados possuem maior incidência de suicídios?
- Existe diferença significativa entre os sexos?
- Qual faixa etária é mais acometida?
- Escolaridade ou estado civil influenciam nos casos?
- Há uma distribuição por horário?

## 🌐 2. Coleta dos Dados

Os dados foram obtidos a partir do OpenDataSUS: (https://opendatasus.saude.gov.br/dataset/sim)

- DO21OPEN.csv
- DO22OPEN.csv
- DO23OPEN.csv

Os arquivos foram enviados para o armazenamento da plataforma Databricks (/FileStore/tables) e tratados via notebooks em PySpark/SQL.



## 🧱 3. Modelagem

Adotou-se o modelo Esquema Estrela com a seguinte estrutura: 
![image](https://github.com/user-attachments/assets/54185654-7019-43a6-b19d-00158a0efa08)

## 🔄  Pipeline de Carga

A carga foi realizada com:

- Leitura de arquivos CSV via spark.read.csv
- Padronização e unificação de colunas
- Conversão de campos como data (DTOBITO → DATA_OBITO) e hora
- Criação de colunas calculadas (FAIXA_ETARIA, ANO, MES)
- Armazenamento em tabelas persistentes com saveAsTable

### 📌 Tabela fato:
- FATO_SUICIDIOS: contém os registros de óbitos classificados com CID-10 entre X60 e X84 (suicídio).

codigo paracriacao da fato : 
![image](https://github.com/user-attachments/assets/87eec4b7-856d-46c2-ba76-8b7d799606a7)

![image](https://github.com/user-attachments/assets/d9a2ce28-caf6-436e-8712-361724506d8f)



### 📌 Tabelas dimensão:
- DIM_SEXO
- DIM_RACACOR
- DIM_ESTCIV
- DIM_ESC
- DIM_LOCOCOR
- DIM_ASSISTMED
- DIM_UF

para as tabelas foi usado os codigos presente no metadata de estrutura do arquivo disponibilizado pelo data sus no link https://opendatasus.saude.gov.br/dataset/sim/resource/b894426e-83dc-4703-91f8-fe90d9b7f8f0
para a tabela de uf foi utilizado a referencia do ibge para os codigos de municipio https://www.ibge.gov.br/explica/codigos-dos-municipios.php

codigo de criaçao das dim :
![image](https://github.com/user-attachments/assets/1a3db020-a8ed-4887-b57c-2d62029b5b58)
![image](https://github.com/user-attachments/assets/efeca753-2317-4c3d-8a9d-5d916055cc75)




## 📈 4. Análise e Visualização

### a. Qualidade dos dados:
- Presença de dados ausentes e valores inválidos em colunas como ESC, ESTCIV, SEXO.
- Aplicação de tratamentos com valores null ou códigos NA.
- Converçao de strings para o formato de date e time

### b. DMs Criadas:
Foram criados para visualizaçoes datamart com os campos calculados pelas dimençoes , e elas foram:
- DM_TEMPO: volume por ano, mês e dia da semana
- DM_UF: cruzamento de UF × sexo × raça/cor
- DM_FAIXA_ETARIA: faixa etária × sexo × local
- DM_ESCOLARIDADE_ESTCIV: escolaridade × estado civil
- DM_HORA: distribuição por hora do óbito

![image](https://github.com/user-attachments/assets/3f4502cf-7086-4861-9327-283437830b9d)





## 5. Respondendo ao Objetivo: Solução do Problema

Com base nas DMs criadas a partir do modelo dimensional, foi possível obter diversos insights relevantes para a compreensão do comportamento dos suicídios no Brasil. Abaixo, destacam-se as principais descobertas:

---

**A. Distribuição por Estado**  
Estados das regiões Sul e Sudeste concentram a maior parte dos óbitos, o que também reflete a densidade populacional mais elevada dessas regiões.  
![Mapa de óbitos por UF](https://github.com/user-attachments/assets/f82774ca-cb98-4b77-b3b6-626acca4d7fd)

---

**B. Evolução Temporal**  
Observa-se um crescimento no volume de casos nos últimos anos. No entanto, não há uma tendência clara de concentração ao longo dos meses do ano.  
![Evolução temporal](https://github.com/user-attachments/assets/44674ed3-c246-45a5-ac58-56fbad0a8a6d)

---

**C. Diferença por Sexo**  
Homens apresentam maior incidência de suicídio em todos os estados analisados, evidenciando um padrão nacional.  
![Distribuição por sexo](https://github.com/user-attachments/assets/a8227994-ba82-4f40-8f9d-2d907a0193f7)

---

**D. Distribuição por Horário**  
As ocorrências tendem a se concentrar no período da manhã, mas estão distribuídas ao longo do dia, com uma redução significativa durante a madrugada.  
![Horário dos óbitos](https://github.com/user-attachments/assets/95f52e49-e38b-49e4-b426-badf645ff056)

---

**E. Faixa Etária e Sexo**  
A faixa etária com maior incidência varia entre 20 e 40 anos, com predominância no sexo masculino.  
![Faixa etária e sexo](https://github.com/user-attachments/assets/fadbd5a9-c50a-4bc6-9852-b3d6709ef5a7)

---

**F. Escolaridade e Estado Civil**  
Essas variáveis demonstram influência parcial no perfil das vítimas, embora com padrões menos acentuados que os anteriores.  
![Escolaridade e estado civil](https://github.com/user-attachments/assets/d121c27f-9ee7-425a-a054-434f5760b4a0)


## 6. Autoavaliação

Consegui atingir todos os objetivos propostos inicialmente, construindo um pipeline de dados funcional, bem estruturado e escalável. A solução permitiu não apenas a ingestão e tratamento dos dados, mas também a modelagem em um esquema estrela, a criação de Data Marts (DMs) e a geração de visualizações que responderam diretamente às perguntas definidas no início do projeto.

Como próximos passos e oportunidades de melhoria, destaco:

- **Aprofundamento nas análises estatísticas**, incluindo o cálculo de desvio padrão, taxas percentuais, indicadores de crescimento e variações por período.
- **Cruzamento com bases externas**, como dados do IBGE (ex: concentração de renda por município e população por UF), o que possibilitaria gerar indicadores mais robustos como a taxa de incidência por 100 mil habitantes.
- **Otimizações na modelagem**, por meio da criação de índices ou materializações nas DMs, a fim de melhorar a performance em consultas de larga escala.
- **Aprimoramento da entrega visual**, com a publicação de dashboards online contendo filtros interativos para análises dinâmicas por perfil, região ou período.

A experiência com o Databricks foi extremamente positiva, principalmente pela integração entre desenvolvimento, persistência e visualização dos dados em um único ambiente.








