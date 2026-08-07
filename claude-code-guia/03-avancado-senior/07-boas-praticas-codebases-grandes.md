# 7. Boas práticas para bases de código grandes

> Nível: Sênior/Avançado · Pré-requisito: [06-output-styles-custos-e-monitoramento.md](./06-output-styles-custos-e-monitoramento.md)

Este capítulo fecha o compêndio consolidando, em forma de referência
rápida, os padrões que fazem diferença em projetos grandes e times maduros.

## Gestão de contexto

| Estratégia | Benefício |
|---|---|
| Iniciar a sessão a partir da subpasta relevante, não da raiz do monorepo | Carrega só o `CLAUDE.md` pertinente, não o de todos os pacotes |
| `CLAUDE.md` em camadas (raiz + por pacote) | Regras gerais uma vez só; especificidades onde fazem sentido |
| Mover procedimentos longos para skills em vez de inflar o `CLAUDE.md` | Conteúdo só entra em contexto quando é usado |
| Bloquear leitura de pastas geradas/pesadas (`node_modules`, `dist`, `.env`) | Evita desperdício de contexto e exposição de segredos |
| Delegar trabalho verboso (logs, buscas extensas) a subagentes | Mantém a sessão principal enxuta |
| Rodar `/context` periodicamente | Mostra visualmente o que está consumindo o espaço disponível |

## Padrões para monorepos

**Iniciar pelo pacote, não pela raiz:**
```bash
cd packages/api
claude
```

**Liberar acesso a pacotes irmãos, quando necessário:**
```bash
claude --add-dir ../shared --add-dir ../web
```
ou de forma persistente em `settings.json`:
```json
{
  "permissions": {
    "additionalDirectories": ["../shared", "../web"]
  }
}
```

## Erros comuns e como evitá-los

| Armadilha | Como evitar |
|---|---|
| Um `CLAUDE.md` gigante e genérico | Dividir por pasta + extrair procedimentos para skills |
| Sessões que arrastam contexto obsoleto de tarefas anteriores | `/clear` ao trocar de assunto |
| Usar sempre o modelo mais caro "por garantia" | Reservar modelos/esforço alto para tarefas que exigem; medir antes de assumir |
| Confiar só nos checkpoints do Claude Code como backup | Continuar commitando no Git com frequência — checkpoints têm limitações (não cobrem mudanças via shell, por exemplo) |
| Deixar servidores MCP não utilizados conectados | Revisar `/mcp` periodicamente e remover o que não é usado |
| Pedir tarefas vagas ("melhore o código") | Ser específico: arquivo, função, critério de sucesso, casos de teste |
| Pular a etapa de plano em mudanças grandes | Usar o modo Plano para revisar a abordagem antes de qualquer edição |

## O hábito que mais compensa: pedidos específicos e verificáveis

```
❌ "Melhore a validação de login."

✅ "Adicione validação de e-mail na função login() em src/auth.ts.
   Deve rejeitar: '', 'user@', e aceitar: 'user@example.com'.
   Escreva um teste cobrindo os três casos antes de considerar concluído."
```

Pedidos específicos, com critério de sucesso claro, permitem que o próprio
Claude verifique o trabalho antes de devolver a resposta — reduzindo ciclos
de correção.

## Checklist final de maturidade

Um time que está usando o Claude Code em nível sênior normalmente já tem:

- [ ] `CLAUDE.md` em camadas, revisado como código
- [ ] `settings.json` de projeto com `allow`/`deny` explícitos, versionado
- [ ] Skills documentando os procedimentos recorrentes do time
- [ ] Hooks garantindo formatação/lint e bloqueando comandos perigosos
- [ ] Pipeline de CI com Claude Code em modo headless, custo monitorado
- [ ] Convenção clara de quando usar subagentes vs. sessões paralelas
- [ ] Rotina de revisão de custo (`/usage`) em tarefas recorrentes
- [ ] Configurações gerenciadas de segurança no nível da organização

## Você concluiu o compêndio 🎓

Do primeiro `claude` no terminal até pipelines de CI com múltiplos agentes
— o caminho é o mesmo de qualquer boa prática de engenharia: comece
simples, adicione automação conforme a necessidade real aparece, e sempre
verifique o trabalho antes de confiar nele.

> 📚 Volte a [`recursos/links-oficiais.md`](../recursos/links-oficiais.md)
> sempre que precisar confirmar um detalhe contra a documentação oficial —
> o produto evolui rápido, e "confira a fonte" é o hábito mais sênior de
> todos.
