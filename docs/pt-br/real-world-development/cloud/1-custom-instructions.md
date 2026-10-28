---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: pré-requisitos][previous-lesson] · [Próxima lição: agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações de contexto que o Copilot deve conhecer —, esse contexto precisa estar disponível. Os [arquivos de instruções][instruction-files] fornecem essa orientação para que o Copilot entenda não apenas *o que* você quer que ele faça, mas também *como*.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo de caminho;
- adicionar um novo padrão geral ao arquivo `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos ambientes do VS Code e do CLI, você não executará aqui um prompt *antes/depois*. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado em tempo real. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests produzidos pelo agente.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

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

Reserve um momento para ler os arquivos de instruções incluídos neste repositório: há um arquivo principal, `copilot-instructions.md`, e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Essas informações se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo — ele define um glob (relativo à raiz do repositório) que determina quais arquivos são abrangidos pelas instruções. Aqui, qualquer arquivo de teste TypeScript, como um que corresponda a `**/*.test.ts`, será incluído.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e vá até o final. Observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e para arquivos existentes no projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot ao gerar código. (Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.)

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicione um novo padrão ao repositório

Agora você fará a única edição deste exercício: adicionar ao arquivo `.github/copilot-instructions.md` um padrão para que a documentação seja incluída no código na forma de comentários TSDoc e cabeçalhos de comentários nos arquivos. O agente de nuvem usará essa orientação nas issues que você atribuir a ele mais adiante neste ambiente de prática.

Antes de editar, crie uma branch para trabalhar (o agente de nuvem lerá suas instruções da branch de destino da issue, mas você fará um commit organizado das mudanças):

1. No terminal do codespace, crie e acesse uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como mencionado anteriormente, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de programação do projeto, mas ainda não contém regras para documentação no código — por isso, o helper gerado não tinha comentários de documentação.
3. Adicione as linhas de Markdown a seguir logo abaixo dos padrões existentes, para orientar o Copilot a adicionar cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções podem ser criadas no nível do repositório (`.github/copilot-instructions.md`) para orientações gerais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra dos comentários de documentação que você acabou de adicionar.

## Faça commit, envie e mescle sua atualização de instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. Quando você atribuir uma issue ao Copilot no próximo exercício, o Copilot criará uma branch a partir de `main`. Portanto, suas mudanças de instruções precisam estar em `main` para que o agente possa usá-las.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e faça a mesclagem. O agente de nuvem lerá essas instruções quando trabalhar nas issues atribuídas a ele nos próximos exercícios.

> [!TIP]
> Se preferir trabalhar diretamente em `main` durante este workshop, você pode pular a etapa da branch e fazer commit diretamente em `main`. A branch está incluída para que o workshop reflita como você trabalharia em um projeto real.

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções deste projeto e adicionou um novo padrão geral ao arquivo `.github/copilot-instructions.md`. Você verá esse padrão em ação nos pull requests que o agente de nuvem produzir ao longo deste ambiente de prática.

Em seguida, você [atribuirá sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — conjunto de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
