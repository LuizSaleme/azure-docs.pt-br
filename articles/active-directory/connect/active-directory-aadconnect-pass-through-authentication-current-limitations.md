---
title: 'Azure AD Connect: autenticação de passagem – Limitações atuais | Microsoft Docs'
description: Este artigo descreve as limitações atuais da autenticação de passagem do Azure AD (Azure Active Directory)
services: active-directory
keywords: Autenticação de Passagem do Azure AD Connect, instalar o Active Directory, componentes necessários para o Azure AD, SSO, Logon único
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a112e2f201109b71b7bab1c2b344ec4fcf2a851c
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39627637"
---
# <a name="azure-active-directory-pass-through-authentication-current-limitations"></a>Autenticação de passagem do Azure Active Directory: limitações atuais

>[!IMPORTANT]
>A Autenticação de Passagem do Azure AD (Azure Active Directory) é um recurso gratuito e você não precisa de nenhuma edição paga do Azure AD para usá-lo. A autenticação de Passagem só está disponível na instância mundial do Azure AD, e não na [nuvem do Microsoft Azure Alemanha](http://www.microsoft.de/cloud-deutschland) nem na [nuvem do Microsoft Azure Governamental](https://azure.microsoft.com/features/gov/).

## <a name="supported-scenarios"></a>Cenários com suporte

Há suporte para os cenários a seguir:

- Entradas de usuário em aplicativos com base em navegador da Web.
- Entradas do usuário para clientes do Outlook usando protocolos herdados, como Exchange ActiveSync, EAS, SMTP, POP e IMAP.
- Entradas de usuário para aplicativos cliente herdados do Office e aplicativos do Office compatíveis com [autenticação moderna](https://aka.ms/modernauthga): versões do Office 2010, 2013 e 2016.
- Entradas de usuário para aplicativos de protocolo herdados, como o PowerShell versão 1.0 e outros.
- Ingressos do Azure AD para dispositivos Windows 10.
- Senhas de aplicativo para Autenticação Multifator.

## <a name="unsupported-scenarios"></a>Cenários sem suporte

Os cenários a seguir _não_ têm suporte:

- Detecção de usuários com [credenciais vazadas](../reports-monitoring/concept-risk-events.md#leaked-credentials).
- Azure AD Domain Services precisa de sincronização de Hash de senha para ser habilitado no locatário. Portanto, os locatários que _somente_ usam a autenticação de passagem não funcionam para cenários que precisam de Azure AD Domain Services.
- A Autenticação de passagem não está integrada com [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md).

>[!IMPORTANT]
>Como uma solução alternativa para cenários sem suporte _apenas_ (exceto integração do Azure AD Connect Health), habilite a Sincronização de Hash de Senha na página [Recursos Opcionais](active-directory-aadconnect-get-started-custom.md#optional-features) no assistente do Azure AD Connect. Quando os usuários entram nos aplicativos listados na seção "cenários sem suporte", essas solicitações de entrada específicas _não_ são tratadas por Agentes de autenticação de passagem e, portanto, não serão registradas no [Logs de autenticação de passagem](active-directory-aadconnect-troubleshoot-pass-through-authentication.md#collecting-pass-through-authentication-agent-logs).

>[!NOTE]
Habilitar a Sincronização de Hash de Senha dará a opção de fazer failover da autenticação se a sua infraestrutura local sofrer uma interrupção. Esse failover da Autenticação de Passagem para a Sincronização de Hash de Senha não é automático. Você precisará alterar manualmente o método de entrada usando o Azure AD Connect. Se o servidor que está executando o Azure AD Connect ficar inativo, será preciso a ajuda do Suporte da Microsoft para desativar a Autenticação de passagem.

## <a name="next-steps"></a>Próximas etapas
- [Início rápido](active-directory-aadconnect-pass-through-authentication-quick-start.md): instale e execute com a Autenticação de Passagem do Azure AD.
- [Migrar do AD FS para Autenticação de Passagem](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx) – um guia detalhado para migrar do AD FS (ou outras tecnologias de federação) para Autenticação de Passagem.
- [Bloqueio Inteligente](../authentication/howto-password-smart-lockout.md): saiba como configurar a capacidade de Bloqueio Inteligente no seu locatário para proteger as contas de usuário.
- [Análise técnica aprofundada](active-directory-aadconnect-pass-through-authentication-how-it-works.md): entenda como funciona o recurso de Autenticação de passagem.
- [Perguntas frequentes](active-directory-aadconnect-pass-through-authentication-faq.md): obtenha respostas para perguntas frequentes sobre o recurso de Autenticação de Passagem.
- [Solução de problemas](active-directory-aadconnect-troubleshoot-pass-through-authentication.md): saiba como resolver problemas comuns com o recurso de Autenticação de Passagem.
- [Aprofundamento em segurança](active-directory-aadconnect-pass-through-authentication-security-deep-dive.md): obtenha informações técnicas avançadas sobre o recurso de Autenticação de passagem.
- [SSO contínuo do Azure AD](active-directory-aadconnect-sso.md): saiba mais sobre esse recurso complementar.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): use o Fórum do Azure Active Directory para arquivar novas solicitações.
