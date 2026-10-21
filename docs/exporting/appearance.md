---
title: "Aparência"
description: "A aparência do site em linhas próprias: theme mods e CSS adicional do Personalizador, estilos e CSS globais do editor do site, e widgets — com as mídias que eles usam."
translatedFrom: "ffdc42c9741729dcf458bebf036c09f8fb7096b8"
---

A aparência do site é exportada em linhas próprias, ao lado dos tipos de conteúdo. Cada linha é uma coisa só, dentro ou fora, e só aparece quando o tema ativo a tem:

| Linha | O que é | Aparece com |
| --- | --- | --- |
| **Theme mods do Personalizador** | *Aparência → Personalizar*: logo, imagem do cabeçalho, imagem e cor de fundo, as configurações do próprio tema | Temas clássicos (ou qualquer tema com configurações no Personalizador) |
| **CSS adicional do Personalizador** | *Personalizar → CSS adicional* | Um tema com CSS adicional |
| **Estilos globais do editor** | *Aparência → Editor → Estilos*: cores, tipografia, layout | Temas de blocos com estilos salvos |
| **CSS global do editor** | *Estilos → CSS adicional* | Temas de blocos com CSS adicional ali |
| **Widgets** | *Aparência → Widgets*: widgets clássicos (texto, imagem, menu…) e widgets de bloco, com as barras laterais | Temas clássicos com widgets nas barras laterais |

Toda linha que o site tem vem sugerida. Elas precisam de um pacote (`.wxrz`, `.zip` ou um `.zip` do Playground): vão no `options.json` dele.

## O que vai junto

As imagens que a aparência usa são exportadas com ela, mesmo sem a linha Anexos marcada: o logo, as imagens de cabeçalho e de fundo, as mídias dos widgets de imagem e de bloco, e os uploads para onde os estilos e o CSS apontam (*Só mídia anexada*, nas [configurações de exportação](../../settings/export-settings/), desliga isso, como no conteúdo).

## No outro site

- Os **theme mods** vão para o tema ativo e são **mesclados** aos dele: o que o arquivo não traz fica (como as posições dos menus). O logo e a imagem do cabeçalho apontam para os anexos importados, e os endereços das imagens de cabeçalho e de fundo para os arquivos importados.
- O **CSS adicional** vira o CSS adicional do tema ativo.
- Os **estilos globais** e o **CSS global** vão para os estilos do tema ativo, quando ele é um tema de blocos. Cada um muda só a sua parte: importar os estilos mantém o CSS adicional do outro site, e vice-versa.
- Os **widgets** voltam para as barras laterais; uma barra que o outro tema não tem manda os widgets para *Widgets inativos*, como o WordPress faz. Um widget de menu de navegação aponta para o menu importado (exporte-o na linha [Menus](../menus/)), um widget de imagem para a imagem importada, e as imagens e os endereços nos widgets de bloco para os importados.

## Pelo WP-CLI e pela habilidade

```bash
wp eizport export --types=page --appearance=theme_mods,custom_css,widgets
wp eizport export --appearance=all --menus=all
```

A habilidade [`eizport/export`](../../developers/abilities/#eizportexport) recebe `appearance`; a [`eizport/get-export-options`](../../developers/abilities/#eizportget-export-options) diz o que o site tem.

## Limites

- A aparência vai para o **tema ativo** do outro site. Exportados de um tema e importados com outro, os theme mods só fazem sentido se esse tema usa as mesmas configurações.
- As fontes instaladas pela Biblioteca de fontes não são exportadas: os estilos que as usam caem nas fontes do outro site.
- Os theme mods não vêm mais com o grupo *Básicas* da linha Opções: marque **Theme mods do Personalizador**. O logo do site de um tema de blocos é uma opção, em *Básicas*.
