---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo agente, ele trabalha de forma autônoma: explora a base de código, propõe mudanças, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição de arquivo à medida que ocorrem. Você pode revisar cada diff inline, aceitar ou rejeitar mudanças individuais e orientar a conversa com prompts de acompanhamento para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar o trabalho do agente de acessibilidade em tempo real;
- revisar e aceitar as edições de arquivos propostas;
- orientar o agente durante a sessão para adicionar um alternador do modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design faz outra solicitação: também quer um alternador para o modo claro, para usuários que consideram o tema padrão escuro demais. Em vez de começar uma conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente — os arquivos que explora, os comandos que executa e as edições que propõe. Cada edição aparece inline no chat com um diff e os controles **Keep**/**Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra o chat para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição, o diff aparecerá inline:
   - selecione **Keep** para aceitar a mudança;
   - selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos modificados pelo agente nesta sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando de terminal), o agente pausará e exibirá uma solicitação **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde o agente terminar de implementar o modo de alto contraste. Você verá uma mensagem final que resume as mudanças.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste foi implementado, você ampliará a mesma conversa para adicionar um alternador do modo claro. Como continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão usado para o armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorando as mudanças existentes e propondo adições para o modo claro.
3. Revise cada nova edição à medida que for feita, como fez com o modo de alto contraste. Use **Keep** ou **Undo** para cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface dos alternadores de uma forma que não agrada a você —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem de esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione um terceiro alternador; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para os dois alternadores").

## Revise o conjunto de mudanças

Antes de fazer commit do trabalho, revise rapidamente tudo que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e nos testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme se os padrões de acessibilidade do agente personalizado foram aplicados — atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das mudanças pelo painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo agente do Copilot Chat para acompanhar e orientar uma sessão em andamento — revisando cada edição à medida que era feita, aceitando ou rejeitando mudanças e enviando prompts de acompanhamento para ampliar o trabalho sem perder contexto.

Nesta lição, você explorou:

- acompanhar o trabalho de um agente em tempo real;
- aceitar e rejeitar edições individuais de arquivos;
- orientar uma sessão durante a execução com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as mudanças produzidas pelo agente personalizado local durante este laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: agentes personalizados][previous-lesson] | [Próxima lição: iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
