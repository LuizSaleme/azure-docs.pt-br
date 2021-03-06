---
title: Amostra de script da CLI do Azure – Criar um Serviço SignalR com um Serviço de Aplicativo | Microsoft Docs
description: Amostra de script da CLI do Azure – Criar Serviço SignalR com um Serviço de Aplicativo
services: signalr
documentationcenter: signalr
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.service: signalr
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: signalr
ms.date: 04/20/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 6ac1646da4c952c78bfb787b0d6ab30f4876f36a
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33766386"
---
# <a name="create-a-signalr-service-with-an-app-service"></a>Criar um Serviço SignalR com um Serviço de Aplicativo

Esse exemplo de script cria um novo recurso do Serviço Azure SignalR, que é usado para enviar atualizações de conteúdo em tempo real para os clientes. Esse script também adiciona um novo plano de serviço de aplicativo e aplicativo Web para hospedar seu aplicativo Web do ASP.NET Core que usa o Serviço SignalR. O aplicativo Web é configurado com uma Configuração de Aplicativo chamada *AzureSignalRConnectionString* para se conectar ao novo recurso de Serviço SignalR.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Se você optar por instalar e usar a CLI localmente, este artigo exigirá que seja executada a CLI do Azure versão 2.0 ou posterior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Script de exemplo

Esse script usa a extensão *signalr* para a CLI do Azure. Execute o seguinte comando para instalar a extensão *signalr* para a CLI do Azure antes de usar esse exemplo de script:

```azurecli-interactive
az extension add -n signalr
```

[!code-azurecli-interactive[main](../../../cli_scripts/azure-signalr/create-signalr-with-app-service/create-signalr-with-app-service.sh "Create a new Azure SignalR Service and Web App")]

Anote o nome real gerado para o novo grupo de recursos. Ele será mostrado na saída. Você usará esse nome de grupo de recursos quando quiser excluir todos os recursos do grupo.

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicação sobre o script

Cada comando na tabela redireciona para a documentação específica do comando. Este script usa os seguintes comandos:

| Comando | Observações |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az signalr create](/cli/azure/signalr#az-signalr-create) | Cria um recurso de Serviço Azure SignalR. |
| [az signalr key list](/cli/azure/signalr/key#az-signalr-key-list) | Lista as chaves que serão usadas pelo seu aplicativo ao enviar atualizações de conteúdo em tempo real com o SignalR. |
| [az appservice plan create](/cli/azure/appservice/plan#az-appservice-plan-create) | Cria um Plano de Serviço de Aplicativo do Azure para hospedar aplicativos web. |
| [az webapp create](/cli/azure/webapp#az-webapp-create) | Cria um aplicativo Web do Azure usando o plano de hospedagem do Serviço de Aplicativo. |
| [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) | Adiciona uma nova configuração de aplicativo para o aplicativo Web. Essa configuração de aplicativo é usada para armazenar a cadeia de conexão do SignalR. |

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre a CLI do Azure, veja a [documentação da CLI do Azure](/cli/azure).

Exemplos adicionais de script CLI do Serviço SignalR do Azure podem ser encontrados na [documentação do Serviço SignalR do Azure](../signalr-cli-samples.md).
