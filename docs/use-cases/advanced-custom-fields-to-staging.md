---
title: "Advanced Custom Fields para o staging"
description: "Receitas feitas com o Advanced Custom Fields (ACF) — o tipo de post, a taxonomia, o grupo de campos e os valores delas — levadas ao staging com cada foto, página e termo apontando certo."
translatedFrom: "92db1aadf4e5adfc8c30ef91dae53ee7df5db753"
---

## O cenário

Um site de culinária guarda as **Receitas** com o Advanced Custom Fields (ACF): um tipo de post *Receitas* e uma taxonomia *Cozinhas* feitos em *ACF → Post Types* e *Taxonomies*, e um grupo de campos *Detalhes da receita* com uma foto, um cartão para imprimir, a página do chef, páginas relacionadas, uma cozinha e quem cozinha. Uma receita nova está pronta no site local; o staging tem o seu próprio conteúdo, então todo ID lá é diferente.

## Os passos

1. No site local, **Ferramentas → Exportar**: marque **Receitas** (ou abra **Escolher itens** para a nova).
2. As linhas sugerem o resto: **Páginas** mostra *2 sugeridos* (a página do chef e a relacionada) e **Advanced Custom Fields (ACF)** mostra *3 sugeridos* (o tipo de post, a taxonomia e o grupo de campos). Marque as duas linhas.
3. **Exportar** (um pacote `.wxrz` ou `.zip`), e importe no staging em **Ferramentas → Importar → WordPress**.

## O que o staging recebe

- O ACF importa o tipo de post *Receitas*, a taxonomia *Cozinhas* e o grupo *Detalhes da receita* **antes** do conteúdo: a receita entra como receita, com a cozinha dela.
- A foto e o cartão vêm no pacote, mesmo sem estar anexados; no staging eles ficam no lugar — ou apontam para os mesmos arquivos que o staging já tinha.
- A página do chef, as páginas relacionadas, a cozinha e quem cozinha apontam para os itens **de lá**, com os IDs do staging; a regra de localização do grupo numa página também.
- Uma segunda entrega atualiza o grupo, o tipo de post e a taxonomia (mesmas chaves) em vez de adicionar cópias.

## O que ele não faz

- Os campos do ACF PRO (repeater, flexible content, galeria) ainda não são cobertos.
- Quem cozinha e não é autor no arquivo fica com o ID de usuário antigo; os autores são mapeados como o importador os mapeia.
