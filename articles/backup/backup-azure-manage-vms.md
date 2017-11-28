---
title: "aaaManage erőforrás-kezelő telepített virtuális gépek biztonsági mentéseinek |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage és a figyelő a Resource Manager telepített virtuális gépek biztonsági mentéseinek"
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
ms.openlocfilehash: 241fc4b2a29c5cd8b8b0ee8efbf8ba4e96c6a7ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-machine-backups"></a><span data-ttu-id="1efcf-103">Azure-beli virtuális gépek biztonsági másolatainak kezelése</span><span class="sxs-lookup"><span data-stu-id="1efcf-103">Manage Azure virtual machine backups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="1efcf-104">Azure virtuális gép biztonsági mentések kezelése</span><span class="sxs-lookup"><span data-stu-id="1efcf-104">Manage Azure VM backups</span></span>](backup-azure-manage-vms.md)
> * [<span data-ttu-id="1efcf-105">Klasszikus virtuális gép biztonsági mentések kezelése</span><span class="sxs-lookup"><span data-stu-id="1efcf-105">Manage Classic VM backups</span></span>](backup-azure-manage-vms-classic.md)
>
>

<span data-ttu-id="1efcf-106">Ez a cikk útmutatást a virtuális gép biztonsági másolatok kezelésére, és hello biztonsági mentésekkel kapcsolatos riasztások adatait, amely a hello portál Irányítópultjára.</span><span class="sxs-lookup"><span data-stu-id="1efcf-106">This article provides guidance on managing VM backups, and explains hello backup alerts information available in hello portal dashboard.</span></span> <span data-ttu-id="1efcf-107">Ez a cikk útmutatást hello toousing virtuális gépek Recovery Services-tárolók vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="1efcf-107">hello guidance in this article applies toousing VMs with Recovery Services vaults.</span></span> <span data-ttu-id="1efcf-108">Ez a cikk nem foglalkozik a virtuális gépek hello létrehozását, se nem ismertetik hogyan tooprotect virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="1efcf-108">This article does not cover hello creation of virtual machines, nor does it explain how tooprotect virtual machines.</span></span> <span data-ttu-id="1efcf-109">A Recovery Services-tároló virtuális gépek Azure Resource Manager üzembe az Azure-ban ellátásáról ismertetése, lásd: [először: készítsen biztonsági mentést a Recovery Services-tároló virtuális gépek tooa](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="1efcf-109">For a primer on protecting Azure Resource Manager-deployed VMs in Azure with a Recovery Services vault, see [First look: Back up VMs tooa Recovery Services vault](backup-azure-vms-first-look-arm.md).</span></span>

## <a name="manage-vaults-and-protected-virtual-machines"></a><span data-ttu-id="1efcf-110">Tárolók és a védett virtuális gépek kezelése</span><span class="sxs-lookup"><span data-stu-id="1efcf-110">Manage vaults and protected virtual machines</span></span>
<span data-ttu-id="1efcf-111">Hello Azure-portálon hello Recovery Services-tároló irányítópult biztosít hozzáférést tooinformation kapcsolatos hello tároló többek között:</span><span class="sxs-lookup"><span data-stu-id="1efcf-111">In hello Azure portal, hello Recovery Services vault dashboard provides access tooinformation about hello vault including:</span></span>

* <span data-ttu-id="1efcf-112">hello legfrissebb biztonsági mentési pillanatképet, amely egyben hello legújabb visszaállítási pont < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-112">hello most recent backup snapshot, which is also hello latest restore point <br\></span></span>
* <span data-ttu-id="1efcf-113">a biztonsági mentési házirend hello < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-113">hello backup policy <br\></span></span>
* <span data-ttu-id="1efcf-114">teljes méret minden biztonsági mentési pillanatképek < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-114">total size of all backup snapshots <br\></span></span>
* <span data-ttu-id="1efcf-115">hello tárolóban védett virtuális gépek száma < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-115">number of virtual machines that are protected with hello vault <br\></span></span>

<span data-ttu-id="1efcf-116">Egy virtuális gép biztonsági másolatából számos felügyeleti feladatot a kezdő hello irányítópulton hello tároló megnyitása.</span><span class="sxs-lookup"><span data-stu-id="1efcf-116">Many management tasks with a virtual machine backup begin with opening hello vault in hello dashboard.</span></span> <span data-ttu-id="1efcf-117">Azonban mivel tárolók lehetnek több elemet (vagy több virtuális gép), nyissa meg a tooview egy adott virtuális gép vonatkozó részletek használt tooprotect hello tároló elem irányítópult.</span><span class="sxs-lookup"><span data-stu-id="1efcf-117">However, because vaults can be used tooprotect multiple items (or multiple VMs), tooview details about a particular VM, open hello vault item dashboard.</span></span> <span data-ttu-id="1efcf-118">hello alábbi eljárás bemutatja, hogyan tooopen hello *tároló irányítópult* és folytassa a toohello *tároló elem irányítópult*.</span><span class="sxs-lookup"><span data-stu-id="1efcf-118">hello following procedure shows you how tooopen hello *vault dashboard* and then continue toohello *vault item dashboard*.</span></span> <span data-ttu-id="1efcf-119">Nincsenek "tippek" mindkét eljárásnál, amelyek hogyan tooadd hello tároló és a tároló elem toohello Azure irányítópult hello PIN-kód toodashboard parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="1efcf-119">There are "tips" in both procedures that point out how tooadd hello vault and vault item toohello Azure dashboard by using hello Pin toodashboard command.</span></span> <span data-ttu-id="1efcf-120">PIN-kód toodashboard egy helyi toohello tárolóhoz vagy az elem létrehozásának módja.</span><span class="sxs-lookup"><span data-stu-id="1efcf-120">Pin toodashboard is a way of creating a shortcut toohello vault or item.</span></span> <span data-ttu-id="1efcf-121">Általános jellegű parancsok a hello helyi is végrehajthat.</span><span class="sxs-lookup"><span data-stu-id="1efcf-121">You can also execute common commands from hello shortcut.</span></span>

> [!TIP]
> <span data-ttu-id="1efcf-122">Ha több irányítópultok és panel nyílik meg, csúszkával hello sötét-kék hello ablak tooslide hello Azure irányítópult oda-vissza hello alján.</span><span class="sxs-lookup"><span data-stu-id="1efcf-122">If you have multiple dashboards and blades open, use hello dark-blue slider at hello bottom of hello window tooslide hello Azure dashboard back and forth.</span></span>
>
>

![A csúszka teljes áttekintése](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-hello-dashboard"></a><span data-ttu-id="1efcf-124">Nyissa meg a Recovery Services-tároló hello irányítópulton:</span><span class="sxs-lookup"><span data-stu-id="1efcf-124">Open a Recovery Services vault in hello dashboard:</span></span>
1. <span data-ttu-id="1efcf-125">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1efcf-125">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="1efcf-126">Hello központ menüben kattintson a **Tallózás** hello az erőforrások listájához, írja be a **Recovery Services**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-126">On hello Hub menu, click **Browse** and in hello list of resources, type **Recovery Services**.</span></span> <span data-ttu-id="1efcf-127">Írja be megkezdése előtt, hello szűrők megjelenítése a megadott feltételeknek.</span><span class="sxs-lookup"><span data-stu-id="1efcf-127">As you begin typing, hello list filters based on your input.</span></span> <span data-ttu-id="1efcf-128">Kattintson a **Recovery Services-tároló** elemre.</span><span class="sxs-lookup"><span data-stu-id="1efcf-128">Click **Recovery Services vault**.</span></span>

    ![Recovery Services-tároló létrehozása – 1. lépés](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    <span data-ttu-id="1efcf-130">Recovery Services-tárolók listája hello jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="1efcf-130">hello list of Recovery Services vaults are displayed.</span></span>

    ![<span data-ttu-id="1efcf-131">Helyreállítási szolgáltatások listája tárolók</span><span class="sxs-lookup"><span data-stu-id="1efcf-131">List of Recovery Services vaults</span></span> ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

   > [!TIP]
   > <span data-ttu-id="1efcf-132">Ha egy tároló toohello Azure irányítópult rögzíti, hogy a tároló érhető el azonnal hello Azure-portál megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="1efcf-132">If you pin a vault toohello Azure Dashboard, that vault is immediately accessible when you open hello Azure portal.</span></span> <span data-ttu-id="1efcf-133">toopin tároló toohello irányítópult hello tároló listában, kattintson a jobb gombbal a hello tárolóban, és válassza ki **PIN-kód toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-133">toopin a vault toohello dashboard, in hello vault list, right-click hello vault, and select **Pin toodashboard**.</span></span>
   >
   >
3. <span data-ttu-id="1efcf-134">A tárolók hello listából válassza az irányítópult hello tároló tooopen.</span><span class="sxs-lookup"><span data-stu-id="1efcf-134">From hello list of vaults, select hello vault tooopen its dashboard.</span></span> <span data-ttu-id="1efcf-135">Ha bejelöli az hello tárolóban, hello tároló irányítópult és hello **beállítások** panel megnyitása.</span><span class="sxs-lookup"><span data-stu-id="1efcf-135">When you select hello vault, hello vault dashboard and hello **Settings** blade open.</span></span> <span data-ttu-id="1efcf-136">A kép a következő hello, hello **Contoso-tároló** irányítópult ki van jelölve.</span><span class="sxs-lookup"><span data-stu-id="1efcf-136">In hello following image, hello **Contoso-vault** dashboard is highlighted.</span></span>

    ![Nyissa meg a tároló irányítópult és a beállítások panel](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a><span data-ttu-id="1efcf-138">Nyissa meg a tároló elem irányítópult</span><span class="sxs-lookup"><span data-stu-id="1efcf-138">Open a vault item dashboard</span></span>
<span data-ttu-id="1efcf-139">Hello előző eljárásban megnyitott hello tároló irányítópult.</span><span class="sxs-lookup"><span data-stu-id="1efcf-139">In hello previous procedure you opened hello vault dashboard.</span></span> <span data-ttu-id="1efcf-140">tooopen hello tároló elem irányítópult:</span><span class="sxs-lookup"><span data-stu-id="1efcf-140">tooopen hello vault item dashboard:</span></span>

1. <span data-ttu-id="1efcf-141">Hello tároló irányítópult hello **biztonsági mentés elemek** csempére, kattintson a **Azure virtuális gépek**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-141">In hello vault dashboard, on hello **Backup Items** tile, click **Azure Virtual Machines**.</span></span>

    ![Nyissa meg a biztonsági másolati elemei csempe](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    <span data-ttu-id="1efcf-143">Hello **biztonsági másolati elemei** panel felsorolja az egyes elemekhez tartozó biztonsági mentési feladat utolsó hello.</span><span class="sxs-lookup"><span data-stu-id="1efcf-143">hello **Backup Items** blade lists hello last backup job for each item.</span></span> <span data-ttu-id="1efcf-144">Ebben a példában a rendszer egy védett virtuális gépek, demovm-markgal, amelyet ehhez a tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="1efcf-144">In this example, there is one virtual machine, demovm-markgal, protected by this vault.</span></span>  

    ![Biztonsági másolati elemei csempe](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > <span data-ttu-id="1efcf-146">Könnyű elérés, a PIN-kód egy tároló elem toohello Azure irányítópult.</span><span class="sxs-lookup"><span data-stu-id="1efcf-146">For ease of access, you can pin a vault item toohello Azure Dashboard.</span></span> <span data-ttu-id="1efcf-147">egy tároló elem hello tároló elemek listáját, kattintson a jobb gombbal hello elemet, és válassza ki a toopin **PIN-kód toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-147">toopin a vault item, in hello vault item list, right-click hello item and select **Pin toodashboard**.</span></span>
   >
   >
2. <span data-ttu-id="1efcf-148">A hello **biztonsági mentés elemek** panelen hello elem tooopen hello tároló elem irányítópult kattintva.</span><span class="sxs-lookup"><span data-stu-id="1efcf-148">In hello **Backup Items** blade, click hello item tooopen hello vault item dashboard.</span></span>

    ![Biztonsági másolati elemei csempe](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    <span data-ttu-id="1efcf-150">hello tároló elem irányítópult és a **beállítások** panel megnyitása.</span><span class="sxs-lookup"><span data-stu-id="1efcf-150">hello vault item dashboard and its **Settings** blade open.</span></span>

    ![Biztonsági másolati elemei irányítópultot, amelynek beállítások panel](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    <span data-ttu-id="1efcf-152">Irányítópultról hello tároló elem például több kulcskezelési feladatok, érhető el:</span><span class="sxs-lookup"><span data-stu-id="1efcf-152">From hello vault item dashboard, you can accomplish many key management tasks, such as:</span></span>

   * <span data-ttu-id="1efcf-153">Módosítsa a házirendek, vagy hozzon létre egy új biztonsági mentési házirend < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-153">change policies or create a new backup policy<br\></span></span>
   * <span data-ttu-id="1efcf-154">megtekintheti a visszaállítási pontok, és a konzisztencia állapota < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-154">view restore points, and see their consistency state <br\></span></span>
   * <span data-ttu-id="1efcf-155">igény szerinti biztonsági mentést a virtuális gépek < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-155">on-demand backup of a virtual machine <br\></span></span>
   * <span data-ttu-id="1efcf-156">Állítsa le a virtuális gépek védelme < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-156">stop protecting virtual machines <br\></span></span>
   * <span data-ttu-id="1efcf-157">a virtuális gépek a védelem folytatásához < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-157">resume protection of a virtual machine <br\></span></span>
   * <span data-ttu-id="1efcf-158">a biztonsági mentési adatok (vagy a helyreállítási pont) törlése < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-158">delete a backup data (or recovery point) <br\></span></span>
   * <span data-ttu-id="1efcf-159">[Állítsa vissza a biztonsági mentési lemezek](backup-azure-arm-restore-vms.md#restore-backed-up-disks) < br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-159">[restore backup disks](backup-azure-arm-restore-vms.md#restore-backed-up-disks)  <br\></span></span>

<span data-ttu-id="1efcf-160">Az alábbi eljárásokat hello kiindulási pontjaként hello hello tároló elem irányítópult.</span><span class="sxs-lookup"><span data-stu-id="1efcf-160">For hello following procedures, hello starting point is hello vault item dashboard.</span></span>

## <a name="manage-backup-policies"></a><span data-ttu-id="1efcf-161">Biztonsági mentési irányelvek kezelése</span><span class="sxs-lookup"><span data-stu-id="1efcf-161">Manage backup policies</span></span>
1. <span data-ttu-id="1efcf-162">A hello [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **összes beállítás** tooopen hello **beállítások** panelen.</span><span class="sxs-lookup"><span data-stu-id="1efcf-162">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **All Settings** tooopen hello **Settings** blade.</span></span>

    ![A biztonsági mentési házirend panel](./media/backup-azure-manage-vms/all-settings-button.png)
2. <span data-ttu-id="1efcf-164">A hello **beállítások** panelen kattintson a **biztonsági mentési házirend** tooopen adott panelhez.</span><span class="sxs-lookup"><span data-stu-id="1efcf-164">On hello **Settings** blade, click **Backup policy** tooopen that blade.</span></span>

    <span data-ttu-id="1efcf-165">Hello paneljén hello biztonsági mentési gyakoriság és megőrzési tartomány részletei láthatók.</span><span class="sxs-lookup"><span data-stu-id="1efcf-165">On hello blade, hello backup frequency and retention range details are shown.</span></span>

    ![A biztonsági mentési házirend panel](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. <span data-ttu-id="1efcf-167">A hello **válassza ki a biztonsági mentési házirend** menüben:</span><span class="sxs-lookup"><span data-stu-id="1efcf-167">From hello **Choose backup policy** menu:</span></span>

   * <span data-ttu-id="1efcf-168">toochange házirendek, jelöljön ki egy másik házirendet, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-168">toochange policies, select a different policy and click **Save**.</span></span> <span data-ttu-id="1efcf-169">Új házirend hello azonnal alkalmazott toohello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="1efcf-169">hello new policy is immediately applied toohello vault.</span></span> <span data-ttu-id="1efcf-170">< br\></span><span class="sxs-lookup"><span data-stu-id="1efcf-170"><br\></span></span>
   * <span data-ttu-id="1efcf-171">toocreate egy házirendet, válassza ki **hozzon létre új**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-171">toocreate a policy, select **Create New**.</span></span>

     ![Virtuális gép biztonsági mentése](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     <span data-ttu-id="1efcf-173">A biztonsági mentési házirend létrehozásával kapcsolatos utasításokért lásd: [biztonsági mentési házirend meghatározása](backup-azure-manage-vms.md#defining-a-backup-policy).</span><span class="sxs-lookup"><span data-stu-id="1efcf-173">For instructions on creating a backup policy, see [Defining a backup policy](backup-azure-manage-vms.md#defining-a-backup-policy).</span></span>

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> <span data-ttu-id="1efcf-174">Miközben a biztonsági mentési házirendek kezelése, győződjön meg arról, hogy toofollow hello [ajánlott eljárások](backup-azure-vms-introduction.md#best-practices) optimális teljesítményének eléréséhez biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="1efcf-174">While managing backup policies, make sure toofollow hello [best practices](backup-azure-vms-introduction.md#best-practices) for optimal backup performance</span></span>
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a><span data-ttu-id="1efcf-175">Igény szerinti biztonsági mentést a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="1efcf-175">On-demand backup of a virtual machine</span></span>
<span data-ttu-id="1efcf-176">Igény szerinti biztonsági mentését egy virtuális gép eltarthat, ha konfigurálva van a védelem.</span><span class="sxs-lookup"><span data-stu-id="1efcf-176">You can take an on-demand backup of a virtual machine once it is configured for protection.</span></span> <span data-ttu-id="1efcf-177">Ha hello kezdeti biztonsági mentés folyamatban, igény szerinti biztonsági mentést a Recovery Services-tároló hello hello virtuális gép teljes másolatot készít.</span><span class="sxs-lookup"><span data-stu-id="1efcf-177">If hello initial backup is pending, on-demand backup creates a full copy of hello virtual machine in hello Recovery Services vault.</span></span> <span data-ttu-id="1efcf-178">Hello kezdeti biztonsági mentésének végrehajtását, ha egy igény szerinti biztonsági mentést csak küldése módosítások hello korábbi pillanatképből, toohello Recovery Services-tároló.</span><span class="sxs-lookup"><span data-stu-id="1efcf-178">If hello initial backup is completed, an on-demand backup will only send changes from hello previous snapshot, toohello Recovery Services vault.</span></span> <span data-ttu-id="1efcf-179">Ez azt jelenti, hogy azt követő biztonsági mentéseket a rendszer mindig növekményes.</span><span class="sxs-lookup"><span data-stu-id="1efcf-179">That is, subsequent backups are always incremental.</span></span>

> [!NOTE]
> <span data-ttu-id="1efcf-180">egy igény szerinti biztonsági mentés megőrzési időtartama hello hello napi biztonsági mentési pontok hello házirendben megadott hello megőrzési idő értékét.</span><span class="sxs-lookup"><span data-stu-id="1efcf-180">hello retention range for an on-demand backup is hello retention value specified for hello Daily backup point in hello policy.</span></span> <span data-ttu-id="1efcf-181">Ha nincs napi biztonsági mentési pont van kijelölve, hello heti biztonsági mentési pont szerepel.</span><span class="sxs-lookup"><span data-stu-id="1efcf-181">If no Daily backup point is selected, then hello weekly backup point is used.</span></span>
>
>

<span data-ttu-id="1efcf-182">tootrigger igény szerinti biztonsági mentést a virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="1efcf-182">tootrigger an on-demand backup of a virtual machine:</span></span>

* <span data-ttu-id="1efcf-183">A hello [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **biztonsági mentés most**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-183">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Backup now**.</span></span>

    ![Biztonsági mentés most gombra.](./media/backup-azure-manage-vms/backup-now-button.png)

    <span data-ttu-id="1efcf-185">hello portal gondoskodik arról, hogy szeretné-e toostart egy igény szerinti biztonsági mentési feladat.</span><span class="sxs-lookup"><span data-stu-id="1efcf-185">hello portal makes sure that you want toostart an on-demand backup job.</span></span> <span data-ttu-id="1efcf-186">Kattintson a **Igen** toostart hello biztonsági mentési feladat.</span><span class="sxs-lookup"><span data-stu-id="1efcf-186">Click **Yes** toostart hello backup job.</span></span>

    ![Biztonsági mentés most gombra.](./media/backup-azure-manage-vms/backup-now-check.png)

    <span data-ttu-id="1efcf-188">hello biztonsági mentési feladat létrehoz egy helyreállítási pontot.</span><span class="sxs-lookup"><span data-stu-id="1efcf-188">hello backup job creates a recovery point.</span></span> <span data-ttu-id="1efcf-189">hello helyreállítási pont megőrzési időtartamának hello van hello ugyanaz, mint a hello virtuális géphez társított hello házirendben megadott megőrzési ideig.</span><span class="sxs-lookup"><span data-stu-id="1efcf-189">hello retention range of hello recovery point is hello same as retention range specified in hello policy associated with hello virtual machine.</span></span> <span data-ttu-id="1efcf-190">tootrack hello folyamatban hello feladat hello tároló irányítópulton kattintson hello **biztonsági mentési feladatok** csempére.</span><span class="sxs-lookup"><span data-stu-id="1efcf-190">tootrack hello progress for hello job, in hello vault dashboard, click hello **Backup Jobs** tile.</span></span>  

## <a name="stop-protecting-virtual-machines"></a><span data-ttu-id="1efcf-191">Virtuális gépek védelmének megszüntetése</span><span class="sxs-lookup"><span data-stu-id="1efcf-191">Stop protecting virtual machines</span></span>
<span data-ttu-id="1efcf-192">Ha úgy dönt, hogy a virtuális gépek védelmének toostop, a program kéri, ha azt szeretné, tooretain hello helyreállítási pontokat.</span><span class="sxs-lookup"><span data-stu-id="1efcf-192">If you choose toostop protecting a virtual machine, you are asked if you want tooretain hello recovery points.</span></span> <span data-ttu-id="1efcf-193">Két módon toostop védelmet nyújtó virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="1efcf-193">There are two ways toostop protecting virtual machines:</span></span>

* <span data-ttu-id="1efcf-194">Állítsa le az összes jövőbeli biztonsági mentési feladat, és törölje az összes helyreállítási pont, vagy</span><span class="sxs-lookup"><span data-stu-id="1efcf-194">stop all future backup jobs and delete all recovery points, or</span></span>
* <span data-ttu-id="1efcf-195">Állítsa le az összes jövőbeli biztonsági mentési feladat, de hagyjon hello helyreállítási pontok</span><span class="sxs-lookup"><span data-stu-id="1efcf-195">stop all future backup jobs but leave hello recovery points</span></span> <br/>

<span data-ttu-id="1efcf-196">Nincs társított helyreállítási pontok hello hagyja a tárolási költség.</span><span class="sxs-lookup"><span data-stu-id="1efcf-196">There is a cost associated with leaving hello recovery points in storage.</span></span> <span data-ttu-id="1efcf-197">Azonban hello hello Kilépés a helyreállítási pontok előnye, később visszaállíthatja hello virtuális gép igény.</span><span class="sxs-lookup"><span data-stu-id="1efcf-197">However, hello benefit of leaving hello recovery points is you can restore hello virtual machine later, if desired.</span></span> <span data-ttu-id="1efcf-198">Hello költség információt, hogy hello a helyreállítási pontok, lásd: hello [díjszabása](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="1efcf-198">For information about hello cost of leaving hello recovery points, see hello  [pricing details](https://azure.microsoft.com/pricing/details/backup/).</span></span> <span data-ttu-id="1efcf-199">Ha úgy dönt, toodelete összes helyreállítási pont, hello virtuális gép nem állítható vissza.</span><span class="sxs-lookup"><span data-stu-id="1efcf-199">If you choose toodelete all recovery points, you cannot restore hello virtual machine.</span></span>

<span data-ttu-id="1efcf-200">a virtuális gép toostop védelmét:</span><span class="sxs-lookup"><span data-stu-id="1efcf-200">toostop protection for a virtual machine:</span></span>

1. <span data-ttu-id="1efcf-201">A hello [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **Stop biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-201">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Stop backup**.</span></span>

    ![Állítsa le a biztonsági mentési gomb](./media/backup-azure-manage-vms/stop-backup-button.png)

    <span data-ttu-id="1efcf-203">hello állítsa le a biztonsági mentés panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="1efcf-203">hello Stop Backup blade opens.</span></span>

    ![Állítsa le a biztonsági mentési panel](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. <span data-ttu-id="1efcf-205">A hello **biztonsági mentés leállítása** panelen válassza ki, hogy tooretain vagy delete hello biztonsági mentési adatokat.</span><span class="sxs-lookup"><span data-stu-id="1efcf-205">On hello **Stop Backup** blade, choose whether tooretain or delete hello backup data.</span></span> <span data-ttu-id="1efcf-206">hello információ írja be a választott ismerteti.</span><span class="sxs-lookup"><span data-stu-id="1efcf-206">hello information box provides details about your choice.</span></span>

    ![Állítsa le a védelmet](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. <span data-ttu-id="1efcf-208">Ha úgy dönt, hogy tooretain hello biztonsági mentési adatokat, akkor hagyja ki toostep 4.</span><span class="sxs-lookup"><span data-stu-id="1efcf-208">If you chose tooretain hello backup data, skip toostep 4.</span></span> <span data-ttu-id="1efcf-209">Ha úgy dönt, hogy a biztonsági mentési adatok toodelete, győződjön meg arról, hogy szeretné, hogy toostop hello biztonsági mentési feladatot, és hello helyreállítási pontok – hello típusnév hello elem törlése.</span><span class="sxs-lookup"><span data-stu-id="1efcf-209">If you chose toodelete backup data, confirm that you want toostop hello backup jobs and delete hello recovery points - type hello name of hello item.</span></span>

    ![Állítsa le a ellenőrzése](./media/backup-azure-manage-vms/item-verification-box.png)

    <span data-ttu-id="1efcf-211">Ha nem biztos a hello elem neve, mutasson hello felkiáltójel tooview hello nevét.</span><span class="sxs-lookup"><span data-stu-id="1efcf-211">If you aren't sure of hello item name, hover over hello exclamation mark tooview hello name.</span></span> <span data-ttu-id="1efcf-212">Hello elem hello nevét is a **állítsa le a biztonsági mentés** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="1efcf-212">Also, hello name of hello item is under **Stop Backup** at hello top of hello blade.</span></span>
4. <span data-ttu-id="1efcf-213">Megadhat egy **OK** vagy **Megjegyzés**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-213">Optionally provide a **Reason** or **Comment**.</span></span>
5. <span data-ttu-id="1efcf-214">Kattintson a toostop hello biztonsági mentési feladat az aktuális elem hello ![leállítás biztonsági mentés gombra.](./media/backup-azure-manage-vms/stop-backup-button-blue.png)</span><span class="sxs-lookup"><span data-stu-id="1efcf-214">toostop hello backup job for hello current item, click  ![Stop backup button](./media/backup-azure-manage-vms/stop-backup-button-blue.png)</span></span>

    <span data-ttu-id="1efcf-215">Értesítési üzenet értesíti Önt arról a hello biztonsági mentési feladatot leállították.</span><span class="sxs-lookup"><span data-stu-id="1efcf-215">A notification message lets you know hello backup jobs have been stopped.</span></span>

    ![Állítsa le a védelmet megerősítése](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a><span data-ttu-id="1efcf-217">A virtuális gépek a védelem folytatásához</span><span class="sxs-lookup"><span data-stu-id="1efcf-217">Resume protection of a virtual machine</span></span>
<span data-ttu-id="1efcf-218">Ha hello **biztonsági mentési adatok megőrzése** lehetőség választása amikor hello virtuális gép védelmét le lett állítva, akkor célszerű lehetséges tooresume védelem.</span><span class="sxs-lookup"><span data-stu-id="1efcf-218">If hello **Retain Backup Data** option was chosen when protection for hello virtual machine was stopped, then it is possible tooresume protection.</span></span> <span data-ttu-id="1efcf-219">Ha hello **biztonságimásolat-adatok törlése** lehetőséget választotta, akkor nem lehet folytatni a hello virtuális gép védelmét.</span><span class="sxs-lookup"><span data-stu-id="1efcf-219">If hello **Delete Backup Data** option was chosen, then protection for hello virtual machine cannot resume.</span></span>

<span data-ttu-id="1efcf-220">hello virtuális gép védelmét tooresume</span><span class="sxs-lookup"><span data-stu-id="1efcf-220">tooresume protection for hello virtual machine</span></span>

1. <span data-ttu-id="1efcf-221">A hello [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **biztonsági mentés folytatása**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-221">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Resume backup**.</span></span>

    ![A védelem folytatásához](./media/backup-azure-manage-vms/resume-backup-button.png)

    <span data-ttu-id="1efcf-223">hello biztonsági szabályzat panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="1efcf-223">hello Backup Policy blade opens.</span></span>

   > [!NOTE]
   > <span data-ttu-id="1efcf-224">Hello virtuális gép ismételt védelmével, amikor dönthet úgy, mint hello házirend, amellyel a virtuális gép védett eredetileg másik szabályt.</span><span class="sxs-lookup"><span data-stu-id="1efcf-224">When re-protecting hello virtual machine, you can choose a different policy than hello policy with which virtual machine was protected initially.</span></span>
   >
   >
2. <span data-ttu-id="1efcf-225">Hello kövesse [biztonsági mentési házirendek kezelése](backup-azure-manage-vms.md#manage-backup-policies) tooassign hello házirend hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="1efcf-225">Follow hello steps in [Manage backup policies](backup-azure-manage-vms.md#manage-backup-policies) tooassign hello policy for hello virtual machine.</span></span>

    <span data-ttu-id="1efcf-226">Ha biztonsági mentési házirend hello alkalmazott toohello virtuális gépet, a következő üzenet hello láthatja.</span><span class="sxs-lookup"><span data-stu-id="1efcf-226">Once hello backup policy is applied toohello virtual machine, you see hello following message.</span></span>

    ![Sikeresen védett virtuális gép](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a><span data-ttu-id="1efcf-228">Biztonságimásolat-adatok törlése</span><span class="sxs-lookup"><span data-stu-id="1efcf-228">Delete Backup data</span></span>
<span data-ttu-id="1efcf-229">Hello hello során a virtuális géphez tartozó biztonsági mentési adatok törlése **Stop biztonsági mentés** feladat, vagy bármikor hello után a biztonsági mentési feladat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="1efcf-229">You can delete hello backup data associated with a virtual machine during hello **Stop backup** job, or anytime after hello backup job has completed.</span></span> <span data-ttu-id="1efcf-230">Akkor lehet hasznos toowait nap vagy hét hello helyreállítási pontjainak törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="1efcf-230">It may even be beneficial toowait days or weeks before deleting hello recovery points.</span></span> <span data-ttu-id="1efcf-231">Ellentétben a helyreállítási pontok visszaállítása a biztonsági mentési adatok törlésekor, adott helyreállítási pontok toodelete nem választható.</span><span class="sxs-lookup"><span data-stu-id="1efcf-231">Unlike restoring recovery points, when deleting backup data, you cannot choose specific recovery points toodelete.</span></span> <span data-ttu-id="1efcf-232">Ha úgy dönt, toodelete a biztonsági mentési adatokat, hello elemhez tartozó összes helyreállítási pont törlése.</span><span class="sxs-lookup"><span data-stu-id="1efcf-232">If you choose toodelete your backup data, you delete all recovery points associated with hello item.</span></span>

<span data-ttu-id="1efcf-233">hello következő eljárás azt feltételezi, hogy hello biztonságimásolat-készítő feladat hello virtuális gép leállítása vagy letiltása.</span><span class="sxs-lookup"><span data-stu-id="1efcf-233">hello following procedure assumes hello Backup job for hello virtual machine has been stopped or disabled.</span></span> <span data-ttu-id="1efcf-234">Hello biztonságimásolat-készítő feladat le van tiltva, miután hello **biztonsági mentés folytatása** és **Delete biztonsági mentés** az hello tároló elem irányítópult érhetők el.</span><span class="sxs-lookup"><span data-stu-id="1efcf-234">Once hello Backup job is disabled, hello **Resume backup** and **Delete backup** options are available in hello vault item dashboard.</span></span>

![Folytatás és a Törlés gombbal](./media/backup-azure-manage-vms/resume-delete-buttons.png)

<span data-ttu-id="1efcf-236">biztonsági mentési adatok toodelete hello egy virtuális gépen *biztonsági mentése le van tiltva*:</span><span class="sxs-lookup"><span data-stu-id="1efcf-236">toodelete backup data on a virtual machine with hello *Backup disabled*:</span></span>

1. <span data-ttu-id="1efcf-237">A hello [tároló elem irányítópult](backup-azure-manage-vms.md#open-a-vault-item-dashboard), kattintson a **Delete biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-237">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Delete backup**.</span></span>

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    <span data-ttu-id="1efcf-239">Hello **biztonságimásolat-adatok törlése** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="1efcf-239">hello **Delete Backup Data** blade opens.</span></span>

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. <span data-ttu-id="1efcf-241">Hello típusnév hello elem tooconfirm kívánt toodelete hello helyreállítási pontokat.</span><span class="sxs-lookup"><span data-stu-id="1efcf-241">Type hello name of hello item tooconfirm you want toodelete hello recovery points.</span></span>

    ![Állítsa le a ellenőrzése](./media/backup-azure-manage-vms/item-verification-box.png)

    <span data-ttu-id="1efcf-243">Ha nem biztos a hello elem neve, mutasson hello felkiáltójel tooview hello nevét.</span><span class="sxs-lookup"><span data-stu-id="1efcf-243">If you aren't sure of hello item name, hover over hello exclamation mark tooview hello name.</span></span> <span data-ttu-id="1efcf-244">Hello elem hello nevét is a **biztonságimásolat-adatok törlése** hello panel hello tetején.</span><span class="sxs-lookup"><span data-stu-id="1efcf-244">Also, hello name of hello item is under **Delete Backup Data** at hello top of hello blade.</span></span>
3. <span data-ttu-id="1efcf-245">Megadhat egy **OK** vagy **Megjegyzés**.</span><span class="sxs-lookup"><span data-stu-id="1efcf-245">Optionally provide a **Reason** or **Comment**.</span></span>
4. <span data-ttu-id="1efcf-246">Kattintson a toodelete hello biztonsági mentési adatok az aktuális elem hello ![leállítás biztonsági mentés gombra.](./media/backup-azure-manage-vms/delete-button.png)</span><span class="sxs-lookup"><span data-stu-id="1efcf-246">toodelete hello backup data for hello current item, click  ![Stop backup button](./media/backup-azure-manage-vms/delete-button.png)</span></span>

    <span data-ttu-id="1efcf-247">Értesítési üzenet értesíti Önt arról a hello biztonsági mentési adatok törölve lett.</span><span class="sxs-lookup"><span data-stu-id="1efcf-247">A notification message lets you know hello backup data has been deleted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1efcf-248">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1efcf-248">Next steps</span></span>
<span data-ttu-id="1efcf-249">Hozza létre újra a virtuális gép helyreállítási pontból információkért tekintse meg [visszaállítása az Azure virtuális gépek](backup-azure-restore-vms.md).</span><span class="sxs-lookup"><span data-stu-id="1efcf-249">For information on re-creating a virtual machine from a recovery point, check out [Restore Azure VMs](backup-azure-restore-vms.md).</span></span> <span data-ttu-id="1efcf-250">Ha a virtuális gépek védelméről tájékoztatásra van szüksége, tekintse meg [először: készítsen biztonsági mentést a Recovery Services-tároló virtuális gépek tooa](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="1efcf-250">If you need information on protecting your virtual machines, see [First look: Back up VMs tooa Recovery Services vault](backup-azure-vms-first-look-arm.md).</span></span> <span data-ttu-id="1efcf-251">Események figyelésével kapcsolatos további információkért lásd: [figyelése az Azure virtuális gépek biztonsági mentéseinek riasztások](backup-azure-monitor-vms.md).</span><span class="sxs-lookup"><span data-stu-id="1efcf-251">For information on monitoring events, see [Monitor alerts for Azure virtual machine backups](backup-azure-monitor-vms.md).</span></span>
