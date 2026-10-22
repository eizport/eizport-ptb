---
title: "Galerias para o staging"
description: "Páginas feitas com blocos Imagem, Galeria e Capa, levadas para um site onde as mídias ganham outros IDs — e ainda editáveis como estavam."
translatedFrom: "c0d140d85ccd8055e30700149fdfbe97106df12d"
---

## O cenário

As páginas do site local usam blocos Imagem, Galeria, Capa e Mídia e texto. O staging já tem conteúdo, então as imagens importadas ganham outros IDs lá. Só com o Importador do WordPress as imagens continuam aparecendo (os endereços são atualizados), mas os blocos mantêm os IDs antigos: o editor mostra que a imagem tem uma origem vazia ou inválida, *Substituir* e as configurações da imagem agem no item errado, e uma galeria não encontra as imagens dela.

## Os passos

1. No site local, **Ferramentas → Exportar**: marque as páginas. As imagens delas vão junto (*mídia usada no conteúdo*).
2. **Exportar**, e no staging **Ferramentas → Importar → WordPress** com o arquivo.

## O que a importação faz

Quando a importação termina, toda página importada — nova ou atualizada — tem os blocos apontados para as mídias importadas: o `id` e a classe `wp-image-N` dos blocos Imagem e Capa, os `ids` da Galeria e o `data-id` das imagens dela, o `mediaId` de Mídia e texto, Arquivo, Vídeo e Áudio, `[gallery ids="…"]`, e os links de navegação para páginas e posts. O mesmo vale para os widgets de bloco.

Só os IDs que vieram no arquivo e mudaram são tocados: um bloco que aponta para algo que o staging já tinha, e que não estava no arquivo, fica como está.

## O que não faz

- Os blocos próprios de um plugin, até ele os listar (o filtro [`eizport_block_id_attributes`](../../developers/hooks/#eizport_block_id_attributes)).
