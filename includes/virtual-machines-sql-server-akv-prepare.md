---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: virtual-machines-windows
author: rothja
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 0a5d0f87b31652b1e1ab32c6b1594021937751b6
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2018
ms.locfileid: "40046949"
---
## <a name="prepare-for-akv-integration"></a>Preparar-se para a integração de AKV
Para usar a integração do Cofre da Chave do Azure e configurar a VM do SQL Server, há vários pré-requisitos: 

1. [Instalar o Azure Powershell](#install)
2. [Criar um Active Directory do Azure](#register)
3. [Criar um cofre de chave](#createkeyvault)

As seções a seguir descrevem esses pré-requisitos e as informações que você precisa coletar para executar posteriormente os cmdlets do PowerShell.

### <a id="install"></a> Instalar o Azure PowerShell
Verifique se você instalou o SDK mais recente do Azure PowerShell. Para obter mais informações, confira [Como instalar e configurar o Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a id="register"></a> Registrar um aplicativo em seu Azure Active Directory

Primeiro, você precisa ter um [Active Directory do Azure](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) em sua assinatura. Entre os diversos benefícios, isso permite que você conceda permissão de acesso ao seu cofre de chave para determinados usuários e aplicativos.

Em seguida, registre um aplicativo com o AAD Isso lhe dará uma conta de Entidade de Serviço com acesso ao cofre de chaves de que sua VM precisará. No artigo Cofre de Chaves do Azure, você pode encontrar estas etapas na seção [Registrar um aplicativo com o Azure Active Directory](../articles/key-vault/key-vault-get-started.md#register), ou pode ver as etapas com capturas de tela na seção **Obter uma identidade para o aplicativo** desta [ostagem de blog](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Antes de concluir essas etapas, você precisa coletar as seguintes informações durante esse registro. Elas serão necessárias mais tarde, quando você habilitar a integração do Azure Key Vault em sua VM do SQL.

* Depois que o aplicativo é adicionado, localize o **ID do aplicativo** na folha **aplicativo registrado**.
    O ID do aplicativo é atribuído posteriormente ao parâmetro **$spName** (nome da Entidade de Serviço) no script do PowerShell a fim de habilitar a integração do Azure Key Vault.

   ![ID do aplicativo](./media/virtual-machines-sql-server-akv-prepare/aad-application-id.png)

* Durante essas etapas, ao criar a chave, copie o segredo de sua chave conforme mostra a seguinte captura de tela. Esse segredo é atribuído posteriormente ao parâmetro **$spSecret** (segredo da Entidade de serviço) no script do PowerShell.

   ![Segredo de AAD](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)

* A ID do aplicativo e o segredo também poderão ser usados para criar uma credencial do SQL Server.

* Você deve autorizar essa nova ID de cliente para ter as seguintes permissões de acesso: **get**, **wrapKey** e **unwrapKey**. Isso é feito com o cmdlet [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) . Para saber mais, consulte [Autorizar o aplicativo a usar a chave ou o segredo](../articles/key-vault/key-vault-get-started.md#authorize).

### <a id="createkeyvault"></a> Criar um cofre de chaves
Para usar o Cofre da Chave do Azure a fim de armazenar as chaves que você usará para criptografia em sua VM, você precisa ter acesso em um cofre de chave. Se você ainda não tiver configurado seu cofre de chaves, crie um usando as etapas no artigo [Introdução ao Azure Key Vault](../articles/key-vault/key-vault-get-started.md). Antes de concluir essas etapas, você precisa coletar algumas informações durante esse configuração que serão necessárias mais tarde quando você habilitar a integração do Azure Key Vault em sua VM do SQL.

    New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Quando você chega à etapa Criar um cofre de chave, observe a propriedade **vaultUri** retornada, que é a URL do cofre de chave. No exemplo fornecido nesta etapa, mostrado abaixo, o nome do cofre de chaves é ContosoKeyVault, portanto a URL do cofre de chaves seria https://contosokeyvault.vault.azure.net/.

A URL do cofre de chave será atribuída posteriormente ao parâmetro **$akvURL** no script do PowerShell a fim de habilitar a integração do Cofre da Chave do Azure.

Depois que o cofre de chaves é criado, é preciso adicionar uma chave para o cofre de chaves, esta chave será mencionada quando criamos uma chave assimétrica criar posteriormente no SQL Server.
