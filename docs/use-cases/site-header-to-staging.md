---
title: "Cabeçalho do site para o staging"
description: "O cabeçalho de um tema de blocos refeito localmente no editor do site — um layout novo, a navegação dele, uma imagem — e um modelo de página, levados para o staging no lugar dos que o staging tem, sem um Cabeçalho 2."
translatedFrom: "3029021021c6849c9bfe01e368f284b72848fe58"
---

## O cenário

O site usa um tema de blocos. Localmente, o cabeçalho foi refeito em **Aparência → Editor**: o logo à esquerda, uma navegação nova, uma imagem de destaque acima dela; e o modelo **Páginas** ganhou uma linha própria acima do conteúdo. O staging tem o próprio cabeçalho, editado ali há meses.

Copiar a marcação de blocos à mão perde a navegação (o `ref` dela é um ID do site local) e a imagem (outro ID), e o Importador do WordPress sozinho acrescenta o cabeçalho de novo como *Cabeçalho 2* ao lado do que o staging tem, que continua aparecendo.

## Os passos

1. No site local, **Ferramentas → Exportar**. Marque **Partes de modelo do editor** — ou abra **Escolher itens** e marque só **Cabeçalho**, cada parte com a marcação de blocos como prévia — e **Modelos do editor** (**Páginas**).
2. Marque também **Menus**, com a navegação do cabeçalho: ela vem sugerida, já que os modelos apontam para ela.
3. **Exportar**, e no staging **Ferramentas → Importar → WordPress** com o arquivo (com o Eizport ativo ali).

## O que o staging recebe

- O **Cabeçalho** dele substituído pelo local, com o mesmo slug — sem *Cabeçalho 2* — e o modelo **Páginas** substituído (ou acrescentado, quando o staging não o tinha editado).
- O bloco de navegação do cabeçalho apontando para a navegação importada, o bloco de imagem para a imagem importada, e os links para o site local apontando para o staging.
- Todo o resto da aparência do staging como estava: os outros modelos e partes dele, os estilos, o CSS.

## O que não vai

- Modelos que estão nos arquivos do tema e que ninguém editou: o tema do staging já os tem.
- Modelos de outro tema: só aparecem os do tema ativo, e eles vão para o mesmo tema no staging. Com outro tema ativo ali, ficam esperando até esse tema ser ativado.
