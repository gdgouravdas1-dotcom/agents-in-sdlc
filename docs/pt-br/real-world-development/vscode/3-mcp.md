---
title: "Exercício 3 - Testar o recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ele funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem no navegador.
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais poderoso quando pode acessar recursos externos ao editor. O Model Context Protocol (MCP) é a maneira como o Copilot faz isso: um padrão para que o agente se comunique com ferramentas e serviços externos.

![Diagrama da estrutura interna do modo de agente e de sua interação com o contexto, o LLM e as ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Assim, eles podem acessar informações atualizadas (usando recursos) e executar ações em seu nome (usando ferramentas).

Essas ferramentas e esses recursos são acessados por meio de um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e serviços externos. O servidor gerencia a comunicação entre o agente e ferramentas externas, como APIs existentes ou ferramentas locais, como pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos aos quais o agente pode ter acesso.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp-server]**: fornece acesso a APIs para gerenciar repositórios do GitHub e permite que o agente execute ações como criar repositórios, atualizar os existentes e gerenciar problemas e pull requests.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegador com o Playwright. O agente pode navegar por páginas Web, preencher formulários e selecionar botões.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições para o ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Você verá um servidor `github` já configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada `github` vem com o modelo do projeto e permitiu que o Copilot lesse o backlog de problemas no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente. O VS Code se conecta por HTTP, e você autentica-se com o GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicionar o servidor MCP do Playwright

Agora, adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável, exatamente o que você precisa para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github` para que o arquivo fique assim:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na máquina executando `npx @playwright/mcp@latest`. A opção `--headless` instrui o Playwright a executar o navegador sem uma janela visível, o que é necessário em um Codespace, que não tem uma área de trabalho para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede confirmação de que você confia nele.

1. Em `.vscode/mcp.json`, uma ação **Start** aparece acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar se a configuração está correta.
2. Se o VS Code pedir que você confirme que confia no servidor, revise a configuração e escolha confiar para que ele seja iniciado.

Iniciar o servidor manualmente é opcional. Se você pular essa etapa, o VS Code iniciará o servidor e pedirá sua confirmação quando o Copilot precisar das ferramentas, na etapa seguinte.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros e ler os resultados. Em seguida, ele apresenta um resumo do que observou. Assim, você confirma rapidamente o comportamento do recurso sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página em vez de capturas de tela. Isso significa que o agente trabalha com elementos estruturados e rotulados (botões, links e itens de lista), como a tecnologia assistiva faz. Portanto, uma verificação funcional rápida também serve como uma avaliação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso de filtragem:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação do problema. Se algo parecer incorreto, faça perguntas complementares ou peça que o Copilot corrija o código antes de abrir um pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para este teste. Se você parou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que o navegador está ausente, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Agora que você confirmou que o recurso funciona, abra um pull request (PR) para que a equipe possa revisá-lo. Primeiro, publique a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no Codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações feitas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias maneiras de criar um pull request, incluindo usar o github.com ou a interface de linha de comando (CLI) do GitHub. Como você já está trabalhando com o GitHub Copilot, peça que ele crie o PR e associe-o ao problema correto.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado no seletor de agentes para permitir que o Copilot use as ferramentas do GitHub.
3. Peça ao Copilot que crie um PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot reúna informações e execute operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se; siga as instruções para autorizar o acesso.
5. Observe que o Copilot pesquisa os problemas, encontra o correto e cria o PR.
6. Selecione o link gerado para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam os recursos do Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou o servidor para testar manualmente o recurso de filtragem no navegador antes de publicá-lo.
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
