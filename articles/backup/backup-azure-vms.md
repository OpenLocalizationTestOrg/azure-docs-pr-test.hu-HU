---
title: "mentése az Azure virtual machines klasszikus telepített tooa mentési tároló aaaBack |} Microsoft Docs"
description: "Ismerje meg, és regisztrálja, készítsen biztonsági másolatot a virtuális gépek ezekkel az eljárásokkal az Azure virtuális gépek biztonsági mentéséhez."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "virtuális gép biztonsági mentése; Készítsen biztonsági másolatot a virtuális gép; biztonsági mentés és katasztrófa utáni helyreállítás; virtuális gép biztonsági mentése"
ms.assetid: c0ab5469-65fd-4af5-ae9b-f5d183f82ce8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 048e32d9b2bd5bdd7a125225a71a6d805bb4fbd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a><span data-ttu-id="2aff3-104">Készítsen biztonsági másolatot az Azure virtuális gépek (klasszikus portál)</span><span class="sxs-lookup"><span data-stu-id="2aff3-104">Back up Azure virtual machines (classic portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2aff3-105">Készítsen biztonsági másolatot a virtuális gépek tooRecovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="2aff3-105">Back up VMs tooRecovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="2aff3-106">Készítsen biztonsági másolatot a virtuális gépek tooBackup tároló</span><span class="sxs-lookup"><span data-stu-id="2aff3-106">Back up VMs tooBackup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="2aff3-107">Ez a cikk hello eljárásokat biztosít biztonsági mentése egy klasszikus telepített Azure virtuális gép (VM) tooa Backup-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="2aff3-107">This article provides hello procedures for backing up a Classic-deployed Azure virtual machine (VM) tooa Backup vault.</span></span> <span data-ttu-id="2aff3-108">Nincsenek tootake care, mielőtt készíthet biztonsági másolatot egy Azure virtuális gépen kell néhány feladatot.</span><span class="sxs-lookup"><span data-stu-id="2aff3-108">There are a few tasks you need tootake care of before you can back up an Azure virtual machine.</span></span> <span data-ttu-id="2aff3-109">Ha még nem tette Igen, a teljes hello [Előfeltételek](backup-azure-vms-prepare.md) tooprepare környezetében a virtuális gépek biztonsági mentéséről.</span><span class="sxs-lookup"><span data-stu-id="2aff3-109">If you haven't already done so, complete hello [prerequisites](backup-azure-vms-prepare.md) tooprepare your environment for backing up your VMs.</span></span>

<span data-ttu-id="2aff3-110">További információkért lásd: hello cikkeket a [az Azure virtuális gép biztonsági mentési infrastruktúrájának megtervezésével](backup-azure-vms-introduction.md) és [Azure virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="2aff3-110">For additional information, see hello articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

> [!NOTE]
> <span data-ttu-id="2aff3-111">Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="2aff3-111">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="2aff3-112">A biztonsági másolatok tárolóját csak klasszikus telepített virtuális gépek védelmére.</span><span class="sxs-lookup"><span data-stu-id="2aff3-112">A Backup vault can only protect Classic-deployed VMs.</span></span> <span data-ttu-id="2aff3-113">A biztonsági másolatok tárolóját erőforrás-kezelő telepített virtuális gépek nem védhetők.</span><span class="sxs-lookup"><span data-stu-id="2aff3-113">You cannot protect Resource Manager-deployed VMs with a Backup vault.</span></span> <span data-ttu-id="2aff3-114">Lásd: [tooRecovery Services-tároló virtuális gépek biztonsági mentése](backup-azure-arm-vms.md) talál részletes információt használata a Recovery Services-tárolók.</span><span class="sxs-lookup"><span data-stu-id="2aff3-114">See [Back up VMs tooRecovery Services vault](backup-azure-arm-vms.md) for details on working with Recovery Services vaults.</span></span>
>
>

<span data-ttu-id="2aff3-115">Az Azure virtuális gépek biztonsági mentését három fő lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="2aff3-115">Backing up Azure virtual machines involves three key steps:</span></span>

![Három lépést tooback egy Azure IaaS virtuális gépet](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> <span data-ttu-id="2aff3-117">A virtuális gépek biztonsági mentése egy helyi folyamat.</span><span class="sxs-lookup"><span data-stu-id="2aff3-117">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="2aff3-118">Nem készíthet biztonsági másolatot a virtuális gépek egy régió tooa biztonságimásolat-tárolóban egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="2aff3-118">You cannot back up virtual machines in one region tooa backup vault in another region.</span></span> <span data-ttu-id="2aff3-119">Ezért létre kell hozni egy mentési tárolót minden Azure-régió, ahol a virtuális gép készül biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="2aff3-119">So, you must create a backup vault in each Azure region, where there are VMs that will be backed up.</span></span>
>
> [!IMPORTANT]
> <span data-ttu-id="2aff3-120">2017. március-től kezdődően már nem használható hello klasszikus portál toocreate mentési tárolókban.</span><span class="sxs-lookup"><span data-stu-id="2aff3-120">Starting March 2017, you can no longer use hello classic portal toocreate Backup vaults.</span></span>
> <span data-ttu-id="2aff3-121">Most már frissítheti a mentési tárolók tooRecovery szolgáltatások tárolókban.</span><span class="sxs-lookup"><span data-stu-id="2aff3-121">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="2aff3-122">További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="2aff3-122">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="2aff3-123">A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="2aff3-123">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="2aff3-124">2017. október 15. után PowerShell toocreate mentési tárolókban nem használható.</span><span class="sxs-lookup"><span data-stu-id="2aff3-124">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="2aff3-125">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="2aff3-125">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="2aff3-126">Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="2aff3-126">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="2aff3-127">Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="2aff3-127">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="2aff3-128">Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.</span><span class="sxs-lookup"><span data-stu-id="2aff3-128">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="step-1---discover-azure-virtual-machines"></a><span data-ttu-id="2aff3-129">1. lépés – az Azure virtuális gépek észlelése</span><span class="sxs-lookup"><span data-stu-id="2aff3-129">Step 1 - Discover Azure virtual machines</span></span>
<span data-ttu-id="2aff3-130">bármely új virtuális gépek (VM) hozzáadott toohello előfizetés azonosítják, mielőtt regisztrálná, tooensure hello felderítési folyamatának futtatása során.</span><span class="sxs-lookup"><span data-stu-id="2aff3-130">tooensure any new virtual machines (VMs) added toohello subscription are identified before registering, run hello discovery process.</span></span> <span data-ttu-id="2aff3-131">hello folyamat lekérdezések Azure hello hello az előfizetést, valamint további információkat a virtuális gépek listája például a felhőalapú szolgáltatás- és hello régió hello.</span><span class="sxs-lookup"><span data-stu-id="2aff3-131">hello process queries Azure for hello list of virtual machines in hello subscription, along with additional information like hello cloud service name and hello region.</span></span>

1. <span data-ttu-id="2aff3-132">Jelentkezzen be toohello [klasszikus portál](http://manage.windowsazure.com/)</span><span class="sxs-lookup"><span data-stu-id="2aff3-132">Sign in toohello [Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="2aff3-133">Azure-szolgáltatások hello listájában kattintson **Recovery Services** tárolók biztonsági mentés és helyreállítás tooopen hello listája.</span><span class="sxs-lookup"><span data-stu-id="2aff3-133">In hello list of Azure services, click **Recovery Services** tooopen hello list of Backup and Site Recovery vaults.</span></span>
    <span data-ttu-id="2aff3-134">![Nyissa meg a tároló listája](./media/backup-azure-vms/choose-vault-list.png)</span><span class="sxs-lookup"><span data-stu-id="2aff3-134">![Open vault list](./media/backup-azure-vms/choose-vault-list.png)</span></span>
3. <span data-ttu-id="2aff3-135">Hello mentési tárolókban, jelölje ki hello tároló tooback virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="2aff3-135">In hello list of Backup vaults, select hello vault tooback up a VM.</span></span>

    <span data-ttu-id="2aff3-136">Ha ez egy új tároló hello portál megnyitása toohello **gyors üzembe helyezés** lap.</span><span class="sxs-lookup"><span data-stu-id="2aff3-136">If this is a new vault hello portal opens toohello **Quick Start** page.</span></span>

    ![Regisztrált elemek menü megnyitása](./media/backup-azure-vms/vault-quick-start.png)

    <span data-ttu-id="2aff3-138">Ha hello tároló már be lett állítva, a hello portál legutóbb használt toohello menü megnyitása.</span><span class="sxs-lookup"><span data-stu-id="2aff3-138">If hello vault has previously been configured, hello portal opens toohello most recently used menu.</span></span>
4. <span data-ttu-id="2aff3-139">(Hello hello oldal tetején) hello tároló menüben kattintson **regisztrált elemek**.</span><span class="sxs-lookup"><span data-stu-id="2aff3-139">From hello vault menu (at hello top of hello page), click **Registered Items**.</span></span>

    ![Regisztrált elemek menü megnyitása](./media/backup-azure-vms/vault-menu.png)
5. <span data-ttu-id="2aff3-141">A hello **típus** menü **Azure virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="2aff3-141">From hello **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Számítási feladat kiválasztása](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="2aff3-143">Kattintson a **felderítési** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="2aff3-143">Click **DISCOVER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="2aff3-144">![Felderítés gomb](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="2aff3-144">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="2aff3-145">hello felderítési folyamat eltarthat néhány percig, amíg hello virtuális gépek megjelennének alatt.</span><span class="sxs-lookup"><span data-stu-id="2aff3-145">hello discovery process may take a few minutes while hello virtual machines are being tabulated.</span></span> <span data-ttu-id="2aff3-146">Nincs üdvözlő képernyőt, amely lehetővé teszi, hogy tudja, hogy fut-e hello folyamat hello alján értesítést.</span><span class="sxs-lookup"><span data-stu-id="2aff3-146">There is a notification at hello bottom of hello screen that lets you know that hello process is running.</span></span>

    ![Virtuális gépek felderítése](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="2aff3-148">hello értesítési módosításokat, ha hello folyamat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="2aff3-148">hello notification changes when hello process is complete.</span></span> <span data-ttu-id="2aff3-149">Ha hello felderítési folyamat nem találta meg a virtuális gépek hello, először biztosítson hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="2aff3-149">If hello discovery process did not find hello virtual machines, first ensure hello VMs exist.</span></span> <span data-ttu-id="2aff3-150">Hello virtuális gépek esetén győződjön meg arról, hello virtuális gépek vannak a hello azonos régióban legyen, mint a hello mentési tároló.</span><span class="sxs-lookup"><span data-stu-id="2aff3-150">If hello VMs exist, ensure hello VMs are in hello same region as hello backup vault.</span></span> <span data-ttu-id="2aff3-151">Ha hello virtuális gépek és azok a hello ugyanabban a régióban, győződjön meg arról, hello virtuális gépek még nem regisztrált tooa mentési tároló.</span><span class="sxs-lookup"><span data-stu-id="2aff3-151">If hello VMs exist and are in hello same region, ensure hello VMs are not already registered tooa backup vault.</span></span> <span data-ttu-id="2aff3-152">Ha a virtuális gép hozzárendelt tooa mentési tároló már nem érhető el toobe hozzárendelt tooother mentési tárolók.</span><span class="sxs-lookup"><span data-stu-id="2aff3-152">If a VM is assigned tooa backup vault it is not available toobe assigned tooother backup vaults.</span></span>

    ![A felderítés kész](./media/backup-azure-vms/discovery-complete.png)

    <span data-ttu-id="2aff3-154">Miután a felfedezett hello új elemek tooStep 2 lépjen, és regisztrálja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2aff3-154">Once you have discovered hello new items, go tooStep 2 and register your VMs.</span></span>

## <a name="step-2---register-azure-virtual-machines"></a><span data-ttu-id="2aff3-155">2. lépés – regisztrálása az Azure virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="2aff3-155">Step 2 - Register Azure virtual machines</span></span>
<span data-ttu-id="2aff3-156">Egy Azure virtuális gép tooassociate regisztrálja azt hello Azure Backup szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2aff3-156">You register an Azure virtual machine tooassociate it with hello Azure Backup service.</span></span> <span data-ttu-id="2aff3-157">Ez általában az egyszeri tevékenység.</span><span class="sxs-lookup"><span data-stu-id="2aff3-157">This is typically a one-time activity.</span></span>

1. <span data-ttu-id="2aff3-158">Keresse meg a toohello mentési tároló alatt **Recovery Services** hello Azure-portálon, és kattintson a **regisztrált elemek**.</span><span class="sxs-lookup"><span data-stu-id="2aff3-158">Navigate toohello backup vault under **Recovery Services** in hello Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="2aff3-159">Válassza ki **Azure virtuális gép** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="2aff3-159">Select **Azure Virtual Machine** from hello drop-down menu.</span></span>

    ![Számítási feladat kiválasztása](./media/backup-azure-vms/discovery-select-workload.png)
3. <span data-ttu-id="2aff3-161">Kattintson a **REGISZTRÁLÁSA** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="2aff3-161">Click **REGISTER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="2aff3-162">![Regisztrálás gomb](./media/backup-azure-vms/register-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="2aff3-162">![Register button](./media/backup-azure-vms/register-button-only.png)</span></span>
4. <span data-ttu-id="2aff3-163">A hello **regisztrálni elemek** helyi menü, jelölje be hello virtuális gépek, amelyet az tooregister.</span><span class="sxs-lookup"><span data-stu-id="2aff3-163">In hello **Register Items** shortcut menu, select hello virtual machines that you want tooregister.</span></span> <span data-ttu-id="2aff3-164">Ha a két vagy több virtuális gép hello azonos hello cloud service toodistinguish közöttük használja.</span><span class="sxs-lookup"><span data-stu-id="2aff3-164">If there are two or more virtual machines with hello same name, use hello cloud service toodistinguish between them.</span></span>

   > [!TIP]
   > <span data-ttu-id="2aff3-165">Egyszerre több virtuális gép is regisztrálható.</span><span class="sxs-lookup"><span data-stu-id="2aff3-165">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="2aff3-166">Létrejön egy feladat minden egyes kiválasztott virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="2aff3-166">A job is created for each virtual machine that you've selected.</span></span>
5. <span data-ttu-id="2aff3-167">Kattintson a **feladat megtekintése** a hello értesítési toogo toohello **feladatok** lap.</span><span class="sxs-lookup"><span data-stu-id="2aff3-167">Click **View Job** in hello notification toogo toohello **Jobs** page.</span></span>

    ![Regisztrációs feladat](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="2aff3-169">hello virtuális gép is regisztrált elemek mellett hello regisztrációs művelet hello állapotának hello listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2aff3-169">hello virtual machine also appears in hello list of registered items, along with hello status of hello registration operation.</span></span>

    ![1. regisztrációs állapot](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="2aff3-171">Hello művelet befejeződésekor hello állapota tooreflect hello *regisztrált* állapotát.</span><span class="sxs-lookup"><span data-stu-id="2aff3-171">When hello operation completes, hello status changes tooreflect hello *registered* state.</span></span>

    ![2. regisztrációs állapot](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a><span data-ttu-id="2aff3-173">3. lépés - az Azure virtuális gépek védelme</span><span class="sxs-lookup"><span data-stu-id="2aff3-173">Step 3 - Protect Azure virtual machines</span></span>
<span data-ttu-id="2aff3-174">Most állíthat be egy biztonsági mentési és adatmegőrzési házirend hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="2aff3-174">Now you can set up a backup and retention policy for hello virtual machine.</span></span> <span data-ttu-id="2aff3-175">Több virtuális gépek védhetők egyetlen művelet védelme.</span><span class="sxs-lookup"><span data-stu-id="2aff3-175">Multiple virtual machines can be protected by using a single protect action.</span></span>

<span data-ttu-id="2aff3-176">Az Azure mentési tárolókban 2015. május kapható után létrehozott alapértelmezett házirend hello tároló beépített.</span><span class="sxs-lookup"><span data-stu-id="2aff3-176">Azure Backup vaults created after May 2015 come with a default policy built into hello vault.</span></span> <span data-ttu-id="2aff3-177">Ez az alapértelmezett házirend tartalmaz egy alapértelmezett megőrzési 30 nap és a biztonsági mentési ütemezés szerint naponta egyszer.</span><span class="sxs-lookup"><span data-stu-id="2aff3-177">This default policy comes with a default retention of 30 days and a once-daily backup schedule.</span></span>

1. <span data-ttu-id="2aff3-178">Keresse meg a toohello mentési tároló alatt **Recovery Services** hello Azure-portálon, és kattintson a **regisztrált elemek**.</span><span class="sxs-lookup"><span data-stu-id="2aff3-178">Navigate toohello backup vault under **Recovery Services** in hello Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="2aff3-179">Válassza ki **Azure virtuális gép** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="2aff3-179">Select **Azure Virtual Machine** from hello drop-down menu.</span></span>

    ![Számítási feladat kiválasztása a portálon](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="2aff3-181">Kattintson a **védelme** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="2aff3-181">Click **PROTECT** at hello bottom of hello page.</span></span>

    <span data-ttu-id="2aff3-182">Hello **elemek védelme varázsló** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2aff3-182">hello **Protect Items wizard** appears.</span></span> <span data-ttu-id="2aff3-183">hello varázsló csak a regisztrált és nem védett virtuális gépek sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="2aff3-183">hello wizard only lists virtual machines that are registered and not protected.</span></span> <span data-ttu-id="2aff3-184">Válassza ki a megjeleníteni kívánt tooprotect hello virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2aff3-184">Select hello virtual machines that you want tooprotect.</span></span>

    <span data-ttu-id="2aff3-185">Ha a két vagy több virtuális gép hello azonos hello cloud service toodistinguish hello virtuális gépek közötti használja.</span><span class="sxs-lookup"><span data-stu-id="2aff3-185">If there are two or more virtual machines with hello same name, use hello cloud service toodistinguish between hello virtual machines.</span></span>

   > [!TIP]
   > <span data-ttu-id="2aff3-186">Egyszerre több virtuális gép védhet.</span><span class="sxs-lookup"><span data-stu-id="2aff3-186">You can protect multiple virtual machines at one time.</span></span>
   >
   >

    ![Méretezett védelem konfigurálása](./media/backup-azure-vms/protect-at-scale.png)

4. <span data-ttu-id="2aff3-188">Válasszon egy **biztonsági mentés ütemezése** tooback kijelölt hello virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2aff3-188">Choose a **backup schedule** tooback up hello virtual machines that you've selected.</span></span> <span data-ttu-id="2aff3-189">Válasszon egy meglévő házirendcsoport címet, vagy új megadása.</span><span class="sxs-lookup"><span data-stu-id="2aff3-189">You can pick from an existing set of policies or define a new one.</span></span>

    <span data-ttu-id="2aff3-190">Minden biztonsági mentési házirendhez több virtuális gép társítható.</span><span class="sxs-lookup"><span data-stu-id="2aff3-190">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="2aff3-191">Azonban hello virtuális gép csak társítható egy házirend álljon időben.</span><span class="sxs-lookup"><span data-stu-id="2aff3-191">However, hello virtual machine can only be associated with one policy at any given point in time.</span></span>

    ![Védelem új házirenddel](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="2aff3-193">A biztonsági mentési házirend hello ütemezett biztonsági mentések megőrzési rendszert tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="2aff3-193">A backup policy includes a retention scheme for hello scheduled backups.</span></span> <span data-ttu-id="2aff3-194">Ha egy meglévő biztonsági mentési házirend, hello adatmegőrzési beállítások hello következő lépésben nem módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="2aff3-194">If you select an existing backup policy, you cannot modify hello retention options in hello next step.</span></span>
   >
   >

5. <span data-ttu-id="2aff3-195">Válasszon egy **megőrzési időtartam** hello biztonsági tooassociate.</span><span class="sxs-lookup"><span data-stu-id="2aff3-195">Choose a **retention range** tooassociate with hello backups.</span></span>

    ![Rugalmas megőrzést védelme](./media/backup-azure-vms/policy-retention.png)

    <span data-ttu-id="2aff3-197">Adatmegőrzési idő másolatának tárolására szolgáló hello hosszát adja meg.</span><span class="sxs-lookup"><span data-stu-id="2aff3-197">Retention policy specifies hello length of time for storing a backup.</span></span> <span data-ttu-id="2aff3-198">Az eltérő megőrzési házirendek hello biztonsági mentés időpontjában alapján is megadhat.</span><span class="sxs-lookup"><span data-stu-id="2aff3-198">You can specify different retention policies based on when hello backup is taken.</span></span> <span data-ttu-id="2aff3-199">Például egy biztonsági mentési pont végrehajtott naponta (ami működési helyreállítási pontjaként szolgál) lehet, hogy megőrzi a 90 napig.</span><span class="sxs-lookup"><span data-stu-id="2aff3-199">For example, a backup point taken daily (which serves as an operational recovery point) might be preserved for 90 days.</span></span> <span data-ttu-id="2aff3-200">Szemben egy biztonsági mentési pont (naplózási célokra) negyedévenként hello végén végrehajtott esetleg toobe sok hónapokban vagy években maradnak.</span><span class="sxs-lookup"><span data-stu-id="2aff3-200">In comparison, a backup point taken at hello end of each quarter (for audit purposes) may need toobe preserved for many months or years.</span></span>

    ![A virtuális gép helyreállítási ponttal van mentve](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="2aff3-202">A példa képen:</span><span class="sxs-lookup"><span data-stu-id="2aff3-202">In this example image:</span></span>

   * <span data-ttu-id="2aff3-203">**Napi adatmegőrzési**: naponta készített biztonsági másolatok 30 napig tárolja.</span><span class="sxs-lookup"><span data-stu-id="2aff3-203">**Daily retention policy**: Backups taken daily are stored for 30 days.</span></span>
   * <span data-ttu-id="2aff3-204">**Heti adatmegőrzési**: 104 hétig minden héten vasárnap készített biztonsági másolatok megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="2aff3-204">**Weekly retention policy**: Backups taken every week on Sunday are preserved for 104 weeks.</span></span>
   * <span data-ttu-id="2aff3-205">**Havi adatmegőrzési**: 120 havi biztonsági másolatokat hello minden hónap utolsó vasárnap megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="2aff3-205">**Monthly retention policy**: Backups taken on hello last Sunday of each month are preserved for 120 months.</span></span>
   * <span data-ttu-id="2aff3-206">**Éves adatmegőrzési**: biztonsági másolatokat hello minden január első vasárnap 99 évig megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="2aff3-206">**Yearly retention policy**: Backups taken on hello first Sunday of every January are preserved for 99 years.</span></span>

     <span data-ttu-id="2aff3-207">Egy feladat jön létre tooconfigure hello védelmi házirendje, és társítsa hello virtuális gépek toothat házirendet, amely a kijelölt virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="2aff3-207">A job is created tooconfigure hello protection policy and associate hello virtual machines toothat policy for each virtual machine that you've selected.</span></span>
6. <span data-ttu-id="2aff3-208">tooview hello listája **Védelemkonfigurálási** feladatok hello tárolók menüben kattintson **feladatok** válassza **Védelemkonfigurálási** a hello **művelet**  szűrő.</span><span class="sxs-lookup"><span data-stu-id="2aff3-208">tooview hello list of **Configure Protection** jobs, from hello vaults menu, click **Jobs** and select **Configure Protection** from hello **Operation** filter.</span></span>

    ![Védelemkonfigurálási feladat](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a><span data-ttu-id="2aff3-210">Kezdeti biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="2aff3-210">Initial backup</span></span>
<span data-ttu-id="2aff3-211">Miután hello virtuális gép védett házirendnek, azt jeleníti meg a hello **védett elemek** hello az állapota lapon *védett - (függőben lévő kezdeti biztonsági másolatot)*.</span><span class="sxs-lookup"><span data-stu-id="2aff3-211">Once hello virtual machine is protected with a policy, it shows up under hello **Protected Items** tab with hello status of *Protected - (pending initial backup)*.</span></span> <span data-ttu-id="2aff3-212">Alapértelmezés szerint hello első ütemezett biztonsági mentés hello *kezdeti biztonsági másolatot*.</span><span class="sxs-lookup"><span data-stu-id="2aff3-212">By default, hello first scheduled backup is hello *initial backup*.</span></span>

<span data-ttu-id="2aff3-213">tootrigger hello kezdeti biztonsági mentés közvetlenül a védelem beállítása után:</span><span class="sxs-lookup"><span data-stu-id="2aff3-213">tootrigger hello initial backup immediately after configuring protection:</span></span>

1. <span data-ttu-id="2aff3-214">Hello hello alján **védett elemek** kattintson **biztonsági mentés most**.</span><span class="sxs-lookup"><span data-stu-id="2aff3-214">At hello bottom of hello **Protected Items** page, click **Backup Now**.</span></span>

    <span data-ttu-id="2aff3-215">hello Azure Backup szolgáltatás létrehoz egy biztonsági mentési feladatot az hello kezdeti biztonsági mentési műveletet.</span><span class="sxs-lookup"><span data-stu-id="2aff3-215">hello Azure Backup service creates a backup job for hello initial backup operation.</span></span>
2. <span data-ttu-id="2aff3-216">Kattintson a hello **feladatok** lapon tooview hello feladatok listája.</span><span class="sxs-lookup"><span data-stu-id="2aff3-216">Click hello **Jobs** tab tooview hello list of jobs.</span></span>

    ![Biztonsági mentés folyamatban](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> <span data-ttu-id="2aff3-218">Hello biztonsági mentési művelet során hello Azure Backup szolgáltatás kibocsát egy parancs toohello biztonsági mentési feladatok írási hibák, és egységes pillanatképet készít minden virtuális gép tooflush bővítményt.</span><span class="sxs-lookup"><span data-stu-id="2aff3-218">During hello backup operation, hello Azure Backup service issues a command toohello backup extension in each virtual machine tooflush all write jobs and take a consistent snapshot.</span></span>
>
>

<span data-ttu-id="2aff3-219">Ha hello kezdeti biztonsági mentés befejezése után hello állapot hello hello virtuális gép **védett elemek** lap *védett*.</span><span class="sxs-lookup"><span data-stu-id="2aff3-219">When hello initial backup finishes, hello status of hello virtual machine in hello **Protected Items** tab is *Protected*.</span></span>

![A virtuális gép helyreállítási ponttal van mentve](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a><span data-ttu-id="2aff3-221">Biztonsági mentés állapotának és a részletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="2aff3-221">Viewing backup status and details</span></span>
<span data-ttu-id="2aff3-222">Ha védett, hello virtuális gépek számát is növekszik hello **irányítópult** összefoglaló lap.</span><span class="sxs-lookup"><span data-stu-id="2aff3-222">Once protected, hello virtual machine count also increases in hello **Dashboard** page summary.</span></span> <span data-ttu-id="2aff3-223">Hello **irányítópult** lapon a feladatok az elmúlt 24 órában, melyeket hello hello száma is látható *sikeres*, rendelkezik *sikertelen*, és a *folyamatban*.</span><span class="sxs-lookup"><span data-stu-id="2aff3-223">hello **Dashboard** page also shows hello number of jobs from hello last 24 hours that were *successful*, have *failed*, and are *in progress*.</span></span> <span data-ttu-id="2aff3-224">A hello **feladatok** lapján hello használata **állapot**, **művelet**, vagy **a** és **való** menük toofilter hello feladatok.</span><span class="sxs-lookup"><span data-stu-id="2aff3-224">On hello **Jobs** page, use hello **Status**, **Operation**, or **From** and **To** menus toofilter hello jobs.</span></span>

![Biztonsági mentés a irányítópult-oldalon állapota](./media/backup-azure-vms/dashboard-protectedvms.png)

<span data-ttu-id="2aff3-226">Hello irányítópult értékeket 24 óránként egyszer frissülnek.</span><span class="sxs-lookup"><span data-stu-id="2aff3-226">Values in hello dashboard are refreshed once every 24 hours.</span></span>

## <a name="troubleshooting-errors"></a><span data-ttu-id="2aff3-227">Kapcsolatos hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="2aff3-227">Troubleshooting errors</span></span>
<span data-ttu-id="2aff3-228">Ha biztonsági során problémákat tapasztal a virtuális gép, nézze meg hello [VM hibaelhárítási cikke](backup-azure-vms-troubleshoot.md) segítségét.</span><span class="sxs-lookup"><span data-stu-id="2aff3-228">If you run into issues while backing up your virtual machine, look at hello [VM     troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2aff3-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2aff3-229">Next steps</span></span>
* [<span data-ttu-id="2aff3-230">A virtuális gépek kezelése és figyelése</span><span class="sxs-lookup"><span data-stu-id="2aff3-230">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="2aff3-231">Virtuális gépek visszaállítása</span><span class="sxs-lookup"><span data-stu-id="2aff3-231">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
