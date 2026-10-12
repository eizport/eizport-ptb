---
title: "Padrões salvos"
description: "Salve as escolhas da tela Exportar como padrão, por usuário ou para o site inteiro, nas duas telas."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

**Salvar como padrão** guarda tudo o que você escolheu: tipos de conteúdo com seus itens (ou filtros, na tela clássica), opções, formato, nome do arquivo, arquivos de mídia e os temas e plugins do Playground. A tela Exportar abre com eles, e **Redefinir para os padrões** volta para eles. Nada mais é guardado: as escolhas que você não salva se perdem ao sair da tela ou recarregá-la.

## De quem são os padrões

**Configurações → Geral → Os padrões são salvos para**:

- **Cada usuário** (padrão) — guardados nos metadados do usuário; quem pode exportar salva os seus.
- **O site inteiro** — um conjunto para todos; salvar ou limpar exige `manage_options`.

## As duas telas

- **Tela nova:** *Salvar como padrão* no cabeçalho; *Redefinir para os padrões* volta para os padrões, e *Limpar padrões* (ativo quando as escolhas já são os padrões) os esquece.
- **Tela clássica:** marque *Salvar estas escolhas como meu padrão* antes de baixar. A tela clássica também já inicia preenchida com os padrões.

## Limpar

*Limpar padrões* no cabeçalho da tela Exportar, *Limpar padrões salvos* em Configurações → Geral, ou a habilidade [`eizport/clear-export-defaults`](../../developers/abilities/#defaults).

## O que é verificado

Os padrões são sanitizados ao salvar e de novo ao carregar: tipos e itens desconhecidos, opções que nunca são exportadas e temas ou plugins que não estão mais instalados são descartados. Os filtros guardam só os valores definidos.
