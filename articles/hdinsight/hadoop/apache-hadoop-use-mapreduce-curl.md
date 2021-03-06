---
title: Usar o MapReduce e Curl com o Hadoop no HDInsight - Microsoft Azure
description: Saiba como executar trabalhos MapReduce remotamente com Hadoop no HDInsight usando o Curl.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: jasonh
ms.openlocfilehash: f6b72a464bfd5eee2bedc52fd9f7163f2c970c13
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590710"
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-rest"></a>Executar trabalhos MapReduce com Hadoop no HDInsight usando REST

Saiba como usar a API REST do WebHCat para executar trabalhos MapReduce em um Hadoop no cluster HDInsight. O Curl é usado para demonstrar como você pode interagir com o HDInsight usando solicitações HTTP brutas para executar trabalhos MapReduce.

> [!NOTE]
> Se você já estiver familiarizado com o uso de servidores Hadoop baseados em Linux, mas for iniciante no HDInsight, consulte o documento [O que você precisa saber sobre Hadoop baseado em Linux no HDInsight](../hdinsight-hadoop-linux-information.md).


## <a id="prereq"></a>Pré-requisitos

* Um Hadoop no cluster HDInsight
* Windows PowerShell ou [Curl](http://curl.haxx.se/) e [jq](http://stedolan.github.io/jq/)

## <a id="curl"></a>Executar um trabalho MapReduce

> [!NOTE]
> Ao usar o Curl ou quaisquer outras comunicações do REST com WebHCat, deve autenticar as solicitações fornecendo o nome de usuário de administrador de cluster HDInsight e a senha. Você também deve usar o nome do cluster como parte do URI usado para enviar as solicitações para o servidor.
>
> A API REST é protegida usando [autenticação básica de acesso](http://en.wikipedia.org/wiki/Basic_access_authentication). Você deve sempre fazer solicitações usando HTTPS para garantir que suas credenciais sejam enviadas com segurança para o servidor.

1. Para definir o logon do cluster utilizado pelos scripts neste documento, use um dos comandos a seguir:

    ```bash
    read -p "Enter your cluster login account name: " LOGIN
    ```

    ```powershell
    $creds = Get-Credential -UserName admin -Message "Enter the cluster login name and password"
    ```

2. Para definir o nome do cluster, use um dos seguintes comandos:

    ```bash
    read -p "Enter the HDInsight cluster name: " CLUSTERNAME
    ```

    ```powershell
    $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
    ```

3. De uma linha de comando, use o seguinte comando para verificar se você pode se conectar ao cluster HDInsight:

    ```bash
    curl -u $LOGIN -G https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clustername.azurehdinsight.net/templeton/v1/status" `
        -Credential $creds `
        -UseBasicParsing
    $resp.Content
    ```

    Você deve receber uma resposta semelhante ao JSON a seguir:

        {"status":"ok","version":"v1"}

    Os parâmetros usados nesse comando são os seguintes:

   * **-u**: o nome de usuário e a senha usada para autenticar a solicitação
   * **-G**: indica que essa operação é uma solicitação GET

   O início do URI, **https://CLUSTERNAME.azurehdinsight.net/templeton/v1**, é o mesmo para todas as solicitações.

4. Para enviar um trabalho MapReduce, use o seguinte comando:

    ```bash
    JOBID=`curl -u $LOGIN -d user.name=$LOGIN -d jar=/example/jars/hadoop-mapreduce-examples.jar -d class=wordcount -d arg=/example/data/gutenberg/davinci.txt -d arg=/example/data/output https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/mapreduce/jar | jq .id`
    echo $JOBID
    ```

    ```powershell
    $reqParams = @{}
    $reqParams."user.name" = "admin"
    $reqParams.jar = "/example/jars/hadoop-mapreduce-examples.jar"
    $reqParams.class = "wordcount"
    $reqParams.arg = @()
    $reqParams.arg += "/example/data/gutenberg/davinci.txt"
    $reqparams.arg += "/example/data/output"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/mapreduce/jar" `
       -Credential $creds `
       -Body $reqParams `
       -Method POST `
       -UseBasicParsing
    $jobID = (ConvertFrom-Json $resp.Content).id
    $jobID
    ```

    O final do URI (/mapreduce/jar) informa ao WebHCat que essa solicitação inicia um trabalho MapReduce de uma classe em um arquivo jar. Os parâmetros usados nesse comando são os seguintes:

   * **-d** já que `-G` não é usado; a solicitação padrão será o método POST. `-d` especifica os valores de dados que são enviados com a solicitação.
    * **user.name**: o usuário que está executando o comando
    * **jar**: o local do arquivo jar que contém a classe para ser executada
    * **class**: a classe que contém a lógica do MapReduce
    * **arg**: os argumentos a serem passados para o trabalho MapReduce. Nesse caso, o arquivo de texto de entrada e o diretório usados para a saída

   Esse comando deve retornar uma ID de trabalho que pode ser usada para verificar o status do trabalho:

       job_1415651640909_0026

5. Para verificar o status do trabalho, use o comando a seguir:

    ```bash
    curl -G -u $LOGIN -d user.name=$LOGIN https://$CLUSTERNAME.azurehdinsight.net/templeton/v1/jobs/$JOBID | jq .status.state
    ```

    ```powershell
    $reqParams=@{"user.name"="admin"}
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/templeton/v1/jobs/$jobID" `
       -Credential $creds `
       -Body $reqParams `
       -UseBasicParsing
    # ConvertFrom-JSON can't handle duplicate names with different case
    # So change one to prevent the error
    $fixDup=$resp.Content.Replace("jobID","job_ID")
    (ConvertFrom-Json $fixDup).status.state
    ```

    Se o trabalho for concluído, o estado retornado será `SUCCEEDED`.

   > [!NOTE]
   > Essa solicitação de Curl retorna um documento JSON com informações sobre o trabalho. Jq é usado para recuperar apenas o valor de estado.

6. Depois que o estado do trabalho for alterado para `SUCCEEDED`, você poderá recuperar os resultados do trabalho do Armazenamento de Blobs do Azure. O parâmetro `statusdir` transmitido com a consulta contém o local do arquivo de saída. Neste exemplo, o local é `/example/curl`. Esse endereço armazena a saída do trabalho no armazenamento padrão de clusters em `/example/curl`.

Você pode listar e baixar esses arquivos usando a [CLI 2.0 do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). Para obter mais informações sobre como trabalhar com blobs na CLI do Azure, consulte o documento [Usar a CLI 2.0 do Azure com o Armazenamento do Azure](../../storage/common/storage-azure-cli.md#create-and-manage-blobs).

## <a id="nextsteps"></a>Próximas etapas

Para obter informações gerais sobre trabalhos de MapReduce no HDInsight:

* [Usar o MapReduce com Hadoop no HDInsight](hdinsight-use-mapreduce.md)

Para obter informações sobre outros modos possíveis de trabalhar com Hadoop no HDInsight:

* [Usar o Hive com Hadoop no HDInsight](hdinsight-use-hive.md)
* [Usar o Pig com Hadoop no HDInsight](hdinsight-use-pig.md)

Para obter mais informações sobre a interface REST usada nesse artigo, consulte a [Referência de WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference).
