---
title: 'Tutorial: Integração do Azure Active Directory ao Velpic SAML | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Velpic SAML.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 2ca95f6fd94036e86aae2059c05a3fbb0380005e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39446289"
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a>Tutorial: Integração do Azure Active Directory ao Velpic SAML

Neste tutorial, você aprenderá a integrar o Velpic SAML ao Azure AD (Active Directory).

A integração do Velpic SAML ao Azure AD proporciona os seguintes benefícios:

- No Azure AD, você pode controlar quem tem acesso ao Velpic SAML
- Você pode permitir que os usuários façam logon automaticamente no Velpic SAML (Logon único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um único local - o portal de Gerenciamento do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao AD do Azure, consulte [O que é o acesso a aplicativos e logon único com o Active Directory do Azure](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Velpic SAML, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura do Velpic SAML habilitada para logon único

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você poderá obter uma avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando Velpic SAML usando a galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-velpic-saml-from-the-gallery"></a>Adicionando Velpic SAML usando a galeria
Para configurar a integração do Velpic SAML ao Azure AD, você precisa adicionar o Velpic SAML da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Velpic SAML da galeria, execute as seguintes etapas:**

1. No **[Portal de Gerenciamento do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
1. Clique em **adicionar** botão na parte superior da caixa de diálogo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **Velpic SAML**.

    ![Criação de um usuário de teste do AD do Azure](./media/velpicsaml-tutorial/tutorial_velpicsaml_search.png)

1. No painel de resultados, selecione **Velpic SAML** e clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configura e testa o logon único do Azure AD com o Velpic SAML, com base em um usuário de teste chamado "Brenda Fernandes".

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Velpic SAML é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Velpic SAML.

Essa relação de vínculo é estabelecida atribuindo o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** no Velpic SAML.

Para configurar e testar o logon único do Azure AD com o Velpic SAML, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
1. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** – para testar o logon único do AD do Azure com Brenda Fernandes.
1. **[Criação de um usuário de teste do Velpic SAML](#creating-a-velpic-saml-test-user)** - para ter um equivalente de Brenda Fernandes no Velpic SAML que esteja vinculado à representação dela no Azure AD.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal de Gerenciamento do Azure e o configura em seu aplicativo Velpic SAML.

**Para configurar o logon único do Azure AD com o Velpic SAML, execute as seguintes etapas:**

1. No portal de Gerenciamento do Azure, na página de integração do aplicativo **Velpic SAML**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, como **Modo**, selecione **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

1. Insira os detalhes na seção **Domínio e URLs do Velpic SAML**

    ![Configurar o logon único](./media/velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    a. Na caixa de texto **URL de Logon**, digite o valor como: `https://<sub-domain>.velpicsaml.net`

    b. Na caixa de texto **Identificador**, cole o valor de **"URL do logon único"** `https://auth.velpic.com/saml/v2/<entity-id>/login`
    
    > [!NOTE]
    > Observe que a URL de Logon será fornecida pela equipe do Velpic SAML e o valor do Identificador estará disponível quando você configurar o Plug-in do SSO no lado do Velpic SAML. É preciso copiar esse valor na página do aplicativo Velpic SAML e colá-lo aqui.

1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo XML em seu computador.

    ![Configurar o logon único](./media/velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/velpicsaml-tutorial/tutorial_general_400.png)

1. Na seção Configuração do Velpic SAML, clique em Configurar o Velpic SAML para abrir a janela Configurar logon. Copie a ID da Entidade SAML da seção Referência Rápida.

1. Em uma janela diferente do navegador da Web, faça logon no site do Velpic SAML na sua empresa como administrador.

1. Clique na guia **Gerenciar** e vá para a seção **Integração**, onde você pode clicar no botão **Plug-ins** para criar novo plug-in para Conectar.

    ![Plug-in](./media/velpicsaml-tutorial/velpic_1.png)

1. Clique no botão **"Adicionar plug-in"**.
    
    ![Plug-in](./media/velpicsaml-tutorial/velpic_2.png)

1. Clique no bloco **SAML** na página Adicionar Plug-in.
    
    ![Plug-in](./media/velpicsaml-tutorial/velpic_3.png)

1. Insira o nome do novo plug-in SAML e clique no botão **"Adicionar"**.

    ![Plug-in](./media/velpicsaml-tutorial/velpic_4.png)

1. Insira os detalhes da seguinte maneira:

    ![Plug-in](./media/velpicsaml-tutorial/velpic_5.png)

    a. Na caixa de texto **Nome**, digite o nome do plug-in SAML.

    b. Na caixa de texto **URL do Emissor**, cole a **ID da Entidade SAML** que copiou da janela **Configurar logon** no portal do Azure.

    c. Em **Configuração dos Metadados do Provedor**, faça upload do arquivo XML de Metadados que você baixou no portal do Azure.

    d. Você também pode optar por habilitar o SAML apenas durante o provisionamento, habilitando a caixa de seleção **"Criar automaticamente novos usuários"**. Se um usuário não existir no Velpic e esse sinalizador não estiver habilitado, o logon no Azure falhará. Se o sinalizador estiver habilitado, o usuário será provisionado automaticamente no Velpic no momento do logon. 

    e. Copie a **URL de logon único** da caixa de texto e cole-a no portal do Azure.
    
    f. Clique em **Salvar**.

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal de Gerenciamento do Azure chamado Britta Simon.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **portal de Gerenciamento do Azure**, no painel navegação à esquerda, clique em **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/velpicsaml-tutorial/create_aaduser_01.png) 

1. Vá para **usuários e grupos** e clique em **todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/velpicsaml-tutorial/create_aaduser_02.png) 

1. Na parte superior da caixa de diálogo clique **adicionar** para abrir o **usuário** caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/velpicsaml-tutorial/create_aaduser_03.png) 

1. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/velpicsaml-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-velpic-saml-test-user"></a>Criando um usuário de teste no Velpic SAML

Normalmente, esta etapa não é necessária, pois o aplicativo dá suporte ao provisionamento de usuário na hora certa. Se o provisionamento automático de usuário não estiver habilitado, a criação manual do usuário poderá ser feita como descrito abaixo.

Entre no site da empresa do seu Velpic SAML como um administrador e execute as seguintes etapas:
    
1. Clique na guia Gerenciar, vá para a seção Usuários e clique no botão Novo para adicionar usuários.

    ![adicionar usuário](./media/velpicsaml-tutorial/velpic_7.png)

1. Na página da caixa de diálogo **"Criar Novo Usuário"**, execute as etapas que se seguem.

    ![usuário](./media/velpicsaml-tutorial/velpic_8.png)
    
    a. Na caixa de texto **Nome**, digite o nome de Brenda Fernandes.

    b. Na caixa de texto **Sobrenome**, digite o sobrenome de Brenda Fernandes.

    c. Na caixa de texto **Nome de Usuário**, digite o nome de usuário de Brenda Fernandes.

    d. Na caixa de texto **Email**, digite o endereço de email da conta de Brenda Fernandes.

    e. O restante das informações é opcional, você pode preenchê-las se necessário.
    
    f. Clique em **SALVAR**.  

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure, concedendo a ela acesso ao Velpic SAML.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Velpic SAML, execute as seguintes etapas:**

1. No Portal de Gerenciamento do Azure, abra a exibição de aplicativos e, em seguida, navegue até o modo de exibição de diretório e vá para **Aplicativos empresariais**, depois clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **Velpic SAML**.

    ![Configurar o logon único](./media/velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

1. Ao clicar no bloco Velpic SAML no Painel de Acesso, você deve acessar a página de logon do aplicativo Velpic SAML. Você deve ver o botão **"Fazer logon com Azure AD"** na página de entrada.

    ![Plug-in](./media/velpicsaml-tutorial/velpic_6.png)

1. Clique no botão **"Fazer logon com Azure AD"** para entrar no Velpic usando sua conta do Azure AD.


## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/velpicsaml-tutorial/tutorial_general_203.png

