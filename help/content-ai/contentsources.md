---
title: Configurar e gerenciar suas fontes de IA de conteúdo
description: Saiba como configurar a IA de conteúdo do AEM no Cloud Manager configurando sua primeira fonte de conteúdo e acionando a aquisição.
topic: Configuration
role: Developer, Admin
level: Beginner
solution: Experience Manager
keywords: IA de conteúdo do AEM, Fontes de IA de conteúdo, Aquisição, Cloud Manager, Adobe Developer Console
source-git-commit: 86c0b8b910583701dc4bd42b61e082cc5429cee8
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 1%

---


# Configurar e gerenciar suas fontes de IA de conteúdo

Este guia aborda a configuração de Fontes de IA de conteúdo no Cloud Manager, desde a reunião de pré-requisitos até a criação de uma fonte de conteúdo e a confirmação da sua indexação e disponibilidade.

## Pré-requisitos {#prerequisites}

Antes de começar, verifique se as seguintes condições foram atendidas:

* Você tem um programa ativo do Cloud Manager com pelo menos um ambiente do AEM as a Cloud Service.
* Você tem a função de **[Administrador do Sistema](https://experienceleague.adobe.com/en/docs/support-resources/adobe-support-tools-guide/adobe-admin-console/admin-roles)** no Admin Console para o programa.
* O perfil de produto do ambiente foi provisionado no **Adobe Admin Console**, consulte [Configurar um Projeto do Adobe Developer Console](setup-adc-project.md).

## Etapa 1 - Abrir a guia Configuração da IA de conteúdo {#open-tab}

1. Entre no [Cloud Manager](https://my.cloudmanager.adobe.com/) e selecione seu programa.

   ![Página inicial da Cloud Manager mostrando o cartão do programa](../assets/content-ai-onboarding-step-1.png)

1. Na **[!UICONTROL Visão geral do programa]**, localize a seção **[!UICONTROL Ambientes]** e selecione o ambiente que deseja configurar.

   ![Visão geral do programa com um ambiente de produção realçado](../assets/content-ai-onboarding-step-2.png)

1. Na página de detalhes do ambiente, selecione a guia **[!UICONTROL Configuração da IA de conteúdo]**.

   ![Página de detalhes do ambiente com a guia Configuração da IA de conteúdo realçada](../assets/content-ai-onboarding-step-3.png)

## Etapa 2 - Criar um Source de IA de conteúdo {#create-source}

Uma fonte de conteúdo define o site que a IA de conteúdo rastrea e indexa.

1. Na guia **[!UICONTROL Configuração da IA de Conteúdo]**, selecione **[!UICONTROL Criar Source]**.

   ![Guia Configuração da IA de conteúdo mostrando o botão Criar Source](../assets/content-ai-onboarding-step-4.png)

1. Na caixa de diálogo **[!UICONTROL Criar/Adicionar novo Source da IA de conteúdo]**, preencha os campos:

   | Texto | Descrição |
   | --- | --- |
   | **[!UICONTROL Nome da configuração de IA de conteúdo]** | Um identificador exclusivo para esta origem (por exemplo, `my-site-index`). Não é possível alterar após a criação. |
   | **[!UICONTROL Descrição]** | *(Opcional)* Uma breve descrição da fonte de conteúdo. |
   | **[!UICONTROL Endereço do site]** | A URL raiz do site a ser rastreado (por exemplo, `https://www.example.com/`). |
   | **[!UICONTROL Excluir URLs]** | *(Opcional)* padrões de URL a serem ignorados durante a rastrea. |
   | **[!UICONTROL Frequência de atualização]** | Com que frequência a IA de conteúdo rastrea novamente a fonte: Semanalmente, Diariamente, Diariamente 4×, 60 Min ou 15 Min. |

   ![Caixa de diálogo Criar Source da IA de Conteúdo com os campos de nome e endereço do site preenchidos e o botão Criar Source realçado](../assets/content-ai-onboarding-step-5-0.png)

   ![Lista suspensa de frequência de atualização mostrando as opções disponíveis](../assets/content-ai-onboarding-step-5-1.png)

1. Selecione **[!UICONTROL Criar Source]**.

## Etapa 3 - Aquisição do acionador {#trigger-acquisition}

Após a criação da origem, seu status é **Novo**. Execute uma aquisição inicial para iniciar a indexação.

1. Na lista de origem, selecione o ícone **mais ações** (...) ao lado da origem e selecione **[!UICONTROL Aquisição de acionador]**.

   ![Lista de origens da IA de conteúdo com o menu de mais ações aberto e aquisição de acionador realçada](../assets/content-ai-onboarding-step-7.png)

1. Na caixa de diálogo **[!UICONTROL Aquisição do acionador]**, examine os detalhes da origem - **[!UICONTROL Fonte de conteúdo]**, **[!UICONTROL Última execução]** e **[!UICONTROL Próxima execução agendada]** - e selecione **[!UICONTROL Acionador]**.

   ![Caixa de diálogo de confirmação de Aquisição do gatilho](../assets/content-ai-onboarding-step-8.png)

## Etapa 4 - Monitorar status de indexação {#monitor-status}

Após o início da aquisição, o status da origem é atualizado em tempo real.

| Status | Significado |
| --- | --- |
| **Novo** | Source criado; nenhuma aquisição foi executada ainda. |
| **Indexando** | Aquisição em andamento; o conteúdo está sendo rastreado e indexado. |
| **Disponível** | A indexação foi concluída; a origem está pronta para atender às consultas de pesquisa. |

![Lista de Fontes de Conteúdo mostrando o status de Indexação](../assets/content-ai-onboarding-step-9.png)

![Lista de Fontes de Conteúdo mostrando o status Disponível](../assets/content-ai-onboarding-step-10.png)

Aguarde o status atingir **Disponível** antes de pesquisar o índice ou testar a API.

## Etapa 5 - Pesquisar conteúdo indexado {#search-content}

Quando o status da origem for **Disponível**, você poderá executar consultas de pesquisa diretamente da Cloud Manager para verificar se o conteúdo foi indexado corretamente.

1. Na lista de origem, selecione **[!UICONTROL Pesquisar]** ao lado da origem.

   ![Lista de Fontes de Conteúdo com o botão Pesquisar realçado em uma fonte disponível](../assets/content-ai-onboarding-step-13.png)

1. Insira uma consulta no campo de pesquisa. Os resultados mostram uma lista de itens correspondentes com pontuação de correspondência e tipo de conteúdo (por exemplo, **PÁGINA** ou **PDF**). Selecionar um resultado abre uma visualização à direita.

   ![Painel de pesquisa com uma consulta, resultados correspondentes com pontuações correspondentes e um painel de visualização para o resultado principal](../assets/content-ai-onboarding-step-14.png)

## Modificar ou excluir uma Source {#modify-source}

Para atualizar uma configuração de origem após sua criação:

1. Na lista de origem, selecione o ícone **mais ações** (...) ao lado da origem e selecione **[!UICONTROL Editar]**.

   ![Lista de Fontes de Conteúdo com o menu de mais ações aberto e a opção Editar realçada](../assets/content-ai-onboarding-step-11.png)

1. Na caixa de diálogo **[!UICONTROL Modificar Source da IA de Conteúdo]**, atualize a **[!UICONTROL Descrição]**, o **[!UICONTROL Endereço do site]**, o **[!UICONTROL Excluir URLs]** ou a **[!UICONTROL Frequência de atualização]** conforme necessário. O **[!UICONTROL Nome da Configuração da IA de Conteúdo]** é somente leitura e não pode ser alterado.

1. Selecione **[!UICONTROL Salvar]** para aplicar as alterações ou selecione **[!UICONTROL Excluir]** no canto inferior esquerdo da caixa de diálogo para remover a origem completamente.

   >[!WARNING]
   >
   >A exclusão de uma origem é permanente. Todo o conteúdo indexado para essa origem é removido e não pode mais atender às consultas de pesquisa.

   ![Modifique a caixa de diálogo Source da IA de Conteúdo com os campos editáveis realçados e um botão Excluir no canto inferior esquerdo](../assets/content-ai-onboarding-step-12.png)

A lista de origem é atualizada para refletir suas alterações. Se você tiver excluído a origem, ela não aparecerá mais na lista.

## Próximas etapas {#next-steps}

* [Configurar um projeto do Adobe Developer Console](setup-adc-project.md) - Crie o projeto ADC e as credenciais necessárias para chamar a API.
* [Referência da API da IA de conteúdo](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/contentai/) - Consulte seu conteúdo indexado usando pontos de extremidade de pesquisa semântica, de texto completo ou híbrida.

## Resolução de problemas {#troubleshooting}

* **O Source permanece na [!UICONTROL Indexação] por um período estendido.** Tente realizar a aquisição novamente no menu (...). Se o status não avançar após uma segunda execução, verifique se o **[!UICONTROL endereço do site]** pode ser acessado publicamente e se os padrões **[!UICONTROL Excluir URLs]** não filtram todas as páginas.
* **O Source retorna para [!UICONTROL Novo] após uma execução.** O rastreador não pôde buscar nenhuma página do URL raiz configurado. Confirme se a URL responde com `200 OK` e se o site não está bloqueando solicitações automatizadas.
* **[!UICONTROL A Pesquisa] não retorna resultados para uma origem [!UICONTROL Disponível].** Indexação bem-sucedida, mas nenhum conteúdo corresponde à consulta. Tente uma consulta mais ampla ou verifique se os URLs rastreados incluem as páginas esperadas.
