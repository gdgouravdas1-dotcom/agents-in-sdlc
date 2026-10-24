---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para realizar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um PR de rascunho para revisão por uma pessoa desenvolvedora. Assim, é possível delegar tarefas tediosas, acelerar o desenvolvimento e liberar tempo para se concentrar em questões mais amplas.

Neste exercício, você explorará com o agente de nuvem do Copilot:

- a personalização do ambiente de geração de código.
- a importância de garantir que as operações sejam realizadas com segurança.
- a importância de problemas com escopo claro.
- a atribuição de problemas ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas que gostaria de resolver. Os prestadores de serviço contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado insatisfatório — na verdade, ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre títulos relacionados — outros jogos da mesma categoria — para que as pessoas possam continuar explorando. Ainda não é necessário ter uma implementação finalizada; a equipe precisa de algo que possa usar para testar a experiência de aceitação. Esse trabalho é um bloqueio, mas há problemas de prioridade maior no momento.

Esses são exemplos de tarefas que podem ser facilmente adiadas e que são ideais para atribuir ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona, permitindo que a pessoa desenvolvedora se concentre em outras tarefas e depois volte para revisar o trabalho e confirmar o resultado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] pode realizar tarefas em segundo plano, de maneira semelhante a uma pessoa desenvolvedora. Assim como no trabalho com uma pessoa, há várias formas de iniciar uma tarefa, incluindo [atribuir um problema do GitHub ao Copilot][assign-issue]. Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode explorar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot: não é possível deixar de pensar. Até tarefas aparentemente simples ficam complexas quando examinadas em detalhe. Por isso, [defina com cuidado o escopo das tarefas atribuídas ao agente de nuvem][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções existentes no repositório, sem editá-los, para reconhecer o efeito deles mais tarde.

Abra os seguintes arquivos na interface Web do GitHub ou em um Codespace, caso já tenha um em execução:

- `.github/copilot-instructions.md` — examine a seção **Code standards**, especialmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir o problema `Code lacks documentation` ao agente de nuvem na próxima seção, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Eles vêm desses arquivos de instruções. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente de nuvem usa [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [fluxo de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. Assim, o agente tem acesso às ferramentas e dependências necessárias. Esse fluxo já está configurado para facilitar o laboratório. Ele disponibiliza Node.js, dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e populado do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se quiser clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente quando elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
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

      # Migra e popula o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro fluxo de trabalho do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho chamado `copilot-setup-steps`. Esse trabalho é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o acionador `workflow_dispatch`, que permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar se o fluxo funciona, sem precisar esperar que o Copilot o execute.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novas pessoas à equipe. O Copilot é especialmente útil na criação de documentação, então este é um problema ideal para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária; em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo problema.
4. Selecione **Blank issue** para criar um problema em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar o problema.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuir um problema ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, com um título semelhante a `[WIP]: Code lacks documentation`. Se o PR ainda não aparecer, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está configurando o ambiente (executando o fluxo mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar no problema.
15. Selecione o botão **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em andamento e acompanhar como o Copilot resolve o problema. Isso inclui explorar o código e compreender seu estado, pausar para pensar e decidir sobre a abordagem e criar código.

O processo pode levar vários minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona, liberando você para se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, voltando nossa atenção à criação de recursos para o aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho para que você possa se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Talvez adicionar uma seção de jogos relacionados para a equipe de design não leve muito tempo, mas esse tempo poderia ser usado em outras tarefas. Vamos atribuí-la ao agente de nuvem.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo problema.
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

7. Selecione **Create** para criar o problema.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

   ![Atribuir um problema ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você verá 👀 no primeiro comentário do problema, indicando que o Copilot começou a trabalhar.

![O Copilot usa o emoji de olhos para indicar que está trabalhando no problema](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot está trabalhando na segunda solicitação. O agente de nuvem funciona de forma semelhante a uma pessoa engenheira de software; não é necessário acompanhá-lo continuamente. Você pode revisar o trabalho quando estiver concluído. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir problemas ao Copilot para que sejam executados de forma assíncrona. Use o Copilot para resolver dívidas técnicas, criar recursos ou ajudar a migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente para a geração de código.
- garantir que as operações sejam realizadas com segurança.
- definir problemas com escopo claro.
- atribuir problemas ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas exploradas nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir problemas do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

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
