---
title: "Claude Agent SDK: construindo seus próprios agentes"
description: "Introdução ao Agent SDK para construir agentes programáticos com o motor do Claude."
slug: "/articles/claude-code-guia/03-avancado-senior/02-agent-sdk"
---

# 2. Claude Agent SDK: construindo seus próprios agentes

> Nível: Sênior/Avançado · Pré-requisito: [01-hooks.md](./01-hooks.md)

> ⚠️ Nomes exatos de pacotes e assinaturas de API do SDK evoluem com
> frequência. Os exemplos abaixo mostram os **conceitos e o formato geral**
> — confirme nome do pacote e API atual em
> [docs.claude.com](https://docs.claude.com) (seção "Agent SDK") antes de
> usar em produção.

## O que é, e quando usar

O **Claude Agent SDK** dá acesso, como biblioteca (Python ou TypeScript), ao
mesmo "motor" de agente que roda o Claude Code: as ferramentas embutidas
(ler/editar arquivos, rodar shell, buscar na web), o sistema de hooks,
subagentes, MCP e permissões — mas embutido na **sua própria aplicação**,
não numa interface interativa de terminal.

Use o SDK quando você quer:

- Construir uma automação ou produto próprio que usa um agente de código
  por baixo dos panos.
- Rodar agentes de forma programática, integrados a um pipeline (não um
  humano digitando no terminal).
- Definir ferramentas customizadas específicas do seu domínio.

Se você só quer usar o Claude Code interativamente ou em CI, o **modo
headless da própria CLI** (próximo capítulo) costuma ser suficiente e mais
simples que integrar o SDK diretamente.

## Conceito básico (Python)

```python
from claude_agent_sdk import Agent  # confira o nome exato do pacote na doc oficial

agent = Agent(
    model="claude-sonnet-5",
    api_key="sua-api-key",
)

result = agent.run(
    prompt="Corrija o teste que está falhando em src/auth.test.ts",
    cwd="/caminho/do/projeto",
)

print(result.final_response)
print(result.total_cost)
```

## Conceito básico (TypeScript)

```typescript
import { Agent } from "@anthropic-ai/claude-agent-sdk"; // confira o nome exato do pacote

const agent = new Agent({
  model: "claude-sonnet-5",
  apiKey: process.env.ANTHROPIC_API_KEY,
});

const result = await agent.run({
  prompt: "Adicione tratamento de erro no fluxo de login",
  cwd: "/caminho/do/projeto",
});

console.log(result.finalResponse);
console.log(result.totalCost);
```

## Recursos disponíveis via SDK

- **Ferramentas embutidas**: leitura/escrita de arquivo, busca, shell, web.
- **Ferramentas customizadas**: você define funções próprias que o agente
  pode chamar (ex.: enviar um e-mail, consultar uma API interna).
- **Hooks**: mesmos conceitos do capítulo anterior, agora programáveis.
- **Subagentes e MCP**: os mesmos mecanismos do Claude Code interativo.
- **Sessões**: manter e retomar contexto entre chamadas.
- **Saída estruturada**: pedir que o resultado siga um schema definido
  (útil para integrar a resposta do agente com o resto do seu sistema).

## Exemplo: ferramenta customizada

```python
from claude_agent_sdk import Tool

class NotificarSlack(Tool):
    def execute(self, canal: str, mensagem: str):
        # sua integração real com a API do Slack aqui
        return {"status": "enviado"}

agent.add_tool(NotificarSlack())
```

## Exemplo: saída estruturada

```python
from pydantic import BaseModel

class RelatorioBug(BaseModel):
    severidade: str
    descricao: str
    sugestao_de_correcao: str

result = agent.run(
    prompt="Analise o código e liste os bugs encontrados",
    response_schema=RelatorioBug,
)
```

## Monitorando custo programaticamente

```python
result = agent.run(prompt)
print(f"Tokens de entrada: {result.usage.input_tokens}")
print(f"Tokens de saída: {result.usage.output_tokens}")
print(f"Custo: ${result.total_cost:.4f}")
```

Isso é essencial quando o agente roda **sem supervisão humana direta** —
você precisa de visibilidade programática de custo para não ter surpresas.

## Diferença entre Agent SDK e "Managed Agents"

Além do SDK (que você hospeda e opera), a Anthropic também oferece um
produto de agentes hospedados por eles ("Managed Agents"). A escolha entre
um e outro é arquitetural: SDK dá controle total sobre onde e como o agente
roda; a opção gerenciada tira de você a responsabilidade operacional de
hospedagem. Avalie conforme a necessidade de controle vs. simplicidade
operacional do seu projeto.

## Próximo passo

[`03-modo-headless-web-e-cicd.md`](./03-modo-headless-web-e-cicd.md) —
rodando o Claude Code sem interação humana, em pipelines e na nuvem.
