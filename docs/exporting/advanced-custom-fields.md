---
title: "Advanced Custom Fields"
description: "O Advanced Custom Fields (ACF) numa linha própria: os tipos de post, taxonomias e grupos de campos dele no formato de exportação do próprio ACF, importados antes do conteúdo, e os campos que guardam IDs apontando para os itens do outro site."
translatedFrom: "05f6e7529e5513decc72ab35ef6f11f82270434d"
---

Com o **Advanced Custom Fields (ACF)** ativo (6.1 ou mais novo, o plugin gratuito), a tela de exportação ganha a linha **Advanced Custom Fields (ACF)**. O painel **Escolher itens** dela lista o que o ACF registra, cada um com uma prévia:

| Item | Prévia |
| --- | --- |
| **Tipo de post** (*ACF → Post Types*) | O nome dele (`receita`) |
| **Taxonomia** (*ACF → Taxonomies*) | O nome dela (`cozinha`) |
| **Grupo de campos** (*ACF → Field Groups*) | Os campos e os tipos deles: *Foto (image), Relacionados (relationship)…* |

Eles viajam no **formato de exportação do próprio ACF** — o JSON de *ACF → Tools → Export* — dentro do `options.json` do pacote, então precisam de um pacote (`.wxrz`, `.zip`, como plugin ou um `.zip` do Playground). Os posts do próprio ACF (`acf-field-group`, `acf-post-type`…) não aparecem como linhas de conteúdo: esta linha os leva.

## Sugeridos

Marque os posts (páginas, produtos, um tipo de post do ACF…) e a linha sugere o que eles usam: os grupos de campos dos valores deles, o tipo de post do ACF do tipo deles e as taxonomias do ACF dos termos deles. Os posts para onde os campos **objeto de post**, **relacionamento** e **link de página** apontam são sugeridos nas linhas deles, e as imagens e arquivos dos campos **imagem** e **arquivo** vão junto como mídia usada.

## No outro site

1. **Antes do conteúdo**, o ACF importa os tipos de post, as taxonomias e os grupos de campos, como *ACF → Tools → Import* faz: um com a mesma chave lá é **atualizado**, não duplicado, então uma segunda entrega atualiza a primeira. Os tipos de post e as taxonomias são registrados na hora, e os posts e termos deles entram.
2. O conteúdo entra, com os valores nos campos personalizados.
3. **Quando a importação termina**, os campos que guardam IDs recebem os IDs que os itens têm lá:

| Tipo de campo | Aponta para | Mapeado por |
| --- | --- | --- |
| Imagem, Arquivo | Anexos | Os anexos importados (ou os que o site já tinha) |
| Objeto de post, Relacionamento, Link de página | Posts, páginas, qualquer tipo | Os itens importados |
| Taxonomia | Termos | Os termos importados |
| Usuário | Usuários | Os autores, como o importador os mapeia |

Só IDs que vieram no arquivo e mudaram são tocados; um link de página para uma URL fica como está. As regras de localização dos grupos de campos numa página ou num post também recebem o ID novo. Os valores de termos (campos mostrados numa taxonomia) são mapeados como os de posts.

## O que ele não faz

- O **ACF PRO** (repeater, flexible content, clone, galeria, páginas de opções, blocos) ainda não é coberto.
- **Valores em usuários** não viajam: um WXR não tem usuários, só os autores dos posts. Um campo de usuário aponta para um usuário lá só quando ele é autor no arquivo.
- **Sem o ACF ativo** no outro site, os itens dele não são importados (os valores vêm assim mesmo, nos campos personalizados).
- Os grupos de campos no **JSON local** do tema (`acf-json/`) viajam com o tema.
