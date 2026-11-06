---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples de um aplicativo costuma exigir alterações em vários arquivos e operações como executar testes. Em geral, a pessoa desenvolvedora precisa localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização... A lista continua.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para atuar de forma mais autônoma na IDE. Ele funciona de maneira semelhante a uma pessoa desenvolvedora: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Apesar dos nomes parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas na IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou um auxiliar para editoras; agora, concluirá o restante do trabalho na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Execute o site da Tailspin Toys

Antes de fazer alterações, vamos explorar o site da Tailspin Toys para entender as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento de software. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro, que renderiza as páginas como HTML estático no momento da compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM — não há uma API de backend separada nem um framework de interface executado no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` do Astro, com escopo limitado e usando APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, o projeto inclui a tarefa `npm run dev`, que inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespace seguindo estas etapas:

1. Volte ao Codespace. Você deverá estar na `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela para trabalhar na filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá um aviso com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar que você abra uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Para abrir o site, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link `localhost` no terminal redirecionará automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor Web.

### Explore o site

Depois que o site estiver em execução, explore suas funcionalidades. Entre os principais recursos:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: quando você seleciona um jogo, abre uma página com mais informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício — as etapas indicarão quando for relevante.

O site já funciona, mas queremos aprimorá-lo com novos recursos. Vamos começar examinando o backlog. Ao configurar o projeto a partir do modelo, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot para mostrar esses itens.

1. Na lista suspensa de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos no terminal e chama ferramentas.

   ![Captura de tela mostrando o seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> As pessoas autoras deste workshop não estão recomendando um modelo específico. Na criação do workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções na esperança de que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os LLMs são probabilísticos, as sugestões podem ser diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot talvez use um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Envie o prompt a seguir ao Copilot para perguntar sobre o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revise os arquivos de instruções

Antes de pedir ao agente para gerar código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar a implementação de funcionalidades no site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de criação de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos de tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso permite reutilizar conteúdo, dividir instruções complexas em partes menores e mais gerenciáveis e fornecer exemplos e modelos.

## Implemente a filtragem

Para concluir a filtragem, será necessário fazer pelo menos três atualizações no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes da mesclagem. O modo de agente do Copilot pode realizar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue na conversa atual do Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela mostrando como selecionar o arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado na lista suspensa de agentes da exibição Chat.

   ![Captura de tela mostrando o seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot para implementar a funcionalidade com base na issue correspondente do backlog, usando o prompt a seguir:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, incluindo os auxiliares da camada de dados, a interface e os testes. Em seguida, ele começará a alterar arquivos e executar os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e executar testes. Dependendo da complexidade da tarefa e da base de código, isso poderá levar alguns minutos. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, haverá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecioná-las enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas e alguns testes falhem durante o processo. Não há problema: o Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, aprendemos a usar o modo de agente do GitHub Copilot para adicionar funcionalidades ao site da Tailspin Toys. Você aprendeu como:

- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados.
- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

Agora, vamos [testar o recurso com o servidor Playwright MCP][next-lesson] e abrir um pull request.

### Exercício extra — implemente a paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use o que aprendeu neste exercício e peça ao Copilot para atualizar o site. Algumas considerações:

- siga as práticas recomendadas existentes, inclusive as dos arquivos de instruções.
- decida como implementar a paginação: se as pessoas poderão escolher o tamanho da página ou se ele será fixo.
- ao criar o prompt, forneça as orientações necessárias para obter a implementação desejada.
- talvez seja necessário iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Copilot nos modos ask, edit e agent: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
