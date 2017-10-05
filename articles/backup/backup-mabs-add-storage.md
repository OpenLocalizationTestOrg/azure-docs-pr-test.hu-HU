---
title: "Modern biztonsági mentési tárhelyet használ az Azure Backup Server v2 |} Microsoft Docs"
description: "További tudnivalók az Azure Backup Server v2 új funkciókkal. A cikkből megtudhatja, hogyan lehet frissíteni a biztonsági mentés Server telepítését."
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
ms.openlocfilehash: 751b9b495fd368dff1f72429707f5f33a0ccb569
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-storage-to-azure-backup-server-v2"></a><span data-ttu-id="35a35-104">Azure Backup Server v2 tároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="35a35-104">Add storage to Azure Backup Server v2</span></span>

<span data-ttu-id="35a35-105">Az Azure Backup Server v2 tartalmaz a System Center 2016 adatok Protection Manager Modern Backup-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="35a35-105">Azure Backup Server v2 comes with System Center 2016 Data Protection Manager Modern Backup Storage.</span></span> <span data-ttu-id="35a35-106">Modern Backup-tárhelyre 50 %-, a biztonsági mentések, amelyek háromszor gyorsabb és hatékonyabb tárolási a tárhely-megtakarítást nyújt.</span><span class="sxs-lookup"><span data-stu-id="35a35-106">Modern Backup Storage offers storage savings of 50 percent, backups that are three times faster, and more efficient storage.</span></span> <span data-ttu-id="35a35-107">Munkaterhelés-kompatibilis tárterületet is biztosít.</span><span class="sxs-lookup"><span data-stu-id="35a35-107">It also offers workload-aware storage.</span></span> 

> [!NOTE]
> <span data-ttu-id="35a35-108">Modern biztonsági mentési tárolót használni, a Windows Server 2016 Backup Server v2 kell futtatnia.</span><span class="sxs-lookup"><span data-stu-id="35a35-108">To use Modern Backup Storage, you must run Backup Server v2 on Windows Server 2016.</span></span> <span data-ttu-id="35a35-109">Ha a Windows Server korábbi verzióján futtatja a biztonsági mentés kiszolgáló v2, Azure Backup Server tudják kihasználni a Modern Backup-tárhelyre.</span><span class="sxs-lookup"><span data-stu-id="35a35-109">If you run Backup Server v2 on an earlier version of Windows Server, Azure Backup Server can't take advantage of Modern Backup Storage.</span></span> <span data-ttu-id="35a35-110">Ehelyett azt védi a munkaterheléseket mint a biztonsági mentés kiszolgáló v1.</span><span class="sxs-lookup"><span data-stu-id="35a35-110">Instead, it protects workloads as it does with Backup Server v1.</span></span> <span data-ttu-id="35a35-111">További információkért lásd: a biztonsági mentés kiszolgálóverzió [védelmi mátrix](backup-mabs-protection-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="35a35-111">For more information, see the Backup Server version [protection matrix](backup-mabs-protection-matrix.md).</span></span>

## <a name="volumes-in-backup-server-v2"></a><span data-ttu-id="35a35-112">A helykiszolgáló biztonsági mentése v2 kötetek</span><span class="sxs-lookup"><span data-stu-id="35a35-112">Volumes in Backup Server v2</span></span>

<span data-ttu-id="35a35-113">Tartalék kiszolgáló v2 tárolókötetek fogad el.</span><span class="sxs-lookup"><span data-stu-id="35a35-113">Backup Server v2 accepts storage volumes.</span></span> <span data-ttu-id="35a35-114">Ha hozzáad egy köteten, a biztonsági mentés Server formázza a kötetet a refs fájlrendszer (ReFS), amelyek Modern Backup-tárhelyre van szükség.</span><span class="sxs-lookup"><span data-stu-id="35a35-114">When you add a volume, Backup Server formats the volume to Resilient File System (ReFS), which Modern Backup Storage requires.</span></span> <span data-ttu-id="35a35-115">Kötet hozzáadása, és később kibontásához, ha szeretné, javasoljuk, hogy a munkafolyamat használja:</span><span class="sxs-lookup"><span data-stu-id="35a35-115">To add a volume, and to expand it later if you need to, we suggest that you use this workflow:</span></span>

1.  <span data-ttu-id="35a35-116">A virtuális gép biztonsági mentése kiszolgáló v2 beállítása.</span><span class="sxs-lookup"><span data-stu-id="35a35-116">Set up Backup Server v2 on a VM.</span></span>
2.  <span data-ttu-id="35a35-117">Kötet létrehozása a virtuális lemez a tárolókészlethez:</span><span class="sxs-lookup"><span data-stu-id="35a35-117">Create a volume on a virtual disk in a storage pool:</span></span>
    1.  <span data-ttu-id="35a35-118">Adjon hozzá egy lemezt a tárolókészletbe, és hozzon létre egy virtuális lemezt egyszerű elrendezés.</span><span class="sxs-lookup"><span data-stu-id="35a35-118">Add a disk to a storage pool and create a virtual disk with simple layout.</span></span>
    2.  <span data-ttu-id="35a35-119">Vegyen fel minden további lemezeket, és a virtuális lemez.</span><span class="sxs-lookup"><span data-stu-id="35a35-119">Add any additional disks, and extend the virtual disk.</span></span>
    3.  <span data-ttu-id="35a35-120">A virtuális lemezen található köteteket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="35a35-120">Create volumes on the virtual disk.</span></span>
3.  <span data-ttu-id="35a35-121">A kötetek hozzá a biztonsági mentés kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="35a35-121">Add the volumes to Backup Server.</span></span>
4.  <span data-ttu-id="35a35-122">Munkaterhelés-t támogató tároló konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="35a35-122">Configure workload-aware storage.</span></span>

## <a name="create-a-volume-for-modern-backup-storage"></a><span data-ttu-id="35a35-123">Kötet létrehozása a Modern Backup-tárhelyre</span><span class="sxs-lookup"><span data-stu-id="35a35-123">Create a volume for Modern Backup Storage</span></span>

<span data-ttu-id="35a35-124">Biztonsági mentés kiszolgáló v2 használatával kötetekkel, mint a lemezes tárolás segíthetnek ellenőrzése alatt tartja a tároló karbantartása.</span><span class="sxs-lookup"><span data-stu-id="35a35-124">Using Backup Server v2 with volumes as disk storage can help you maintain control over storage.</span></span> <span data-ttu-id="35a35-125">A kötet lehet egyetlen lemezre.</span><span class="sxs-lookup"><span data-stu-id="35a35-125">A volume can be a single disk.</span></span> <span data-ttu-id="35a35-126">Ha azt szeretné, a későbbiekben kibővítheti a tárolási, azonban a tárolóhelyek használatával létrehozott lemezterülete kötet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="35a35-126">However, if you want to extend storage in the future, create a volume out of a disk created by using storage spaces.</span></span> <span data-ttu-id="35a35-127">Ez segít, ha azt szeretné, bontsa ki a kötetet a biztonsági másolatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="35a35-127">This can help if you want to expand the volume for backup storage.</span></span> <span data-ttu-id="35a35-128">Ez a rész nyújt gyakorlati tanácsok a kötet létrehozása ezzel a beállítással.</span><span class="sxs-lookup"><span data-stu-id="35a35-128">This section offers best practices for creating a volume with this setup.</span></span>

1. <span data-ttu-id="35a35-129">A Kiszolgálókezelő ablakában válassza **fájl- és tárolási szolgáltatások** > **kötetek** > **Tárolókészletek**.</span><span class="sxs-lookup"><span data-stu-id="35a35-129">In Server Manager, select **File and Storage Services** > **Volumes** > **Storage Pools**.</span></span> <span data-ttu-id="35a35-130">A **fizikai lemezek**, jelölje be **új tárolókészlet**.</span><span class="sxs-lookup"><span data-stu-id="35a35-130">Under **PHYSICAL DISKS**, select **New Storage Pool**.</span></span> 

    ![Új tárolókészlet létrehozása](./media/backup-mabs-add-storage/mabs-add-storage-1.png)

2. <span data-ttu-id="35a35-132">Az a **feladatok** legördülő mezőben válassza **új virtuális lemez**.</span><span class="sxs-lookup"><span data-stu-id="35a35-132">In the **TASKS** drop-down box, select **New Virtual Disk**.</span></span>

    ![Virtuális lemez hozzáadása](./media/backup-mabs-add-storage/mabs-add-storage-2.png)

3. <span data-ttu-id="35a35-134">A tárolókészlet, majd válassza ki és **fizikai lemez hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="35a35-134">Select the storage pool, and then select **Add Physical Disk**.</span></span>

    ![Fizikai lemez hozzáadása](./media/backup-mabs-add-storage/mabs-add-storage-3.png)

4. <span data-ttu-id="35a35-136">A fizikai lemezt, majd válassza ki és **virtuális lemez bővítése**.</span><span class="sxs-lookup"><span data-stu-id="35a35-136">Select the physical disk, and then select **Extend Virtual Disk**.</span></span>

    ![A virtuális lemez](./media/backup-mabs-add-storage/mabs-add-storage-4.png)

5. <span data-ttu-id="35a35-138">A virtuális lemezre, majd válassza ki és **új kötet**.</span><span class="sxs-lookup"><span data-stu-id="35a35-138">Select the virtual disk, and then select **New Volume**.</span></span>

    ![Hozzon létre egy új kötetet](./media/backup-mabs-add-storage/mabs-add-storage-5.png)

6. <span data-ttu-id="35a35-140">Az a **válassza ki a kiszolgálót és a lemez** párbeszédpanelen válassza ki a kiszolgálót és az új lemez.</span><span class="sxs-lookup"><span data-stu-id="35a35-140">In the **Select the server and disk** dialog, select the server and the new disk.</span></span> <span data-ttu-id="35a35-141">Ezt követően válassza **következő**.</span><span class="sxs-lookup"><span data-stu-id="35a35-141">Then, select **Next**.</span></span>

    ![Válassza ki a kiszolgáló és lemez](./media/backup-mabs-add-storage/mabs-add-storage-6.png)

## <a name="add-volumes-to-backup-server-disk-storage"></a><span data-ttu-id="35a35-143">A biztonsági mentés lemezre tárhelyéhez kötetek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="35a35-143">Add volumes to Backup Server disk storage</span></span>

<span data-ttu-id="35a35-144">Kiszolgáló biztonsági mentése, a kötet hozzáadása a **felügyeleti** ablaktáblában ellenőrizze újra a tárolót, és válassza **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="35a35-144">To add a volume to Backup Server, in the **Management** pane, rescan the storage, and then select **Add**.</span></span> <span data-ttu-id="35a35-145">Az összes biztonsági mentés tárolására adható hozzá köteteket jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="35a35-145">A list of all the volumes available to be added for Backup Server Storage appears.</span></span> <span data-ttu-id="35a35-146">Után rendelkezésre álló köteteken hozzáadódnak a kijelölt kötetek listáját, azokat egy rövid nevet adhat, hogyan kezelheti azokat.</span><span class="sxs-lookup"><span data-stu-id="35a35-146">After available volumes are added to the list of selected volumes, you can give them a friendly name to help you manage them.</span></span> <span data-ttu-id="35a35-147">Ezeket a köteteket a refs fájlrendszer fájlformátumba biztonsági mentés kiszolgáló használatával a Modern Backup-tárhelyre előnyeit, jelölje be **OK**.</span><span class="sxs-lookup"><span data-stu-id="35a35-147">To format these volumes to ReFS so Backup Server can use the benefits of Modern Backup Storage, select **OK**.</span></span>

![Adja hozzá a rendelkezésre álló köteteken](./media/backup-mabs-add-storage/mabs-add-storage-7.png)

## <a name="set-up-workload-aware-storage"></a><span data-ttu-id="35a35-149">Munkaterhelés-kompatibilis tárolás beállítása</span><span class="sxs-lookup"><span data-stu-id="35a35-149">Set up workload-aware storage</span></span>

<span data-ttu-id="35a35-150">Munkaterhelés-kompatibilis tárolóval kiválaszthatja a bizonyos típusú munkaterhelések lehetőleg tárolására szolgáló köteteknél.</span><span class="sxs-lookup"><span data-stu-id="35a35-150">With workload-aware storage, you can select the volumes that preferentially store certain kinds of workloads.</span></span> <span data-ttu-id="35a35-151">Például beállíthat költséges kötetek, amelyek támogatják a bemeneti/kimeneti műveletek száma másodpercenként (IOPS) túl magas száma csak a szolgáltatások, amelyek rendszeres, nagy mennyiségű biztonsági másolatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="35a35-151">For example, you can set expensive volumes that support a high number of input/output operations per second (IOPS) to store only the workloads that require frequent, high-volume backups.</span></span> <span data-ttu-id="35a35-152">Példa: SQL Server, a tranzakciós naplók.</span><span class="sxs-lookup"><span data-stu-id="35a35-152">An example is SQL Server with transaction logs.</span></span> <span data-ttu-id="35a35-153">Egyéb munkaterhelések, amelyekről a ritkábban, például a virtuális gépek, alacsony költségű kötetek készíthető.</span><span class="sxs-lookup"><span data-stu-id="35a35-153">Other workloads that are backed up less frequently, like VMs, can be backed up to low-cost volumes.</span></span>

### <a name="update-dpmdiskstorage"></a><span data-ttu-id="35a35-154">Frissítés-DPMDiskStorage</span><span class="sxs-lookup"><span data-stu-id="35a35-154">Update-DPMDiskStorage</span></span>

<span data-ttu-id="35a35-155">Munkaterhelés-t támogató tárolási állíthat be egy kötetet a tárolókészletből a Data Protection Manager-kiszolgáló tulajdonságait frissítve frissítés-DPMDiskStorage PowerShell-parancsmag használatával.</span><span class="sxs-lookup"><span data-stu-id="35a35-155">You can set up workload-aware storage by using the PowerShell cmdlet Update-DPMDiskStorage, which updates the properties of a volume in the storage pool on a Data Protection Manager server.</span></span>

<span data-ttu-id="35a35-156">Szintaxis:</span><span class="sxs-lookup"><span data-stu-id="35a35-156">Syntax:</span></span>

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```
<span data-ttu-id="35a35-157">Az alábbi képernyőfelvételen látható a frissítés-DPMDiskStorage parancsmag a PowerShell-ablakban.</span><span class="sxs-lookup"><span data-stu-id="35a35-157">The following screenshot shows the Update-DPMDiskStorage cmdlet in the PowerShell window.</span></span>

![A frissítés-DPMDiskStorage parancsot a PowerShell-ablakban](./media/backup-mabs-add-storage/mabs-add-storage-8.png)

<span data-ttu-id="35a35-159">A PowerShell használatával a módosítások megjelennek a biztonsági mentés Server felügyeleti konzol.</span><span class="sxs-lookup"><span data-stu-id="35a35-159">The changes you make by using PowerShell are reflected in the Backup Server Administrator Console.</span></span>

![A lemezek és kötetek a felügyeleti konzol](./media/backup-mabs-add-storage/mabs-add-storage-9.png)

## <a name="next-steps"></a><span data-ttu-id="35a35-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="35a35-161">Next steps</span></span>
<span data-ttu-id="35a35-162">Biztonsági kiszolgáló telepítése után megtudhatja, hogyan készíti elő a kiszolgálót, vagy indítsa el a védelmet a munkaterhelés.</span><span class="sxs-lookup"><span data-stu-id="35a35-162">After you install Backup Server, learn how to prepare your server, or begin protecting a workload.</span></span>

- [<span data-ttu-id="35a35-163">A kiszolgálói biztonsági mentési feladatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="35a35-163">Prepare Backup Server workloads</span></span>](backup-azure-microsoft-azure-backup.md)
- [<span data-ttu-id="35a35-164">Készítsen biztonsági másolatot a VMware server Backup Server használatával</span><span class="sxs-lookup"><span data-stu-id="35a35-164">Use Backup Server to back up a VMware server</span></span>](backup-azure-backup-server-vmware.md)
- [<span data-ttu-id="35a35-165">Készítsen biztonsági másolatot az SQL Server biztonsági másolat-kiszolgáló használatával</span><span class="sxs-lookup"><span data-stu-id="35a35-165">Use Backup Server to back up SQL Server</span></span>](backup-azure-sql-mabs.md)

