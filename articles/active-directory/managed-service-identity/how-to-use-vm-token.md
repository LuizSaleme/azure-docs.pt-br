---
title: Como usar uma Identidade de Serviço Gerenciado da VM do Azure para adquiri um token de acesso
description: Instruções e exemplos passo a passo para usar uma MSI de VM do Azure para adquirir um token de acesso OAuth.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/01/2017
ms.author: daveba
ms.openlocfilehash: 42ac1cc7dd50f46ada263089437740e680928e70
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39596045"
---
# <a name="how-to-use-an-azure-vm-managed-service-identity-msi-for-token-acquisition"></a>Como usar uma MSI (Identidade de Serviço Gerenciado) da VM do Azure para aquisição de token 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]  

A Identidade de Serviço Gerenciado fornece aos serviços do Azure uma identidade gerenciada automaticamente no Active Directory do Azure. Você pode usar essa identidade para autenticar em qualquer serviço que dá suporte à autenticação do Azure AD, incluindo o Key Vault, sem ter as credenciais no seu código. 

Este artigo fornece vários exemplos de código e de script para aquisição de token, assim como diretrizes sobre tópicos importantes como a manipulação de expiração de token e erros HTTP. 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

Se você planeja usar os exemplos do Azure PowerShell neste artigo, instale a versão mais recente do [Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM).


> [!IMPORTANT]
> - Todo código/script de exemplo neste artigo supõe que o cliente está sendo executado em uma máquina Virtual com uma identidade de serviço gerenciado. Use o recurso "Conectar" da VM no Portal do Azure para conectar-se remotamente à sua VM. Para obter detalhes sobre como habilitar a MSI em uma VM, consulte [Configurar uma MSI (Identidade de Serviço Gerenciado) da VM usando o portal do Azure](qs-configure-portal-windows-vm.md) ou um dos artigos variantes (o PowerShell, CLI, modelo ou SDK do Azure). 

> [!IMPORTANT]
> - O limite de segurança de uma identidade de serviço gerenciado é o recurso que está sendo usado. Todos os códigos/scripts em execução em uma máquina Virtual pode solicitar e recuperar tokens para qualquer identidade de serviço gerenciado nele disponíveis. 

## <a name="overview"></a>Visão geral

Um aplicativo cliente pode solicitar uma identidade de serviço gerenciado [token de acesso somente de aplicativo](../develop/developer-glossary.md#access-token) para acessar um determinado recurso. O token é [com base na entidade de serviço da MSI](overview.md#how-does-it-work). Sendo assim, o cliente não precisa se registrar para obter um token de acesso em sua própria entidade de serviço. O token é adequado para uso como um token de portador em [chamadas de serviço a serviço que exigem credenciais de cliente](../develop/v1-oauth2-client-creds-grant-flow.md).

|  |  |
| -------------- | -------------------- |
| [Obter um token usando HTTP](#get-a-token-using-http) | Detalhes de protocolo para o ponto de extremidade do token da MSI |
| [Obtenha um token usando a biblioteca Microsoft.Azure.Services.AppAuthentication para .NET](#get-a-token-using-the-microsoftazureservicesappauthentication-library-for-net) | Exemplo de como usar a biblioteca Microsoft.Azure.Services.AppAuthentication de um cliente .NET
| [Obter um token usando C#](#get-a-token-using-c) | Exemplo de como usar o ponto de extremidade REST da MSI de um cliente C# |
| [Obter um token usando Go](#get-a-token-using-go) | Exemplo de como usar o ponto de extremidade REST da MSI de um cliente Go |
| [Obter um token usando o Azure PowerShell](#get-a-token-using-azure-powershell) | Exemplo de como usar o ponto de extremidade REST da MSI de um cliente PowerShell |
| [Obter um token usando CURL](#get-a-token-using-curl) | Exemplo de como usar o ponto de extremidade REST da MSI de um cliente Bash/CURL |
| [Tratamento de cache de token](#handling-token-caching) | Diretrizes para manipular tokens de acesso expirados |
| [Tratamento de erros](#error-handling) | Diretrizes para tratamento de erros HTTP retornados do ponto de extremidade do token da MSI |
| [IDs de recurso para serviços do Azure](#resource-ids-for-azure-services) | Onde obter IDs de recurso para os serviços do Azure compatíveis |

## <a name="get-a-token-using-http"></a>Obter um token usando HTTP 

A interface fundamental para adquirir um token de acesso é baseada em REST, tornando-a acessível para qualquer aplicativo cliente em execução na VM que pode fazer chamadas REST HTTP. Isso é semelhante ao modelo de programação do Azure AD, exceto que o cliente usa um ponto de extremidade na máquina virtual (em vez de um ponto de extremidade do Azure AD).

Exemplo de solicitação usando o ponto de extremidade do serviço de metadados na instância do Azure (IMDS) *(recomendado)*:

```
GET 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/' HTTP/1.1 Metadata: true
```

| Elemento | DESCRIÇÃO |
| ------- | ----------- |
| `GET` | O verbo HTTP, indicando que você deseja recuperar os dados do ponto de extremidade. Neste caso, um token de acesso OAuth. | 
| `http://169.254.169.254/metadata/identity/oauth2/token` | O ponto de extremidade da MSI para o Serviço de Metadados de Instância. |
| `api-version`  | Um parâmetro de cadeia de caracteres, que indica a versão de API para o ponto de extremidade IMDS. Use a versão de API `2018-02-01` ou superior. |
| `resource` | Um parâmetro de cadeia de caracteres de consulta que indica o URI da ID do aplicativo do recurso de destino. Ele também aparece na declaração `aud` (público) do token emitido. Este exemplo solicita um token para acessar o Azure Resource Manager, que tem um URI de ID do aplicativo de https://management.azure.com/. |
| `Metadata` | Um campo de cabeçalho de solicitação HTTP, exigido pela MSI como uma mitigação contra ataques de SSRF (Falsificação de Solicitação no Lado de Servidor). Esse valor deve ser definido como "true", com todas as letras minúsculas. |
| `object_id` | (Opcional) Um parâmetro de string de consulta, indicando o object_id da identidade gerenciada para a qual você deseja o token. Obrigatório, se sua VM tiver várias identidades gerenciadas atribuídas pelo usuário.|
| `client_id` | (Opcional) Um parâmetro de cadeia de consulta, indicando o client_id da identidade gerenciada para a qual você deseja o token. Obrigatório, se sua VM tiver várias identidades gerenciadas atribuídas pelo usuário.|

Exemplo de solicitação usando o ponto de extremidade de extensão de VM de identidade de serviço gerenciado (MSI) *(para ser substituído)*:

```
GET http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fmanagement.azure.com%2F HTTP/1.1
Metadata: true
```

| Elemento | DESCRIÇÃO |
| ------- | ----------- |
| `GET` | O verbo HTTP, indicando que você deseja recuperar os dados do ponto de extremidade. Neste caso, um token de acesso OAuth. | 
| `http://localhost:50342/oauth2/token` | O ponto de extremidade da MSI em que 50342 é a porta padrão e é configurável. |
| `resource` | Um parâmetro de cadeia de caracteres de consulta que indica o URI da ID do aplicativo do recurso de destino. Ele também aparece na declaração `aud` (público) do token emitido. Este exemplo solicita um token para acessar o Azure Resource Manager, que tem um URI de ID do aplicativo de https://management.azure.com/. |
| `Metadata` | Um campo de cabeçalho de solicitação HTTP, exigido pela MSI como uma mitigação contra ataques de SSRF (Falsificação de Solicitação no Lado de Servidor). Esse valor deve ser definido como "true", com todas as letras minúsculas.|
| `object_id` | (Opcional) Um parâmetro de string de consulta, indicando o object_id da identidade gerenciada para a qual você deseja o token. Obrigatório, se sua VM tiver várias identidades gerenciadas atribuídas pelo usuário.|
| `client_id` | (Opcional) Um parâmetro de cadeia de consulta, indicando o client_id da identidade gerenciada para a qual você deseja o token. Obrigatório, se sua VM tiver várias identidades gerenciadas atribuídas pelo usuário.|


Exemplo de resposta:

```
HTTP/1.1 200 OK
Content-Type: application/json
{
  "access_token": "eyJ0eXAi...",
  "refresh_token": "",
  "expires_in": "3599",
  "expires_on": "1506484173",
  "not_before": "1506480273",
  "resource": "https://management.azure.com/",
  "token_type": "Bearer"
}
```

| Elemento | DESCRIÇÃO |
| ------- | ----------- |
| `access_token` | O token de acesso solicitado. Ao chamar uma API REST protegida, o token é inserido no campo de cabeçalho de solicitação `Authorization` como um token "portador", permitindo que a API autentique o chamador. | 
| `refresh_token` | Não é usado pela MSI. |
| `expires_in` | O número de segundos que o token de acesso continua sendo válido antes da expiração desde o momento da emissão. A hora da emissão pode ser encontrada na declaração `iat` do token. |
| `expires_on` | O período de expiração do token de acesso. A data é representada como o número de segundos de “1970-01-01T0:0:0Z UTC” (corresponde à declaração `exp` do token). |
| `not_before` | O período para o token de acesso entrar em vigor e poder ser aceito. A data é representada como o número de segundos de “1970-01-01T0:0:0Z UTC” (corresponde à declaração `nbf` do token). |
| `resource` | O recurso para o qual o token de acesso foi solicitado, que corresponde ao parâmetro de cadeia de consulta `resource` da solicitação. |
| `token_type` | O tipo de token, que é um token de acesso "Portador", o que significa que o recurso pode conceder acesso ao portador desse token. |

## <a name="get-a-token-using-the-microsoftazureservicesappauthentication-library-for-net"></a>Obtenha um token usando a biblioteca Microsoft.Azure.Services.AppAuthentication para .NET

Para aplicativos e funções .NET, a maneira mais simples de trabalhar com uma identidade de serviço gerenciado é por meio do pacote Microsoft.Azure.Services.AppAuthentication. Essa biblioteca também permitirá que você teste seu código localmente em sua máquina de desenvolvimento, usando sua conta de usuário do Visual Studio, a [Azure CLI](https://docs.microsoft.com/cli/azure?view=azure-cli-latest) ou a Autenticação Integrada do Active Directory. Para saber mais sobre as opções de desenvolvimento local com essa biblioteca, consulte a [Referência Microsoft.Azure.Services.AppAuthentication]. Esta seção mostra a você como começar a usar a biblioteca no seu código.

1. Adicione uma referência aos pacotes NuGet [Microsoft.Azure.Services.AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication) e [Microsoft.Azure.KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault) no aplicativo.

2.  Adicione o código a seguir à sua página:

    ```csharp
    using Microsoft.Azure.Services.AppAuthentication;
    using Microsoft.Azure.KeyVault;
    // ...
    var azureServiceTokenProvider = new AzureServiceTokenProvider();
    string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://management.azure.com/");
    // OR
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
    ```
    
Para saber mais sobre o Microsoft.Azure.Services.AppAuthentication e as operações que ele expõe, consulte a [Referência Microsoft.Azure.Services.AppAuthentication](/azure/key-vault/service-to-service-authentication) e [Serviço de Aplicativo e KeyVault com a amostra MSI .NET](https://github.com/Azure-Samples/app-service-msi-keyvault-dotnet).

## <a name="get-a-token-using-c"></a>Obter um token usando C#

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 

// Build request to acquire MSI token
HttpWebRequest request = (HttpWebRequest)WebRequest.Create(http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https://management.azure.com/");
request.Headers["Metadata"] = "true";
request.Method = "GET";

try
{
    // Call /token endpoint
    HttpWebResponse response = (HttpWebResponse)request.GetResponse();

    // Pipe response Stream to a StreamReader, and extract access token
    StreamReader streamResponse = new StreamReader(response.GetResponseStream()); 
    string stringResponse = streamResponse.ReadToEnd();
    JavaScriptSerializer j = new JavaScriptSerializer();
    Dictionary<string, string> list = (Dictionary<string, string>) j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
    string accessToken = list["access_token"];
}
catch (Exception e)
{
    string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
}

```

## <a name="get-a-token-using-go"></a>Obter um token usando Go

```
package main

import (
  "fmt"
  "io/ioutil"
  "net/http"
  "net/url"
  "encoding/json"
)

type responseJson struct {
  AccessToken string `json:"access_token"`
  RefreshToken string `json:"refresh_token"`
  ExpiresIn string `json:"expires_in"`
  ExpiresOn string `json:"expires_on"`
  NotBefore string `json:"not_before"`
  Resource string `json:"resource"`
  TokenType string `json:"token_type"`
}

func main() {
    
    // Create HTTP request for MSI token to access Azure Resource Manager
    var msi_endpoint *url.URL
    msi_endpoint, err := url.Parse("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01")
    if err != nil {
      fmt.Println("Error creating URL: ", err)
      return 
    }
    msi_parameters := url.Values{}
    msi_parameters.Add("resource", "https://management.azure.com/")
    msi_endpoint.RawQuery = msi_parameters.Encode()
    req, err := http.NewRequest("GET", msi_endpoint.String(), nil)
    if err != nil {
      fmt.Println("Error creating HTTP request: ", err)
      return 
    }
    req.Header.Add("Metadata", "true")

    // Call MSI /token endpoint
    client := &http.Client{}
    resp, err := client.Do(req) 
    if err != nil{
      fmt.Println("Error calling token endpoint: ", err)
      return
    }

    // Pull out response body
    responseBytes,err := ioutil.ReadAll(resp.Body)
    defer resp.Body.Close()
    if err != nil {
      fmt.Println("Error reading response body : ", err)
      return
    }

    // Unmarshall response body into struct
    var r responseJson
    err = json.Unmarshal(responseBytes, &r)
    if err != nil {
      fmt.Println("Error unmarshalling the response:", err)
      return
    }

    // Print HTTP response and marshalled response body elements to console
    fmt.Println("Response status:", resp.Status)
    fmt.Println("access_token: ", r.AccessToken)
    fmt.Println("refresh_token: ", r.RefreshToken)
    fmt.Println("expires_in: ", r.ExpiresIn)
    fmt.Println("expires_on: ", r.ExpiresOn)
    fmt.Println("not_before: ", r.NotBefore)
    fmt.Println("resource: ", r.Resource)
    fmt.Println("token_type: ", r.TokenType)
}
```

## <a name="get-a-token-using-azure-powershell"></a>Obter um token usando o Azure PowerShell

O exemplo a seguir demonstra como usar o ponto de extremidade REST da MSI de um cliente do PowerShell para:

1. Adquirir um token de acesso.
2. Use o token de acesso para chamar uma API REST do Azure Resource Manager e obter informações sobre a VM. Substitua sua ID de assinatura, o nome do grupo de recursos e o nome da máquina virtual para `<SUBSCRIPTION-ID>`, `<RESOURCE-GROUP>` e `<VM-NAME>`, respectivamente.

```azurepowershell
Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Headers @{Metadata="true"}
```

Exemplo sobre como analisar o token de acesso da resposta:
```azurepowershell
# Get an access token for the MSI
$response = Invoke-WebRequest -Uri http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F `
                              -Headers @{Metadata="true"}
$content =$response.Content | ConvertFrom-Json
$access_token = $content.access_token
echo "The MSI access token is $access_token"

# Use the access token to get resource information for the VM
$vmInfoRest = (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Compute/virtualMachines/<VM-NAME>?api-version=2017-12-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $access_token"}).content
echo "JSON returned from call to get VM info:"
echo $vmInfoRest

```

## <a name="get-a-token-using-curl"></a>Obter um token usando CURL

```bash
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true -s
```


Exemplo sobre como analisar o token de acesso da resposta:

```bash
response=$(curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true -s)
access_token=$(echo $response | python -c 'import sys, json; print (json.load(sys.stdin)["access_token"])')
echo The MSI access token is $access_token
```

## <a name="token-caching"></a>Armazenamento de token em cache

Enquanto o subsistema de identidade de serviço gerenciado (MSI) que está sendo usado (extensão de VM IMDS/MSI) do cache tokens, também é recomendável para implementar o cache de token em seu código. Como resultado, você deve se preparar para cenários em que o recurso indica que o token expirou. 

Chamadas durante a transmissão para o Azure AD resultam apenas quando:
- Erro de cache ocorre devido a nenhum token no cache do subsistema de MSI
- o token está expirado

## <a name="error-handling"></a>Tratamento de erros

O ponto de extremidade de identidade de serviço gerenciado sinaliza os erros por meio do campo de código de status do cabeçalho da mensagem de resposta HTTP, como erros 4xx ou 5xx:

| Código de status | Motivo do erro | Como tratar |
| ----------- | ------------ | ------------- |
| 404 Não Encontrado: | Ponto de extremidade IMDS está atualizando. | Tente novamente com Expontential retirada. Consulte as diretrizes abaixo. |
| 429 Número excessivo de solicitações. |  Atingido o limite de restrição IMDS. | Tentar novamente com Retirada Exponencial. Consulte as diretrizes abaixo. |
| o erro 4xx na solicitação. | Um ou mais parâmetros de solicitação estava incorreto. | Não tente novamente.  Examine os detalhes do erro para obter mais informações.  Os erros 4xx são erros de tempo de design.|
| Erro 5xx transitório do serviço. | O subsistema da MSI ou o Azure Active Directory retornou um erro transitório. | É seguro tentar novamente após aguardar pelo menos 1 segundo.  Se tentar novamente muito rápido ou com muita frequência, o IMDS e/ou o Microsoft Azure Active Directory poderá retornar um erro de limite de taxa (429).|
| Tempo limite | Ponto de extremidade IMDS está atualizando. | Tente novamente com Expontential retirada. Consulte as diretrizes abaixo. |

Se ocorrer um erro, o corpo da resposta HTTP correspondente conterá o JSON com os detalhes do erro:

| Elemento | DESCRIÇÃO |
| ------- | ----------- |
| error   | Identificador do erro. |
| error_description | Descrição detalhada do erro. **Descrições de erro podem ser alteradas a qualquer momento. Não escreva código que se ramifique com base nos valores na descrição do erro.**|

### <a name="http-response-reference"></a>Referência de resposta HTTP

Esta seção documenta as possíveis respostas de erro. Um status "200 OK" é uma resposta bem-sucedida, e o token de acesso está contido no JSON do corpo de resposta, no elemento access_token.

| Código de status | Erro | Descrição do erro | Solução |
| ----------- | ----- | ----------------- | -------- |
| 400 Solicitação Inválida | invalid_resource | AADSTS50001: o aplicativo chamado *\<URI\>* não foi encontrado no locatário chamado *\<TENANT-ID\>*. Isso poderá acontecer se o aplicativo não tiver sido instalado pelo administrador do locatário ou aceito por qualquer usuário no locatário. Talvez você tenha enviado a solicitação de autenticação ao locatário errado.\ | (Apenas Linux) |
| 400 Solicitação Inválida | bad_request_102 | Cabeçalho de metadados necessário não especificado | O campo de cabeçalho de solicitação `Metadata` está ausente da solicitação ou está formatado incorretamente. O valor deve ser especificado como `true`, com todas as letras minúsculas. Consulte a "Amostra de solicitação" na [seção REST anterior](#rest) para obter um exemplo.|
| 401 Não Autorizado | unknown_source | *\<URI\>* de origem desconhecida | Verifique se o URI da solicitação HTTP GET está formatado corretamente. A parte `scheme:host/resource-path` deve ser especificada como `http://localhost:50342/oauth2/token`. Consulte a "Amostra de solicitação" na [seção REST anterior](#rest) para obter um exemplo.|
|           | invalid_request | A solicitação não tem um parâmetro obrigatório, inclui um valor de parâmetro inválido, inclui um parâmetro mais de uma vez ou está malformada. |  |
|           | unauthorized_client | O cliente não está autorizado a solicitar um token de acesso usando este método. | Causado por uma solicitação que não usou o loopback local para chamar a extensão ou em uma VM que não tem uma MSI configurada corretamente. Consulte [Configure a VM Managed Service Identity (MSI) using the Azure portal](qs-configure-portal-windows-vm.md) (Configurar uma MSI [Identidade de Serviço Gerenciado] da VM usando o Portal do Azure) se precisar de ajuda com a configuração da VM. |
|           | access_denied | O proprietário do recurso ou o servidor de autorização negou a solicitação. |  |
|           | unsupported_response_type | O servidor de autorização não dá suporte à obtenção de um token de acesso usando este método. |  |
|           | invalid_scope | O escopo solicitado é inválido, desconhecido ou malformado. |  |
| Erro interno do servidor 500 | unknown | Falha ao recuperar o token do Active Directory. Para obter detalhes, consulte os logs no *\<caminho do arquivo\>* | Verifique se a MSI foi habilitada na VM. Consulte [Configure a VM Managed Service Identity (MSI) using the Azure portal](qs-configure-portal-windows-vm.md) (Configurar uma MSI [Identidade de Serviço Gerenciado] da VM usando o Portal do Azure) se precisar de ajuda com a configuração da VM.<br><br>Verifique também se seu URI de solicitação GET HTTP foi formatado corretamente, principalmente o URI do recurso especificado na cadeia de caracteres de consulta. Consulte a "Solicitação de amostra" na [seção REST anterior](#rest) para obter um exemplo ou [Serviços do Azure que dão suporte para autenticação do Azure AD](services-support-msi.md) para obter uma lista de serviços e seus respectivos IDs de recurso.

## <a name="retry-guidance"></a>Repita a orientação 

É recomendado tentar novamente se você receber o código de erro 404, 429 ou 5xx (confira [Tratamento de erro](#error-handling) acima).

Limitação limites se aplicam ao número de chamadas feitas para o ponto de extremidade IMDS. Quando o limite de limitação é excedido, o ponto de extremidade IMDS limita qualquer solicitação adicional enquanto a limitação está em vigor. Durante esse período, o ponto de extremidade de IMDS da MSI retornará o código de status HTTP 429 ("Muitas solicitações") e as solicitações falharão. 

Para tentar novamente, é recomendável a estratégia a seguir: 

| **Estratégia de repetição** | **Configurações** | **Valores** | **Como funciona** |
| --- | --- | --- | --- |
|ExponentialBackoff |Contagem de repetição<br />Retirada mín.<br />Retirada máx.<br />Retirada delta<br />Primeira repetição rápida |5<br />0 s<br />60 s<br />2 s<br />falso |1ª tentativa — intervalo de 0 s<br />2ª tentativa — intervalo de ~2 s<br />3ª tentativa — intervalo de ~6 s<br />4ª tentativa — intervalo de ~14 s<br />5ª tentativa — intervalo de ~30 s |

## <a name="resource-ids-for-azure-services"></a>IDs de recurso para serviços do Azure

Consulte [Azure services that support Azure AD authentication](services-support-msi.md) (Serviços do Azure que dão suporte à autenticação do Azure AD) para obter uma lista dos recursos compatíveis com o Azure AD e que foram testados com a MSI e as respectivas IDs do recurso.


## <a name="related-content"></a>Conteúdo relacionado

- Para habilitar a MSI em uma VM do Azure, consulte [Configure a VM Managed Service Identity (MSI) using the Azure portal](qs-configure-portal-windows-vm.md) (Configurar uma MSI (Identidade de Serviço Gerenciado) usando o portal do Azure).

Use a seção de comentários a seguir para fornecer seus comentários e nos ajudar a aprimorar e adaptar nosso conteúdo.








