---
title: "WP-CLI"
description: "Exporte e importe pacotes pela linha de comando com wp eizport export e wp eizport import, guarde uma entrega numa pasta para comparar e rode as habilidades com wp eval."
translatedFrom: "eb84d2800a1d07522114a9f68fb8428d95ae84c5"
---

## wp eizport export

Exporta como a tela Exportar faz, para uma cópia guardada no site, e imprime o caminho dela. Cada execução é um processo próprio, então um script pode fazer quantas exportações precisar.

```bash
wp eizport export --types=page:43,product --options=basics,page_on_front
wp eizport export --types=page:43+44 --as-plugin --update-existing --filename=delivery
wp eizport export --types=page:43 --playground --themes=twentytwentyfive --plugins=woocommerce/woocommerce.php --landing=page:43 --language=pt_BR
wp eizport export --types=page --menus=classic:18 --filename=header
```

| Argumento | |
| --- | --- |
| `--types=<types>` | Tipos de conteúdo, separados por vírgula: `tipo:ID+ID` para alguns itens, o tipo sozinho para todos. Nenhum: todo o conteúdo |
| `--options=<options>` | Grupos de opções (como `basics`) e nomes de opções, separados por vírgula |
| `--appearance=<parts>` | A [aparência](../../exporting/appearance/) do site: `all`, ou linhas separadas por vírgula, cada uma inteira ou com as chaves dos itens depois de dois-pontos, unidas por `+` (`theme_mods:custom_logo+background_color,custom_css:r:1a2b3c4d5e`); com o ACF ativo, `advanced-custom-fields` leva os [tipos de post, taxonomias e grupos de campos](../../exporting/advanced-custom-fields/) dele (`advanced-custom-fields:group_abc123+post_type_def456`); com o Elementor ativo, `elementor` leva as [configurações do site e os modelos](../../exporting/elementor/) dele (`elementor:kit+t:28`) |
| `--css-mode=<mode>` | Como as regras CSS entram na importação: `end` (padrão), `start`, `replace` (o bloco de mesmo nome; no fim sem nenhum) ou `replace_all` |
| `--[no-]css-markers` | Comentários head/tail em volta das regras CSS (padrão: Configurações → Exportação) |
| `--menus=<menus>` | [Menus](../../exporting/menus/): `all`, ou `classic:ID+ID` (IDs de termos de menu) e `navigation:ID+ID` (IDs de `wp_navigation`), separados por vírgula. Nenhum: sem menus (todo o conteúdo já os tem) |
| `--[no-]media` | Arquivos de mídia dentro; padrão: sim |
| `--zip` | Um pacote `.zip` em vez de `.wxrz` |
| `--as-plugin` | Um `.zip` de [plugin](../../exporting/as-plugin/) que importa o próprio pacote |
| `--update-existing` | Pede à importação que [atualize os itens existentes](../../getting-started/importing/#atualizar-os-itens-existentes) |
| `--playground` | Um [.zip do Playground](../../formats/playground-zip/) em vez de um pacote (não com `--as-plugin`, `--update-existing` nem `--dir`) |
| `--themes=<stylesheets>` | Com `--playground`: temas a instalar, separados por vírgula (um tema descendente traz o ascendente) |
| `--plugins=<files>` | Com `--playground`: arquivos de plugin a instalar, separados por vírgula (`woocommerce/woocommerce.php`), ativos ou inativos como neste site |
| `--mu-plugins=<files>` | Com `--playground`: plugins obrigatórios a gravar, arquivos de `wp-content/mu-plugins` separados por vírgula (`cpt.php`), com as pastas que eles carregam |
| `--landing=<where>` | Com `--playground`: onde abre — `home` (padrão), `admin`, `page:ID` (salvo como `/?pagename=<o caminho dela>`) ou um caminho (`/loja/`) |
| `--language=<language>` | Com `--playground`: o idioma em que abre — `site` (padrão), `user`, `en_US` ou um locale instalado (`pt_BR`) |
| `--filename=<name>` | Nome do arquivo, sem extensão |
| `--dir=<folder>` | Grava também os arquivos do pacote numa pasta (veja abaixo) |

### Uma entrega numa pasta

`--dir` grava os arquivos do pacote numa pasta: `manifest.json`, o WXR **formatado linha a linha**, `post_content/*.html`, `uploads/…` e `options.json`. Os arquivos de pacote que uma exportação anterior gravou ali são substituídos; os outros arquivos (`.git`, anotações) ficam. Mantenha a pasta sob controle de versão, e o `git diff` mostra o que mudou desde a última entrega — um título, um preço — além das datas da exportação.

```bash
wp eizport export --types=page:43,product --filename=delivery --dir=deliveries/site
git -C deliveries diff
```

O WXR da pasta mantém o mesmo nome de uma entrega para a outra — `<--filename>.xml`, senão `<site>.WordPress.xml`, sem a data —, então o `git diff` mostra o que mudou nele. A pasta volta com `wp eizport import <pasta>`.

## wp eizport import

Importa um pacote `.wxrz` ou `.zip`, uma pasta de pacote (do `--dir`), um WXR compactado com gzip (`.xml.gz`) ou um `.xml` simples com o Importador do WordPress — a mídia vem do pacote, os arquivos de `post_content/` voltam para o conteúdo e as opções são definidas no final.

```bash
wp eizport import site.wxrz --user=admin
```

| Argumento | |
| --- | --- |
| `<file>` | O arquivo ou a pasta de pacote a importar |
| `--user=<login>` | A quem os posts são atribuídos; padrão: o primeiro administrador |
| `--[no-]update-existing` | Atualiza os itens que já existem (mesmo tipo e slug) em vez de adicionar cópias; padrão: o que o pacote pede |
| `--css-mode=<mode>` | Como as regras CSS do pacote entram: `end`, `start`, `replace` (os blocos head/tail de mesmo nome que este site tem; sem nenhum, no fim) ou `replace_all`; padrão: o que o pacote pede |
| `--[no-]css-markers` | Comentários head/tail em volta delas; padrão: o que o pacote pede |

Ele precisa do plugin Importador do WordPress instalado (não precisa estar ativo).

## Exportar com a habilidade

`wp eizport export` roda a habilidade [`eizport/export`](../abilities/#eizportexport); para passar o que não tem flag (filtros, avaliações), rode a própria habilidade:

```bash
wp eval '
wp_set_current_user( 1 );
$copy = wp_get_ability( "eizport/export" )->execute( array(
	"types"   => array( "page" => array( "ids" => array( 43 ) ) ),
	"options" => array( "groups" => array( "basics" ) ),
) );
echo is_wp_error( $copy ) ? $copy->get_error_message() : $copy["download_url"];
'
```

A cópia fica em `wp-content/uploads/eizport/tmp/`; uma exportação por processo `wp`.

## Outras habilidades

Qualquer habilidade funciona do mesmo jeito: `wp_get_ability( 'eizport/get-copies' )->execute()`, `wp_get_ability( 'eizport/delete-copies' )->execute( array( 'all' => true ) )`…
