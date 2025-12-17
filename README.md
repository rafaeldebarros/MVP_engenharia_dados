# MVP_engenharia_dados
Projeto para conclusão de módulo de engenharia de dados da Pós de Ciência de Dados e Analytics da PUC-RIO

Este projeto consiste em um **MVP de engenharia de dados aplicado à análise de dados judiciais do Supremo Tribunal Federal (STF)**, com foco na **tramitação processual**, **tempo de julgamento** e **resultado das decisões finais**, especialmente em processos distribuídos no ano de 2025.

O trabalho foi desenvolvido em ambiente **Databricks**, utilizando **Spark, SQL e PySpark**, com modelagem em camadas (Bronze, Silver e Gold) e geração de visões analíticas para exploração dos dados.

---

## 🎯 Objetivos do Projeto

* Estruturar dados brutos do STF em um **pipeline analítico confiável**;
* Analisar o **tempo de tramitação dos processos**, desde a autuação até a baixa;
* Identificar e mensurar o impacto de **recursos internos**;
* Classificar o **resultado final dos julgamentos** (favorável, desfavorável, etc.);
* Avaliar **tendências decisórias** por:

  * classe processual;
  * órgão julgador;
  * tribunal de origem;
  * temas/assuntos.

---

## 🗂️ Fontes de Dados

Dados públicos do STF, organizados inicialmente em arquivos CSV:

* **`distribuicoes2025stf`**
  Informações sobre a distribuição dos processos (classe, tribunal de origem, datas, etc.).

* **`decisoes2025stf`**
  Informações sobre decisões proferidas (tipo de decisão, relator, andamento, órgão julgador, etc.).

---

## 🏗️ Arquitetura e Modelagem

### Camadas do Projeto

* **Bronze**
  Dados brutos importados dos CSVs.

* **Prata**
  Dados normalizados, com:

  * padronização de tipos;
  * tratamento de valores inválidos;
  * normalização de chaves (`processo`);
  * remoção de inconsistências.

* **Ouro**
  Visões analíticas prontas para consulta e BI.

### Principais Views Criadas

* **`gold.vw_distribuicoes2025_decisoes`**
  Integra distribuição e decisões, calculando:

  * tempo total de tramitação;
  * tempo até a primeira decisão;
  * tempo para julgamento de recurso interno;
  * flags de existência de decisão e recurso.

* **`gold.vw_resultado_decisao_final_2025`**
  Consolida **uma linha por processo**, contendo:

  * decisão final (ou decisão em recurso interno, quando aplicável);
  * relator e órgão julgador da decisão final;
  * tribunal de origem;
  * classificação do resultado do julgamento.

---

## ⚖️ Classificação do Resultado do Julgamento

O resultado final do processo é classificado com base no campo **`andamento_decisao`**, considerando apenas decisões finais ou decisões em recurso interno.

### Categorias adotadas:

* **favorável**
* **desfavorável**
* **sigiloso**
* **homologação**
* **devolução**

A classificação como **favorável** ocorre **exclusivamente** quando o texto do andamento corresponde a rótulos previamente definidos (ex.: *Procedente*, *Conhecido e provido*, *Concedida a ordem*, etc.), evitando inferências genéricas por palavras-chave.

---

## 📊 Principais Análises Realizadas

* Quantidade de processos por classe processual;
* Tempo médio, mediano, mínimo e máximo de tramitação;
* Análise de dispersão (boxplots) do tempo de tramitação;
* Identificação de processos com e sem recurso interno;
* Resultado dos julgamentos por:

  * órgão julgador;
  * tribunal de origem;
  * classe processual;
* Percentual de decisões favoráveis e desfavoráveis por tribunal.



## 👤 Autor

**Rafael Souza de Barros**
Projeto desenvolvido como MVP de engenharia de dados aplicado à análise jurídica.

---


