---
title: "settings.json: permissões persistentes e variáveis de ambiente"
description: "Como configurar permissões e variáveis persistentes para o Claude Code."
slug: "/articles/claude-code-guia/02-intermediario/02-settings-permissoes-e-variaveis"
---

# 2. settings.json: permissões persistentes e variáveis de ambiente

> Nível: Intermediário · Pré-requisito: [01-skills-e-comandos-customizados.md](./01-skills-e-comandos-customizados.md)

## Por que sair do "aprovar sempre no chat"

No nível iniciante, você aprendeu a aprovar ações uma a uma. Isso é ótimo
para aprender, mas não escala: em um projeto que você já confia, faz sentido
deixar comandos rotineiros (rodar testes, formatar código) pré-aprovados —
e continuar exigindo aprovação manual para o que é sensível (deletar
arquivos, acessar segredos, `git push --force`).

Isso se configura em arquivos `settings.json`.

## Hierarquia de configurações

Do menor para o maior nível de prioridade:

1. **Usuário** — `~/.claude/settings.json` (pessoal, vale para todos os projetos)
2. **Projeto** — `.claude/settings.json` (compartilhado com o time via Git)
3. **Local** — `.claude/settings.local.json` (pessoal, específico deste repo — deve entrar no `.gitignore`)
4. **Gerenciado** (organizacional) — definido por um administrador, tem prioridade máxima (nível avançado)

> ⚠️ Regras de **permissão** (`allow`/`deny`) de escopos diferentes se
> **combinam** (não se sobrescrevem) — todas valem ao mesmo tempo, com `deny`
> sempre vencendo em caso de conflito.

## Exemplo de `settings.json` de projeto

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run lint)",
      "Bash(npm test *)",
      "Read(./docs/**)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force*)",
      "Read(./.env)",
      "Read(./.env.*)",
      "Read(./**/secrets/**)"
    ]
  }
}
```

- `allow`: aprova automaticamente ações que casam com o padrão.
- `deny`: bloqueia completamente — tem prioridade sobre `allow`.
- O que não está em nenhuma lista continua pedindo aprovação manual (`ask`,
  implícito).

Os padrões usam a sintaxe `Ferramenta(padrão)` — por exemplo,
`Bash(npm test *)` cobre qualquer comando que comece com `npm test `.

## Variáveis de ambiente

```json
{
  "env": {
    "NODE_ENV": "development",
    "DEBUG": "true"
  }
}
```

Essas variáveis ficam disponíveis para os comandos que o Claude executa
nessa sessão — úteis para configurar comportamento de scripts sem
precisar exportá-las manualmente no terminal toda vez.

## Configuração rápida sem editar o arquivo

```
/config verbose=true
```

Muitas preferências podem ser ajustadas assim, sem abrir o JSON na mão.

## Checklist prático para configurar um projeto

1. Crie `.claude/settings.json` na raiz e versione no Git.
2. Libere comandos rotineiros e seguros: testes, lint, build.
3. Bloqueie explicitamente o que é sensível: arquivos de segredo/credenciais,
   comandos destrutivos, push forçado.
4. Adicione `.claude/settings.local.json` ao `.gitignore` — é o lugar para
   preferências pessoais que não devem ir para o repositório.
5. Rode `/doctor` de vez em quando para validar a configuração.

## Próximo passo

[`03-subagentes.md`](./03-subagentes.md) — delegando tarefas para agentes
especializados com contexto próprio.
