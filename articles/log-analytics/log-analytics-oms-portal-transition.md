---
title: Portal do OMS migrando para o Azure | Microsoft Docs
description: O portal do OMS está sendo desativado com toda a sua funcionalidade migrando para o portal do Azure. Este artigo fornece detalhes sobre essa transição.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/27/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: b9fb32f4f014f8e0fb67b558a2806d74edaac56c
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39576008"
---
# <a name="oms-portal-moving-to-azure"></a>Portal do OMS migrando para o Azure

> [!NOTE]
> Este artigo aplica-se a nuvem pública do Azure e a nuvem do governo, exceto quando indicado o contrário.

Um feedback recebido repetidamente dos clientes do Log Analytics é a necessidade de uma única experiência do usuário para monitorar e gerenciar cargas de trabalho locais e do Azure. Provavelmente você sabe que o portal do Azure é o hub para todos os serviços do Azure e oferece uma rica experiência de gestão, com funcionalidades como painéis para fixação de recursos, pesquisa inteligente para localização de recursos e marcação para gerenciamento de recursos. Para consolidar e simplificar o fluxo de trabalho de monitoramento e gerenciamento, começamos adicionando as funcionalidades do portal do OMS ao portal do Azure. Estamos felizes em anunciar que a maioria dos recursos do portal do OMS agora fazem parte do portal do Azure. Na verdade, alguns dos novos recursos, como análise de tráfego só estão disponíveis no portal do Azure. Existem apenas algumas lacunas restantes, incluindo algumas soluções que ainda estão em processo de ser movidos para o portal do Azure. Se não estiver usando esses recursos, você poderá fazer tudo o que você estava fazendo no portal do OMS com o portal do Azure e muito mais. Se você ainda não fez isso, recomendamos que comece a usar o portal do Azure hoje mesmo! 

Esperamos preencher as lacunas restantes entre os dois portais até agosto de 2018. Com base nos comentários dos clientes, anunciaremos o cronograma para a desativação do portal do OMS. Estamos animados com a migração para o portal do Azure e esperamos que a transição seja fácil. No entanto, sabemos que alterações desse tipo são difíceis e que podem gerar inconvenientes. Enviar quaisquer perguntas, comentários ou preocupações para **LAUpgradeFeedback@microsoft.com**. O restante deste artigo aborda os cenários principais, as lacunas atuais e o roteiro para essa transição. 

## <a name="progress"></a>Andamento
A seguir estão as atualizações que foram concluídas desde as versões anteriores deste artigo.

### <a name="july-27"></a>27 de julho

- [Análise de DNS](log-analytics-dns.md) agora está totalmente funcional no portal do Azure.
- [Gerenciamento de atualizações](../automation/automation-update-management.md) foi atualizado para ser totalmente funcional no portal do Azure. Consulte [Migrar suas implantações de atualização do OMS para o Azure](../automation/migrate-oms-update-deployments.md) para obter detalhes.
- Os [Alertas](#changes-to-alerts) foram totalmente estendidos para o portal do Azure.
- [O recurso de visualização de registros personalizados](log-analytics-data-sources-custom-logs.md) agora é ativado automaticamente para todos os espaços de trabalho.

## <a name="what-will-change"></a>O que será alterado? 
As alterações a seguir estão sendo anunciadas com a substituição do portal do OMS. Cada uma dessas alterações é descrita mais detalhadamente nas seções a seguir.

- Você poderá criar novos espaços de trabalho somente no portal do Azure.
- A nova experiência de gerenciamento de alertas substituirá a solução de Gerenciamento de Alertas.
- O gerenciamento de acesso será realizado no portal do Azure usando o controle de acesso baseado em função do Azure.
- O Application Insights Connector não é mais necessário, pois a mesma funcionalidade pode ser ativada por meio de consultas de espaço de trabalho cruzado.
- O aplicativo móvel do OMS será preterido. 
- A solução NSG está sendo substituída com funcionalidade aprimorada disponível por meio da solução de Análise de Tráfego.
- Novas conexões do System Center Operations Manager para o Log Analytics exigem pacotes de gerenciamento atualizados.


## <a name="current-known-gaps"></a>Lacunas conhecidas atualmente 
Atualmente, há algumas lacunas de funcionalidade que exigem que você use o Portal do OMS. Essas lacunas estão sendo eliminadas, e esse documento será atualizado adequadamente. Você também deve fazer referência a [Atualizações do Azure](https://azure.microsoft.com/updates/?product=log-analytics) para comunicados contínuos sobre extensões e alterações.

- As soluções a seguir ainda não estão totalmente funcionais no portal do Azure. Você deve continuar a usar essas soluções no portal clássico do até que sejam atualizadas.

    - Soluções do Windows Analytics ([Upgrade Readiness](https://technet.microsoft.com/itpro/windows/deploy/upgrade-readiness-get-started), [Integridade do Dispositivo](https://docs.microsoft.com/windows/deployment/update/device-health-monitor), e [Conformidade de Atualizações](https://technet.microsoft.com/itpro/windows/manage/update-compliance-get-started))
    - [Hub de Superfície](log-analytics-surface-hubs.md)

-  Para acessar recursos do Log Analytics no Azure, o acesso deve ser permitido ao usuário por meio do [acesso baseado em função do Azure](#user-access-and-role-migration).


## <a name="what-should-i-do-now"></a>O que devo fazer agora?  
Você deve referir-se às [perguntas comuns para a transição do portal do OMS para o portal do Azure para usuários do Log Analytics](../log-analytics/log-analytics-oms-portal-faq.md) para obter informações sobre como fazer a transição para o portal do Azure. Se as [lacunas descritas acima](#current-known-gaps) não se aplicam ao seu ambiente, considere a possibilidade de começar a usar o portal do Azure como sua experiência primária. Envie quaisquer perguntas, comentários ou dúvidas para **LAUpgradeFeedback@microsoft.com**.

A maioria dos recursos continuará a funcionar sem executar qualquer migração. As exceções estão listadas abaixo.

- Veja [Migrar suas implantações de atualização do OMS para o Azure](../automation/migrate-oms-update-deployments.md) para obter detalhes sobre a transição da solução de Gerenciamento de Atualizações. 

## <a name="new-workspaces"></a>Novos espaços de trabalho
A partir de 29 de julho, você não poderá mais criar novos espaços de trabalho usando o portal do OMS. Siga as orientações [criar um espaço de trabalho do Log Analytics no portal do Azure](log-analytics-quick-create-workspace.md) para criar um novo espaço de trabalho no portal do Azure.

## <a name="changes-to-alerts"></a>Alterações a alertas 

### <a name="alert-extension"></a>Extensão de alerta  

> [!NOTE]
> Os alertas agora foram totalmente estendidos para o portal do Azure. As regras de alerta existentes podem ser visualizadas no portal do OMS, mas elas só podem ser gerenciadas no portal do Azure. Extensão de alertas no portal do Azure será iniciada para a nuvem de governo do Azure em outubro de 2018.

Os alertas estão em processo de serem [estendidos no portal do Azure](../monitoring-and-diagnostics/monitoring-alerts-extend.md). Quando isso for concluído, as ações de gerenciamento de alertas só estarão disponíveis no portal do Azure. Os alertas existentes continuarão sendo listados no portal do OMS. Se você acessar os alertas de forma programática usando a API REST de Alerta do Log Analytics ou o Modelo de Recurso de Alerta do Log Analytics, use grupos de ação em vez de ações nas chamadas à API, modelos do Azure Resource Manager e comandos do PowerShell.

### <a name="alert-management-solution"></a>solução de Gerenciamento de Alertas
Em vez da [solução de gerenciamento de alertas](log-analytics-solution-alert-management.md), você pode usar a [interface de alertas unificada do Azure Monitor](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) para visualizar e gerenciar seus alertas. Essa nova experiência agrega alertas de várias fontes no Azure, incluindo alertas de log do Log Analytics. Você pode ser distribuições de seus alertas, tirar proveito de agrupamento automatizado de alertas relacionados por meio de grupos inteligentes, bem como exibir alertas entre várias assinaturas durante a aplicação de filtros avançados. Todos esses recursos estão disponíveis em versão prévia desde 4 de junho de 2018. A solução de gerenciamento de alertas não estará disponível no portal do Azure. 

Os dados coletados pela solução de Gerenciamento de Alertas (registros com um tipo de alerta) continuarão no Log Analytics enquanto a solução estiver instalada para o espaço de trabalho. A partir de agosto de 2018, o streaming de alertas dos alertas unificados nos espaços de trabalho será habilitado, substituindo essa funcionalidade. Algumas alterações de esquema são esperadas e serão apresentadas em uma data posterior.

## <a name="user-access-and-role-migration"></a>Acesso do usuário e migração de função
O gerenciamento de acesso ao portal do Azure é mais avançado e mais poderoso do que o gerenciamento de acesso no Portal do OMS, mas exige algumas conversões. Consulte [Gerenciar espaços de trabalho](log-analytics-manage-access.md#manage-accounts-and-users) para obter detalhes sobre o gerenciamento de acesso no Log Analytics.

A partir de 30 de julho, a conversão automática das permissões de controle de acesso do portal do OMS para as permissões do portal do Azure será iniciada. Depois que a conversão for concluída, a seção de gerenciamento de usuário do Portal do OMS roteará os usuários para o controle de acesso (IAM) no Azure. 

Durante a conversão, o sistema verificará cada usuário ou grupo de segurança que tiver permissões no portal do OMS e determinará se ele tem o mesmo nível ou permissões no Azure. Se qualquer permissão estiver ausente, ele atribuirá as funções a seguir para os espaços de trabalho e soluções relevantes.

| Permissão de portal do OMS | Função do Azure |
|:---|:---|
| ReadOnly | Leitor do Log Analytics |
| Colaborador | Colaborador do Log Analytics |
| Administrador | Proprietário |

Para certificar-se de que não sejam atribuídas permissões em excesso aos usuários, o sistema não atribuirá automaticamente essas permissões no nível do grupo de recursos. Como resultado, os administradores do espaço de trabalho devem atribuir manualmente a si mesmos as funções de _proprietário_ ou _colaborador_ no nível de assinatura ou de grupo de recursos para executar as ações a seguir.

- Adicionar ou remover soluções
- Definir novas exibições personalizadas
- Gerenciar alertas 

Em alguns casos, a conversão automática não é capaz de aplicar a permissão e solicita que o administrador atribua as permissões manualmente.

## <a name="oms-mobile-app"></a>Aplicativo móvel do OMS
O aplicativo móvel do OMS será desativado juntamente com o portal do OMS. Em lugar do aplicativo móvel do OMS, para acessar informações sobre sua infraestrutura de IT, painéis e consultas salvas, você pode acessar o portal do Azure diretamente do navegador em seu dispositivo móvel. Para obter alertas, você deve configurar [Grupos de Ação do Azure](../monitoring-and-diagnostics/monitoring-action-groups.md) para receber notificações na forma de SMS ou uma chamada de voz

## <a name="application-insights-connector-and-solution"></a>Conector do Application Insights e solução
O [Conector do Application Insights](log-analytics-app-insights-connector.md) fornece uma maneira de colocar os dados do Application Insights em um espaço de trabalho do Log Analytics. Essa duplicação de dados era necessária para permitir a visibilidade entre dados de aplicativos e de infraestrutura.

Com o suporte de [consultas entre recursos](log-analytics-cross-workspace-search.md), não há mais essa necessidade de duplicar dados. Assim, a solução Application Insights existente será preterida. A partir de julho, você não poderá vincular os novos recursos do Application Insights a espaços de trabalho do Log Analytics. Os painéis e os links existentes continuarão a funcionar até novembro de 2018.


## <a name="azure-network-security-group-analytics"></a>Análise de Grupo de Segurança de Rede do Azure
A [solução de Análise do Grupo de Segurança de Rede do Azure](log-analytics-azure-networking-analytics.md#azure-network-security-group-analytics-solution-in-log-analytics) será substituída pela recém-lançada [Análise de Tráfego](https://azure.microsoft.com/en-in/blog/traffic-analytics-in-preview/) que fornece visibilidade sobre a atividade de usuário e do aplicativo em redes de nuvem. A Análise de Tráfego ajuda você a auditar a atividade de rede de sua organização, proteger aplicativos e dados, otimizar o desempenho da carga de trabalho e permanecer em conformidade. 

Esta solução analisa logs de fluxo NSG e fornece insights sobre os elementos a seguir.

- Fluxos de tráfego em suas redes entre o Azure e a Internet, regiões de nuvem pública, VNETs e sub-redes.
- Aplicativos e protocolos em sua rede, sem a necessidade de sniffers (farejadores) ou dispositivos de coleta de fluxo dedicados.
- Principais talkers, aplicativos chatty, conversas de VM na nuvem, pontos de acesso de tráfego.
- Origens e destinos do tráfego entre VNETs, inter-relacionamentos entre serviços críticos de negócios e aplicativos.
- Segurança, incluindo tráfego malicioso, portas abertas para a Internet, aplicativos ou VMs que tentam acessar a Internet.
- Utilização de capacidade, que lhe ajuda a eliminar problemas de sobreprovisionamento ou subutilização.

Você pode continuar a contar com as Configurações de Diagnóstico para enviar logs de NSG para o Log Analytics para que suas pesquisas salvas, alertas e painéis existentes continuem a funcionar. Os clientes que já instalaram a solução podem continuar a usá-la até nova ordem. Iniciando em 15 de agosto, a solução de análise do grupo de segurança de rede será removida do marketplace e disponibilizada por meio da comunidade como um [modelo de início rápido do Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Operationalinsights).

## <a name="system-center-operations-manager"></a>System Center Operations Manager
Se você [conectou seu grupo de gerenciamento do Operations Manager ao Log Analytics](log-analytics-om-agents.md), ele continuará funcionando sem alterações. No entanto, para novas conexões, você deve seguir as orientações em [Pacote de Gerenciamento do Microsoft System Center Operations Manager para configurar o Operations Management Suite](https://blogs.technet.microsoft.com/momteam/2018/07/25/microsoft-system-center-operations-manager-management-pack-to-configure-operations-management-suite/).

## <a name="next-steps"></a>Próximas etapas
- Consulte [Perguntas comuns para a transição do portal do OMS para o portal do Azure para usuários do Log Analytics](log-analytics-oms-portal-faq.md) para obter diretrizes sobre como fazer a transição do portal do OMS para o portal do Azure.