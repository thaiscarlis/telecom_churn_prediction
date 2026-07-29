# Prevendo o Churn em Telecom: Um projeto prático de Machine Learning focado em negócios

### Introdução: O calcanhar de Aquiles das empresas de assinatura
Imagine que a sua empresa gasta centenas de reais em marketing para conquistar um único cliente. Ele assina o serviço, usa por dois meses e cancela. Todo o Custo de Aquisição de Cliente (CAC) vai por água abaixo. No mercado de serviços recorrentes, reter quem já está na base é até 5 vezes mais barato do que trazer alguém novo. É por isso que o **Churn (taxa de cancelamento)** é o principal indicador de saúde financeira de uma empresa. Mas e se pudéssemos ler os sinais antes do cliente ir embora? Neste projeto, utilizei Ciência de Dados para antecipar esse comportamento.

```python
# ========================================== #
# 1. IMPORTAÇÃO DAS BIBLIOTECAS ESSENCIAIS #
# ========================================== #
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler, LabelEncoder
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import classification_report, confusion_matrix, accuracy_score
```

### O impacto financeiro: O problema de negócio por trás dos dados
Utilizando uma base de dados real com mais de 7.000 clientes de uma empresa de Telecomunicações, o desafio foi desenhar uma solução em duas frentes:
1. Construir um modelo preditivo de classificação capaz de apontar quais usuários têm alta probabilidade de evasão.
2. Identificar os gargalos operacionais e comerciais que funcionam como gatilhos para a perda desses clientes.

```python
# ========================================== #
# 2. CARREGAMENTO DOS DADOS                  #
# ========================================== #
# Se estiver no Colab, certifique-se de fazer o upload do arquivo com este nome exato:
df = pd.read_csv('WA_Fn-UseC_-Telco-Customer-Churn.csv')
print("Formato dos dados originais:", df.shape)
```

### Tratamento de dados e engenharia de atributos com Python
Nenhum dado real vem pronto para o modelo preditivo. O primeiro passo envolveu uma etapa rigorosa de Data Cleaning e preparação de variáveis com a biblioteca **Pandas**:
* **Controle de Tipagem:** A coluna de faturamento total (`TotalCharges`) continha espaços vazios escondidos e estava em formato de texto. Forcei a conversão para dados numéricos e preenchi as ausências com a mediana da coluna.
* **Codificação:** Removi variáveis irrelevantes (como IDs de clientes) e apliquei `LabelEncoder` para transformar categorias de texto (como tipo de internet e gênero) em dados numéricos, permitindo o processamento matemático do algoritmo.

```python
# ========================================== #
# 3. LIMPEZA E TRATAMENTO DE DADOS (DATA CLEANING) #
# ========================================== #
# Remover ID do cliente, que não serve para o modelo matemático
df = df.drop(columns=['customerID'], errors='ignore')

# Correção crucial: a coluna 'TotalCharges' veio como texto (object) devido a espaços em branco.
# Vamos forçar a conversão para número e transformar espaços vazios em valores nulos (NaN)
df['TotalCharges'] = pd.to_numeric(df['TotalCharges'], errors='coerce')

# Preencher os valores nulos gerados em 'TotalCharges' com a mediana da coluna
df['TotalCharges'] = df['TotalCharges'].fillna(df['TotalCharges'].median())

# Transformar todas as colunas de texto (categóricas) em números usando LabelEncoder
label_encoders = {}
for col in df.select_dtypes(include=['object']).columns:
    le = LabelEncoder()
    df[col] = le.fit_transform(df[col].astype(str))
    label_encoders[col] = le # Guarda o encoder caso precise reverter o texto depois
```

### A inteligência preditiva: Classificação com Random Forest
Para garantir a validação do modelo, dividi a base em 80% para treinamento e 20% para testes. Escolhi o algoritmo **Random Forest Classifier** por ser extremamente robusto para dados tabulares e menos propenso a distorções (overfitting). Apliquei a padronização de escala com `StandardScaler` nas variáveis preditoras antes de rodar o treinamento da floresta de decisão.

```python
# ========================================== #
# 4. SEPARAÇÃO DOS DADOS E DIVISÃO TREINO/TESTE #
# ========================================== #
# Variáveis preditoras (X) e Variável alvo (y - Churn)
X = df.drop(columns=['Churn'])
y = df['Churn']

# Divisão clássica: 80% dos dados para treinar o modelo e 20% para testar a qualidade
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42, stratify=y)

# ========================================== #
# 5. NORMALIZAÇÃO DOS DADOS (FEATURE SCALING) #
# ========================================== #
# Mantém os dados na mesma escala para evitar que números grandes distorçam o modelo
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)

# ========================================== #
# 6. CRIAÇÃO E TREINAMENTO DO MODELO (MACHINE LEARNING) #
# ========================================== #
# Usaremos o Random Forest, um dos algoritmos mais robustos para tabelas
modelo_churn = RandomForestClassifier(random_state=42, n_estimators=100, max_depth=10)
modelo_churn.fit(X_train, y_train)
```

### Insights do modelo: O que realmente faz o cliente ir embora?
O modelo alcançou uma acurácia geral sólida na casa dos 80% (79.63% nos testes), mas o verdadeiro valor de um cientista de dados está em extrair valor de negócio a partir do código. Ao analisar o peso que o modelo deu para cada factor (Feature Importance), descobri três insights críticos:
1. **O Tipo de Contrato dita as regras:** Contratos mensais (*Month-to-month*) têm uma taxa de cancelamento drasticamente maior do que contratos de longo prazo (1 ou 2 anos).
2. **O perigo do "Tempo de Casa" (Tenure):** O risco de evasão é crítico nos primeiros 6 meses de assinatura. Se o cliente ultrapassar essa barreira, a curva de fidelidade se estabiliza.
3. **Faturamento sem Suporte:** Cobranças mensais elevadas sem que o cliente tenha contratado pacotes de suporte técnico ou proteção de dispositivo aceleram a insatisfação.

![Gráfico de Importância das Variáveis](grafico_churn.png)

```python
# ========================================== #
# 7. AVALIAÇÃO DO MODELO (MÉTRICAS DO PORTFÓLIO) #
# ========================================== #
y_pred = modelo_churn.predict(X_test)
print("\n--- RESULTADOS DO MODELO ---")
print(f"Acurácia Geral: {accuracy_score(y_test, y_pred) * 100:.2f}%")
print("\nRelatório Completo de Classificação:")
print(classification_report(y_test, y_pred))
```

```python
# Gráfico de Importância das Variáveis (O que mais faz o cliente cancelar?)
importancias = pd.Series(modelo_churn.feature_importances_, index=X.columns).sort_values(ascending=False)
plt.figure(figsize=(10, 6))
sns.barplot(x=importancias, y=importancias.index, palette="viridis")
plt.title("Quais fatores mais influenciam o Cancelamento (Churn)?")
plt.xlabel("Grau de Importância")
plt.ylabel("Variáveis do Dataset")
plt.tight_layout()
plt.show()
```

### Próximos passos: Como os times podem usar este modelo?
Com essa inteligência rodando em ambiente de produção, a empresa passa a agir de forma preventiva e inteligente, não mais reativa:
* O time de **Marketing** e **Customer Success** pode extrair listas automatizadas de clientes que atingiram uma probabilidade de Churn acima de 75%.
* Com essa lista, a empresa pode disparar campanhas oferecendo upgrades de suporte ou migração vantajosa para planos anuais exatamente para os clientes em risco, blindando a receita antes do cancelamento acontecer.

A Ciência de Dados prova o seu valor quando deixa de ser apenas uma ferramenta técnica de código e passa a guiar decisões de faturamento e governança.
