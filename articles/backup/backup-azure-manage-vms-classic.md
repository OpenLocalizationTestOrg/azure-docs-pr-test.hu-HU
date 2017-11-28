---
title: "aaaManage és megfigyelése az Azure virtuális gépek biztonsági mentéseinek |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage és megfigyelése az Azure virtuális gép biztonsági mentések"
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
ms.openlocfilehash: cb95e0b3760c96f7fd563c42cb4c635553f48957
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-hello-classic-portal"></a><span data-ttu-id="61665-103">Közös Azure biztonsági mentési feladatok és a klasszikus portálon hello eseményindító riasztások kezelése</span><span class="sxs-lookup"><span data-stu-id="61665-103">Manage common Azure Backup jobs and trigger alerts in hello classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="61665-104">Azure virtuális gép biztonsági mentések kezelése</span><span class="sxs-lookup"><span data-stu-id="61665-104">Manage Azure VM backups</span></span>](backup-azure-manage-vms.md)
> * [<span data-ttu-id="61665-105">Klasszikus virtuális gép biztonsági mentések kezelése</span><span class="sxs-lookup"><span data-stu-id="61665-105">Manage Classic VM backups</span></span>](backup-azure-manage-vms-classic.md)
>
>

<span data-ttu-id="61665-106">Ez a cikk tájékoztatást ad azokról a közös felügyeleti és megfigyelési feladatok Klasszikus-modell virtuális gépek Azure-ral védett.</span><span class="sxs-lookup"><span data-stu-id="61665-106">This article provides information about common management and monitoring tasks for Classic-model virtual machines protected in Azure.</span></span>  

> [!NOTE]
> <span data-ttu-id="61665-107">Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="61665-107">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="61665-108">Lásd: [készítse elő a környezetet tooback Azure virtuális gépek](backup-azure-vms-prepare.md) talál részletes információt használata a klasszikus telepítési modell virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="61665-108">See [Prepare your environment tooback up Azure virtual machines](backup-azure-vms-prepare.md) for details on working with Classic deployment model VMs.</span></span>
>
> [!IMPORTANT]
><span data-ttu-id="61665-109">2017. március-től kezdődően már nem használható hello klasszikus portál toocreate mentési tárolókban.</span><span class="sxs-lookup"><span data-stu-id="61665-109">Starting March 2017, you can no longer use hello classic portal toocreate Backup vaults.</span></span>
>
> <span data-ttu-id="61665-110">Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban.</span><span class="sxs-lookup"><span data-stu-id="61665-110">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="61665-111">További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="61665-111">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="61665-112">A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="61665-112">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="61665-113">2017. október 15. után PowerShell toocreate mentési tárolókban nem használható.</span><span class="sxs-lookup"><span data-stu-id="61665-113">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="61665-114">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="61665-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="61665-115">Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="61665-115">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="61665-116">Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="61665-116">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="61665-117">Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.</span><span class="sxs-lookup"><span data-stu-id="61665-117">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>

## <a name="manage-protected-virtual-machines"></a><span data-ttu-id="61665-118">Védett virtuális gépek kezelése</span><span class="sxs-lookup"><span data-stu-id="61665-118">Manage protected virtual machines</span></span>
<span data-ttu-id="61665-119">toomanage védett virtuális gépet:</span><span class="sxs-lookup"><span data-stu-id="61665-119">toomanage protected virtual machines:</span></span>

1. <span data-ttu-id="61665-120">tooview és kezelheti a biztonsági mentési beállításait, a virtuális gépek kattintson hello **védett elemek** fülre.</span><span class="sxs-lookup"><span data-stu-id="61665-120">tooview and manage backup settings for a virtual machine click hello **Protected Items** tab.</span></span>
2. <span data-ttu-id="61665-121">Kattintson a védett elem toosee hello hello nevét a **biztonsági mentési részletek** fülre, amely hello utolsó biztonsági mentés adatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="61665-121">Click on hello name of a protected item toosee hello **Backup Details** tab, which shows you information about hello last backup.</span></span>

    ![Virtuális gép biztonsági mentése](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. <span data-ttu-id="61665-123">tooview és kezelheti a biztonsági mentési házirend beállításai a virtuális gépek kattintson hello **házirendek** fülre.</span><span class="sxs-lookup"><span data-stu-id="61665-123">tooview and manage backup policy settings for a virtual machine click hello **Policies** tab.</span></span>

    ![Virtuális gép házirend](./media/backup-azure-manage-vms/manage-policy-settings.png)

    <span data-ttu-id="61665-125">Hello **biztonsági mentési házirendek** lapra mutat be, akkor a meglévő házirend hello.</span><span class="sxs-lookup"><span data-stu-id="61665-125">hello **Backup Policies** tab shows you hello existing policy.</span></span> <span data-ttu-id="61665-126">Igény szerint módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="61665-126">You can modify as needed.</span></span> <span data-ttu-id="61665-127">Ha toocreate egy új házirendet kell kattintson **létrehozása** a hello **házirendek** lap.</span><span class="sxs-lookup"><span data-stu-id="61665-127">If you need toocreate a new policy click **Create** on hello **Policies** page.</span></span> <span data-ttu-id="61665-128">Ne feledje, hogy ha azt szeretné, hogy tooremove házirend, nem a vele társított virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="61665-128">Note that if you want tooremove a policy it shouldn't have any virtual machines associated with it.</span></span>

    ![Virtuális gép házirend](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. <span data-ttu-id="61665-130">Műveletek vagy állapottal kapcsolatos további információk a virtuális gép hello kaphat **feladatok** lap.</span><span class="sxs-lookup"><span data-stu-id="61665-130">You can get more information about actions or status for a virtual machine on hello **Jobs** page.</span></span> <span data-ttu-id="61665-131">Kattintson egy feladat hello lista tooget további részleteket, vagy egy adott virtuális gép szűrheti a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="61665-131">Click a job in hello list tooget more details, or filter jobs for a specific virtual machine.</span></span>

    ![Feladatok](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a><span data-ttu-id="61665-133">Igény szerinti biztonsági mentést a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="61665-133">On-demand backup of a virtual machine</span></span>
<span data-ttu-id="61665-134">Igény szerinti biztonsági mentését egy virtuális gép eltarthat, ha konfigurálva van a védelem.</span><span class="sxs-lookup"><span data-stu-id="61665-134">You can take an on-demand backup of a virtual machine once it is configured for protection.</span></span> <span data-ttu-id="61665-135">Ha hello kezdeti biztonsági másolatot függőben lévő hello virtuális géphez, igény szerinti biztonsági másolatot hoz létre teljes hello virtuális gép Azure biztonságimásolat-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="61665-135">If hello initial backup is pending for hello virtual machine, on-demand backup will create a full copy of hello virtual machine in Azure backup vault.</span></span> <span data-ttu-id="61665-136">Ha első biztonsági mentés befejeződött, igény szerinti biztonsági mentése nem fog csak a korábbi biztonsági mentési tooAzure biztonsági mentés küldési módosítások tároló azaz azt nem mindig növekményes.</span><span class="sxs-lookup"><span data-stu-id="61665-136">If first backup is completed, on-demand backup will only send changes from previous backup tooAzure backup vault i.e. it is always incremental.</span></span>

> [!NOTE]
> <span data-ttu-id="61665-137">Egy igény szerinti biztonsági mentés megőrzési idejét a biztonsági mentési házirend megfelelő toohello VM napi megőrzési megadott tooretention érték van beállítva.</span><span class="sxs-lookup"><span data-stu-id="61665-137">Retention range of an on-demand backup is set tooretention value specified for Daily retention in backup policy corresponding toohello VM.</span></span>  
>
>

<span data-ttu-id="61665-138">tootake igény szerinti biztonsági mentést a virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="61665-138">tootake an on-demand backup of a virtual machine:</span></span>

1. <span data-ttu-id="61665-139">Keresse meg a toohello **védett elemek** lapon, és válassza **Azure virtuális gép** , **típus** (Ha még nincs kiválasztva), majd kattintson a **Válasszon**gombra.</span><span class="sxs-lookup"><span data-stu-id="61665-139">Navigate toohello **Protected Items** page and select **Azure Virtual Machine** as **Type** (if not already selected) and click on **Select** button.</span></span>

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="61665-141">Válassza ki a hello virtuális gép, amelyen szeretné tootake igény szerinti biztonsági mentési, majd kattintson a **a biztonsági mentés gombra** alján hello hello gombra.</span><span class="sxs-lookup"><span data-stu-id="61665-141">Select hello virtual machine on which you want tootake an on-demand backup and click on **Backup Now** button at hello bottom of hello page.</span></span>

    ![Biztonsági másolat készítése](./media/backup-azure-manage-vms/backup-now.png)

    <span data-ttu-id="61665-143">Ezzel létrehoz egy biztonsági mentési feladat kijelölt hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="61665-143">This will create a backup job on hello selected virtual machine.</span></span> <span data-ttu-id="61665-144">Ez a feladat segítségével létrehozott helyreállítási pont megőrzési időtartamának ugyanaz, mint a megadott hello virtuális géphez társított hello házirend lesz.</span><span class="sxs-lookup"><span data-stu-id="61665-144">Retention range of recovery point created through this job will be same as that specified in hello policy associated with hello virtual machine.</span></span>

    ![Biztonsági mentési feladat létrehozása](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > <span data-ttu-id="61665-146">a virtuális géphez társított, részletezési le a virtuális gép hello tooview hello házirend **védett elemek** lap, és lépjen toobackup szabályzatlap.</span><span class="sxs-lookup"><span data-stu-id="61665-146">tooview hello policy associated with a virtual machine, drill down into virtual machine in hello **Protected Items** page and go toobackup policy tab.</span></span>
   >
   >
3. <span data-ttu-id="61665-147">Miután hello feladat jön létre, rákattinthat a **feladat megtekintése** hello bejelentési toosee hello megfelelő feladat a hello feladatok lapján sáv gombjára.</span><span class="sxs-lookup"><span data-stu-id="61665-147">Once hello job is created, you can click on **View job** button in hello toast bar toosee hello corresponding job in hello jobs page.</span></span>

    ![Biztonsági mentési feladat létrehozása](./media/backup-azure-manage-vms/created-job.png)
4. <span data-ttu-id="61665-149">Hello feladat sikeres befejezése után egy helyreállítási pontot létrehozza amellyel toorestore hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="61665-149">After successful completion of hello job, a recovery point will be created which you can use toorestore hello virtual machine.</span></span> <span data-ttu-id="61665-150">Ez hello helyreállítási oszlop értékét is növeli a 1 **védett elemek** lap.</span><span class="sxs-lookup"><span data-stu-id="61665-150">This will also increment hello recovery point column value by 1 in **Protected Items** page.</span></span>

## <a name="stop-protecting-virtual-machines"></a><span data-ttu-id="61665-151">Virtuális gépek védelmének megszüntetése</span><span class="sxs-lookup"><span data-stu-id="61665-151">Stop protecting virtual machines</span></span>
<span data-ttu-id="61665-152">Az alábbi beállítások hello toostop hello a jövőbeni biztonsági mentés, a virtuális gép lehet választani:</span><span class="sxs-lookup"><span data-stu-id="61665-152">You can choose toostop hello future backups of a virtual machine with hello following options:</span></span>

* <span data-ttu-id="61665-153">Azure biztonságimásolat-kezelő tárolójában a virtuális géphez tartozó biztonsági mentési adatok megőrzése mellett</span><span class="sxs-lookup"><span data-stu-id="61665-153">Retain backup data associated with virtual machine in Azure Backup vault</span></span>
* <span data-ttu-id="61665-154">A virtuális géphez tartozó biztonsági mentési adatok törlése</span><span class="sxs-lookup"><span data-stu-id="61665-154">Delete backup data associated with virtual machine</span></span>

<span data-ttu-id="61665-155">Ha be van jelölve a virtuális géphez tartozó biztonsági mentési adatok tooretain, hello biztonsági mentési adatok toorestore hello virtuális gép is használhatja.</span><span class="sxs-lookup"><span data-stu-id="61665-155">If you have selected tooretain backup data associated with virtual machine, you can use hello backup data toorestore hello virtual machine.</span></span> <span data-ttu-id="61665-156">Ilyen virtuális gépek díjszabása, kattintson a [Itt](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="61665-156">For pricing details for such virtual machines, click [here](https://azure.microsoft.com/pricing/details/backup/).</span></span>

<span data-ttu-id="61665-157">a virtuális gép tooStop védelmét:</span><span class="sxs-lookup"><span data-stu-id="61665-157">tooStop protection for a virtual machine:</span></span>

1. <span data-ttu-id="61665-158">Keresse meg a túl**védett elemek** lapon, és válassza **Azure virtuális gép** hello szűrőtípus (Ha még nincs kiválasztva), és kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="61665-158">Navigate too**Protected Items** page and select **Azure virtual machine** as hello filter type (if not already selected) and click on **Select** button.</span></span>

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="61665-160">Válassza ki hello virtuális gépet, majd kattintson a **védelem kikapcsolása** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="61665-160">Select hello virtual machine and click on **Stop Protection** at hello bottom of hello page.</span></span>

    ![Állítsa le a védelmet](./media/backup-azure-manage-vms/stop-protection.png)
3. <span data-ttu-id="61665-162">Alapértelmezés szerint Azure Backup szolgáltatással nem törli hello hello virtuális géphez tartozó biztonsági mentési adatokat.</span><span class="sxs-lookup"><span data-stu-id="61665-162">By default, Azure Backup doesn’t delete hello backup data associated with hello virtual machine.</span></span>

    ![Állítsa le a védelmet megerősítése](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    <span data-ttu-id="61665-164">Ha azt szeretné, hogy a biztonsági mentési adatok toodelete, jelölje be a hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="61665-164">If you want toodelete backup data, select hello check box.</span></span>

    ![Jelölőnégyzet](./media/backup-azure-manage-vms/checkbox.png)

    <span data-ttu-id="61665-166">Válasszon ki egy hello biztonsági mentés leállításának oka.</span><span class="sxs-lookup"><span data-stu-id="61665-166">Please select a reason for stopping hello backup.</span></span> <span data-ttu-id="61665-167">Ez nem választható, ok megadása a rendszer Azure biztonsági mentés toowork hello visszajelzések segítségével és rangsorolhatja hello forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="61665-167">While this is optional, providing a reason will help Azure Backup toowork on hello feedback and prioritize hello customer scenarios.</span></span>
4. <span data-ttu-id="61665-168">Kattintson a **Submit** gomb toosubmit hello **állítsa le a védelmet** feladat.</span><span class="sxs-lookup"><span data-stu-id="61665-168">Click on **Submit** button toosubmit hello **Stop protection** job.</span></span> <span data-ttu-id="61665-169">Kattintson a **feladat megtekintése** toosee hello megfelelő hello feladat **feladatok** lap.</span><span class="sxs-lookup"><span data-stu-id="61665-169">Click on **View Job** toosee hello corresponding hello job in **Jobs** page.</span></span>

    ![Állítsa le a védelmet](./media/backup-azure-manage-vms/stop-protect-success.png)

    <span data-ttu-id="61665-171">Ha nem választott **Delete tartozó biztonsági mentési adatok** során lehetőséget **védelem kikapcsolása** varázsló, majd a feladás egy vagy több feladat befejezése után védelmi állapota túl**állítvaavédelem**.</span><span class="sxs-lookup"><span data-stu-id="61665-171">If you have not selected **Delete associated backup data** option during **Stop Protection** wizard, then post job completion, protection status changes too**Protection Stopped**.</span></span> <span data-ttu-id="61665-172">az Azure Backup hello adatok marad, amíg le nem explicit módon.</span><span class="sxs-lookup"><span data-stu-id="61665-172">hello data remains with Azure Backup until it is explicitly deleted.</span></span> <span data-ttu-id="61665-173">Hello adatok hello hello virtuális gép kiválasztásával mindig törölhetők **védett elemek** lap, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="61665-173">You can always delete hello data by selecting hello virtual machine in hello **Protected Items** page and clicking **Delete**.</span></span>

    ![Leállított védelme](./media/backup-azure-manage-vms/protection-stopped-status.png)

    <span data-ttu-id="61665-175">Ha be van jelölve hello **Delete tartozó biztonsági mentési adatok** beállításnál hello virtuális gép nem lesz része hello **védett elemek** lap.</span><span class="sxs-lookup"><span data-stu-id="61665-175">If you have selected hello **Delete associated backup data** option, hello virtual machine won’t be part of hello **Protected Items** page.</span></span>

## <a name="re-protect-virtual-machine"></a><span data-ttu-id="61665-176">Virtuális gép védelmének újbóli beállításához</span><span class="sxs-lookup"><span data-stu-id="61665-176">Re-protect Virtual machine</span></span>
<span data-ttu-id="61665-177">Ha nincs kiválasztva hello **társítása biztonsági mentési adatok** beállítást **védelem kikapcsolása**, ismételt védelemmel láthatná el hello virtuális gép által a következő hello lépéseket hasonló toobacking be virtuális regisztrálva gépek.</span><span class="sxs-lookup"><span data-stu-id="61665-177">If you have not selected hello **Delete associate backup data** option in **Stop Protection**, you can re-protect hello virtual machine by following hello steps similar toobacking up registered virtual machines.</span></span> <span data-ttu-id="61665-178">Ha védett, a virtuális gép lesz a biztonsági mentési adatok őrződnek meg előzetes toostop védelem és a helyreállítási pontok létrehozása után védelmének újbóli beállítását.</span><span class="sxs-lookup"><span data-stu-id="61665-178">Once protected, this virtual machine will have backup data retained prior toostop protection and recovery points created after re-protect.</span></span>

<span data-ttu-id="61665-179">Ismételt védelemmel való ellátása után hello virtuális gép védelmi állapotát változnak túl**védett** Ha nincsenek helyreállítási pontok korábbi túl**védelem kikapcsolása**.</span><span class="sxs-lookup"><span data-stu-id="61665-179">After re-protect, hello virtual machine’s protection status will be changed too**Protected** if there are recovery points prior too**Stop Protection**.</span></span>

  ![A feladatátvételen VM](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> <span data-ttu-id="61665-181">Hello virtuális gép ismételt védelmével, amikor dönthet úgy, mint hello házirend, amellyel a virtuális gép védett eredetileg másik szabályt.</span><span class="sxs-lookup"><span data-stu-id="61665-181">When re-protecting hello virtual machine, you can choose a different policy than hello policy with which virtual machine was protected initially.</span></span>
>
>

## <a name="unregister-virtual-machines"></a><span data-ttu-id="61665-182">Virtuális gépek regisztrációjának törlése</span><span class="sxs-lookup"><span data-stu-id="61665-182">Unregister virtual machines</span></span>
<span data-ttu-id="61665-183">Ha azt szeretné, hogy tooremove hello virtuális gépet a mentési tároló hello:</span><span class="sxs-lookup"><span data-stu-id="61665-183">If you want tooremove hello virtual machine from hello backup vault:</span></span>

1. <span data-ttu-id="61665-184">Kattintson a hello **UNREGISTER** alján hello hello gombra.</span><span class="sxs-lookup"><span data-stu-id="61665-184">Click on hello **UNREGISTER** button at hello bottom of hello page.</span></span>

    ![Tiltsa le a védelmet](./media/backup-azure-manage-vms/unregister-button.png)

    <span data-ttu-id="61665-186">Egy bejelentési értesítést megerősítést kér üdvözlő képernyőt hello alján jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="61665-186">A toast notification will appear at hello bottom of hello screen requesting confirmation.</span></span> <span data-ttu-id="61665-187">Kattintson a **Igen** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="61665-187">Click **YES** toocontinue.</span></span>

    ![Tiltsa le a védelmet](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a><span data-ttu-id="61665-189">Biztonságimásolat-adatok törlése</span><span class="sxs-lookup"><span data-stu-id="61665-189">Delete Backup data</span></span>
<span data-ttu-id="61665-190">Hello vagy egy virtuális géphez tartozó biztonsági mentési adatokat is törli:</span><span class="sxs-lookup"><span data-stu-id="61665-190">You can delete hello backup data associated with a virtual machine, either:</span></span>

* <span data-ttu-id="61665-191">Védelemleállítási feladat során</span><span class="sxs-lookup"><span data-stu-id="61665-191">During Stop Protection Job</span></span>
* <span data-ttu-id="61665-192">Után állítsa le a védelmet feladat befejezése után a virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="61665-192">After a stop protection job is completed on a virtual machine</span></span>

<span data-ttu-id="61665-193">a virtuális gépen, amely hello toodelete a biztonsági mentési adatok *állítva a védelem* állapota sikeres befejezése utáni egy **biztonsági mentés leállítása** feladat:</span><span class="sxs-lookup"><span data-stu-id="61665-193">toodelete backup data on a virtual machine, which is in hello *Protection Stopped* state post successful completion of a **Stop Backup** job:</span></span>

1. <span data-ttu-id="61665-194">Keresse meg a toohello **védett elemek** lapon, és válassza **Azure virtuális gép** , *típus* hello kattintson **kiválasztása** gombra.</span><span class="sxs-lookup"><span data-stu-id="61665-194">Navigate toohello **Protected Items** page and select **Azure Virtual Machine** as *type* and click hello **Select** button.</span></span>

    ![Virtuálisgép-típussá](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="61665-196">Válassza ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="61665-196">Select hello virtual machine.</span></span> <span data-ttu-id="61665-197">hello virtuális gép lesz a **állítva a védelem** állapotát.</span><span class="sxs-lookup"><span data-stu-id="61665-197">hello virtual machine will be in **Protection Stopped** state.</span></span>

    ![Védelem leállítása](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. <span data-ttu-id="61665-199">Kattintson a hello **törlése** alján hello hello gombra.</span><span class="sxs-lookup"><span data-stu-id="61665-199">Click hello **DELETE** button at hello bottom of hello page.</span></span>

    ![Törölje a biztonsági mentés](./media/backup-azure-manage-vms/delete-backup.png)
4. <span data-ttu-id="61665-201">A hello **biztonsági mentési adatok törlése** varázsló, jelöljön ki egy (ajánlott) biztonsági mentési adatok törlésének okát, majd **Submit**.</span><span class="sxs-lookup"><span data-stu-id="61665-201">In hello **Delete backup data** wizard, select a reason for deleting backup data (highly recommended) and click **Submit**.</span></span>

    ![biztonsági mentési adatok törlése](./media/backup-azure-manage-vms/delete-backup-data.png)
5. <span data-ttu-id="61665-203">Ekkor létrejön egy feladat toodelete biztonsági mentési adatok kijelölt virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="61665-203">This will create a job toodelete backup data of selected virtual machine.</span></span> <span data-ttu-id="61665-204">Kattintson a **feladat megtekintése** toosee megfelelő feladatot a feladatok lapot.</span><span class="sxs-lookup"><span data-stu-id="61665-204">Click **View job** toosee corresponding job in Jobs page.</span></span>

    ![Adatok törlése sikeresen megtörtént](./media/backup-azure-manage-vms/delete-data-success.png)

    <span data-ttu-id="61665-206">Ha hello feladat befejeződött, hello bejegyzést a megfelelő toohello virtuális gép törlődik **védett elemek** lap.</span><span class="sxs-lookup"><span data-stu-id="61665-206">Once hello job is completed, hello entry corresponding toohello virtual machine will be removed from **Protected items** page.</span></span>

## <a name="dashboard"></a><span data-ttu-id="61665-207">Irányítópult</span><span class="sxs-lookup"><span data-stu-id="61665-207">Dashboard</span></span>
<span data-ttu-id="61665-208">A hello **irányítópult** lapon tekintse át az információkat az Azure virtuális gépek, a tároló és a feladatok a hozzájuk társított runbookokat hello az elmúlt 24 órában.</span><span class="sxs-lookup"><span data-stu-id="61665-208">On hello **Dashboard** page you can review information about Azure virtual machines, their storage, and jobs associated with them in hello last 24 hours.</span></span> <span data-ttu-id="61665-209">Megtekintheti a biztonsági mentés állapotának és a kapcsolódó biztonsági mentési hibák.</span><span class="sxs-lookup"><span data-stu-id="61665-209">You can view backup status and any associated backup errors.</span></span>

![Irányítópult](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> <span data-ttu-id="61665-211">Hello irányítópult értékeket 24 óránként egyszer frissülnek.</span><span class="sxs-lookup"><span data-stu-id="61665-211">Values in hello dashboard are refreshed once every 24 hours.</span></span>
>
>

## <a name="auditing-operations"></a><span data-ttu-id="61665-212">Naplózási műveletek</span><span class="sxs-lookup"><span data-stu-id="61665-212">Auditing Operations</span></span>
<span data-ttu-id="61665-213">Azure biztonsági mentés biztosít hello "művelet logs" biztonsági mentési műveleteket, így könnyen toosee pontosan milyen felügyeleti műveleteket a mentési tároló hello végrehajtott hello ügyfél által indított át kell tekinteni.</span><span class="sxs-lookup"><span data-stu-id="61665-213">Azure backup provides review of hello "operation logs" of backup operations triggered by hello customer making it easy toosee exactly what management operations were performed on hello backup vault.</span></span> <span data-ttu-id="61665-214">Műveleti naplók engedélyezése a nagy levágást, és a naplózási hello biztonsági mentési műveletek támogatása.</span><span class="sxs-lookup"><span data-stu-id="61665-214">Operations logs enable great post-mortem and audit support for hello backup operations.</span></span>

<span data-ttu-id="61665-215">a következő műveletek hello műveletnaplók vannak bejelentkezve:</span><span class="sxs-lookup"><span data-stu-id="61665-215">hello following operations are logged in Operation logs:</span></span>

* <span data-ttu-id="61665-216">Regisztráljon</span><span class="sxs-lookup"><span data-stu-id="61665-216">Register</span></span>
* <span data-ttu-id="61665-217">Regisztrálás törlése</span><span class="sxs-lookup"><span data-stu-id="61665-217">Unregister</span></span>
* <span data-ttu-id="61665-218">A védelem konfigurálása</span><span class="sxs-lookup"><span data-stu-id="61665-218">Configure protection</span></span>
* <span data-ttu-id="61665-219">Biztonsági mentés (mindkettő ütemezett és igény szerinti biztonsági mentést BackupNow keresztül)</span><span class="sxs-lookup"><span data-stu-id="61665-219">Backup ( Both scheduled as well as on-demand backup through BackupNow)</span></span>
* <span data-ttu-id="61665-220">Visszaállítás</span><span class="sxs-lookup"><span data-stu-id="61665-220">Restore</span></span>
* <span data-ttu-id="61665-221">Állítsa le a védelmet</span><span class="sxs-lookup"><span data-stu-id="61665-221">Stop protection</span></span>
* <span data-ttu-id="61665-222">biztonsági mentési adatok törlése</span><span class="sxs-lookup"><span data-stu-id="61665-222">Delete backup data</span></span>
* <span data-ttu-id="61665-223">Házirend hozzáadása</span><span class="sxs-lookup"><span data-stu-id="61665-223">Add policy</span></span>
* <span data-ttu-id="61665-224">Szabályzat törlése</span><span class="sxs-lookup"><span data-stu-id="61665-224">Delete policy</span></span>
* <span data-ttu-id="61665-225">Házirend frissítése</span><span class="sxs-lookup"><span data-stu-id="61665-225">Update policy</span></span>
* <span data-ttu-id="61665-226">Megszakítása</span><span class="sxs-lookup"><span data-stu-id="61665-226">Cancel job</span></span>

<span data-ttu-id="61665-227">tooview műveletnaplókat tooa megfelelő mentési tároló:</span><span class="sxs-lookup"><span data-stu-id="61665-227">tooview operation logs corresponding tooa backup vault:</span></span>

1. <span data-ttu-id="61665-228">Keresse meg a túl**szolgáltatások** Azure-portálon, és kattintson a hello **műveletnaplók** fülre.</span><span class="sxs-lookup"><span data-stu-id="61665-228">Navigate too**Management services** in Azure portal, and then click hello **Operation Logs** tab.</span></span>

    ![A műveletnaplók](./media/backup-azure-manage-vms/ops-logs.png)
2. <span data-ttu-id="61665-230">Hello szűrők, válassza ki **biztonsági mentési** , *típus* , és adja meg a mentési tároló nevére hello *szolgáltatásnév* , majd kattintson a **Submit**.</span><span class="sxs-lookup"><span data-stu-id="61665-230">In hello filters, select **Backup** as *Type* and specify hello backup vault name in *service name* and click on **Submit**.</span></span>

    ![A művelet naplók szűrő](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. <span data-ttu-id="61665-232">Hello műveletek naplófájlokban, jelölje ki az összes műveletet, majd kattintson **részletek** toosee részletek megfelelő tooan műveletet.</span><span class="sxs-lookup"><span data-stu-id="61665-232">In hello operations logs, select any operation and click  **Details** toosee details corresponding tooan operation.</span></span>

    ![Művelet naplók beolvasása részletei](./media/backup-azure-manage-vms/ops-logs-details.png)

    <span data-ttu-id="61665-234">Hello **részletek varázsló** hello művelet működésbe. a feladat azonosítóját, amelyen ez a művelet akkor váltódik ki, és kezdési időpont hello művelet erőforrás kapcsolatos információt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="61665-234">hello **Details wizard** contains information about hello operation triggered, job Id, resource on which this operation is triggered, and start time of hello operation.</span></span>

    ![Művelet részletei](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a><span data-ttu-id="61665-236">Riasztási értesítések</span><span class="sxs-lookup"><span data-stu-id="61665-236">Alert notifications</span></span>
<span data-ttu-id="61665-237">Hello feladatok egyéni riasztási értesítéseket kaphat a portálon.</span><span class="sxs-lookup"><span data-stu-id="61665-237">You can get custom alert notifications for hello jobs in portal.</span></span> <span data-ttu-id="61665-238">PowerShell-alapú riasztási szabályok definiálása a műveleti naplókat események ehhez.</span><span class="sxs-lookup"><span data-stu-id="61665-238">This is achieved by defining PowerShell-based alert rules on operational logs events.</span></span> <span data-ttu-id="61665-239">Azt javasoljuk, *PowerShell 1.3.0 verzió vagy újabb*.</span><span class="sxs-lookup"><span data-stu-id="61665-239">We recommend using *PowerShell version 1.3.0 or above*.</span></span>

<span data-ttu-id="61665-240">a biztonsági mentési hibák az értesítő üzenet egyéni szövegében tooalert toodefine, egy minta parancs fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="61665-240">toodefine a custom notification tooalert for backup failures, a sample command will look like:</span></span>

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

<span data-ttu-id="61665-241">**ResourceId**: letölthető ez Operations Logs előugró fenti szakaszban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="61665-241">**ResourceId**: You can get this from Operations Logs popup as described in above section.</span></span> <span data-ttu-id="61665-242">Egy művelet részletei előugró ablakban ResourceUri hello ResourceId toobe ennek a parancsmagnak megadott.</span><span class="sxs-lookup"><span data-stu-id="61665-242">ResourceUri in details popup window of an operation is hello ResourceId toobe supplied for this cmdlet.</span></span>

<span data-ttu-id="61665-243">**OperationName**: hello formátumban lesz "Microsoft.Backup/backupvault/<EventName>" EventName az egyik regisztrálása, Unregister, ConfigureProtection, biztonsági mentése, visszaállítása, StopProtection, DeleteBackupData, CreateProtectionPolicy, DeleteProtectionPolicy, UpdateProtectionPolicy</span><span class="sxs-lookup"><span data-stu-id="61665-243">**OperationName**: This will be of hello format "Microsoft.Backup/backupvault/<EventName>" where EventName is one of Register,Unregister,ConfigureProtection,Backup,Restore,StopProtection,DeleteBackupData,CreateProtectionPolicy,DeleteProtectionPolicy,UpdateProtectionPolicy</span></span>

<span data-ttu-id="61665-244">**Állapot**: támogatott értékek vannak-elindítva, sikeres és sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="61665-244">**Status**: Supported values are- Started, Succeeded and Failed.</span></span>

<span data-ttu-id="61665-245">**Erőforráscsoport**: erőforráscsoport, amelyen művelet indított hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="61665-245">**ResourceGroup**:ResourceGroup of hello resource on which operation is triggered.</span></span> <span data-ttu-id="61665-246">Ezt úgy szerezheti be ResourceId értékből.</span><span class="sxs-lookup"><span data-stu-id="61665-246">You can obtain this from ResourceId value.</span></span> <span data-ttu-id="61665-247">Érték mező között */resourceGroups/* és */providers/* a ResourceId érték hello a ResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="61665-247">Value between fields */resourceGroups/* and */providers/* in ResourceId value is hello value for ResourceGroup.</span></span>

<span data-ttu-id="61665-248">**Név**: hello riasztási szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="61665-248">**Name**: Name of hello Alert Rule.</span></span>

<span data-ttu-id="61665-249">**CustomEmail**: Adja meg a kívánt toosend riasztási értesítés hello egyéni e-mail cím toowhich</span><span class="sxs-lookup"><span data-stu-id="61665-249">**CustomEmail**: Specify hello custom email address toowhich you want toosend alert notification</span></span>

<span data-ttu-id="61665-250">**SendToServiceOwners**: ezt a beállítást küldi el – riasztási értesítés tooall rendszergazdák és a társadminisztrátorok hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="61665-250">**SendToServiceOwners**: This option sends alert notification tooall administrators and co-administrators of hello subscription.</span></span> <span data-ttu-id="61665-251">A használat **New-AzureRmAlertRuleEmail** parancsmag</span><span class="sxs-lookup"><span data-stu-id="61665-251">It can be used in **New-AzureRmAlertRuleEmail** cmdlet</span></span>

### <a name="limitations-on-alerts"></a><span data-ttu-id="61665-252">Riasztások korlátozásai</span><span class="sxs-lookup"><span data-stu-id="61665-252">Limitations on Alerts</span></span>
<span data-ttu-id="61665-253">Eseményalapú riasztások az éppen vetni toohello a következő korlátozások vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="61665-253">Event-based alerts are subjected toohello following limitations:</span></span>

1. <span data-ttu-id="61665-254">A figyelmeztetéseket hello mentési tároló összes virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="61665-254">Alerts are triggered on all virtual machines in hello backup vault.</span></span> <span data-ttu-id="61665-255">Nem szabhatja testre az tooget riasztásokat az adott virtuális gépek csoportja tartalmazza a biztonságimásolat-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="61665-255">You cannot customize it tooget alerts for specific set of virtual machines in a backup vault.</span></span>
2. <span data-ttu-id="61665-256">A funkció jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="61665-256">This feature is in Preview.</span></span> [<span data-ttu-id="61665-257">További információ</span><span class="sxs-lookup"><span data-stu-id="61665-257">Learn more</span></span>](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. <span data-ttu-id="61665-258">Értesítéseket küld a "alerts-noreply@mail.windowsazure.com".</span><span class="sxs-lookup"><span data-stu-id="61665-258">You will receive alerts from "alerts-noreply@mail.windowsazure.com".</span></span> <span data-ttu-id="61665-259">Hello e-mailt olyasvalaki jelenleg nem módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="61665-259">Currently you can't modify hello email sender.</span></span>

## <a name="next-steps"></a><span data-ttu-id="61665-260">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="61665-260">Next steps</span></span>
* [<span data-ttu-id="61665-261">Állítsa vissza az Azure virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="61665-261">Restore Azure VMs</span></span>](backup-azure-restore-vms.md)
