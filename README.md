# Classificação de Risco dos Países com Machine Learning

Este repositório apresenta um estudo aplicado de **classificação de risco soberano de países**, utilizando técnicas de *Machine Learning* supervisionado. O objetivo é investigar se indicadores macroeconômicos e institucionais permitem reproduzir, de forma aproximada, as classificações de risco atribuídas por agências especializadas.

---

## 📌 Motivação

As agências de classificação de risco (rating agencies) desempenham papel central no mercado financeiro ao avaliar a capacidade de países e empresas honrarem suas dívidas. Suas classificações — que variam de **AAA (risco mínimo)** até **D (inadimplência)** — influenciam diretamente decisões de investimento, taxas de juros e fluxos internacionais de capital.

Dado o avanço das técnicas de *Machine Learning*, surge a seguinte questão:

> **É possível, a partir de indicadores econômicos e institucionais públicos, treinar modelos capazes de classificar países de forma semelhante às agências de rating?**

Este projeto busca responder a essa pergunta de forma experimental e exploratória.

---

## 📊 Base de Dados

### 🌍 Indicadores dos Países

Os dados explicativos (features) foram obtidos a partir do **World Development Indicators (WDI)**, do *World Bank*. A base contém centenas de indicadores socioeconômicos, institucionais e financeiros.

Como nem todos os indicadores são relevantes para avaliação de risco soberano, foi realizada uma **seleção manual de atributos**, baseada em critérios econômicos e institucionais. Exemplos de indicadores utilizados:

- Estimativa de controle da corrupção
- Efetividade do governo
- Valor presente da dívida externa
- Indicadores fiscais e macroeconômicos

A lista completa encontra-se no arquivo:

```
atributos.xlsx
```

---

### 🏦 Classificações de Risco (Variável Alvo)

As classificações de risco soberano foram extraídas da **Standard & Poor’s (S&P)** para dois anos:

- **2019**
- **2021**

Arquivos disponíveis:

```
paises_2019_rating.xlsx
paises_2021_rating.xlsx
```

#### 🔄 Redução do Número de Classes

Como a S&P utiliza muitas categorias (AAA, AA+, AA, AA-, A+, ...), as classificações foram **agrupadas em 3 classes**, com o objetivo de tornar o problema mais estável estatisticamente:

- **Classe 1** – Melhor qualidade de crédito
- **Classe 2** – Qualidade intermediária
- **Classe 3** – Maior risco relativo

Essas classes podem ser encontradas no arquivo: 

```
classes.png
```

A distribuição final das classes ficou aproximadamente balanceada:

- Classe 1: ~40%
- Classe 2: ~31%
- Classe 3: ~29%

---

## 🛠️ Tratamento e Preparação dos Dados

O processo de construção do *dataset* final seguiu as etapas abaixo:

1. Seleção apenas dos países presentes nas classificações de 2019 **e** 2021
2. Construção de um *DataFrame* no formato:
   - **Linhas:** país-ano (2019 e 2021)
   - **Colunas:** indicadores selecionados
3. Tratamento de valores ausentes (*missing values*):
   - Remoção de linhas com mais de 10 valores ausentes
   - Remoção de colunas com mais de 10 valores ausentes
   - Preenchimento dos valores restantes com a média da coluna
4. Remoção de colunas não numéricas (ex.: nome dos países)

### 📐 Dimensão final do dataset

- **Linhas:** 248
- **Colunas:** 34 (33 atributos + 1 variável alvo)

Após o tratamento, os dados foram divididos em:

- **70%** para treinamento
- **30%** para teste

---

## 🤖 Modelos de Machine Learning Utilizados

Foram aplicados e comparados os seguintes algoritmos de classificação:

- Árvore de Decisão (*Decision Tree*)
- Random Forest
- Perceptron
- Adaline (SGDClassifier)
- Naive Bayes
- Regressão Logística
- Support Vector Machine (SVM)
- XGBoost

Sempre que conceitualmente necessário, os dados foram **normalizados** utilizando `StandardScaler`.

---

## 📈 Avaliação dos Modelos

A métrica principal utilizada foi:

- **Acurácia (Accuracy)**

Inicialmente, todos os modelos foram avaliados no conjunto de teste. Em seguida, os **três melhores modelos** foram submetidos à **validação cruzada K-Fold (K = 5)**, com o objetivo de obter uma estimativa mais robusta de desempenho.

---

## 🏆 Resultados

Os resultados indicaram que o modelo com melhor desempenho médio foi o:

### ⭐ **XGBoost**

- **Acurácia média (5-Fold CV):** **≈ 75,4%**

Esse resultado sugere que modelos de *ensemble boosting* são particularmente adequados para capturar relações complexas entre indicadores macroeconômicos e classificações de risco soberano.

---

## 📌 Conclusões

- É possível obter **resultados consistentes** na classificação de risco soberano utilizando técnicas de *Machine Learning* e dados públicos.
- O desempenho do XGBoost indica que relações **não lineares e interações entre variáveis** são relevantes nesse tipo de problema.
- Apesar da boa acurácia, o estudo possui limitações, principalmente relacionadas ao tamanho da amostra.

### 🔮 Trabalhos Futuros

Algumas extensões naturais deste projeto incluem:

- Inclusão de classificações de outros anos
- Teste de novos indicadores econômicos
- Ajuste fino de hiperparâmetros (*hyperparameter tuning*)
- Aplicação de modelos de **classificação ordinal**, mais alinhados à natureza do problema

---

## 📁 Estrutura do Repositório

```
├── main.ipynb                # Notebook principal com toda a análise
├── atributos.xlsx                # Lista de indicadores selecionados
├── paises_2019_rating.xlsx       # Classificações S&P – 2019
├── paises_2021_rating.xlsx       # Classificações S&P – 2021
├── README.md                     # Este arquivo
```

---

## 📄 Observações Finais

- Este projeto possui **caráter acadêmico e exploratório**. As análises e resultados não devem ser interpretados como recomendações de investimento ou avaliações oficiais de risco.
- O arquivo da base de dados (WDICSV.csv) não está no repositório visto que o arquivo é muito pesado para o github.