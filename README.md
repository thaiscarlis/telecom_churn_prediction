<img width="1600" height="400" alt="WhatsApp Image 2026-09-01 at 3 01 06 PM" src="https://github.com/user-attachments/assets/c03c2a7d-f73f-49cb-b9ac-cffdb0f2bbc0" />
# Previsão de Churn em Telecom

## Sobre o projeto

Este projeto utiliza Machine Learning para analisar o comportamento de clientes de uma empresa de telecomunicações e identificar aqueles que apresentam maior probabilidade de cancelamento.

O objetivo não é apenas construir um modelo preditivo, mas entender os padrões encontrados nos dados e transformar essas informações em apoio para estratégias de retenção de clientes.

O projeto foi desenvolvido em Python, utilizando análise exploratória de dados, pré-processamento, Random Forest e métricas de classificação.

---

## O problema de negócio

A perda de clientes, conhecida como **churn**, pode representar impacto direto na receita de empresas que trabalham com serviços recorrentes.

Nesse cenário, uma pergunta importante para o negócio é:

> **Quais clientes apresentam maior probabilidade de cancelar o serviço?**

Uma previsão desse tipo pode ajudar equipes de retenção, Customer Success e Marketing a priorizar clientes para ações de relacionamento.

O modelo não substitui a decisão da empresa. Ele funciona como uma ferramenta de apoio, permitindo direcionar esforços para os clientes que apresentam maior risco segundo os padrões identificados nos dados.

---

## Base de dados

O conjunto de dados utilizado contém informações de **7.043 clientes** e reúne características relacionadas ao perfil, serviços contratados, tipo de contrato, tempo de permanência e cobrança.

Entre as principais variáveis analisadas estão:

| Variável          | Descrição                                            |
| ----------------- | ---------------------------------------------------- |
| `tenure`          | Tempo de permanência do cliente na empresa, em meses |
| `Contract`        | Tipo de contrato do cliente                          |
| `MonthlyCharges`  | Valor cobrado mensalmente                            |
| `TotalCharges`    | Total acumulado de cobranças                         |
| `PaymentMethod`   | Forma de pagamento utilizada                         |
| `InternetService` | Tipo de serviço de internet contratado               |
| `Churn`           | Indica se o cliente cancelou o serviço               |

A variável `Churn` é o alvo que o modelo tenta prever.

---

## Distribuição do Churn

Antes de construir o modelo, foi analisada a distribuição entre clientes que permaneceram e clientes que cancelaram o serviço.

![Distribuição de Churn](grafico1.png)

A quantidade de clientes que permaneceu na empresa é maior do que a quantidade de clientes que cancelou.

Essa diferença entre as classes é importante porque mostra que **acurácia, sozinha, não é suficiente para avaliar o modelo**.

Em um problema de churn, também é necessário observar métricas como precisão, recall, F1-score e ROC-AUC.

---

## Análise exploratória

A análise exploratória foi utilizada para observar possíveis associações entre características dos clientes e o cancelamento.

Os resultados apresentados nesta etapa representam padrões observados na base. Eles não devem ser interpretados como evidência de causalidade.

### Churn por tipo de contrato

O tipo de contrato apresenta diferenças na taxa de cancelamento entre os grupos.

![Taxa de Churn por contrato](grafico2.png)

Essa análise ajuda a identificar quais tipos de contrato concentram uma proporção maior de clientes que cancelaram.

Esse tipo de informação pode ser utilizado como ponto de partida para investigar estratégias específicas de retenção.

---

## Metodologia

O desenvolvimento do modelo foi dividido em algumas etapas.

### 1. Preparação dos dados

Foi realizada a remoção do identificador do cliente e a conversão da variável `TotalCharges` para formato numérico.

A variável `Churn` foi transformada em uma variável binária, sendo:

* `0` = cliente permaneceu
* `1` = cliente cancelou

### 2. Separação entre treino e teste

Os dados foram divididos em:

* **80% para treinamento**
* **20% para teste**

A divisão foi realizada de forma estratificada, mantendo a proporção das classes de `Churn` nos dois conjuntos.

### 3. Pré-processamento

As variáveis categóricas foram transformadas utilizando `OneHotEncoder`.

Valores ausentes foram tratados dentro do pipeline utilizando imputação pela mediana para variáveis numéricas e pelo valor mais frequente para variáveis categóricas.

Essa abordagem evita que informações do conjunto de teste sejam utilizadas durante o treinamento.

### 4. Modelagem

Foi utilizado o algoritmo **Random Forest Classifier**, um modelo baseado na combinação de diversas árvores de decisão.

---

## Por que Random Forest?

O Random Forest foi escolhido como modelo inicial por sua capacidade de trabalhar com relações não lineares e diferentes tipos de variáveis após o pré-processamento.

Outro ponto importante para este projeto é a possibilidade de analisar a importância das variáveis utilizadas pelo modelo.

O objetivo, entretanto, não é assumir que as variáveis mais importantes sejam necessariamente as causas do churn. A importância representa o quanto determinada variável contribuiu para as decisões do modelo.

---

## Avaliação do modelo

O modelo foi avaliado tanto no conjunto de treinamento quanto no conjunto de teste.

Essa comparação permite observar não apenas o desempenho durante o treinamento, mas também a capacidade de generalização para dados que o modelo não viu anteriormente.

As principais métricas utilizadas foram:

| Métrica      | O que representa                                                                    |
| ------------ | ----------------------------------------------------------------------------------- |
| **Acurácia** | Proporção total de previsões corretas                                               |
| **Precisão** | Entre os clientes classificados como churn, quantos realmente cancelaram            |
| **Recall**   | Entre os clientes que realmente cancelaram, quantos foram identificados pelo modelo |
| **F1-score** | Equilíbrio entre precisão e recall                                                  |
| **ROC-AUC**  | Capacidade do modelo de distinguir as duas classes                                  |

### Resultados

Os valores abaixo devem ser preenchidos com os resultados reais obtidos durante a execução do notebook.

| Métrica  |      Treino |       Teste |
| -------- | ----------: | ----------: |
| Acurácia | `resultado` | `resultado` |
| Precisão | `resultado` | `resultado` |
| Recall   | `resultado` | `resultado` |
| F1-score | `resultado` | `resultado` |
| ROC-AUC  | `resultado` | `resultado` |

> **Importante:** os resultados apresentados aqui devem ser os mesmos obtidos na execução final do notebook.

Em um problema de churn, o **recall** merece atenção especial.

Um falso negativo representa um cliente que realmente cancelou, mas que não foi identificado pelo modelo como possível churn.

Nesse contexto, aumentar o recall pode ser importante para reduzir a quantidade de clientes em risco que deixam de ser identificados.

---

## Da previsão para a ação

Uma possível aplicação do modelo seria utilizar as probabilidades previstas para criar grupos de prioridade.

```text
Dados dos clientes
        ↓
Pré-processamento
        ↓
Modelo de Machine Learning
        ↓
Probabilidade de Churn
        ↓
Segmentação de risco
        ↓
Ações de retenção
```

Por exemplo, clientes classificados com maior probabilidade de churn poderiam ser priorizados para ações de relacionamento, ofertas personalizadas ou contato da equipe responsável pela retenção.

O modelo deve ser utilizado como **apoio à decisão**, e não como mecanismo automático para determinar qual ação será tomada com cada cliente.

---

## Cuidados com o desenvolvimento

Um dos pontos importantes deste projeto foi evitar **data leakage**, situação em que informações que deveriam estar disponíveis apenas após a separação dos dados acabam influenciando o treinamento.

Para isso, o pré-processamento foi incorporado ao `Pipeline` do Scikit-learn.

Essa estrutura permite que as transformações sejam ajustadas utilizando os dados de treinamento e posteriormente aplicadas ao conjunto de teste.

Outro cuidado foi não utilizar `StandardScaler` desnecessariamente. Como o modelo escolhido é um Random Forest, a padronização das escalas das variáveis não é necessária para o funcionamento do algoritmo.

---

## Tecnologias utilizadas

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Google Colab
* GitHub

---

## Estrutura do projeto

```text
telecom_churn_prediction/
│
├── README.md
├── WA_Fn-UseC_-Telco-Customer-Churn.csv
├── notebook_churn.ipynb
├── grafico1.png
├── grafico2.png
└── requirements.txt
```

---

## Conclusão

Este projeto demonstrou como técnicas de Machine Learning podem ser utilizadas para identificar padrões associados ao cancelamento de clientes em uma empresa de telecomunicações.

A análise exploratória permitiu compreender características da base e observar diferenças entre clientes que permaneceram e clientes que cancelaram.

A utilização de um pipeline de pré-processamento junto ao Random Forest também contribuiu para uma estrutura mais adequada de treinamento e avaliação, reduzindo o risco de data leakage.

Além da construção do modelo, o projeto busca demonstrar como uma previsão de churn pode ser transformada em informação útil para o negócio.

Na prática, o modelo pode apoiar equipes de retenção na identificação e priorização de clientes com maior risco, permitindo que estratégias de relacionamento sejam direcionadas de forma mais eficiente.

## Como próximos passos, o projeto poderia evoluir com testes de outros algoritmos, ajuste de hiperparâmetros, análise de diferentes thresholds de classificação e acompanhamento do desempenho do modelo em dados reais.

## Notebook

O notebook completo com todas as etapas de análise, preparação dos dados, treinamento e avaliação do modelo está disponível neste repositório.
