---
title: "Elementor"
description: "O Elementor com o exportador e o importador dele: as configurações do site (o Kit) e os modelos numa linha própria, o design de cada página num arquivo .json, e na importação cada imagem, modelo e link apontando para os itens do outro site."
translatedFrom: "eb84d2800a1d07522114a9f68fb8428d95ae84c5"
---

Com o **Elementor** ativo, o Eizport leva o que o *Import / Export Kit* do próprio Elementor leva — nos formatos do Elementor, entregues ao importador do próprio Elementor no outro site.

## A linha Elementor

O painel **Escolher itens** da linha **Elementor** lista:

| Item | O que é | Prévia |
| --- | --- | --- |
| **Configurações do site** | O Kit ativo: cores e fontes globais, layout, estilo do tema (*Elementor → Site Settings*), sem o nome, a descrição, o logo e o ícone do site — como a exportação de Kit do Elementor os deixa de fora | As cores globais |
| **Cada modelo** (*Templates → Saved Templates*) | Uma seção, uma página, um contêiner… no formato de arquivo de modelo do Elementor, o que *Export template* grava | O tipo dele |

Eles viajam no `options.json` do pacote, então precisam de um pacote (`.wxrz`, `.zip`, como plugin ou um `.zip` do Playground). A biblioteca do Elementor (`elementor_library`) não aparece como linha de conteúdo: esta linha a leva.

## Páginas feitas com o Elementor

Marque páginas, posts — de qualquer tipo — feitos com o Elementor nas linhas deles. Com *Conteúdo como arquivos .html* ativado ([Configurações → Exportar](../../settings/export-settings/)), o design de cada um vai como **`post_content/<slug>.json`**, no formato de arquivo de modelo do Elementor, em vez do `_elementor_data` dentro do WXR; os arquivos `.html` continuam para o conteúdo em blocos. Desativado, o design fica no WXR e é tratado do mesmo jeito na importação.

As linhas sugerem o resto: **Elementor** mostra as configurações do site e os modelos que os widgets de modelo das páginas mostram como *sugeridos*, e as imagens que os designs usam — widgets de imagem e galeria, fundos — vão junto como mídia usada.

## No outro site

Quando a importação termina, o design passa pela **importação do próprio Elementor**:

1. **Os modelos** são criados como a importação de Kit do Elementor os cria; um modelo já importado do mesmo site é atualizado, então uma segunda entrega não adiciona cópias.
2. **As configurações do site** passam pela importação de *configurações do site* do próprio Elementor: um Kit novo e ativo, com as cores e fontes globais somadas às do site. Uma entrega seguinte do mesmo site atualiza esse Kit, com as cores e fontes trocadas pelo ID em vez de somadas de novo.
3. **O design de cada página** passa pela importação do documento dela. As imagens vêm dos anexos que o pacote trouxe — o Elementor os acha pela URL de origem em vez de baixá-los de novo —, os widgets de modelo apontam para os modelos importados, e os links para o site de origem passam a apontar para este.
4. O **CSS gerado** pelo Elementor é limpo, para ser gerado de novo.

Sem o Elementor ativo no outro site, os designs são gravados nos metas das páginas, com as imagens e os modelos mapeados, prontos para quando o Elementor for ativado.

## O que ele não faz

- O **Elementor Pro** (modelos e condições do Theme Builder, popups, formulários) ainda não é coberto.
- **Tags dinâmicas** que apontam para um post ou termo pelo ID mantêm esse ID.
- Um link para uma página que não está no arquivo aponta para o mesmo endereço neste site.
