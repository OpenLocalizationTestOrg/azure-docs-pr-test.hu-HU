---
title: "Hosszú távú biztonsági másolatok megőrzésének - Azure SQL adatbázis konfigurálása |} Microsoft Docs"
description: "Ismerje meg, hogyan toostore automatikus hello Azure Recovery Services-tároló és az Azure Recovery Services-tároló hello toorestore a biztonsági másolatok"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: carlrab
ms.openlocfilehash: 603f4dd21cee4407d46f749655aba8f9ef3322c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention"></a>Konfigurálja és az Azure SQL Database hosszú távú biztonsági mentés megőrzési visszaállítás

Hello Azure Recovery Services tároló toostore Azure SQL adatbázis biztonsági másolatait is beállíthat, és egy adatbázis biztonsági mentések használatával őrzi meg a hello tárolót használ majd helyreállítás hello Azure-portálon vagy a PowerShell használatával.

## <a name="azure-portal"></a>Azure Portal

a következő szakaszok megjelenítése hogyan toouse hello Azure portál tooconfigure hello Azure Recovery Services használó, megtekinthető biztonsági mentések hello tárolóban, és állítsa vissza a hello tárolójából hello.

### <a name="configure-hello-vault-register-hello-server-and-select-databases"></a>Hello tároló konfigurálása hello kiszolgáló regisztrálásához és adatbázisok kiválasztása

Ön [egy Azure Recovery Services-tároló tooretain automatikus biztonsági mentések konfigurálása](sql-database-long-term-retention.md) a szolgáltatási rétegben hello megőrzési időszaknál hosszabb időtartamra. 

1. Nyissa meg hello **SQL Server** a kiszolgálóhoz tartozó lapon.

   ![SQL server lap](./media/sql-database-get-started-portal/sql-server-blade.png)

2. Kattintson a **Biztonsági másolat hosszú távú megőrzése** elemre.

   ![biztonsági másolat hosszú távú megőrzése hivatkozás](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. A hello **hosszú távú biztonsági másolatok megőrzésének** a kiszolgáló lapon tekintse meg és fogadja el a hello preview feltételeket (kivéve, ha Ön már megtette -, vagy ez a szolgáltatás már nincs az előzetes verzió).

   ![hello preview feltételek elfogadása](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. hosszú távú biztonsági másolatok megőrzésének tooconfigure, válassza ki, hogy az adatbázis hello rácsban, és kattintson **konfigurálása** hello eszköztáron.

   ![adatbázis kijelölése biztonsági mentés hosszú távú megőrzéséhez](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. A hello **konfigurálása** kattintson **kötelező beállítások konfigurálása** alatt **helyreállítási szolgáltatás tároló**.

   ![tároló konfigurálása hivatkozás](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. A hello **Recovery services-tároló** lapon, válassza ki egy meglévő tárolóban, ha van ilyen. Ha nincs recovery services-tároló az előfizetés található, ellenkező esetben tooexit hello folyamata kattintson, és a recovery services-tároló létrehozása.

   ![tároló hivatkozás létrehozása](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. A hello **Recovery Services-tárolók** kattintson **Hozzáadás**.

   ![tároló hivatkozás hozzáadása](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. A hello **Recovery Services-tároló** lapján hello Recovery Services-tároló meg egy érvényes nevet.

   ![új tároló neve](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Válassza ki az előfizetését és erőforráscsoportot, majd válassza ki a hello hello tároló helyét. Ha befejezte, kattintson a **Létrehozás** elemre.

   ![tároló létrehozása](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > hello tárolóban kell lennie a hello és hello Azure SQL logikai kiszolgálóra ugyanabban a régióban, és kell használata hello azonos hello logikai kiszolgáló erőforráscsoportjában.
   >

10. Hello új tároló létrehozása után hajtható végre hello szükséges lépéseket tooreturn toohello **Recovery services-tároló** lap.

11. A hello **Recovery services-tároló** hello tároló lapon, majd kattintson a gombra **válasszon**.

   ![meglévő tároló kiválasztása](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. A hello **konfigurálása** lapon adjon meg egy érvényes nevet hello új megőrzési házirend, módosítsa hello alapértelmezett megőrzési házirendet szükség szerint, és kattintson a **OK**.

   ![adatmegőrzési házirend definiálása](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. A hello **hosszú távú biztonsági másolatok megőrzésének** az adatbázis lapján kattintson **mentése** majd **OK** tooapply hello hosszú távú biztonsági mentés megőrzési házirend tooall kijelölt adatbázisok.

   ![adatmegőrzési házirend definiálása](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Kattintson a **mentése** tooenable hosszú távú biztonsági másolatok megőrzésének az új házirend toohello Azure Recovery Services-tároló konfigurált használatával.

   ![adatmegőrzési házirend definiálása](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> Beállítása után biztonsági másolatok jelennek meg az hello tároló következő hét napban. Ez az oktatóanyag nem folytatható, amíg a biztonsági mentések hello tárolóban lévő állapottal jelenik meg.
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a>Az Azure-portált használja, a hosszú távú megőrzési biztonsági másolatok megtekintése

Tekintse meg az adatbázis a biztonsági másolatok [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md). 

1. A hello Azure-portálon, nyissa meg az Azure Recovery Services-tároló az adatbázis biztonsági mentés (Ugrás túl**összes erőforrás** , és jelölje ki az előfizetéshez tartozó erőforrásokat hello lista) tooview hello mennyisége az adatbázis által használt tároló a biztonsági másolatok hello tárolóban.

   ![biztonsági másolatokat tartalmazó helyreállítási tár megtekintése](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Nyissa meg hello **SQL-adatbázis** lap az adatbázis számára.

   ![Új minta db lap](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. Hello eszköztáron kattintson **visszaállítása**.

   ![visszaállítási eszköztár](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. Hello visszaállítási lapján kattintson a **hosszú távú**.

5. Az Azure tároló biztonsági mentést, kattintson **válassza ki a biztonsági másolatot** tooview hello elérhető adatbázis a biztonsági másolatok hosszú távú biztonsági másolatok megőrzésének.

   ![biztonsági másolatok a tárolóban](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-hello-azure-portal"></a>Állítson vissza egy adatbázist egy biztonsági másolatból, a hosszú távú biztonsági másolatok megőrzésének hello Azure-portál használatával

Az Azure Recovery Services-tároló hello másolatból állítsa vissza a hello tooa új adatbázist.

1. A hello **tárolóban az Azure biztonsági mentések** hello biztonsági mentési toorestore lapon, majd kattintson a gombra **válasszon**.

   ![tárolóban lévő biztonsági másolat kiválasztása](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. A hello **adatbázisnév** szöveg hello visszaállított adatbázis hello nevét adja meg.

   ![új adatbázis neve](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Kattintson a **OK** toorestore hello másolatból hello tároló toohello új adatbázis az adatbázis.

4. Hello eszköztáron kattintson a hello értesítési ikon tooview hello hello visszaállítási feladat állapota.

   ![tárolóból való visszaállítási feladat állapota](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Hello visszaállítási feladat befejezése után nyissa meg a hello **SQL-adatbázisok** lap tooview hello újonnan visszaállított adatbázis.

   ![tárolóból visszaállított adatbázis](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> Itt csatlakoztathatja toohello visszaállított adatbázis használata az SQL Server Management Studio tooperform szükséges feladatok, például a túl[hello visszaállított adatbázis toocopy hello meglévő adatbázis vagy toodelete hello meglévő bit típust az adatok kinyerése adatbázis és az Átnevezés hello visszaállított adatbázis toohello meglévő adatbázis neve](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="powershell"></a>PowerShell

a következő szakaszok hello megtudhatja, hogyan toouse PowerShell tooconfigure hello Azure Recovery Services-tároló, biztonsági mentések megtekintéséhez hello tárolóban, és állítsa vissza a hello tárolójából.

### <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása

Használjon hello [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) toocreate egy helyreállítási szolgáltatások tárolóban.

> [!IMPORTANT]
> hello tárolóban kell lennie a hello és hello Azure SQL logikai kiszolgálóra ugyanabban a régióban, és kell használata hello azonos hello logikai kiszolgáló erőforráscsoportjában.

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-toouse-hello-recovery-vault-for-its-long-term-retention-backups"></a>A kiszolgáló toouse hello recovery-tároló megadása a hosszú távú megőrzési biztonsági mentések

Használjon hello [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) parancsmag tooassociate egy korábban létrehozott helyreállítási szolgáltatások tároló egy adott Azure SQL-kiszolgálóval.

```PowerShell
# Set your server toouse hello vault toofor long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Adatmegőrzési házirend létrehozása

Adatmegőrzési, amelyen beállíthatja mennyi ideig tookeep egy adatbázis biztonsági mentése. Használjon hello [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) parancsmag tooget hello alapértelmezett megőrzési házirend toouse hello sablonként szabályzatok létrehozására. Ez a sablon a hello megőrzési időszak 2 év van beállítva. Ezután futtassa az hello [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) toofinally hello-szabályzat létrehozása. 

> [!NOTE]
> Néhány parancsmaggal szükséges, hogy állítsa a hello tárolóban helyi futtatása előtt ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)), ez a parancsmag a néhány kapcsolódó kódtöredékek látja. Hello környezetben akkor állítható be, mert hello házirend hello tároló része. Hozzon létre több adatmegőrzési házirend minden egyes tároló, és ezután alkalmazza a szükséges hello házirend toospecific adatbázisok. 


```PowerShell
# Retrieve hello default retention policy for hello AzureSQLDatabase workload type
$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set hello retention value tootwo years (you can set tooany time between 1 week and 10 years)
$retentionPolicy.RetentionDurationType = "Years"
$retentionPolicy.RetentionCount = 2
$retentionPolicyName = "my2YearRetentionPolicy"

# Set hello vault context toohello vault you are creating hello policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create hello new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy
```

### <a name="configure-a-database-toouse-hello-previously-defined-retention-policy"></a>Adatbázis toouse korábban definiált hello megőrzési házirend konfigurálása

Használjon hello [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) parancsmag tooapply hello új házirend tooa adott adatbázist.

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a>Biztonsági másolatok és adataik megtekintése hosszú távú megőrzés alatt

Tekintse meg az adatbázis a biztonsági másolatok [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md). 

A következő parancsmagok tooview biztonsági mentési információt hello használata:

- [Get-AzureRmRecoveryServicesBackupContainer](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [Get-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [Get-AzureRmRecoveryServicesBackupRecoveryPoint](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

```PowerShell
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$databaseNeedingRestore = $databaseName

# Set hello vault context toohello vault we want toorestore from
#$vault = Get-AzureRmRecoveryServicesVault -ResourceGroupName $resourceGroupName
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# hello following commands find hello container associated with hello server 'myserver' under resource group 'myresourcegroup'
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get hello long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore

# Get all available backups for hello previously indicated database
# Optionally, set hello -StartDate and -EndDate parameters tooreturn backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
```

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Adatbázis visszaállítása hosszú távú megőrzés alatt álló biztonsági másolatból

A hosszú távú biztonsági mentés megőrzési visszaállítás használ hello [visszaállítási-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) parancsmag.

```PowerShell
# Restore hello most recent backup: $availableBackups[0]
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$restoredDatabaseName = "{new-database-name}"
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDb = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel
$restoredDb
```


> [!NOTE]
> Itt vissza toohello adatbázist az SQL Server Management Studio tooperform szükséges feladatok használatával is elérheti, például a tooextract bit típust hello származó adatok helyre adatbázis toocopy hello meglévő adatbázis vagy toodelete hello meglévő adatbázist, és nevezze át hello visszaállított adatbázis toohello meglévő adatbázis neve. Lásd: [időponthoz kötött visszaállításra](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Következő lépések

- toolearn szolgáltatás által létrehozott automatikus biztonsági mentésekhez, kapcsolatban lásd: [automatikus biztonsági mentés](sql-database-automated-backups.md)
- toolearn vonatkozó hosszú távú biztonsági másolatok megőrzésének, lásd: [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md)
- toolearn visszaállítása biztonsági másolatból, lásd: [állítsa vissza biztonsági másolatból](sql-database-recovery-using-backups.md)
