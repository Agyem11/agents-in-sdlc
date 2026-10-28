---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados, adaptados a tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes com arquivos Markdown na pasta `.github/agents` do repositório, você fornece ao Copilot instruções focadas, práticas recomendadas, padrões de código e conhecimento específico do domínio. As equipes podem transformar seus conhecimentos em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica a conformidade com [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos os campos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Comparação entre agentes personalizados e skills de agente

Há certa sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. A distinção mais simples é: um **agente personalizado** é quem trabalha, e as **skills** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e são criados para orquestrar skills (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade; ele poderia usar skills para executar um fluxo de pull request ou executar e gerenciar testes.

> [!NOTE]
> Não existe uma única maneira certa de criar um agente personalizado. Como em tudo que envolve IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todos, independentemente das habilidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que os usuários possam ativar e desativar.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente;
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Um agente personalizado para acessibilidade já foi criado. Vamos revisar seu conteúdo para entender como ele orientará o Copilot.

Volte ao seu codespace e abra um terminal. Crie uma branch nova a partir de `main` para o trabalho de acessibilidade (o PR de filtragem do Exercício 3 ficará separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções que destacam:
   - responsabilidades principais ao gerar código para um site acessível;
   - práticas recomendadas de acessibilidade;
   - exemplos de código para HTML, CSS e JavaScript;
   - armadilhas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no seletor de agentes, na parte inferior da visualização Copilot Chat. Você pode selecionar um agente personalizado para aplicar as instruções e ferramentas dele à sessão de chat.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao seu codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam conforme o exercício — cada etapa indica o que fazer quando isso for relevante.

1. Se ainda não estiver selecionado, escolha **Agent** no seletor de agentes da visualização Chat.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione o seletor de agentes na parte inferior da visualização Chat (por padrão, ele mostra o agente ativo **default**).
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o seguinte prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará a trabalhar: explorará a base de código, proporá mudanças e aplicará edições aos arquivos do projeto. Cada edição aparecerá inline no chat com o caminho do arquivo e um diff para revisão.
6. À medida que as edições forem feitas, o indicador **Files changed** será atualizado com os arquivos alterados pelo agente.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot está fazendo mudanças reais no repositório — editará arquivos existentes, como componentes Astro, CSS e testes relacionados, conforme necessário.

Você revisará e orientará esse trabalho em andamento na próxima lição.

## Resumo e próximas etapas

Nesta lição, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados e adaptados a tarefas e domínios específicos. Com agentes personalizados, você pode transformar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot a executar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos;
- como usar um agente personalizado no modo agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as mudanças à medida que são feitas e adicionando um alternador para o modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no seu IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: testar sua funcionalidade com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
