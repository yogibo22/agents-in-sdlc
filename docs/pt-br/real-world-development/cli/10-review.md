---
title: "Lição 10 - Encerramento e próximos passos"
description: "Relembre o fluxo do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo da Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue inicial de filtragem.
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e mesclou manualmente seu primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, desenvolveu o recurso no modo Autopilot e revisou o resultado no modo Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill existente `quality-checks` e a usou para executar testes unitários, lint e verificação de tipos.
- adicionou o servidor Playwright Model Context Protocol (MCP) e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou a alteração completa de filtragem e autorizou o Agent Merge no PR de filtragem.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch a partir do `main` atualizado:

1. **Avaliações por estrelas:** exibe a `starRating` existente e um estado explícito de não avaliado nos cards dos jogos.
2. **Filtragem e fluxo de qualidade:** implementa a filtragem, atualiza as instruções e as aplica ao recurso, personaliza o relatório `quality-checks`, cria um perfil de QA e inclui os testes associados.

Do planejamento da filtragem até a abertura do PR, você usou a mesma conversa e a mesma branch. Combinamos esse trabalho em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: com testes automatizados, com sua própria inspeção no navegador e com a exploração do navegador pelo Copilot usando MCP. A skill `quality-checks` executou testes unitários, lint e verificação de tipos e relatou os resultados no novo formato. Antes do PR, o QA reuniu esses resultados com uma revisão dos requisitos e da cobertura de testes.

Os testes adicionados devem cobrir lacunas reais; uma execução de QA sem novos testes também pode ser correta. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas depois de fazer alterações.

## Boas práticas

O contexto e as ferramentas que você fornece ao Copilot moldam o trabalho. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em diferentes conversas e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Combine o **modo e o modelo** com a tarefa. Use **Plan** para pensar na abordagem antes de desenvolver, **Interactive** para manter o controle em alterações focadas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e um mais capaz para trabalhos complexos.

O contexto continua tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer desenvolver, *por que* e *como* altera significativamente o resultado.

## Mais para explorar

Você concluiu o fluxo principal. Alguns outros recursos do CLI que vale a pena conhecer:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para discutir um problema e obter outra perspectiva.
- `/fleet` para orquestrar subtarefas independentes em paralelo.
- `/worktree` para isolar outra tarefa.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a. Use-a para código de produção, projetos pessoais ou para aquele pequeno aplicativo que você sempre quis criar, mas nunca conseguiu começar. Compartilhe o que aprendeu com sua equipe e aprenda com os outros. E, como sempre, explore a documentação.

Se quiser ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
