---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de determinada maneira, é importante que essa orientação esteja disponível para o Copilot. Os [arquivos de instrução][instruction-files] descrevem não apenas *o que* você quer no código, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instrução por caminho chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de codificação sejam seguidos.
- observar o impacto dos arquivos de instrução no código.

## Cenário

Como qualquer boa empresa de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Eles incluem:

- Os comentários devem explicar a intenção e as decisões não óbvias, em vez de repetir o código.
- Funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, parâmetros e valores de retorno com TSDoc/JSDoc, incluindo o argumento injetável `db` quando houver.
- Componentes Astro reutilizáveis devem documentar seus contratos de `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- A formatação e as orientações de lint existentes devem ser preservadas.

Com os arquivos de instrução, você garante que o Copilot tenha as informações necessárias para realizar tarefas de acordo com as práticas destacadas.

## Arquivos de instrução

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Esse recurso ajuda a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de codificação, bibliotecas e até os tipos de comentários que deseja incluir no código. Também pode criar instruções para todo o repositório ou para tipos específicos de arquivos, com contexto no nível da tarefa.

Existem dois tipos de arquivos de instrução:

- `.github/copilot-instructions.md`, um único arquivo de instrução enviado ao Copilot para **todas** as solicitações do repositório. Ele deve conter informações de nível de projeto relevantes para a maioria das solicitações.
- Arquivos `.github/instructions/*.instructions.md`, que fornecem diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instrução personalizados deste projeto

O projeto inicial já inclui um conjunto de arquivos de instrução. Antes de fazer uma alteração, vamos explorar o que já existe para ver o impacto.

1. Volte ao seu Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Explore o arquivo, observando a breve descrição do projeto e as orientações de codificação. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Você verá instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um padrão glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instrução e arquivos existentes do projeto. Assim, conjuntos grandes de instruções podem ser divididos em arquivos menores e reutilizáveis, além de indicar ao Copilot exemplos a seguir.

## Atualizar os arquivos de instrução para corresponder às orientações da equipe

Embora os arquivos existentes sejam um bom começo, ainda falta uma orientação. Vamos modificar o arquivo principal `copilot-instructions.md` para garantir que o novo código TypeScript inclua comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção intitulada **Code formatting guidance**, que deve estar por volta da linha 35.
2. Adicione o seguinte como último item da lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente!

## Usar a orientação atualizada

O Copilot CLI carrega as instruções do repositório quando uma conversa é iniciada. Depois de salvar as instruções atualizadas, saia e retome a conversa de filtragem para que o Copilot comece com a nova orientação sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme se a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot para aplicar a orientação atualizada:

   ```plaintext
   Acabamos de atualizar nossas instruções e as orientações de código. Você pode atualizar o código gerado para seguir essas orientações?
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme se explicam o código com precisão.

## Resumo e próximos passos

Você explorou como o Copilot CLI coleta o contexto dos arquivos de instrução e aplicou um novo padrão ao recurso. Especificamente, você:

- explorou como as instruções do repositório e os arquivos de instrução por caminho chegam ao agente.
- atualizou o arquivo de instruções para garantir que os padrões de codificação sejam seguidos.
- observou o impacto dos arquivos de instrução no código.

Em seguida, você [personalizará e executará a skill reutilizável `quality-checks`][next-lesson] para garantir que linting e testes sejam executados.

## Recursos

- [Adicionar instruções personalizadas para o Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot: uma coleção de arquivos de instrução e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
