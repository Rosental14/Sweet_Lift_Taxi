# Previsão de Demanda de Táxis da Sweet Lift

## Descrição do Projeto
A empresa Sweet Lift Taxi coletou dados históricos sobre pedidos de táxi nos aeroportos. Para atrair mais motoristas durante o horário de pico, precisamos prever a quantidade de pedidos de táxi para a próxima hora. Construímos diversos modelos preditivos para alcançar este objetivo, avaliando-os com base na REQM (Raíz do Erro Quadrático Médio) e no tempo de treinamento.

## Índice
- [Previsão de Demanda de Táxis da Sweet Lift](#previsão-de-demanda-de-táxis-da-sweet-lift)
  - [Descrição do Projeto](#descrição-do-projeto)
  - [Índice](#índice)
  - [Preparação](#preparação)
  - [Análise dos Dados](#análise-dos-dados)
    - [Decomposição](#decomposição)
    - [Diferenças de Séries Temporais](#diferenças-de-séries-temporais)
    - [Criação de Características](#criação-de-características)
    - [Divisão dos Dados](#divisão-dos-dados)
  - [Treinamento e Avaliação dos Modelos](#treinamento-e-avaliação-dos-modelos)
    - [Regressão Linear](#regressão-linear)
    - [Floresta Aleatória](#floresta-aleatória)
    - [CatBoost](#catboost)
    - [LightGBM](#lightgbm)
    - [XGBoost](#xgboost)
  - [Resultados](#resultados)
  - [Testes Adicionais](#testes-adicionais)
  - [Conclusão](#conclusão)

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
O LightGBM também mostrou bons resultados, com um equilíbrio entre tempo de treinamento e REQM, porém, o REQM obtido foi pior do que o do CatBoost e também do XGBoost.

### XGBoost
O XGBoost foi testado e apresentou resultados competitivos de REQM e com um tempo de treinamento mais adequado do que o do CatBoost.

## Resultados
Os resultados mostraram que o modelo XGBoost apresentou melhor equilíbrio entre REQM e tempo de treinamento, tornando-se a melhor escolha para previsão de demanda de táxis.

## Testes Adicionais


Foram realizados testes adicionais com diferentes conjuntos de características:

`created_24` - treinamento dos modelos usando as características criadas (exceto a coluna 'diff') + lag=24 + rolling_mean=24,

`diff_24`   - treinamento dos modelos usando as características criadas + coluna 'diff' + lag=24 + rolling_mean=24,

`diff_100`  - treinamento dos modelos usando as características criadas + coluna 'diff' + lag=100 + rolling_mean=100,


## Conclusão
Ficou evidente que a coluna 'diff', com as diferenças das séries temporais apresentou uma importância muito grande na melhoria dos modelos, sendo que os testes que retornaram os melhores REQM foram os que incluíram esta coluna.

O aumento das colunas de dados de defasagem de 24 para 100, assim como o aumento da média móvel de 24 para 100, refletiram em uma melhora irrelevante no REQM se comparado ao prejuízo que elas trouxeram em relação ao tempo para o treinamento do modelo

Após analisar todos os testes, **o modelo XGBoost do teste diff_24**  (incluindo todas as características criadas + coluna 'diff' + lag=24 + rolling_mean=24), obteve o melhor desempenho para a previsão de demanda de táxis, considerando o equilíbrio entre REQM e o tempo de treinamento.