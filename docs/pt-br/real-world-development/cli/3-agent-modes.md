---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Mudanças maiores, porém, exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo existente da organização e ajudar a desenvolver as soluções certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo comum de desenvolvimento orientado por agentes: começará com uma issue para gerar um recurso, verificará o código e o comportamento e, por fim, fará a mesclagem no projeto.

> [!NOTE]
> Ao longo do fluxo de desenvolvimento do recurso, você usará a mesma conversa e a mesma branch. Normalmente, haveria branches ou pull requests diferentes para cada tipo de arquivo, mas neste workshop usaremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa com o Copilot a partir de uma issue do GitHub.
- definir requisitos no modo Plan.
- implementar o recurso no modo Autopilot.
- revisar o código.
- validar manualmente o recurso no navegador usando o encaminhamento de porta do Codespaces.

Ao continuar o trabalho no recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará a validação por MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo e as pessoas visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de começar a programação. Você usará o modo Plan para tomar essas decisões e, depois, autorizará uma implementação com escopo definido no modo Autopilot.

## Contexto

Incluir agentes de programação de IA no fluxo de desenvolvimento não altera os fundamentos — na verdade, torna-os ainda mais importantes. A maioria das pessoas desenvolvedoras segue um processo semelhante a este:

1. Começar com uma issue registrada que descreva o trabalho necessário.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria segue uma variação deste processo.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** determina o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e, depois, use Autopilot para implementá-lo.

## Comece pela issue

Antes de começar a filtragem, volte ao Codespace e confirme se o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se estiver parado, reinicie-o antes de continuar.
2. Confirme se o pull request (PR) das avaliações por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Atualize a `main` e crie uma branch para a filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que agora o prompt começa com `#7` (ou um número semelhante). O caractere `#` permite adicionar ao contexto uma issue ou um pull request (PR) do GitHub.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem para implementar um recurso ou executar tarefas antes de pedir que o Copilot trabalhe. Para tarefas complexas, é sempre recomendável dedicar algum tempo ao planejamento. Vamos mudar para o modo Plan e pedir ao Copilot que crie um plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme se o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite o prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a elaborar o plano. Primeiro, explorará o projeto e, em seguida, determinará a melhor abordagem.

3. Durante o processo, o Copilot talvez faça perguntas sobre o comportamento da filtragem. Responda de acordo com suas preferências. Não há respostas certas ou erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role a página para revisar o plano.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Autorize o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot executar o trabalho no modo Autopilot.

O Autopilot permite que o Copilot itere até considerar que a tarefa está concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou uma opção com nome semelhante na versão instalada.
2. Confirme se o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot seguir o plano, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Depois que o código for gerado, revise-o antes de mesclar, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar se tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme se o indicador não mostra mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da exibição de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija eventuais falhas.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Experimente filtrar por categoria, por editora e pelas combinações definidas no plano.
4. Confirme se a redefinição dos filtros e o comportamento sem resultados correspondem à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou modos de conversa diferentes para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma conversa com o Copilot a partir de uma issue do GitHub.
- definiu requisitos no modo Plan.
- implementou o recurso no modo Autopilot.
- revisou o código.
- validou manualmente o recurso no navegador usando o encaminhamento de porta do Codespaces.

Na próxima lição, você aprenderá mais sobre a geração de código e como garantir que ele siga as práticas documentadas ao [usar instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
