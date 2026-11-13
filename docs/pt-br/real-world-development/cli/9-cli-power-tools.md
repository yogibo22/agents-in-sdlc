---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para gerenciar contexto, selecionar modelos, compartilhar sessões e delegar tarefas opcionalmente para a nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como qualquer boa ferramenta de linha de comando, o GitHub Copilot CLI inclui muitos comandos de barra para interagir com ele. Esses comandos expõem funcionalidades avançadas, informações internas e opções adicionais de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usar `/model` para explorar os modelos disponíveis.
- aprender como `/share` pode exportar ou compartilhar uma sessão.
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação para um agente de nuvem.

## Cenário

Você concluiu o fluxo principal do CLI. Agora vamos conhecer outras capacidades: gerenciar contexto, alternar modelos, compartilhar sessões e, opcionalmente, delegar trabalho para o [agente de nuvem do Copilot][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Ao trabalhar em tarefas maiores ou mais complexas, você pode atingir o limite de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário, e você pode inspecioná-la ou compactá-la por conta própria com comandos de barra.

1. Volte ao seu Codespace e inicie o Copilot CLI na raiz do repositório, se ele ainda não estiver aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e como o contexto é dividido entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare o resultado. Talvez não haja uma mudança significativa se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente quando a janela fica cheia. Use `/compact` quando quiser escolher o momento. Use `/clear` ou `/new` ao mudar para uma tarefa não relacionada e quiser iniciar uma conversa nova.

## Escolher um modelo

Modelos diferentes têm pontos fortes diferentes, e cada desenvolvedor tem suas preferências. O Copilot CLI permite listar os modelos disponíveis e selecionar o que deseja usar.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todos a melhorar o uso de ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um gist do GitHub.

1. Digite `/help` e revise as opções de `/share` na versão instalada.
2. Se quiser compartilhar esta sessão, escolha o destino que melhor atenda às suas necessidades, como `/share file` para uma exportação local em Markdown.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações de sessão podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que sua equipe não pretenda compartilhar.

## Opcional: ampliar ou delegar

O workshop principal está concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar uma área de trabalho Git isolada em uma worktree separada para outra tarefa.
- `/delegate` pode enviar uma tarefa para o agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais porque podem criar worktrees adicionais ou trabalho remoto. Antes de experimentá-los, comece com uma tarefa nova e bem definida e revise o resultado com seu fluxo de trabalho habitual. Se quiser aprender mais sobre trabalho assíncrono com agentes, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Usar comandos de barra no Copilot CLI permite configurá-lo, compartilhar sessões e ver o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usou `/model` para explorar os modelos disponíveis.
- aprendeu como `/share` pode exportar ou compartilhar uma sessão.
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação para um agente de nuvem.

Há mais comandos de barra e muito mais para explorar no Copilot CLI! Vamos encerrar nossa jornada [revisando o que aprendemos][next-lesson] e alguns próximos passos para continuar aprendendo.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
