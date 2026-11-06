---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações — se é que alguma — não tenham problemas com dívida técnica. Ela pode envolver problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, o agente cria um pull request de rascunho para que uma pessoa desenvolvedora o revise. Isso permite delegar tarefas tediosas, acelerar o desenvolvimento e liberar a equipe para se concentrar em objetivos mais amplos.

Neste exercício, você vai explorar como usar o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues bem definidas.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeira etapa, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer aprimorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo exiba jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar explorando. Ainda não é necessário que a implementação esteja pronta para produção; por enquanto, basta que possa ser usada em testes de aceitação da experiência. O trabalho está bloqueado, mas há outras issues que são prioritárias neste momento.

Esses são exemplos de tarefas que podem ser adiadas com facilidade e são boas candidatas para atribuição ao agente de nuvem do Copilot. O agente trabalha nelas de forma assíncrona, permitindo que a pessoa desenvolvedora se concentre em outras atividades e, depois, revise o trabalho para confirmar se está correto.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Assim como no trabalho com outras pessoas desenvolvedoras, há várias maneiras de atribuir tarefas, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que recebe uma tarefa, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão do Copilot enquanto o trabalho está em andamento ou depois de concluído. Quando a solução proposta estiver pronta para revisão, o agente mencionará você no pull request.

## A importância de instruções bem definidas

Não há mágica no GitHub Copilot — não é possível deixar de pensar no problema. Mesmo operações aparentemente simples ficam complexas quando examinadas de perto; por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação baseada em IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já concluiu um exercício prático sobre instruções personalizadas, viu o arquivo `.github/copilot-instructions.md` em ação; se não, aproveite para examiná-lo. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções incluídos neste repositório sem fazer alterações. Assim, você poderá observar seus efeitos mais tarde.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — Examine a seção **Code standards**, principalmente as expectativas para convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — Observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe se o pull request resultante contém comentários TSDoc, segue as convenções de TypeScript e inclui cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

Todo trabalho de programação costuma exigir um ambiente específico e alguns scripts de configuração para deixar tudo pronto. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de forma semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, para instalar as ferramentas e dependências necessárias antes de o agente começar. Esse workflow já foi configurado para facilitar o fluxo do laboratório e permitir que você explore esse recurso. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos navegadores do Playwright para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido usado pelo aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para as etapas. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Se as etapas de configuração precisarem clonar o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Se o repositório não for clonado nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend e do aplicativo: Node.js (o aplicativo agora é todo baseado em Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente pela guia **Actions** do repositório. Isso é útil para verificar se o workflow funciona sem esperar que o Copilot o execute.

## Adicione documentação

Embora todas as pessoas reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ficar sem solução, o que reduz a produtividade e dificulta a manutenção da base de código ou a integração de novas pessoas desenvolvedoras. Felizmente, o Copilot é muito bom em criar documentação, então esta é uma issue ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para produzir a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se não aparecer um novo PR na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role até a linha do tempo do pull request. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe a sessão em andamento e acompanhe como o Copilot resolve o problema. Ele explora o código e o contexto, interrompe o trabalho para refletir e escolher uma abordagem e, então, começa a gerar código.

O processo pode levar alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona para que você possa se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, voltar a atenção para o desenvolvimento de uma funcionalidade.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Talvez a seção de jogos relacionados solicitada pela equipe de design não leve muito tempo para ser criada, mas esse tempo poderia ser usado em outras atividades. Vamos atribuir essa tarefa ao agente de nuvem do Copilot.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina o **Title** como `Show related games on the game details page`.
6. Defina a **Description** como:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver o emoji de olhos 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![Emoji de olhos 👀 usado pelo Copilot para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando diligentemente na segunda solicitação. Como o agente de nuvem do Copilot trabalha de forma semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo o tempo todo; basta revisar o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, é possível atribuir issues ao Copilot para execução assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de issues bem definidas.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, você pode criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e receber instruções personalizadas, que foram abordadas nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
