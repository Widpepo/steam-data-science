
# Análise e Classificação de Aplicativos da Steam

Projeto acadêmico de Ciência de Dados que investiga se informações de preço e avaliações dos usuários podem ser utilizadas para classificar aplicativos da Steam em categorias de menor ou maior alcance estimado.

## Objetivo e problema

A questão investigada é:

> É possível classificar um aplicativo da Steam como de menor ou maior alcance estimado utilizando seu preço e a quantidade de avaliações positivas e negativas?

Neste projeto, **alcance estimado** é definido a partir da faixa de proprietários (`estimated_owners`) disponibilizada no dataset. Portanto, a variável criada não representa vendas exatas nem usuários ativos.

## Dados

- **Fonte:** [Steam Games Dataset 2025 — Kaggle](https://www.kaggle.com/datasets/artermiloff/steam-games-dataset)
- **Base original:** 94.948 registros e 47 colunas.
- **Base após a remoção dos registros com `estimated_owners = 0 - 0`:** 81.292 registros.
- **Categorias:** 59.379 registros de menor alcance e 21.913 de maior alcance.

Consulte [data/README.md](data/README.md) para informações sobre obtenção e organização dos dados. O arquivo original do dataset não é incluído neste repositório.

## Metodologia

O trabalho foi desenvolvido nas seguintes etapas:

1. Obtenção e inspeção do dataset.
2. Seleção das variáveis relevantes e preparação dos dados.
3. Remoção de registros sem estimativa útil de proprietários (`0 - 0`).
4. Análise exploratória e criação de visualizações.
5. Transformação logarítmica das avaliações positivas e negativas com `log1p`.
6. Padronização das variáveis com `StandardScaler`.
7. Treinamento, validação e avaliação de um modelo K-Nearest Neighbors (KNN).
8. Web Scraping de uma pequena amostra de páginas da Steam, utilizando Requests e BeautifulSoup.

### Variáveis do modelo

| Tipo | Variável | Descrição |
|---|---|---|
| Entrada | `price` | Preço do aplicativo |
| Entrada | `positive` | Quantidade de avaliações positivas |
| Entrada | `negative` | Quantidade de avaliações negativas |
| Alvo | `popularidade` | Categoria derivada de `estimated_owners` |

A variável-alvo foi definida como:

| Classe | Critério |
|---|---|
| 0 — Menor alcance | Faixa de 0 a 20.000 proprietários |
| 1 — Maior alcance | Faixas acima de 20.000 proprietários |

## Modelo e resultados

Foi utilizado o algoritmo **K-Nearest Neighbors (KNN)**. Foram comparados valores de K iguais a 3, 5, 7, 9 e 11, com ponderação `uniform` e `distance`.

A configuração escolhida na validação foi **K = 11**, com pesos **uniform**.

Resultados da avaliação final no conjunto de teste:

| Métrica | Menor alcance | Maior alcance |
|---|---:|---:|
| Precisão | 0,88 | 0,83 |
| Recall | 0,95 | 0,63 |
| F1-score | 0,91 | 0,72 |

**Acurácia geral: 86,59%.**

Apesar da acurácia, o modelo teve maior dificuldade para identificar aplicativos de maior alcance. A distribuição desigual entre as categorias deve ser considerada na interpretação dos resultados.

## Web Scraping

Foi realizada uma coleta demonstrativa em três páginas de aplicativos da Steam. Com Requests e BeautifulSoup, foram extraídos campos como nome, data de lançamento, gêneros e desenvolvedoras. Os resultados foram organizados em CSV e comparados com o dataset original pelo AppID.

Essa amostra é pequena, tem finalidade demonstrativa e não foi utilizada no treinamento do KNN. O campo de publicadora não retornou dados para as páginas consultadas.

## Tecnologias e bibliotecas

- Python
- Google Colab
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Requests
- BeautifulSoup

As dependências Python estão listadas em [requirements.txt](requirements.txt).

## Estrutura do repositório

```text
steam-data-science/
├── README.md
├── notebook/
├── data/
│   └── README.md
├── reports/
│   └── README.md
├── presentation/
│   └── README.md
├── results/
│   └── README.md
├── requirements.txt
└── .gitignore
```

Os notebooks, relatório, apresentação, gráficos e resultados exportados serão adicionados às respectivas pastas conforme a organização do projeto for concluída.

## Como reproduzir

1. Baixe o dataset na página do Kaggle indicada acima.
2. Abra o notebook da pasta `notebook/` no Google Colab.
3. Disponibilize o arquivo CSV no ambiente de execução, conforme o caminho utilizado no notebook.
4. Instale as dependências listadas em `requirements.txt`, se necessário.
5. Execute as células na ordem apresentada.

Consulte também [data/README.md](data/README.md) para orientações sobre os dados.

## Limitações

- A variável-alvo utiliza faixas estimadas de proprietários, não números exatos de vendas ou usuários ativos.
- As categorias apresentam desequilíbrio de quantidade.
- O recall da classe de maior alcance foi inferior ao da classe de menor alcance.
- O Web Scraping foi realizado em apenas três páginas e não representa todo o catálogo da Steam.
- O conjunto de teste foi consultado em uma avaliação inicial antes da avaliação final; por isso, não deve ser considerado totalmente inédito.

## Finalidade

Projeto desenvolvido para fins acadêmicos, com aplicação prática de preparação e análise de dados, visualização, classificação supervisionada e Web Scraping.
