---
title: "Enterprise, segurança e configuração para times"
description: "Padrões de segurança e configuração para uso corporativo do Claude Code."
slug: "/articles/claude-code-guia/03-avancado-senior/05-enterprise-seguranca-e-times"
---

# 5. Enterprise, segurança e configuração para times

> Nível: Sênior/Avançado · Pré-requisito: [04-orquestracao-multi-agente.md](./04-orquestracao-multi-agente.md)

## CLAUDE.md em escala (monorepos e organizações grandes)

Em bases de código grandes, um único `CLAUDE.md` na raiz não escala. O
padrão recomendado é hierárquico:

```
monorepo/
  CLAUDE.md                 # regras gerais do repositório
  packages/
    api/
      CLAUDE.md              # convenções específicas da API
      .claude/skills/        # skills escopadas a esse pacote
    web/
      CLAUDE.md               # convenções do frontend
      .claude/skills/
```

O Claude carrega o `CLAUDE.md` da pasta pai **mais** o da pasta onde a
sessão foi iniciada. Para excluir pastas irrelevantes do carregamento
automático (ex.: código legado ou gerado), use algo como:

```json
{
  "claudeMdExcludes": [
    "**/packages/legacy/**"
  ]
}
```

## Configurações gerenciadas (organização)

Administradores podem impor políticas que **nenhum** `settings.json` de
usuário ou projeto consegue sobrescrever — normalmente distribuídas via
sistema de gestão de configuração da organização (MDM, política de grupo)
ou um arquivo de configurações gerenciadas no ambiente corporativo:

```json
{
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Read(./**/.env*)"
    ]
  },
  "allowManagedPermissionRulesOnly": true
}
```

Isso garante uma base mínima de segurança consistente para toda a
organização, independentemente do que cada desenvolvedor configura
localmente.

## Escolhendo o modo de permissão certo por contexto

| Contexto | Modo recomendado |
|---|---|
| Desenvolvimento exploratório, aprendendo o código | Manual |
| Trabalho do dia a dia, já confiando no fluxo | Aceitar edições |
| Tarefa grande/arriscada, quer revisar antes de agir | Plano |
| Pipeline de CI, ambiente isolado e efêmero | Bypass (com muita cautela — veja capítulo anterior) |

## Sandboxing

Em ambientes que suportam, é possível restringir o que o agente alcança
mesmo além das regras de permissão — por exemplo, desabilitando acesso de
rede por padrão:

```json
{
  "sandbox": {
    "enabled": true,
    "network": "disabled"
  }
}
```

Sessões na nuvem, por padrão, já rodam em máquinas isoladas — e credenciais
sensíveis não ficam diretamente expostas dentro do ambiente de execução do
agente.

## Checklist de segurança para adoção em time/empresa

1. **Nunca versione segredos** em `settings.json` de projeto — use
   `settings.local.json` (fora do Git) ou variáveis de ambiente/gerenciador
   de segredos.
2. **Liste explicitamente o que é proibido** (`deny`) além do que é
   permitido — não confie só em "o que não está liberado, não roda":
   documente as proibições ativamente.
3. **Combine hooks + permissões** para regras críticas — hooks são
   determinísticos e não dependem de o modelo "lembrar" da política.
4. **Configure limites de gasto** por usuário/organização nas configurações
   administrativas da sua conta Claude for Teams/Enterprise.
5. **Revise `CLAUDE.md` e skills de projeto em Pull Request**, como
   qualquer outro código — eles influenciam diretamente o comportamento do
   agente em todo o time.
6. **Trate credenciais de CI como as mais sensíveis do pipeline**: escopo
   mínimo, rotação periódica, nunca em texto plano no workflow.

## Recursos administrativos disponíveis (times/empresa)

- Métricas de uso e adoção por usuário.
- Limites de gasto configuráveis.
- Login único (SSO) e captura de domínio corporativo.
- Políticas de retenção e auditoria (planos Enterprise).

> 📚 Detalhes exatos de cada recurso administrativo variam por plano
> (Team vs. Enterprise) e mudam com o tempo — confirme na documentação
> oficial e no painel administrativo da sua organização.

## Próximo passo

[`06-output-styles-custos-e-monitoramento.md`](./06-output-styles-custos-e-monitoramento.md) —
customizando a saída e monitorando custo/tokens de perto.
