---
title: Instalar a extensão de Funções Duráveis e exemplos – Azure
description: Saiba como instalar a extensão de Funções Duráveis do Azure Functions, para desenvolvimento no portal ou desenvolvimento no Visual Studio.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 6ed8265a0b1a014ad15a6bb42fabb6003fb6a775
ms.sourcegitcommit: 4597964eba08b7e0584d2b275cc33a370c25e027
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2018
ms.locfileid: "37343120"
---
# <a name="install-the-durable-functions-extension-and-samples-azure-functions"></a>Instalar a extensão de Funções Duráveis e exemplos (Azure Functions)

A extensão [Funções Duráveis](durable-functions-overview.md) do Azure Functions é fornecida no pacote de NuGet [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). Este artigo mostra como instalar o pacote, bem como um conjunto de exemplos para os ambientes de desenvolvimento a seguir:

* Visual Studio 2017 (recomendado para C#) 
* Visual Studio Code (recomendado para JavaScript)
* Portal do Azure

## <a name="visual-studio-2017"></a>Visual Studio 2017

Atualmente, o Visual Studio fornece a melhor experiência para desenvolver aplicativos que usam Funções Duráveis.  Suas funções podem ser executadas localmente e também podem ser publicadas no Azure. Você pode começar com um projeto vazio ou com um conjunto de funções de exemplo.

### <a name="prerequisites"></a>pré-requisitos

* Instale a [versão mais recente do Visual Studio](https://www.visualstudio.com/downloads/) (versão 15.3 ou posterior). Inclua a carga de trabalho do **desenvolvimento do Azure** em suas opções de instalação.

### <a name="start-with-sample-functions"></a>Comece com as funções de exemplo 

1. Baixe o [arquivo .zip do Aplicativo de Exemplo para o Visual Studio](https://azure.github.io/azure-functions-durable-extension/files/VSDFSampleApp.zip). Você não precisa adicionar a referência do NuGet, porque o projeto de exemplo já a tem.
2. Instale e execute o [Emulador de Armazenamento do Azure](https://docs.microsoft.com/azure/storage/storage-use-emulator) 5.2 ou posterior. Como alternativa, você pode atualizar o arquivo *local.appsettings.json* com cadeias de conexão reais do Armazenamento do Azure.
3. Abra o projeto no Visual Studio 2017. 
4. Para obter instruções de como executar o exemplo, comece com [Encadeamento de funções – Sequência de exemplo Hello](durable-functions-sequence.md). O exemplo pode ser executado localmente ou publicado no Azure.

### <a name="start-with-an-empty-project"></a>Comece com um projeto vazio
 
Siga as mesmas instruções para começar com o exemplo, mas execute as etapas a seguir em vez de baixar o arquivo *.zip*:

1. Crie um projeto de Aplicativo de Funções.
2. Pesquise a seguinte referência de pacote NuGet usando *Gerenciar Pacotes do NuGet* e adicione-o ao projeto: Microsoft.Azure.WebJobs.Extensions.DurableTask v1.5.0
   
## <a name="visual-studio-code"></a>Visual Studio Code

O Visual Studio Code oferece uma experiência de desenvolvimento local abrangendo todas as principais plataformas: macOS, Windows e Linux.  Suas funções podem ser executadas localmente e também ser publicadas no Azure. Você pode começar com um projeto vazio ou com um conjunto de funções de exemplo.

### <a name="prerequisites"></a>pré-requisitos

* Instalar a versão mais recente do [Visual Studio Code](https://code.visualstudio.com/Download) 

* Siga as instruções em "Instalar as ferramentas básicas do Azure Functions" em [Codificar e testar o Azure Functions localmente](https://docs.microsoft.com/azure/azure-functions/functions-run-local)

    >[!IMPORTANT]
    > Se você já tem as ferramentas de plataforma cruzada do Azure Functions, deve atualizá-las para a versão mais recente disponível.

    >[!IMPORTANT]
    >As Funções Duráveis em JavaScript requerem a versão 2.x das ferramentas principais do Azure Functions.

*  Se você estiver em um computador Windows, instale e execute o [Emulador de Armazenamento do Azure](https://docs.microsoft.com/azure/storage/storage-use-emulator) 5.2 ou posterior. Como alternativa, você pode atualizar o arquivo *local.appsettings.json* com conexão real do Armazenamento do Azure. 


### <a name="start-with-sample-functions"></a>Comece com as funções de exemplo

#### <a name="c"></a>C#

1. Clone o [repositório Funções Duráveis](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navegue em seu computador para a [pasta de exemplos de script C#](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx). 
3. Instale a Extensão Durável do Azure Functions executando o seguinte em uma janela/um terminal de comando:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.5.0
    ```
4. Instale a Extensão Twilio do Azure Functions executando o seguinte em uma janela do terminal/prompt de comando:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.Twilio -v 3.0.0-beta5
    ```
5. Execute o Emulador do Armazenamento do Azure ou atualize o arquivo *local.appsettings.json* com cadeias de conexão reais do Armazenamento do Azure.
6. Abra o projeto no Visual Studio Code. 
7. Para obter instruções de como executar o exemplo, comece com [Encadeamento de funções – Sequência de exemplo Hello](durable-functions-sequence.md). O exemplo pode ser executado localmente ou publicado no Azure.
8. Inicie o projeto executando o seguinte comando no prompt/terminal de comando:
    ```bash
    func host start
    ```

#### <a name="javascript-functions-v2-only"></a>JavaScript (apenas Functions v2)

1. Clone o [repositório Funções Duráveis](https://github.com/Azure/azure-functions-durable-extension.git).
2. Navegue em seu computador para a [pasta de exemplos de JavaScript](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/javascript). 
3. Instale a Extensão Durável do Azure Functions executando o seguinte em uma janela/um terminal de comando:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.5.0
    ```
4. Restaure os pacotes npm executando o seguinte em uma janela do terminal/prompt de comando:
    
    ```bash
    npm install
    ``` 
5. Atualize o arquivo *local.appsettings.json* com a cadeia de conexão real do Armazenamento do Azure.
6. Abra o projeto no Visual Studio Code. 
7. Para obter instruções de como executar o exemplo, comece com [Encadeamento de funções – Sequência de exemplo Hello](durable-functions-sequence.md). O exemplo pode ser executado localmente ou publicado no Azure.
8. Inicie o projeto executando o seguinte comando no prompt/terminal de comando:
    ```bash
    func host start
    ```

### <a name="start-with-an-empty-project"></a>Comece com um projeto vazio
 
1. No terminal/prompt de comando, navegue até a pasta que hospedará o aplicativo de funções.
2. Instale a Extensão Durável do Azure Functions executando o seguinte em uma janela/um terminal de comando:

    ```bash
    func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.5.0
    ```
3. Crie um projeto de aplicativo de funções executando o seguinte comando:

    ```bash
    func init
    ``` 
4. Execute o Emulador do Armazenamento do Azure ou atualize o arquivo *local.appsettings.json* com cadeias de conexão reais do Armazenamento do Azure.
5. Em seguida, crie uma nova função executando o seguinte comando e siga as etapas do assistente:

    ```bash
    func new
    ```
    >[!IMPORTANT]
    > O modelo Função Durável não está disponível atualmente, mas você pode começar com uma das opções com suporte e modificar o código. Use, para referência, os exemplos de [Cliente de orquestração](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/HttpStart), [Gatilho de orquestração](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence) e [Gatilho de atividade](https://github.com/Azure/azure-functions-durable-extension/tree/master/samples/csx/E1_HelloSequence).

6. Abra a pasta do projeto no Visual Studio Code e continue modificando o código de modelo. 
7. Inicie o projeto executando o seguinte comando no prompt/terminal de comando:
    ```bash
    func host start
    ```

## <a name="azure-portal"></a>Portal do Azure

Se preferir, você poderá usar o portal do Azure para o desenvolvimento de Funções Duráveis.

   > [!NOTE]
   > As Funções Duráveis em JavaScript ainda não estão disponíveis no portal.

### <a name="create-an-orchestrator-function"></a>Crie uma função de orquestrador

1. Crie um novo aplicativo de funções em [functions.azure.com](https://functions.azure.com/signin).

2. Configure o aplicativo de funções para [usar a versão de tempo de execução 2.0](set-runtime-version.md).

   A extensão Funções Duráveis funciona no tempo de execução 1.X e no tempo de execução 2.0, mas os modelos do Portal do Azure somente estarão disponíveis ao segmentar o tempo de execução 2.0.

3. Crie uma nova função selecionando **"criar sua própria função personalizada".** .

4. Altere a **Linguagem** para **C#**, **Cenário** para **Funções Duráveis** e selecione o modelo **Iniciador de HTTP do Funções Duráveis -C#**.

5. Em **Extensões não instaladas**, clique em **Instalar** para baixar a extensão de NuGet.org. 

6. Quando a instalação for concluída, prossiga com a criação de uma função de cliente de orquestração, **"HttpStart"**, que é criada com a escolha do modelo **Iniciador de HTTP do Funções Duráveis -C#**.

7. Agora, crie uma função de orquestração **"HelloSequence"** do modelo **Orquestrador de Funções Duráveis - C#**.

8. A última função terá o nome de **"Hello"** do modelo **Atividade do Funções Duráveis - C#**.

9. Vá para a função **"HttpStart"** e copie a URL.

10. Use Postman ou cURL para chamar a função durável. Antes de testar, substitua a URL **{functionName}** pelo nome de função do orquestrador: **HelloSequence**.  Nenhum dado é necessário; use apenas o verbo POST. 

    ```bash
    curl -X POST https://{your function app name}.azurewebsites.net/api/orchestrators/HelloSequence
    ```

11. Em seguida, chame o ponto de extremidade **"statusQueryGetUri"** e você verá o status atual da Função Durável

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

12. Continue a chamar o ponto de extremidade **"statusQueryGetUri"** até que o status seja alterado para **"Concluído"** 

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

Parabéns! A primeira função durável está em execução no Portal do Azure!

## <a name="next-steps"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Executar o exemplo de encadeamento de funções](durable-functions-sequence.md)
