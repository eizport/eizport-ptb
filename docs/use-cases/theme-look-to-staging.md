---
title: "Aparência do tema para o staging"
description: "Um tema personalizado localmente — logo, fundo, CSS adicional, widgets, ou os estilos de um tema de blocos — repetido no staging sem refazer no Personalizador ou no editor do site."
translatedFrom: "78585a2106b4117538ba50277fab0f9e9cbf62d2"
---

## O cenário

Um tema clássico foi personalizado localmente: o logo, uma imagem e uma cor de fundo no Personalizador, algumas linhas de CSS adicional, e a barra lateral do rodapé com uma imagem, um menu e um bloco de texto. O staging tem o mesmo tema, recém-instalado. Refazer tudo à mão — e enviar as imagens de novo — é a parte em que sempre se esquece alguma coisa.

O mesmo vale para um tema de blocos personalizado no editor do site: as cores e a tipografia (*Estilos*) e o CSS adicional dele.

## Os passos

1. No site local, **Ferramentas → Exportar**. As linhas **Theme mods do Personalizador**, **CSS adicional do Personalizador** e **Widgets** dizem *Sugerido* (com um tema de blocos: **Estilos globais do editor** e **CSS global do editor**). Marque-as.
2. Marque também **Menus** se um widget mostra um menu, e as páginas de que o site precisa.
3. **Exportar**, e no staging **Ferramentas → Importar → WordPress** com o arquivo (com o Eizport ativo lá, que aplica a aparência quando a importação termina).

## O que o arquivo leva

- Os theme mods, o CSS adicional e os widgets com as barras laterais — ou os estilos e o CSS do tema de blocos.
- As imagens que eles usam: o logo, o fundo, a imagem do widget de imagem.

No staging, o logo e o fundo são as imagens importadas, os widgets voltam para o rodapé, o widget de menu mostra o menu importado, e o CSS é o CSS adicional do tema. As configurações do próprio staging que o arquivo não traz ficam como estavam.

## O que não leva

- Fontes instaladas pela Biblioteca de fontes.
- Uma barra lateral que o tema do staging não tem: os widgets dela vão para *Widgets inativos*.

## Só o CSS

Para levar só o CSS adicional de um tema de blocos, marque apenas **CSS global do editor**: o staging mantém as próprias cores e tipografia.
