---
title: 'Tutorial: Configurar Pingboard para o provisionamento automático de usuário com o Azure Active Directory | Microsoft Docs'
description: Saiba como configurar o Azure Active Directory para provisionar e desprovisionar contas de usuário automaticamente para o Pingboard.
services: active-directory
documentationcenter: ''
author: asmalser-msft
writer: asmalser-msft
manager: sakula
ms.assetid: 0b38ee73-168b-42cb-bd8b-9c5e5126d648
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.openlocfilehash: c5cc20b4f20e3a4f746ab15aa5f139c8e7201a2c
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39447159"
---
# <a name="tutorial-configure-pingboard-for-automatic-user-provisioning"></a>Tutorial: Configurar Pingboard para provisionamento automático de usuário

O objetivo deste tutorial é mostrar as etapas que você precisa executar para habilitar o provisionamento e desprovisionamento automáticos de contas de usuário do Azure AD (Azure Active Directory) para o Pingboard.

## <a name="prerequisites"></a>Pré-requisitos

O cenário descrito neste tutorial pressupõe que você já tem os seguintes itens:

*   Um locatário do Azure AD
*   Uma [Conta Pro](https://pingboard.com/pricing) do locatário do Pingboard 
*   Uma conta de usuário no Pingboard com permissões de administrador 

> [!NOTE] 
> A integração do provisionamento do Azure AD depende da [API Pingboard](`https://your_domain.pingboard.com/scim/v2`) que está disponível para a sua conta.

## <a name="assign-users-to-pingboard"></a>Atribuir usuários ao Pingboard

O Azure AD usa um conceito chamado "atribuições" para determinar quais usuários devem receber acesso aos aplicativos selecionados. No contexto do provisionamento automático de conta de usuário, somente os usuários atribuídos a um aplicativo no Azure AD são sincronizados. 

Antes de configurar e habilitar o serviço de provisionamento, você deve decidir quais usuários, no Azure AD, precisam de acesso ao aplicativo Pingboard. Então, você pode atribuir esses usuários ao seu aplicativo Pingboard seguindo as instruções aqui:

[Atribuir um usuário a um aplicativo empresarial](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-pingboard"></a>Dicas importantes para atribuir usuários ao Pingboard

Recomendamos que você atribua um único usuário do Azure AD ao Pingboard para testar a configuração de provisionamento. Você pode atribuir outros usuários mais tarde.

## <a name="configure-user-provisioning-to-pingboard"></a>Configurar o provisionamento de usuário para o Pingboard 

Esta seção orienta você sobre como conectar o Azure AD à API de provisionamento de conta de usuário do Pingboard. Você também configura o serviço de provisionamento para criar, atualizar e desabilitar contas de usuário atribuídas no Pingboard com base nas atribuições de usuário do Azure AD.

> [!TIP]
> Para habilitar o logon único baseado em SAML para o Pingboard, siga as instruções fornecidas no [Portal do Azure](https://portal.azure.com). O logon único pode ser configurado independentemente do provisionamento automático, embora esses dois recursos sejam complementares.

### <a name="to-configure-automatic-user-account-provisioning-to-pingboard-in-azure-ad"></a>Para configurar o provisionamento automático de conta de usuário para Pingboard no Azure AD

1. No [Portal do Azure](https://portal.azure.com), navegue até a seção **Azure Active Directory** > **Aplicativos Empresariais** > **Todos os aplicativos**.

1. Se você já configurou o Pingboard para logon único, pesquise sua instância do Pingboard usando o campo de pesquisa. Caso contrário, selecione **Adicionar** e procure **Pingboard** na galeria de aplicativos. Selecione **Pingboard** nos resultados da pesquisa e adicione-o à sua lista de aplicativos.

1. Selecione sua instância do Pingboard e, em seguida, selecione a guia **Provisionamento**.

1. Defina o **Modo de Provisionamento** como **Automático**.

    ![Provisionamento de Pingboard](./media/pingboard-provisioning-tutorial/pingboardazureprovisioning.png)
    
1. Na seção **Credenciais de Administrador**, realize as seguintes etapas:

    a. Em **URL do Locatário**, insira `https://your_domain.pingboard.com/scim/v2` e substitua "your_domain" pelo seu domínio real.

    b. Entre no [Pingboard](https://pingboard.com/) usando sua conta do administrador.

    c. Selecione **Complementos** > **Integrações** > **Azure Active Directory**.

    d. Acesse a guia **Configurar** e selecione **Habilitar provisionamento de usuário do Azure**.

    e. Copie o token em **Token de Portador OAuth** e insira-o em **Token Secreto**.

1. No Portal do Azure, selecione **Testar Conectividade** para verificar se o Azure AD pode se conectar ao aplicativo Pingboard. Se a conexão falhar, verifique se sua conta do Pingboard tem permissões de administrador e tente a etapa **Testar Conectividade** novamente.

1. Insira o endereço de email de uma pessoa ou um grupo que você deseja que receba notificações de erro de provisionamento no campo **Email de Notificação**. Marque a caixa de seleção logo abaixo.

1. Clique em **Salvar**. 

1. Na seção **Mapeamentos**, selecione **Sincronizar usuários do Azure Active Directory com o Pingboard**.

1. Na seção **Mapeamentos de Atributo**, verifique os atributos do usuário a serem sincronizados entre o Azure AD e o Pingboard. Os atributos selecionados como propriedades de **Correspondência** são usados para associar as contas de usuário no Pingboard para operações de atualização. Para confirmar eventuais alterações, selecione **Salvar**. Para obter mais informações, consulte [Personalizar mapeamentos de atributo de provisionamento do usuário](../active-directory-saas-customizing-attribute-mappings.md).

1. Para habilitar o serviço de provisionamento do Azure AD para o Pingboard, na seção **Configurações**, altere o **Status de Provisionamento** para **Ativado**.

1. Selecione **Salvar** para iniciar a sincronização inicial de usuários atribuídos ao Pingboard.

A sincronização inicial leva mais tempo do que as sincronizações posteriores, que ocorrem aproximadamente a cada 40 minutos, desde que o serviço esteja em execução. Use a seção **Detalhes de Sincronização** para monitorar o andamento e seguir os links para os logs da atividade de provisionamento. Os logs descrevem todas as ações executadas pelo serviço de provisionamento no aplicativo Pingboard.

Para saber mais sobre como ler os logs de provisionamento do Azure AD, consulte [Relatórios sobre o provisionamento automático de contas de usuário](../active-directory-saas-provisioning-reporting.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Gerenciar provisionamento de conta de usuário para aplicativos empresariais](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configurar Logon Único](pingboard-tutorial.md)
