---
title: "CLAUDE.md: dando contexto persistente ao seu projeto"
description: "Como escrever um CLAUDE.md para fornecer contexto persistente ao Claude Code."
slug: "/articles/claude-code-guia/01-iniciante/02-claude-md-e-contexto"
---

# 2. CLAUDE.md: dando contexto persistente ao seu projeto

> Nível: Iniciante · Pré-requisito: [01-instalacao-e-primeiros-passos.md](./01-instalacao-e-primeiros-passos.md)

## O problema que o CLAUDE.md resolve

Sem contexto, o Claude precisa redescobrir do zero, a cada sessão, coisas como:
"qual comando roda os testes?", "qual o padrão de nomenclatura daqui?",
"por que essa pasta `legacy/` existe e não deve ser mexida?".

O `CLAUDE.md` é um arquivo markdown que o Claude lê **automaticamente no
início de toda sessão**, sem você precisar colar nada.

## Onde colocar

| Escopo | Caminho | Compartilhado com o time? |
|---|---|---|
| Projeto (raiz) | `CLAUDE.md` na raiz do repositório | Sim, versionado no Git |
| Pessoal | `~/.claude/CLAUDE.md` (pasta do usuário) | Não |
| Por subpasta | `packages/api/CLAUDE.md`, por exemplo | Sim, se versionado |

Em monorepos, o Claude carrega o `CLAUDE.md` da pasta onde a sessão foi
iniciada **mais** os arquivos das pastas pai — não é preciso repetir regras
gerais em cada subpasta.

## O que colocar (nível iniciante)

Comece simples. Um bom `CLAUDE.md` inicial responde:

```markdown
# Meu Projeto

## Stack
- Node.js 20, TypeScript, Express
- Banco: PostgreSQL via Prisma

## Comandos essenciais
- Instalar dependências: `npm install`
- Rodar em dev: `npm run dev`
- Rodar testes: `npm test`
- Lint: `npm run lint`

## Convenções
- Componentes React em PascalCase, um por arquivo
- Sempre escrever teste antes de dar a tarefa como concluída
- Não editar a pasta `legacy/` — código em processo de descontinuação

## O que evitar
- Não instalar novas dependências sem perguntar
- Não commitar diretamente na branch `main`
```

## Gerando automaticamente

O Claude Code consegue examinar seu projeto e propor um `CLAUDE.md` inicial:

```
/init
```

Revise o resultado — o arquivo gerado é um ponto de partida, não a versão
final. Ajuste com as convenções reais do seu time.

## Editando durante a sessão

```
/memory
```

Abre o `CLAUDE.md` (e a memória automática — veja abaixo) para edição direta.

## Memória automática (bônus)

Além do `CLAUDE.md` que você escreve, o Claude Code também guarda
aprendizados automáticos ao longo do uso (padrões descobertos, comandos de
build, armadilhas já encontradas) em um arquivo próprio na sua máquina. Você
não precisa gerenciar isso manualmente no início — é bônus, não obrigação.

## Regra prática

> Trate o `CLAUDE.md` como a "ficha de integração" que você daria a um novo
> desenvolvedor no primeiro dia: contexto do projeto, comandos que ele vai
> usar toda hora, e os "não faça isso" mais importantes. Nada mais que isso
> no começo — você vai refinar com o tempo.

## Próximo passo

[`03-permissoes-basicas.md`](./03-permissoes-basicas.md) — como o Claude
Code pede (ou não) sua aprovação antes de agir.
