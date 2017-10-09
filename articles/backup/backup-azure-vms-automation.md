---
title: "aaaDeploy és erőforrás-kezelő telepített virtuális gépek PowerShell használatával kezelheti a biztonsági mentések |} Microsoft Docs"
description: "PowerShell toodeploy használja, és a Resource Manager telepített virtuális gépek az Azure biztonsági mentések kezelése"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 68606e4f-536d-4eac-9f80-8a198ea94d52
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/28/2017
ms.author: markgal;trinadhk
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 486fb3ae1902403fe6bf303df57244b76677ab17
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-tooback-up-virtual-machines"></a>Virtuális gépek AzureRM.RecoveryServices.Backup parancsmagok tooback használata
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-vms-automation.md)
> * [Klasszikus](backup-azure-vms-classic-automation.md)
>
>

Ez a cikk bemutatja, hogyan toouse Azure PowerShell parancsmagok tooback mentése és helyreállítása egy Azure virtuális gép (VM) a Recovery Services tároló. Recovery Services-tároló egy Azure Resource Manager-erőforrás és használt tooprotect adatok és eszközök is az Azure Backup, és az Azure Site Recovery szolgáltatásban. A Recovery Services-tároló tooprotect Azure Service Manager telepített virtuális gépek és az Azure Resource Manager telepített virtuális gépek is használhatja.

> [!NOTE]
> Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello Resource Manager-modell használatával létrehozott virtuális gépek történő használatra szolgál.
>
>

Ez a cikk bemutatja, hogyan PowerShell tooprotect használatával egy virtuális gép, és a visszaállítási adatok helyreállítási pontból.

## <a name="concepts"></a>Alapelvek
Ha nem ismeri az Azure Backup szolgáltatás hello szolgáltatás áttekintését hello kivétele [Mi az Azure Backup?](backup-introduction-to-azure-backup.md) Megkezdése előtt ellenőrizze, hogy hello essentials hello szükséges előfeltételeket toowork Azure Backup szolgáltatással kapcsolatos foglalkozik, és hello hello aktuális virtuális gép biztonsági mentési megoldásra vonatkozó korlátozások.

toouse PowerShell gyakorlatilag szükség toounderstand hello hierarchia objektumok és honnan toostart.

![Helyreállítási szolgáltatások eltér az objektumhierarchia](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

tooview hello AzureRm.RecoveryServices.Backup PowerShell parancsmag-referencia, lásd: hello [Azure Backup - helyreállítási szolgáltatások parancsmagjai](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) hello Azure könyvtárban található.

## <a name="setup-and-registration"></a>Telepítését és regisztrálását
toobegin:

1. [Hello PowerShell legújabb verziójának letöltése](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (hello szükséges minimális verziója: 1.4.0)
2. Keresse meg a rendelkezésre álló hello Azure biztonsági mentés PowerShell-parancsmagok hello a következő parancs beírásával:

```
PS C:\> Get-Command *azurermrecoveryservices*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmRecoveryServicesBackupItem           1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Disable-AzureRmRecoveryServicesBackupProtection    1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Enable-AzureRmRecoveryServicesBackupProtection     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupContainer         1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupItem              1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupJob               1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupJobDetails        1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupManagementServer  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
Cmdlet          Get-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRMRecoveryServicesBackupRecoveryPoint     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupRetentionPolic... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesBackupSchedulePolicy... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Get-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
Cmdlet          Get-AzureRmRecoveryServicesVaultSettingsFile       1.4.0      AzureRM.RecoveryServices
Cmdlet          New-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          New-AzureRmRecoveryServicesVault                   1.4.0      AzureRM.RecoveryServices
Cmdlet          Remove-AzureRmRecoveryServicesProtectionPolicy     1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Remove-AzureRmRecoveryServicesVault                1.4.0      AzureRM.RecoveryServices
Cmdlet          Restore-AzureRMRecoveryServicesBackupItem          1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Set-AzureRmRecoveryServicesBackupProperties        1.4.0      AzureRM.RecoveryServices
Cmdlet          Set-AzureRmRecoveryServicesBackupProtectionPolicy  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Set-AzureRmRecoveryServicesVaultContext            1.4.0      AzureRM.RecoveryServices
Cmdlet          Stop-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Unregister-AzureRmRecoveryServicesBackupContainer  1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Unregister-AzureRmRecoveryServicesBackupManagem... 1.4.0      AzureRM.RecoveryServices.Backup
Cmdlet          Wait-AzureRmRecoveryServicesBackupJob              1.4.0      AzureRM.RecoveryServices.Backup
```


a következő feladatok hello automatizálható a PowerShell használatával:

* Recovery Services-tároló létrehozása
* Azure-beli virtuális gépek biztonsági mentése
* A biztonsági mentési feladatot indít
* A figyelő egy biztonsági mentési feladat
* Állítsa vissza az Azure virtuális gép

## <a name="create-a-recovery-services-vault"></a>Recovery Services-tároló létrehozása
a lépéseket követve hello vezethet a Recovery Services-tároló létrehozása. Recovery Services-tároló nem egyezik egy biztonsági mentési tárolót.

1. Ha használ Azure Backup a hello először, használnia kell a hello  **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  parancsmag tooregister hello Azure helyreállítási szolgáltató az előfizetéshez.

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. hello Recovery Services-tárolónak egy olyan erőforrás-kezelő erőforrás, ezért meg kell tooplace az erőforráscsoporton belül. Használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy erőforráscsoportot hello  **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**  parancsmag. Erőforráscsoport létrehozásakor adja meg a hello és hello erőforrásnak helyét.  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. Használjon hello  **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)**  parancsmag toocreate hello Recovery Services-tároló. Győződjön meg arról, hogy toospecify hello hello tároló ugyanazon a helyen, mint az erőforráscsoport hello használt.

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. Adja meg a tárolási redundancia toouse; hello típusa használhat [helyileg redundáns tárolás (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) vagy [földrajzi redundáns tárolás (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). hello következő példa bemutatja hello - BackupStorageRedundancy testvault vonatkozó beállítás tooGeoRedundant.

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > Sok Azure biztonsági mentést készítő parancsmagok bemenetként hello Recovery Services-tároló objektum szükséges. Emiatt egy kényelmes toostore hello biztonsági mentést a Recovery Services tároló objektum egy változóban.
   >
   >

## <a name="view-hello-vaults-in-a-subscription"></a>Nézet hello tárolók az előfizetés
Használjon  **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**  ebben az előfizetésben hello összes tárolók tooview hello listája. Ez a parancs toocheck, hogy egy új tároló lett létrehozva, vagy toosee hello hello előfizetésben elérhető tárolók is használhatja.

Hello parancs, a Get-AzureRmRecoveryServicesVault, minden tooview-tárolók hello előfizetésben. hello alábbi példában látható minden egyes tároló megjelenő hello információ.

```
PS C:\> Get-AzureRmRecoveryServicesVault
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```


## <a name="back-up-azure-vms"></a>Azure-beli virtuális gépek biztonsági mentése
A Recovery Services-tároló tooprotect használja a virtuális gépeket. Mielőtt hello adatvédelem hello tároló környezetben (hello típus védett hello tárolóban lévő adatok), és hello védelmi házirend ellenőrzése. hello védelmi házirend hello ütemezés hello biztonsági mentési feladatok futtatásakor, és mennyi ideig őrzi meg minden egyes biztonsági mentési pillanatképet.

### <a name="set-vault-context"></a>Tároló környezet beállítása
Ahhoz, hogy a virtuális gép védelme, használjon  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello tároló környezetben. Miután hello tároló környezet van beállítva, tooall későbbi parancsmagok vonatkozik. hello alábbi mintakód hello tároló környezetben hello tároló *testvault*.

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a>Védelmi házirend létrehozása
Recovery Services-tároló létrehozásakor az alapértelmezett védelem és adatmegőrzési ismét. hello alapértelmezett védelmi házirendet a biztonsági mentési feladatot, a megadott időpontban naponta váltja ki. hello alapértelmezett megőrzési házirend hello napi helyreállítási pont 30 napig őrzi meg. Használhatja a hello alapértelmezett házirend tooquickly a virtuális gép védelmét, és később különböző adatokkal hello házirend szerkesztése.

Használjon  **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**  tooview hello adatvédelmi szabályzatok hello tárolóban lévő állapottal. Ez a parancsmag tooget is használhatja, egy adott házirend, vagy egy munkaterhelés-típushoz tartozó tooview hello házirendek. a következő példa hello lekérdezi munkaterhelésének típusát, AzureVM házirendeket.

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> hello BackupTime mezőt a PowerShellben hello időzónáját UTC. Azonban hello Azure-portálon hello biztonságimásolat-készítési időpont látható, ha hello ideje módosított tooyour helyi időzónára.
>
>

A biztonsági mentési házirenddel legalább egy megőrzési házirend társítva. Adatmegőrzési házirend határozza meg, hány helyreállítási pont tartják után törli a rendszer. Használjon  **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**  tooview hello alapértelmezett megőrzési házirend.  Hasonló módon használhatja  **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)**  tooobtain hello alapértelmezett ütemezés házirend. Hello  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  parancsmag létrehoz egy PowerShell-objektum, amely tartalmazza a biztonsági mentési házirend. hello ütemezés és a megőrzési csoportházirend-objektumokat kell használni, mint a bemeneti toohello  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  parancsmag. hello alábbi példa tárolja hello ütemezés házirend- és hello adatmegőrzési változók. hello példa változók toodefine hello paraméterek használ, a védelmi házirend létrehozásakor *NewPolicy*.

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a>Védelem engedélyezése
Hello védelem biztonsági mentési házirend meghatározása után továbbra is engedélyeznie kell egy elem hello házirend. Használjon  **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**  tooenable védelmet. A védelem engedélyezéséhez szükséges a két objektum – hello és a hello szabályzat. Miután hello házirend társítva hello tárolóban, hello biztonsági mentési munkafolyamat kiváltásakor hello hello házirend-ütemezést meghatározott időpontban.

a következő példa engedélyezi hello elem védelmét, V2VM, a házirend hello NewPolicy hello. az erőforrás-kezelő virtuális gépeken futó nem titkosított tooenable hello védelme

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

tooenable hello védelmet a virtuális gépek (titkosítja BEK és KEK) titkosított, toogive hello Azure Backup szolgáltatás engedély tooread kulcsok és titkos kulcs tárolóból van szüksége.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

tooenable hello védelmet a virtuális gépek (titkosítja BEK csak) titkosítva, a kulcstároló toogive hello Azure Backup szolgáltatás engedély tooread titkok van szüksége.

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> Ha hello Azure Government felhő használ, majd használja hello érték ff281ffe-705c-4f53-9f37-a40e6f2c68f3 hello paraméter **- ServicePrincipalName** a [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) parancsmag .
>
>

Klasszikus virtuális gépekhez

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a>A védelmi házirend módosítása
toomodify hello védelmi házirendje, használjon [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) toomodify hello SchedulePolicy vagy RetentionPolicy objektumot.

hello példa megváltoztatja hello helyreállítási pont megőrzési too365 nap.

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a>A biztonsági mentés
Használhat  **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  tootrigger egy biztonsági mentési feladat. Ha hello kezdeti biztonsági másolatot, akkor egy teljes biztonsági mentés. Azt követő biztonsági mentéseket egy növekményes másolatot igénybe vehet. Lehet, hogy toouse  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello tároló környezetben hello biztonsági mentési feladat elindítása előtt. a következő példa hello azt feltételezi, hogy a tároló környezet beállítása történt.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> hello időzóna hello StartTime és az EndTime megadása mezők PowerShell UTC. Azonban hello idő hello Azure-portálon látható, amikor hello ideje módosított tooyour helyi időzónára.
>
>

## <a name="monitoring-a-backup-job"></a>A biztonsági mentési feladatot figyelése
Figyelheti a hosszú ideig futó műveletek, például a biztonsági mentési feladatok hello Azure portál használata nélkül. egy folyamatban lévő feladat, használjon hello tooget hello állapota  **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**  parancsmag. Ez a parancsmag lekérdezi a biztonsági mentési feladataihoz hello egy adott tárolóban, és, hogy a tároló hello tároló környezetben van megadva. hello következő példa egy folyamatban lévő feladat tömbként hello állapotának beolvasása, és hello állapota a hello $joblist változó.

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Ezek a feladatok befejezésére – ami szükségtelen kód - lekérdezési helyett használja a hello  **[várakozási-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  parancsmag. Ez a parancsmag hello végrehajtási felfüggesztése, amíg hello feladat befejeződik, vagy hello megadott időtúllépési érték elérésekor.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a>Állítsa vissza az Azure virtuális gép
Hello visszaállítását egy virtuális gép hello Azure-portál használatával, és a PowerShell virtuális gépek visszaállítása közötti fő különbség van. A PowerShell-lel, hello visszaállítási művelet be nem fejeződött hello lemezét és konfigurációs adatát hello helyreállítási pont létrehozása után.

> [!NOTE]
> hello visszaállítási művelet nem hoz létre egy virtuális gépet.
>
>

a lemezről, virtuális gép toocreate hello, részben [tárolt lemezekből a virtuális gép létrehozása hello](backup-azure-vms-automation.md#create-a-vm-from-stored-disks). hello lépéseken toorestore egy Azure virtuális Gépen a következők:

* Válassza ki a virtuális gép hello
* A helyreállítási pont kiválasztása
* Hello lemezek visszaállítása
* Hozzon létre virtuális gép hello tárolt lemezekből

hello alábbi ábrán látható hello objektum hierarchia hello RecoveryServicesVault toohello BackupRecoveryPoint le.

![Helyreállítási szolgáltatások eltér az objektumhierarchia BackupContainer megjelenítése](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

toorestore biztonsági mentési adatokat, hello biztonsági másolat elem és hello időpontban adatokat tartalmazó hello helyreállítási pont azonosítása. Használjon hello  **[visszaállítási-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  hello parancsmag toorestore adatait tároló toohello felhasználói fiókhoz.

### <a name="select-hello-vm"></a>Válassza ki a virtuális gép hello
tooget hello PowerShell-objektum, amely azonosítja a jobb oldali hello elem biztonsági mentését, hello tárolóban hello tároló-tól kezdődnek és a hello objektum hierarchia valamennyi alsóbb szintjén módon működnek. virtuális gép, használjon hello hello jelölő tooselect hello tároló  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  parancsmag és a csövön keresztüli adott toohello  **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  parancsmag.

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" –Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem –Container $namedContainer  –WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a>A helyreállítási pont kiválasztása
Használjon hello  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  parancsmag toolist hello biztonsági mentési elem minden helyreállítási pontja. Ezután válasszon ki hello helyreállítási pont toorestore. Ha biztos abban, hogy melyik helyreállítási ponton toouse, a rendszer egy célszerű toochoose hello legutóbbi RecoveryPointType = AppConsistent pont hello listában.

A következő parancsfájl hello, hello változó, **$rp**, a helyreállítási pontok egy tömb van hello kijelölt elem biztonsági mentése hello az elmúlt hét napban. hello tömb fordított sorrendben rendezve idő hello legújabb helyreállítási ponttal 0. indexnél. Használja a következő szabványos PowerShell tömböt indexelő toopick hello helyreállítási pontot. Hello példában $rp [0] hello legutóbbi helyreállítási pontot választja ki.

```
PS C:\> $startDate = (Get-Date).AddDays(-7)
PS C:\> $endDate = Get-Date
PS C:\> $rp = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime()
PS C:\> $rp[0]
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```



### <a name="restore-hello-disks"></a>Hello lemezek visszaállítása
Használjon hello  **[visszaállítási-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  parancsmag toorestore egy biztonsági mentési elem adatait és a konfigurációs tooa helyreállítási pontok. Ha azonosított egy helyreállítási pontot, hello értékét a használni hello **- RecoveryPoint** paraméter. A hello előző példakód **$rp [0]** hello helyreállítási pont toouse volt. Az alábbi kódmintában hello **$rp [0]** hello helyreállítási pont toouse hello lemez visszaállítására van.

toorestore hello lemezét és konfigurációs adatát:

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

Használjon hello  **[várakozási-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  parancsmag toowait hello visszaállítási feladat toocomplete számára.

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

Ha hello visszaállítási feladat befejeződött, a hello  **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**  parancsmag tooget hello részleteit hello visszaállítási műveletet. hello JobDetails tulajdonság hello információ szükséges toorebuild hello virtuális gép van.

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

Miután helyreállította hello lemezek, nyissa meg toohello következő szakasz toocreate hello virtuális gép.

## <a name="create-a-vm-from-restored-disks"></a>Hozzon létre egy virtuális Gépet visszaállított lemezekből
Hello lemezek visszaállítását követően használja ezeket a lépéseket toocreate, és konfigurálja a lemezről hello virtuális gépet.

> [!NOTE]
> toocreate virtuális gépek titkosítása a visszaállított lemezek, a Azure szerepkörnek rendelkeznie kell engedéllyel tooperform hello művelet **Microsoft.KeyVault/vaults/deploy/action**. Ha a szerepkör nem rendelkezik ezzel az engedéllyel, hozzon létre egy egyéni biztonsági szerepkört ezt a műveletet. További információkért lásd: [egyéni szerepkörök az Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).
>
>

1. Lekérdezés hello lemez a részleteket a Tulajdonságok hello feladat visszaállítva.

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. Az Azure storage-környezet hello beállítása, és állítsa vissza a hello JSON-konfigurációs fájlt.

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. Hello JSON konfigurációs fájl toocreate hello Virtuálisgép-konfiguráció használata.

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. Hello operációsrendszer-lemez és adatlemezek csatolni. A virtuális gépek hello konfigurációjától függően kattintson hello vonatkozó hivatkozás tooview megfelelő parancsmagokat: 
    - [A nem felügyelt, nem titkosított virtuális gépek](#non-managed-non-encrypted-vms)
    - [A nem felügyelt, titkosított virtuális gépek (csak BEK)](#non-managed-encrypted-vms-bek-only)
    - [A nem felügyelt, titkosított virtuális gépek (BEK és KEK)](#non-managed-encrypted-vms-bek-and-kek)
    - [Felügyelt, nem titkosított virtuális gépek](#managed-non-encrypted-vms)
    - [Felügyelt, titkosított virtuális gépek (BEK és KEK)](#managed-encrypted-vms-bek-and-kek)
    
    #### <a name="non-managed-non-encrypted-vms"></a>A nem felügyelt, nem titkosított virtuális gépek

    Minta nem kezelt, nem titkosított virtuális gépek a következő hello használata.

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a>A nem felügyelt, titkosított virtuális gépek (csak BEK)

    Nem kezelt, titkosított virtuális gépen (titkosítja BEK csak) toorestore hello titkos toohello kulcstároló előtt kell csatolhat a lemezeket. További információkért lásd: hello cikk [visszaállítani egy titkosított virtuális gépet az Azure biztonsági mentési helyreállítási pontokról](backup-azure-restore-key-secret.md). a következő minta hello jeleníti meg, hogyan tooattach az operációs rendszer és az adatlemezek titkosított virtuális gépeket.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a>A nem felügyelt, titkosított virtuális gépek (BEK és KEK)

    Nem kezelt, titkosított virtuális gépen (titkosítja BEK és KEK) toorestore hello kulcsot és titkos toohello kulcstároló előtt kell csatolhat a lemezeket. További információkért lásd: hello cikk [visszaállítani egy titkosított virtuális gépet az Azure biztonsági mentési helyreállítási pontokról](backup-azure-restore-key-secret.md). a következő minta hello jeleníti meg, hogyan tooattach az operációs rendszer és az adatlemezek titkosított virtuális gépeket.

    ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

    #### <a name="managed-non-encrypted-vms"></a>Felügyelt, nem titkosított virtuális gépek

    Felügyelt nem titkosított virtuális gépekhez akkor lesz kell felügyelt toocreate lemezeit a blob storage, és majd csatlakoztassa a hello lemezeket. Részletes információkért lásd: hello cikk [csatolni egy adatok lemez tooa Windows virtuális gép PowerShell-lel](../virtual-machines/windows/attach-disk-ps.md). a következő példakód hello jeleníti meg, hogyan tooattach hello adatlemezek felügyelt nem titkosított virtuális gépeket.

    ```
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="managed-encrypted-vms-bek-and-kek"></a>Felügyelt, titkosított virtuális gépek (BEK és KEK)

    Felügyelt titkosított virtuális gépek (titkosítja BEK és KEK) akkor lesz kell felügyelt toocreate lemezeit a blob storage, és majd csatlakoztassa a hello lemezeket. Részletes információkért lásd: hello cikk [csatolni egy adatok lemez tooa Windows virtuális gép PowerShell-lel](../virtual-machines/windows/attach-disk-ps.md). hello következő mintakód bemutatja, hogyan tooattach hello adatlemezek felügyelt titkosított virtuális gépeket.

     ```
    PS C:\> $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
    PS C:\> $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
    PS C:\> $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
    PS C:\> $storageType = "StandardLRS"
    PS C:\> $osDiskName = $vm.Name + "_osdisk"
    PS C:\> $osVhdUri = $obj.'properties.storageProfile'.osDisk.vhd.uri
    PS C:\> $diskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $osVhdUri
    PS C:\> $osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk $diskConfig -ResourceGroupName "test"
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
    PS C:\> foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $dataDiskName = $vm.Name + $dd.name ;
     $dataVhdUri = $dd.vhd.uri ;
     $dataDiskConfig = New-AzureRmDiskConfig -AccountType $storageType -Location "West US" -CreateOption Import -SourceUri $dataVhdUri ;
     $dataDisk2 = New-AzureRmDisk -DiskName $dataDiskName -Disk $dataDiskConfig -ResourceGroupName "test" ;
     Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -ManagedDiskId $dataDisk2.Id -Lun $dd.Lun -CreateOption "Attach"
     }
    ```

5. Hello hálózati beállításainak megadása.

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. Hello virtuális gép létrehozása.

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="next-steps"></a>Következő lépések
Ha jobban szeret toouse PowerShell tooengage együtt az Azure-erőforrások, lásd: hello PowerShell cikk [telepítés és a Windows Server biztonsági másolat kezelése](backup-client-automation.md). Ha Ön kezeli a DPM biztonsági mentések, tekintse meg a hello cikket, [telepítés és a DPM a biztonsági mentés kezelése](backup-dpm-automation.md). Ezek a cikkek mindegyikét verziónál Resource Manager üzembe helyezések vagy a klasszikus telepítések esetén.  
