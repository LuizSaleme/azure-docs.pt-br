---
title: Exemplos de CLI do Azure - Serviço Azure SignalR | Microsoft Docs
description: Exemplos de CLI do Azure - Serviço Azure SignalR
services: signalr
documentationcenter: signalr
author: wesmc7777
manager: cfowler
editor: ''
tags: azure-service-management
ms.service: signalr
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: signalr
ms.date: 04/20/2018
ms.author: wesmc
ms.custom: mvc
ms.openlocfilehash: 6e76ee89160c84ff0f1033590d14c288f023f201
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33779847"
---
# <a name="azure-cli-samples"></a>Exemplos de CLI do Azure

A tabela a seguir inclui links para scripts bash para o Serviço do Azure SignalR do Microsoft Azure usando a CLI do Azure.

| | |
|-|-|
|**Criar**||
| [Criar um novo Serviço SignalR e grupo de recurso](scripts/signalr-cli-create-service.md) | Cria um novo recurso do Serviço Azure SignalR em um novo grupo de recursos com um nome aleatório.  |
|**Integração**||
| [Criar um novo SignalR Service e o aplicativo Web configurado para usar o SignalR](scripts/signalr-cli-create-with-app-service.md) | Cria um novo recurso do Serviço Azure SignalR em um novo grupo de recursos com um nome aleatório. Também adiciona aplicativo Web e um Plano do Serviço de Aplicativo para hospedar um aplicativo Web ASP.NET que usa o Serviço SignalR. O aplicativo web é configurado com uma Configuração de Aplicativo para se conectar ao novo recurso de serviço SignalR. |
| [Criar um novo SignalR Service e aplicativo Web configurado para usar o SignalR e o GitHub OAuth](scripts/signalr-cli-create-with-app-service-github-oauth.md) | Cria um novo recurso do Serviço Azure SignalR em um novo grupo de recursos com um nome aleatório. Também adiciona aplicativo Web e um Plano do Serviço de Aplicativo para hospedar um aplicativo Web ASP.NET que usa o Serviço SignalR. O aplicativo web está configurado com configurações de aplicativo para a cadeia de caracteres de conexão para o novo recurso de serviço SignalR e os segredos de cliente para oferecer suporte a [autenticação GitHub](https://developer.github.com/v3/guides/basics-of-authentication/) conforme demonstrado no [tutorial de autenticação](signalr-authenticate-oauth.md). O aplicativo Web também é configurado para usar uma fonte de implantação do repositório git local. |
| | |
