---
title: "Compatibilidades"
description: "Quais plugins são suportados (WooCommerce, Advanced Custom Fields) e quais ainda não oficialmente (Elementor), como o suporte é feito com os hooks públicos e como um plugin traz o seu próprio."
translatedFrom: "05f6e7529e5513decc72ab35ef6f11f82270434d"
---

O suporte a outro plugin é uma **compatibilidade**: um arquivo que usa só os [hooks](../hooks/) públicos. O plugin traz duas, para o WooCommerce e o Advanced Custom Fields; qualquer plugin pode adicionar a sua — ou substituir a que vem incluída.

## Resumo

| Plugin | Situação | O que esperar |
| --- | --- | --- |
| **WooCommerce** | Suportado | Produtos, variações, pedidos e cupons como tipos de conteúdo; as páginas da loja, do carrinho, de finalização de compra, Minha conta e de termos como configurações vinculadas; galerias de produtos e imagens das categorias; produtos mostrados por blocos sugeridos; avaliações de produtos em linha própria; a tabela auxiliar de produtos atualizada depois de uma importação |
| **Advanced Custom Fields (ACF)** | Suportado (6.1 ou mais novo, gratuito) | Os tipos de post, taxonomias e grupos de campos dele no formato de exportação próprio, em linha própria, importados antes do conteúdo; campos que guardam IDs remapeados; grupos de campos e posts ligados sugeridos; campos de imagem e arquivo como mídia usada |
| **Elementor** | Ainda não suportado oficialmente, não testado | Veja abaixo |

Não testado quer dizer só isso: nada foi conferido com o Elementor. O que vem a seguir vem de como ele guarda os dados e do que o Importador do WordPress faz com eles.

### Advanced Custom Fields

Suportado, com linha própria: veja [Advanced Custom Fields](../../exporting/advanced-custom-fields/). Em resumo:

- **Tipos de post, taxonomias e grupos de campos** vão no formato de exportação do próprio ACF e são importados antes do conteúdo, atualizando os que têm a mesma chave.
- **Os valores dos campos** viajam nos campos personalizados dos posts e termos; os que guardam IDs (imagem, arquivo, objeto de post, relacionamento, link de página, taxonomia, usuário) são **remapeados** para os itens do outro site.
- O **ACF PRO** (repeater, flexible content, galeria, páginas de opções, blocos) ainda não é coberto; valores em usuários não viajam (um WXR não tem usuários).

### Elementor

- **Os designs** ficam nos campos personalizados de cada página (`_elementor_data`, um JSON com os widgets, com IDs e URLs de imagens), então vão junto com as páginas.
- **IDs e URLs dentro desse JSON** não são remapeados pelo Importador do WordPress: as imagens podem continuar apontando para os IDs e endereços do site antigo.
- **Estilos globais e modelos** (o kit do site, modelos salvos) são conteúdo do próprio Elementor; a ferramenta *Import / Export Kit* do Elementor é feita para eles.
- **Os arquivos de CSS gerados** não são exportados; o Elementor os gera de novo (*Elementor → Tools → Regenerate CSS & Data*, como aparece em inglês).

Usa um desses e quer que seja suportado? [Abra uma issue](https://github.com/eizport/eizport/issues) contando o que você exporta — ou escreva uma compatibilidade, como abaixo.

## A compatibilidade com o WooCommerce

`includes/compat/woocommerce.php`, cadastrada em `eizport_register_compat` quando `WooCommerce` existe:

| Hook | Adiciona |
| --- | --- |
| `eizport_linked_options` | Página da loja, Página do carrinho, Página de finalização de compra, Página Minha conta e Página de termos e condições, como funções |
| `eizport_option_groups` | O grupo *Páginas do WooCommerce* |
| `eizport_denied_options` | `woocommerce_version`, `woocommerce_db_version` |
| `eizport_media_used` | Galerias de produtos e variações (`_product_image_gallery`) e as imagens das categorias dos produtos |
| `eizport_term_media` | O `thumbnail_id` das categorias de produto, remapeado para a imagem importada |
| `import_end` | Salva cada produto importado do jeito que o WooCommerce faz e atualiza a linha dele em `wc_product_meta_lookup` (preços, SKU, estoque: o que os filtros e a ordenação da loja leem) |
| `eizport_suggested_items` | Produtos que blocos e `[products ids="…"]` mostram nos itens escolhidos |
| `export_wp` + `query` | Avaliações de produtos: só as escolhidas, quando a exportação diz quais (`reviews`) |

## A compatibilidade com o Advanced Custom Fields

`includes/compat/advanced-custom-fields.php`, cadastrada em `eizport_register_compat` quando o ACF 6.1 ou mais novo está ativo, e `includes/import-advanced-custom-fields.php` para a importação:

| Hook | Adiciona |
| --- | --- |
| `eizport_content_types` | Tira os tipos de post do próprio ACF das linhas de conteúdo |
| `eizport_look_items`, `eizport_look_data`, `eizport_look_suggested` | A linha *Advanced Custom Fields (ACF)*: os itens dela, a exportação deles (no formato do ACF) e o que os posts escolhidos sugerem |
| `eizport_media_used` | Os anexos dos campos de imagem e arquivo |
| `eizport_suggested_items` | Os posts dos campos objeto de post, relacionamento e link de página |
| `import_start` | Importa os tipos de post, as taxonomias e os grupos de campos (`acf_import_internal_post_type()`) e os registra |
| `import_end` | Mapeia os IDs nos valores e nas regras de localização dos grupos de campos |
| `wp_import_existing_post` | Os posts do próprio ACF num WXR existem pela chave, não pelo título e pela data |

## Como escrever uma

```php
add_action( 'eizport_register_compat', function () {
	if ( ! class_exists( 'My_Plugin' ) ) {
		return;
	}
	add_filter( 'eizport_linked_options', 'my_plugin_eizport_linked' );
	add_filter( 'eizport_option_groups', 'my_plugin_eizport_groups' );
	add_filter( 'eizport_denied_options', 'my_plugin_eizport_denied' );
} );
```

`eizport_register_compat` é disparado em `plugins_loaded` com prioridade 20, depois que todos os plugins carregaram, então as verificações com `class_exists()` são confiáveis.

## Como substituir a incluída

Um plugin que traz o seu próprio suporte remove primeiro o que vem incluído:

```php
remove_action( 'eizport_register_compat', 'Eizport\Compat\WooCommerce\register' );
```

## Testes

Teste as funções da compatibilidade diretamente, para que os testes passem sem o outro plugin instalado, e verifique que ela só se cadastra quando esse plugin está ativo.
