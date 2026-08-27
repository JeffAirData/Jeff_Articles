# Glossário

| Termo | Significado |
|---|---|
| **Claude Code** | Agente de codificação do Claude — lê, edita e executa código conversando em linguagem natural, disponível em CLI, Desktop, web e IDEs |
| **Sessão** | Uma conversa/execução do Claude Code, com contexto próprio |
| **CLAUDE.md** | Arquivo markdown com contexto do projeto, lido automaticamente no início de cada sessão |
| **Skill** | Pacote de instruções reutilizável, carregado sob demanda (manual ou automaticamente), evolução dos antigos "comandos customizados" |
| **Subagente** | Assistente especializado que roda em contexto isolado da sessão principal, útil para delegar tarefas |
| **MCP (Model Context Protocol)** | Padrão aberto para conectar o Claude a ferramentas e serviços externos (bancos de dados, GitHub, Slack etc.) |
| **Hook** | Comando de shell disparado automaticamente em pontos do ciclo de vida da sessão (ex.: antes/depois de uma ferramenta rodar) — determinístico, não depende de "decisão" do modelo |
| **settings.json** | Arquivo de configuração de permissões, variáveis de ambiente e preferências, em diferentes escopos (usuário, projeto, local, gerenciado) |
| **Permissões (allow/deny/ask)** | Sistema que controla quais ações o Claude pode executar automaticamente, quais são bloqueadas, e quais exigem aprovação manual |
| **Modo Plano (Plan mode)** | Modo em que o Claude explora e propõe uma abordagem sem editar arquivos, até você aprovar |
| **Checkpoint / Rewind** | Sistema de retrato automático do estado dos arquivos antes de cada pedido, permitindo voltar atrás |
| **Modo headless** | Execução não interativa (flag `-p`), usada em scripts e pipelines de CI/CD |
| **Claude Agent SDK** | Biblioteca (Python/TypeScript) para construir aplicações próprias usando o mesmo motor de agente do Claude Code |
| **Worktree** | Diretório de trabalho isolado do Git, usado para rodar tarefas paralelas em branches diferentes sem interferência |
| **Contexto (context window)** | Quantidade de informação (código, conversa, arquivos) que o modelo consegue "ver" de uma vez em uma sessão |

> Encontrou um termo que faltou? Abra uma
> [issue](../../../../issues) ou envie um PR — veja
> [`CONTRIBUTING.md`](../../../../CONTRIBUTING.md).
