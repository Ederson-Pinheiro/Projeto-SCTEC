# Projeto-SCTEC
Projeto do primeiro módulo do curso de aprendizado de máquina e visão computacional. Apenas python nativo.

# Projeto de Sanitização e Padronização de Dados da Olist

## Sumário

1. [Descrição do Projeto](#1-descricao-do-projeto)
2. [Objetivos](#2-objetivos)
3. [Metodologia de Sanitização](#3-metodologia-de-sanitizacao)
4. [Guia de Execução](#4-guia-de-execucao)
5. [Reflexão Teórica sobre Machine Learning](#5-reflexao-teorica-sobre-machine-learning)

## 1. Descrição do Projeto

Este projeto foca na tarefa de **sanitização e padronização de dados** provenientes dos datasets `olist_orders_dataset.csv` e `olist_products_dataset.csv` da Olist.

Como uma grande plataforma de e-commerce, a Olist lida diariamente com um grande volume de informações, o que pode gerar inconsistências, valores ausentes e formatos não padronizados. A qualidade desses dados é um fator determinante para a precisão das análises de negócio e para a eficácia de futuros modelos de `Machine Learning`.

O script desenvolvido atua como uma solução para transformar dados brutos em um formato limpo e consistente, pronto para consumo por outras etapas do pipeline de dados ou diretamente por modelos preditivos. Esse processo é essencial para garantir que os insights gerados sejam confiáveis e que os modelos construídos sobre esses dados sejam robustos e justos.

## 2. Objetivos

Os principais objetivos deste projeto são:

* Garantir a integridade dos dados, eliminando inconsistências e registros problemáticos.
* Padronizar formatos para facilitar análises e processamento.
* Tratar dados ausentes de forma consistente.
* Validar regras de negócio relacionadas aos pedidos.
* Preparar os dados para aplicações de `Machine Learning`.
* Gerar um relatório consolidado das correções realizadas.

## 3. Metodologia de Sanitização

### 3.1 Tratamento de Dados Ausentes (Dataset de Produtos)

#### Campo `product_category_name`

Valores vazios são substituídos pela string:

```text
Sem Categoria
```

Essa abordagem garante que todos os produtos possuam uma categorização explícita.

#### Dimensões Físicas

Os seguintes campos são analisados:

* `product_weight_g`
* `product_length_cm`
* `product_height_cm`
* `product_width_cm`

Quando um valor está ausente, ele é substituído pela `média` da respectiva coluna.

Essa estratégia evita a perda de registros e preserva a quantidade de dados disponível para análise.

### 3.2 Padronização de Strings e Regex (Dataset de Produtos)

O campo `product_category_name` passa pelas seguintes transformações:

1. Conversão para letras minúsculas.
2. Remoção de caracteres especiais utilizando expressões regulares (`Regex`).
3. Preservação apenas de letras, números, espaços e `_`.

Exemplo:

```text
BELEZA & SAÚDE!!!
↓
beleza saude
```

Essa padronização facilita agrupamentos, filtros e análises estatísticas.

### 3.3 Validação de Regras de Negócio (Dataset de Pedidos)

#### Validação de Entregas Nulas

É realizada uma verificação na coluna:

```text
order_delivered_customer_date
```

Os pedidos com data de entrega vazia são classificados em dois grupos:

* Status `canceled`
* Status diferente de `canceled`

Essa validação permite identificar possíveis inconsistências operacionais.

### 3.4 Formatação Temporal (DateTime)

A coluna `order_approved_at` é convertida do formato:

```text
%Y-%m-%d %H:%M:%S
```

para:

```text
%d/%m/%Y
```

Exemplo:

```text
2018-05-14 13:22:10
↓
14/05/2018
```

Isso facilita a leitura e a análise temporal dos dados.

### 3.5 Relatório de Status

Ao final da execução é gerado um relatório contendo:

* Total de produtos processados.
* Total de pedidos processados.
* Quantidade de categorias corrigidas.
* Quantidade de dimensões corrigidas.
* Quantidade de pedidos cancelados.
* Quantidade de pedidos com entrega nula e não cancelados.

Exemplo:

```text
===== RELATÓRIO DE SANITIZAÇÃO =====

Produtos processados: 32951
Pedidos processados: 99441

Categorias corrigidas: 610
Dimensões corrigidas: 2488

Pedidos cancelados: 625
Pedidos com entrega nula e não cancelados: 2965

Sanitização concluída com sucesso.
```

## `Tecnologias Utilizadas`

* `Python 3`
* `csv`
* `re`
* `datetime`
* `Google Colab`

## 4. Guia de Execução

### 4.1. Ambiente de Desenvolvimento

O projeto foi desenvolvido e testado utilizando o `Google Colab`.

### 4.2. Bibliotecas Necessárias

O script utiliza apenas bibliotecas nativas do Python:

```python
import csv
import re
from datetime import datetime
```

Nenhuma instalação adicional é necessária.

### 4.3 Download e Configuração dos Datasets(csv)
`Datasets usados`:
* `olist_orders_dataset.csv`
* `olist_products_dataset.csv`

Estrutura utilizada neste projeto:
### 4.4 Utilizando o Google Colab
```text
Faça o upload dos arquivos:

1 - Baixe a base de Dados(.csv): `https://github.com/fiesc-junior-prado/mine_projeto_bloco_1`
por onde eu peguei!
Ou também pode ser encontrado na base pública do Kaggle.

2 - Depois se for usar o google colab, faça o upload para o Google Drive, esse foi caminho usado por mim:
```text
/content/drive/MyDrive/
├── olist_orders_dataset.csv
└── olist_products_dataset.csv
```
### 4.5 Utilizando o VsCode
```text
Faça o upload dos arquivos:

1 - Baixe a base de Dados(.csv): `https://github.com/fiesc-junior-prado/mine_projeto_bloco_1`
por onde eu peguei!
Ou também pode ser encontrado na base pública do Kaggle.

2 - Depois coloque os dois `datasets` na mesma pasta do arquivo principal.
├── main.py
├── olist_orders_dataset.csv
└── olist_products_dataset.csv
```
### 4.6 Abertura do Notebook

Abra o notebook `.ipynb` contendo o código do projeto.

### 4.7 Execução Sequencial

Execute as células na seguinte ordem:

1. Importação das bibliotecas.
2. Leitura dos arquivos.
3. Definição das funções:

   * `tratar_dados_ausentes()`
   * `padronizar_categorias()`
   * `validar_entregas_nulas()`
   * `converter_datas()`
   * `gerar_relatorio()`
4. Execução da célula principal (`MAIN`).

Ao final da execução, o relatório de sanitização será exibido no console.

## 5. Reflexão Teórica sobre Machine Learning

A etapa de limpeza e preparação dos dados é uma das fases mais importantes do desenvolvimento de sistemas de `Machine Learning`. Dados incompletos, inconsistentes ou incorretos podem levar os algoritmos a aprender padrões inexistentes ou irrelevantes, comprometendo sua capacidade de generalização.

Quando informações faltantes não são tratadas adequadamente, o modelo pode associar comportamentos incorretos aos exemplos de treinamento, reduzindo sua precisão em cenários reais.

Além disso, a utilização de dados limpos ajuda a minimizar vieses estatísticos e reduz o risco de `Overfitting`. O `Overfitting` ocorre quando um modelo aprende detalhes específicos ou ruídos presentes no conjunto de treinamento, em vez de capturar padrões verdadeiramente representativos.

Ao aplicar regras consistentes de validação, correção e tratamento de dados ausentes, cria-se uma base mais confiável para o treinamento dos algoritmos, permitindo previsões mais robustas, precisas e justas.
