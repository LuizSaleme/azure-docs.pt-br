---
title: Arquivo de inclusão
description: Arquivo de inclusão
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 70a9e2a8f6327c0af92698b49d5b622bebba3661
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313601"
---
1. Inicie sessão na sua assinatura do Azure com o comando [az login](/cli/azure/#login) e siga as instruções na tela. Para saber mais sobre registro em log, veja a [Introdução à CLI do Azure 2.0](/cli/azure/get-started-with-azure-cli).

  ```azurecli
  az login
  ```
2. Se tiver mais de uma assinatura do Azure, liste as assinaturas para a conta.

  ```azurecli
  az account list --all
  ```
3. Especifique a assinatura que você deseja usar.

  ```azurecli
  az account set --subscription <replace_with_your_subscription_id>
  ```