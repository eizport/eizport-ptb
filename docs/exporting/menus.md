---
title: "Menus"
description: "Menus clássicos com os itens e as posições do tema, e navegações de bloco, numa linha própria; o que é sugerido, o que a importação faz com os menus que o outro site já tem, e os limites."
translatedFrom: "c0d140d85ccd8055e30700149fdfbe97106df12d"
---

Os menus de navegação são exportados numa linha própria, **Menus**, ao lado dos tipos de conteúdo. O painel lateral dela lista os dois tipos:

| Tipo | O que é | Vai junto |
| --- | --- | --- |
| **Menu clássico** | *Aparência → Menus*, nos temas clássicos | Os itens (com a ordem e os submenus) e as posições do tema em que ele está |
| **Navegação** | O menu de um bloco *Navegação*, nos temas de blocos (*Aparência → Editor → Navegação*) | Os links |

## Sugeridos

Sem escolher nada, a linha sugere:

- os menus clássicos que estão numa **posição do tema** (*Principal*, *Rodapé*…);
- as **navegações** que os itens escolhidos usam no conteúdo, e as que os modelos e partes de modelo do tema ativo usam (o bloco *Navegação* de um cabeçalho). Um bloco *Navegação* que não aponta para nenhum menu mostra o publicado mais recentemente: é esse que é sugerido.

Escolher um menu clássico sugere, nas linhas deles, os itens para onde ele aponta — páginas, posts, produtos. Exporte-os também: o importador deixa de fora um item de menu cuja página não está no outro site. Links personalizados vão sempre.

## No outro site

- **Menus clássicos** são criados com os itens, que apontam para as páginas, os posts e os termos importados; os submenus mantêm os pais. Cada menu volta para as **posições do tema**, quando o tema ativo do outro site as tem (um tema de blocos não tem nenhuma). As posições precisam do Eizport no outro site: só com o Importador do WordPress, os menus chegam sem elas.
- **Navegações** são criadas, e os blocos *Navegação* que apontavam para elas — nas páginas importadas e nos modelos do outro site — passam a apontar para as novas.
- **Um menu que o outro site já tem** (mesmo slug) mantém os itens dele, a não ser que a importação [atualize os itens que já existem](../../getting-started/importing/#atualizar-os-itens-existentes): aí os itens são substituídos pelos do arquivo, e ele fica igual ao deste site. Uma navegação com o mesmo slug é atualizada do mesmo jeito.

## Pelo WP-CLI e pela habilidade

```bash
wp eizport export --types=page --menus=classic:18,navigation:4
wp eizport export --menus=all
```

A habilidade [`eizport/export`](../../developers/abilities/#eizportexport) recebe `menus` (`classic`, `navigation` ou `all`); a [`eizport/get-export-options`](../../developers/abilities/#eizportget-export-options) lista os menus.

## Limites

- **Todo o conteúdo** (nenhum tipo escolhido) já leva todos os menus, como a exportação do próprio WordPress; escolher menus aí acrescenta as posições do tema.
- Sem nenhum tipo marcado e com menus escolhidos, só os menus são exportados.
- A tela clássica (*Ferramentas → Exportar → Clássica*) não tem a linha Menus.
