---
title: Mapeamento de declarações de usuário de colaboração B2B no Azure Active Directory | Microsoft Docs
description: Personalize as declarações de usuário emitidas no token SAML para usuários de B2B do Microsoft Azure Active Directory (Azure AD).
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/06/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: f2617fb79fc20fcb385c7e2fc3596245ab18caba
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34266943"
---
# <a name="b2b-collaboration-user-claims-mapping-in-azure-active-directory"></a>Mapeamento de declarações de usuário de colaboração B2B no Azure Active Directory

O Azure AD (Azure Active Directory) dá suporte à personalização das declarações emitidas no token SAML para usuários da colaboração B2B. Quando um usuário é autenticado no aplicativo, o Azure AD emite um token SAML para o aplicativo que contém informações (ou declarações) sobre o usuário que o identifica com exclusividade. Por padrão, isso inclui o nome de usuário, o endereço de email, o nome e sobrenome do usuário.

No [Portal do Azure](https://portal.azure.com), você pode exibir ou editar as declarações enviadas no token SAML para o aplicativo. Para acessar as configurações, selecione **Microsoft Azure Active Directory** > **Aplicativos empresariais** > o aplicativo que está configurado para logon único > **Logon único**. Consulte as configurações de token SAML na seção **Atributos de usuário**.

![Mostra os atributos de token SAML na interface do usuário](media/claims-mapping/view-claims-in-saml-token.png)

Há dois possíveis motivos para você precisar editar as declarações emitidas no token SAML:

1. O aplicativo exige um conjunto diferente de URIs ou valores de declaração.

2. O aplicativo exige que a declaração NameIdentifier seja algo diferente do nome UPN armazenado no Microsoft Azure Active Directory.

Para obter informações sobre como adicionar e editar declarações, consulte [Personalizando declarações emitidas no token SAML para aplicativos empresariais no Azure Active Directory](../develop/active-directory-saml-claims-customization.md).

Para usuários da colaboração B2B, o mapeamento de NameID e UPN entre locatários é impedido, por motivos de segurança.

## <a name="next-steps"></a>Próximas etapas

- Para obter informações sobre as propriedades de usuário de colaboração B2B, consulte [Propriedades de um usuário de colaboração B2B do Microsoft Azure Active Directory](user-properties.md).
- Para obter informações sobre tokens de usuário para usuários de colaboração B2B, consulte [Noções básicas sobre tokens de usuário na colaboração B2B do Microsoft Azure Active Directory](user-token.md).

