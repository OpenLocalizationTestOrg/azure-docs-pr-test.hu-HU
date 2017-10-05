---
title: "Klasszikus telepített Azure virtuális gépek biztonsági mentése a mentési tárolóban |} Microsoft Docs"
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
ms.openlocfilehash: e1da8bce96078a43c656f84005cefc8bbe81c9e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a><span data-ttu-id="7f143-104">Készítsen biztonsági másolatot az Azure virtuális gépek (klasszikus portál)</span><span class="sxs-lookup"><span data-stu-id="7f143-104">Back up Azure virtual machines (classic portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7f143-105">Recovery Services-tároló virtuális gépek mentésére</span><span class="sxs-lookup"><span data-stu-id="7f143-105">Back up VMs to Recovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="7f143-106">Biztonsági másolatot a virtuális gépek mentési tárolóba</span><span class="sxs-lookup"><span data-stu-id="7f143-106">Back up VMs to Backup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="7f143-107">Ez a cikk a eljárásokat biztosít a biztonsági másolatot a klasszikus üzembe az Azure virtuális gép (VM) a biztonsági másolatok tárolóját.</span><span class="sxs-lookup"><span data-stu-id="7f143-107">This article provides the procedures for backing up a Classic-deployed Azure virtual machine (VM) to a Backup vault.</span></span> <span data-ttu-id="7f143-108">Nincsenek néhány feladatot kell gondoskodunk előtt készíthet biztonsági másolatot egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="7f143-108">There are a few tasks you need to take care of before you can back up an Azure virtual machine.</span></span> <span data-ttu-id="7f143-109">Ha még nem tette meg, végezze el a [Előfeltételek](backup-azure-vms-prepare.md) készítse fel a környezetet a virtuális gépek biztonsági mentéséről.</span><span class="sxs-lookup"><span data-stu-id="7f143-109">If you haven't already done so, complete the [prerequisites](backup-azure-vms-prepare.md) to prepare your environment for backing up your VMs.</span></span>

<span data-ttu-id="7f143-110">További információkért tekintse meg a cikkek [az Azure virtuális gép biztonsági mentési infrastruktúrájának megtervezésével](backup-azure-vms-introduction.md) és [Azure virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="7f143-110">For additional information, see the articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

> [!NOTE]
> <span data-ttu-id="7f143-111">Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7f143-111">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="7f143-112">A biztonsági másolatok tárolóját csak klasszikus telepített virtuális gépek védelmére.</span><span class="sxs-lookup"><span data-stu-id="7f143-112">A Backup vault can only protect Classic-deployed VMs.</span></span> <span data-ttu-id="7f143-113">A biztonsági másolatok tárolóját erőforrás-kezelő telepített virtuális gépek nem védhetők.</span><span class="sxs-lookup"><span data-stu-id="7f143-113">You cannot protect Resource Manager-deployed VMs with a Backup vault.</span></span> <span data-ttu-id="7f143-114">Lásd: [készítsen biztonsági mentést a Recovery Services-tároló virtuális gépek](backup-azure-arm-vms.md) talál részletes információt használata a Recovery Services-tárolók.</span><span class="sxs-lookup"><span data-stu-id="7f143-114">See [Back up VMs to Recovery Services vault](backup-azure-arm-vms.md) for details on working with Recovery Services vaults.</span></span>
>
>

<span data-ttu-id="7f143-115">Az Azure virtuális gépek biztonsági mentését három fő lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="7f143-115">Backing up Azure virtual machines involves three key steps:</span></span>

![Készítsen biztonsági másolatot az Azure infrastruktúra-szolgáltatási virtuális gép három lépésben](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> <span data-ttu-id="7f143-117">A virtuális gépek biztonsági mentése egy helyi folyamat.</span><span class="sxs-lookup"><span data-stu-id="7f143-117">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="7f143-118">Nem készíthet biztonsági másolatot egyetlen virtuális gépeket egy másik régióban egy biztonsági mentési tárolóba.</span><span class="sxs-lookup"><span data-stu-id="7f143-118">You cannot back up virtual machines in one region to a backup vault in another region.</span></span> <span data-ttu-id="7f143-119">Ezért létre kell hozni egy mentési tárolót minden Azure-régió, ahol a virtuális gép készül biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="7f143-119">So, you must create a backup vault in each Azure region, where there are VMs that will be backed up.</span></span>
>
> [!IMPORTANT]
> <span data-ttu-id="7f143-120">2017 márciusától már nem hozhat létre Backup-tárolókat a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="7f143-120">Starting March 2017, you can no longer use the classic portal to create Backup vaults.</span></span>
> <span data-ttu-id="7f143-121">A biztonsági mentési tárakról mostantól lehetőség van helyreállítási tárakra váltani.</span><span class="sxs-lookup"><span data-stu-id="7f143-121">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="7f143-122">A részletekről bővebben az [Váltás biztonsági mentési tárolóról Recovery Services-tárolóra](backup-azure-upgrade-backup-to-recovery-services.md) című cikkben olvashat.</span><span class="sxs-lookup"><span data-stu-id="7f143-122">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="7f143-123">A Microsoft azt javasolja, hogy a biztonsági mentési tárolóról váltson Recovery Services-tárolóra.</span><span class="sxs-lookup"><span data-stu-id="7f143-123">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="7f143-124">2017. október 15-től a PowerShell nem használható Backup-tárolók létrehozására.</span><span class="sxs-lookup"><span data-stu-id="7f143-124">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="7f143-125">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="7f143-125">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="7f143-126">Minden fennmaradó Backup-tároló automatikusan Recovery Services-tárolóra frissül.</span><span class="sxs-lookup"><span data-stu-id="7f143-126">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="7f143-127">A klasszikus portálon nem lehet majd hozzáférni a biztonsági másolati adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="7f143-127">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="7f143-128">Helyette az Azure Portal segítségével férhet hozzá a Recovery Services-tárolókban található biztonsági mentési adatokhoz.</span><span class="sxs-lookup"><span data-stu-id="7f143-128">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="step-1---discover-azure-virtual-machines"></a><span data-ttu-id="7f143-129">1. lépés – az Azure virtuális gépek észlelése</span><span class="sxs-lookup"><span data-stu-id="7f143-129">Step 1 - Discover Azure virtual machines</span></span>
<span data-ttu-id="7f143-130">Győződjön meg arról, mielőtt regisztrálná azonosított bármely új virtuális gépek (VM) az előfizetéshez történő hozzáadása, futtassa a felderítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="7f143-130">To ensure any new virtual machines (VMs) added to the subscription are identified before registering, run the discovery process.</span></span> <span data-ttu-id="7f143-131">A folyamat lekéri az Azure-ból az előfizetésben található virtuális gépek listáját, olyan kiegészítő információkkal, mint a felhőszolgáltatás neve és a régió.</span><span class="sxs-lookup"><span data-stu-id="7f143-131">The process queries Azure for the list of virtual machines in the subscription, along with additional information like the cloud service name and the region.</span></span>

1. <span data-ttu-id="7f143-132">Jelentkezzen be a [klasszikus portál](http://manage.windowsazure.com/)</span><span class="sxs-lookup"><span data-stu-id="7f143-132">Sign in to the [Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="7f143-133">Azure-szolgáltatáshoz, kattintson a **Recovery Services** tárolók biztonsági mentés és helyreállítás listájának megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="7f143-133">In the list of Azure services, click **Recovery Services** to open the list of Backup and Site Recovery vaults.</span></span>
    <span data-ttu-id="7f143-134">![Nyissa meg a tároló listája](./media/backup-azure-vms/choose-vault-list.png)</span><span class="sxs-lookup"><span data-stu-id="7f143-134">![Open vault list](./media/backup-azure-vms/choose-vault-list.png)</span></span>
3. <span data-ttu-id="7f143-135">A mentési tárolókban, jelölje ki a tároló biztonsági mentéséhez egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="7f143-135">In the list of Backup vaults, select the vault to back up a VM.</span></span>

    <span data-ttu-id="7f143-136">Ha ez egy új tárolót a portál megnyitja a **gyors üzembe helyezés** lap.</span><span class="sxs-lookup"><span data-stu-id="7f143-136">If this is a new vault the portal opens to the **Quick Start** page.</span></span>

    ![Regisztrált elemek menü megnyitása](./media/backup-azure-vms/vault-quick-start.png)

    <span data-ttu-id="7f143-138">Ha a tároló már be lett állítva, a portál megnyitja a legutóbb használt menübe.</span><span class="sxs-lookup"><span data-stu-id="7f143-138">If the vault has previously been configured, the portal opens to the most recently used menu.</span></span>
4. <span data-ttu-id="7f143-139">A tároló menüben (a lap tetején), kattintson az **regisztrált elemek**.</span><span class="sxs-lookup"><span data-stu-id="7f143-139">From the vault menu (at the top of the page), click **Registered Items**.</span></span>

    ![Regisztrált elemek menü megnyitása](./media/backup-azure-vms/vault-menu.png)
5. <span data-ttu-id="7f143-141">A **Típus** menüből válassza az **Azure virtuális gép** elemet.</span><span class="sxs-lookup"><span data-stu-id="7f143-141">From the **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Számítási feladat kiválasztása](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="7f143-143">Kattintson a lap alján található **FELDERÍTÉS** gombra.</span><span class="sxs-lookup"><span data-stu-id="7f143-143">Click **DISCOVER** at the bottom of the page.</span></span>
    <span data-ttu-id="7f143-144">![Felderítés gomb](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="7f143-144">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="7f143-145">A felderítési folyamat eltarthat pár percig, amíg a virtuális gépeket táblázatba rendezi a rendszer.</span><span class="sxs-lookup"><span data-stu-id="7f143-145">The discovery process may take a few minutes while the virtual machines are being tabulated.</span></span> <span data-ttu-id="7f143-146">A képernyő alján lévő értesítés jelzi, hogy a folyamat fut.</span><span class="sxs-lookup"><span data-stu-id="7f143-146">There is a notification at the bottom of the screen that lets you know that the process is running.</span></span>

    ![Virtuális gépek felderítése](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="7f143-148">Az értesítés módosul, amikor a folyamat befejeződött.</span><span class="sxs-lookup"><span data-stu-id="7f143-148">The notification changes when the process is complete.</span></span> <span data-ttu-id="7f143-149">Ha a felderítési folyamat nem találta meg a virtuális gépek, először biztosítson a virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="7f143-149">If the discovery process did not find the virtual machines, first ensure the VMs exist.</span></span> <span data-ttu-id="7f143-150">A virtuális gépek esetén ellenőrizze a virtuális gépek és a biztonsági mentési tárolónak ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="7f143-150">If the VMs exist, ensure the VMs are in the same region as the backup vault.</span></span> <span data-ttu-id="7f143-151">Ha a virtuális gépek létezik, és ugyanabban a régióban van, győződjön meg arról, a virtuális gépek nem már regisztrálva van a mentési tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7f143-151">If the VMs exist and are in the same region, ensure the VMs are not already registered to a backup vault.</span></span> <span data-ttu-id="7f143-152">Ha egy virtuális Géphez van rendelve egy mentési tárolót, hozzá kell rendelni a többi biztonsági mentési tárolók nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="7f143-152">If a VM is assigned to a backup vault it is not available to be assigned to other backup vaults.</span></span>

    ![A felderítés kész](./media/backup-azure-vms/discovery-complete.png)

    <span data-ttu-id="7f143-154">A felfedezett az új elemek után folytassa a 2, és regisztrálja a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="7f143-154">Once you have discovered the new items, go to Step 2 and register your VMs.</span></span>

## <a name="step-2---register-azure-virtual-machines"></a><span data-ttu-id="7f143-155">2. lépés – regisztrálása az Azure virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="7f143-155">Step 2 - Register Azure virtual machines</span></span>
<span data-ttu-id="7f143-156">Rögzítheti az Azure Backup szolgáltatás társítsa Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="7f143-156">You register an Azure virtual machine to associate it with the Azure Backup service.</span></span> <span data-ttu-id="7f143-157">Ez általában az egyszeri tevékenység.</span><span class="sxs-lookup"><span data-stu-id="7f143-157">This is typically a one-time activity.</span></span>

1. <span data-ttu-id="7f143-158">Lépjen a mentési tároló alatt **Recovery Services** az Azure-portálon, és kattintson a **regisztrált elemek**.</span><span class="sxs-lookup"><span data-stu-id="7f143-158">Navigate to the backup vault under **Recovery Services** in the Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="7f143-159">Válassza az **Azure virtuális gép** lehetőséget a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="7f143-159">Select **Azure Virtual Machine** from the drop-down menu.</span></span>

    ![Számítási feladat kiválasztása](./media/backup-azure-vms/discovery-select-workload.png)
3. <span data-ttu-id="7f143-161">Kattintson a lap alján lévő **REGISZTRÁLÁS** gombra.</span><span class="sxs-lookup"><span data-stu-id="7f143-161">Click **REGISTER** at the bottom of the page.</span></span>
    <span data-ttu-id="7f143-162">![Regisztrálás gomb](./media/backup-azure-vms/register-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="7f143-162">![Register button](./media/backup-azure-vms/register-button-only.png)</span></span>
4. <span data-ttu-id="7f143-163">Az **Elemek regisztrálása** helyi menüben válassza ki azokat a virtuális gépeket, amelyeket regisztrálni szeretne.</span><span class="sxs-lookup"><span data-stu-id="7f143-163">In the **Register Items** shortcut menu, select the virtual machines that you want to register.</span></span> <span data-ttu-id="7f143-164">Ha két vagy több virtuális gépek ugyanazzal a névvel, a felhőalapú szolgáltatás segítségével megkülönböztetését.</span><span class="sxs-lookup"><span data-stu-id="7f143-164">If there are two or more virtual machines with the same name, use the cloud service to distinguish between them.</span></span>

   > [!TIP]
   > <span data-ttu-id="7f143-165">Egyszerre több virtuális gép is regisztrálható.</span><span class="sxs-lookup"><span data-stu-id="7f143-165">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="7f143-166">Létrejön egy feladat minden egyes kiválasztott virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="7f143-166">A job is created for each virtual machine that you've selected.</span></span>
5. <span data-ttu-id="7f143-167">Kattintson a **Feladat megtekintése** gombra az értesítésben a **Feladatok** lap megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="7f143-167">Click **View Job** in the notification to go to the **Jobs** page.</span></span>

    ![Regisztrációs feladat](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="7f143-169">A virtuális gép a regisztrált elemek listájában is megjelenik a regisztrációs művelet állapotával együtt.</span><span class="sxs-lookup"><span data-stu-id="7f143-169">The virtual machine also appears in the list of registered items, along with the status of the registration operation.</span></span>

    ![1. regisztrációs állapot](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="7f143-171">A művelet befejezése után az állapot módosul, hogy a *regisztrált* állapotot jelezze.</span><span class="sxs-lookup"><span data-stu-id="7f143-171">When the operation completes, the status changes to reflect the *registered* state.</span></span>

    ![2. regisztrációs állapot](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a><span data-ttu-id="7f143-173">3. lépés - az Azure virtuális gépek védelme</span><span class="sxs-lookup"><span data-stu-id="7f143-173">Step 3 - Protect Azure virtual machines</span></span>
<span data-ttu-id="7f143-174">Most a virtuális gép egy biztonsági mentési és adatmegőrzési házirend állíthat be.</span><span class="sxs-lookup"><span data-stu-id="7f143-174">Now you can set up a backup and retention policy for the virtual machine.</span></span> <span data-ttu-id="7f143-175">Több virtuális gépek védhetők egyetlen művelet védelme.</span><span class="sxs-lookup"><span data-stu-id="7f143-175">Multiple virtual machines can be protected by using a single protect action.</span></span>

<span data-ttu-id="7f143-176">A tároló beépített alapértelmezett házirend létrehozása után a 2015. május kapható Azure mentési tárolókban.</span><span class="sxs-lookup"><span data-stu-id="7f143-176">Azure Backup vaults created after May 2015 come with a default policy built into the vault.</span></span> <span data-ttu-id="7f143-177">Ez az alapértelmezett házirend tartalmaz egy alapértelmezett megőrzési 30 nap és a biztonsági mentési ütemezés szerint naponta egyszer.</span><span class="sxs-lookup"><span data-stu-id="7f143-177">This default policy comes with a default retention of 30 days and a once-daily backup schedule.</span></span>

1. <span data-ttu-id="7f143-178">Lépjen a mentési tároló alatt **Recovery Services** az Azure-portálon, és kattintson a **regisztrált elemek**.</span><span class="sxs-lookup"><span data-stu-id="7f143-178">Navigate to the backup vault under **Recovery Services** in the Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="7f143-179">Válassza az **Azure virtuális gép** lehetőséget a legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="7f143-179">Select **Azure Virtual Machine** from the drop-down menu.</span></span>

    ![Számítási feladat kiválasztása a portálon](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="7f143-181">Kattintson a lap alján lévő **VÉDELEM** gombra.</span><span class="sxs-lookup"><span data-stu-id="7f143-181">Click **PROTECT** at the bottom of the page.</span></span>

    <span data-ttu-id="7f143-182">A **elemek védelme varázsló** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7f143-182">The **Protect Items wizard** appears.</span></span> <span data-ttu-id="7f143-183">A varázsló csak a regisztrált és nem védett virtuális gépek sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="7f143-183">The wizard only lists virtual machines that are registered and not protected.</span></span> <span data-ttu-id="7f143-184">Válassza ki a védeni kívánt virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="7f143-184">Select the virtual machines that you want to protect.</span></span>

    <span data-ttu-id="7f143-185">Ha két vagy több virtuális gépek ugyanazzal a névvel, a felhőalapú szolgáltatás segítségével különböztetheti meg egymástól a virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="7f143-185">If there are two or more virtual machines with the same name, use the cloud service to distinguish between the virtual machines.</span></span>

   > [!TIP]
   > <span data-ttu-id="7f143-186">Egyszerre több virtuális gép védhet.</span><span class="sxs-lookup"><span data-stu-id="7f143-186">You can protect multiple virtual machines at one time.</span></span>
   >
   >

    ![Méretezett védelem konfigurálása](./media/backup-azure-vms/protect-at-scale.png)

4. <span data-ttu-id="7f143-188">Válasszon egy **biztonsági mentés ütemezése** biztonsági másolatot készíteni a kijelölt virtuális gépek számára.</span><span class="sxs-lookup"><span data-stu-id="7f143-188">Choose a **backup schedule** to back up the virtual machines that you've selected.</span></span> <span data-ttu-id="7f143-189">Válasszon egy meglévő házirendcsoport címet, vagy új megadása.</span><span class="sxs-lookup"><span data-stu-id="7f143-189">You can pick from an existing set of policies or define a new one.</span></span>

    <span data-ttu-id="7f143-190">Minden biztonsági mentési házirendhez több virtuális gép társítható.</span><span class="sxs-lookup"><span data-stu-id="7f143-190">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="7f143-191">Azonban a virtuális gép csak társítható egy házirend álljon időben.</span><span class="sxs-lookup"><span data-stu-id="7f143-191">However, the virtual machine can only be associated with one policy at any given point in time.</span></span>

    ![Védelem új házirenddel](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="7f143-193">A biztonsági mentési házirendekben található egy megőrzési séma az ütemezett biztonsági mentésekhez.</span><span class="sxs-lookup"><span data-stu-id="7f143-193">A backup policy includes a retention scheme for the scheduled backups.</span></span> <span data-ttu-id="7f143-194">Ha egy meglévő biztonsági mentési házirend, nem módosítható a megőrzési lehetőségek a következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="7f143-194">If you select an existing backup policy, you cannot modify the retention options in the next step.</span></span>
   >
   >

5. <span data-ttu-id="7f143-195">Válasszon egy **megőrzési időtartam** rendelje hozzá a biztonsági mentés.</span><span class="sxs-lookup"><span data-stu-id="7f143-195">Choose a **retention range** to associate with the backups.</span></span>

    ![Rugalmas megőrzést védelme](./media/backup-azure-vms/policy-retention.png)

    <span data-ttu-id="7f143-197">A megőrzési házirend határozza meg a biztonsági másolatok tárolásának hosszát.</span><span class="sxs-lookup"><span data-stu-id="7f143-197">Retention policy specifies the length of time for storing a backup.</span></span> <span data-ttu-id="7f143-198">Különböző megőrzési házirendeket határozhat meg a biztonsági másolat készítésének ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="7f143-198">You can specify different retention policies based on when the backup is taken.</span></span> <span data-ttu-id="7f143-199">Például egy biztonsági mentési pont végrehajtott naponta (ami működési helyreállítási pontjaként szolgál) lehet, hogy megőrzi a 90 napig.</span><span class="sxs-lookup"><span data-stu-id="7f143-199">For example, a backup point taken daily (which serves as an operational recovery point) might be preserved for 90 days.</span></span> <span data-ttu-id="7f143-200">Összehasonlításképpen egy biztonsági mentési pont (naplózási célokra) minden negyedév végén kell sok hónap vagy év megőrzi.</span><span class="sxs-lookup"><span data-stu-id="7f143-200">In comparison, a backup point taken at the end of each quarter (for audit purposes) may need to be preserved for many months or years.</span></span>

    ![A virtuális gép helyreállítási ponttal van mentve](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="7f143-202">A példa képen:</span><span class="sxs-lookup"><span data-stu-id="7f143-202">In this example image:</span></span>

   * <span data-ttu-id="7f143-203">**Napi adatmegőrzési**: naponta készített biztonsági másolatok 30 napig tárolja.</span><span class="sxs-lookup"><span data-stu-id="7f143-203">**Daily retention policy**: Backups taken daily are stored for 30 days.</span></span>
   * <span data-ttu-id="7f143-204">**Heti adatmegőrzési**: 104 hétig minden héten vasárnap készített biztonsági másolatok megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="7f143-204">**Weekly retention policy**: Backups taken every week on Sunday are preserved for 104 weeks.</span></span>
   * <span data-ttu-id="7f143-205">**Havi adatmegőrzési**: 120 hónapig minden hónap utolsó vasárnap biztonsági másolatokat megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="7f143-205">**Monthly retention policy**: Backups taken on the last Sunday of each month are preserved for 120 months.</span></span>
   * <span data-ttu-id="7f143-206">**Éves adatmegőrzési**: minden január első vasárnap biztonsági másolatokat 99 évig megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="7f143-206">**Yearly retention policy**: Backups taken on the first Sunday of every January are preserved for 99 years.</span></span>

     <span data-ttu-id="7f143-207">Egy feladat jön létre, konfigurálja a védelmi házirendet, és rendelje hozzá a virtuális gépek minden egyes virtuális gép újraszinkronizálását választotta, az adott házirendnek.</span><span class="sxs-lookup"><span data-stu-id="7f143-207">A job is created to configure the protection policy and associate the virtual machines to that policy for each virtual machine that you've selected.</span></span>
6. <span data-ttu-id="7f143-208">A listájának megtekintéséhez **Védelemkonfigurálási** feladatok, a tárolók menüben kattintson a **feladatok** válassza ki **Védelemkonfigurálási** a a **művelet**szűrő.</span><span class="sxs-lookup"><span data-stu-id="7f143-208">To view the list of **Configure Protection** jobs, from the vaults menu, click **Jobs** and select **Configure Protection** from the **Operation** filter.</span></span>

    ![Védelemkonfigurálási feladat](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a><span data-ttu-id="7f143-210">Kezdeti biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="7f143-210">Initial backup</span></span>
<span data-ttu-id="7f143-211">Után a virtuális gép védett szabályzatnak, akkor megjelenik a a **védett elemek** lapon a állapotú *védett - (függőben lévő kezdeti biztonsági másolatot)*.</span><span class="sxs-lookup"><span data-stu-id="7f143-211">Once the virtual machine is protected with a policy, it shows up under the **Protected Items** tab with the status of *Protected - (pending initial backup)*.</span></span> <span data-ttu-id="7f143-212">Alapértelmezés szerint az első ütemezett biztonsági mentés a *kezdeti biztonsági mentés*.</span><span class="sxs-lookup"><span data-stu-id="7f143-212">By default, the first scheduled backup is the *initial backup*.</span></span>

<span data-ttu-id="7f143-213">A kezdeti biztonsági mentés elindítása után azonnal védelmének konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="7f143-213">To trigger the initial backup immediately after configuring protection:</span></span>

1. <span data-ttu-id="7f143-214">Alján a **védett elemek** kattintson **biztonsági mentés most**.</span><span class="sxs-lookup"><span data-stu-id="7f143-214">At the bottom of the **Protected Items** page, click **Backup Now**.</span></span>

    <span data-ttu-id="7f143-215">Az Azure Backup szolgáltatás biztonsági mentési feladatot hoz létre a kezdeti biztonsági mentési művelethez.</span><span class="sxs-lookup"><span data-stu-id="7f143-215">The Azure Backup service creates a backup job for the initial backup operation.</span></span>
2. <span data-ttu-id="7f143-216">Kattintson a **Feladatok** fülre a feladatok listájának megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="7f143-216">Click the **Jobs** tab to view the list of jobs.</span></span>

    ![Biztonsági mentés folyamatban](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> <span data-ttu-id="7f143-218">A biztonsági mentési művelet során az Azure Backup szolgáltatás minden egyes virtuális gépen ürítse ki az összes írási feladat, és egységes pillanatképet készít a biztonsági mentési bővítmény parancsot ad ki.</span><span class="sxs-lookup"><span data-stu-id="7f143-218">During the backup operation, the Azure Backup service issues a command to the backup extension in each virtual machine to flush all write jobs and take a consistent snapshot.</span></span>
>
>

<span data-ttu-id="7f143-219">Ha a kezdeti biztonsági mentés befejezése után a virtuális gép állapotát a **védett elemek** lap *védett*.</span><span class="sxs-lookup"><span data-stu-id="7f143-219">When the initial backup finishes, the status of the virtual machine in the **Protected Items** tab is *Protected*.</span></span>

![A virtuális gép helyreállítási ponttal van mentve](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a><span data-ttu-id="7f143-221">Biztonsági mentés állapotának és a részletek megtekintése</span><span class="sxs-lookup"><span data-stu-id="7f143-221">Viewing backup status and details</span></span>
<span data-ttu-id="7f143-222">Védett, miután a virtuális gépek számát is növekszik a **irányítópult** összefoglaló lap.</span><span class="sxs-lookup"><span data-stu-id="7f143-222">Once protected, the virtual machine count also increases in the **Dashboard** page summary.</span></span> <span data-ttu-id="7f143-223">A **irányítópult** lapon is látható az elmúlt 24 órában, melyeket a feladatok száma *sikeres*, rendelkezik *sikertelen*, és a *folyamatban* .</span><span class="sxs-lookup"><span data-stu-id="7f143-223">The **Dashboard** page also shows the number of jobs from the last 24 hours that were *successful*, have *failed*, and are *in progress*.</span></span> <span data-ttu-id="7f143-224">A a **feladatok** oldalon a **állapot**, **művelet**, vagy **a** és **való** menü használatával szűrje a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="7f143-224">On the **Jobs** page, use the **Status**, **Operation**, or **From** and **To** menus to filter the jobs.</span></span>

![Biztonsági mentés a irányítópult-oldalon állapota](./media/backup-azure-vms/dashboard-protectedvms.png)

<span data-ttu-id="7f143-226">Az irányítópult értékeket 24 óránként egyszer frissülnek.</span><span class="sxs-lookup"><span data-stu-id="7f143-226">Values in the dashboard are refreshed once every 24 hours.</span></span>

## <a name="troubleshooting-errors"></a><span data-ttu-id="7f143-227">Kapcsolatos hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="7f143-227">Troubleshooting errors</span></span>
<span data-ttu-id="7f143-228">Ha biztonsági során problémákat tapasztal a virtuális gép, tekintse meg a [VM hibaelhárítási cikke](backup-azure-vms-troubleshoot.md) segítségét.</span><span class="sxs-lookup"><span data-stu-id="7f143-228">If you run into issues while backing up your virtual machine, look at the [VM     troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f143-229">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7f143-229">Next steps</span></span>
* [<span data-ttu-id="7f143-230">A virtuális gépek kezelése és figyelése</span><span class="sxs-lookup"><span data-stu-id="7f143-230">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="7f143-231">Virtuális gépek visszaállítása</span><span class="sxs-lookup"><span data-stu-id="7f143-231">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
