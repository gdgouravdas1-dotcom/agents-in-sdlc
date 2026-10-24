---
title: "Exercício 3 - Agentes personalizados"
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

Neste exercício, você explorará:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é fundamental, queremos implementar esse recurso o mais rápido possível. Para isso, usaremos um agente personalizado.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado para você. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao Codespace e revise o arquivo do agente personalizado de acessibilidade:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML e os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Criar e atribuir um problema

O painel de agentes é o local central para trabalhar com agentes no seu ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorar tarefas e até redirecionar uma sessão em andamento e fornecer orientações adicionais. Vamos atribuir ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse o repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de novo problema.
4. Selecione **Blank issue** para criar um problema em branco.
5. Defina o **Title** como `Add high contrast mode to website`.
6. Defina a **Description** como:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. Selecione **Create** para criar o problema.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

   ![Captura de tela da atribuição ao agente de nuvem, com o agente personalizado Accessibility destacado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Este exercício apresentou os [agentes personalizados][custom-agents], assistentes de IA especializados para tarefas e áreas específicas. Com eles, você pode transformar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot na execução de determinados tipos de trabalho.

Você explorou estes conceitos:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, vamos [acompanhar e orientar a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar-se para usar agentes personalizados na organização][org-custom-agents]
- [Preparar-se para usar agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
