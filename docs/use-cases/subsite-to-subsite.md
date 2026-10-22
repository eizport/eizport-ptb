---
title: "De subsite para subsite"
description: "Numa rede, um site montado num subsite e levado para outro — o publicado, ou uma cópia nova — com as mídias, os links e as configurações apontando para o site certo."
translatedFrom: "3dc0222b2a05c2e50e848fdd964046c781cdadd0"
---

## O cenário

Uma rede tem um subsite para montar (`example.com/montagem`) e o publicado (`example.com/loja`). As páginas, os produtos e os menus novos estão prontos em *montagem*; *loja* precisa deles. Copiar tabelas do banco entre subsites é arriscado (IDs, prefixos, usuários compartilhados pela rede), e a exportação do próprio WordPress deixa links e imagens apontando para *montagem*.

## Os passos

1. Em *montagem*, **Ferramentas → Exportar**: marque o que vai (páginas, produtos, menus, a aparência…).
2. **Exportar**, e em *loja* **Ferramentas → Importar → WordPress** com o arquivo. Ou pelo WP-CLI:

   ```bash
   wp --url=https://example.com/montagem eizport export --types=page,product --menus=all --filename=entrega
   wp --url=https://example.com/loja eizport import entrega.wxrz --update-existing
   ```

## O que a importação faz

- As mídias vão para a pasta da própria *loja* (`uploads/sites/<id>/`).
- Links para `example.com/montagem/…` apontam para `example.com/loja/…`; o endereço da rede e os uploads da própria *loja* nunca são tocados.
- Os IDs em blocos, menus e configurações apontam para os itens importados na *loja*.
- Os padrões salvos e as configurações de *montagem* ficam em *montagem*.

## O que não faz

- Usuários: eles pertencem à rede; o importador atribui os posts a um usuário existente (ou cria um), como o WordPress faz.
- As configurações da rede (*Painel da rede → Configurações*) não são exportadas: pertencem à rede inteira.
