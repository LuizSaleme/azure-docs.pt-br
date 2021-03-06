---
title: Início rápido Python da API de Detecção de Emoções | Microsoft Docs
description: Obtenha informações e exemplos de códigos para ajudar você a começar a usar a API de Detecção de Emoções com Python em Serviços Cognitivos.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/05/2018
ms.author: anroth
ms.openlocfilehash: ff1f6b2ddc872d0ee63d9885b04b1f007bc86e33
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2018
ms.locfileid: "35363434"
---
# <a name="emotion-api-python-quickstart"></a>Início Rápido Python da API de Detecção de Emoções

> [!IMPORTANT]
> A Versão Prévia da API de Vídeo terminou no dia 30 de outubro de 2017. Experimente a nova [versão prévia da API do Video Indexer](https://azure.microsoft.com/services/cognitive-services/video-indexer/) para extrair facilmente as informações dos vídeos e aprimorar as experiências de descoberta de conteúdo, tais como resultados da pesquisa através da detecção de palavras faladas, faces, personagens e emoções. [Saiba mais](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Este passo a passo fornece informações e exemplos de código para ajudar você a começar a usar o [método de reconhecimento da API de Detecção de Emoções](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) com Python para reconhecer as emoções expressadas por uma ou mais pessoas em uma imagem. 

Execute este exemplo como um bloco de anotações do Jupyter em [MyBinder](https://mybinder.org) clicando na notificação de inicialização do Associador: [![Associador](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=EmotionAPI.ipynb)


## <a name="prerequisite"></a>Pré-requisito
Obtenha sua chave de assinatura gratuita [aqui](https://azure.microsoft.com/try/cognitive-services/)

## <a name="running-the-walkthrough"></a>Executando o passo a passo
Para continuar com este passo a passo, substitua `subscription_key` com a chave de API que você obteve anteriormente.


```python
subscription_key = None
assert subscription_key
```

Em seguida, verifique se a URL do serviço corresponde com a região que você usou ao configurar a chave de API. Se você estiver usando uma chave de avaliação, você não precisa fazer alterações.


```python
emotion_recognition_url = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
```

Este passo a passo usa imagens que são armazenadas no disco. Você também pode usar imagens que estão disponíveis por meio de uma URL publicamente acessível. Para obter mais informações, consulte a [Documentação da API REST](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa).

Como os dados da imagem são passados como parte do corpo da solicitação, observe que você precisa definir o cabeçalho `Content-Type` para `application/octet-stream`. Se você estiver passando uma imagem através de uma URL, lembre-se de definir o cabeçalho:
```python
header = {'Ocp-Apim-Subscription-Key': subscription_key }
```
crie um dicionário que contém a URL:
```python
data = {'url': image_url}
```
e passe-o para a biblioteca `requests` usando:
```python
requests.post(emotion_recognition_url, headers=headers, json=image_data)
```

Primeiro, baixe algumas imagens de amostra do site da [API de Detecção de Emoções](https://azure.microsoft.com/services/cognitive-services/emotion/).


```bash
%%bash
mkdir -p images
curl -Ls https://aka.ms/csnb-emotion-1 -o images/emotion_1.jpg
curl -Ls https://aka.ms/csnb-emotion-2 -o images/emotion_2.jpg
```


```python
image_path = "images/emotion_1.jpg"
image_data = open(image_path, "rb").read()
```


```python
import requests
headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
response.raise_for_status()
analysis = response.json()
analysis
```




    [{'faceRectangle': {'height': 162, 'left': 130, 'top': 141, 'width': 162},
      'scores': {'anger': 9.29041e-06,
       'contempt': 0.000118981574,
       'disgust': 3.15619363e-05,
       'fear': 0.000589638,
       'happiness': 0.06630674,
       'neutral': 0.00555004273,
       'sadness': 7.44669524e-06,
       'surprise': 0.9273863}}]



O objeto JSON retornado contém as caixas delimitadoras das faces que foram reconhecidas junto com as emoções detectadas. Cada emoção é associada a uma pontuação entre 0 e 1, onde uma pontuação mais alta é mais indicativa de emoção que uma pontuação inferior. 

As seguintes linhas de código detectaram as emoções em faces na imagem usando a biblioteca `matplotlib`. Para reduzir o ruído, somente as três emoções principais são mostradas.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
from io import BytesIO
from PIL import Image

plt.figure(figsize=(5,5))

image  = Image.open(BytesIO(image_data))
ax     = plt.imshow(image, alpha=0.6)

for face in analysis:
    fr = face["faceRectangle"]
    em = face["scores"]
    origin = (fr["left"], fr["top"])
    p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
    ax.axes.add_patch(p)
    ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
    plt.text(origin[0], origin[1], ct, fontsize=20)    
_ = plt.axis("off")
```

A função `annotate_image` mostrada a seguir pode ser usada para sobrepor emoções na parte superior de um arquivo de imagem, dado seu caminho no sistema de arquivos. Ele se baseia no código para chamar a API de Detecção de Emoções mostrada anteriormente.


```python
def annotate_image(image_path):    
    image_data = open(image_path, "rb").read()
    headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
    response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
    response.raise_for_status()
    analysis = response.json()

    plt.figure()

    image  = Image.open(image_path)
    ax     = plt.imshow(image, alpha=0.6)

    for face in analysis:
        fr = face["faceRectangle"]
        em = face["scores"]
        origin = (fr["left"], fr["top"])
        p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
        ax.axes.add_patch(p)
        ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
        plt.text(origin[0], origin[1], ct, fontsize=20, va="bottom")    
    _ = plt.axis("off")
```

Por fim, a função `annotate_image` pode ser chamada em um arquivo de imagem, conforme mostrado na linha a seguir:


```python
annotate_image("images/emotion_2.jpg")
```
