---
title: Gerenciar retenção de backup de longo prazo do Banco de Dados SQL do Azure | Microsoft Docs
description: Saiba como armazenar backups automatizados no armazenamento do SQL do Azure e como restaurá-los
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 6f0d8a3a09ce000ddff078614c4febfc44ac941f
ms.sourcegitcommit: a5eb246d79a462519775a9705ebf562f0444e4ec
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/26/2018
ms.locfileid: "39264936"
---
# <a name="manage-azure-sql-database-long-term-backup-retention"></a>Gerenciar a retenção de backup de longo prazo do Banco de Dados SQL do Azure

Você pode configurar o banco de dados do SQL do Azure com uma política de [retenção de backup de longo prazo](sql-database-long-term-retention.md) (LTR) para reter automaticamente os backups no armazenamento de blobs do Azure por até 10 anos. Em seguida, você pode recuperar um banco de dados usando esses backups pelo Portal do Azure ou o PowerShell.

## <a name="use-the-azure-portal-to-configure-long-term-retention-policies-and-restore-backups"></a>Usar o Portal do Azure para configurar políticas de retenção de longo prazo e restaurar backups

As seções a seguir mostram como usar o Portal do Azure para configurar a retenção de longo prazo, exibir os backups em retenção de longo prazo e restaurar backups da retenção de longo prazo.

### <a name="configure-long-term-retention-policies"></a>Configurar políticas de retenção de longo prazo

Você pode configurar o Banco de Dados SQL para [reter backups automatizados](sql-database-long-term-retention.md) por um período maior que o período de retenção da camada de serviços. 

1. No portal do Azure, selecione o servidor SQL e clique em **Gerenciar Backups**. Na guia **Configurar políticas**, selecione o banco de dados no qual você deseja definir ou modificar políticas de retenção de backup de longo prazo.

   ![link gerenciar backups](./media/sql-database-long-term-retention/ltr-configure-ltr.png)

2. No painel **Configurar políticas**, selecione se deseja reter backups semanais, mensais ou anuais e especifique o período de retenção para cada um. 

   ![configurar políticas](./media/sql-database-long-term-retention/ltr-configure-policies.png)

3. Quando concluir, clique em **Aplicar**.

### <a name="view-backups-and-restore-from-a-backup-using-azure-portal"></a>Exibir os backups e restaurar de um backup usando o Portal do Azure

Exiba os backups que são mantidos para um banco de dados específico com uma política LTR e restaure a partir desses backups. 

1. No portal do Azure, selecione o servidor SQL e clique em **Gerenciar Backups**. Na guia **Backups disponíveis**, selecione o banco de dados para o qual você deseja ver os backups disponíveis.

   ![selecionar banco de dados](./media/sql-database-long-term-retention/ltr-available-backups-select-database.png)

3. No painel **Backups disponíveis**, examine os backups disponíveis. 

   ![exibir backups](./media/sql-database-long-term-retention/ltr-available-backups.png)

4. Selecione o backup do qual você deseja restaurar e, em seguida, especifique o novo nome do banco de dados.

   ![restaurar](./media/sql-database-long-term-retention/ltr-restore.png)

5. Clique em **OK** para restaurar seu banco de dados a partir do backup no armazenamento do SQL do Azure para o novo banco de dados.

6. Na barra de ferramentas, clique no ícone de notificação para exibir o status do trabalho de restauração.

   ![progresso do trabalho de restauração](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Quando o trabalho de restauração for concluído, abra a página **Bancos de dados SQL** para exibir o banco de dados recém-restaurado.

> [!NOTE]
> A partir daqui, você pode conectar o banco de dados restaurado usando o SQL Server Management Studio para executar as tarefas necessárias, tais como, [extrair um pouco de dados do banco de dados restaurado para copiar para o banco de dados existente ou excluir o banco de dados existente e renomear o banco de dados restaurado com o nome do banco de dados existente](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="use-powershell-to-configure-long-term-retention-policies-and-restore-backups"></a>Usar o PowerShell para configurar políticas de retenção de longo prazo e restaurar backups

As seções a seguir mostram como usar o PowerShell para configurar a retenção de backup de longo prazo, exibir backups no armazenamento do SQL do Azure e restaurar a partir de um backup no armazenamento do SQL do Azure.

> [!IMPORTANT]
> Há suporte para a API de LTR V2 nas seguintes versões do PowerShell:
- [AzureRM.Sql-4.5.0](https://www.powershellgallery.com/packages/AzureRM.Sql/4.5.0) ou mais recente
- [AzureRM-6.1.0](https://www.powershellgallery.com/packages/AzureRM/6.1.0) ou mais recente
> 

### <a name="create-an-ltr-policy"></a>Criar uma política LTR

```powershell
# Get the SQL server 
# $subId = “{subscription-id}”
# $serverName = “{server-name}”
# $resourceGroup = “{resource-group-name}” 
# $dbName = ”{database-name}”

Connect-AzureRmAccount
Select-AzureRmSubscription -SubscriptionId $subId

# get the server
$server = Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroup

# create LTR policy with WeeklyRetention = 12 weeks. MonthlyRetention and YearlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W 

# create LTR policy with WeeklyRetention = 12 weeks, YearlyRetetion = 5 years and WeekOfYear = 16 (week of April 15). MonthlyRetention = 0 by default.
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -WeeklyRetention P12W -YearlyRetention P5Y -WeekOfYear 16
```

### <a name="view-ltr-policies"></a>Exibir políticas LTR
Este exemplo mostra como listar as políticas LTR dentro de um servidor

```powershell
# Get all LTR policies within a server
$ltrPolicies = Get-AzureRmSqlDatabase -ResourceGroupName Default-SQL-WestCentralUS -ServerName trgrie-ltr-server | Get-AzureRmSqlDatabaseLongTermRetentionPolicy -Current 

# Get the LTR policy of a specific database 
$ltrPolicies = Get-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName  -ResourceGroupName $resourceGroup -Current
```
### <a name="clear-an-ltr-policy"></a>Limpar uma política LTR
Este exemplo mostra como limpar uma política LTR de um banco de dados

```powershell
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ServerName $serverName -DatabaseName $dbName -ResourceGroupName $resourceGroup -RemovePolicy
```

### <a name="view-ltr-backups"></a>Exibir backups LTR

Este exemplo mostra como listar os backups LTR dentro de um servidor. 

```powershell
# Get the list of all LTR backups in a specific Azure region 
# The backups are grouped by the logical database id.
# Within each group they are ordered by the timestamp, the earliest
# backup first.  
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location 

# Get the list of LTR backups from the Azure region under 
# the named server. 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName

# Get the LTR backups for a specific database from the Azure region under the named server 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName -DatabaseName $dbName

# List LTR backups only from live databases (you have option to choose All/Live/Deleted)
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -DatabaseState Live

# Only list the latest LTR backup for each database 
$ltrBackups = Get-AzureRmSqlDatabaseLongTermRetentionBackup -LocationName $server.Location -ServerName $serverName -OnlyLatestPerDatabase
```

### <a name="delete-ltr-backups"></a>Excluir backups LTR

Este exemplo mostra como excluir um backup LTR da lista de backups.

```powershell
# remove the earliest backup 
$ltrBackup = $ltrBackups[0]
Remove-AzureRmSqlDatabaseLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId
```

### <a name="restore-from-ltr-backups"></a>Restaurar a partir de backups LTR
Este exemplo mostra como restaurar a partir de um backup LTR. Observe que esta interface não foi alterada, mas o parâmetro de identificação do recurso agora requer a identificação do recurso do backup LTR. 

```powershell
# Restore LTR backup as an S3 database
Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $ltrBackup.ResourceId -ServerName $serverName -ResourceGroupName $resourceGroup -TargetDatabaseName $dbName -ServiceObjectiveName S3
```

> [!NOTE]
> A partir daqui, você pode conectar o banco de dados restaurado usando o SQL Server Management Studio para executar as tarefas necessárias, tais como, extrair um pouco de dados do banco de dados restaurado para copiar para o banco de dados existente ou excluir o banco de dados existente e renomear o banco de dados restaurado com o nome do banco de dados existente. Confira [recuperação pontual](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Próximas etapas

- Para saber mais sobre backups automáticos gerados pelo serviço, veja [backups automáticos](sql-database-automated-backups.md)
- Para saber mais sobre a retenção de backup de longo prazo, consulte [retenção de backup de longo prazo](sql-database-long-term-retention.md)
