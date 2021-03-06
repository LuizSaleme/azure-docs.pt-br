---
title: Adicionar ou alterar funções de administração de assinatura do Azure | Microsoft Docs
description: 'Descreve como adicionar ou alterar o Coadministrador, Administrador de Serviços e Administrador da Conta do Azure '
services: ''
documentationcenter: ''
author: genlin
manager: jlian
editor: ''
tags: billing
ms.assetid: 13a72d76-e043-4212-bcac-a35f4a27ee26
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/14/2018
ms.author: genli
ms.openlocfilehash: 97210c0d9dba9c4130b1da9ad17a257ff1d81b42
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37448699"
---
# <a name="add-or-change-azure-subscription-administrators"></a>Adicionar ou alterar administradores de assinatura do Azure

Para gerenciar o acesso aos recursos do Azure, você deve ter a função de administrador apropriada. Este artigo descreve como adicionar ou alterar a função de administrador para um usuário no nível da assinatura.

## <a name="what-administrator-role-do-i-use"></a>Qual função de administrador usar?

O Azure tem várias funções diferentes. Para gerenciar o acesso aos recursos, você pode usar as funções de administrador de assinatura clássicos, como administrador de serviços e coadministrador ou um sistema de autorização mais recente, chamado de controle de acesso baseado em função (RBAC). Para garantir um melhor controle e simplificar o gerenciamento de acesso, é recomendável que você use o RBAC para todas as necessidades de gerenciamento de acesso. Se possível, recomendamos que você reconfigure as políticas de acesso existentes usando o RBAC. Para obter mais informações, consulte [o que é controle de acesso baseado em função (RBAC)](../role-based-access-control/overview.md) e [entender as diferentes funções no Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).

<a name="add-an-admin-for-a-subscription"></a>

## <a name="add-an-rbac-owner-for-a-subscription-in-azure-portal"></a>Adicionar um RBAC proprietário para uma assinatura no Portal do Azure 

Para adicionar alguém como administrador para uma assinatura do Azure, atribuí-los à função [proprietário](../role-based-access-control/built-in-roles.md#owner) (uma função RBAC) no escopo da assinatura. A função de proprietário pode gerenciar os recursos na assinatura que você atribuiu e não tem o privilégio de acesso a outras assinaturas.

1. Visite [**Assinaturas** no Portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
2. Selecione a assinatura à qual você deseja conceder acesso.
3. Selecione **Adicionar**.
   (Se o botão Adicionar estiver ausente, você não terá permissão para adicionar permissões.)
4. Selecione **Controle de acesso (IAM)** na lista.
5. Na caixa **Função** selecione **Proprietário**. 
6. Na caixa **Atribuir acesso a** selecione **Usuário, grupo ou aplicativo do Azure AD**. 
7. Na caixa **Selecionar**, digite o endereço de email do usuário que você deseja adicionar como Proprietário. Selecione o usuário e, em seguida, selecione **Salvar**.

    ![Captura de tela que mostra a função Proprietário selecionada](./media/billing-add-change-azure-subscription-administrator/add-role.png)

Isso concede ao usuário o acesso completo a todos os recursos, inclusive o direito de delegar acesso a outras pessoas. Para conceder acesso a um escopo diferente, como um grupo de recursos, visite a página **Controle de acesso (IAM)** desse escopo.

## <a name="add-or-change-co-administrator"></a>Adicionar ou alterar coadministrador

Somente um [Proprietário](../role-based-access-control/built-in-roles.md#owner) pode ser adicionado como um coadministrador. Outros usuários com funções como [Colaborador](../role-based-access-control/built-in-roles.md#contributor) e [Leitor](../role-based-access-control/built-in-roles.md#reader) não podem ser adicionados como coadministradores.

> [!TIP]
> Você só precisa adicionar a conta Proprietário como Coadministrador se o usuário tem que gerenciar implantações clássicas do Azure. É recomendável usar o RBAC para todas as outras finalidades.

1. Se você ainda não o fez, adicione alguém como proprietário seguindo as instruções acima.
2. **Clique com botão direito do mouse** no usuário proprietário você acabou de adicionar e, em seguida, selecione **Adicionar como coadministrador**. Se a opção **Adicionar como coadministrador** não for exibida, atualize a página ou tente usar outro navegador da Internet. 

    ![Captura de tela que adiciona o coadministrador](./media/billing-add-change-azure-subscription-administrator/add-coadmin.png)

    Para remover a permissão de Coadministrador, **clique com o botão direito do mouse** no usuário Coadministrador e, em seguida, selecione **Remover Coadministrador**.

    ![Captura de tela que remove o coadministrador](./media/billing-add-change-azure-subscription-administrator/remove-coadmin.png)

<a name="change-service-administrator-for-a-subscription"></a>

## <a name="change-the-service-administrator-for-an-azure-subscription"></a>Alterar o Administrador de Serviços de uma assinatura do Azure

Somente o Administrador da Conta pode alterar o Administrador de Serviços de uma assinatura. Por padrão, ao criar uma conta, o Administrador de Serviços também é o Administrador da Conta. Se o Administrador de Serviços for alterado para um usuário diferente, o Administrador da Conta perderá o acesso ao Portal do Azure. No entanto, o Administrador da Conta sempre poderá usar o Centro de Contas para alterar o Administrador de Serviços de volta para si mesmo.

1. Verifique se há suporte para o seu cenário, verificando os [limites para alterar os Administradores de Serviço](#limits).
1. Entre no [Centro de Contas](https://account.windowsazure.com/subscriptions) como o Administrador da Conta.
1. Selecione uma assinatura.
1. No lado direito, selecione **Editar detalhes da assinatura**.

    ![Captura de tela mostrando o botão Editar assinatura no Centro de Contas](./media/billing-add-change-azure-subscription-administrator/editsub.png)
1. Na caixa **ADMINISTRADOR DE SERVIÇOS**, insira o endereço de email do novo administrador de serviços.

    ![Captura de tela mostrando a caixa para alterar o email do Administrador de Serviços](./media/billing-add-change-azure-subscription-administrator/changeSA.png)

<a name="limits"></a>

### <a name="limitations-for-changing-service-administrators"></a>Limitações para alterar os Administradores de Serviços

* Cada assinatura está associada a um diretório do Azure AD. Para localizar o diretório com o qual a assinatura está associada, acesse [**Assinaturas**](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) e, em seguida, selecione uma assinatura para ver o diretório.
* Se você estiver conectado a uma conta corporativa ou de estudante, poderá adicionar outras contas em sua organização como Administrador de Serviços. Por exemplo, abby@contoso.com pode adicionar bob@contoso.com como Administrador de Serviços, mas não pode adicionar john@notcontoso.com, a menos que john@notcontoso.com esteja no diretório contoso.com. Usuários conectados usando contas corporativas ou de estudante podem continuar adicionando usuários da conta da Microsoft como Administradores de Serviços.

  | Método de entrada | Adicionar usuário Conta da Microsoft como um administrador de serviço? | Adicionar conta corporativa ou de estudante na mesma organização que administrador de Serviço? | Adicionar conta corporativa ou de estudante em uma diferente organização que administrador de Serviço? |
  | --- | --- | --- | --- |
  |  Conta da Microsoft |sim |Não |Não  |
  |  Conta corporativa ou de estudante |sim |sim |Não  |

## <a name="change-the-account-administrator-for-an-azure-subscription"></a>Alterar o Administrador da Conta de uma assinatura do Azure

O Administrador da Conta é o usuário que inicialmente se inscreveu na assinatura do Azure e é responsável como o proprietário de cobrança da assinatura. Para alterar o Administrador de Conta de uma assinatura, consulte [Transferir a propriedade de uma assinatura do Azure para outra conta](billing-subscription-transfer.md).

<a name="check-the-account-administrator-of-the-subscription"></a>

**Não tem certeza sobre quem é Administrador da Conta?** Siga estas etapas:

1. Visite [**Assinaturas** no Portal do Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1. Selecione a assinatura que você deseja verificar e olhe as **Configurações**.
1. Selecione **Propriedades**. O Administrador da Conta da assinatura é exibido na caixa **Administrador da Conta**.  

## <a name="learn-more-about-resource-access-control-and-active-directory"></a>Saiba mais sobre o controle de acesso a recursos e o Active Directory

* Para saber mais sobre o RBAC, consulte [O que é o Controle de Acesso Baseado em Função(RBAC)?](../role-based-access-control/overview.md)
* Para saber mais sobre todas as funções no Azure, consulte [entender as diferentes funções no Azure](../role-based-access-control/rbac-and-directory-admin-roles.md).
* Para saber mais sobre como o Azure Active Directory, confira [Como as assinaturas do Azure estão associadas ao Azure Active Directory](../active-directory/active-directory-how-subscriptions-associated-directory.md) e [Atribuição de funções de administrador no Azure Active Directory](../active-directory/users-groups-roles/directory-assign-admin-roles.md).

## <a name="need-help-contact-support"></a>Precisa de ajuda? Entre em contato com o suporte.

Se ainda tiver dúvidas, [entre em contato com o suporte](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver seu problema rapidamente.
