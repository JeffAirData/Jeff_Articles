# ---
title: "Hooks: automação determinística no ciclo de vida do agente"
description: "Uso de hooks para automatizar ações determinísticas no Claude Code."
slug: "/articles/claude-code-guia/03-avancado-senior/01-hooks"
---

# 1. Hooks: automação determinística no ciclo de vida do agente

> Nível: Sênior/Avançado · Pré-requisito: nível Intermediário completo

## Hooks vs. skills: qual a diferença

- **Skills** são baseadas em raciocínio do modelo: o Claude *decide* se e
  como usar uma skill.
- **Hooks** são **comandos de shell determinísticos**, disparados
  automaticamente em pontos específicos do ciclo de vida da sessão — sempre
  rodam, sem depender de "decisão" do modelo.

Use hooks quando o comportamento precisa ser **garantido**, não apenas
sugerido: formatação automática de código, bloqueio de comandos perigosos,
notificações, auditoria.

## Eventos disponíveis (principais)

| Evento | Quando dispara | Uso típico |
|---|---|---|
| `SessionStart` | Início da sessão | Injetar contexto adicional, logging |
| `PreToolUse` | Antes de uma ferramenta ser executada | Bloquear/validar/modificar a ação antes que aconteça |
| `PostToolUse` | Depois que uma ferramenta é executada com sucesso | Formatar código automaticamente, rodar lint, pós-processar resultado |
| `SessionEnd` | Fim da sessão | Relatórios, arquivamento, notificações |

> ℹ️ O conjunto exato de eventos disponíveis pode variar entre versões.
> Confira `/hooks` na sua instalação e a documentação oficial
> (seção "Hooks") para a lista completa e atualizada.

## Formato de configuração

Em `settings.json`:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/format-on-save.sh"
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/bloquear-comandos-perigosos.sh"
          }
        ]
      }
    ]
  }
}
```

O `matcher` filtra a qual ferramenta o hook se aplica (ex.: `Edit`, `Bash`,
ou um padrão mais específico como `Bash(git push*)`).

## Exemplo: bloquear comandos destrutivos

```bash
#!/bin/bash
# ~/.claude/hooks/bloquear-comandos-perigosos.sh
input=$(cat)
cmd=$(echo "$input" | jq -r '.tool_input.command // empty')

if echo "$cmd" | grep -qE 'rm -rf /|git push --force'; then
  echo '{"hookSpecificOutput":{"permissionDecision":"deny","reason":"Comando bloqueado por política de segurança"}}'
  exit 0
fi

echo '{"hookSpecificOutput":{"permissionDecision":"ask"}}'
```

O hook recebe um JSON no `stdin` com detalhes da ação proposta e responde
via `stdout` com a decisão (permitir, negar, ou manter o comportamento
padrão de perguntar).

## Exemplo: formatar código automaticamente após cada edição

```bash
#!/bin/bash
# ~/.claude/hooks/format-on-save.sh
input=$(cat)
file=$(echo "$input" | jq -r '.tool_input.file_path // empty')

if [[ "$file" == *.ts || "$file" == *.tsx ]]; then
  npx prettier --write "$file"
fi
```

## Casos de uso de nível sênior

- **Gate de segurança**: negar automaticamente qualquer tentativa de leitura
  de arquivos de segredo, independentemente do que `settings.json` já
  cobre — uma segunda camada de defesa.
- **Qualidade garantida**: rodar lint/format toda vez que um arquivo é
  editado, sem depender de o Claude "lembrar" de fazer isso.
- **Auditoria e observabilidade**: registrar em log toda ação executada
  numa sessão, para compliance ou debugging posterior.
- **Notificações**: avisar em um canal (Slack, e-mail) quando uma sessão
  longa termina.

## Boas práticas

1. **Hooks devem ser rápidos.** Eles rodam de forma síncrona no fluxo do
   agente — um hook lento trava a experiência.
2. **Trate hooks como código de produção**: teste, versione, revise em PR.
3. **Prefira `deny` explícito a depender só de `settings.json`** para
   regras de segurança críticas — hooks são a camada mais confiável porque
   não dependem de interpretação do modelo.
4. **Documente cada hook no `CLAUDE.md` ou em um `HOOKS.md`** — hooks são
   "mágica invisível" para quem não sabe que existem.

## Próximo passo

[`02-agent-sdk.md`](./02-agent-sdk.md) — construindo seus próprios agentes
com o Claude Agent SDK.
