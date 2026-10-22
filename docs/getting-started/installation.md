---
title: "Instalação"
description: "Instale o plugin pelo diretório de plugins do WordPress.org, confira os requisitos e adicione o Importador do WordPress para importar."
translatedFrom: "3b3b2992af6efdf59a350ba71692a4ee09e6a285"
---

## Pelo diretório de plugins

1. No WordPress, vá em **Plugins → Adicionar plugin** e pesquise por **Eizport** — ou abra a página dele, [wordpress.org/plugins/eizport](https://br.wordpress.org/plugins/eizport/).
2. Clique em **Instalar agora** e depois em **Ativar**. Ferramentas → Exportar agora abre a nova tela Exportar.

As atualizações chegam do mesmo jeito, em **Painel → Atualizações**.

## Com WP-CLI

```bash
wp plugin install eizport --activate
```

## Versões de desenvolvimento e versões anteriores

Toda versão também está no [GitHub](https://github.com/eizport/eizport/releases), como `eizport.zip`: adicione-o em **Plugins → Adicionar plugin → Enviar plugin**.

## Experimente antes

Abra a [demonstração ao vivo no WordPress Playground](https://playground.wordpress.net/?blueprint-url=https://raw.githubusercontent.com/eizport/eizport/trunk/playground/blueprint.json&language=pt_BR): um site descartável no seu navegador com o plugin e páginas de exemplo.

## Para importar

Importar exige o **Importador do WordPress**. Em **Ferramentas → Importar**, clique em **Instalar agora** abaixo de *WordPress* e depois em **Executar importador**. Exportar funciona sem ele.

## Requisitos em detalhe

| Requisito | Por quê |
| --- | --- |
| WordPress 7.0+ | As telas Exportar e Configurações são construídas com `@wordpress/build` e iniciadas pelo `@wordpress/boot` dos arquivos básicos do WordPress. |
| PHP 7.4+ | O mínimo do plugin. |
| `zlib` | Os pacotes `.wxrz` são arquivos tar compactados com gzip. |
| `ZipArchive` (ou PclZip) | Pacotes `.zip` e arquivos `.zip` do Playground. O PclZip dos arquivos básicos do WordPress é a contingência. |
| Abilities API | Faz parte do WordPress 6.9+; as telas usam as habilidades do plugin. |

## Desinstalação

Excluir o plugin remove `wp-content/uploads/eizport/` (cópias temporárias e importações inacabadas), as configurações dele e os padrões de exportação salvos (o do site e os de cada usuário).
