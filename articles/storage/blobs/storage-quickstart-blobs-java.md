---
title: Início Rápido do Azure – Criar um blob no armazenamento de objeto usando o SDK de Armazenamento de Java V7 | Microsoft Docs
description: Neste início rápido, você criará uma conta de armazenamento e um contêiner no armazenamento de objeto (Blob). Em seguida, você deve usar a biblioteca de clientes de armazenamento para Java a fim de carregar um blob no Armazenamento do Azure, baixar um blob e listar os blobs em um contêiner.
services: storage
author: roygara
ms.custom: mvc
ms.service: storage
ms.topic: quickstart
ms.date: 04/09/2018
ms.author: rogarana
ms.openlocfilehash: 7fc5b164792907644a0a41615436806f998a8b1e
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398702"
---
# <a name="quickstart-upload-download-and-list-blobs-using-java-sdk-v7"></a>Início Rápido: Carregar, baixar e listar blobs usando o SDK do Java V7

Neste guia de início rápido, você aprenderá como usar Java para carregar, baixar e listar blobs de bloco em um contêiner no Armazenamento de Blobs do Azure.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este guia de início rápido:

* Instale um IDE que tenha integração com o Maven

* Ou instale e configure o Maven para trabalhar da linha de comando

Este tutorial usa o [Eclipse](http://www.eclipse.org/downloads/) com a configuração "Eclipse IDE para desenvolvedores de Java".

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>Baixar o aplicativo de exemplo

O [aplicativo de exemplo](https://github.com/Azure-Samples/storage-blobs-java-quickstart) usado neste guia de início rápido é um aplicativo de console básico.  

Use o [git](https://git-scm.com/) para baixar uma cópia do aplicativo para seu ambiente de desenvolvimento. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-java-quickstart.git
```

Este comando clona o repositório para sua pasta do git local. Para abrir o projeto, inicialize o Eclipse e feche a tela de boas-vinda. Selecione **Arquivo**, em seguida, **Abrir projetos do sistema de arquivos...** . Certifique-se de que **Detectar e configurar natureza do projeto** esteja marcado. Selecione **Diretório**, em seguida, vá até onde você armazenou o repositório clonado, dentro dele, selecione a pasta **javaBlobsQuickstart**. Verifique se o projeto **javaBlobsQuickstarts** aparece como um projeto do Eclipse, em seguida, selecione **Concluir**.

Depois que o projeto termina a importação, abra **AzureApp.java** (localizado em **blobQuickstart.blobAzureApp** dentro de **/src/main/java**) e substitua `accountname` e `accountkey` dentro da cadeia de caracteres `storageConnectionString`. Em seguida, execute o aplicativo.

[!INCLUDE [storage-copy-connection-string-portal](../../../includes/storage-copy-connection-string-portal.md)]    

## <a name="configure-your-storage-connection-string"></a>Configurar a cadeia de conexão de armazenamento
    
No aplicativo, você deve fornecer a cadeia de conexão para sua conta de armazenamento. Abra o arquivo **AzureApp.Java**. Localize a variável `storageConnectionString` e cole o valor da cadeia de conexão copiada na seção anterior. A variável `storageConnectionString` deve ser semelhante ao seguinte:

```java
public static final String storageConnectionString =
"DefaultEndpointsProtocol=https;" +
"AccountName=<account-name>;" +
"AccountKey=<account-key>";
```

## <a name="run-the-sample"></a>Execute o exemplo

Este exemplo cria um arquivo de teste em sua diretório padrão (Meus Documentos, para usuários do Windows), carrega-o para o armazenamento de Blobs, lista os blobs no contêiner e faz o download do arquivo com um novo nome para que você possa comparar o arquivo novo com antigo. 

Execute o exemplo usando o Maven na linha de comando. Abra um shell e navegue até **blobAzureApp** dentro de seu diretório clonado. Em seguida, digite `mvn compile exec:java`. 

Este é um exemplo de saída se você executar o aplicativo no Windows.

```
Azure Blob storage quick start sample
Creating container: quickstartcontainer
Creating a sample file at: C:\Users\<user>\AppData\Local\Temp\sampleFile514658495642546986.txt
Uploading the sample file 
URI of blob is: https://myexamplesacct.blob.core.windows.net/quickstartcontainer/sampleFile514658495642546986.txt
The program has completed successfully.
Press the 'Enter' key while in the console to delete the sample files, example container, and exit the application.

Deleting the container
Deleting the source, and downloaded files
```

Antes de continuar, verifique o diretório padrão (Meus Documentos para usuários do Windows) para os dois arquivos. Você pode abri-los e ver que eles são idênticos. Copie a URL para o blob fora da janela do console e cole-a em um navegador para exibir o conteúdo do arquivo no Armazenamento de Blobs. Quando você pressiona a tecla Enter, ele exclui o contêiner de armazenamento e os arquivos. 

Você também pode usar uma ferramenta como o [Gerenciador de Armazenamento do Azure](http://storageexplorer.com/?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) para exibir os arquivos no Armazenamento de Blobs. O Gerenciador de Armazenamento do Azure é uma ferramenta gratuita de multiplataforma que permite que você acesse as informações da sua conta de armazenamento.

Depois de verificar os arquivos, pressione a tecla Enter para concluir a demonstração e excluir os arquivos de teste. Agora que você sabe o que o exemplo faz, abra o arquivo **AzureApp.java** para examinar o código. 

## <a name="understand-the-sample-code"></a>Entender o código de exemplo

Em seguida, vamos percorrer o código de exemplo para que você possa entender como ele funciona.

### <a name="get-references-to-the-storage-objects"></a>Obter referências a objetos de armazenamento

A primeira coisa a fazer é criar as referências aos objetos usados para acessar e gerenciar o Armazenamento de Blobs. Esses objetos dependem uns dos outros, cada um é usado pelo próximo na lista.

* Crie uma instância do objeto [CloudStorageAccount](/java/api/com.microsoft.azure.management.storage._storage_account) apontando para a conta de armazenamento.

    O objeto **CloudStorageAccount** é uma representação de sua conta de armazenamento e permite definir e acessar propriedades da conta de armazenamento de forma programática. Usando o objeto **CloudStorageAccount**, você pode criar uma instância do **CloudBlobClient**, que é necessário para acessar o serviço blob.

* Crie uma instância do objeto **CloudBlobClient**, que aponta para o [serviço Blob](/java/api/com.microsoft.azure.storage.blob._cloud_blob_client) na sua conta de armazenamento.

    O **CloudBlobClient** fornece um ponto de acesso ao serviço blob, permitindo que você defina e acesse as propriedades do armazenamento de blobs de forma programática. Usando o **CloudBlobClient** você pode criar uma instância do objeto **CloudBlobContainer**, que é necessário para criar contêineres.

* Crie uma instância do objeto [CloudBlobContainer](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container) que representa o contêiner que você está acessando. Os contêineres são usados para organizar seus blobs da mesma forma que você usa pastas no seu computador para organizar seus arquivos.    

    Assim que tiver o **CloudBlobContainer**, será possível criar uma instância do objeto [CloudBlockBlob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob), que aponta para o blob específico em que você está interessado e realizar a operação de upload, download, cópia etc.

> [!IMPORTANT]
> Os nomes de contêiner devem estar em minúsculas. Consulte [Naming and Referencing Containers, Blobs, and Metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Nomenclatura e referência de contêineres, blobs e metadados) para obter mais informações sobre nomes de contêiner e de blobs.

### <a name="create-a-container"></a>Criar um contêiner

Nesta seção, você cria uma instância de objetos, cria um novo contêiner e, em seguida, define as permissões no contêiner para que os blobs sejam públicos e possam ser acessados com apenas uma URL. O contêiner é chamado de **quickstartblobs**. 

Este exemplo usa [CreateIfNotExists](/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.createifnotexists) porque queremos criar um novo contêiner cada vez que a amostra for executada. Em um ambiente de produção em que você usa o mesmo contêiner em um aplicativo, é uma prática recomendada chamar **CreateIfNotExists** somente uma vez. Como alternativa, é possível criar o contêiner antecipadamente para não precisar criá-lo no código.

```java
// Parse the connection string and create a blob client to interact with Blob storage
storageAccount = CloudStorageAccount.parse(storageConnectionString);
blobClient = storageAccount.createCloudBlobClient();
container = blobClient.getContainerReference("quickstartcontainer");

// Create the container if it does not exist with public access.
System.out.println("Creating container: " + container.getName());
container.createIfNotExists(BlobContainerPublicAccessType.CONTAINER, new BlobRequestOptions(), new OperationContext());
```

### <a name="upload-blobs-to-the-container"></a>Carregar blobs para o contêiner

O Armazenamento de Blobs dá suporte a blobs de blocos, blobs de acréscimo e blobs de páginas. Os blobs de bloco são utilizados com mais frequência e é o que é usado nesse guia de início rápido. 

Para carregar um arquivo em um blob, obtenha uma referência para o blob no contêiner de destino. Após obter a referência de blob, você pode carregar dados nele usando [CloudBlockBlob.Upload](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload#com_microsoft_azure_storage_blob__cloud_block_blob_upload_final_InputStream_final_long). Essa operação cria o blob, se ele ainda não existir, ou o substitui, se ele já existir.

O código de exemplo cria um arquivo local a ser usado para upload e download. armazenando o arquivo a ser carregado como a **fonte** e o nome do blob em **blob**. O exemplo a seguir carrega o arquivo para seu contêiner chamado **quickstartblobs**.

```java
//Creating a sample file
sourceFile = File.createTempFile("sampleFile", ".txt");
System.out.println("Creating a sample file at: " + sourceFile.toString());
Writer output = new BufferedWriter(new FileWriter(sourceFile));
output.write("Hello Azure!");
output.close();

//Getting a blob reference
CloudBlockBlob blob = container.getBlockBlobReference(sourceFile.getName());

//Creating blob and uploading file to it
System.out.println("Uploading the sample file ");
blob.uploadFromFile(sourceFile.getAbsolutePath());
```

Existem vários métodos para carregar [upload](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.upload), [uploadBlock](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadblock), [uploadFullBlob](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadfullblob), [uploadStandardBlobTier](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadstandardblobtier)e [uploadText](/java/api/com.microsoft.azure.storage.blob._cloud_block_blob.uploadtext) que você pode usar com o armazenamento de Blobs. Por exemplo, caso você tenha uma cadeia de caracteres, use o método UploadText, em vez do método Upload. 

Blobs de bloco podem ser qualquer tipo de arquivo binário ou de texto. Os blobs de páginas são usados principalmente para os arquivos VHD usados auxiliar as VMs IaaS. Os blobs de acréscimo são usados para registro em log, como quando você quer gravar em um arquivo e depois adicionar mais informações. A maioria dos objetos armazenados no Armazenamento de Blobs são blobs de blocos.

### <a name="list-the-blobs-in-a-container"></a>Listar os blobs em um contêiner

Você pode obter uma lista de arquivos no contêiner usando [CloudBlobContainer.ListBlobs](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.listblobs#com_microsoft_azure_storage_blob__cloud_blob_container_listBlobs). O código a seguir recupera a lista de blobs e a percorre, mostrando os URIs dos blobs encontrados. Você pode copiar o URI da janela de comando e colá-lo em um navegador para exibir o arquivo.

```java
//Listing contents of container
for (ListBlobItem blobItem : container.listBlobs()) {
    System.out.println("URI of blob is: " + blobItem.getUri());
}
```

### <a name="download-blobs"></a>Baixar blobs

Faça download dos blobs para o disco local usando [CloudBlob.DownloadToFile](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob.downloadtofile#com_microsoft_azure_storage_blob__cloud_blob_downloadToFile_final_String).

O código a seguir baixa o blob carregado em uma seção anterior, adicionando um sufixo de "_DOWNLOADED" ao nome do blob para que você possa ver ambos os arquivos no disco local. 

```java
// Download blob. In most cases, you would have to retrieve the reference
// to cloudBlockBlob here. However, we created that reference earlier, and 
// haven't changed the blob we're interested in, so we can reuse it. 
// Here we are creating a new file to download to. Alternatively you can also pass in the path as a string into downloadToFile method: blob.downloadToFile("/path/to/new/file").
downloadedFile = new File(sourceFile.getParentFile(), "downloadedFile.txt");
blob.downloadToFile(downloadedFile.getAbsolutePath());
```

### <a name="clean-up-resources"></a>Limpar recursos

Se você não precisar mais dos blobs carregados neste guia de início rápido, poderá excluir o contêiner inteiro usando [CloudBlobContainer.DeleteIfExists](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.blob._cloud_blob_container.deleteifexists#com_microsoft_azure_storage_blob__cloud_blob_container_deleteIfExists). Isso também exclui os arquivos no contêiner.

```java
try {
if(container != null)
    container.deleteIfExists();
} catch (StorageException ex) {
System.out.println(String.format("Service error. Http code: %d and error code: %s", ex.getHttpStatusCode(), ex.getErrorCode()));
}

System.out.println("Deleting the source, and downloaded files");

if(downloadedFile != null)
downloadedFile.deleteOnExit();
        
if(sourceFile != null)
sourceFile.deleteOnExit();
```

## <a name="next-steps"></a>Próximas etapas

Nesse guia de início rápido, você aprendeu a transferir arquivos entre um disco local e o Armazenamento de Blobs do Azure usando Java. Para saber mais sobre como trabalhar com Java, prossiga para o nosso repositório de código de origem do GitHub.

> [!div class="nextstepaction"]
> [SDK do Armazenamento do Azure para Java](https://github.com/azure/azure-storage-java) 
> [Referência da API](https://docs.microsoft.com/en-us/java/api/storage/client?view=azure-java-stable)
> [Exemplos de código para Java](../common/storage-samples-java.md)

* Para obter mais informações sobre o Gerenciador de Armazenamento e os Blobs, consulte [Gerenciar os recursos de Armazenamento de Blobs do Azure com o Gerenciador de Armazenamento](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).