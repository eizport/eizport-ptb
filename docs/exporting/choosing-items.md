---
title: "Escolher itens"
description: "Itens por tipo de conteúdo, escolhidos no painel lateral na hora; sugestões; o filtro Seleção; descendentes, anexos, avaliações e funções."
translatedFrom: "16a6899a5e88ec5903f9592d2a96aff00f7920a8"
---

Cada tipo de conteúdo é exportado inteiro (com a linha dele marcada na lista) ou **por itens**, escolhidos no painel lateral dele.

![O painel Páginas: uma caixa de seleção por página, funções como selos.](../../../assets/screenshots/items-panel.webp)

## Escolher

Marque um item, ou clique nele, e ele entra na exportação na hora: a lista atrás do painel mostra **1 item**, **3 itens**… Desmarque o último e o tipo volta a ficar de fora. A escolha sobrevive à pesquisa e à paginação: marque alguns na página 1, pesquise, marque mais. **Fechar** volta para a lista. O painel Opções funciona do mesmo jeito.

## Sugestões

O que você escolhe pode sugerir mais, mostrado como **N sugeridos** nas outras linhas — contados, sem nomear, e só exportados quando essa linha é marcada:

| Escolher | Sugere |
| --- | --- |
| Uma página para a qual uma configuração aponta (página inicial, página de posts, loja…) | Essas configurações, em **Opções** ([configurações vinculadas](../options/#linked-settings)) |
| Uma página cujos blocos mostram produtos (*Produtos selecionados manualmente*, *Coleção de produtos* com produtos selecionados manualmente, *Produto único*, *Produto em destaque*, `[products ids="…"]`) | Esses produtos, em **Produtos** |
| Produtos com avaliações | As avaliações aprovadas deles, em **Avaliações de produtos** |
| Qualquer coisa | Os menus clássicos nas posições do tema e as navegações que os itens ou os modelos do tema usam, em **Menus** ([menus](../menus/)) |
| Um menu | As páginas, os posts e os produtos para onde ele aponta, nas linhas deles |

Marcar uma linha com sugestões as traz, e ela continua dizendo *2 sugeridos* — com o que mais você escolheu: *2 sugeridos + Básicas + 1 item*. Desmarcar uma delas no painel da linha transforma o resto em itens comuns. **Exportar tudo** leva o tipo inteiro. Plugins acrescentam as próprias sugestões com [`eizport_suggested_items`](../../developers/hooks/#eizport_suggested_items).

## Encontrar itens

- **Seleção** — *Todos*, *Selecionados* ou *Sugeridos*. Um painel abre em *Selecionados* quando a linha dele tem itens escolhidos, senão em *Sugeridos* quando há sugestões.
- **Status** e cada **taxonomia** (categorias de produtos, tags…) restringem a lista para encontrar itens; sozinhos, não exportam nada.

Exportar por filtros (status, autor, datas, termos) continua na [tela clássica](../classic-screen/) e na habilidade [`eizport/export`](../../developers/abilities/).

## O que vai junto

| Com | Vai junto |
| --- | --- |
| Qualquer item | Sua mídia anexada e sua imagem destacada (o WordPress faz isso) |
| Qualquer item | A mídia usada no conteúdo sem estar anexada ([mídia](../media/)) |
| Qualquer item | Suas categorias, tags e outros termos completos — descrição, categorias ascendentes e term meta (a miniatura e o tipo de exibição de uma categoria de produto) — não só os nomes |
| Um produto | Suas variações. As avaliações só com a linha **Avaliações de produtos** (elas são sugeridas) — na tela clássica e na habilidade sem `reviews`, todas elas |
| Uma página | Suas páginas descendentes só se também forem escolhidas |
| Um item com uma função | As configurações que apontam para ele, sugeridas na linha Opções ([configurações vinculadas](../options/#linked-settings)) |

## Funções

Itens para os quais uma configuração aponta mostram um selo: **Página inicial**, **Página de posts**, **Página de política de privacidade**, **Ícone do site** e as páginas que os plugins cadastram, como a **Página da loja**, a **Página do carrinho**, a **Página de finalização de compra**, a **Página Minha conta** e a **Página de termos e condições** do WooCommerce. Os plugins adicionam as suas com [`eizport_linked_options`](../../developers/hooks/#eizport_linked_options).

## Limites

A tela clássica lista os 1.000 itens mais recentes por tipo (filtro [`eizport_selection_limit`](../../developers/hooks/#eizport_selection_limit)); a nova tela pagina por todos eles.
