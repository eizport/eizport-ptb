---
title: "Habilidades"
description: "Cada habilidade eizport/* da Abilities API do WordPress — entrada, saída, permissão — por REST, PHP e MCP."
translatedFrom: "647a05806d14a20c3a9c721fb9c5b9c04ec99b18"
---

Tudo o que as telas fazem é uma **habilidade** da [Abilities API](https://developer.wordpress.org/apis/abilities/) (WordPress 6.9+), na categoria `eizport`. As telas Exportar e Configurações também as chamam, então código, agentes de IA e a interface seguem o mesmo caminho.

## Como chamá-las

**REST** (`/wp-json/wp-abilities/v1/abilities/<name>/run`, autenticado: cookie + nonce, ou uma senha de aplicativo):

```bash
curl -u admin:APP_PASSWORD -X POST \
  -H "Content-Type: application/json" \
  https://example.com/wp-json/wp-abilities/v1/abilities/eizport/export/run \
  -d '{"input":{"types":{"page":{"ids":[43]}},"options":{"groups":["basics"]}}}'
```

Habilidades somente leitura rodam com `GET` e a entrada na string de consulta.

**PHP**:

```php
$copy = wp_get_ability( 'eizport/export' )->execute(
	array(
		'types' => array( 'product' => array( 'filters' => array( 'status' => 'publish' ) ) ),
		'zip'   => true,
	)
);
if ( ! is_wp_error( $copy ) ) {
	echo $copy['download_url'];
}
```

**MCP**: com o [MCP adapter](https://github.com/WordPress/mcp-adapter), cada habilidade é uma ferramenta que um agente de IA pode chamar.

> Roda uma exportação por solicitação: `export_wp()` define suas funções auxiliares dentro de si mesma. Uma segunda `eizport/export` na mesma solicitação retorna `eizport_export_unavailable`.

## Visão geral

| Habilidade | Faz | Permissão | Anotações |
| --- | --- | --- | --- |
| `eizport/get-export-options` | O que pode ser exportado | `export` | readonly |
| `eizport/get-items` | Uma página dos itens de um tipo | `export` | readonly |
| `eizport/get-options` | As opções do site, uma a uma | `export` | readonly |
| `eizport/get-suggestions` | O que os itens escolhidos sugerem | `export` | readonly |
| `eizport/get-reviews` | Uma página de avaliações de produtos | `export` | readonly |
| `eizport/export` | Exporta para uma cópia | `export` | — |
| `eizport/get-copies` | As cópias mantidas | `export` | readonly |
| `eizport/delete-copies` | Exclui cópias | `export` | destructive, idempotent |
| `eizport/import` | Importa uma cópia ou um arquivo de mídia | `import` | — |
| `eizport/get-export-defaults` | Os padrões salvos | `export` | readonly |
| `eizport/save-export-defaults` | Salva os padrões | `export` (os do site: `manage_options`) | idempotent |
| `eizport/clear-export-defaults` | Esquece os padrões | como acima | destructive |
| `eizport/get-settings` | As configurações do plugin | `manage_options` | readonly |
| `eizport/update-settings` | Altera as configurações | `manage_options` | idempotent |

## eizport/get-export-options

Sem entrada. Retorna `types` (cada um com `type`, `label`, `count`, `hierarchical`, `statuses` e `taxonomies` com seus termos), `option_groups` (`group`, `label`, `options`), `front_pages` (IDs de `front` e `posts`), `themes`, `plugins`, `copy_lifetime` (segundos), `filename` (o nome padrão de hoje), `linked` (o modo das configurações vinculadas), `bundle_zip`, `review_count` (avaliações de produtos aprovadas; 0 sem o WooCommerce) `appearance` (`theme_mods`, `custom_css`, `global_styles`, `global_css`, `widgets` → os itens deles: `key`, `label`, `details`, `preview`; veja [Aparência](../../exporting/appearance/)) e `menus`: `classic` (`id`, `name`, `slug`, `items`, `locations` — os nomes das posições do tema em que ele está) e `navigation` (`id`, `title`, `links`) — veja [Menus](../../exporting/menus/).

## eizport/get-items

| Entrada | Tipo | |
| --- | --- | --- |
| `type` | string, obrigatório | Um tipo de post de `get-export-options` |
| `search` | string | |
| `page`, `per_page` | integer | `per_page` até 100 |
| `orderby` | `date` \| `title` \| `menu_order` | |
| `order` | `asc` \| `desc` | |
| `filters` | object | `author`, `start_date`/`end_date` (`YYYY-MM`), `status`, `terms` (IDs de termos) |
| `include` | integer[] | Só estes itens (nenhum: uma página vazia) |

Retorna `items` — `id`, `title`, `status`, `date`, `parent`, `author`, `file` (anexos), `roles` (`option`, `label`, `with`) — além de `total` e `total_pages`.

## eizport/get-options

Entrada: `search`, `group` (uma chave de `option_groups`), `names` (só estas), `page`, `per_page`. Retorna `items` (`name`, `size`, `value` — numa linha, arrays como JSON, até 200 caracteres — `autoload`, `post_id`), `total`, `total_pages`. As opções que nunca são exportadas e os transients ficam de fora.

## eizport/get-suggestions

Entrada: `types` (como o `eizport/export` os recebe), `whole` (tipos de post escolhidos inteiros, o mesmo que `{}` em `types`) e `menus.classic` (os menus escolhidos). Retorna `items` (tipo de post → IDs que o conteúdo dos itens usa, por [`eizport_suggested_items`](../hooks/#eizport_suggested_items), e as páginas e os posts para onde os menus escolhidos apontam), `reviews` (avaliações aprovadas dos produtos escolhidos), `options` (configurações que apontam para os itens, a menos que *Configurações vinculadas* esteja em *Nunca* ou *Na exportação*), `appearance` (linha → as chaves dos itens dela, todos sugeridos) e `menus` (`classic`: os menus nas posições do tema; `navigation`: as navegações que o conteúdo dos itens ou os modelos do tema usam). Nada é adicionado a uma exportação sozinho.

## eizport/get-reviews

Entrada: `search`, `include` (só estas), `page`, `per_page`. Retorna `items` (`id`, `author`, `product`, `product_title`, `rating`, `excerpt`, `date`), `total`, `total_pages`; vazio sem o WooCommerce.

## eizport/export

| Entrada | Tipo | Padrão |
| --- | --- | --- |
| `types` | object: tipo de post → `{ "ids": [...] }` ou `{ "filters": {...} }` | todo o conteúdo |
| `media` | boolean: arquivos de mídia dentro | `true` |
| `media_used` | boolean: mídia usada no conteúdo | `true`, a menos que *Só mídia anexada* |
| `post_content` | boolean: `post_content/*.html` | *Conteúdo como arquivos .html* |
| `bundle` | boolean: `false` para o `.xml` simples do WordPress | `true` |
| `zip` | boolean: `.zip` em vez de `.wxrz` | *Pacote como .zip* |
| `options` | object: `groups`, `names`, `all`, `front_pages`, `linked` | nenhuma |
| `blueprint` | object: `themes`, `plugins` — gera um `.zip` do Playground | — |
| `as_plugin` | boolean: um `.zip` de plugin que importa o próprio pacote ([como plugin](../../exporting/as-plugin/)); não com `blueprint` | `false` |
| `update_existing` | boolean: pede à importação que atualize os itens que já existem (mesmo tipo e slug); fica gravado no pacote | `false` |
| `reviews` | object: `ids` (IDs de comentário; `[]` para nenhuma) ou `all` — avaliações de produtos do WooCommerce | todas as avaliações dos produtos |
| `appearance` | object: linha (`theme_mods`, `custom_css`, `global_styles`, `global_css`, `widgets`) → `"all"` ou as chaves dos itens dela (do `eizport/get-export-options`); uma lista de linhas significa cada uma inteira — a [aparência](../../exporting/appearance/) do site, com as mídias que ela usa; precisa de `bundle` | nenhuma |
| `css_mode` | `end`, `start` ou `replace`: como as regras CSS entram na importação | `end` |
| `css_markers` | comentários head/tail em volta das regras CSS | Configurações → Exportação |
| `menus` | object: `classic` (IDs de termos de menu, com os itens e as posições do tema), `navigation` (IDs de `wp_navigation`) ou `all` — [menus](../../exporting/menus/) | nenhum (*todo o conteúdo* já os tem) |
| `filename` | string, sem extensão | `<site>.WordPress.<date>` |

Retorna a cópia:

```json
{
  "name": "site-w9bjZ9pyEWxC8VMf.zip",
  "format": "playground",
  "size": 8178533,
  "created": "2026-10-10T22:11:58+00:00",
  "expires": "2026-10-11T22:11:58+00:00",
  "download_url": "https://example.com/?eizport_playground=site-w9bjZ9pyEWxC8VMf.zip",
  "playground_url": "https://playground.wordpress.net/?blueprint-url=…",
  "command": "npx @wp-playground/cli@latest server --blueprint=./site-w9bjZ9pyEWxC8VMf.zip"
}
```

`format` é `wxrz`, `zip`, `wxr`, `playground` ou `plugin`; `expires` é `""` para cópias mantidas até serem excluídas. Erros: `eizport_invalid_type`, `eizport_invalid_options` (uma opção que nunca é exportada), `eizport_invalid_blueprint` (um tema ou plugin não instalado), `eizport_export_unavailable`.

## eizport/get-copies e eizport/delete-copies

`get-copies` retorna a lista de cópias, como acima. `delete-copies` recebe `names` (como `get-copies` os retorna) ou `all: true`, e retorna `{ "deleted": 3 }`.

## eizport/import

Entrada: `copy` (o nome de uma cópia) ou `attachment_id` (um `.wxrz`, `.zip`, `.xml` ou `.xml.gz` na biblioteca de mídia). `update_existing` (boolean) atualiza os itens que já existem (mesmo tipo e slug) em vez de adicionar cópias; padrão: o que o pacote pede. `css_mode` (`end`, `start`, `replace`) e `css_markers` (boolean) mudam como as regras CSS do pacote entram ([aparência](../../exporting/appearance/)); padrão: o que o pacote pede. Precisa do Importador do WordPress instalado. Um plugin feito [como plugin](../../exporting/as-plugin/) é recusado (`eizport_plugin_zip`): instale-o como plugin. Retorna `posts` (quantos o importador criou ou encontrou), `options` (os nomes definidos) e `log` (as mensagens do importador, como texto).

## Padrões

- `eizport/get-export-defaults` — sem entrada; retorna `{ "types": …, "settings": … }` ou `{}`.
- `eizport/save-export-defaults` — entrada `types` e `settings` (o estado da tela Exportar: `format`, `filename`, `media`, `option_names`, `option_groups`, `option_all`, `include_suggested`, `review_ids`, `review_all`, `as_plugin`, `update_existing`, `themes`, `plugins`); retorna o que foi salvo, sanitizado.
- `eizport/clear-export-defaults` — sem entrada.

## Configurações

`eizport/get-settings` retorna `copy_lifetime`, `replace_export`, `classic_ui`, `style_import`, `content_files`, `attached_only`, `bundle_zip`, `defaults_scope`, `linked_options` e as opções de escolha de cada um (`lifetime_choices`, `scope_choices`, `linked_choices`). `eizport/update-settings` recebe qualquer um deles (com valores dessas escolhas) e retorna todos; chaves desconhecidas são recusadas.
