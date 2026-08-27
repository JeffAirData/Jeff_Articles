---
title: "Skills e comandos customizados"
description: "Introdução a skills e comandos customizados no Claude Code."
slug: "/articles/claude-code-guia/02-intermediario/01-skills-e-comandos-customizados"
date: "2026-08-26"
author: "Jefferson O. Melo"
tags: []
---

# 1. Skills e comandos customizados

> Nível: Intermediário · Pré-requisito: nível Iniciante completo

## De comando customizado a skill

No começo do Claude Code, "comandos customizados" eram simples arquivos
markdown que viravam comandos `/nome`. Esse conceito evoluiu para **skills**:
pacotes de instruções reutilizáveis que o Claude carrega sob demanda — seja
porque você chamou explicitamente (`/nome-da-skill`), seja porque o próprio
Claude percebeu que a skill é relevante para a tarefa atual.

A ideia central: em vez de colar as mesmas instruções repetidamente no chat,
ou inflar o `CLAUDE.md` com procedimentos longos, você guarda esse
conhecimento em um arquivo que só entra no contexto quando é realmente
necessário — economizando espaço de contexto no dia a dia.

## Criando sua primeira skill

Estrutura de pastas:

```
.claude/skills/deploy/SKILL.md
```

Conteúdo:

```markdown
---
name: deploy
description: Procedimento de deploy para staging e produção
trigger: manual
allowed-tools:
  - Bash(npm run deploy*)
  - Read(./scripts/deploy/*)
---

# Deploy

1. Rode `npm run test` e confirme que passou
2. Rode `npm run build`
3. Execute `npm run deploy -- --env=staging`
4. Peça confirmação humana antes de rodar com `--env=production`
```

Depois disso, `/deploy` executa esse procedimento.

## Campos do frontmatter

| Campo | Para que serve |
|---|---|
| `name` | Nome da skill — vira o comando `/nome` |
| `description` | Descrição que o Claude lê para decidir se a skill é relevante (importante para o modo automático) |
| `trigger` | `manual` (só via `/nome`) ou `auto` (Claude decide quando usar) |
| `allowed-tools` | Lista de ferramentas pré-aprovadas para essa skill, evitando prompts de permissão repetidos |
| `model` | Sobrescreve o modelo usado para essa skill específica |
| `disable-model-invocation` | Mantém a descrição fora do contexto até a skill ser usada — útil para skills grandes |

## Onde guardar

| Escopo | Caminho | Compartilhado? |
|---|---|---|
| Projeto | `.claude/skills/` na raiz | Sim, versionado |
| Pessoal | `~/.claude/skills/` | Não |
| Por subpasta | `packages/api/.claude/skills/` | Sim, escopado àquele pacote |

## Quando criar uma skill (em vez de só pedir no chat)

Crie uma skill quando você perceber que:

- Está colando as mesmas instruções pela terceira vez.
- Uma seção do `CLAUDE.md` virou um procedimento de vários passos.
- A tarefa é específica de um tipo de arquivo ou pasta (ex.: "toda vez que
  eu mexer em `src/api/`, siga este checklist").

## Boas práticas

1. **Descrição curta e objetiva**, começando com verbo de ação — é o que o
   Claude usa para decidir se aciona a skill automaticamente.
2. **Pré-aprove as ferramentas** que a skill sempre vai usar
   (`allowed-tools`), reduzindo interrupções.
3. **Uma skill, um objetivo.** Prefira várias skills pequenas a uma
   gigante e genérica.
4. **Versione as skills de projeto no Git** — elas fazem parte do
   conhecimento do time, assim como o `CLAUDE.md`.

## Próximo passo

[`02-settings-permissoes-e-variaveis.md`](./02-settings-permissoes-e-variaveis.md) —
como configurar permissões e variáveis de ambiente de forma persistente.
