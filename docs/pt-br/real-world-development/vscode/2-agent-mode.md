---
title: "Exercício 2 - Adicionar funcionalidades com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até a atualização mais simples de um aplicativo costuma exigir mudanças em vários arquivos e a execução de operações, como testes. Normalmente, o fluxo de um desenvolvedor envolve localizar todos os arquivos necessários, fazer as mudanças, executar testes, depurar, descobrir qual arquivo ficou de fora, fazer outra atualização... E assim por diante.

É aí que entra o modo agente do Copilot.

O modo agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele funciona de maneira semelhante a um desenvolvedor: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam semelhantes, o modo agente e o agente de nuvem oferecem experiências diferentes. O modo agente trabalha no IDE, possibilitando ciclos rápidos de feedback e interação. O agente de nuvem foi criado para atuar como um colega de programação, trabalhando de forma assíncrona como um membro da equipe e interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar mudanças;
- o modo agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados;
- revisar as mudanças e os testes gerados pelo modo agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem por categoria e editora. Você já adicionou um helper de editoras no exercício anterior e agora concluirá o restante do trabalho na camada de dados, na interface e nos testes com o modo agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer qualquer mudança, vamos explorar o site Tailspin Toys para entender a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com um tema voltado a desenvolvedores. Ele permite listar jogos e mostrar seus detalhes. O site é um único aplicativo Astro, que renderiza páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o Drizzle ORM — não há uma API de backend separada nem um framework de interface do lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com pequenos elementos `<script>` do Astro, isolados e que usam as APIs padrão do DOM.

### Inicie o site

Para facilitar a execução, foi disponibilizada a tarefa `npm run dev`, que inicia o único servidor de desenvolvimento Astro. Siga estas etapas para executá-la no GitHub Codespace:

1. Volte ao seu codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e acesse uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute este comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá uma mensagem com a URL, semelhante a esta:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link para a URL local no terminal do Codespace redireciona você automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse endereço é um túnel privado para seu codespace, onde o servidor Web está em execução.

### Explore o site

Com o site em execução, explore suas funcionalidades. Os principais recursos incluem:

- **Página inicial**: mostra uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você acessa uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam conforme o exercício — cada etapa indica o que fazer quando isso for relevante.

A implementação inicial do site funciona, mas queremos ampliar seus recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do template, uma lista de issues foi criada automaticamente. Peça ao GitHub Copilot para mostrar esses itens.

1. Selecione **Agent** no menu suspenso de agentes da visualização Chat. O agente planeja e implementa mudanças de forma autônoma, executa comandos de terminal e chama ferramentas.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos Claude Sonnet 4.5 ao criar o workshop e, por isso, o incluímos nas instruções. Esperamos que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. Como LLMs são probabilísticos, porém, as sugestões podem ser diferentes das descritas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot poderá usar um comando MCP diferente, mas deverá conseguir concluir a tarefa.

3. Envie este prompt ao Copilot para perguntar sobre o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe como o Copilot priorizou os itens de acordo com o que considera mais útil para o usuário.

## Revise os arquivos de instruções

Antes de pedir ao agente para gerar código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contém orientações sobre como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura;
   - princípios de design de componentes, testabilidade e acessibilidade;
   - links para arquivos de instruções de tipos específicos, incluindo:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Isso permite reutilizar conteúdo, dividir conjuntos maiores de instruções em arquivos menores e gerenciáveis e fornecer exemplos e templates ao Copilot.

## Implemente a filtragem

Para concluir a filtragem, será necessário fazer pelo menos três atualizações no aplicativo:

- adicionar ou refinar a lógica de filtragem restante na camada de dados (`src/lib/`);
- adicionar ou atualizar testes do comportamento de filtragem;
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes da mesclagem. O modo agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue na conversa atual do Copilot ou inicie uma nova selecionando **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela mostrando como selecionar o arquivo de instruções de interface](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado no menu suspenso de agentes da visualização Chat.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue correspondente do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados à funcionalidade, incluindo os helpers da camada de dados, a interface e os testes. Em seguida, ele começará a modificar arquivos e executar testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, talvez apareçam os botões **Keep** e **Undo** no editor. Quando o Copilot terminar, você poderá aceitar ou desfazer todas as mudanças; não é necessário selecionar esses botões enquanto o trabalho está em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo do Copilot Chat que pergunta se você quer executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e alguns testes podem falhar durante o processo. Tudo bem. O Copilot alterna entre a geração de código e os testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do modo agente do Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador onde o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, exploramos como usar o modo agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo agente do GitHub Copilot pode implementar funcionalidades na interface e na camada de dados;
- o modo agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar mudanças;
- revisar as mudanças e os testes gerados pelo modo agente do Copilot antes de mesclá-los à base de código.

Agora, vamos [testar a funcionalidade com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — implementar paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Use o que aprendeu neste exercício e peça ao Copilot para implementar a paginação no site. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções;
- decidir como a paginação deve funcionar e se o usuário poderá escolher o tamanho da página ou se ele será fixo;
- fornecer ao Copilot a orientação necessária no prompt para criar a implementação desejada;
- iterar com o GitHub Copilot, pedindo mudanças e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
