---
title: "Configurações de exportação"
description: "Configurações do plugin → Exportar — mídia anexada, configurações vinculadas, pacotes .zip e conteúdo como arquivos .html."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

![Configurações do plugin → Exportar.](../../../assets/screenshots/settings-export.webp)

Tudo em um cartão, **Com os itens exportados**:

| Configuração | Padrão | Por exportação (`eizport/export`) |
| --- | --- | --- |
| **Só mídia anexada** — os itens trazem só a mídia anexada e as imagens destacadas, como faz o WordPress, não a mídia usada no conteúdo ([mídia](../../exporting/media/)) | Desativado | `media_used` |
| **Configurações vinculadas** — quando vêm junto as configurações que apontam para os itens exportados: quando os itens são marcados e na exportação, quando marcados, na exportação, nunca ([opções](../../exporting/options/#linked-settings)) | Ambos | `options.linked` |
| **Pacote como .zip** — os pacotes são `.zip` simples em vez de `.wxrz` ([pacote ZIP](../../formats/zip-bundle/)) | Desativado | `zip` |
| **Como plugin: permitir importar de novo** — um plugin exportado [como plugin](../../exporting/as-plugin/) continua oferecendo *Importar de novo* depois de importar | Desativado | — |
| **Como plugin traz o pacote como** — num plugin exportado [como plugin](../../exporting/as-plugin/): um arquivo `.wxrz`, ou os arquivos dele ao lado dos do plugin, ou numa pasta `bundle/` | Um arquivo .wxrz | — |
| **Conteúdo como arquivos .html** — o conteúdo de cada post em `post_content/<slug>.html` | Ativado | `post_content` |
