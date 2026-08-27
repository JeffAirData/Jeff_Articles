---
title: "Modo headless, Claude Code na web e integração com CI/CD"
description: "Execução não interativa (headless) e integração com pipelines CI/CD."
slug: "/articles/claude-code-guia/03-avancado-senior/03-modo-headless-web-e-cicd"
date: "2026-08-26"
author: "Jefferson O. Melo"
tags: []
---

# 3. Modo headless, Claude Code na web e integração com CI/CD

> Nível: Sênior/Avançado · Pré-requisito: [02-agent-sdk.md](./02-agent-sdk.md)

## Modo não interativo (headless) na CLI

A flag `-p` roda o Claude Code sem interface interativa: você dá um prompt,
ele executa e devolve o resultado — ideal para scripts e pipelines.

```bash
claude -p "Traduza src/messages.json para francês" --output-format json
```

Saída estruturada (formato aproximado):

```json
{
  "result": "...",
  "session_id": "sess_...",
  "usage": { "input_tokens": 1234, "output_tokens": 567 },
  "cost": 0.042
}
```

Flags úteis nesse modo:

| Flag | Para que serve |
|---|---|
| `-p` | Ativa o modo não interativo |
| `--output-format json` | Retorna JSON estruturado, fácil de parsear em script |
| `--max-turns N` | Limita quantas idas e vindas o agente pode fazer |
| `--model <modelo>` | Define o modelo a usar |
| `--resume <id>` | Continua uma sessão existente em vez de começar do zero |

> ⚠️ Modo headless costuma ser combinado com o modo de permissão
> `bypassPermissions` (só pode ser ativado ao iniciar o processo) para rodar
> sem ninguém disponível para aprovar ações. **Use com extremo cuidado** —
> normalmente restrito a ambientes isolados (containers efêmeros, CI) com
> escopo de acesso limitado, nunca em uma máquina com credenciais amplas.

## Claude Code na web

Em `claude.ai/code`, sessões rodam em uma máquina gerenciada pela Anthropic
na nuvem — você pode iniciar uma tarefa pelo celular e continuar depois no
terminal, ou vice-versa.

```bash
claude --cloud "Corrija o bug de autenticação em src/auth/login.ts"
```

Recursos típicos desse modo:

- Sessões persistem mesmo com o terminal fechado (a máquina é reciclada após
  um período de inatividade).
- Integração com GitHub: clonar repositório, criar PRs, observar CI.
- Capacidade de acompanhar um PR e reagir automaticamente a falhas de CI e
  comentários de revisão.
- Sessões podem ser compartilhadas com o time.

## Integração com GitHub Actions

```yaml
name: Claude Code Review
on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: actions/checkout@v6
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: "/code-review"
```

Dois padrões comuns de uso em Actions:

1. **Modo interativo**: o job só roda quando alguém menciona `@claude` em
   um comentário de PR/issue.
2. **Modo automação**: o job roda em todo PR/push, com um `prompt` fixo
   (ex.: sempre rodar `/code-review` ou `/security-review`).

## Checklist para produção (CI/CD)

1. **Escopo mínimo de permissões** no token do GitHub Actions
   (`contents`, `pull-requests`, `issues` — só o necessário).
2. **`ANTHROPIC_API_KEY` como secret**, nunca hardcoded no workflow.
3. **Defina `--max-turns`** para evitar loops longos e custo inesperado.
4. **Monitore custo** — pipelines automatizados rodam sem supervisão humana
   a cada evento; tenha alertas de gasto configurados.
5. **Rode em ambiente isolado** (runner efêmero, sem acesso a segredos que
   não sejam estritamente necessários para aquele job).

## Próximo passo

[`04-orquestracao-multi-agente.md`](./04-orquestracao-multi-agente.md) —
coordenando múltiplos agentes trabalhando em paralelo.
