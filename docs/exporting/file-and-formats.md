---
title: "Arquivo e formatos"
description: "Escolha entre o WXR simples do WordPress, um pacote .wxrz ou .zip, um .zip do Playground e um plugin que se importa sozinho; nomes de arquivo e o que cada um contém."
translatedFrom: "eb84d2800a1d07522114a9f68fb8428d95ae84c5"
---

O botão de alternância **Formato**, no painel **Detalhes** acima da lista, tem duas opções; o nome da primeira acompanha o que vai no arquivo.

| Aparece como | Arquivo | Quando | Contém |
| --- | --- | --- | --- |
| **WXR XML** | `.xml` | Arquivos de mídia desativados e nenhuma opção | A exportação do próprio WordPress, sem mudanças |
| **WXR ZIP** | `.zip` | Arquivos de mídia ou opções (o padrão) | Um [pacote como um .zip simples](../../formats/zip-bundle/): o WXR, a mídia, `post_content/`, `options.json` |
| **WXRZ** | `.wxrz` | Igual, com *Pacote como .zip* desativado | O mesmo [pacote como um tar com gzip](../../formats/wxrz-bundle/) |
| **Playground** | `.zip` | Escolhido | Um [pacote de blueprint do Playground](../../formats/playground-zip/) com temas e plugins |
| **Como plugin** | `.zip` | *Como plugin* ativado (não com o Playground) | Um plugin com o pacote dentro, que o importa no outro site com um clique ([como plugin](../as-plugin/)) |

## Como plugin

O botão **Como plugin**, ao lado de *Arquivos de mídia* (o ⓘ explica), embrulha o pacote num `.zip` de plugin. Instale-o no outro site como qualquer plugin e clique em **Executar importação** na linha dele na tela Plugins: sem arquivo para enviar. Veja [como plugin](../as-plugin/).

## Atualizar existentes

**Atualizar existentes**, no painel **Detalhes**, pede à importação que atualize os itens dos tipos de conteúdo escolhidos que já existem no outro site (mesmo tipo e slug) — o ⓘ dele cita esses tipos (todos, com nada marcado), e ele fica desativado quando só linhas sem conteúdo estão marcadas: menus, opções, a aparência — em vez de adicionar cópias. A escolha fica gravada no pacote (`update_existing` no manifesto), e a caixa do importador já vem marcada. Não vale para WXR XML nem Playground. Veja [atualizar os itens existentes](../../getting-started/importing/#atualizar-os-itens-existentes).

## Nome do arquivo

Deixe vazio para o nome de sempre do WordPress, `<site name>.WordPress.<date>` — o texto de marcação mostra o nome exato de hoje. Um nome digitado é sanitizado e a extensão é adicionada.

## Conteúdo como arquivos .html

Com **Configurações → Exportar → Conteúdo como arquivos .html** (ativado por padrão), o conteúdo de cada post vai para `post_content/<slug>.html` em vez de ficar dentro do WXR — fácil de ler, comparar e manter sob controle de versão. Na importação, ele é devolvido antes de o importador rodar. Uma página feita com o [Elementor](../elementor/) vai como `post_content/<slug>.json`: o design dela no formato do Elementor, trazido pela importação do próprio Elementor.

## Cópias

Cada exportação feita na nova tela ou pela habilidade fica guardada como uma **cópia** por um tempo (um dia, por padrão), com um nome impossível de adivinhar, e listada em [Configurações → Cópias](../../settings/copies/).
