---
title: "Output styles, status line e monitoramento de custo"
description: "Como customizar saída e monitorar custo e tokens em sessões do Claude Code."
slug: "/articles/claude-code-guia/03-avancado-senior/06-output-styles-custos-e-monitoramento"
---

# 6. Output styles, status line e monitoramento de custo

> Nível: Sênior/Avançado · Pré-requisito: [05-enterprise-seguranca-e-times.md](./05-enterprise-seguranca-e-times.md)

## Por que isso importa em nível sênior

Em uso individual e esporádico, custo e formatação de saída são detalhes
menores. Em uso intenso — times inteiros, pipelines automatizados, sessões
longas — eles viram parte da engenharia: você precisa **enxergar** onde o
orçamento está indo para poder otimizar.

## Estilos de saída

```
/config
```

Permite ajustar como as respostas são exibidas no terminal (formatação mais
enxuta, estilo mais próximo de markdown do GitHub, etc.), conforme seu
fluxo de trabalho e preferências de legibilidade.

## Personalizando a barra de status

É possível customizar a barra de status para mostrar, por exemplo, o nome
da sessão, modelo em uso, modo de permissão ativo, custo acumulado e tokens
consumidos — direto na tela, sem precisar rodar `/usage` a cada verificação.
Consulte a documentação oficial para o arquivo e formato de configuração
mais atuais da sua versão instalada.

## Acompanhando custo e uso

```
/usage
```

Mostra, tipicamente:

- Custo total da sessão atual.
- Detalhamento por modelo (se mais de um foi usado).
- Tokens de entrada/saída, incluindo hits/writes de cache.
- Limites do seu plano (Pro, Max, Team, Enterprise).

Em contas Team/Enterprise, limites de gasto por usuário ou por organização
são configuráveis no painel administrativo.

## Estratégias para reduzir custo

| Estratégia | Efeito |
|---|---|
| `/clear` entre tarefas não relacionadas | Evita carregar contexto obsoleto/irrelevante |
| Usar Sonnet em vez de Opus quando suficiente | Reduz custo por token significativamente |
| Delegar subtarefas simples a modelos mais leves (ex.: Haiku) em subagentes | Reserva o modelo caro para o raciocínio complexo |
| Restringir MCP aos servidores realmente usados | Menos definições de ferramenta ocupando contexto |
| Instruções de compactação customizadas no `CLAUDE.md` | `/compact` preserva o que importa e descarta o resto com mais eficiência |
| Modo Plano antes de tarefas grandes | Evita retrabalho (e reprocessamento de contexto) por abordagem errada |

## Monitoramento em nível de organização

Para engenheiros responsáveis por adoção em time:

1. Acompanhe `/usage` como rotina em sessões longas, não só no final.
2. Em pipelines de CI, sempre capture o custo retornado
   (`--output-format json`) e registre em algum sistema de observabilidade
   — custo de automação que roda a cada PR/push soma rápido.
3. Estabeleça um orçamento de referência por tipo de tarefa (ex.: "revisão
   de PR não deveria custar mais que X") e investigue outliers.
4. Reserve os modelos e níveis de esforço mais altos para tarefas que
   comprovadamente precisam — meça antes de assumir que "mais esforço" é
   sempre melhor custo-benefício.

## Próximo passo

[`07-boas-praticas-codebases-grandes.md`](./07-boas-praticas-codebases-grandes.md) —
consolidando tudo em boas práticas para bases de código grandes.
