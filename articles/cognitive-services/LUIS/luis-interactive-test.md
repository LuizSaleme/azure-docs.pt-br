---
title: Treinar e testar seu aplicativo LUIS – Azure | Microsoft Docs
description: Use o LUIS (Serviço Inteligente de Reconhecimento Vocal) para trabalhar continuamente em seu aplicativo para refiná-lo e melhorar seu reconhecimento vocal.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: diberry
ms.openlocfilehash: 925b96b61de876d40105c89556fd6dcb600d1b1a
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2018
ms.locfileid: "39223643"
---
# <a name="test-your-luis-app"></a>Testar seu aplicativo LUIS
<a name="train-your-app"></a>
[Treinar](luis-how-to-train.md) e [testar](luis-concept-test.md) um aplicativo é um processo iterativo. Após treinar seu aplicativo LUIS, teste-o com declarações de exemplo para ver se as intenções e as entidades são reconhecidas corretamente. Caso contrário, faça atualizações no aplicativo LUIS, treine e teste novamente. 

<!-- anchors for H2 name changes -->
<a name="test-your-app"></a>
<a name="access-the-test-page"></a>
<a name="luis-interactive-testing"></a>
## <a name="test-an-utterance"></a>Testar uma declaração

1. Acesse seu aplicativo selecionando seu nome na página **Meus aplicativos**. 

2. Para acessar o painel extensível **Testar**, selecione **Testar** no painel superior do seu aplicativo.

    ![Página Treinar e testar aplicativo](./media/luis-how-to-interactive-test/test.png)

3. Insira uma declaração na caixa de texto e selecione Enter. É possível digitar quantas declarações de teste você desejar no **Teste**, mas apenas uma declaração por vez.

4. A declaração, sua principal intenção e a pontuação são adicionadas à lista de declarações embaixo da caixa de texto.

    ![O teste interativo identifica a intenção incorreta](./media/luis-how-to-interactive-test/test-weather-1.png)

## <a name="clear-test-panel"></a>Limpar painel de teste
Para limpar todas as declarações de teste inseridas e seus resultados do console de teste, selecione **Começar de novo** no canto superior esquerdo do **Painel de teste**. 

## <a name="close-test-panel"></a>Fechar painel de teste
Para fechar o painel de **Teste**, selecione o botão **Testar** novamente.

## <a name="inspect-score"></a>Inspecionar pontuação
Inspecione detalhes do resultado do teste no painel **Inspecionar**. 
 
1. Com o painel extensível **Testar** aberto, selecione **Inspecionar** para uma declaração que você deseja comparar. 

    ![Botão Inspecionar](./media/luis-how-to-interactive-test/inspect.png)

2. O painel **Inspeção** é exibido. O painel inclui a principal intenção de pontuação e as entidades identificadas. O painel mostra o resultado da declaração selecionada.

    ![Botão Inspecionar](./media/luis-how-to-interactive-test/inspect-panel.png)

## <a name="correct-top-scoring-intent"></a>Corrigir principal intenção de pontuação

1. Se a principal intenção de pontuação estiver incorreta, selecione o botão **Editar**.

2.  Na lista suspensa, selecione a intenção correta para a declaração.

    ![Selecionar intenção correta](./media/luis-how-to-interactive-test/intent-select.png)

## <a name="view-sentiment-results"></a>Exibir resultados de sentimento

Se a **Análise de sentimento** estiver configurada na página **[Publicar](luis-how-to-publish-app.md#enable-sentiment-analysis)**, os resultados de teste incluirão o sentimento encontrado na declaração. 

![Imagem do painel de Teste com análise de sentimento](./media/luis-how-to-interactive-test/sentiment.png)

## <a name="correct-matched-patterns-intent"></a>Corrigir intenção do padrão correspondente
Se estiver usando [Padrões](luis-concept-patterns.md) e a declaração tiver correspondido a um padrão, mas a intenção incorreta estava prevista, selecione o link **Editar** pelo padrão e selecione a intenção correta.

## <a name="compare-with-published-version"></a>Comparar com a versão publicada
É possível testar a versão ativa do seu aplicativo com a versão do [ponto de extremidade](luis-glossary.md#endpoint) publicada. No painel **Inspecionar**, selecione **Comparar com publicado**. Qualquer teste com relação ao modelo publicado é deduzido do saldo da sua cota de assinatura do Azure. 

![Comparar com o publicado](./media/luis-how-to-interactive-test/inspect-panel-compare.png)

## <a name="view-endpoint-json-in-test-panel"></a>Exibir JSON de ponto de extremidade no painel de teste
É possível exibir o JSON do ponto de extremidade retornado para a comparação selecionando **Mostrar exibição JSON**.

![Resposta JSON publicada](./media/luis-how-to-interactive-test/inspect-panel-compare-json.png)

<!--Service name is 'Bing Spell Check v7 API' in the portal-->
## <a name="additional-settings-in-test-panel"></a>Configurações adicionais no painel de teste

### <a name="luis-endpoint"></a>Ponto de extremidade LUIS
Se tiver vários pontos de extremidade LUIS, use o link **Configurações Adicionais** no painel Publicado do Teste para alterar o ponto de extremidade usado para teste. Se não tiver certeza sobre qual ponto de extremidade usar, selecione o **Starter_Key** padrão. 

![Testar painel com o link Configurações Adicionais realçado](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)


### <a name="view-bing-spell-check-corrections-in-test-panel"></a>Exibir correções da Verificação Ortográfica do Bing no painel de teste
Requisitos para exibir as correções de ortografia: 

* Aplicativo publicado
* [Chave de serviço](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) da Verificação Ortográfica do Bing. A chave de serviço não é armazenada e precisa ser redefinida para cada sessão do navegador. 

Use o seguinte procedimento para incluir o serviço [Verificação Ortográfica do Bing v7](https://azure.microsoft.com/services/cognitive-services/spell-check/) nos resultados do painel de teste. 

1. No painel **Teste**, insira uma declaração. Quando as declarações são previstas, selecione **[Inspecionar](#inspect-score)** embaixo da declaração inserida. 

2. Quando o painel **Inspecionar** abrir, selecione **[Comparar com publicado](#compare-with-published-version)**. 

3. Quando o painel **Publicado** abrir, selecione **[Configurações Adicionais](#additional-settings-in-test-panel)**.

4. Na caixa de diálogo pop-up, insira sua chave de serviço da **Verificação Ortográfica do Bing**. 
    ![Inserir chave de serviço da Verificação Ortográfica do Bing](./media/luis-how-to-interactive-test/interactive-with-spell-check-service-key.png)

5. Insira uma consulta com uma ortografia incorreta como `book flite to seattle` e selecione enter. A ortografia incorreta da palavra `flite` é substituída na consulta enviada para o LUIS e o JSON resultante mostra a consulta original, como `query`, e a ortografia correta na consulta, como `alteredQuery`.

    ![JSON de ortografia corrigido](./media/luis-how-to-interactive-test/interactive-with-spell-check-results.png)

<a name="json-file-with-no-duplicates"></a>
<a name="import-a-dataset-file-for-batch-testing"></a>
<a name="export-rename-delete-or-download-dataset"></a>
<a name="run-a-batch-test-on-your-trained-app"></a>
<a name="access-batch-test-result-details-in-a-visualized-view"></a>
<a name="filter-chart-results-by-intent-or-entity"></a>
<a name="investigate-false-sections"></a>
<a name="view single-point utterance data"></a>
<a name="relabel-utterances-and-retrain"></a>
<a name="false-test-results"></a>
## <a name="batch-testing"></a>Teste de lote
Confira os [conceitos](luis-concept-batch-test.md) de teste do lote e saiba [como](luis-how-to-batch-test.md) testar um lote de declarações.

## <a name="next-steps"></a>Próximas etapas

Se o teste indica que seu aplicativo LUIS não reconhece as intenções e entidades corretas, é possível trabalhar para melhorar a precisão do seu aplicativo LUIS rotulando mais declarações ou adicionando recursos. 

* [Declarações sugeridas de rótulo com o LUIS](luis-how-to-review-endoint-utt.md) 
* [Usar recursos para melhorar o desempenho do aplicativo de LUIS](luis-how-to-add-features.md) 
