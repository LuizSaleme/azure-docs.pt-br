---
title: Def. dos Serviços de Nuvem do Azure Esquema NetworkTrafficRules | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 351b369f-365e-46c1-82ce-03fc3655cc88
caps.latest.revision: 17
author: jpconnock
ms.author: jeconnoc
manager: timlt
ms.openlocfilehash: 71c791c9ac6f679f0f67b014c8fb5dd915d1a3e3
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39004397"
---
# <a name="azure-cloud-services-definition-networktrafficrules-schema"></a>Esquema NetworkTrafficRules de definição dos Serviços de Nuvem do Azure
O nó `NetworkTrafficRules` é um elemento opcional no arquivo de definição de serviço que especifica como as funções comunicam-se entre si. Ele limita quais funções podem acessar os pontos de extremidade internos da função específica. O `NetworkTrafficRules` não é um elemento autônomo; ele é combinado com duas ou mais funções em um arquivo de definição de serviço.

A extensão padrão do arquivo de definição de serviço é .csdef.

> [!NOTE]
>  O nó `NetworkTrafficRules` só está disponível usando o SDK do Azure versão 1.3 ou superior.

## <a name="basic-service-definition-schema-for-the-network-traffic-rules"></a>Esquema de definição de serviço básico para as regras de tráfego de rede
O formato básico de um arquivo de definição de serviço que contém definições de tráfego de rede é o seguinte.

```xml
<ServiceDefinition …>
   <NetworkTrafficRules>
      <OnlyAllowTrafficTo>
         <Destinations>
            <RoleEndpoint endpointName="<name-of-the-endpoint>" roleName="<name-of-the-role-containing-the-endpoint>"/>
         </Destinations>
         <AllowAllTraffic/>
         <WhenSource matches="[AnyRule]">
            <FromRole roleName="<name-of-the-role-to-allow-traffic-from>"/>
         </WhenSource>
      </OnlyAllowTrafficTo>
   </NetworkTrafficRules>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Elementos de esquema
O nó `NetworkTrafficRules` do arquivo de definição de serviço inclui estes elementos, descritos em detalhes nas seções subsequentes neste tópico:

[Elemento NetworkTrafficRules](#NetworkTrafficRules)

[Elemento OnlyAllowTrafficTo](#OnlyAllowTrafficTo)

[Elemento Destinations](#Destinations)

[Elemento RoleEndpoint](#RoleEndpoint)

[Elemento AllowAllTraffic](#AllowAllTraffic)

[Elemento WhenSource](#WhenSource)

[Elemento FromRole](#FromRole)

##  <a name="NetworkTrafficRules"></a> NetworkTrafficRules Element
O elemento `NetworkTrafficRules` especifica quais funções podem se comunicar com qual ponto de extremidade em outra função. Um serviço pode conter uma definição `NetworkTrafficRules`.

##  <a name="OnlyAllowTrafficTo"></a> Elemento OnlyAllowTrafficTo
O elemento `OnlyAllowTrafficTo` descreve uma coleção de pontos de extremidade de destino e as funções que podem se comunicar com eles. É possível especificar vários nós `OnlyAllowTrafficTo`.

##  <a name="Destinations"></a> Elemento Destinations
O elemento `Destinations` descreve uma coleção de RoleEndpoints com a qual é possível se comunicar.

##  <a name="RoleEndpoint"></a> Elemento RoleEndpoint
O elemento `RoleEndpoint` descreve um ponto de extremidade em uma função com a qual ele permite comunicações. Será possível especificar vários elementos `RoleEndpoint` se houver mais de um ponto de extremidade na função.

| Atributo      | Tipo     | DESCRIÇÃO |
| -------------- | -------- | ----------- |
| `endpointName` | `string` | Obrigatório. O nome do ponto de extremidade com o qual permitir o tráfego.|
| `roleName`     | `string` | Obrigatório. O nome da função web com a qual permitir a comunicação.|

## <a name="allowalltraffic-element"></a>Elemento AllowAllTraffic
O elemento `AllowAllTraffic` é uma regra que permite que todas as funções se comuniquem com os pontos de extremidade definidos no nó `Destinations`.

##  <a name="WhenSource"></a> Elemento WhenSource
O elemento `WhenSource` descreve uma coleção de funções que podem se comunicar com os pontos de extremidade definidos no nó `Destinations`.

| Atributo | Tipo     | DESCRIÇÃO |
| --------- | -------- | ----------- |
| `matches` | `string` | Obrigatório. Especifica a regra a ser aplicada ao permitir comunicações. No momento, o único valor válido é `AnyRule`.|
  
##  <a name="FromRole"></a> Elemento FromRole
O elemento `FromRole` especifica as funções que podem se comunicar com os pontos de extremidade definidos no nó `Destinations`. Será possível especificar vários elementos `FromRole` se houver mais de uma função que pode se comunicar com os pontos de extremidade.

| Atributo  | Tipo     | DESCRIÇÃO |
| ---------- | -------- | ----------- |
| `roleName` | `string` | Obrigatório. O nome da função da qual permitir a comunicação.|

## <a name="see-also"></a>Veja também
[Esquema de definição do Serviço de Nuvem (clássico)](schema-csdef-file.md)