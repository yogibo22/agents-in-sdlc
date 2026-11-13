---
title: "Lição 2 - Adicionar avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards dos jogos, revise-a em um navegador encaminhado e mescle-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e tentou uma conversa, é hora de fazer sua primeira alteração no projeto. Você manterá isso pequeno: os jogos já têm uma avaliação por estrelas em seus dados, mas os cards dos jogos na página inicial ainda não a exibem. Você pedirá ao agente para exibi-la, revisará a alteração e a mesclará como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de recursos.
- pedir ao agente para fazer uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- iniciar o aplicativo para confirmar a alteração em um navegador encaminhado.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, e ela já aparece na página de detalhes do jogo. Os cards dos jogos na página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como exercício de aquecimento, você fará com que o agente exiba a avaliação existente em cada card — uma alteração pequena e autônoma, perfeita para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferente do Copilot app, uma conversa normal do CLI usa o repositório e a branch Git atualmente verificadas no terminal, em vez de criar uma worktree dedicada. Conversas salvas permitem que você volte para a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo estado Git comum no disco.

Dentro de uma conversa, você verá três coisas: suas solicitações e as respostas do agente, a atividade de ferramentas do agente enquanto explora e edita arquivos e as alterações que você pode inspecionar com `/diff`.

## Iniciar uma conversa e solicitar nossa alteração

Vamos iniciar uma nova conversa para começar a implementação do nosso recurso.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se ainda não estiver aberto, inicie o Copilot com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Garanta que uma nova sessão seja iniciada usando o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Use a seguinte solicitação para pedir a alteração:

   ```plaintext
   Mostre a starRating de cada jogo em uma escala de 5 nos cards dos jogos da página de listagem. Se a avaliação for nula, mostre "No rating yet". Mantenha o layout do card como está, adicione testes e execute as verificações relevantes.
   ```

O Copilot explora o projeto, localiza os arquivos usados para exibir os detalhes do jogo e cria o código necessário. Agora você adicionou um novo recurso com o Copilot CLI!

## Revisar o diff

Todas as alterações geradas por IA merecem revisão antes de serem mescladas, mesmo as pequenas. Vamos explorar as alterações aqui mesmo no Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme que o card do jogo exibe a avaliação numérica quando ela está presente e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes cobrem os dois estados.
4. Revise os resultados das verificações executadas pelo Copilot e peça a ele para corrigir qualquer falha.
5. Quando sua revisão estiver concluída, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como o Copilot, como todas as ferramentas de IA generativa, é probabilístico em vez de determinístico, seu código exato pode variar. Revise o comportamento em vez de esperar uma implementação exata.

## Verificar as alterações

Claro, não devemos apenas ler o código e assumir que ele funciona. Vamos pedir ao Copilot para iniciar nosso site para examinarmos a interface do usuário (UI) atualizada no navegador encaminhado pelos Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa inspecionar a alteração de avaliação por estrelas no meu navegador. Me diga a URL e deixe o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cards dos jogos exibem suas avaliações em uma escala de cinco.
4. Volte ao Copilot e peça a ele para interromper o servidor que iniciou:

   ```plaintext
   Interrompa o servidor de desenvolvimento que você iniciou.
   ```

## Abrir e mesclar seu primeiro pull request

Agora você criou o recurso! É hora de criar um pull request (PR) para mesclar o novo código no projeto.

1. Peça ao agente padrão para confirmar o commit da alteração:

   ```plaintext
   Confirme o commit das alterações de avaliação por estrelas revisadas com uma mensagem de commit apropriada.
   ```

2. Digite `/pr create`. O Copilot CLI envia o commit existente quando cria o PR e exibe a URL do PR.
3. Abra o PR segurando <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao seu Codespace e saia do Copilot CLI com `/exit`.
7. Atualize seu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você enviou sua primeira alteração usando o GitHub Copilot CLI! Especificamente, você:

- iniciou uma conversa focada do Copilot em uma branch de recursos.
- instruiu o agente a fazer uma pequena alteração nos cards dos jogos.
- revisou a alteração com `/diff`.
- iniciou o aplicativo para confirmar a avaliação por estrelas em um navegador encaminhado.
- abriu e mesclou seu primeiro pull request.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
