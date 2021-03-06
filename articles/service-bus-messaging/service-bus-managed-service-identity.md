---
title: Versão prévia da Identidade de Serviço Gerenciada com o Barramento de Serviço do Azure | Microsoft Docs
description: Usar Identidades de Serviço Gerenciadas com o Barramento de Serviço do Azure
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2018
ms.author: sethm
ms.openlocfilehash: 30df312e349bd6f6ebd1f38141075382be2522a2
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39397977"
---
# <a name="managed-service-identity-preview"></a>Identidade de Serviço Gerenciada (versão prévia)

Uma MSI (Identidade de Serviço Gerenciada) é um recurso do Azure que permite criar uma identidade segura associada à implantação na qual o código do aplicativo é executado. Em seguida, você pode associar essa identidade a funções de controle de acesso que concedem permissões personalizadas para acessar os recursos do Azure específicos que seu aplicativo precisa.

Com a MSI, a plataforma Azure gerencia essa identidade de tempo de execução. Você não precisa armazenar e proteger as chaves de acesso no código ou na configuração do aplicativo, seja para a identidade ou para os recursos que você precisa acessar. Um aplicativo cliente do Barramento de Serviço em execução dentro de um aplicativo de Serviço de Aplicativo do Azure ou em uma máquina virtual com suporte de MSI habilitado não precisa lidar com regras e chaves de SAS nem com nenhum outro token de acesso. O aplicativo cliente precisa apenas do endereço do ponto de extremidade do namespace das Mensagens do Barramento de Serviço. Quando o aplicativo se conecta, o Barramento de Serviço associa o contexto de MSI ao cliente em uma operação que será mostrada em um exemplo mais adiante neste artigo. 

Quando um cliente do Barramento de Serviço está associado a uma Identidade de Serviço Gerenciada, ele pode executar todas as operações autorizadas. A autorização é concedida ao associar uma MSI a funções do Barramento de Serviço. 

## <a name="service-bus-roles-and-permissions"></a>Permissões e funções do Barramento de Serviço

Na versão prévia pública inicial, é possível adicionar apenas uma Identidade de Serviço Gerenciada nas funções "Proprietário" ou "Colaborador" de um namespace de Barramento de Serviço, que concede controle total à identidade em todas as entidades no namespace. No entanto, as operações de gerenciamento que alteram a topologia do namespace são compatíveis inicialmente apenas com o Azure Resource Manager e não com a interface de gerenciamento REST nativa do Barramento de Serviço. Isso também significa que não é possível usar o objeto cliente [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) do .NET Framework dentro de uma Identidade de Serviço Gerenciada.

## <a name="use-service-bus-with-a-managed-service-identity"></a>Usar o Barramento de Serviço com uma Identidade de Serviço Gerenciada

A seção a seguir descreve as etapas necessárias para criar e implantar um aplicativo de exemplo que é executado em uma Identidade de Serviço Gerenciada, como conceder a essa identidade o acesso a um namespace das Mensagens do Barramento de Serviço e como o aplicativo interage com as entidades do Barramento de Serviço usando essa identidade.

Esta introdução descreve um aplicativo Web hospedado no [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/). As etapas necessárias para um aplicativo hospedado em VM são semelhantes.

### <a name="create-an-app-service-web-application"></a>Criar um aplicativo Web do Serviço de Aplicativo

A primeira etapa é criar um aplicativo ASP.NET do Serviço de Aplicativo. Se você não estiver familiarizado com a forma de fazer isso no Azure, siga [este guia de instruções](../app-service/app-service-web-get-started-dotnet-framework.md). No entanto, em vez de criar um aplicativo MVC, conforme é mostrado no tutorial, crie um aplicativo Web Forms.

### <a name="set-up-the-managed-service-identity"></a>Configurar a Identidade de Serviço Gerenciada

Depois de criar o aplicativo, navegue até o aplicativo Web recém-criado no portal do Azure (também mostrado nas instruções) e, em seguida, navegue até a página **Identidade de Serviço Gerenciada** e habilite o recurso: 

![](./media/service-bus-managed-service-identity/msi1.png)

Depois de habilitar o recurso, uma nova identidade do serviço será criada no Azure Active Directory e configurada no host do Serviço de Aplicativo.

### <a name="create-a-new-service-bus-messaging-namespace"></a>Criar um novo namespace das Mensagens do Barramento de Serviço

Em seguida, [crie um namespace das Mensagens do Barramento de Serviço](service-bus-create-namespace-portal.md) em uma das regiões do Azure que tenham suporte para a versão prévia do RBAC: **Leste dos EUA**, **Leste dos EUA 2** ou **Europa Ocidental**. 

Navegue até a página **Controle de Acesso (IAM)** do namespace no portal e, em seguida, clique em **Adicionar** para adicionar a Identidade de Serviço Gerenciada à função **Proprietário**. Para fazer isso, procure o nome do aplicativo Web no campo **Selecionar** do painel **Adicionar permissões** e, em seguida, clique na entrada. Em seguida, clique em **Salvar**.

![](./media/service-bus-managed-service-identity/msi2.png)
 
A Identidade de Serviço Gerenciada do aplicativo Web agora tem acesso ao namespace do Barramento de Serviço e à fila criada anteriormente. 

### <a name="run-the-app"></a>Execute o aplicativo

Agora, modifique a página padrão do aplicativo ASP.NET que você criou. Também é possível usar o código do aplicativo Web [deste repositório GitHub](https://github.com/Azure-Samples/app-service-msi-servicebus-dotnet).  

A página Default.aspx é sua página de aterrissagem. O código pode ser encontrado no arquivo Default.aspx.cs. O resultado é um aplicativo Web mínimo com alguns campos de entrada e os botões **enviar** e **receber** que se conectam ao Barramento de Serviço para enviar ou receber mensagens.

Observe como o objeto [MessagingFactory](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) é inicializado. Em vez de usar o provedor de token SAS (Token de Acesso Compartilhado), o código cria um provedor de token para a Identidade de Serviço Gerenciada com a chamada `TokenProvider.CreateManagedServiceIdentityTokenProvider(ServiceAudience.ServiceBusAudience)`. Dessa forma, não há nenhum segredo a ser retido e uso. O fluxo do contexto da Identidade de Serviço Gerenciada para o Barramento de Serviço e o handshake de autorização são manipulados automaticamente pelo provedor de token, que é um modelo mais simples do que o uso do SAS.

Depois de fazer essas alterações, publique e execute o aplicativo. Uma maneira fácil de obter os dados de publicação correto é baixar e, em seguida, importar um perfil de publicação no Visual Studio:

![](./media/service-bus-managed-service-identity/msi3.png)
 
Para enviar ou receber mensagens, insira o nome do namespace e o nome da entidade que você criou e, em seguida, clique em **enviar** ou **receber**.


> [!NOTE]
> - A identidade de serviço gerenciado funciona somente dentro do ambiente do Azure, em serviços de aplicativos, VMs do Azure e nos conjuntos de dimensionamento. Para aplicativos .NET, a biblioteca Microsoft.Azure.Services.AppAuthentication, que é usada pelo pacote NuGet do Barramento de Serviço, fornece uma abstração sobre esse protocolo e dá suporte a uma experiência de desenvolvimento local. Essa biblioteca também permite testar o código localmente no computador de desenvolvimento, usando a conta de usuário do Visual Studio, a CLI 2.0 do Azure ou a Autenticação Integrada do Active Directory. Para obter mais informações sobre as opções de desenvolvimento local com essa biblioteca, consulte [Autenticação de serviço a serviço para o Azure Key Vault usando .NET](../key-vault/service-to-service-authentication.md).  
> 
> - Atualmente, as identidades de serviço gerenciadas não funcionam com slots de implantação do Serviço de Aplicativo.

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre as mensagens do Barramento de Serviço, confira os tópicos a seguir.

* [Conceitos fundamentais do barramento de serviço](service-bus-fundamentals-hybrid-solutions.md)
* [Filas, tópicos e assinaturas do Barramento de Serviço](service-bus-queues-topics-subscriptions.md)
* [Introdução às filas do Barramento de Serviço](service-bus-dotnet-get-started-with-queues.md)
* [Como usar tópicos e assinaturas do Barramento de Serviço](service-bus-dotnet-how-to-use-topics-subscriptions.md)