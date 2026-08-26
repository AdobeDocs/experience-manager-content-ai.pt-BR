---
title: Comece a usar a Pesquisa com IA de Conteúdo do AEM
description: 'Este guia explica como habilitar a pesquisa em seu site com a IA de conteúdo: conecte seu conteúdo e escolha um componente de pesquisa para apresentá-lo aos visitantes.'
topic: Configuration
role: Developer, Admin
level: Beginner
solution: Experience Manager
keywords: IA de conteúdo do AEM, Pesquisa com IA de conteúdo do AEM, GenSearch, Pesquisa rápida, Fontes de IA de conteúdo, Aquisição, Cloud Manager
source-git-commit: 51fa66b5ac0ef77e438db76530788826da65f91e
workflow-type: ht
source-wordcount: '1487'
ht-degree: 100%

---


# Comece a usar a Pesquisa com IA de Conteúdo do AEM

A pesquisa tradicional no site compara as palavras digitadas pelo visitante com as palavras do seu conteúdo. Isso funciona bem quando os visitantes usam a mesma terminologia que seu conteúdo, mas detalha o momento em que fazem uma pergunta, expressam uma intenção ou simplesmente expressam coisas de forma diferente. A pesquisa é um dos sinais mais claros da intenção do visitante em um site, portanto, uma correspondência malsucedida geralmente significa uma jornada malsucedida: o conteúdo não é descoberto, o engajamento diminui e as conversões são perdidas. Os visitantes esperam cada vez mais que a pesquisa entenda o que querem dizer, e não apenas o que digitaram — e é essa mesma base orientada pela intenção que possibilita as respostas generativas.

A Pesquisa com IA de conteúdo do AEM não substitui a experiência de pesquisa do seu site. Ela a aprimora, passando da correspondência de palavras-chave à compreensão do significado e da intenção, para responder diretamente às perguntas. A pesquisa semântica adiciona uma recuperação orientada pela intenção à sua experiência de pesquisa existente, apresentando conteúdo relevante mesmo quando uma consulta não contém as palavras exatas do conteúdo. A Pesquisa generativa se baseia nessa mesma estrutura de recuperação para produzir respostas contextuais e geradas com base no próprio conteúdo do site — uma etapa distinta, não a mesma coisa que a recuperação semântica.

Para os visitantes, isso significa maior relevância, suporte à linguagem natural, menos pesquisas sem resultado e respostas mais rápidas. Para sua empresa, isso significa uma correspondência melhor com a intenção, uma descoberta de conteúdo mais eficiente e uma base de pesquisa pronta para IA, sem precisar reconstruir sua experiência de pesquisa do zero. E, para sua equipe, é uma atualização incremental: seu componente de pesquisa existente pode evoluir da capacidade lexical para a semântica e, depois, para os recursos generativos, passo a passo, em vez de exigir uma implementação totalmente nova.

Chegar lá se resume a duas decisões: como seu conteúdo chega à IA de conteúdo e qual componente o apresenta aos visitantes. Conecte seu conteúdo e adicione um componente de pesquisa a uma página, e seu site estará pronto para fornecer aos visitantes os resultados mais relevantes e respostas baseadas em intenções.

## Pré-requisitos {#prerequisites}

Antes de começar, verifique se as seguintes condições foram atendidas:

* Você tem um programa ativo do Cloud Manager com pelo menos um ambiente do AEM as a Cloud Service.
* Seu usuário está atribuído ao perfil de produto **[!UICONTROL Usuários do AEM]** (para exibir fontes de conteúdo) e/ou **[!UICONTROL Administradores do AEM]** (para criá-las e editá-las), atribuído na camada **publicar** — a IA de conteúdo indexa conteúdo publicado, não conteúdo não autorizado. Consulte [Atribua um usuário a um perfil de produto do AEM](contentsources.md#assign-product-profile) para ver o procedimento completo.
* O perfil de produto do ambiente foi provisionado no **Adobe Admin Console**.

>[!NOTE]
>
>Somente o acesso ao Cloud Manager não é suficiente. Um usuário também precisa de um perfil de produto do AEM atribuído no nível de publicação para exibir ou gerenciar fontes de conteúdo.

## Etapa 1a — Conectar um índice existente {#option-a}

Os índices de repositórios existentes aparecem automaticamente na lista Fonte de conteúdo como Tipo de origem AEM — mostrado pelo que eles indexam, como Páginas, Ativos ou Fragmentos de conteúdo. Eles começam coma **Restritos** e bloqueados, ainda não podendo ser pesquisados pela IA de conteúdo.

1. Entre no [Cloud Manager](https://my.cloudmanager.adobe.com/), selecione seu programa e abra a guia **[!UICONTROL Configuração da IA de Conteúdo]** do ambiente que deseja configurar.
1. Encontre a origem que deseja pesquisar (por exemplo, **Páginas**) e selecione o ícone de cadeado. Somente usuários com o perfil de produto **[!UICONTROL Administradores do AEM]** podem fazer isso — **[!UICONTROL Usuários do AEM]** podem exibir fontes de conteúdo,mas não alterar sua capacidade de pesquisa.
1. Ler **Tornar a origem pesquisável?** caixa de diálogo com atenção. Ele avisa que as listas de controle de acesso (ACLs) do Apache Oak não serão aplicadas a esse índice depois que ele se tornar pesquisável — qualquer usuário autenticado poderá recuperar todo o conteúdo. Verifique **Compreendo que os controles de acesso (ACLs) não são aplicados e que todo o conteúdo desta origem poderá ser pesquisado**; em seguida, selecione **Tornar pesquisável**.
1. Confirme que o status muda para **Disponível**. Um ícone de aviso permanece ao lado da origem como um lembrete permanente de que as ACLs são ignoradas por ele.
1. Execute uma pesquisa de teste para verificar se os resultados retornam corretamente.

>[!WARNING]
>
>Tornar um índice existente pesquisável dessa maneira ignora completamente as ACLs do Apache Oak para essa origem — qualquer usuário autenticado pode recuperar todo o conteúdo por meio da pesquisa, independentemente das permissões normais do repositório. Faça isso somente para origens cujo conteúdo você se sinta confortável em expor integralmente.

>[!NOTE]
>
>Esse caminho é adequado se você já tiver um índice com o conteúdo do seu site — por exemplo, o conteúdo das suas páginas. Use esse índice em vez de configurar um mecanismo de rastreamento separado.

## Etapa 1b - Rastrear um site {#option-b}

Use esse caminho se ainda não tiver um índice de pesquisa para o site. O próprio rastreador da IA de conteúdo cria e atualiza um para você. Esse processo de rastree também é chamado de **aquisição** no Cloud Manager e neste guia.

1. Abrir a guia **[!UICONTROL Configuração da IA de Conteúdo]**, da mesma forma que na Etapa 1a.
1. Selecione **[!UICONTROL Criar origem]** e preencha os campos. Somente usuários com o perfil de produto **[!UICONTROL Administradores do AEM]** podem adicionar novas fontes de conteúdo.

   | Texto | Descrição |
   | --- | --- |
   | **[!UICONTROL Nome da configuração da IA de conteúdo]** | Um identificador exclusivo para esta origem. Não pode ser alterado após a criação. |
   | **[!UICONTROL Endereço do site]** | O URL raiz a ser rastreado, por exemplo, `https://www.example.com/`. |
   | **[!UICONTROL Excluir URLs]** | *(Opcional)* padrões de URL a serem ignorados durante o rastreamento. |
   | **[!UICONTROL Frequência de atualização]** | Semanalmente, diariamente, 4 vezes ao dia, a cada 60 min ou a cada 15 min. |

1. Selecione **[!UICONTROL Criar fonte]**. A aquisição é iniciada automaticamente e a fonte é movida para **Indexação**.
1. Monitore o status até que ele atinja **Disponível**:

   | Status | Significado |
   | --- | --- |
   | **Novo** | Origem recém-criada; a aquisição automática ainda não começou. |
   | **Indexação** | Rastreamento e indexação em andamento. |
   | **Disponível** | Indexação concluída — pronto para atender consultas de pesquisa. |

1. Selecione o ícone **pesquisar** ao lado da origem e execute uma consulta de teste para confirmar se o conteúdo foi indexado corretamente.

>[!CAUTION]
>
>Uma origem paralisada na **[!UICONTROL Indexação]**? Tente primeiro a aquisição novamente no menu (...). Se ainda assim não avançar, confirme se o endereço do site está acessível publicamente e se os padrões **[!UICONTROL Excluir URLs]** não estão filtrando todas as páginas.

## Etapa 2 — Escolher um componente de pesquisa {#choose-component}

Há dois componentes que podem adicionar pesquisa a uma página, desenvolvidos com base em diferentes fundamentos:

| | Pesquisa rápida (v3) com Pesquisa semântica | Pesquisa com IA de conteúdo do AEM |
| --- | --- | --- |
| Foundation | Componente principal de Pesquisa rápida existente, atualizado para v3 | Novo componente independente — chama as APIs da IA de conteúdo diretamente |
| Origem de conteúdo | O conteúdo existente do site, já em um índice, foi enriquecido para correspondência semântica | Uma origem de IA de conteúdo (Etapa 1a ou 1b) |
| Resposta generativa | Não — melhora apenas a qualidade da correspondência da lista de resultados existente | Sim — resumo opcional gerado por IA com fontes e um aviso de isenção de responsabilidade |
| Melhor ajuste | Sites que já usam a Pesquisa rápida e querem uma atualização incremental e mais simples | O componente recomendado para aproveitar todo o conjunto de recursos da IA de conteúdo — pesquisa semântica, pesquisa generativa e pesquisa em linguagem natural (NLS) |

## Pesquisa rápida (v3) com Pesquisa semântica {#quicksearch}

Se o seu site já usa o componente clássico de Pesquisa rápida do [!DNL AEM], a v3 adiciona uma opção de aceitação da **Pesquisa com IA** para que os visitantes possam ativá-la; não é necessário um novo componente, proxy ou fonte de conteúdo.

* A pesquisa continua sendo executada pelo mesmo caminho JCR/QueryBuilder de hoje — nada muda no servlet de resultados nem na forma como os resultados são renderizados.
* Quando o visitante ativa o botão de alternância o componente adiciona um marcador especial à consulta para direcioná-la à correspondência semântica, em vez da pesquisa de texto completo simples por palavras-chave.
* Não há resumo de resposta generativa neste caminho. Isso melhora a qualidade de correspondência da lista de resultados existente; não adiciona uma resposta de IA generativa.
* **A Etapa 1 (integração da IA de conteúdo) não se aplica a este caminho.** Não há nenhuma Fonte de conteúdo para criar ou conectar — este componente consulta diretamente o índice de páginas existente.

>[!NOTE]
>
>Se a pesquisa semântica não estiver funcionando como esperado depois de habilitar o botão de alternância, gere um tíquete de suporte.

Esse caminho é uma boa opção se você quiser uma atualização de pesquisa semântica incremental sem adotar um novo componente ou Fontes de conteúdo. Esse não é o caminho correto se você deseja a experiência de resposta generativa; use a Pesquisa com IA de conteúdo do AEM para isso.

## Pesquisa com IA de conteúdo do AEM {#gensearch}

A Pesquisa com IA de conteúdo do AEM é um Componente principal do [!DNL AEM] que permite que os visitantes pesquisem uma Fonte de conteúdo diretamente de uma página, com recursos de pesquisa semântica e pesquisa generativa.

>[!VIDEO](https://video.tv.adobe.com/v/3497308)

>[!NOTE]
>
>Os recursos de pesquisa generativa são adquiridos separadamente por meio de um SKU de IA. Entre em contato com seu representante de vendas da Adobe para habilitar esse recurso para sua conta.

### Pré-requisitos {#gensearch-prerequisites}

* [!DNL AEM] Componentes principais instalados em seu projeto.
* Pelo menos uma Fonte de conteúdo já foi criada e está com o status **Disponível**.
* A configuração OSGi **do cliente de IA de conteúdo do AEM** (`ContentAIClientImpl`) foi definida para autor e publicação, com uma credencial de API válida e uma Fonte de conteúdo padrão.

Para ver o guia completo de configuração — disponibilizar o componente para os autores, configurá-lo na biblioteca de clientes e definir a caixa de diálogo — consulte a [documentação de Componentes principais](https://www.adobe.com/go/aem_cmp_library_br).

## Parabéns! {#congratulations}

Você configurou com sucesso os recursos de pesquisa semântica e generativa.

>[!VIDEO](https://video.tv.adobe.com/v/3497306)

## Próximas etapas {#next-steps}

* [Configurar um projeto no Adobe Developer Console](setup-adc-project.md) — crie o projeto do ADC e as credenciais necessárias para chamar diretamente a API de IA de conteúdo.
* [Referência da API de IA de conteúdo](https://developer.adobe.com/experience-cloud/experience-manager-apis/api/experimental/contentai/) — consulte o conteúdo indexado usando pontos de acesso de pesquisa semântica, generativa ou híbrida.
* [Documentação dos Componentes principais](https://www.adobe.com/go/aem_cmp_library_br) — saiba mais sobre componentes proxy e políticas de modelos.
