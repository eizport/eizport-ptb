---
title: ".zip do Playground"
description: "Um pacote de blueprint do WordPress Playground que inicia um site com o conteúdo, a mídia, o tema e os plugins exportados — como é montado e como executá-lo."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

Com o formato **Playground**, a exportação é um `.zip` pronto para o [WordPress Playground](https://wordpress.org/playground/): um pacote de blueprint que instala os temas e plugins que você escolheu e importa o conteúdo.

![Temas e plugins do Playground: o que o .zip instala.](../../../assets/screenshots/playground-options.webp)

## Execute

```bash
npx @wp-playground/cli@latest server --blueprint=./site.zip
```

Ou abra o link dele no playground.wordpress.net (nova tela: a ação **Abrir no Playground** em [Cópias](../../settings/copies/); tela clássica: *Manter uma cópia para abrir por link*). A cópia é servida com os cabeçalhos CORS de que o Playground precisa.

**Mesmo de um servidor local.** O Playground roda no seu navegador, então uma cópia feita no WordPress Studio ou em qualquer site `localhost` abre no playground.wordpress.net sem ser enviada a lugar nenhum — o navegador a coleta da sua máquina (o link só funciona no seu computador).

## Temas e plugins

Qualquer tema ou plugin **instalado** pode entrar. Os do diretório do WordPress.org não são copiados: o blueprint os instala do diretório, **na versão que este site tem** (`woocommerce.11.2.1.zip`). Os outros — premium, personalizados, internos — são compactados a partir do site dentro do pacote, e o Playground os instala dali. A própria verificação de atualizações do WordPress diz quais vêm do diretório.

> **Use com responsabilidade.** Temas e plugins premium e personalizados são copiados para dentro do arquivo. Compartilhe um `.zip` do Playground só onde as licenças deles permitem — guarde os de licença paga para você ou sua equipe.

## Dentro

```text
blueprint.json
<site>.WordPress.<date>.xml        the plain WXR (content inside, not in post_content/)
uploads/…                          media files, when Media files is on
post_content/…                     content files, when Content as .html files is on
themes/<stylesheet>.zip            cada tema escolhido que não está no diretório (um filho traz o ascendente)
plugins/<slug>.zip                 cada plugin escolhido que não está no diretório
```

## O que o blueprint faz

1. Instala os temas (os ascendentes primeiro) — do diretório ou do pacote — e ativa o tema ativo, se escolhido, senão o primeiro.
2. Instala e ativa os plugins, do mesmo jeito.
3. Grava os arquivos de mídia e de conteúdo e dois plugins obrigatórios temporários: um responde com os arquivos quando o importador baixa a mídia e devolve cada `.html` ao seu post (`wp_import_post_data_raw`); o outro define as opções quando a importação termina, com os IDs remapeados.
4. Executa `importWxr` (o próprio Playground instala o Importador do WordPress).
5. Remove os plugins e arquivos temporários.

O filtro [`eizport_playground_blueprint`](../../developers/hooks/#eizport_playground_blueprint) altera o blueprint antes de ele ser gravado.
