---
title: Coleta de dados na Central de Segurança do Azure | Microsoft Docs
description: " Saiba como habilitar a coleta de dados na Central de Segurança do Azure. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2018
ms.author: rkarlin
ms.openlocfilehash: a5151d1f9498b29c79638445a58a8337abff8961
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281915"
---
# <a name="data-collection-in-azure-security-center"></a>Coleta de dados na Central de Segurança do Azure
A Central de Segurança coleta dados de suas VMs (máquinas virtuais) do Azure e dos computadores não Azure a fim de monitorar as ameaças e vulnerabilidades de segurança. Os dados são coletados usando o Microsoft Monitoring Agent, que lê várias configurações e logs de eventos relacionados à segurança do computador e copia os dados em seu espaço de trabalho para serem analisados. Exemplos desses dados são: tipo e versão do sistema operacional, logs do sistema operacional (logs de eventos do Windows), processos em execução, nome do computador, endereços IP, usuário registrado em log, eventos do AppLocker e ID do locatário. O Microsoft Monitoring Agent também copia os arquivos de despejo de memória para seu espaço de trabalho.

> [!NOTE]
> Para habilitar a coleta de dados para [Controles de Aplicativo Adaptáveis](security-center-adaptive-application.md), a Central de Segurança configura uma política do AppLocker local no modo de Auditoria para permitir todos os aplicativos. Isso fará com que o AppLocker gere eventos que são coletados e aproveitados pela Central de Segurança. É importante observar que essa política não será configurada em computadores nos quais já houver uma política do AppLocker configurada. 
>

## <a name="enable-automatic-provisioning-of-microsoft-monitoring-agent"></a>Habilitar o provisionamento automático do Microsoft Monitoring Agent     
O provisionamento automático é desativado por padrão. Quando o provisionamento automático está habilitado, a Central de Segurança provisiona o Microsoft Monitoring Agent em todas as VMs do Azure com suporte, bem como em quaisquer novas VMs que forem criadas. O provisionamento automático é altamente recomendável, mas a instalação manual do agente também está disponível. [Saiba como instalar a extensão do Microsoft Monitoring Agent](../log-analytics/log-analytics-quick-collect-azurevm.md#enable-the-log-analytics-vm-extension).

> [!NOTE]
> - Desabilitar o provisionamento automático limita o monitoramento de segurança dos seus recursos. Para obter mais informações, consulte [desabilitar o provisionamento automático](security-center-enable-data-collection.md#disable-automatic-provisioning) neste artigo. Os instantâneos de disco da VM e a coleção de artefatos ficam habilitados mesmo que o provisionamento automático esteja desabilitado.
>

Para habilitar o provisionamento automático do Microsoft Monitoring Agent:
1. No menu principal da Central de segurança, selecione **Política de segurança**.
2. Selecione a assinatura.

  ![Escolha a assinatura][7]

3. Em **Política de segurança**, selecione **Coleta de Dados**.
4. Em **Provisionamento Automático**, selecione **Ativar** para habilitar o provisionamento automático.
5. Clique em **Salvar**.

  ![Habilitar o provisionamento automático][1]

## <a name="default-workspace-configuration"></a>Configuração do espaço de trabalho padrão
Os dados coletados pela Central de Segurança são armazenados no espaço do Log Analytics.  Você pode optar por ter os dados coletados de VMs do Azure armazenados em espaços de trabalho criados pela Central de Segurança ou em um espaço de trabalho existente que você criou.

Para usar o espaço de trabalho existente do Log Analytics:
- O espaço de trabalho deve ser associado à sua assinatura do Azure selecionada.
- No mínimo, você deve ter permissões de leitura para acessar o espaço de trabalho.

Para selecionar um espaço de trabalho existente do Log Analytics:

1. Em **Configuração do espaço de trabalho padrão**, selecione **Usar outro espaço de trabalho**.

   ![Selecionar um espaço de trabalho existente][2]

2. No menu suspenso, selecione um espaço de trabalho para armazenar os dados coletados.

  > [!NOTE]
  > No menu suspenso, todos os espaços de trabalho em todas as suas inscrições estão disponíveis. Consulte a [seleção de espaço de trabalho de assinatura cruzada](security-center-enable-data-collection.md#cross-subscription-workspace-selection) para obter mais informações.
  >
  >

3. Clique em **Salvar**.
4. Ao selecionar **Salvar**, você será questionado se deseja reconfigurar as VMs monitoradas.

   - Selecione **Não** se quiser que as novas configurações de espaço de trabalho sejam aplicadas somente às novas VMs. As novas configurações de espaço de trabalho se aplicam somente a novas instalações de agente: VMs recém-descobertas que não têm o Microsoft Monitoring Agent instalado.
   - Selecione **Sim** se quiser que as novas configurações de espaço de trabalho sejam aplicadas a todas as VMs. Além disso, todas as VMs conectadas a um espaço de trabalho criado da Central de Segurança serão reconectadas ao novo espaço de trabalho de destino.

   > [!NOTE]
   > Se selecionar Sim, você não deverá excluir os espaços de trabalho criados pela Central de Segurança até que todas as VMs sejam reconectadas ao novo espaço de trabalho de destino. Essa operação falhará se um espaço de trabalho for excluído muito cedo.
   >
   >

   - Selecione **Cancelar** para cancelar a operação.

     ![Selecionar um espaço de trabalho existente][3]

## <a name="cross-subscription-workspace-selection"></a>Seleção de espaço de trabalho de assinatura cruzada
Ao selecionar um espaço de trabalho para armazenar os dados, todos os espaços de trabalho em todas as assinaturas estarão disponíveis. A seleção da área de trabalho de assinatura cruzada permite coletar dados de máquinas virtuais em execução em assinaturas diferentes e armazená-las no espaço de trabalho de sua escolha. Esse recurso funciona para ambas as máquinas virtuais em execução no Linux e no Windows.

> [!NOTE]
> A seleção de espaço de trabalho de assinatura cruzada faz parte da Camada Gratuita da Central de Segurança do Azure. Confira os [Preços](security-center-pricing.md) para saber mais sobre os tipos de preço da Central de Segurança.
>
>

## <a name="data-collection-tier"></a>Camada de coleta de dados
A Central de Segurança pode reduzir o volume de eventos, mantendo, ao mesmo tempo, eventos suficientes para investigação, auditoria e detecção de ameaças. Você pode escolher a política de filtragem correta para as suas assinaturas e espaços de trabalho dentre quatro conjuntos de eventos a serem coletados pelo agente.

- **Todos os eventos** – para clientes que desejam se certificar de que todos os eventos sejam coletados. Esse é o padrão.
- **Comum** – este é um conjunto de eventos que satisfaz a maioria dos clientes, garantindo uma trilha de auditoria completa.
- **Mínimo** – um conjunto menor de eventos, para clientes que desejam minimizar o volume de eventos.
- **Nenhum** – desabilitar a coleta de eventos de segurança da segurança e dos logs do AppLocker. Para clientes que escolherem esta opção, os painéis de segurança apresentarão somente os logs do Firewall do Windows e as avaliações proativas, como antimalware, linha de base e atualização.

> [!NOTE]
> Esses conjuntos de eventos de segurança estão disponíveis apenas na Camada Standard da Central de Segurança. Confira os [Preços](security-center-pricing.md) para saber mais sobre os tipos de preço da Central de Segurança.
Esses conjuntos foram projetados para lidar com cenários típicos. Avalie qual atende às suas necessidades antes de implementá-los.
>
>

Para determinar os eventos que farão parte dos conjuntos de eventos **Comum** e **Mínimo**, trabalhamos com clientes e padrões do setor para saber mais sobre a frequência não filtrada de cada evento e seu uso. Neste processo, usamos as seguintes diretrizes:

- **Mínimo** – Certificar-se de que esse conjunto aborde apenas os eventos que possam indicar uma violação com êxito e eventos importantes que tenham um volume muito baixo. Por exemplo, este conjunto conterá logon de usuário bem-sucedidos e malsucedidos (IDs de evento 4624 e 4625), mas não conterá o logout que seja importante para a auditoria, mas não seja significativo para a detecção e que tenha um volume relativamente alto. A maior parte do volume de dados desse conjunto é dos eventos de logon e do evento de criação de processo (ID de evento 4688).
- **Comum** – fornecer uma trilha de auditoria de usuário completa neste conjunto. Por exemplo, este conjunto contém tanto os logons quanto os logoffs de usuário (ID de evento 4634). Incluímos ações de auditoria, como alterações de grupo de segurança, operações Kerberos do controlador de domínio de chave e outros eventos que são recomendados por organizações do setor.

Eventos que têm um volume muito baixo foram incluídos no conjunto Comum, pois a principal motivação para escolhê-los dentre todos os eventos era reduzir o volume e não filtrar eventos específicos.

Aqui está um detalhamento completo das IDs de eventos de Segurança e do AppLocker para cada conjunto:

| Camada de dados | Indicadores de eventos coletados |
| --- | --- |
| Mínimo | 1102,4624,4625,4657,4663,4688,4700,4702,4719,4720,4722,4723,4724,4727,4728,4732,4735,4737,4739,4740,4754,4755, |
| | 4756,4767,4799,4825,4946,4948,4956,5024,5033,8001,8002,8003,8004,8005,8006,8007,8222 |
| Comum | 1,299,300,324,340,403,404,410,411,412,413,431,500,501,1100,1102,1107,1108,4608,4610,4611,4614,461,4622, |
| |  4624,4625,4634,4647,4648,4649,4657,4661,4662,4663,4665,4666,4667,4688,4670,4672,4673,4674,4675,4689,4697, |
| | 4700,4702,4704,4705,4716,4717,4718,4719,4720,4722,4723,4724,4725,4726,4727,4728,4729,4733,4732,4735,4737, |
| | 4738,4739,4740,4742,4744,4745,4746,4750,4751,4752,4754,4755,4756,4757,4760,4761,4762,4764,4767,4768,4771, |
| | 4774,4778,4779,4781,4793,4797,4798,4799,4800,4801,4802,4803,4825,4826,4870,4886,4887,4888,4893,4898,4902, |
| | 4904,4905,4907,4931,4932,4933,4946,4948,4956,4985,5024,5033,5059,5136,5137,5140,5145,5632,6144,6145,6272, |
| | 6273,6278,6416,6423,6424,8001,8002,8003,8004,8005,8006,8007,8222,26401,30004 |

> [!NOTE]
> - Se você está usando o GPO (Objeto de Política de Grupo), é recomendável habilitar o Evento de Criação de Processo 4688 das políticas de auditoria, bem como o campo *CommandLine* dentro do evento 4688. Para obter mais informações sobre o Processo de Criação do Evento 4688, consulte as [Perguntas Frequentes](security-center-faq.md#what-happens-when-data-collection-is-enabled) da Central de Segurança. Para obter mais informações sobre essas políticas de auditoria, consulte [Recomendações de Política de Auditoria](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations).
> -  Para habilitar a coleta de dados para [Controles de Aplicativo Adaptáveis](security-center-adaptive-application.md), a Central de Segurança configura uma política do AppLocker local no modo de Auditoria para permitir todos os aplicativos. Isso fará com que o AppLocker gere eventos que são coletados e aproveitados pela Central de Segurança. É importante observar que essa política não será configurada em computadores nos quais já houver uma política do AppLocker configurada. 
>

Para escolher a política de filtragem:
1. Na folha **Política de Segurança – Coleta de Dados**, selecione a política de filtragem em **Eventos de Segurança**.
2. Clique em **Salvar**.

   ![Escolher a política de filtragem][5]

## <a name="disable-automatic-provisioning"></a>Desabilitar o provisionamento automático
Você pode desabilitar o provisionamento automático de recursos a qualquer momento, desativando essa configuração na política de segurança. O provisionamento automático é altamente recomendável a fim de obter alertas de segurança e recomendações sobre atualizações do sistema, vulnerabilidades do sistema operacional e proteção do ponto de extremidade.

> [!NOTE]
> Desabilitar o provisionamento automático não remove o Microsoft Monitoring Agent das VMs do Azure em que o agente tenha sido provisionado.
>
>

1. Retorne ao menu principal da Central de Segurança e selecione a Política de segurança.
2. Selecione a assinatura em que você deseja desabilitar o provisionamento automático.
3. Na folha **Política de Segurança – Coleta de Dados** em **Provisionamento Automático** selecione **Desativar**.
4. Clique em **Salvar**.

  ![Desabilitar o provisionamento automático][6]

Quando o provisionamento automático estiver desabilitado (desativado), a seção de configuração do espaço de trabalho padrão não será exibida.

## <a name="next-steps"></a>Próximas etapas
Este artigo mostrou como a coleta de dados e o provisionamento automático na Central de Segurança funcionam. Para saber mais sobre a Central de Segurança, confira o seguinte:

* [Configurando políticas de segurança na Central de Segurança do Azure](security-center-policies.md) : saiba como configurar políticas de segurança para suas assinaturas e grupos de recursos do Azure.
* [Gerenciar as recomendações de segurança na Central de Segurança do Azure](security-center-recommendations.md) – saiba como as recomendações ajudam a proteger os recursos do Azure.
* [Monitoramento da integridade de segurança na Central de Segurança do Azure](security-center-monitoring.md)– saiba como monitorar a integridade dos recursos do Azure.
* [Gerenciar e responder aos alertas de segurança na Central de Segurança do Azure](security-center-managing-and-responding-alerts.md): aprenda a gerenciar e responder aos alertas de segurança.
* [Monitoramento de soluções de parceiros com a Central de Segurança do Azure](security-center-partner-solutions.md) – saiba como monitorar o status de integridade de suas soluções de parceiro.
- [Segurança de dados da Central de Segurança do Azure](security-center-data-security.md) – saiba como os dados são gerenciados e protegidos na Central de Segurança do Azure.
* [Perguntas frequentes sobre a Central de Segurança do Azure](security-center-faq.md): encontre perguntas frequentes sobre como usar o serviço.
* [Blog de Segurança do Azure](http://blogs.msdn.com/b/azuresecurity/): obtenha as últimas notícias de segurança e informações do Azure.

<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
