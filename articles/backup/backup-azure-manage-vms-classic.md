---
title: "Kezelni és megfigyelni az Azure virtuális gépek biztonsági mentéseinek |} Microsoft Docs"
description: "Ismerje meg, hogyan kezelheti és figyelheti az Azure virtuális gépek biztonsági mentéseinek"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: trinadhk;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d876bb1759600fa29a26730bfa8b4ec19db1e442
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-the-classic-portal"></a><span data-ttu-id="fbc23-103">Közös Azure biztonsági mentési feladatok és a klasszikus portálon eseményindító-riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="fbc23-103">Manage common Azure Backup jobs and trigger alerts in the classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fbc23-104">Azure virtuális gép biztonsági mentések kezelése</span><span class="sxs-lookup"><span data-stu-id="fbc23-104">Manage Azure VM backups</span></span>](backup-azure-manage-vms.md)
> * [<span data-ttu-id="fbc23-105">Klasszikus virtuális gép biztonsági mentések kezelése</span><span class="sxs-lookup"><span data-stu-id="fbc23-105">Manage Classic VM backups</span></span>](backup-azure-manage-vms-classic.md)
>
>

<span data-ttu-id="fbc23-106">Ez a cikk tájékoztatást ad azokról a közös felügyeleti és megfigyelési feladatok Klasszikus-modell virtuális gépek Azure-ral védett.</span><span class="sxs-lookup"><span data-stu-id="fbc23-106">This article provides information about common management and monitoring tasks for Classic-model virtual machines protected in Azure.</span></span>  

> [!NOTE]
> <span data-ttu-id="fbc23-107">Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="fbc23-107">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="fbc23-108">Lásd: [készítse elő a környezetet a biztonsági mentése Azure virtuális gépek](backup-azure-vms-prepare.md) talál részletes információt használata a klasszikus telepítési modell virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="fbc23-108">See [Prepare your environment to back up Azure virtual machines](backup-azure-vms-prepare.md) for details on working with Classic deployment model VMs.</span></span>
>
> [!IMPORTANT]
><span data-ttu-id="fbc23-109">2017 márciusától már nem hozhat létre Backup-tárolókat a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="fbc23-109">Starting March 2017, you can no longer use the classic portal to create Backup vaults.</span></span>
>
> <span data-ttu-id="fbc23-110">A biztonsági mentési tárakról mostantól lehetőség van helyreállítási tárakra váltani.</span><span class="sxs-lookup"><span data-stu-id="fbc23-110">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="fbc23-111">A részletekről bővebben az [Váltás biztonsági mentési tárolóról Recovery Services-tárolóra](backup-azure-upgrade-backup-to-recovery-services.md) című cikkben olvashat.</span><span class="sxs-lookup"><span data-stu-id="fbc23-111">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="fbc23-112">A Microsoft azt javasolja, hogy a biztonsági mentési tárolóról váltson Recovery Services-tárolóra.</span><span class="sxs-lookup"><span data-stu-id="fbc23-112">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="fbc23-113">2017. október 15-től a PowerShell nem használható Backup-tárolók létrehozására.</span><span class="sxs-lookup"><span data-stu-id="fbc23-113">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="fbc23-114">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="fbc23-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="fbc23-115">Minden fennmaradó Backup-tároló automatikusan Recovery Services-tárolóra frissül.</span><span class="sxs-lookup"><span data-stu-id="fbc23-115">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="fbc23-116">A klasszikus portálon nem lehet majd hozzáférni a biztonsági másolati adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="fbc23-116">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="fbc23-117">Helyette az Azure Portal segítségével férhet hozzá a Recovery Services-tárolókban található biztonsági mentési adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="fbc23-117">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>

## <a name="manage-protected-virtual-machines"></a><span data-ttu-id="fbc23-118">Védett virtuális gépek kezelése</span><span class="sxs-lookup"><span data-stu-id="fbc23-118">Manage protected virtual machines</span></span>
<span data-ttu-id="fbc23-119">Védett virtuális gépek kezeléséhez:</span><span class="sxs-lookup"><span data-stu-id="fbc23-119">To manage protected virtual machines:</span></span>

1. <span data-ttu-id="fbc23-120">Megtekintéséhez és kezeléséhez kattintson a virtuális gép biztonsági mentési beállítások a **védett elemek** fülre.</span><span class="sxs-lookup"><span data-stu-id="fbc23-120">To view and manage backup settings for a virtual machine click the **Protected Items** tab.</span></span>
2. <span data-ttu-id="fbc23-121">Kattintson a nevére, a védett elem megtekintéséhez a **biztonsági mentési részletek** lap, amely jelzi, hogy a legutóbbi biztonsági mentés kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="fbc23-121">Click on the name of a protected item to see the **Backup Details** tab, which shows you information about the last backup.</span></span>

    ![Virtuális gép biztonsági mentése](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. <span data-ttu-id="fbc23-123">Megtekintéséhez és kezeléséhez kattintson a virtuális gép biztonsági mentési házirend beállításai a **házirendek** fülre.</span><span class="sxs-lookup"><span data-stu-id="fbc23-123">To view and manage backup policy settings for a virtual machine click the **Policies** tab.</span></span>

    ![Virtuális gép házirend](./media/backup-azure-manage-vms/manage-policy-settings.png)

    <span data-ttu-id="fbc23-125">A **biztonsági mentési házirendek** lapon láthatók a meglévő házirendet.</span><span class="sxs-lookup"><span data-stu-id="fbc23-125">The **Backup Policies** tab shows you the existing policy.</span></span> <span data-ttu-id="fbc23-126">Igény szerint módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="fbc23-126">You can modify as needed.</span></span> <span data-ttu-id="fbc23-127">Ha szeretne létrehozni egy új házirend kattintson **létrehozása** a a **házirendek** lap.</span><span class="sxs-lookup"><span data-stu-id="fbc23-127">If you need to create a new policy click **Create** on the **Policies** page.</span></span> <span data-ttu-id="fbc23-128">Ne feledje, hogy ha el szeretné távolítani egy házirendet, nem a vele társított virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="fbc23-128">Note that if you want to remove a policy it shouldn't have any virtual machines associated with it.</span></span>

    ![Virtuális gép házirend](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. <span data-ttu-id="fbc23-130">Kaphat műveletek vagy állapottal kapcsolatos további információk a virtuális gépek a **feladatok** lap.</span><span class="sxs-lookup"><span data-stu-id="fbc23-130">You can get more information about actions or status for a virtual machine on the **Jobs** page.</span></span> <span data-ttu-id="fbc23-131">Kattintson egy feladatra a további részletek a listában, vagy egy adott virtuális gép szűrve.</span><span class="sxs-lookup"><span data-stu-id="fbc23-131">Click a job in the list to get more details, or filter jobs for a specific virtual machine.</span></span>

    ![Feladatok](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a><span data-ttu-id="fbc23-133">Igény szerinti biztonsági mentést a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="fbc23-133">On-demand backup of a virtual machine</span></span>
<span data-ttu-id="fbc23-134">Igény szerinti biztonsági mentését egy virtuális gép eltarthat, ha konfigurálva van a védelem.</span><span class="sxs-lookup"><span data-stu-id="fbc23-134">You can take an on-demand backup of a virtual machine once it is configured for protection.</span></span> <span data-ttu-id="fbc23-135">Ha a kezdeti biztonsági másolatot függőben a virtuális gép igény szerinti biztonsági mentést a virtuális gép teljes másolata létrehoz az Azure mentési tárolóval.</span><span class="sxs-lookup"><span data-stu-id="fbc23-135">If the initial backup is pending for the virtual machine, on-demand backup will create a full copy of the virtual machine in Azure backup vault.</span></span> <span data-ttu-id="fbc23-136">Ha első biztonsági mentés befejeződött, igény szerinti biztonsági mentése nem fog csak küldési módosítások korábbi biztonsági mentés az Azure biztonsági mentésre tároló azaz azt nem mindig növekményes.</span><span class="sxs-lookup"><span data-stu-id="fbc23-136">If first backup is completed, on-demand backup will only send changes from previous backup to Azure backup vault i.e. it is always incremental.</span></span>

> [!NOTE]
> <span data-ttu-id="fbc23-137">Egy igény szerinti biztonsági mentés a megőrzési tartomány megadott biztonsági mentési házirendben, a virtuális gép megfelelő napi megőrzési megőrzési értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="fbc23-137">Retention range of an on-demand backup is set to retention value specified for Daily retention in backup policy corresponding to the VM.</span></span>  
>
>

<span data-ttu-id="fbc23-138">Igény szerinti biztonsági mentését egy virtuális gép érvénybe:</span><span class="sxs-lookup"><span data-stu-id="fbc23-138">To take an on-demand backup of a virtual machine:</span></span>

1. <span data-ttu-id="fbc23-139">Keresse meg a **védett elemek** lapon, és válassza **Azure virtuális gép** , **típus** (Ha még nincs kiválasztva), majd kattintson a **válasszon** jelölt gombra.</span><span class="sxs-lookup"><span data-stu-id="fbc23-139">Navigate to the **Protected Items** page and select **Azure Virtual Machine** as **Type** (if not already selected) and click on **Select** button.</span></span>

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="fbc23-141">Válassza ki a virtuális gépet, amelyen igény szerinti biztonsági másolatok készítéséhez, majd kattintson a kívánt **a biztonsági mentés gombra** gombra az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="fbc23-141">Select the virtual machine on which you want to take an on-demand backup and click on **Backup Now** button at the bottom of the page.</span></span>

    ![Biztonsági másolat készítése](./media/backup-azure-manage-vms/backup-now.png)

    <span data-ttu-id="fbc23-143">Ezzel létrehoz egy biztonsági mentési feladat a kijelölt virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="fbc23-143">This will create a backup job on the selected virtual machine.</span></span> <span data-ttu-id="fbc23-144">Ez a feladat segítségével létrehozott helyreállítási pont megőrzési időtartamának ugyanaz, mint a virtuális géphez társított házirendben megadott lesz.</span><span class="sxs-lookup"><span data-stu-id="fbc23-144">Retention range of recovery point created through this job will be same as that specified in the policy associated with the virtual machine.</span></span>

    ![Biztonsági mentési feladat létrehozása](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > <span data-ttu-id="fbc23-146">A virtuális gépekhez társított szabályzat megtekintéséhez részletekbe menően tárhatják fel a virtuális gép a **védett elemek** lapon, majd kattintson a biztonsági mentési házirend fülre.</span><span class="sxs-lookup"><span data-stu-id="fbc23-146">To view the policy associated with a virtual machine, drill down into virtual machine in the **Protected Items** page and go to backup policy tab.</span></span>
   >
   >
3. <span data-ttu-id="fbc23-147">A feladat létrehozása után kattintson a **feladat megtekintése** a feladatok lapon a megfelelő feladat megtekintéséhez bejelentési található gombra.</span><span class="sxs-lookup"><span data-stu-id="fbc23-147">Once the job is created, you can click on **View job** button in the toast bar to see the corresponding job in the jobs page.</span></span>

    ![Biztonsági mentési feladat létrehozása](./media/backup-azure-manage-vms/created-job.png)
4. <span data-ttu-id="fbc23-149">A feladat sikeres befejezése után egy helyreállítási pontot létrehozza, amelyek segítségével visszaállítani a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="fbc23-149">After successful completion of the job, a recovery point will be created which you can use to restore the virtual machine.</span></span> <span data-ttu-id="fbc23-150">Ez a helyreállítási pont oszlop értékét is növeli a 1 **védett elemek** lap.</span><span class="sxs-lookup"><span data-stu-id="fbc23-150">This will also increment the recovery point column value by 1 in **Protected Items** page.</span></span>

## <a name="stop-protecting-virtual-machines"></a><span data-ttu-id="fbc23-151">Virtuális gépek védelmének megszüntetése</span><span class="sxs-lookup"><span data-stu-id="fbc23-151">Stop protecting virtual machines</span></span>
<span data-ttu-id="fbc23-152">Ha szeretné, állítsa le a jövőbeli biztonsági mentések a virtuális gépek a következő beállításokkal:</span><span class="sxs-lookup"><span data-stu-id="fbc23-152">You can choose to stop the future backups of a virtual machine with the following options:</span></span>

* <span data-ttu-id="fbc23-153">Azure biztonságimásolat-kezelő tárolójában a virtuális géphez tartozó biztonsági mentési adatok megőrzése mellett</span><span class="sxs-lookup"><span data-stu-id="fbc23-153">Retain backup data associated with virtual machine in Azure Backup vault</span></span>
* <span data-ttu-id="fbc23-154">A virtuális géphez tartozó biztonsági mentési adatok törlése</span><span class="sxs-lookup"><span data-stu-id="fbc23-154">Delete backup data associated with virtual machine</span></span>

<span data-ttu-id="fbc23-155">Ha be van jelölve a virtuális géphez tartozó biztonsági mentési adatok megőrzése mellett, a biztonsági mentési adatok segítségével visszaállítani a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="fbc23-155">If you have selected to retain backup data associated with virtual machine, you can use the backup data to restore the virtual machine.</span></span> <span data-ttu-id="fbc23-156">Ilyen virtuális gépek díjszabása, kattintson a [Itt](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="fbc23-156">For pricing details for such virtual machines, click [here](https://azure.microsoft.com/pricing/details/backup/).</span></span>

<span data-ttu-id="fbc23-157">Szüntesse meg a virtuális gép védelmét:</span><span class="sxs-lookup"><span data-stu-id="fbc23-157">To Stop protection for a virtual machine:</span></span>

1. <span data-ttu-id="fbc23-158">Navigáljon a **védett elemek** lapon, és válassza **Azure virtuális gép** , a szűrő típusa (Ha még nincs kiválasztva), majd kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="fbc23-158">Navigate to **Protected Items** page and select **Azure virtual machine** as the filter type (if not already selected) and click on **Select** button.</span></span>

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="fbc23-160">Válassza ki a virtuális gépet, majd kattintson a **védelem kikapcsolása** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="fbc23-160">Select the virtual machine and click on **Stop Protection** at the bottom of the page.</span></span>

    ![Állítsa le a védelmet](./media/backup-azure-manage-vms/stop-protection.png)
3. <span data-ttu-id="fbc23-162">Alapértelmezés szerint Azure Backup szolgáltatással nem törli a virtuális géphez tartozó biztonsági mentési adatokat.</span><span class="sxs-lookup"><span data-stu-id="fbc23-162">By default, Azure Backup doesn’t delete the backup data associated with the virtual machine.</span></span>

    ![Állítsa le a védelmet megerősítése](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    <span data-ttu-id="fbc23-164">Ha a törlendő biztonsági mentési adatokat, jelölje be a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="fbc23-164">If you want to delete backup data, select the check box.</span></span>

    ![Jelölőnégyzet](./media/backup-azure-manage-vms/checkbox.png)

    <span data-ttu-id="fbc23-166">Válasszon ki egy a biztonsági mentés leállításának oka.</span><span class="sxs-lookup"><span data-stu-id="fbc23-166">Please select a reason for stopping the backup.</span></span> <span data-ttu-id="fbc23-167">Ez nem kötelező, ok megadása segít Azure biztonsági mentés a visszajelzés működik, és priorizálhatja azokat a forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="fbc23-167">While this is optional, providing a reason will help Azure Backup to work on the feedback and prioritize the customer scenarios.</span></span>
4. <span data-ttu-id="fbc23-168">Kattintson a **Submit** gombra kattintva küldje el a **állítsa le a védelmet** feladat.</span><span class="sxs-lookup"><span data-stu-id="fbc23-168">Click on **Submit** button to submit the **Stop protection** job.</span></span> <span data-ttu-id="fbc23-169">Kattintson a **feladat megtekintése** a megfelelő a feladat megtekintéséhez **feladatok** lap.</span><span class="sxs-lookup"><span data-stu-id="fbc23-169">Click on **View Job** to see the corresponding the job in **Jobs** page.</span></span>

    ![Állítsa le a védelmet](./media/backup-azure-manage-vms/stop-protect-success.png)

    <span data-ttu-id="fbc23-171">Ha nem választott **Delete tartozó biztonsági mentési adatok** során lehetőséget **védelem kikapcsolása** varázsló, majd a feladás egy vagy több feladat befejeződése védelmi állapot módosításai **állítvaavédelem**.</span><span class="sxs-lookup"><span data-stu-id="fbc23-171">If you have not selected **Delete associated backup data** option during **Stop Protection** wizard, then post job completion, protection status changes to **Protection Stopped**.</span></span> <span data-ttu-id="fbc23-172">Az adatok továbbra is az Azure Backup szolgáltatással, amíg le nem explicit módon.</span><span class="sxs-lookup"><span data-stu-id="fbc23-172">The data remains with Azure Backup until it is explicitly deleted.</span></span> <span data-ttu-id="fbc23-173">Az adatokat a virtuális gép kiválasztásával bármikor törölheti a **védett elemek** lap, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="fbc23-173">You can always delete the data by selecting the virtual machine in the **Protected Items** page and clicking **Delete**.</span></span>

    ![Leállított védelme](./media/backup-azure-manage-vms/protection-stopped-status.png)

    <span data-ttu-id="fbc23-175">Ha be van jelölve a **Delete tartozó biztonsági mentési adatok** beállítást, a virtuális gép nem része a **védett elemek** lap.</span><span class="sxs-lookup"><span data-stu-id="fbc23-175">If you have selected the **Delete associated backup data** option, the virtual machine won’t be part of the **Protected Items** page.</span></span>

## <a name="re-protect-virtual-machine"></a><span data-ttu-id="fbc23-176">Virtuális gép védelmének újbóli beállításához</span><span class="sxs-lookup"><span data-stu-id="fbc23-176">Re-protect Virtual machine</span></span>
<span data-ttu-id="fbc23-177">Ha nincs kiválasztva a **társítása biztonsági mentési adatok** beállítást **védelem kikapcsolása**, ismételt védelemmel láthatná el a virtuális gép lépések hasonlóak regisztrált virtuális gépek biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="fbc23-177">If you have not selected the **Delete associate backup data** option in **Stop Protection**, you can re-protect the virtual machine by following the steps similar to backing up registered virtual machines.</span></span> <span data-ttu-id="fbc23-178">Ha védett, a virtuális gép lesz biztonsági mentési adatok őrződnek meg előtt állítsa le a védelmet, és a helyreállítási pontok létrehozása után védelmének újbóli beállítását.</span><span class="sxs-lookup"><span data-stu-id="fbc23-178">Once protected, this virtual machine will have backup data retained prior to stop protection and recovery points created after re-protect.</span></span>

<span data-ttu-id="fbc23-179">Ismételt védelemmel való ellátása után a virtuális gép védelmi állapotát változik **védett** ha vannak a helyreállítási pontok előtt **védelem kikapcsolása**.</span><span class="sxs-lookup"><span data-stu-id="fbc23-179">After re-protect, the virtual machine’s protection status will be changed to **Protected** if there are recovery points prior to **Stop Protection**.</span></span>

  ![A feladatátvételen VM](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> <span data-ttu-id="fbc23-181">Ha a virtuális gép ismételt védelmével, dönthet úgy, hogy egy másik házirend érvényesíthető, mint a házirendet, amellyel a virtuális gép védett kezdetben ki.</span><span class="sxs-lookup"><span data-stu-id="fbc23-181">When re-protecting the virtual machine, you can choose a different policy than the policy with which virtual machine was protected initially.</span></span>
>
>

## <a name="unregister-virtual-machines"></a><span data-ttu-id="fbc23-182">Virtuális gépek regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="fbc23-182">Unregister virtual machines</span></span>
<span data-ttu-id="fbc23-183">Ha azt szeretné, hogy távolítsa el a virtuális gépet a mentési tároló:</span><span class="sxs-lookup"><span data-stu-id="fbc23-183">If you want to remove the virtual machine from the backup vault:</span></span>

1. <span data-ttu-id="fbc23-184">Kattintson a **UNREGISTER** gombra az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="fbc23-184">Click on the **UNREGISTER** button at the bottom of the page.</span></span>

    ![Tiltsa le a védelmet](./media/backup-azure-manage-vms/unregister-button.png)

    <span data-ttu-id="fbc23-186">Egy bejelentési értesítést megerősítést kér a képernyő alján jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fbc23-186">A toast notification will appear at the bottom of the screen requesting confirmation.</span></span> <span data-ttu-id="fbc23-187">Kattintson a **Igen** folytatja.</span><span class="sxs-lookup"><span data-stu-id="fbc23-187">Click **YES** to continue.</span></span>

    ![Tiltsa le a védelmet](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a><span data-ttu-id="fbc23-189">Biztonságimásolat-adatok törlése</span><span class="sxs-lookup"><span data-stu-id="fbc23-189">Delete Backup data</span></span>
<span data-ttu-id="fbc23-190">A biztonsági mentési adatokat, vagy egy virtuális géphez tartozó törölheti:</span><span class="sxs-lookup"><span data-stu-id="fbc23-190">You can delete the backup data associated with a virtual machine, either:</span></span>

* <span data-ttu-id="fbc23-191">Védelemleállítási feladat során</span><span class="sxs-lookup"><span data-stu-id="fbc23-191">During Stop Protection Job</span></span>
* <span data-ttu-id="fbc23-192">Után állítsa le a védelmet feladat befejezése után a virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="fbc23-192">After a stop protection job is completed on a virtual machine</span></span>

<span data-ttu-id="fbc23-193">A virtuális gépen, amely a biztonsági mentési adatok törléséhez a *állítva a védelem* állapota sikeres befejezése utáni egy **biztonsági mentés leállítása** feladat:</span><span class="sxs-lookup"><span data-stu-id="fbc23-193">To delete backup data on a virtual machine, which is in the *Protection Stopped* state post successful completion of a **Stop Backup** job:</span></span>

1. <span data-ttu-id="fbc23-194">Keresse meg a **védett elemek** lapon, és válassza **Azure virtuális gép** , *típus* , és kattintson a **kiválasztása** gombra.</span><span class="sxs-lookup"><span data-stu-id="fbc23-194">Navigate to the **Protected Items** page and select **Azure Virtual Machine** as *type* and click the **Select** button.</span></span>

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="fbc23-196">Válassza ki a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="fbc23-196">Select the virtual machine.</span></span> <span data-ttu-id="fbc23-197">A virtuális gép lesz a **állítva a védelem** állapotát.</span><span class="sxs-lookup"><span data-stu-id="fbc23-197">The virtual machine will be in **Protection Stopped** state.</span></span>

    ![Védelem leállítása](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. <span data-ttu-id="fbc23-199">Kattintson a **törlése** gombra az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="fbc23-199">Click the **DELETE** button at the bottom of the page.</span></span>

    ![Törölje a biztonsági mentés](./media/backup-azure-manage-vms/delete-backup.png)
4. <span data-ttu-id="fbc23-201">Az a **biztonsági mentési adatok törlése** varázsló, jelöljön ki egy (ajánlott) biztonsági mentési adatok törlésének okát, majd **Submit**.</span><span class="sxs-lookup"><span data-stu-id="fbc23-201">In the **Delete backup data** wizard, select a reason for deleting backup data (highly recommended) and click **Submit**.</span></span>

    ![biztonsági mentési adatok törlése](./media/backup-azure-manage-vms/delete-backup-data.png)
5. <span data-ttu-id="fbc23-203">Ezzel létrehoz egy feladatot, amely törli a kijelölt virtuális gép biztonsági mentési adatokat.</span><span class="sxs-lookup"><span data-stu-id="fbc23-203">This will create a job to delete backup data of selected virtual machine.</span></span> <span data-ttu-id="fbc23-204">Kattintson a **feladat megtekintése** feladatok lapján a megfelelő feladat megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="fbc23-204">Click **View job** to see corresponding job in Jobs page.</span></span>

    ![Adatok törlése sikeresen megtörtént](./media/backup-azure-manage-vms/delete-data-success.png)

    <span data-ttu-id="fbc23-206">Ha a feladat befejeződött, a virtuális gép megfelelő bejegyzés törlődik **védett elemek** lap.</span><span class="sxs-lookup"><span data-stu-id="fbc23-206">Once the job is completed, the entry corresponding to the virtual machine will be removed from **Protected items** page.</span></span>

## <a name="dashboard"></a><span data-ttu-id="fbc23-207">Irányítópult</span><span class="sxs-lookup"><span data-stu-id="fbc23-207">Dashboard</span></span>
<span data-ttu-id="fbc23-208">Az a **irányítópult** lapon áttekintheti az Azure virtual machines, a tároló és a feladatok az elmúlt 24 órában a hozzájuk kapcsolódó információkat.</span><span class="sxs-lookup"><span data-stu-id="fbc23-208">On the **Dashboard** page you can review information about Azure virtual machines, their storage, and jobs associated with them in the last 24 hours.</span></span> <span data-ttu-id="fbc23-209">Megtekintheti a biztonsági mentés állapotának és a kapcsolódó biztonsági mentési hibák.</span><span class="sxs-lookup"><span data-stu-id="fbc23-209">You can view backup status and any associated backup errors.</span></span>

![Irányítópult](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> <span data-ttu-id="fbc23-211">Az irányítópult értékeket 24 óránként egyszer frissülnek.</span><span class="sxs-lookup"><span data-stu-id="fbc23-211">Values in the dashboard are refreshed once every 24 hours.</span></span>
>
>

## <a name="auditing-operations"></a><span data-ttu-id="fbc23-212">Naplózási műveletek</span><span class="sxs-lookup"><span data-stu-id="fbc23-212">Auditing Operations</span></span>
<span data-ttu-id="fbc23-213">Azure biztonsági mentés át kell tekinteni a "művelet logs" biztonsági mentési műveleteket, így könnyen, hogy pontosan milyen felügyeleti műveleteket a mentési tároló végrehajtott az ügyfél által indított biztosít.</span><span class="sxs-lookup"><span data-stu-id="fbc23-213">Azure backup provides review of the "operation logs" of backup operations triggered by the customer making it easy to see exactly what management operations were performed on the backup vault.</span></span> <span data-ttu-id="fbc23-214">Műveleti naplók kiváló levágást engedélyezése, és a biztonsági mentési műveletek támogatása naplózására.</span><span class="sxs-lookup"><span data-stu-id="fbc23-214">Operations logs enable great post-mortem and audit support for the backup operations.</span></span>

<span data-ttu-id="fbc23-215">A műveletnaplók jelentkezett a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="fbc23-215">The following operations are logged in Operation logs:</span></span>

* <span data-ttu-id="fbc23-216">Regisztráljon</span><span class="sxs-lookup"><span data-stu-id="fbc23-216">Register</span></span>
* <span data-ttu-id="fbc23-217">Regisztrálás törlése</span><span class="sxs-lookup"><span data-stu-id="fbc23-217">Unregister</span></span>
* <span data-ttu-id="fbc23-218">A védelem konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fbc23-218">Configure protection</span></span>
* <span data-ttu-id="fbc23-219">Biztonsági mentés (mindkettő ütemezett és igény szerinti biztonsági mentést BackupNow keresztül)</span><span class="sxs-lookup"><span data-stu-id="fbc23-219">Backup ( Both scheduled as well as on-demand backup through BackupNow)</span></span>
* <span data-ttu-id="fbc23-220">Visszaállítás</span><span class="sxs-lookup"><span data-stu-id="fbc23-220">Restore</span></span>
* <span data-ttu-id="fbc23-221">Állítsa le a védelmet</span><span class="sxs-lookup"><span data-stu-id="fbc23-221">Stop protection</span></span>
* <span data-ttu-id="fbc23-222">biztonsági mentési adatok törlése</span><span class="sxs-lookup"><span data-stu-id="fbc23-222">Delete backup data</span></span>
* <span data-ttu-id="fbc23-223">Házirend hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fbc23-223">Add policy</span></span>
* <span data-ttu-id="fbc23-224">Szabályzat törlése</span><span class="sxs-lookup"><span data-stu-id="fbc23-224">Delete policy</span></span>
* <span data-ttu-id="fbc23-225">Házirend frissítése</span><span class="sxs-lookup"><span data-stu-id="fbc23-225">Update policy</span></span>
* <span data-ttu-id="fbc23-226">Megszakítása</span><span class="sxs-lookup"><span data-stu-id="fbc23-226">Cancel job</span></span>

<span data-ttu-id="fbc23-227">A mentési tárolóban megfelelő műveletnaplók megtekintése:</span><span class="sxs-lookup"><span data-stu-id="fbc23-227">To view operation logs corresponding to a backup vault:</span></span>

1. <span data-ttu-id="fbc23-228">Navigáljon a **szolgáltatások** az Azure-portálon, és kattintson a **műveletnaplók** fülre.</span><span class="sxs-lookup"><span data-stu-id="fbc23-228">Navigate to **Management services** in Azure portal, and then click the **Operation Logs** tab.</span></span>

    ![A műveletnaplók](./media/backup-azure-manage-vms/ops-logs.png)
2. <span data-ttu-id="fbc23-230">Válassza ki a szűrők **biztonsági mentési** , *típus* , és adja meg a mentési tároló nevére a *szolgáltatásnév* , majd kattintson a **küldje el a következőt**.</span><span class="sxs-lookup"><span data-stu-id="fbc23-230">In the filters, select **Backup** as *Type* and specify the backup vault name in *service name* and click on **Submit**.</span></span>

    ![A művelet naplók szűrő](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. <span data-ttu-id="fbc23-232">A műveletek naplófájlokban, válassza ki az összes műveletet, majd kattintson **részletek** művelet megfelelő részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="fbc23-232">In the operations logs, select any operation and click  **Details** to see details corresponding to an operation.</span></span>

    ![Művelet naplók beolvasása részletei](./media/backup-azure-manage-vms/ops-logs-details.png)

    <span data-ttu-id="fbc23-234">A **részletek varázsló** feladat azonosítóját, amelyen ez a művelet akkor váltódik ki, és kezdő időpont a művelet erőforrás működésbe működésével kapcsolatos adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fbc23-234">The **Details wizard** contains information about the operation triggered, job Id, resource on which this operation is triggered, and start time of the operation.</span></span>

    ![Művelet részletei](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a><span data-ttu-id="fbc23-236">Riasztási értesítések</span><span class="sxs-lookup"><span data-stu-id="fbc23-236">Alert notifications</span></span>
<span data-ttu-id="fbc23-237">A feladatok egyéni riasztási értesítéseket kaphat a portálon.</span><span class="sxs-lookup"><span data-stu-id="fbc23-237">You can get custom alert notifications for the jobs in portal.</span></span> <span data-ttu-id="fbc23-238">PowerShell-alapú riasztási szabályok definiálása a műveleti naplókat események ehhez.</span><span class="sxs-lookup"><span data-stu-id="fbc23-238">This is achieved by defining PowerShell-based alert rules on operational logs events.</span></span> <span data-ttu-id="fbc23-239">Azt javasoljuk, *PowerShell 1.3.0 verzió vagy újabb*.</span><span class="sxs-lookup"><span data-stu-id="fbc23-239">We recommend using *PowerShell version 1.3.0 or above*.</span></span>

<span data-ttu-id="fbc23-240">Egyéni biztonsági mentési hibák riasztási értesítést megadásához egy minta parancs alábbihoz hasonlóan jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="fbc23-240">To define a custom notification to alert for backup failures, a sample command will look like:</span></span>

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

<span data-ttu-id="fbc23-241">**ResourceId**: letölthető ez Operations Logs előugró fenti szakaszban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="fbc23-241">**ResourceId**: You can get this from Operations Logs popup as described in above section.</span></span> <span data-ttu-id="fbc23-242">Egy művelet részletei előugró ablakban ResourceUri adni, a parancsmag az erőforrás-azonosítója.</span><span class="sxs-lookup"><span data-stu-id="fbc23-242">ResourceUri in details popup window of an operation is the ResourceId to be supplied for this cmdlet.</span></span>

<span data-ttu-id="fbc23-243">**OperationName**: Ez a formátum lesz "Microsoft.Backup/backupvault/<EventName>" EventName az egyik regisztrálása, Unregister, ConfigureProtection, biztonsági mentése, visszaállítása, StopProtection, DeleteBackupData, CreateProtectionPolicy, DeleteProtectionPolicy, UpdateProtectionPolicy</span><span class="sxs-lookup"><span data-stu-id="fbc23-243">**OperationName**: This will be of the format "Microsoft.Backup/backupvault/<EventName>" where EventName is one of Register,Unregister,ConfigureProtection,Backup,Restore,StopProtection,DeleteBackupData,CreateProtectionPolicy,DeleteProtectionPolicy,UpdateProtectionPolicy</span></span>

<span data-ttu-id="fbc23-244">**Állapot**: támogatott értékek vannak-elindítva, sikeres és sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="fbc23-244">**Status**: Supported values are- Started, Succeeded and Failed.</span></span>

<span data-ttu-id="fbc23-245">**Erőforráscsoport**: az erőforrás, amelyeken a művelet akkor váltódik ki, ResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="fbc23-245">**ResourceGroup**:ResourceGroup of the resource on which operation is triggered.</span></span> <span data-ttu-id="fbc23-246">Ezt úgy szerezheti be ResourceId értékből.</span><span class="sxs-lookup"><span data-stu-id="fbc23-246">You can obtain this from ResourceId value.</span></span> <span data-ttu-id="fbc23-247">Érték mező között */resourceGroups/* és */providers/* a ResourceId érték a a ResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="fbc23-247">Value between fields */resourceGroups/* and */providers/* in ResourceId value is the value for ResourceGroup.</span></span>

<span data-ttu-id="fbc23-248">**Név**: a riasztási szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="fbc23-248">**Name**: Name of the Alert Rule.</span></span>

<span data-ttu-id="fbc23-249">**CustomEmail**: Adja meg a kívánt riasztási értesítés küldése egyéni e-mail cím</span><span class="sxs-lookup"><span data-stu-id="fbc23-249">**CustomEmail**: Specify the custom email address to which you want to send alert notification</span></span>

<span data-ttu-id="fbc23-250">**SendToServiceOwners**: ezt a beállítást a rendszergazdák és a társadminisztrátorok az előfizetés riasztási értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="fbc23-250">**SendToServiceOwners**: This option sends alert notification to all administrators and co-administrators of the subscription.</span></span> <span data-ttu-id="fbc23-251">A használat **New-AzureRmAlertRuleEmail** parancsmag</span><span class="sxs-lookup"><span data-stu-id="fbc23-251">It can be used in **New-AzureRmAlertRuleEmail** cmdlet</span></span>

### <a name="limitations-on-alerts"></a><span data-ttu-id="fbc23-252">Riasztások korlátozásai</span><span class="sxs-lookup"><span data-stu-id="fbc23-252">Limitations on Alerts</span></span>
<span data-ttu-id="fbc23-253">Eseményalapú riasztások végzik a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="fbc23-253">Event-based alerts are subjected to the following limitations:</span></span>

1. <span data-ttu-id="fbc23-254">A figyelmeztetéseket az összes virtuális gépet a mentési tároló.</span><span class="sxs-lookup"><span data-stu-id="fbc23-254">Alerts are triggered on all virtual machines in the backup vault.</span></span> <span data-ttu-id="fbc23-255">Nem szabhatja testre, és máris beolvashatja a riasztásokat az adott virtuális gépek csoportja tartalmazza a biztonságimásolat-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="fbc23-255">You cannot customize it to get alerts for specific set of virtual machines in a backup vault.</span></span>
2. <span data-ttu-id="fbc23-256">A funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="fbc23-256">This feature is in Preview.</span></span> [<span data-ttu-id="fbc23-257">További információ</span><span class="sxs-lookup"><span data-stu-id="fbc23-257">Learn more</span></span>](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. <span data-ttu-id="fbc23-258">Értesítéseket küld a "alerts-noreply@mail.windowsazure.com".</span><span class="sxs-lookup"><span data-stu-id="fbc23-258">You will receive alerts from "alerts-noreply@mail.windowsazure.com".</span></span> <span data-ttu-id="fbc23-259">Az e-mailt olyasvalaki jelenleg nem módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="fbc23-259">Currently you can't modify the email sender.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fbc23-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fbc23-260">Next steps</span></span>
* [<span data-ttu-id="fbc23-261">Állítsa vissza az Azure virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="fbc23-261">Restore Azure VMs</span></span>](backup-azure-restore-vms.md)
