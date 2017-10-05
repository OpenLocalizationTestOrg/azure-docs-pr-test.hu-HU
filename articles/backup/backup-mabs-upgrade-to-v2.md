---
title: "Telepítse az Azure biztonságimásolat-kiszolgáló v2 |} Microsoft Docs"
description: "Az Azure Backup Server v2 lehetővé teszi a virtuális gép, fájlok és mappák, munkaterhelések és több védelméhez továbbfejlesztett biztonsági mentési lehetőségeket. Megtudhatja, hogyan telepítse, vagy frissítsen az Azure Backup Server v2."
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
ms.openlocfilehash: 1bbb16afef7940933b4c3ae23873f212770137e0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="install-azure-backup-server-v2"></a><span data-ttu-id="cb845-104">Azure biztonságimásolat-kiszolgáló v2 telepítése</span><span class="sxs-lookup"><span data-stu-id="cb845-104">Install Azure Backup Server v2</span></span>

<span data-ttu-id="cb845-105">Az Azure Backup Server a virtuális gépek (VM) munkaterhelések, fájlok és mappák és több védi.</span><span class="sxs-lookup"><span data-stu-id="cb845-105">Azure Backup Server helps protect your virtual machines (VMs), workloads, files and folders, and more.</span></span> <span data-ttu-id="cb845-106">Az Azure Backup Server v2 Azure Backup Server v1 épül, és lehetővé teszi az új szolgáltatások nem érhetők el az 1-es verzió.</span><span class="sxs-lookup"><span data-stu-id="cb845-106">Azure Backup Server v2 builds on Azure Backup Server v1, and gives you new features that are not available in v1.</span></span> <span data-ttu-id="cb845-107">A szolgáltatások közötti v1 és v2, lásd: [Azure Backup Server védelmi mátrix](backup-mabs-protection-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="cb845-107">For a comparison of features between v1 and v2, see [Azure Backup Server protection matrix](backup-mabs-protection-matrix.md).</span></span> 

<span data-ttu-id="cb845-108">A biztonsági mentés kiszolgáló v2 további funkciók a biztonsági mentés kiszolgáló v1 frissítés.</span><span class="sxs-lookup"><span data-stu-id="cb845-108">The additional features in Backup Server v2 are an upgrade from Backup Server v1.</span></span> <span data-ttu-id="cb845-109">Biztonsági mentés kiszolgáló v1 viszont nem a biztonsági mentés kiszolgáló v2 telepítésének előfeltétele.</span><span class="sxs-lookup"><span data-stu-id="cb845-109">However, Backup Server v1 is not a prerequisite for installing Backup Server v2.</span></span> <span data-ttu-id="cb845-110">Ha szeretné frissíteni a biztonsági mentés kiszolgáló v1 Backup Server v2, telepítse a biztonsági mentés kiszolgáló v2-kiszolgálón a biztonsági mentés védelmi.</span><span class="sxs-lookup"><span data-stu-id="cb845-110">If you want to upgrade from Backup Server v1 to Backup Server v2, install Backup Server v2 on the Backup Server protection server.</span></span> <span data-ttu-id="cb845-111">A meglévő biztonsági mentés beállításait változatlanok maradnak.</span><span class="sxs-lookup"><span data-stu-id="cb845-111">Your existing Backup Server settings remain intact.</span></span>

<span data-ttu-id="cb845-112">Biztonsági mentés kiszolgáló v2 telepíthető Windows Server 2012 R2 vagy Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="cb845-112">You can install Backup Server v2 on Windows Server 2012 R2 or Windows Server 2016.</span></span> <span data-ttu-id="cb845-113">Új szolgáltatások, mint a System Center 2016 adatok Protection Manager Modern Backup-tárhelyre előnyeit, telepítenie kell biztonsági mentés kiszolgáló v2 Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="cb845-113">To take advantage of new features like System Center 2016 Data Protection Manager Modern Backup Storage, you must install Backup Server v2 on Windows Server 2016.</span></span> <span data-ttu-id="cb845-114">Előtt, frissítsen, vagy telepítse a biztonsági mentés kiszolgáló v2, olvassa el a [telepítésének előfeltételei](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="cb845-114">Before you upgrade to or install Backup Server v2, read about the [installation prerequisites](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).</span></span>

> [!NOTE]
> <span data-ttu-id="cb845-115">Az Azure Backup Server alap, a System Center Data Protection Manager ugyanazt a kódot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cb845-115">Azure Backup Server has the same code base as System Center Data Protection Manager.</span></span> <span data-ttu-id="cb845-116">Tartalék kiszolgáló v1 megegyezik a Data Protection Manager 2012 R2-re, és biztonsági mentés kiszolgáló v2 megegyezik a Data Protection Manager 2016-os.</span><span class="sxs-lookup"><span data-stu-id="cb845-116">Backup Server v1 is equivalent to Data Protection Manager 2012 R2, and Backup Server v2 is equivalent to Data Protection Manager 2016.</span></span> <span data-ttu-id="cb845-117">Ez a cikk alkalmanként a Data Protection Manager dokumentációs hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="cb845-117">This article occasionally references the Data Protection Manager documentation.</span></span>
>
>

## <a name="upgrade-backup-server-to-v2"></a><span data-ttu-id="cb845-118">Tartalék kiszolgáló frissítsen v2</span><span class="sxs-lookup"><span data-stu-id="cb845-118">Upgrade Backup Server to v2</span></span>
<span data-ttu-id="cb845-119">A biztonsági mentés kiszolgáló v1 frissítsen Backup Server v2, győződjön meg a telepítést a szükséges frissítéseket:</span><span class="sxs-lookup"><span data-stu-id="cb845-119">To upgrade from Backup Server v1 to Backup Server v2, make sure your installation has the required updates:</span></span>

- <span data-ttu-id="cb845-120">[A védelmi ügynökök frissítésének](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) a védett kiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="cb845-120">[Update the protection agents](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) on the protected servers.</span></span>
- <span data-ttu-id="cb845-121">Windows Server 2012 R2, Windows Server 2016 váltson.</span><span class="sxs-lookup"><span data-stu-id="cb845-121">Upgrade Windows Server 2012 R2 to Windows Server 2016.</span></span>
- <span data-ttu-id="cb845-122">Azure biztonsági mentés kiszolgáló távoli felügyeleti frissítse az összes üzemi kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="cb845-122">Upgrade Azure Backup Server Remote Administrator on all production servers.</span></span>
- <span data-ttu-id="cb845-123">Győződjön meg arról, hogy a biztonsági mentés van beállítva az üzemi kiszolgáló újraindítása nélkül szeretné folytatni.</span><span class="sxs-lookup"><span data-stu-id="cb845-123">Ensure that backups are set to continue without restarting your production server.</span></span>


### <a name="upgrade-steps-for-backup-server-v2"></a><span data-ttu-id="cb845-124">Biztonsági mentés kiszolgáló 2-es verzió frissítési lépések</span><span class="sxs-lookup"><span data-stu-id="cb845-124">Upgrade steps for Backup Server v2</span></span>

1. <span data-ttu-id="cb845-125">A letöltőközpontban [töltse le a frissítési telepítő](https://go.microsoft.com/fwlink/?LinkId=626082).</span><span class="sxs-lookup"><span data-stu-id="cb845-125">In the Download Center, [download the upgrade installer](https://go.microsoft.com/fwlink/?LinkId=626082).</span></span>

2. <span data-ttu-id="cb845-126">Miután a telepítő varázsló bontsa ki, győződjön meg arról, hogy **hajtható végre a setup.exe** kiválasztva, majd jelölje ki **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="cb845-126">After you extract the setup wizard, make sure that **Execute setup.exe** is selected, and then select **Finish**.</span></span>

  ![A telepítő installer - futtassa a telepítő](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. <span data-ttu-id="cb845-128">A Microsoft Azure Backup Server varázslóban a **telepítése**, jelölje be **Microsoft Azure Backup Server**.</span><span class="sxs-lookup"><span data-stu-id="cb845-128">In the Microsoft Azure Backup Server wizard, under **Install**, select **Microsoft Azure Backup Server**.</span></span>

  ![A telepítő installer - válassza telepítése](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. <span data-ttu-id="cb845-130">Az a **üdvözlő** lapon olvassa el a figyelmeztetéseket, és válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="cb845-130">On the **Welcome** page, review the warnings, and then select **Next**.</span></span>

  ![A telepítő installer - kezdőlap](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. <span data-ttu-id="cb845-132">A telepítővarázsló előfeltétel-ellenőrzéseket győződjön meg arról, hogy a környezet frissíthető hajt végre.</span><span class="sxs-lookup"><span data-stu-id="cb845-132">The setup wizard performs prerequisite checks to make sure your environment can upgrade.</span></span> <span data-ttu-id="cb845-133">Az a **előfeltételek ellenőrzésének** lapon jelölje be **ellenőrizze**.</span><span class="sxs-lookup"><span data-stu-id="cb845-133">On the **Prerequisite Checks** page, select **Check**.</span></span>

  ![A telepítő telepítő – Előfeltételek ellenőrzése lap](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. <span data-ttu-id="cb845-135">A környezet meg kell felelnie az előfeltétel-ellenőrzések.</span><span class="sxs-lookup"><span data-stu-id="cb845-135">Your environment must pass the prerequisite checks.</span></span> <span data-ttu-id="cb845-136">Ha a környezet nem feleltek meg a során, vegye figyelembe a problémákat, és kijavíthatja azokat.</span><span class="sxs-lookup"><span data-stu-id="cb845-136">If your environment doesn't pass the checks, note the issues and fix them.</span></span> <span data-ttu-id="cb845-137">Ezt követően válassza **ellenőrizze újra**.</span><span class="sxs-lookup"><span data-stu-id="cb845-137">Then, select **Check Again**.</span></span> <span data-ttu-id="cb845-138">Amikor az előfeltétel-ellenőrzést, válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="cb845-138">After you pass the prerequisite checks, select **Next**.</span></span>

  ![A telepítő installer - ellenőrizze újra gomb](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. <span data-ttu-id="cb845-140">Az a **SQL-beállítások** lapon, a megfelelő beállítást az SQL-telepítés, majd válassza ki és **ellenőrzés és telepítés**.</span><span class="sxs-lookup"><span data-stu-id="cb845-140">On the **SQL Settings** page, select the relevant option for your SQL installation, and then select **Check and Install**.</span></span>

  ![A telepítő installer - SQL-beállítások lap](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  <span data-ttu-id="cb845-142">Az ellenőrzések néhány percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="cb845-142">The checks might take a few minutes.</span></span> <span data-ttu-id="cb845-143">Az ellenőrzések befejezésekor válassza **következő**.</span><span class="sxs-lookup"><span data-stu-id="cb845-143">When the checks are finished, select **Next**.</span></span>

  ![A telepítő installer - SQL ellenőrizze és telepítése gombra](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. <span data-ttu-id="cb845-145">Az a **telepítési beállítások** lapon, a helyre, ahol a biztonsági mentés Server telepítve van, vagy az ideiglenes helyre végezze el a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="cb845-145">On the **Installation Settings** page, make any changes to the location where Backup Server is installed, or to the Scratch Location.</span></span> <span data-ttu-id="cb845-146">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="cb845-146">Select **Next**.</span></span>

  ![A telepítő installer - telepítési beállítások lapon](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. <span data-ttu-id="cb845-148">A telepítő varázsló befejezéséhez válassza ki a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="cb845-148">To finish the setup wizard, select **Finish**.</span></span>

  ![A telepítő telepítő - a befejezési](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a><span data-ttu-id="cb845-150">Tároló hozzáadása a Modern Backup-tárhelyre</span><span class="sxs-lookup"><span data-stu-id="cb845-150">Add storage for Modern Backup Storage</span></span>

<span data-ttu-id="cb845-151">Biztonsági mentési tároló-hatékonyságot biztosít javítása érdekében tartalék kiszolgáló v2 támogatást nyújt a köteteket.</span><span class="sxs-lookup"><span data-stu-id="cb845-151">To improve backup storage efficiency, Backup Server v2 adds support for volumes.</span></span> <span data-ttu-id="cb845-152">Biztonsági mentés kiszolgáló v1, például a biztonsági mentés kiszolgáló v2 lemezeit támogatja.</span><span class="sxs-lookup"><span data-stu-id="cb845-152">Like Backup Server v1, Backup Server v2 supports disks.</span></span>

### <a name="add-volumes-and-disks"></a><span data-ttu-id="cb845-153">Kötetek és a lemezek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cb845-153">Add volumes and disks</span></span>
<span data-ttu-id="cb845-154">Ha Windows Server 2016 tartalék kiszolgáló v2 futtatja, használhatja a kötetek biztonsági mentési adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="cb845-154">If you run Backup Server v2 on Windows Server 2016, you can use volumes to store backup data.</span></span> <span data-ttu-id="cb845-155">Kötet nyújtja a tárhely-megtakarítást és gyorsabb biztonsági mentéseket.</span><span class="sxs-lookup"><span data-stu-id="cb845-155">Volumes offer storage savings and faster backups.</span></span> <span data-ttu-id="cb845-156">Mivel kötetek új biztonsági másolat kiszolgálóhoz, hozzá kell adnia őket.</span><span class="sxs-lookup"><span data-stu-id="cb845-156">Because volumes are new to Backup Server, you must add them.</span></span> 

<span data-ttu-id="cb845-157">A kötet biztonsági mentés kiszolgáló hozzáadásakor adhat egy rövid nevet a kötetet.</span><span class="sxs-lookup"><span data-stu-id="cb845-157">When you add a volume to Backup Server, you can give the volume a friendly name.</span></span> <span data-ttu-id="cb845-158">Kattintson a **rövid név** nevet szeretne oszlopban.</span><span class="sxs-lookup"><span data-stu-id="cb845-158">Click the **Friendly Name** column of the volume you want to name.</span></span> <span data-ttu-id="cb845-159">Később bármikor módosíthatja a nevet, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="cb845-159">You can change the name later, if necessary.</span></span> <span data-ttu-id="cb845-160">Is használhatja PowerShell hozzáadása vagy módosítása kötetek rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="cb845-160">You also can use PowerShell to add or change friendly names for volumes.</span></span>

<span data-ttu-id="cb845-161">A kötet hozzáadása a felügyeleti konzol:</span><span class="sxs-lookup"><span data-stu-id="cb845-161">To add a volume in the Administrator Console:</span></span>

1. <span data-ttu-id="cb845-162">Válassza ki a Azure Backup Server felügyeleti konzol **felügyeleti** > **lemezegységet** > **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="cb845-162">In the Azure Backup Server Administrator Console, select **Management** > **Disk Storage** > **Add**.</span></span>

    ![Nyissa meg a lemezes tárolás hozzáadása varázsló](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    <span data-ttu-id="cb845-164">Ekkor megnyílik a lemezes tárolás hozzáadása varázsló.</span><span class="sxs-lookup"><span data-stu-id="cb845-164">This opens the Add Disk Storage wizard.</span></span>

2. <span data-ttu-id="cb845-165">A a **adja hozzá a lemezes tárolás** lap a **rendelkezésre álló köteteken** mezőben, válassza ki a kötet, majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="cb845-165">On the **Add Disk Storage** page, in the **Available volumes** box, select a volume, and then select **Add**.</span></span>
3. <span data-ttu-id="cb845-166">Az a **kijelölt kötetek** mezőbe, adjon meg egy rövid nevet a kötet, és válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="cb845-166">In the **Selected volumes** box, enter a friendly name for the volume, and then select **OK**.</span></span>

      ![Adja hozzá a lemezes tárolás varázsló – a kötet hozzáadása](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  <span data-ttu-id="cb845-168">Ha hozzá szeretne adni egy lemezt, a lemez egy örökölt tárolási rendelkező védelmi csoporthoz kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="cb845-168">If you want to add a disk, the disk must belong to a protection group that has legacy storage.</span></span> <span data-ttu-id="cb845-169">Ezek a lemezek csak a védelmi csoportok használható.</span><span class="sxs-lookup"><span data-stu-id="cb845-169">These disks can only be used for these protection groups.</span></span> <span data-ttu-id="cb845-170">Ha a biztonsági kiszolgáló nem kapcsolódik az adatforrásokat, amelyek az örökölt védelmet, a lemez nem szerepel a listában.</span><span class="sxs-lookup"><span data-stu-id="cb845-170">If Backup Server doesn't have sources that have legacy protection, the disk isn't listed.</span></span>

  <span data-ttu-id="cb845-171">Lemezek hozzáadásával kapcsolatos további információkért lásd: [örökölt tárolási növelje a lemezek hozzáadása a](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage).</span><span class="sxs-lookup"><span data-stu-id="cb845-171">For more information about adding disks, see [Adding disks to increase legacy storage](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage).</span></span> <span data-ttu-id="cb845-172">A lemez nem adjon egy rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="cb845-172">You can't give a disk a friendly name.</span></span>


### <a name="assign-workloads-to-volumes"></a><span data-ttu-id="cb845-173">Munkaterhelések hozzárendelése kötetek</span><span class="sxs-lookup"><span data-stu-id="cb845-173">Assign workloads to volumes</span></span>

<span data-ttu-id="cb845-174">A kiszolgáló biztonsági mentése megadhatja, mely munkaterhelések kötetek vannak rendelve.</span><span class="sxs-lookup"><span data-stu-id="cb845-174">In Backup Server, you specify which workloads are assigned to which volumes.</span></span> <span data-ttu-id="cb845-175">Például beállíthat költséges kötetek, amelyek támogatják a bemeneti/kimeneti műveletek száma másodpercenként (IOPS) túl magas száma csak azok a szolgáltatások, amelyek rendszeres, nagy mennyiségű biztonsági másolatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="cb845-175">For example, you can set expensive volumes that support a high number of input/output operations per second (IOPS) to store only workloads that require frequent, high-volume backups.</span></span> <span data-ttu-id="cb845-176">Példa: SQL Server, a tranzakciós naplók.</span><span class="sxs-lookup"><span data-stu-id="cb845-176">An example is SQL Server with transaction logs.</span></span>

#### <a name="update-dpmdiskstorage"></a><span data-ttu-id="cb845-177">Frissítés-DPMDiskStorage</span><span class="sxs-lookup"><span data-stu-id="cb845-177">Update-DPMDiskStorage</span></span>

<span data-ttu-id="cb845-178">Frissítés-DPMDiskStorage PowerShell-parancsmag segítségével egy kötetet a biztonsági mentés Server tárolókészletben tulajdonságainak frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cb845-178">To update the properties of a volume in the storage pool in Backup Server, use the PowerShell cmdlet Update-DPMDiskStorage.</span></span>

<span data-ttu-id="cb845-179">Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="cb845-179">Syntax:</span></span>

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

<span data-ttu-id="cb845-180">Az összes végrehajtott módosításokat a PowerShell használatával is megjelennek a felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="cb845-180">All changes that you make by using PowerShell are reflected in the UI.</span></span>


## <a name="protect-data-sources"></a><span data-ttu-id="cb845-181">Adatforrások védelméhez</span><span class="sxs-lookup"><span data-stu-id="cb845-181">Protect data sources</span></span>
<span data-ttu-id="cb845-182">Az adatforrások védelmét, létre kell hoznia egy védelmi csoportot.</span><span class="sxs-lookup"><span data-stu-id="cb845-182">To begin protecting data sources, create a protection group.</span></span> <span data-ttu-id="cb845-183">Az alábbi lépéseket kiemelése módosításait és kiegészítéseit az új védelmi csoport létrehozása varázsló.</span><span class="sxs-lookup"><span data-stu-id="cb845-183">The following steps highlight changes or additions to the New Protection Group wizard.</span></span>

<span data-ttu-id="cb845-184">A védelmi csoport létrehozása:</span><span class="sxs-lookup"><span data-stu-id="cb845-184">To create a protection group:</span></span>

1. <span data-ttu-id="cb845-185">Válassza ki a biztonsági mentés Server felügyeleti konzol **védelmi**.</span><span class="sxs-lookup"><span data-stu-id="cb845-185">In the Backup Server Administrator Console, select **Protection**.</span></span>

2. <span data-ttu-id="cb845-186">Válassza ki az eszközök menüszalagján **új**.</span><span class="sxs-lookup"><span data-stu-id="cb845-186">On the tool ribbon, select **New**.</span></span>

    <span data-ttu-id="cb845-187">Ekkor megnyílik az új védelmi csoport létrehozása varázsló.</span><span class="sxs-lookup"><span data-stu-id="cb845-187">This opens the Create New Protection Group wizard.</span></span>

  ![Új védelmi csoport létrehozása varázsló](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. <span data-ttu-id="cb845-189">Az a **üdvözlő** lapon jelölje be **következő**.</span><span class="sxs-lookup"><span data-stu-id="cb845-189">On the **Welcome** page, select **Next**.</span></span>
4. <span data-ttu-id="cb845-190">Az a **védelmi csoport típusának kiválasztása** lapon, válassza ki a védelmi csoport létrehozásához, és válassza ki a kívánt **következő**.</span><span class="sxs-lookup"><span data-stu-id="cb845-190">On the **Select Protection Group Type** page, select the type of protection group you want to create, and then select **Next**.</span></span>

  ![Válassza ki a védelmi csoport típusa lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. <span data-ttu-id="cb845-192">Az a **csoporttagok kiválasztása** lap a **választható tagok** ablaktáblán, a tagokat a védelmi ügynökök találhatók.</span><span class="sxs-lookup"><span data-stu-id="cb845-192">On the **Select Group Members** page, in the **Available members** pane, the members with protection agents are listed.</span></span> <span data-ttu-id="cb845-193">Ehhez a példához válassza ki a kötetet a D:\ és E:\, és hozzáadhatja a **kijelölt tagok** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="cb845-193">For this example, select volume D:\ and E:\ and add them to the **Selected members** pane.</span></span> <span data-ttu-id="cb845-194">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="cb845-194">Select **Next**.</span></span>

  ![Csoport kijelölése tagok lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. <span data-ttu-id="cb845-196">A a **adatvédelmi módszer kiválasztása** lapján adjon meg egy **védelmi csoport neve**, a védelmi módszert, majd válassza ki és **következő**.</span><span class="sxs-lookup"><span data-stu-id="cb845-196">On the **Select Data Protection Method** page, enter a **Protection group name**, select the protection method, and then select **Next**.</span></span> <span data-ttu-id="cb845-197">Ha azt szeretné, hogy a rövid távú védelem, ki kell választania a **lemez** metódus biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="cb845-197">If you want short-term protection, you must select the **Disk** backup method.</span></span>

  ![Adatvédelmi módszer kiválasztása lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. <span data-ttu-id="cb845-199">Az a **rövid távú célok megadása** lapon, válassza ki a részletes **megőrzési időtartam** és **szinkronizálási gyakoriság**.</span><span class="sxs-lookup"><span data-stu-id="cb845-199">On the **Specify Short-Term Goals** page, select the details for **Retention range** and **Synchronization frequency**.</span></span> <span data-ttu-id="cb845-200">Ezt követően válassza **következő**.</span><span class="sxs-lookup"><span data-stu-id="cb845-200">Then, select **Next**.</span></span> <span data-ttu-id="cb845-201">Ha szükséges, amikor készít a helyreállítási pontok ütemezésének módosításához válassza **módosítás**.</span><span class="sxs-lookup"><span data-stu-id="cb845-201">Optionally, to change the schedule for when recovery points are taken, select **Modify**.</span></span>

  ![Adja meg a rövid távú célok lapja](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. <span data-ttu-id="cb845-203">Az a **lemezterület-foglalás tekintse át** lapon tekintse át adatforrásokra vonatkozó beállítást jelölte ki, a mérete és részletes a kiépítendő terület és a célként megadott tárolási kötet értékeit.</span><span class="sxs-lookup"><span data-stu-id="cb845-203">On the **Review Disk Storage Allocation** page, review details about the data sources you selected, their size, and  values for the space to be provisioned and the target storage volume.</span></span>

  ![Áttekintés lemezterület-foglalás lap](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  <span data-ttu-id="cb845-205">Tárolási köteteket a munkaterhelés kötet foglalási (beállítása a PowerShell használatával) és a rendelkezésre álló tár alapulnak.</span><span class="sxs-lookup"><span data-stu-id="cb845-205">Storage volumes are based on the workload volume allocation (set by using PowerShell) and the available storage.</span></span> <span data-ttu-id="cb845-206">A tároló kötetek más kötetek kiválasztja a legördülő menüben módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="cb845-206">You can change the storage volumes by selecting other volumes in the drop-down menu.</span></span> <span data-ttu-id="cb845-207">Ha megváltoztatja a **célként megadott**, értéke **szabad tárhely** dinamikusan megfelelően változik értékeket az **szabad terület** és **Underprovisioned terület**.</span><span class="sxs-lookup"><span data-stu-id="cb845-207">If you change the value for **Target Storage**, the value for **Available disk storage** dynamically changes to reflect values under **Free Space** and **Underprovisioned Space**.</span></span>

  <span data-ttu-id="cb845-208">Ha adatforrások nő, ahogy a tervezett, értékét a **Underprovisioned terület** oszlopa **szabad tárhely** tükrözi a további szükséges tárolókapacitást.</span><span class="sxs-lookup"><span data-stu-id="cb845-208">If the data sources grow as planned, the value for the **Underprovisioned Space** column in **Available disk storage** reflects the amount of additional storage that's needed.</span></span> <span data-ttu-id="cb845-209">Ez az érték a segítségével zökkenőmentes biztonsági másolatok tárolási igényeinek tervezése.</span><span class="sxs-lookup"><span data-stu-id="cb845-209">Use this value to help plan your storage needs for smooth backups.</span></span> <span data-ttu-id="cb845-210">Ha az érték nulla, nincsenek nem tárolási érintő lehetséges problémákat a közeljövőben.</span><span class="sxs-lookup"><span data-stu-id="cb845-210">If the value is zero, there are no potential problems with storage in the foreseeable future.</span></span> <span data-ttu-id="cb845-211">Ha az érték egy számot a nullától eltérő, nincs elegendő tárhely lefoglalt (a védelmi házirend és az adatok mérete a védett tagok alapján).</span><span class="sxs-lookup"><span data-stu-id="cb845-211">If the value is a number other than zero, you do not have sufficient storage allocated  (based on your protection policy and the data size of your protected members).</span></span>

  ![Szabad kapacitással rendelkező erőforrásokkal lemezes tárolás](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   <span data-ttu-id="cb845-213">A védelmi csoport létrehozásának befejezéséhez fejezze be a varázslót.</span><span class="sxs-lookup"><span data-stu-id="cb845-213">To finish creating your protection group, complete the wizard.</span></span>

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a><span data-ttu-id="cb845-214">Modern Backup-tárhelyre örökölt tárolójának áttelepítése</span><span class="sxs-lookup"><span data-stu-id="cb845-214">Migrate legacy storage to Modern Backup Storage</span></span>
<span data-ttu-id="cb845-215">Vagy a biztonsági mentés kiszolgáló v2 telepítéséhez, és frissítse az operációs rendszert a Windows Server 2016 frissítése után frissítse a Modern biztonsági mentési tárolót használjanak a védelmi csoportok.</span><span class="sxs-lookup"><span data-stu-id="cb845-215">After you upgrade to or install Backup Server v2 and upgrade the operating system to Windows Server 2016, update your protection groups to use Modern Backup Storage.</span></span> <span data-ttu-id="cb845-216">Alapértelmezés szerint a védelmi csoportok nem változik.</span><span class="sxs-lookup"><span data-stu-id="cb845-216">By default, protection groups are not changed.</span></span> <span data-ttu-id="cb845-217">Ezek működésének fenntartása mellett azokat eredetileg állított be.</span><span class="sxs-lookup"><span data-stu-id="cb845-217">They continue to function as they were initially set up.</span></span> 

<span data-ttu-id="cb845-218">Védelmi csoportok Modern Backup-tárhelyre használandó frissítése nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="cb845-218">Updating protection groups to use Modern Backup Storage is optional.</span></span> <span data-ttu-id="cb845-219">A védelmi csoport frissítésére, állítsa le az összes adatforrás védelmét az adatmegőrzési opció használatával.</span><span class="sxs-lookup"><span data-stu-id="cb845-219">To update the protection group, stop protection of all data sources by using the retain data option.</span></span> <span data-ttu-id="cb845-220">Ezt követően vegye fel az adatforrásokat egy új védelmi csoportba.</span><span class="sxs-lookup"><span data-stu-id="cb845-220">Then, add the data sources to a new protection group.</span></span>

1. <span data-ttu-id="cb845-221">A felügyeleti konzolon válassza ki a **védelmi** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cb845-221">In the Administrator Console, select the **Protection** feature.</span></span> <span data-ttu-id="cb845-222">Az a **védelmi csoport tagjához** listában kattintson a jobb gombbal a tagja, és válassza ki **tag védelmének kikapcsolása**.</span><span class="sxs-lookup"><span data-stu-id="cb845-222">In the **Protection Group Member** list, right-click the member, and then select **Stop protection of member**.</span></span>

  ![Tag védelmének kikapcsolása](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. <span data-ttu-id="cb845-224">Az a **távolítsa el a csoportból** párbeszédpanelen tekintse át a felhasznált lemezterület és a rendelkezésre álló szabad területet a tárolókészlethez.</span><span class="sxs-lookup"><span data-stu-id="cb845-224">In the **Remove from Group** dialog box, review the used disk space and the available free space for the storage pool.</span></span> <span data-ttu-id="cb845-225">Az alapértelmezett érték a helyreállítási pontok a lemezen, és lehetővé teszik a kapcsolódó adatmegőrzési / lejár.</span><span class="sxs-lookup"><span data-stu-id="cb845-225">The default is to leave the recovery points on the disk and allow them to expire per their associated retention policy.</span></span> <span data-ttu-id="cb845-226">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="cb845-226">Click **OK**.</span></span>

  <span data-ttu-id="cb845-227">Ha azt szeretné, azonnal vissza foglalni a szabad lemezterület, válassza a **lemezen tárolt replika törlése** melletti jelölőnégyzetet, hogy törölje a biztonsági mentési adatok (és a helyreállítási pontok) társított tag.</span><span class="sxs-lookup"><span data-stu-id="cb845-227">If you want to immediately return the used disk space to the free storage pool, select the **Delete replica on disk** check box to delete the backup data (and recovery points) associated with that member.</span></span>

  ![Távolítsa el a csoport párbeszédpanel](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. <span data-ttu-id="cb845-229">Hozzon létre egy védelmi csoportot, amely Modern biztonsági mentési tárolót használ.</span><span class="sxs-lookup"><span data-stu-id="cb845-229">Create a protection group that uses Modern Backup Storage.</span></span> <span data-ttu-id="cb845-230">A nem védett adatforrások tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cb845-230">Include the unprotected data sources.</span></span>


## <a name="add-disks-to-increase-legacy-storage"></a><span data-ttu-id="cb845-231">Vegyen fel lemezeket a hagyományos tárolási növelése</span><span class="sxs-lookup"><span data-stu-id="cb845-231">Add disks to increase legacy storage</span></span>

<span data-ttu-id="cb845-232">Ha azt szeretné, hogy örökölt tárhelyet használ a biztonsági mentés kiszolgáló, szükség lehet vegyen fel lemezeket a hagyományos tárolási növelése érdekében.</span><span class="sxs-lookup"><span data-stu-id="cb845-232">If you want to use legacy storage with Backup Server, you might need to add disks to increase legacy storage.</span></span> 

<span data-ttu-id="cb845-233">Lemezes tárolás hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="cb845-233">To add disk storage:</span></span>

1. <span data-ttu-id="cb845-234">Válassza ki a felügyeleti konzol **felügyeleti** > **lemezegységet** > **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="cb845-234">In the Administrator Console, select **Management** > **Disk Storage** > **Add**.</span></span>

    ![Adja hozzá a lemezes tárolás párbeszédpanel](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. <span data-ttu-id="cb845-236">Az a **adja hozzá a lemezes tárolás** párbeszédablakban válassza **vegyen fel lemezeket**.</span><span class="sxs-lookup"><span data-stu-id="cb845-236">In the **Add Disk Storage** dialog, select **Add disks**.</span></span>

5. <span data-ttu-id="cb845-237">A rendelkezésre álló lemezek, jelölje ki szeretné hozzáadni, válassza a lemezek **Hozzáadás**, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="cb845-237">In the list of available disks, select the disks you want to add, select **Add**, and then select **OK**.</span></span>

## <a name="update-the-data-protection-manager-protection-agent"></a><span data-ttu-id="cb845-238">A Data Protection Manager védelmi ügynök frissítése</span><span class="sxs-lookup"><span data-stu-id="cb845-238">Update the Data Protection Manager protection agent</span></span>

<span data-ttu-id="cb845-239">Helykiszolgáló biztonsági mentése a System Center Data Protection Manager védelmi ügynök használja a frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="cb845-239">Backup Server uses the System Center Data Protection Manager protection agent for updates.</span></span> <span data-ttu-id="cb845-240">Ha frissíti a védelmi ügynök, amely nem kapcsolódik a hálózathoz, a Data Protection Manager rendszergazdai konzol nem használhat a csatlakoztatott ügynökök frissítési befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="cb845-240">If you are upgrading a protection agent that is not connected to the network, you cannot use the Data Protection Manager Administrator Console to complete a connected agent upgrade.</span></span> <span data-ttu-id="cb845-241">Frissítenie kell a védelmi ügynök egy nem aktív tartomány környezetében.</span><span class="sxs-lookup"><span data-stu-id="cb845-241">You must upgrade the protection agent in a nonactive domain environment.</span></span> <span data-ttu-id="cb845-242">Az ügyfélszámítógép csatlakozik a hálózathoz, amíg a Data Protection Manager rendszergazdai konzol jeleníti meg, hogy a védelmi ügynök frissítése függőben.</span><span class="sxs-lookup"><span data-stu-id="cb845-242">Until the client computer is connected to the network, the Data Protection Manager Administrator Console shows that the protection agent update is pending.</span></span>

<span data-ttu-id="cb845-243">Az alábbi szakaszok azt ismertetik, hogyan frissíteni a védelmi ügynökök csatlakozó ügyfélszámítógépek és a nem csatlakozó ügyfélszámítógépekről.</span><span class="sxs-lookup"><span data-stu-id="cb845-243">The following sections describe how to update protection agents for client computers that are connected and client computers that are not connected.</span></span>

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a><span data-ttu-id="cb845-244">Frissítse a védelmi ügynököt a csatlakoztatott ügyfélszámítógépen</span><span class="sxs-lookup"><span data-stu-id="cb845-244">Update a protection agent for a connected client computer</span></span>

1. <span data-ttu-id="cb845-245">Válassza ki a biztonsági mentés Server felügyeleti konzol **felügyeleti** > **ügynökök**.</span><span class="sxs-lookup"><span data-stu-id="cb845-245">In the Backup Server Administrator Console, select **Management** > **Agents**.</span></span>

2. <span data-ttu-id="cb845-246">A megjelenítési ablaktáblán jelölje ki azon ügyfélszámítógépeket, amelyekhez a védelmi ügynök frissítése.</span><span class="sxs-lookup"><span data-stu-id="cb845-246">In the display pane, select the client computers for which you want to update the protection agent.</span></span>

  > [!NOTE]
  > <span data-ttu-id="cb845-247">A **Ügynökfrissítéssel** oszlop azt jelzi, ha a védelmi ügynök frissítése a védett számítógépekhez.</span><span class="sxs-lookup"><span data-stu-id="cb845-247">The **Agent Updates** column indicates when a protection agent update is available for each protected computer.</span></span> <span data-ttu-id="cb845-248">Az a **műveletek** ablaktáblán, a **frissítés** művelet áll rendelkezésre, csak ha egy védett számítógép van kiválasztva, és frissítések érhetők el.</span><span class="sxs-lookup"><span data-stu-id="cb845-248">In the **Actions** pane, the **Update** action is available only when a protected computer is selected and updates are available.</span></span>
  >
  >

3. <span data-ttu-id="cb845-249">A kijelölt számítógépen, a frissített védelmi ügynök telepítése a **műveletek** ablaktáblán válassza előbb **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="cb845-249">To install updated protection agents on the selected computers, in the **Actions** pane, select **Update**.</span></span>

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a><span data-ttu-id="cb845-250">Egy ügyfélszámítógépen, amely nem csatlakozik a védelmi ügynök frissítése</span><span class="sxs-lookup"><span data-stu-id="cb845-250">Update a protection agent on a client computer that is not connected</span></span>

1. <span data-ttu-id="cb845-251">Válassza ki a biztonsági mentés Server felügyeleti konzol **felügyeleti** > **ügynökök**.</span><span class="sxs-lookup"><span data-stu-id="cb845-251">In the Backup Server Administrator Console, select **Management** > **Agents**.</span></span>

2. <span data-ttu-id="cb845-252">A megjelenítési ablaktáblán jelölje ki azon ügyfélszámítógépeket, amelyekhez a védelmi ügynök frissítése.</span><span class="sxs-lookup"><span data-stu-id="cb845-252">In the display pane, select the client computers for which you want to update the protection agent.</span></span>

  > [!NOTE]
   > <span data-ttu-id="cb845-253">A **Ügynökfrissítéssel** oszlop azt jelzi, ha a védelmi ügynök frissítése a védett számítógépekhez.</span><span class="sxs-lookup"><span data-stu-id="cb845-253">The **Agent Updates** column indicates when a protection agent update is available for each protected computer.</span></span> <span data-ttu-id="cb845-254">Az a **műveletek** ablaktáblán, a **frissítés** művelet nem érhető el, ha egy védett számítógép van kiválasztva, kivéve, ha frissítések érhetők el.</span><span class="sxs-lookup"><span data-stu-id="cb845-254">In the **Actions** pane, the **Update** action is not available when a protected computer is selected unless updates are available.</span></span>
  >
  >

3. <span data-ttu-id="cb845-255">A frissített védelmi ügynök telepítéséhez a kijelölt számítógépen, válassza ki a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="cb845-255">To install updated protection agents on the selected computers, select **Update**.</span></span>

4. <span data-ttu-id="cb845-256">Az ügyfélszámítógép, amely nem csatlakozik a hálózathoz, amíg a számítógép kapcsolódik a hálózathoz a **ügynök állapota** az oszlopban látható a állapotának **frissítés függőben**.</span><span class="sxs-lookup"><span data-stu-id="cb845-256">For a client computer that is not connected to the network, until the computer is connected to the network, the **Agent Status** column shows a status of **Update Pending**.</span></span>

  <span data-ttu-id="cb845-257">Miután egy ügyfélszámítógép csatlakozik a hálózathoz, a **Ügynökfrissítéssel** az ügyfélszámítógép az oszlopban látható a állapotának **Frissítéskísérleti**.</span><span class="sxs-lookup"><span data-stu-id="cb845-257">After a client computer is connected to the network, the **Agent Updates** column for the client computer shows a status of **Updating**.</span></span>
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-the-new-version-with-azure"></a><span data-ttu-id="cb845-258">Örökölt védelmi csoportok áthelyezése régi verziója, és szinkronizálja az új verziót az Azure-ral</span><span class="sxs-lookup"><span data-stu-id="cb845-258">Move legacy Protection groups from old version and sync the new version with Azure</span></span>

<span data-ttu-id="cb845-259">Amennyiben az Azure Backup Server és az operációs rendszer is frissítve lett, készen áll a Modern biztonsági mentési tárolót használó új adatforrások védelméhez.</span><span class="sxs-lookup"><span data-stu-id="cb845-259">Once Azure Backup Server and the OS are both updated, you are ready to protect new data sources using Modern Backup Storage.</span></span> <span data-ttu-id="cb845-260">Azonban már védett adatforrások továbbra is az Azure Backup Server, de minden új védelmi voltak a hagyományos módon kell védeni fogja használni a Modern Backup-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="cb845-260">However already protected data sources will continue to be protected in the legacy way as they were in Azure Backup Server but all new protection will use Modern Backup Storage.</span></span>

<span data-ttu-id="cb845-261">Következő lépések telepítsen át adatforrásokat védelmi örökölt üzemmódú Modern biztonságimásolat-tároláshoz.</span><span class="sxs-lookup"><span data-stu-id="cb845-261">Below steps are to migrate data sources from legacy  mode of protection to Modern backup storage.</span></span>

<span data-ttu-id="cb845-262">• Adja hozzá az új kötetek a DPM-tárolókészletben, és rendelje hozzá a rövid nevek és adatok forrás, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="cb845-262">• Add the new volume(s) to the DPM storage pool and assign friendly names and data source tags if desired.</span></span>
<span data-ttu-id="cb845-263">• Minden adatforrás, amely a hagyományos módban van, az adatforrások és a "Védett adatok megőrzése" állítsa le a védelmet.</span><span class="sxs-lookup"><span data-stu-id="cb845-263">• For each data source that is in legacy mode, stop protection of the data sources and “Retain Protected Data”.</span></span>  <span data-ttu-id="cb845-264">Ez lehetővé teszi a helyreállítás a régi helyreállítási pontok áttelepítés után.</span><span class="sxs-lookup"><span data-stu-id="cb845-264">This will allow recovery of old recovery points after migration.</span></span>

<span data-ttu-id="cb845-265">• Hozzon létre egy új PG, és válassza ki az adatforrásokat, amelyek a új formátum használatával kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="cb845-265">• Create a new PG and select the data sources that are to be stored using new format.</span></span>
<span data-ttu-id="cb845-266">• A DPM mindent replika másolatot a korábbi biztonsági mentési tárolóból be helyileg a Modern biztonsági másolatokat tároló kötetet.</span><span class="sxs-lookup"><span data-stu-id="cb845-266">• DPM will do a replica copy from the legacy backup storage into the Modern Backup Storage volume locally.</span></span>
<span data-ttu-id="cb845-267">Megjegyzés: Ez fogja látni, a helyreállítás után a művelet feladat • minden új szinkronizálási és helyreállítási pontok majd tárolódnak a Modern Backup-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="cb845-267">Note: This will be seen as a post-recovery operation job • All new sync and recovery points will then be stored in Modern Backup Storage.</span></span>
<span data-ttu-id="cb845-268">• A régi helyreállítási pontok lesz kiürítve kimenő lejár, és végül a lemezterület felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="cb845-268">• Old recovery points will be pruned out as they expire and eventually free up the disk space.</span></span>
<span data-ttu-id="cb845-269">• A hagyományos köteteket a régi tárterületet, a lemez a törölt távolíthatók el az Azure biztonsági mentésre és a rendszer.</span><span class="sxs-lookup"><span data-stu-id="cb845-269">• Once all the legacy volumes are deleted from the old storage, the disk can be removed from Azure backup and the system.</span></span>
<span data-ttu-id="cb845-270">• A Azure a dpmdb biztonsági mentés készítése.</span><span class="sxs-lookup"><span data-stu-id="cb845-270">• Take a backup of the  Azure DPMDB.</span></span>

<span data-ttu-id="cb845-271">2. lépés:-fontos elemek > az új kiszolgáló neve lehet azonos az eredeti Azure Backup-kiszolgálóként kell.</span><span class="sxs-lookup"><span data-stu-id="cb845-271">Part 2: -Important items> The new server will need to be named same as the original Azure Backup server.</span></span> <span data-ttu-id="cb845-272">Az új Azure biztonsági mentési kiszolgáló neve nem módosítható, ha szeretné megőrizni a régi tárolókészlet és a DPMDB használni kívánt helyreállítási pontok - kell rendelkeznie a dpmdb biztonsági másolat, el kell állítani</span><span class="sxs-lookup"><span data-stu-id="cb845-272">You cannot change the name of the new Azure backup server if you want to use old storage pool and DPMDB to retain recovery points -Must have backup of DPMDB  as it will need to be restored</span></span>

1) <span data-ttu-id="cb845-273">Az eredeti Azure leállítása biztonsági mentése a kiszolgáló, vagy ki a vezeték-EK.</span><span class="sxs-lookup"><span data-stu-id="cb845-273">Shutdown the original Azure backup server or take it off the wire.</span></span>
2) <span data-ttu-id="cb845-274">Alaphelyzetbe állítja a számítógépfiókot az active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="cb845-274">Reset the machine account in active directory.</span></span>
3) <span data-ttu-id="cb845-275">Server 2016 telepítse az új számítógépen, és nevezze el a számítógép neve megegyezik az eredeti kiszolgáló Azure Backup szolgáltatásnál.</span><span class="sxs-lookup"><span data-stu-id="cb845-275">Install Server 2016 on new machine and name it the same machine name as the original Azure Backup server.</span></span>
4) <span data-ttu-id="cb845-276">A tartományhoz</span><span class="sxs-lookup"><span data-stu-id="cb845-276">Join the Domain</span></span>
5) <span data-ttu-id="cb845-277">Telepítse az Azure Backup server V2 (helyezze át a DPM Tárolókészletének lemezeit a régi kiszolgáló és az importálás)</span><span class="sxs-lookup"><span data-stu-id="cb845-277">Install Azure Backup server V2 (Move DPM Storage pool disks from old server and import)</span></span>
6) <span data-ttu-id="cb845-278">A 2. rész oldalától végrehajtott DPMDB visszaállítása</span><span class="sxs-lookup"><span data-stu-id="cb845-278">Restore the DPMDB taken from end of part 2</span></span>
7) <span data-ttu-id="cb845-279">A tároló csatlakoztatása az eredeti biztonsági mentési kiszolgálóról az új kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="cb845-279">Attach the storage from the original backup server to the new server.</span></span>
8) <span data-ttu-id="cb845-280">A DPMDB SQL visszaállításból</span><span class="sxs-lookup"><span data-stu-id="cb845-280">From SQL Restore the DPMDB</span></span>
9) <span data-ttu-id="cb845-281">A Microsoft Azure Backup szolgáltatás új server CD-n rendszergazdai parancssorból telepítése helyét és a bin mappa</span><span class="sxs-lookup"><span data-stu-id="cb845-281">From admin command line on new server cd to Microsoft Azure Backup install location and bin folder</span></span>

<span data-ttu-id="cb845-282">Példa az útvonalra: C:\windows\system32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\\</span><span class="sxs-lookup"><span data-stu-id="cb845-282">Path example: C:\windows\system32>cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\\</span></span>
<span data-ttu-id="cb845-283">az Azure biztonsági mentés futtassa a DPMSYNC-SYNC</span><span class="sxs-lookup"><span data-stu-id="cb845-283">to Azure backup Run DPMSYNC -SYNC</span></span>

10) <span data-ttu-id="cb845-284">Futtassa a DPMSYNC-SYNC Megjegyzés felvett új lemezt a DPM tárolókészletéhez helyett a régiek áthelyezése futtassa a DPMSYNC - Reallocatereplica</span><span class="sxs-lookup"><span data-stu-id="cb845-284">Run DPMSYNC -SYNC Note If you have added NEW disks to the DPM Storage pool instead of moving the old ones, then run DPMSYNC -Reallocatereplica</span></span>

## <a name="new-powershell-cmdlets-in-v2"></a><span data-ttu-id="cb845-285">A v2 új PowerShell-parancsmagok</span><span class="sxs-lookup"><span data-stu-id="cb845-285">New PowerShell cmdlets in v2</span></span>

<span data-ttu-id="cb845-286">Ha telepíti az Azure Backup Server v2, a két új parancsmagok érhetők el:</span><span class="sxs-lookup"><span data-stu-id="cb845-286">When you install Azure Backup Server v2, two new cmdlets are available:</span></span> 
* [<span data-ttu-id="cb845-287">Csatlakoztatási-DPMRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="cb845-287">Mount-DPMRecoveryPoint</span></span>](https://technet.microsoft.com/library/mt787159.aspx)
* [<span data-ttu-id="cb845-288">Dismount-DPMRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="cb845-288">Dismount-DPMRecoveryPoint</span></span>](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a><span data-ttu-id="cb845-289">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cb845-289">Next steps</span></span>

<span data-ttu-id="cb845-290">Megtudhatja, hogyan készíti elő a kiszolgálót, vagy a munkaterhelések védelmének megkezdéséhez:</span><span class="sxs-lookup"><span data-stu-id="cb845-290">Learn how to prepare your server or begin protecting a workload:</span></span>
- [<span data-ttu-id="cb845-291">A kiszolgálói biztonsági mentési feladatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="cb845-291">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="cb845-292">Készítsen biztonsági másolatot a VMware server Backup Server használatával</span><span class="sxs-lookup"><span data-stu-id="cb845-292">Use Backup Server to back up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="cb845-293">Készítsen biztonsági másolatot az SQL Server biztonsági másolat-kiszolgáló használatával</span><span class="sxs-lookup"><span data-stu-id="cb845-293">Use Backup Server to back up SQL Server</span></span>](backup-azure-sql-mabs.md)
- [<span data-ttu-id="cb845-294">Modern biztonsági mentési tárhelyet használ a helykiszolgáló biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="cb845-294">Use Modern Backup Storage with Backup Server</span></span>](backup-mabs-add-storage.md)

