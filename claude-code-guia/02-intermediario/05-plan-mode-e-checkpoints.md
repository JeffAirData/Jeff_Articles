# 5. Plan mode e checkpoints: planejar antes e voltar atrás quando precisar

> Nível: Intermediário · Pré-requisito: [04-mcp-servers.md](./04-mcp-servers.md)

## Modo de planejamento (Plan mode)

Para tarefas grandes ou arriscadas, vale a pena separar "pensar" de "agir".
No **modo Plano** (alterne com `Shift+Tab` ou inicie com
`claude --permission-mode plan`), o Claude explora o código e propõe uma
abordagem **sem editar nenhum arquivo**.

Fluxo recomendado:

1. Entre em modo Plano.
2. Descreva a tarefa.
3. Revise o plano proposto — questione decisões, peça alternativas.
4. Aprove o plano.
5. O Claude sai do modo Plano e implementa o que foi combinado.

> 💡 Isso equivale a revisar o design de uma mudança antes de escrever o
> código — evita retrabalho em tarefas complexas.

## Controlando o "esforço de raciocínio"

Para tarefas que exigem mais raciocínio (arquitetura, debugging difícil,
refatorações amplas), você pode aumentar o quanto o modelo "pensa" antes de
responder:

```
/model
```

permite escolher o modelo e, dependendo da versão, o nível de esforço
associado. Como padrão geral: níveis mais altos de esforço custam mais
tokens e demoram mais, mas tendem a produzir respostas mais bem
raciocinadas em problemas difíceis — reserve para tarefas que realmente
precisam.

## Checkpoints e "rewind" (voltar no tempo)

Antes de cada novo pedido seu, o Claude Code tira um retrato (snapshot) do
estado dos arquivos. Se uma mudança não saiu como esperado, você pode
voltar atrás:

- Pressione **Esc duas vezes** (com o campo de mensagem vazio) para abrir o
  menu de rewind.
- Escolha o que restaurar:
  - **Código e conversa** (desfaz tudo)
  - **Só a conversa** (mantém o código como está)
  - **Só o código** (mantém o histórico da conversa)

### Limitações importantes

- Mudanças feitas **por comandos de terminal** (ex.: um script que apaga
  arquivos) **não** são capturadas pelos checkpoints — só edições diretas
  de arquivo pelo Claude são rastreadas.
- Alterações feitas por **você manualmente**, fora do Claude Code, também
  não entram no checkpoint.
- Edições feitas por **subagentes em segundo plano** não aparecem no
  histórico de rewind da sessão principal.

> 🛟 Checkpoints são uma rede de segurança extra, **não um substituto do
> Git**. Continue commitando seu trabalho com frequência — é a forma mais
> confiável de conseguir voltar atrás.

## Próximo passo

[`06-fluxo-git-e-github.md`](./06-fluxo-git-e-github.md) — como o Claude
Code trabalha com Git, GitHub, issues e Pull Requests.
