---
title: "Links internos para o staging"
description: "Páginas que apontam umas para as outras, para produtos e para categorias, montadas localmente e levadas para o staging — com todo link apontando para o staging, não de volta para o site local."
translatedFrom: "c0d140d85ccd8055e30700149fdfbe97106df12d"
---

## O cenário

A página inicial do site local aponta para a loja, para três categorias de produto, para a página *Sobre* e para alguns produtos — em botões, em links de navegação, no texto. Só com o Importador do WordPress, o staging recebe esses links como estavam: `http://localhost:8909/product/…`, e cada clique no staging leva de volta a um site que só um computador consegue abrir. A saída era um `search-replace` depois de cada importação.

## Os passos

1. No site local, **Ferramentas → Exportar**: marque as páginas e os produtos (e os menus, os widgets…).
2. **Exportar**, e no staging **Ferramentas → Importar → WordPress** com o arquivo.

## O que a importação faz

Quando a importação termina, o endereço do site local vira o do staging no que foi importado:

- o conteúdo e o resumo de todo item importado, novo ou atualizado — links no texto, botões, links de navegação (também dentro dos atributos de bloco, onde o endereço vem escapado como `http:\/\/…`), com http ou https;
- os links personalizados dos menus clássicos importados;
- os widgets e os theme mods que vieram no arquivo.

O endereço só é trocado onde termina: `old.example` nunca mexe em `old.example.org`.

## O que não faz

- Conteúdo que não estava no arquivo: as páginas do próprio staging mantêm os links delas.
- Modelos e partes de modelo ainda não são exportados, então os links no cabeçalho ou no rodapé de um tema de blocos ficam como estavam.
