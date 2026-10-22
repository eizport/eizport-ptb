---
title: "Tela Exportar"
description: "A nova tela Ferramentas → Exportar, parte por parte: as ações do cabeçalho, o painel Detalhes, a lista e os painéis laterais."
translatedFrom: "ed7bad965f80e82c674848ca566269b29ec912dd"
---

Ferramentas → Exportar abre uma tela construída como as telas mais recentes do painel do WordPress (DataViews e DataForm, sobre a estrutura de telas do painel dos arquivos básicos do WordPress). Ela usa as [habilidades](../../developers/abilities/) do plugin, então a tela, a REST e os agentes de IA exportam do mesmo jeito.

![A tela Exportar com Páginas (1 item), e Produtos e Opções trazendo o que a página inicial sugere.](../../../assets/screenshots/export-screen.webp)

## A lista

Uma linha para cada coisa que pode ser exportada:

- **Tipos de conteúdo** — Posts, Páginas, Produtos, Pedidos, Cupons, Formulários e qualquer tipo de post personalizado com tela no painel. Os descendentes vão junto com os ascendentes (as variações de um produto).
- **Anexos** — itens de mídia, escolhidos diretamente.
- **Avaliações de produtos** — as avaliações aprovadas do WooCommerce, logo depois de Produtos ([escolher itens](../choosing-items/#sugestões)). Os produtos vão sem as avaliações, a menos que esta linha esteja marcada.
- **Opções** — configurações do site ([mais](../options/)).

A coluna **Exportar** diz o que vai: *Não incluído*, *Tudo*, *3 itens* ou, em Opções, *Básicas + 2 itens*; uma linha Opções desmarcada mostra *2 sugeridos* (a página inicial: `page_on_front` e `show_on_front`) quando os itens escolhidos têm [configurações vinculadas](../options/#linked-settings). **Nada marcado exporta todo o conteúdo**, como faz o WordPress.

As ações de cada linha: **Escolher itens** (abre o painel lateral; sempre visível, não só ao passar o mouse; **Fechar itens** na linha cujo painel está aberto), **Exportar tudo** (onde ainda não exporta tudo; em Opções, todas as opções) e **Não exportar** (linhas incluídas). Marque várias linhas para agir sobre todas de uma vez.

## O painel lateral

Clicar em uma linha abre os itens dela em um painel à direita ([escolher itens](../choosing-items/)): o que você marca ali entra na exportação na hora, e **Fechar** volta. O URL do painel pode ser compartilhado: `…&p=/export?type=page`.

## O painel Detalhes

Acima da lista, uma linha **Detalhes** resume o arquivo — como *WXRZ · nome de arquivo padrão · arquivos de mídia · como plugin* (*nome de arquivo personalizado* quando você digita um). Clique nela para abrir as configurações (ele sempre começa fechado):

| Campo | O que faz |
| --- | --- |
| **Formato** | *WXR XML* (o arquivo simples do WordPress), *WXRZ* ou *WXR ZIP* (um pacote, assim que arquivos de mídia ou configurações vão junto) ou *Playground* ([formatos](../file-and-formats/)) |
| **Nome do arquivo** | Vazio: o nome de sempre do WordPress, `<site>.WordPress.<date>`, mostrado como texto de marcação |
| **Arquivos de mídia** | Coloca os arquivos dos anexos exportados dentro do pacote; o ⓘ explica |
| **Como plugin** | Transforma o pacote num plugin que se importa sozinho ([como plugin](../as-plugin/)); o ⓘ explica. Desligado e desativado com o Playground |
| **Atualizar existentes** | A importação atualiza as páginas, posts e produtos que já existem (mesmo slug) em vez de criar cópias ([mais](../../getting-started/importing/#updating-items-that-already-exist)); o ⓘ explica. Desligado e desativado com o Playground, que começa de um site novo |
| **Opções do Playground** | Onde o .zip do Playground abre e os temas e plugins que ele instala (ativado com o formato Playground); um contador no botão diz quantos foram definidos ([.zip do Playground](../../formats/playground-zip/#opções-do-playground)) |

## Ações do cabeçalho

- **Redefinir para os padrões** — volta para os padrões salvos (ou para nada escolhido, sem eles). Fica esmaecido quando as escolhas já são essas.
- **Limpar padrões** — esquece os padrões salvos; só fica ativo enquanto **Redefinir para os padrões** está esmaecido.
- **Salvar como padrão** — guarda as escolhas atuais para iniciar com elas da próxima vez ([padrões salvos](../saved-defaults/)).
- **Exportar** — grava o arquivo como uma [cópia](../../settings/copies/) e oferece baixá-lo.

## No celular

A lista mostra só a coluna de título, com o que é exportado embaixo; o painel Detalhes fica empilhado; o painel lateral ocupa a tela.
