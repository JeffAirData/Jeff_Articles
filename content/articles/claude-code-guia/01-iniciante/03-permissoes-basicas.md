---
title: "Permissões: como o Claude Code pede (ou não) sua aprovação"
description: "Explicação dos modos de permissão do Claude Code e boas práticas."
slug: "/articles/claude-code-guia/01-iniciante/03-permissoes-basicas"
---

# 3. Permissões: como o Claude Code pede (ou não) sua aprovação

> Nível: Iniciante · Pré-requisito: [01-instalacao-e-primeiros-passos.md](./01-instalacao-e-primeiros-passos.md)

## Por que existe esse sistema

O Claude Code pode editar arquivos e rodar comandos no seu terminal. Isso é
poderoso, mas exige controle: você decide o quanto de autonomia dar, e pode
mudar isso a qualquer momento — inclusive no meio de uma sessão.

## Os modos de permissão

Pressione **Shift+Tab** durante uma sessão para alternar entre os modos:

| Modo | Comportamento |
|---|---|
| **Manual** (padrão) | Pergunta antes de editar arquivos ou rodar comandos |
| **Aceitar edições** | Aprova automaticamente edições de arquivo e comandos simples de sistema de arquivos (criar pasta, mover, remover); ainda pergunta para o resto |
| **Plano** | O Claude só explora e propõe um plano — não edita nada até você aprovar |
| **Bypass** (avançado) | Pula todas as confirmações — só pode ser ativado ao iniciar o Claude Code, não no meio da sessão |

> ⚠️ Como iniciante, fique no modo **Manual** até se sentir confortável lendo
> e entendendo o que cada edição/comando proposto faz. Isso é o equivalente
> a revisar um Pull Request antes de aprovar.

## O que acontece quando ele pede permissão

Você verá uma prévia da ação (o diff do arquivo, ou o comando exato que será
executado no terminal) e três opções típicas:

- **Sim, uma vez** — aprova só essa ação.
- **Sim, sempre para isso** — aprova esse tipo de ação para o resto da sessão
  (ou permanentemente, se você configurar em `settings.json` — assunto do
  nível intermediário).
- **Não** — recusa; você pode explicar por que e pedir uma abordagem
  diferente.

## Boas práticas para quem está começando

1. **Leia o diff antes de aprovar.** É o mesmo hábito de revisar código de
   um colega.
2. **Prefira "sim, uma vez" no início.** Você aprende o padrão de ações que
   o Claude costuma propor antes de liberar automações mais amplas.
3. **Desconfie de comandos destrutivos** (`rm -rf`, `git push --force`,
   alterações em produção) — sempre revise com atenção redobrada, mesmo em
   modos mais automáticos.
4. **Use o modo Plano** para tarefas grandes ou arriscadas: peça para o
   Claude só explorar e propor a abordagem antes de tocar em qualquer
   arquivo.

## Próximo passo

[`04-comandos-e-atalhos.md`](./04-comandos-e-atalhos.md) — os comandos slash
e atalhos de teclado que você vai usar todos os dias.
