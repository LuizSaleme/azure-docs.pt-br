---
title: Tutorial de Python do Azure IoT Edge | Microsoft Docs
description: Este tutorial mostra como criar um módulo do IoT Edge com código em Python e implantá-lo em um dispositivo de borda.
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 06/26/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: 5766f9708d2439f42f9ad77169fd1fe7f7dc451e
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439105"
---
# <a name="tutorial-develop-and-deploy-a-python-iot-edge-module-to-your-simulated-device"></a>Tutorial: Desenvolver e implantar um módulo do IoT Edge em Python em seu dispositivo simulado

Use os módulos do Azure IoT Edge para implantar um código que implementa a lógica de negócios diretamente em seus dispositivos IoT Edge. Este tutorial o orienta através da criação e implantação de um módulo IoT Edge que filtra os dados do sensor. Você utilizará o dispositivo IoT Edge simulado que foi criado em Implantar Azure IoT Edge em um dispositivo simulado nos inícios rápidos de [Windows][lnk-quickstart-win] ou [Linux][lnk-quickstart-lin]. Neste tutorial, você aprenderá como:    

> [!div class="checklist"]
> * Usar o Visual Studio Code para criar um módulo do IoT Edge Python.
> * Usar o Visual Studio Code e o Docker para criar uma imagem do Docker e publicá-la no registro.
> * Implantar o módulo no dispositivo IoT Edge.
> * Exibir os dados gerados.


O módulo IoT Edge que criado neste tutorial filtra os dados de temperatura gerados pelo seu dispositivo. Ele somente envia mensagens upstream se a temperatura estiver acima de um limite especificado. Esse tipo de análise na borda é útil para reduzir a quantidade de dados que é comunicada e armazenada na nuvem. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Pré-requisitos

Um dispositivo do Azure IoT Edge:

* Você pode usar seu computador de desenvolvimento ou uma máquina virtual como um dispositivo do Edge seguindo as etapas no início rápido do [Linux](quickstart-linux.md).
* Os módulos de Python do IoT Edge não dão suporte a processadores ARM ou a dispositivos Windows.

Recursos de nuvem:

* Um [Hub IoT](../iot-hub/iot-hub-create-through-portal.md) na camada padrão no Azure. 

Recursos de desenvolvimento:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [Extensão do Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) para Visual Studio Code.
* [Extensão do Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) para Visual Studio Code. 
* [CE do Docker](https://docs.docker.com/engine/installation/). 
* [Python](https://www.python.org/downloads/).
* [PIP](https://pip.pypa.io/en/stable/installing/#installation) para instalar os pacotes do Python (normalmente incluídos com a instalação do Python).

## <a name="create-a-container-registry"></a>Criar um registro de contêiner
Neste tutorial, você utiliza a extensão do Azure IoT Edge do Visual Studio Code para compilar um módulo e criar uma **imagem de contêiner** dos arquivos. Em seguida, você efetua push dessa imagem para um **registro** que armazena e gerencia suas imagens. Finalmente, você implanta a imagem do seu registro para executar no dispositivo IoT Edge.  

Você pode usar qualquer registro compatível com o Docker neste tutorial. Dois serviços de registro do Docker populares disponíveis na nuvem são o [Registro de Contêiner do Azure](https://docs.microsoft.com/azure/container-registry/) e o [Hub do Docker](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). Este tutorial utiliza o Registro de Contêiner do Azure. 

1. No [Portal do Azure](https://portal.azure.com), selecione **Criar um recurso** > **Contêineres** > **Registro de Contêiner do Azure** .
2. Nomeie seu registro, escolha uma assinatura, selecione um grupo de recursos e defina a SKU para **Básica**. 
3. Selecione **Criar**.
4. Depois que o registro de contêiner for criado, navegue até ele e selecione **Chaves de acesso**. 
5. Alterne **Usuário administrador** para **Ativar**.
6. Copie os valores para **Servidor de logon**, **Nome de usuário** e **Senha**. Você usará esses valores mais tarde no tutorial. 

## <a name="create-an-iot-edge-module-project"></a>Criar um projeto de módulo do IoT Edge
As etapas a seguir criam um módulo Python do IoT Edge usando o Visual Studio Code e a extensão do Azure IoT Edge.

### <a name="create-a-new-solution"></a>Criar uma nova solução

Use o pacote do Python **cookiecutter** para criar um modelo de solução em Python que servirá de base. 

1. No Visual Studio Code, selecione **Exibir** > **Terminal Integrado** para abrir o terminal integrado do Visual Studio Code.

2. No terminal integrado, digite o seguinte comando para instalar (ou atualizar) **cookiecutter**, que você pode usar para criar o modelo de solução IoT Edge no VS Code:

    ```cmd/sh
    pip install --upgrade --user cookiecutter
    ```

3. Selecione **Exibir** > **Paleta de comandos** para abrir a paleta de comandos do VS Code. 

4. Na paleta de comandos, insira e execute o comando **Azure: Entrar** e siga as instruções para entrar em sua conta do Azure. Se já tiver entrado, pode ignorar esta etapa.

5. Na paleta de comandos, insira e execute o comando **Azure IoT Edge: nova solução IoT Edge**. Na paleta de comandos, forneça as seguintes informações para criar sua solução: 

   1. Selecione a pasta na qual deseja criar a solução. 
   2. Forneça um nome para a solução ou aceite o padrão **EdgeSolution**.
   3. Escolha **Módulo Python** como o modelo de módulo. 
   4. Nomeie seu módulo **PythonModule**. 
   5. Especifique o registro de contêiner do Azure que você criou na seção anterior como o repositório de imagens do primeiro módulo. Substitua **localhost:5000** pelo valor de servidor de logon que você copiou. A cadeia de caracteres final se parece com \<nome do registro\>.azurecr.io/pythonmodule.
 
A janela do VS Code carrega seu espaço de trabalho da solução IoT Edge: uma pasta de módulos, um arquivo de modelo do manifesto de implantação e um arquivo \.env. 

### <a name="add-your-registry-credentials"></a>Adicionar suas credenciais de registro

O arquivo do ambiente armazena as credenciais para o repositório de contêiner e as compartilha com o tempo de execução do IoT Edge. O tempo de execução precisa dessas credenciais para efetuar pull de imagens privadas para o dispositivo IoT Edge. 

1. No explorador do VS Code, abra o arquivo .env. 
2. Atualize os campos com os valores de **nome de usuário** e **senha** que você copiou do registro de contêiner do Azure. 
3. Salve o arquivo. 

### <a name="update-the-module-with-custom-code"></a>Atualizar o módulo com código personalizado

Cada modelo inclui código de exemplo que usa dados simulados de sensor do módulo **tempSensor** e os encaminha ao hub IoT. Nesta seção, adicione o código que expande o **PythonModule** para analisar as mensagens antes de enviá-las. 

1. No explorador do VS Code, abra **modules** > **PythonModule** > **main.py**.

2. No início do arquivo **main.py**, importe a biblioteca **json**:

    ```python
    import json
    ```

3. Adicione as variáveis **TEMPERATURE_THRESHOLD** e **TWIN_CALLBACKS** nos contadores globais. O limite de temperatura define o valor que a temperatura da máquina medida deve exceder para que os dados sejam enviados ao hub IoT.

    ```python
    TEMPERATURE_THRESHOLD = 25
    TWIN_CALLBACKS = 0
    ```

4. Substitua a função **receive_message_callback** pelo seguinte código:

    ```python
    # receive_message_callback is invoked when an incoming message arrives on the specified 
    # input queue (in the case of this sample, "input1").  Because this is a filter module, 
    # we forward this message to the "output1" queue.
    def receive_message_callback(message, hubManager):
        global RECEIVE_CALLBACKS
        global TEMPERATURE_THRESHOLD
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        message_text = message_buffer[:size].decode('utf-8')
        print ( "    Data: <<<%s>>> & Size=%d" % (message_text, size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        data = json.loads(message_text)
        if "machine" in data and "temperature" in data["machine"] and data["machine"]["temperature"] > TEMPERATURE_THRESHOLD:
            map_properties.add("MessageType", "Alert")
            print("Machine temperature %s exceeds threshold %s" % (data["machine"]["temperature"], TEMPERATURE_THRESHOLD))
        hubManager.forward_event_to_output("output1", message, 0)
        return IoTHubMessageDispositionResult.ACCEPTED
    ```

5. Adicione uma nova função chamada **module_twin_callback**. Essa função é invocada quando as propriedades desejadas são atualizadas.

    ```python
    # module_twin_callback is invoked when the module twin's desired properties are updated.
    def module_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global TEMPERATURE_THRESHOLD
        print ( "\nTwin callback called with:\nupdateStatus = %s\npayload = %s\ncontext = %s" % (update_state, payload, user_context) )
        data = json.loads(payload)
        if "desired" in data and "TemperatureThreshold" in data["desired"]:
            TEMPERATURE_THRESHOLD = data["desired"]["TemperatureThreshold"]
        if "TemperatureThreshold" in data:
            TEMPERATURE_THRESHOLD = data["TemperatureThreshold"]
        TWIN_CALLBACKS += 1
        print ( "Total calls confirmed: %d\n" % TWIN_CALLBACKS )
    ```

6. Na classe **HubManager**, adicione uma nova linha ao método **__init__** para inicializar a função **module_twin_callback** que você acabou de adicionar:

    ```python
    # Sets the callback when a module twin's desired properties are updated.
    self.client.set_module_twin_callback(module_twin_callback, self)
    ```

7. Salve o arquivo.

## <a name="build-your-iot-edge-solution"></a>Criar solução IoT Edge

Na seção anterior, você criou uma solução IoT Edge e adicionou um código a **PythonModule** para filtrar mensagens em que a temperatura relatada da máquina estiver abaixo do limite aceitável. Agora você precisa compilar a solução como uma imagem de contêiner e enviá-la por push para seu registro de contêiner. 

1. Entre no Docker inserindo o comando a seguir no terminal integrado do Visual Studio Code. Em seguida, envie sua imagem de módulo por push para o registro de contêiner do Azure: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Use o nome de usuário, a senha e o servidor de logon que você copiou de seu registro de contêiner do Azure na primeira seção. Você também pode recuperar esses valores da seção **Chaves de acesso** do registro no portal do Azure.

2. No explorador do VS Code, abra o arquivo deployment.template.json no espaço de trabalho da solução IoT Edge. 

   Este arquivo manda o **$edgeAgent** implantar dois módulos: **tempSensor**, que simula a dados do dispositivo, e **PythonModule**. O valor **PythonModule.image** é definido como uma versão Linux amd64 da imagem. Para saber mais sobre manifestos de implantação, consulte [Entender como os módulos do IoT Edge podem ser utilizados, configurados e reutilizados](module-composition.md).

   Esse arquivo também contém suas credenciais de registro. No arquivo de modelo, o nome de usuário e a senha são preenchidos com espaços reservados. Quando você gera o manifesto de implantação, os campos são atualizados com os valores adicionados ao arquivo .env. 

3. Adicione o módulo gêmeo **PythonModule** no manifesto de implantação. Insira o seguinte conteúdo JSON na parte inferior da seção **moduleContent**, após o módulo gêmeo do **$edgeHub**: 
    ```json
        "PythonModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Salve o arquivo.

5. No explorador do VS Code, clique com o botão direito do mouse no arquivo deployment.template.json e selecione **Compilar solução IoT Edge**. 

Quando você solicitar ao Visual Studio Code para compilar sua solução, primeiro ele usará as informações no modelo de implantação e gerará um arquivo deployment.json em uma nova pasta chamada **config**. Em seguida, ele executará dois comandos no terminal integrado: `docker build` e `docker push`. Esses dois comandos compilam seu código, conteinerizam o código em Python e enviam o código por push para o registro de contêiner especificado ao inicializar a solução. 

Você pode obter o endereço de imagem de contêiner completo com marca no comando `docker build` executado no terminal integrado do VS Code. O endereço da imagem é criado de informações do arquivo module.json com o formato \<repositório\>:\<versão\>-\<plataforma\>. Para este tutorial,ele deve ser parecido com registryname.azurecr.io/pythonmodule:0.0.1-amd64.

## <a name="deploy-and-run-the-solution"></a>Implantar e executar a solução

Você pode usar o portal do Azure para implantar o módulo Python em um dispositivo IoT Edge como foi feito nos inícios rápidos. Você também pode implantar e monitorar os módulos no Visual Studio Code. As seções a seguir usam a extensão do Azure IoT Edge para VS Code que estava listada nos pré-requisitos. Instale a extensão agora, caso ainda não tenha feito isso. 

1. Abra a paleta de comandos do VS Code selecionando **Exibir** > **Paleta de comandos**.

2. Pesquise e execute o comando **Azure: Entrar**. Siga as instruções para entrar na conta do Azure. 

3. Na paleta de comandos, pesquise e execute o comando **Hub IoT do Azure: Selecionar Hub IoT**. 

4. Selecione a assinatura que contém seu hub IoT e selecione o hub IoT que você deseja acessar.

5. No explorador do VS Code, expanda a seção **Dispositivos do Hub IoT do Azure**. 

6. Clique com o botão direito do mouse no nome do seu dispositivo IoT Edge e escolha **Criar implantação de dispositivo IoT Edge**. 

7. Navegue até a pasta de solução que contém **PythonModule**. Abra a pasta de configuração, escolha o arquivo deployment.json e escolha **Selecionar manifesto de implantação do Edge**.

8. Atualize a seção **Dispositivos Hub IoT do Azure**. Você deve ver o novo **PythonModule** sendo executado junto com o módulo **TempSensor** em **$edgeAgent** e **$edgeHub**. 

## <a name="view-generated-data"></a>Exibir os dados gerados

1. Para monitorar os dados que chegam ao hub IoT, selecione as reticências (**...** ) e selecione **Iniciar Monitoramento de Mensagens D2C**.
2. Para monitorar a mensagem D2C para um dispositivo específico, clique com o botão direito do mouse no dispositivo na lista e selecione **Iniciar Monitoramento de Mensagens D2C**.
3. Para interromper o monitoramento de dados, execute o comando **Hub IoT do Azure: Parar o monitoramento de mensagens D2C** na paleta de comandos. 
4. Para exibir ou atualizar o módulos gêmeo, clique com o botão direito do mouse no módulo na lista e selecione **Editar módulo gêmeo**. Para atualizar o módulo gêmeo, salve o arquivo JSON gêmeo, clique com o botão direito do mouse na área do editor e selecione **Atualizar Módulo Gêmeo**.
5. Para exibir os logs do Docker, instale o [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) para VS Code. Você pode encontrar os módulos em execução localmente no Docker Explorer. No menu de contexto, clique em **Mostrar Logs** para exibi-los no terminal integrado. 

## <a name="clean-up-resources"></a>Limpar recursos 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Se você pretende continuar no próximo artigo recomendado, pode manter os recursos e as configurações já criados e reutilizá-los.

Caso contrário, você pode excluir as configurações locais e os recursos do Azure criados neste artigo para evitar encargos. 

> [!IMPORTANT]
> A exclusão de recursos do Azure e dos grupos de recursos é irreversível. Quando os itens são excluídos, o grupo de recursos e todos os recursos contidos nele são excluídos permanentemente. Não exclua acidentalmente grupo de recursos ou recursos incorretos. Caso tenha criado o hub IoT dentro de um grupo de recursos existente com recursos que você deseja manter, exclua o próprio recurso hub IoT em vez de excluir o grupo de recursos.
>

Para excluir apenas o hub IoT, execute o comando abaixo usando o nome do hub e do grupo de recursos:

```azurecli-interactive
az iot hub delete --name {hub_name} --resource-group IoTEdgeResources
```


Para excluir o grupo de recursos inteiro por nome:

1. Entre no [portal do Azure](https://portal.azure.com) e selecione **Grupos de recursos**.

2. Na caixa de texto **Filtrar por nome**, insira o nome do grupo de recursos que contém seu Hub IoT. 

3. À direita do seu grupo de recursos na lista de resultados, selecione as reticências (**...**) e selecione **Excluir grupo de recursos**.

4. Você receberá uma solicitação para confirmar a exclusão do grupo de recursos. Insira novamente o nome do grupo de recursos para confirmar e selecione **Excluir**. Após alguns instantes, o grupo de recursos, e todos os recursos contidos nele, serão excluídos.

## <a name="next-steps"></a>Próximas etapas

Neste tutorial, você criou um módulo do IoT Edge com código para filtrar os dados brutos gerados pelo seu dispositivo IoT Edge. Você pode seguir para os próximos tutoriais a fim de descobrir outras formas pelas quais o Azure IoT Edge pode ajudá-lo a transformar dados em informações de negócios na borda.

> [!div class="nextstepaction"]
> [Implantar o Azure Functions como um módulo](tutorial-deploy-function.md)
> [Implantar o Azure Stream Analytics como um módulo](tutorial-deploy-stream-analytics.md)


<!-- Links -->
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md

<!-- Images -->
[1]: ./media/tutorial-csharp-module/programcs.png
[2]: ./media/tutorial-csharp-module/build-module.png
[3]: ./media/tutorial-csharp-module/docker-os.png
