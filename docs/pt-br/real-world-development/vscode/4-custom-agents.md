---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados para tarefas ou áreas específicas do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções focadas, práticas recomendadas, padrões de programação e conhecimento específico do domínio para que ele execute determinados tipos de trabalho com mais eficiência. As equipes podem transformar seu conhecimento em agentes reutilizáveis: um agente de acessibilidade que aplica a conformidade com [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo contém frontmatter YAML com, no mínimo, `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam a IA sobre como executar tarefas. A maneira mais simples de distingui-los é: um **agente personalizado** é a pessoa que executa o trabalho; as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e são projetados para coordenar skills — e até outros agentes — durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade. Como parte do trabalho, ele pode chamar skills como uma skill de fluxo de pull request ou uma que executa e gerencia testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer aplicação de IA, teste e itere para descobrir o que funciona melhor nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é fundamental, queremos implementar esse recurso o mais rápido possível. Para isso, usaremos um agente personalizado.

Neste exercício, você aprenderá a:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace e abra um terminal. Crie uma nova branch a partir de `main` para o trabalho de acessibilidade, mantendo separado o PR de filtragem do Exercício 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

Em seguida, revise o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no seletor de agentes, na parte inferior do Copilot Chat. Você pode selecionar um agente personalizado para limitar a sessão de chat às instruções e ferramentas dele.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar as etapas abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam por exercício; cada etapa indicará o que é relevante.

1. Selecione **Agent** no seletor de agentes da exibição Chat, caso ainda não esteja selecionado.

   ![Captura de tela do seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione o seletor de agentes na parte inferior da exibição de chat. Ele mostra o agente ativo — por padrão, **default**.
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie este prompt ao agente de acessibilidade:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e aplicará edições aos arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. À medida que as edições forem feitas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos alterados pelo agente.

> [!NOTE]
> O processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no repositório: editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Este exercício apresentou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados para tarefas e áreas específicas. Com eles, você pode transformar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot na execução de determinados tipos de trabalho.

Você explorou estes conceitos:

- como agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que forem feitas e adicionando um controle de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
