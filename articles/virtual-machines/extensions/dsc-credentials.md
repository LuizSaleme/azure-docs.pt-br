---
title: Passar credenciais para o Azure usando a Configuração de Estado Desejado
description: Saiba como passar com segurança as credenciais para máquinas virtuais do Azure usando a Configuração de Estado Desejado (DSC) do PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: DCtheGeek
manager: carmonm
editor: ''
tags: azure-resource-manager
keywords: dsc
ms.assetid: ea76b7e8-b576-445a-8107-88ea2f3876b9
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 05/02/2018
ms.author: dacoulte
ms.openlocfilehash: 666253d16ac51dcc21174211f71794f8b0ede07d
ms.sourcegitcommit: 909469bf17211be40ea24a981c3e0331ea182996
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/10/2018
ms.locfileid: "34012539"
---
# <a name="pass-credentials-to-the-azure-dscextension-handler"></a>Pssar credenciais para o manipulador de extensões DSC do Azure

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Este artigo aborda a extensão Configuração de Estado Desejado (DSC) do Azure. Para obter uma visão geral do manipulador de extensões DSC, consulte [Introdução ao manipulador de extensões Configuração de Estado Desejado do Azure](dsc-overview.md).

## <a name="pass-in-credentials"></a>Passar credenciais

Como parte do processo de configuração, talvez seja necessário configurar contas de usuário, acessar serviços ou instalar um programa em um contexto de usuário. Para fazer isso, você precisa fornecer credenciais.

Você pode usar a DSC para definir as configurações parametrizadas. Em uma configuração parametrizada, as credenciais são passadas na configuração e seguramente armazenadas em arquivos MOF. O manipulador de extensões do Azure simplifica o gerenciamento de credenciais fornecendo gerenciamento automático de certificados.

O seguinte script de configuração da DSC cria uma conta de usuário local com a senha especificada:

```powershell
configuration Main
{
    param(
        [Parameter(Mandatory=$true)]
        [ValidateNotNullorEmpty()]
        [PSCredential]
        $Credential
    )
    Node localhost {
        User LocalUserAccount
        {
            Username = $Credential.UserName
            Password = $Credential
            Disabled = $false
            Ensure = "Present"
            FullName = "Local User Account"
            Description = "Local User Account"
            PasswordNeverExpires = $true
        }
    }
}
```

É importante incluir **node localhost** como parte da configuração. O manipulador de extensão procura especificamente a instrução **node localhost**. Se essa instrução estiver ausente, as etapas a seguir não funcionam. Também é importante incluir a conversão de tipo **[PsCredential]**. Esse tipo específico dispara a extensão para criptografar as credenciais.

Para publicar esse script no armazenamento de blobs do Azure:

`Publish-AzureRmVMDscConfiguration -ConfigurationPath .\user_configuration.ps1`

Para definir a extensão DSC do Azure e fornecer a credencial:

```powershell
$configurationName = 'Main'
$configurationArguments = @{ Credential = Get-Credential }
$configurationArchive = 'user_configuration.ps1.zip'
$vm = Get-AzureRmVM -Name 'example-1'

$vm = Set-AzureRmVMDscExtension -VMName $vm -ConfigurationArchive $configurationArchive -ConfigurationName $configurationName -ConfigurationArgument @configurationArguments

$vm | Update-AzureRmVM
```

## <a name="how-a-credential-is-secured"></a>Como uma credencial é protegida

Executar esse código solicita uma credencial. Depois que a credencial é fornecida, ela é armazenada brevemente na memória. Quando a credencial é publicada usando o cmdlet **Set-AzureRmVMDscExtension**, a credencial é transmitida por HTTPS para a VM. Na VM, o Azure armazena a credencial criptografada no disco usando o certificado local da VM. A credencial é brevemente descriptografada na memória e, em seguida, criptografada novamente para passá-la à DSC.

Esse processo é diferente de [usar configurações seguras sem o manipulador de extensões](/powershell/dsc/securemof). O ambiente do Azure oferece uma forma de transmitir dados de configuração com segurança por meio de certificados. Ao usar o manipulador de extensão DSC, não é necessário fornecer **$CertificatePath** ou uma entrada **$CertificateID**/ **$Thumbprint** em **ConfigurationData**.

## <a name="next-steps"></a>Próximas etapas

- Obtenha uma [introdução ao manipulador de extensões DSC do Azure](dsc-overview.md).
- Examine o [modelo do Azure Resource Manager para a extensão de DSC](dsc-template.md).
- Para saber mais sobre a DSC do PowerShell, acesse o [centro de documentação do PowerShell](/powershell/dsc/overview).
- Para mais funcionalidade que você pode gerenciar com a DSC do PowerShell e para mais recursos de DSC, navegue na [galeria do PowerShell](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0).