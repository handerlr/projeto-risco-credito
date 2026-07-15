# Projeto de Risco de Crédito - Análise Exploratória

## 1. Relatório de Análise Exploratória (EDA)

### 1.1 Análise Estatística e Estrutural
- **Volume:** A base analisada possui 32.581 registros.
- **Limpeza Necessária:** Identificamos nulos em `loan_int_rate` e `person_emp_length`. Também detectamos outliers na idade (144 anos) e tempo de emprego (123 anos), que serão tratados na fase de limpeza.

### 1.2 Insights Visuais
- **Distribuição da Variável Alvo:** O `countplot` revela um desbalanceamento na variável `loan_status`. A classe majoritária (0) domina, o que exigirá técnicas de reamostragem no treinamento para evitar um modelo enviesado.
- **Correlação:** O `heatmap` destaca correlações moderadas entre `loan_status` e variáveis como `loan_int_rate` (taxa de juros) e `loan_percent_income`. Isso indica que estas variáveis são preditores importantes para o nosso modelo.

### 1.3 Decisões para o Pipeline
1. **Limpeza:** Remover registros com dados impossíveis (outliers).
2. **Imputação:** Utilizar a mediana para tratar os nulos, dado que a distribuição não é perfeitamente normal.
3. **Engenharia:** Criar a coluna `comprometimento_renda` para aumentar o poder preditivo do modelo.

## 2. Tratamento e Limpeza (Data Prep)

### 2.1 Limpeza de Dados
* **Remoção de Outliers:** Identificamos idades de 144 anos e tempo de emprego de 123 anos, que são inconsistentes. Essas linhas foram removidas para garantir a qualidade do treinamento.
* **Duplicatas:** Removidas para evitar viés nos dados.

### 2.2 Tratamento de Nulos
* **Abordagem:** As variáveis `person_emp_length` e `loan_int_rate` continham valores nulos.
* **Justificativa:** Optamos pela **imputação pela mediana** em vez da exclusão das linhas, para não perder volume de dados (importante para o modelo KNN).

### 2.3 Engenharia de Atributos
* **Nova Variável:** Criamos a coluna `comprometimento_renda`, calculada pela divisão do montante do empréstimo pela renda anual, para normalizar o poder de pagamento do solicitante.

## 3. Pré-processamento e Preparação do Modelo

### 3.1 Encoding
* **Ação:** Convertemos variáveis categóricas para numéricas usando `get_dummies`. 
* **Justificativa:** Algoritmos de ML exigem input numérico. Utilizamos `drop_first=True` para evitar a "armadilha de variáveis dummy" (multicolinearidade).

### 3.2 Divisão dos Dados
* **Ação:** Dividimos a base em Treino (80%) e Teste (20%).
* **Justificativa:** O treino é usado para o aprendizado do modelo, e o teste para validar a performance com dados que o modelo nunca viu, garantindo que não estamos apenas "decorando" os dados (overfitting).

### 3.3 Normalização (Scaling)
* **Ação:** Utilizamos o `StandardScaler` para padronizar as features.
* **Justificativa:** O modelo KNN (que pretendemos usar) calcula distâncias euclidianas. Sem o escalonamento, variáveis com grande escala (ex: Renda) dominariam o cálculo, ignorando variáveis menores (ex: Idade).
