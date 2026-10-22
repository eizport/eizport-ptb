---
title: "Pacote WXRZ"
description: "O formato .wxrz, entrada por entrada — manifesto, WXR, mídia, post_content e options.json — e as regras de segurança quando um pacote é aberto."
translatedFrom: "eb84d2800a1d07522114a9f68fb8428d95ae84c5"
---

Um arquivo `.wxrz` é um **arquivo tar compactado com gzip** (`ustar`). Qualquer ferramenta que lê `.tar.gz` o abre:

```bash
tar tzvf site.wxrz        # list
tar xzf site.wxrz -C out  # extract
```

## Entradas

```text
manifest.json                      required, first
<site>.WordPress.<date>.xml        the WXR (WordPress eXtended RSS)
options.json                       optional: settings
post_content/<slug>.html           optional: one per post with content
post_content/<slug>.json           optional: one per page built with Elementor, its design
uploads/2026/10/photo.jpg          optional: media files, as in wp-content/uploads
```

## manifest.json

```json
{
  "format": "wxrz",
  "version": 1,
  "generator": "Eizport 1.0.0",
  "site": "https://example.com/",
  "created": "2026-10-10T22:11:50+00:00",
  "wxr": [ "example.WordPress.2026-10-10.xml" ],
  "files": {
    "https://example.com/wp-content/uploads/2026/10/photo.jpg": "uploads/2026/10/photo.jpg"
  },
  "options": "options.json",
  "update_existing": true,
  "term_media": {
    "product_cat": { "western": { "thumbnail_id": 13 } }
  },
  "post_content": {
    "home.html": { "id": 43, "type": "page", "slug": "home" }
  }
}
```

| Chave | Significado |
| --- | --- |
| `format`, `version` | Sempre `"wxrz"` e `1` para esta estrutura; um leitor recusa outras versões |
| `generator`, `site`, `created` | Quem fez, de onde, quando (informativo) |
| `wxr` | O nome da entrada do WXR (a primeira é lida) |
| `files` | URL do anexo no WXR → entrada no arquivo. Quando o importador baixa esse URL, recebe a entrada como resposta |
| `options` | A entrada do arquivo de opções, quando há uma |
| `update_existing` | `true` quando a importação deve atualizar os itens que já existem (mesmo tipo e slug); a caixa do importador já vem marcada |
| `term_media` | Taxonomia → slug do termo → chave de meta → ID do anexo no site que exportou, para os termos que os itens usam ([`eizport_term_media`](../../developers/hooks/#eizport_term_media)); remapeado para o anexo importado no fim da importação |
| `post_content` | Arquivo de conteúdo → o item a que pertence: `id`, `type` e `slug` originais |

## options.json

```json
{
  "format": "eizport-options",
  "version": 1,
  "options": {
    "show_on_front": "page",
    "page_on_front": "43",
    "blogname": "Example"
  },
  "post_ids": [ "page_on_front" ]
}
```

As `options` são definidas com `update_option()` quando a importação termina. Cada nome em `post_ids` guarda um ID de post: recebe o ID que o importador deu a esse post, ou fica de fora se o post não foi importado.

## post_content/

Cada arquivo é o `post_content` do item, byte por byte; no WXR, o `<content:encoded>` do item fica vazio. Na importação, o conteúdo volta para o item do WXR com o mesmo ID original (ou, senão, o mesmo tipo e slug) antes de o importador lê-lo. O filtro [`eizport_post_content`](../../developers/hooks/#eizport_post_content) pode alterá-lo.

## Abrindo um pacote com segurança

Quando um pacote é importado:

- só são gravadas as entradas que o manifesto lista, mais os arquivos de `post_content/` e o `options.json`;
- toda entrada de mídia tem de estar em `uploads/`, sem `..`, barra invertida, byte nulo ou `//`, e com um tipo de arquivo que o WordPress permite enviar (`wp_check_filetype()`): nunca PHP;
- tudo vai para uma nova pasta privada em `wp-content/uploads/eizport/tmp/`, excluída após a importação;
- o arquivo pode se expandir até **512 MB** no máximo (filtro [`eizport_wxrz_max_bytes`](../../developers/hooks/#eizport_wxrz_max_bytes)), lido em etapas, contra bombas de descompactação;
- nomes de entrada mais longos do que o `ustar` permite são ignorados na exportação: o importador baixa esses arquivos.

> Só importe arquivos em que você confia, como com qualquer WXR: um pacote não pode executar código, mas o conteúdo e as configurações dele passam a ser do seu site.
