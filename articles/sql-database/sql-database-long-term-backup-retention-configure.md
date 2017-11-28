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
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention"></a><span data-ttu-id="a3025-103">Konfigurálja és az Azure SQL Database hosszú távú biztonsági mentés megőrzési visszaállítás</span><span class="sxs-lookup"><span data-stu-id="a3025-103">Configure and restore from Azure SQL Database long-term backup retention</span></span>

<span data-ttu-id="a3025-104">Hello Azure Recovery Services tároló toostore Azure SQL adatbázis biztonsági másolatait is beállíthat, és egy adatbázis biztonsági mentések használatával őrzi meg a hello tárolót használ majd helyreállítás hello Azure-portálon vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="a3025-104">You can configure hello Azure Recovery Services vault toostore Azure SQL database backups and then recover a database using backups retained in hello vault using hello Azure portal or PowerShell.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="a3025-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a3025-105">Azure portal</span></span>

<span data-ttu-id="a3025-106">a következő szakaszok megjelenítése hogyan toouse hello Azure portál tooconfigure hello Azure Recovery Services használó, megtekinthető biztonsági mentések hello tárolóban, és állítsa vissza a hello tárolójából hello.</span><span class="sxs-lookup"><span data-stu-id="a3025-106">hello following sections show you how toouse hello Azure portal tooconfigure hello Azure Recovery Services vault, view backups in hello vault, and restore from hello vault.</span></span>

### <a name="configure-hello-vault-register-hello-server-and-select-databases"></a><span data-ttu-id="a3025-107">Hello tároló konfigurálása hello kiszolgáló regisztrálásához és adatbázisok kiválasztása</span><span class="sxs-lookup"><span data-stu-id="a3025-107">Configure hello vault, register hello server, and select databases</span></span>

<span data-ttu-id="a3025-108">Ön [egy Azure Recovery Services-tároló tooretain automatikus biztonsági mentések konfigurálása](sql-database-long-term-retention.md) a szolgáltatási rétegben hello megőrzési időszaknál hosszabb időtartamra.</span><span class="sxs-lookup"><span data-stu-id="a3025-108">You [configure an Azure Recovery Services vault tooretain automated backups](sql-database-long-term-retention.md) for a period longer than hello retention period for your service tier.</span></span> 

1. <span data-ttu-id="a3025-109">Nyissa meg hello **SQL Server** a kiszolgálóhoz tartozó lapon.</span><span class="sxs-lookup"><span data-stu-id="a3025-109">Open hello **SQL Server** page for your server.</span></span>

   ![SQL server lap](./media/sql-database-get-started-portal/sql-server-blade.png)

2. <span data-ttu-id="a3025-111">Kattintson a **Biztonsági másolat hosszú távú megőrzése** elemre.</span><span class="sxs-lookup"><span data-stu-id="a3025-111">Click **Long-term backup retention**.</span></span>

   ![biztonsági másolat hosszú távú megőrzése hivatkozás](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. <span data-ttu-id="a3025-113">A hello **hosszú távú biztonsági másolatok megőrzésének** a kiszolgáló lapon tekintse meg és fogadja el a hello preview feltételeket (kivéve, ha Ön már megtette -, vagy ez a szolgáltatás már nincs az előzetes verzió).</span><span class="sxs-lookup"><span data-stu-id="a3025-113">On hello **Long-term backup retention** page for your server, review and accept hello preview terms (unless you have already done so - or this feature is no longer in preview).</span></span>

   ![hello preview feltételek elfogadása](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. <span data-ttu-id="a3025-115">hosszú távú biztonsági másolatok megőrzésének tooconfigure, válassza ki, hogy az adatbázis hello rácsban, és kattintson **konfigurálása** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="a3025-115">tooconfigure long-term backup retention, select that database in hello grid and then click **Configure** on hello toolbar.</span></span>

   ![adatbázis kijelölése biztonsági mentés hosszú távú megőrzéséhez](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. <span data-ttu-id="a3025-117">A hello **konfigurálása** kattintson **kötelező beállítások konfigurálása** alatt **helyreállítási szolgáltatás tároló**.</span><span class="sxs-lookup"><span data-stu-id="a3025-117">On hello **Configure** page, click **Configure required settings** under **Recovery service vault**.</span></span>

   ![tároló konfigurálása hivatkozás](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. <span data-ttu-id="a3025-119">A hello **Recovery services-tároló** lapon, válassza ki egy meglévő tárolóban, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="a3025-119">On hello **Recovery services vault** page, select an existing vault, if any.</span></span> <span data-ttu-id="a3025-120">Ha nincs recovery services-tároló az előfizetés található, ellenkező esetben tooexit hello folyamata kattintson, és a recovery services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a3025-120">Otherwise, if no recovery services vault found for your subscription, click tooexit hello flow and create a recovery services vault.</span></span>

   ![tároló hivatkozás létrehozása](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. <span data-ttu-id="a3025-122">A hello **Recovery Services-tárolók** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a3025-122">On hello **Recovery Services vaults** page, click **Add**.</span></span>

   ![tároló hivatkozás hozzáadása](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. <span data-ttu-id="a3025-124">A hello **Recovery Services-tároló** lapján hello Recovery Services-tároló meg egy érvényes nevet.</span><span class="sxs-lookup"><span data-stu-id="a3025-124">On hello **Recovery Services vault** page, provide a valid name for hello Recovery Services vault.</span></span>

   ![új tároló neve](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. <span data-ttu-id="a3025-126">Válassza ki az előfizetését és erőforráscsoportot, majd válassza ki a hello hello tároló helyét.</span><span class="sxs-lookup"><span data-stu-id="a3025-126">Select your subscription and resource group, and then select hello location for hello vault.</span></span> <span data-ttu-id="a3025-127">Ha befejezte, kattintson a **Létrehozás** elemre.</span><span class="sxs-lookup"><span data-stu-id="a3025-127">When done, click **Create**.</span></span>

   ![tároló létrehozása](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > <span data-ttu-id="a3025-129">hello tárolóban kell lennie a hello és hello Azure SQL logikai kiszolgálóra ugyanabban a régióban, és kell használata hello azonos hello logikai kiszolgáló erőforráscsoportjában.</span><span class="sxs-lookup"><span data-stu-id="a3025-129">hello vault must be located in hello same region as hello Azure SQL logical server, and must use hello same resource group as hello logical server.</span></span>
   >

10. <span data-ttu-id="a3025-130">Hello új tároló létrehozása után hajtható végre hello szükséges lépéseket tooreturn toohello **Recovery services-tároló** lap.</span><span class="sxs-lookup"><span data-stu-id="a3025-130">After hello new vault is created, execute hello necessary steps tooreturn toohello **Recovery services vault** page.</span></span>

11. <span data-ttu-id="a3025-131">A hello **Recovery services-tároló** hello tároló lapon, majd kattintson a gombra **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="a3025-131">On hello **Recovery services vault** page, click hello vault and then click **Select**.</span></span>

   ![meglévő tároló kiválasztása](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. <span data-ttu-id="a3025-133">A hello **konfigurálása** lapon adjon meg egy érvényes nevet hello új megőrzési házirend, módosítsa hello alapértelmezett megőrzési házirendet szükség szerint, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="a3025-133">On hello **Configure** page, provide a valid name for hello new retention policy, modify hello default retention policy as appropriate, and then click **OK**.</span></span>

   ![adatmegőrzési házirend definiálása](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. <span data-ttu-id="a3025-135">A hello **hosszú távú biztonsági másolatok megőrzésének** az adatbázis lapján kattintson **mentése** majd **OK** tooapply hello hosszú távú biztonsági mentés megőrzési házirend tooall kijelölt adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="a3025-135">On hello **Long-term backup retention** page for your database, click **Save** and then click **OK** tooapply hello long-term backup retention policy tooall selected databases.</span></span>

   ![adatmegőrzési házirend definiálása](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. <span data-ttu-id="a3025-137">Kattintson a **mentése** tooenable hosszú távú biztonsági másolatok megőrzésének az új házirend toohello Azure Recovery Services-tároló konfigurált használatával.</span><span class="sxs-lookup"><span data-stu-id="a3025-137">Click **Save** tooenable long-term backup retention using this new policy toohello Azure Recovery Services vault that you configured.</span></span>

   ![adatmegőrzési házirend definiálása](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> <span data-ttu-id="a3025-139">Beállítása után biztonsági másolatok jelennek meg az hello tároló következő hét napban.</span><span class="sxs-lookup"><span data-stu-id="a3025-139">Once configured, backups show up in hello vault within next seven days.</span></span> <span data-ttu-id="a3025-140">Ez az oktatóanyag nem folytatható, amíg a biztonsági mentések hello tárolóban lévő állapottal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a3025-140">Do not continue this tutorial until backups show up in hello vault.</span></span>
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a><span data-ttu-id="a3025-141">Az Azure-portált használja, a hosszú távú megőrzési biztonsági másolatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="a3025-141">View backups in long-term retention using Azure portal</span></span>

<span data-ttu-id="a3025-142">Tekintse meg az adatbázis a biztonsági másolatok [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md).</span><span class="sxs-lookup"><span data-stu-id="a3025-142">View information about your database backups in [long-term backup retention](sql-database-long-term-retention.md).</span></span> 

1. <span data-ttu-id="a3025-143">A hello Azure-portálon, nyissa meg az Azure Recovery Services-tároló az adatbázis biztonsági mentés (Ugrás túl**összes erőforrás** , és jelölje ki az előfizetéshez tartozó erőforrásokat hello lista) tooview hello mennyisége az adatbázis által használt tároló a biztonsági másolatok hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a3025-143">In hello Azure portal, open your Azure Recovery Services vault for your database backups (go too**All resources** and select it from hello list of resources for your subscription) tooview hello amount of storage used by your database backups in hello vault.</span></span>

   ![biztonsági másolatokat tartalmazó helyreállítási tár megtekintése](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. <span data-ttu-id="a3025-145">Nyissa meg hello **SQL-adatbázis** lap az adatbázis számára.</span><span class="sxs-lookup"><span data-stu-id="a3025-145">Open hello **SQL database** page for your database.</span></span>

   ![Új minta db lap](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. <span data-ttu-id="a3025-147">Hello eszköztáron kattintson **visszaállítása**.</span><span class="sxs-lookup"><span data-stu-id="a3025-147">On hello toolbar, click **Restore**.</span></span>

   ![visszaállítási eszköztár](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. <span data-ttu-id="a3025-149">Hello visszaállítási lapján kattintson a **hosszú távú**.</span><span class="sxs-lookup"><span data-stu-id="a3025-149">On hello Restore page, click **Long-term**.</span></span>

5. <span data-ttu-id="a3025-150">Az Azure tároló biztonsági mentést, kattintson **válassza ki a biztonsági másolatot** tooview hello elérhető adatbázis a biztonsági másolatok hosszú távú biztonsági másolatok megőrzésének.</span><span class="sxs-lookup"><span data-stu-id="a3025-150">Under Azure vault backups, click **Select a backup** tooview hello available database backups in long-term backup retention.</span></span>

   ![biztonsági másolatok a tárolóban](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-hello-azure-portal"></a><span data-ttu-id="a3025-152">Állítson vissza egy adatbázist egy biztonsági másolatból, a hosszú távú biztonsági másolatok megőrzésének hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="a3025-152">Restore a database from a backup in long-term backup retention using hello Azure portal</span></span>

<span data-ttu-id="a3025-153">Az Azure Recovery Services-tároló hello másolatból állítsa vissza a hello tooa új adatbázist.</span><span class="sxs-lookup"><span data-stu-id="a3025-153">You restore hello database tooa new database from a backup in hello Azure Recovery Services vault.</span></span>

1. <span data-ttu-id="a3025-154">A hello **tárolóban az Azure biztonsági mentések** hello biztonsági mentési toorestore lapon, majd kattintson a gombra **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="a3025-154">On hello **Azure vault backups** page, click hello backup toorestore and then click **Select**.</span></span>

   ![tárolóban lévő biztonsági másolat kiválasztása](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. <span data-ttu-id="a3025-156">A hello **adatbázisnév** szöveg hello visszaállított adatbázis hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="a3025-156">In hello **Database name** text box, provide hello name for hello restored database.</span></span>

   ![új adatbázis neve](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. <span data-ttu-id="a3025-158">Kattintson a **OK** toorestore hello másolatból hello tároló toohello új adatbázis az adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a3025-158">Click **OK** toorestore your database from hello backup in hello vault toohello new database.</span></span>

4. <span data-ttu-id="a3025-159">Hello eszköztáron kattintson a hello értesítési ikon tooview hello hello visszaállítási feladat állapota.</span><span class="sxs-lookup"><span data-stu-id="a3025-159">On hello toolbar, click hello notification icon tooview hello status of hello restore job.</span></span>

   ![tárolóból való visszaállítási feladat állapota](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. <span data-ttu-id="a3025-161">Hello visszaállítási feladat befejezése után nyissa meg a hello **SQL-adatbázisok** lap tooview hello újonnan visszaállított adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a3025-161">When hello restore job is completed, open hello **SQL databases** page tooview hello newly restored database.</span></span>

   ![tárolóból visszaállított adatbázis](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> <span data-ttu-id="a3025-163">Itt csatlakoztathatja toohello visszaállított adatbázis használata az SQL Server Management Studio tooperform szükséges feladatok, például a túl[hello visszaállított adatbázis toocopy hello meglévő adatbázis vagy toodelete hello meglévő bit típust az adatok kinyerése adatbázis és az Átnevezés hello visszaállított adatbázis toohello meglévő adatbázis neve](sql-database-recovery-using-backups.md#point-in-time-restore).</span><span class="sxs-lookup"><span data-stu-id="a3025-163">From here, you can connect toohello restored database using SQL Server Management Studio tooperform needed tasks, such as too[extract a bit of data from hello restored database toocopy into hello existing database or toodelete hello existing database and rename hello restored database toohello existing database name](sql-database-recovery-using-backups.md#point-in-time-restore).</span></span>
>

## <a name="powershell"></a><span data-ttu-id="a3025-164">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a3025-164">PowerShell</span></span>

<span data-ttu-id="a3025-165">a következő szakaszok hello megtudhatja, hogyan toouse PowerShell tooconfigure hello Azure Recovery Services-tároló, biztonsági mentések megtekintéséhez hello tárolóban, és állítsa vissza a hello tárolójából.</span><span class="sxs-lookup"><span data-stu-id="a3025-165">hello following sections show you how toouse PowerShell tooconfigure hello Azure Recovery Services vault, view backups in hello vault, and restore from hello vault.</span></span>

### <a name="create-a-recovery-services-vault"></a><span data-ttu-id="a3025-166">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3025-166">Create a recovery services vault</span></span>

<span data-ttu-id="a3025-167">Használjon hello [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) toocreate egy helyreállítási szolgáltatások tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a3025-167">Use hello [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) toocreate a recovery services vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a3025-168">hello tárolóban kell lennie a hello és hello Azure SQL logikai kiszolgálóra ugyanabban a régióban, és kell használata hello azonos hello logikai kiszolgáló erőforráscsoportjában.</span><span class="sxs-lookup"><span data-stu-id="a3025-168">hello vault must be located in hello same region as hello Azure SQL logical server, and must use hello same resource group as hello logical server.</span></span>

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-toouse-hello-recovery-vault-for-its-long-term-retention-backups"></a><span data-ttu-id="a3025-169">A kiszolgáló toouse hello recovery-tároló megadása a hosszú távú megőrzési biztonsági mentések</span><span class="sxs-lookup"><span data-stu-id="a3025-169">Set your server toouse hello recovery vault for its long-term retention backups</span></span>

<span data-ttu-id="a3025-170">Használjon hello [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) parancsmag tooassociate egy korábban létrehozott helyreállítási szolgáltatások tároló egy adott Azure SQL-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="a3025-170">Use hello [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) cmdlet tooassociate a previously created recovery services vault with a specific Azure SQL server.</span></span>

```PowerShell
# Set your server toouse hello vault toofor long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a><span data-ttu-id="a3025-171">Adatmegőrzési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="a3025-171">Create a retention policy</span></span>

<span data-ttu-id="a3025-172">Adatmegőrzési, amelyen beállíthatja mennyi ideig tookeep egy adatbázis biztonsági mentése.</span><span class="sxs-lookup"><span data-stu-id="a3025-172">A retention policy is where you set how long tookeep a database backup.</span></span> <span data-ttu-id="a3025-173">Használjon hello [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) parancsmag tooget hello alapértelmezett megőrzési házirend toouse hello sablonként szabályzatok létrehozására.</span><span class="sxs-lookup"><span data-stu-id="a3025-173">Use hello [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) cmdlet tooget hello default retention policy toouse as hello template for creating policies.</span></span> <span data-ttu-id="a3025-174">Ez a sablon a hello megőrzési időszak 2 év van beállítva.</span><span class="sxs-lookup"><span data-stu-id="a3025-174">In this template, hello retention period is set for 2 years.</span></span> <span data-ttu-id="a3025-175">Ezután futtassa az hello [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) toofinally hello-szabályzat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a3025-175">Next, run hello [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) toofinally create hello policy.</span></span> 

> [!NOTE]
> <span data-ttu-id="a3025-176">Néhány parancsmaggal szükséges, hogy állítsa a hello tárolóban helyi futtatása előtt ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)), ez a parancsmag a néhány kapcsolódó kódtöredékek látja.</span><span class="sxs-lookup"><span data-stu-id="a3025-176">Some cmdlets require that you set hello vault context before running ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) so you see this cmdlet in a few related snippets.</span></span> <span data-ttu-id="a3025-177">Hello környezetben akkor állítható be, mert hello házirend hello tároló része.</span><span class="sxs-lookup"><span data-stu-id="a3025-177">You set hello context because hello policy is part of hello vault.</span></span> <span data-ttu-id="a3025-178">Hozzon létre több adatmegőrzési házirend minden egyes tároló, és ezután alkalmazza a szükséges hello házirend toospecific adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="a3025-178">You can create multiple retention policies for each vault and then apply hello desired policy toospecific databases.</span></span> 


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

### <a name="configure-a-database-toouse-hello-previously-defined-retention-policy"></a><span data-ttu-id="a3025-179">Adatbázis toouse korábban definiált hello megőrzési házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a3025-179">Configure a database toouse hello previously defined retention policy</span></span>

<span data-ttu-id="a3025-180">Használjon hello [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) parancsmag tooapply hello új házirend tooa adott adatbázist.</span><span class="sxs-lookup"><span data-stu-id="a3025-180">Use hello [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) cmdlet tooapply hello new policy tooa specific database.</span></span>

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a><span data-ttu-id="a3025-181">Biztonsági másolatok és adataik megtekintése hosszú távú megőrzés alatt</span><span class="sxs-lookup"><span data-stu-id="a3025-181">View backup info, and backups in long-term retention</span></span>

<span data-ttu-id="a3025-182">Tekintse meg az adatbázis a biztonsági másolatok [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md).</span><span class="sxs-lookup"><span data-stu-id="a3025-182">View information about your database backups in [long-term backup retention](sql-database-long-term-retention.md).</span></span> 

<span data-ttu-id="a3025-183">A következő parancsmagok tooview biztonsági mentési információt hello használata:</span><span class="sxs-lookup"><span data-stu-id="a3025-183">Use hello following cmdlets tooview backup information:</span></span>

- [<span data-ttu-id="a3025-184">Get-AzureRmRecoveryServicesBackupContainer</span><span class="sxs-lookup"><span data-stu-id="a3025-184">Get-AzureRmRecoveryServicesBackupContainer</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [<span data-ttu-id="a3025-185">Get-AzureRmRecoveryServicesBackupItem</span><span class="sxs-lookup"><span data-stu-id="a3025-185">Get-AzureRmRecoveryServicesBackupItem</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [<span data-ttu-id="a3025-186">Get-AzureRmRecoveryServicesBackupRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="a3025-186">Get-AzureRmRecoveryServicesBackupRecoveryPoint</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

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

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a><span data-ttu-id="a3025-187">Adatbázis visszaállítása hosszú távú megőrzés alatt álló biztonsági másolatból</span><span class="sxs-lookup"><span data-stu-id="a3025-187">Restore a database from a backup in long-term backup retention</span></span>

<span data-ttu-id="a3025-188">A hosszú távú biztonsági mentés megőrzési visszaállítás használ hello [visszaállítási-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="a3025-188">Restoring from long-term backup retention uses hello [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet.</span></span>

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
> <span data-ttu-id="a3025-189">Itt vissza toohello adatbázist az SQL Server Management Studio tooperform szükséges feladatok használatával is elérheti, például a tooextract bit típust hello származó adatok helyre adatbázis toocopy hello meglévő adatbázis vagy toodelete hello meglévő adatbázist, és nevezze át hello visszaállított adatbázis toohello meglévő adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="a3025-189">From here, you can connect toohello restored database using SQL Server Management Studio tooperform needed tasks, such as tooextract a bit of data from hello restored database toocopy into hello existing database or toodelete hello existing database and rename hello restored database toohello existing database name.</span></span> <span data-ttu-id="a3025-190">Lásd: [időponthoz kötött visszaállításra](sql-database-recovery-using-backups.md#point-in-time-restore).</span><span class="sxs-lookup"><span data-stu-id="a3025-190">See [point in time restore](sql-database-recovery-using-backups.md#point-in-time-restore).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a3025-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3025-191">Next steps</span></span>

- <span data-ttu-id="a3025-192">toolearn szolgáltatás által létrehozott automatikus biztonsági mentésekhez, kapcsolatban lásd: [automatikus biztonsági mentés](sql-database-automated-backups.md)</span><span class="sxs-lookup"><span data-stu-id="a3025-192">toolearn about service-generated automatic backups, see [automatic backups](sql-database-automated-backups.md)</span></span>
- <span data-ttu-id="a3025-193">toolearn vonatkozó hosszú távú biztonsági másolatok megőrzésének, lásd: [hosszú távú biztonsági másolatok megőrzésének](sql-database-long-term-retention.md)</span><span class="sxs-lookup"><span data-stu-id="a3025-193">toolearn about long-term backup retention, see [long-term backup retention](sql-database-long-term-retention.md)</span></span>
- <span data-ttu-id="a3025-194">toolearn visszaállítása biztonsági másolatból, lásd: [állítsa vissza biztonsági másolatból](sql-database-recovery-using-backups.md)</span><span class="sxs-lookup"><span data-stu-id="a3025-194">toolearn about restoring from backups, see [restore from backup](sql-database-recovery-using-backups.md)</span></span>
