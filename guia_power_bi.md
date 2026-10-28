# Guia rápido: refazer os gráficos no Power BI Desktop

## 1. Importar e preparar

1. Abra o Power BI Desktop e use **Obter dados → Excel** para selecionar `dados_filmes_jogos.xlsx`.
2. Selecione apenas a aba `dados` e clique em **Transformar dados**. Também é possível carregar `fonte` como tabela de documentação.
3. Configure `data_lancamento` como Data; `ano_lancamento` como Número inteiro; `orcamento_milhoes_usd`, `bilheteria_mundial_milhoes_usd`, `rotten_tomatoes_pct` e `metacritic` como Número decimal. Configure `titulo`, `decada`, `publisher_original` e demais campos descritivos como Texto.
4. Preserve os valores vazios como `null`; não substitua por zero. Os campos `*_fonte` são textos de auditoria e não devem alimentar medidas.
5. Se desejar, renomeie a tabela importada para `Filmes`.

## 2. Medidas DAX

```DAX
Total de filmes = COUNTROWS(Filmes)

Bilheteria informada (mi US$) =
SUM(Filmes[bilheteria_mundial_milhoes_usd])

Média Rotten Tomatoes (%) =
AVERAGE(Filmes[rotten_tomatoes_pct])

Filmes com orçamento e bilheteria =
COUNTROWS(
    FILTER(
        Filmes,
        NOT ISBLANK(Filmes[orcamento_milhoes_usd])
            && NOT ISBLANK(Filmes[bilheteria_mundial_milhoes_usd])
    )
)

Recuperou orçamento =
COUNTROWS(
    FILTER(
        Filmes,
        NOT ISBLANK(Filmes[orcamento_milhoes_usd])
            && NOT ISBLANK(Filmes[bilheteria_mundial_milhoes_usd])
            && Filmes[bilheteria_mundial_milhoes_usd] >= Filmes[orcamento_milhoes_usd]
    )
)
```

Crie também `Não recuperou orçamento` como contagem filtrada pela mesma condição de campos preenchidos e bilheteria menor que orçamento. A classificação usa os valores aproximados da fonte e a regra explícita `bilheteria ≥ orçamento`.

## 3. Visuais

| Pergunta | Visual e campos |
|---|---|
| Quantos filmes por década? | Gráfico de colunas agrupadas; Eixo X `decada`, Eixo Y contagem de `titulo`. |
| A média Rotten Tomatoes mudou? | Gráfico de linhas; Eixo X `ano_lancamento` agrupado em intervalos de 5 anos (ou `data_lancamento`), Eixo Y média de `rotten_tomatoes_pct`; filtro para valores não vazios. |
| Orçamento maior significa bilheteria maior? | Gráfico de dispersão; X `orcamento_milhoes_usd`, Y `bilheteria_mundial_milhoes_usd`, Detalhes `titulo`; filtrar valores não vazios e habilitar escala logarítmica em ambos os eixos. |
| 10 maiores bilheterias? | Gráfico de barras agrupadas; Eixo Y `titulo`, Eixo X `bilheteria_mundial_milhoes_usd`; filtro Top N = 10 por bilheteria, ordem decrescente. |
| Publishers com mais adaptações? | Gráfico de barras agrupadas; Eixo Y `publisher_original`, Eixo X contagem de `titulo`; filtro Top N = 5, ordem decrescente. |
| Distribuição das notas? | Histograma (visual de histograma do AppSource, se disponível) com `rotten_tomatoes_pct`, largura de compartimento 10; alternativa nativa: colunas com grupos de 10 pontos. Filtrar nota vazia. |
| Quantos recuperaram o orçamento? | Gráfico de colunas com as medidas `Recuperou orçamento` e `Não recuperou orçamento`, ou eixo com categoria calculada e contagem. |

Para contar exclusões, use filtros de campos não vazios correspondentes a cada visual. Informe ao lado de cada gráfico o n efetivamente usado e a razão das exclusões; no Top 10 e Top 5, registre também que o visual limita as categorias.

## 4. Aparência e validação

- Paleta consistente: azul `#1f4e79`, cinza `#8a8f98` e laranja `#e07b00` apenas para destaque; não use arco-íris.
- Ative rótulos de dados para barras e títulos/unidades nos eixos. Eixos de contagem e bilheteria em barras devem iniciar em zero; não aplique truncamento.
- Use cartões para total de filmes, período, soma da bilheteria informada e média Rotten Tomatoes. Rotule bilheteria como aproximada e inclua fonte e consulta em 27/10/2026.
- O gráfico de recuperação compara orçamento de produção com bilheteria aproximada e não é lucro nem retorno líquido.
