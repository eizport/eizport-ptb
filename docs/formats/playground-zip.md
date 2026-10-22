---
title: ".zip do Playground"
description: "Um pacote de blueprint do WordPress Playground que inicia um site com o conteúdo, a mídia, o tema e os plugins exportados — como é montado e como executá-lo."
translatedFrom: "a620d4eee848fb59dd392ad124393c0c5a9e04b5"
---

Com o formato **Playground**, a exportação é um `.zip` pronto para o [WordPress Playground](https://wordpress.org/playground/): um pacote de blueprint que instala os temas e plugins que você escolheu e importa o conteúdo.

![Temas e plugins do Playground: o que o .zip instala.](../../../assets/screenshots/playground-options.webp)

## Execute

```bash
npx @wp-playground/cli@latest server --blueprint=./site.zip
```

Ou abra o link dele no playground.wordpress.net (nova tela: a ação **Abrir no Playground** em [Cópias](../../settings/copies/); tela clássica: *Manter uma cópia para abrir por link*). A cópia é servida com os cabeçalhos CORS de que o Playground precisa.

**Mesmo de um servidor local.** O Playground roda no seu navegador, então uma cópia feita no WordPress Studio ou em qualquer site `localhost` abre no playground.wordpress.net sem ser enviada a lugar nenhum — o navegador a coleta da sua máquina (o link só funciona no seu computador).

## Opções do Playground

**Detalhes → Opções do Playground** (ativado com o formato Playground) abre três grupos, um de cada vez; o botão conta o que foi definido.

### Página inicial

Onde o Playground abre quando o site fica pronto — **Página inicial** (o padrão), **Painel**, **Uma página** deste site, ou **Um caminho** (`/loja/`). Uma página vai no blueprint como caminho relativo, `/?pagename=<o caminho dela>`, para abrir a mesma página seja qual for o ID que ela ganhe no Playground. (Sem isso, o próprio Playground abriria o painel.)

### Idioma

O idioma em que o Playground abre — **Idioma do site** (o padrão), **Seu idioma** (aparece quando o do seu perfil é outro) ou **English (United States)**. Num idioma que não é o inglês, o blueprint executa o passo `setSiteLanguage` do Playground depois das instalações: ele define o idioma do site e baixa as traduções do WordPress e dos temas e plugins do diretório do WordPress.org. As traduções que este site tem dos outros — premium, personalizados — vão dentro do `.zip`. Um idioma que este site não instalou não é oferecido.

### Temas e plugins

Qualquer tema ou plugin **instalado** pode entrar. Os plugins são instalados **ativos ou inativos como estão neste site**; dos temas, o deste site é ativado (quando escolhido), e um tema descendente traz o ascendente. Os do diretório do WordPress.org não são copiados: o blueprint os instala do diretório, **na versão que este site tem** (`woocommerce.11.2.1.zip`), quando o diretório tem essa versão — uma versão lançada, não mais nova que a do diretório. Os outros — premium, personalizados, internos, e uma versão de desenvolvimento, beta ou RC (`1.1.0-dev`) — são compactados a partir do site dentro do pacote, e o Playground os instala dali. A própria verificação de atualizações do WordPress diz quais vêm do diretório.

Os **plugins obrigatórios** (`wp-content/mu-plugins/`) também aparecem, quando o site tem algum: cada um escolhido vai dentro do `.zip`, com a pasta que ele carrega (`require __DIR__ . '/<pasta>/…'`, ou uma pasta `<nome>/` ao lado de `<nome>.php`). Os que a plataforma põe lá — o carregador do SQLite do WordPress Studio, os das hospedagens — e os temporários do próprio Eizport não são oferecidos; o filtro [`eizport_playground_mu_plugins`](../../developers/hooks/#eizport_playground_mu_plugins) altera a lista.

> **Use com responsabilidade.** Temas e plugins premium e personalizados são copiados para dentro do arquivo. Compartilhe um `.zip` do Playground só onde as licenças deles permitem — guarde os de licença paga para você ou sua equipe.

## Dentro

```text
blueprint.json
<site>.WordPress.<date>.xml        the plain WXR (content inside, not in post_content/)
uploads/…                          media files, when Media files is on
post_content/…                     content files, when Content as .html files is on
themes/<stylesheet>.zip            cada tema escolhido que não está no diretório (um filho traz o ascendente)
plugins/<slug>.zip                 cada plugin escolhido que não está no diretório
languages/plugins|themes/…         as traduções deles, num idioma que não é o inglês
mu-plugins/<arquivo>.php           cada plugin obrigatório escolhido
mu-plugins/<pasta>.zip             cada pasta que eles carregam
```

## O que o blueprint faz

1. Instala os temas (os ascendentes primeiro) — do diretório ou do pacote — e ativa o tema ativo, se escolhido, senão o primeiro.
2. Instala e ativa os plugins, do mesmo jeito.
3. Em outro idioma: define o idioma (`setSiteLanguage`, que baixa as traduções do diretório) e grava as que vêm no pacote.
4. Grava os plugins obrigatórios e descompacta as pastas deles em `wp-content/mu-plugins/`, antes da importação: os tipos de post e as taxonomias que eles registram já existem para ela.
5. Grava os arquivos de mídia e de conteúdo e dois plugins obrigatórios temporários: um responde com os arquivos quando o importador baixa a mídia e devolve cada `.html` ao seu post (`wp_import_post_data_raw`); o outro define as opções quando a importação termina, com os IDs remapeados.
6. Executa `importWxr` (o próprio Playground instala o Importador do WordPress).
7. Remove os plugins e arquivos temporários.

O filtro [`eizport_playground_blueprint`](../../developers/hooks/#eizport_playground_blueprint) altera o blueprint antes de ele ser gravado.
