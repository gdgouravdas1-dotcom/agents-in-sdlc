---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use Plan para combinar uma abordagem, Autopilot para criar a filtragem a partir de um problema e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Mudanças maiores, porém, exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi criado para trabalhar com o fluxo de trabalho existente de uma organização e ajudar a construir as coisas certas da maneira certa. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento com agentes: começará com um problema para gerar um recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, concluirá a mesclagem ao projeto.

> [!NOTE]
> Você usará a mesma conversa e a mesma branch ao longo do fluxo de trabalho do recurso. Normalmente, seriam usadas branches ou PRs diferentes para cada tipo de arquivo, mas faremos um atalho para focar nos conceitos principais.

Nesta lição, você vai:

- iniciar uma conversa do Copilot a partir de um problema do GitHub.
- definir requisitos no modo Plan.
- implementar o recurso usando o modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador encaminhado.

Ao continuar o trabalho no recurso, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação por MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo do Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. O problema no backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes da programação. Você usará o modo Plan para resolver essas decisões e, em seguida, autorizará uma implementação com escopo definido no modo Autopilot.

## Contexto

Apresentar agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! A maioria dos desenvolvedores segue um processo parecido com este:

1. Começar por um problema registrado que descreve o que precisa ser feito.
2. Planejar o que será criado.
3. Criar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, mesclar o código.

> [!NOTE]
> Os detalhes exatos variam conforme a equipe e a organização, mas a maioria dos processos segue esse padrão.

Seguir esse processo padrão ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelas mesmas verificações que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham juntos. O agente sugere alterações e aguarda sua entrada antes de continuar.
- **Plan**: o agente cria um plano primeiro e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma, escrevendo código, executando testes e repetindo o processo até considerar a tarefa concluída.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Começar pelo problema

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confirme que o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme que o PR das avaliações por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

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
7. Pressione <kbd>A</kbd> para exibir todos os problemas.
8. Use as teclas de seta para destacar o problema **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar o problema ao prompt e voltar à guia **Session**.

Observe que o prompt agora começa com `#7` (ou um número parecido). O `#` permite incluir no contexto um problema ou pull request (PR) do GitHub.

## Planejar o recurso de filtragem

O planejamento permite definir a abordagem de implementação antes de delegar o trabalho ao Copilot. Para tarefas complexas, é sempre uma boa ideia reservar um tempo para planejar. Vamos alternar para o modo Plan e pedir que o Copilot crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência ao problema adicionada na etapa anterior, insira o seguinte prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começa a elaborar o plano: primeiro explora o projeto e depois determina a melhor abordagem.

3. Durante o processo, o Copilot pode fazer perguntas sobre o funcionamento da filtragem. Responda de acordo com suas preferências; não há respostas erradas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que revise qualquer parte que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Deixe o Copilot trabalhar usando o modo Autopilot.

O Autopilot permite que o Copilot continue trabalhando na tarefa até considerar que ela foi concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção com nome semelhante na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Observe o Copilot seguir o plano, gerar o código e executar os testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, então revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Depois que o código for gerado, revise-o antes da mesclagem, como faria com qualquer outro código. Vamos revisar o código e executar o site para confirmar que tudo está funcionando.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com o problema e com as decisões tomadas durante o planejamento.
4. Depois de revisar o código, pressione <kbd>Esc</kbd> para sair da visualização de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija eventuais falhas.

## Explorar a nova funcionalidade

O código parece correto, mas será que funciona? Inicie o aplicativo como antes e abra o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria, por editora e as combinações definidas no plano.
4. Confirme que a redefinição dos filtros e o comportamento sem resultados correspondem ao problema e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma conversa do Copilot a partir de um problema do GitHub.
- definiu requisitos no modo Plan.
- implementou o recurso usando o modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, vamos explorar como garantir que o código siga as práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
