---
title: Script da CLI do Azure – Criar um Banco de Dados do Azure para MySQL
description: Este exemplo de script da CLI cria um Banco de Dados do Azure para servidor MySQL e configura uma regra de firewall no nível do servidor.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.devlang: azure-cli
ms.custom: mvc
ms.topic: sample
ms.date: 02/28/2018
ms.openlocfilehash: acdc2b40530be190212b8b8f35443ce8d4ee4bde
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432507"
---
# <a name="create-a-mysql-server-and-configure-a-firewall-rule-using-the-azure-cli"></a>Criar um servidor MySQL e configurar uma regra de firewall usando a CLI do Azure
Este exemplo de script da CLI cria um Banco de Dados do Azure para servidor MySQL e configura uma regra de firewall no nível do servidor. Após o script ser executado com êxito, o servidor MySQL será acessível por todos os serviços do Azure e pelo endereço IP configurado.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Se você optar por executar a CLI localmente, este artigo exigirá a CLI do Azure versão 2.0 ou posterior. Verifique a versão executando `az --version`. Confira [Instalar a CLI do Azure 2.0]( /cli/azure/install-azure-cli) para instalar ou atualizar sua versão da CLI do Azure. 

## <a name="sample-script"></a>Script de exemplo
Neste script de exemplo, edite as linhas destacadas para atualizar o nome de usuário administrador e a senha com os seus próprios.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/create-mysql-server-and-firewall-rule.sh?highlight=18-19 "Create an Azure Database for MySQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Limpar a implantação
Use o comando a seguir para remover o grupo de recursos e todos os recursos associados a ele após executar o exemplo de script. 
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/create-mysql-server-and-firewall-rule/delete-mysql.sh "Delete the resource group.")]

## <a name="script-explanation"></a>Explicação sobre o script
Esse script usa os comandos descritos na tabela abaixo:

| **Comando** | **Observações** |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Cria um grupo de recursos no qual todos os recursos são armazenados. |
| [az mysql server create](/cli/azure/mysql/server#az-msql-server-create) | Cria um servidor MySQL que hospeda os bancos de dados. |
| [az mysql server firewall create](/cli/azure/mysql/server/firewall-rule#az-mysql-server-firewall-rule-create) | Cria uma regra de firewall para permitir o acesso ao servidor e aos bancos de dados contidos nele no intervalo de endereços IP inserido. |
| [az group delete](/cli/azure/group#az-group-delete) | Exclui um grupo de recursos, incluindo todos os recursos aninhados. |

## <a name="next-steps"></a>Próximas etapas
- Para ler mais sobre a CLI do Azure: [documentação da CLI do Azure](/cli/azure).
- Experimente scripts adicionais: [exemplos da CLI do Azure para o Banco de Dados do Azure para MySQL](../sample-scripts-azure-cli.md)
