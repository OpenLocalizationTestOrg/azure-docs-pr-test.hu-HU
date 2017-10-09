---
title: Azure Backup Server v2 aaaInstall |} Microsoft Docs
description: "Az Azure Backup Server v2 lehetővé teszi a virtuális gép, fájlok és mappák, munkaterhelések és több védelméhez továbbfejlesztett biztonsági mentési lehetőségeket. Megtudhatja, hogyan tooinstall vagy frissítési tooAzure Backup Server v2."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: 5b1699dadd3a173f1c0ef91a1a600bc5e12f20ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-azure-backup-server-v2"></a><span data-ttu-id="9a120-104">Azure biztonságimásolat-kiszolgáló v2 telepítése</span><span class="sxs-lookup"><span data-stu-id="9a120-104">Install Azure Backup Server v2</span></span>

<span data-ttu-id="9a120-105">Az Azure Backup Server a virtuális gépek (VM) munkaterhelések, fájlok és mappák és több védi.</span><span class="sxs-lookup"><span data-stu-id="9a120-105">Azure Backup Server helps protect your virtual machines (VMs), workloads, files and folders, and more.</span></span> <span data-ttu-id="9a120-106">Az Azure Backup Server v2 Azure Backup Server v1 épül, és lehetővé teszi az új szolgáltatások nem érhetők el az 1-es verzió.</span><span class="sxs-lookup"><span data-stu-id="9a120-106">Azure Backup Server v2 builds on Azure Backup Server v1, and gives you new features that are not available in v1.</span></span> <span data-ttu-id="9a120-107">A szolgáltatások közötti v1 és v2, lásd: [Azure Backup Server védelmi mátrix](backup-mabs-protection-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="9a120-107">For a comparison of features between v1 and v2, see [Azure Backup Server protection matrix](backup-mabs-protection-matrix.md).</span></span> 

<span data-ttu-id="9a120-108">hello további szolgáltatásokat, a biztonsági mentés kiszolgáló v2 a biztonsági mentés kiszolgáló v1 frissítés.</span><span class="sxs-lookup"><span data-stu-id="9a120-108">hello additional features in Backup Server v2 are an upgrade from Backup Server v1.</span></span> <span data-ttu-id="9a120-109">Biztonsági mentés kiszolgáló v1 viszont nem a biztonsági mentés kiszolgáló v2 telepítésének előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="9a120-109">However, Backup Server v1 is not a prerequisite for installing Backup Server v2.</span></span> <span data-ttu-id="9a120-110">Ha azt szeretné, hogy a biztonsági mentés kiszolgáló v1 tooBackup kiszolgáló v2 tooupgrade, telepítse a biztonsági mentés kiszolgáló v2 hello Backup Server védelmi kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9a120-110">If you want tooupgrade from Backup Server v1 tooBackup Server v2, install Backup Server v2 on hello Backup Server protection server.</span></span> <span data-ttu-id="9a120-111">A meglévő biztonsági mentés beállításait változatlanok maradnak.</span><span class="sxs-lookup"><span data-stu-id="9a120-111">Your existing Backup Server settings remain intact.</span></span>

<span data-ttu-id="9a120-112">Biztonsági mentés kiszolgáló v2 telepíthető Windows Server 2012 R2 vagy Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="9a120-112">You can install Backup Server v2 on Windows Server 2012 R2 or Windows Server 2016.</span></span> <span data-ttu-id="9a120-113">új szolgáltatásainak tootake előnyeit, például a System Center 2016 adatok Protection Manager Modern Backup-tárhelyre, a biztonsági mentés kiszolgáló v2 telepítenie kell a Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="9a120-113">tootake advantage of new features like System Center 2016 Data Protection Manager Modern Backup Storage, you must install Backup Server v2 on Windows Server 2016.</span></span> <span data-ttu-id="9a120-114">Biztonsági mentés kiszolgáló v2 tooor telepítés frissítése előtt olvassa el hello [telepítésének előfeltételei](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="9a120-114">Before you upgrade tooor install Backup Server v2, read about hello [installation prerequisites](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="9a120-115">Az Azure Backup Server rendelkezik hello alap, a System Center Data Protection Manager ugyanazt a kódot.</span><span class="sxs-lookup"><span data-stu-id="9a120-115">Azure Backup Server has hello same code base as System Center Data Protection Manager.</span></span> <span data-ttu-id="9a120-116">Tartalék kiszolgáló v1 egyenértékű tooData Protection Manager 2012 R2-ben, és biztonsági mentés kiszolgáló v2 egyenértékű tooData Protection Manager 2016.</span><span class="sxs-lookup"><span data-stu-id="9a120-116">Backup Server v1 is equivalent tooData Protection Manager 2012 R2, and Backup Server v2 is equivalent tooData Protection Manager 2016.</span></span> <span data-ttu-id="9a120-117">Ez a cikk alkalmanként hello Data Protection Manager dokumentációs hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="9a120-117">This article occasionally references hello Data Protection Manager documentation.</span></span>
>
>

## <a name="upgrade-backup-server-toov2"></a><span data-ttu-id="9a120-118">Tartalék kiszolgáló toov2 frissítése</span><span class="sxs-lookup"><span data-stu-id="9a120-118">Upgrade Backup Server toov2</span></span>
<span data-ttu-id="9a120-119">a biztonsági mentés kiszolgáló v1 tooBackup kiszolgáló v2 tooupgrade ellenőrizze, hogy a telepítés rendelkezik hello szükséges frissítések:</span><span class="sxs-lookup"><span data-stu-id="9a120-119">tooupgrade from Backup Server v1 tooBackup Server v2, make sure your installation has hello required updates:</span></span>

- <span data-ttu-id="9a120-120">[Hello védelmi ügynökök frissítésének](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) hello a védett kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="9a120-120">[Update hello protection agents](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) on hello protected servers.</span></span>
- <span data-ttu-id="9a120-121">Frissítse a Windows Server 2012 R2 tooWindows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="9a120-121">Upgrade Windows Server 2012 R2 tooWindows Server 2016.</span></span>
- <span data-ttu-id="9a120-122">Azure biztonsági mentés kiszolgáló távoli felügyeleti frissítse az összes üzemi kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="9a120-122">Upgrade Azure Backup Server Remote Administrator on all production servers.</span></span>
- <span data-ttu-id="9a120-123">Győződjön meg arról, hogy a biztonsági mentések megfelelőek-e a toocontinue az üzemi kiszolgáló újraindítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="9a120-123">Ensure that backups are set toocontinue without restarting your production server.</span></span>


### <a name="upgrade-steps-for-backup-server-v2"></a><span data-ttu-id="9a120-124">Biztonsági mentés kiszolgáló 2-es verzió frissítési lépések</span><span class="sxs-lookup"><span data-stu-id="9a120-124">Upgrade steps for Backup Server v2</span></span>

1. <span data-ttu-id="9a120-125">A letöltőközpontból, hello [hello frissítési telepítő letöltési](https://go.microsoft.com/fwlink/?LinkId=626082).</span><span class="sxs-lookup"><span data-stu-id="9a120-125">In hello Download Center, [download hello upgrade installer](https://go.microsoft.com/fwlink/?LinkId=626082).</span></span>

2. <span data-ttu-id="9a120-126">Miután kibontása hello beállítása varázsló, győződjön meg arról, hogy **hajtható végre a setup.exe** kiválasztva, majd jelölje ki **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="9a120-126">After you extract hello setup wizard, make sure that **Execute setup.exe** is selected, and then select **Finish**.</span></span>

  ![A telepítő installer - futtassa a telepítő](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. <span data-ttu-id="9a120-128">A Microsoft Azure Backup Server varázslóban hello alatt **telepítése**, jelölje be **Microsoft Azure Backup Server**.</span><span class="sxs-lookup"><span data-stu-id="9a120-128">In hello Microsoft Azure Backup Server wizard, under **Install**, select **Microsoft Azure Backup Server**.</span></span>

  ![A telepítő installer - válassza telepítése](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. <span data-ttu-id="9a120-130">A hello **üdvözlő** lapon olvassa el hello figyelmeztetéseket, és válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a120-130">On hello **Welcome** page, review hello warnings, and then select **Next**.</span></span>

  ![A telepítő installer - kezdőlap](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. <span data-ttu-id="9a120-132">hello beállítása varázslót, hogy a környezet verzióra frissítés előfeltétel-ellenőrzések toomake hajt végre.</span><span class="sxs-lookup"><span data-stu-id="9a120-132">hello setup wizard performs prerequisite checks toomake sure your environment can upgrade.</span></span> <span data-ttu-id="9a120-133">A hello **előfeltételek ellenőrzésének** lapon jelölje be **ellenőrizze**.</span><span class="sxs-lookup"><span data-stu-id="9a120-133">On hello **Prerequisite Checks** page, select **Check**.</span></span>

  ![A telepítő telepítő – Előfeltételek ellenőrzése lap](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. <span data-ttu-id="9a120-135">A környezet hello előfeltétel-ellenőrzések meg kell felelnie.</span><span class="sxs-lookup"><span data-stu-id="9a120-135">Your environment must pass hello prerequisite checks.</span></span> <span data-ttu-id="9a120-136">Ha a környezet nem feleltek meg a hello során, hello szempontokat kell figyelembe vennie, és hárítsa el őket.</span><span class="sxs-lookup"><span data-stu-id="9a120-136">If your environment doesn't pass hello checks, note hello issues and fix them.</span></span> <span data-ttu-id="9a120-137">Ezt követően válassza **ellenőrizze újra**.</span><span class="sxs-lookup"><span data-stu-id="9a120-137">Then, select **Check Again**.</span></span> <span data-ttu-id="9a120-138">Amikor hello előfeltétel-ellenőrzéseket, válassza ki a **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a120-138">After you pass hello prerequisite checks, select **Next**.</span></span>

  ![A telepítő installer - ellenőrizze újra gomb](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. <span data-ttu-id="9a120-140">A hello **SQL-beállítások** lapon, az SQL-telepítés hello a kívánt beállítást, majd válassza ki és **ellenőrzés és telepítés**.</span><span class="sxs-lookup"><span data-stu-id="9a120-140">On hello **SQL Settings** page, select hello relevant option for your SQL installation, and then select **Check and Install**.</span></span>

  ![A telepítő installer - SQL-beállítások lap](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  <span data-ttu-id="9a120-142">hello ellenőrzések néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="9a120-142">hello checks might take a few minutes.</span></span> <span data-ttu-id="9a120-143">Ha hello ellenőrzése kész, jelölje be-e **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a120-143">When hello checks are finished, select **Next**.</span></span>

  ![A telepítő installer - SQL ellenőrizze és telepítése gombra](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. <span data-ttu-id="9a120-145">A hello **telepítési beállítások** lapján bármilyen módosítások toohello a helyet, ahol a biztonsági mentés Server telepítve van, vagy toohello ideiglenes helyet.</span><span class="sxs-lookup"><span data-stu-id="9a120-145">On hello **Installation Settings** page, make any changes toohello location where Backup Server is installed, or toohello Scratch Location.</span></span> <span data-ttu-id="9a120-146">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a120-146">Select **Next**.</span></span>

  ![A telepítő installer - telepítési beállítások lapon](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. <span data-ttu-id="9a120-148">toofinish hello beállítása varázsló, jelölje be **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="9a120-148">toofinish hello setup wizard, select **Finish**.</span></span>

  ![A telepítő telepítő - a befejezési](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a><span data-ttu-id="9a120-150">Tároló hozzáadása a Modern Backup-tárhelyre</span><span class="sxs-lookup"><span data-stu-id="9a120-150">Add storage for Modern Backup Storage</span></span>

<span data-ttu-id="9a120-151">tooimprove biztonsági mentési tároló-hatékonyságot biztosít, v2 helykiszolgáló biztonsági mentése a kötetek támogatást.</span><span class="sxs-lookup"><span data-stu-id="9a120-151">tooimprove backup storage efficiency, Backup Server v2 adds support for volumes.</span></span> <span data-ttu-id="9a120-152">Biztonsági mentés kiszolgáló v1, például a biztonsági mentés kiszolgáló v2 lemezeit támogatja.</span><span class="sxs-lookup"><span data-stu-id="9a120-152">Like Backup Server v1, Backup Server v2 supports disks.</span></span>

### <a name="add-volumes-and-disks"></a><span data-ttu-id="9a120-153">Kötetek és a lemezek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9a120-153">Add volumes and disks</span></span>
<span data-ttu-id="9a120-154">Ha Windows Server 2016 tartalék kiszolgáló v2 futtatja, használhatja kötetek toostore biztonsági mentési adatokat.</span><span class="sxs-lookup"><span data-stu-id="9a120-154">If you run Backup Server v2 on Windows Server 2016, you can use volumes toostore backup data.</span></span> <span data-ttu-id="9a120-155">Kötet nyújtja a tárhely-megtakarítást és gyorsabb biztonsági mentéseket.</span><span class="sxs-lookup"><span data-stu-id="9a120-155">Volumes offer storage savings and faster backups.</span></span> <span data-ttu-id="9a120-156">Mivel kötetek új tooBackup kiszolgálóhoz, hozzá kell adnia őket.</span><span class="sxs-lookup"><span data-stu-id="9a120-156">Because volumes are new tooBackup Server, you must add them.</span></span> 

<span data-ttu-id="9a120-157">Egy kötet tooBackup kiszolgáló hozzáadásakor hello kötet egy rövid nevet adhat.</span><span class="sxs-lookup"><span data-stu-id="9a120-157">When you add a volume tooBackup Server, you can give hello volume a friendly name.</span></span> <span data-ttu-id="9a120-158">Kattintson a hello **rövid név** oszlop hello kötet kívánt tooname.</span><span class="sxs-lookup"><span data-stu-id="9a120-158">Click hello **Friendly Name** column of hello volume you want tooname.</span></span> <span data-ttu-id="9a120-159">Később bármikor módosíthatja hello nevét, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="9a120-159">You can change hello name later, if necessary.</span></span> <span data-ttu-id="9a120-160">Is használhatja a PowerShell tooadd vagy módosíthatja a kötetek rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="9a120-160">You also can use PowerShell tooadd or change friendly names for volumes.</span></span>

<span data-ttu-id="9a120-161">a felügyeleti konzol hello kötet tooadd:</span><span class="sxs-lookup"><span data-stu-id="9a120-161">tooadd a volume in hello Administrator Console:</span></span>

1. <span data-ttu-id="9a120-162">Hello Azure Backup Server felügyeleti konzolt, jelölje ki **felügyeleti** > **lemezegységet** > **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9a120-162">In hello Azure Backup Server Administrator Console, select **Management** > **Disk Storage** > **Add**.</span></span>

    ![Nyissa meg hello lemezegységet hozzáadása varázsló](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    <span data-ttu-id="9a120-164">Ekkor megnyílik a hello lemezegységet hozzáadása varázsló.</span><span class="sxs-lookup"><span data-stu-id="9a120-164">This opens hello Add Disk Storage wizard.</span></span>

2. <span data-ttu-id="9a120-165">A hello **adja hozzá a lemezes tárolás** lap hello **rendelkezésre álló köteteken** mezőben, jelöljön ki egy kötetet, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9a120-165">On hello **Add Disk Storage** page, in hello **Available volumes** box, select a volume, and then select **Add**.</span></span>
3. <span data-ttu-id="9a120-166">A hello **kijelölt kötetek** mezőbe, adjon meg egy rövid nevet hello kötet, és válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a120-166">In hello **Selected volumes** box, enter a friendly name for hello volume, and then select **OK**.</span></span>

      ![Adja hozzá a lemezes tárolás varázsló – a kötet hozzáadása](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  <span data-ttu-id="9a120-168">Ha azt szeretné, hogy a lemez tooadd, hello lemez örökölt tárolási rendelkező tooa védelmi csoporthoz kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="9a120-168">If you want tooadd a disk, hello disk must belong tooa protection group that has legacy storage.</span></span> <span data-ttu-id="9a120-169">Ezek a lemezek csak a védelmi csoportok használható.</span><span class="sxs-lookup"><span data-stu-id="9a120-169">These disks can only be used for these protection groups.</span></span> <span data-ttu-id="9a120-170">Ha a biztonsági kiszolgáló nem kapcsolódik az adatforrásokat, amelyek az örökölt védelmet, hello lemez nem szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="9a120-170">If Backup Server doesn't have sources that have legacy protection, hello disk isn't listed.</span></span>

  <span data-ttu-id="9a120-171">Lemezek hozzáadásával kapcsolatos további információkért lásd: [hozzáadása a lemezek tooincrease örökölt tárolási](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage).</span><span class="sxs-lookup"><span data-stu-id="9a120-171">For more information about adding disks, see [Adding disks tooincrease legacy storage](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage).</span></span> <span data-ttu-id="9a120-172">A lemez nem adjon egy rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="9a120-172">You can't give a disk a friendly name.</span></span>


### <a name="assign-workloads-toovolumes"></a><span data-ttu-id="9a120-173">Rendelje hozzá a munkaterhelések toovolumes</span><span class="sxs-lookup"><span data-stu-id="9a120-173">Assign workloads toovolumes</span></span>

<span data-ttu-id="9a120-174">A kiszolgáló biztonsági mentése megadhatja, mely munkaterhelések toowhich kötetek vannak hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="9a120-174">In Backup Server, you specify which workloads are assigned toowhich volumes.</span></span> <span data-ttu-id="9a120-175">Például beállíthat költséges kötetek, amely támogatja a bemeneti/kimeneti műveletek második (IOPS) toostore csak igénylő munkaterhelések gyakori, nagy mennyiségű biztonsági másolatok száma túl magas száma.</span><span class="sxs-lookup"><span data-stu-id="9a120-175">For example, you can set expensive volumes that support a high number of input/output operations per second (IOPS) toostore only workloads that require frequent, high-volume backups.</span></span> <span data-ttu-id="9a120-176">Példa: SQL Server, a tranzakciós naplók.</span><span class="sxs-lookup"><span data-stu-id="9a120-176">An example is SQL Server with transaction logs.</span></span>

#### <a name="update-dpmdiskstorage"></a><span data-ttu-id="9a120-177">Frissítés-DPMDiskStorage</span><span class="sxs-lookup"><span data-stu-id="9a120-177">Update-DPMDiskStorage</span></span>

<span data-ttu-id="9a120-178">hello tárolókészletben a kiszolgáló biztonsági mentési kötet tooupdate hello tulajdonságainak hello PowerShell parancsmag frissítés-DPMDiskStorage használható.</span><span class="sxs-lookup"><span data-stu-id="9a120-178">tooupdate hello properties of a volume in hello storage pool in Backup Server, use hello PowerShell cmdlet Update-DPMDiskStorage.</span></span>

<span data-ttu-id="9a120-179">Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="9a120-179">Syntax:</span></span>

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

<span data-ttu-id="9a120-180">Az összes végrehajtott módosításokat a PowerShell használatával hello felhasználói felület is megjelennek.</span><span class="sxs-lookup"><span data-stu-id="9a120-180">All changes that you make by using PowerShell are reflected in hello UI.</span></span>


## <a name="protect-data-sources"></a><span data-ttu-id="9a120-181">Adatforrások védelméhez</span><span class="sxs-lookup"><span data-stu-id="9a120-181">Protect data sources</span></span>
<span data-ttu-id="9a120-182">toobegin védelmet nyújtó adatforrások, hozzon létre egy védelmi csoportot.</span><span class="sxs-lookup"><span data-stu-id="9a120-182">toobegin protecting data sources, create a protection group.</span></span> <span data-ttu-id="9a120-183">a következő lépéseket kiemelési módosításait és kiegészítéseit toohello új védelmi csoport varázsló hello.</span><span class="sxs-lookup"><span data-stu-id="9a120-183">hello following steps highlight changes or additions toohello New Protection Group wizard.</span></span>

<span data-ttu-id="9a120-184">a védelmi csoport toocreate:</span><span class="sxs-lookup"><span data-stu-id="9a120-184">toocreate a protection group:</span></span>

1. <span data-ttu-id="9a120-185">Válassza ki a biztonsági mentés Server felügyeleti konzol hello, **védelmi**.</span><span class="sxs-lookup"><span data-stu-id="9a120-185">In hello Backup Server Administrator Console, select **Protection**.</span></span>

2. <span data-ttu-id="9a120-186">Hello eszközsávon kiválasztása **új**.</span><span class="sxs-lookup"><span data-stu-id="9a120-186">On hello tool ribbon, select **New**.</span></span>

    <span data-ttu-id="9a120-187">Ekkor megnyílik a hello új védelmi csoport létrehozása varázsló.</span><span class="sxs-lookup"><span data-stu-id="9a120-187">This opens hello Create New Protection Group wizard.</span></span>

  ![Új védelmi csoport létrehozása varázsló](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. <span data-ttu-id="9a120-189">A hello **üdvözlő** lapon jelölje be **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a120-189">On hello **Welcome** page, select **Next**.</span></span>
4. <span data-ttu-id="9a120-190">A hello **védelmi csoport típusának kiválasztása** lapon, válassza ki a kívánt toocreate, és válassza ki a védelmi csoport hello típusú **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a120-190">On hello **Select Protection Group Type** page, select hello type of protection group you want toocreate, and then select **Next**.</span></span>

  ![Válassza ki a védelmi csoport típusa lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. <span data-ttu-id="9a120-192">A hello **csoporttagok kiválasztása** lap hello **választható tagok** ablaktáblában hello tagokat a védelmi ügynökök találhatók.</span><span class="sxs-lookup"><span data-stu-id="9a120-192">On hello **Select Group Members** page, in hello **Available members** pane, hello members with protection agents are listed.</span></span> <span data-ttu-id="9a120-193">Ehhez a példához válassza ki a kötetet a D:\ és E:\ hozzá toohello **kijelölt tagok** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="9a120-193">For this example, select volume D:\ and E:\ and add them toohello **Selected members** pane.</span></span> <span data-ttu-id="9a120-194">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a120-194">Select **Next**.</span></span>

  ![Csoport kijelölése tagok lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. <span data-ttu-id="9a120-196">A hello **adatvédelmi módszer kiválasztása** lapján adjon meg egy **védelmi csoport neve**, hello védelmi módszert, majd válassza ki és **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a120-196">On hello **Select Data Protection Method** page, enter a **Protection group name**, select hello protection method, and then select **Next**.</span></span> <span data-ttu-id="9a120-197">Ha azt szeretné, hogy a rövid távú védelem, ki kell választania hello **lemez** metódus biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="9a120-197">If you want short-term protection, you must select hello **Disk** backup method.</span></span>

  ![Adatvédelmi módszer kiválasztása lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. <span data-ttu-id="9a120-199">A hello **rövid távú célok megadása** lapon, jelölje be hello részletei **megőrzési időtartam** és **szinkronizálási gyakoriság**.</span><span class="sxs-lookup"><span data-stu-id="9a120-199">On hello **Specify Short-Term Goals** page, select hello details for **Retention range** and **Synchronization frequency**.</span></span> <span data-ttu-id="9a120-200">Ezt követően válassza **következő**.</span><span class="sxs-lookup"><span data-stu-id="9a120-200">Then, select **Next**.</span></span> <span data-ttu-id="9a120-201">Másik lehetőségként toochange hello ütemezés amikor helyreállítási pontok tett, jelölje be a **módosítás**.</span><span class="sxs-lookup"><span data-stu-id="9a120-201">Optionally, toochange hello schedule for when recovery points are taken, select **Modify**.</span></span>

  ![Adja meg a rövid távú célok lapja](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. <span data-ttu-id="9a120-203">A hello **lemezterület-foglalás tekintse át** lapon ellenőrizze a kiválasztott hello adatforrások adatait, a méretének és hello terület toobe kiosztott értékeket, és hello cél tárolókötetet.</span><span class="sxs-lookup"><span data-stu-id="9a120-203">On hello **Review Disk Storage Allocation** page, review details about hello data sources you selected, their size, and  values for hello space toobe provisioned and hello target storage volume.</span></span>

  ![Áttekintés lemezterület-foglalás lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  <span data-ttu-id="9a120-205">Tároló kötetek hello munkaterhelés kötet foglalási (beállítása a PowerShell használatával) alapulnak, és a rendelkezésre álló tár hello.</span><span class="sxs-lookup"><span data-stu-id="9a120-205">Storage volumes are based on hello workload volume allocation (set by using PowerShell) and hello available storage.</span></span> <span data-ttu-id="9a120-206">Hello tárolókötetek hello legördülő menü más kötetek választásával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="9a120-206">You can change hello storage volumes by selecting other volumes in hello drop-down menu.</span></span> <span data-ttu-id="9a120-207">Ha megváltoztatja hello **célként megadott**, értékét hello **szabad tárhely** tooreflect értékek alapján dinamikusan változik **szabad terület** és **Terület underprovisioned**.</span><span class="sxs-lookup"><span data-stu-id="9a120-207">If you change hello value for **Target Storage**, hello value for **Available disk storage** dynamically changes tooreflect values under **Free Space** and **Underprovisioned Space**.</span></span>

  <span data-ttu-id="9a120-208">Hello adatforrások nő, ahogy a tervezett, ha hello hello értéke **Underprovisioned terület** oszlopa **szabad tárhely** további tárhely szükséges mennyiségű hello tükrözi.</span><span class="sxs-lookup"><span data-stu-id="9a120-208">If hello data sources grow as planned, hello value for hello **Underprovisioned Space** column in **Available disk storage** reflects hello amount of additional storage that's needed.</span></span> <span data-ttu-id="9a120-209">Ez a tárolási igényei zökkenőmentes biztonsági mentés érték toohelp-csomag használata.</span><span class="sxs-lookup"><span data-stu-id="9a120-209">Use this value toohelp plan your storage needs for smooth backups.</span></span> <span data-ttu-id="9a120-210">Hello értéke nulla, ha nincsenek nem tárolási kapcsolatos esetleges problémák hello közeljövőben.</span><span class="sxs-lookup"><span data-stu-id="9a120-210">If hello value is zero, there are no potential problems with storage in hello foreseeable future.</span></span> <span data-ttu-id="9a120-211">Hello értéke egy számot a nullától eltérő, ha nincs elegendő tárterület lefoglalt (a védelmi házirend- és hello adatok mérete a védett tagok alapján).</span><span class="sxs-lookup"><span data-stu-id="9a120-211">If hello value is a number other than zero, you do not have sufficient storage allocated  (based on your protection policy and hello data size of your protected members).</span></span>

  ![Szabad kapacitással rendelkező erőforrásokkal lemezes tárolás](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   <span data-ttu-id="9a120-213">a védelmi csoport, teljes hello varázsló létrehozása toofinish.</span><span class="sxs-lookup"><span data-stu-id="9a120-213">toofinish creating your protection group, complete hello wizard.</span></span>

## <a name="migrate-legacy-storage-toomodern-backup-storage"></a><span data-ttu-id="9a120-214">Telepítse át a hagyományos tárolási tooModern Backup-tárhelyre</span><span class="sxs-lookup"><span data-stu-id="9a120-214">Migrate legacy storage tooModern Backup Storage</span></span>
<span data-ttu-id="9a120-215">A frissítés befejezése után tooor telepítése Server biztonsági másolat v2 és frissítési hello operációs rendszer tooWindows Server 2016 frissítse a védelmi csoportok toouse Modern Backup-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="9a120-215">After you upgrade tooor install Backup Server v2 and upgrade hello operating system tooWindows Server 2016, update your protection groups toouse Modern Backup Storage.</span></span> <span data-ttu-id="9a120-216">Alapértelmezés szerint a védelmi csoportok nem változik.</span><span class="sxs-lookup"><span data-stu-id="9a120-216">By default, protection groups are not changed.</span></span> <span data-ttu-id="9a120-217">Toofunction akkor folytatható, mert kezdetben voltak beállítva.</span><span class="sxs-lookup"><span data-stu-id="9a120-217">They continue toofunction as they were initially set up.</span></span> 

<span data-ttu-id="9a120-218">Védelmi csoportok toouse Modern Backup-tárhelyre frissítése nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="9a120-218">Updating protection groups toouse Modern Backup Storage is optional.</span></span> <span data-ttu-id="9a120-219">tooupdate hello védelmi csoport összes adatforrásának hello használatával állítsa le a védelmet továbbra is az adatokat.</span><span class="sxs-lookup"><span data-stu-id="9a120-219">tooupdate hello protection group, stop protection of all data sources by using hello retain data option.</span></span> <span data-ttu-id="9a120-220">Adja hozzá a hello adatok források tooa új védelmi csoportot.</span><span class="sxs-lookup"><span data-stu-id="9a120-220">Then, add hello data sources tooa new protection group.</span></span>

1. <span data-ttu-id="9a120-221">A felügyeleti konzol hello, válassza ki a hello **védelmi** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="9a120-221">In hello Administrator Console, select hello **Protection** feature.</span></span> <span data-ttu-id="9a120-222">A hello **védelmi csoport tagjához** listában, majd válassza ki és kattintson a jobb gombbal a hello tag **tag védelmének kikapcsolása**.</span><span class="sxs-lookup"><span data-stu-id="9a120-222">In hello **Protection Group Member** list, right-click hello member, and then select **Stop protection of member**.</span></span>

  ![Tag védelmének kikapcsolása](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. <span data-ttu-id="9a120-224">A hello **távolítsa el a csoportból** párbeszédpanelen tekintse át a hello használt lemezterület és hello rendelkezésre álló szabad területet hello tárolókészlethez.</span><span class="sxs-lookup"><span data-stu-id="9a120-224">In hello **Remove from Group** dialog box, review hello used disk space and hello available free space for hello storage pool.</span></span> <span data-ttu-id="9a120-225">hello alapértelmezett tooleave hello helyreállítási pontok hello lemezen, és lehetővé teszik a kapcsolódó adatmegőrzési / tooexpire.</span><span class="sxs-lookup"><span data-stu-id="9a120-225">hello default is tooleave hello recovery points on hello disk and allow them tooexpire per their associated retention policy.</span></span> <span data-ttu-id="9a120-226">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9a120-226">Click **OK**.</span></span>

  <span data-ttu-id="9a120-227">Ha tooimmediately használt visszatérési hello szabad terület toohello szabad tárolókészlet, jelölje be a hello **lemezen tárolt replika törlése** jelölőnégyzetet toodelete hello biztonsági mentési adatok (és a helyreállítási pontok) társított tag.</span><span class="sxs-lookup"><span data-stu-id="9a120-227">If you want tooimmediately return hello used disk space toohello free storage pool, select hello **Delete replica on disk** check box toodelete hello backup data (and recovery points) associated with that member.</span></span>

  ![Távolítsa el a csoport párbeszédpanel](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. <span data-ttu-id="9a120-229">Hozzon létre egy védelmi csoportot, amely Modern biztonsági mentési tárolót használ.</span><span class="sxs-lookup"><span data-stu-id="9a120-229">Create a protection group that uses Modern Backup Storage.</span></span> <span data-ttu-id="9a120-230">Védelem nélküli hello adatforrásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9a120-230">Include hello unprotected data sources.</span></span>


## <a name="add-disks-tooincrease-legacy-storage"></a><span data-ttu-id="9a120-231">Lemezek tooincrease örökölt tároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9a120-231">Add disks tooincrease legacy storage</span></span>

<span data-ttu-id="9a120-232">Ha azt szeretné, hogy toouse örökölt storage Server biztonsági másolat, szükség lehet a tooadd lemezek tooincrease örökölt tároló.</span><span class="sxs-lookup"><span data-stu-id="9a120-232">If you want toouse legacy storage with Backup Server, you might need tooadd disks tooincrease legacy storage.</span></span> 

<span data-ttu-id="9a120-233">tooadd lemezterület:</span><span class="sxs-lookup"><span data-stu-id="9a120-233">tooadd disk storage:</span></span>

1. <span data-ttu-id="9a120-234">Hello felügyeleti konzolt, jelölje ki **felügyeleti** > **lemezegységet** > **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9a120-234">In hello Administrator Console, select **Management** > **Disk Storage** > **Add**.</span></span>

    ![Adja hozzá a lemezes tárolás párbeszédpanel](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. <span data-ttu-id="9a120-236">A hello **adja hozzá a lemezes tárolás** párbeszédablakban válassza **vegyen fel lemezeket**.</span><span class="sxs-lookup"><span data-stu-id="9a120-236">In hello **Add Disk Storage** dialog, select **Add disks**.</span></span>

5. <span data-ttu-id="9a120-237">A rendelkezésre álló lemezek hello listában válassza ki a kívánt tooadd, jelölje be a hello lemezek **Hozzáadás**, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a120-237">In hello list of available disks, select hello disks you want tooadd, select **Add**, and then select **OK**.</span></span>

## <a name="update-hello-data-protection-manager-protection-agent"></a><span data-ttu-id="9a120-238">Hello Data Protection Manager védelmi ügynök frissítése</span><span class="sxs-lookup"><span data-stu-id="9a120-238">Update hello Data Protection Manager protection agent</span></span>

<span data-ttu-id="9a120-239">Tartalék kiszolgáló hello System Center Data Protection Manager védelmi ügynök frissítések használ.</span><span class="sxs-lookup"><span data-stu-id="9a120-239">Backup Server uses hello System Center Data Protection Manager protection agent for updates.</span></span> <span data-ttu-id="9a120-240">Ha frissíti a védelmi ügynök, amely nem csatlakoztatott toohello hálózat, a Data Protection Manager felügyeleti konzol toocomplete a csatlakoztatott ügynökök frissítési hello nem használhat.</span><span class="sxs-lookup"><span data-stu-id="9a120-240">If you are upgrading a protection agent that is not connected toohello network, you cannot use hello Data Protection Manager Administrator Console toocomplete a connected agent upgrade.</span></span> <span data-ttu-id="9a120-241">Hello védelmi ügynök egy nem aktív tartomány környezetében kell frissítenie.</span><span class="sxs-lookup"><span data-stu-id="9a120-241">You must upgrade hello protection agent in a nonactive domain environment.</span></span> <span data-ttu-id="9a120-242">Amíg hello ügyfélszámítógép csatlakoztatott toohello hálózati, hello Data Protection Manager felügyeleti konzol jeleníti meg, hogy hello védelmi ügynök frissítése függőben.</span><span class="sxs-lookup"><span data-stu-id="9a120-242">Until hello client computer is connected toohello network, hello Data Protection Manager Administrator Console shows that hello protection agent update is pending.</span></span>

<span data-ttu-id="9a120-243">hello alábbi szakaszok azt ismertetik, hogyan tooupdate védelmi ügynököket a csatlakozó ügyfélszámítógépek és a nem csatlakozó ügyfélszámítógépekről.</span><span class="sxs-lookup"><span data-stu-id="9a120-243">hello following sections describe how tooupdate protection agents for client computers that are connected and client computers that are not connected.</span></span>

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a><span data-ttu-id="9a120-244">Frissítse a védelmi ügynököt a csatlakoztatott ügyfélszámítógépen</span><span class="sxs-lookup"><span data-stu-id="9a120-244">Update a protection agent for a connected client computer</span></span>

1. <span data-ttu-id="9a120-245">Válassza ki a biztonsági mentés Server felügyeleti konzol hello, **felügyeleti** > **ügynökök**.</span><span class="sxs-lookup"><span data-stu-id="9a120-245">In hello Backup Server Administrator Console, select **Management** > **Agents**.</span></span>

2. <span data-ttu-id="9a120-246">Hello megjelenítési ablaktáblán jelölje ki a kívánt tooupdate hello védelmi ügynök hello ügyfélszámítógépeket.</span><span class="sxs-lookup"><span data-stu-id="9a120-246">In hello display pane, select hello client computers for which you want tooupdate hello protection agent.</span></span>

  > [!NOTE]
  > <span data-ttu-id="9a120-247">Hello **Ügynökfrissítéssel** oszlop azt jelzi, ha a védelmi ügynök frissítése a védett számítógépekhez.</span><span class="sxs-lookup"><span data-stu-id="9a120-247">hello **Agent Updates** column indicates when a protection agent update is available for each protected computer.</span></span> <span data-ttu-id="9a120-248">A hello **műveletek** ablaktáblában hello **frissítés** művelet áll rendelkezésre, csak ha egy védett számítógép van kiválasztva, és frissítések érhetők el.</span><span class="sxs-lookup"><span data-stu-id="9a120-248">In hello **Actions** pane, hello **Update** action is available only when a protected computer is selected and updates are available.</span></span>
  >
  >

3. <span data-ttu-id="9a120-249">tooinstall frissíti a védelmi ügynököket a kiválasztott hello számítógépek hello **műveletek** ablaktáblán válassza előbb **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="9a120-249">tooinstall updated protection agents on hello selected computers, in hello **Actions** pane, select **Update**.</span></span>

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a><span data-ttu-id="9a120-250">Egy ügyfélszámítógépen, amely nem csatlakozik a védelmi ügynök frissítése</span><span class="sxs-lookup"><span data-stu-id="9a120-250">Update a protection agent on a client computer that is not connected</span></span>

1. <span data-ttu-id="9a120-251">Válassza ki a biztonsági mentés Server felügyeleti konzol hello, **felügyeleti** > **ügynökök**.</span><span class="sxs-lookup"><span data-stu-id="9a120-251">In hello Backup Server Administrator Console, select **Management** > **Agents**.</span></span>

2. <span data-ttu-id="9a120-252">Hello megjelenítési ablaktáblán jelölje ki a kívánt tooupdate hello védelmi ügynök hello ügyfélszámítógépeket.</span><span class="sxs-lookup"><span data-stu-id="9a120-252">In hello display pane, select hello client computers for which you want tooupdate hello protection agent.</span></span>

  > [!NOTE]
   > <span data-ttu-id="9a120-253">Hello **Ügynökfrissítéssel** oszlop azt jelzi, ha a védelmi ügynök frissítése a védett számítógépekhez.</span><span class="sxs-lookup"><span data-stu-id="9a120-253">hello **Agent Updates** column indicates when a protection agent update is available for each protected computer.</span></span> <span data-ttu-id="9a120-254">A hello **műveletek** ablaktáblában hello **frissítés** művelet nem érhető el, ha egy védett számítógép van kiválasztva, kivéve, ha frissítések érhetők el.</span><span class="sxs-lookup"><span data-stu-id="9a120-254">In hello **Actions** pane, hello **Update** action is not available when a protected computer is selected unless updates are available.</span></span>
  >
  >

3. <span data-ttu-id="9a120-255">tooinstall hello kijelölt számítógépekre, válassza ki a védelmi ügynökök frissítése **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="9a120-255">tooinstall updated protection agents on hello selected computers, select **Update**.</span></span>

4. <span data-ttu-id="9a120-256">Az ügyfélszámítógép, amely nem toohello hálózati csatlakoztatva, amíg a hello számítógép csatlakoztatott toohello hálózati, hello **ügynök állapota** az oszlopban látható a állapotának **frissítés függőben**.</span><span class="sxs-lookup"><span data-stu-id="9a120-256">For a client computer that is not connected toohello network, until hello computer is connected toohello network, hello **Agent Status** column shows a status of **Update Pending**.</span></span>

  <span data-ttu-id="9a120-257">Miután egy ügyfélszámítógép csatlakoztatott toohello hálózati, hello **Ügynökfrissítéssel** hello ügyfélszámítógép az oszlopban látható a állapotának **Frissítéskísérleti**.</span><span class="sxs-lookup"><span data-stu-id="9a120-257">After a client computer is connected toohello network, hello **Agent Updates** column for hello client computer shows a status of **Updating**.</span></span>
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-hello-new-version-with-azure"></a><span data-ttu-id="9a120-258">Helyezze át a hagyományos védelmi csoportok régi verziója szinkronizálási hello új verziója az Azure-ral</span><span class="sxs-lookup"><span data-stu-id="9a120-258">Move legacy Protection groups from old version and sync hello new version with Azure</span></span>

<span data-ttu-id="9a120-259">Amennyiben az Azure Backup Server és az operációs rendszer hello is frissítve lett, áll készen tooprotect új adatforrásokból Modern Backup-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="9a120-259">Once Azure Backup Server and hello OS are both updated, you are ready tooprotect new data sources using Modern Backup Storage.</span></span> <span data-ttu-id="9a120-260">Azonban már védett adatforrások továbbra is toobe védett hello régebbi, mint az Azure Backup Server voltak, de minden új védelmi Modern biztonsági mentés a tárhelyet fogja használni.</span><span class="sxs-lookup"><span data-stu-id="9a120-260">However already protected data sources will continue toobe protected in hello legacy way as they were in Azure Backup Server but all new protection will use Modern Backup Storage.</span></span>

<span data-ttu-id="9a120-261">Következő lépések vannak védelmi tooModern biztonsági mentési tároló örökölt üzemmódú toomigrate erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="9a120-261">Below steps are toomigrate data sources from legacy  mode of protection tooModern backup storage.</span></span>

<span data-ttu-id="9a120-262">• Hello új kötet(ek) toohello DPM-tárolókészlet hozzáadása, és rendelje hozzá a rövid nevek és adatok forrás, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="9a120-262">• Add hello new volume(s) toohello DPM storage pool and assign friendly names and data source tags if desired.</span></span>
<span data-ttu-id="9a120-263">• Minden adatforrás, amely örökölt üzemmódban, hello adatforrások és a "Védett adatok megőrzése" állítsa le a védelmet.</span><span class="sxs-lookup"><span data-stu-id="9a120-263">• For each data source that is in legacy mode, stop protection of hello data sources and “Retain Protected Data”.</span></span>  <span data-ttu-id="9a120-264">Ez lehetővé teszi a helyreállítás a régi helyreállítási pontok áttelepítés után.</span><span class="sxs-lookup"><span data-stu-id="9a120-264">This will allow recovery of old recovery points after migration.</span></span>

<span data-ttu-id="9a120-265">• Hozzon létre egy új PG, és válassza ki, amelyek tárolása a új formátum használatával toobe hello adatforrásokat.</span><span class="sxs-lookup"><span data-stu-id="9a120-265">• Create a new PG and select hello data sources that are toobe stored using new format.</span></span>
<span data-ttu-id="9a120-266">• A DPM rendszer lemásolni egy replika hello korábbi biztonsági mentési tárolóból hello Modern biztonságimásolat-tárolási kötet helyileg történő.</span><span class="sxs-lookup"><span data-stu-id="9a120-266">• DPM will do a replica copy from hello legacy backup storage into hello Modern Backup Storage volume locally.</span></span>
<span data-ttu-id="9a120-267">Megjegyzés: Ez fogja látni, a helyreállítás után a művelet feladat • minden új szinkronizálási és helyreállítási pontok majd tárolódnak a Modern Backup-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="9a120-267">Note: This will be seen as a post-recovery operation job • All new sync and recovery points will then be stored in Modern Backup Storage.</span></span>
<span data-ttu-id="9a120-268">• A régi helyreállítási pontok lesz kiürítve kimenő lejár, és végül hello lemezterület felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="9a120-268">• Old recovery points will be pruned out as they expire and eventually free up hello disk space.</span></span>
<span data-ttu-id="9a120-269">• Összes hello örökölt kötetek hello régi tárterületet, hello lemez a törölt távolíthatók el az Azure biztonsági mentési és hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="9a120-269">• Once all hello legacy volumes are deleted from hello old storage, hello disk can be removed from Azure backup and hello system.</span></span>
<span data-ttu-id="9a120-270">• A hello Azure DPMDB biztonsági mentés készítése.</span><span class="sxs-lookup"><span data-stu-id="9a120-270">• Take a backup of hello  Azure DPMDB.</span></span>

<span data-ttu-id="9a120-271">2. lépés:-fontos elemek > hello új kiszolgáló megnevezett azonos hello eredeti Azure Backup server toobe lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="9a120-271">Part 2: -Important items> hello new server will need toobe named same as hello original Azure Backup server.</span></span> <span data-ttu-id="9a120-272">Hello hello új Azure biztonságimásolat-kiszolgáló neve nem módosítható, ha szeretné, hogy toouse régi tárolókészlet és a DPMDB tooretain helyreállítási pontok - rendelkeznie kell a dpmdb biztonsági másolat, el kell visszaállítani toobe</span><span class="sxs-lookup"><span data-stu-id="9a120-272">You cannot change hello name of hello new Azure backup server if you want toouse old storage pool and DPMDB tooretain recovery points -Must have backup of DPMDB  as it will need toobe restored</span></span>

1) <span data-ttu-id="9a120-273">Leállítás hello eredeti Azure biztonságimásolat-kiszolgálóval, vagy ki hello vezetékes-EK.</span><span class="sxs-lookup"><span data-stu-id="9a120-273">Shutdown hello original Azure backup server or take it off hello wire.</span></span>
2) <span data-ttu-id="9a120-274">Alaphelyzetbe állítja a hello számítógépfiókot az active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="9a120-274">Reset hello machine account in active directory.</span></span>
3) <span data-ttu-id="9a120-275">Server 2016 telepítése új gépen, akkor hello gép neve megegyezik a hello eredeti Azure Backup server neve.</span><span class="sxs-lookup"><span data-stu-id="9a120-275">Install Server 2016 on new machine and name it hello same machine name as hello original Azure Backup server.</span></span>
4) <span data-ttu-id="9a120-276">Csatlakozás tartományhoz hello</span><span class="sxs-lookup"><span data-stu-id="9a120-276">Join hello Domain</span></span>
5) <span data-ttu-id="9a120-277">Telepítse az Azure Backup server V2 (helyezze át a DPM Tárolókészletének lemezeit a régi kiszolgáló és az importálás)</span><span class="sxs-lookup"><span data-stu-id="9a120-277">Install Azure Backup server V2 (Move DPM Storage pool disks from old server and import)</span></span>
6) <span data-ttu-id="9a120-278">2. rész oldalától végrehajtott DPMDB hello visszaállítása</span><span class="sxs-lookup"><span data-stu-id="9a120-278">Restore hello DPMDB taken from end of part 2</span></span>
7) <span data-ttu-id="9a120-279">Hello tárolási csatolása hello eredeti tartalék kiszolgáló toohello új kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="9a120-279">Attach hello storage from hello original backup server toohello new server.</span></span>
8) <span data-ttu-id="9a120-280">A DPMDB hello SQL visszaállítása</span><span class="sxs-lookup"><span data-stu-id="9a120-280">From SQL Restore hello DPMDB</span></span>
9) <span data-ttu-id="9a120-281">A rendszergazdai parancssorból az új kiszolgáló cd tooMicrosoft Azure Backup szolgáltatás telepítése a helyét és a bin mappa</span><span class="sxs-lookup"><span data-stu-id="9a120-281">From admin command line on new server cd tooMicrosoft Azure Backup install location and bin folder</span></span>

<span data-ttu-id="9a120-282">Példa az útvonalra: C:\windows\system32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\\</span><span class="sxs-lookup"><span data-stu-id="9a120-282">Path example: C:\windows\system32>cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\\</span></span>
<span data-ttu-id="9a120-283">tooAzure biztonsági másolat, futtassa a DPMSYNC-SYNC</span><span class="sxs-lookup"><span data-stu-id="9a120-283">tooAzure backup Run DPMSYNC -SYNC</span></span>

10) <span data-ttu-id="9a120-284">Futtassa a DPMSYNC-SYNC Megjegyzés felvett új lemezek toohello DPM tárolókészlet helyett hello régiek, futtassa a DPMSYNC - Reallocatereplica</span><span class="sxs-lookup"><span data-stu-id="9a120-284">Run DPMSYNC -SYNC Note If you have added NEW disks toohello DPM Storage pool instead of moving hello old ones, then run DPMSYNC -Reallocatereplica</span></span>

## <a name="new-powershell-cmdlets-in-v2"></a><span data-ttu-id="9a120-285">A v2 új PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="9a120-285">New PowerShell cmdlets in v2</span></span>

<span data-ttu-id="9a120-286">Ha telepíti az Azure Backup Server v2, a két új parancsmagok érhetők el:</span><span class="sxs-lookup"><span data-stu-id="9a120-286">When you install Azure Backup Server v2, two new cmdlets are available:</span></span> 
* [<span data-ttu-id="9a120-287">Csatlakoztatási-DPMRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="9a120-287">Mount-DPMRecoveryPoint</span></span>](https://technet.microsoft.com/library/mt787159.aspx)
* [<span data-ttu-id="9a120-288">Dismount-DPMRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="9a120-288">Dismount-DPMRecoveryPoint</span></span>](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a><span data-ttu-id="9a120-289">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a120-289">Next steps</span></span>

<span data-ttu-id="9a120-290">Megtudhatja, hogyan tooprepare a kiszolgáló vagy a munkaterhelések védelmének megkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="9a120-290">Learn how tooprepare your server or begin protecting a workload:</span></span>
- [<span data-ttu-id="9a120-291">A kiszolgálói biztonsági mentési feladatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="9a120-291">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="9a120-292">Használja a VMware Server biztonsági másolat Server tooback</span><span class="sxs-lookup"><span data-stu-id="9a120-292">Use Backup Server tooback up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="9a120-293">SQL Server biztonsági másolat Server tooback használata</span><span class="sxs-lookup"><span data-stu-id="9a120-293">Use Backup Server tooback up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="9a120-294">Modern biztonsági mentési tárhelyet használ a helykiszolgáló biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="9a120-294">Use Modern Backup Storage with Backup Server</span></span>](backup-mabs-add-storage.md)

