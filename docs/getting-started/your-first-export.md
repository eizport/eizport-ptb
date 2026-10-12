---
title: "Sua primeira exportação"
description: "Exporte uma página com sua mídia e a configuração de página inicial em menos de um minuto."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

Este passo a passo exporta uma página — digamos, a página inicial — com suas imagens e as configurações que fazem dela a página inicial, e depois a importa em outro site.

## 1. Abra Ferramentas → Exportar

A tela Exportar lista o que pode ser exportado: cada tipo de conteúdo (Posts, Páginas, Produtos…), **Anexos** e **Opções**. Nada está marcado ainda, o que significa *tudo*.

## 2. Escolha a página

Clique em **Páginas** (ou em *Escolher itens* na linha). Um painel lateral lista as páginas, cada uma com uma caixa de seleção. Marque a página inicial — ela mostra o selo **Página inicial**: a linha Páginas diz **1 item** na hora. Clique em **Fechar**.

![O painel de itens: páginas com suas funções como selos (Página inicial, Página da loja…), a página inicial marcada.](../../../assets/screenshots/items-panel.webp)

A linha **Opções** agora diz **2 sugeridos**: `page_on_front` e `show_on_front` apontam para a página que você escolheu ([configurações vinculadas](../../exporting/options/#linked-settings)). Marque a linha e eles vão junto — ela continua dizendo *2 sugeridos*. Se a página inicial mostra produtos com um bloco do WooCommerce, a linha **Produtos** os sugere do mesmo jeito.

## 3. Confira o arquivo

A linha **Detalhes** acima da lista resume o arquivo — *WXRZ · nome de arquivo padrão · arquivos de mídia*. Clique nela para abrir as configurações:

- **Formato** mostra **WXRZ**: um pacote, porque arquivos de mídia e configurações vão junto.
- **Arquivos de mídia** está ativado: as imagens anexadas à página, a imagem destacada e as imagens do conteúdo vão dentro.
- **Nome do arquivo** está vazio: o arquivo recebe o nome de sempre do WordPress, mostrado como texto de marcação.

## 4. Exporte

Clique em **Exportar**. O arquivo é gravado como uma [cópia](../../settings/copies/) no site e um aviso oferece **Baixar**.

## 5. Importe

No outro site, vá em **Ferramentas → Importar** — ela abre o Importador do WordPress —, envie o `.wxrz` e execute o importador como sempre. Ou exporte com **Como plugin** ligado, instale esse `.zip` como plugin e clique em **Executar importação** na tela dele ([como plugin](../../exporting/as-plugin/)). A página, as imagens e a configuração de página inicial chegam juntas — com o ID da nova página em `page_on_front`.

> O site antigo não precisa estar on-line: as imagens vêm do pacote.
