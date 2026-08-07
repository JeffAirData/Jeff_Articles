# 4. Orquestração multi-agente

> Nível: Sênior/Avançado · Pré-requisito: [03-modo-headless-web-e-cicd.md](./03-modo-headless-web-e-cicd.md)

## Três padrões de paralelismo

À medida que os projetos crescem, um único fluxo conversacional deixa de
ser suficiente. Existem, hoje, três formas de paralelizar o trabalho com
o Claude Code:

### 1. Subagentes (dentro de uma sessão)

Já vistos no nível intermediário: um subagente cuida de uma subtarefa
isolada (pesquisa, análise de logs) e devolve um resumo. Bom para tarefas
que **não** vão gerar edições de código conflitantes entre si.

### 2. Sessões em segundo plano (background)

Uma tarefa pode ser delegada para rodar em segundo plano enquanto você
continua conversando na sessão principal:

```
/background Refatore o módulo de logging para usar o novo formato
```

Acompanhe com:

```
/tasks
```

### 3. Múltiplas sessões na nuvem, em paralelo

Para tarefas totalmente independentes (que não tocam nos mesmos arquivos),
o padrão mais robusto é abrir **sessões separadas na nuvem**:

```bash
claude --cloud "Corrija o teste instável em auth.spec.ts"
claude --cloud "Atualize a documentação da API"
claude --cloud "Refatore o módulo de logger"
```

Cada uma roda isolada, com seu próprio contexto e custo. Monitore todas
com `/tasks`.

## Worktrees: isolamento de branches em paralelo

Quando duas tarefas paralelas *podem* tocar arquivos parecidos, mas você
quer evitar qualquer interferência entre elas, use **worktrees** do Git —
diretórios de trabalho separados para branches diferentes do mesmo
repositório:

```bash
claude --worktree
```

Cada worktree tem seu próprio estado de arquivos e Git, mas compartilha o
histórico do repositório. Isso permite ter, por exemplo, uma sessão
trabalhando em `feature/checkout` e outra em `feature/relatorios`, sem uma
pisar no diretório de trabalho da outra.

## Equipes de agentes (recurso experimental)

Algumas versões do Claude Code oferecem um modo **experimental** de "agent
teams" — múltiplos agentes conversando entre si, coordenados em uma tarefa
maior, ativado por uma variável de ambiente específica. Por ser
experimental, comportamento e disponibilidade podem mudar sem aviso, e o
**custo em tokens é significativamente mais alto** (cada agente da equipe
mantém seu próprio contexto). Antes de adotar em um fluxo real:

- Confirme a disponibilidade e o nome exato da flag na documentação oficial
  atual do Claude Code (seção sobre múltiplos agentes/equipes).
- Rode um piloto pequeno e meça custo antes de escalar.

## Quando vale a pena paralelizar

| Cenário | Padrão recomendado |
|---|---|
| Investigação que não edita código | Subagente |
| Uma tarefa longa, você quer continuar trabalhando em outra coisa | Sessão em background |
| Tarefas totalmente independentes, sem sobreposição de arquivos | Sessões paralelas na nuvem |
| Tarefas que podem tocar arquivos parecidos, mas branches diferentes | Worktrees |
| Coordenação complexa entre múltiplos agentes na mesma tarefa | Agent teams (experimental — avalie custo/benefício) |

## Próximo passo

[`05-enterprise-seguranca-e-times.md`](./05-enterprise-seguranca-e-times.md) —
padrões de segurança e configuração para times e organizações.
