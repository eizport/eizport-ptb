---
title: "Categorias de produto para o staging"
description: "Os produtos de uma loja levados com as categorias como estão — descrições, categorias ascendentes e miniaturas — e não só com os nomes delas."
translatedFrom: "16a6899a5e88ec5903f9592d2a96aff00f7920a8"
---

## O cenário

A loja local tem os produtos em categorias com descrições (mostradas nas páginas das categorias), algumas abaixo de uma ascendente (*Café → Origem única*), e com miniaturas. Só os produtos são exportados, não *Todo o conteúdo*. Com a exportação do próprio WordPress, cada produto nomeia as categorias dele e nada mais: o staging recebe categorias com os nomes certos, sem descrição, sem ascendente, sem miniatura.

## Os passos

1. No site local, **Ferramentas → Exportar**: marque **Produtos** (todos ou alguns).
2. **Exportar**, e no staging **Ferramentas → Importar → WordPress** com o arquivo.

## O que o arquivo leva

Toda categoria, tag e outro termo que os itens exportados usam, completos, com as categorias ascendentes mesmo quando nenhum produto está nelas:

- o nome, o slug e a **descrição**;
- a **ascendente**, para a hierarquia ser refeita (as ascendentes vêm primeiro);
- o **term meta**: a miniatura e o tipo de exibição do WooCommerce, ou os campos próprios de um plugin.

No staging, as categorias chegam como estavam, e as miniaturas apontam para as imagens importadas.

## O que não leva

- Uma categoria que o staging já tem (mesmo slug) é usada como está: a descrição dela não é substituída.
- Termos que nenhum item exportado usa ficam de fora — exporte *Todo o conteúdo* para levar todos os termos.
