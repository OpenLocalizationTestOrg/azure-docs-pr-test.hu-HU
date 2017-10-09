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
# <a name="use-azurermrecoveryservicesbackup-cmdlets-tooback-up-virtual-machines"></a><span data-ttu-id="b765d-103">Virtuális gépek AzureRM.RecoveryServices.Backup parancsmagok tooback használata</span><span class="sxs-lookup"><span data-stu-id="b765d-103">Use AzureRM.RecoveryServices.Backup cmdlets tooback up virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b765d-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="b765d-104">Resource Manager</span></span>](backup-azure-vms-automation.md)
> * [<span data-ttu-id="b765d-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="b765d-105">Classic</span></span>](backup-azure-vms-classic-automation.md)
>
>

<span data-ttu-id="b765d-106">Ez a cikk bemutatja, hogyan toouse Azure PowerShell parancsmagok tooback mentése és helyreállítása egy Azure virtuális gép (VM) a Recovery Services tároló.</span><span class="sxs-lookup"><span data-stu-id="b765d-106">This article shows you how toouse Azure PowerShell cmdlets tooback up and recover an Azure virtual machine (VM) from a Recovery Services vault.</span></span> <span data-ttu-id="b765d-107">Recovery Services-tároló egy Azure Resource Manager-erőforrás és használt tooprotect adatok és eszközök is az Azure Backup, és az Azure Site Recovery szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="b765d-107">A Recovery Services vault is an Azure Resource Manager resource and is used tooprotect data and assets in both Azure Backup and Azure Site Recovery services.</span></span> <span data-ttu-id="b765d-108">A Recovery Services-tároló tooprotect Azure Service Manager telepített virtuális gépek és az Azure Resource Manager telepített virtuális gépek is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b765d-108">You can use a Recovery Services vault tooprotect Azure Service Manager-deployed VMs, and Azure Resource Manager-deployed VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="b765d-109">Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="b765d-109">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="b765d-110">Ez a cikk hello Resource Manager-modell használatával létrehozott virtuális gépek történő használatra szolgál.</span><span class="sxs-lookup"><span data-stu-id="b765d-110">This article is for use with VMs created using hello Resource Manager model.</span></span>
>
>

<span data-ttu-id="b765d-111">Ez a cikk bemutatja, hogyan PowerShell tooprotect használatával egy virtuális gép, és a visszaállítási adatok helyreállítási pontból.</span><span class="sxs-lookup"><span data-stu-id="b765d-111">This article walks you through using PowerShell tooprotect a VM, and restore data from a recovery point.</span></span>

## <a name="concepts"></a><span data-ttu-id="b765d-112">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="b765d-112">Concepts</span></span>
<span data-ttu-id="b765d-113">Ha nem ismeri az Azure Backup szolgáltatás hello szolgáltatás áttekintését hello kivétele [Mi az Azure Backup?](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="b765d-113">If you are not familiar with hello Azure Backup service, for an overview of hello service, check out [What is Azure Backup?](backup-introduction-to-azure-backup.md)</span></span> <span data-ttu-id="b765d-114">Megkezdése előtt ellenőrizze, hogy hello essentials hello szükséges előfeltételeket toowork Azure Backup szolgáltatással kapcsolatos foglalkozik, és hello hello aktuális virtuális gép biztonsági mentési megoldásra vonatkozó korlátozások.</span><span class="sxs-lookup"><span data-stu-id="b765d-114">Before you start, ensure that you cover hello essentials about hello prerequisites needed toowork with Azure Backup, and hello limitations of hello current VM backup solution.</span></span>

<span data-ttu-id="b765d-115">toouse PowerShell gyakorlatilag szükség toounderstand hello hierarchia objektumok és honnan toostart.</span><span class="sxs-lookup"><span data-stu-id="b765d-115">toouse PowerShell effectively, it is necessary toounderstand hello hierarchy of objects and from where toostart.</span></span>

![Helyreállítási szolgáltatások eltér az objektumhierarchia](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

<span data-ttu-id="b765d-117">tooview hello AzureRm.RecoveryServices.Backup PowerShell parancsmag-referencia, lásd: hello [Azure Backup - helyreállítási szolgáltatások parancsmagjai](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) hello Azure könyvtárban található.</span><span class="sxs-lookup"><span data-stu-id="b765d-117">tooview hello AzureRm.RecoveryServices.Backup PowerShell cmdlet reference, see hello [Azure Backup - Recovery Services Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in hello Azure library.</span></span>

## <a name="setup-and-registration"></a><span data-ttu-id="b765d-118">Telepítését és regisztrálását</span><span class="sxs-lookup"><span data-stu-id="b765d-118">Setup and Registration</span></span>
<span data-ttu-id="b765d-119">toobegin:</span><span class="sxs-lookup"><span data-stu-id="b765d-119">toobegin:</span></span>

1. <span data-ttu-id="b765d-120">[Hello PowerShell legújabb verziójának letöltése](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (hello szükséges minimális verziója: 1.4.0)</span><span class="sxs-lookup"><span data-stu-id="b765d-120">[Download hello latest version of PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (hello minimum version required is: 1.4.0)</span></span>
2. <span data-ttu-id="b765d-121">Keresse meg a rendelkezésre álló hello Azure biztonsági mentés PowerShell-parancsmagok hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="b765d-121">Find hello Azure Backup PowerShell cmdlets available by typing hello following command:</span></span>

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


<span data-ttu-id="b765d-122">a következő feladatok hello automatizálható a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="b765d-122">hello following tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="b765d-123">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="b765d-123">Create a Recovery Services vault</span></span>
* <span data-ttu-id="b765d-124">Azure-beli virtuális gépek biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="b765d-124">Back up Azure VMs</span></span>
* <span data-ttu-id="b765d-125">A biztonsági mentési feladatot indít</span><span class="sxs-lookup"><span data-stu-id="b765d-125">Trigger a backup job</span></span>
* <span data-ttu-id="b765d-126">A figyelő egy biztonsági mentési feladat</span><span class="sxs-lookup"><span data-stu-id="b765d-126">Monitor a backup job</span></span>
* <span data-ttu-id="b765d-127">Állítsa vissza az Azure virtuális gép</span><span class="sxs-lookup"><span data-stu-id="b765d-127">Restore an Azure VM</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="b765d-128">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="b765d-128">Create a recovery services vault</span></span>
<span data-ttu-id="b765d-129">a lépéseket követve hello vezethet a Recovery Services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b765d-129">hello following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="b765d-130">Recovery Services-tároló nem egyezik egy biztonsági mentési tárolót.</span><span class="sxs-lookup"><span data-stu-id="b765d-130">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="b765d-131">Ha használ Azure Backup a hello először, használnia kell a hello  **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  parancsmag tooregister hello Azure helyreállítási szolgáltató az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="b765d-131">If you are using Azure Backup for hello first time, you must use hello **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** cmdlet tooregister hello Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="b765d-132">hello Recovery Services-tárolónak egy olyan erőforrás-kezelő erőforrás, ezért meg kell tooplace az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="b765d-132">hello Recovery Services vault is a Resource Manager resource, so you need tooplace it within a resource group.</span></span> <span data-ttu-id="b765d-133">Használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy erőforráscsoportot hello  **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**  parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b765d-133">You can use an existing resource group, or create a resource group with hello **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)** cmdlet.</span></span> <span data-ttu-id="b765d-134">Erőforráscsoport létrehozásakor adja meg a hello és hello erőforrásnak helyét.</span><span class="sxs-lookup"><span data-stu-id="b765d-134">When creating a resource group, specify hello name and location for hello resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="b765d-135">Használjon hello  **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)**  parancsmag toocreate hello Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="b765d-135">Use hello **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** cmdlet toocreate hello Recovery Services vault.</span></span> <span data-ttu-id="b765d-136">Győződjön meg arról, hogy toospecify hello hello tároló ugyanazon a helyen, mint az erőforráscsoport hello használt.</span><span class="sxs-lookup"><span data-stu-id="b765d-136">Be sure toospecify hello same location for hello vault as was used for hello resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="b765d-137">Adja meg a tárolási redundancia toouse; hello típusa használhat [helyileg redundáns tárolás (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) vagy [földrajzi redundáns tárolás (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="b765d-137">Specify hello type of storage redundancy toouse; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="b765d-138">hello következő példa bemutatja hello - BackupStorageRedundancy testvault vonatkozó beállítás tooGeoRedundant.</span><span class="sxs-lookup"><span data-stu-id="b765d-138">hello following example shows hello -BackupStorageRedundancy option for testvault is set tooGeoRedundant.</span></span>

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > <span data-ttu-id="b765d-139">Sok Azure biztonsági mentést készítő parancsmagok bemenetként hello Recovery Services-tároló objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="b765d-139">Many Azure Backup cmdlets require hello Recovery Services vault object as an input.</span></span> <span data-ttu-id="b765d-140">Emiatt egy kényelmes toostore hello biztonsági mentést a Recovery Services tároló objektum egy változóban.</span><span class="sxs-lookup"><span data-stu-id="b765d-140">For this reason, it is convenient toostore hello Backup Recovery Services vault object in a variable.</span></span>
   >
   >

## <a name="view-hello-vaults-in-a-subscription"></a><span data-ttu-id="b765d-141">Nézet hello tárolók az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b765d-141">View hello vaults in a subscription</span></span>
<span data-ttu-id="b765d-142">Használjon  **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**  ebben az előfizetésben hello összes tárolók tooview hello listája.</span><span class="sxs-lookup"><span data-stu-id="b765d-142">Use **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)** tooview hello list of all vaults in hello current subscription.</span></span> <span data-ttu-id="b765d-143">Ez a parancs toocheck, hogy egy új tároló lett létrehozva, vagy toosee hello hello előfizetésben elérhető tárolók is használhatja.</span><span class="sxs-lookup"><span data-stu-id="b765d-143">You can use this command toocheck that a new vault was created, or toosee hello available vaults in hello subscription.</span></span>

<span data-ttu-id="b765d-144">Hello parancs, a Get-AzureRmRecoveryServicesVault, minden tooview-tárolók hello előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="b765d-144">Run hello command, Get-AzureRmRecoveryServicesVault, tooview all vaults in hello subscription.</span></span> <span data-ttu-id="b765d-145">hello alábbi példában látható minden egyes tároló megjelenő hello információ.</span><span class="sxs-lookup"><span data-stu-id="b765d-145">hello following example shows hello information displayed for each vault.</span></span>

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


## <a name="back-up-azure-vms"></a><span data-ttu-id="b765d-146">Azure-beli virtuális gépek biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="b765d-146">Back up Azure VMs</span></span>
<span data-ttu-id="b765d-147">A Recovery Services-tároló tooprotect használja a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b765d-147">Use a Recovery Services vault tooprotect your virtual machines.</span></span> <span data-ttu-id="b765d-148">Mielőtt hello adatvédelem hello tároló környezetben (hello típus védett hello tárolóban lévő adatok), és hello védelmi házirend ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="b765d-148">Before you apply hello protection, set hello vault context (hello type of data protected in hello vault), and verify hello protection policy.</span></span> <span data-ttu-id="b765d-149">hello védelmi házirend hello ütemezés hello biztonsági mentési feladatok futtatásakor, és mennyi ideig őrzi meg minden egyes biztonsági mentési pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="b765d-149">hello protection policy is hello schedule when hello backup jobs run, and how long each backup snapshot is retained.</span></span>

### <a name="set-vault-context"></a><span data-ttu-id="b765d-150">Tároló környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="b765d-150">Set vault context</span></span>
<span data-ttu-id="b765d-151">Ahhoz, hogy a virtuális gép védelme, használjon  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello tároló környezetben.</span><span class="sxs-lookup"><span data-stu-id="b765d-151">Before enabling protection on a VM, use **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** tooset hello vault context.</span></span> <span data-ttu-id="b765d-152">Miután hello tároló környezet van beállítva, tooall későbbi parancsmagok vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b765d-152">Once hello vault context is set, it applies tooall subsequent cmdlets.</span></span> <span data-ttu-id="b765d-153">hello alábbi mintakód hello tároló környezetben hello tároló *testvault*.</span><span class="sxs-lookup"><span data-stu-id="b765d-153">hello following example sets hello vault context for hello vault, *testvault*.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a><span data-ttu-id="b765d-154">Védelmi házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="b765d-154">Create a protection policy</span></span>
<span data-ttu-id="b765d-155">Recovery Services-tároló létrehozásakor az alapértelmezett védelem és adatmegőrzési ismét.</span><span class="sxs-lookup"><span data-stu-id="b765d-155">When you create a Recovery Services vault, it comes with default protection and retention policies.</span></span> <span data-ttu-id="b765d-156">hello alapértelmezett védelmi házirendet a biztonsági mentési feladatot, a megadott időpontban naponta váltja ki.</span><span class="sxs-lookup"><span data-stu-id="b765d-156">hello default protection policy triggers a backup job each day at a specified time.</span></span> <span data-ttu-id="b765d-157">hello alapértelmezett megőrzési házirend hello napi helyreállítási pont 30 napig őrzi meg.</span><span class="sxs-lookup"><span data-stu-id="b765d-157">hello default retention policy retains hello daily recovery point for 30 days.</span></span> <span data-ttu-id="b765d-158">Használhatja a hello alapértelmezett házirend tooquickly a virtuális gép védelmét, és később különböző adatokkal hello házirend szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="b765d-158">You can use hello default policy tooquickly protect your VM and edit hello policy later with different details.</span></span>

<span data-ttu-id="b765d-159">Használjon  **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**  tooview hello adatvédelmi szabályzatok hello tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="b765d-159">Use **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)** tooview hello protection policies in hello vault.</span></span> <span data-ttu-id="b765d-160">Ez a parancsmag tooget is használhatja, egy adott házirend, vagy egy munkaterhelés-típushoz tartozó tooview hello házirendek.</span><span class="sxs-lookup"><span data-stu-id="b765d-160">You can use this cmdlet tooget a specific policy, or tooview hello policies associated with a workload type.</span></span> <span data-ttu-id="b765d-161">a következő példa hello lekérdezi munkaterhelésének típusát, AzureVM házirendeket.</span><span class="sxs-lookup"><span data-stu-id="b765d-161">hello following example gets policies for workload type, AzureVM.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> <span data-ttu-id="b765d-162">hello BackupTime mezőt a PowerShellben hello időzónáját UTC.</span><span class="sxs-lookup"><span data-stu-id="b765d-162">hello timezone of hello BackupTime field in PowerShell is UTC.</span></span> <span data-ttu-id="b765d-163">Azonban hello Azure-portálon hello biztonságimásolat-készítési időpont látható, ha hello ideje módosított tooyour helyi időzónára.</span><span class="sxs-lookup"><span data-stu-id="b765d-163">However, when hello backup time is shown in hello Azure portal, hello time is adjusted tooyour local timezone.</span></span>
>
>

<span data-ttu-id="b765d-164">A biztonsági mentési házirenddel legalább egy megőrzési házirend társítva.</span><span class="sxs-lookup"><span data-stu-id="b765d-164">A backup protection policy is associated with at least one retention policy.</span></span> <span data-ttu-id="b765d-165">Adatmegőrzési házirend határozza meg, hány helyreállítási pont tartják után törli a rendszer.</span><span class="sxs-lookup"><span data-stu-id="b765d-165">Retention policy defines how long a recovery point is kept before it is deleted.</span></span> <span data-ttu-id="b765d-166">Használjon  **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**  tooview hello alapértelmezett megőrzési házirend.</span><span class="sxs-lookup"><span data-stu-id="b765d-166">Use **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)** tooview hello default retention policy.</span></span>  <span data-ttu-id="b765d-167">Hasonló módon használhatja  **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)**  tooobtain hello alapértelmezett ütemezés házirend.</span><span class="sxs-lookup"><span data-stu-id="b765d-167">Similarly you can use **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** tooobtain hello default schedule policy.</span></span> <span data-ttu-id="b765d-168">Hello  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  parancsmag létrehoz egy PowerShell-objektum, amely tartalmazza a biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="b765d-168">hello **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet creates a PowerShell object that holds backup policy information.</span></span> <span data-ttu-id="b765d-169">hello ütemezés és a megőrzési csoportházirend-objektumokat kell használni, mint a bemeneti toohello  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b765d-169">hello schedule and retention policy objects are used as inputs toohello **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet.</span></span> <span data-ttu-id="b765d-170">hello alábbi példa tárolja hello ütemezés házirend- és hello adatmegőrzési változók.</span><span class="sxs-lookup"><span data-stu-id="b765d-170">hello following example stores hello schedule policy and hello retention policy in variables.</span></span> <span data-ttu-id="b765d-171">hello példa változók toodefine hello paraméterek használ, a védelmi házirend létrehozásakor *NewPolicy*.</span><span class="sxs-lookup"><span data-stu-id="b765d-171">hello example uses those variables toodefine hello parameters when creating a protection policy, *NewPolicy*.</span></span>

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a><span data-ttu-id="b765d-172">Védelem engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b765d-172">Enable protection</span></span>
<span data-ttu-id="b765d-173">Hello védelem biztonsági mentési házirend meghatározása után továbbra is engedélyeznie kell egy elem hello házirend.</span><span class="sxs-lookup"><span data-stu-id="b765d-173">Once you have defined hello backup protection policy, you still must enable hello policy for an item.</span></span> <span data-ttu-id="b765d-174">Használjon  **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**  tooenable védelmet.</span><span class="sxs-lookup"><span data-stu-id="b765d-174">Use **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)** tooenable protection.</span></span> <span data-ttu-id="b765d-175">A védelem engedélyezéséhez szükséges a két objektum – hello és a hello szabályzat.</span><span class="sxs-lookup"><span data-stu-id="b765d-175">Enabling protection requires two objects - hello item and hello policy.</span></span> <span data-ttu-id="b765d-176">Miután hello házirend társítva hello tárolóban, hello biztonsági mentési munkafolyamat kiváltásakor hello hello házirend-ütemezést meghatározott időpontban.</span><span class="sxs-lookup"><span data-stu-id="b765d-176">Once hello policy has been associated with hello vault, hello backup workflow is triggered at hello time defined in hello policy schedule.</span></span>

<span data-ttu-id="b765d-177">a következő példa engedélyezi hello elem védelmét, V2VM, a házirend hello NewPolicy hello.</span><span class="sxs-lookup"><span data-stu-id="b765d-177">hello following example enables protection for hello item, V2VM, using hello policy, NewPolicy.</span></span> <span data-ttu-id="b765d-178">az erőforrás-kezelő virtuális gépeken futó nem titkosított tooenable hello védelme</span><span class="sxs-lookup"><span data-stu-id="b765d-178">tooenable hello protection on non-encrypted Resource Manager VMs</span></span>

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

<span data-ttu-id="b765d-179">tooenable hello védelmet a virtuális gépek (titkosítja BEK és KEK) titkosított, toogive hello Azure Backup szolgáltatás engedély tooread kulcsok és titkos kulcs tárolóból van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b765d-179">tooenable hello protection on encrypted VMs (encrypted using BEK and KEK), you need toogive hello Azure Backup service permission tooread keys and secrets from key vault.</span></span>

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

<span data-ttu-id="b765d-180">tooenable hello védelmet a virtuális gépek (titkosítja BEK csak) titkosítva, a kulcstároló toogive hello Azure Backup szolgáltatás engedély tooread titkok van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b765d-180">tooenable hello protection on encrypted VMs (encrypted using BEK only), you need toogive hello Azure Backup service permission tooread secrets from key vault.</span></span>

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> <span data-ttu-id="b765d-181">Ha hello Azure Government felhő használ, majd használja hello érték ff281ffe-705c-4f53-9f37-a40e6f2c68f3 hello paraméter **- ServicePrincipalName** a [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) parancsmag .</span><span class="sxs-lookup"><span data-stu-id="b765d-181">If you are using hello Azure Government cloud, then use hello value ff281ffe-705c-4f53-9f37-a40e6f2c68f3 for hello parameter **-ServicePrincipalName** in [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet.</span></span>
>
>

<span data-ttu-id="b765d-182">Klasszikus virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="b765d-182">For classic VMs</span></span>

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a><span data-ttu-id="b765d-183">A védelmi házirend módosítása</span><span class="sxs-lookup"><span data-stu-id="b765d-183">Modify a protection policy</span></span>
<span data-ttu-id="b765d-184">toomodify hello védelmi házirendje, használjon [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) toomodify hello SchedulePolicy vagy RetentionPolicy objektumot.</span><span class="sxs-lookup"><span data-stu-id="b765d-184">toomodify hello protection policy, use [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) toomodify hello SchedulePolicy or RetentionPolicy objects.</span></span>

<span data-ttu-id="b765d-185">hello példa megváltoztatja hello helyreállítási pont megőrzési too365 nap.</span><span class="sxs-lookup"><span data-stu-id="b765d-185">hello following example changes hello recovery point retention too365 days.</span></span>

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a><span data-ttu-id="b765d-186">A biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="b765d-186">Trigger a backup</span></span>
<span data-ttu-id="b765d-187">Használhat  **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  tootrigger egy biztonsági mentési feladat.</span><span class="sxs-lookup"><span data-stu-id="b765d-187">You can use **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** tootrigger a backup job.</span></span> <span data-ttu-id="b765d-188">Ha hello kezdeti biztonsági másolatot, akkor egy teljes biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="b765d-188">If it is hello initial backup, it is a full backup.</span></span> <span data-ttu-id="b765d-189">Azt követő biztonsági mentéseket egy növekményes másolatot igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="b765d-189">Subsequent backups take an incremental copy.</span></span> <span data-ttu-id="b765d-190">Lehet, hogy toouse  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  tooset hello tároló környezetben hello biztonsági mentési feladat elindítása előtt.</span><span class="sxs-lookup"><span data-stu-id="b765d-190">Be sure toouse **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** tooset hello vault context before triggering hello backup job.</span></span> <span data-ttu-id="b765d-191">a következő példa hello azt feltételezi, hogy a tároló környezet beállítása történt.</span><span class="sxs-lookup"><span data-stu-id="b765d-191">hello following example assumes vault context was set.</span></span>

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> <span data-ttu-id="b765d-192">hello időzóna hello StartTime és az EndTime megadása mezők PowerShell UTC.</span><span class="sxs-lookup"><span data-stu-id="b765d-192">hello timezone of hello StartTime and EndTime fields in PowerShell is UTC.</span></span> <span data-ttu-id="b765d-193">Azonban hello idő hello Azure-portálon látható, amikor hello ideje módosított tooyour helyi időzónára.</span><span class="sxs-lookup"><span data-stu-id="b765d-193">However, when hello time is shown in hello Azure portal, hello time is adjusted tooyour local timezone.</span></span>
>
>

## <a name="monitoring-a-backup-job"></a><span data-ttu-id="b765d-194">A biztonsági mentési feladatot figyelése</span><span class="sxs-lookup"><span data-stu-id="b765d-194">Monitoring a backup job</span></span>
<span data-ttu-id="b765d-195">Figyelheti a hosszú ideig futó műveletek, például a biztonsági mentési feladatok hello Azure portál használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="b765d-195">You can monitor long-running operations, such as backup jobs, without using hello Azure portal.</span></span> <span data-ttu-id="b765d-196">egy folyamatban lévő feladat, használjon hello tooget hello állapota  **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**  parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b765d-196">tooget hello status of an in-progress job, use hello **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)** cmdlet.</span></span> <span data-ttu-id="b765d-197">Ez a parancsmag lekérdezi a biztonsági mentési feladataihoz hello egy adott tárolóban, és, hogy a tároló hello tároló környezetben van megadva.</span><span class="sxs-lookup"><span data-stu-id="b765d-197">This cmdlet gets hello backup jobs for a specific vault, and that vault is specified in hello vault context.</span></span> <span data-ttu-id="b765d-198">hello következő példa egy folyamatban lévő feladat tömbként hello állapotának beolvasása, és hello állapota a hello $joblist változó.</span><span class="sxs-lookup"><span data-stu-id="b765d-198">hello following example gets hello status of an in-progress job as an array, and stores hello status in hello $joblist variable.</span></span>

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

<span data-ttu-id="b765d-199">Ezek a feladatok befejezésére – ami szükségtelen kód - lekérdezési helyett használja a hello  **[várakozási-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b765d-199">Instead of polling these jobs for completion - which is unnecessary additional code - use hello **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet.</span></span> <span data-ttu-id="b765d-200">Ez a parancsmag hello végrehajtási felfüggesztése, amíg hello feladat befejeződik, vagy hello megadott időtúllépési érték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="b765d-200">This cmdlet pauses hello execution until either hello job completes or hello specified timeout value is reached.</span></span>

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a><span data-ttu-id="b765d-201">Állítsa vissza az Azure virtuális gép</span><span class="sxs-lookup"><span data-stu-id="b765d-201">Restore an Azure VM</span></span>
<span data-ttu-id="b765d-202">Hello visszaállítását egy virtuális gép hello Azure-portál használatával, és a PowerShell virtuális gépek visszaállítása közötti fő különbség van.</span><span class="sxs-lookup"><span data-stu-id="b765d-202">There is a key difference between hello restoring a VM using hello Azure portal and restoring a VM using PowerShell.</span></span> <span data-ttu-id="b765d-203">A PowerShell-lel, hello visszaállítási művelet be nem fejeződött hello lemezét és konfigurációs adatát hello helyreállítási pont létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="b765d-203">With PowerShell, hello restore operation is complete once hello disks and configuration information from hello recovery point are created.</span></span>

> [!NOTE]
> <span data-ttu-id="b765d-204">hello visszaállítási művelet nem hoz létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="b765d-204">hello restore operation does not create a virtual machine.</span></span>
>
>

<span data-ttu-id="b765d-205">a lemezről, virtuális gép toocreate hello, részben [tárolt lemezekből a virtuális gép létrehozása hello](backup-azure-vms-automation.md#create-a-vm-from-stored-disks).</span><span class="sxs-lookup"><span data-stu-id="b765d-205">toocreate a virtual machine from disk, see hello section, [Create hello VM from stored disks](backup-azure-vms-automation.md#create-a-vm-from-stored-disks).</span></span> <span data-ttu-id="b765d-206">hello lépéseken toorestore egy Azure virtuális Gépen a következők:</span><span class="sxs-lookup"><span data-stu-id="b765d-206">hello basic steps toorestore an Azure VM are:</span></span>

* <span data-ttu-id="b765d-207">Válassza ki a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="b765d-207">Select hello VM</span></span>
* <span data-ttu-id="b765d-208">A helyreállítási pont kiválasztása</span><span class="sxs-lookup"><span data-stu-id="b765d-208">Choose a recovery point</span></span>
* <span data-ttu-id="b765d-209">Hello lemezek visszaállítása</span><span class="sxs-lookup"><span data-stu-id="b765d-209">Restore hello disks</span></span>
* <span data-ttu-id="b765d-210">Hozzon létre virtuális gép hello tárolt lemezekből</span><span class="sxs-lookup"><span data-stu-id="b765d-210">Create hello VM from stored disks</span></span>

<span data-ttu-id="b765d-211">hello alábbi ábrán látható hello objektum hierarchia hello RecoveryServicesVault toohello BackupRecoveryPoint le.</span><span class="sxs-lookup"><span data-stu-id="b765d-211">hello following graphic shows hello object hierarchy from hello RecoveryServicesVault down toohello BackupRecoveryPoint.</span></span>

![Helyreállítási szolgáltatások eltér az objektumhierarchia BackupContainer megjelenítése](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

<span data-ttu-id="b765d-213">toorestore biztonsági mentési adatokat, hello biztonsági másolat elem és hello időpontban adatokat tartalmazó hello helyreállítási pont azonosítása.</span><span class="sxs-lookup"><span data-stu-id="b765d-213">toorestore backup data, identify hello backed-up item and hello recovery point that holds hello point-in-time data.</span></span> <span data-ttu-id="b765d-214">Használjon hello  **[visszaállítási-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  hello parancsmag toorestore adatait tároló toohello felhasználói fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="b765d-214">Use hello **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet toorestore data from hello vault toohello customer's account.</span></span>

### <a name="select-hello-vm"></a><span data-ttu-id="b765d-215">Válassza ki a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="b765d-215">Select hello VM</span></span>
<span data-ttu-id="b765d-216">tooget hello PowerShell-objektum, amely azonosítja a jobb oldali hello elem biztonsági mentését, hello tárolóban hello tároló-tól kezdődnek és a hello objektum hierarchia valamennyi alsóbb szintjén módon működnek.</span><span class="sxs-lookup"><span data-stu-id="b765d-216">tooget hello PowerShell object that identifies hello right backup item, start from hello container in hello vault, and work your way down hello object hierarchy.</span></span> <span data-ttu-id="b765d-217">virtuális gép, használjon hello hello jelölő tooselect hello tároló  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  parancsmag és a csövön keresztüli adott toohello  **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b765d-217">tooselect hello container that represents hello VM, use hello **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** cmdlet and pipe that toohello **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** cmdlet.</span></span>

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" –Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem –Container $namedContainer  –WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a><span data-ttu-id="b765d-218">A helyreállítási pont kiválasztása</span><span class="sxs-lookup"><span data-stu-id="b765d-218">Choose a recovery point</span></span>
<span data-ttu-id="b765d-219">Használjon hello  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  parancsmag toolist hello biztonsági mentési elem minden helyreállítási pontja.</span><span class="sxs-lookup"><span data-stu-id="b765d-219">Use hello **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** cmdlet toolist all recovery points for hello backup item.</span></span> <span data-ttu-id="b765d-220">Ezután válasszon ki hello helyreállítási pont toorestore.</span><span class="sxs-lookup"><span data-stu-id="b765d-220">Then choose hello recovery point toorestore.</span></span> <span data-ttu-id="b765d-221">Ha biztos abban, hogy melyik helyreállítási ponton toouse, a rendszer egy célszerű toochoose hello legutóbbi RecoveryPointType = AppConsistent pont hello listában.</span><span class="sxs-lookup"><span data-stu-id="b765d-221">If you are unsure which recovery point toouse, it is a good practice toochoose hello most recent RecoveryPointType = AppConsistent point in hello list.</span></span>

<span data-ttu-id="b765d-222">A következő parancsfájl hello, hello változó, **$rp**, a helyreállítási pontok egy tömb van hello kijelölt elem biztonsági mentése hello az elmúlt hét napban.</span><span class="sxs-lookup"><span data-stu-id="b765d-222">In hello following script, hello variable, **$rp**, is an array of recovery points for hello selected backup item, from hello past seven days.</span></span> <span data-ttu-id="b765d-223">hello tömb fordított sorrendben rendezve idő hello legújabb helyreállítási ponttal 0. indexnél.</span><span class="sxs-lookup"><span data-stu-id="b765d-223">hello array is sorted in reverse order of time with hello latest recovery point at index 0.</span></span> <span data-ttu-id="b765d-224">Használja a következő szabványos PowerShell tömböt indexelő toopick hello helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="b765d-224">Use standard PowerShell array indexing toopick hello recovery point.</span></span> <span data-ttu-id="b765d-225">Hello példában $rp [0] hello legutóbbi helyreállítási pontot választja ki.</span><span class="sxs-lookup"><span data-stu-id="b765d-225">In hello example, $rp[0] selects hello latest recovery point.</span></span>

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



### <a name="restore-hello-disks"></a><span data-ttu-id="b765d-226">Hello lemezek visszaállítása</span><span class="sxs-lookup"><span data-stu-id="b765d-226">Restore hello disks</span></span>
<span data-ttu-id="b765d-227">Használjon hello  **[visszaállítási-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  parancsmag toorestore egy biztonsági mentési elem adatait és a konfigurációs tooa helyreállítási pontok.</span><span class="sxs-lookup"><span data-stu-id="b765d-227">Use hello **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet toorestore a backup item's data and configuration tooa recovery point.</span></span> <span data-ttu-id="b765d-228">Ha azonosított egy helyreállítási pontot, hello értékét a használni hello **- RecoveryPoint** paraméter.</span><span class="sxs-lookup"><span data-stu-id="b765d-228">Once you have identified a recovery point, use it as hello value for hello **-RecoveryPoint** parameter.</span></span> <span data-ttu-id="b765d-229">A hello előző példakód **$rp [0]** hello helyreállítási pont toouse volt.</span><span class="sxs-lookup"><span data-stu-id="b765d-229">In hello previous sample code, **$rp[0]** was hello recovery point toouse.</span></span> <span data-ttu-id="b765d-230">Az alábbi kódmintában hello **$rp [0]** hello helyreállítási pont toouse hello lemez visszaállítására van.</span><span class="sxs-lookup"><span data-stu-id="b765d-230">In hello following sample code, **$rp[0]** is hello recovery point toouse for restoring hello disk.</span></span>

<span data-ttu-id="b765d-231">toorestore hello lemezét és konfigurációs adatát:</span><span class="sxs-lookup"><span data-stu-id="b765d-231">toorestore hello disks and configuration information:</span></span>

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

<span data-ttu-id="b765d-232">Használjon hello  **[várakozási-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  parancsmag toowait hello visszaállítási feladat toocomplete számára.</span><span class="sxs-lookup"><span data-stu-id="b765d-232">Use hello **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet toowait for hello Restore job toocomplete.</span></span>

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

<span data-ttu-id="b765d-233">Ha hello visszaállítási feladat befejeződött, a hello  **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**  parancsmag tooget hello részleteit hello visszaállítási műveletet.</span><span class="sxs-lookup"><span data-stu-id="b765d-233">Once hello Restore job has completed, use hello **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)** cmdlet tooget hello details of hello restore operation.</span></span> <span data-ttu-id="b765d-234">hello JobDetails tulajdonság hello információ szükséges toorebuild hello virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="b765d-234">hello JobDetails property has hello information needed toorebuild hello VM.</span></span>

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

<span data-ttu-id="b765d-235">Miután helyreállította hello lemezek, nyissa meg toohello következő szakasz toocreate hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="b765d-235">Once you restore hello disks, go toohello next section toocreate hello VM.</span></span>

## <a name="create-a-vm-from-restored-disks"></a><span data-ttu-id="b765d-236">Hozzon létre egy virtuális Gépet visszaállított lemezekből</span><span class="sxs-lookup"><span data-stu-id="b765d-236">Create a VM from restored disks</span></span>
<span data-ttu-id="b765d-237">Hello lemezek visszaállítását követően használja ezeket a lépéseket toocreate, és konfigurálja a lemezről hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="b765d-237">After you have restored hello disks, use these steps toocreate and configure hello virtual machine from disk.</span></span>

> [!NOTE]
> <span data-ttu-id="b765d-238">toocreate virtuális gépek titkosítása a visszaállított lemezek, a Azure szerepkörnek rendelkeznie kell engedéllyel tooperform hello művelet **Microsoft.KeyVault/vaults/deploy/action**.</span><span class="sxs-lookup"><span data-stu-id="b765d-238">toocreate encrypted VMs from restored disks, your Azure role must have permission tooperform hello action, **Microsoft.KeyVault/vaults/deploy/action**.</span></span> <span data-ttu-id="b765d-239">Ha a szerepkör nem rendelkezik ezzel az engedéllyel, hozzon létre egy egyéni biztonsági szerepkört ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="b765d-239">If your role does not have this permission, create a custom role with this action.</span></span> <span data-ttu-id="b765d-240">További információkért lásd: [egyéni szerepkörök az Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="b765d-240">For more information, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>
>
>

1. <span data-ttu-id="b765d-241">Lekérdezés hello lemez a részleteket a Tulajdonságok hello feladat visszaállítva.</span><span class="sxs-lookup"><span data-stu-id="b765d-241">Query hello restored disk properties for hello job details.</span></span>

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. <span data-ttu-id="b765d-242">Az Azure storage-környezet hello beállítása, és állítsa vissza a hello JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="b765d-242">Set hello Azure storage context and restore hello JSON configuration file.</span></span>

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. <span data-ttu-id="b765d-243">Hello JSON konfigurációs fájl toocreate hello Virtuálisgép-konfiguráció használata.</span><span class="sxs-lookup"><span data-stu-id="b765d-243">Use hello JSON configuration file toocreate hello VM configuration.</span></span>

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. <span data-ttu-id="b765d-244">Hello operációsrendszer-lemez és adatlemezek csatolni.</span><span class="sxs-lookup"><span data-stu-id="b765d-244">Attach hello OS disk and data disks.</span></span> <span data-ttu-id="b765d-245">A virtuális gépek hello konfigurációjától függően kattintson hello vonatkozó hivatkozás tooview megfelelő parancsmagokat:</span><span class="sxs-lookup"><span data-stu-id="b765d-245">Depending on hello configuration of your VMs, click on hello relevant link tooview respective cmdlets:</span></span> 
    - [<span data-ttu-id="b765d-246">A nem felügyelt, nem titkosított virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="b765d-246">Non-managed, non-encrypted VMs</span></span>](#non-managed-non-encrypted-vms)
    - [<span data-ttu-id="b765d-247">A nem felügyelt, titkosított virtuális gépek (csak BEK)</span><span class="sxs-lookup"><span data-stu-id="b765d-247">Non-managed, encrypted VMs (BEK only)</span></span>](#non-managed-encrypted-vms-bek-only)
    - [<span data-ttu-id="b765d-248">A nem felügyelt, titkosított virtuális gépek (BEK és KEK)</span><span class="sxs-lookup"><span data-stu-id="b765d-248">Non-managed, encrypted VMs (BEK and KEK)</span></span>](#non-managed-encrypted-vms-bek-and-kek)
    - [<span data-ttu-id="b765d-249">Felügyelt, nem titkosított virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="b765d-249">Managed, non-encrypted VMs</span></span>](#managed-non-encrypted-vms)
    - [<span data-ttu-id="b765d-250">Felügyelt, titkosított virtuális gépek (BEK és KEK)</span><span class="sxs-lookup"><span data-stu-id="b765d-250">Managed, encrypted VMs (BEK and KEK)</span></span>](#managed-encrypted-vms-bek-and-kek)
    
    #### <a name="non-managed-non-encrypted-vms"></a><span data-ttu-id="b765d-251">A nem felügyelt, nem titkosított virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="b765d-251">Non-managed, non-encrypted VMs</span></span>

    <span data-ttu-id="b765d-252">Minta nem kezelt, nem titkosított virtuális gépek a következő hello használata.</span><span class="sxs-lookup"><span data-stu-id="b765d-252">Use hello following sample for non-managed, non-encrypted VMs.</span></span>

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a><span data-ttu-id="b765d-253">A nem felügyelt, titkosított virtuális gépek (csak BEK)</span><span class="sxs-lookup"><span data-stu-id="b765d-253">Non-managed, encrypted VMs (BEK only)</span></span>

    <span data-ttu-id="b765d-254">Nem kezelt, titkosított virtuális gépen (titkosítja BEK csak) toorestore hello titkos toohello kulcstároló előtt kell csatolhat a lemezeket.</span><span class="sxs-lookup"><span data-stu-id="b765d-254">For non-managed, encrypted VMs (encrypted using BEK only), you need toorestore hello secret toohello key vault before you can attach disks.</span></span> <span data-ttu-id="b765d-255">További információkért lásd: hello cikk [visszaállítani egy titkosított virtuális gépet az Azure biztonsági mentési helyreállítási pontokról](backup-azure-restore-key-secret.md).</span><span class="sxs-lookup"><span data-stu-id="b765d-255">For more information, please see hello article, [Restore an encrypted virtual machine from an Azure Backup recovery point](backup-azure-restore-key-secret.md).</span></span> <span data-ttu-id="b765d-256">a következő minta hello jeleníti meg, hogyan tooattach az operációs rendszer és az adatlemezek titkosított virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b765d-256">hello following sample shows how tooattach OS and data disks for encrypted VMs.</span></span>

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

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a><span data-ttu-id="b765d-257">A nem felügyelt, titkosított virtuális gépek (BEK és KEK)</span><span class="sxs-lookup"><span data-stu-id="b765d-257">Non-managed, encrypted VMs (BEK and KEK)</span></span>

    <span data-ttu-id="b765d-258">Nem kezelt, titkosított virtuális gépen (titkosítja BEK és KEK) toorestore hello kulcsot és titkos toohello kulcstároló előtt kell csatolhat a lemezeket.</span><span class="sxs-lookup"><span data-stu-id="b765d-258">For non-managed, encrypted VMs (encrypted using BEK and KEK), you need toorestore hello key and secret toohello key vault before you can attach disks.</span></span> <span data-ttu-id="b765d-259">További információkért lásd: hello cikk [visszaállítani egy titkosított virtuális gépet az Azure biztonsági mentési helyreállítási pontokról](backup-azure-restore-key-secret.md).</span><span class="sxs-lookup"><span data-stu-id="b765d-259">For more information, please see hello article, [Restore an encrypted virtual machine from an Azure Backup recovery point](backup-azure-restore-key-secret.md).</span></span> <span data-ttu-id="b765d-260">a következő minta hello jeleníti meg, hogyan tooattach az operációs rendszer és az adatlemezek titkosított virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b765d-260">hello following sample shows how tooattach OS and data disks for encrypted VMs.</span></span>

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

    #### <a name="managed-non-encrypted-vms"></a><span data-ttu-id="b765d-261">Felügyelt, nem titkosított virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="b765d-261">Managed, non-encrypted VMs</span></span>

    <span data-ttu-id="b765d-262">Felügyelt nem titkosított virtuális gépekhez akkor lesz kell felügyelt toocreate lemezeit a blob storage, és majd csatlakoztassa a hello lemezeket.</span><span class="sxs-lookup"><span data-stu-id="b765d-262">For managed non-encrypted VMs, you'll need toocreate managed disks from blob storage, and then attach hello disks.</span></span> <span data-ttu-id="b765d-263">Részletes információkért lásd: hello cikk [csatolni egy adatok lemez tooa Windows virtuális gép PowerShell-lel](../virtual-machines/windows/attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b765d-263">For in-depth information, see hello article, [Attach a data disk tooa Windows VM using PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span></span> <span data-ttu-id="b765d-264">a következő példakód hello jeleníti meg, hogyan tooattach hello adatlemezek felügyelt nem titkosított virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b765d-264">hello following sample code shows how tooattach hello data disks for managed non-encrypted VMs.</span></span>

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

    #### <a name="managed-encrypted-vms-bek-and-kek"></a><span data-ttu-id="b765d-265">Felügyelt, titkosított virtuális gépek (BEK és KEK)</span><span class="sxs-lookup"><span data-stu-id="b765d-265">Managed, encrypted VMs (BEK and KEK)</span></span>

    <span data-ttu-id="b765d-266">Felügyelt titkosított virtuális gépek (titkosítja BEK és KEK) akkor lesz kell felügyelt toocreate lemezeit a blob storage, és majd csatlakoztassa a hello lemezeket.</span><span class="sxs-lookup"><span data-stu-id="b765d-266">For managed encrypted VMs (encrypted using BEK and KEK), you'll need toocreate managed disks from blob storage, and then attach hello disks.</span></span> <span data-ttu-id="b765d-267">Részletes információkért lásd: hello cikk [csatolni egy adatok lemez tooa Windows virtuális gép PowerShell-lel](../virtual-machines/windows/attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="b765d-267">For in-depth information, see hello article, [Attach a data disk tooa Windows VM using PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span></span> <span data-ttu-id="b765d-268">hello következő mintakód bemutatja, hogyan tooattach hello adatlemezek felügyelt titkosított virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="b765d-268">hello following sample code shows how tooattach hello data disks for managed encrypted VMs.</span></span>

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

5. <span data-ttu-id="b765d-269">Hello hálózati beállításainak megadása.</span><span class="sxs-lookup"><span data-stu-id="b765d-269">Set hello Network settings.</span></span>

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. <span data-ttu-id="b765d-270">Hello virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b765d-270">Create hello virtual machine.</span></span>

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="next-steps"></a><span data-ttu-id="b765d-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b765d-271">Next steps</span></span>
<span data-ttu-id="b765d-272">Ha jobban szeret toouse PowerShell tooengage együtt az Azure-erőforrások, lásd: hello PowerShell cikk [telepítés és a Windows Server biztonsági másolat kezelése](backup-client-automation.md).</span><span class="sxs-lookup"><span data-stu-id="b765d-272">If you prefer toouse PowerShell tooengage with your Azure resources, see hello PowerShell article, [Deploy and Manage Backup for Windows Server](backup-client-automation.md).</span></span> <span data-ttu-id="b765d-273">Ha Ön kezeli a DPM biztonsági mentések, tekintse meg a hello cikket, [telepítés és a DPM a biztonsági mentés kezelése](backup-dpm-automation.md).</span><span class="sxs-lookup"><span data-stu-id="b765d-273">If you manage DPM backups, see hello article, [Deploy and Manage Backup for DPM](backup-dpm-automation.md).</span></span> <span data-ttu-id="b765d-274">Ezek a cikkek mindegyikét verziónál Resource Manager üzembe helyezések vagy a klasszikus telepítések esetén.</span><span class="sxs-lookup"><span data-stu-id="b765d-274">Both of these articles have a version for Resource Manager deployments and Classic deployments.</span></span>  
