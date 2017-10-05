---
title: "Erőforrás-kezelő telepített virtuális gépek biztonsági mentéseinek kezelése |} Microsoft Docs"
description: "Ismerje meg, hogyan kezelheti és figyelheti az erőforrás-kezelő telepített virtuális gépek biztonsági mentéseinek"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: f3050283-d60f-472d-b464-cb844e70d67e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: trinadhk;markgal
ms.openlocfilehash: 35a21cb99ca4bad124a9f764cef9da453e1fe47f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-virtual-machine-backups"></a><span data-ttu-id="2a36e-103">Azure-beli virtuális gépek biztonsági másolatainak kezelése</span><span class="sxs-lookup"><span data-stu-id="2a36e-103">Manage Azure virtual machine backups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2a36e-104">Azure virtuális gép biztonsági mentések kezelése</span><span class="sxs-lookup"><span data-stu-id="2a36e-104">Manage Azure VM backups</span></span>](backup-azure-manage-vms.md)
> * [<span data-ttu-id="2a36e-105">Klasszikus virtuális gép biztonsági mentések kezelése</span><span class="sxs-lookup"><span data-stu-id="2a36e-105">Manage Classic VM backups</span></span>](backup-azure-manage-vms-classic.md)
>
>

<span data-ttu-id="2a36e-106">Ez a cikk nyújt útmutatást a virtuális gép biztonsági másolatok kezelésére, és a biztonsági mentési riasztás adatait a portál irányítópultján érhető el.</span><span class="sxs-lookup"><span data-stu-id="2a36e-106">This article provides guidance on managing VM backups, and explains the backup alerts information available in the portal dashboard.</span></span> <span data-ttu-id="2a36e-107">Az útmutató a virtuális gépek használata a Recovery Services-tárolók vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="2a36e-107">The guidance in this article applies to using VMs with Recovery Services vaults.</span></span> <span data-ttu-id="2a36e-108">Ez a cikk nem foglalkozik a virtuális gépek létrehozását, és nem az azt ismertetik, hogyan védi a virtuális gépek nem.</span><span class="sxs-lookup"><span data-stu-id="2a36e-108">This article does not cover the creation of virtual machines, nor does it explain how to protect virtual machines.</span></span> <span data-ttu-id="2a36e-109">A Recovery Services-tároló virtuális gépek Azure Resource Manager üzembe az Azure-ban ellátásáról ismertetése, lásd: [először: készítsen biztonsági mentést a Recovery Services-tároló virtuális gépek](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="2a36e-109">For a primer on protecting Azure Resource Manager-deployed VMs in Azure with a Recovery Services vault, see [First look: Back up VMs to a Recovery Services vault](backup-azure-vms-first-look-arm.md).</span></span>

## <a name="manage-vaults-and-protected-virtual-machines"></a><span data-ttu-id="2a36e-110">Tárolók és a védett virtuális gépek kezelése</span><span class="sxs-lookup"><span data-stu-id="2a36e-110">Manage vaults and protected virtual machines</span></span>
<span data-ttu-id="2a36e-111">Az Azure portálon a Recovery Services-tároló irányítópult információit, a tároló, beleértve a hozzáférést biztosítja:</span><span class="sxs-lookup"><span data-stu-id="2a36e-111">In the Azure portal, the Recovery Services vault dashboard provides access to information about the vault including:</span></span>

* <span data-ttu-id="2a36e-112">a legutóbbi biztonsági mentési pillanatképet, amely egyben a legutóbbi helyreállítási pontot < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-112">the most recent backup snapshot, which is also the latest restore point <br\></span></span>
* <span data-ttu-id="2a36e-113">a biztonsági mentési házirend < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-113">the backup policy <br\></span></span>
* <span data-ttu-id="2a36e-114">teljes méret minden biztonsági mentési pillanatképek < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-114">total size of all backup snapshots <br\></span></span>
* <span data-ttu-id="2a36e-115">a tárolóval védett virtuális gépek száma < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-115">number of virtual machines that are protected with the vault <br\></span></span>

<span data-ttu-id="2a36e-116">Egy virtuális gép biztonsági másolatából számos felügyeleti feladatot a kezdő ellenőrzéséhez nyissa meg a tároló az irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="2a36e-116">Many management tasks with a virtual machine backup begin with opening the vault in the dashboard.</span></span> <span data-ttu-id="2a36e-117">Azonban az tárolók használható arra, hogy több elem (vagy több virtuális gép), egy adott virtuális gép adatainak megtekintése, mert az a tároló elem irányítópult megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2a36e-117">However, because vaults can be used to protect multiple items (or multiple VMs), to view details about a particular VM, open the vault item dashboard.</span></span> <span data-ttu-id="2a36e-118">Az alábbi eljárás bemutatja, hogyan nyissa meg a *tároló irányítópult* és folytassa a *tároló elem irányítópult*.</span><span class="sxs-lookup"><span data-stu-id="2a36e-118">The following procedure shows you how to open the *vault dashboard* and then continue to the *vault item dashboard*.</span></span> <span data-ttu-id="2a36e-119">Nincsenek "tippek" mindkét eljárásnál, amelyek arra, hogyan adja hozzá a tárolóhoz, és a tároló cikk az Azure-irányítópulton a PIN-kód irányítópult parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="2a36e-119">There are "tips" in both procedures that point out how to add the vault and vault item to the Azure dashboard by using the Pin to dashboard command.</span></span> <span data-ttu-id="2a36e-120">Rögzítés az irányítópulton a tárolóhoz vagy a cikk parancsikon létrehozásának módja.</span><span class="sxs-lookup"><span data-stu-id="2a36e-120">Pin to dashboard is a way of creating a shortcut to the vault or item.</span></span> <span data-ttu-id="2a36e-121">Általános jellegű parancsok a parancsikonnal is végrehajthat.</span><span class="sxs-lookup"><span data-stu-id="2a36e-121">You can also execute common commands from the shortcut.</span></span>

> [!TIP]
> <span data-ttu-id="2a36e-122">Ha több irányítópultok és panel nyílik meg, a csúszkával sötét-kék az ablak alján csúsztassa be a Azure irányítópult oda-vissza.</span><span class="sxs-lookup"><span data-stu-id="2a36e-122">If you have multiple dashboards and blades open, use the dark-blue slider at the bottom of the window to slide the Azure dashboard back and forth.</span></span>
>
>

![A csúszka teljes áttekintése](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-the-dashboard"></a><span data-ttu-id="2a36e-124">Az irányítópult nyissa meg a Recovery Services-tároló:</span><span class="sxs-lookup"><span data-stu-id="2a36e-124">Open a Recovery Services vault in the dashboard:</span></span>
1. <span data-ttu-id="2a36e-125">Jelentkezzen be az [Azure Portalra](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2a36e-125">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="2a36e-126">A központi menüben kattintson a **Tallózás** elemre, majd az erőforrások listájába írja be a következőt: **Recovery Services**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-126">On the Hub menu, click **Browse** and in the list of resources, type **Recovery Services**.</span></span> <span data-ttu-id="2a36e-127">Ahogy elkezd gépelni, a lista a beírtak alapján szűri a lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="2a36e-127">As you begin typing, the list filters based on your input.</span></span> <span data-ttu-id="2a36e-128">Kattintson a **Recovery Services-tároló** elemre.</span><span class="sxs-lookup"><span data-stu-id="2a36e-128">Click **Recovery Services vault**.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    <span data-ttu-id="2a36e-130">A Recovery Services-tárolók listája megjelenik.</span><span class="sxs-lookup"><span data-stu-id="2a36e-130">The list of Recovery Services vaults are displayed.</span></span>

    ![<span data-ttu-id="2a36e-131">Helyreállítási szolgáltatások listája tárolók</span><span class="sxs-lookup"><span data-stu-id="2a36e-131">List of Recovery Services vaults</span></span> ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

   > [!TIP]
   > <span data-ttu-id="2a36e-132">Ha rögzíti egy tárolót az Azure-irányítópultot, a tároló érhető el közvetlenül az Azure-portál megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="2a36e-132">If you pin a vault to the Azure Dashboard, that vault is immediately accessible when you open the Azure portal.</span></span> <span data-ttu-id="2a36e-133">PIN-kód egy tárolót az irányítópulton, a tároló listában, kattintson a jobb gombbal a tárolóra, és válassza ki **rögzítés az irányítópulton**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-133">To pin a vault to the dashboard, in the vault list, right-click the vault, and select **Pin to dashboard**.</span></span>
   >
   >
3. <span data-ttu-id="2a36e-134">A tárolók listában jelölje ki a kívánt tárolóra az irányítópult megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2a36e-134">From the list of vaults, select the vault to open its dashboard.</span></span> <span data-ttu-id="2a36e-135">Ha bejelöli a tárolóban, a tároló irányítópult és a **beállítások** panel megnyitása.</span><span class="sxs-lookup"><span data-stu-id="2a36e-135">When you select the vault, the vault dashboard and the **Settings** blade open.</span></span> <span data-ttu-id="2a36e-136">Az alábbi ábrán a **Contoso-tároló** irányítópult ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="2a36e-136">In the following image, the **Contoso-vault** dashboard is highlighted.</span></span>

    ![Nyissa meg a tároló irányítópult és a beállítások panel](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a><span data-ttu-id="2a36e-138">Nyissa meg a tároló elem irányítópult</span><span class="sxs-lookup"><span data-stu-id="2a36e-138">Open a vault item dashboard</span></span>
<span data-ttu-id="2a36e-139">Az előző eljárásban megnyitott a tároló irányítópult.</span><span class="sxs-lookup"><span data-stu-id="2a36e-139">In the previous procedure you opened the vault dashboard.</span></span> <span data-ttu-id="2a36e-140">A tároló elem irányítópult megnyitása:</span><span class="sxs-lookup"><span data-stu-id="2a36e-140">To open the vault item dashboard:</span></span>

1. <span data-ttu-id="2a36e-141">A tároló irányítópultjának a a **biztonsági mentés elemek** csempére, kattintson a **Azure virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-141">In the vault dashboard, on the **Backup Items** tile, click **Azure Virtual Machines**.</span></span>

    ![Nyissa meg a biztonsági másolati elemei csempe](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    <span data-ttu-id="2a36e-143">A **biztonsági másolati elemei** panel megjeleníti az utolsó biztonsági mentési feladat minden elemhez.</span><span class="sxs-lookup"><span data-stu-id="2a36e-143">The **Backup Items** blade lists the last backup job for each item.</span></span> <span data-ttu-id="2a36e-144">Ebben a példában a rendszer egy védett virtuális gépek, demovm-markgal, amelyet ehhez a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="2a36e-144">In this example, there is one virtual machine, demovm-markgal, protected by this vault.</span></span>  

    ![Biztonsági másolati elemei csempe](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > <span data-ttu-id="2a36e-146">Könnyű elérés, a PIN-kód egy tároló elemet, az Azure-irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="2a36e-146">For ease of access, you can pin a vault item to the Azure Dashboard.</span></span> <span data-ttu-id="2a36e-147">PIN-kód egy tároló elem a tároló elem listában, kattintson a jobb gombbal az elemet, és válassza **rögzítés az irányítópulton**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-147">To pin a vault item, in the vault item list, right-click the item and select **Pin to dashboard**.</span></span>
   >
   >
2. <span data-ttu-id="2a36e-148">Az a **biztonsági mentés elemek** panelen a elemre kattintva nyissa meg a tároló elem irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="2a36e-148">In the **Backup Items** blade, click the item to open the vault item dashboard.</span></span>

    ![Biztonsági másolati elemei csempe](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    <span data-ttu-id="2a36e-150">A tároló elem irányítópult és a **beállítások** panel megnyitása.</span><span class="sxs-lookup"><span data-stu-id="2a36e-150">The vault item dashboard and its **Settings** blade open.</span></span>

    ![Biztonsági másolati elemei irányítópultot, amelynek beállítások panel](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    <span data-ttu-id="2a36e-152">A tároló elem irányítópultról például több kulcskezelési feladatok, érhető el:</span><span class="sxs-lookup"><span data-stu-id="2a36e-152">From the vault item dashboard, you can accomplish many key management tasks, such as:</span></span>

   * <span data-ttu-id="2a36e-153">Módosítsa a házirendek, vagy hozzon létre egy új biztonsági mentési házirend < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-153">change policies or create a new backup policy<br\></span></span>
   * <span data-ttu-id="2a36e-154">megtekintheti a visszaállítási pontok, és a konzisztencia állapota < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-154">view restore points, and see their consistency state <br\></span></span>
   * <span data-ttu-id="2a36e-155">igény szerinti biztonsági mentést a virtuális gépek < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-155">on-demand backup of a virtual machine <br\></span></span>
   * <span data-ttu-id="2a36e-156">Állítsa le a virtuális gépek védelme < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-156">stop protecting virtual machines <br\></span></span>
   * <span data-ttu-id="2a36e-157">a virtuális gépek a védelem folytatásához < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-157">resume protection of a virtual machine <br\></span></span>
   * <span data-ttu-id="2a36e-158">a biztonsági mentési adatok (vagy a helyreállítási pont) törlése < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-158">delete a backup data (or recovery point) <br\></span></span>
   * <span data-ttu-id="2a36e-159">[Állítsa vissza a biztonsági mentési lemezek](backup-azure-arm-restore-vms.md#restore-backed-up-disks) < br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-159">[restore backup disks](backup-azure-arm-restore-vms.md#restore-backed-up-disks)  <br\></span></span>

<span data-ttu-id="2a36e-160">A következő eljárásokat a kiindulási pontja egy tároló elem irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="2a36e-160">For the following procedures, the starting point is the vault item dashboard.</span></span>

## <a name="manage-backup-policies"></a><span data-ttu-id="2a36e-161">Biztonsági mentési irányelvek kezelése</span><span class="sxs-lookup"><span data-stu-id="2a36e-161">Manage backup policies</span></span>
1. <span data-ttu-id="2a36e-162">Az a [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **összes beállítás** megnyitásához a **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="2a36e-162">On the [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **All Settings** to open the **Settings** blade.</span></span>

    ![A biztonsági mentési házirend panel](./media/backup-azure-manage-vms/all-settings-button.png)
2. <span data-ttu-id="2a36e-164">Az a **beállítások** panelen kattintson a **biztonsági mentési házirend** , hogy a panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2a36e-164">On the **Settings** blade, click **Backup policy** to open that blade.</span></span>

    <span data-ttu-id="2a36e-165">A panelen a biztonsági mentési gyakoriság és megőrzési tartomány részletei láthatók.</span><span class="sxs-lookup"><span data-stu-id="2a36e-165">On the blade, the backup frequency and retention range details are shown.</span></span>

    ![A biztonsági mentési házirend panel](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. <span data-ttu-id="2a36e-167">Az a **válassza ki a biztonsági mentési házirend** menüben:</span><span class="sxs-lookup"><span data-stu-id="2a36e-167">From the **Choose backup policy** menu:</span></span>

   * <span data-ttu-id="2a36e-168">Házirendek módosításához jelöljön ki egy másik házirendet, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-168">To change policies, select a different policy and click **Save**.</span></span> <span data-ttu-id="2a36e-169">Ekkor a rendszer automatikusan alkalmazza az új házirendet a tárolón.</span><span class="sxs-lookup"><span data-stu-id="2a36e-169">The new policy is immediately applied to the vault.</span></span> <span data-ttu-id="2a36e-170">< br\></span><span class="sxs-lookup"><span data-stu-id="2a36e-170"><br\></span></span>
   * <span data-ttu-id="2a36e-171">Egy házirend létrehozásához válassza **hozzon létre új**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-171">To create a policy, select **Create New**.</span></span>

     ![Virtuális gép biztonsági mentése](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     <span data-ttu-id="2a36e-173">A biztonsági mentési házirend létrehozásával kapcsolatos utasításokért lásd: [biztonsági mentési házirend meghatározása](backup-azure-manage-vms.md#defining-a-backup-policy).</span><span class="sxs-lookup"><span data-stu-id="2a36e-173">For instructions on creating a backup policy, see [Defining a backup policy](backup-azure-manage-vms.md#defining-a-backup-policy).</span></span>

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> <span data-ttu-id="2a36e-174">Biztonsági házirendek kezelése során ügyeljen arra, hogy kövesse a [ajánlott eljárások](backup-azure-vms-introduction.md#best-practices) optimális teljesítményének eléréséhez biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="2a36e-174">While managing backup policies, make sure to follow the [best practices](backup-azure-vms-introduction.md#best-practices) for optimal backup performance</span></span>
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a><span data-ttu-id="2a36e-175">Igény szerinti biztonsági mentést a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="2a36e-175">On-demand backup of a virtual machine</span></span>
<span data-ttu-id="2a36e-176">Igény szerinti biztonsági mentését egy virtuális gép eltarthat, ha konfigurálva van a védelem.</span><span class="sxs-lookup"><span data-stu-id="2a36e-176">You can take an on-demand backup of a virtual machine once it is configured for protection.</span></span> <span data-ttu-id="2a36e-177">Ha a kezdeti biztonsági mentés folyamatban, igény szerinti biztonsági mentést a virtuális gép teljes másolatot készít a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="2a36e-177">If the initial backup is pending, on-demand backup creates a full copy of the virtual machine in the Recovery Services vault.</span></span> <span data-ttu-id="2a36e-178">Ha a kezdeti biztonsági mentés befejeződött, egy igény szerinti biztonsági mentést fogja csak elküldi a módosításokat a korábbi pillanatképből, a Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="2a36e-178">If the initial backup is completed, an on-demand backup will only send changes from the previous snapshot, to the Recovery Services vault.</span></span> <span data-ttu-id="2a36e-179">Ez azt jelenti, hogy azt követő biztonsági mentéseket a rendszer mindig növekményes.</span><span class="sxs-lookup"><span data-stu-id="2a36e-179">That is, subsequent backups are always incremental.</span></span>

> [!NOTE]
> <span data-ttu-id="2a36e-180">Egy igény szerinti biztonsági mentés megőrzési tartománya a napi biztonsági mentési pont a házirendben megadott megőrzési értéknek.</span><span class="sxs-lookup"><span data-stu-id="2a36e-180">The retention range for an on-demand backup is the retention value specified for the Daily backup point in the policy.</span></span> <span data-ttu-id="2a36e-181">Ha nincs napi biztonsági mentési pont van kijelölve, a heti biztonsági mentési pont szerepel.</span><span class="sxs-lookup"><span data-stu-id="2a36e-181">If no Daily backup point is selected, then the weekly backup point is used.</span></span>
>
>

<span data-ttu-id="2a36e-182">Egy igény szerinti biztonsági mentést a virtuális gép indításához:</span><span class="sxs-lookup"><span data-stu-id="2a36e-182">To trigger an on-demand backup of a virtual machine:</span></span>

* <span data-ttu-id="2a36e-183">Az a [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **biztonsági mentés most**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-183">On the [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Backup now**.</span></span>

    ![Biztonsági mentés most gombra.](./media/backup-azure-manage-vms/backup-now-button.png)

    <span data-ttu-id="2a36e-185">A portál gondoskodik arról, hogy szeretné-e az igény szerinti biztonsági mentési feladat elindítása.</span><span class="sxs-lookup"><span data-stu-id="2a36e-185">The portal makes sure that you want to start an on-demand backup job.</span></span> <span data-ttu-id="2a36e-186">Kattintson a **Igen** elindítja a biztonsági mentési feladatot.</span><span class="sxs-lookup"><span data-stu-id="2a36e-186">Click **Yes** to start the backup job.</span></span>

    ![Biztonsági mentés most gombra.](./media/backup-azure-manage-vms/backup-now-check.png)

    <span data-ttu-id="2a36e-188">A biztonsági mentési feladat létrehoz egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="2a36e-188">The backup job creates a recovery point.</span></span> <span data-ttu-id="2a36e-189">A helyreállítási pont megőrzési időtartamának megegyezik a virtuális géphez társított házirendben megadott megőrzési ideig.</span><span class="sxs-lookup"><span data-stu-id="2a36e-189">The retention range of the recovery point is the same as retention range specified in the policy associated with the virtual machine.</span></span> <span data-ttu-id="2a36e-190">A folyamat előrehaladását a feladathoz, a tároló irányítópulton kattintson a **biztonsági mentési feladatok** csempére.</span><span class="sxs-lookup"><span data-stu-id="2a36e-190">To track the progress for the job, in the vault dashboard, click the **Backup Jobs** tile.</span></span>  

## <a name="stop-protecting-virtual-machines"></a><span data-ttu-id="2a36e-191">Virtuális gépek védelmének megszüntetése</span><span class="sxs-lookup"><span data-stu-id="2a36e-191">Stop protecting virtual machines</span></span>
<span data-ttu-id="2a36e-192">Ha kikapcsolja a virtuális gép védelmét, megkérdezi, hogy kívánja-e a helyreállítási pontok megőrzése.</span><span class="sxs-lookup"><span data-stu-id="2a36e-192">If you choose to stop protecting a virtual machine, you are asked if you want to retain the recovery points.</span></span> <span data-ttu-id="2a36e-193">Virtuális gépek védelmének kikapcsolását két módja van:</span><span class="sxs-lookup"><span data-stu-id="2a36e-193">There are two ways to stop protecting virtual machines:</span></span>

* <span data-ttu-id="2a36e-194">Állítsa le az összes jövőbeli biztonsági mentési feladat, és törölje az összes helyreállítási pont, vagy</span><span class="sxs-lookup"><span data-stu-id="2a36e-194">stop all future backup jobs and delete all recovery points, or</span></span>
* <span data-ttu-id="2a36e-195">Állítsa le az összes jövőbeli biztonsági mentési feladat, de hagyja meg a helyreállítási pontok</span><span class="sxs-lookup"><span data-stu-id="2a36e-195">stop all future backup jobs but leave the recovery points</span></span> <br/>

<span data-ttu-id="2a36e-196">Nincs társított áthelyezni a helyreállítási pontok tárolási költség.</span><span class="sxs-lookup"><span data-stu-id="2a36e-196">There is a cost associated with leaving the recovery points in storage.</span></span> <span data-ttu-id="2a36e-197">Azonban az, hogy a helyreállítási pontok előnye, később visszaállíthatja a virtuális gép igény.</span><span class="sxs-lookup"><span data-stu-id="2a36e-197">However, the benefit of leaving the recovery points is you can restore the virtual machine later, if desired.</span></span> <span data-ttu-id="2a36e-198">A költség, hogy a helyreállítási pontok információ: a [díjszabása](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="2a36e-198">For information about the cost of leaving the recovery points, see the  [pricing details](https://azure.microsoft.com/pricing/details/backup/).</span></span> <span data-ttu-id="2a36e-199">Ha törli az összes helyreállítási pontot választja, a virtuális gép nem állítható vissza.</span><span class="sxs-lookup"><span data-stu-id="2a36e-199">If you choose to delete all recovery points, you cannot restore the virtual machine.</span></span>

<span data-ttu-id="2a36e-200">Szüntesse meg a virtuális gép védelmét:</span><span class="sxs-lookup"><span data-stu-id="2a36e-200">To stop protection for a virtual machine:</span></span>

1. <span data-ttu-id="2a36e-201">Az a [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **Stop biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-201">On the [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Stop backup**.</span></span>

    ![Állítsa le a biztonsági mentési gomb](./media/backup-azure-manage-vms/stop-backup-button.png)

    <span data-ttu-id="2a36e-203">A biztonsági mentés leállítása panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a36e-203">The Stop Backup blade opens.</span></span>

    ![Állítsa le a biztonsági mentési panel](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. <span data-ttu-id="2a36e-205">A a **biztonsági mentés leállítása** panelen válassza ki, hogy megtartja vagy a biztonsági mentési adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="2a36e-205">On the **Stop Backup** blade, choose whether to retain or delete the backup data.</span></span> <span data-ttu-id="2a36e-206">Az információ írja be a választott ismerteti.</span><span class="sxs-lookup"><span data-stu-id="2a36e-206">The information box provides details about your choice.</span></span>

    ![Állítsa le a védelmet](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. <span data-ttu-id="2a36e-208">Ha úgy döntött, hogy a biztonsági mentési adatok megőrzése mellett, ugorjon a 4.</span><span class="sxs-lookup"><span data-stu-id="2a36e-208">If you chose to retain the backup data, skip to step 4.</span></span> <span data-ttu-id="2a36e-209">Ha úgy döntött, hogy törölje a biztonsági mentési adatokat, győződjön meg arról, hogy szeretné-e állítani a biztonsági mentési feladatokat, és törölje a helyreállítási pontok – írja be az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="2a36e-209">If you chose to delete backup data, confirm that you want to stop the backup jobs and delete the recovery points - type the name of the item.</span></span>

    ![Állítsa le a ellenőrzése](./media/backup-azure-manage-vms/item-verification-box.png)

    <span data-ttu-id="2a36e-211">Ha nem biztos a cikk-neve, mutasson a felkiáltójel a nevének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2a36e-211">If you aren't sure of the item name, hover over the exclamation mark to view the name.</span></span> <span data-ttu-id="2a36e-212">Az elem nevét is a **állítsa le a biztonsági mentés** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="2a36e-212">Also, the name of the item is under **Stop Backup** at the top of the blade.</span></span>
4. <span data-ttu-id="2a36e-213">Megadhat egy **OK** vagy **Megjegyzés**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-213">Optionally provide a **Reason** or **Comment**.</span></span>
5. <span data-ttu-id="2a36e-214">A biztonsági mentési feladat az aktuális elem leállításához kattintson ![leállítás biztonsági mentés gombra.](./media/backup-azure-manage-vms/stop-backup-button-blue.png)</span><span class="sxs-lookup"><span data-stu-id="2a36e-214">To stop the backup job for the current item, click  ![Stop backup button](./media/backup-azure-manage-vms/stop-backup-button-blue.png)</span></span>

    <span data-ttu-id="2a36e-215">Egy értesítési üzenetet lehetővé teszi a biztonsági mentési feladatok leállították.</span><span class="sxs-lookup"><span data-stu-id="2a36e-215">A notification message lets you know the backup jobs have been stopped.</span></span>

    ![Állítsa le a védelmet megerősítése](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a><span data-ttu-id="2a36e-217">A virtuális gépek a védelem folytatásához</span><span class="sxs-lookup"><span data-stu-id="2a36e-217">Resume protection of a virtual machine</span></span>
<span data-ttu-id="2a36e-218">Ha a **biztonsági mentési adatok megőrzése** beállítás választása a virtuális gép védelmét le lett állítva, ha lehetséges, a védelem folytatásához.</span><span class="sxs-lookup"><span data-stu-id="2a36e-218">If the **Retain Backup Data** option was chosen when protection for the virtual machine was stopped, then it is possible to resume protection.</span></span> <span data-ttu-id="2a36e-219">Ha a **biztonságimásolat-adatok törlése** lehetőséget választotta, akkor a virtuális gép védelmét nem folytatható.</span><span class="sxs-lookup"><span data-stu-id="2a36e-219">If the **Delete Backup Data** option was chosen, then protection for the virtual machine cannot resume.</span></span>

<span data-ttu-id="2a36e-220">A virtuális gép védelmének folytatása</span><span class="sxs-lookup"><span data-stu-id="2a36e-220">To resume protection for the virtual machine</span></span>

1. <span data-ttu-id="2a36e-221">Az a [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **biztonsági mentés folytatása**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-221">On the [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Resume backup**.</span></span>

    ![A védelem folytatásához](./media/backup-azure-manage-vms/resume-backup-button.png)

    <span data-ttu-id="2a36e-223">A biztonsági mentési házirend panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a36e-223">The Backup Policy blade opens.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2a36e-224">Ha a virtuális gép ismételt védelmével, dönthet úgy, hogy egy másik házirend érvényesíthető, mint a házirendet, amellyel a virtuális gép védett kezdetben ki.</span><span class="sxs-lookup"><span data-stu-id="2a36e-224">When re-protecting the virtual machine, you can choose a different policy than the policy with which virtual machine was protected initially.</span></span>
   >
   >
2. <span data-ttu-id="2a36e-225">Kövesse a [biztonsági mentési házirendek kezelése](backup-azure-manage-vms.md#manage-backup-policies) a virtuális gép házirend hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="2a36e-225">Follow the steps in [Manage backup policies](backup-azure-manage-vms.md#manage-backup-policies) to assign the policy for the virtual machine.</span></span>

    <span data-ttu-id="2a36e-226">Miután a virtuális gép alkalmazza a biztonsági mentési házirend, a következő üzenet jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2a36e-226">Once the backup policy is applied to the virtual machine, you see the following message.</span></span>

    ![Sikeresen védett virtuális gép](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a><span data-ttu-id="2a36e-228">Biztonságimásolat-adatok törlése</span><span class="sxs-lookup"><span data-stu-id="2a36e-228">Delete Backup data</span></span>
<span data-ttu-id="2a36e-229">A során a virtuális géphez tartozó biztonsági mentési adatok törlése a **Stop biztonsági mentés** feladat, vagy a biztonsági mentés azt követően bármikor feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="2a36e-229">You can delete the backup data associated with a virtual machine during the **Stop backup** job, or anytime after the backup job has completed.</span></span> <span data-ttu-id="2a36e-230">Akkor lehet hasznos nap vagy hét, a helyreállítási pontok törlése előtt várja meg.</span><span class="sxs-lookup"><span data-stu-id="2a36e-230">It may even be beneficial to wait days or weeks before deleting the recovery points.</span></span> <span data-ttu-id="2a36e-231">Ellentétben a helyreállítási pontok visszaállítása a biztonsági mentési adatok törlésekor, adott helyreállítási pont törlése nem választható.</span><span class="sxs-lookup"><span data-stu-id="2a36e-231">Unlike restoring recovery points, when deleting backup data, you cannot choose specific recovery points to delete.</span></span> <span data-ttu-id="2a36e-232">Ha törli a biztonsági mentési adatokat, az elemhez tartozó összes helyreállítási pont törlése.</span><span class="sxs-lookup"><span data-stu-id="2a36e-232">If you choose to delete your backup data, you delete all recovery points associated with the item.</span></span>

<span data-ttu-id="2a36e-233">A következő eljárás azt feltételezi, hogy a biztonsági mentési feladatot a virtuális gép leállítása vagy letiltása.</span><span class="sxs-lookup"><span data-stu-id="2a36e-233">The following procedure assumes the Backup job for the virtual machine has been stopped or disabled.</span></span> <span data-ttu-id="2a36e-234">A biztonsági mentési feladat le van tiltva, ha a **biztonsági mentés folytatása** és **Delete biztonsági mentés** beállítások érhetők el a tároló elem irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="2a36e-234">Once the Backup job is disabled, the **Resume backup** and **Delete backup** options are available in the vault item dashboard.</span></span>

![Folytatás és a Törlés gombbal](./media/backup-azure-manage-vms/resume-delete-buttons.png)

<span data-ttu-id="2a36e-236">A virtuális gépen, a biztonsági mentési adatok törléséhez a *biztonsági mentése le van tiltva*:</span><span class="sxs-lookup"><span data-stu-id="2a36e-236">To delete backup data on a virtual machine with the *Backup disabled*:</span></span>

1. <span data-ttu-id="2a36e-237">Az a [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **Delete biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-237">On the [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Delete backup**.</span></span>

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    <span data-ttu-id="2a36e-239">A **biztonságimásolat-adatok törlése** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a36e-239">The **Delete Backup Data** blade opens.</span></span>

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. <span data-ttu-id="2a36e-241">Írja be annak ellenőrzéséhez, hogy törli a helyreállítási pontok az elem nevét.</span><span class="sxs-lookup"><span data-stu-id="2a36e-241">Type the name of the item to confirm you want to delete the recovery points.</span></span>

    ![Állítsa le a ellenőrzése](./media/backup-azure-manage-vms/item-verification-box.png)

    <span data-ttu-id="2a36e-243">Ha nem biztos a cikk-neve, mutasson a felkiáltójel a nevének megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="2a36e-243">If you aren't sure of the item name, hover over the exclamation mark to view the name.</span></span> <span data-ttu-id="2a36e-244">Az elem nevét is a **biztonságimásolat-adatok törlése** a panel tetején.</span><span class="sxs-lookup"><span data-stu-id="2a36e-244">Also, the name of the item is under **Delete Backup Data** at the top of the blade.</span></span>
3. <span data-ttu-id="2a36e-245">Megadhat egy **OK** vagy **Megjegyzés**.</span><span class="sxs-lookup"><span data-stu-id="2a36e-245">Optionally provide a **Reason** or **Comment**.</span></span>
4. <span data-ttu-id="2a36e-246">A biztonsági mentési adatok az aktuális elem törléséhez kattintson ![leállítás biztonsági mentés gombra.](./media/backup-azure-manage-vms/delete-button.png)</span><span class="sxs-lookup"><span data-stu-id="2a36e-246">To delete the backup data for the current item, click  ![Stop backup button](./media/backup-azure-manage-vms/delete-button.png)</span></span>

    <span data-ttu-id="2a36e-247">Egy értesítési üzenetet lehetővé teszi a biztonsági mentési adatok törölve lett.</span><span class="sxs-lookup"><span data-stu-id="2a36e-247">A notification message lets you know the backup data has been deleted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a36e-248">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a36e-248">Next steps</span></span>
<span data-ttu-id="2a36e-249">Hozza létre újra a virtuális gép helyreállítási pontból információkért tekintse meg [visszaállítása az Azure virtuális gépek](backup-azure-restore-vms.md).</span><span class="sxs-lookup"><span data-stu-id="2a36e-249">For information on re-creating a virtual machine from a recovery point, check out [Restore Azure VMs](backup-azure-restore-vms.md).</span></span> <span data-ttu-id="2a36e-250">Ha a virtuális gépek védelméről tájékoztatásra van szüksége, tekintse meg [először: készítsen biztonsági mentést a Recovery Services-tároló virtuális gépek](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="2a36e-250">If you need information on protecting your virtual machines, see [First look: Back up VMs to a Recovery Services vault](backup-azure-vms-first-look-arm.md).</span></span> <span data-ttu-id="2a36e-251">Események figyelésével kapcsolatos további információkért lásd: [figyelése az Azure virtuális gépek biztonsági mentéseinek riasztások](backup-azure-monitor-vms.md).</span><span class="sxs-lookup"><span data-stu-id="2a36e-251">For information on monitoring events, see [Monitor alerts for Azure virtual machine backups](backup-azure-monitor-vms.md).</span></span>
