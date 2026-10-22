---
title: "Segunda entrega para o staging"
description: "Entregar páginas novas a um staging que já tem as mídias da primeira entrega — sem duplicar as imagens e sem links para o site local."
translatedFrom: "5cb859aa8161f5fe2e99bb7d4665f35b6f9e75f0"
---

## O cenário

A primeira entrega levou as páginas e as imagens delas para o staging. Uma semana depois há duas páginas novas que reaproveitam algumas dessas imagens, mais uma imagem nova. A segunda exportação leva tudo; o staging já tem a maioria das imagens.

## Os passos

1. No site local, **Ferramentas → Exportar**: marque as duas páginas novas (as imagens delas vão junto).
2. **Exportar**, e no staging **Ferramentas → Importar → WordPress** com o arquivo — com *Atualizar existentes* ligado ou desligado.

## O que a importação faz

- As imagens que o staging já tem (mesmo título e data do arquivo) são **reaproveitadas**, não importadas de novo.
- Os endereços delas nas páginas novas — o tamanho completo e cada tamanho de imagem (`foto-300x200.jpg`) — apontam para os arquivos do staging, não para o site local. Antes, isso só acontecia com *Atualizar existentes* ligado.
- A imagem nova é importada, e os blocos apontam para ela ([galerias para o staging](../galleries-to-staging/)).

## O que não faz

- Uma imagem cujo arquivo mudou no site local mantém o arquivo antigo do staging: ligue *Atualizar existentes* para atualizar as páginas, e substitua a imagem na biblioteca de mídia.
