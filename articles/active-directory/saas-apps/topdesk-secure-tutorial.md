---
title: 'Tutorial: integração do Azure Active Directory com o TOPdesk - Secure | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o TOPdesk – Secure.
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8e06ee33-18f9-4c05-9168-e6b162079d88
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: jeedes
ms.openlocfilehash: 26d20fbbe81cb4e1a526ea8c776462d9e851e688
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39429763"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a>Tutorial: Integração do Active Directory do Azure ao TOPdesk - Secure

Neste tutorial, você aprenderá a integrar o TOPdesk – Secure ao Azure AD (Azure Active Directory).

A integração do TOPdesk – Secure ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao TOPdesk – Secure.
- Você pode permitir que os usuários façam logon automaticamente no TOPdesk – Secure (logon único) com as respectivas contas do Azure AD.
- Você pode gerenciar suas contas em um único local central – o portal do Azure.

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao TOPdesk – Secure, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do TOPdesk - Secure

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do Azure AD, você pode [obter uma versão de avaliação de um mês](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionar o TOPdesk – Secure da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-topdesk---secure-from-the-gallery"></a>Adicionar o TOPdesk – Secure da galeria
Para configurar a integração do TOPdesk – Secure ao Azure AD, é necessário adicionar o TOPdesk – Secure da galeria à lista de aplicativos SaaS gerenciados.

**Para adicionar o TOPdesk – Secure da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![O botão Azure Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![A folha Aplicativos empresariais][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![O botão Novo aplicativo][3]

1. Na caixa de pesquisa, digite **TOPdesk – Secure**, selecione **TOPdesk – Secure** no painel de resultados e clique no botão **Adicionar** para adicionar o aplicativo.

    ![TOPdesk – Secure na lista de resultados](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configurar e testar logon único do Azure AD

Nesta seção, você configurará e testará o logon único do Azure AD com o TOPdesk – Secure, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do TOPdesk – Secure é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado no TOPdesk – Secure.

No TOPdesk – Secure, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o TOPdesk – Secure, você precisará concluir os seguintes blocos de construção:

1. **[Configurar o logon único do Azure AD](#configure-azure-ad-single-sign-on)** – para habilitar seus usuários a usar esse recurso.
1. **[Criar um usuário de teste do Azure AD](#create-an-azure-ad-test-user)** – para testar o logon único do Azure AD com Brenda Fernandes.
1. **[Criar um usuário de teste do TOPdesk – Secure](#create-a-topdesk---secure-test-user)** – para ter um equivalente de Brenda Fernandes no TOPdesk – Secure vinculado à representação do usuário no Azure AD.
1. **[Atribuir o usuário de teste do Azure AD](#assign-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do Azure AD.
1. **[Teste o logon único](#test-single-sign-on)** – para verificar se a configuração funciona.

### <a name="configure-azure-ad-single-sign-on"></a>Configurar o logon único do Azure AD

Nesta seção, você habilitará o logon único do Azure AD no Portal do Azure e configurará o logon único no aplicativo TOPdesk – Secure.

**Para configurar o logon único do Azure AD com o TOPdesk – Secure, execute as seguintes etapas:**

1. No Portal do Azure, na página de integração de aplicativos do **TOPdesk – Secure**, clique em **Logon único**.

    ![Link Configurar logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Caixa de diálogo Logon único](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_samlbase.png)

1. Na seção **Domínio e URLs do TOPdesk – Secure**, execute as seguintes etapas:

    ![Informações de logon único de Domínio e URLs do TOPdesk – Secure](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<companyname>.topdesk.net`

    b. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<companyname>.topdesk.net/tas/secure/login/verify`

    c. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://<companyname>.topdesk.net/tas/public/login/saml`

    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com a URL de Entrada e o Identificador reais. A URL de resposta é explicada posteriormente no tutorial. Contate a [equipe de suporte ao cliente do TOPdesk – Secure](http://www.topdesk.com/us/support) para obter esses valores. 

1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![O link de download do Certificado](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_certificate.png) 

1. Clique no botão **Salvar** .

    ![Botão Salvar em Configurar Logon Único](./media/topdesk-secure-tutorial/tutorial_general_400.png)

1. Na seção **Configuração do TOPdesk – Secure**, clique em **Configurar o TOPdesk – Secure** para abrir a janela **Configurar o logon**. Copie a **URL de saída, a ID da Entidade SAML e a URL do Serviço de Logon Único SAML** da **seção de Referência Rápida.**

    ![Configuração do TOPdesk - Secure](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_configure.png)
    
1. Faça logon em seu site de empresa do **TOPdesk - Secure** como administrador.

1. No menu **TOPdesk**, clique em **Configurações**.

    ![Configurações](./media/topdesk-secure-tutorial/ic790598.png "Configurações")

1. Clique em **Configurações de Logon**.

    ![Configurações de logon](./media/topdesk-secure-tutorial/ic790599.png "Configurações de logon")

1. Expanda o menu **Configurações de Logon** e clique em **Geral**.

    ![Geral](./media/topdesk-secure-tutorial/ic790600.png "Geral")

1. Na seção **Seguro** da seção de configuração de **Logon do SAML**, realize as seguintes etapas:

    ![Configurações técnicas](./media/topdesk-secure-tutorial/ic790855.png "Configurações técnicas")
   
    a. Clique em **Baixar** para baixar o arquivo de metadados públicos e salve-o localmente no computador.
   
    b. Abra o arquivo de metadados e localize o nó **AssertionConsumerService** .
    
    ![Serviço de Declaração do Consumidor](./media/topdesk-secure-tutorial/ic790856.png "Serviço de Declaração do Consumidor")
   
    c. Copie o valor **AssertionConsumerService**, cole-o na caixa de texto URL de resposta na seção **Domínio e URLs do TOPdesk – Secure**.

1. Execute as seguintes etapas para criar um arquivo de certificado:
    
    ![Certificado](./media/topdesk-secure-tutorial/ic790606.png "Certificado")
    
    a. Abra o arquivo de metadados baixado do Portal do Azure.

    b. Expanda o nó **RoleDescriptor** que contém um **xsi:type** de **fed:ApplicationServiceType**.

    c. Copie o valor do nó **X509Certificate** .

    d. Salve o valor copiado de **X509Certificate** localmente no computador em um arquivo.

1. Na seção **Público**, clique em **Adicionar**.
    
    ![Adicionar](./media/topdesk-secure-tutorial/ic790607.png "Adicionar")

1. Na página do diálogo **Assistente de configuração do SAML** , realize as seguintes etapas:
    
    ![Assistente de configuração SAML](./media/topdesk-secure-tutorial/ic790608.png "Assistente de configuração SAML")
    
    a. Para carregar o arquivo de metadados baixado do Portal do Azure, em **Metadados de Federação**, clique em **Procurar**.

    b. Para carregar o arquivo de certificado, em **Certificado (RSA)**, clique em **Procurar**.

    c. Para carregar o arquivo de logotipo que você recebeu da equipe de suporte do TOPdesk, em **Ícone do logotipo**, clique em **Procurar**.

    d. Na caixa de texto **Atributo de nome de usuário**, digite `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.

    e. Na caixa de texto **Nome de exibição** , digite um nome para a sua configuração.

    f. Clique em **Salvar**.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="create-an-azure-ad-test-user"></a>Criar um usuário de teste do Azure AD

O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

   ![Criar um usuário de teste do Azure AD][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No portal do Azure, no painel esquerdo, clique no botão **Azure Active Directory**.

    ![O botão Azure Active Directory](./media/topdesk-secure-tutorial/create_aaduser_01.png)

1. Para exibir a lista de usuários, acesse **Usuários e grupos** e, depois, clique em **Todos os usuários**.

    ![Os links “Usuários e grupos” e “Todos os usuários”](./media/topdesk-secure-tutorial/create_aaduser_02.png)

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo **Todos os Usuários**.

    ![O botão Adicionar](./media/topdesk-secure-tutorial/create_aaduser_03.png)

1. Na caixa de diálogo **Usuário**, execute as seguintes etapas:

    ![A caixa de diálogo Usuário](./media/topdesk-secure-tutorial/create_aaduser_04.png)

    a. Na caixa **Nome**, digite **BrendaFernandes**.

    b. Na caixa **Nome de usuário**, digite o endereço de email do usuário Brenda Fernandes.

    c. Marque a caixa de seleção **Mostrar Senha** e, em seguida, anote o valor exibido na caixa **Senha**.

    d. Clique em **Criar**.
 
### <a name="create-a-topdesk---secure-test-user"></a>Criar um usuário de teste do TOPdesk – Secure

Para permitir que os usuários do AD do Azure façam logon no TOPdesk - Secure, eles deverão ser provisionados no TOPdesk - Secure.  
No caso do TOPdesk - Secure, o provisionamento é uma tarefa manual.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Para configurar o provisionamento de usuários, execute as seguintes etapas:
1. Faça logon em seu site de empresa do **TOPdesk - Secure** como administrador.
1. No menu na parte superior, clique em **TOPdesk \> Novo \> Arquivos de Suporte \> Operador**.
   
    ![Operador](./media/topdesk-secure-tutorial/ic790610.png "Operador")

1. No diálogo **Novo Operador** , realize as seguintes etapas:
   
    ![Novo operador](./media/topdesk-secure-tutorial/ic790611.png "Novo operador")
   
    a. Clique na guia **Geral**.
   
    b. Na caixa de texto **Sobrenome**, digite o sobrenome do usuário, como **Simon**.
   
    c. Selecione um **Site** para a conta na seção **Local**.
   
    d. Na caixa de texto **Nome de Logon** da seção **Logon no TOPdesk**, digite um nome de logon para o usuário.
   
    e. Clique em **Salvar**.

> [!NOTE]
> É possível usar qualquer outra ferramenta de criação da conta de usuário do TOPdesk - Secure ou APIs fornecidas pelo TOPdesk - Secure para provisionar as contas de usuário do AAD.

### <a name="assign-the-azure-ad-test-user"></a>Atribuir o usuário de teste do Azure AD

Nesta seção, você permitirá que Britta Simon use o logon único do Azure concedendo-lhe acesso ao TOPdesk – Secure.

![Atribuir a função de usuário][200] 

**Para atribuir Britta Simon ao TOPdesk – Secure, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **TOPdesk – Secure**.

    ![O link do TOPdesk – Secure na lista de Aplicativos](./media/topdesk-secure-tutorial/tutorial_topdesk-secure_app.png)  

1. No menu à esquerda, clique em **usuários e grupos**.

    ![O link “Usuários e grupos”][202]

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![O painel Adicionar Atribuição][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="test-single-sign-on"></a>Testar logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco TOPdesk – Secure no Painel de Acesso, você deverá ser conectado automaticamente ao aplicativo TOPdesk – Secure.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/topdesk-secure-tutorial/tutorial_general_01.png
[2]: ./media/topdesk-secure-tutorial/tutorial_general_02.png
[3]: ./media/topdesk-secure-tutorial/tutorial_general_03.png
[4]: ./media/topdesk-secure-tutorial/tutorial_general_04.png

[100]: ./media/topdesk-secure-tutorial/tutorial_general_100.png

[200]: ./media/topdesk-secure-tutorial/tutorial_general_200.png
[201]: ./media/topdesk-secure-tutorial/tutorial_general_201.png
[202]: ./media/topdesk-secure-tutorial/tutorial_general_202.png
[203]: ./media/topdesk-secure-tutorial/tutorial_general_203.png

