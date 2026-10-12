---
title: "Tela clássica"
description: "A tela Ferramentas → Exportar do próprio WordPress com as opções do plugin adicionadas, para quem prefere; também no celular."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

**Configurações → Geral → Usar a tela Exportar clássica** faz Ferramentas → Exportar abrir a tela do próprio WordPress, com as opções do plugin adicionadas. Ela sempre está em `wp-admin/export.php?classic=1`.

Ela só existe enquanto **Substituir Ferramentas → Exportar** está ativado (o padrão). Desativado, a tela Exportar do WordPress não recebe nada do plugin, e a configuração fica oculta: a tela do plugin fica em **Ferramentas → Eizport**.

## O que ela adiciona

- **Vários tipos de conteúdo:** a escolha única do WordPress vira caixas de seleção (“Todo o conteúdo” continua).
- **Por filtros ou por seleção** em cada tipo: os filtros do WordPress para posts, páginas e mídia; o mesmo tipo de filtro para produtos e tipos de post personalizados (datas, status, autor, cada taxonomia); ou uma lista de seleção (uma árvore para páginas) com um campo para filtrá-la.
- **Pacote (.wxrz ou .zip):** nome do arquivo, arquivos de mídia, configurações (grupos, página inicial e página de posts, outras opções pelo nome), um blueprint do Playground com temas e plugins e uma cópia temporária para abrir por link em playground.wordpress.net.
- **Salvar estas escolhas como meu padrão**, e uma tela que já inicia com elas.

## No celular

Cada caixa de seleção e cada botão de opção é uma linha de pelo menos 40px de altura para tocar, com a caixa desenhada como o WordPress desenha; nada rola para os lados.

![A tela clássica no celular: opções do pacote com linhas de 40px.](../../../assets/screenshots/classic-mobile.webp)

## Sem JavaScript

As caixas de seleção de tipo continuam sendo os botões de opção do WordPress e um tipo é exportado, ainda por filtros ou por seleção.
