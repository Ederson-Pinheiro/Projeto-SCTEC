# Projeto-SCTEC
Projeto do primeiro módulo do curso de aprendizado de máquina e visão computacional. Apenas python nativo.

# Projeto de Sanitização e Padronização de Dados da Olist

## Sumário

1.  [Descrição do Projeto](#descricao-do-projeto)
2.  [Objetivos](#objetivos)
3.  [Metodologia de Sanitização](#metodologia-de-sanitizacao)
4.  [Guia de Execução](#guia-de-execucao)
5.  [Reflexão Teórica sobre Machine Learning](#reflexao-teorica-sobre-machine-learning)

## 1. Descrição do Projeto

Este projeto foca na crucial tarefa de **sanitização e padronização de dados** provenientes dos datasets de pedidos (`olist_orders_dataset.csv`) e produtos (`olist_products_dataset.csv`) da Olist. A Olist, como uma grande plataforma de e-commerce, lida diariamente com um volume massivo de informações, o que naturalmente pode levar a inconsistências, valores ausentes e formatos não uniformes nos dados coletados. A qualidade desses dados é um fator determinante para a precisão de análises de negócio e a eficácia de futuros modelos de Machine Learning.

O roteiro(script) desenvolvido atua como uma solução robusta para enfrentar esses desafios. Ele emprega uma série de técnicas de pré-processamento para transformar dados brutos em um formato limpo e consistente, pronto para consumo por outras etapas do pipeline de dados ou diretamente por modelos preditivos. Este processo é essencial para garantir que qualquer insight gerado a partir desses dados seja confiável e que os modelos de IA construídos sobre eles sejam robustos e justos.

## 2. Objetivos

Os principais objetivos deste projeto são:

*   **Garantir a integridade dos dados**: Assegurar que os datasets estejam livres de erros, inconsistências e duplicações que possam comprometer a qualidade das análises.
*   **Padronizar formatos**: Uniformizar a representação de informações, como categorias de produtos e datas, para facilitar o processamento e a interpretação.
*   **Tratar dados ausentes**: Implementar estratégias para lidar com valores faltantes de maneira inteligente, evitando perdas de informação ou introdução de viés.
*   **Validar regras de negócio**: Aplicar lógicas específicas da Olist para verificar a coerência dos dados, como a validação de entregas nulas versus status de pedidos.
*   **Preparar os dados para Machine Learning**: Criar uma base de dados limpa e estruturada, otimizada para o treinamento e avaliação de modelos de Inteligência Artificial, minimizando problemas como overfitting e viés.
*   **Gerar um relatório consolidado**: Fornecer um resumo claro das ações de sanitização realizadas, quantificando as correções e confirmando a qualidade da base final.

## 3. Metodologia de Sanitização

A metodologia aplicada neste projeto abrange as seguintes etapas principais:

### 3.1. Tratamento de Dados Ausentes (Dataset de Produtos)

*   **`product_category_name`**: Valores vazios são substituídos pela string "Sem Categoria" para garantir que todos os produtos tenham uma categorização explícita.
*   **Dimensões Físicas**: Campos como `product_weight_g`, `product_length_cm`, `product_height_cm` e `product_width_cm` que possuem valores ausentes são preenchidos com a média dos valores existentes para cada respectiva coluna. Isso evita a perda de registros e fornece uma estimativa razoável para os dados faltantes.

### 3.2. Padronização de Strings e Regex (Dataset de Produtos)

*   **`product_category_name`**: As strings das categorias são convertidas para minúsculas e quaisquer caracteres especiais (que não sejam letras, números, espaços ou `_`) são removidos utilizando expressões regulares (regex). Isso garante uma uniformidade nas categorias, facilitando agrupamentos e análises.

### 3.3. Lógica de Regra de Negócio (Filtros e Validação - Dataset de Pedidos)

*   **Validação de Entregas Nulas**: Uma verificação é realizada para identificar pedidos onde a data de entrega ao cliente (`order_delivered_customer_date`) está nula. Esses casos são categorizados entre pedidos `canceled` e `não canceled`, permitindo identificar e quantificar potenciais inconsistências onde uma entrega não foi registrada, mas o pedido não foi formalmente cancelado.

### 3.4. Formatação Temporal (DateTime - Dataset de Pedidos)

*   **`order_approved_at`**: A coluna `order_approved_at` (data de aprovação do pedido) é convertida do formato original (`%Y-%m-%d %H:%M:%S`) para um formato mais amigável e consistente (`%d/%m/%Y`). Isso padroniza as datas para análises temporais e exibição.

### 3.5. Relatório de Status Manual

*   Ao final do processo, um relatório consolidado é gerado, apresentando o total de produtos e pedidos processados, o número de categorias e dimensões corrigidas, e a quantidade de pedidos cancelados. Este relatório serve como um sumário executivo da sanitização, confirmando a conclusão bem-sucedida do tratamento de dados.

## 4. Guia de Execução

Para executar o código e replicar o processo de sanitização dos dados, siga os passos abaixo:

1.  **Ambiente de Desenvolvimento**: Este projeto foi desenvolvido e testado em um ambiente Google Colab. Certifique-se de ter acesso a uma instância de notebook Colab.
2.  **Bibliotecas Necessárias**: O script utiliza as bibliotecas `csv`, `re` e `datetime`, que são parte da biblioteca padrão do Python e não exigem instalação adicional.
3.  **Preparação dos Dados**: Faça o upload dos arquivos `olist_orders_dataset.csv` e `olist_products_dataset.csv` para o diretório `/content/drive/MyDrive/` em seu Google Drive. Se os arquivos estiverem em outro local, certifique-se de ajustar os caminhos de arquivo nas células que realizam a abertura (`with open(...)`) dos datasets.
4.  **Abertura do Notebook**: Abra o arquivo `.ipynb` (notebook Jupyter/Colab) que contém o código deste projeto.
5.  **Execução Sequencial**: O notebook está estruturado em seções lógicas (`IMPORTAÇÃO`, `ABRINDO ARQUIVOS`, `VALIDAÇÃO E TRATAMENTOS DE DADOS AUSENTES`, etc.). É crucial executar as células **sequencialmente**, da primeira à última, para garantir que as funções sejam definidas e os dados sejam processados na ordem correta.
    *   **Células de Definição**: As primeiras células de código definem as funções de tratamento de dados (`tratar_dados_ausentes`, `padronizar_categorias`, `validar_entregas_nulas`, `converter_datas`, `gerar_relatorio`).
    *   **Célula `MAIN`**: A célula marcada como `MAIN` orquestra a chamada de todas as funções de tratamento e, por fim, gera o relatório final.

Ao final da execução da célula `MAIN`, um relatório detalhado será exibido no console, confirmando as ações de sanitização e a qualidade da base de dados resultante.

## Reflexão Teórica sobre Machine Learning

A etapa de limpeza e preparação dos dados é uma das fases mais importantes do desenvolvimento de sistemas de Machine Learning. Dados incompletos, inconsistentes ou incorretos podem induzir os algoritmos a aprender padrões inexistentes ou irrelevantes, comprometendo a capacidade de generalização dos modelos. Quando informações faltantes não são tratadas adequadamente, o modelo pode associar comportamentos errados aos exemplos de treinamento, reduzindo sua precisão em cenários reais.

Além disso, a utilização de dados limpos ajuda a minimizar vieses estatísticos e reduz o risco de overfitting. O overfitting ocorre quando um modelo aprende detalhes específicos ou ruídos presentes no conjunto de treinamento em vez de capturar padrões verdadeiramente representativos. Ao aplicar regras consistentes de validação, correção e tratamento de dados ausentes, cria-se uma base mais confiável para o treinamento dos algoritmos, permitindo que eles produzam previsões mais robustas, precisas e justas.
