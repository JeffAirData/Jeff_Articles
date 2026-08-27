---
title: "Comandos slash essenciais e atalhos de teclado"
description: "Lista de comandos e atalhos essenciais do Claude Code."
slug: "/articles/claude-code-guia/01-iniciante/04-comandos-e-atalhos"
---

# 4. Comandos slash essenciais e atalhos de teclado

> Nível: Iniciante · Pré-requisito: [03-permissoes-basicas.md](./03-permissoes-basicas.md)

> ℹ️ O Claude Code recebe novos comandos com frequência. A lista abaixo
> cobre os comandos **essenciais e estáveis** para o dia a dia. Para ver a
> lista completa e atualizada na sua versão instalada, rode `/help` a
> qualquer momento dentro de uma sessão.

## Comandos de sessão

| Comando | O que faz |
|---|---|
| `/help` | Mostra a ajuda e a lista de comandos disponíveis na sua versão |
| `/init` | Gera um `CLAUDE.md` inicial analisando o projeto |
| `/clear` | Encerra a conversa atual e começa uma nova, com contexto limpo |
| `/resume` | Abre um seletor para retomar uma sessão anterior |
| `/compact` | Resume a conversa atual para liberar espaço de contexto |
| `/export` | Exporta a conversa atual como texto |
| `/exit` | Sai do Claude Code |

## Configuração

| Comando | O que faz |
|---|---|
| `/model` | Troca o modelo usado (ex.: Sonnet, Opus) e permite salvar como padrão |
| `/config` | Abre as configurações (tema, editor, atalhos, estilo de saída) |
| `/memory` | Abre o `CLAUDE.md` e a memória automática para edição |
| `/login` / `/logout` | Entra ou sai da sua conta |
| `/add-dir <caminho>` | Libera acesso a uma pasta fora do diretório atual, na sessão |
| `/ide` | Gerencia integração com sua IDE (VS Code, JetBrains) |
| `/doctor` | Roda um diagnóstico de configuração e aponta problemas comuns |

## Qualidade e revisão

| Comando | O que faz |
|---|---|
| `/review` (ou `/code-review`) | Pede uma revisão do diff atual, apontando bugs e melhorias |

## Custos e uso

| Comando | O que faz |
|---|---|
| `/usage` (ou `/cost`) | Mostra tokens consumidos e custo estimado da sessão |

## Integrações

| Comando | O que faz |
|---|---|
| `/agents` | Cria e gerencia subagentes (nível intermediário) |
| `/mcp` | Gerencia conexões com servidores MCP (nível intermediário) |

## Atalhos de teclado

| Atalho | Ação |
|---|---|
| `Ctrl+C` | Cancela/interrompe a ação atual |
| `Ctrl+D` (2x) | Sai do Claude Code |
| `Esc` | Cancela a entrada / fecha um menu |
| `Esc` (2x, com campo vazio) | Abre o menu de "voltar no tempo" (rewind) |
| `Shift+Tab` | Alterna entre os modos de permissão |
| `Ctrl+L` | Limpa/redesenha a tela |
| `Ctrl+R` | Busca no histórico de comandos |
| `Enter` | Envia a mensagem |
| `↑` / `↓` | Navega pelo histórico de mensagens |
| `Tab` | Aceita sugestão de autocompletar |

> 💡 O conjunto exato de atalhos pode ser conferido e customizado com
> `/keybindings`, que abre o arquivo de configuração de atalhos.

## Praticando

Experimente agora, em um projeto de teste:

1. `claude` para iniciar
2. `/init` para gerar um `CLAUDE.md`
3. Peça uma pequena mudança de código
4. `/review` para o Claude revisar a própria mudança
5. `/usage` para ver quanto custou
6. `/clear` para começar uma tarefa nova do zero

## Você concluiu o nível Iniciante 🎉

Próximo passo: [`02-intermediario/`](../02-intermediario/) — comandos
customizados, skills, subagentes, MCP e o fluxo completo com Git/GitHub.
