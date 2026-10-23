---
title: "Agentes de IA"
description: "Deixe um agente de IA exportar, importar e gerenciar cópias por você: as habilidades via MCP, o que pedir, o que o agente vê e a segurança embutida."
translatedFrom: "9fd577e7caba305c2b5ac04e7413466a6437c413"
---

O Eizport é **pensado para agentes**: tudo o que as telas fazem é uma [habilidade](../abilities/) da Abilities API do WordPress — uma ação com nome, um JSON Schema para a entrada e a saída, uma verificação de permissão e indicações se ela só lê ou se exclui. É exatamente o que um agente de IA precisa para agir num site com segurança, sem raspar telas.

## Conecte um agente

1. Instale o [MCP Adapter](https://github.com/WordPress/mcp-adapter) no site. Ele expõe as habilidades como **ferramentas MCP**.
2. Conecte seu cliente MCP (um assistente de IA, um agente de IDE, um agente de linha de comando) ao site como descrito na documentação do adaptador, com um usuário que possa exportar (e importar, se o agente for importar).
3. Peça em palavras simples. O agente lista as ferramentas `eizport/*`, lê os esquemas e as chama.

Num site local do [WordPress Studio](https://developer.wordpress.com/studio/), o próprio servidor MCP do Studio pode executar as mesmas habilidades pela ferramenta `wp_cli` (`wp eval`, veja [WP-CLI](../wp-cli/)).

## O que pedir

| Você pede | O agente chama |
| --- | --- |
| “O que posso exportar deste site?” | `eizport/get-export-options` |
| “Qual página é a página inicial, e quais são a loja e a finalização de compra?” | `eizport/get-items` (`roles` em cada item) |
| “Exporte a página inicial com as imagens e as configurações básicas como .zip.” | `eizport/get-items`, depois `eizport/export` com `types`, `options.groups: ["basics"]`, `zip: true` |
| “Exporte todos os produtos publicados da categoria Sapatos, para o Playground, com o WooCommerce.” | `eizport/get-export-options` (ID do termo), depois `eizport/export` com `filters` e `blueprint.plugins` |
| “Quais configurações mencionam a newsletter?” | `eizport/get-options` com `search` |
| “Liste as cópias e exclua as que têm mais de um dia.” | `eizport/get-copies`, depois `eizport/delete-copies` |
| “Importe aqui a cópia store-products.” | `eizport/import` com `copy` |
| “Salve isto como minha exportação padrão.” | `eizport/save-export-defaults` |

## O que o agente vê

Cada ferramenta traz a **descrição**, o **esquema de entrada** (tipos, enums, padrões, campos obrigatórios) e o **esquema de saída**, além das anotações MCP:

- **somente leitura** — `get-export-options`, `get-items`, `get-options`, `get-copies`, `get-export-defaults`, `get-settings`: seguras para chamar enquanto explora;
- **destrutivas** — `delete-copies`, `clear-export-defaults`: um agente bem-comportado pergunta antes de chamá-las;
- **idempotentes** — chamar de novo não muda mais nada.

Os erros voltam como códigos com que um agente pode agir: `eizport_invalid_type` (um tipo de post que não pode ser exportado), `eizport_invalid_options` (uma opção que nunca é exportada), `eizport_invalid_blueprint` (um tema ou plugin não instalado), `eizport_export_unavailable` (uma exportação por solicitação: chame de novo).

## Segurança embutida

- **As permissões do WordPress:** o agente só pode fazer o que o usuário dele pode — `export` para exportações e cópias, `import` para importar, `manage_options` para configurações.
- **Nenhum segredo sai:** URLs do site, o e-mail do administrador, chaves e salts, usuários e funções, plugins ativos, transients e caches nunca são exportados, não importa o que o agente peça — nem opções, modificações do tema ou campos de widget com nome de segredo (senhas, tokens, chaves de API e de licença).
- **As cópias são temporárias e ocultas:** um token aleatório de 16 caracteres em cada link, `noindex` e robots.txt, uma pasta bloqueada contra acesso direto, e exclusão quando expiram ([cópias](../../settings/copies/)).
- **As importações mesclam**, não sobrescrevem o site.

## Documentação para agentes

Esta documentação também é escrita para agentes:

- [`/llms.txt`](https://eizport.github.io/llms.txt) — um índice de todas as páginas, com descrições, apontando para as versões em Markdown;
- [`/llms-full.txt`](https://eizport.github.io/llms-full.txt) — a documentação inteira num só arquivo;
- cada página em Markdown — o mesmo URL terminando em `.md` (veja “Ver em Markdown” na parte inferior de cada página).

Aponte seu agente para o `llms.txt` e ele conhece o plugin antes da primeira chamada.
