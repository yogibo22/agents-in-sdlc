---
slug: pt-br/real-world-development/cloud
title: "Agente em nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[Agente em nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui uma tarefa no GitHub e o agente em nuvem a assume em segundo plano — explorando o repositório, fazendo alterações e abrindo um pull request — enquanto você fica livre para fazer outras coisas.

Ao longo destes exercícios, você adicionará instruções personalizadas que o agente em nuvem seguirá e, depois, atribuirá uma issue do GitHub para que ele implemente o trabalho. Você revisará e usará agentes personalizados para moldar sua abordagem, monitorará e orientará sessões no painel de agentes e, por fim, revisará os pull requests e iterará sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|--------|-----------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione instruções personalizadas que o agente em nuvem seguirá |
| [2. Agente em nuvem][ex2] | Agente assíncrono | Atribua issues ao agente em nuvem do Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciando agentes][ex4] | Monitoramento | Monitore e oriente sessões de agentes |
| [5. Iterando][ex5] | Revisão | Revise PRs, itere sobre o trabalho do Copilot e escolha os próximos passos |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo do **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] O agente em nuvem do Copilot habilitado para sua conta ou organização

> [!TIP]
> Sem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui o agente, MCP, revisão de código e recursos do Copilot CLI usados neste workshop, então você pode concluir todos os exercícios com ele.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O agente em nuvem está disponível para **Copilot Student, Pro, Pro+, Business e Enterprise** — todos os planos, exceto o Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitá-lo para sua organização.

> [!NOTE]
> O MCP não é abordado neste exercício. Para explorar o uso de servidores MCP com o Copilot, consulte o [workshop do CLI](../cli/) ou o [workshop do VS Code](../vscode/).

## Comece

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
