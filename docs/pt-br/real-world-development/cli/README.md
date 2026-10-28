---
slug: pt-br/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Crie, verifique e entregue duas mudanças na Tailspin Toys enquanto aprende sobre modos do Copilot CLI, personalizações, ferramentas MCP e automação de pull requests."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O **[GitHub Copilot CLI][about-copilot-cli]** coloca o GitHub Copilot no terminal como um assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando, para que você mantenha o foco sem trocar para um editor gráfico.

O workshop segue um fluxo contínuo da Tailspin Toys:

1. Prepare o projeto no GitHub Codespaces, instale o Copilot CLI e conheça o ambiente.
2. Faça uma alteração específica de avaliação por estrelas, revise-a no navegador e mescle manualmente seu primeiro pull request (PR).
3. Comece pela issue de filtragem, defina a abordagem no modo Plan, desenvolva-a no modo Autopilot e revise-a no modo Interactive.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill `quality-checks` existente e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem em um navegador.
7. Crie um agente personalizado de garantia da qualidade (QA) e use-o para revisar requisitos, cobertura e evidências de verificação.
8. Revise toda a mudança de filtragem e use o Agent Merge no PR da funcionalidade.
9. Explore comandos slash úteis para contexto, modelos, compartilhamento e delegação opcional à nuvem.

Para manter o foco do workshop, você criará dois PRs: um para avaliações por estrelas e outro para filtragem, com as atualizações de instruções e da skill, o perfil de QA e os testes. O fluxo de filtragem e qualidade compartilha uma conversa e uma branch para que você possa aproveitar o trabalho à medida que explora cada ferramenta.

## Lições

| Lição | Tópico | Descrição |
| ------ | ----- | ----------- |
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu Codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI e conheça o ambiente |
| [2. Adicionar avaliações em estrelas: uma melhoria rápida][ex2] | Primeira mudança | Exiba as avaliações existentes e a alternativa para null e mescle seu primeiro PR |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje a funcionalidade a partir da issue, desenvolva-a com o Autopilot e revise-a no modo Interactive |
| [4. Orientar o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções e aplique-as à filtragem |
| [5. Personalizar e usar uma skill de quality-checks][ex5] | Verificações repetíveis | Explore a skill existente, altere o formato do relatório e execute-a |
| [6. Validar a funcionalidade com o Playwright MCP][ex6] | Observação no navegador | Configure o MCP no CLI e examine o comportamento da filtragem |
| [7. Criar e usar um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especializado e reúna evidências da verificação final |
| [8. Criar e mesclar o PR da funcionalidade][ex8] | Revisão e mesclagem | Revise toda a mudança, crie o PR e use o Agent Merge |
| [9. Comandos slash do GitHub Copilot CLI][ex9] | Recursos do CLI | Explore o contexto, os modelos, o compartilhamento e a delegação opcional ao agente de nuvem |
| [10. Encerramento e próximas etapas][ex10] | Resumo | Revise o fluxo, as personalizações reutilizáveis e outros recursos |
| [Opcional: incorporar o Foundry][foundry] | Agentes hospedados | Prepare um modelo, implante um agente baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Permissão para criar um repositório e um Codespace
- [ ] Familiaridade básica com operações de terminal ou linha de comando

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][student-plan]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, verifique se o administrador habilitou o Copilot CLI.

## Comece

**[Comece pelos pré-requisitos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
