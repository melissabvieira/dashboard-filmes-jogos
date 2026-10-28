# Dashboard: filmes baseados em jogos de videogame

Dashboard em HTML que responde a 7 perguntas sobre adaptações de jogos para o cinema. Atividade de Visualização de Dados.

**Como abrir:** baixe `dashboard_filmes_jogos.html` e abra no navegador. Não precisa instalar nada, e funciona sem internet.

## Fonte dos dados

- **Origem:** [List of films based on video games (Wikipedia)](https://en.wikipedia.org/wiki/List_of_films_based_on_video_games)
- **Data de obtenção:** 27/10/2026, por leitura HTML com `pandas.read_html`
- **Recorte:** 68 filmes das tabelas em inglês Live-action (55) e Animated (13). Ficaram de fora próximos lançamentos e tabelas de outros idiomas.
- **Atenção:** orçamento e bilheteria são valores aproximados, segundo a própria Wikipedia.

## Tratamento dos dados

- Datas viram `AAAA-MM-DD`; ano e década são derivados da data.
- Orçamento e bilheteria em **milhões de US$**. Faixas de valor, outras moedas (ex.: libra) e valores ausentes ficam **vazios**, sem estimativa ou conversão. O texto original foi mantido em `orcamento_fonte` e `bilheteria_fonte`.
- Rotten Tomatoes: só o percentual. Metacritic: só a nota de 0 a 100. `N/A` vira vazio.
- Em cada gráfico entram só os filmes com todos os campos necessários. A contagem e o motivo das exclusões aparecem na nota de cada cartão.

## Gráficos

| Pergunta | Gráfico | Filmes usados |
|---|---|---|
| Quantos filmes foram lançados em cada década? | Barras verticais | 68 |
| A nota média no Rotten Tomatoes mudou ao longo dos anos? | Linha | 64 |
| Orçamento maior significa bilheteria maior? | Dispersão (escala log) | 50 |
| Quais são as 10 maiores bilheterias? | Barras horizontais | 67 |
| Quais publishers têm mais adaptações? | Barras horizontais (top 5) | 68 |
| Como se distribuem as notas do Rotten Tomatoes? | Histograma (faixas de 10) | 64 |
| Quantos filmes recuperaram o orçamento? | Barras | 50 |

Cada tipo foi escolhido pelo objetivo da pergunta: tendência (linha), comparação (barras), relação entre duas variáveis (dispersão) e distribuição (histograma).

## Princípios de visualização

Eixo em zero nas barras, paleta única sem arco-íris, sem 3D nem pizza, no máximo 5 categorias nos rankings, rótulos de valor (a informação não depende só da cor) e título e unidade em todos os eixos.

## Limitações

- Orçamento e bilheteria são aproximados e vêm de fonte colaborativa.
- Os gráficos de dispersão e de "recuperou o orçamento" usam só os 50 filmes com os dois valores em dólar.
- "Recuperou o orçamento" significa bilheteria ≥ orçamento, e não considera marketing nem a divisão da receita com os cinemas.

## Arquivos

- `dashboard_filmes_jogos.html`: dashboard com Plotly.js incorporado
- `dados_filmes_jogos.csv`: tabela limpa (UTF-8)
- `dados_filmes_jogos.xlsx`: aba `dados` com a tabela e aba `fonte` com as explicações das colunas
