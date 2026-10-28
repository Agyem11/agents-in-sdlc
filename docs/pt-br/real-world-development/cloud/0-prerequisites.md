---
title: "Exercício 0: pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do agente de nuvem do Copilot, você precisa preparar tudo. Você criará sua própria cópia do repositório Tailspin Toys e iniciará um [codespace][codespaces] para editar arquivos de instruções e revisar o trabalho produzido pelo agente de nuvem.

## Configurar o repositório de laboratório

Para criar uma cópia do repositório com o código que você usará, crie uma instância a partir do [template][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório, e você a usará ao longo dos exercícios.

1. Em uma nova janela do navegador, acesse o [template da Tailspin Toys][tailspin-template].
2. Crie sua cópia do repositório selecionando o botão **Use this template** na página do repositório de laboratório. Em seguida, selecione **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Configurações do template do repositório](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório que você criou (**organization-or-user-name/repository-name**), pois você fará referência a ele mais adiante no laboratório.

## Criar um codespace

Em seguida, você usará um codespace para concluir os exercícios do laboratório.

O [GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que você acabou de criar.
2. Selecione o botão verde **Code**.

    ![Botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e, em seguida, selecione o botão **+** para criar um novo Codespace.

    ![Criar um novo codespace](../../../_images/ex0-create-codespace.png)

A criação do codespace levará alguns minutos, embora ainda seja muito mais rápida do que instalar todos os serviços manualmente. Enquanto isso, você pode explorar outros recursos do GitHub Copilot, que serão apresentados a seguir.

> [!CAUTION]
> Você voltará ao codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi criado para ser executado em um codespace ou em um [dev container][dev-containers] local. Ambos garantem que o ambiente tenha todos os pré-requisitos necessários para uma experiência tranquila. Se preferir executar tudo localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo dev container usado pelo codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumo

Parabéns, você criou uma cópia do repositório de laboratório! Você também iniciou a criação do seu codespace, que será usado ao trabalhar com o agente de nuvem do Copilot.

## Próxima etapa

Vamos adicionar instruções personalizadas que o agente de nuvem seguirá. Continue para [Exercício 1: instruções personalizadas][next-lesson].

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um template][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
