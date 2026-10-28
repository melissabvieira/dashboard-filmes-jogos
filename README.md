# Dashboard: filmes baseados em jogos de videogame

## Fonte e recorte

Dados obtidos em 27/10/2026 da [lista de filmes baseados em jogos de videogame da Wikipedia](https://en.wikipedia.org/wiki/List_of_films_based_on_video_games), por leitura HTML com `pandas.read_html`. O conjunto contém 68 filmes das tabelas em inglês **Live-action** (55 linhas) e **Animated** (13 linhas). Tabelas de próximos lançamentos, outras listas sem o conjunto completo de colunas e tabelas de outros idiomas não fazem parte deste recorte.

Orçamento e bilheteria são valores aproximados segundo a própria Wikipedia. O dashboard soma apenas os 67 valores de bilheteria disponíveis, sem estimar os ausentes. Faixas de orçamento (por exemplo, intervalos em US$) não foram reduzidas a um ponto médio; orçamento em libra esterlina não foi convertido para US$. O texto original destes dois campos foi mantido em `orcamento_fonte` e `bilheteria_fonte`. Os campos numéricos correspondentes ficam vazios nesses casos. Referências entre colchetes foram retiradas apenas dos valores normalizados.

## Limpeza

- Datas completas são normalizadas para `AAAA-MM-DD`; ano inteiro e década são derivados da data. Datas ausentes ou não interpretáveis ficam vazias.
- Orçamentos e bilheterias em dólares são expressos em milhões de US$. Valores em faixa, em outra moeda, ausentes ou não reconhecidos ficam vazios, sem estimativa ou conversão externa.
- Rotten Tomatoes mantém somente o percentual numérico; quantidade de reviews não entra na nota. `N/A` é vazio. Metacritic mantém somente a nota de 0 a 100.
- Publisher e demais campos textuais mantêm o conteúdo da tabela; valores `N/A` ficam vazios. Publisher é contado conforme o nome registrado na fonte.
- Em cada gráfico, só entram linhas com todos os campos necessários. A contagem efetiva e a razão de exclusão constam na nota do respectivo cartão.

## Filmes usados e excluídos por gráfico

| Gráfico | Uso dos dados e exclusões |
|---|---|
| Década | n=68; 0 sem ano |
| Nota média por período | n=64; 4 sem ano ou nota |
| Orçamento x bilheteria | n=50; 18 sem orçamento ou bilheteria numéricos |
| 10 maiores bilheterias | n=67; 1 sem bilheteria; mostra os 10 maiores |
| Publishers | n=68; 0 sem publisher; mostra os 5 maiores |
| Distribuição Rotten Tomatoes | n=64; 4 sem nota numérica |
| Recuperou orçamento | n=50; 18 sem orçamento ou bilheteria numéricos |

## Pergunta → tipo de gráfico → por que

| Pergunta | Tipo de gráfico | Por que esse tipo |
|---|---|---|
| Quantos filmes baseados em jogos foram lançados em cada década? | Barras verticais | Compara contagens entre categorias temporais; eixo começa em zero. |
| A nota média no Rotten Tomatoes mudou ao longo dos anos? | Linha | Mostra a tendência da média em períodos de cinco anos. |
| Orçamento maior significa bilheteria maior? | Dispersão | Compara duas variáveis numéricas por filme; escala logarítmica. |
| Quais são as 10 maiores bilheterias entre os filmes baseados em jogos? | Barras horizontais | Ordena e compara os dez maiores valores com leitura dos títulos. |
| Quais publishers de jogos têm mais adaptações para o cinema? | Barras horizontais | Compara a frequência dos cinco publishers mais recorrentes. |
| Como se distribuem as notas do Rotten Tomatoes? | Histograma (faixas de 10 pontos) | Mostra a frequência das notas em intervalos numéricos. |
| Quantos filmes recuperaram o orçamento na bilheteria? | Barras | Compara duas contagens definidas pela regra bilheteria ≥ orçamento. |

## Arquivos

- `dashboard_filmes_jogos.html`: dashboard autônomo, com Plotly.js incorporado no HTML.
- `dados_filmes_jogos.csv`: tabela limpa em UTF-8 com BOM.
- `dados_filmes_jogos.xlsx`: tabela na aba `dados` e metadados/explicações na aba `fonte`.
