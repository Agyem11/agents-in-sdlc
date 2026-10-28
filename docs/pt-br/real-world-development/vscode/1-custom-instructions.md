---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: pré-requisitos][previous-lesson] · [Próxima lição: modo agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer —, esse contexto precisa estar disponível. Há várias maneiras de compartilhar contexto específico com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que fornecem orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo de caminho;
- gerar a primeira parte da camada de dados para filtragem (um helper de editoras) usando as instruções *atuais*;
- adicionar um novo padrão geral ao arquivo `.github/copilot-instructions.md`;
- enviar o mesmo prompt novamente e observar o código gerado adotar o novo padrão;
- fazer commit das atualizações de instruções e da parte de filtragem em `main` para que o Copilot use a orientação atualizada no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança após a atualização das instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- A documentação deve ser adicionada ao código na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot, ajudando-o a entender melhor seu estilo de programação e seus requisitos. Esse recurso permite orientar as sugestões para que sejam mais relevantes, especificando convenções de programação, bibliotecas e até os tipos de comentários que você quer incluir no código. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo de instruções enviado ao Copilot **em todas** as solicitações feitas para o repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações de chat ou CLI, como a pilha de tecnologias, uma visão geral do que está sendo criado, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivos específicos. Eles podem conter diretrizes para linguagens específicas (como TypeScript ou Astro) ou tarefas, como criar um componente de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para a geração de código no Copilot Chat — não para conclusões de código ou sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha o conteúdo de `copilot-instructions.md` focado em orientações para todo o projeto, como uma descrição do que está sendo criado, a estrutura do projeto e os padrões gerais de programação.
- Use arquivos `*.instructions.md` para fornecer diretrizes específicas para tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve ou não deve ser.

Não há uma única maneira certa de criar arquivos de instruções, assim como não há uma única maneira certa de usar IA. Com a experimentação, você descobrirá o que funciona nos seus ambientes e cenários.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, talvez você encontre instruções para várias tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece essa opção de uma forma diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI) — a lição do ambiente em que você estiver indicará quando ela for relevante.
>
> Procurando templates ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Para começar, o projeto inclui vários arquivos de instruções. Vamos explorá-los antes de fazer uma mudança para observar o impacto.

1. Volte ao seu Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Essas informações se aplicam a todas as interações com o Copilot.
4. Abra a pasta `.github/instructions` e explore seus arquivos. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo — ele define um glob (relativo à raiz do repositório) que determina quais arquivos são abrangidos pelas instruções. Aqui, qualquer arquivo de teste TypeScript, como um que corresponda a `**/*.test.ts`, será incluído.
6. Observe as instruções específicas para criar testes unitários neste projeto.
7. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Crie uma branch para as mudanças

Vamos seguir algumas práticas recomendadas e criar uma branch para trabalhar.

1. Volte ao codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e acesse uma nova branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam conforme o exercício — cada etapa indica o que fazer quando isso for relevante.

Para observar o impacto das instruções personalizadas, primeiro envie um prompt com o arquivo de instruções atual. Depois, você o atualizará e enviará o mesmo prompt novamente para comparar os resultados.

1. Feche as guias de editor abertas nos exercícios anteriores para que o Copilot receba apenas o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar ao Copilot onde o helper deverá ficar.
3. Na visualização Chat, selecione **Agent** no menu suspenso de agentes para que o Copilot possa aplicar mudanças aos arquivos.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações, muitas vezes aos arquivos do helper e aos testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (aplicáveis a `src/lib/*.ts`).
7. Observe que o código proposto **não tem** comentários TSDoc nem cabeçalho de comentários no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem — o objetivo é observar a melhoria de consistência depois da atualização das instruções.

## Adicione um novo padrão ao repositório

Como mencionado anteriormente, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela documenta os padrões de programação, mas ainda não exige documentação no código — por isso, o helper gerado não tinha comentários de documentação.
3. Logo abaixo dos padrões existentes, adicione estas linhas de Markdown para orientar o Copilot a adicionar cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções podem ser criadas no nível do repositório (`.github/copilot-instructions.md`) para orientações gerais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra dos comentários de documentação que você acabou de adicionar.

## Envie o prompt novamente e observe a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione novamente `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras da plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus IDs e nomes.
    *
    * @param db - O cliente do banco de dados Drizzle.
    * @returns Uma promessa que é resolvida com uma matriz de objetos de editora.
    */
   ```

Você acabou de orientar o Copilot a seguir um novo padrão e aplicá-lo ao código real que será usado no próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são gerenciados pelo controle de versão como qualquer outro recurso. Vamos fazer commit das mudanças e enviar a branch ao repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme se as atualizações de instruções e do helper estão presentes:

   ```bash
   git status
   ```

3. No terminal, prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se o git status mostrar outras atualizações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Depois de enviar a branch, crie um pull request e associe-o à issue de padrões de documentação no backlog. Você poderia fazer isso manualmente, mas o Copilot pode encontrar a issue, criar um PR associado e mesclá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot atua no GitHub em seu nome. O template do projeto já configura as ferramentas do GitHub para seu ambiente, então não é necessário fazer nenhuma configuração. Na primeira vez que o Copilot usar uma ferramenta do GitHub, talvez o VS Code peça para você entrar no GitHub. Siga as instruções. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no seu codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme se **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot para localizar a issue relacionada à atualização dos arquivos de instruções e criar um PR a partir da branch atual, descrevendo tanto as atualizações das instruções quanto a base do helper de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando o Copilot pedir permissão para executar ações do GitHub, revise o comando e selecione **Allow** se for apropriado.
6. Quando o PR for criado, peça ao Copilot para mesclá-lo e retornar sua branch local para main:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando o Copilot pedir permissão para executar ações do GitHub e comandos do shell, revise o comando e selecione **Allow** se for apropriado.

Agora você criou e mesclou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*;
- adicionar um novo padrão geral ao arquivo `.github/copilot-instructions.md`;
- enviar novamente o mesmo prompt e observar o código proposto adotar o novo padrão;
- fazer commit das atualizações das instruções e da base do helper em `main` para que o próximo exercício possa usá-las.

Em seguida, você colocará as instruções em prática no [modo agente][next-lesson] enquanto o Copilot adiciona uma nova funcionalidade à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — conjunto de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
