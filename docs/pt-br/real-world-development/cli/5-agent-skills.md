---
title: "Lição 5 - Personalizar e usar uma skill de quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas programar. Já validamos manualmente se o código funciona e usamos arquivos de instruções para garantir que ele siga nossos padrões. Mas e os testes? E o lint? E as outras partes da integração contínua (CI)?

Para esses tipos de tarefas, as **skills de agente** são a melhor opção! Elas ajudam o Copilot a entender como executar operações como essas corretamente.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que o agente carrega sob demanda. Em sua essência, são pastas com o nome da skill e um arquivo Markdown chamado `SKILL.md`. Esse arquivo contém frontmatter com um nome e uma descrição que definem a skill, uma visão geral do que ela faz e orientações sobre quando deve ser chamada. A pasta também pode conter subpastas com scripts e outros recursos que a skill utiliza.

> [!NOTE]
> Uma skill não precisa de arquivos e pastas adicionais. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos existentes no projeto.

As skills podem ficar na pasta `.github/skills` do projeto para se tornarem um recurso do repositório, compartilhado e reutilizado pela equipe, ou na pasta de skills do usuário em `~/.copilot/skills`.

## Explore a skill

Vamos explorar a skill `quality-checks` criada pela equipe da Tailspin Toys para executar testes unitários, lint e verificações de tipos.

1. Volte ao seu Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a entender quando chamar a skill.
3. Leia as instruções e observe como orientam o Copilot nas etapas de teste e lint.
4. Observe que a skill ainda não tem uma seção **Formatação da saída dos resultados**.

## Execute a skill antes de fazer uma alteração

Vamos pedir ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Use o seguinte prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final.

## Personalize o relatório

Vamos aprimorar o relatório para que ele informe o que foi executado, se houve sucesso e o que as ferramentas relataram. Vamos atualizar a skill para gerar esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Formatação da saída dos resultados

   Ao concluir, informe cada comando executado e se foi aprovado, reprovado ou bloqueado. Inclua contagens de testes, durações, erros, avisos e outras métricas somente quando a ferramenta as informar. Indique a próxima ação para qualquer falha ou bloqueio e nunca descreva uma verificação ignorada ou incompleta como aprovada.
   ```

3. O arquivo é salvo automaticamente.

## Execute a skill atualizada

Depois de fazer a alteração, veja a skill em ação. O Copilot CLI recarrega skills editadas sem precisar reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. A seguir, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor Playwright MCP][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade servem como referência, não como tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes no navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
