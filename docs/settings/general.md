---
title: "Geral"
description: "Configurações do plugin → Geral — se Ferramentas → Exportar é substituída, a tela clássica, os padrões salvos e por quanto tempo as cópias são mantidas."
translatedFrom: "fd91ed47b73026c542c7910f368d8ca1f1399eeb"
---

![Configurações do plugin → Geral.](../../../assets/screenshots/settings-general.webp)

As configurações do plugin abrem pelo link **Configurações** na linha dele em **Plugins** (`wp-admin/plugins.php?page=eizport-settings-wp-admin`); elas não têm item no menu Configurações.

## Tela Exportar

| Configuração | Padrão | |
| --- | --- | --- |
| **Substituir Ferramentas → Exportar** | Ativado | Ferramentas → Exportar abre a tela Exportar do plugin. Desativado, a tela Exportar do WordPress fica como está, sem nada adicionado, e a do plugin fica em **Ferramentas → Eizport** |
| **Usar a tela Exportar clássica** | Desativado | Só enquanto Ferramentas → Exportar é substituída. Ferramentas → Exportar abre a tela do próprio WordPress com as opções do plugin, em vez da nova ([tela clássica](../../exporting/classic-screen/)) |
| **Os padrões são salvos para** | Cada usuário | Ou *O site inteiro* ([padrões salvos](../../exporting/saved-defaults/)) |

## Tela Importar

| Configuração | Padrão | |
| --- | --- | --- |
| **Abrir o Importador do WordPress em Ferramentas → Importar** | Ativado | Ferramentas → Importar vai direto ao Importador do WordPress, quando ele está ativo; a tela dele leva a **Todos os importadores** (`import.php?all=1`). Desativado, Ferramentas → Importar os lista, como faz o WordPress |
| **Estilizar Ferramentas → Importar → WordPress** | Ativado | O Importador do WordPress fica com o visual da tela Exportar do plugin: um cabeçalho com o botão principal, as etapas em cartões e um botão de arquivo no idioma do painel. Desativado, ele fica como o WordPress fez; pacotes `.wxrz` são aceitos de qualquer jeito ([importação](../../getting-started/importing/)) |

*Limpar padrões salvos* os esquece (os do usuário atual ou os do site).

## Cópias

**Manter cópias por**: 1 hora, 12 horas, **1 dia** (padrão), 1 semana ou até serem excluídas. Depois disso, a cópia deixa de ser servida e é excluída por um evento do cron (e no próximo uso, onde o cron não roda). As cópias já guardadas também seguem uma nova escolha. Desenvolvedores podem mudar isso com [`eizport_copy_lifetime`](../../developers/hooks/#eizport_copy_lifetime).

## Quem pode mudá-las

As configurações exigem `manage_options`. A página em si abre para qualquer pessoa que possa exportar (para ver as cópias).
