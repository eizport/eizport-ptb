---
title: "Hooks"
description: "Cada filtro e ação do Eizport, com seus parâmetros e um exemplo."
translatedFrom: "9fd577e7caba305c2b5ac04e7413466a6437c413"
---

Todos os hooks têm o prefixo `eizport_`. Adicione-os a partir de um plugin ou do `functions.php` de um tema.

## eizport_linked_options

Opções que apontam para um post, com a função que dão a ele. Elas recebem o ID do post importado na importação, aparecem como um selo no item e podem ir junto com ele ([configurações vinculadas](../../exporting/options/#linked-settings)).

```php
add_filter( 'eizport_linked_options', function ( array $linked ) {
	$linked['my_plugin_landing_page'] = array(
		'label' => __( 'Landing page', 'my-plugin' ),     // The role, shown as a badge.
		'with'  => array( 'my_plugin_landing_enabled' ),  // Options that go with it.
		'when'  => array( 'my_plugin_mode' => 'landing' ), // Only while these hold these values.
	);
	return $linked;
} );
```

| Parâmetro | |
| --- | --- |
| `array $linked` | Nome da opção → `label` (string), `with` (string[], opcional), `when` (opção → valor, opcional) |

## eizport_option_groups

Os grupos de opções oferecidos nas telas e em `options.groups`.

```php
add_filter( 'eizport_option_groups', function ( array $groups ) {
	$groups['my-plugin'] = array(
		'label'   => __( 'My Plugin: layout and colors', 'my-plugin' ), // "Name: what it holds".
		'options' => array( 'my_plugin_layout', 'my_plugin_colors' ),
	);
	return $groups;
} );
```

## eizport_denied_options

Mais opções que nunca são exportadas — chaves, tokens, licenças, números de versão. Retorna nomes a **adicionar**; as opções negadas do próprio WordPress não podem ser removidas.

```php
add_filter( 'eizport_denied_options', function ( array $names ) {
	return array_merge( $names, array( 'my_plugin_api_key', 'my_plugin_db_version' ) );
} );
```

## eizport_is_secret

Se um nome — uma opção, uma modificação do tema, um campo de widget — guarda um segredo, e por isso nunca é exportado nem listado. Por padrão: nomes com `secret`, `password`, `token`, `api_key`, `license`, `private`, `credential`, `access_key`, `auth_key` ou `webhook`, e os lugares conhecidos onde plugins os guardam (WP Mail SMTP, gateways de pagamento). Marque os seus, ou deixe passar um:

```php
add_filter( 'eizport_is_secret', function ( bool $secret, string $name ) {
	return 'my_plugin_connection' === $name ? true : $secret;
}, 10, 2 );
```

## eizport_media_used

Anexos que um post usa além dos que o conteúdo cita, como uma galeria guardada em meta. Roda quando alguns itens são exportados e *Só mídia anexada* está desativada.

```php
add_filter( 'eizport_media_used', function ( array $used, WP_Post $post ) {
	if ( 'portfolio' === $post->post_type ) {
		$used = array_merge( $used, wp_parse_id_list( get_post_meta( $post->ID, 'gallery', true ) ) );
	}
	return $used;
}, 10, 2 );
```

## eizport_term_media

Os term meta que guardam um ID de anexo, por taxonomia, como a imagem de uma categoria. Os anexos vão com a exportação (para os termos que os itens usam), o manifesto os lista (`term_media`) e a importação os aponta para os anexos importados. A compatibilidade com o WooCommerce adiciona `product_cat` → `thumbnail_id`.

```php
add_filter( 'eizport_term_media', function ( array $keys ) {
	$keys['portfolio_type'][] = 'cover_id';
	return $keys;
} );
```

Use junto com [`eizport_media_used`](#eizport_media_used), que adiciona os anexos à exportação.

## eizport_updated_from_import

Dispara depois que um post existente foi atualizado a partir de um item importado, com *Atualizar existentes* ligado. Use para atualizar o que um plugin guarda em outro lugar.

```php
add_action( 'eizport_updated_from_import', function ( int $id, array $post ) {
	clean_post_cache( $id );
}, 10, 2 );
```

## eizport_block_id_attributes

Os blocos cujos atributos guardam IDs de posts, remapeados para os itens importados quando a importação termina (Imagem, Galeria, Capa, Mídia e texto, Arquivo, Vídeo, Áudio e os links de navegação do core já estão lá). Acrescente o bloco de um plugin pelo nome completo.

```php
add_filter( 'eizport_block_id_attributes', function ( array $blocks ) {
	$blocks['my-plugin/card'] = array( 'imageId', 'postIds' ); // Um número ou uma lista de números.
	return $blocks;
} );
```

## eizport_menus_update_existing

Se uma importação substitui os itens dos menus clássicos que o site já tem (`true`) ou os deixa como estão (`false`, o padrão). *Atualizar existentes* liga isso na importação dele.

```php
add_filter( 'eizport_menus_update_existing', '__return_true' );
```

## eizport_suggested_items

Posts que um item sugere exportar também, como os produtos que um bloco no conteúdo dele mostra. A tela Exportar os mostra como *N sugeridos* na linha do tipo deles ([escolher itens](../../exporting/choosing-items/#sugestões)); nada é adicionado sozinho. A compatibilidade com o WooCommerce o usa para produtos.

```php
add_filter( 'eizport_suggested_items', function ( array $suggested, WP_Post $post ) {
	// Um bloco "relacionados" guarda os IDs dos posts que mostra.
	if ( preg_match_all( '/"relatedIds":\[([\d,]+)\]/', $post->post_content, $m ) ) {
		foreach ( $m[1] as $list ) {
			$suggested = array_merge( $suggested, wp_parse_id_list( $list ) );
		}
	}
	return $suggested;
}, 10, 2 );
```

Só são sugeridos posts de tipos que podem ser exportados.

## eizport_content_types

Os tipos de post que a tela de exportação oferece como linhas: tipo de post => valor do rádio *conteúdo* do core. Um plugin tira os seus tipos que viajam de outro jeito (a compatibilidade com o Advanced Custom Fields tira os grupos de campos, tipos de post e taxonomias do ACF).

## eizport_look_items

Os itens de uma linha ao lado das da aparência (`$items`, `$row`): um plugin preenche a sua própria linha, cada item com `key`, `label`, `details` e `preview` (`type` e `value`). Usado pela compatibilidade com o Advanced Custom Fields na linha `advanced-custom-fields`.

## eizport_look_data

Os dados do arquivo de opções para uma linha assim (`$out`, `$row`, `$keys`: as chaves dos itens escolhidos): a entrada da linha vai em `$out`.

## eizport_look_suggested

As chaves que uma linha assim sugere para os posts escolhidos (`$keys`, `$row`, `$chosen`: tipo de post => IDs).

## eizport_look_media

Os anexos que a aparência escolhida usa (`$ids`, `$choice`): a linha de um plugin acrescenta os que os itens dela usam (a linha Elementor: imagens de modelos e das configurações do site).

## eizport_content_file

O arquivo para onde vai o conteúdo de um post com *Conteúdo como arquivos .html* ativado (`$file`, `$post`): `null` para `post_content/<slug>.html`, ou `array( 'extension' => 'json', 'data' => … )` para o design de um construtor de páginas no formato dele — o conteúdo do post então fica no WXR, e o importador guarda o `.json` para quem o aplica quando a importação termina (`$GLOBALS['eizport_content_data']`, pelo ID original do item). A compatibilidade com o Elementor o usa.

## eizport_advanced_custom_fields_id_types

Os tipos de campo do ACF cujos valores são IDs que uma importação mapeia: tipo => `post`, `term` ou `user`. Adicione um tipo de campo de um plugin:

```php
add_filter( 'eizport_advanced_custom_fields_id_types', function ( array $types ) {
	$types['my_product_picker'] = 'post';
	return $types;
} );
```

## eizport_register_compat

Ação, em `plugins_loaded` (prioridade 20), onde as compatibilidades conectam os filtros acima. Veja [compatibilidades](../compatibilities/).

## eizport_post_id_options

Opções que guardam um ID de post, mapeadas na importação. Por padrão, as chaves de `eizport_linked_options`; mantido por compatibilidade.

| Parâmetro | |
| --- | --- |
| `string[] $names` | Nomes das opções |

## eizport_post_content

O conteúdo que um item importado recebe do seu arquivo em `post_content/`.

| Parâmetro | |
| --- | --- |
| `string\|null $content` | O conteúdo do arquivo, ou `null` quando não há |
| `array $item` | `id` (ID original), `type`, `slug` |
| `string $dir` | Pasta em que o pacote foi desempacotado |

## eizport_wxrz_max_bytes

O máximo que um pacote pode ocupar ao ser expandido na importação. Padrão `512 * MB_IN_BYTES`.

## eizport_playground_blueprint

O blueprint de um `.zip` do Playground, antes de ser gravado.

| Parâmetro | |
| --- | --- |
| `array $blueprint` | O blueprint (`steps`, `landingPage`…) |
| `string $wxr_name` | O nome do WXR dentro do `.zip` |

## eizport_playground_mu_plugins

Os plugins obrigatórios que um `.zip` do Playground pode levar — sem os da plataforma (WordPress Studio, hospedagens) e sem os do próprio Eizport. Tire o carregador de uma hospedagem, ou devolva um que ficou de fora.

| Parâmetro | |
| --- | --- |
| `array $choices` | Arquivo => dados do plugin, como `get_mu_plugins()` os dá |

## eizport_selection_limit

Quantos itens cada lista *Por seleção* da tela clássica mostra (os mais recentes). Padrão `1000`.

## eizport_copy_lifetime

Quanto tempo uma cópia dura, em segundos; `0` mantém as cópias até serem excluídas. Padrão: o escolhido em Configurações.
