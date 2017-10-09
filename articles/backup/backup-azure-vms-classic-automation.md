---
title: "aaaDeploy és a biztonsági mentés kezelése az Azure virtuális gépek PowerShell használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy és kezelése az Azure Backup szolgáltatáshoz a PowerShell használatával."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 2e24b1d9-4375-4049-a28d-e3bc01152f32
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;trinadhk;jimpark
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f3ecd3f94c5e3e8fc8018e8786302edd4847744b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azurermbackup-cmdlets-tooback-up-virtual-machines"></a><span data-ttu-id="19d8a-103">Virtuális gépek AzureRM.Backup parancsmagok tooback használata</span><span class="sxs-lookup"><span data-stu-id="19d8a-103">Use AzureRM.Backup cmdlets tooback up virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="19d8a-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="19d8a-104">Resource Manager</span></span>](backup-azure-vms-automation.md)
> * [<span data-ttu-id="19d8a-105">Klasszikus</span><span class="sxs-lookup"><span data-stu-id="19d8a-105">Classic</span></span>](backup-azure-vms-classic-automation.md)
>
>

<span data-ttu-id="19d8a-106">Ez a cikk bemutatja, hogyan toouse Azure PowerShell, a biztonsági mentése és helyreállítása Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="19d8a-106">This article shows you how toouse Azure PowerShell for backup and recovery of Azure VMs.</span></span> <span data-ttu-id="19d8a-107">Az Azure két különböző üzemi modellel rendelkezik az erőforrások létrehozásához és használatához: Resource Manager-alapú és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="19d8a-107">Azure has two different deployment models for creating and working with resources: Resource Manager and Classic.</span></span> <span data-ttu-id="19d8a-108">Ez a cikk ismerteti, használatával hello klasszikus telepítési modell tooback be adatok tooa Backup-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="19d8a-108">This article covers using hello Classic deployment model tooback up data tooa Backup vault.</span></span> <span data-ttu-id="19d8a-109">Ha nem hozott létre a biztonsági másolatok tárolóját az előfizetéshez, lásd: hello Resource Manager verziója, ez a cikk [használata AzureRM.RecoveryServices.Backup parancsmagok tooback virtuális gépek](backup-azure-vms-automation.md).</span><span class="sxs-lookup"><span data-stu-id="19d8a-109">If you have not created a Backup vault in your subscription, see hello Resource Manager version of this article, [Use AzureRM.RecoveryServices.Backup cmdlets tooback up virtual machines](backup-azure-vms-automation.md).</span></span> <span data-ttu-id="19d8a-110">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="19d8a-110">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="19d8a-111">Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban.</span><span class="sxs-lookup"><span data-stu-id="19d8a-111">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="19d8a-112">További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="19d8a-112">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="19d8a-113">A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="19d8a-113">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="19d8a-114">2017. október 15. után PowerShell toocreate mentési tárolókban nem használható.</span><span class="sxs-lookup"><span data-stu-id="19d8a-114">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="19d8a-115">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="19d8a-115">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="19d8a-116">Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="19d8a-116">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="19d8a-117">Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="19d8a-117">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="19d8a-118">Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.</span><span class="sxs-lookup"><span data-stu-id="19d8a-118">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="concepts"></a><span data-ttu-id="19d8a-119">Alapelvek</span><span class="sxs-lookup"><span data-stu-id="19d8a-119">Concepts</span></span>
<span data-ttu-id="19d8a-120">Ez a cikk adott toohello PowerShell-parancsmagok használt virtuális gépek tooback információkat biztosít.</span><span class="sxs-lookup"><span data-stu-id="19d8a-120">This article provides information specific toohello PowerShell cmdlets used tooback up virtual machines.</span></span> <span data-ttu-id="19d8a-121">Az Azure virtuális gépek védelméről bevezető információkat talál [tervezze meg a virtuális gép biztonsági mentési infrastruktúra az Azure-ban](backup-azure-vms-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="19d8a-121">For introductory information about protecting Azure VMs, please see [Plan your VM backup infrastructure in Azure](backup-azure-vms-introduction.md).</span></span>

> [!NOTE]
> <span data-ttu-id="19d8a-122">Kezdés előtt olvassa el a hello [Előfeltételek](backup-azure-vms-prepare.md) az Azure biztonsági mentési és hello szükséges toowork [korlátozások](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) hello aktuális virtuális gép biztonsági mentési megoldás.</span><span class="sxs-lookup"><span data-stu-id="19d8a-122">Before you start, read hello [prerequisites](backup-azure-vms-prepare.md) required toowork with Azure Backup, and hello [limitations](backup-azure-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm) of hello current VM backup solution.</span></span>
>
>

<span data-ttu-id="19d8a-123">toouse PowerShell hatékony, eltarthat egy rövid ideig toounderstand hello hierarchiában, az objektumok és honnan toostart.</span><span class="sxs-lookup"><span data-stu-id="19d8a-123">toouse PowerShell effectively, take a moment toounderstand hello hierarchy of objects and from where toostart.</span></span>

![Eltér az objektumhierarchia](./media/backup-azure-vms-classic-automation/object-hierarchy.png)

<span data-ttu-id="19d8a-125">hello két legfontosabb adatfolyamok engedélyezze a virtuális gép védelmét, és adatok helyreállításához a helyreállítási pontból.</span><span class="sxs-lookup"><span data-stu-id="19d8a-125">hello two most important flows are enabling protection for a VM, and restoring data from a recovery point.</span></span> <span data-ttu-id="19d8a-126">hello Ez a cikk célja toohelp adept, két forgatókönyvekben hello PowerShell parancsmagok tooenable használata válik.</span><span class="sxs-lookup"><span data-stu-id="19d8a-126">hello focus of this article is toohelp you become adept at working with hello PowerShell cmdlets tooenable these two scenarios.</span></span>

## <a name="setup-and-registration"></a><span data-ttu-id="19d8a-127">Telepítését és regisztrálását</span><span class="sxs-lookup"><span data-stu-id="19d8a-127">Setup and Registration</span></span>
<span data-ttu-id="19d8a-128">toobegin:</span><span class="sxs-lookup"><span data-stu-id="19d8a-128">toobegin:</span></span>

1. <span data-ttu-id="19d8a-129">[Töltse le a legfrissebb PowerShell](https://github.com/Azure/azure-powershell/releases) (szükséges minimális verziója: 1.0.0)</span><span class="sxs-lookup"><span data-stu-id="19d8a-129">[Download latest PowerShell](https://github.com/Azure/azure-powershell/releases) (minimum version required is : 1.0.0)</span></span>
2. <span data-ttu-id="19d8a-130">Keresse meg a rendelkezésre álló hello Azure biztonsági mentés PowerShell-parancsmagok hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="19d8a-130">Find hello Azure Backup PowerShell cmdlets available by typing hello following command:</span></span>

```
PS C:\> Get-Command *azurermbackup*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Backup-AzureRmBackupItem                           1.0.1      AzureRM.Backup
Cmdlet          Disable-AzureRmBackupProtection                    1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupContainerReregistration        1.0.1      AzureRM.Backup
Cmdlet          Enable-AzureRmBackupProtection                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupContainer                         1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupItem                              1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJob                               1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupJobDetails                        1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupRecoveryPoint                     1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Get-AzureRmBackupVaultCredentials                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupRetentionPolicyObject             1.0.1      AzureRM.Backup
Cmdlet          New-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Register-AzureRmBackupContainer                    1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupProtectionPolicy               1.0.1      AzureRM.Backup
Cmdlet          Remove-AzureRmBackupVault                          1.0.1      AzureRM.Backup
Cmdlet          Restore-AzureRmBackupItem                          1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupProtectionPolicy                  1.0.1      AzureRM.Backup
Cmdlet          Set-AzureRmBackupVault                             1.0.1      AzureRM.Backup
Cmdlet          Stop-AzureRmBackupJob                              1.0.1      AzureRM.Backup
Cmdlet          Unregister-AzureRmBackupContainer                  1.0.1      AzureRM.Backup
Cmdlet          Wait-AzureRmBackupJob                              1.0.1      AzureRM.Backup
```

<span data-ttu-id="19d8a-131">hello a következő és nyilvántartási feladatok automatizálhatók a PowerShell használatával:</span><span class="sxs-lookup"><span data-stu-id="19d8a-131">hello following setup and registration tasks can be automated with PowerShell:</span></span>

* <span data-ttu-id="19d8a-132">Backup-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="19d8a-132">Create a backup vault</span></span>
* <span data-ttu-id="19d8a-133">Hello virtuális gépek regisztrálása hello Azure Backup szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="19d8a-133">Registering hello VMs with hello Azure Backup service</span></span>

### <a name="create-a-backup-vault"></a><span data-ttu-id="19d8a-134">Backup-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="19d8a-134">Create a backup vault</span></span>
> [!WARNING]
> <span data-ttu-id="19d8a-135">A felhasználó Azure Backup segítségével hello az első alkalommal esetén tooregister hello Azure biztonsági mentés szolgáltató toobe használja az előfizetéskor kell.</span><span class="sxs-lookup"><span data-stu-id="19d8a-135">For customers using Azure Backup for hello first time, you need tooregister hello Azure Backup provider toobe used with your subscription.</span></span> <span data-ttu-id="19d8a-136">Ezt megteheti hello a következő parancs futtatásával: Register-AzureRmResourceProvider - ProviderNamespace "Microsoft.Backup"</span><span class="sxs-lookup"><span data-stu-id="19d8a-136">This can be done by running hello following command: Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.Backup"</span></span>
>
>

<span data-ttu-id="19d8a-137">Létrehozhat egy új mentési tárolót használó hello **New-AzureRmBackupVault** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="19d8a-137">You can create a new backup vault using hello **New-AzureRmBackupVault** cmdlet.</span></span> <span data-ttu-id="19d8a-138">hello mentési tároló egy ARM-erőforrás, ezért meg kell tooplace az erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="19d8a-138">hello backup vault is an ARM resource, so you need tooplace it within a Resource Group.</span></span> <span data-ttu-id="19d8a-139">Egy rendszergazda jogú Azure PowerShell-konzolban futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="19d8a-139">In an elevated Azure PowerShell console, run hello following commands:</span></span>

```
PS C:\> New-AzureRmResourceGroup –Name “test-rg” –Location “West US”
PS C:\> $backupvault = New-AzureRmBackupVault –ResourceGroupName “test-rg” –Name “test-vault” –Region “West US” –Storage GeoRedundant
```

<span data-ttu-id="19d8a-140">Minden hello mentési tárolók listája egy adott feliratkozás hello olvashatók be **Get-AzureRmBackupVault** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="19d8a-140">You can get a list of all hello backup vaults in a given subscription using hello **Get-AzureRmBackupVault** cmdlet.</span></span>

> [!NOTE]
> <span data-ttu-id="19d8a-141">Tetszés szerinti toostore hello mentési tároló objektum egy változóba.</span><span class="sxs-lookup"><span data-stu-id="19d8a-141">It is convenient toostore hello backup vault object into a variable.</span></span> <span data-ttu-id="19d8a-142">sok Azure Backup-parancsmagokhoz tartozó bemeneti adatokként hello tároló objektum szükséges.</span><span class="sxs-lookup"><span data-stu-id="19d8a-142">hello vault object is needed as an input for many Azure Backup cmdlets.</span></span>
>
>

### <a name="registering-hello-vms"></a><span data-ttu-id="19d8a-143">Hello virtuális gépek regisztrálása</span><span class="sxs-lookup"><span data-stu-id="19d8a-143">Registering hello VMs</span></span>
<span data-ttu-id="19d8a-144">hello első lépést konfigurálása az Azure Backup szolgáltatás biztonsági mentési van tooregister a számítógép vagy virtuális gép egy Azure Backup-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="19d8a-144">hello first step towards configuring backup with Azure Backup is tooregister your machine or VM with an Azure Backup vault.</span></span> <span data-ttu-id="19d8a-145">Hello **Register-AzureRmBackupContainer** parancsmag hello Azure IaaS virtuális gépként bemeneti adatokat fogad, és regisztrálja azt hello megadott tárolóban.</span><span class="sxs-lookup"><span data-stu-id="19d8a-145">hello **Register-AzureRmBackupContainer** cmdlet takes hello input information of an Azure IaaS virtual machine and registers it with hello specified vault.</span></span> <span data-ttu-id="19d8a-146">hello regisztrációs műveletet hello Azure virtuális gép társítja hello mentési tárolót, és nyomon követi a virtuális gép hello hello biztonsági mentési teljes életciklusukon keresztül.</span><span class="sxs-lookup"><span data-stu-id="19d8a-146">hello register operation associates hello Azure virtual machine with hello backup vault and tracks hello VM through hello backup lifecycle.</span></span>

<span data-ttu-id="19d8a-147">A virtuális gép regisztrálása hello Azure Backup szolgáltatás hoz létre a legfelső szintű tároló objektumok.</span><span class="sxs-lookup"><span data-stu-id="19d8a-147">Registering your VM with hello Azure Backup service creates a top-level container object.</span></span> <span data-ttu-id="19d8a-148">A tároló általában több olyan elemeket tartalmaz, biztonsági másolat készíthető, de hello esetben a virtuális gépek nem lesznek hello tároló csak egy biztonsági mentési elemet.</span><span class="sxs-lookup"><span data-stu-id="19d8a-148">A container typically contains multiple items that can be backed up, but in hello case of VMs there will be only one backup item for hello container.</span></span>

```
PS C:\> $registerjob = Register-AzureRmBackupContainer -Vault $backupvault -Name "testvm" -ServiceName "testvm"
```

## <a name="backup-azure-vms"></a><span data-ttu-id="19d8a-149">Azure-beli virtuális gépek biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="19d8a-149">Backup Azure VMs</span></span>
### <a name="create-a-protection-policy"></a><span data-ttu-id="19d8a-150">Védelmi házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="19d8a-150">Create a protection policy</span></span>
<span data-ttu-id="19d8a-151">Kötelező toocreate egy új védelmi házirend toostart biztonsági másolatot a virtuális gépek nincs.</span><span class="sxs-lookup"><span data-stu-id="19d8a-151">It is not mandatory toocreate a new protection policy toostart backup of your VMs.</span></span> <span data-ttu-id="19d8a-152">hello tároló tartalmaz egy "alapértelmezett házirendet a" védelem engedélyezése a használt tooquickly lehet, és majd hello jobb oldali később szerkeszthetők.</span><span class="sxs-lookup"><span data-stu-id="19d8a-152">hello vault comes with a 'Default Policy' that can be used tooquickly enable protection, and then edited later with hello right details.</span></span> <span data-ttu-id="19d8a-153">Hello tárolóban elérhető hello házirendek listáját kaphat hello segítségével **Get-AzureRmBackupProtectionPolicy** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="19d8a-153">You can get a list of hello policies available in hello vault by using hello **Get-AzureRmBackupProtectionPolicy** cmdlet:</span></span>

```
PS C:\> Get-AzureRmBackupProtectionPolicy -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DefaultPolicy             AzureVM            Daily              26-Aug-15 12:30:00 AM
```

> [!NOTE]
> <span data-ttu-id="19d8a-154">hello BackupTime mezőt a PowerShellben hello időzónáját UTC.</span><span class="sxs-lookup"><span data-stu-id="19d8a-154">hello timezone of hello BackupTime field in PowerShell is UTC.</span></span> <span data-ttu-id="19d8a-155">Azonban hello biztonságimásolat-készítési időpont látható hello Azure-portálon, hello időzóna esetén igazított tooyour helyi rendszer hello UTC eltolás mellett.</span><span class="sxs-lookup"><span data-stu-id="19d8a-155">However, when hello backup time is shown in hello Azure portal, hello timezone is aligned tooyour local system along with hello UTC offset.</span></span>
>
>

<span data-ttu-id="19d8a-156">A biztonsági mentési házirend legalább egy megőrzési házirend társítva.</span><span class="sxs-lookup"><span data-stu-id="19d8a-156">A backup policy is associated with at least one retention policy.</span></span> <span data-ttu-id="19d8a-157">hello megőrzési házirend határozza meg, hány helyreállítási pont tartják az Azure Backup szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="19d8a-157">hello retention policy defines how long a recovery point is kept with Azure Backup.</span></span> <span data-ttu-id="19d8a-158">Hello **New-AzureRmBackupRetentionPolicy** parancsmag megőrzési házirend adatokat PowerShell-objektumokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="19d8a-158">hello **New-AzureRmBackupRetentionPolicy** cmdlet creates PowerShell objects that hold retention policy information.</span></span> <span data-ttu-id="19d8a-159">A megőrzési csoportházirend-objektumokat kell használni, mint a bemeneti toohello *New-AzureRmBackupProtectionPolicy* parancsmagot, vagy közvetlenül az hello *Enable-AzureRmBackupProtection* parancsmag.</span><span class="sxs-lookup"><span data-stu-id="19d8a-159">These retention policy objects are used as inputs toohello *New-AzureRmBackupProtectionPolicy* cmdlet, or directly with hello *Enable-AzureRmBackupProtection* cmdlet.</span></span>

<span data-ttu-id="19d8a-160">A biztonsági mentési házirend határozza meg, mikor és milyen gyakran hello elem biztonsági mentését történik.</span><span class="sxs-lookup"><span data-stu-id="19d8a-160">A backup policy defines when and how often hello backup of an item is done.</span></span> <span data-ttu-id="19d8a-161">Hello **New-AzureRmBackupProtectionPolicy** parancsmag létrehoz egy PowerShell-objektum, amely tartalmazza a biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="19d8a-161">hello **New-AzureRmBackupProtectionPolicy** cmdlet creates a PowerShell object that holds backup policy information.</span></span> <span data-ttu-id="19d8a-162">hello biztonsági mentési házirend szolgál egy bemeneti toohello *Enable-AzureRmBackupProtection* parancsmag.</span><span class="sxs-lookup"><span data-stu-id="19d8a-162">hello backup policy is used as an input toohello *Enable-AzureRmBackupProtection* cmdlet.</span></span>

```
PS C:\> $Daily = New-AzureRmBackupRetentionPolicyObject -DailyRetention -Retention 30
PS C:\> $newpolicy = New-AzureRmBackupProtectionPolicy -Name DailyBackup01 -Type AzureVM -Daily -BackupTime ([datetime]"3:30 PM") -RetentionPolicy $Daily -Vault $backupvault

Name                      Type               ScheduleType       BackupTime
----                      ----               ------------       ----------
DailyBackup01             AzureVM            Daily              01-Sep-15 3:30:00 PM
```

### <a name="enable-protection"></a><span data-ttu-id="19d8a-163">Védelem engedélyezése</span><span class="sxs-lookup"><span data-stu-id="19d8a-163">Enable protection</span></span>
<span data-ttu-id="19d8a-164">Védelem engedélyezése magában foglalja a két objektum – hello elem és hello házirend, és mindkét kell toobelong toohello azonos tároló.</span><span class="sxs-lookup"><span data-stu-id="19d8a-164">Enabling protection involves two objects - hello Item and hello Policy, and both need toobelong toohello same vault.</span></span> <span data-ttu-id="19d8a-165">Miután hello házirend társítva hello elem, hello biztonsági mentési munkafolyamat fog indítsa hello meghatározott ütemezés szerint.</span><span class="sxs-lookup"><span data-stu-id="19d8a-165">Once hello policy has been associated with hello item, hello backup workflow will kick in at hello defined schedule.</span></span>

```
PS C:\> Get-AzureRmBackupContainer -Type AzureVM -Status Registered -Vault $backupvault | Get-AzureRmBackupItem | Enable-AzureRmBackupProtection -Policy $newpolicy
```

### <a name="initial-backup"></a><span data-ttu-id="19d8a-166">Kezdeti biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="19d8a-166">Initial backup</span></span>
<span data-ttu-id="19d8a-167">hello biztonsági mentés ütemezése hello azt követő biztonsági mentéseket a hello teljes kezdeti hello elem és hello növekményes módon kezeli.</span><span class="sxs-lookup"><span data-stu-id="19d8a-167">hello backup schedule will take care of doing hello full initial copy for hello item and hello incremental copy for hello subsequent backups.</span></span> <span data-ttu-id="19d8a-168">Azonban ha azt szeretné, hogy tooforce hello kezdeti biztonsági mentési toohappen bizonyos egyszerre, vagy akár azonnal használja hello **Backup-AzureRmBackupItem** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="19d8a-168">However, if you want tooforce hello initial backup toohappen at a certain time or even immediately then use hello **Backup-AzureRmBackupItem** cmdlet:</span></span>

```
PS C:\> $container = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -Name "testvm"
PS C:\> $backupjob = Get-AzureRmBackupItem -Container $container | Backup-AzureRmBackupItem
PS C:\> $backupjob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

> [!NOTE]
> <span data-ttu-id="19d8a-169">hello StartTime időzónáját hello és PowerShell EndTime mezőlistájában UTC.</span><span class="sxs-lookup"><span data-stu-id="19d8a-169">hello timezone of hello StartTime and EndTime fields shown in PowerShell is UTC.</span></span> <span data-ttu-id="19d8a-170">Azonban hello hasonló információkat hello Azure-portálon látható, hello időzóna esetén igazított tooyour rendszeróra.</span><span class="sxs-lookup"><span data-stu-id="19d8a-170">However, when hello similar information is shown in hello Azure portal, hello timezone is aligned tooyour local system clock.</span></span>
>
>

### <a name="monitoring-a-backup-job"></a><span data-ttu-id="19d8a-171">A biztonsági mentési feladatot figyelése</span><span class="sxs-lookup"><span data-stu-id="19d8a-171">Monitoring a backup job</span></span>
<span data-ttu-id="19d8a-172">A legtöbb hosszú ideig futó műveletek Azure Backup feladatként van modellezni.</span><span class="sxs-lookup"><span data-stu-id="19d8a-172">Most long-running operations in Azure Backup are modelled as a job.</span></span> <span data-ttu-id="19d8a-173">Így könnyen tootrack folyamatban mindig tookeep hello Azure portál megnyitása nélkül.</span><span class="sxs-lookup"><span data-stu-id="19d8a-173">This makes it easy tootrack progress without having tookeep hello Azure portal open at all times.</span></span>

<span data-ttu-id="19d8a-174">tooget hello legújabb egy folyamatban lévő feladat állapota, használjon hello **Get-AzureRmBackupJob** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="19d8a-174">tooget hello latest status of an in-progress job, use hello **Get-AzureRmBackupJob** cmdlet.</span></span>

```
PS C:\> $joblist = Get-AzureRmBackupJob -Vault $backupvault -Status InProgress
PS C:\> $joblist[0]

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Backup          InProgress      01-Sep-15 12:24:01 PM  01-Jan-01 12:00:00 AM
```

<span data-ttu-id="19d8a-175">Ezek a feladatok befejezésére – ami szükségtelen, további kód - lekérdezési helyett egyszerűbb toouse hello **várakozási-AzureRmBackupJob** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="19d8a-175">Instead of polling these jobs for completion - which is unnecessary, additional code - it is simpler toouse hello **Wait-AzureRmBackupJob** cmdlet.</span></span> <span data-ttu-id="19d8a-176">Egy parancsfájlban használatakor hello parancsmag hello végrehajtási idejére felfüggeszti hello feladat befejeződik, vagy hello megadott időtúllépési érték elérésekor.</span><span class="sxs-lookup"><span data-stu-id="19d8a-176">When used in a script, hello cmdlet will pause hello execution until either hello job completes or hello specified timeout value is reached.</span></span>

```
PS C:\> Wait-AzureRmBackupJob -Job $joblist[0] -Timeout 43200
```


## <a name="restore-an-azure-vm"></a><span data-ttu-id="19d8a-177">Állítsa vissza az Azure virtuális gép</span><span class="sxs-lookup"><span data-stu-id="19d8a-177">Restore an Azure VM</span></span>
<span data-ttu-id="19d8a-178">A sorrend toorestore biztonsági mentési adatokat tooidentify hello biztonsági másolat elem van szüksége, és hello helyreállítási pont, amely hello időpontban adatokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="19d8a-178">In order toorestore backup data, you need tooidentify hello backed-up Item and hello Recovery Point that holds hello point-in-time data.</span></span> <span data-ttu-id="19d8a-179">Ez az információ a megadott toohello visszaállítási-AzureRmBackupItem parancsmag tooinitiate adatainak hello tároló toohello felhasználói fiókhoz való visszaállítás.</span><span class="sxs-lookup"><span data-stu-id="19d8a-179">This information is supplied toohello Restore-AzureRmBackupItem cmdlet tooinitiate a restore of data from hello vault toohello customer's account.</span></span>

### <a name="select-hello-vm"></a><span data-ttu-id="19d8a-180">Válassza ki a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="19d8a-180">Select hello VM</span></span>
<span data-ttu-id="19d8a-181">tooget hello PowerShell-objektum, amely azonosítja a helyes biztonságimásolat-cikk hello, a tároló hello hello tárolóban toostart kell, és működnek az objektum hierarchia valamennyi alsóbb szintjén.</span><span class="sxs-lookup"><span data-stu-id="19d8a-181">tooget hello PowerShell object that identifies hello right backup Item, you need toostart from hello Container in hello vault, and work your way down object hierarchy.</span></span> <span data-ttu-id="19d8a-182">virtuális gép, használjon hello hello jelölő tooselect hello tároló **Get-AzureRmBackupContainer** parancsmag és az adott toohello pipe **Get-AzureRmBackupItem** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="19d8a-182">tooselect hello container that represents hello VM, use hello **Get-AzureRmBackupContainer** cmdlet and pipe that toohello **Get-AzureRmBackupItem** cmdlet.</span></span>

```
PS C:\> $backupitem = Get-AzureRmBackupContainer -Vault $backupvault -Type AzureVM -name "testvm" | Get-AzureRmBackupItem
```

### <a name="choose-a-recovery-point"></a><span data-ttu-id="19d8a-183">A helyreállítási pont kiválasztása</span><span class="sxs-lookup"><span data-stu-id="19d8a-183">Choose a recovery point</span></span>
<span data-ttu-id="19d8a-184">Most listázhatja hello hello biztonsági mentési elem hello használata az összes helyreállítási pontról **Get-AzureRmBackupRecoveryPoint** parancsmagot, és válassza ki a hello helyreállítási pont toorestore.</span><span class="sxs-lookup"><span data-stu-id="19d8a-184">You can now list all hello recovery points for hello backup item using hello **Get-AzureRmBackupRecoveryPoint** cmdlet, and choose hello recovery point toorestore.</span></span> <span data-ttu-id="19d8a-185">Általában válassza ki a felhasználók a legutóbbi hello *AppConsistent* hello listában mutasson.</span><span class="sxs-lookup"><span data-stu-id="19d8a-185">Typically users pick hello most recent *AppConsistent* point in hello list.</span></span>

```
PS C:\> $rp =  Get-AzureRmBackupRecoveryPoint -Item $backupitem
PS C:\> $rp

RecoveryPointId    RecoveryPointType  RecoveryPointTime      ContainerName
---------------    -----------------  -----------------      -------------
15273496567119     AppConsistent      01-Sep-15 12:27:38 PM  iaasvmcontainer;testvm;testv...
```

<span data-ttu-id="19d8a-186">hello változó ```$rp``` a helyreállítási pontok egy tömb van hello kijelölt elem biztonsági mentése, fordított sorrendben idő – hello legújabb helyreállítási pont 0. indexnél.</span><span class="sxs-lookup"><span data-stu-id="19d8a-186">hello variable ```$rp``` is an array of recovery points for hello selected backup item, sorted in reverse order of time - hello latest recovery point is at index 0.</span></span> <span data-ttu-id="19d8a-187">Használja a következő szabványos PowerShell tömböt indexelő toopick hello helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="19d8a-187">Use standard PowerShell array indexing toopick hello recovery point.</span></span> <span data-ttu-id="19d8a-188">Például: ```$rp[0]``` hello legutóbbi helyreállítási pontot választja.</span><span class="sxs-lookup"><span data-stu-id="19d8a-188">For example: ```$rp[0]``` will select hello latest recovery point.</span></span>

### <a name="restoring-disks"></a><span data-ttu-id="19d8a-189">Lemezek visszaállítása</span><span class="sxs-lookup"><span data-stu-id="19d8a-189">Restoring disks</span></span>
<span data-ttu-id="19d8a-190">Hello Azure-portálon keresztül, és Azure PowerShell használatával végzett hello visszaállítási műveletek közötti fő különbség van.</span><span class="sxs-lookup"><span data-stu-id="19d8a-190">There is a key difference between hello restore operations done through hello Azure portal and through Azure PowerShell.</span></span> <span data-ttu-id="19d8a-191">A PowerShell-lel hello visszaállítási művelet megáll hello lemezét és konfigurációs adatát visszaállítása hello helyreállítási pontból.</span><span class="sxs-lookup"><span data-stu-id="19d8a-191">With PowerShell, hello restore operation stops at restoring hello disks and config information from hello recovery point.</span></span> <span data-ttu-id="19d8a-192">Nem hoz létre egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="19d8a-192">It does not create a virtual machine.</span></span>

> [!WARNING]
> <span data-ttu-id="19d8a-193">hello visszaállítási-AzureRmBackupItem nem hoz létre egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="19d8a-193">hello Restore-AzureRmBackupItem does not create a VM.</span></span> <span data-ttu-id="19d8a-194">Csak visszaállítja a hello lemezek toohello megadott tárfiók.</span><span class="sxs-lookup"><span data-stu-id="19d8a-194">It only restores hello disks toohello specified storage account.</span></span> <span data-ttu-id="19d8a-195">Ez a rendszer nem hello fog tapasztalni hello Azure-portálon a kívánt viselkedést eredményező beállítást.</span><span class="sxs-lookup"><span data-stu-id="19d8a-195">This is not hello same behavior you will experience in hello Azure portal.</span></span>
>
>

```
PS C:\> $restorejob = Restore-AzureRmBackupItem -StorageAccountName "DestAccount" -RecoveryPoint $rp[0]
PS C:\> $restorejob

WorkloadName    Operation       Status          StartTime              EndTime
------------    ---------       ------          ---------              -------
testvm          Restore         InProgress      01-Sep-15 1:14:01 PM   01-Jan-01 12:00:00 AM
```

<span data-ttu-id="19d8a-196">Hello részletekért hello visszaállítási művelet használatával hello **Get-AzureRmBackupJobDetails** parancsmag hello visszaállítási feladat befejezése után.</span><span class="sxs-lookup"><span data-stu-id="19d8a-196">You can get hello details of hello restore operation using hello **Get-AzureRmBackupJobDetails** cmdlet once hello Restore job has completed.</span></span> <span data-ttu-id="19d8a-197">Hello *hiba részletei* tulajdonságnak lesz hello információ szükséges toorebuild hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="19d8a-197">hello *ErrorDetails* property will have hello information needed toorebuild hello VM.</span></span>

```
PS C:\> $restorejob = Get-AzureRmBackupJob -Job $restorejob
PS C:\> $details = Get-AzureRmBackupJobDetails -Job $restorejob
```

### <a name="build-hello-vm"></a><span data-ttu-id="19d8a-198">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="19d8a-198">Build hello VM</span></span>
<span data-ttu-id="19d8a-199">Épület hello VM kívül vissza hello lemezek végezhető hello régebbi Azure Service Management PowerShell-parancsmagok használatával, új Azure Resource Manager-sablonok hello, vagy hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="19d8a-199">Building hello VM out of hello restored disks can be done using hello older Azure Service Management PowerShell cmdlets, hello new Azure Resource Manager templates, or even using hello Azure portal.</span></span> <span data-ttu-id="19d8a-200">Az első példában bemutatjuk a hogyan tooget ott hello Azure Szolgáltatáskezelés-parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="19d8a-200">In a quick example, we will show how tooget there using hello Azure Service Management cmdlets.</span></span>

```
$properties  = $details.Properties

$storageAccountName = $properties["Target Storage Account Name"]
$containerName = $properties["Config Blob Container Name"]
$blobName = $properties["Config Blob Name"]

$keys = Get-AzureStorageKey -StorageAccountName $storageAccountName
$storageAccountKey = $keys.Primary
$storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey


$destination_path = "C:\Users\admin\Desktop\vmconfig.xml"
Get-AzureStorageBlobContent -Container $containerName -Blob $blobName -Destination $destination_path -Context $storageContext


$obj = [xml](((Get-Content -Path $destination_path -Encoding UniCode)).TrimEnd([char]0x00))
$pvr = $obj.PersistentVMRole
$os = $pvr.OSVirtualHardDisk
$dds = $pvr.DataVirtualHardDisks
$osDisk = Add-AzureDisk -MediaLocation $os.MediaLink -OS $os.OS -DiskName "panbhaosdisk"
$vm = New-AzureVMConfig -Name $pvr.RoleName -InstanceSize $pvr.RoleSize -DiskName $osDisk.DiskName

if (!($dds -eq $null))
{
    foreach($d in $dds.DataVirtualHardDisk)
    {
        $lun = 0
        if(!($d.Lun -eq $null))
        {
            $lun = $d.Lun
        }
        $name = "panbhadataDisk" + $lun
        Add-AzureDisk -DiskName $name -MediaLocation $d.MediaLink
        $vm | Add-AzureDataDisk -Import -DiskName $name -LUN $lun
    }
}

New-AzureVM -ServiceName "panbhasample" -Location "SouthEast Asia" -VM $vm
```

<span data-ttu-id="19d8a-201">További információt a toobuild egy virtuális merevlemezről vissza hello, olvassa el a következő parancsmagok hello:</span><span class="sxs-lookup"><span data-stu-id="19d8a-201">For more information on how toobuild a VM from hello restored disks, read about hello following cmdlets:</span></span>

* [<span data-ttu-id="19d8a-202">Adja hozzá AzureDisk</span><span class="sxs-lookup"><span data-stu-id="19d8a-202">Add-AzureDisk</span></span>](https://msdn.microsoft.com/library/azure/dn495252.aspx)
* [<span data-ttu-id="19d8a-203">Új AzureVMConfig</span><span class="sxs-lookup"><span data-stu-id="19d8a-203">New-AzureVMConfig</span></span>](https://msdn.microsoft.com/library/azure/dn495159.aspx)
* [<span data-ttu-id="19d8a-204">Új AzureVM</span><span class="sxs-lookup"><span data-stu-id="19d8a-204">New-AzureVM</span></span>](https://msdn.microsoft.com/library/azure/dn495254.aspx)

## <a name="code-samples"></a><span data-ttu-id="19d8a-205">Kódminták</span><span class="sxs-lookup"><span data-stu-id="19d8a-205">Code samples</span></span>
### <a name="1-get-hello-completion-status-of-job-sub-tasks"></a><span data-ttu-id="19d8a-206">1. A feladat alfeladatok hello befejezési állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="19d8a-206">1. Get hello completion status of job sub-tasks</span></span>
<span data-ttu-id="19d8a-207">tootrack hello befejezési állapotát az egyes alfeladatok hello használható **Get-AzureRmBackupJobDetails** parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="19d8a-207">tootrack hello completion status of individual sub-tasks, you can use hello **Get-AzureRmBackupJobDetails** cmdlet:</span></span>

```
PS C:\> $details = Get-AzureRmBackupJobDetails -JobId $backupjob.InstanceId -Vault $backupvault
PS C:\> $details.SubTasks

Name                                                        Status
----                                                        ------
Take Snapshot                                               Completed
Transfer data tooBackup vault                               InProgress
```

### <a name="2-create-a-dailyweekly-report-of-backup-jobs"></a><span data-ttu-id="19d8a-208">2. A biztonsági mentési feladatok napi vagy heti jelentés létrehozása</span><span class="sxs-lookup"><span data-stu-id="19d8a-208">2. Create a daily/weekly report of backup jobs</span></span>
<span data-ttu-id="19d8a-209">A rendszergazdák általában érdemes tooknow hello utolsó 24 órában, milyen biztonsági mentési feladat futtatása adott biztonsági mentési feladatok állapotának hello.</span><span class="sxs-lookup"><span data-stu-id="19d8a-209">Administrators typically want tooknow what backup jobs ran in hello last 24 hours, hello status of those backup jobs.</span></span> <span data-ttu-id="19d8a-210">Emellett az átvitt adatok mennyisége hello lehetővé teszi a rendszergazdák olyan módon tooestimate havi használati adatok.</span><span class="sxs-lookup"><span data-stu-id="19d8a-210">Additionally, hello amount of data transferred gives administrators a way tooestimate their monthly data usage.</span></span> <span data-ttu-id="19d8a-211">az alábbi parancsfájl hello hello hello Azure Backup szolgáltatás nyers adatait kéri le, és hello megjeleníti hello PowerShell-konzolban.</span><span class="sxs-lookup"><span data-stu-id="19d8a-211">hello script below pulls hello raw data from hello Azure Backup service and displays hello information in hello PowerShell console.</span></span>

```
param(  [Parameter(Mandatory=$True,Position=1)]
        [string]$backupvaultname,

        [Parameter(Mandatory=$False,Position=2)]
        [int]$numberofdays = 7)


#Initialize variables
$DAILYBACKUPSTATS = @()
$backupvault = Get-AzureRmBackupVault -Name $backupvaultname
$enddate = ([datetime]::Today).AddDays(1)
$startdate = ([datetime]::Today)

for( $i = 1; $i -le $numberofdays; $i++ )
{
    # We query one day at a time because pulling 7 days of data might be too much
    $dailyjoblist = Get-AzureRmBackupJob -Vault $backupvault -From $startdate -too$enddate -Type AzureVM -Operation Backup
    Write-Progress -Activity "Getting job information for hello last $numberofdays days" -Status "Day -$i" -PercentComplete ([int]([decimal]$i*100/$numberofdays))

    foreach( $job in $dailyjoblist )
    {
        #Extract hello information for hello reports
        $newstatsobj = New-Object System.Object
        $newstatsobj | Add-Member -Type NoteProperty -Name Date -Value $startdate
        $newstatsobj | Add-Member -Type NoteProperty -Name VMName -Value $job.WorkloadName
        $newstatsobj | Add-Member -Type NoteProperty -Name Duration -Value $job.Duration
        $newstatsobj | Add-Member -Type NoteProperty -Name Status -Value $job.Status

        $details = Get-AzureRmBackupJobDetails -Job $job
        $newstatsobj | Add-Member -Type NoteProperty -Name BackupSize -Value $details.Properties["Backup Size"]
        $DAILYBACKUPSTATS += $newstatsobj
    }

    $enddate = $enddate.AddDays(-1)
    $startdate = $startdate.AddDays(-1)
}

$DAILYBACKUPSTATS | Out-GridView
```

<span data-ttu-id="19d8a-212">Ha azt szeretné, hogy tooadd diagramkészítési képességek toothis jelentés kimenetében, ismerje meg, a hello TechNet blogbejegyzés [diagramkészítési a PowerShell használatával](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)</span><span class="sxs-lookup"><span data-stu-id="19d8a-212">If you want tooadd charting capabilities toothis report output, learn from hello TechNet blog post [Charting with PowerShell](http://blogs.technet.com/b/richard_macdonald/archive/2009/04/28/3231887.aspx)</span></span>

## <a name="next-steps"></a><span data-ttu-id="19d8a-213">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19d8a-213">Next steps</span></span>
<span data-ttu-id="19d8a-214">Ha jobban szeret PowerShell tooengage használata az Azure-erőforrások, tekintse meg a Windows Server védelmének hello PowerShell cikk [telepítés és a Windows Server biztonsági másolat kezelése](backup-client-automation-classic.md).</span><span class="sxs-lookup"><span data-stu-id="19d8a-214">If you prefer using PowerShell tooengage with your Azure resources, check out hello PowerShell article for protecting Windows Server, [Deploy and Manage Backup for Windows Server](backup-client-automation-classic.md).</span></span> <span data-ttu-id="19d8a-215">Is van a DPM biztonsági mentések kezelése a PowerShell cikk [telepítés és a DPM a biztonsági mentés kezelése](backup-dpm-automation-classic.md).</span><span class="sxs-lookup"><span data-stu-id="19d8a-215">There is also a PowerShell article for managing DPM backups, [Deploy and Manage Backup for DPM](backup-dpm-automation-classic.md).</span></span> <span data-ttu-id="19d8a-216">Ezek a cikkek mindegyikét a Resource Manager üzembe helyezések és a klasszikus központi telepítések verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="19d8a-216">Both of these articles have a version for Resource Manager deployments as well as Classic deployments.</span></span>
