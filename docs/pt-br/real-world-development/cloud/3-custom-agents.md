---
title: "Exercício 3 - Agentes personalizados"
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

Com os agentes personalizados, você vai explorar:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys se comprometeu a garantir que todas as pessoas possam usar sua plataforma de financiamento coletivo, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é essencial, você quer implementar esse recurso o mais rápido possível. Para isso, usará um agente personalizado.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Revise seu conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace e examine o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Crie e atribua uma issue

O painel de agentes é o local central para trabalhar com agentes no seu ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorar o trabalho e até redirecionar a sessão ou fornecer orientações adicionais. Vamos atribuir ao Copilot uma tarefa para criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o **Title** como `Add high contrast mode to website`.
6. Defina a **Description** como:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

   ![Captura de tela da atribuição da tarefa ao agente de nuvem, com o agente personalizado e a opção de acessibilidade destacados](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents], assistentes de IA especializados em tarefas e domínios específicos. Com eles, é possível registrar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot para executar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot trabalha na implementação do modo de alto contraste, você pode [monitorar e orientar a sessão do agente][next-lesson] pelo painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar sua organização para usar agentes personalizados][org-custom-agents]
- [Preparar sua empresa para usar agentes personalizados][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
