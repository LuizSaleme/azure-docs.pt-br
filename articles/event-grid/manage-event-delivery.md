---
title: Mensagens mortas e nova tentativa de políticas para as assinaturas da Grade de Eventos do Azure
description: Descreve como personalizar opções de entrega de eventos para a Grade de Eventos. Definir um destino de inatividade e especificar o tempo de entrega novamente.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 08/03/2018
ms.author: tomfitz
ms.openlocfilehash: 5a37fadc179157ba590b31a79fcd98f223cb1869
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39501942"
---
# <a name="dead-letter-and-retry-policies"></a>Mensagens mortas e tentar novas políticas

Ao criar uma assinatura de evento, você pode personalizar as configurações para entrega de eventos. É possível definir por quanto tempo a Grade de Eventos tenta entregar a mensagem. Você pode definir uma conta de armazenamento a ser usada para armazenar os eventos que não podem ser entregues a um ponto de extremidade.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="set-dead-letter-location"></a>Defina o local de mensagens mortas

Quando a Grade de Eventos não pode fornecer um evento, ela pode enviar o evento não entregue para uma conta de armazenamento. Este processo é conhecido como armazenamento de mensagens mortas. Por padrão, a Grade de Eventos não ativa o armazenamento de mensagens mortas. Para habilitá-lo, você deve especificar uma conta de armazenamento para reter eventos que não foram entregues ao criar a assinatura do evento. Você aciona eventos dessa conta de armazenamento para resolver as entregas.

A Grade de Eventos envia um evento para o local de mensagens mortas se ela tiver tentado todas as novas tentativas ou se receber uma mensagem de erro indicando que a entrega nunca terá êxito. Por exemplo, se a Grade de Eventos receber um erro de formato inadequado durante a entrega de um evento, ela esse evento para o local de mensagens mortas. Há um atraso de cinco minutos entre a última tentativa de entregar de um evento e quando ela é entregue para o local de inatividade. Esse atraso tem como objetivo reduzir o número de operações de armazenamento Blob. Se o local de inatividade não estiver disponível por quatro horas, o evento será descartado.

Antes de configurar o local de mensagens mortas, você deve ter uma conta de armazenamento com um contêiner. É possível fornecer o ponto de extremidade para esse contêiner ao criar a assinatura do evento. O ponto de extremidade está no formato de: `/subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>/blobServices/default/containers/<container-name>`

O script a seguir obtém a ID de recurso de uma conta de armazenamento existente e cria uma assinatura de evento que usa um contêiner na conta de armazenamento para o ponto de extremidade de mensagens mortas.

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

storagename=demostorage
containername=testcontainer

storageid=$(az storage account show --name $storagename --resource-group gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --deadletter-endpoint $storageid/blobServices/default/containers/$containername
```

Para usar a Grade de Eventos para responder a eventos não entregues, [crie uma assinatura de evento](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) para o armazenamento de blob de mensagens mortas. Toda vez que seu armazenamento de blob de mensagens mortas recebe um evento não entregue, a Grade de Eventos notifica o manipulador. O manipulador responde com ações que você deseja executar para reconciliar eventos não entregues. 

Para desativar a colocação em fila de mensagens mortas, execute novamente o comando para criar a assinatura de evento, mas não forneça um valor para `deadletter-endpoint`. Você não precisa excluir a assinatura do evento.

## <a name="set-retry-policy"></a>Definir a política de repetição

Ao criar uma assinatura de Grade de Eventos, você pode definir valores de por quanto tempo a Grade de Eventos deve tentar entregar o evento. Por padrão, a Grade de Eventos tenta por 24 horas (1440 minutos) e tenta um máximo de 30 vezes. Você pode definir esses valores para a sua assinatura de grade de eventos. O valor de tempo de vida do evento deve ser um inteiro de 1 a 1440. O valor de tentativas de entrega máximo deve ser um inteiro de 1 a 30.

Não é possível configurar o [intervalo de repetição](delivery-and-retry.md#retry-intervals-and-duration).

Para definir o evento de vida útil para um valor diferente de 1440 minutos, use:

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --event-ttl 720
```

Para definir o máximo de novas tentativas com um valor diferente de 30, use:

```azurecli-interactive
az eventgrid event-subscription create \
  -g gridResourceGroup \
  --topic-name <topic_name> \
  --name <event_subscription_name> \
  --endpoint <endpoint_URL> \
  --max-delivery-attempts 18
```

Se você definir `event-ttl` e `max-deliver-attempts`, a Grade de Eventos usa o primeiro a expirar para novas tentativas.

## <a name="next-steps"></a>Próximas etapas

* Para um aplicativo de exemplo que usa um aplicativo de função do Azure para processar eventos de mensagens mortas, consulte [Exemplos de Mensagens Mortas de Grade de Eventos do Azure para .NET](https://azure.microsoft.com/resources/samples/event-grid-dotnet-handle-deadlettered-events/).
* Para obter informações sobre a entrega de eventos e repetições, [Entrega e repetição de mensagens da Grade de Eventos](delivery-and-retry.md).
* Para ver uma introdução à Grade de Eventos, confira [About Event Grid](overview.md) (Sobre a Grade de Eventos).
* Para começar a usar rapidamente a Grade de Eventos, confira [Create and route custom events with Azure Event Grid](custom-event-quickstart.md) (Criar e rotear eventos personalizados com a Grade de Eventos do Azure).
