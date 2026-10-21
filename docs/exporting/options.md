---
title: "Opções"
description: "Exporte as configurações do site uma a uma, por grupo ou todas de uma vez; configurações vinculadas; o que nunca é exportado; como os IDs são remapeados."
translatedFrom: "78585a2106b4117538ba50277fab0f9e9cbf62d2"
---

A linha **Opções** exporta as configurações do site num `options.json` dentro do pacote, definidas no outro site quando a importação termina.

![O painel Opções em Selecionados: as configurações que a página inicial traz.](../../../assets/screenshots/options-panel.webp)

## Três formas de escolher

- **Uma a uma** — todas as opções que o site tem (pesquisa, páginas), cada uma com o tamanho e um selo *IDs remapeados na importação* quando guarda um ID de post.
- **Por grupo** — filtre o painel por um grupo e exporte-o: **Básicas** (sugeridas), Leitura, Geral, Links permanentes, Discussão, Mídia, Privacidade, Logo e ícone do site, Páginas do WooCommerce. As configurações do Personalizador do tema, o CSS adicional, os estilos do editor do site e os widgets têm [linhas próprias](../appearance/). Um grupo é resolvido quando o arquivo é gerado, então acompanha o tema ativo.
- **Todas** — todas as opções que podem ser exportadas.

Na tela nova, marcar uma opção no painel já a inclui (o filtro *Seleção* mostra as escolhidas); grupos e *todas* vêm de padrões salvos ou da tela clássica. Sem nada escolhido ainda, o painel abre em **Básicas**: título, descrição, idioma, fuso horário, formatos de data, página inicial e página de posts, posts por página, links permanentes, logo e ícone do site.

<a id="linked-settings"></a>
## Configurações vinculadas

Algumas opções apontam para um post: `page_on_front`, `page_for_posts`, `wp_page_for_privacy_policy`, `site_icon`, os IDs de página do WooCommerce. Quando o post é exportado, essas opções podem vir junto — com as que elas precisam, como `show_on_front`.

Na tela nova, elas são **sugeridas**: escolha a página inicial e a linha Opções desmarcada mostra **2 sugeridos** (`page_on_front` e `show_on_front`, contados, sem nomear). Marque a linha e eles vão na exportação; a linha continua dizendo **2 sugeridos** (mais os grupos ou *N itens* que você escolheu). Desmarque um deles no painel e o resto vira itens comuns — **1 item**. Com a linha desmarcada, eles não vão.

A configuração decide quando elas são sugeridas ou adicionadas:

| Modo (Configurações → Exportar) | Quando |
| --- | --- |
| **Quando itens são marcados, e na exportação** (padrão) | Sugeridas na linha Opções quando você marca um item com uma função; na tela clássica e na habilidade, também adicionadas quando o arquivo é gerado — para itens exportados por filtros ou com todo o conteúdo |
| **Quando itens são marcados** | Sugeridas pela seleção do painel lateral |
| **Na exportação** | Só quando o arquivo é gerado (tela clássica, habilidade) |
| **Nunca** | Você escolhe cada opção |

## Nunca exportadas

URLs do site (`siteurl`, `home`), o e-mail do administrador, chaves e salts, `active_plugins`, funções (`<prefix>user_roles`), quem pode se registrar e com qual função (`users_can_register`, `default_role`), `cron`, `rewrite_rules`, versões do banco de dados, credenciais do servidor de e-mail, as próprias configurações do plugin e todos os transients. Plugins acrescentam as suas com [`eizport_denied_options`](../../developers/hooks/#eizport_denied_options); as do próprio WordPress não podem ser removidas.

Uma importação também nunca grava as do próprio WordPress, seja o que for que o pacote traga: um pacote é um arquivo que qualquer um pode ter escrito.

## IDs na importação

O `options.json` lista, em `post_ids`, as opções que guardam um ID de post. Na importação, cada uma recebe o ID que o importador deu a esse post; se o post não foi importado, a opção fica de fora em vez de apontar para a coisa errada. Os valores de `theme_mods_<theme>` também são remapeados (logo, menus).
