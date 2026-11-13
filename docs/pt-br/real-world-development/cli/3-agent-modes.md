---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para concordar com uma abordagem, o Autopilot para construir a filtragem a partir de uma issue e o modo Interativo para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao nosso projeto. No entanto, alterações maiores exigem um processo mais robusto. Felizmente, o GitHub Copilot CLI foi projetado para funcionar com o fluxo de trabalho existente de uma organização e ajudar a construir as coisas certas da maneira correta. Esta é a primeira de várias lições em que você seguirá um processo típico de desenvolvimento orientado por agentes: usará uma issue para gerar um novo recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, garantirá que ele seja integrado corretamente ao projeto.

> [!NOTE]
> Você continuará usando a mesma conversa e a mesma branch ao longo do fluxo de trabalho do recurso. Normalmente, você usaria branches ou PRs diferentes para os vários tipos de arquivos em que trabalharia, mas usaremos um atalho para manter o foco nos conceitos principais.

Para começar, nesta lição você vai:

- iniciar uma nova conversa do Copilot a partir de uma issue do GitHub.
- definir requisitos no modo Plan.
- implementar o novo recurso usando o modo Autopilot.
- revisar o código.
- validar o recurso manualmente em um navegador encaminhado.

À medida que avançar nesse recurso, você atualizará as instruções do repositório, personalizará a skill existente `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser acordados antes de começar a codificar. Você usará o modo Plan para resolver essas decisões e, em seguida, autorizará uma implementação limitada com o Autopilot.

## Contexto

Introduzir agentes de codificação com IA no seu fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! A maioria dos desenvolvedores segue um fluxo semelhante a este:

1. Começar por uma issue registrada que descreva o trabalho necessário.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente o novo recurso.
6. Criar um pull request (PR).
7. Depois que o código for revisado e o processo de integração contínua for concluído com sucesso, mesclar o código.

> [!NOTE]
> Os detalhes exatos podem variar de acordo com sua equipe e organização, mas a maioria seguirá alguma variação desse processo.

Seguir essa abordagem padrão ajuda a garantir que o código gerado por IA atenda aos requisitos estabelecidos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: Você e o agente trabalham juntos. O agente sugere alterações e aguarda sua resposta antes de continuar.
- **Plan**: O agente cria um plano primeiro e fica impedido de editar os arquivos do projeto.
- **Autopilot**: O agente trabalha de forma autônoma: escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e use o Autopilot para implementá-lo.

## Começar pela issue

Antes de começar o trabalho de filtragem, volte ao seu Codespace e verifique se o repositório e o terminal estão prontos.

1. Volte ao seu Codespace. Se ele estiver parado, reinicie-o antes de continuar.
2. Confirme se o PR de avaliação por estrelas foi mesclado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize o `main` e, em seguida, crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as teclas de seta para destacar a issue intitulada **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe como o prompt agora começa com `#7` (ou um número semelhante). O caractere `#` permite incluir uma issue ou um pull request (PR) do GitHub no contexto.

## Planejar o recurso de filtragem

O planejamento permite definir a abordagem para implementar um recurso ou realizar tarefas antes de entregá-las ao Copilot. É sempre uma boa ideia reservar um tempo para planejar algo complexo. Vamos mudar para o modo Plan e pedir ao Copilot para criar o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme se o indicador abaixo do prompt exibe **Plan**.
2. Após a referência à issue adicionada na etapa anterior, digite a seguinte solicitação:

   ```plaintext
   Crie um plano para implementar esse recurso.
   ```

O Copilot começa a preparar o plano. Primeiro, ele explora o projeto e, em seguida, determina a melhor abordagem.

3. Durante o processo, o Copilot pode fazer perguntas sobre como as opções de filtragem devem funcionar. Responda de acordo com suas preferências. Não há respostas erradas!
4. Quando o plano estiver concluído, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Role para cima e para baixo para revisar o plano.
6. Peça ao Copilot para revisar qualquer parte do plano que não corresponda às suas decisões.

## Aprovar o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo! Deixe o Copilot trabalhar usando o Autopilot.

O Autopilot permite que o Copilot itere na tarefa até considerar que ela está concluída.

1. Selecione **Accept plan and build on autopilot (recommended)** ou uma opção com texto semelhante na versão instalada.
2. Confirme se o indicador abaixo do prompt exibe **Autopilot**.
3. Observe enquanto o Copilot segue o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que há dependências ausentes ou conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revisar e verificar a implementação

Depois que o código for gerado, ele precisa ser revisado antes de ser mesclado, assim como qualquer outro código. Vamos revisar o código e iniciar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme se o indicador não exibe mais **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Quando terminar a revisão do código, pressione <kbd>Esc</kbd> para sair da tela de diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot para corrigir qualquer falha.

## Explorar o novo recurso

O código parece bom, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada dos Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa testar o recurso de filtragem no meu navegador. Informe a URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste a filtragem por categoria, por editora e pelas combinações acordadas no plano.
4. Confirme se o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao seu Codespace e peça ao Copilot para interromper o servidor de desenvolvimento iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de uma issue do GitHub.
- definiu requisitos no modo Plan.
- implementou o novo recurso usando o modo Autopilot.
- revisou o código.
- validou o recurso manualmente em um navegador encaminhado.

Em seguida, você vai analisar melhor como o código é gerado e confirmar se ele segue práticas documentadas usando [instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
