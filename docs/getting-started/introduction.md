---
title: "Introdução"
description: "O que o Eizport faz, como ele se encaixa na exportação e na importação do próprio WordPress e para onde ir em seguida."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

O Eizport leva conteúdo do WordPress de um site para outro **com tudo o que o faz funcionar**: os arquivos de mídia, as configurações que apontam para o conteúdo e o conteúdo de cada post como um arquivo legível. Ele se apoia nas ferramentas que o WordPress já tem — **Ferramentas → Exportar** e o **Importador do WordPress** — em vez de substituí-las.

![A tela Exportar: tipos de conteúdo, Avaliações de produtos, Anexos e Opções numa só lista, com os Detalhes do arquivo acima dela.](../../../assets/screenshots/export-screen.webp)

## O que muda

| Só o WordPress | Com o Eizport |
| --- | --- |
| Um tipo de conteúdo, ou tudo | Vários tipos de uma vez, cada um inteiro ou **por itens** — e o que eles usam aparece como **sugestão** (os produtos que uma página mostra, as configurações que apontam para ela, as avaliações de produtos) |
| Um arquivo WXR (`.xml`) que aponta para a mídia no site antigo | Um **pacote** (`.wxrz` ou `.zip`) com os arquivos de mídia dentro |
| O importador baixa cada imagem do site antigo | A mídia vem do pacote: o site antigo pode estar off-line |
| As configurações ficam para trás | As **opções** vão junto: as que apontam para os itens (página inicial, loja…) são sugeridas e mapeadas para os novos IDs; acrescente qualquer outra uma a uma |
| Conteúdo dentro do XML | O conteúdo de cada post como `post_content/<slug>.html` |
| — | Um `.zip` do **WordPress Playground** que inicia um site com o conteúdo, o tema e os plugins |
| — | **Como plugin**: um `.zip` de plugin que se importa sozinho no outro site com um clique ([como plugin](../../exporting/as-plugin/)) |
| O visual próprio do importador | **Ferramentas → Importar** abre direto o Importador do WordPress, com o visual da tela Exportar |
| — | Cada etapa como uma **habilidade** (Abilities API, REST, MCP) e um comando **WP-CLI** |

## Não é uma migração completa — de propósito

O Eizport **não copia o banco de dados inteiro**, e é justamente essa a ideia:

- **Uma migração do site inteiro** (um dump do banco de dados, um plugin de migração) leva tudo: transients, registros, sessões, spam, as tabelas de plugins removidos há anos, usuários, os URLs do site antigo — e sobrescreve o site de destino. A limpeza depois é o custo escondido.
- **A exportação do WordPress** é simples, mas deixa lacunas: os arquivos de mídia ficam no site antigo (o importador precisa baixá-los, então ele precisa estar on-line), as configurações não viajam, a página inicial deixa de ser a página inicial, e as imagens usadas no conteúdo sem estarem anexadas ficam para trás.

O plugin leva **o que você escolhe e tudo de que isso depende**: os itens, seus arquivos de mídia, a mídia que o conteúdo deles usa, as configurações que apontam para eles (com os IDs remapeados na importação) e as configurações que você escolher — e **mescla** tudo isso no site de destino. URLs do site, usuários, chaves, funções, plugins ativos, transients e caches nunca vão.

Use uma migração completa para clonar ou mover um site inteiro para outra hospedagem. Use o Eizport para levar **conteúdo**: uma seção, o catálogo de uma loja, um conjunto de páginas, uma página inicial com suas configurações, uma demonstração.

## Como se encaixa

```text
Tools → Export ──► eizport/export ──► .wxrz / .zip / .xml / Playground .zip
                                          │
Tools → Import → WordPress ◄──────────────┘  (or eizport/import, or wp eizport import)
```

- **A exportação** acontece na nova tela Exportar (ou na clássica, se você preferir) e pela habilidade `eizport/export`. As duas rodam o mesmo código: `export_wp()` com uma consulta restrita, mais o pacote em volta.
- **A importação** usa o Importador do WordPress — ou o arquivo exportado [como plugin](../../exporting/as-plugin/) o executa com um clique. O plugin desempacota o pacote primeiro, serve a partir dele a mídia que o importador baixa, devolve os conteúdos `.html` ao WXR e define as opções quando a importação termina.

## Requisitos

- WordPress **7.0** ou superior (as telas Exportar e Configurações usam a estrutura de telas do painel do WordPress).
- PHP **7.4** ou superior, com `zlib` (para `.wxrz`) e `ZipArchive` ou o PclZip dos arquivos básicos do WordPress (para `.zip`).
- O plugin **Importador do WordPress** para importar (o WordPress oferece instalá-lo em Ferramentas → Importar).

## Próximos passos

- [Instale o plugin](../installation/) e [faça sua primeira exportação](../your-first-export/).
- Conheça a [tela Exportar](../../exporting/export-screen/) ou vá direto aos [formatos de arquivo](../../formats/wxrz-bundle/) e às [habilidades](../../developers/abilities/).
