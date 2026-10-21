---
title: "Menus do cabeçalho para o staging"
description: "Um site montado localmente — páginas, um menu de cabeçalho com submenus e a posição dele no tema — repetido no staging sem refazer o menu à mão."
translatedFrom: "90ca59034a86059bfc2f8ef2fadb76b112fd7d91"
---

## O cenário

Um site é montado localmente a partir de um design: as páginas, depois o menu do cabeçalho — *Início*, *Sobre*, *Serviços* com três páginas abaixo, *Contato* e um link para a loja — na posição *Principal* do tema. O staging precisa das mesmas páginas e do mesmo menu, e ninguém quer refazer o menu à mão, item por item, depois de cada rodada de mudanças.

## Os passos

1. No site local, **Ferramentas → Exportar**. Marque **Páginas** — ou só as que o menu aponta.
2. A linha **Menus** diz *1 sugerido*: o menu na posição *Principal*. Marque-a. Abrindo o painel, ele aparece como *Menu clássico* em *Principal*.
3. Se uma página do menu não foi escolhida, a linha **Páginas** a sugere: marque a linha para trazê-la.
4. **Exportar**, e no staging **Ferramentas → Importar → WordPress** com o arquivo (com o Eizport ativo lá também, para a posição do menu).

O mesmo vale para o cabeçalho de um tema de blocos: a linha **Menus** sugere a *Navegação* que o cabeçalho usa.

## O que o arquivo leva

- As páginas, com as mídias delas.
- O menu, com os itens em ordem, o submenu de *Serviços*, o link personalizado para a loja e o lugar dele em *Principal*.

No staging, os itens do menu apontam para as páginas do staging (os IDs novos) e o menu fica em *Principal*.

## O que não leva

- Uma página que o menu aponta e que não foi exportada: o item dela fica de fora.
- A posição, quando o tema do staging não tem *Principal* (outro tema, ou um tema de blocos).

## Da próxima vez

Depois de mais mudanças, exporte de novo e importe com *Atualizar páginas, posts e produtos que já existem (mesmo slug)* marcada (veja [atualizar os itens existentes](../../getting-started/importing/#atualizar-os-itens-existentes)): as páginas são atualizadas, e o menu recebe os itens do arquivo em vez de ganhar uma segunda cópia deles.
