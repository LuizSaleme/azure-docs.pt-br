---
title: Trocar o disco do SO para uma VM do Azure usando a CLI | Microsoft Docs
description: Mude o disco do sistema operacional usado por uma máquina virtual do Azure usando a CLI.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 04/24/2018
ms.author: cynthn
ms.openlocfilehash: 1732b60ee135b765cdeea43f981bcef9575ff32c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2018
ms.locfileid: "32195969"
---
# <a name="change-the-os-disk-used-by-an-azure-vm-using-the-cli"></a>Mudar o disco do sistema operacional usado por uma VM do Azure usando a CLI


Se você tiver uma VM já existente, mas quiser trocar o disco por um disco de backup ou outro disco do sistema operacional, você pode usar a CLI do Azure para trocar os discos do SO. Você não precisa excluir e recriar a VM. Você pode até usar um disco gerenciado em outro grupo de recursos, desde que ele não esteja em uso.

A VM precisa ser interrompida\desalocada e, em seguida, a ID de recurso do disco gerenciado pode ser substituída pela ID de recurso de um disco gerenciado diferente. 

Certifique-se de que o tipo de armazenamento e o tamanho da VM sejam compatíveis com o disco que você deseja anexar. Por exemplo, se o disco que você deseja usar estiver no armazenamento Premium, a VM precisa ter capacidade de armazenamento Premium (como um tamanho da série DS).

Este tutorial requer a CLI do Azure, versão 2.0.25 ou superior. Execute `az --version` para encontrar a versão. Se você precisa instalar ou atualizar, consulte [Instalar a CLI 2.0 do Azure]( /cli/azure/install-azure-cli). 


Use [az disk list](/cli/azure/disk#list) para obter uma lista dos discos no grupo de recursos.

```azurecli-interactive
az disk list \
   -g myResourceGroupDisk \
   --query '[*].{diskId:id}' \
   --output table
```


Use [az vm stop](/cli/azure/vm#stop) para interromper\desalocar a VM antes de trocar os discos.

```azurecli-interactive
az vm stop \
   -n myVM \
   -g myResourceGroup
```


Use [az vm update](/cli/azure/vm#az-vm-update) com a ID de recurso completo do novo disco para o parâmetro `--osdisk` 

```azurecli-interactive 
az vm update \
   -g myResourceGroup \
   -n myVM \
   --os-disk /subscriptions/<subscription ID>/resourceGroups/swap/providers/Microsoft.Compute/disks/myDisk 
   ```
   
Reinicie a VM usando [az vm start](/cli/azure/vm#start).

```azurecli-interactive
az vm start \
   -n myVM \
   -g myResourceGroup
```

   
**Próximas etapas**

Para criar uma cópia de um disco, consulte [Instantâneo de um disco](snapshot-copy-managed-disk.md).