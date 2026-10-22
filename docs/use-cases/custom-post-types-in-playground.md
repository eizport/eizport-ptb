---
title: "Tipos de post personalizados no Playground"
description: "Um site cujos tipos de post ficam num plugin obrigatório, mostrado no WordPress Playground: o plugin vai no .zip e roda antes da importação."
translatedFrom: "9ec38c9c09613b210f273b38f9b40505372fa980"
---

## O cenário

O site guarda o tipo de post **Receitas** num plugin obrigatório — `wp-content/mu-plugins/receitas.php`, que carrega `receitas/register.php` — para ninguém poder desativá-lo. Uma demonstração no Playground com as receitas precisa desse plugin: sem ele, não existe o tipo de post Receitas para receber a importação.

## Os passos

1. **Ferramentas → Exportar**: marque **Receitas** (e o que as receitas precisam).
2. **Detalhes → Formato: Playground**, depois **Opções do Playground → Temas e plugins**:
   - **Plugins obrigatórios a instalar**: o plugin das receitas;
   - o tema do site e os plugins que as receitas usam.
3. **Exportar**, depois **Abrir no Playground** na cópia, em [Cópias](../../settings/copies/).

## O que acontece

O `.zip` leva `mu-plugins/receitas.php` e `mu-plugins/receitas.zip` (a pasta que ele carrega). O blueprint os grava em `wp-content/mu-plugins/` do Playground **antes da importação**, então o tipo de post Receitas já existe quando as receitas chegam, e o menu mostra **Receitas** com elas.

## O que ele não faz

- Os plugins obrigatórios da própria hospedagem (e o carregador do SQLite do WordPress Studio) não são oferecidos: o Playground tem os dele.
- Um plugin obrigatório que carrega arquivos de outro lugar (um caminho fora de `mu-plugins/`) leva só o que está em `mu-plugins/`.
- Código personalizado vai dentro do arquivo: compartilhe só onde a licença dele permite.
