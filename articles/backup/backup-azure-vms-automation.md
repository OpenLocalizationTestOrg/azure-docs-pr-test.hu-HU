---
title: "Telepítéséhez és kezeléséhez biztonsági mentések erőforrás-kezelő telepített virtuális gépek PowerShell használatával |} Microsoft Docs"
description: "Használja a PowerShell telepítése és kezelése az Azure biztonsági mentések erőforrás-kezelő telepített virtuális gépekhez"
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
ms.openlocfilehash: 861346a50df6641abb9e454644228146e14b4078
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azurermrecoveryservicesbackup-cmdlets-to-back-up-virtual-machines"></a><span data-ttu-id="e0fb1-103">Készítsen biztonsági másolatot a virtuális gépek AzureRM.RecoveryServices.Backup-parancsmagok használatával</span><span class="sxs-lookup"><span data-stu-id="e0fb1-103">Use AzureRM.RecoveryServices.Backup cmdlets to back up virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e0fb1-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="e0fb1-104">Resource Manager</span></span>](backup-azure-vms-automation.md)
> * [<span data-ttu-id="e0fb1-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="e0fb1-105">Classic</span></span>](backup-azure-vms-classic-automation.md)
>
>

<span data-ttu-id="e0fb1-106">Ez a cikk bemutatja, hogyan Azure PowerShell-parancsmagok segítségével biztonsági mentése és helyreállítása Azure virtuális gép (VM) a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-106">This article shows you how to use Azure PowerShell cmdlets to back up and recover an Azure virtual machine (VM) from a Recovery Services vault.</span></span> <span data-ttu-id="e0fb1-107">Recovery Services-tároló egy Azure Resource Manager-erőforrás és védelmét az adatok és eszközök is az Azure Backup, és az Azure Site Recovery szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-107">A Recovery Services vault is an Azure Resource Manager resource and is used to protect data and assets in both Azure Backup and Azure Site Recovery services.</span></span> <span data-ttu-id="e0fb1-108">Recovery Services-tároló segítségével Azure Service Manager telepített virtuális gépek és az Azure Resource Manager telepített virtuális gépek védelme.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-108">You can use a Recovery Services vault to protect Azure Service Manager-deployed VMs, and Azure Resource Manager-deployed VMs.</span></span>

> [!NOTE]
> <span data-ttu-id="e0fb1-109">Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-109">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="e0fb1-110">Ez a cikk a Resource Manager-modell használatával létrehozott virtuális gépek történő használatra szolgál.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-110">This article is for use with VMs created using the Resource Manager model.</span></span>
>
>

<span data-ttu-id="e0fb1-111">Ez a cikk bemutatja, hogyan PowerShell segítségével a virtuális gép védelme, és az adatok helyreállítását a helyreállítási pontból.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-111">This article walks you through using PowerShell to protect a VM, and restore data from a recovery point.</span></span>

## <a name="concepts"></a><span data-ttu-id="e0fb1-112">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="e0fb1-112">Concepts</span></span>
<span data-ttu-id="e0fb1-113">Ha nem ismeri az Azure Backup szolgáltatással, a szolgáltatás áttekintését kivétele [Mi az Azure Backup?](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-113">If you are not familiar with the Azure Backup service, for an overview of the service, check out [What is Azure Backup?](backup-introduction-to-azure-backup.md)</span></span> <span data-ttu-id="e0fb1-114">Mielőtt elkezdené, győződjön meg arról, hogy vonatkozik-e az Azure biztonsági mentési és a virtuális gép aktuális biztonsági mentési megoldásra vonatkozó korlátozások használatához szükséges előfeltételek kapcsolatos essentials.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-114">Before you start, ensure that you cover the essentials about the prerequisites needed to work with Azure Backup, and the limitations of the current VM backup solution.</span></span>

<span data-ttu-id="e0fb1-115">PowerShell hatékony használatához fontos tudni, hogy a hierarchiában, az objektumok és hol kell elkezdeni az.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-115">To use PowerShell effectively, it is necessary to understand the hierarchy of objects and from where to start.</span></span>

![Helyreállítási szolgáltatások eltér az objektumhierarchia](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

<span data-ttu-id="e0fb1-117">A AzureRm.RecoveryServices.Backup PowerShell parancsmag-referencia megtekintéséhez lásd: a [Azure Backup - helyreállítási szolgáltatások parancsmagjai](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) az Azure-könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-117">To view the AzureRm.RecoveryServices.Backup PowerShell cmdlet reference, see the [Azure Backup - Recovery Services Cmdlets](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup) in the Azure library.</span></span>

## <a name="setup-and-registration"></a><span data-ttu-id="e0fb1-118">Telepítését és regisztrálását</span><span class="sxs-lookup"><span data-stu-id="e0fb1-118">Setup and Registration</span></span>
<span data-ttu-id="e0fb1-119">Megkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="e0fb1-119">To begin:</span></span>

1. <span data-ttu-id="e0fb1-120">[A PowerShell legújabb verziójának letöltése](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (a szükséges minimális verziója: 1.4.0)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-120">[Download the latest version of PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) (the minimum version required is: 1.4.0)</span></span>
2. <span data-ttu-id="e0fb1-121">Keresse meg az Azure biztonsági mentés PowerShell-parancsmagok érhető el a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="e0fb1-121">Find the Azure Backup PowerShell cmdlets available by typing the following command:</span></span>

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


<span data-ttu-id="e0fb1-122">A PowerShell segítségével automatizálhatók a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="e0fb1-122">The following tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="e0fb1-123">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0fb1-123">Create a Recovery Services vault</span></span>
* <span data-ttu-id="e0fb1-124">Azure-beli virtuális gépek biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="e0fb1-124">Back up Azure VMs</span></span>
* <span data-ttu-id="e0fb1-125">A biztonsági mentési feladatot indít</span><span class="sxs-lookup"><span data-stu-id="e0fb1-125">Trigger a backup job</span></span>
* <span data-ttu-id="e0fb1-126">A figyelő egy biztonsági mentési feladat</span><span class="sxs-lookup"><span data-stu-id="e0fb1-126">Monitor a backup job</span></span>
* <span data-ttu-id="e0fb1-127">Állítsa vissza az Azure virtuális gép</span><span class="sxs-lookup"><span data-stu-id="e0fb1-127">Restore an Azure VM</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="e0fb1-128">Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0fb1-128">Create a recovery services vault</span></span>
<span data-ttu-id="e0fb1-129">A következő lépések alapján a Recovery Services-tároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-129">The following steps lead you through creating a Recovery Services vault.</span></span> <span data-ttu-id="e0fb1-130">Recovery Services-tároló nem egyezik egy biztonsági mentési tárolót.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-130">A Recovery Services vault is different than a Backup vault.</span></span>

1. <span data-ttu-id="e0fb1-131">Ha az Azure biztonsági mentés először használ, kell használnia a  **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)**  parancsmag futtatásával regisztrálja az Azure Recovery szolgáltató az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-131">If you are using Azure Backup for the first time, you must use the **[Register-AzureRmResourceProvider](http://docs.microsoft.com/powershell/module/azurerm.resources/register-azurermresourceprovider)** cmdlet to register the Azure Recovery Service provider with your subscription.</span></span>

    ```
    PS C:\> Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```
2. <span data-ttu-id="e0fb1-132">A Recovery Services-tároló egy Resource Manager szerinti erőforrás,, ezért el kell helyezni az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-132">The Recovery Services vault is a Resource Manager resource, so you need to place it within a resource group.</span></span> <span data-ttu-id="e0fb1-133">Használjon egy meglévő erőforráscsoportot, vagy hozzon létre egy erőforráscsoportot a a  **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)**  parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-133">You can use an existing resource group, or create a resource group with the **[New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/azurerm.resources/new-azurermresourcegroup)** cmdlet.</span></span> <span data-ttu-id="e0fb1-134">Erőforráscsoport létrehozásakor meg nevét és helyét, ahhoz az erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-134">When creating a resource group, specify the name and location for the resource group.</span></span>  

    ```
    PS C:\> New-AzureRmResourceGroup –Name "test-rg" –Location "West US"
    ```
3. <span data-ttu-id="e0fb1-135">Használja a  **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)**  parancsmaggal hozhat létre a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-135">Use the **[New-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault)** cmdlet to create the Recovery Services vault.</span></span> <span data-ttu-id="e0fb1-136">Ne felejtse el ugyanazon a helyen, a tároló adja meg, mint az erőforráscsoport használt.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-136">Be sure to specify the same location for the vault as was used for the resource group.</span></span>

    ```
    PS C:\> New-AzureRmRecoveryServicesVault -Name "testvault" -ResourceGroupName " test-rg" -Location "West US"
    ```
4. <span data-ttu-id="e0fb1-137">Megadhatja a használandó; adattároló redundanciája, amely használhat [helyileg redundáns tárolás (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) vagy [földrajzi redundáns tárolás (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-137">Specify the type of storage redundancy to use; you can use [Locally Redundant Storage (LRS)](../storage/common/storage-redundancy.md#locally-redundant-storage) or [Geo Redundant Storage (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage).</span></span> <span data-ttu-id="e0fb1-138">A következő példa bemutatja a - BackupStorageRedundancy beállítás a testvault GeoRedundant értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-138">The following example shows the -BackupStorageRedundancy option for testvault is set to GeoRedundant.</span></span>

    ```
    PS C:\> $vault1 = Get-AzureRmRecoveryServicesVault –Name "testvault"
    PS C:\> Set-AzureRmRecoveryServicesBackupProperties  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > <span data-ttu-id="e0fb1-139">Sok Azure biztonsági mentést készítő parancsmagok bemeneti adatokként a Recovery Services-tároló objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-139">Many Azure Backup cmdlets require the Recovery Services vault object as an input.</span></span> <span data-ttu-id="e0fb1-140">Emiatt célszerű a Recovery Services biztonsági másolat tároló objektum tárolható egy változóban.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-140">For this reason, it is convenient to store the Backup Recovery Services vault object in a variable.</span></span>
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a><span data-ttu-id="e0fb1-141">A tárolók előfizetés megtekintése</span><span class="sxs-lookup"><span data-stu-id="e0fb1-141">View the vaults in a subscription</span></span>
<span data-ttu-id="e0fb1-142">Használjon  **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)**  megtekintéséhez az összes tárolók listája az aktuális előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-142">Use **[Get-AzureRmRecoveryServicesVault](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/get-azurermrecoveryservicesvault)** to view the list of all vaults in the current subscription.</span></span> <span data-ttu-id="e0fb1-143">Ezt a parancsot használhatja, ellenőrizze, hogy létrejött-e egy új tárolót, vagy az előfizetést az elérhető tárolók megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-143">You can use this command to check that a new vault was created, or to see the available vaults in the subscription.</span></span>

<span data-ttu-id="e0fb1-144">Futtassa a parancsot, Get-AzureRmRecoveryServicesVault, az előfizetés összes tárolók megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-144">Run the command, Get-AzureRmRecoveryServicesVault, to view all vaults in the subscription.</span></span> <span data-ttu-id="e0fb1-145">A következő példa bemutatja a minden egyes tároló megjelenő adatokat.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-145">The following example shows the information displayed for each vault.</span></span>

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


## <a name="back-up-azure-vms"></a><span data-ttu-id="e0fb1-146">Azure-beli virtuális gépek biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="e0fb1-146">Back up Azure VMs</span></span>
<span data-ttu-id="e0fb1-147">Recovery Services-tároló segítségével a virtuális gépek védelmére.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-147">Use a Recovery Services vault to protect your virtual machines.</span></span> <span data-ttu-id="e0fb1-148">Alkalmazza a védelmet, mielőtt a tárolóban (a tárolóban lévő védett adatok típusától) környezetben, és a védelmi házirend ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-148">Before you apply the protection, set the vault context (the type of data protected in the vault), and verify the protection policy.</span></span> <span data-ttu-id="e0fb1-149">A védelmi házirend az ütemezés a biztonsági mentési feladatok futtatásakor, és mennyi ideig őrzi meg minden egyes biztonsági mentési pillanatképet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-149">The protection policy is the schedule when the backup jobs run, and how long each backup snapshot is retained.</span></span>

### <a name="set-vault-context"></a><span data-ttu-id="e0fb1-150">Tároló környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="e0fb1-150">Set vault context</span></span>
<span data-ttu-id="e0fb1-151">Ahhoz, hogy a virtuális gép védelme, használjon  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  beállítani a tároló a környezetben.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-151">Before enabling protection on a VM, use **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** to set the vault context.</span></span> <span data-ttu-id="e0fb1-152">A tároló-környezet van beállítva, ha az összes későbbi parancsmag vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-152">Once the vault context is set, it applies to all subsequent cmdlets.</span></span> <span data-ttu-id="e0fb1-153">Az alábbi példa megállapítja a tárolóban, a tároló *testvault*.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-153">The following example sets the vault context for the vault, *testvault*.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesVault -Name "testvault" | Set-AzureRmRecoveryServicesVaultContext
```

### <a name="create-a-protection-policy"></a><span data-ttu-id="e0fb1-154">Védelmi házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0fb1-154">Create a protection policy</span></span>
<span data-ttu-id="e0fb1-155">Recovery Services-tároló létrehozásakor az alapértelmezett védelem és adatmegőrzési ismét.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-155">When you create a Recovery Services vault, it comes with default protection and retention policies.</span></span> <span data-ttu-id="e0fb1-156">Az alapértelmezett védelmi házirendet a biztonsági mentési feladatot, a megadott időpontban naponta váltja ki.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-156">The default protection policy triggers a backup job each day at a specified time.</span></span> <span data-ttu-id="e0fb1-157">Az alapértelmezett megőrzési házirend 30 napig őrzi meg a napi helyreállítási pont.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-157">The default retention policy retains the daily recovery point for 30 days.</span></span> <span data-ttu-id="e0fb1-158">Az alapértelmezett házirend segítségével gyorsan védelme a virtuális Gépet, és később különböző adatokkal házirend szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-158">You can use the default policy to quickly protect your VM and edit the policy later with different details.</span></span>

<span data-ttu-id="e0fb1-159">Használjon  **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)**  az adatvédelmi szabályzatok megtekintéséhez a tárolóban lévő állapottal.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-159">Use **[Get-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupprotectionpolicy)** to view the protection policies in the vault.</span></span> <span data-ttu-id="e0fb1-160">Ez a parancsmag is használhatja, egy adott házirend segítségével, vagy egy munkaterhelés-típushoz tartozó házirendek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-160">You can use this cmdlet to get a specific policy, or to view the policies associated with a workload type.</span></span> <span data-ttu-id="e0fb1-161">Az alábbi példa-házirendet munkaterhelés, AzureVM lekérdezi.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-161">The following example gets policies for workload type, AzureVM.</span></span>

```
PS C:\> Get-AzureRmRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM"
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> <span data-ttu-id="e0fb1-162">Az időzóna PowerShell BackupTime mező UTC.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-162">The timezone of the BackupTime field in PowerShell is UTC.</span></span> <span data-ttu-id="e0fb1-163">Azonban ha az Azure-portálon a biztonságimásolat-készítési időpont látható, az idő módosul az a helyi időzónára.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-163">However, when the backup time is shown in the Azure portal, the time is adjusted to your local timezone.</span></span>
>
>

<span data-ttu-id="e0fb1-164">A biztonsági mentési házirenddel legalább egy megőrzési házirend társítva.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-164">A backup protection policy is associated with at least one retention policy.</span></span> <span data-ttu-id="e0fb1-165">Adatmegőrzési házirend határozza meg, hány helyreállítási pont tartják után törli a rendszer.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-165">Retention policy defines how long a recovery point is kept before it is deleted.</span></span> <span data-ttu-id="e0fb1-166">Használjon  **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)**  megtekintéséhez az alapértelmezett megőrzési házirend.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-166">Use **[Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupretentionpolicyobject)** to view the default retention policy.</span></span>  <span data-ttu-id="e0fb1-167">Hasonló módon használhatja  **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)**  az alapértelmezett ütemezés házirend beszerzéséhez.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-167">Similarly you can use **[Get-AzureRmRecoveryServicesBackupSchedulePolicyObject](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupschedulepolicyobject)** to obtain the default schedule policy.</span></span> <span data-ttu-id="e0fb1-168">A  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  parancsmag létrehoz egy PowerShell-objektum, amely tartalmazza a biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-168">The **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet creates a PowerShell object that holds backup policy information.</span></span> <span data-ttu-id="e0fb1-169">Az ütemezés és a megőrzési csoportházirend-objektumok bemeneteként szolgálnak a  **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)**  parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-169">The schedule and retention policy objects are used as inputs to the **[New-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy)** cmdlet.</span></span> <span data-ttu-id="e0fb1-170">A következő példa az ütemezési házirend és az adatmegőrzési változók tárolja.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-170">The following example stores the schedule policy and the retention policy in variables.</span></span> <span data-ttu-id="e0fb1-171">A példában ezeket a változókat paraméterek megadásához a védelmi házirend létrehozásakor *NewPolicy*.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-171">The example uses those variables to define the parameters when creating a protection policy, *NewPolicy*.</span></span>

```
PS C:\> $schPol = Get-AzureRmRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> New-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```


### <a name="enable-protection"></a><span data-ttu-id="e0fb1-172">Védelem engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e0fb1-172">Enable protection</span></span>
<span data-ttu-id="e0fb1-173">Miután meghatározta a biztonsági mentési házirenddel, továbbra is engedélyeznie kell egy elem a házirendet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-173">Once you have defined the backup protection policy, you still must enable the policy for an item.</span></span> <span data-ttu-id="e0fb1-174">Használjon  **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)**  kívánja engedélyezni a védelmet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-174">Use **[Enable-AzureRmRecoveryServicesBackupProtection](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/enable-azurermrecoveryservicesbackupprotection)** to enable protection.</span></span> <span data-ttu-id="e0fb1-175">Két objektum – az elemet, és a házirend-alapú védelem engedélyezését igényli.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-175">Enabling protection requires two objects - the item and the policy.</span></span> <span data-ttu-id="e0fb1-176">Ha a házirend már társítva van a tárolóban, a biztonsági mentési munkafolyamat kiváltásakor a házirend-ütemezést meghatározott időpontban.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-176">Once the policy has been associated with the vault, the backup workflow is triggered at the time defined in the policy schedule.</span></span>

<span data-ttu-id="e0fb1-177">A következő példa engedélyezi a védelmet a cikkhez V2VM, a házirend, NewPolicy.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-177">The following example enables protection for the item, V2VM, using the policy, NewPolicy.</span></span> <span data-ttu-id="e0fb1-178">Engedélyezze a védelmet a nem titkosított erőforrás-kezelő virtuális gépeken</span><span class="sxs-lookup"><span data-stu-id="e0fb1-178">To enable the protection on non-encrypted Resource Manager VMs</span></span>

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

<span data-ttu-id="e0fb1-179">Titkosított virtuális gépeken (titkosítja BEK és KEK) a védelem engedélyezéséhez szükséges engedélyt az Azure Backup szolgáltatás kulcsok és titkos olvasni kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-179">To enable the protection on encrypted VMs (encrypted using BEK and KEK), you need to give the Azure Backup service permission to read keys and secrets from key vault.</span></span>

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

<span data-ttu-id="e0fb1-180">A védelem engedélyezése az titkosítja a virtuális gépek (titkosítja BEK csak), hozzá kell rendelnie az Azure Backup szolgáltatás engedély megnyithassa a kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-180">To enable the protection on encrypted VMs (encrypted using BEK only), you need to give the Azure Backup service permission to read secrets from key vault.</span></span>

```
PS C:\> Set-AzureRmKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1"
```

> [!NOTE]
> <span data-ttu-id="e0fb1-181">Ha az Azure Government felhő használja, használja a érték ff281ffe-705c-4f53-9f37-a40e6f2c68f3 paraméter **- ServicePrincipalName** a [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-181">If you are using the Azure Government cloud, then use the value ff281ffe-705c-4f53-9f37-a40e6f2c68f3 for the parameter **-ServicePrincipalName** in [Set-AzureRmKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet.</span></span>
>
>

<span data-ttu-id="e0fb1-182">Klasszikus virtuális gépekhez</span><span class="sxs-lookup"><span data-stu-id="e0fb1-182">For classic VMs</span></span>

```
PS C:\> $pol=Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Enable-AzureRmRecoveryServicesBackupProtection -Policy $pol -Name "V1VM" -ServiceName "ServiceName1"
```

### <a name="modify-a-protection-policy"></a><span data-ttu-id="e0fb1-183">A védelmi házirend módosítása</span><span class="sxs-lookup"><span data-stu-id="e0fb1-183">Modify a protection policy</span></span>
<span data-ttu-id="e0fb1-184">A védelmi házirend módosításához használható [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) a SchedulePolicy vagy RetentionPolicy objektumok módosítására.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-184">To modify the protection policy, use [Set-AzureRmRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/set-azurermrecoveryservicesbackupprotectionpolicy) to modify the SchedulePolicy or RetentionPolicy objects.</span></span>

<span data-ttu-id="e0fb1-185">A következő példa a helyreállítási pontok megőrzésének ideje 365 nap módosításait.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-185">The following example changes the recovery point retention to 365 days.</span></span>

```
PS C:\> $retPol = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
PS C:\> $retPol.DailySchedule.DurationCountInDays = 365
PS C:\> $pol= Get-AzureRmRecoveryServicesBackupProtectionPolicy -Name "NewPolicy"
PS C:\> Set-AzureRmRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol
```

## <a name="trigger-a-backup"></a><span data-ttu-id="e0fb1-186">A biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="e0fb1-186">Trigger a backup</span></span>
<span data-ttu-id="e0fb1-187">Használhat  **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  a biztonsági mentési feladatot indít.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-187">You can use **[Backup-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)** to trigger a backup job.</span></span> <span data-ttu-id="e0fb1-188">Ha a kezdeti biztonsági másolatot, akkor egy teljes biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-188">If it is the initial backup, it is a full backup.</span></span> <span data-ttu-id="e0fb1-189">Azt követő biztonsági mentéseket egy növekményes másolatot igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-189">Subsequent backups take an incremental copy.</span></span> <span data-ttu-id="e0fb1-190">Használjon  **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  időt. a biztonsági mentési feladat előtt állítsa be a tároló környezetében.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-190">Be sure to use **[Set-AzureRmRecoveryServicesVaultContext](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)** to set the vault context before triggering the backup job.</span></span> <span data-ttu-id="e0fb1-191">Az alábbi példa azt feltételezi, hogy a tároló környezet beállítása történt.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-191">The following example assumes vault context was set.</span></span>

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM"
PS C:\> $item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM"
PS C:\> $job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup               InProgress            4/23/2016 5:00:30 PM                       cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> <span data-ttu-id="e0fb1-192">A StartTime és az EndTime megadása mezők PowerShell időzóna UTC.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-192">The timezone of the StartTime and EndTime fields in PowerShell is UTC.</span></span> <span data-ttu-id="e0fb1-193">Azonban az idő az Azure portálon megjelenítésekor a idő módosul az a helyi időzónára.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-193">However, when the time is shown in the Azure portal, the time is adjusted to your local timezone.</span></span>
>
>

## <a name="monitoring-a-backup-job"></a><span data-ttu-id="e0fb1-194">A biztonsági mentési feladatot figyelése</span><span class="sxs-lookup"><span data-stu-id="e0fb1-194">Monitoring a backup job</span></span>
<span data-ttu-id="e0fb1-195">Hosszú ideig futó műveletek, például a biztonsági mentési feladatok segítségével figyelheti az Azure portál használata nélkül.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-195">You can monitor long-running operations, such as backup jobs, without using the Azure portal.</span></span> <span data-ttu-id="e0fb1-196">Ahhoz, hogy az egy folyamatban lévő feladat állapotát, használja a  **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)**  parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-196">To get the status of an in-progress job, use the **[Get-AzureRmRecoveryservicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjob)** cmdlet.</span></span> <span data-ttu-id="e0fb1-197">Ez a parancsmag lekérdezi a biztonsági mentési feladatok számára egy adott tárolóban, és adott tárolóhoz van megadva a tároló a környezetben.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-197">This cmdlet gets the backup jobs for a specific vault, and that vault is specified in the vault context.</span></span> <span data-ttu-id="e0fb1-198">A következő példa egy folyamatban lévő feladat tömbként állapotát olvassa be, és a $joblist változó állapotát tárolja.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-198">The following example gets the status of an in-progress job as an array, and stores the status in the $joblist variable.</span></span>

```
PS C:\> $joblist = Get-AzureRmRecoveryservicesBackupJob –Status "InProgress"
PS C:\> $joblist[0]
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016 5:00:30 PM           cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

<span data-ttu-id="e0fb1-199">Ezek a feladatok befejezésére – ami szükségtelen kód - lekérdezési helyett használja a  **[várakozási-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-199">Instead of polling these jobs for completion - which is unnecessary additional code - use the **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet.</span></span> <span data-ttu-id="e0fb1-200">Ez a parancsmag végrehajtása felfüggesztése, addig, amíg a feladat befejeződik, vagy a megadott időtúllépési érték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-200">This cmdlet pauses the execution until either the job completes or the specified timeout value is reached.</span></span>

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200
```

## <a name="restore-an-azure-vm"></a><span data-ttu-id="e0fb1-201">Állítsa vissza az Azure virtuális gép</span><span class="sxs-lookup"><span data-stu-id="e0fb1-201">Restore an Azure VM</span></span>
<span data-ttu-id="e0fb1-202">A visszaállítását egy virtuális Gépet az Azure portál használatával és visszaállítása a PowerShell virtuális gépek közötti fő különbség van.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-202">There is a key difference between the restoring a VM using the Azure portal and restoring a VM using PowerShell.</span></span> <span data-ttu-id="e0fb1-203">A PowerShell használatával a visszaállítás befejeződött a lemezek és a konfigurációs adatokat a helyreállítási pont létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-203">With PowerShell, the restore operation is complete once the disks and configuration information from the recovery point are created.</span></span>

> [!NOTE]
> <span data-ttu-id="e0fb1-204">A visszaállítási művelet nem hoz létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-204">The restore operation does not create a virtual machine.</span></span>
>
>

<span data-ttu-id="e0fb1-205">A virtuális gép létrehozása a lemezről, című szakaszban [a virtuális gép létrehozása tárolt lemezekből](backup-azure-vms-automation.md#create-a-vm-from-stored-disks).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-205">To create a virtual machine from disk, see the section, [Create the VM from stored disks](backup-azure-vms-automation.md#create-a-vm-from-stored-disks).</span></span> <span data-ttu-id="e0fb1-206">Az alapvető lépéseken, egy Azure virtuális gép visszaállítására a következők:</span><span class="sxs-lookup"><span data-stu-id="e0fb1-206">The basic steps to restore an Azure VM are:</span></span>

* <span data-ttu-id="e0fb1-207">Válassza ki a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="e0fb1-207">Select the VM</span></span>
* <span data-ttu-id="e0fb1-208">A helyreállítási pont kiválasztása</span><span class="sxs-lookup"><span data-stu-id="e0fb1-208">Choose a recovery point</span></span>
* <span data-ttu-id="e0fb1-209">A lemezek visszaállítása</span><span class="sxs-lookup"><span data-stu-id="e0fb1-209">Restore the disks</span></span>
* <span data-ttu-id="e0fb1-210">A virtuális gép létrehozása tárolt lemezekből</span><span class="sxs-lookup"><span data-stu-id="e0fb1-210">Create the VM from stored disks</span></span>

<span data-ttu-id="e0fb1-211">A következő ábra a eltér az objektumhierarchia le a BackupRecoveryPoint RecoveryServicesVault a jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-211">The following graphic shows the object hierarchy from the RecoveryServicesVault down to the BackupRecoveryPoint.</span></span>

![Helyreállítási szolgáltatások eltér az objektumhierarchia BackupContainer megjelenítése](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

<span data-ttu-id="e0fb1-213">Biztonsági mentési adatok helyreállítását, a biztonsági másolat elem és a helyreállítási pont a időpontban adatokat tartalmazó azonosítása.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-213">To restore backup data, identify the backed-up item and the recovery point that holds the point-in-time data.</span></span> <span data-ttu-id="e0fb1-214">Használja a  **[visszaállítási-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  parancsmagot, hogy a tároló vissza adatokat a felhasználói fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-214">Use the **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet to restore data from the vault to the customer's account.</span></span>

### <a name="select-the-vm"></a><span data-ttu-id="e0fb1-215">Válassza ki a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="e0fb1-215">Select the VM</span></span>
<span data-ttu-id="e0fb1-216">Ahhoz, hogy a PowerShell-objektum, amely a helyes biztonságimásolat-elem azonosítja, indítsa el a tárolóban lévő-tárolójából, és az objektum egy hierarchiában lejjebb lévő módon működnek.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-216">To get the PowerShell object that identifies the right backup item, start from the container in the vault, and work your way down the object hierarchy.</span></span> <span data-ttu-id="e0fb1-217">Válassza ki a tárolóhoz, amelybe a virtuális Gépet jelöl, használja a  **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)**  parancsmag és a csövön keresztüli, hogy a  **[ Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)**  parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-217">To select the container that represents the VM, use the **[Get-AzureRmRecoveryServicesBackupContainer](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)** cmdlet and pipe that to the **[Get-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)** cmdlet.</span></span>

```
PS C:\> $namedContainer = Get-AzureRmRecoveryServicesBackupContainer  -ContainerType "AzureVM" –Status "Registered" -FriendlyName "V2VM"
PS C:\> $backupitem = Get-AzureRmRecoveryServicesBackupItem –Container $namedContainer  –WorkloadType "AzureVM"
```

### <a name="choose-a-recovery-point"></a><span data-ttu-id="e0fb1-218">A helyreállítási pont kiválasztása</span><span class="sxs-lookup"><span data-stu-id="e0fb1-218">Choose a recovery point</span></span>
<span data-ttu-id="e0fb1-219">Használja a  **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)**  parancsmagot, hogy az összes helyreállítási pontról biztonsági mentési elem listán.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-219">Use the **[Get-AzureRmRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)** cmdlet to list all recovery points for the backup item.</span></span> <span data-ttu-id="e0fb1-220">Válassza ki a visszaállítani kívánt helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-220">Then choose the recovery point to restore.</span></span> <span data-ttu-id="e0fb1-221">Ha biztos abban, hogy melyik helyreállítási pontot szeretné használni, ajánlott válassza ki a legutóbbi RecoveryPointType = AppConsistent pontot a listában.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-221">If you are unsure which recovery point to use, it is a good practice to choose the most recent RecoveryPointType = AppConsistent point in the list.</span></span>

<span data-ttu-id="e0fb1-222">A következő parancsfájlt, a változó a **$rp**, helyreállítási pontot készíteni a kijelölt biztonsági mentési elemet, az elmúlt hét napban tömbje.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-222">In the following script, the variable, **$rp**, is an array of recovery points for the selected backup item, from the past seven days.</span></span> <span data-ttu-id="e0fb1-223">A tömb fordított sorrendben rendezve idő 0 indexnél a legújabb helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-223">The array is sorted in reverse order of time with the latest recovery point at index 0.</span></span> <span data-ttu-id="e0fb1-224">Standard PowerShell tömb indexelő segítségével válassza ki a helyreállítási pont.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-224">Use standard PowerShell array indexing to pick the recovery point.</span></span> <span data-ttu-id="e0fb1-225">A példában $rp [0] választja ki a legutóbbi helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-225">In the example, $rp[0] selects the latest recovery point.</span></span>

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



### <a name="restore-the-disks"></a><span data-ttu-id="e0fb1-226">A lemezek visszaállítása</span><span class="sxs-lookup"><span data-stu-id="e0fb1-226">Restore the disks</span></span>
<span data-ttu-id="e0fb1-227">Használja a  **[visszaállítási-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)**  parancsmag történő visszaállításához, egy biztonsági mentési elem adatot és konfigurációs egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-227">Use the **[Restore-AzureRmRecoveryServicesBackupItem](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/restore-azurermrecoveryservicesbackupitem)** cmdlet to restore a backup item's data and configuration to a recovery point.</span></span> <span data-ttu-id="e0fb1-228">Ha azonosított egy helyreállítási pontot, az legyen értékét a **- RecoveryPoint** paraméter.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-228">Once you have identified a recovery point, use it as the value for the **-RecoveryPoint** parameter.</span></span> <span data-ttu-id="e0fb1-229">Előző példakód **$rp [0]** volt a helyreállítási pontot szeretné használni.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-229">In the previous sample code, **$rp[0]** was the recovery point to use.</span></span> <span data-ttu-id="e0fb1-230">Az alábbi példakód a **$rp [0]** van a lemez helyreállításához használni kívánt helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-230">In the following sample code, **$rp[0]** is the recovery point to use for restoring the disk.</span></span>

<span data-ttu-id="e0fb1-231">A lemezek és a konfigurációs adatok visszaállítása:</span><span class="sxs-lookup"><span data-stu-id="e0fb1-231">To restore the disks and configuration information:</span></span>

```
PS C:\> $restorejob = Restore-AzureRmRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG"
PS C:\> $restorejob
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

<span data-ttu-id="e0fb1-232">Használja a  **[várakozási-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)**  parancsmag a visszaállítási feladat befejeződésére vár.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-232">Use the **[Wait-AzureRmRecoveryServicesBackupJob](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/wait-azurermrecoveryservicesbackupjob)** cmdlet to wait for the Restore job to complete.</span></span>

```
PS C:\> Wait-AzureRmRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

<span data-ttu-id="e0fb1-233">A visszaállítási feladat befejezése után használja a  **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)**  parancsmagot, hogy megkapja a visszaállítás részleteit.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-233">Once the Restore job has completed, use the **[Get-AzureRmRecoveryServicesBackupJobDetails](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupjobdetails)** cmdlet to get the details of the restore operation.</span></span> <span data-ttu-id="e0fb1-234">A JobDetails tulajdonsághoz nem tartozik az információk szükségesek ahhoz, hogy a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-234">The JobDetails property has the information needed to rebuild the VM.</span></span>

```
PS C:\> $restorejob = Get-AzureRmRecoveryServicesBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmRecoveryServicesBackupJobDetails -Job $restorejob
```

<span data-ttu-id="e0fb1-235">Miután helyreállította a lemezeket, nyissa meg a virtuális gép létrehozásához a következő szakasszal.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-235">Once you restore the disks, go to the next section to create the VM.</span></span>

## <a name="create-a-vm-from-restored-disks"></a><span data-ttu-id="e0fb1-236">Hozzon létre egy virtuális Gépet visszaállított lemezekből</span><span class="sxs-lookup"><span data-stu-id="e0fb1-236">Create a VM from restored disks</span></span>
<span data-ttu-id="e0fb1-237">Miután visszaállította a lemezeket, ezek lépések segítségével hozza létre és konfigurálja a virtuális gép lemezéről.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-237">After you have restored the disks, use these steps to create and configure the virtual machine from disk.</span></span>

> [!NOTE]
> <span data-ttu-id="e0fb1-238">Titkosított virtuális gépek létrehozásához visszaállított lemezekről, az Azure szerepkör a művelet végrehajtásához szükséges engedéllyel kell rendelkeznie **Microsoft.KeyVault/vaults/deploy/action**.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-238">To create encrypted VMs from restored disks, your Azure role must have permission to perform the action, **Microsoft.KeyVault/vaults/deploy/action**.</span></span> <span data-ttu-id="e0fb1-239">Ha a szerepkör nem rendelkezik ezzel az engedéllyel, hozzon létre egy egyéni biztonsági szerepkört ezt a műveletet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-239">If your role does not have this permission, create a custom role with this action.</span></span> <span data-ttu-id="e0fb1-240">További információkért lásd: [egyéni szerepkörök az Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-240">For more information, see [Custom Roles in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md).</span></span>
>
>

1. <span data-ttu-id="e0fb1-241">A feladat részleteit a visszaállított lemez tulajdonságainak lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-241">Query the restored disk properties for the job details.</span></span>

  ```
  PS C:\> $properties = $details.properties
  PS C:\> $storageAccountName = $properties["Target Storage Account Name"]
  PS C:\> $containerName = $properties["Config Blob Container Name"]
  PS C:\> $blobName = $properties["Config Blob Name"]
  ```

2. <span data-ttu-id="e0fb1-242">Állítsa be az Azure storage-környezetben, és állítsa vissza a JSON-konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-242">Set the Azure storage context and restore the JSON configuration file.</span></span>

    ```
    PS C:\> Set-AzureRmCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
    PS C:\> $destination_path = "C:\vmconfig.json"
    PS C:\> Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path
    PS C:\> $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
    ```

3. <span data-ttu-id="e0fb1-243">A JSON-konfigurációs fájlt használja a Virtuálisgép-konfiguráció létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-243">Use the JSON configuration file to create the VM configuration.</span></span>

    ```
   PS C:\> $vm = New-AzureRmVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
    ```

4. <span data-ttu-id="e0fb1-244">Az operációsrendszer-lemez és adatlemezek csatolni.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-244">Attach the OS disk and data disks.</span></span> <span data-ttu-id="e0fb1-245">Attól függően, hogy a virtuális gépek konfigurációját kattintson a megfelelő parancsmagok megtekintéséhez megfelelő hivatkozásra:</span><span class="sxs-lookup"><span data-stu-id="e0fb1-245">Depending on the configuration of your VMs, click on the relevant link to view respective cmdlets:</span></span> 
    - [<span data-ttu-id="e0fb1-246">A nem felügyelt, nem titkosított virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="e0fb1-246">Non-managed, non-encrypted VMs</span></span>](#non-managed-non-encrypted-vms)
    - [<span data-ttu-id="e0fb1-247">A nem felügyelt, titkosított virtuális gépek (csak BEK)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-247">Non-managed, encrypted VMs (BEK only)</span></span>](#non-managed-encrypted-vms-bek-only)
    - [<span data-ttu-id="e0fb1-248">A nem felügyelt, titkosított virtuális gépek (BEK és KEK)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-248">Non-managed, encrypted VMs (BEK and KEK)</span></span>](#non-managed-encrypted-vms-bek-and-kek)
    - [<span data-ttu-id="e0fb1-249">Felügyelt, nem titkosított virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="e0fb1-249">Managed, non-encrypted VMs</span></span>](#managed-non-encrypted-vms)
    - [<span data-ttu-id="e0fb1-250">Felügyelt, titkosított virtuális gépek (BEK és KEK)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-250">Managed, encrypted VMs (BEK and KEK)</span></span>](#managed-encrypted-vms-bek-and-kek)
    
    #### <a name="non-managed-non-encrypted-vms"></a><span data-ttu-id="e0fb1-251">A nem felügyelt, nem titkosított virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="e0fb1-251">Non-managed, non-encrypted VMs</span></span>

    <span data-ttu-id="e0fb1-252">Használja az alábbi minta nem kezelt, nem titkosított virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-252">Use the following sample for non-managed, non-encrypted VMs.</span></span>

    ```
    PS C:\> Set-AzureRmVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
    PS C:\> $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
    PS C:\> foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
    {
    $vm = Add-AzureRmVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
    }
    ```

    #### <a name="non-managed-encrypted-vms-bek-only"></a><span data-ttu-id="e0fb1-253">A nem felügyelt, titkosított virtuális gépek (csak BEK)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-253">Non-managed, encrypted VMs (BEK only)</span></span>

    <span data-ttu-id="e0fb1-254">Nem kezelt, titkosított virtuális gépek (titkosítja BEK csak) a titkos kulcsot a key vault visszaállításához, mielőtt csatolható lemezek kell.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-254">For non-managed, encrypted VMs (encrypted using BEK only), you need to restore the secret to the key vault before you can attach disks.</span></span> <span data-ttu-id="e0fb1-255">További információkért lásd: a cikk [visszaállítani egy titkosított virtuális gépet az Azure biztonsági mentési helyreállítási pontokról](backup-azure-restore-key-secret.md).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-255">For more information, please see the article, [Restore an encrypted virtual machine from an Azure Backup recovery point](backup-azure-restore-key-secret.md).</span></span> <span data-ttu-id="e0fb1-256">A következő példa bemutatja, hogyan operációsrendszer- és adatlemezek titkosított virtuális géphez csatolása.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-256">The following sample shows how to attach OS and data disks for encrypted VMs.</span></span>

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

    #### <a name="non-managed-encrypted-vms-bek-and-kek"></a><span data-ttu-id="e0fb1-257">A nem felügyelt, titkosított virtuális gépek (BEK és KEK)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-257">Non-managed, encrypted VMs (BEK and KEK)</span></span>

    <span data-ttu-id="e0fb1-258">Nem kezelt, titkosított virtuális gépen (titkosítja BEK és KEK) kell visszaállítani a kulcsot és titkos kulcs a key vault előtt lemezek.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-258">For non-managed, encrypted VMs (encrypted using BEK and KEK), you need to restore the key and secret to the key vault before you can attach disks.</span></span> <span data-ttu-id="e0fb1-259">További információkért lásd: a cikk [visszaállítani egy titkosított virtuális gépet az Azure biztonsági mentési helyreállítási pontokról](backup-azure-restore-key-secret.md).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-259">For more information, please see the article, [Restore an encrypted virtual machine from an Azure Backup recovery point](backup-azure-restore-key-secret.md).</span></span> <span data-ttu-id="e0fb1-260">A következő példa bemutatja, hogyan operációsrendszer- és adatlemezek titkosított virtuális géphez csatolása.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-260">The following sample shows how to attach OS and data disks for encrypted VMs.</span></span>

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

    #### <a name="managed-non-encrypted-vms"></a><span data-ttu-id="e0fb1-261">Felügyelt, nem titkosított virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="e0fb1-261">Managed, non-encrypted VMs</span></span>

    <span data-ttu-id="e0fb1-262">Felügyelt nem titkosított virtuális gépekhez szüksége lesz felügyelt lemezek létrehozásához az blob-tárolóból, és majd csatlakoztassa a lemezeket.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-262">For managed non-encrypted VMs, you'll need to create managed disks from blob storage, and then attach the disks.</span></span> <span data-ttu-id="e0fb1-263">Részletes információkért lásd: a cikk [adatlemezt csatolni egy Windows-VM PowerShell-lel](../virtual-machines/windows/attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-263">For in-depth information, see the article, [Attach a data disk to a Windows VM using PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span></span> <span data-ttu-id="e0fb1-264">Az alábbi mintakód bemutatja, hogyan felügyelt nem titkosított virtuális gépek a adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-264">The following sample code shows how to attach the data disks for managed non-encrypted VMs.</span></span>

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

    #### <a name="managed-encrypted-vms-bek-and-kek"></a><span data-ttu-id="e0fb1-265">Felügyelt, titkosított virtuális gépek (BEK és KEK)</span><span class="sxs-lookup"><span data-stu-id="e0fb1-265">Managed, encrypted VMs (BEK and KEK)</span></span>

    <span data-ttu-id="e0fb1-266">Felügyelt titkosított virtuális gépek (titkosítja BEK és KEK) szüksége lesz felügyelt lemezek létrehozásához az blob-tárolóból, és majd csatlakoztassa a lemezeket.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-266">For managed encrypted VMs (encrypted using BEK and KEK), you'll need to create managed disks from blob storage, and then attach the disks.</span></span> <span data-ttu-id="e0fb1-267">Részletes információkért lásd: a cikk [adatlemezt csatolni egy Windows-VM PowerShell-lel](../virtual-machines/windows/attach-disk-ps.md).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-267">For in-depth information, see the article, [Attach a data disk to a Windows VM using PowerShell](../virtual-machines/windows/attach-disk-ps.md).</span></span> <span data-ttu-id="e0fb1-268">Az alábbi mintakód bemutatja, hogyan felügyelt titkosított virtuális gépek a adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-268">The following sample code shows how to attach the data disks for managed encrypted VMs.</span></span>

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

5. <span data-ttu-id="e0fb1-269">A hálózati beállításainak megadása.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-269">Set the Network settings.</span></span>

    ```
    PS C:\> $nicName="p1234"
    PS C:\> $pip = New-AzureRmPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    PS C:\> $vnet = Get-AzureRmVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    PS C:\> $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    PS C:\> $vm=Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
    ```
6. <span data-ttu-id="e0fb1-270">Hozza létre a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-270">Create the virtual machine.</span></span>

    ```    
    PS C:\> New-AzureRmVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

## <a name="next-steps"></a><span data-ttu-id="e0fb1-271">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0fb1-271">Next steps</span></span>
<span data-ttu-id="e0fb1-272">Ha jobban szeret PowerShell használata az Azure-erőforrások bevonásához, olvassa el a PowerShell, [telepítés és a Windows Server biztonsági másolat kezelése](backup-client-automation.md).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-272">If you prefer to use PowerShell to engage with your Azure resources, see the PowerShell article, [Deploy and Manage Backup for Windows Server](backup-client-automation.md).</span></span> <span data-ttu-id="e0fb1-273">DPM biztonsági mentések kezelése, tekintse meg a cikket, [telepítés és a DPM a biztonsági mentés kezelése](backup-dpm-automation.md).</span><span class="sxs-lookup"><span data-stu-id="e0fb1-273">If you manage DPM backups, see the article, [Deploy and Manage Backup for DPM](backup-dpm-automation.md).</span></span> <span data-ttu-id="e0fb1-274">Ezek a cikkek mindegyikét verziónál Resource Manager üzembe helyezések vagy a klasszikus telepítések esetén.</span><span class="sxs-lookup"><span data-stu-id="e0fb1-274">Both of these articles have a version for Resource Manager deployments and Classic deployments.</span></span>  
