---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real no seu computador, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo à medida que são feitas. Você pode revisar cada diff na conversa, aceitar ou rejeitar alterações individuais e orientar o chat com prompts complementares para aprimorar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivos propostas.
- orientar o agente durante a sessão para adicionar uma alternância do modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design faz outra solicitação: também quer uma opção de modo claro para as pessoas que consideram o tema padrão escuro demais. Em vez de começar uma conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: os arquivos que ele explora, os comandos que executa e as edições propostas. Cada edição aparece na conversa com um diff e controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução ele solicita.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** no início do chat para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando de terminal, o agente será pausado e exibirá o prompt **Allow**. Aprove somente os comandos com os quais você se sentir confortável.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Você deverá ver uma mensagem final que resume as alterações.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar uma alternância do modo claro. Como continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão usado para o armazenamento local.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt complementar:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente continuará o trabalho explorando as alterações existentes e propondo adições para o modo claro.
3. Revise cada nova edição à medida que aparecer, como fez com o modo de alto contraste. Use **Keep** ou **Undo** para cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface das alternâncias de um jeito que você não prefere — interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem complementar para esclarecer o que deseja.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para solicitar ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione uma terceira alternância; inclua o modo claro no painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para as duas alternâncias").

## Revise o conjunto de alterações

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em quaisquer testes relacionados.
3. Abra alguns dos arquivos alterados e examine os diffs. Confirme se eles seguem os padrões de acessibilidade do agente personalizado: atributos ARIA, navegação pelo teclado, HTML semântico e persistência por meio do armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você explorou:

- monitorar o trabalho de um agente em tempo real.
- aceitar e rejeitar edições individuais.
- orientar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade como um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
