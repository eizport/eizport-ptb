# Eizport — pt-BR

Tradução em português do Brasil do site do [Eizport](https://github.com/eizport/eizport): https://eizport.github.io/br/.

Este repositório só tem texto traduzido. O site é montado no repositório do plugin, que baixa esta tradução a cada publicação (veja `site/translations.json` e `site/scripts/translations.mjs` lá). Um push aqui publica o site de novo.

## O que há aqui

| Caminho | O quê |
| --- | --- |
| `strings.json` | Os textos das páginas e dos componentes do site (menus, botões, landing, doações). Mesmo formato de `site/src/i18n/en.ts` do repositório do plugin; o que faltar aparece em inglês. |
| `docs/<grupo>/<página>.md` | A documentação: mesmo caminho e nome da página em inglês em `site/src/content/docs/`. |
| `assets/screenshots/` | Screenshots da interface em português, com os mesmos nomes dos em inglês (`npm run screenshots -- --locale br=…` no repositório do plugin). |
| `video/captions.txt`, `video/voice.json` | As legendas do vídeo de demonstração (uma linha `id: texto` por cena, os mesmos ids de `site/video/STORYBOARD.md` do repositório do plugin) e a voz da narração (`say` do macOS: voz, velocidade, como ler "ponto" e siglas). `npm run video -- --locale br=<este repositório>` grava o vídeo em português em `assets/video/`. |
| `video/youtube.md` | Título, descrição e capítulos para enviar o vídeo em português ao YouTube. |

A tradução do próprio plugin (a interface e o readme) fica no [translate.wordpress.org](https://translate.wordpress.org/locale/pt-br/default/wp-plugins/eizport/) e chega aos sites pelo pacote de idioma. `npm run plugin-languages`, no repositório do plugin, instala esse pacote nos sites locais.

## Como traduzir uma página

1. Copie a página em inglês de `site/src/content/docs/<grupo>/<página>.md` para `docs/<grupo>/<página>.md`.
2. Traduza `title`, `description` e o texto, seguindo o [glossário da comunidade WordPress Brasil](https://translate.wordpress.org/locale/pt-br/default/glossary/) e os textos da interface do plugin como estão na [tradução oficial do plugin](https://translate.wordpress.org/locale/pt-br/default/wp-plugins/eizport/). Não mude código, nomes de arquivos, URLs, alvos de links nem caminhos de imagens.
3. Em `translatedFrom`, ponha o commit do repositório do plugin de onde a página em inglês foi copiada.

Quando a página em inglês muda depois desse commit, o site mostra um aviso de que a tradução está desatualizada. Atualize o texto e o `translatedFrom`. Uma página que ainda não existe aqui aparece em inglês, com um link para traduzi-la.

## Ver localmente

No repositório do plugin, com este ao lado:

```sh
cd site
npm run translations -- --local br=../../eizport-ptb
npm run dev   # http://localhost:4321/br/
```
