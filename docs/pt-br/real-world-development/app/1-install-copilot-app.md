---
title: "Lição 1 - Instalar o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele foi criado com base no GitHub Copilot CLI e tem integração nativa com o GitHub, portanto seus repositórios, branches e pipelines de CI funcionam sem configuração adicional. Ele foi projetado para fluxos de trabalho nos quais você orienta vários agentes em paralelo, cada um em seu próprio espaço de trabalho isolado, em vez de fazer todo o trabalho por conta própria, além de automatizar tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, instale o aplicativo e conecte o repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar seu projeto ao aplicativo por meio do repositório do GitHub.
- conhecer o espaço de trabalho, incluindo o backlog criado pelo modelo.
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e mesclar pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instalar e configurar o aplicativo GitHub Copilot

Há versões do aplicativo para Windows, macOS e Linux. Vamos instalá-lo, autenticar sua conta e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe e instale o aplicativo para sua plataforma seguindo as instruções da página.
3. Abra o aplicativo.
4. Selecione **Sign in to GitHub** e siga as instruções para entrar com uma conta do GitHub.com ou GHE.com. Se você usa o GitHub Enterprise Cloud com residência de dados, escolha **GitHub Enterprise Cloud** e informe o nome de host `*.ghe.com` quando solicitado.

   Se você usa o GitHub Enterprise Server, primeiro conclua a integração com sua conta na nuvem. Depois, abra **Settings**, selecione **Accounts**, selecione **Add account** e escolha **GitHub Enterprise Server**. Siga as instruções para autorizar o aplicativo no navegador, conforme o [guia de login no Enterprise Server][ghes-sign-in].

5. Se o processo de integração oferecer a seleção de repositórios, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Escolha um tema quando solicitado e conclua a integração.
7. Procure seu repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Informe o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme se o repositório aparece na barra lateral. Neste layout, as pastas de repositório, como `tailspin-toys`, ficam agrupadas em **Sessions**, e as sessões individuais aparecem sob cada repositório.

## Conhecer o espaço de trabalho

Com o projeto conectado, conheça as áreas do espaço de trabalho, organizadas na barra lateral:

- **New**: inicia uma nova sessão de chat com o Copilot.
- **Pull requests**: seus pull requests, exibidos pela integração nativa com o GitHub. Você pode procurá-los e filtrá-los, verificar o status de CI e revisá-los sem sair do aplicativo.
- **Issues**: suas issues, também integradas ao GitHub. Você pode procurá-las e filtrá-las e iniciar uma sessão a partir de uma issue.
- **Automations**: tarefas de agente salvas que são executadas em uma agenda ou sob demanda. Elas ajudam a gerenciar listas de tarefas, manutenção regular do projeto e outras atividades repetitivas. O resumo inclui links para esse recurso como próximo passo, não como outro exercício do workshop.
- **Customize**: adiciona recursos ao aplicativo na forma de servidores MCP, plugins, skills e outros componentes. Você usará essa área para configurar o Playwright MCP.
- **Chats**: conversas rápidas para perguntas e brainstorming que não precisam de branch ou espaço de trabalho próprios. Essa seção aparece depois do primeiro chat. Se não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions**: agrupa as pastas de repositório e as sessões de agentes. Selecione o nome do repositório, como `tailspin-toys`, para abrir a visualização do repositório, ou selecione uma sessão para voltar à conversa. Cada sessão é executada em um espaço de trabalho isolado, permitindo trabalhar em várias sessões sem conflitos. Você iniciará sua primeira sessão ao adicionar classificações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho.

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se não souber como fazer algo ou se algo é possível, ele poderá orientar você.

### Localizar o backlog criado pelo modelo

Quase todo projeto tem um backlog, e a Tailspin Toys não é exceção. Vamos explorar o backlog criado quando você usou o modelo.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual. Depois, selecione **Issues** na navegação superior do repositório.
2. Encontre estas issues pelo título, em vez de presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de concluir uma primeira alteração rápida.

> [!NOTE]
> As listas de **Issues** e **Pull requests** são filtradas automaticamente para exibir somente itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Experimentar um chat rápido

Uma ótima maneira de conhecer o aplicativo é perguntar sobre o *próprio aplicativo*. Um **chat rápido** é ideal para isso: você pode fazer perguntas ou pensar em ideias sem criar uma branch ou worktree, o que é perfeito para uma pergunta rápida e descartável, sem iniciar uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois.
2. Pergunte como funcionam as sessões do aplicativo:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Cada sessão é executada em seu próprio git worktree isolado, o que permite trabalhar com vários agentes em paralelo sem conflitos. Você pode continuar a conversa ou iniciar outro chat a qualquer momento.

## Resumo e próximos passos

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar no GitHub.
- adicionar um projeto por meio do repositório do GitHub.
- conhecer o espaço de trabalho e localizar o backlog criado pelo modelo em **Issues**.
- usar um chat rápido para fazer uma pergunta simples e descartável.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] e a usará para exibir uma classificação por estrelas nos cards dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
