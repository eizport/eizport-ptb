---
title: "Multisite"
description: "O Eizport numa rede WordPress: ativação na rede ou por site, quem pode o quê, onde ficam os arquivos de cada site, WP-CLI com --url e desinstalação."
translatedFrom: "3dc0222b2a05c2e50e848fdd964046c781cdadd0"
---

O Eizport funciona em cada site de uma rede como num site simples: um site exporta o próprio conteúdo e importa nele mesmo, seja o que for que os outros sites tenham.

## Ativação

Use **Ativar na rede** em *Painel da rede → Plugins* para tê-lo em todos os sites, ou ative-o nos sites que precisam. Nada é compartilhado entre os sites: cada um mantém as próprias configurações, cópias e padrões salvos.

## Quem pode o quê

| Quem | Pode |
| --- | --- |
| Administradores de site | Exportar e importar no site deles, e mudar as configurações do Eizport nele |
| Super administradores | Tudo isso em todos os sites, e instalar plugins — um `.zip` [como plugin](../../exporting/as-plugin/) precisa de um para ser instalado; administradores de site importam o pacote que vai dentro dele em Ferramentas → Importar |
| Usuários que não podem publicar HTML sem filtro | Importam como o WordPress deixa: numa rede só os super administradores podem, então scripts e alguns embeds do conteúdo importado são filtrados para os outros, como seriam se digitados no editor |

## Os arquivos de cada site

As mídias, as cópias (`uploads/eizport/`) e os arquivos importados de um site ficam na pasta de uploads dele (`wp-content/uploads/sites/<id>/`). Uma exportação de um site e uma importação em outro — de subsite para subsite, de um subsite para um site simples, ou o contrário — funcionam como entre dois sites separados:

- as mídias vêm do pacote para a pasta do site que importa;
- os links para o site que exportou (o endereço dele, como `https://example.com/loja`) apontam para o que importa;
- os IDs em blocos, menus e configurações apontam para os itens importados.

## Padrões salvos

*Salvar como padrão* guarda as escolhas de cada usuário **por site**: o que alguém salva num site não aparece em outro. Com *Os padrões são salvos para: O site inteiro* nas [configurações gerais](../../settings/general/), um site tem os próprios padrões para todos nele.

## WP-CLI

Escolha o site com `--url`:

```bash
wp --url=https://example.com/loja eizport export --types=product --filename=loja
wp --url=https://example.com/staging eizport import loja.wxrz
```

## Desinstalação

Excluir o plugin em *Painel da rede → Plugins* remove os dados dele de todos os sites da rede: a pasta `uploads/eizport/` de cada site, as configurações, os padrões salvos e a limpeza agendada.
