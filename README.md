<img width="1600" height="400" alt="WhatsApp Image 2026-09-01 at 3 01 06 PM" src="https://github.com/user-attachments/assets/c03c2a7d-f73f-49cb-b9ac-cffdb0f2bbc0" />

# 📊 Previsão de Churn em Telecom

Projeto de análise de dados e Machine Learning desenvolvido para identificar padrões associados ao cancelamento de clientes em uma empresa de telecomunicações.

O objetivo é construir um modelo capaz de identificar clientes com maior probabilidade de churn e, a partir dessas informações, gerar insights que possam apoiar estratégias de retenção.

---

## 🎯 Problema de negócio

A perda de clientes representa um impacto direto na receita e na sustentabilidade de uma empresa de telecomunicações.

Nesse contexto, o objetivo deste projeto é responder à seguinte pergunta:

> **Quais características dos clientes estão mais associadas ao cancelamento e como um modelo preditivo pode ajudar a identificar clientes com maior risco de churn?**

A proposta não é apenas prever quais clientes podem cancelar, mas transformar os resultados do modelo em informações úteis para apoiar decisões de retenção.

---

## 📂 Dataset

Foi utilizado o dataset **Telco Customer Churn**, disponibilizado originalmente pela IBM e amplamente utilizado em projetos de análise e Machine Learning.

A base contém **7.043 clientes** e informações relacionadas a:

* Tempo de permanência na empresa (`tenure`)
* Tipo de contrato (`Contract`)
* Cobrança mensal (`MonthlyCharges`)
* Valor total pago (`TotalCharges`)
* Forma de pagamento (`PaymentMethod`)
* Tipo de serviço de internet (`InternetService`)
* Cancelamento do serviço (`Churn`)
* Entre outras características dos clientes

A variável `Churn` representa o cancelamento do serviço e foi utilizada como variável-alvo do modelo.

---

## 📊 Distribuição do Churn

Antes da construção do modelo, foi analisada a distribuição entre clientes que permaneceram e clientes que cancelaram o serviço.

![Distribuição de Churn](grafico1.png)

A quantidade de clientes que permaneceu na empresa é maior do que a quantidade de clientes que cancelou.

Essa diferença entre as classes é importante para a avaliação do modelo, pois mostra que **acurácia, sozinha, não é suficiente** para analisar um problema de churn.

Por esse motivo, também foram consideradas métricas como:

* Precisão
* Recall
* F1-score
* ROC-AUC

---

## 🔎 Análise exploratória

A análise exploratória foi realizada para compreender melhor o comportamento dos clientes e identificar possíveis padrões relacionados ao cancelamento.

### Churn por tipo de contrato

O tipo de contrato apresenta diferenças na ocorrência de cancelamentos entre os grupos.

![Churn por tipo de contrato](grafico2.png)

Essa análise permite observar quais categorias de contrato apresentam maior concentração de clientes que cancelaram.

Esse resultado pode ser utilizado como ponto de partida para investigar estratégias de retenção específicas para cada perfil de cliente.

É importante destacar que uma associação observada nos dados **não significa necessariamente que uma variável seja a causa do cancelamento**.

---

## 🧹 Tratamento dos dados

Antes da construção do modelo, foram realizadas algumas etapas de preparação:

* Remoção da identificação individual dos clientes (`customerID`)
* Conversão da variável `TotalCharges` para formato numérico
* Tratamento de valores ausentes
* Transformação da variável `Churn` em variável binária
* Separação entre variáveis numéricas e categóricas

O pré-processamento das variáveis preditoras foi realizado dentro de um `Pipeline`, evitando que informações do conjunto de teste fossem utilizadas durante o treinamento.

---

## 🧪 Divisão entre treino e teste

Os dados foram divididos em:

* **80% para treinamento**
* **20% para teste**

Foi utilizada divisão estratificada para preservar aproximadamente a mesma proporção de clientes que cancelaram e permaneceram nos dois conjuntos.

Essa separação permite avaliar o desempenho do modelo em dados que não foram utilizados durante seu treinamento.

---

## ⚙️ Pré-processamento

As variáveis numéricas e categóricas receberam tratamentos diferentes.

### Variáveis numéricas

Foi utilizado tratamento de valores ausentes por meio da mediana.

### Variáveis categóricas

Foi realizado:

* Tratamento de valores ausentes pela categoria mais frequente
* Codificação utilizando `OneHotEncoder`

Todo esse processo foi incorporado ao pipeline do modelo.

Essa abordagem reduz o risco de **data leakage**, garantindo que o conjunto de teste não influencie o processo de preparação dos dados utilizado durante o treinamento.

---

## 🌳 Modelo utilizado

Foi utilizado o algoritmo **Random Forest Classifier**.

A escolha do Random Forest ocorreu por sua capacidade de trabalhar com diferentes tipos de variáveis e capturar relações não lineares presentes nos dados.

Além disso, o modelo permite analisar a importância das variáveis utilizadas nas decisões preditivas.

O modelo foi configurado utilizando:

* `n_estimators = 100`
* `max_depth = 10`
* `random_state = 42`

---

## 📈 Avaliação do modelo

O modelo foi avaliado tanto no conjunto de treinamento quanto no conjunto de teste.

Foram utilizadas as seguintes métricas:

### Accuracy

Indica a proporção total de previsões classificadas corretamente.

### Precision

Indica, entre os clientes classificados como churn, quantos realmente cancelaram.

### Recall

Indica, entre os clientes que realmente cancelaram, quantos foram identificados pelo modelo.

### F1-score

Combina Precision e Recall em uma única métrica.

### ROC-AUC

Avalia a capacidade do modelo de diferenciar clientes que cancelaram daqueles que permaneceram.

---

## 🎯 Por que o Recall é importante?

Em um problema de churn, deixar de identificar um cliente que realmente está em risco pode representar uma oportunidade perdida de retenção.

Por isso, o **Recall merece atenção especial**.

Um modelo com alta acurácia pode parecer bom mesmo quando apresenta dificuldades para identificar corretamente a classe de clientes que cancelaram.

Dessa forma, a avaliação deve considerar o conjunto de métricas e não apenas uma única medida de desempenho.

---

## 🔢 Matriz de confusão

A matriz de confusão permite visualizar a quantidade de:

* Verdadeiros negativos
* Falsos positivos
* Falsos negativos
* Verdadeiros positivos

Essa análise ajuda a compreender de forma mais detalhada quais tipos de erro o modelo está cometendo.

---

## 💡 Interpretação das variáveis

A análise das variáveis permite compreender quais características possuem maior participação nas decisões do modelo.

Entre as informações analisadas estão características como:

* Tipo de contrato
* Tempo de permanência
* Cobrança mensal
* Forma de pagamento
* Tipo de serviço contratado

A importância das variáveis representa a contribuição delas **dentro do modelo**, não uma relação de causa e efeito.

Portanto, uma variável considerada importante pelo modelo não deve ser interpretada automaticamente como a causa do churn.

---

## 💼 Aplicação no negócio

Os resultados do modelo podem ser utilizados como apoio a uma estratégia de retenção.

Um possível fluxo seria:

```text
Cliente
   ↓
Modelo preditivo
   ↓
Probabilidade de churn
   ↓
Identificação de clientes prioritários
   ↓
Análise do perfil
   ↓
Estratégia de retenção
```

Por exemplo, clientes identificados pelo modelo como apresentando maior risco poderiam ser direcionados para análises ou ações de retenção.

O modelo deve funcionar como **ferramenta de apoio à decisão**, e não como único critério para definir uma ação comercial.

---

## ⚠️ Cuidados durante o desenvolvimento

Alguns cuidados foram considerados durante a construção do projeto:

* Separação entre treino e teste antes do treinamento do modelo
* Utilização de Pipeline para evitar data leakage
* Tratamento das variáveis dentro do processo de modelagem
* Avaliação utilizando múltiplas métricas
* Atenção especial ao Recall
* Interpretação das variáveis sem assumir causalidade
* Utilização das previsões como apoio às decisões de negócio

---

## 🛠️ Tecnologias utilizadas

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Jupyter Notebook / Google Colab
* GitHub

---

## 📁 Estrutura do projeto

```text
telecom_churn_prediction/
│
├── README.md
├── notebook_churn.ipynb
├── WA_Fn-UseC_-Telco-Customer-Churn.csv
├── grafico1.png
└── grafico2.png
```

---

## 🚀 Como executar o projeto

### 1. Clone o repositório

```bash
git clone https://github.com/thaiscarlis/telecom_churn_prediction.git
```

### 2. Acesse a pasta

```bash
cd telecom_churn_prediction
```

### 3. Instale as bibliotecas necessárias

```bash
pip install pandas numpy matplotlib seaborn scikit-learn
```

### 4. Execute o notebook

Abra o arquivo:

```text
notebook_churn.ipynb
```

O projeto também pode ser executado diretamente pelo Google Colab.

---

## 📌 Conclusão

Este projeto demonstra a aplicação de técnicas de análise exploratória e Machine Learning para um problema real de negócio: a previsão de churn em uma empresa de telecomunicações.

A análise permite compreender o perfil dos clientes e identificar padrões associados ao cancelamento.

O modelo preditivo pode ser utilizado para **priorizar clientes com maior probabilidade de churn**, permitindo que a empresa investigue esses perfis e desenvolva estratégias de retenção mais direcionadas.

Como próximos passos, seria possível testar outros algoritmos, realizar ajuste de hiperparâmetros, avaliar diferentes estratégias de balanceamento das classes e buscar melhorias específicas no Recall do modelo.
