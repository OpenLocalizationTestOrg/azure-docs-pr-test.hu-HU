---
title: "az Azure Backup Server 2 Modern Backup-tárhelyre aaaUse |} Microsoft Docs"
description: "További tudnivalók az Azure Backup Server v2 hello új funkciói. Ez a cikk ismerteti, hogyan tooupgrade a biztonsági mentés Server telepítését."
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
ms.openlocfilehash: b2a1ed27a6a682bd611fea1d2df9ef93314404e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-storage-tooazure-backup-server-v2"></a><span data-ttu-id="c971a-104">Adja hozzá a tárolási tooAzure Backup Server v2</span><span class="sxs-lookup"><span data-stu-id="c971a-104">Add storage tooAzure Backup Server v2</span></span>

<span data-ttu-id="c971a-105">Az Azure Backup Server v2 tartalmaz a System Center 2016 adatok Protection Manager Modern Backup-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="c971a-105">Azure Backup Server v2 comes with System Center 2016 Data Protection Manager Modern Backup Storage.</span></span> <span data-ttu-id="c971a-106">Modern Backup-tárhelyre 50 %-, a biztonsági mentések, amelyek háromszor gyorsabb és hatékonyabb tárolási a tárhely-megtakarítást nyújt.</span><span class="sxs-lookup"><span data-stu-id="c971a-106">Modern Backup Storage offers storage savings of 50 percent, backups that are three times faster, and more efficient storage.</span></span> <span data-ttu-id="c971a-107">Munkaterhelés-kompatibilis tárterületet is biztosít.</span><span class="sxs-lookup"><span data-stu-id="c971a-107">It also offers workload-aware storage.</span></span> 

> [!NOTE]
> <span data-ttu-id="c971a-108">toouse Modern Backup-tárhelyre, futtatnia kell biztonsági mentést kiszolgáló v2 Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="c971a-108">toouse Modern Backup Storage, you must run Backup Server v2 on Windows Server 2016.</span></span> <span data-ttu-id="c971a-109">Ha a Windows Server korábbi verzióján futtatja a biztonsági mentés kiszolgáló v2, Azure Backup Server tudják kihasználni a Modern Backup-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="c971a-109">If you run Backup Server v2 on an earlier version of Windows Server, Azure Backup Server can't take advantage of Modern Backup Storage.</span></span> <span data-ttu-id="c971a-110">Ehelyett azt védi a munkaterheléseket mint a biztonsági mentés kiszolgáló v1.</span><span class="sxs-lookup"><span data-stu-id="c971a-110">Instead, it protects workloads as it does with Backup Server v1.</span></span> <span data-ttu-id="c971a-111">További információkért lásd: hello biztonsági mentés kiszolgálóverzió [védelmi mátrix](backup-mabs-protection-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="c971a-111">For more information, see hello Backup Server version [protection matrix](backup-mabs-protection-matrix.md).</span></span>

## <a name="volumes-in-backup-server-v2"></a><span data-ttu-id="c971a-112">A helykiszolgáló biztonsági mentése v2 kötetek</span><span class="sxs-lookup"><span data-stu-id="c971a-112">Volumes in Backup Server v2</span></span>

<span data-ttu-id="c971a-113">Tartalék kiszolgáló v2 tárolókötetek fogad el.</span><span class="sxs-lookup"><span data-stu-id="c971a-113">Backup Server v2 accepts storage volumes.</span></span> <span data-ttu-id="c971a-114">Ha hozzáad egy kötetet, biztonsági mentés Server formázza hello kötet tooResilient File System (ReFS), amelyek Modern Backup-tárhelyre van szükség.</span><span class="sxs-lookup"><span data-stu-id="c971a-114">When you add a volume, Backup Server formats hello volume tooResilient File System (ReFS), which Modern Backup Storage requires.</span></span> <span data-ttu-id="c971a-115">egy köteten, tooadd és tooexpand később Ha kell, javasoljuk, hogy a munkafolyamat használja:</span><span class="sxs-lookup"><span data-stu-id="c971a-115">tooadd a volume, and tooexpand it later if you need to, we suggest that you use this workflow:</span></span>

1.  <span data-ttu-id="c971a-116">A virtuális gép biztonsági mentése kiszolgáló v2 beállítása.</span><span class="sxs-lookup"><span data-stu-id="c971a-116">Set up Backup Server v2 on a VM.</span></span>
2.  <span data-ttu-id="c971a-117">Kötet létrehozása a virtuális lemez a tárolókészlethez:</span><span class="sxs-lookup"><span data-stu-id="c971a-117">Create a volume on a virtual disk in a storage pool:</span></span>
    1.  <span data-ttu-id="c971a-118">Vegye fel a lemez tooa tárolókészletet, és hozzon létre egy virtuális lemezt egyszerű elrendezés.</span><span class="sxs-lookup"><span data-stu-id="c971a-118">Add a disk tooa storage pool and create a virtual disk with simple layout.</span></span>
    2.  <span data-ttu-id="c971a-119">Vegyen fel minden további lemezeket, és hello virtuális lemez.</span><span class="sxs-lookup"><span data-stu-id="c971a-119">Add any additional disks, and extend hello virtual disk.</span></span>
    3.  <span data-ttu-id="c971a-120">Hozzon létre köteteket hello virtuális lemezen.</span><span class="sxs-lookup"><span data-stu-id="c971a-120">Create volumes on hello virtual disk.</span></span>
3.  <span data-ttu-id="c971a-121">Adja hozzá a hello kötetek tooBackup kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="c971a-121">Add hello volumes tooBackup Server.</span></span>
4.  <span data-ttu-id="c971a-122">Munkaterhelés-t támogató tároló konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="c971a-122">Configure workload-aware storage.</span></span>

## <a name="create-a-volume-for-modern-backup-storage"></a><span data-ttu-id="c971a-123">Kötet létrehozása a Modern Backup-tárhelyre</span><span class="sxs-lookup"><span data-stu-id="c971a-123">Create a volume for Modern Backup Storage</span></span>

<span data-ttu-id="c971a-124">Biztonsági mentés kiszolgáló v2 használatával kötetekkel, mint a lemezes tárolás segíthetnek ellenőrzése alatt tartja a tároló karbantartása.</span><span class="sxs-lookup"><span data-stu-id="c971a-124">Using Backup Server v2 with volumes as disk storage can help you maintain control over storage.</span></span> <span data-ttu-id="c971a-125">A kötet lehet egyetlen lemezre.</span><span class="sxs-lookup"><span data-stu-id="c971a-125">A volume can be a single disk.</span></span> <span data-ttu-id="c971a-126">Ha hello tooextend tárolás jövőbeli, azonban a tárolóhelyek használatával létrehozott lemezterülete kötet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c971a-126">However, if you want tooextend storage in hello future, create a volume out of a disk created by using storage spaces.</span></span> <span data-ttu-id="c971a-127">Ez segít, ha azt szeretné, tooexpand hello kötet biztonságimásolat-tároláshoz.</span><span class="sxs-lookup"><span data-stu-id="c971a-127">This can help if you want tooexpand hello volume for backup storage.</span></span> <span data-ttu-id="c971a-128">Ez a rész nyújt gyakorlati tanácsok a kötet létrehozása ezzel a beállítással.</span><span class="sxs-lookup"><span data-stu-id="c971a-128">This section offers best practices for creating a volume with this setup.</span></span>

1. <span data-ttu-id="c971a-129">A Kiszolgálókezelő ablakában válassza **fájl- és tárolási szolgáltatások** > **kötetek** > **Tárolókészletek**.</span><span class="sxs-lookup"><span data-stu-id="c971a-129">In Server Manager, select **File and Storage Services** > **Volumes** > **Storage Pools**.</span></span> <span data-ttu-id="c971a-130">A **fizikai lemezek**, jelölje be **új tárolókészlet**.</span><span class="sxs-lookup"><span data-stu-id="c971a-130">Under **PHYSICAL DISKS**, select **New Storage Pool**.</span></span> 

    ![Új tárolókészlet létrehozása](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. <span data-ttu-id="c971a-132">A hello **feladatok** legördülő mezőben válassza **új virtuális lemez**.</span><span class="sxs-lookup"><span data-stu-id="c971a-132">In hello **TASKS** drop-down box, select **New Virtual Disk**.</span></span>

    ![Virtuális lemez hozzáadása](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. <span data-ttu-id="c971a-134">Hello tárolókészlet, majd válassza ki és **fizikai lemez hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="c971a-134">Select hello storage pool, and then select **Add Physical Disk**.</span></span>

    ![Fizikai lemez hozzáadása](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. <span data-ttu-id="c971a-136">Hello fizikai lemezt, majd válassza ki és **virtuális lemez bővítése**.</span><span class="sxs-lookup"><span data-stu-id="c971a-136">Select hello physical disk, and then select **Extend Virtual Disk**.</span></span>

    ![Hello virtuális lemez kiterjesztése](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. <span data-ttu-id="c971a-138">Hello virtuális lemezt, majd válassza ki és **új kötet**.</span><span class="sxs-lookup"><span data-stu-id="c971a-138">Select hello virtual disk, and then select **New Volume**.</span></span>

    ![Hozzon létre egy új kötetet](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. <span data-ttu-id="c971a-140">A hello **hello kiszolgáló és lemez kiválasztása** párbeszédpanelen, a select hello kiszolgáló és a hello új lemez.</span><span class="sxs-lookup"><span data-stu-id="c971a-140">In hello **Select hello server and disk** dialog, select hello server and hello new disk.</span></span> <span data-ttu-id="c971a-141">Ezt követően válassza **következő**.</span><span class="sxs-lookup"><span data-stu-id="c971a-141">Then, select **Next**.</span></span>

    ![Válassza ki a hello kiszolgáló és lemez](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-toobackup-server-disk-storage"></a><span data-ttu-id="c971a-143">Adja hozzá a kötetek tooBackup Server lemezes tárolás</span><span class="sxs-lookup"><span data-stu-id="c971a-143">Add volumes tooBackup Server disk storage</span></span>

<span data-ttu-id="c971a-144">egy kötet tooBackup Server, a hello tooadd **felügyeleti** ablaktáblán ismételt vizsgálat hello tárolási, és válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="c971a-144">tooadd a volume tooBackup Server, in hello **Management** pane, rescan hello storage, and then select **Add**.</span></span> <span data-ttu-id="c971a-145">Megjelenik az összes hello kötetek elérhető toobe Server Backup-tárhelyre hozzá listája.</span><span class="sxs-lookup"><span data-stu-id="c971a-145">A list of all hello volumes available toobe added for Backup Server Storage appears.</span></span> <span data-ttu-id="c971a-146">Rendelkezésre álló köteteken ad hozzá a kijelölt kötetek toohello listáját, miután adhat a azokat egy rövid nevet toohelp mobileszközökként kezeli azokat.</span><span class="sxs-lookup"><span data-stu-id="c971a-146">After available volumes are added toohello list of selected volumes, you can give them a friendly name toohelp you manage them.</span></span> <span data-ttu-id="c971a-147">Ezek a kötetek tooReFS, biztonsági mentés Server használhassa hello előnyei Modern Backup-tárhelyre, válassza ki tooformat **OK**.</span><span class="sxs-lookup"><span data-stu-id="c971a-147">tooformat these volumes tooReFS so Backup Server can use hello benefits of Modern Backup Storage, select **OK**.</span></span>

![Adja hozzá a rendelkezésre álló köteteken](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a><span data-ttu-id="c971a-149">Munkaterhelés-kompatibilis tárolás beállítása</span><span class="sxs-lookup"><span data-stu-id="c971a-149">Set up workload-aware storage</span></span>

<span data-ttu-id="c971a-150">Munkaterhelés-kompatibilis tárolóval kiválaszthatja a lehetőleg tároló bizonyos típusú munkaterhelések hello kötetekről.</span><span class="sxs-lookup"><span data-stu-id="c971a-150">With workload-aware storage, you can select hello volumes that preferentially store certain kinds of workloads.</span></span> <span data-ttu-id="c971a-151">Például beállíthat költséges kötetek, amely támogatja a bemeneti/kimeneti műveletek második (IOPS) toostore csak hello igénylő munkaterhelések gyakori, nagy mennyiségű biztonsági másolatok száma túl magas száma.</span><span class="sxs-lookup"><span data-stu-id="c971a-151">For example, you can set expensive volumes that support a high number of input/output operations per second (IOPS) toostore only hello workloads that require frequent, high-volume backups.</span></span> <span data-ttu-id="c971a-152">Példa: SQL Server, a tranzakciós naplók.</span><span class="sxs-lookup"><span data-stu-id="c971a-152">An example is SQL Server with transaction logs.</span></span> <span data-ttu-id="c971a-153">Egyéb munkaterhelések, amelyekről a ritkábban, például a virtuális gépek, toolow költségű kötetek készíthető.</span><span class="sxs-lookup"><span data-stu-id="c971a-153">Other workloads that are backed up less frequently, like VMs, can be backed up toolow-cost volumes.</span></span>

### <a name="update-dpmdiskstorage"></a><span data-ttu-id="c971a-154">Frissítés-DPMDiskStorage</span><span class="sxs-lookup"><span data-stu-id="c971a-154">Update-DPMDiskStorage</span></span>

<span data-ttu-id="c971a-155">Hello PowerShell-parancsmaggal frissíti hello tárolókészletben a Data Protection Manager-kiszolgálón található kötet hello tulajdonságainak frissítése – DPMDiskStorage munkaterhelés-t támogató tárolási állíthat be.</span><span class="sxs-lookup"><span data-stu-id="c971a-155">You can set up workload-aware storage by using hello PowerShell cmdlet Update-DPMDiskStorage, which updates hello properties of a volume in hello storage pool on a Data Protection Manager server.</span></span>

<span data-ttu-id="c971a-156">Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="c971a-156">Syntax:</span></span>

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
<span data-ttu-id="c971a-157">hello alábbi képernyőfelvételen látható hello frissítés-DPMDiskStorage parancsmag hello PowerShell-ablakban.</span><span class="sxs-lookup"><span data-stu-id="c971a-157">hello following screenshot shows hello Update-DPMDiskStorage cmdlet in hello PowerShell window.</span></span>

![hello frissítés-DPMDiskStorage parancs hello PowerShell-ablakban](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

<span data-ttu-id="c971a-159">hello PowerShell használatával is tükröződnek hello Backup Server felügyeleti konzol.</span><span class="sxs-lookup"><span data-stu-id="c971a-159">hello changes you make by using PowerShell are reflected in hello Backup Server Administrator Console.</span></span>

![A lemezek és kötetek a hello felügyeleti konzol](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a><span data-ttu-id="c971a-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c971a-161">Next steps</span></span>
<span data-ttu-id="c971a-162">Biztonsági kiszolgáló telepítése után megtudhatja, hogyan tooprepare kiszolgálóját, vagy indítsa el a védelmet a munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="c971a-162">After you install Backup Server, learn how tooprepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="c971a-163">A kiszolgálói biztonsági mentési feladatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="c971a-163">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="c971a-164">Használja a VMware Server biztonsági másolat Server tooback</span><span class="sxs-lookup"><span data-stu-id="c971a-164">Use Backup Server tooback up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="c971a-165">SQL Server biztonsági másolat Server tooback használata</span><span class="sxs-lookup"><span data-stu-id="c971a-165">Use Backup Server tooback up SQL Server</span></span>](backup-azure-sql-mabs.md)

