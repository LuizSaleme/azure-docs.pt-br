---
title: Dimensionar ações de nó de unidade na pilha do Azure | Microsoft Docs
description: Saiba como exibir o status de nó e use o poder no desligamento, Esvaziar e retomar as ações de nó em um sistema de pilha do Azure integradas.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 3ecc8885a30a11472fe93bbda60c39131c6b3bd7
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801408"
---
# <a name="scale-unit-node-actions-in-azure-stack"></a>Ações de nó de unidade de escala na pilha do Azure

*Aplica-se a: sistemas integrados de pilha do Azure*

Este artigo descreve como exibir o status de uma unidade de escala e seus nós associados e como usar as ações de nó disponível. Ações de nó incluem ativação, desativar, drenar, retomam e reparar. Normalmente, você deve usar essas ações de nó durante a substituição de campo de partes de ou para cenários de recuperação de nó.

> [!Important]  
> Todas as ações de nó descritas neste artigo devem apenas destino um nó por vez.


## <a name="view-the-status-of-a-scale-unit-and-its-nodes"></a>Exibir o status de uma unidade de escala e de seus nós

No portal do administrador, você pode facilmente exibir o status de uma unidade de escala e seus nós associado.

Para exibir o status de uma unidade de escala:

1. Sobre o **gerenciamento região** lado a lado, selecione a região.
2. À esquerda, em **recursos de infraestrutura**, selecione **unidade de escala**.
3. Nos resultados, selecione a unidade de escala.
 
Aqui, você pode exibir as seguintes informações:

- nome da região. O nome da região é referenciado com **-local** no módulo do PowerShell.
- tipo de sistema
- núcleos lógicos total
- total de memória
- a lista de nós individuais e seus status; o **executando** ou **interrompido**.

![Bloco de unidade de escala, mostrando o status de execução para cada nó](media/azure-stack-node-actions/ScaleUnitStatus.PNG)

## <a name="view-information-about-a-scale-unit-node"></a>Exibir informações sobre um nó de unidade de escala

Se você selecionar um nó individual, você pode exibir as seguintes informações:

- nome da região
- modelo do servidor
- Endereço IP do controlador de gerenciamento da placa-base (BMC)
- Estado operacional
- número total de núcleos
- quantidade total de memória
 
![Bloco de unidade de escala, mostrando o status de execução para cada nó](media/azure-stack-node-actions/NodeActions.PNG)

Você também pode executar ações de nó da unidade de escala aqui.

## <a name="scale-unit-node-actions"></a>Ações de nó da unidade de escala

Quando você exibe informações sobre um nó de unidade de escala, você também pode executar ações de nó, como:

- ligar e desligar
- Esvaziar e continuar
- reparar

O estado operacional do nó determina quais opções estão disponíveis.

### <a name="power-off"></a>Desligar

O **desligue** ação desativa o nó. É o mesmo como se você pressionar o botão de energia. Ele faz **não** envia um sinal de desligamento para o sistema operacional. Para operações de desligamento planejado, verifique se que você descarregar um nó de unidade de escala pela primeira vez.

Esta ação normalmente é usada quando um nó está em um estado suspenso e não responde às solicitações.

> [!Important] 
> Essa funcionalidade só está disponível por meio do PowerShell. Ele estará disponível no portal do administrador do Azure pilha novamente mais tarde.


Para executar o desligamento de ação por meio do PowerShell:

````PowerShell
  Stop-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
```` 

No caso improvável de que a ação de desligamento não funciona, use a interface da web do BMC.

### <a name="power-on"></a>Ligue

O **ligar** ação ativa o nó. É o mesmo como se você pressionar o botão de energia. 

> [!Important] 
> Essa funcionalidade só está disponível por meio do PowerShell. Ele estará disponível no portal do administrador do Azure pilha novamente mais tarde.

Para executar a potência em ação por meio do PowerShell:

````PowerShell
  Start-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
````

No caso improvável de que a ação de ligar não funciona, use a interface da web do BMC.

### <a name="drain"></a>Esvaziar

O **drenar** ação leva todas as cargas de trabalho ativas por distribuí-los entre os nós restantes dessa unidade de escala específica.

Esta ação normalmente é usada durante a substituição de campo de partes, como a substituição de um nó inteiro.

> [!IMPORTANT]  
> Certifique-se de que você descarregar um nó somente durante uma janela de manutenção planejada, onde os usuários foram notificados. Sob algumas condições, cargas de trabalho ativas podem sofrer interrupções.

Para executar a ação esvaziar por meio do PowerShell:

  ````PowerShell
  Disable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="resume"></a>Continuar

O **retomar** ação retoma um nó esvaziado e marca-ativa para o posicionamento da carga de trabalho. As cargas de trabalho anteriores que estavam em execução no nó de failback. (Se você descarregar um nó e, em seguida, desligue, quando você religue o nó, ele não está marcado como ativo para o posicionamento da carga de trabalho. Quando estiver pronto, você deve usar a ação de retomada para marcar como ativo no nó.)

Para executar a ação de continuação por meio do PowerShell:

  ````PowerShell
  Enable-AzsScaleUnitNode -Location <RegionName> -Name <NodeName>
  ````

### <a name="repair"></a>Reparar

O **reparo** ação repara um nó. Usá-lo somente para qualquer um dos seguintes cenários:

- Substituição completa de nó (com ou sem novos discos de dados)
- Após a falha de componente de hardware e de substituição (se aconselhado a documentação do campo FRU (unidade renovável)).

> [!IMPORTANT]  
> Consulte a documentação de FRU do fornecedor de hardware seu OEM para obter as etapas exatas quando você precisa substituir um nó ou componentes de hardware individual. A documentação de FRU especificará se você precisa executar a ação de reparo depois de substituir um componente de hardware.  

Quando você executa a ação de reparo, você precisa especificar o endereço IP do BMC. 

Para executar a ação de reparo por meio do PowerShell:

  ````PowerShell
  Repair-AzsScaleUnitNode -Location <RegionName> -Name <NodeName> -BMCIPAddress <BMCIPAddress>
  ````

## <a name="next-steps"></a>Próximas etapas

Para saber mais sobre o módulo de administrador de malha de pilha do Azure, consulte [Azs.Fabric.Admin](https://docs.microsoft.com/powershell/module/azs.fabric.admin/?view=azurestackps-1.3.0).