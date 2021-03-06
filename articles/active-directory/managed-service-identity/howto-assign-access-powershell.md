---
title: Como atribuir um acesso MSI a um recurso do Azure usando o PowerShell
description: Instruções passo a passo sobre como atribuir um MSI em um recurso, acesso a outro recurso, usando o PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 6d503526ed97dd1f61269acd83810cd44598d72f
ms.sourcegitcommit: 6116082991b98c8ee7a3ab0927cf588c3972eeaa
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2018
ms.locfileid: "33929122"
---
# <a name="assign-a-managed-service-identity-msi-access-to-a-resource-using-powershell"></a>Atribuir um acesso de MSI (Identidade de Serviço Gerenciado) a um recurso usando o PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Depois de configurar um recurso do Azure com um MSI, você pode conceder o acesso de MSI a outro recurso, assim como qualquer entidade de segurança. Este exemplo mostra como conceder o acesso de MSI da máquina de virtual do Azure a uma conta de Armazenamento do Azure usando o PowerShell.

## <a name="prerequisites"></a>pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Instale também o [Azure PowerShell versão 4.3.1](https://www.powershellgallery.com/packages/AzureRM/4.3.1), se ainda não tiver feito isso.

## <a name="use-rbac-to-assign-the-msi-access-to-another-resource"></a>Usar o RBAC para atribuir o acesso de MSI a outro recurso

Depois de habilitar o MSI em um recurso do Azure, [como uma VM do Azure](qs-configure-powershell-windows-vm.md):

1. Entrar no Azure usando o cmdlet `Connect-AzureRmAccount`. Use uma conta que está associada à assinatura do Azure sob a qual você configurou o MSI:

   ```powershell
   Connect-AzureRmAccount
   ```
2. Neste exemplo, fornecemos um acesso da VM do Azure para uma conta de armazenamento. Primeiro, usamos [Get-AzureRMVM](/powershell/module/azurerm.compute/get-azurermvm) para obter a entidade de serviço da VM chamada "myVM", que foi criada ao habilitarmos o MSI. Em seguida, usamos [New-AzureRmRoleAssignment](/powershell/module/AzureRM.Resources/New-AzureRmRoleAssignment) para dar a VM acesso de "Leitor" para uma conta de armazenamento chamada "myStorageAcct":

    ```powershell
    $spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
    New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
    ```

## <a name="troubleshooting"></a>solução de problemas

Se o MSI para o recurso não aparecer na lista de identidades disponíveis, verifique se ele foi habilitado corretamente. Em nosso caso, podemos voltar à VM do Azure no [Portal do Azure](https://portal.azure.com) e:

- Examine a página "Configuração" e certifique-se de que o MSI esteja habilitado = "Sim".
- Examine a página "Extensões" e verifique se a extensão do MSI foi implantada com êxito.

Se um deles estiver incorreto, reimplante o MSI em seu recurso ou solucione a falha de implantação.

## <a name="related-content"></a>Conteúdo relacionado

- Para obter uma visão geral do MSI, confira [Visão geral da Identidade de Serviço Gerenciado](overview.md).
- Para habilitar o MSI em uma VM do Azure, confira [Configurar um MSI (identidade do serviço gerenciada) usando o PowerShell](qs-configure-powershell-windows-vm.md).

Use a seção de comentários a seguir para fornecer seus comentários e nos ajudar a aprimorar e adaptar nosso conteúdo.

