---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações, se é que alguma, não enfrentam dificuldades com dívida técnica. Ela pode consistir em problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de funcionalidades que ficaram paradas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um PR de rascunho para revisão por um desenvolvedor. Isso permite delegar tarefas repetitivas, acelerar o desenvolvimento e liberar os desenvolvedores para se concentrarem em questões mais abrangentes.

Neste exercício, você vai explorar como:

- personalizar o ambiente para gerar código;
- garantir a segurança das operações;
- criar issues com escopo claro;
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. Os prestadores de serviço contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeira etapa, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. Ainda não é necessário criar uma implementação refinada; basta algo que a equipe possa usar para testar a experiência. Isso é um bloqueio, mas outras issues têm prioridade mais alta no momento.

Esses são exemplos de tarefas que podem ser facilmente adiadas e que são ótimas candidatas para atribuir ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona, permitindo que o desenvolvedor se dedique a outras tarefas e depois retorne para revisar o trabalho e confirmar que tudo está correto.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de forma semelhante a um desenvolvedor humano. Assim como no trabalho com uma pessoa, há várias maneiras de usá-lo, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois de receber a atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão enquanto o trabalho está em andamento ou depois que for concluído. Quando a solução proposta estiver pronta para revisão, o Copilot mencionará você no pull request.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica: você não precisa deixar de pensar. Até operações aparentemente simples ficam complexas quando examinadas de perto; por isso, [defina com atenção o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já fez um exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; se ainda não fez, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções incluídos neste repositório, sem alterá-los, para depois identificar seus efeitos.

Abra os seguintes arquivos na interface Web do GitHub para seu repositório ou em um codespace que já esteja em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, principalmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentários no pull request resultante. Essas instruções vêm dos arquivos que você revisou. Voltaremos a isso ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem

Normalmente, a criação de código exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow de configuração especial][setup-workflow], definido no arquivo `.github/workflows/copilot-setup-steps.yml`, para ser executado antes do início do trabalho. Assim, o agente tem acesso às ferramentas de desenvolvimento e dependências necessárias. Esse workflow já foi configurado para facilitar o andamento do laboratório e permitir que você conheça esse recurso. Ele disponibiliza Node.js, dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mais restritas possíveis para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
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

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes que o Copilot comece a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow funciona, em vez de esperar o Copilot executá-lo.

## Adicione documentação

Todos entendem a importância da documentação, mas muitos projetos têm informações desatualizadas ou não têm documentação. Esse tipo de dívida técnica costuma não ser resolvido, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novos desenvolvedores. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma issue perfeita para atribuir ao agente de nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina o **Title** como `Code lacks documentation`.
6. Defina a **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se nenhum PR novo aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá ter criado uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está criando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e definindo a melhor abordagem para a tarefa.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Ele explora o código, entende o estado atual, faz uma pausa para pensar e definir um plano e, em seguida, cria código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente de nuvem do Copilot é executar tarefas de forma assíncrona, liberando você para trabalhar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, em seguida, criar código para adicionar funcionalidades ao aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Talvez a seção de jogos relacionados solicitada pela equipe de design não leve muito tempo, mas esse tempo ainda pode ser usado em outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
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

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir o Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver um conjunto de olhos 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando com afinco na segunda solicitação. O agente de nuvem funciona de forma semelhante a um engenheiro de software: não é necessário monitorá-lo continuamente, mas sim revisar o trabalho quando estiver concluído. Vamos começar a criar e usar agentes personalizados.

## Resumo e próximas etapas

Nesta lição, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com o agente de nuvem, você pode atribuir issues ao Copilot para execução assíncrona. Use-o para resolver dívidas técnicas, criar funcionalidades ou ajudar na migração de código de um framework para outro.

Você explorou estes conceitos:

- personalizar o ambiente para gerar código;
- garantir a segurança das operações;
- criar issues com escopo claro;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e pode acessar instruções personalizadas, que foram abordadas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
