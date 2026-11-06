---
title: "Lição 6 - Validar a funcionalidade com o Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e explore o recurso de filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas programar. É preciso trabalhar com dados, serviços externos e até disponibilizar outras automações para o Copilot. É aí que entram os servidores MCP: eles ampliam os recursos integrados à CLI e fornecem ferramentas e serviços adicionais ao Copilot.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor Playwright MCP, se ele ainda não estiver disponível.
- pedir ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária, automatizando ainda mais as alterações e aumentando a confiança de que elas funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Com o MCP, os agentes podem acessar informações atualizadas e executar ações por você.

Essas ferramentas e recursos são acessados por meio de um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP bastante usados:

- **[GitHub MCP Server][github-mcp]**: oferece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: oferece recursos de automação de navegador usando o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub hospeda um [registro MCP][mcp-registry] para facilitar a descoberta de servidores e a colaboração no ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um deles, examine o código-fonte, verifique quem o publicou e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Em nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Peça ao Copilot para explorar o recurso com o Playwright

Você já confirmou manualmente que o recurso funciona. Agora vamos usar o servidor Playwright MCP disponível no Copilot para fazer o mesmo.

1. Use o prompt a seguir para pedir ao Copilot que valide a funcionalidade com o servidor Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário instruir o Copilot a usar o servidor MCP, pois ele geralmente o identifica por conta própria. Mas, como você sabe qual ferramenta deve ser usada, indicar isso pode ajudar a obter resultados mais consistentes e economizar tokens.

2. Observe as etapas que o Copilot executa no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme se tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, parará o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor Playwright MCP para explorar seu recurso em um navegador real a partir do Copilot CLI. Você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor Playwright MCP, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Na próxima lição, você [criará um agente personalizado de QA][next-lesson] que combinará a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor Playwright MCP da Microsoft][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
