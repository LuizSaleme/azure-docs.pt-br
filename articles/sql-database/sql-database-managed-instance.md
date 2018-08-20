---
title: Visão geral da Instância Gerenciada do Banco de Dados SQL do Azure | Microsoft Docs
description: Este tópico descreve uma Instância Gerenciada do Banco de Dados SQL do Azure e explica como funciona e se difere de um banco de dados individual no Banco de Dados SQL do Azure.
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: bonova
ms.openlocfilehash: edacb9fe1d09a4e775f8f7107dfa4d9810f53f07
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006037"
---
# <a name="what-is-a-managed-instance-preview"></a>O que é uma Instância Gerenciada (versão prévia)?

A Instância Gerenciada do Banco de Dados SQL do Azure (versão prévia) é um novo recurso do Banco de Dados SQL do Azure que proporciona quase 100% de compatibilidade com o SQL Server local (Enterprise Edition), fornecendo uma implementação de [rede virtual (VNet)](../virtual-network/virtual-networks-overview.md) nativa que aborda questões de segurança comuns e um [modelo corporativo](https://azure.microsoft.com/pricing/details/sql-database/) favorável para clientes do Cliente do Microsoft SQL Server local. A Instância Gerenciada permite que os clientes do SQL Server existentes façam lift-and-shift dos aplicativos locais para a nuvem com alterações mínimas do banco de dados e aplicativo. Ao mesmo tempo, a Instância Gerenciada preserva todos os recursos de PaaS (aplicação automática de patches e atualizações de versões, backup, alta disponibilidade), que reduz drasticamente a sobrecarga de gerenciamento e o TCO.

> [!IMPORTANT]
> Para obter uma lista de regiões nas quais a Instância Gerenciada está disponível no momento, consulte [Migrar os bancos de dados para um serviço totalmente gerenciado com a Instância Gerenciada do Banco de Dados SQL do Azure](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
O diagrama a seguir apresenta os principais recursos da Instância Gerenciada:

![principais recursos](./media/sql-database-managed-instance/key-features.png)

A Instância Gerenciada é idealizada como plataforma preferencial para os seguintes cenários: 

- Usuários do SQL Server local/IaaS que procuram migrar os aplicativos para um serviço totalmente gerenciado com alterações de design mínimas.
- ISVs baseados em Bancos de Dados SQL, que querem habilitar seus clientes a migrarem para a nuvem e, desse modo, obterem uma vantagem competitiva substancial ou alcance no mercado global. 

Por Disponibilidade Geral, a Instância Gerenciada visa entregar aproximadamente 100% de compatibilidade de área de superfície com a última versão do SQL Server local por meio de um plano de lançamento em etapas. 

A tabela a seguir apresenta as principais diferenças e os cenários de uso previstos entre SQL IaaS, Banco de Dados SQL do Azure e Instância Gerenciada do Banco de Dados SQL:

| | Cenário de uso | 
| --- | --- | 
|Instância Gerenciada do Banco de Dados SQL |Para clientes que procuram migrar uma grande quantidade de aplicativos locais ou IaaS, auto-compilados, ou ISV fornecidos, com o menor esforço de migração possível, propõe-se a Instância Gerenciada. Utilizando o [DMS (Serviço de Migração de Dados) ](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) totalmente automatizado no Azure, os clientes podem fazer lift-and-shift do SQL Server local para uma Instância Gerenciada que oferece compatibilidade com o SQL Server local e isolamento completo de instâncias do cliente com suporte nativo de VNET.  Com o Software Assurance, é possível trocar suas licenças existentes por tarifas com desconto em uma Instância Gerenciada do Banco de Dados SQL usando o [Benefício de uso híbrido do Azure para SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  A Instância Gerenciada do Banco de Dados SQL é o melhor destino da migração na nuvem para instâncias do SQL Server que exigem alta segurança e uma superfície de programação avançada. |
|Banco de Dados SQL do Azure (único ou pool) |**Pools Elásticos**: para os clientes que desenvolvem novos aplicativos multilocatários SaaS ou intencionalmente transformando seus aplicativos locais existentes em um aplicativo multilocatário SaaS, propõe-se pools elásticos. Os benefícios desse modelo são: <br><ul><li>Conversão do modelo de negócios da venda de licenças para venda de assinaturas de serviços (para ISVs)</li></ul><ul><li>Isolamento de locatário fácil e à prova de marcador</li></ul><ul><li>Um modelo de programação centrada em banco de dados simplificado</li></ul><ul><li>O potencial para escalar horizontalmente atingir um limite rígido</li></ul>**Banco de dados individuais**: para clientes que desenvolvem novos aplicativos diferentes do multilocatário SaaS, cuja carga de trabalho é estável e previsível, propõe-se bancos de dados individuais. Os benefícios desse modelo são:<ul><li>Um modelo de programação centrada em banco de dados simplificado</li></ul>  <ul><li>Desempenho previsível para cada banco de dados</li></ul>|
|Máquina virtual de IaaS do SQL|Para clientes que necessitam personalizar o sistema operacional ou o servidor de banco de dados, assim como clientes que possuem requisitos específicos em termos de execução de aplicativos de terceiros junto com SQL Server (na mesma VM), propõe-se IaaS/VMs do SQL como a solução ideal|
|||

## <a name="how-to-programmatically-identify-a-managed-instance"></a>Como identificar programaticamente uma Instância Gerenciada

A tabela a seguir mostra várias propriedades, acessíveis por meio do Transact-SQL que podem ser utilizadas para detectar se o aplicativo está trabalhando com a Instância Gerenciada e recuperar propriedades importantes.

|Propriedade|Valor|Comentário|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 07/03/2018 Copyright (C) 2018 Microsoft Corporation.|Esse valor é o mesmo que no Banco de Dados SQL.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Esse valor é o mesmo que no Banco de Dados SQL.|
|`SERVERPROPERTY('EngineEdition')`|8|Esse valor identifica exclusivamente a Instância Gerenciada.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Nome DNS da instância completo no seguinte formato:<instanceName>.<dnsPrefix>.database.windows.net, onde <instanceName> é o nome fornecido pelo cliente, enquanto <dnsPrefix> é a parte gerada automaticamente do nome, garantindo exclusividade de nome DNS global ("wcus17662feb9ce98", por exemplo)|Exemplo: my-managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="key-features-and-capabilities-of-a-managed-instance"></a>Principais recursos e capacidades de uma Instância Gerenciada 

> [!IMPORTANT]
> Uma Instância Gerenciada executa com todos os recursos da versão mais recente do SQL Server, incluindo operações online, correções de plano automático e outros aprimoramentos de desempenho do enterprise. 

| **Benefícios de PaaS** | **Continuidade dos negócios** |
| --- | --- |
|Sem gerenciamento e compra de hardware <br>Sem sobrecarga de gerenciamento para gerenciar infraestrutura subjacente <br>Rápido provisionamento e dimensionamento de serviço <br>Aplicação de patch automatizado e atualização da versão <br>Integração com outros serviços de dados PaaS |99,99% do SLA de tempo de atividade  <br>Compilado em alta disponibilidade <br>Dados protegidos com backups automatizados <br>Período de retenção de backup configurável pelo cliente (corrigido para 7 dias na Visualização Pública) <br>Backups iniciados pelo usuário <br>Capacidade de restauração pontual do banco de dados |
|**Segurança e conformidade** | **Gerenciamento**|
|Ambiente isolado (integração de VNet, serviço de locatário único, computação dedicada e armazenamento) <br>Transparent Data Encryption<br>Autenticação do Microsoft Azure AD, suporte de logon único <br>Cumpre os padrões de conformidade assim como o Banco de Dados SQL do Azure <br>Auditoria do SQL <br>Detecção de ameaças |API do Azure Resource Manager para automatizar o dimensionamento e provisionamento do serviço <br>Funcionalidade do Portal do Azure para dimensionamento e provisionamento manual do serviço <br>Serviço de Migração de Dados 

![logon único](./media/sql-database-managed-instance/sso.png) 

## <a name="vcore-based-purchasing-model"></a>Modelo de compra baseado em vCore

O modelo de compra baseado em vCore proporciona flexibilidade, controle e transparência, além de ser uma maneira simples de mover os requisitos das cargas de trabalho locais para a nuvem. Esse modelo permite escalar computação, memória e armazenamento com base nas necessidades de carga de trabalho. O modelo vCore também pode ser usado para economias de até 30% com o [Benefício de Uso Híbrido do Azure para SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Um núcleo virtual representa a CPU lógica oferecida com uma opção para escolher entre gerações de hardware.
- As CPUs Lógicas de 4ª geração são baseadas em processadores Intel E5-2673 v3 (Haswell) 2,4 GHz.
- As CPUs Lógicas de 5ª geração são baseadas em processadores E5-2673 v4 (Broadwell) 2,3 GHz.

A tabela a seguir o ajudará a entender como selecionar a configuração ideal de seus recursos de computação, memória, armazenamento e E/S.

||Gen 4|Gen 5|
|----|------|-----|
|Hardware|Processadores Intel E5-2673 v3 (Haswell) 2,4 GHz, SSD anexado vCore = 1 PP (núcleo físico)|Processadores V4 Intel E5-2673 (Broadwell) 2,3 GHz, SSD eNVM rápido, vCore = 1 LP (hyper-thread)|
|Níveis de desempenho|8, 16, 24 vCores|8, 16, 24, 32, 40, 64, 80 vCores|
|Memória|7 GB por vCore|5.5 GB por vCore|
||||

## <a name="managed-instance-service-tiers"></a>Camadas de serviço de Instância Gerenciada

A instância gerenciada está disponível em duas camadas de serviço:
- **Uso geral**: projetado para aplicativos com disponibilidade típica e requisitos de latência de E/S comuns.
- **Negócio crítico**: Projetado para aplicativos com alta disponibilidade e baixos requisitos de latência de E/S.
 
> [!IMPORTANT]
> Não há suporte para alterar sua camada de serviço de uso geral para Comercialmente Crítico ou vice-versa em visualização pública. Se você quiser migrar seus bancos de dados para uma instância na camada de serviço diferentes, pode criar a nova instância, restaurar os bancos de dados com recuperação pontual da instância original e, em seguida, descarte a instância original se não for mais necessária. 

### <a name="general-purpose-service-tier"></a>Camada de serviço de Uso Geral

A lista a seguir apresenta a principal característica da camada de serviço de Uso Geral: 

- Design para a maioria dos aplicativos de negócios com desempenho típico e requisitos de HA 
- Armazenamento Premium do Azure de alto desempenho (8 TB) 
- 100 bancos de dados/instâncias 

Nessa camada, é possível selecionar de forma independente a capacidade de computação e o armazenamento. 

O diagrama a seguir ilustra a computação ativa e os nós redundantes nessa camada de serviço.
 
![Camada de serviço de Uso Geral](./media/sql-database-managed-instance/general-purpose-service-tier.png) 

A lista a seguir apresenta a principal característica da camada de serviço de Uso Geral:

|Recurso | DESCRIÇÃO|
|---|---|
| Número de vCores* | 8, 16, 24 (Geração 4)<br>8, 16, 24, 32, 40, 64, 80 (Geração 5)|
| Compilação/versão do SQL Server | SQL Server (mais recente disponível) |
| Tamanho mínimo de armazenamento | 32 GB |
| Tamanho máximo de armazenamento | 8 TB |
| Armazenamento máximo por banco de dados | Determinado pelo tamanho de armazenamento máximo por instância |
| IOPS de armazenamento esperado | 500-7500 IOPS por arquivo de dados (depende do arquivo de dados). Consulte [Armazenamento Premium](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes) |
| Número de arquivos de dados (LINHAS) por banco de dados | Vários | 
| Número de arquivos de log (LOG) por banco de dados | 1 | 
| Backups automatizados gerenciados | SIM |
| HA | Com base em armazenamento remoto e [Microsoft Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Métricas e monitoramento de banco de dados e instância interna | SIM |
| Aplicação automática de patches de software | SIM |
| VNet - Implantação do Azure Resource Manager | SIM |
| VNet - Modelo de implantação clássico | Não  |
| Suporte do Portal | SIM|
|||

\* Um núcleo virtual representa a CPU lógica oferecida com uma opção para escolher entre gerações de hardware. As CPUs Lógicas Geração 4 são baseadas em processadores Intel E5-2673 v3 (Haswell) de 2,4 GHz e as CPUs Lógicas Geração 5 são baseadas em processadores Intel E5-2673 v4 (Broadwell) de 2,3 GHz. 

### <a name="business-critical-service-tier"></a>Camada de serviço comercialmente crítica

Camada de serviço Comercialmente Crítico desenvolvida para aplicativos com altos requisitos de E/S. Oferece maior resiliência a falhas usando réplicas Always On. O diagrama a seguir ilustra a arquitetura de serviço desta camada de serviço:

![Camada de serviço comercialmente crítica](./media/sql-database-managed-instance/business-critical-service-tier.png)  

A lista a seguir apresenta as características principais da camada Comercialmente Crítico: 
-   Projetada para aplicativos de negócios com requisitos de alta disponibilidade e desempenho mais alto 
-   Vem com armazenamento SSD super rápido (até 1 TB no Gen 4 e até 4 TB no Gen 5)
-   Dá suporte a até 100 bancos de dados por instância 

|Recurso | DESCRIÇÃO|
|---|---|
| Número de vCores* | 8, 16, 24, 32 (Geração 4)<br>8, 16, 24, 32, 40, 64, 80 (Geração 5)|
| Compilação/versão do SQL Server | SQL Server (mais recente disponível) |
| Recursos adicionais | [OLTP in-memory](sql-database-in-memory.md)<br> 1 réplica somente leitura adicional ([expansão de leitura](sql-database-read-scale-out.md))
| Tamanho mínimo de armazenamento | 32 GB |
| Tamanho máximo de armazenamento | Geração 4: 1 TB (todos os tamanhos de vCore<br> Geração 5:<ul><li>1 TB para 8, 16 vCores</li><li>2 TB para 24 vCores</li><li>4 TB for 32, 40, 64, 80 vCores</ul>|
| Armazenamento máximo por banco de dados | Determinado pelo tamanho de armazenamento máximo por instância |
| Número de arquivos de dados (LINHAS) por banco de dados | Vários | 
| Número de arquivos de log (LOG) por banco de dados | 1 | 
| Backups automatizados gerenciados | SIM |
| HA | Com base em [grupos de disponibilidade AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) e [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Métricas e monitoramento de banco de dados e instância interna | SIM |
| Aplicação automática de patches de software | SIM |
| VNet - Implantação do Azure Resource Manager | SIM |
| VNet - Modelo de implantação clássico | Não  |
| Suporte do Portal | SIM|
|||

## <a name="advanced-security-and-compliance"></a>Segurança e conformidade avançadas 

### <a name="managed-instance-security-isolation"></a>Isolamento de segurança da Instância Gerenciada 

A Instância Gerenciada fornece isolamento de segurança adicional de outros locatários na nuvem do Azure. O isolamento de segurança inclui: 

- [Implementação e conectividade de rede virtual nativa](sql-database-managed-instance-vnet-configuration.md) com o ambiente local usando a Rota Expressa do Azure ou Gateway de VPN 
- O ponto de extremidade do SQL é exposto apenas por meio de um endereço IP privado, permitindo conectividade segura a partir de redes híbridas ou privadas do Azure
- Locatário único com infraestrutura subjacente dedicada (computação, armazenamento)

O diagrama a seguir descreve várias opções de conectividade para seus aplicativos: 

![alta disponibilidade](./media/sql-database-managed-instance/application-deployment-topologies.png)  

Para saber mais detalhes sobre a integração de rede virtual e a aplicação de políticas no nível de sub-rede de rede, consulte [Configurar uma rede virtual para o banco de dados de instância gerenciada do SQL](sql-database-managed-instance-vnet-configuration.md) e [conectar seu aplicativo à Instância Gerenciada do Banco de Dados SQL do Azure](sql-database-managed-instance-connect-app.md). 

> [!IMPORTANT]
> Coloque várias instâncias gerenciadas na mesma sub-rede, onde quer que o permitido pela seus requisitos de segurança, como o que trará benefícios adicionais. Disposição das instâncias na mesma sub-rede será significativamente simplificar a manutenção de infraestrutura de rede e reduzir o tempo, o provisionamento, pois o provisionamento de longa duração está associado ao custo de implantação de primeira instância gerenciada em uma sub-rede de instância.


### <a name="auditing-for-compliance-and-security"></a>Auditoria de segurança e conformidade 

A [auditoria de Instância Gerenciada](sql-database-managed-instance-auditing.md) rastreia eventos de banco de dados e os grava em um log de auditoria na conta de armazenamento do Azure. A auditoria pode ajudar a manter conformidade com as normas, entender a atividade do banco de dados e ter ideia das discrepâncias e anomalias que podem gerar preocupações comerciais ou violações suspeitas de seguranças. 

### <a name="data-encryption-in-motion"></a>Criptografia dos dados em trânsito 

A Instância Gerenciada protege os dados, fornecendo criptografia para dados em movimentação usando o protocolo TLS.

Além do protocolo TLS, a Instância Gerenciada do Banco de Dados SQL oferece proteção de dados confidenciais em trânsito, em repouso e durante processamento de consulta com [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Always Encrypted é pioneiro na indústria, oferecendo segurança de dados incomparável contra violações envolvendo o roubo de dados críticos. Por exemplo, com o Always Encrypted, números de cartão de crédito são armazenados sempre criptografados no banco de dados, mesmo durante a consulta de processamento, permitindo a descriptografia no ponto de uso por pessoal autorizado ou aplicativos que precisam processar os dados. 

### <a name="data-encryption-at-rest"></a>Criptografia de dados em repouso 
A [TDE (Transparent Data Encryption)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) criptografa arquivos de dados de Instância Gerenciada do Banco de Dados SQL do Azure, conhecido como dados de criptografia em repouso. A TDE realiza a criptografia e a descriptografia de E/S em tempo real dos arquivos de log e de dados. A criptografia usa uma chave de criptografia de banco de dados (DEK), que é armazenada no registro de inicialização do banco de dados para disponibilidade durante a recuperação. Você pode proteger todos os seus bancos de dados na instância gerenciada com a criptografia transparente de dados. A TDE é a tecnologia de criptografia ociosa comprovada da SQL exigida por vários padrões de conformidade para proteger contra roubo de mídia de armazenamento. Durante a visualização pública, o modelo de gerenciamento automático de chaves é compatível (realizada pela plataforma de PaaS). 

Há suporte para a migração de um banco de dados criptografado para a instância gerenciada do SQL por meio do serviço de migração de banco de dados do Azure (DMS) ou restauração nativa. Se você planeja migrar o banco de dados criptografado usando restauração nativa, a migração do certificado TDE existente do SQL Server no local ou VM do SQL Server para instância gerenciada é uma etapa necessária. Para obter mais informações sobre as opções de migração consulte [migração da instância do SQL Server à Instância Gerenciada do Banco de Dados SQL do Azure](sql-database-managed-instance-migrate.md).

### <a name="dynamic-data-masking"></a>Mascaramento de dados dinâmicos 

A [máscara de dados dinâmicos](/sql/relational-databases/security/dynamic-data-masking) do Banco de Dados SQL limita a exposição de dados confidenciais por meio da máscara dos dados para usuários sem privilégios. A máscara de dados dinâmicos ajuda a impedir o acesso não autorizado a dados confidenciais, permitindo que você designe a quantidade de dados confidenciais a ser revelada, com impacto mínimo sobre a camada de aplicativo. É um recurso de segurança baseado em políticas que oculta os dados confidenciais no conjunto de resultados de uma consulta em relação aos campos do banco de dados designado, enquanto os dados no banco de dados não são alterados. 

### <a name="row-level-security"></a>Segurança em nível de linha 

A [segurança em nível de linha](/sql/relational-databases/security/row-level-security) permite controlar o acesso às linhas em uma tabela de banco de dados com base nas características do usuário que executa uma consulta (por exemplo, uma associação de grupo ou um contexto de execução). A RLS (Segurança em Nível de Linha) simplifica o design e codificação de segurança em seu aplicativo. Ela permite implementar restrições de acesso à linha de dados. Por exemplo, garantindo que os funcionários possam acessar apenas as linhas de dados pertinentes ao seu departamento ou restringindo o acesso a dados apenas para dados relevantes. 

### <a name="threat-detection"></a>Detecção de ameaças 

A [Detecção de Ameaças de Instância Gerenciada](sql-database-managed-instance-threat-detection.md) complementa a [auditoria de Instância Gerenciada](sql-database-managed-instance-auditing.md), fornecendo uma camada adicional de inteligência de segurança compilada para o serviço que detecta tentativas incomuns e potencialmente perigosas de acessar ou explorar bancos de dados. Você é alertado sobre atividades suspeitas, vulnerabilidades potenciais, ataques de injeção de SQL, bem como padrões de acesso do banco de dados anormais. Os alertas da Detecção de Ameaças podem ser exibidos no [Azure Security Center](https://azure.microsoft.com/services/security-center/) e fornecem detalhes de atividades suspeitas e recomendam ação de como investigar e atenuar a ameaça.  

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Integração do Azure Active Directory e autenticação multifator 

O Banco de Dados SQL permite gerenciar centralmente as identidades de usuário do banco de dados e de outros serviços da Microsoft com a [integração do Azure Active Directory](sql-database-aad-authentication.md). Esse recurso simplifica o gerenciamento de permissão e aprimora a segurança. O Azure Active Directory é compatível com [MFA](sql-database-ssms-mfa-authentication-configure.md) (autenticação multifator) para aumentar a segurança de aplicativos e dados e dá suporte a um processo de logon único. 

### <a name="authentication"></a>Autenticação 
A autenticação do Banco de Dados SQL refere-se a como os usuários comprovam a identidade ao conectarem-se ao banco de dados. O Banco de Dados SQL dá suporte a dois tipos de autenticação:  

- Autenticação do SQL, que usa um nome de usuário e senha.
- Autenticação do Azure Active Directory, que usa identidades gerenciadas pelo Azure Active Directory e que tem suporte para domínios gerenciados e integrados. 

### <a name="authorization"></a>Autorização

Autorização refere-se ao que um usuário pode fazer em um Banco de Dados SQL do Azure e é controlado pelas associações a uma função de banco de dados e pelas permissões no nível de objeto da conta de usuário. A Instância Gerenciada possui os mesmos recursos de autorização que o SQL Server 2017. 

## <a name="database-migration"></a>Migração de banco de dados 

A Instância Gerenciada direciona cenários de usuários com migração de banco de dados em massa de implementações de bancos de dados locais ou IaaS. A Instância Gerenciada oferece suporte a várias opções de migração de banco de dados: 

### <a name="data-migration-service"></a>Serviço de Migração de Dados

O Serviço de Migração de Banco de Dados do Azure é um serviço totalmente gerenciado projetado para permitir migrações perfeitas de várias fontes de banco de dados para plataformas de dados do Azure com um tempo de inatividade mínimo. Esse serviço simplifica as tarefas necessárias para mover bancos de dados de terceiros e SQL Server existentes para o Azure. As opções de implantação incluem Banco de Dados SQL do Azure, Instance Gerenciada e SQL Server na VM do Azure em Visualização Pública. Consulte [Como migrar o banco de dados local para a Instância Gerenciada usando DMS](https://aka.ms/migratetoMIusingDMS). 

### <a name="backup-and-restore"></a>Backup e restauração  

A abordagem de migração aproveita backups do SQL para Azure Storage Blob. Backups armazenados no Azure Storage Blob podem ser restaurados diretamente na Instância Gerenciada. Para restaurar um banco de dados SQL existente para uma Instância Gerenciada, você pode:

- Usar o [DMS (Serviço de Migração de Dados)](../dms/dms-overview.md). Para obter um tutorial, consulte [Migrar para uma Instância Gerenciada usando o DMS (Serviço de Migração de Dados) do Azure](../dms/tutorial-sql-server-to-managed-instance.md) para restaurar a partir de um arquivo de backup do banco de dados
- Use o [comando T-SQL RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql). 
  - Para obter um tutorial mostrando como restaurar a Wide World Importers - Arquivo de backup do banco de dados padrão, consulte [Restaurar um arquivo de backup para uma instância gerenciada](sql-database-managed-instance-restore-from-backup-tutorial.md). Este tutorial mostra que você precisa carregar um arquivo de backup para o armazenamento de blog do Azure e o proteja usando uma Chave de assinatura de acesso compartilhado (SAS).
  - Para obter informações sobre restauração de URL, consulte [Restauração nativa de URL](sql-database-managed-instance-migrate.md#native-restore-from-url).

## <a name="sql-features-supported"></a>Recursos do SQL com suporte 

A Instância Gerenciada visa entregar aproximadamente 100% de compatibilidade de área de superfície com o SQL Server local em etapas, até a disponibilidade geral do serviço. Para um recurso e lista de comparação, consulte [Recursos comuns do SQL](sql-database-features.md).
 
A Instância Gerenciada tem suporte para compatibilidade com versões anteriores para Bancos de Dados do SQL 2008. A migração direta dos servidores do Banco de Dados do SQL 2005 tem suporte, o nível de compatibilidade para Bancos de Dados do SQL 2005 migrados é atualizado para o SQL 2008. 
 
O diagrama a seguir apresenta a compatibilidade da área de superfície na Instância Gerenciada:  

![migração](./media/sql-database-managed-instance/migration.png) 

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Principais diferenças entre SQL Server local e Instância Gerenciada 

A Instância Gerenciada se beneficia de estar sempre atualizada na nuvem, o que significa que alguns recursos no SQL Server local podem estar obsoletos, desativados ou ter alternativas. Há casos específicos em que as ferramentas precisam reconhecer que um recurso particular funciona de forma ligeiramente diferente ou que o serviço não está executando em um ambiente que não totalmente controlado: 

- Alta disponibilidade é compilada e pré-configurada. Alta disponibilidade Always On não são expostos da mesma forma que nas implementações SQL IaaS 
- Backups automatizados e restauração pontual. O cliente pode iniciar backups `copy-only` que não interferem na cadeia de backup automático. 
- A Instância Gerenciada não permite especificar caminhos físicos completos, então, todos os cenários correspondentes devem ter suporte de forma diferente: RESTORE DB não dá suporte para WITH MOVE, CREATE DB não permite caminhos físicos, BULK INSERT funciona apenas com Blobs do Azure e etc. 
- A Instância Gerenciada dá suporte para [Autenticação do Azure AD](sql-database-aad-authentication.md) como alternativa de nuvem para autenticação do Windows. 
- A Instância Gerenciada gerencia automaticamente o grupo de arquivos XTP e os arquivos para bancos de dados que contêm objetos OLTP na memória
- Instância gerenciada suporta SSIS (SQL Server Integration Services) e pode hospedar SSIS catalog (SSISDB) que armazena pacotes SSIS, mas eles são executados em um Azure-ISR (Azure-SSIS Integration Runtime) no Azure Data Factory (ADF), consulte [ Crie o IR do Azure-SSIS no ADF ](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). Para comparar os recursos do SSIS no banco de dados SQL e a instância gerenciada, consulte [comparar banco de dados SQL e a instância gerenciada (versão prévia)](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview).

### <a name="managed-instance-administration-features"></a>Recursos de administração de Instância Gerenciada do Banco de Dados SQL do Azure  

A Instância Gerenciada permite que o administrador do sistema concentre-se no que é mais importante para os negócios. Muitas atividades DBA/administrador do sistema não são necessárias ou são simples. Por exemplo, instalação do RDBMS/SO e aplicação de patch, redimensionamento de instância dinâmica e configuração, backups, replicação de banco de dados (incluindo bancos de dados do sistema), configuração de alta disponibilidade e configuração de fluxos de dados de monitoramento de desempenho e integridade. 

> [!IMPORTANT]
> Para obter uma lista de recursos com suporte, suporte parcial e sem suporte, consulte [Recursos do Banco de Dados SQL](sql-database-features.md). Para obter uma lista de diferenças T-SQL em Instâncias Gerenciadas em comparação com SQL Server, consulte [Diferenças T-SQL de Instância Gerenciada do SQL Server](sql-database-managed-instance-transact-sql-information.md)
 
## <a name="next-steps"></a>Próximas etapas

- Para obter uma lista de recursos e de comparação, consulte [Recursos comuns do SQL](sql-database-features.md).
- Para saber mais sobre a configuração de rede virtual, confira [Configuração de VNet de Instância Gerenciada](sql-database-managed-instance-vnet-configuration.md).
- Para obter um tutorial que cria uma Instância Gerenciada e restaura um banco de dados de um arquivo de backup, consulte [Criar uma Instância Gerenciada](sql-database-managed-instance-create-tutorial-portal.md).
- Para obter um tutorial usando o DMS (Serviço de Migração de Banco de Dados do Azure) para a migração, confira [Migração de Instância Gerenciada usando DMS](../dms/tutorial-sql-server-to-managed-instance.md).
- Para saber mais sobre preços, consulte [Preços do Banco de Dados SQL gerenciados](https://azure.microsoft.com/pricing/details/sql-database/managed/).
