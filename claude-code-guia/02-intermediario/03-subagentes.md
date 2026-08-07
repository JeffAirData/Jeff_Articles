# 3. Subagentes: delegando tarefas para agentes especializados

> Nível: Intermediário · Pré-requisito: [02-settings-permissoes-e-variaveis.md](./02-settings-permissoes-e-variaveis.md)

## O problema que subagentes resolvem

Algumas tarefas geram muito "ruído" no contexto: uma busca extensa na web,
a leitura de dezenas de arquivos de log, uma investigação exploratória em
documentação. Se tudo isso entrar na sua conversa principal, o contexto
enche rápido e fica mais caro e mais lento continuar trabalhando.

**Subagentes** são assistentes especializados que rodam em uma **janela de
contexto própria**, isolada da conversa principal. Eles fazem o trabalho
"sujo" e devolvem só um resumo para a sessão principal.

## Criando um subagente

```
.claude/agents/pesquisador/AGENT.md
```

```markdown
---
name: pesquisador
model: haiku
description: Pesquisa e resume documentação técnica
allowed-tools:
  - Read(./docs/**)
  - WebSearch
  - WebFetch
---

Você é um agente de pesquisa. Sua função é ler documentação e
sintetizar os achados em resumos objetivos.

Ao final, entregue um resumo em bullet points, com as fontes usadas.
```

## Quando usar subagentes

- **Pesquisa** que gera muito texto intermediário (busca na web, leitura de
  documentação extensa).
- **Investigação exploratória** que não vai alterar arquivos-fonte.
- **Trabalho em paralelo** enquanto você continua conversando na sessão
  principal.
- **Isolar operações pesadas em contexto** (análise de logs grandes,
  processamento de dados).
- **Economizar custo**: delegar subtarefas simples para um modelo mais
  barato (ex.: Haiku), reservando o modelo principal para o raciocínio mais
  complexo.

## Escopo de ferramentas

Assim como nas skills, `allowed-tools` restringe o que o subagente pode
fazer. Um subagente de pesquisa, por exemplo, não precisa (e não deveria)
ter permissão de `Edit` ou `Bash` com comandos destrutivos.

## Como invocar

O Claude aciona subagentes automaticamente quando percebe que fazem sentido
para a tarefa, ou você pode delegar explicitamente:

```
/subtask Pesquise como implementamos autenticação hoje e resuma os padrões usados
```

Acompanhe o trabalho em andamento com:

```
/tasks
```

## Boas práticas

1. **Nomeie e descreva com precisão** — a descrição é o que ajuda o Claude
   (e você) a saber quando delegar para aquele subagente específico.
2. **Restrinja ferramentas ao mínimo necessário.** Um subagente de leitura
   não precisa de permissão de escrita.
3. **Use modelos mais leves para tarefas simples e repetitivas** — nem
   toda subtarefa exige o modelo mais caro.
4. **Peça resumos objetivos** no prompt do subagente, para que o retorno à
   sessão principal seja enxuto.

## Próximo passo

[`04-mcp-servers.md`](./04-mcp-servers.md) — conectando o Claude Code a
ferramentas e serviços externos via MCP.
