---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer ao Copilot instruções focadas, práticas recomendadas, padrões de código e conhecimentos específicos do domínio para ajudá-lo a executar determinados tipos de trabalho com mais eficiência. As equipes podem registrar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguidos de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Comparação entre agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar operações. A distinção mais simples é: um **agente personalizado** é quem executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e são criados para orquestrar skills (e até outros agentes) como parte do trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade; como parte do trabalho, ele pode chamar skills como uma skill de fluxo de pull requests ou outra que execute e gerencie testes.

> [!NOTE]
> Não existe uma única forma correta de criar um agente personalizado. Como em qualquer trabalho com IA, teste e itere para encontrar o que funciona em cada ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys se comprometeu a garantir que todas as pessoas possam usar sua plataforma de financiamento coletivo, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é essencial, você quer implementar esse recurso o mais rápido possível. Para isso, usará um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Revise seu conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace, abra um terminal e mude para uma branch nova baseada na `main` para trabalhar nos recursos de acessibilidade (assim, o pull request de filtragem do Exercício 3 continuará separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` na lista suspensa de agentes, na parte inferior da exibição Copilot Chat. Você pode selecionar um agente personalizado para limitar a sessão de chat às instruções e ferramentas desse agente.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício — as etapas indicarão quando for relevante.

1. Na lista suspensa de agentes da exibição Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Captura de tela mostrando o seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione a lista suspensa de agentes na parte inferior da exibição Chat. Ela mostra o agente ativo; por padrão, **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o prompt a seguir ao agente de acessibilidade:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat começará a trabalhar: explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para você revisar.
6. Conforme as edições forem aplicadas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos alterados pelo agente.

> [!NOTE]
> O processo provavelmente levará alguns minutos. O Copilot fará alterações reais no repositório: editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você revisará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com eles, é possível registrar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot para executar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Na próxima lição, você [monitorará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que são feitas e adicionando uma alternância para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados na IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o servidor Playwright MCP][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
