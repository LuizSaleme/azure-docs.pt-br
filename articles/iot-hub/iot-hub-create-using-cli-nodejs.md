---
title: Criar um Hub IoT usando a CLI do Azure (azure.js) | Microsoft Docs
description: Como criar um Hub IoT do Azure usando a CLI do Azure de plataforma cruzada (azure.js).
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: kgremban
ms.openlocfilehash: 5b8a7ded940f59bba63556e844c45bd7cfb8eb63
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34632084"
---
# <a name="create-an-iot-hub-using-the-azure-cli"></a>Criar um Hub IoT usando a CLI do Azure

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Introdução

É possível usar a CLI do Azure (azure.js) para criar e gerenciar Hubs IoT do Azure de forma programática. Este artigo mostra como usar a CLI do Azure (azure.js) para criar um Hub IoT.

Você pode concluir a tarefa usando uma das seguintes versões da CLI:

* CLI do Azure (azure.js) – a CLI para os modelos de implantação clássico e de gerenciamento de recursos, conforme descrita neste artigo.
* [CLI do Azure 2.0 (az.py)](iot-hub-create-using-cli.md) – a próxima geração da CLI para o modelo de implantação de gerenciamento de recursos.

Para concluir este tutorial, você precisará do seguinte:

* Uma conta ativa do Azure. Se não tiver uma, você poderá criar uma [conta gratuita][lnk-free-trial] em apenas alguns minutos.
* [CLI do Azure 0.10.4][lnk-CLI-install] ou posterior. Se já tiver a CLI do Azure instalada, você poderá validar a versão atual no prompt de comando com o seguinte comando:

```azurecli
azure --version
```

> [!NOTE]
> O Azure tem dois modelos de implantação diferentes para criar e trabalhar com recursos: [Azure Resource Manager e clássico](../azure-resource-manager/resource-manager-deployment-model.md). A CLI do Azure deve estar no modo Gerenciador de Recursos do Azure:
>
> ```azurecli
> azure config mode arm
> ```

## <a name="set-your-azure-account-and-subscription"></a>Definir sua conta e assinatura do Azure

1. No prompt de comando, faça logon digitando o seguinte comando:

   ```azurecli
    azure login
   ```

   Use o navegador sugerido e o código para autenticar.
1. Caso tenha várias assinaturas do Azure, conectar-se ao Azure concede a você acesso a todas as assinaturas do Azure associadas às suas credenciais. Você pode exibir as assinaturas do Azure e identificar qual delas é a padrão, usando o comando:

   ```azurecli
    azure account list
   ```

   Para definir o contexto de assinatura sob o qual você deseja executar o restante dos comandos, use:

   ```azurecli
    azure account set <subscription name>
   ```

1. Caso não tenha um grupo de recursos, você poderá criar um chamado **exampleResourceGroup**:

   ```azurecli
    azure group create -n exampleResourceGroup -l westus
   ```

> [!TIP]
> O artigo [Use the Azure CLI to manage Azure resources and resource groups][lnk-CLI-arm] (Usar a CLI do Azure para gerenciar recursos e grupos de recursos do Azure) fornece mais informações sobre como usar a CLI do Azure para gerenciar recursos do Azure.

## <a name="create-an-iot-hub"></a>Crie um Hub IoT

Parâmetros requeridos:

```azurecli
azure iothub create -g <resource-group> -n <name> -l <location> -s <sku-name> -u <units>
```

* **resource-group**. O nome do grupo de recursos. O formato é alfanumérico, não diferencia maiúsculas de minúsculas, é sublinhado, com hífen e um comprimento de 1 a 64.
* **name**. O nome do Hub IoT a ser criado. O formato é alfanumérico, não diferencia maiúsculas de minúsculas, com hífen e um comprimento de 3 a 50.
* **location**. O local (região/datacenter do Azure) para provisionar o hub IoT.
* **sku-name**. O nome do sku, sendo um destes: [F1, S1, S2, S3]. Para obter detalhes sobre cada SKU, consulte [Preços do Hub IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/). Atualmente, as camadas Básicas estão disponíveis somente pelo portal. 
* **units**. O número de unidades provisionadas. Para detalhes sobre os limites da unidade, consulte [Preços do Hub IoT](https://azure.microsoft.com/pricing/details/iot-hub/).

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

Para ver todos os parâmetros disponíveis para a criação, você pode usar o comando de ajuda no prompt de comando:

```azurecli
azure iothub create -h
```

Exemplo rápido: para criar um Hub IoT chamado **exampleIoTHubName** no grupo de recursos **exampleResourceGroup**, execute o seguinte comando:

```azurecli
azure iothub create -g exampleResourceGroup -n exampleIoTHubName -l westus -k s1 -u 1
```

> [!NOTE]
> Este comando da CLI do Azure cria um Hub IoT Standard S1 pelo qual você será cobrado. Você pode excluir o hub IoT **exampleIoTHubName** usando o seguinte comando:
>
> ```azurecli
> azure iothub delete -g exampleResourceGroup -n exampleIoTHubName
> ```

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre como desenvolver para o Hub IoT, veja o seguinte artigo:

* [SDKs do IoT][lnk-sdks]

Para explorar melhor as funcionalidades do Hub IoT, consulte:

* [Uso do Portal do Azure para gerenciar o Hub IoT][lnk-portal]

<!-- Links -->
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-azure-portal]: https://portal.azure.com/
[lnk-status]: https://azure.microsoft.com/status/
[lnk-CLI-install]:../cli-install-nodejs.md
[lnk-rest-api]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-CLI-arm]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md

[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-portal]: iot-hub-create-through-portal.md 
