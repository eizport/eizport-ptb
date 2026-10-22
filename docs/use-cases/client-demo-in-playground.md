---
title: "Demonstração para o cliente no Playground"
description: "Uma página nova mostrada a um cliente no WordPress Playground: um link que abre direto nessa página, com os plugins do site como estão."
translatedFrom: "544ab4b1e3d0caaf18b283d8899c18cee030893b"
---

## O cenário

Uma landing page nova está pronta localmente, e o cliente quer vê-la antes de ela ir para o staging. Mandar o link de um site local não funciona, e um Playground que abre no painel, com todos os plugins ligados (inclusive os que o site deixa desligados), não é o que o cliente deve ver.

## Os passos

1. No site local, **Ferramentas → Exportar**: marque a página (e o que ela precisa: menus, a aparência…).
2. **Detalhes → Formato: Playground**, e depois **Opções do Playground**:
   - **Página inicial → Uma página**: a landing page nova;
   - **Temas e plugins**: o tema do site e os plugins que a página usa.
3. **Exportar**, e depois **Abrir no Playground** na cópia, em [Cópias](../../settings/copies/) do plugin: esse é o link para mandar, no playground.wordpress.net.

## O que o cliente vê

O Playground monta o site no navegador e abre **na landing page** — a página vai no blueprint como `/?pagename=<o caminho dela>`, então é encontrada seja qual for o ID que ganhe lá. Os plugins ficam ligados ou desligados como estão no site local: um plugin que o site deixa inativo é instalado, mas continua desligado.

## O que não faz

- O link abre um site descartável: o que o cliente muda ali se perde quando a aba fecha.
- Plugins premium vão dentro do arquivo: compartilhe-o só onde as licenças deles permitirem.
