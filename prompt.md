# Prompts usados para gerar a dashboard

## Prompt inicial (v1)

```
Você é um analista de dados e designer de dashboards.

Anexei a base "porsche_vendas_tratada.csv" com 100 vendas da Porsche nos EUA.
Colunas: id, data_venda (vazia quando a data original era inválida), modelo,
familia (911, 718, Cayenne, Macan, Panamera, Taycan), ano_modelo, preco (US$),
quilometragem_mi, pagamento, cidade, estado (UF), status, status_grupo
(Entregue, Em trânsito, Aguardando entrega, Pendente, Cancelada).

Crie uma dashboard em HTML, arquivo único, com os dados embutidos no próprio
arquivo, em português, que responda a TRÊS perguntas de negócio:

1. Qual linha de modelo sustenta o faturamento? → barras horizontais de receita
   por família, ordenadas, mostrando também quantidade e ticket médio.
2. Quanto da receita já virou entrega? → receita por status_grupo, com cores de
   status (verde = entregue, vermelho = cancelada) e uma barra empilhada 100%.
3. Como os clientes pagam e quanto depende de crédito? → receita por meio de
   pagamento, destacando Financing e Lease em outra cor.

Abaixo de cada gráfico, um texto curto que responda à pergunta com os números
do filtro atual (gerado automaticamente).

Filtros no topo, em uma linha: linha de modelo, estado, ano do modelo,
pagamento e status, mais um botão "Limpar filtros". Todos os gráficos, KPIs e a
tabela devem reagir aos filtros.

KPIs no topo: vendas, receita, ticket médio e % da receita já entregue.

No fim, uma tabela com as vendas filtradas, ordenável pelo cabeçalho.

Visual: inspirado na Porsche — fundo grafite escuro, vermelho Porsche (#d5001c)
como destaque, barras em dourado/champanhe, títulos em Barlow Condensed e texto
em Inter. Tooltip ao passar o mouse nas barras. Responsivo para celular.
Não use bibliotecas externas de gráfico: desenhe as barras com HTML/CSS.
```

## Refinamentos até a versão final

| # | O que pedi | Por quê |
|---|---|---|
| 1 | Trocar o formato `$4,272,700` por `US$ 4.272.700` e `US$ 12,83 mi` | A primeira versão misturava o padrão americano com rótulos em português |
| 2 | Mudar "ainda em aberto" para "ainda não entregues" no KPI | O valor incluía vendas canceladas, que não estão "em aberto" |
| 3 | Corrigir o estouro horizontal no celular | O console/inspeção mostrou a tabela empurrando a largura da página para 977px; o card precisava de `min-width: 0` |
| 4 | Alargar a coluna de nomes e de valores nas barras | Rótulos como "Aguardando entrega" e "Transferência (wire)" eram cortados e os valores quebravam em duas linhas |

## O que aprendi com os refinamentos

- A pergunta de negócio precisa estar **no prompt**; sem ela, a IA tende a jogar um gráfico para cada coluna.
- Pedir o **texto de resposta automático** abaixo de cada gráfico fez a dashboard "falar" — ela não só mostra, ela responde.
- Testar com filtros extremos (ex.: 911 + 2020 + Wire Transfer, que retorna zero vendas) revelou a necessidade de uma mensagem de estado vazio.
