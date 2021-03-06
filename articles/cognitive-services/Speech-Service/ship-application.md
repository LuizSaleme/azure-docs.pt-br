---
title: Documentação da API do SDK de Fala dos Serviços Cognitivos, Serviços Cognitivos do Azure - Tutoriais, Referência de API
description: Saiba como criar e desenvolver aplicativos com o SDK de Serviços Cognitivos de Fala
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: df6224208e93f4a332247440933b3a4d11c1bb43
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283024"
---
# <a name="shipping-an-application"></a>Enviar um aplicativo

Observe a [Licença do SDK de fala](license.md), bem como as [notificações de software de terceiros](third-party-notices.md) ao distribuir o SDK de Serviços Cognitivos de Fala. Além disso, leia a [Política de privacidade da Microsoft](https://aka.ms/csspeech/privacy).

Dependendo da plataforma, existem dependências diferentes para executar seu aplicativo.

## <a name="windows"></a>Windows

O SDK dos Serviços Cognitivos de Fala é testado no Windows 10 e no Windows Server 2016.

O SDK dos Serviços Cognitivos de Fala exige o [Pacotes Redistribuíveis do Visual C++ para Visual Studio 2017 da Microsoft](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) no sistema. Você pode baixar instaladores para a versão mais recente do `Microsoft Visual C++ Redistributable for Visual Studio 2017`:

- [Win32](https://aka.ms/vs/15/release/vc_redist.x86.exe)
- [x64](https://aka.ms/vs/15/release/vc_redist.x64.exe)

Se seu aplicativo estiver usando código gerenciado, o `.Net Framework 4.6.1` ou posterior será necessário no computador de destino.

Para a entrada do microfone, as bibliotecas do Media Foundation precisam ser instaladas. Essas bibliotecas fazem parte do Windows 10 e do Windows Server 2016. É possível usar o SDK de Fala sem essas bibliotecas, contanto que o microfone não seja usado como o dispositivo de entrada de áudio.

Os arquivos necessários do SDK de Fala podem ser implantados no mesmo diretório do seu aplicativo. Dessa forma, seu aplicativo pode acessar diretamente as bibliotecas. Selecione a versão correta (Win32/x64) correspondente ao seu aplicativo.

| NOME | Função
|:-----|:----|
| `Microsoft.CognitiveServices.Speech.core.dll` | SDK principal, necessário para implantação nativa e gerenciada
| `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` | necessário para implantação gerenciada
| `Microsoft.CognitiveServices.Speech.csharp.dll` | necessário para implantação gerenciada

## <a name="linux"></a>Linux

Para um aplicativo nativo, você precisa enviar a biblioteca do SDK de Fala, `libMicrosoft.CognitiveServices.Speech.core.so`.
Selecione a versão (x86/x64) correspondente ao seu aplicativo. Dependendo da versão do Linux, você precisará também incluir as seguintes dependências:

* As bibliotecas compartilhadas da biblioteca GNU C (incluindo a biblioteca de programação de Threads POSIX, `libpthreads`)
* A biblioteca OpenSSL (`libssl.so.1.0.0`)
* A biblioteca cURL (`libcurl.so.4`)
* A biblioteca compartilhada para aplicativos ALSA (`libasound.so.2`)

No Ubuntu 16.04, por exemplo, as bibliotecas de GNU C já devem estar instaladas por padrão. Os três últimos podem ser instalados usando esses comandos:

```sh
sudo apt-get update
sudo apt-get install libssl1.0.0 libcurl3 libasound2 wget
```

## <a name="next-steps"></a>Próximas etapas

* [Obtenha sua assinatura de avaliação de Fala](https://azure.microsoft.com/try/cognitive-services/)
* [Veja como reconhecer fala em C#](quickstart-csharp-dotnet-windows.md)
