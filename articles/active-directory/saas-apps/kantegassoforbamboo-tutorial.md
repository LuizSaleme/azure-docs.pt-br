---
title: 'Tutorial: Integração do Azure Active Directory ao SSO do Kantega para o Bamboo | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o SSO do Kantega para o Bamboo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e238b574-9e9b-43b7-ab98-d2a87ff89d48
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: ceab1293b5bd1fbae9088783651d0effa8c5a78a
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39448995"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-bamboo"></a>Tutorial: Integração do Azure Active Directory ao SSO do Kantega para o Bamboo

Neste tutorial, você aprende a integrar o SSO do Kantega para o Bamboo ao Azure AD (Azure Active Directory).

A integração do SSO do Kantega para o Bamboo ao Azure AD oferece os seguintes benefícios:

- No Azure AD, é possível controlar quem tem acesso ao SSO do Kantega para o Bamboo
- É possível permitir que os usuários se conectem automaticamente ao SSO do Kantega para o Bamboo (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao SSO do Kantega para o Bamboo, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do SSO do Kantega para o Bamboo

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adicionando o SSO do Kantega para o Bamboo por meio da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-kantega-sso-for-bamboo-from-the-gallery"></a>Adicionando o SSO do Kantega para o Bamboo por meio da galeria
Para configurar a integração do SSO do Kantega para o Bamboo ao Azure AD, é necessário adicionar o SSO do Kantega para o Bamboo à lista de aplicativos SaaS gerenciados por meio da galeria.

**Para adicionar o SSO do Kantega para o Bamboo por meio da galeria, realize as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **SSO do Kantega para o Bamboo**.

    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_search.png)

1. No painel de resultados, selecione **SSO do Kantega para o Bamboo** e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configura e testa o logon único do Azure AD com o SSO do Kantega para o Bamboo, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do SSO do Kantega para o Bamboo é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do SSO do Kantega para o Bamboo.

No SSO do Kantega para o Bamboo, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o SSO do Kantega para o Bamboo, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
1. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** – para testar o logon único do AD do Azure com Brenda Fernandes.
1. **[Criando um usuário de teste do SSO do Kantega para o Bamboo](#creating-a-kantega-sso-for-bamboo-test-user)** – para ter um equivalente de Brenda Fernandes no SSO do Kantega para o Bamboo que esteja vinculado à representação de usuário do Azure AD.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo SSO do Kantega para o Bamboo.

**Para configurar o logon único do Azure AD com o SSO do Kantega para o Bamboo, realize as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo **SSO do Kantega para o Bamboo**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_samlbase.png)

1. No modo iniciado pelo **IDP**, na seção **Domínio e URLs do SSO do Kantega para o Bamboo**, realize a seguinte etapa:

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_url1.png)
    
    a. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

1. No modo iniciado pelo **SP**, marque a opção **Mostrar configurações de URL avançadas** e realize a seguinte etapa:

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_url2.png)
    
    Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`
     
    > [!NOTE] 
    > Esses valores não são reais. Atualize esses valores com o Identificador real, a URL de Resposta e a URL de Entrada. Esses valores são recebidos durante a configuração do plug-in do Bamboo, que é explicada adiante no tutorial.

1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/tutorial_general_400.png)
    
1. Em outra janela do navegador da Web, faça logon no servidor local do Bamboo como administrador.

1. Passe o cursor do mouse sobre a engrenagem e clique em **Complementos**.

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon1.png)

1. Na seção da guia Complementos, clique em **Localizar novos complementos**. Pesquise **SSO do Kantega para o Bamboo (SAML e Kerberos)** e clique no botão **Instalar** para instalar o novo plug-in do SAML.

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon2.png)

1. A instalação do plug-in será iniciada.

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon21.png)

1. Quando a instalação for concluída. Clique em **fechar**

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon33.png)

1.  Clique em **Gerenciar**.

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon34.png)
    
1. Clique em **Configurar** para configurar o novo plug-in. 

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon3.png)

1. Na seção **SAML**. Selecione **Azure AD (Azure Active Directory)** na lista suspensa **Adicionar provedor de identidade**.

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon4.png)

1. Selecione o nível de assinatura como **Básico**.

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon5.png)

1. Na seção **Propriedades do aplicativo**, realize as seguintes etapas:

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon6.png)

    a. Copie o valor da **URI da ID do Aplicativo** e use-o como **o Identificador, a URL de Resposta e a URL de Logon** na seção **Domínio e URLs do SSO do Kantega para o Bamboo** do portal do Azure.

    b. Clique em **Próximo**.

1. Na seção **Importação de metadados**, realize as seguintes etapas:

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon7.png)

    a. Selecione **Arquivo de metadados no meu computador** e carregue um arquivo de metadados baixado no portal do Azure.

    b. Clique em **Próximo**.

1. Na seção **Nome e localização de SSO**, realize as seguintes etapas:

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon8.png)

    a. Adicione Nome do Provedor de Identidade à caixa de texto **Nome do provedor de identidade** (por exemplo, Azure AD).

    b. Clique em **Próximo**.

1. Verifique o Certificado de autenticação e clique em **Avançar**.   

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon9.png)

1. Na seção **Contas de usuário do Bamboo**, realize as seguintes etapas:

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon10.png)

    a. Selecione **Criar usuários no Diretório interno do Bamboo, se necessário** e insira o nome apropriado do grupo de usuários (podem ser vários números de grupos separados por vírgula).

    b. Clique em **Próximo**.

1. Clique em **Concluir**.

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon11.png)

1. Na seção **Domínios conhecidos do Azure AD**, realize as seguintes etapas:  

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/addon12.png)

    a. Selecione **Domínios conhecidos** no painel esquerdo da página.

    b. Insira o nome de domínio na caixa de texto **Domínios conhecidos**.

    c. Clique em **Salvar**.

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforbamboo-tutorial/create_aaduser_01.png) 

1. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforbamboo-tutorial/create_aaduser_02.png) 

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforbamboo-tutorial/create_aaduser_03.png) 

1. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/kantegassoforbamboo-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-kantega-sso-for-bamboo-test-user"></a>Criando um usuário de teste do SSO do Kantega para o Bamboo

Para permitir que os usuários do Azure AD façam logon no Bamboo, eles devem ser provisionados no Bamboo. No SSO do Kantega para o Bamboo, o provisionamento é uma tarefa manual.

**Para provisionar uma conta de usuário, execute as seguintes etapas:**

1. Faça logon no Bamboo no servidor local como administrador.

1. Passe o cursor do mouse sobre a engrenagem e clique em **Gerenciamento de usuário**.

    ![Adicionar Funcionário](./media/kantegassoforbamboo-tutorial/user1.png) 

1. Clique em **Usuários**. Na seção **Adicionar usuário**, realize as seguintes etapas:

    ![Adicionar Funcionário](./media/kantegassoforbamboo-tutorial/user2.png) 

    a. Na caixa de texto **Nome de usuário**, digite o email do usuário, como Brittasimon@contoso.com.
    
    b. Na caixa de texto **Senha**, digite a senha do usuário.

    c. Na caixa de texto **Confirmar Senha**, insira novamente a senha do usuário.
    
    d. Na caixa de texto **Nome completo**, digite o nome completo do usuário, como Brenda Fernandes.
    
    e. Na caixa de texto **Email**, digite o endereço de email do usuário, como Brittasimon@contoso.com.
    
    f. Clique em **Salvar**.

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permite que Brenda Fernandes use o logon único do Azure concedendo acesso ao SSO do Kantega para o Bamboo.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao SSO do Kantega para o Bamboo, realize as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **SSO do Kantega para o Bamboo**.

    ![Configurar o logon único](./media/kantegassoforbamboo-tutorial/tutorial_kantegassoforbamboo_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Quando você clicar no bloco do SSO do Kantega para o Bamboo no Painel de Acesso, deverá ser conectado automaticamente ao aplicativo SSO do Kantega para o Bamboo.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/kantegassoforbamboo-tutorial/tutorial_general_01.png
[2]: ./media/kantegassoforbamboo-tutorial/tutorial_general_02.png
[3]: ./media/kantegassoforbamboo-tutorial/tutorial_general_03.png
[4]: ./media/kantegassoforbamboo-tutorial/tutorial_general_04.png

[100]: ./media/kantegassoforbamboo-tutorial/tutorial_general_100.png

[200]: ./media/kantegassoforbamboo-tutorial/tutorial_general_200.png
[201]: ./media/kantegassoforbamboo-tutorial/tutorial_general_201.png
[202]: ./media/kantegassoforbamboo-tutorial/tutorial_general_202.png
[203]: ./media/kantegassoforbamboo-tutorial/tutorial_general_203.png

