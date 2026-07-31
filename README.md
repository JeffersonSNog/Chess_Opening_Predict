# Chess Opening Predict

Modelo de Machine Learning para prever a probabilidade de vitória das peças brancas em uma partida de xadrez, com base na abertura jogada (código ECO), no número de lances da abertura (opening ply) e na diferença de rating entre os jogadores.

## Objetivo

Responder à pergunta: dado que as brancas abriram com determinada abertura, contra um oponente de rating conhecido, qual a probabilidade de vitória?

## Dataset

Dados de partidas do Lichess (`data/raw/games.csv`), contendo resultado da partida, abertura jogada (código ECO), quantidade de lances na fase de abertura e ratings dos jogadores.

Fonte: [Chess Game Dataset (Lichess)](https://www.kaggle.com/datasets/datasnaek/chess), disponível no Kaggle.

> **Nota:** a pasta `data/` está no `.gitignore` e não é versionada no repositório. Baixe o dataset no link acima e coloque o arquivo como `data/raw/games.csv` antes de rodar o notebook.

Após limpeza (remoção de nulos, duplicatas e colunas irrelevantes) e engenharia de features (criação da coluna `white_rating_diff`), o dataset processado fica disponível em `data/processed/chess_clean.csv`.

## Features utilizadas

- `opening_eco`: código ECO (Encyclopaedia of Chess Openings) da abertura jogada, transformado via target encoding
- `opening_ply`: número de lances na fase de abertura
- `white_rating_diff`: diferença de rating entre jogador das brancas e das pretas

## Target

- `winner`: resultado da partida (vitória das brancas ou não)

## Metodologia

1. Análise exploratória (`notebooks/eda.ipynb`): checagem de nulos, duplicatas, tipos, cardinalidade e distribuição das features
2. Split treino/teste estratificado (80/20), preservando a proporção de vitórias
3. Target encoding do `opening_eco`: cada categoria substituída pela taxa histórica de vitória das brancas, calculada apenas no conjunto de treino para evitar vazamento de dados (data leakage)
4. Treinamento e comparação de modelos (baseline com Logistic Regression, seguido de Random Forest e Gradient Boosting)
5. Avaliação com métricas de classificação (F1, AUC, matriz de confusão)

## Estrutura do projeto

```
Chess_Opening_Predict/
├── app/
│   └── main.py              # Aplicação/API de servir o modelo
├── data/
│   ├── raw/
│   │   └── games.csv        # Dataset original
│   └── processed/
│       ├── chess_clean.csv  # Dataset limpo
│       ├── X_train.csv
│       ├── X_test.csv
│       ├── y_train.csv
│       └── y_test.csv
├── notebooks/
│   └── eda.ipynb            # Análise exploratória e preparação dos dados
├── src/
│   ├── preprocess.py        # Funções de pré-processamento reutilizáveis
│   └── train.py             # Script de treinamento do modelo
├── tests/
│   ├── test_api.py
│   └── test_preprocess.py
├── requirements.txt
└── README.md
```

## Status atual

- [x] Limpeza e análise exploratória dos dados
- [x] Split treino/teste estratificado
- [x] Target encoding sem vazamento de dados
- [ ] Treinamento do modelo baseline
- [ ] Comparação entre modelos
- [ ] Avaliação final e seleção do modelo campeão
- [ ] Empacotamento em API (FastAPI)

## Como rodar

```bash
pip install -r requirements.txt
```

Notebook de exploração e preparação:

```bash
jupyter notebook notebooks/eda.ipynb
```

## Autor

Jefferson (JeffersonSNog)