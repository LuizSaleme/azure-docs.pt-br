---
title: 'Tutorial: Integração do Azure Active Directory ao Clever | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Clever.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 069ff13a-310e-4366-a147-d6ec5cca12a5
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2018
ms.author: jeedes
ms.openlocfilehash: 483d03fcc72e0a93111d10b0221164459de27d12
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39431854"
---
# <a name="tutorial-azure-active-directory-integration-with-clever"></a>Tutorial: integração do Active Directory do Azure ao Clever

Neste tutorial, você aprende a integrar o Clever ao Azure AD (Azure Active Directory).

A integração do Clever ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao Clever.
- Você pode permitir que usuários façam logon automaticamente no Clever (logon único) com as respectivas contas do AD do Azure.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Clever, você precisará dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Clever habilitada para logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o Clever da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-clever-from-the-gallery"></a>Adicionando o Clever da galeria
Para configurar a integração do Clever ao Azure AD, você precisará adicionar o Clever da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Clever da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

1. Na caixa de pesquisa, digite **Clever**, selecione **Clever** no painel de resultados e, em seguida, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Clever na lista de resultados](./media/clever-tutorial/tutorial_clever_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o Clever, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Clever é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do AD do Azure e o usuário relacionado em Clever.

No Clever, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Clever, você precisa concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
1. **[Criar um usuário de teste do Clever](#create-a-clever-test-user)** – para ter um equivalente de Brenda Fernandes no Clever que esteja vinculado à representação de usuário do Azure AD.
1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
1. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo Clever.

**Para configurar o logon único do Azure AD com o Clever, execute as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo **Clever**, clique em **Logon único**.

    ![Link Configurar logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.

    ![Caixa de diálogo Logon único](./media/clever-tutorial/tutorial_clever_samlbase.png)

1. Na seção **URLs e Domínio do Clever**, execute as seguintes etapas:

    ![Informações de logon único em Domínio e URLs do Clever](./media/clever-tutorial/tutorial_clever_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://clever.com/in/<companyname>`

    b. Na caixa de texto **Identificador**, digite a URL: `https://clever.com/oauth/saml/metadata.xml`

    > [!NOTE]
    > O valor da URL de logon não é real. Atualize esse valor com a URL de Logon real. Contate a [equipe de suporte ao cliente do Clever](https://clever.com/about/contact/) para obter esse valor.

1. Na seção **Certificado de Autenticação SAML**, clique no botão copiar para copiar a **URL de metadados de federação do aplicativo** e cole-a no Bloco de Notas.
    
    ![Configurar o logon único](./media/clever-tutorial/tutorial_metadataurl.png)

1. O aplicativo Clever espera as declarações do SAML em um formato específico, o que exige que você adicione mapeamentos de atributo personalizados de acordo com a sua configuração de **Atributos do Token SAML** .

    A captura de tela a seguir mostra um exemplo disso.

    ![Configurar o logon único](./media/clever-tutorial/tutorial_clever_07.png)

1. Na seção **Atributos do usuário**, na caixa de diálogo **Logon único**, configure o atributo do token SAML na imagem acima e siga as etapas abaixo:
    
    | Nome do atributo  | Valor do atributo |
    | --------------- | -------------------- |
    | clever.teacher.credentials.district_username|user.userprincipalname|
    | clever.student.credentials.district_username| user.userprincipalname |
    | Firstname  | user.givenname |
    | Sobrenome  | user.surname |

    a. Clique em **Adicionar atributo** para abrir o diálogo **Adicionar Atributo**.

    ![Configurar o logon único](./media/clever-tutorial/tutorial_attribute_04.png)
    
    ![Configurar o logon único](./media/clever-tutorial/tutorial_attribute_05.png)
    
    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.

    c. Na lista **Valor**, digite o valor do atributo mostrado para essa linha.

    d. Deixe a caixa de texto **Namespace** em branco.
    
    d. Clique em **OK**.
    
1. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/clever-tutorial/tutorial_general_400.png)

1. Em outra janela do navegador da Web, faça logon em seu site de empresa do Clever como um administrador.

1. Na barra de ferramentas, clique em **Logon Instantâneo**.

    ![Logon Instantâneo](./media/clever-tutorial/ic798984.png "Logon Instantâneo")

    > [!NOTE]
    > Antes de testar o logon único, você precisa entrar em contato com a [equipe de suporte de Clever Client](https://clever.com/about/contact/) para habilitar o SSO do Office 365 no back-end.

1. Na página **Logon Instantâneo** , execute as seguintes etapas:
    
      ![Logon Instantâneo](./media/clever-tutorial/ic798985.png "Logon Instantâneo")
    
      a. Digite a **URL de Logon**.
    
      >[!NOTE]
      >A **URL de Logon** é um valor personalizado. Contate a [equipe de suporte ao cliente do Clever](https://clever.com/about/contact/) para obter esse valor.
    
      b. Para **Sistema de Identidade**, selecione **ADFS**.

      c. Na caixa de texto **URL de Metadados**, cole o valor da **URL de metadados de federação do aplicativo** que você copiou do Portal do Azure.
    
      d. Clique em **Salvar**.

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

   ![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/clever-tutorial/create_aaduser_01.png)

1. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/clever-tutorial/create_aaduser_02.png)

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/clever-tutorial/create_aaduser_03.png)

1. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/clever-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

    c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

    d. Clique em **Criar**.

### <a name="create-a-clever-test-user"></a>Criar um usuário de teste do Clever

Para permitir que os usuários do Azure AD façam logon no Clever, eles devem ser provisionados no Clever.

No caso de Clever, trabalhar com [equipe de suporte de Clever Client](https://clever.com/about/contact/) para adicionar os usuários na plataforma Clever. Os usuários devem ser criados e ativados antes de usar o logon único.

>[!NOTE]
>É possível usar qualquer outra ferramenta de criação da conta de usuário do Clever ou as APIs fornecidas pelo Clever para provisionar as contas de usuário do Azure AD.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao Clever.

![Atribuir a função de usuário][200]

**Para atribuir Britta Simon ao Clever, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201]

1. Na lista de aplicativos, selecione **Clever**.

    ![O link do Clever na lista de Aplicativos](./media/clever-tutorial/tutorial_clever_app.png)

1. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.

### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco de Clever no Painel de Acesso, você deverá ser conectado automaticamente ao aplicativo Clever.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/clever-tutorial/tutorial_general_01.png
[2]: ./media/clever-tutorial/tutorial_general_02.png
[3]: ./media/clever-tutorial/tutorial_general_03.png
[4]: ./media/clever-tutorial/tutorial_general_04.png

[100]: ./media/clever-tutorial/tutorial_general_100.png

[200]: ./media/clever-tutorial/tutorial_general_200.png
[201]: ./media/clever-tutorial/tutorial_general_201.png
[202]: ./media/clever-tutorial/tutorial_general_202.png
[203]: ./media/clever-tutorial/tutorial_general_203.png

