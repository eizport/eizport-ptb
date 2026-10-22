---
title: "Aparência"
description: "A aparência do site em linhas próprias, item por item: theme mods e regras CSS do Personalizador, grupos de estilo e regras CSS do editor do site, e widgets — com uma prévia de cada um, as mídias que eles usam e como o outro site os recebe."
translatedFrom: "647a05806d14a20c3a9c721fb9c5b9c04ec99b18"
---

A aparência do site é exportada em linhas próprias, ao lado dos tipos de conteúdo. Uma linha só aparece quando o tema ativo tem algo nela, e o painel **Escolher itens** dela lista o que ela tem, cada item com uma prévia:

| Linha | Cada item | Prévia | Aparece com |
| --- | --- | --- | --- |
| **Theme mods do Personalizador** | Uma configuração de *Aparência → Personalizar*: Logo, Imagem do cabeçalho, Imagem e cor de fundo, Cor do texto do cabeçalho, as configurações do próprio tema | A imagem, a cor ou o valor | Um tema com configurações no Personalizador |
| **CSS adicional do Personalizador** | Uma regra de *Personalizar → CSS adicional* (um bloco `@media` conta como uma; um comentário logo antes vai junto) | A regra, e qual é: *Regra 3 de 3 (a última)* | Um tema com CSS adicional |
| **Estilos globais do editor** | Um grupo de *Aparência → Editor → Estilos*: Paleta de cores, Cores do site, Tipografia, Espaçamento e layout, Elementos, cada bloco com estilos próprios | As cores dele, ou um resumo | Temas de blocos com estilos salvos |
| **CSS global do editor** | Uma regra do *CSS adicional* do editor do site | A regra | Temas de blocos com CSS adicional ali |
| **Widgets** | Um widget — clássico (texto, imagem, menu…) ou de bloco — com a barra lateral dele | O texto ou a imagem | Temas clássicos com widgets nas barras laterais |

Marque uma linha para levar tudo o que ela tem (todo item vem sugerido), ou abra **Escolher itens** e marque só alguns: a última regra CSS, o logo e a cor de fundo, um widget. Elas precisam de um pacote (`.wxrz`, `.zip` ou um `.zip` do Playground): vão no `options.json` dele.

## O que vai junto

As imagens que os itens escolhidos usam são exportadas com eles, mesmo sem a linha Anexos marcada: o logo, as imagens de cabeçalho e de fundo, as mídias dos widgets de imagem e de bloco, e os uploads para onde os estilos e o CSS apontam (*Só mídia anexada*, nas [configurações de exportação](../../settings/export-settings/), desliga isso, como no conteúdo).

## No outro site

- As **regras CSS** (as do Personalizador e as do editor do site) são **acrescentadas** ao CSS do outro site, como diz **Detalhes → Regras CSS na importação**: no fim (o padrão), no início, ou substituindo-o. Elas vão entre dois comentários com o nome do arquivo:

  ```css
  .deles { color: purple; }

  /* head entrega */
  .site-footer { background: #ffeedd; }
  /* tail entrega */
  ```

  O nome é o nome do arquivo (sem extensão): uma marca de onde as regras vieram. Cada importação acrescenta o próprio bloco — os blocos não são únicos, e o mesmo nome pode aparecer mais de uma vez. Sem os comentários ([configurações de exportação](../../settings/export-settings/), ou o importador), regras que o outro site já tem (mesmo texto) não são repetidas.
- Quem importa pode mudar as duas coisas na etapa 1 do importador (**CSS adicional**), com `css_mode`/`css_markers` no [`eizport/import`](../../developers/abilities/#eizportimport), ou com `--css-mode`/`--[no-]css-markers` no [WP-CLI](../../developers/wp-cli/).
- Os **theme mods** escolhidos vão para o tema ativo e são **mesclados** aos dele: o que o arquivo não traz fica (as outras configurações, as posições dos menus). O logo e a imagem do cabeçalho apontam para os anexos importados, e os endereços das imagens de cabeçalho e de fundo para os arquivos importados.
- Os **grupos de estilo** escolhidos substituem os mesmos grupos nos estilos do outro site (um tema de blocos); os outros grupos dele ficam.
- Os **widgets** escolhidos vão para o **fim da mesma barra lateral**, ao lado dos widgets do próprio outro site (para *Widgets inativos* quando o tema dele não tem essa barra), cada um com um número livre. Importar o mesmo arquivo de novo com *Atualizar existentes* os atualiza no lugar em vez de acrescentá-los duas vezes. Um widget de menu de navegação aponta para o menu importado (exporte-o na linha [Menus](../menus/)), um widget de imagem para a imagem importada, e as imagens e os endereços nos widgets de bloco para os importados.

## Pelo WP-CLI e pela habilidade

```bash
wp eizport export --types=page --appearance=theme_mods:custom_logo+background_color,custom_css,widgets:text-3
wp eizport export --appearance=all --css-mode=start --no-css-markers
wp eizport import entrega.wxrz --css-mode=replace
```

Uma linha sozinha leva tudo dela; a chave de uma regra CSS (`r:…`) vem do [`eizport/get-export-options`](../../developers/abilities/#eizportget-export-options), que lista os itens de cada linha. A habilidade [`eizport/export`](../../developers/abilities/#eizportexport) recebe `appearance` (linha → `"all"` ou chaves), `css_mode` e `css_markers`.

## Limites

- A aparência vai para o **tema ativo** do outro site. Exportados de um tema e importados com outro, os theme mods só fazem sentido se esse tema usa as mesmas configurações.
- As fontes instaladas pela Biblioteca de fontes não são exportadas: os estilos que as usam caem nas fontes do outro site.
- Os theme mods não vêm mais com o grupo *Básicas* da linha Opções: marque **Theme mods do Personalizador**. O logo do site de um tema de blocos é uma opção, em *Básicas*.
