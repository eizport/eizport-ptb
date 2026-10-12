---
title: "Cópias"
description: "As exportações guardadas no site — baixar, abrir no Playground, copiar o link, importar, excluir — e como os links delas funcionam."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

![Configurações do plugin → Cópias: um .wxrz, um pacote .zip e um .zip do Playground.](../../../assets/screenshots/settings-copies.webp)

Toda exportação feita pela nova tela ou pela habilidade é guardada como cópia em `wp-content/uploads/eizport/tmp/`, listada como na biblioteca de mídia: pesquisa, ordenação, filtro por formato.

## Ações

| Ação | Para |
| --- | --- |
| **Abrir no Playground** | Cópias `.zip` do Playground: abre o playground.wordpress.net com o link da cópia |
| **Baixar** | Todas as cópias |
| **Copiar link** | Todas as cópias: qualquer pessoa com o link pode baixá-la até que expire |
| **Importar** | Pacotes e arquivos WXR: importa neste site |
| **Excluir permanentemente** | Uma, várias (marque-as) ou **Excluir todas** no cabeçalho |

## Links e privacidade

O link de uma cópia é feito para ser compartilhado com o Playground ou com alguém da equipe, não para ser encontrado por mais ninguém:

- **Um token como uma senha:** o nome termina num token aleatório de 16 caracteres (`site-w9bjZ9pyEWxC8VMf.zip`, de um gerador criptográfico), então o link não pode ser adivinhado. A lista mostra o nome sem ele (`site.zip`); passar o mouse mostra o nome inteiro.
- **Servido pelo PHP, só até expirar:** o link passa pelo WordPress (`?eizport_playground=<name>`), que verifica a validade e envia os cabeçalhos CORS de que o Playground precisa.
- **Fora dos mecanismos de pesquisa:** a resposta traz `X-Robots-Tag: noindex, nofollow, noarchive`, e o `robots.txt` do site recebe regras `Disallow` para a pasta e os links das cópias.
- **Sem acesso direto:** o `uploads/eizport/.htaccess` nega solicitações diretas no Apache e no LiteSpeed, e toda pasta tem um `index.php`, então nada é listado. No nginx, o `.htaccess` não se aplica; negue `/wp-content/uploads/eizport/` na configuração do servidor para o mesmo efeito — o token, a validade e o robots.txt continuam protegendo os arquivos.

Não guarde cópias de conteúdo que precisa continuar privado.

## Num site local

Cópias feitas num site local (WordPress Studio, `localhost`) também abrem no playground.wordpress.net: o Playground roda **no seu navegador**, que alcança seu servidor local. O link só funciona no seu computador — perfeito para testar uma demonstração antes de compartilhá-la.

## Limpeza

As cópias expiradas são excluídas por um evento do cron e no próximo uso; a própria pasta `tmp/` vai embora quando fica vazia. Desinstalar o plugin exclui `uploads/eizport/`.
