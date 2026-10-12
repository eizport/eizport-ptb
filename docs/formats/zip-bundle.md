---
title: "Pacote ZIP"
description: "O mesmo pacote como um .zip comum, que qualquer computador abre — como ativá-lo e como ele é lido."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

Com **Configurações → Exportar → Pacote como .zip**, os pacotes são arquivos `.zip` comuns em vez de `.wxrz`. As entradas são **exatamente as mesmas** — `manifest.json`, o WXR, `options.json`, `post_content/`, `uploads/` — então tudo o que está na [página do WXRZ](../wxrz-bundle/) vale aqui.

## Por quê

Um `.zip` abre com um clique duplo em qualquer computador; o `.wxrz` precisa de uma ferramenta que leia `.tar.gz`. Escolha `.zip` quando pessoas forem olhar dentro do arquivo; mantenha `.wxrz` quando ele só viaja entre sites.

## Por exportação

A configuração é o padrão. A habilidade [`eizport/export`](../../developers/abilities/#eizportexport) aceita `zip: true` ou `zip: false` para uma exportação.

## Como é gravado e lido

O plugin grava o pacote tar e o transforma num `.zip` (com `ZipArchive`, ou o PclZip dos arquivos básicos do WordPress) e, na importação, transforma o `.zip` de volta num tar antes de lê-lo — um só caminho de código para os dois, com as mesmas verificações:

- só as entradas do pacote são aproveitadas (o manifesto, um `.xml` no nível de cima, `options.json`, `post_content/*.html`, arquivos seguros de `uploads/`);
- o tamanho descompactado conta para o mesmo limite de 512 MB.

## Diferenciando pacotes de arquivos do Playground

Os dois terminam em `.zip`. Um pacote tem `manifest.json` e não tem `blueprint.json`; um `.zip` do Playground tem `blueprint.json`. O importador e a lista de Cópias os diferenciam assim.
