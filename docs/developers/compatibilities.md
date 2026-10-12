---
title: "Compatibilidades"
description: "Quais plugins são suportados (WooCommerce) e quais ainda não oficialmente (ACF, Elementor), como o suporte é feito com os hooks públicos e como um plugin traz o seu próprio."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

O suporte a outro plugin é uma **compatibilidade**: um arquivo que usa só os [hooks](../hooks/) públicos. O plugin traz uma, para o WooCommerce; qualquer plugin pode adicionar a sua — ou substituir a que vem incluída.

## Resumo

| Plugin | Situação | O que esperar |
| --- | --- | --- |
| **WooCommerce** | Suportado | Produtos, variações, pedidos e cupons como tipos de conteúdo; as páginas da loja, do carrinho, de finalização de compra, Minha conta e de termos como configurações vinculadas; galerias de produtos e imagens das categorias; produtos mostrados por blocos sugeridos; avaliações de produtos em linha própria; a tabela auxiliar de produtos atualizada depois de uma importação |
| **Advanced Custom Fields (ACF)** | Ainda não suportado oficialmente, não testado | Veja abaixo |
| **Elementor** | Ainda não suportado oficialmente, não testado | Veja abaixo |

Não testado quer dizer só isso: nada aqui foi conferido com esses plugins. O que vem a seguir vem de como eles guardam os dados e do que o Importador do WordPress faz com eles.

### Advanced Custom Fields

- **Os valores dos campos** ficam nos campos personalizados de cada post (post meta), e o WXR leva os campos personalizados de todos os posts, então eles vão junto com os itens exportados.
- **Campos que guardam IDs** (imagem, arquivo, galeria, relacionamento, objeto de post, link de página, usuário) mantêm os IDs do site antigo: o Importador do WordPress não remapeia IDs dentro de campos personalizados. Onde o outro site dá IDs novos aos itens, esses campos apontam para o item errado ou para nada.
- **Os grupos de campos** são guardados pelo ACF como tipos de post próprios, ou em arquivos JSON locais no tema; nenhum deles é escolhido por você. Exporte-os com as ferramentas do próprio ACF (*ACF → Tools → Export*, como aparece em inglês), ou mantenha-os no JSON do tema.
- **As páginas de opções** guardam os valores em opções do site (nomes que começam com `options_`), que a linha **Opções** pode exportar pelo nome.

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
