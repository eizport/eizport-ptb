---
title: "Instalação"
description: "Instale o plugin pelo zip da versão, confira os requisitos e adicione o Importador do WordPress para importar."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

## Pelo zip da versão

1. Baixe a versão mais recente: [`eizport.zip`](https://github.com/eizport/eizport/releases/latest/download/eizport.zip). O URL sempre aponta para a versão mais recente.
2. No WordPress, vá em **Plugins → Adicionar plugin → Enviar plugin**, escolha o arquivo e clique em **Instalar agora**.
3. **Ative** o plugin. Ferramentas → Exportar agora abre a nova tela Exportar.

## Com WP-CLI

```bash
wp plugin install https://github.com/eizport/eizport/releases/latest/download/eizport.zip --activate
```

## Experimente antes

Abra a [demonstração ao vivo no WordPress Playground](https://playground.wordpress.net/?blueprint-url=https://raw.githubusercontent.com/eizport/eizport/trunk/playground/blueprint.json): um site descartável no seu navegador com o plugin e páginas de exemplo.

## Para importar

Importar exige o **Importador do WordPress**. Em **Ferramentas → Importar**, clique em **Instalar agora** abaixo de *WordPress* e depois em **Executar importador**. Os links do plugin levam até lá enquanto o importador não está instalado. Exportar funciona sem ele.

## Requisitos em detalhe

| Requisito | Por quê |
| --- | --- |
| WordPress 7.0+ | As telas Exportar e Configurações são construídas com `@wordpress/build` e iniciadas pelo `@wordpress/boot` dos arquivos básicos do WordPress. |
| PHP 7.4+ | O mínimo do plugin. |
| `zlib` | Os pacotes `.wxrz` são arquivos tar compactados com gzip. |
| `ZipArchive` (ou PclZip) | Pacotes `.zip` e arquivos `.zip` do Playground. O PclZip dos arquivos básicos do WordPress é a contingência. |
| Abilities API | Faz parte do WordPress 6.9+; as telas usam as habilidades do plugin. |

## Desinstalação

Excluir o plugin remove `wp-content/uploads/eizport/` (cópias temporárias e importações inacabadas), as configurações dele e os padrões de exportação salvos (o do site e os de cada usuário).
