# 🧪 MVP – Engenharia de Dados: Análise da COVID-19

## 📌 Descrição

Neste trabalho, foi construído um pipeline de dados utilizando tecnologias na nuvem (Databricks). O pipeline envolveu as etapas de **busca, coleta, modelagem, carga e análise** de dados públicos sobre **casos, mortes e vacinação** contra a COVID-19 ao redor do mundo. O projeto foi realizado como parte da disciplina de Engenharia de Dados, com foco em práticas de modelagem dimensional e uso de ferramentas de Big Data.

---

## 🎯 Objetivo

### Objetivo Geral

Desenvolver um MVP de engenharia de dados na nuvem, utilizando o **Databricks**, para consolidar e analisar dados sobre a pandemia de **COVID-19**, com foco em compreender os impactos da vacinação na evolução de casos e mortes, através de um modelo de dados analítico em **estrela**.

### Objetivos Específicos

1. Realizar a ingestão, tratamento e transformação dos dados de casos/mortes e vacinação no Databricks.  
2. Modelar os dados utilizando a abordagem dimensional (modelo estrela), com fato e dimensões adequadas.  
3. Integrar as duas bases de dados em um modelo analítico único, utilizando chaves comuns como país e data.  
4. Construir visualizações e análises exploratórias para responder perguntas relevantes sobre a pandemia.  
5. Identificar padrões e possíveis correlações entre o avanço da vacinação e a redução de casos/mortes.  
6. Avaliar a qualidade e integridade dos dados utilizados.  

### Perguntas que se deseja responder

1. Como evoluíram os casos e mortes por COVID-19 ao longo do tempo em diferentes países?  
2. Qual o impacto do avanço da vacinação na redução de casos e mortes por país ou continente?  
3. Quais países apresentaram a maior velocidade de vacinação e como isso refletiu nos indicadores da pandemia?  
4. Existe uma defasagem temporal média entre o início da vacinação e a queda nos números de mortes?  
5. Quais países apresentaram comportamento fora da média (casos/mortes altos mesmo com vacinação avançada)?  
6. Há países onde os dados de vacinação não coincidem com a queda de casos, indicando outros fatores influenciando?  

---

## 🧰 Plataforma

A plataforma escolhida para o desenvolvimento do pipeline foi o **Databricks Community Edition**, por oferecer recursos de processamento distribuído com **Apache Spark** e suporte à arquitetura **Delta Lake** (Bronze, Silver, Gold).  

Apesar das limitações da edição gratuita, foi possível realizar toda a transformação, integração e análise diretamente na nuvem, com persistência de dados e uso de notebooks Python.

---

## 🔍 Detalhamento

### 1. Busca pelos dados

As bases de dados foram escolhidas com base na possibilidade de responder às perguntas analíticas propostas. Os dados são públicos e foram obtidos diretamente da plataforma **Kaggle**:

- [COVID-19 World Vaccination Progress](https://www.kaggle.com/gpreda/covid-world-vaccination-progress)
- [Covid Cases and Deaths WorldWide](https://www.kaggle.com/datasets/sudalairajkumar/novel-corona-virus-2019-dataset)

Estas bases são amplamente utilizadas para estudos sobre a pandemia e não possuem restrições de uso educacional.

---

### 2. Coleta

As bases foram baixadas manualmente da plataforma Kaggle e, em seguida, enviadas para o ambiente de arquivos do **Databricks (DBFS)**. As fontes estão no formato `.csv` e foram lidas diretamente com PySpark para criação de DataFrames.

---

### 3. Modelagem

Foi construído um modelo dimensional utilizando **esquema estrela**, com **tabelas fato e dimensões**.  

As tabelas principais estão na camada **Gold**, resultantes de transformações nas camadas Bronze e Silver.  

#### Catálogo de Dados

##### 🟨 `vacinas_total_por_pais_fabricante`
| Coluna       | Tipo   | Descrição                                |
|--------------|--------|------------------------------------------|
| pais         | string | Nome do país                             |
| fabricante   | string | Nome do fabricante da vacina             |
| total_vacinas| long   | Total de vacinas aplicadas               |

##### 🟨 `casos_mortes_por_pais`
| Coluna       | Tipo   | Descrição                                |
|--------------|--------|------------------------------------------|
| pais         | string | Nome do país                             |
| total_casos  | long   | Total de casos registrados               |
| total_mortes | long   | Total de mortes registradas              |
| population   | long   | População estimada do país               |

##### 🟨 `vacinas_casos_mortes_flat`
| Coluna       | Tipo   | Descrição                                |
|--------------|--------|------------------------------------------|
| pais         | string | Nome do país                             |
| fabricante   | string | Nome do fabricante da vacina             |
| total_vacinas| long   | Total de vacinas aplicadas               |
| total_casos  | long   | Total de casos registrados               |
| total_mortes | long   | Total de mortes registradas              |
| population   | long   | População estimada do país               |

---

### 4. Carga

A carga foi realizada nas seguintes camadas do Delta Lake:

- **Bronze**: ingestão dos arquivos `.csv` originais.  
- **Silver**: limpeza e padronização dos dados (casting de tipos, remoção de nulos, renomeações).  
- **Gold**: agregações, junções e criação de tabelas analíticas conforme o modelo estrela.

Todos os dados foram persistidos como tabelas Delta e podem ser reutilizados em notebooks SQL e Python no Databricks.

---

## 📒 Notebook Principal

O notebook com todo o pipeline (extração, transformação, carga e análise) está disponível na pasta:  
📁 `notebook/`

---

## 📁 Organização do Repositório


---

## 🙋‍♀️ Autoavaliação

Durante a execução do projeto, o objetivo principal foi construir um pipeline de engenharia de dados funcional, aplicar conceitos de transformação com modelo estrela e criar análises de valor com base em dados reais. Com base no trabalho concluído, acredito que os objetivos foram atingidos.
Entre as principais dificuldades enfrentadas, destacam-se:
•	Tratamento de dados inconsistentes: as bases exigiram múltiplas limpezas e conversões, especialmente para garantir integridade nas colunas numéricas.
•	Problemas com schemas e partições: alguns erros no Databricks, como conflitos de schema, exigiram ajustes de configuração e uso de mergeSchema.
•	União de tabelas complexas: por envolver junções de fontes distintas, foi necessário alinhar nome de colunas e formatos para garantir que as queries funcionassem.
Apesar das dificuldades, o aprendizado técnico foi enriquecedor, especialmente no uso do PySpark e do Databricks como ferramenta de processamento em larga escala.

## Trabalhos Futuros
Como sugestões para evolução do projeto:
•	Adicionar a dimensão temporal ao modelo, para análise de tendências mais apuradas.
•	Explorar visualizações mais avançadas, com dashboards interativos em Power BI ou Plotly.
•	Incorporar novas variáveis, como lockdowns, taxa de testagem ou mobilidade populacional.
•	Utilizar machine learning para prever evolução de casos com base no histórico por país.


- **Tratamento de dados inconsistentes**: exigiu limpezas e conversões, especialmente em colunas numéricas. 
