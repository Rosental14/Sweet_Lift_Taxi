# Previsão de Demanda de Táxis da Sweet Lift

## Descrição do Projeto
A empresa Sweet Lift Taxi coletou dados históricos sobre pedidos de táxi nos aeroportos. Para atrair mais motoristas durante o horário de pico, precisamos prever a quantidade de pedidos de táxi para a próxima hora. Construímos diversos modelos preditivos para alcançar este objetivo, avaliando-os com base na REQM (Raíz do Erro Quadrático Médio) e no tempo de treinamento.

## Índice
1. [Preparação](#preparação)
2. [Análise dos Dados](#análise-dos-dados)
    - [Decomposição](#decomposição)
    - [Diferenças de Séries Temporais](#diferenças-de-séries-temporais)
    - [Criação de Características](#criação-de-características)
    - [Divisão dos Dados](#divisão-dos-dados)
3. [Treinamento e Avaliação dos Modelos](#treinamento-e-avaliação-dos-modelos)
    - [Regressão Linear](#regressão-linear)
    - [Floresta Aleatória](#floresta-aleatória)
    - [CatBoost](#catboost)
    - [LightGBM](#lightgbm)
    - [XGBoost](#xgboost)
4. [Resultados](#resultados)
5. [Testes Adicionais](#testes-adicionais)
    - [Análise de REQM](#análise-de-rmse)
    - [Análise de Tempo de Treinamento](#análise-de-tempo-de-treinamento)
6. [Conclusão](#conclusão)

## Preparação
Os dados foram reamostrados para intervalos de uma hora para melhor adequação aos modelos de previsão.

## Análise dos Dados

### Decomposição
Os dados foram decompostos em três componentes principais: tendência, sazonalidade e resíduos. A análise mostrou que a demanda de táxis segue um padrão diário, com picos e vales significativos ao longo do dia.

### Diferenças de Séries Temporais
Para lidar com a sazonalidade e tendência, criamos uma nova coluna com a diferença das séries temporais.

### Criação de Características
Criamos várias características a partir dos dados originais, incluindo:
- Dia da semana
- Dia do mês
- Mês
- Hora do dia
- Média móvel

### Divisão dos Dados
Os dados foram divididos em conjuntos de treinamento, validação e teste, utilizando 80% dos dados para treinamento, 10% para validação e 10% para teste.

## Treinamento e Avaliação dos Modelos

### Regressão Linear
A Regressão Linear foi utilizada como modelo base. No entanto, em alguns dos testes os resultados apresentaram sinais de overfitting.

### Floresta Aleatória
Um modelo de Floresta Aleatória foi treinado, mostrando melhorias em relação à Regressão Linear, quando trabalhado com dados originais e diferenças de séries temporais.

### CatBoost
O modelo CatBoost apresentou o melhor desempenho em termos de REQM, apesar de um tempo de treinamento mais longo.

### LightGBM
O LightGBM também mostrou bons resultados, com um equilíbrio entre tempo de treinamento e REQM, porém, o REQM obtido foi um pouco pior do que o CatBoost.

### XGBoost
O XGBoost foi testado e apresentou resultados competitivos, mas com um tempo de treinamento muito mais longo.

## Resultados
Os resultados mostraram que o modelo CatBoost obteve o menor RMSE, tornando-se a melhor escolha para previsão de demanda de táxis. O tempo de treinamento, embora maior que os outros modelos, foi considerado aceitável devido à precisão alcançada.

## Testes Adicionais

### Análise de REQM
Foram realizados testes adicionais com diferentes conjuntos de características:
- `num_orders`: treinamento dos modelos usando apenas a coluna num_orders com os dados originais;
- `diff`: treinamento dos modelos usando apenas a coluna diff com dados das diferenças de séries temporais;
- `diff_24`: treinamento dos modelos usando a coluna diff + características + lag=24 + rolling_mean=24;
- `diff_100`: treinamento dos modelos usando a coluna diff + características + lag=100 + rolling_mean=100;
- `both_100`: treinamento dos modelos usando colunas diff + num_orders + características + lag=100 + rolling_mean=100;
- `both_24`: treinamento dos modelos usando coluna diff + num_orders + características + lag=24 + rolling_mean=24.

### Análise de Tempo de Treinamento
O tempo de treinamento foi analisado para cada conjunto de características e modelo, destacando-se a Regressão Linear com o menor tempo, mas com resultados de REQM não confiáveis.

## Conclusão
O modelo CatBoost foi o melhor desempenho para a previsão de demanda de táxis, considerando o REQM e o tempo de treinamento. A análise de diferentes conjuntos de características demonstrou que a combinação de dados originais com diferenças de séries temporais, além das demais características criadas, como `dia`, `dia da semana`, `mês`, `média móvel` e `dados de defasagem` produziu os melhores resultados.