---
title: Usar uma VM do Windows para acessar o SQL do Azure
description: Um tutorial que orienta o processo de usar uma Identidade de Serviço Gerenciada de VM do Windows para acessar o SQL do Azure.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: bryanla
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: ace7f11eeea081077855a409824272b4b55f3c33
ms.sourcegitcommit: 156364c3363f651509a17d1d61cf8480aaf72d1a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/25/2018
ms.locfileid: "39247220"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-sql"></a>Tutorial: Usar a Identidade de Serviço Gerenciada da VM do Windows para acessar o SQL do Azure

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra como usar uma Identidade de Serviço Gerenciada para uma VM (máquina virtual) do Windows para acessar um SQL Server do Azure. As Identidades de Serviço Gerenciadas são gerenciadas automaticamente pelo Azure e permitem a você autenticar os serviços que dão suporte à autenticação do Azure AD sem necessidade de inserir as credenciais em seu código. Você aprenderá como:

> [!div class="checklist"]
> * Habilitar a Identidade de Serviço Gerenciada em uma VM do Windows 
> * Conceda à sua VM acesso a um SQL Server do Azure
> * Obtenha um token de acesso usando a identidade da VM e use-o para consultar um SQL Server do Azure

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Entrar no Azure

Entre no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Windows em um novo grupo de recursos

Para este tutorial, vamos criar uma nova VM do Windows.  Você também pode habilitar a Identidade de Serviço Gerenciada em uma VM existente.

1.  Clique no botão **Criar um recurso** localizado no canto superior esquerdo do Portal do Azure.
2.  Selecione **Computação** e, em seguida, selecione **Windows Server 2016 Datacenter**. 
3.  Insira as informações da máquina virtual. O **Nome de usuário** e **Senha** criados aqui são as credenciais usadas para fazer logon na máquina virtual.
4.  Escolha uma **Assinatura** para a máquina virtual na lista suspensa.
5.  Para selecionar um novo **Grupo de Recursos** no qual será criada a máquina virtual, escolha **Criar Novo**. Ao concluir, clique em **OK**.
6.  Selecione o tamanho para a VM. Para ver mais tamanhos, selecione **Exibir todos os** ou altere o filtro **Tipo de disco com suporte**. Na página Configurações, mantenha os padrões e clique em **OK**.

    ![Texto Alt da imagem](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-service-identity-on-your-vm"></a>Habilitar a Identidade de Serviço Gerenciada em sua VM 

Uma Identidade de Serviço Gerenciada de VM permite que você obtenha tokens de acesso do Azure AD sem a necessidade de colocar as credenciais em seu código. Habilitar a Identidade de Serviço Gerenciada informa ao Azure para criar uma identidade gerenciada para sua VM. Em segundo plano, habilitar a Identidade de Serviço Gerenciada faz duas coisas: registra sua VM com o Azure Active Directory para criar sua identidade gerenciada e configura a identidade na VM.

1.  Selecione a **Máquina Virtual** na qual você deseja habilitar a Identidade de Serviço Gerenciada.  
2.  Na barra de navegação à esquerda, clique em **Configuração**. 
3.  Você verá **Identidade de Serviço Gerenciado**. Para registrar e habilitar a Identidade de Serviço Gerenciada, selecione **Sim**. Se você desejar desabilitá-la, escolha Não. 
4.  Lembre-se de clicar em **Salvar** para salvar a configuração.  
    ![Texto Alt da imagem](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="grant-your-vm-access-to-a-database-in-an-azure-sql-server"></a>Conceda à VM acesso a um banco de dados em um SQL Server do Azure

Agora você pode conceder à VM acesso a um banco de dados em um SQL Server do Azure.  Para esta etapa, você pode usar um SQL Server existente ou criar um novo.  Para criar um novo servidor e banco de dados usando o portal do Azure, siga este [início rápido do Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal). Também há inícios rápidos que usam a CLI do Azure e o Azure PowerShell na [documentação do SQL Azure](https://docs.microsoft.com/azure/sql-database/).

Há três etapas para conceder acesso da VM a um banco de dados:
1.  Crie um grupo no Azure AD e torne a Identidade de Serviço Gerenciada da VM um membro do grupo.
2.  Habilite a autenticação do Azure AD para o SQL Server.
3.  Crie um **usuário contido** no banco de dados que representa o grupo do Azure AD.

> [!NOTE]
> Normalmente, você criaria um usuário contido mapeado diretamente para a Identidade de Serviço Gerenciada da VM.  No momento, o SQL Azure não permite a Entidade de Serviço do Azure AD que representa a Identidade de Serviço Gerenciada da VM a ser mapeada para um usuário contido.  Como uma solução alternativa com suporte, torne a Identidade de Serviço Gerenciada da VM um membro de um grupo do Azure AD e crie um usuário contido no banco de dados que representa o grupo.


### <a name="create-a-group-in-azure-ad-and-make-the-vm-managed-service-identity-a-member-of-the-group"></a>Crie um grupo no Azure AD e torne a Identidade de Serviço Gerenciada da VM um membro do grupo

Você pode usar um grupo do Azure AD existente ou criar um novo usando o PowerShell do Azure AD.  

Primeiro, instale o módulo do [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2). Entre usando `Connect-AzureAD` e execute o seguinte comando para criar o grupo e salvá-lo em uma variável:

```powershell
$Group = New-AzureADGroup -DisplayName "VM Managed Service Identity access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
```

A saída é semelhante à seguinte, que também examina o valor da variável:

```powershell
$Group = New-AzureADGroup -DisplayName "VM Managed Service Identity access to SQL" -MailEnabled $false -SecurityEnabled $true -MailNickName "NotSet"
$Group
ObjectId                             DisplayName          Description
--------                             -----------          -----------
6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 VM Managed Service Identity access to SQL
```

Em seguida, adicione a Identidade de Serviço Gerenciada da VM ao grupo.  Você precisa da **ObjectId** da Identidade de Serviço Gerenciada, que você pode obter usando o Azure PowerShell.  Primeiro, baixe o [Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps). Então entre usando `Connect-AzureRmAccount` e execute os seguintes comandos para:
- Garantir que o contexto da sessão esteja definido para a assinatura do Azure desejada, se você tiver várias.
- Listar os recursos disponíveis na sua assinatura do Azure e verificar os nomes de VM e grupo de recursos corretos.
- Obtenha as propriedades da VM da Identidade de Serviço Gerenciada usando os valores apropriados para `<RESOURCE-GROUP>` e `<VM-NAME>`.

```powershell
Set-AzureRMContext -subscription "bdc79274-6bb9-48a8-bfd8-00c140fxxxx"
Get-AzureRmResource
$VM = Get-AzureRmVm -ResourceGroup <RESOURCE-GROUP> -Name <VM-NAME>
```

A saída é semelhante à seguinte, que também examina a ID de Objeto de entidade de serviço da Identidade de Serviço Gerenciada da VM:
```powershell
$VM = Get-AzureRmVm -ResourceGroup DevTestGroup -Name DevTestWinVM
$VM.Identity.PrincipalId
b83305de-f496-49ca-9427-e77512f6cc64
```

Agora, adicione a Identidade de Serviço Gerenciada da VM ao grupo.  Você só pode adicionar uma entidade de serviço a um grupo usando o Azure AD PowerShell.  Execute este comando:
```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
```

Se você também examinar a associação de grupo posteriormente, a saída terá a seguinte aparência:

```powershell
Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId $VM.Identity.PrincipalId
Get-AzureAdGroupMember -ObjectId $Group.ObjectId

ObjectId                             AppId                                DisplayName
--------                             -----                                -----------
b83305de-f496-49ca-9427-e77512f6cc64 0b67a6d6-6090-4ab4-b423-d6edda8e5d9f DevTestWinVM
```

### <a name="enable-azure-ad-authentication-for-the-sql-server"></a>Habilitar a autenticação do Azure AD para o SQL Server

Agora que você criou o grupo e adicionou a Identidade de Serviço Gerenciada da VM para a associação, pode [configurar a autenticação do Azure AD para o SQL Server](/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-server) usando as seguintes etapas:

1.  No portal do Azure, selecione **Servidores SQL** na navegação à esquerda.
2.  Clique no SQL Server a ser habilitado para autenticação do Azure AD.
3.  Na seção **Configurações** da folha, clique em **Administrador do Active Directory**.
4.  Na barra de comandos, clique em **Definir administrador**.
5.  Selecione uma conta de usuário do Azure AD para ser um administrador do servidor e clique em **Selecionar.**
6.  Na barra de comandos, clique em **Salvar**.

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Crie um usuário contido no banco de dados que representa o grupo do Azure AD

Para esta próxima etapa, você precisará do SSMS ([Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)). Antes de começar, também pode ser útil examinar os seguintes artigos para obter informações sobre a integração do Azure AD:

- [Autenticação Universal com o Banco de Dados SQL e o SQL Data Warehouse (suporte de SSMS para MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication.md)
- [Configurar e gerenciar o Azure Active Directory para autenticação com o Banco de Dados SQL ou o SQL Data Warehouse](/azure/sql-database/sql-database-aad-authentication-configure.md)

1.  Inicie o SQL Server Management Studio.
2.  Na caixa de diálogo **Conectar-se ao servidor**, insira o nome de seu SQL Server no campo **Nome do servidor**.
3.  No campo **Autenticação**, selecione **Active Directory – Universal com suporte para MFA**.
4.  No campo **Nome de usuário**, digite o nome da conta do Azure AD que você definir como o administrador do servidor, por exemplo, helen@woodgroveonline.com
5.  Clique em **Opções**.
6.  No campo **Conectar-se ao banco de dados**, digite o nome do banco de dados não são do sistema que você deseja configurar.
7.  Clique em **Conectar**.  Conclua o processo de conexão.
8.  No **Pesquisador de Objetos**, expanda a pasta **Bancos de Dados**.
9.  Clique com o botão direito do mouse em um banco de dados de usuário e clique em **Nova consulta**.
10.  Na janela de consulta, insira a linha a seguir e clique em **Executar** na barra de ferramentas:
    
     ```
     CREATE USER [VM Managed Service Identity access to SQL] FROM EXTERNAL PROVIDER
     ```
    
     O comando deve ser concluído com êxito, criando o usuário independente para o grupo.
11.  Desmarque a janela de consulta, insira a linha a seguir e clique em **Executar** na barra de ferramentas:
     
     ```
     ALTER ROLE db_datareader ADD MEMBER [VM Managed Service Identity access to SQL]
     ```

     O comando deve ser concluído com êxito, concedendo ao usuário independente a capacidade de ler todo o banco de dados.

O código em execução na VM agora pode obter um token da Identidade de Serviço Gerenciada e usar o token para autenticação no SQL Server.

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-sql"></a>Obter um token de acesso usando a identidade da VM e usando-a para chamar o Azure SQL 

O Azure SQL tem suporte nativo para autenticação do Azure AD, de modo que pode aceitar diretamente os tokens de acesso obtidos usando a Identidade de Serviço Gerenciada.  Você usa o método **token de acesso** para criar uma conexão para o SQL.  Isso faz parte da integração do SQL Azure ao Azure AD e é diferente de fornecer as credenciais na cadeia de conexão.

Aqui está um exemplo de código .Net de abertura de uma conexão a SQL usando um token de acesso.  Esse código deve ser executado na VM para poder acessar o ponto de extremidade da Identidade de Serviço Gerenciada da VM.  É necessário ter o **.NET Framework 4.6** ou superior para usar o método de token de acesso.  Substitua os valores de AZURE-SQL-SERVERNAME e DATABASE de acordo.  Observe a ID de recurso para o SQL Azure é "https://database.windows.net/".

```csharp
using System.Net;
using System.IO;
using System.Data.SqlClient;
using System.Web.Script.Serialization;

//
// Get an access token for SQL.
//
HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://database.windows.net/");
request.Headers["Metadata"] = "true";
request.Method = "GET";
string accessToken = null;

try
{
    // Call Managed Service Identity endpoint.
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader and extract access token.
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

//
// Open a connection to the SQL server using the access token.
//
if (accessToken != null) {
    string connectionString = "Data Source=<AZURE-SQL-SERVERNAME>; Initial Catalog=<DATABASE>;";
    SqlConnection conn = new SqlConnection(connectionString);
    conn.AccessToken = accessToken;
    conn.Open();
}
```

Como alternativa, uma maneira rápida de testar a configuração de ponta a ponta sem necessidade de escrever e implantar um aplicativo na VM é usando o PowerShell.

1.  No portal, navegue até **Máquinas Virtuais** e vá para a máquina virtual do Windows e em **Visão geral**, clique em **Conectar**. 
2.  Insira o seu **Nome de usuário** e **Senha** que você adicionou quando criou a VM do Windows. 
3.  Agora que você criou uma **Conexão de Área de Trabalho Remota** com a máquina virtual, abra o **PowerShell** na sessão remota. 
4.  Usando o `Invoke-WebRequest` do PowerShell, faça uma solicitação para o ponto de extremidade da Identidade de Serviço Gerenciada local para obter um token de acesso ao SQL Azure.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatabase.windows.net%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    Converta a resposta de um objeto JSON para um objeto do PowerShell. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```

    Extraia o token de acesso da resposta.
    
    ```powershell
    $AccessToken = $content.access_token
    ```

5.  Abra uma conexão com o SQL Server. Lembre-se de substituir os valores para AZURE-SQL-SERVERNAME e DATABASE.
    
    ```powershell
    $SqlConnection = New-Object System.Data.SqlClient.SqlConnection
    $SqlConnection.ConnectionString = "Data Source = <AZURE-SQL-SERVERNAME>; Initial Catalog = <DATABASE>"
    $SqlConnection.AccessToken = $AccessToken
    $SqlConnection.Open()
    ```

    Em seguida, crie e envie uma consulta para o servidor.  Lembre-se de substituir o valor para TABLE.

    ```powershell
    $SqlCmd = New-Object System.Data.SqlClient.SqlCommand
    $SqlCmd.CommandText = "SELECT * from <TABLE>;"
    $SqlCmd.Connection = $SqlConnection
    $SqlAdapter = New-Object System.Data.SqlClient.SqlDataAdapter
    $SqlAdapter.SelectCommand = $SqlCmd
    $DataSet = New-Object System.Data.DataSet
    $SqlAdapter.Fill($DataSet)
    ```

Examine o valor de `$DataSet.Tables[0]` para exibir os resultados da consulta.  Parabéns, você consultou o banco de dados usando uma Identidade de Serviço Gerenciada da VM e sem a necessidade de fornecer credenciais!

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a criar uma identidade de serviço gerenciado para acessar o servidor SQL do Azure.  Para saber mais sobre o Azure SQL Server, consulte:

> [!div class="nextstepaction"]
>[ Serviço do banco de dados SQL do Azure ](/azure/sql-database/sql-database-technical-overview)
