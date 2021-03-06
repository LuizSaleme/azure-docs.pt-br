---
title: Acessando Máquinas Virtuais do Azure do Gerenciador de Servidores | Microsoft Docs
description: Obtenha uma visão geral de como exibir e gerenciar máquinas virtuais (VMs) do Azure no Gerenciador de Servidores no Visual Studio.
services: visual-studio-online
author: ghogen
manager: douge
assetId: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 8/31/2017
ms.author: ghogen
ms.openlocfilehash: a19f33c4dd2654538c5718d2cd7dbe5d018e4de1
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31792878"
---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Acessando máquinas virtuais do Azure por meio do Gerenciador de Servidores

Se tiver máquinas virtuais hospedadas pelo Azure, você pode acessá-las no Gerenciador de Servidores. Você primeiro deve entrar sua assinatura do Azure para exibir os serviços móveis. Por exemplo, no Gerenciador de Servidores, você pode abrir o menu de atalho do nó do Azure e escolher **Conectar ao Microsoft Azure**.

1. No Cloud Explorer, escolha uma máquina virtual e pressione a tecla F4 para mostrar a respectiva janela de propriedades.

    A tabela a seguir mostra quais propriedades estão disponíveis, mas elas são somente leitura. Para alterá-las, use o [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

   | Propriedade | DESCRIÇÃO |
   | --- | --- |
   | Nome DNS |A URL com o endereço de Internet da máquina virtual. |
   | Ambiente |Para uma máquina virtual, o valor dessa propriedade é sempre produção. |
   | NOME |O nome da máquina virtual. |
   | Tamanho |O tamanho da máquina virtual, que reflete a quantidade de memória e espaço em disco disponível. Para obter mais informações, consulte os [tamanhos de máquina virtual](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs). |
   | Status |Os valores incluem Inicial, Iniciado, Parando, Parado e Recuperando Status. Se Recuperando Status aparecer, o status atual é desconhecido. Os valores para essa propriedade diferem dos valores que são usados no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). |
   | SubscriptionID |A ID de assinatura para sua conta do Azure. Você pode mostrar essas informações no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040) exibindo as propriedades de uma assinatura. |
2. Escolha um nó do ponto de extremidade e exiba a janela **Propriedades** .
3. A tabela a seguir descreve as propriedades de pontos de extremidade disponíveis, mas eles são somente leitura. Para adicionar ou editar os pontos de extremidade para uma máquina virtual, use o [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040). 

   | Propriedade | DESCRIÇÃO |
   | --- | --- |
   | NOME |Um identificador para o ponto de extremidade. |
   | Porta privada |A porta para acesso à rede interna para o seu aplicativo. |
   | Protocolo |O protocolo de camada de transporte que este ponto de extremidade usa: TCP ou UDP. |
   | Porta pública |A porta usada para acesso público ao seu aplicativo. |
