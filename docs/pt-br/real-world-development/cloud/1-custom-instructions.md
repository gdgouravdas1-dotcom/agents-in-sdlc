---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se o Copilot precisa conhecer informações adicionais —, disponibilize esse contexto. Os [arquivos de instruções][instruction-files] fornecem orientações para que o Copilot entenda não só *o que* você quer, mas também *como* quer que o trabalho seja feito.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e dos arquivos de instruções específicos por caminho.
- adicionar um novo padrão geral do repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos do VS Code e do CLI, aqui você não executará um prompt *antes e depois*. O agente de nuvem do Copilot trabalha de forma assíncrona nos problemas do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests produzidos pelo agente de nuvem.

## Arquivos de instruções

### Cenário

Como qualquer boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes de unidade.
- A interface do usuário deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para realizar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Você pode especificar convenções de programação, bibliotecas e até os tipos de comentários que prefere. Crie instruções para todo o repositório ou para tipos de arquivo e tarefas específicos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações no nível do projeto relevantes para a maioria das solicitações de chat ou CLI, como a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações globais.
- `.github/instructions/*.instructions.md`, arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Use-os para orientar linguagens como TypeScript ou Astro ou tarefas como criar um componente de UI ou um conjunto de testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, mas não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. Ainda assim, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha `copilot-instructions.md` focado em orientações para o projeto, como uma descrição do que está sendo criado, a estrutura do projeto e padrões globais de programação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo (testes de unidade, componentes Astro e a camada de dados) ou tarefas específicas.
- Use linguagem natural. Escreva orientações claras e dê exemplos de como o código deve ou não deve ser escrito.

Não há uma única maneira correta de criar arquivos de instruções, assim como não há uma única maneira de usar IA. Com a experimentação, você descobrirá o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto robusto de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá instruções para vários tipos de tarefas, incluindo [atualizações de UI][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada superfície apresenta esse recurso de maneira diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição correspondente indicará quando ele for relevante.
>
> Procura modelos ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos neste repositório: há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a descrição breve do projeto e as seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas para **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Por exemplo, ele corresponde a qualquer arquivo de teste TypeScript que corresponda a `**/*.test.ts`.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o fim. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes no projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicionar um padrão geral ao repositório

Agora você editará o arquivo para estabelecer uma regra geral: a documentação deve aparecer no código em comentários TSDoc e em um cabeçalho de comentário no arquivo. O agente de nuvem usará essa orientação ao trabalhar nos problemas que você atribuir mais adiante neste percurso.

Antes de editar, crie uma branch. O agente de nuvem lê as instruções da branch de destino do problema, mas você também fará o commit das alterações de forma organizada:

1. No terminal do Codespace, crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como mencionado, `.github/copilot-instructions.md` fornece informações sobre o projeto ao Copilot. Vamos documentar os padrões de programação para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de programação, mas ainda não tem uma regra para documentação no código — por isso, a função auxiliar gerada não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como visto na lição anterior, os arquivos de instruções podem estar no nível do repositório (`.github/copilot-instructions.md`) para oferecer orientações globais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação adicionada agora.

## Fazer commit, push e mesclar a atualização de instruções

O agente de nuvem lê os arquivos de instruções da branch de destino do problema. No próximo exercício, ao atribuir um problema ao Copilot, ele criará uma branch a partir de `main`. Portanto, para que o agente use suas instruções, elas precisam estar em `main`.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça a mesclagem. O agente de nuvem lerá essas instruções ao trabalhar nos problemas que serão atribuídos no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar a etapa da branch e fazer o commit diretamente nela. A branch é usada para que o workshop reflita a abordagem comum em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e adicionou um novo padrão geral a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests produzidos pelo agente de nuvem ao longo do restante do percurso.

Em seguida, você [atribuirá seu primeiro problema ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
