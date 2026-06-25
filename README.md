# TrabalhoIA

# Relatório 

## 1. Análise Exploratória e Engenharia de Pré-processamento

A etapa inicial do projeto consistiu na análise exploratória dos dados (EDA), visando compreender a distribuição das variáveis, identificar possíveis inconsistências e avaliar o comportamento da variável alvo (*fraud_flag*).

Durante o processo de preparação dos dados, foram removidos atributos sem relevância preditiva direta, como identificadores únicos (*transaction_id* e *customer_id*), uma vez que esses campos não contribuem para a generalização dos modelos. Variáveis temporais também foram descartadas por não agregarem informação suficiente ao contexto analisado.

Adicionalmente, a variável *fraud_risk* foi removida para evitar problemas de **Data Leakage**, situação em que informações derivadas do próprio evento fraudulento poderiam influenciar artificialmente o treinamento dos modelos.

As variáveis numéricas foram normalizadas por meio da técnica **Z-Score**, garantindo que atributos financeiros e frequências transacionais permanecessem em escalas comparáveis. Esse procedimento foi especialmente importante para algoritmos baseados em distância, como o K-Nearest Neighbors (KNN), e para otimizar a convergência da Regressão Logística.

Por fim, os dados foram divididos em conjuntos de treinamento e teste utilizando amostragem estratificada, assegurando a preservação da distribuição das classes durante o processo de validação.

---

## 2. Avaliação Comparativa dos Modelos

Foram avaliadas três abordagens supervisionadas (Regressão Logística, Random Forest e KNN) e uma abordagem não supervisionada (Isolation Forest), utilizando métricas de Acurácia, Precisão, Recall, F1-Score e ROC-AUC.

### Regressão Logística

A Regressão Logística apresentou desempenho excepcional, alcançando métricas praticamente perfeitas no conjunto de teste.

A matriz de confusão demonstrou ausência de falsos positivos e falsos negativos, resultando em valores máximos de Precisão, Recall, F1-Score e ROC-AUC. Esse resultado evidencia que as variáveis selecionadas possuem elevada capacidade discriminatória para a identificação de fraudes no cenário analisado.

Além de sua alta performance, a Regressão Logística oferece excelente interpretabilidade, permitindo compreender o impacto de cada variável na decisão final do modelo.

---

### Random Forest

O Random Forest apresentou desempenho extremamente próximo ao ideal, registrando apenas três erros de classificação em todo o conjunto de teste.

A combinação de múltiplas árvores de decisão permitiu capturar relações complexas e não lineares entre variáveis comportamentais e financeiras, produzindo métricas superiores a 99,9% em praticamente todos os indicadores avaliados.

Sua robustez frente a ruídos e sua capacidade de modelar interações complexas tornam essa abordagem particularmente atrativa para ambientes reais de produção.

---

### K-Nearest Neighbors (KNN)

O modelo KNN apresentou desempenho satisfatório, alcançando aproximadamente 93% de Acurácia, Precisão, Recall e F1-Score.

Apesar dos resultados positivos, observou-se uma quantidade significativamente maior de falsos positivos e falsos negativos quando comparado à Regressão Logística e ao Random Forest.

Essa diferença ocorre porque o KNN depende diretamente da proximidade entre observações no espaço de atributos. Em problemas complexos de fraude, nos quais as fronteiras de decisão podem ser altamente não lineares, essa estratégia tende a ser menos eficiente do que métodos baseados em árvores ou modelos probabilísticos.

---

### Isolation Forest

O Isolation Forest foi avaliado como abordagem não supervisionada para detecção de anomalias.

Embora tenha conseguido identificar uma parcela relevante das fraudes, apresentou desempenho significativamente inferior aos modelos supervisionados, registrando aproximadamente 67% de Acurácia, Precisão, Recall e F1-Score.

A matriz de confusão revelou um volume elevado de falsos positivos e falsos negativos, indicando que o modelo teve dificuldades para separar transações legítimas e fraudulentas apenas com base em padrões de anomalia.

Entretanto, seu principal diferencial está na capacidade de detectar comportamentos atípicos sem depender de exemplos previamente rotulados. Essa característica o torna uma ferramenta complementar valiosa para identificação de fraudes inéditas (*zero-day frauds*) e novos padrões de ataque ainda não observados durante o treinamento.

---

## 3. Comparação Geral dos Resultados

A comparação entre os modelos evidencia uma clara superioridade das abordagens supervisionadas para o conjunto de dados utilizado.

A Regressão Logística apresentou desempenho perfeito nos dados de teste, enquanto o Random Forest obteve resultados praticamente equivalentes, cometendo apenas três erros de classificação.

O KNN demonstrou boa capacidade preditiva, porém ficou abaixo dos dois melhores modelos devido ao maior número de erros de classificação.

Já o Isolation Forest apresentou desempenho consideravelmente inferior, reforçando que métodos não supervisionados tendem a ser mais adequados como mecanismos complementares de monitoramento e descoberta de padrões anômalos do que como solução principal para classificação de fraudes.

---

# Escolha do Modelo para Produção

Considerando os resultados obtidos, duas alternativas se destacam para implantação em ambiente produtivo: a Regressão Logística e o Random Forest.

Embora a Regressão Logística tenha alcançado desempenho perfeito no conjunto de teste, o Random Forest apresenta maior capacidade de generalização em cenários reais, especialmente diante de relações complexas e não lineares entre variáveis de risco.

Dessa forma, recomenda-se a adoção do **Random Forest integrado ao pipeline de pré-processamento desenvolvido**, utilizando o **ColumnTransformer** para tratamento consistente das variáveis numéricas, categóricas e comportamentais.

Além disso, o Isolation Forest pode ser implementado paralelamente como uma camada complementar de monitoramento de anomalias, ampliando a capacidade do sistema de detectar padrões de fraude inéditos que eventualmente não estejam representados nos dados históricos.

Essa combinação oferece um equilíbrio adequado entre precisão operacional, robustez estatística e capacidade de adaptação a novas ameaças no ambiente de e-commerce.
