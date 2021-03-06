---
title: 'Tutorial: Integração do Azure Active Directory ao Tableau Server | Microsoft Docs'
description: Saiba como configurar o logon único entre o Azure Active Directory e o Tableau Server.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 78f58f28eb9c25e0b5f6869f7e2348b41780fb60
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437059"
---
# <a name="tutorial-azure-active-directory-integration-with-tableau-server"></a>Tutorial: Integração do Azure Active Directory ao Tableau Server

Neste tutorial, você aprenderá a integrar o Tableau Server ao Azure AD (Azure Active Directory).

A integração do Tableau Server ao Azure AD oferece os seguintes benefícios:

- Você pode controlar no Azure AD quem tem acesso ao Tableau Server
- Você pode habilitar os usuários a entrar automaticamente no Tableau Server (Logon Único) com suas contas do Azure AD
- Você pode gerenciar suas contas em um única localização: o Portal do Azure

Para conhecer mais detalhadamente a integração de aplicativos de SaaS ao Azure AD, consulte [o que é o acesso a aplicativos e logon único com o Azure Active Directory](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Pré-requisitos

Para configurar a integração do Azure AD ao Tableau Server, você precisa dos seguintes itens:

- Uma assinatura do AD do Azure
- Uma assinatura habilitada para logon único do Tableau Server

> [!NOTE]
> Para testar as etapas deste tutorial, nós não recomendamos o uso de um ambiente de produção.

Para testar as etapas deste tutorial, você deve seguir estas recomendações:

- Não use o ambiente de produção, a menos que seja necessário.
- Se não tiver um ambiente de avaliação do AD do Azure, você pode obter uma versão de avaliação de um mês [aqui](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descrição do cenário
Neste tutorial, você testará o logon único do Azure AD em um ambiente de teste. O cenário descrito neste tutorial consiste em dois blocos de construção principais:

1. Adição do Tableau Server a partir da galeria
1. configurar e testar o logon único do AD do Azure

## <a name="adding-tableau-server-from-the-gallery"></a>Adição do Tableau Server a partir da galeria
Para configurar a integração do Tableau Server ao Azure AD, você precisa adicionar o Tableau Server da galeria à sua lista de aplicativos SaaS gerenciados.

**Para adicionar o Tableau Server por meio da galeria, execute as seguintes etapas:**

1. No **[Portal do Azure](https://portal.azure.com)**, no painel navegação à esquerda, clique no ícone **Azure Active Directory**. 

    ![Active Directory][1]

1. Navegue até **aplicativos empresariais**. Em seguida, vá para **todos os aplicativos**.

    ![APLICATIVOS][2]
    
1. Clique no botão **Novo aplicativo** na parte superior da caixa de diálogo para adicionar o novo aplicativo.

    ![APLICATIVOS][3]

1. Na caixa de pesquisa, digite **Tableau Server**.

    ![Criação de um usuário de teste do AD do Azure](./media/tableauserver-tutorial/tutorial_tableauserver_search.png)

1. No painel de resultados, selecione **Tableau Server** e, depois, clique no botão **Adicionar** para adicionar o aplicativo.

    ![Criação de um usuário de teste do AD do Azure](./media/tableauserver-tutorial/tutorial_tableauserver_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>configurar e testar o logon único do AD do Azure
Nesta seção, você configurará e testará o logon único do Azure AD com o Tableau Server, com base em um usuário de teste chamado “Brenda Fernandes”.

Para que o logon único funcione, o Azure AD precisa saber qual usuário do Tableau Server é equivalente a um usuário do Azure AD. Em outras palavras, é necessário estabelecer uma relação de vínculo entre um usuário do Azure AD e o usuário relacionado do Tableau Server.

No Tableau Server, atribua o valor do **nome de usuário** no Azure AD como o valor do **Nome de usuário** para estabelecer a relação de vínculo.

Para configurar e testar o logon único do Azure AD com o Tableau Server, você precisa concluir os seguintes blocos de construção:

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - para habilitar seus usuários a usar esse recurso.
1. **[Criação de um usuário de teste do AD do Azure](#creating-an-azure-ad-test-user)** – para testar o logon único do AD do Azure com Brenda Fernandes.
1. **[Criação de um usuário de teste do Tableau Server](#creating-a-tableau-server-test-user)** – para ter um equivalente de Brenda Fernandes no Tableau Server que esteja vinculado à representação do usuário no Azure AD.
1. **[Atribuição do usuário de teste do AD do Azure](#assigning-the-azure-ad-test-user)** – para permitir que Brenda Fernandes use o logon único do AD do Azure.
1. **[Teste do logon único](#testing-single-sign-on)** : para verificar se a configuração funciona.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuração do logon único do Azure AD

Nesta seção, você habilita o logon único do Azure AD no portal do Azure e configura o logon único no aplicativo Tableau Server.

**Para configurar o logon único do Azure AD com o Tableau Server, execute as seguintes etapas:**

1. No portal do Azure, na página de integração do aplicativo do **Tableau Server**, clique em **Logon único**.

    ![Configurar o logon único][4]

1. Na caixa de diálogo **Logon único**, selecione **Modo** como **Logon baseado em SAML** para habilitar o logon único.
 
    ![Configurar o logon único](./media/tableauserver-tutorial/tutorial_tableauserver_samlbase.png)

1. Na seção **Domínio e URLs do Tableau Server**, realize as seguintes etapas:

    ![Configurar o logon único](./media/tableauserver-tutorial/tutorial_tableauserver_url.png)

    a. Na caixa de texto **URL de Logon**, digite uma URL usando o seguinte padrão: `https://azure.<domain name>.link`
    
    b. Na caixa de texto **Identificador**, digite uma URL usando o seguinte padrão: `https://azure.<domain name>.link`

    c. Na caixa de texto **URL de resposta**, digite uma URL no seguinte padrão: `https://azure.<domain name>.link/wg/saml/SSO/index.html`
     
    > [!NOTE] 
    > Os valores anteriores não são valores reais. Posteriormente, você deve atualizar os valores com a URL e o identificador reais da página de configuração do Tableau Server. 

1. O aplicativo Tableau Server espera que as declarações SAML estejam em um formato específico. Configure as declarações a seguir para este aplicativo. Você pode gerenciar os valores desses atributos da seção **“Atributos de Usuário”** na página de integração do aplicativo. A captura de tela a seguir mostra um exemplo disso.
    
    ![Configurar o logon único](./media/tableauserver-tutorial/3.png)
    
1. Na seção **Atributos do usuário**, na caixa de diálogo **Logon único**, configure o atributo do token SAML na imagem acima e siga as etapas abaixo:
    
    | Nome do atributo | Valor do atributo |
    | ---------------| --------------- |    
    | Nome de Usuário | *user.mailnickname* |

    a. Clique em **Adicionar atributo** para abrir o diálogo **Adicionar Atributo**.

    ![Configurar o logon único](./media/tableauserver-tutorial/tutorial_officespace_04.png)

    ![Configurar o logon único](./media/tableauserver-tutorial/tutorial_officespace_05.png)
    
    b. Na caixa de texto **Nome** , digite o nome do atributo mostrado para essa linha.
    
    c. Na lista **Valor**, digite o valor do atributo mostrado para essa linha.
    
    d. Clique em **Ok**


1. Na seção **Certificado de Autenticação SAML**, clique em **Metadados XML** e, em seguida, salve o arquivo de metadados em seu computador.

    ![Configurar o logon único](./media/tableauserver-tutorial/tutorial_tableauserver_certificate.png) 

1. Clique no botão **Salvar** .

    ![Configurar Logon Único](./media/tableauserver-tutorial/tutorial_general_400.png)
<CS>
1. Para configurar o SSO para o aplicativo, você precisa entrar no locatário Tableau Server como administrador.
   
   a. Na configuração do Tableau Server, clique na guia **SAML** .
  
    ![Configurar o logon único](./media/tableauserver-tutorial/tutorial_tableauserver_001.png) 
  
   b. Marque a caixa de seleção **Usar SAML para logon único**.
   
   c. URL de retorno do Tableau Server — a URL que os usuários do Tableau Server acessarão, por exemplo http://tableau_server. Usar http://localhost não é recomendado. Usar uma URL com uma barra à direita (por exemplo, http://tableau_server/) não é aceito. Copie a **URL de retorno do Tableau Server** e cole-a na caixa de texto **URL de Logon** do Azure AD, na seção **URLs e Domínio do Tableau Server**.
   
   d. ID de entidade SAML - a ID de entidade identifica com exclusividade sua instalação do Tableau Server para o IdP. Você pode digitar a URL do Tableau Server novamente aqui, se desejar, mas ela não precisa ser sua URL do Tableau Server. Copie a **ID de Entidade do SAML** e cole-a na caixa de texto **Identificador** do Azure AD, na seção **URLs e Domínio do Tableau Server**.
     
   e. Clique em **Exportar Arquivo de Metadados** e abra-o no aplicativo de editor de texto. Localize a URL do Serviço de Declaração do Consumidor com Http Post e Índice 0 e copie a URL. Agora a cole na caixa de texto **URL de Resposta** do Azure AD, na seção **URLs e Domínio do Tableau Server**.
   
   f. Localize o arquivo de Metadados de Federação baixado do portal do Azure e carregue-o no **arquivo de metadados do IDP do SAML**.
   
   g. Clique no botão **OK** na página Configuração do Tableau Server.
   
    >[!NOTE] 
    >O cliente precisa carregar um certificado na configuração de SSO do SAML do Tableau Server e ele será ignorado no fluxo de SSO.
    >Se precisar de ajuda para configurar o SAML no Tableau Server, consulte o artigo [Configurar SAML](http://onlinehelp.tableau.com/current/server/en-us/config_saml.htm).
    >
<CE>

> [!TIP]
> É possível ler uma versão concisa dessas instruções no [Portal do Azure](https://portal.azure.com), enquanto você estiver configurando o aplicativo!  Depois de adicionar esse aplicativo da seção **Active Directory > Aplicativos Empresariais**, basta clicar na guia **Logon Único** e acessar a documentação inserida por meio da seção **Configuração** na parte inferior. Saiba mais sobre a funcionalidade de documentação inserida aqui: [Documentação inserida do Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Criação de um usuário de teste do AD do Azure
O objetivo desta seção é criar um usuário de teste no Portal do Azure chamado Brenda Fernandes.

![Criar um usuário do AD do Azure][100]

**Para criar um usuário de teste no AD do Azure, execute as seguintes etapas:**

1. No **Portal do Azure**, no painel de navegação esquerdo, clique no ícone **Azure Active Directory**.

    ![Criação de um usuário de teste do AD do Azure](./media/tableauserver-tutorial/create_aaduser_01.png) 

1. Vá para **Usuários e grupos** e clique em **Todos os usuários** para exibir a lista de usuários.
    
    ![Criação de um usuário de teste do AD do Azure](./media/tableauserver-tutorial/create_aaduser_02.png) 

1. Para abrir a caixa de diálogo **Usuário**, clique em **Adicionar** na parte superior da caixa de diálogo.
 
    ![Criação de um usuário de teste do AD do Azure](./media/tableauserver-tutorial/create_aaduser_03.png) 

1. Na página do diálogo **Usuário**, execute as seguintes etapas:
 
    ![Criação de um usuário de teste do AD do Azure](./media/tableauserver-tutorial/create_aaduser_04.png) 

    a. Na caixa de texto **Nome**, digite **Brenda Fernandes**.

    b. Na caixa de texto **Nome de usuário**, digite o **endereço de email** da conta de Brenda Fernandes.

    c. Selecione **Mostrar senha** e anote o valor de **senha**.

    d. Clique em **Criar**.
 
### <a name="creating-a-tableau-server-test-user"></a>Criação de um usuário de teste do Tableau Server

O objetivo desta seção é criar uma usuária chamada Brenda Fernandes no Tableau Server. Será necessário provisionar todos os usuários no Tableau Server. 

O nome de usuário deve corresponder ao valor que você configurou no atributo personalizado **username** do Azure AD. Com o mapeamento correto, a integração deve funcionar, conforme [Configuração do Logon Único do Azure AD](#configuring-azure-ad-single-sign-on).

>[!NOTE]
>Se precisar criar um usuário manualmente, entre em contato com o administrador do Tableau Server em sua organização.
> 
> 

### <a name="assigning-the-azure-ad-test-user"></a>Atribuição do usuário de teste do AD do Azure

Nesta seção, você permitirá que Brenda Fernandes use o logon único do Azure concedendo-lhe acesso ao Tableau Server.

![Atribuir usuário][200] 

**Para atribuir Brenda Fernandes ao Tableau Server, execute as seguintes etapas:**

1. No Portal do Azure, abra a exibição de aplicativos e, em seguida, navegue até a exibição de diretório e vá para **Aplicativos Empresariais** e clique em **Todos os aplicativos**.

    ![Atribuir usuário][201] 

1. Na lista de aplicativos, selecione **Tableau Server**.

    ![Configurar o logon único](./media/tableauserver-tutorial/tutorial_tableauserver_app.png) 

1. No menu à esquerda, clique em **usuários e grupos**.

    ![Atribuir usuário][202] 

1. Clique no botão **Adicionar**. Em seguida, selecione **usuários e grupos** na **Adicionar atribuição** caixa de diálogo.

    ![Atribuir usuário][203]

1. Em **usuários e grupos** caixa de diálogo, selecione **Britta Simon** na lista de usuários.

1. Clique em **selecione** botão **usuários e grupos** caixa de diálogo.

1. Clique em **atribuir** botão **Adicionar atribuição** caixa de diálogo.
    
### <a name="testing-single-sign-on"></a>Teste do logon único

Nesta seção, você testará sua configuração de logon único do Azure AD usando o Painel de Acesso.

Ao clicar no bloco Tableau Server no Painel de Acesso, você deverá ser conectado automaticamente a seu aplicativo Tableau Server.
Para saber mais sobre o Painel de Acesso, veja [Introdução ao Painel de Acesso](../user-help/active-directory-saas-access-panel-introduction.md). 

## <a name="additional-resources"></a>Recursos adicionais

* [Lista de tutoriais sobre como integrar aplicativos SaaS com o Active Directory do Azure](tutorial-list.md)
* [O que é o acesso a aplicativos e logon único com o Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/tableauserver-tutorial/tutorial_general_01.png
[2]: ./media/tableauserver-tutorial/tutorial_general_02.png
[3]: ./media/tableauserver-tutorial/tutorial_general_03.png
[4]: ./media/tableauserver-tutorial/tutorial_general_04.png

[100]: ./media/tableauserver-tutorial/tutorial_general_100.png

[200]: ./media/tableauserver-tutorial/tutorial_general_200.png
[201]: ./media/tableauserver-tutorial/tutorial_general_201.png
[202]: ./media/tableauserver-tutorial/tutorial_general_202.png
[203]: ./media/tableauserver-tutorial/tutorial_general_203.png

