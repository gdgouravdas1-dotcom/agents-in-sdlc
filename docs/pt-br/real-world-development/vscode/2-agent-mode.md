---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo normalmente exigem alterações em vários arquivos e a execução de tarefas como testes. O fluxo comum de desenvolvimento envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para trabalhar com mais autonomia no IDE. Ele se comporta de forma semelhante a uma pessoa desenvolvedora: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Apesar dos nomes parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos de feedback e interação rápidos. O agente de nuvem funciona de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de problemas e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar recursos na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo modo de agente antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou uma função auxiliar para editoras. Agora, usará o modo de agente do Copilot para concluir o trabalho restante na camada de dados, na interface e nos testes.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site Tailspin Toys para entender a funcionalidade atual.

O site é uma plataforma de financiamento coletivo de jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o ORM Drizzle; não há uma API de backend separada nem um framework de UI do lado do cliente. As funções reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um pequeno `<script>` Astro com escopo definido e APIs DOM padrão.

### Iniciar o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespace:

1. Volte ao Codespace. Depois do exercício anterior, você deve estar na branch `main`.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um aviso com o URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço `http://localhost:4321` do servidor Astro no terminal.

> [!NOTE]
> Em um Codespace, selecionar o link localhost no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o Codespace, que agora hospeda o servidor Web.

### Explorar o site

Com o site em execução, explore suas funcionalidades. Os principais recursos são:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar as etapas abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam por exercício; cada etapa indicará o que é relevante.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar examinando o backlog. Quando você criou o projeto a partir do modelo, uma lista de problemas foi criada automaticamente. Peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** no seletor de agentes da exibição Chat. O agente trabalha de forma autônoma, planeja e implementa alterações em arquivos, executa comandos de terminal e chama ferramentas.

   ![Captura de tela do seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão expressando preferência por um modelo específico. Durante a criação do workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções para tentar manter as sugestões relativamente consistentes. No entanto, os modelos de linguagem são probabilísticos, então as sugestões podem ser diferentes das descritas. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem, o Copilot pode usar outro comando MCP e ainda assim concluir a tarefa.

3. Peça ao Copilot que mostre o backlog enviando o prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todos os problemas.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens com base nos que considera mais úteis para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere o código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)][ui-instructions], que explica como adicionar funcionalidades ao site.

1. No Codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, capacidade de teste e acessibilidade.
   - links para arquivos de instruções específicos para vários tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso permite reutilizar orientações, dividir conjuntos complexos em partes menores e fornecer exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, será necessário fazer pelo menos três alterações distintas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar os testes do comportamento de filtragem.
- atualizar a página de listagem de jogos com a interface de filtragem.

Além disso, os testes devem ser executados e aprovados antes da mesclagem. O modo de agente do Copilot pode fazer esse trabalho.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado no seletor de agentes da exibição Chat.

   ![Captura de tela do seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base no problema relacionado do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, incluindo as funções auxiliares da camada de dados, a interface e os testes. Depois, ele modificará os arquivos e executará os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você verá um botão **Keep** ou **Undo** para cada alteração; não é necessário selecionar nenhum enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando permissão para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Podem ocorrer pausas e alguns testes podem falhar durante o processo. Isso é esperado: o Copilot alterna entre a geração de código e os testes até concluir a tarefa sem detectar erros.

   ![Sessão completa do modo de agente do Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar funcionalidades ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar recursos na interface e na camada de dados.
- o modo de agente pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar as alterações e os testes gerados pelo modo de agente antes de mesclá-los à base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — Implementar paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Use as habilidades aprendidas neste exercício e peça ao Copilot que atualize o site. Algumas considerações:

- siga as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções disponíveis.
- decida como a paginação deve funcionar: por exemplo, se a pessoa usuária poderá selecionar o tamanho da página ou se ele será fixo.
- forneça ao Copilot as orientações necessárias para implementar o comportamento desejado.
- talvez seja necessário iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos Ask, Edit e Agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
