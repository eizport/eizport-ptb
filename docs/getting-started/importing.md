---
title: "Importação"
description: "Como um pacote é importado com o Importador do WordPress, o que acontece com a mídia, os arquivos de conteúdo e as opções, e as outras formas de importar."
translatedFrom: "12cb65d62b687dc57030c9df21b19288070463ac"
---

## Com o Importador do WordPress

Envie um pacote `.wxrz` ou `.zip` em **Ferramentas → Importar → WordPress**, como qualquer arquivo WXR. Arquivos WXR compactados com gzip (`.xml.gz`) e arquivos `.xml` simples também funcionam.

Exportou [como plugin](../../exporting/as-plugin/)? Instale o `.zip` em **Plugins → Adicionar plugin → Enviar plugin** e use o **Executar importação** dele em Ferramentas.

**Ferramentas → Importar** abre direto o Importador do WordPress, e a tela dele leva a **Todos os importadores** ([Configurações → Geral](../../settings/general/) desliga isso).

O importador fica com o visual da tela Exportar do plugin: um cabeçalho com o botão principal da etapa (*Enviar arquivo e importar*, *Enviar*), cada etapa em cartões e um botão **Escolher arquivo** no idioma do painel, na linha abaixo do rótulo; *Enviar arquivo e importar* espera até um arquivo ser escolhido. **Estilizar Ferramentas → Importar → WordPress**, em [Configurações → Geral](../../settings/general/) do plugin, desliga isso; os pacotes são aceitos de qualquer jeito.

## Atualizar os itens existentes

O Importador do WordPress pula um item que já existe e adiciona uma cópia quando só o slug coincide: a página *Home* do tema vira `home-2`, e uma segunda entrega do mesmo pacote não muda nada. **Atualizar páginas, posts e produtos que já existem (mesmo slug) em vez de adicionar cópias**, na etapa 1 do importador (e na tela de um plugin exportado [como plugin](../../exporting/as-plugin/)), muda isso:

- um item do mesmo tipo e slug **é** o item: ele recebe o título, o conteúdo, o resumo, a ordem, os campos personalizados e os termos do pacote, e mantém o ID;
- as configurações que apontam para ele (a página inicial, a página da loja…) continuam apontando para ele, então a *Home* do tema vira a página inicial com o conteúdo do pacote;
- os anexos que já existem são reaproveitados, e as URLs deles no conteúdo atualizado apontam para este site.

Fica desligado, a não ser que o pacote peça: **Atualizar existentes**, no painel **Detalhes** da tela Exportar, grava a escolha no pacote, e a caixa já vem marcada. Quem importa ainda pode desmarcar.

## Versões

- **Um pacote feito por um Eizport mais novo** — que precisa de uma versão que este site não tem — não é importado: o envio, `wp eizport import`, a habilidade e a tela de um plugin de pacote dizem de qual Eizport ele precisa, e o plugin de pacote oferece **Atualizar agora**. Pacotes feitos por versões anteriores importam como antes.
- **O Importador do WordPress** precisa ser 0.9.6 ou mais novo (o Eizport para e pede para atualizá-lo). Uma versão mais nova que a testada pelo Eizport ainda importa, com um aviso na tela do importador, na do plugin de pacote e no WP-CLI.

## Nos bastidores

Antes de o importador ler qualquer coisa:

1. **O pacote é descompactado** em uma pasta privada dentro de `wp-content/uploads/eizport/tmp/<random>/`. Só os arquivos listados no manifesto são gravados, só dentro dessa pasta e só com os tipos de arquivo que o WordPress permite enviar (nunca PHP).
2. **O WXR substitui o envio**, então as verificações e etapas do próprio importador rodam como sempre.
3. **Os arquivos de conteúdo voltam ao lugar:** cada `post_content/<slug>.html` é devolvido ao seu item, pelo ID original ou, se não houver, pelo slug.
4. **A mídia é servida a partir do pacote:** quando o importador baixa um anexo, um filtro `pre_http_request` responde com o arquivo da pasta em vez da rede.
5. **As opções são aplicadas no final** (`import_end`), com cada ID de post remapeado para o post importado. Opções que apontam para itens que não foram importados ficam de fora.
6. **As imagens dos termos são remapeadas no final:** os term meta que guardam um ID de anexo (a miniatura de uma categoria de produto), listados em `term_media` no manifesto, passam a apontar para o anexo importado.

A pasta é excluída quando a importação termina ou é abandonada.

## De uma cópia no site

As cópias feitas pela tela Exportar ficam listadas nas **Configurações → Cópias** do plugin (pela linha dele em Plugins). Cada uma tem a ação **Importar**, que a importa no mesmo site — útil para restaurar conteúdo ou testar um pacote.

## Com código

- A habilidade [`eizport/import`](../../developers/abilities/#eizportimport) importa uma `copy` ou um `attachment_id` (um `.wxrz`, `.zip` ou `.xml` na biblioteca de mídia).
- [`wp eizport import <file>`](../../developers/wp-cli/) faz isso pela linha de comando — também a partir de uma pasta de pacote gravada por `wp eizport export --dir`, e com `--update-existing`.

## O que é remapeado

| No arquivo | Na importação |
| --- | --- |
| URLs dos anexos | Os arquivos vêm do pacote; o importador cria os anexos e os tamanhos de imagem como sempre. Um anexo que o site já tem é reaproveitado, com ou sem *Atualizar existentes*: as URLs dele (e as dos tamanhos de imagem) no conteúdo importado apontam para o arquivo daqui |
| `post_content/<slug>.html` | Devolvido ao conteúdo do item |
| `options.json` | Definido no final; as opções listadas em `post_ids` recebem o ID do post importado |
| `term_media` no manifesto | Os term meta que guardam um ID de anexo recebem o ID do anexo importado |
| IDs de posts em blocos | Os blocos Imagem, Galeria, Capa, Mídia e texto, Arquivo, Vídeo e Áudio (`"id"`, `"ids"`, `"mediaId"`, `wp-image-N`, o `data-id` da galeria), `[gallery ids]` e os links de navegação para páginas e posts apontam para os itens importados, nos itens novos e atualizados e nos widgets de bloco. Plugins acrescentam os blocos deles com `eizport_block_id_attributes` |
| A aparência ([aparência](../../exporting/appearance/)) | As regras CSS entram no CSS deste site como o pacote pede (no fim ou no início; comentários head/tail), ou — escolhido na importação — substituindo um bloco head/tail de mesmo nome ou todo o CSS — a etapa 1 do importador, `css_mode`/`css_markers` e `--css-mode`/`--[no-]css-markers` mudam isso; theme mods, grupos de estilo e widgets se juntam aos deste site |
| Termos | As categorias, tags e outros termos dos itens chegam completos (descrição, ascendente, term meta) e são criados com os ascendentes primeiro; um termo que o site já tem (mesmo slug) é usado como está |
| Links para o site de origem | O endereço do site de origem (o `base_site_url` do WXR) vira o deste site no conteúdo e no resumo dos itens importados (também escapado nos atributos de bloco, http ou https), nos links personalizados dos menus e nos widgets e theme mods |
| Produtos e suas variações, páginas e suas descendentes | Importados com seus ascendentes, como faz o Importador do WordPress |
