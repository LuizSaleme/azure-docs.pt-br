---
title: 'Azure Active Directory Domain Services: criar uma conta de serviço gerenciado de grupo| Microsoft Docs'
description: Administrar domínios gerenciados dos Serviços de Domínio do Azure Active Directory
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: e6faeddd-ef9e-4e23-84d6-c9b3f7d16567
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/26/2018
ms.author: maheshu
ms.openlocfilehash: 9bfd38b2eba3cab5012e5715ad283b9cb7b84a9b
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2018
ms.locfileid: "39505885"
---
# <a name="create-a-group-managed-service-account-gmsa-on-an-azure-ad-domain-services-managed-domain"></a>Criar uma gMSA (conta de serviço gerenciado de grupo) em um domínio gerenciado do Azure AD Domain Services
Este artigo descreve como criar contas de serviço gerenciadas em um domínio gerenciado do Azure AD Domain Services.

## <a name="managed-service-accounts"></a>Contas de serviço gerenciado
Uma sMSA (conta autônoma de serviço gerenciado) é uma conta de domínio gerenciado cuja senha é gerenciada automaticamente. Isso simplifica o gerenciamento do SPN (nome da entidade de serviço) e permite o gerenciamento delegado para outros administradores. Esse tipo de MSA (conta de serviço gerenciado) foi introduzido no Windows Server 2008 R2 e no Windows 7.

A gMSA (conta de serviço gerenciado de grupo) fornece os mesmos benefícios para muitos servidores no domínio. Todas as instâncias de um serviço hospedado em um farm de servidores precisam usar a mesma entidade de serviço para que os protocolos de autenticação mútua funcionem. Quando uma gMSA é utilizada como entidade de serviço, o sistema operacional Windows gerencia a senha da conta, em vez de depender do administrador.

**Mais informações:**
- [Visão geral das contas de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Introdução às contas de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)


## <a name="using-service-accounts-in-azure-ad-domain-services"></a>Usar contas de serviço nos serviços do Azure AD Domain Services
Os domínios gerenciados do Azure AD Domain Services são bloqueados e gerenciados pela Microsoft. Há algumas considerações importantes ao usar contas de serviço com o Azure AD Domain Services.

### <a name="create-service-accounts-within-custom-organizational-units-ou-on-the-managed-domain"></a>Criar contas de serviço em UO (unidades organizacionais) personalizada no domínio gerenciado
Não é possível criar uma conta de serviço nas unidades organizacionais internas 'AADDC Users' ou 'AADDC Computers'. [Crie uma UO personalizada](active-directory-ds-admin-guide-create-ou.md) no domínio gerenciado e, em seguida, crie contas de serviço nessa UO personalizada.

### <a name="the-key-distribution-services-kds-root-key-is-already-pre-created"></a>A chave raiz de KDS (Serviços de Distribuição de Chaves) já está pré-criada
A chave raiz de KDS (Serviços de Distribuição de Chaves) é pré-criada em um domínio gerenciado do Azure AD Domain Services. Não é necessário criar uma chave raiz de KDS e também não há privilégios para fazer isso. Não é possível exibir a chave raiz de KDS no domínio gerenciado.

## <a name="sample---create-a-gmsa-using-powershell"></a>Exemplo - criar um gMSA usando o PowerShell
O exemplo a seguir mostra como criar uma UO personalizada usando o PowerShell. É possível criar uma gMSA na UO usando o parâmetro ```-Path``` para especificar a UO.

```powershell
# Create a new custom OU on the managed domain
New-ADOrganizationalUnit -Name "MyNewOU" -Path "DC=CONTOSO100,DC=COM"

# Create a service account 'WebFarmSvc' within the custom OU.
New-ADServiceAccount -Name WebFarmSvc  `
-DNSHostName ` WebFarmSvc.contoso100.com  `
-Path "OU=MYNEWOU,DC=CONTOSO100,DC=com"  `
-KerberosEncryptionType AES128, AES256  ` -ManagedPasswordIntervalInDays 30  `
-ServicePrincipalNames http/WebFarmSvc.contoso100.com/contoso100.com, `
http/WebFarmSvc.contoso100.com/contoso100,  `
http/WebFarmSvc/contoso100.com, http/WebFarmSvc/contoso100  `
-PrincipalsAllowedToRetrieveManagedPassword CONTOSO-SERVER$
```

**Documentação do cmdlet do PowerShell:**
- [Cmdlet New-ADOrganizationalUnit](https://docs.microsoft.com/powershell/module/addsadministration/new-adorganizationalunit)
- [Cmdlet New-ADServiceAccount](https://docs.microsoft.com/powershell/module/addsadministration/New-ADServiceAccount)


## <a name="next-steps"></a>Próximas etapas
- [Criar uma UO personalizada em um domínio gerenciado](active-directory-ds-admin-guide-create-ou.md)
- [Visão geral das contas de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/group-managed-service-accounts-overview)
- [Introdução às contas de serviço gerenciado de grupo](https://docs.microsoft.com/windows-server/security/group-managed-service-accounts/getting-started-with-group-managed-service-accounts)
