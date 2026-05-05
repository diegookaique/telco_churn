# Telco Customer Churn — Predição de Cancelamento de Clientes

Projeto completo de ciência de dados para prever quais clientes de uma empresa de telecomunicações têm maior probabilidade de cancelar o serviço (churn). O objetivo é permitir que a empresa tome ações preventivas de retenção antes que a perda ocorra, reduzindo o impacto financeiro do cancelamento.

---

## Resultados do Modelo

| Métrica | XGBoost (melhor modelo) |
|---|---|
| AUC-ROC | 0.8470 |
| Recall | 0.8182 |
| F1-Score | 0.6418 |
| Otimização | Optuna — 50 trials, 5-fold CV |

O XGBoost com hiperparâmetros otimizados pelo Optuna obteve o melhor desempenho em AUC-ROC e Recall — a métrica mais crítica neste contexto, pois um churn não detectado representa perda direta de receita.

---

## Principais Insights

- **26.5%** dos clientes cancelaram — dataset desbalanceado, o que exige métricas adequadas (AUC-ROC, Recall, F1)
- Clientes com **contrato mensal** têm taxa de churn de **42.7%**, contra apenas **2.8%** em contratos bienais
- Clientes nos **primeiros 12 meses** de contrato apresentam risco de churn duas vezes maior que a média
- **Internet via fibra óptica** e **ausência de suporte técnico** estão associadas a taxas de cancelamento significativamente acima da média
- Segundo os SHAP values, `tenure` e `MonthlyCharges` são as features de maior impacto individual nas predições

---

## Estrutura do Projeto

```
telco-customer-churn/
│
├── data/
│   ├── raw/                        # Dataset original do Kaggle
│   └── processed/                  # Dataset após feature engineering
│
├── notebooks/
│   └── telco_churn.ipynb           # Notebook completo (EDA + FE + Modelagem + SHAP)
│
├── reports/
│   └── figures/                    # Gráficos gerados pelo projeto
│
└── README.md
```

---

## Fluxo do Projeto

```
EDA  →  Feature Engineering  →  Modelagem  →  Interpretabilidade (SHAP)
```

**Parte 1 — EDA**
Análise da distribuição do churn, variáveis numéricas e categóricas, e identificação do período crítico de retenção via segmentação por tenure.

**Parte 2 — Feature Engineering**
Encoding de variáveis binárias e categóricas, criação de três features derivadas motivadas pelo EDA (`avg_monthly_spend`, `num_services`, `is_new_customer`), normalização com MinMaxScaler e divisão treino/teste com estratificação.

**Parte 3 — Modelagem**
Treinamento de Regressão Logística, Random Forest e XGBoost. Otimização de hiperparâmetros com Optuna (50 trials por modelo). Avaliação com curvas ROC, matrizes de confusão, validação cruzada 5-fold e análise de impacto financeiro.

**Parte 4 — Interpretabilidade com SHAP**
Summary plot, bar plot, dependence plot para `tenure` e `MonthlyCharges`, waterfall plot e force plot para explicação de predições individuais.

---

## Stack Tecnológica

| Categoria | Ferramentas |
|---|---|
| Linguagem | Python 3.10 |
| Manipulação de dados | Pandas, NumPy |
| Visualização | Matplotlib, Seaborn |
| Modelagem | Scikit-learn, XGBoost |
| Otimização | Optuna |
| Interpretabilidade | SHAP |
| Ambiente | Google Colab |

---

## Como Executar

**1. Clone o repositório**
```bash
git clone https://github.com/diegookaique/telco-customer-churn.git
cd telco-customer-churn
```

**2. Faça o download do dataset**

Acesse [Kaggle — Telco Customer Churn](https://www.kaggle.com/datasets/blastchar/telco-customer-churn), baixe o arquivo `WA_Fn-UseC_-Telco-Customer-Churn.csv` e coloque em `data/raw/`.

**3. Abra o notebook**

O notebook foi desenvolvido no Google Colab. A primeira célula instala automaticamente as dependências necessárias (`optuna`, `shap`).

---

## Dicionário de Dados

| Coluna | Tipo | Descrição |
|---|---|---|
| `customerID` | texto | Identificador único do cliente |
| `gender` | binária | Gênero: `Male` ou `Female` |
| `SeniorCitizen` | binária | Se o cliente é idoso: `1` (sim) ou `0` (não) |
| `Partner` | binária | Se possui cônjuge ou parceiro |
| `Dependents` | binária | Se possui dependentes |
| `tenure` | numérica | Tempo de contrato em meses (0 a 72) |
| `PhoneService` | binária | Possui serviço de telefonia |
| `MultipleLines` | categórica | Possui múltiplas linhas telefônicas |
| `InternetService` | categórica | Tipo de internet: `DSL`, `Fiber optic`, `No` |
| `OnlineSecurity` | categórica | Possui segurança online |
| `OnlineBackup` | categórica | Possui backup online |
| `DeviceProtection` | categórica | Possui proteção de dispositivos |
| `TechSupport` | categórica | Possui suporte técnico |
| `StreamingTV` | categórica | Possui streaming de TV |
| `StreamingMovies` | categórica | Possui streaming de filmes |
| `Contract` | categórica | Tipo de contrato: mensal, anual ou bienal |
| `PaperlessBilling` | binária | Fatura digital |
| `PaymentMethod` | categórica | Forma de pagamento |
| `MonthlyCharges` | numérica | Valor cobrado mensalmente (USD) |
| `TotalCharges` | numérica | Valor total cobrado no período (USD) |
| **`Churn`** | **binária** | **Variável alvo: cancelou (`Yes`) ou não (`No`)** |

---

## Recomendações de Negócio

Com base nos resultados do modelo e nos SHAP values, as ações de maior impacto para redução do churn são:

- Priorizar campanhas de retenção nos primeiros 12 meses de contrato, período de maior risco identificado
- Oferecer migração para contratos anuais ou bienais a clientes de contrato mensal com alto score de churn
- Incentivar a contratação de suporte técnico como fator de aumento do custo de troca (switching cost)
- Revisar a experiência dos clientes de internet via fibra óptica, que apresentam taxa de churn acima da média

---

## Autor

**Diego Kaique**  

[![LinkedIn](https://img.shields.io/badge/LinkedIn-diego--kaique-blue?logo=linkedin)](https://linkedin.com/in/diego-kaique-9ba3697b)
[![GitHub](https://img.shields.io/badge/GitHub-diegookaique-black?logo=github)](https://github.com/diegookaique)
[![Email](https://img.shields.io/badge/Email-kaique__0208%40hotmail.com-red?logo=gmail)](mailto:kaique_0208@hotmail.com)

---

> *"Dados não mentem — mas precisam ser bem perguntados."*
