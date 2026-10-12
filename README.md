# painel-vendas-power-bi
# README: Painel de Vendas (Power BI)

Painel em Power BI que consolida 3 planilhas mensais de vendas e
atualiza com um clique.

## Problema

Uma loja fictícia com 3 pontos de venda (Centro, Shopping e Online)
recebe um CSV de vendas por mês. Hoje, alguém junta os arquivos à mão e
arruma os erros antes de ver qualquer número. O trabalho se repete todo
mês, é lento e dá margem para erro. Tempo manual estimado: de 3 a 5
horas.

## Dados

Todos os dados são fictícios, criados para este projeto. São 3 CSVs
mensais de vendas (julho a setembro de 2026) e um cadastro de 8
produtos, com 718 linhas de venda no total. Os arquivos chegam com
problemas propositais, comuns na vida real:

![Base de dados original](painel-vendas-power-bi/painel vendas power bi/imagens/base_dados.png)

| Problema                                         | Quanto    |
|--------------------------------------------------|-----------|
| Datas em 2 formatos (29/07/2026 e 2026-07-29)    | 63 linhas |
| Valor com "R\$" misturado ao número              | 38 linhas |
| Espaços e maiúsculas erradas em vendedor e loja  | várias    |
| Vendas duplicadas (mesmo id_venda)               | 9 linhas  |
| Linhas inválidas (valor "-" ou quantidade vazia) | 6 linhas  |

## O que eu fiz

**Limpeza no Power Query**

- Li todos os arquivos `vendas_*` de uma pasta, de forma que um mês novo
  entra sozinho na atualização.

- Converti valores e datas para o formato brasileiro e tratei os 2
  formatos de data.

- Padronizei vendedor e loja (sem espaços e com maiúsculas corretas).

- Removi 9 duplicatas e 6 linhas inválidas. De 718 linhas sobraram **703
  vendas válidas**.

- Juntei o cadastro de produtos para trazer categoria e custo, e
  calculei o custo de cada venda.

![Dados tratados no Power Query](imagens/dados_tratados.png)

**Modelo e medidas**

- Criei uma tabela de datas ligada às vendas.

- Medidas: Receita, Custo, Margem, Margem %, Nº de Vendas, Ticket Médio,
  Receita Mês Anterior e Variação %.

**Dashboard (uma página)**

- 4 cartões: Receita, Margem %, Ticket Médio e Variação %.

- Gráfico de linhas com a receita por mês.

- Barras de receita e de margem % por categoria, e barras de receita por
  vendedor.

- Filtros de mês, loja e forma de pagamento.

Layout planejado antes da construção:

![Layout do painel](imagens/layout_painel_vendas.png)

## Resultado

De 3 planilhas bagunçadas saiu um painel que atualiza com um clique.
Antes: de 3 a 5 horas de trabalho. Depois: atualizar o painel leva minutos.

Números do período (julho a setembro de 2026, todas as lojas):

| Indicador                  | Valor                     |
|----------------------------|---------------------------|
| Receita                    | R\$ 107.593,74            |
| Margem bruta               | 54,93%                    |
| Vendas válidas             | 703                       |
| Ticket médio               | R\$ 153,05                |
| Categoria que mais vende   | Vestuário (R\$ 47.057,85) |
| Categoria com maior margem | Acessórios (58,6%)        |
| Vendedor com maior receita | Diego (R\$ 30.572,78)     |

Por mês: julho R\$ 37.393,52, agosto R\$ 34.175,19 (−8,6%) e setembro
R\$ 36.025,03 (+5,4%).

![Painel pronto (setembro)](imagens/painel_pronto_set.png)

## Como atualizar

1.  Coloque o novo CSV do mês na pasta dados, com nome começando em
    `vendas_` (ex.: vendas_2026-10.csv).

2.  Abra o arquivo do painel no Power BI Desktop.

3.  Clique em **Atualizar** (guia Página Inicial).

Cada id_venda precisa ser único: se o mesmo id aparecer de novo, a venda
é tratada como duplicada e removida.

## Como ler o painel

| Medida       | O que mostra                                                      |
|--------------|-------------------------------------------------------------------|
| Receita      | Quanto a loja vendeu, já com os descontos                         |
| Margem %     | De cada R\$ 100 vendidos, quanto sobra depois do custo do produto |
| Ticket Médio | Quanto cada venda rendeu em média (Receita ÷ número de vendas)    |
| Variação %   | Quanto a receita cresceu ou caiu em relação ao mês anterior       |

Exemplo de leitura, com setembro selecionado: "Vendemos R\$ 36.025, 5,4%
a mais que em agosto. Foram 227 vendas, com ticket médio de R\$ 158,70."

## Limitações

- Os dados são fictícios e cobrem 3 meses de um só ano.

- A margem é bruta: considera só o custo do produto, sem impostos,
  frete, aluguel ou comissão.

- A Variação % só faz sentido com um único mês selecionado. Julho não
  tem variação, porque não há mês anterior.

- Linhas inválidas são removidas e não corrigidas. Neste projeto foram 6
  em 718.

- O ticket médio é por venda, não por cliente.
