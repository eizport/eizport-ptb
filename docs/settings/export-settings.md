---
title: "Configurações de exportação"
description: "Configurações do plugin → Exportar — mídia anexada, configurações vinculadas, pacotes .zip e conteúdo como arquivos .html."
translatedFrom: "f2fe7ce537c3f04919216ec7fdb87b2d0a5deec7"
---

![Configurações do plugin → Exportar.](../../../assets/screenshots/settings-export.webp)

Tudo em um cartão, **Com os itens exportados**:

| Configuração | Padrão | Por exportação (`eizport/export`) |
| --- | --- | --- |
| **Só mídia anexada** — os itens trazem só a mídia anexada e as imagens destacadas, como faz o WordPress, não a mídia usada no conteúdo ([mídia](../../exporting/media/)) | Desativado | `media_used` |
| **Configurações vinculadas** — quando vêm junto as configurações que apontam para os itens exportados: quando os itens são marcados e na exportação, quando marcados, na exportação, nunca ([opções](../../exporting/options/#linked-settings)) | Ambos | `options.linked` |
| **Pacote como .zip** — os pacotes são `.zip` simples em vez de `.wxrz` ([pacote ZIP](../../formats/zip-bundle/)), também dentro de um plugin feito [como plugin](../../exporting/as-plugin/) | Ativado | `zip` |
| **Como plugin: permitir importar de novo** — um plugin exportado [como plugin](../../exporting/as-plugin/) continua oferecendo *Importar de novo* depois de importar | Desativado | — |
| **Como plugin traz o pacote como** — num plugin exportado [como plugin](../../exporting/as-plugin/): um arquivo (`.zip`, ou `.wxrz` com *Pacote como .zip* desativado), ou os arquivos dele ao lado dos do plugin, ou numa pasta `bundle/` | Um arquivo | — |
| **Conteúdo como arquivos .html** — o conteúdo de cada post em `post_content/<slug>.html` (uma página feita com o Elementor: o design dela em `<slug>.json`) | Ativado | `post_content` |
| **Marcar o CSS importado** — as regras CSS das [linhas da aparência](../../exporting/appearance/) vão entre comentários `/* head … */` e `/* tail … */` com o nome do arquivo, para mostrar de onde vieram; o importador pode mudar isso | Ativado | `css_markers` |
