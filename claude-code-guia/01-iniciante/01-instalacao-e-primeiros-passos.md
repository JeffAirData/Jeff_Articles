# 1. Instalação e primeiros passos

> Nível: Iniciante · Pré-requisito: nenhum

## O que é o Claude Code

Claude Code é o "modo agente" de codificação do Claude: em vez de só responder
perguntas, ele lê o seu projeto, edita arquivos, executa comandos no terminal
e verifica o próprio trabalho — tudo isso conversando com você em linguagem
natural. Está disponível em quatro formatos:

- **Terminal (CLI)** — o jeito mais completo e flexível de usar.
- **Aplicativo Desktop** (Mac/Windows) — interface gráfica sobre o mesmo motor.
- **Web** (`claude.ai/code`) — roda em uma máquina na nuvem da Anthropic.
- **Extensões de IDE** (VS Code, JetBrains) — direto no seu editor.

O loop de trabalho é sempre o mesmo: **entender o contexto → agir (editar
código, rodar comando) → verificar o resultado**, repetindo até concluir a
tarefa.

## Instalando

Escolha o método conforme seu sistema operacional:

| Plataforma | Comando |
|---|---|
| macOS / Linux / WSL | `curl -fsSL https://claude.ai/install.sh \| bash` |
| Windows (PowerShell) | `irm https://claude.ai/install.ps1 \| iex` |
| macOS (Homebrew) | `brew install --cask claude-code` |
| Windows (WinGet) | `winget install Anthropic.ClaudeCode` |
| Qualquer SO com Node.js | `npm install -g @anthropic-ai/claude-code` |

> 💡 Os instaladores nativos (`.sh`/`.ps1`) se atualizam sozinhos. Instalações
> via `brew`, `winget` ou `npm` exigem atualização manual
> (`npm install -g @anthropic-ai/claude-code@latest`, por exemplo).

Depois de instalar, confirme que funcionou:

```bash
claude --version
```

## Primeiro login

1. Entre na pasta do seu projeto pelo terminal.
2. Rode:
   ```bash
   claude
   ```
3. Na primeira execução, o Claude Code abre o navegador para você fazer login
   (conta Claude.ai Pro/Max, Team/Enterprise, ou uma chave de API do Console).
4. Após autenticar, a sessão começa direto no terminal.

As credenciais ficam guardadas com segurança no seu sistema (Keychain no
macOS; arquivo com permissão restrita no Linux/Windows) — você não precisa
logar de novo a cada sessão.

## Como o Claude Code "enxerga" seu projeto

Quando você inicia uma sessão, o Claude tem acesso a:

- Os arquivos do diretório de trabalho atual (e subpastas).
- O estado e histórico do Git do projeto.
- O arquivo `CLAUDE.md`, se existir (ver próximo capítulo).
- Comandos, skills, subagentes e servidores MCP configurados no projeto.

Ele **não** tem acesso automático a pastas fora do diretório onde você
iniciou a sessão, a menos que você libere isso explicitamente (com
`/add-dir`, por exemplo — veremos nos comandos).

## Sessões: o que são e como retomar

Cada conversa é uma **sessão**, salva localmente no seu computador. Sessões
são independentes: uma nova sessão começa com contexto zerado.

```bash
claude --continue   # retoma a sessão mais recente
claude --resume      # abre um seletor para escolher qual sessão retomar
```

## Seu primeiro pedido

Dentro de uma sessão, basta escrever em linguagem natural o que você quer:

```
Crie um script em Python que leia um CSV de vendas e mostre o total por mês.
```

O Claude vai propor um plano, criar o arquivo, e (dependendo do modo de
permissão ativo — explicado no capítulo sobre permissões) pedir sua
aprovação antes de escrever no disco ou rodar comandos.

## Próximos passos

- [`02-claude-md-e-contexto.md`](./02-claude-md-e-contexto.md) — como dar
  contexto persistente sobre seu projeto.
- [`03-comandos-e-atalhos.md`](./03-comandos-e-atalhos.md) — comandos
  essenciais e atalhos de teclado.

> 📚 Fonte oficial: [code.claude.com/docs](https://code.claude.com/docs) —
> seções "Overview", "Setup" e "Authentication". Como o Claude Code recebe
> atualizações com frequência, confira sempre a documentação oficial para
> detalhes de versão mais recentes.
