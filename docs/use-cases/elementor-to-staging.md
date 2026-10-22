---
title: "Elementor para o staging"
description: "Uma página de destino feita com o Elementor — as imagens dela, um modelo de seção salvo e as cores e fontes globais do site — levada ao staging com o importador do próprio Elementor, tudo apontando para os itens do staging."
translatedFrom: "f2fe7ce537c3f04919216ec7fdb87b2d0a5deec7"
---

## O cenário

Uma página de destino foi feita com o Elementor no site local: uma imagem de fundo, uma galeria, uma **seção promocional salva como modelo** e mostrada por um widget de modelo, um botão para a página do chef, e a cor e a fonte da marca em *Elementor → Site Settings*. O staging já tem o seu próprio conteúdo, então todo ID lá é diferente, e o seu próprio Kit.

## Os passos

1. No site local, **Ferramentas → Exportar**: em **Páginas**, **Escolher itens** → a página de destino.
2. A linha **Elementor** mostra *2 sugeridos*: as configurações do site e a seção promocional. Marque-a.
3. **Exportar**: o design da página vai como `post_content/<slug>.json`, o modelo e as configurações do site no pacote, com as imagens.
4. No staging, **Ferramentas → Importar → WordPress**: envie o pacote.

## O que o staging recebe

- A página, com o design trazido pela **importação do próprio Elementor**: o fundo e a galeria mostram as imagens que o pacote trouxe (sem cópias delas), o widget de modelo mostra a seção promocional importada, e o botão vai para a página do chef do staging.
- A **seção promocional** em *Templates → Saved Templates*.
- Um **Kit** com a cor e a fonte da marca somadas às do staging, ativo — o título aparece na cor da marca.
- Uma segunda entrega atualiza o modelo e esse Kit em vez de adicionar cópias.

## O que ele não faz

- O Theme Builder do Elementor Pro (cabeçalhos, rodapés e as condições deles) e os popups ainda não são cobertos.
