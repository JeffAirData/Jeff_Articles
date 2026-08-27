---
title: "Fluxo com Git e GitHub"
description: "Práticas e integração do Claude Code com Git e GitHub."
slug: "/articles/claude-code-guia/02-intermediario/06-fluxo-git-e-github"
date: "2026-08-26"
author: "Jefferson O. Melo"
tags: []
---

# 6. Fluxo com Git e GitHub

> Nível: Intermediário · Pré-requisito: [05-plan-mode-e-checkpoints.md](./05-plan-mode-e-checkpoints.md)

## O que o Claude Code faz nativamente com Git

Sem nenhuma configuração extra, o Claude consegue:

- Ver o estado atual (`git status`) e o diff de mudanças não commitadas.
- Criar e trocar de branch.
- Adicionar arquivos ao stage e criar commits com mensagens descritivas.
- Consultar o histórico de commits para entender o contexto de um trecho de
  código ("por que essa linha existe?").
- Fazer merge de branches.

Na prática, você pode simplesmente pedir:

```
Crie uma branch a partir da main chamada fix/validacao-email,
corrija o bug de validação de e-mail em src/auth.ts, e faça o commit.
```

## Boas práticas ao pedir commits

- Peça para o Claude **revisar o diff antes de commitar**
  (`/review` ajuda aqui).
- Prefira mensagens de commit descritivas — peça explicitamente esse
  padrão no seu `CLAUDE.md` se seu time segue uma convenção (ex.:
  Conventional Commits).
- Nunca deixe o Claude fazer `push --force` sem revisão explícita —
  bloqueie isso em `settings.json` (veja o capítulo de permissões).

## Integração com GitHub

Instalando o app oficial do GitHub:

```
/install-github-app
```

Isso habilita o Claude a:

- Ler issues e Pull Requests do repositório.
- Criar branches a partir de uma issue.
- Abrir PRs e responder a comentários de revisão.

## Automatizando revisão de PR via GitHub Actions

Um workflow básico que responde quando alguém menciona `@claude` em um
comentário:

```yaml
name: Claude Code
on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]

jobs:
  claude:
    if: contains(github.event.comment.body, '@claude')
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pull-requests: write
      issues: write
    steps:
      - uses: actions/checkout@v6
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
```

> 📚 Este workflow é um ponto de partida. Consulte a documentação oficial
> da `claude-code-action` para a versão e opções mais atuais antes de usar
> em produção.

## Acompanhando um PR até o fim

Este próprio repositório usa esse fluxo: depois de abrir um PR, é possível
pedir para o Claude **observar** o PR e reagir automaticamente a falhas de
CI e comentários de revisão, sem precisar de intervenção manual a cada
evento — útil para manter um PR avançando mesmo quando você está longe do
teclado.

## Próximo passo

Você concluiu o nível Intermediário! Vá para
[`03-avancado-senior/`](../03-avancado-senior/) — hooks, Agent SDK, modo
headless/CI, orquestração multi-agente e práticas de nível sênior.
