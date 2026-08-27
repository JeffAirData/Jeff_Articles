---
title: "MCP: conectando o Claude Code a ferramentas externas"
description: "O que é MCP e como conectar o Claude a serviços externos."
slug: "/articles/claude-code-guia/02-intermediario/04-mcp-servers"
date: "2026-08-26"
author: "Jefferson O. Melo"
tags: []
---

# 4. MCP: conectando o Claude Code a ferramentas externas

> Nível: Intermediário · Pré-requisito: [03-subagentes.md](./03-subagentes.md)

## O que é MCP

**MCP (Model Context Protocol)** é um padrão aberto para conectar o Claude a
serviços externos: bancos de dados, GitHub, Slack, Jira, Google Drive,
Figma, e praticamente qualquer sistema que exponha um servidor MCP (oficial
ou criado por você/sua empresa).

Na prática: sem MCP, o Claude só conhece o que está no seu sistema de
arquivos local. Com MCP, ele pode consultar um ticket no Jira, ler um
documento de especificação no Google Drive, ou rodar uma query direto no
banco de dados — sempre respeitando as permissões que você configurar.

## Instalando um servidor MCP

Via marketplace de plugins:

```
/plugin install github
```

Ou configurando manualmente em `.claude/settings.json`:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["@anthropic-ai/github-mcp"],
      "env": {
        "GITHUB_TOKEN": "seu-token-aqui"
      }
    }
  }
}
```

> 🔒 Nunca coloque tokens/segredos direto em um `settings.json` versionado
> no Git. Use `.claude/settings.local.json` (fora do controle de versão) ou
> uma variável de ambiente já exportada no seu sistema.

## Casos de uso comuns

| Servidor | Para que serve |
|---|---|
| GitHub | Ler/criar issues e PRs, consultar histórico de commits |
| Jira / Linear | Ler e atualizar tickets, linkar com o código |
| Google Drive | Buscar documentos de especificação, atas de reunião |
| Slack | Consultar contexto de discussões em canais |
| Bancos de dados (Postgres, MySQL) | Rodar consultas diretamente |
| Sentry | Buscar dados de erros de produção |
| Figma | Ler arquivos e specs de design |

## Gerenciando conexões

```
/mcp
```

Mostra os servidores configurados, status da conexão e permite gerenciar
autenticação OAuth quando aplicável.

## Cuidado com o contexto

Cada ferramenta de cada servidor MCP consome espaço de contexto quando
carregada. Em setups com muitos servidores, o Claude Code usa uma técnica de
"busca de ferramentas" (tool search): por padrão, só os **nomes** das
ferramentas ficam no contexto; a definição completa só é carregada quando a
ferramenta é efetivamente usada. Ainda assim, é boa prática:

- Instalar apenas os servidores MCP que você realmente usa naquele projeto.
- Revisar periodicamente com `/mcp` e remover o que não é mais necessário.

## Próximo passo

[`05-plan-mode-e-checkpoints.md`](./05-plan-mode-e-checkpoints.md) —
planejando antes de agir e voltando no tempo quando algo dá errado.
