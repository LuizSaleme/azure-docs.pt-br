---
title: Usar a Identidade de Serviço Gerenciada da VM do Linux para acessar o Armazenamento do Azure usando uma credencial SAS
description: Um tutorial que mostra como usar uma Identidade de Serviço Gerenciada da VM do Linux para acessar o armazenamento do Azure usando uma credencial SAS, em vez de uma chave de acesso da conta de armazenamento.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: a8eb733cf90d0160fe4b36cfb8c30df3ff19566e
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258480"
---
# <a name="tutorial-use-a-linux-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Tutorial: Usar a Identidade de Serviço Gerenciada da VM do Linux para acessar o Armazenamento do Microsoft Azure por meio de uma credencial SAS

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Este tutorial mostra como habilitar a Identidade de Serviço Gerenciada para uma Máquina Virtual do Linux e então usar a Identidade de Serviço Gerenciada para obter uma credencial SAS (Assinatura de Acesso Compartilhado) de armazenamento. Especificamente, uma [credencial SAS de serviço](/azure/storage/common/storage-dotnet-shared-access-signature-part-1?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Uma SAS de Serviço permite conceder acesso limitado a objetos em uma conta de armazenamento por um tempo limitado e para um serviço específico (em nosso caso, o serviço blob) sem expor uma chave de acesso de conta. Você pode usar a credencial SAS normalmente ao realizar operações de armazenamento, por exemplo, ao usar o SDK de Armazenamento. Para este tutorial, vamos demonstrar o upload e o download de um blob usando a CLI do Armazenamento do Azure. Você saberá como:


> [!div class="checklist"]
> * Habilitar a Identidade de Serviço Gerenciada na Máquina Virtual do Linux 
> * Conceda à sua VM acesso a SAS de conta de armazenamento no Resource Manager 
> * Obter um token de acesso usando a identidade da VM e usá-lo para recuperar SAS do Resource Manager 

## <a name="prerequisites"></a>Pré-requisitos

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Entrar no Azure
Entre no Portal do Azure em [https://portal.azure.com](https://portal.azure.com).


## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Criar uma máquina virtual do Linux em um novo grupo de recursos

Para este tutorial, vamos criar uma nova VM do Linux. Você também pode habilitar a Identidade de Serviço Gerenciada em uma VM existente.

1. Clique no botão **+/Criar novo serviço** encontrado no canto superior esquerdo do portal do Azure.
2. Selecione **Computação** e, em seguida, selecione **Ubuntu Server 16.04 LTS**.
3. Insira as informações da máquina virtual. Para **Tipo de autenticação**, selecione **Chave pública SSH** ou **Senha**. As credenciais criadas permitirão que você faça logon na máquina virtual.

    ![Texto Alt da imagem](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Escolha uma **Assinatura** para a máquina virtual na lista suspensa.
5. Para selecionar um novo **Grupo de Recursos** no qual você deseja criar a máquina virtual, escolha **Criar Novo**. Ao concluir, clique em **OK**.
6. Selecione o tamanho para a VM. Para ver mais tamanhos, selecione **Exibir todos** ou altere o filtro Tipo de disco com suporte. Na folha de configurações, mantenha os padrões e clique em **OK**.

## <a name="enable-managed-service-identity-on-your-vm"></a>Habilitar a Identidade de Serviço Gerenciada em sua VM

Uma Identidade de Serviço Gerenciada de Máquina Virtual permite que você obtenha tokens de acesso do Azure AD sem a necessidade de colocar as credenciais em seu código. Habilitar a Identidade do Serviço Gerenciado em uma VM faz duas coisas: registra sua VM com o Microsoft Azure Active Directory para criar sua identidade gerenciada e configura a identidade na VM. 

1. Navegue até o grupo de recursos de sua nova máquina virtual e selecione a máquina virtual que você criou na etapa anterior.
2. Em “Configurações” da VM à esquerda, clique em **Configuração**.
3. Para registrar e habilitar a Identidade de Serviço Gerenciada, selecione **Sim**. Se você desejar desabilitá-la, escolha Não.
4. Lembre-se de clicar em **Salvar** para salvar a configuração.

    ![Texto Alt da imagem](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="create-a-storage-account"></a>Criar uma conta de armazenamento 

Se você ainda não tiver uma, agora você criará uma conta de armazenamento.  Você também pode ignorar esta etapa e concedam acesso da Identidade de Serviço Gerenciada da VM às chaves de uma conta de armazenamento existente. 

1. Clique no botão **+/Criar novo serviço** encontrado no canto superior esquerdo do portal do Azure.
2. Clique em **Armazenamento**, então **Conta de Armazenamento** e um novo painel "Criar conta de armazenamento" será exibido.
3. Insira um **Nome** para a conta de armazenamento, que você usará mais tarde.  
4. **Modelo de implantação** e **Tipo de conta** devem ser definidos como "Resource manager" e "Finalidade geral", respectivamente. 
5. Verifique se a **Assinatura** e o **Grupo de Recursos** correspondem ao que você especificou quando criou sua VM na etapa anterior.
6. Clique em **Criar**.

    ![Criar nova conta de armazenamento](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Criar um contêiner de blobs na conta de armazenamento

Mais tarde vamos carregar e baixar um arquivo para a nova conta de armazenamento. Como arquivos exigem armazenamento de blobs, é preciso criar um contêiner de blobs para armazenar o arquivo.

1. Navegue de volta para sua conta de armazenamento criado recentemente.
2. Clique no link **Contêineres** no painel à esquerda, em "Serviço Blob".
3. Clique em **+ Contêiner** na parte superior da página e um painel "Novo contêiner" deslizará para fora.
4. Dê um nome ao contêiner, selecione um nível de acesso, então clique em **OK**. O nome especificado será usado posteriormente no tutorial. 

    ![Criar um contêiner de armazenamento](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-managed-service-identity-access-to-use-a-storage-sas"></a>Conceder acesso à Identidade de Serviço Gerenciada da VM para usar uma SAS de armazenamento 

O Armazenamento do Azure não dá suporte nativo a autenticação do Azure AD.  No entanto, você pode usar uma Identidade de Serviço Gerenciada para recuperar uma SAS de armazenamento do Resource Manager e usar a SAS para acessar o armazenamento.  Nesta etapa, você concede acesso da Identidade de Serviço Gerenciada da VM à SAS da sua conta de armazenamento.   

1. Navegue de volta à sua conta de armazenamento recém-criada.   
2. Clique no link do **Controle de acesso (IAM)** no painel à esquerda.  
3. Clique em **+ Adicionar** na parte superior da página para adicionar uma nova atribuição de função para a sua VM
4. Defina **Função** como "Colaborador da Conta de Armazenamento" no lado direito da página. 
5. No próximo menu suspenso, escolha **Atribuir acesso a** ao recurso “Máquina Virtual”.  
6. Em seguida, verifique se a assinatura correta é listada no menu suspenso **Assinatura** e, em seguida, defina **Grupo de Recursos** como "Todos os grupos de recursos".  
7. Por fim, em **Selecionar**, escolha sua Máquina Virtual do Linux no menu suspenso e clique em **Salvar**.  

    ![Texto Alt da imagem](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Obter um token de acesso usando a identidade da máquina virtual e usá-la para chamar o Azure Resource Manager

Para o restante do tutorial, trabalharemos na VM criada anteriormente.

Para concluir essas etapas, você precisará de cliente SSH. Se você estiver usando o Windows, poderá usar o cliente SSH no [Subsistema do Windows para Linux](https://msdn.microsoft.com/commandline/wsl/install_guide). Se precisar de ajuda para configurar as chaves do cliente SSH, confira [Como usar chaves SSH com o Windows no Azure](../../virtual-machines/linux/ssh-from-windows.md), ou [Como criar e usar um par de chaves SSH pública e privada para VMs Linux no Azure](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. No portal do Azure, navegue até **Máquinas Virtuais**, vá para a máquina virtual do Linux e na página **Visão geral**, clique em **Conectar** na parte superior. Copie a cadeia de caracteres para conectar-se à VM. 
2. Conecte-se à VM usando um cliente SSH.  
3. Em seguida, você receberá uma solicitação para inserir sua **Senha**, adicionada durante a criação da **VM do Linux**. Assim, você deve se conectar com êxito.  
4. Use o CURL para obter um token de acesso para o Azure Resource Manager.  

    A solicitação CURL e a resposta para o token de acesso está abaixo:
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true    
    ```
    
    > [!NOTE]
    > Na solicitação anterior, o valor do parâmetro "resource" deve ser uma correspondência exata do que é esperado pelo Azure AD. Ao usar a ID de recurso do Azure Resource Manager, você deve incluir a barra à direita no URI.
    > Na resposta seguinte, o elemento access_token foi reduzido para fins de brevidade.
    
    ```bash
    {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"} 
     ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Obter uma credencial SAS do Azure Resource Manager para fazer chamadas de armazenamento

Agora use a CURL para chamar o Resource Manager usando o token de acesso recuperado da seção anterior para criar uma credencial SAS de armazenamento. Assim que tivermos a credencial SAS, poderemos chamar operações de upload/download de armazenamento.

Para esta solicitação, vamos usar os parâmetros de solicitação HTTP a seguir para criar a credencial SAS:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite.  Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Esses parâmetros são incluídos no corpo da POSTAGEM da solicitação para a credencial SAS. Para obter mais informações sobre os parâmetros para a criação de uma credencial SAS, consulte [Referência REST de SAS do Serviço de Lista](/rest/api/storagerp/storageaccounts/listservicesas).

Use a seguinte solicitação CURL para obter a credencial SAS. Substitua os valores dos parâmetros `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, `<CONTAINER NAME>` e `<EXPIRATION TIME>` pelos seus próprios valores. Substitua o valor `<ACCESS TOKEN>` pelo token de acesso que você recuperou anteriormente:

```bash 
curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/listServiceSas/?api-version=2017-06-01 -X POST -d "{\"canonicalizedResource\":\"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>\",\"signedResource\":\"c\",\"signedPermission\":\"rcw\",\"signedProtocol\":\"https\",\"signedExpiry\":\"<EXPIRATION TIME>\"}" -H "Authorization: Bearer <ACCESS TOKEN>"
```

> [!NOTE]
> O texto na URL anterior diferencia maiúsculas de minúsculas, portanto, verifique se você está usando letras maiúsculas e minúsculas para os Grupos de Recursos refletirem-nas adequadamente. Além disso, é importante saber que se trata de uma solicitação POST, não de uma solicitação GET.

A resposta de CURL retorna a credencial SAS:  

```bash 
{"serviceSasToken":"sv=2015-04-05&sr=c&spr=https&st=2017-09-22T00%3A10%3A00Z&se=2017-09-22T02%3A00%3A00Z&sp=rcw&sig=QcVwljccgWcNMbe9roAJbD8J5oEkYoq%2F0cUPlgriBn0%3D"} 
```

Crie um arquivo de blob de exemplo para carregar o contêiner de armazenamento de blobs. Em uma VM do Linux, você pode fazer isso com o seguinte comando. 

```bash
echo "This is a test file." > test.txt
```

Em seguida, autentique com o comando `az storage` da CLI usando a credencial SAS e carregue o arquivo para o contêiner de blob. Nesta etapa, você precisará [instalar a CLI do Azure mais recente](https://docs.microsoft.com/cli/azure/install-azure-cli) na sua VM, se ainda não o fez.

```azurecli-interactive
 az storage blob upload --container-name 
                        --file 
                        --name
                        --account-name 
                        --sas-token
```

Resposta: 

```JSON
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D4F9929765C139\"",
  "lastModified": "2017-09-21T03:58:56+00:00"
}
```

Além disso, baixe o arquivo usando a CLI do Azure e autentique com a credencial SAS. 

Solicitação: 

```azurecli-interactive
az storage blob download --container-name
                         --file 
                         --name 
                         --account-name
                         --sas-token
```

Resposta: 

```JSON
{
  "content": null,
  "metadata": {},
  "name": "testblob",
  "properties": {
    "appendBlobCommittedBlockCount": null,
    "blobType": "BlockBlob",
    "contentLength": 16,
    "contentRange": "bytes 0-15/16",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentMd5": "Aryr///Rb+D8JQ8IytleDA==",
      "contentType": "text/plain"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D4F9929765C139\"",
    "lastModified": "2017-09-21T03:58:56+00:00",
    "lease": {
      "duration": null,
      "state": "available",
      "status": "unlocked"
    },
    "pageBlobSequenceNumber": null,
    "serverEncrypted": false
  },
  "snapshot": null
}
```

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a usar uma Identidade de Serviço Gerenciada para uma máquina virtual do Linux para acessar um Armazenamento do Azure usando uma credencial de SAS.  Para saber mais sobre SAS de Armazenamento do Azure, confira:

> [!div class="nextstepaction"]
>[Como usar SAS (assinaturas de acesso compartilhado)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
