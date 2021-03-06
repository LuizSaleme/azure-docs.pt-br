---
title: Criar um modelo do Azure Resource Manager para implantar uma conta de armazenamento criptografada | Microsoft Docs
description: Use o Visual Studio Code para criar um modelo e implantar uma conta de armazenamento criptografada.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 07/20/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 7c78636a210ae90c5bfe1d0bfd35e4e05633f5cd
ms.sourcegitcommit: bf522c6af890984e8b7bd7d633208cb88f62a841
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2018
ms.locfileid: "39188192"
---
# <a name="tutorial-create-an-azure-resource-manager-template-for-deploying-an-encrypted-storage-account"></a>Tutorial: Criar um modelo do Azure Resource Manager para implantar uma conta de armazenamento criptografada

Saiba como localizar informações para preencher um modelo do Azure Resource Manager.

Neste tutorial, você pode usar um modelo de base dos modelos de Início Rápido do Azure para criar uma conta do Armazenamento do Azure.  Usando a documentação de referência do modelo, personalize o modelo de base para criar uma conta de armazenamento criptografada.

Este tutorial cobre as seguintes tarefas:

> [!div class="checklist"]
> * Abrir um modelo de Início Rápido
> * Entender o formato de modelo
> * Usar parâmetros no modelo
> * Usar variáveis no modelo
> * Editar o modelo
> * Implantar o modelo

Se você não tiver uma assinatura do Azure, [crie uma conta gratuita](https://azure.microsoft.com/free/) antes de começar.

## <a name="prerequisites"></a>Pré-requisitos

Para concluir este artigo, você precisa do seguinte:

* [Visual Studio Code](https://code.visualstudio.com/).
* Extensão das Ferramentas do Gerenciador de Recursos. Para instalar, consulte [Instalar a extensão de Ferramentas do Gerenciador de Recursos](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).

## <a name="open-a-quickstart-template"></a>Abrir um modelo de Início Rápido

O modelo usado neste início rápido é chamado [Criar uma conta de armazenamento padrão](https://azure.microsoft.com/resources/templates/101-storage-account-create/). O modelo define um recurso da conta de Armazenamento do Azure.

1. No Visual Studio Code, escolha **Arquivo**>**Abrir Arquivo**.
2. Em **Nome do arquivo**, cole a seguinte URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Escolha **Abrir** para abrir o arquivo.
4. Escolha **Arquivo**>**Salvar como** para salvar o arquivo como **azuredeploy.json** em seu computador local.

## <a name="understand-the-format"></a>Entender o formato

No VS Code, recolha o modelo até o nível raiz. Você tem uma estrutura mais simples com os seguintes elementos:

![Estrutura mais simples do modelo do Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-simplest-structure.png)

* **$schema**: especifique o local do arquivo de esquema JSON que descreve a versão da linguagem do modelo.
* **contentVersion**: especifique algum valor para esse elemento a fim de documentar alterações significativas no modelo.
* **parameters**: especifique os valores que são fornecidos quando a implantação é executada para personalizar a implantação dos recursos.
* **variables**: especifique os valores que são usados como fragmentos JSON no modelo para simplificar expressões de linguagem do modelo.
* **resources**: especifique os tipos de recursos que são implantados ou atualizados em um grupo de recursos.
* **gera**: especifique os valores que serão retornados após a implantação.

## <a name="use-parameters-in-template"></a>Usar parâmetros no modelo

Os parâmetros permitem que você personalize a implantação fornecendo valores que são personalizados para determinado ambiente. Você usa os parâmetros definidos no modelo na hora de definir valores para a conta de armazenamento.

![Parâmetros do modelo do Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-parameters.png)

Neste modelo, dois parâmetros são definidos. Observe que uma função de modelo é usada em location.defaultValue:

```json
"defaultValue": "[resourceGroup().location]",
```

A função resourceGroup() retorna um objeto que representa o grupo de recursos atual. Para obter a lista completa das funções do modelo, confira [Funções de modelo do Azure Resource Manager](./resource-group-template-functions.md).

Para usar os parâmetros definidos no modelo:

```json
"location": "[parameters('location')]",
"name": "[parameters('storageAccountType')]"
```

## <a name="use-variables-in-template"></a>Usar variáveis no modelo

Variables permite construir valores que podem ser usados em todo o modelo. Variáveis ajudam a reduzir a complexidade dos modelos.

![Variáveis de modelo do Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-variables.png)

Esse modelo define uma variável *storageAccountName*. Na definição, duas funções de modelo são usadas:

- **concat()**: concatena cadeias de caracteres. Para obter mais informações, confira [concat](./resource-group-template-functions-string.md#concat).
- **uniqueString()**: crie uma cadeia de caracteres de hash determinístico com base nos valores fornecidos como parâmetros. Cada conta de armazenamento do Azure deve ter um nome exclusivo em todo o Azure. Essa função fornece uma cadeia de caracteres exclusiva. Para obter mais funções de cadeia de caracteres, consulte [Funções de cadeia de caracteres](./resource-group-template-functions-string.md).

Para usar a variável definida no modelo:

```json
"name": "[variables('storageAccountName')]"
```

## <a name="edit-the-template"></a>Editar o modelo

Para localizar a configuração de criptografia relacionadas à conta de armazenamento, você pode usar a referência de modelo da conta do Armazenamento do Azure.

1. Navegue até [Modelos do Azure](https://docs.microsoft.com/azure/templates/).
2. No Sumário à esquerda, selecione **Referência**->**Armazenamento**->**Contas de Armazenamento**. A página contém as informações para definir informações da Conta de Armazenamento.
3. Explore as informações relacionadas à criptografia.  
4. Dentro do elemento properties da definição de recurso de conta de armazenamento, adicione o seguinte json:

    ```json
    "encryption": {
        "keySource": "Microsoft.Storage",
        "services": {
            "blob": {
                "enabled": true
            }
        }
    }
    ```
    Essa parte habilita a função de criptografia do serviço de armazenamento de blobs.

O elemento resources final fica mais ou menos assim:

![Recursos da conta de armazenamento criptografada do modelo do Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-resources.png)

## <a name="deploy-the-template"></a>Implantar o modelo

Há muitos métodos para implantar modelos.  Neste tutorial, use o Cloud Shell no portal do Azure. O Cloud Shell oferece suporte à CLI do Azure e o Azure PowerShell. As instruções fornecidas aqui usam a CLI.

1. Entre no [Portal do Azure](https://portal.azure.com)
2. Escolha **Cloud Shell** no canto superior direito, conforme mostrado na imagem a seguir:

    ![Cloud Shell no portal do Azure](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell.png)

3. Selecione a seta para baixo e selecione **Bash** se não for Bash. Você usará a CLI do Azure neste tutorial.

    ![CLI do Cloud Shell no portal do Azure](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-choose-cli.png)
4. Escolha **Reiniciar** para reiniciar o shell.
5. Escolha **Carregar/fazer o download dos arquivos** e, em seguida, escolha **Carregar**.

    ![Cloud Shell no portal do Azure carregar arquivo](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-upload-file.png)
6. Escolha o arquivo que você salvou anteriormente no tutorial. O nome padrão é **azuredeploy.json**.
7. No Cloud Shell, execute o comando **ls** para verificar se o arquivo foi carregado com êxito. Você também pode usar o comando **cat** para verificar o conteúdo do modelo.

    ![Cloud Shell no portal do Azure listar arquivo](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-list-file.png)
8. Do Cloud Shell, execute os seguintes comandos:

    ```cli
    az group create --name <ResourceGroupName> --location <AzureLocation>

    az group deployment create --name <DeploymentName> --resource-group <ResourceGroupName> --template-file azuredeploy.json
    ```
    Veja uma captura de tela de uma implantação de exemplo:

    ![Cloud Shell no portal do Azure implantar modelo](./media/resource-manager-tutorial-create-encrypted-storage-accounts/azure-portal-cloud-shell-deploy-template.png)

    Na captura de tela, esses valores são usados:

    * **&lt;ResourceGroupName>**: myresourcegroup0719. Há dois aspectos do parâmetro.  Certifique-se de usar o mesmo valor.
    * **&lt;AzureLocation>**: eastus2
    * **&lt;DeployName>**: mydeployment0719
    * **&lt;TemplateFile>**: azuredeploy.json

    Na saída da captura de tela, o nome da conta de armazenamento é *fhqbfslikdqdsstandardsa*. 

9. Execute o seguinte comando do PowerShell para listar as contas de armazenamento criadas recentemente:

    ```cli
    az storage account show --resource-group <ResourceGroupName> --name <StorageAccountName>
    ```

    Você deverá ver uma saída semelhante à seguinte captura de tela que indica a habilitação da criptografia para o armazenamento de blobs.

    ![Conta de armazenamento criptografada do Azure Resource Manager](./media/resource-manager-tutorial-create-encrypted-storage-accounts/resource-manager-template-encrypted-storage-account.png)

## <a name="clean-up-resources"></a>Limpar recursos

Quando os recursos do Azure já não forem necessários, limpe os recursos implantados excluindo o grupo de recursos.

1. No portal do Azure, escolha **Grupos de recursos** do menu à esquerda.
2. No campo **Filtrar por nome**, insira o nome do grupo de recursos.
3. Escolha o nome do grupo de recursos.  Você deverá ver um total de seis recursos no grupo de recursos.
4. Escolha **Excluir grupo de recursos** no menu superior.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você aprendeu a usar a referência de modelo para personalizar um modelo existente. O modelo usado neste tutorial contém apenas um recurso do Azure.  No próximo tutorial, você desenvolverá um modelo com vários recursos.  Alguns dos recursos têm recursos dependentes.

> [!div class="nextstepaction"]
> [Criar vários recursos](./resource-manager-tutorial-create-templates-with-dependent-resources.md)
