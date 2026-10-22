---
title: "Aparência do tema para o staging"
description: "Uma mudança na aparência de um tema feita localmente — a última regra CSS, o logo, um widget, ou a paleta de um tema de blocos — levada para o staging sem mexer no que o staging tem de próprio."
translatedFrom: "fd8a8ff7d1c76143e1e93971971b809fe7e23fb8"
---

## O cenário

O staging já tem a aparência dele: o próprio CSS adicional, os widgets, as cores. Localmente, entrou uma última regra CSS para o rodapé, o logo mudou, e um widget *Horário* foi para o rodapé. Só isso deve chegar ao staging — copiar a aparência inteira apagaria o que o staging tem de próprio, e refazer à mão é onde algo é esquecido.

O mesmo vale para um tema de blocos personalizado no editor do site: uma paleta de cores nova, mais uma regra no CSS adicional dele.

## Os passos

1. No site local, **Ferramentas → Exportar**. Abra **Escolher itens** em **CSS adicional do Personalizador** e marque só a última regra (*Regra 3 de 3 (a última)*), cada uma com a prévia; em **Theme mods do Personalizador**, o Logo; em **Widgets**, *Horário*. Com um tema de blocos: o grupo *Paleta de cores* de **Estilos globais do editor** e uma regra de **CSS global do editor**.
2. **Detalhes → Regras CSS na importação**: *Acrescentar no fim do CSS do site* (o padrão).
3. **Exportar**, e no staging **Ferramentas → Importar → WordPress** com o arquivo (com o Eizport ativo lá). A etapa 1 mostra **CSS adicional**, como o arquivo pede; mude ali se precisar.

## O que o staging recebe

- O próprio CSS, e depois a regra, entre comentários `/* head … */` e `/* tail … */` com o nome do arquivo.
- O logo (a imagem é importada junto); os outros theme mods dele como estavam.
- *Horário* no fim do rodapé, depois dos widgets dele.
- Com um tema de blocos: a paleta; as próprias cores e a tipografia como estavam.

## Da próxima vez

Dê às exportações o nome da tarefa — `COMB-584.wxrz` — e as regras no staging ficam entre `/* head COMB-584 */` e `/* tail COMB-584 */`. Outra mudança da mesma tarefa, outra exportação com o mesmo nome — com **Detalhes → Regras CSS na importação: Substituir o bloco de mesmo nome**: a etapa 1 do importador vem com **Substituir o bloco "COMB-584" que este site já tem** escolhido, que troca as regras antigas pelas novas e deixa o resto do CSS do staging como está. Com *Acrescentar no fim*, um segundo bloco entra depois do primeiro (o importador também oferece a substituição). Com *Atualizar existentes* ligado, o widget *Horário* é atualizado no lugar em vez de ser acrescentado de novo.

## O que não leva

- Fontes instaladas pela Biblioteca de fontes.
- Uma barra lateral que o tema do staging não tem: os widgets dela vão para *Widgets inativos*.
