# Previsão de Churn em Telecom

Projeto de Machine Learning desenvolvido para analisar o cancelamento de clientes de uma empresa de telecomunicações e identificar quais características estão mais relacionadas ao churn.

A ideia foi trabalhar o problema não apenas como uma classificação de clientes, mas também como uma análise de negócio: entender **quem está mais propenso a cancelar e quais fatores estão relacionados a esse comportamento**.

## Objetivo

O projeto foi dividido em duas partes principais:

* Criar um modelo capaz de prever o churn dos clientes.
* Analisar os dados para identificar os principais fatores associados ao cancelamento.

A base utilizada possui mais de 7 mil registros de clientes e informações como tipo de contrato, tempo de permanência, serviços contratados, cobrança mensal e forma de pagamento.

---

## 1. Preparação dos dados

A primeira etapa foi verificar a estrutura da base e tratar os problemas encontrados.

Uma das inconsistências estava na coluna `TotalCharges`, que estava armazenada como texto e possuía alguns valores vazios.

Fiz a conversão da coluna para o formato numérico e tratei os valores ausentes antes de seguir para a modelagem.

Também removi o `customerID`, já que o identificador do cliente não possui utilidade como variável preditora.

```python
# ========================================== #
# 1. IMPORTAÇÃO DAS BIBLIOTECAS             #
# ========================================== #

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, confusion_matrix, accuracy_score
```

### Carregamento dos dados

```python
# ========================================== #
# 2. CARREGAMENTO DOS DADOS                  #
# ========================================== #

df = pd.read_csv('WA_Fn-UseC_-Telco-Customer-Churn.csv')

print("Formato dos dados originais:", df.shape)
```

### Tratamento de `TotalCharges`

```python
# ========================================== #
# 3. LIMPEZA E TRATAMENTO DOS DADOS          #
# ========================================== #

df = df.drop(columns=['customerID'], errors='ignore')

df['TotalCharges'] = pd.to_numeric(
    df['TotalCharges'],
    errors='coerce'
)

df['TotalCharges'] = df['TotalCharges'].fillna(
    df['TotalCharges'].median()
)
```

Depois disso, as variáveis categóricas foram transformadas em valores numéricos para que pudessem ser utilizadas pelo modelo.

```python
label_encoders = {}

for col in df.select_dtypes(include=['object']).columns:
    le = LabelEncoder()
    df[col] = le.fit_transform(df[col].astype(str))
    label_encoders[col] = le
```

---

## 2. Separação entre treino e teste

A variável `Churn` foi definida como variável alvo.

Separei 80% dos dados para treinamento e 20% para teste. Também utilizei `stratify` para manter uma proporção semelhante de clientes que cancelaram e não cancelaram nos dois conjuntos.

```python
# ========================================== #
# 4. SEPARAÇÃO DOS DADOS                    #
# ========================================== #

X = df.drop(columns=['Churn'])
y = df['Churn']

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,
    random_state=42,
    stratify=y
)
```

---

## 3. Modelo utilizado

Para a classificação utilizei o **Random Forest Classifier**.

Escolhi esse algoritmo por ser uma opção adequada para trabalhar com dados tabulares e por permitir analisar a importância das variáveis utilizadas nas decisões do modelo.

```python
# ========================================== #
# 5. CRIAÇÃO E TREINAMENTO DO MODELO        #
# ========================================== #

modelo_churn = RandomForestClassifier(
    random_state=42,
    n_estimators=100,
    max_depth=10
)

modelo_churn.fit(X_train, y_train)
```

---

## 4. Avaliação do modelo

Após o treinamento, utilizei os dados de teste para avaliar o desempenho do modelo.

```python
# ========================================== #
# 6. AVALIAÇÃO DO MODELO                    #
# ========================================== #

y_pred = modelo_churn.predict(X_test)

print("\n--- RESULTADOS DO MODELO ---")

print(
    f"Acurácia: {accuracy_score(y_test, y_pred) * 100:.2f}%"
)

print("\nRelatório de classificação:")

print(
    classification_report(y_test, y_pred)
)
```

O modelo apresentou aproximadamente **79,63% de acurácia** no conjunto de teste.

Além da acurácia, também analisei precision, recall e F1-score para ter uma visão mais completa do desempenho da classificação.

---

## 5. Insights da análise

Depois do treinamento, analisei a importância das variáveis para entender quais características tiveram maior influência nas previsões do modelo.

Os principais pontos observados foram:

1. **Tipo de contrato:** Clientes com contratos mensais apresentam uma ocorrência maior de churn em comparação com clientes com contratos de maior duração.

2. **Tempo de permanência:** Clientes com pouco tempo de relacionamento com a empresa apresentam maior ocorrência de cancelamento, principalmente nos primeiros meses.

3. **Faturamento e serviços contratados:** O valor da cobrança mensal aparece entre as variáveis relevantes. A relação com serviços adicionais, como suporte técnico e proteção de dispositivo, pode ser explorada para entender melhor o comportamento desses clientes.

### Importância das variáveis

![Gráfico de Importância das Variáveis](https://github.com/thaiscarlis/telecom_churn_prediction/raw/main/cancelamento%3Bchurn.png)

---

## 6. Análise de importância das variáveis

A importância das variáveis foi obtida diretamente a partir do modelo Random Forest.

```python
# ========================================== #
# 7. IMPORTÂNCIA DAS VARIÁVEIS              #
# ========================================== #

importancias = pd.Series(
    modelo_churn.feature_importances_,
    index=X.columns
).sort_values(ascending=False)

plt.figure(figsize=(10, 6))

sns.barplot(
    x=importancias,
    y=importancias.index
)

plt.title("Importância das variáveis para previsão de Churn")
plt.xlabel("Importância")
plt.ylabel("Variável")

plt.tight_layout()
plt.show()
```

Esse gráfico ajuda a visualizar quais variáveis foram mais utilizadas pelo modelo durante suas decisões.

É importante destacar que **importância da variável não significa causalidade**. Ou seja, o fato de uma característica apresentar maior importância no modelo não significa necessariamente que ela seja a causa do cancelamento.

---

## 7. Possível aplicação no negócio

O modelo pode ser utilizado como parte de uma estratégia de retenção de clientes.

Por exemplo, a empresa poderia gerar uma lista de clientes com maior probabilidade de churn e utilizar essa informação para priorizar ações do time de Customer Success ou Marketing.

Algumas possibilidades seriam:

* oferecer condições específicas para clientes em risco;
* incentivar a migração para contratos de maior duração;
* revisar serviços contratados;
* identificar clientes que precisam de maior acompanhamento;
* acompanhar a evolução do churn ao longo do tempo.

A ideia não seria simplesmente prever quem vai cancelar, mas utilizar a previsão como uma informação adicional para apoiar decisões.

---

## Tecnologias utilizadas

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Machine Learning
* Random Forest
* Análise de dados

---

## O que eu desenvolvi neste projeto

Com esse projeto, pratiquei principalmente:

* limpeza e preparação de dados;
* tratamento de valores ausentes;
* transformação de variáveis categóricas;
* separação entre treino e teste;
* treinamento de modelo de classificação;
* avaliação de métricas;
* análise de importância das variáveis;
* interpretação dos resultados;
* aplicação dos resultados em um contexto de negócio.

---

## Próximos passos

Algumas melhorias que podem ser exploradas em uma próxima versão:

* comparar o Random Forest com outros algoritmos de classificação;
* analisar o balanceamento da variável `Churn`;
* testar diferentes hiperparâmetros;
* utilizar métricas como ROC-AUC;
* analisar a matriz de confusão com mais detalhes;
* realizar uma análise exploratória mais aprofundada;
* criar um dashboard para acompanhar os principais indicadores de churn;
* gerar uma saída com a probabilidade de churn de cada cliente.
