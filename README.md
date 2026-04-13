# Dashboard-bi-directy

Dashboard Power BI (Directy) - Análise de Faturamento e Metas 2017-2019


🗂️ Estrutura do Repositório



dashboard-bi-faturamento/

├── dados/

│   ├── vendas.xlsx

│   ├── metas.xlsx

│   ├── clientes.xlsx

│   ├── produtos.csv

│   └── ...

├── dashboard/

│   └── Dashboard-Directy.pbix

├── docs/

|   ├── Desafio Vendas x Metas.pdf
│   ├── Inicio.png
|   ├── Comparativo.png
|   └── modelo dados.png

└── README.md



---



🎯 Objetivo



Desenvolver um dashboard no Power BI que permita:



\- Analisar a evolução do faturamento e compará-lo com as metas estabelecidas

\- Apresentar uma visão geral do período completo

\- Realizar uma análise comparativa entre os anos de 2017 e 2018, gerando insights para revisão das metas de 2019



---



🧩 Modelo de Dados



!\[Modelo de Dados](docs/modelo\_dados.png)



Relacionamentos:

\- dCalendário → Vendas

\- Subcategoria → Vendas
- Produto → Vendas

\- Cliente → Vendas
- Localização → Vendas
- 2017 → Metas
- 2018 → Metas
- 2019 → Metas
- Metas → Subcategoria
- Medidas



---



⚙️ Principais Medidas DAX





% Maior Categoria = 

VAR MaiorCat = \[Maior Categoria]

VAR FaturamentoMaior = 

&nbsp;   CALCULATE(

&nbsp;       \[Faturamento],

&nbsp;       Subcategoria\[Categoria] = MaiorCat

&nbsp;   )

VAR FaturamentoTotal = 

&nbsp;   CALCULATE(\[Faturamento], ALL(Subcategoria\[Subcategoria]))

RETURN

&nbsp;   DIVIDE(FaturamentoMaior, FaturamentoTotal, 0)


% Meta = DIVIDE(\[Faturamento], \[Meta Total], 0)

% Variação\_Homóloga YoY = 

DIVIDE(

&nbsp;   \[Variação\_Entre\_Anos],

&nbsp;   \[Faturamento Ano Anterior],

&nbsp;   0

)



Faturamento = SUM(Vendas\[Valor\_da\_Venda])

Faturamento Ano Anterior = 

&nbsp;   CALCULATE(\[Faturamento], SAMEPERIODLASTYEAR('dCalendário'\[Date]))

Faturamento Nao Vazio = 

IF(

&nbsp;   ISBLANK(\[Faturamento]),

&nbsp;   0,                      

&nbsp;   1

)



Filtrar Top 3 Subcategorias = 

VAR RankingAtual = \[Ranking Subcategoria]

RETURN

&nbsp;   IF(RankingAtual <= 3, 1, 0)

Maior Categoria = 

TOPN(

&nbsp;   1,

&nbsp;   VALUES(Subcategoria\[Categoria]),

&nbsp;   CALCULATE(SUM(Vendas\[Valor\_da\_Venda])),

&nbsp;   DESC

)

Meta Total = SUM(Metas\[Valor])

Ranking Subcategoria = 

RANKX(

&nbsp;   ALL(Subcategoria\[Subcategoria]),

&nbsp;   \[Faturamento],

&nbsp;   ,

&nbsp;   DESC,

&nbsp;   DENSE

)

Top 3 Subcategorias = 

CONCATENATEX(

&nbsp;   TOPN(3, VALUES(Subcategoria\[Subcategoria]), \[Faturamento], DESC),

&nbsp;   Subcategoria\[Subcategoria],

&nbsp;   ", "

)

Variação Positiva = 

IF(\[Variação\_Entre\_Anos] > 0, "✅ Positiva", "❌ Negativa")

Variação\_Entre\_Anos = \[Faturamento] - \[Faturamento Ano Anterior]





---



📋 Páginas do Dashboard



Página 1 — Visão Geral





\[Inicio](docs/inicio.png)



\*\*Recursos:\*\*

\- Faturamento total do período

\- Meta total e % de atingimento

\- Faturamento por categoria
- Top 3 subcategorias por faturamento

\- Faturamento por continente nas top 3 subcategorias



---



Página 2 — Comparativo Anual 



\[Comparativo](docs/comparativo.png)



\*\*Recursos:\*\*

\- Variação de faturamento 2017 → 2018

\- Variação por subcategoria com formatação condicional

\- Análise de subcategorias por continente

\- Faturamento do ultimo ano
- Variação do Faturamento atual e o faturamento do ano anterior
- Porcentagem da Variação Homóloga



---



📊 Principais Resultados





Visão Geral

| Indicador | Valor |

|---|---|

| Faturamento Total | R$ 171,38 MI|

| Meta Total | R$ 205,02 MI |

| % Atingimento da Meta | 83,59 % |

| Categoria com Maior Faturamento | Computers |

| Representatividade da Categoria | 74,33 % |

| Top 3 Subcategorias | 1. Projectors \& Screens / 2. Laptops / 3. Desktops |



\### Comparativo 2017 vs 2018

| Indicador | Valor |

|---|---|

| Faturamento 2017 | R$ 67,2 MI |

| Faturamento 2018 | R$ 85,47 MI |

| Variação R$ | R$ 18,27 MI |

| Variação % | 27,19 % |

| Categoria com Maior Faturamento em 2018 | Projectors \& Screens |

| Categoria com Maior Variação % | Home Theater System |

| Subcategoria com Variação Negativa | Desktops |

| Continente responsável pela queda em Desktops | Europe |



---



🛠️ Tecnologias Utilizadas



\- \[Power BI Desktop](https://powerbi.microsoft.com/)

\- DAX (Data Analysis Expressions)

\- Power Query (ETL e transformação dos dados)



---



👤 Autor



Wesley Wernersbach Aranha  

Data Analyst  

\[LinkedIn](https://linkedin.com/in/wesley-wernersbach) · \[GitHub](https://github.com/WesWerner)



---



📅 Período dos Dados



`01/01/2017` → `16/03/2019`



