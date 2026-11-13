---
title: "Lição 12 - Implantar o site com o GitHub Pages"
description: "Use o aplicativo do GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar seu pull request e verificar o Space Quiz em produção."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um espaço público implantando-o com o GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas você continua responsável por revisar as alterações e confirmar que o site está em produção.

Nesta lição, você vai:

- pedir ao Copilot para configurar uma implantação do GitHub Pages.
- revisar o fluxo de trabalho e o pull request antes da mesclagem.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages o torna acessível por uma URL pública, sem adicionar um servidor ou uma dependência de runtime.

## Peça ao Copilot para implantar o site

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Habilite o GitHub Pages para este repositório. Crie um fluxo de trabalho do GitHub Actions que compile o site, se necessário, e implante-o no GitHub Pages. Adicione um README descrevendo o aplicativo e como executá-lo localmente se ainda não houver um. Crie um pull request com essas alterações e mescle-o após a aprovação dos checks obrigatórios. Acompanhe a implantação até que ela seja concluída e, em seguida, informe a URL final do site em produção. Se você não tiver permissão para habilitar o Pages ou mesclar, diga exatamente o que preciso aprovar ou fazer; não informe sucesso até que o site esteja em produção.
```

![Diagrama em três etapas: um pull request do space-quiz mesclado; uma execução bem-sucedida do GitHub Actions Deploy website com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz em produção em seu-nome.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e mescle um pull request e altere as configurações de Pages do repositório. Revise o diff, as permissões de implantação e qualquer prompt de aprovação do GitHub antes de continuar.

## Revise o fluxo de trabalho e verifique a implantação

1. Confirme se o README será adicionado somente se o repositório ainda não tiver um.
2. Revise as alterações do pull request, incluindo as etapas de compilação e implantação do fluxo de trabalho.
3. Faça a mesclagem somente depois que os checks obrigatórios do pull request forem aprovados.
4. Acompanhe a execução da implantação do GitHub Actions até a conclusão.
5. Abra a URL informada e confirme se o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou mesclar o pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site em produção carregue.

## Resumo e próximos passos

Seu Space Quiz agora está disponível como um site em produção, e você verificou a implantação em vez de confiar apenas no relatório do agente. Continue para [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
