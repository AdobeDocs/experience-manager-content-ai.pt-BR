---
title: Introdução às Pesquisas com IA de conteúdo do AEM
description: 'Este guia explica como habilitar a pesquisa em seu site com a IA de conteúdo: conecte seu conteúdo e escolha um componente de pesquisa para apresentá-lo aos visitantes.'
topic: Configuration
role: Developer, Admin
level: Beginner
solution: Experience Manager
keywords: IA de conteúdo do AEM, Pesquisa com IA de conteúdo do AEM, GenSearch, Pesquisa rápida, Fontes de IA de conteúdo, Aquisição, Cloud Manager
source-git-commit: 51fa66b5ac0ef77e438db76530788826da65f91e
workflow-type: tm+mt
source-wordcount: '1487'
ht-degree: 6%

---


# Introdução às Pesquisas com IA de conteúdo do AEM

A pesquisa tradicional do site corresponde as palavras que um visitante digita com as palavras do seu conteúdo. Isso funciona bem quando os visitantes usam a mesma terminologia que seu conteúdo, mas detalha o momento em que fazem uma pergunta, expressam uma intenção ou simplesmente expressam coisas de forma diferente. A pesquisa é um dos sinais mais claros da intenção do visitante em um site, portanto, uma correspondência com falha geralmente significa uma jornada com falha: o conteúdo passa despercebido, o engajamento cai e as conversões são perdidas. Os visitantes esperam, cada vez mais, que a pesquisa entenda o que eles significam, não apenas o que digitaram, e essa mesma base consciente de intenções é o que torna as respostas geradoras possíveis em primeiro lugar.

A Pesquisa com IA de conteúdo do AEM não substitui a experiência de pesquisa do seu site; ela evolui, desde a correspondência de palavras-chave até a compreensão do significado e da intenção e a resposta direta às perguntas. A Pesquisa semântica adiciona recuperação com reconhecimento de intenção a partir de sua experiência de pesquisa existente, exibindo conteúdo relevante mesmo quando uma consulta não compartilha o texto exato do conteúdo. A Pesquisa gerativa se baseia na mesma base de recuperação para produzir respostas contextuais e geradas com base no conteúdo do próprio site: uma etapa distinta, não a mesma coisa que a recuperação semântica.

Para os visitantes, isso significa melhor relevância, suporte em linguagem natural, menos pesquisas sem resultados e respostas mais rápidas. Para sua empresa, significa melhor correspondência de intenções, detecção de conteúdo mais sólida e uma base de pesquisa pronta para IA, sem reconstruir do zero sua experiência de pesquisa. E, para sua equipe, é uma atualização incremental: seu componente de pesquisa existente pode mudar de léxico, semântico, para recursos geradores passo a passo, em vez de exigir uma implementação totalmente nova.

Chegar lá se resume a duas decisões: como seu conteúdo entra na IA de conteúdo e qual componente a traz aos visitantes. Conecte seu conteúdo e adicione um componente de pesquisa a uma página, e seu site estará pronto para fornecer aos visitantes os resultados mais relevantes e respostas baseadas em intenções.

## Pré-requisitos {#prerequisites}

Antes de começar, verifique se as seguintes condições foram atendidas:

* Você tem um programa ativo do Cloud Manager com pelo menos um ambiente do AEM as a Cloud Service.
* Seu usuário está atribuído ao perfil de produto **[!UICONTROL Usuários do AEM]** (para exibir fontes de conteúdo) e/ou **[!UICONTROL Administradores do AEM]** (para criá-los e editá-los), atribuído na camada **publicar** - Índices de IA de conteúdo publicados, não conteúdo criado. Consulte [Atribuir um usuário a um perfil de produto do AEM](contentsources.md#assign-product-profile) para obter o procedimento completo.
* O perfil de produto do ambiente foi provisionado no **Adobe Admin Console**.

>[!NOTE]
>
>Somente o acesso ao Cloud Manager não é suficiente. Um usuário também precisa de um perfil de produto do AEM atribuído no nível de publicação para exibir ou gerenciar fontes de conteúdo.

## Etapa 1a - Conectar um índice existente {#option-a}

Os índices de repositório existentes são exibidos automaticamente na lista Fontes de conteúdo como Source Type AEM - mostrado pelo que indexam, como Páginas, Assets ou Fragmentos de conteúdo. Eles começam com **Restrito** e são bloqueados, mas ainda não podem ser pesquisados pela IA de conteúdo.

1. Entre no [Cloud Manager](https://my.cloudmanager.adobe.com/), selecione seu programa e abra a guia **[!UICONTROL Configuração da IA de Conteúdo]** para o ambiente que deseja configurar.
1. Localize a origem na qual deseja pesquisar (por exemplo, **Páginas**) e selecione seu ícone de bloqueio. Somente usuários com o perfil de produto **[!UICONTROL Administradores do AEM]** podem fazer isso - **[!UICONTROL Usuários do AEM]** podem exibir fontes de conteúdo, não alterar sua capacidade de pesquisa.
1. Ler a **Tornar a fonte pesquisável?** com cuidado. Ele avisa que as listas de controle de acesso (ACLs) do Apache Oak não serão impostas para esse índice depois de pesquisáveis - qualquer usuário autenticado poderá recuperar todo o conteúdo. Verifique **Compreendo que os ACLs (controles de acesso) não são aplicados e todo o conteúdo desta fonte será pesquisável**, depois selecione **Tornar pesquisável**.
1. Confirme as alterações de status para **Disponível**. Um ícone de aviso permanece ao lado da origem como um lembrete permanente de que as ACLs são ignoradas por ele.
1. Execute uma pesquisa de teste para verificar se os resultados retornam corretamente.

>[!WARNING]
>
>Tornar um índice existente pesquisável dessa maneira ignora completamente as ACLs do Apache Oak para essa origem - qualquer usuário autenticado pode recuperar todo o conteúdo por meio da pesquisa, independentemente das permissões normais do repositório. Faça isso apenas para fontes que você se sente confortável em expor na íntegra.

>[!NOTE]
>
>Esse caminho é um bom ajuste se você já tiver um índice com o conteúdo do site - por exemplo, o conteúdo da página. Use esse índice em vez de configurar um mecanismo de rastree separado.

## Etapa 1b - Rastrear um site {#option-b}

Use esse caminho se ainda não tiver um índice de pesquisa para o site. O próprio rastreador da IA de conteúdo cria e atualiza um para você. Esse processo de rastree também é chamado de **aquisição** em toda a Cloud Manager e neste guia.

1. Abra a guia **[!UICONTROL Configuração da IA de conteúdo]**, a mesma da Etapa 1a.
1. Selecione **[!UICONTROL Criar Source]** e preencha os campos. Somente usuários com o perfil de produto **[!UICONTROL Administradores do AEM]** podem adicionar novas fontes de conteúdo.

   | Texto | Descrição |
   | --- | --- |
   | **[!UICONTROL Nome da configuração da IA de conteúdo]** | Um identificador exclusivo para esta origem. Não pode ser alterado após a criação. |
   | **[!UICONTROL Endereço do site]** | A URL raiz a ser rastreada, por exemplo `https://www.example.com/`. |
   | **[!UICONTROL Excluir URLs]** | *(Opcional)* padrões de URL a serem ignorados durante o rastreamento. |
   | **[!UICONTROL Frequência de atualização]** | Semanalmente, Diariamente, Diariamente 4×, 60 Min ou 15 Min. |

1. Selecione **[!UICONTROL Criar fonte]**. A aquisição é iniciada automaticamente e a fonte é movida para **Indexação**.
1. Monitore o status até que ele atinja **Disponível**:

   | Status | Significado |
   | --- | --- |
   | **Novo** | O Source acabou de ser criado; a aquisição automática ainda não começou. |
   | **Indexação** | Rastree e indexação em andamento. |
   | **Disponível** | Indexação concluída - pronto para atender às consultas de pesquisa. |

1. Selecione o ícone **pesquisar** ao lado da origem e execute uma consulta de teste para confirmar se o conteúdo foi indexado corretamente.

>[!CAUTION]
>
>Uma origem paralisada na **[!UICONTROL Indexação]**? Tente adquirir novamente a partir do menu (...) primeiro. Se ainda não avançar, confirme se o endereço do site pode ser acessado publicamente e se os seus padrões **[!UICONTROL Excluir URLs]** não estão filtrando todas as páginas.

## Etapa 2 - Escolher um componente de pesquisa {#choose-component}

Há dois componentes que podem colocar a pesquisa em uma página, criada em bases diferentes:

| | Pesquisa rápida (v3) com Pesquisa semântica | Pesquisa com IA de conteúdo do AEM |
| --- | --- | --- |
| Foundation | Componente principal de Pesquisa rápida existente, atualizado para v3 | Novo componente independente — chama as APIs da IA de conteúdo diretamente |
| Origem de conteúdo | O conteúdo existente do site, já em um índice, foi enriquecido para correspondência semântica | Um Source de IA de conteúdo (Etapa 1a ou 1b) |
| Resposta geradora | Não - melhora a qualidade de correspondência somente da lista de resultados existente | Sim - resumo opcional gerado por IA com fontes e um aviso de isenção de responsabilidade |
| Melhor ajuste | Sites que já usam a Pesquisa rápida e que desejam uma atualização mais leve e incremental | O componente sugerido para a gama completa de recursos da IA de conteúdo — pesquisa semântica, pesquisa gerativa e pesquisa em linguagem natural (NLS) |

## Pesquisa rápida (v3) com Pesquisa semântica {#quicksearch}

Se o seu site já usa o componente de Pesquisa rápida do [!DNL AEM] clássico, a v3 adiciona uma opção de aceitação **Pesquisa com IA** para que os visitantes possam ativar a opção; não é necessário um novo componente, proxy ou Source de conteúdo.

* A pesquisa ainda é executada pelo mesmo caminho JCR/QueryBuilder como hoje - nada muda no servlet de resultado ou na renderização dos resultados.
* Quando um visitante ativa a alternância, o componente prefixa a consulta com um marcador especial que o direciona para a correspondência semântica em vez de texto completo de palavra-chave simples.
* Não há um resumo de resposta gerativa neste caminho. Isso melhora a qualidade de correspondência da lista de resultados existente; não adiciona uma resposta de IA gerativa.
* **A Etapa 1 (integração da IA de conteúdo) não se aplica a este caminho.** Não há Source de conteúdo para criar ou conectar - esse componente consulta o índice de página existente diretamente.

>[!NOTE]
>
>Se a pesquisa semântica não estiver funcionando como esperado depois de habilitar o botão, gere um tíquete de suporte.

Esse caminho é uma boa opção se você quiser uma atualização de pesquisa semântica incremental sem adotar um novo componente ou Fontes de conteúdo. Esse não é o caminho correto se você deseja a experiência de resposta gerativa; use a Pesquisa com IA de conteúdo do AEM para isso.

## Pesquisa com IA de conteúdo do AEM {#gensearch}

A Pesquisa com IA de conteúdo do AEM é um Componente principal do [!DNL AEM] que permite que os visitantes pesquisem um Source de conteúdo diretamente de uma página, com recursos de pesquisa semântica e pesquisa gerativa.

>[!VIDEO](https://video.tv.adobe.com/v/3497308)

>[!NOTE]
>
>Os recursos de pesquisa gerativa são comprados separadamente por meio de uma SKU de IA. Entre em contato com o representante de vendas da Adobe para ativá-lo para sua conta.

### Pré-requisitos {#gensearch-prerequisites}

* [!DNL AEM] Componentes principais instalados em seu projeto.
* Pelo menos uma Source de Conteúdo já foi criada e está no status **Disponível**.
* A **configuração OSGi do Cliente da IA de Conteúdo do AEM** (`ContentAIClientImpl`) foi definida no autor e na publicação, com uma credencial de API válida e uma Source de Conteúdo padrão.

Para obter o guia de instalação completo - disponibilizar o componente para autores, conectar sua biblioteca do cliente e configurar a caixa de diálogo - consulte a [documentação sobre os Componentes principais](https://www.adobe.com/go/aem_cmp_library_br).

## Parabéns! {#congratulations}

Você configurou seus recursos de pesquisa semântica e gerativa com êxito.

>[!VIDEO](https://video.tv.adobe.com/v/3497306)

## Próximas etapas {#next-steps}

* [Configurar um projeto do Adobe Developer Console](setup-adc-project.md) - Crie o projeto ADC e as credenciais necessárias para chamar a API da IA de conteúdo diretamente.
* [Referência da API da IA de conteúdo](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/contentai/) - Consulte seu conteúdo indexado usando pontos de extremidade de pesquisa semântica, generativa ou híbrida.
* [Documentação dos Componentes principais](https://www.adobe.com/go/aem_cmp_library_br) - Mais informações sobre componentes proxy e políticas de modelo.
