---
title: "Como plugin"
description: "Exporte um plugin que importa o próprio pacote: instale-o no outro site, confira o que ele precisa, Executar importação, depois exclua-o."
translatedFrom: "80ba23eb5a0d7c185044ae06f6ebb563d42d62b4"
---

**Como plugin**, ao lado de *Arquivos de mídia* no painel **Detalhes** da tela Exportar, transforma o arquivo num `.zip` de plugin com o pacote dentro. Instale-o no outro site como qualquer plugin, e ele se importa sozinho — sem arquivo para enviar, sem passar pela tela do importador.

![A tela do plugin no outro site depois de Executar importação: tudo o que ele precisa conferido, o resultado e "Desativar e excluir este plugin".](../../../assets/screenshots/bundle-plugin.webp)

## O que vai dentro

```
meusite-wordpress-2026-10-12.zip
└── meusite-wordpress-2026-10-12-k3x9q2m7a1b8c4d6/
    ├── meusite-wordpress-2026-10-12-k3x9q2m7a1b8c4d6.php   o plugin: "Importação: meusite-wordpress-2026-10-12"
    ├── readme.txt                                           o que ele é, como pede o Plugin Check do WordPress.org
    └── meusite-wordpress-2026-10-12-k3x9q2m7a1b8c4d6.zip   o pacote (conteúdo, mídia, configurações, como escolhidos)
```

A pasta e o arquivo do plugin levam o nome do arquivo da exportação, mais a parte aleatória do nome da cópia em minúsculas (a mesma que [Cópias](../../settings/copies/) mostra). Assim nenhum nome de arquivo o transforma em outro plugin: uma exportação chamada `woocommerce` continua `woocommerce-<aleatório>/`, nunca a pasta do WooCommerce. Cada entrega se instala como um plugin próprio.

**Configurações → Exportar → Como plugin traz o pacote como** pode pôr os arquivos do pacote no plugin em vez do arquivo do pacote — as mesmas entradas, com o WXR formatado linha a linha, para ver ou mudar antes de instalar. **Ao lado dos do plugin**, a pasta do plugin é o pacote:

```
meusite-wordpress-2026-10-12.zip
└── meusite-wordpress-2026-10-12-k3x9q2m7a1b8c4d6/
    ├── meusite-wordpress-2026-10-12-k3x9q2m7a1b8c4d6.php
    ├── manifest.json
    ├── meusite.WordPress.2026-10-12.xml
    ├── post_content/home.html
    ├── options.json
    └── uploads/2026/10/foto.jpg
```

**Numa pasta bundle/**, os mesmos arquivos vão para `bundle/`, separados dos do plugin. O cabeçalho do plugin (`Eizport Bundle:`) aponta para `manifest.json` ou `bundle/manifest.json`.

O padrão continua um arquivo — um `.zip`, ou um `.wxrz` com *Pacote como .zip* desativado: um download menor, e os arquivos do pacote não se misturam aos do plugin. Nos dois casos, os arquivos podem ser acessados por URL enquanto o plugin está instalado — exclua-o depois da importação. Não funciona com o formato Playground: lá o toggle fica desligado e desativado.

## No outro site

1. **Plugins → Adicionar plugin → Enviar plugin**, escolha o `.zip`, **Instalar agora**, **Ativar**.
2. **Executar importação**, na linha do plugin na tela Plugins — ou a entrada dele em **Ferramentas → Importar**, destacada dos outros importadores, e um aviso até a importação — abre a tela dele. Ela não fica no menu (uma importação feita uma vez não tem lugar ali); nela, *Ferramentas → Importar* aparece como item atual. Enquanto ele espera, Ferramentas → Importar mostra a lista de importadores em vez de ir direto ao Importador do WordPress. Nada redireciona você ao ativar.
3. A tela lista o que ele precisa, cada item com um ✓ ou um link:
   - **Eizport**, ativo — ele desenha a tela e faz a importação. Sem ele, ou numa versão mais antiga que a que o pacote precisa, a mesma tela mostra ✗ e **Instalar agora**, **Ativar** ou **Atualizar agora**, feitos ali mesmo; depois a página recarrega com o que o Eizport lê. Até lá, **O que ele contém** fica esmaecido e fechado, e *Atualizar existentes* e **Executar importação** ficam desativados;
   - **o Importador do WordPress**, instalado — com um botão *Instalar agora* que o instala e ativa ali mesmo, como Ferramentas → Importar instala um importador (*Instalando…*, *Ativando…*, depois ✓ e **Executar importação** pronto);
   - **o pacote**, com tamanho e data;
   - **Atualizar páginas, posts e produtos que já existem (mesmo slug)**, marcada quando a exportação tinha *Atualizar existentes* ligado (o cabeçalho `Eizport Update:` do plugin). Veja [atualizar os itens existentes](../../getting-started/importing/#atualizar-os-itens-existentes).
4. **Executar importação** importa o pacote que o plugin traz. A página espera enquanto ela roda (o Importador do WordPress não informa o progresso) e depois mostra quantos itens foram importados, as configurações definidas e as mensagens do importador.
5. Depois da importação, o botão principal da tela é **Desativar e excluir**: ele cumpriu seu papel e guarda um arquivo grande. Em **Também excluir**, o Eizport e o Importador do WordPress podem ir na mesma ação — marcados quando foram instalados para esta importação (o plugin anota, ao ser ativado, quais já estavam lá), desmarcados nos outros casos. Onde o WordPress pede credenciais de FTP para excluir arquivos, ele é desativado e você o exclui pela tela Plugins.

Depois da importação, a tela não oferece importar de novo, a não ser que a exportação tivesse **Configurações → Exportar → Como plugin: permitir importar de novo** ligada: aí **Importar de novo** continua, recolhido. Importar de novo encontra os itens já importados e não os duplica; com *Atualizar existentes*, uma nova entrega do mesmo conteúdo atualiza os itens.

O plugin leva o nome do arquivo da exportação: *Importação: EIZP-000* para `EIZP-000.zip`.

O mesmo `.zip` também é importado sem ser instalado: envie-o em **Ferramentas → Importar → WordPress**, como um `.wxrz`, ou passe-o para `wp eizport import`. O Eizport tira o pacote de dentro do plugin.


## Segurança

O plugin importa só o pacote que o próprio cabeçalho nomeia (`Eizport Bundle:`), da própria pasta. **Executar importação** exige a capacidade `import` e um nonce; excluir exige `delete_plugins`. O código do plugin é um arquivo pequeno que qualquer pessoa pode ler; ele não baixa nada.

Em [Cópias](../../settings/copies/) ele aparece como *.zip de plugin*: pode ser baixado, não importado no mesmo site — instale-o como plugin.
