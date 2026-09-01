<img width="1600" height="400" alt="WhatsApp Image 2026-09-01 at 3 01 06 PM" src="https://github.com/user-attachments/assets/af10bc17-935f-4413-a699-1d143ffde350" />
# Previsão de Churn em Telecom

![Capa do projeto](capa-churn-callcenter)

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

![Distribuição de Churn](grafico1)

A quantidade de clientes que permaneceu na empresa é maior do que a quantidade de clientes que cancelou.

Essa diferença entre as classes é importante porque mostra que **acurácia, sozinha, não é suficiente para avaliar o modelo**.

Em um problema de churn, também é necessário observar métricas como precisão, recall, F1-score e ROC-AUC.

---

## Análise exploratória

A análise exploratória foi utilizada para observar possíveis associações entre características dos clientes e o cancelamento.

Os resultados apresentados nesta etapa representam padrões observados na base. Eles não devem ser interpretados como evidência de causalidade.

### Churn por tipo de contrato

O tipo de contrato apresenta diferenças na taxa de cancelamento entre os grupos.

![Taxa de Churn por contrato](grafico2)

Essa análise ajuda a identificar quais tipos de contrato concentram uma proporção maior de clientes que cancelaram.

Esse tipo de informação pode ser utilizado como ponto de partida para investigar estratégias específicas de retenção.

---

### Tempo de permanência

A variável `tenure` representa o número de meses que o cliente permaneceu na empresa.

![Tempo de permanência por Churn](imagens/tenure-churn.png)

A comparação permite observar como o tempo de relacionamento se distribui entre clientes que permaneceram e aqueles que cancelaram.

Essa variável também será utilizada pelo modelo na identificação de padrões associados ao churn.

---

### Cobrança mensal

Também foi analisada a distribuição dos valores de cobrança mensal entre os dois grupos.

![Cobrança mensal por Churn](imagens/monthly-charges-churn.png)

A visualização permite comparar a distribuição de `MonthlyCharges` entre clientes que permaneceram e clientes que cancelaram.

Assim como nas análises anteriores, essa relação representa uma associação encontrada nos dados e não significa que o valor da cobrança seja, isoladamente, responsável pelo cancelamento.

---

## Metodologia

O desenvolvimento do modelo foi dividido em algumas etapas:

**1. Preparação dos dados**

Remoção do identificador do cliente e conversão da variável `TotalCharges` para formato numérico.

**2. Separação entre treino e teste**

Os dados foram divididos em:

* 80% para treinamento
* 20% para teste

A divisão foi realizada de forma estratificada, mantendo a proporção das classes de Churn nos dois conjuntos.

![Divisão entre treino e teste](imagens/divisao-treino-teste.png)

**3. Pré-processamento**

As variáveis categóricas foram transformadas utilizando `OneHotEncoder`.

Valores ausentes foram tratados dentro do pipeline utilizando imputação pela mediana para variáveis numéricas e pelo valor mais frequente para variáveis categóricas.

Essa abordagem evita que informações do conjunto de teste sejam utilizadas durante o treinamento.

**4. Modelagem**

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

Em um problema de churn, o **recall** merece atenção especial. Um falso negativo representa um cliente que realmente cancelou, mas que não foi identificado pelo modelo como possível churn.

---

## Matriz de confusão

A matriz de confusão permite observar de forma mais detalhada os acertos e erros realizados pelo modelo no conjunto de teste.

![Matriz de Confusão](imagens/matriz-confusao.png)

Os principais resultados podem ser interpretados como:

* **Verdadeiro negativo:** cliente que não cancelou e foi corretamente classificado.
* **Verdadeiro positivo:** cliente que cancelou e foi corretamente identificado.
* **Falso positivo:** cliente que não cancelou, mas foi classificado como churn.
* **Falso negativo:** cliente que cancelou, mas não foi identificado pelo modelo.

Para a área de retenção, os falsos negativos merecem atenção porque representam clientes que poderiam ter sido priorizados para uma ação, mas não foram identificados pelo modelo.

---

## Variáveis mais importantes

O Random Forest permite analisar quais variáveis tiveram maior importância nas decisões realizadas pelo modelo.

![Importância das variáveis](imagens/importancia-variaveis.png)

Essa análise deve ser interpretada como uma forma de compreender o comportamento do modelo.

Uma variável com alta importância não significa necessariamente que ela seja a causa do cancelamento. Ela indica que o modelo utilizou aquela informação com maior frequência ou relevância para realizar suas previsões.

Além disso, variáveis categóricas são transformadas em diferentes categorias durante o One-Hot Encoding, o que deve ser considerado na interpretação do gráfico.

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

![Fluxo de aplicação](imagens/fluxo-retencao.png)

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
│
├── imagens/
│   ├── capa-telecom-churn.png
│   ├── distribuicao-churn.png
│   ├── divisao-treino-teste.png
│   ├── churn-por-contrato.png
│   ├── tenure-churn.png
│   ├── monthly-charges-churn.png
│   ├── matriz-confusao.png
│   ├── importancia-variaveis.png
│   └── fluxo-retencao.png
│
└── requirements.txt
```

---

## Conclusão

Este projeto demonstra uma aplicação prática de Machine Learning para um problema de negócio relacionado à retenção de clientes.

A partir da análise exploratória, foi possível observar diferentes padrões associados ao churn. Em seguida, foi desenvolvido um modelo de Random Forest com pré-processamento estruturado em pipeline, evitando vazamento de informações entre treinamento e teste.

A avaliação foi realizada utilizando diferentes métricas, permitindo uma visão mais completa do desempenho do modelo do que a utilização isolada da acurácia.

Mais do que prever quem pode cancelar, o objetivo é transformar a previsão em informação útil para apoiar a priorização de estratégias de retenção.

### Próximos passos

Como evolução do projeto, algumas possibilidades seriam:

* Comparar o Random Forest com Regressão Logística e outros algoritmos.
* Realizar otimização de hiperparâmetros.
* Avaliar a calibração das probabilidades previstas.
* Criar faixas de risco de churn.
* Desenvolver um dashboard no Power BI.
* Criar uma rotina para disponibilizar novas previsões.
* Avaliar o impacto das ações de retenção após a implementação do modelo.

---

## Notebook

O desenvolvimento completo, incluindo preparação dos dados, análise exploratória, treinamento e avaliação do modelo, está disponível no notebook:

**`notebook_churn.ipynb`**
