---
title: "Lição 7 - Criar e usar um agente QA"
description: "Crie um agente QA personalizado que combine requisitos da issue, a skill quality-checks e o Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o Playwright MCP para observar a experiência de filtragem em um navegador. Agora você reunirá essas capacidades em um agente personalizado com um processo de QA claramente definido.

Nesta lição, você vai:

- explorar como um agente personalizado funciona com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente QA e revisar suas conclusões em relação à issue de filtragem.

## Cenário

A Tailspin Toys deseja uma revisão consistente de requisitos, qualidade do código, verificações automatizadas, cobertura de testes e comportamento do navegador antes de abrir um pull request (PR). Um agente personalizado pode coordenar esse processo de QA e fornecer um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot definida em um perfil Markdown. O perfil descreve a finalidade do agente, suas instruções e as ferramentas disponíveis. Neste workshop, você definirá uma função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até aqui têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` agrupa verificações repetíveis. O Playwright MCP fornece ferramentas de navegador. O perfil de QA diz ao Copilot como usar esses recursos para avaliar os requisitos e relatar as conclusões. Ele não substitui essas ferramentas nem exige outra conversa.

## Criar o perfil de QA

Antes de abrir o PR do recurso, você pedirá ao Copilot para criar um perfil reutilizável de QA. O perfil definirá tanto as verificações de QA quanto os limites que deve seguir.

1. Volte ao seu Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão para criar o novo agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve verificar os recursos em relação às issues e aos requisitos acordados, seguir as instruções do repositório, executar a skill quality-checks, usar o Playwright MCP para verificar o comportamento e adicionar testes quando faltar cobertura.

   Ele deve informar cada requisito como aprovado, falhou ou bloqueado, com evidências que sustentem o resultado. Deve pedir antes de alterar o código de implementação e não deve confirmar alterações nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecionar o perfil

Antes de usar o novo agente, revise o perfil para confirmar que o Copilot capturou o fluxo de trabalho e os limites de QA esperados. Isso evita que um agente incompleto ou amplo demais altere o recurso quando você só deseja verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas incluí-lo dá ao agente um nome de exibição claro.
3. Leia as instruções do perfil e confirme que QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o Playwright MCP.
4. Confirme que QA relata suas conclusões com evidências, pergunta antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado deixar de incluir qualquer uma dessas responsabilidades ou limites, peça ao agente padrão para revisá-lo antes de continuar.

## Executar o QA em relação à issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Depois de criar o perfil, retome a mesma conversa de filtragem e selecione QA para que ele possa usar a issue e as decisões de planejamento já presentes no contexto.

1. Ative o agente usando a seguinte solicitação:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, ele talvez não apareça na lista de agentes disponíveis. Ele está disponível, e o comando acima o ativará.

2. Use a seguinte solicitação para pedir ao agente QA que revise o recurso:

   ```plaintext
   Revise o recurso de filtragem em relação à issue e às decisões do nosso plano. Ele está pronto para um PR?
   ```

3. O agente QA começa a trabalhar!
4. Leia o relatório fornecido quando o trabalho terminar.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao fluxo de trabalho e revisou seu trabalho. Nesta lição, você:

- explorou como um agente personalizado funciona com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente QA e revisou suas conclusões em relação à issue de filtragem.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, [reunirá tudo em um PR do recurso e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
