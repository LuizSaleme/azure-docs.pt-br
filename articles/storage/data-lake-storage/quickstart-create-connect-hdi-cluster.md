---
title: Configuração de cluster Hadoop, Spark, Kafka, HBase ou R Server – Azure HDInsight
description: Configure clusters Hadoop, Kafka, Spark, HBase, R Server ou Storm para HDInsight de um navegador, da CLI do Azure, do Azure PowerShell, do REST ou do SDK.
keywords: instalação de cluster hadoop, instalação de cluster kafka, instalação de cluster spark, o que é um cluster no hadoop
services: storage
author: jamesbak
tags: azure-portal
ms.component: data-lake-storage-gen2
ms.service: storage
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: jamesbak
ms.openlocfilehash: e0816e8609ba1ab0ef1b4f685731339378fee844
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39525579"
---
# <a name="quickstart-set-up-clusters-in-hdinsight"></a>Início Rápido: configurar clusters em HDInsight

Nesse início rápido, saiba como instalar e configurar clusters em HDInsight com Hadoop, Spark, Kafka, Consulta Interativa, HBase, R Server ou Storm. Você também vai aprender a personalizar clusters, juntá-los em um domínio e anexá-los ao [Azure Data Lake Storage Gen2 Versão Prévia](introduction.md).

Um cluster Hadoop é composto por várias máquinas virtuais (nós), usadas para processamento distribuído de tarefas. O Azure HDInsight manipula os detalhes de implementação da instalação e configuração de nós individuais, portanto, você precisa fornecer apenas informações de configuração geral.

> [!IMPORTANT]
>A cobrança do cluster HDInsight começa quando um cluster é criado e para quando o cluster é excluído. A cobrança ocorre por minuto, portanto, sempre exclua o cluster quando ele não estiver mais sendo usado. Saiba como [excluir um cluster.](../../hdinsight/hdinsight-delete-cluster.md)

O Data Lake Storage é usado como a camada de dados nesse início rápido. Com seu Serviço de Namespace Hierárquico e o [driver do Hadoop](abfs-driver.md), o Data Lake Storage é otimizado para processamento e análise distribuídos. Os dados armazenados no Data Lake Storage persistem mesmo depois que um cluster HDInsight é excluído.

## <a name="cluster-setup-methods"></a>Métodos de instalação de cluster

A tabela a seguir mostra os diferentes métodos que você pode usar para configurar um cluster HDInsight.

| Clusters criados com | Navegador da Web | Linha de comando | API REST | . | 
| --- |:---:|:---:|:---:|:---:|
| [portal do Azure](../../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md) |✔ |&nbsp; |&nbsp; |&nbsp; |
| [Azure Data Factory](../../hdinsight/hdinsight-hadoop-create-linux-clusters-adf.md) |✔ |✔ |✔ |✔ |
| [CLI do Azure (versão 1.0)](../../hdinsight/hdinsight-hadoop-create-linux-clusters-azure-cli.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [PowerShell do Azure](../../hdinsight/hdinsight-hadoop-create-linux-clusters-azure-powershell.md) |&nbsp; |✔ |&nbsp; |&nbsp; |
| [Curl](../../hdinsight/hdinsight-hadoop-create-linux-clusters-curl-rest.md) |&nbsp; |✔ |✔ |&nbsp; |
| [Modelos do Gerenciador de Recursos do Azure](../../hdinsight/hdinsight-hadoop-create-linux-clusters-arm-templates.md) |&nbsp; |✔ |&nbsp; |&nbsp; |

## <a name="quick-create-basic-cluster-setup"></a>Criação rápida: instalação de cluster Básico

Este artigo orienta você pela instalação no [Portal do Azure](https://portal.azure.com), em que você pode criar um cluster HDInsight usando a *Criação rápida* ou *Personalizada*. 

![criação rápida personalizada das opções de criação do hdinsight](media/quickstart-create-connect-hdi-cluster/hdinsight-creation-options.png)

Siga as instruções na tela para fazer uma instalação de cluster básico. Os detalhes são fornecidos abaixo para:

* [Nome do grupo de recursos](#resource-group-name)
* [Tipos de cluster e configuração](#cluster-types) 
* [Logon de cluster e o nome de usuário SSH](#cluster-login-and-ssh-user-name)
* [Localidade](#location)

> [!IMPORTANT]
> O Linux é o único sistema operacional usado no HDInsight versão 3.4 ou superior. Para obter mais informações, confira [HDInsight 3.3 retirement](../../hdinsight/hdinsight-component-versioning.md#hdinsight-windows-retirement) (Desativação do HDInsight 3.3).

## <a name="resource-group-name"></a>Nome do grupo de recursos

O [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) ajuda você a trabalhar com os recursos de seu aplicativo como um grupo, chamado de grupo de recursos do Azure. Você pode implantar, atualizar, monitorar ou excluir todos os recursos do seu aplicativo com uma única operação coordenada.

## <a name="cluster-types"></a> Tipos de cluster e configuração
Atualmente, o Azure HDInsight apresenta os seguintes tipos de cluster, cada um com um conjunto de componentes para fornecer determinadas funcionalidades.

> [!IMPORTANT]
> Clusters HDInsight estão disponíveis em vários tipos, cada um para uma carga de trabalho ou tecnologia distinta. Não há nenhum método com suporte para criar um cluster que combina vários tipos, como o Storm e HBase em um cluster. Se sua solução exige tecnologias que sejam distribuídas entre vários tipos de cluster HDInsight, uma [rede virtual do Azure](https://docs.microsoft.com/azure/virtual-network) pode conectar os tipos de cluster necessários. 
>
>

| Tipo de cluster | Funcionalidade |
| --- | --- |
| [Hadoop](../../hdinsight/hadoop/apache-hadoop-introduction.md) |Consulta de Lote e a análise de dados armazenados |
| [HBase](../../hdinsight/hbase/apache-hbase-overview.md) |Processamento de grandes quantidades de dados NoSQL sem esquema |
| [Consulta Interativa](../../hdinsight/interactive-query/apache-interactive-query-get-started.md) |Caching na memória para consultas de Hive interativas e mais rápidas |
| [Kafka](../../hdinsight/kafka/apache-kafka-introduction.md) | Uma plataforma de streaming distribuída que pode ser usada para compilar pipelines e aplicativos de dados de streaming em tempo real |
| [Servidor R](../../hdinsight/r-server/r-server-overview.md) |Uma variedade de recursos de estatísticas de Big Data, modelagem preditiva e aprendizado de máquina |
| [Spark](../../hdinsight/spark/apache-spark-overview.md) |Processamento na memória, consultas interativas, processamento de transmissão de microlotes |
| [Storm](../../hdinsight/storm/apache-storm-overview.md) |Processamento de eventos em tempo real |

### <a name="hdinsight-version"></a>Versão do HDInsight

Escolha a versão do HDInsight para este cluster. Para saber mais, confira [Versões do HDInsight com suporte](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="enterprise-security-package"></a>Pacote de segurança empresarial

Para tipos de cluster Hadoop, Spark e Consulta Interativa, você pode optar por habilitar o **Pacote de Segurança Empresarial**. Esse pacote fornece a opção de ter uma configuração mais segura de cluster usando o Apache Ranger e integrando com o Azure Active Directory. Para obter mais informações, consulte [Pacote de Segurança Empresarial no Azure HDInsight](../../hdinsight/domain-joined/apache-domain-joined-introduction.md).

![opções de criação do hdinsight escolher pacote de segurança empresarial](./media/quickstart-create-connect-hdi-cluster/hdinsight-creation-enterprise-security-package.png)

Para obter mais informações sobre a criação de cluster HDInsight ingressado no domínio, consulte [Criar ambiente de área restrita do HDInsight ingressado no domínio](../../hdinsight/domain-joined/apache-domain-joined-configure.md).

## <a name="cluster-login-and-ssh-user-name"></a>Logon de cluster e o nome de usuário SSH

Com os clusters HDInsight, você pode configurar duas contas de usuário durante a criação de cluster:

* Usuário HTTP: o nome de usuário padrão é *admin*. Ele usa a configuração básica no portal do Azure. Às vezes, ele é chamado "Usuário de cluster".
* Usuário SSH (clusters Linux): é usado para conectar-se ao cluster usando SSH. Para obter mais informações, confira [Usar SSH com HDInsight](../../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md).

O Pacote de Segurança Empresarial permite que você integre o HDInsight com o Active Directory e o Apache Ranger. Vários usuários podem ser criados usando o pacote de segurança empresarial.

## <a name="location"></a>Local (regiões) para armazenamento e clusters

Você não precisa especificar o local do cluster explicitamente; o cluster está no mesmo local que o armazenamento padrão. Para ver uma lista das regiões com suporte, clique na lista suspensa **Região** em [Preços do HDInsight](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

## <a name="storage-endpoints-for-clusters"></a>Pontos de extremidade de armazenamento para clusters

Embora uma instalação local do Hadoop use o HDFS (Sistema de Arquivos Distribuído Hadoop) para armazenamento no cluster, na nuvem você usa pontos de extremidade de armazenamento conectados ao cluster. Clusters HDInsight usam o [Data Lake Storage Gen2](abfs-driver.md) ou [Blobs no Armazenamento do Azure](../../hdinsight/hdinsight-hadoop-use-blob-storage.md). Usar o Armazenamento do Azure ou o Data Lake Storage significa que você pode excluir com segurança os clusters HDInsight usados para cálculo enquanto ainda retém os dados.

> [!WARNING]
> Não há suporte para usar uma conta de armazenamento adicional em um local diferente do cluster HDInsight.

Durante a configuração, para o ponto de extremidade de armazenamento padrão você especifica o Data Lake Storage. O armazenamento padrão contém os logs de aplicativo e de sistema. Opcionalmente, você pode especificar contas adicionais do Azure Data Lake Storage vinculadas que o cluster possa acessar. O cluster HDInsight e as contas de armazenamento padrão dependentes devem estar na mesma localização do Azure.

![Configurações de armazenamento de cluster: pontos de extremidade de armazenamento compatível com HDFS](media/quickstart-create-connect-hdi-cluster/hdinsight-cluster-creation-storage2.png)

> [!IMPORTANT]
> **Desabilite o acesso ao Data Lake Store**. Essas configurações se referem à antiga funcionalidade do *Data Lake Store* e precisam ser desabilitadas para que os recursos do *Data Lake Storage* operem corretamente.

[!INCLUDE [secure-transfer-enabled-storage-account](../../../includes/hdinsight-secure-transfer.md)]

### <a name="optional-metastores"></a>Metastores opcionais
Você pode criar metastores Hive ou Oozie opcionais. No entanto, nem todos os tipos de cluster dão suporte a metastores e o SQL Data Warehouse do Azure não é compatível com metastores. 

Para obter mais informações, consulte [Usar armazenamentos de metadados externos no Azure HDInsight](../../hdinsight/hdinsight-use-external-metadata-stores.md).

> [!IMPORTANT]
> Ao criar um metastore personalizado, não use traços, hifens ou espaços no nome do banco de dados. Isso pode fazer com que o processo de criação de cluster falhe.

### <a name="use-hiveoozie-metastore"></a>Metastore do Hive

Se você deseja reter suas tabelas Hive depois de excluir um cluster HDInsight, use uma metastore personalizada. Você poderá então anexar a metastore a outro cluster HDInsight.

Um metastore do HDInsight criado para uma versão de cluster do HDInsight não pode ser compartilhado entre diferentes versões de cluster do HDInsight. Para obter uma lista das versões do HDInsight, consulte [Versões do HDInsight com suporte](../../hdinsight/hdinsight-component-versioning.md#supported-hdinsight-versions).

### <a name="oozie-metastore"></a>Metastore do Oozie

Para aumentar o desempenho durante o uso do Oozie, use um metastore personalizado. Uma metastore também pode fornecer acesso a dados de trabalho do Oozie depois de você excluir o cluster. 

> [!IMPORTANT]
> Não é possível reutilizar um metastore personalizado do Oozie. Para usar um metastore personalizado do Oozie, é necessário fornecer um Banco de Dados SQL do Azure vazio ao criar o cluster HDInsight.

## <a name="configure-cluster-size"></a>Configurar o tamanho do cluster

Você é cobrado pelo uso de nó enquanto o cluster existe. A cobrança é iniciada quando um cluster é criado e para quando o cluster é excluído. Os clusters não podem ser desalocados ou colocados em espera.


### <a name="number-of-nodes-for-each-cluster-type"></a>Número de nós para cada tipo de cluster
Cada tipo de cluster tem seu próprio número de nós, terminologia para nós no cluster e tamanho da VM padrão. Na tabela a seguir, o número de nós para cada tipo de nó está entre parênteses.

| Tipo | Nós | Diagrama |
| --- | --- | --- |
| O Hadoop |Nó de cabeçalho (2), Nó de dados (1+) |![Nós de cluster Hadoop do HDInsight](media/quickstart-create-connect-hdi-cluster/hdinsight-hadoop-cluster-type-nodes.png) |
| HBase |Servidor de cabeçalho (2), Servidor de região (1 +), Nó mestre/do ZooKeeper (3) |![Nós de cluster HBase do HDInsight](media/quickstart-create-connect-hdi-cluster/hdinsight-hbase-cluster-type-setup.png) |
| Storm |Nó do Nimbus (2), Servidor do supervisor (1+), Nó do ZooKeeper (3) |![Nós de cluster Storm do HDInsight](media/quickstart-create-connect-hdi-cluster/hdinsight-storm-cluster-type-setup.png) |
| Spark |Nó de cabeçalho (2), nó de trabalho (1+), nó do ZooKeeper (3) (gratuito para tamanho de VM A1 do ZooKeeper) |![Nós de cluster Spark do HDInsight](media/quickstart-create-connect-hdi-cluster/hdinsight-spark-cluster-type-setup.png) |

Para obter mais informações, consulte [Configuração de nó padrão e tamanhos de máquina virtual para clusters](../../hdinsight/hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters) em "Quais são os componentes do Hadoop e as versões no HDInsight?"

O custo de clusters HDInsight é determinado pelo número de nós e pelos tamanhos de máquinas virtuais para os nós. 

Diferentes tipos de cluster têm diferentes tipos de nó, números de nós e tamanhos de nós:
* Padrão de tipo de cluster Hadoop: 
    * Dois *nós de cabeçalho*  
    * Quatro *nós de dados*
* Padrão de tipo de cluster Storm: 
    * Dois *nós Nimbus*
    * Três *nós ZooKeeper*
    * Quatro *nós supervisores* 

Se você estiver apenas experimentando o HDInsight, é recomendável usar um nó de dados. Para obter mais informações sobre os preços do HDInsight, confira [Preços do HDInsight](https://go.microsoft.com/fwLink/?LinkID=282635&clcid=0x409).

> [!NOTE]
> O limite de tamanho do cluster varia entre as assinaturas do Azure. Contate o [suporte de cobrança do Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) para aumentar o limite.
>

Quando você usar o portal do Azure para configurar o cluster, o tamanho do nó está disponível na folha **Tipos de Preço de Nó** . No portal, você também pode ver o custo associado aos diferentes tamanhos de nós. 

![Tamanhos de nó de VM do HDInsight](media/quickstart-create-connect-hdi-cluster/hdinsight-node-sizes.png)

### <a name="virtual-machine-sizes"></a>Tamanhos de máquina virtual 
Quando você implantar clusters, escolha os recursos de computação com base na solução que você planeja implantar. As VMs a seguir são usadas para clusters HDInsight:
* VMs A e série D1-4: [Tamanhos de VM Linux de uso geral](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-general)
* VM série D11-14: [Tamanhos de VM Linux com otimização de memória](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory)

Para descobrir qual valor você deve usar para especificar um tamanho de VM ao criar um cluster usando os diferentes SDKs ou ao usar o Azure PowerShell, consulte [Tamanhos de VM a serem usados para clusters HDInsight](../../cloud-services/cloud-services-sizes-specs.md#size-tables). Deste artigo vinculado, use o valor na coluna **tamanho** das tabelas.

> [!IMPORTANT]
> Se você precisa de mais de 32 nós de trabalho em um cluster, você precisa selecionar um tamanho de nó de cabeçalho com pelo menos 8 núcleos e 14 GB de RAM.
>
>

Para obter mais informações, confira [Tamanhos das máquinas virtuais](../../virtual-machines/windows/sizes.md). Para obter informações sobre os preços dos vários tamanhos, confira [Preços do HDInsight](https://azure.microsoft.com/pricing/details/hdinsight).   

## <a name="custom-cluster-setup"></a>Instalação de cluster personalizado
A instalação de cluster personalizado é realizada usando as configurações de Início rápido e adiciona as seguintes opções:
- [Aplicativos HDInsight](#hdinsight-applications)
- [Tamanho do cluster](#cluster-size)
- Configurações avançadas
  - [Ações de script](#customize-clusters-using-script-action)
  - [Rede virtual](#use-virtual-network)

## <a name="install-hdinsight-applications-on-clusters"></a>Instalar aplicativos HDInsight em clusters

Um aplicativo do HDInsight é um aplicativo que os usuários podem instalar em um cluster HDInsight baseado em Linux. Você pode usar os aplicativos fornecidos pela Microsoft, a terceiros ou que você desenvolver por conta própria. Para saber mais, confira [Instalar aplicativos de terceiros do Hadoop no Azure HDInsight](../../hdinsight/hdinsight-apps-install-applications.md).

A maioria dos aplicativos HDInsight é instalada em um nó de borda vazia.  Um nó de borda vazio é uma máquina virtual Linux com as mesmas ferramentas de cliente instaladas e configuradas do nó principal. Você pode usar o nó de borda para acessar o cluster, testar e hospedar seus aplicativos clientes. Para saber mais, confira [Usar nós de borda vazia no HDInsight](../../hdinsight/hdinsight-apps-use-edge-node.md).

## <a name="advanced-settings-script-actions"></a>Configurações avançadas: ações de Script

Você pode instalar componentes adicionais ou personalizar a configuração de cluster por meio de scripts durante a criação. Tais scripts são chamados usando a **Ação de Script**, que é uma opção de configuração que pode ser usada no portal do Azure, cmdlets do Windows PowerShell do HDInsight ou SDK do .NET do HDInsight. Para obter mais informações, consulte [Personalizar cluster HDInsight usando a Ação de Script](../../hdinsight/hdinsight-hadoop-customize-cluster-linux.md).

Alguns componentes nativos do Java, como Mahout e Cascading, podem ser executados no cluster como arquivos Java Archive (JAR). Esses arquivos JAR podem ser distribuídos para o Armazenamento do Azure ou o Azure Data Lake Storage e enviados aos clusters HDInsight por meio de mecanismos de envio de trabalho do Hadoop. Para obter mais informações, consulte [Enviar trabalhos do Hadoop de forma programática](../../hdinsight/hadoop/submit-apache-hadoop-jobs-programmatically.md).

> [!NOTE]
> Se você tiver problemas para implantar arquivos JAR nos clusters do HDInsight ou ao chamar arquivos JAR nesses clusters, entre em contato com o [Suporte da Microsoft](https://azure.microsoft.com/support/options/).
>
> A cascata não tem suporte do HDInsight e não está qualificada para o Suporte da Microsoft. Para obter as listas dos componentes suportados, confira [Novidades nas versões de cluster fornecidas pelo HDInsight](../../hdinsight/hdinsight-component-versioning.md).
>
>

Às vezes, você deseja definir os seguintes arquivos de configuração durante o processo de criação:

* clusterIdentity.xml
* core-site.xml
* gateway.xml
* hbase-env.xml
* hbase-site.xml
* hdfs-site.xml
* hive-env.xml
* hive-site.xml
* mapred-site
* oozie-site.xml
* oozie-env.xml
* storm-site.xml
* tez-site.xml
* webhcat-site.xml
* yarn-site.xml

Para saber mais, confira [Personalizar clusters HDInsight usando a Inicialização](../../hdinsight/hdinsight-hadoop-customize-cluster-bootstrap.md).

## <a name="advanced-settings-extend-clusters-with-a-virtual-network"></a>Configurações avançadas: estender clusters com uma rede virtual
Se sua solução exige tecnologias que sejam distribuídas entre vários tipos de cluster HDInsight, uma [rede virtual do Azure](../../hdinsight/https://docs.microsoft.com/azure/virtual-network) pode conectar os tipos de cluster necessários. Essa configuração permite que os clusters e qualquer código que você implantar neles se comuniquem diretamente uns com os outros.

Para obter mais informações sobre como usar uma rede virtual do Azure com HDInsight, confira [Estender o HDInsight com redes virtuais do Azure](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md).

Para obter um exemplo de como usar dois tipos de cluster em uma rede virtual do Azure, confira [Usar Streaming Estruturado do Spark com o Kafka](../../hdinsight/hdinsight-apache-kafka-spark-structured-streaming.md). Para saber mais sobre como usar o HDInsight com uma rede virtual, incluindo requisitos de configuração específicos para a rede virtual, confira [Estender as funcionalidades do HDInsight usando uma Rede Virtual do Azure](../../hdinsight/hdinsight-extend-hadoop-virtual-network.md).

## <a name="troubleshoot-access-control-issues"></a>Solucionar problemas de controle de acesso

Se você tiver problemas com a criação de clusters HDInsight, confira os [requisitos de controle de acesso](../../hdinsight/hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Próximas etapas

- [Driver ABFS Hadoop Filesystem para o Azure Data Lake Storage Gen2](abfs-driver.md)
- [Tutorial: extrair, transformar e carregar dados usando o Apache Hive no Azure HDInsight](tutorial-extract-transform-load-hive.md)
- [O que são o HDInsight, o ecossistema do Hadoop e os clusters Hadoop?](../../hdinsight/hadoop/apache-hadoop-introduction.md)
- [Introdução ao uso do Hadoop no HDInsight](../../hdinsight/hadoop/apache-hadoop-linux-tutorial-get-started.md)
- [Trabalhar em Hadoop no HDInsight em um computador com Windows](../../hdinsight/hdinsight-hadoop-windows-tools.md)
