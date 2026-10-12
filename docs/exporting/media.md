---
title: "Mídia"
description: "Quais anexos vêm com os itens exportados, como os arquivos de mídia vão dentro dos pacotes e a configuração \"Só mídia anexada\"."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

## Quais anexos são exportados

Quando você exporta **alguns** itens (não todo o conteúdo):

1. **O WordPress** adiciona a mídia anexada a cada item (`post_parent`) e a imagem destacada (`_thumbnail_id`).
2. **O plugin** também adiciona a mídia que os itens **usam no conteúdo sem estar anexada**: imagens, galerias, coberturas, arquivos, vídeo e áudio inseridos da biblioteca. Ele a encontra por:
   - atributos de bloco: `"id"` (imagem, cobertura, arquivo, vídeo, áudio), `"mediaId"` (mídia e texto), `"ids"` (galeria);
   - classes `wp-image-<id>` e o shortcode `[gallery ids="…"]`;
   - URLs de arquivos enviados, inclusive os redimensionados (`-300x200`), por meio de `attachment_url_to_postid()`.
3. **Plugins** adicionam mais com [`eizport_media_used`](../../developers/hooks/#eizport_media_used) — a compatibilidade com o WooCommerce adiciona as galerias de produtos.

Desative a etapa 2 em **Configurações → Exportar → Só mídia anexada** ou, por exportação, com `media_used: false`.

## Arquivos de mídia no pacote

**Arquivos de mídia** (ativado por padrão) coloca os arquivos de cada anexo exportado dentro do pacote, em `uploads/`, com as mesmas pastas do site antigo. Na importação, eles são servidos ao importador a partir do pacote, então nada é baixado e o site antigo pode estar off-line.

Só entram os arquivos da pasta de envios deste site; a mídia hospedada fora (uma CDN, S3) fica para o importador baixar, como sempre.

## Exemplo

Uma página com uma imagem anexada (A), uma imagem destacada diferente (B) e uma imagem da biblioteca no conteúdo (C):

| Configuração | Exportado |
| --- | --- |
| Padrão | a página, A, B, C |
| Só mídia anexada | a página, A, B |
