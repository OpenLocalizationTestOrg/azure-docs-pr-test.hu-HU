---
title: "Áttekintés: Azure virtuális gépek biztonsági mentése Backup-tárolóval | Microsoft Docs"
description: "Használja a hello klasszikus portál tooback mentése Azure virtuális gépek tooa Backup-tárolóban. Ez az oktatóanyag azt ismerteti, például hello mentési tároló létrehozása, hello virtuális gépek regisztrálása, biztonsági mentési házirend létrehozása és hello kezdeti biztonsági mentési feladat futtató összes fázisban."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;
ms.openlocfilehash: 77700e69eab9faccbc7ef923e1eb4e1f97be75d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="first-look-backing-up-azure-virtual-machines"></a><span data-ttu-id="e5867-104">Áttekintés: Azure virtuális gépek biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="e5867-104">First look: Backing up Azure virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e5867-105">Virtuális gépek védelme Recovery Services-tárolóval</span><span class="sxs-lookup"><span data-stu-id="e5867-105">Protect VMs with a recovery services vault</span></span>](backup-azure-vms-first-look-arm.md)
> * [<span data-ttu-id="e5867-106">Azure virtuális gépek védelme Backup-tárolóval</span><span class="sxs-lookup"><span data-stu-id="e5867-106">Protect Azure VMs with a backup vault</span></span>](backup-azure-vms-first-look.md)
>
>

<span data-ttu-id="e5867-107">Ez az oktatóanyag végigvezeti egy Azure virtuális gép (VM) tooa biztonságimásolat-tárolóban az Azure biztonsági mentéséről hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="e5867-107">This tutorial takes you through hello steps for backing up an Azure virtual machine (VM) tooa backup vault in Azure.</span></span> <span data-ttu-id="e5867-108">Ez a cikk ismerteti a klasszikus modellt hello vagy a Service Manager telepítési modell, virtuális gépek biztonsági mentéséről.</span><span class="sxs-lookup"><span data-stu-id="e5867-108">This article describes hello classic model or Service Manager deployment model, for backing up VMs.</span></span> <span data-ttu-id="e5867-109">hello lépések alkalmazása csak hello a klasszikus portálon létrehozott tooBackup tárolók.</span><span class="sxs-lookup"><span data-stu-id="e5867-109">hello following steps apply only tooBackup vaults created in hello classic portal.</span></span> <span data-ttu-id="e5867-110">A Microsoft azt javasolja, hogy az új központi telepítéseknél hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="e5867-110">Microsoft recommends using hello Resource Manager model for new deployments.</span></span>

<span data-ttu-id="e5867-111">Ha érdekli VM tooa Recovery Services-tároló tooa erőforráscsoporthoz tartozó biztonsági mentéséről, olvassa el [először: virtuális gépek védelme a recovery services-tároló](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="e5867-111">If you are interested in backing up a VM tooa Recovery Services vault that belongs tooa Resource Group, see [First look: Protect VMs with a recovery services vault](backup-azure-vms-first-look-arm.md).</span></span>

<span data-ttu-id="e5867-112">toosuccessfully hello következő végezze el az oktatóanyagban az Előfeltételek léteznie kell:</span><span class="sxs-lookup"><span data-stu-id="e5867-112">toosuccessfully complete hello following tutorial, these prerequisites must exist:</span></span>

* <span data-ttu-id="e5867-113">Létrehozott egy virtuális gépet az Azure-előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="e5867-113">You have created a VM in your Azure subscription.</span></span>
* <span data-ttu-id="e5867-114">virtuális gép hello kapcsolat tooAzure nyilvános IP-címmel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="e5867-114">hello VM has connectivity tooAzure public IP addresses.</span></span> <span data-ttu-id="e5867-115">További információkért lásd: [Hálózati kapcsolatok](backup-azure-vms-prepare.md#network-connectivity).</span><span class="sxs-lookup"><span data-stu-id="e5867-115">For additional information, see [Network connectivity](backup-azure-vms-prepare.md#network-connectivity).</span></span>


> [!NOTE]
> <span data-ttu-id="e5867-116">Az Azure két üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="e5867-116">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="e5867-117">Ez az oktatóanyag hello a klasszikus portálon létrehozott virtuális gépekkel történő használatra szolgál.</span><span class="sxs-lookup"><span data-stu-id="e5867-117">This tutorial is for use with virtual machines created in hello classic portal.</span></span>
>
>

## <a name="create-a-backup-vault"></a><span data-ttu-id="e5867-118">Backup-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5867-118">Create a backup vault</span></span>
<span data-ttu-id="e5867-119">A mentési tárolóban olyan entitás, amely hello biztonsági mentések és adott idő alatt létrehozott helyreállítási pontokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="e5867-119">A backup vault is an entity that stores all hello backups and recovery points that have been created over time.</span></span> <span data-ttu-id="e5867-120">hello mentési tároló hello biztonsági mentési házirendek, amelyek alkalmazott toohello virtuális gépek biztonsági mentését is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e5867-120">hello backup vault also contains hello backup policies that are applied toohello virtual machines being backed up.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e5867-121">2017. március-től kezdődően már nem használható hello klasszikus portál toocreate mentési tárolókban.</span><span class="sxs-lookup"><span data-stu-id="e5867-121">Starting March 2017, you can no longer use hello classic portal toocreate Backup vaults.</span></span>
> <span data-ttu-id="e5867-122">A mentési tárolók tooRecovery szolgáltatások tárolókban frissítheti.</span><span class="sxs-lookup"><span data-stu-id="e5867-122">You can upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="e5867-123">További információkért lásd: hello cikk [frissíteni a biztonsági mentési tároló tooa Recovery Services-tároló](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="e5867-123">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="e5867-124">A Microsoft tooupgrade támogatja a mentési tárolókat tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="e5867-124">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="e5867-125">2017. október 15. után PowerShell toocreate mentési tárolókban nem használható.</span><span class="sxs-lookup"><span data-stu-id="e5867-125">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="e5867-126">**2017. november 1-től**:</span><span class="sxs-lookup"><span data-stu-id="e5867-126">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="e5867-127">Az összes többi biztonsági mentési tárolók lesz automatikusan frissített tooRecovery szolgáltatások tárolók.</span><span class="sxs-lookup"><span data-stu-id="e5867-127">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="e5867-128">Ön nem fogja tudni tooaccess a biztonsági mentési adatok hello a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="e5867-128">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="e5867-129">Ehelyett használjon hello Azure portál tooaccess a biztonsági mentési adatok a Recovery Services-tárolók.</span><span class="sxs-lookup"><span data-stu-id="e5867-129">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="discover-and-register-azure-virtual-machines"></a><span data-ttu-id="e5867-130">Az Azure virtuális gépek felderítése és regisztrálása</span><span class="sxs-lookup"><span data-stu-id="e5867-130">Discover and Register Azure virtual machines</span></span>
<span data-ttu-id="e5867-131">Mielőtt regisztrálná hello VM egy tárolóban, futtassa hello felderítési folyamat tooidentify bármely új virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e5867-131">Before registering hello VM with a vault, run hello discovery process tooidentify any new VMs.</span></span> <span data-ttu-id="e5867-132">Ez a virtuális gépek listáját hello az előfizetést, és további információkat, például a felhőalapú szolgáltatás- és hello régió hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e5867-132">This returns a list of virtual machines in hello subscription, along with additional information like hello cloud service name and hello region.</span></span>

1. <span data-ttu-id="e5867-133">Jelentkezzen be toohello [a klasszikus Azure portálon](http://manage.windowsazure.com/)</span><span class="sxs-lookup"><span data-stu-id="e5867-133">Sign in toohello [Azure Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="e5867-134">Hello a klasszikus Azure portálon, kattintson **Recovery Services** Recovery Services-tárolók tooopen hello listája.</span><span class="sxs-lookup"><span data-stu-id="e5867-134">In hello Azure classic portal, click **Recovery Services** tooopen hello list of Recovery Services vaults.</span></span>
    <span data-ttu-id="e5867-135">![Számítási feladat kiválasztása](./media/backup-azure-vms-first-look/recovery-services-icon.png)</span><span class="sxs-lookup"><span data-stu-id="e5867-135">![Select workload](./media/backup-azure-vms-first-look/recovery-services-icon.png)</span></span>
3. <span data-ttu-id="e5867-136">Tárolók hello listában jelölje ki hello tároló tooback virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="e5867-136">From hello list of vaults, select hello vault tooback up a VM.</span></span>

    <span data-ttu-id="e5867-137">A tároló kiválasztásakor megnyílik a hello **gyors üzembe helyezés** lap</span><span class="sxs-lookup"><span data-stu-id="e5867-137">When you select your vault, it opens in hello **Quick Start** page</span></span>
4. <span data-ttu-id="e5867-138">Hello tároló menüben kattintson **regisztrált elemek**.</span><span class="sxs-lookup"><span data-stu-id="e5867-138">From hello vault menu, click **Registered Items**.</span></span>

    ![Számítási feladat kiválasztása](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. <span data-ttu-id="e5867-140">A hello **típus** menü **Azure virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="e5867-140">From hello **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Számítási feladat kiválasztása](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="e5867-142">Kattintson a **felderítési** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="e5867-142">Click **DISCOVER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="e5867-143">![Felderítés gomb](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="e5867-143">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="e5867-144">hello felderítési folyamat eltarthat néhány percig, amíg hello virtuális gépek megjelennének alatt.</span><span class="sxs-lookup"><span data-stu-id="e5867-144">hello discovery process may take a few minutes while hello virtual machines are being tabulated.</span></span> <span data-ttu-id="e5867-145">Nincs üdvözlő képernyőt, amely lehetővé teszi, hogy tudja, hogy fut-e hello folyamat hello alján értesítést.</span><span class="sxs-lookup"><span data-stu-id="e5867-145">There is a notification at hello bottom of hello screen that lets you know that hello process is running.</span></span>

    ![Virtuális gépek felderítése](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="e5867-147">hello értesítési módosításokat, ha hello folyamat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="e5867-147">hello notification changes when hello process is complete.</span></span>

    ![A felderítés kész](./media/backup-azure-vms-first-look/discovery-complete.png)
7. <span data-ttu-id="e5867-149">Kattintson a **REGISZTRÁLÁSA** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="e5867-149">Click **REGISTER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="e5867-150">![Regisztrálás gomb](./media/backup-azure-vms-first-look/register-icon.png)</span><span class="sxs-lookup"><span data-stu-id="e5867-150">![Register button](./media/backup-azure-vms-first-look/register-icon.png)</span></span>
8. <span data-ttu-id="e5867-151">A hello **regisztrálni elemek** helyi menü, jelölje be hello virtuális gépek, amelyet az tooregister.</span><span class="sxs-lookup"><span data-stu-id="e5867-151">In hello **Register Items** shortcut menu, select hello virtual machines that you want tooregister.</span></span>

   > [!TIP]
   > <span data-ttu-id="e5867-152">Egyszerre több virtuális gép is regisztrálható.</span><span class="sxs-lookup"><span data-stu-id="e5867-152">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="e5867-153">Létrejön egy feladat minden egyes kiválasztott virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="e5867-153">A job is created for each virtual machine that you've selected.</span></span>
9. <span data-ttu-id="e5867-154">Kattintson a **feladat megtekintése** a hello értesítési toogo toohello **feladatok** lap.</span><span class="sxs-lookup"><span data-stu-id="e5867-154">Click **View Job** in hello notification toogo toohello **Jobs** page.</span></span>

    ![Regisztrációs feladat](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="e5867-156">hello virtuális gép is regisztrált elemek mellett hello regisztrációs művelet hello állapotának hello listájában jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e5867-156">hello virtual machine also appears in hello list of registered items, along with hello status of hello registration operation.</span></span>

    ![1. regisztrációs állapot](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="e5867-158">Hello művelet befejeződésekor hello állapota tooreflect hello *regisztrált* állapotát.</span><span class="sxs-lookup"><span data-stu-id="e5867-158">When hello operation completes, hello status changes tooreflect hello *registered* state.</span></span>

    ![2. regisztrációs állapot](./media/backup-azure-vms/register-status02.png)

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a><span data-ttu-id="e5867-160">Hello Virtuálisgép-ügynök telepítése hello virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="e5867-160">Install hello VM Agent on hello virtual machine</span></span>
<span data-ttu-id="e5867-161">hello Azure Virtuálisgép-ügynök hello hello biztonsági mentés bővítmény toowork Azure virtuális gép telepítve kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e5867-161">hello Azure VM Agent must be installed on hello Azure virtual machine for hello Backup extension toowork.</span></span> <span data-ttu-id="e5867-162">Ha a virtuális Gépet az Azure katalógusában hello lett létrehozva, a Virtuálisgép-ügynök hello már szerepel a hello VM; túl kihagyhatja[a virtuális gépek védelmének](backup-azure-vms-first-look.md#create-the-backup-policy).</span><span class="sxs-lookup"><span data-stu-id="e5867-162">If your VM was created from hello Azure gallery, hello VM Agent is already present on hello VM; you can skip too[protecting your VMs](backup-azure-vms-first-look.md#create-the-backup-policy).</span></span>

<span data-ttu-id="e5867-163">Ha a virtuális Gépet egy olyan helyszíni adatközpontban átemelt, hello VM valószínűleg nem rendelkezik Virtuálisgép-ügynök telepítve hello.</span><span class="sxs-lookup"><span data-stu-id="e5867-163">If your VM migrated from an on-premises datacenter, hello VM probably does not have hello VM Agent installed.</span></span> <span data-ttu-id="e5867-164">Hello virtuális gépen a Folytatás tooprotect hello VM előtt telepítenie kell hello Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="e5867-164">You must install hello VM Agent on hello virtual machine before proceeding tooprotect hello VM.</span></span> <span data-ttu-id="e5867-165">Telepítéséről részletes lépéseket hello Virtuálisgép-ügynök, a következő témakörben: hello [hello biztonsági másolat virtuális gépek cikk Virtuálisgép-ügynök része](backup-azure-vms-prepare.md#vm-agent).</span><span class="sxs-lookup"><span data-stu-id="e5867-165">For detailed steps on installing hello VM Agent, see hello [VM Agent section of hello Backup VMs article](backup-azure-vms-prepare.md#vm-agent).</span></span>

## <a name="create-hello-backup-policy"></a><span data-ttu-id="e5867-166">Hello biztonsági mentési házirend létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5867-166">Create hello backup policy</span></span>
<span data-ttu-id="e5867-167">Mielőtt hello kezdeti biztonsági mentési feladatot indít, hello ütemezés beállítása, ha a biztonsági mentési pillanatképet készít a.</span><span class="sxs-lookup"><span data-stu-id="e5867-167">Before you trigger hello initial backup job, set hello schedule when backup snapshots are taken.</span></span> <span data-ttu-id="e5867-168">hello ütemezés biztonsági mentési pillanatképet készít, és mennyi ideig hello ezeket a pillanatképeket a rendszer megőrzi, hello biztonsági mentési házirend.</span><span class="sxs-lookup"><span data-stu-id="e5867-168">hello schedule when backup snapshots are taken, and hello length of time those snapshots are retained, is hello backup policy.</span></span> <span data-ttu-id="e5867-169">hello megőrzési információ szerzett-édesapja-fia biztonsági mentési rotációs rendszerben alapul.</span><span class="sxs-lookup"><span data-stu-id="e5867-169">hello retention information is based on Grandfather-father-son backup rotation scheme.</span></span>

1. <span data-ttu-id="e5867-170">Keresse meg a toohello mentési tároló alatt **Recovery Services** hello a klasszikus Azure portálon, és kattintson a **regisztrált elemek**.</span><span class="sxs-lookup"><span data-stu-id="e5867-170">Navigate toohello backup vault under **Recovery Services** in hello Azure Classic portal, and  click **Registered Items**.</span></span>
2. <span data-ttu-id="e5867-171">Válassza ki **Azure virtuális gép** hello legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="e5867-171">Select **Azure Virtual Machine** from hello drop-down menu.</span></span>

    ![Számítási feladat kiválasztása a portálon](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="e5867-173">Kattintson a **védelme** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="e5867-173">Click **PROTECT** at hello bottom of hello page.</span></span>
    <span data-ttu-id="e5867-174">![Kattintson a Védelem gombra](./media/backup-azure-vms-first-look/protect-icon.png)</span><span class="sxs-lookup"><span data-stu-id="e5867-174">![Click Protect](./media/backup-azure-vms-first-look/protect-icon.png)</span></span>

    <span data-ttu-id="e5867-175">Hello **elemek védelme varázsló** vagy látható *csak* regisztrálva és a nem védett virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e5867-175">hello **Protect Items wizard** appears and lists *only* virtual machines that are registered and not protected.</span></span>

    ![Méretezett védelem konfigurálása](./media/backup-azure-vms/protect-at-scale.png)
4. <span data-ttu-id="e5867-177">Válassza ki a megjeleníteni kívánt tooprotect hello virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e5867-177">Select hello virtual machines that you want tooprotect.</span></span>

    <span data-ttu-id="e5867-178">Ha hello két vagy több virtuális gép azonos neve, használjon hello Felhőszolgáltatás toodistinguish hello virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="e5867-178">If there are two or more virtual machines with hello same name, use hello Cloud Service toodistinguish between hello virtual machines.</span></span>
5. <span data-ttu-id="e5867-179">A hello **Védelemkonfigurálás** menüben válassza ki a meglévő házirend, vagy hozzon létre egy új házirend tooprotect azonosított hello virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e5867-179">On hello **Configure protection** menu select an existing policy or create a new policy tooprotect hello virtual machines that you identified.</span></span>

    <span data-ttu-id="e5867-180">Új mentési tárolókban hello tárolóhoz társított alapértelmezett házirenddel rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="e5867-180">New Backup vaults have a default policy associated with hello vault.</span></span> <span data-ttu-id="e5867-181">Ez a házirend egyes este pillanatkép napi, valamint a hello napi pillanatkép 30 napig őrzi meg.</span><span class="sxs-lookup"><span data-stu-id="e5867-181">This policy takes a daily snapshot each evening, and hello daily snapshot is retained for 30 days.</span></span> <span data-ttu-id="e5867-182">Minden biztonsági mentési házirendhez több virtuális gép társítható.</span><span class="sxs-lookup"><span data-stu-id="e5867-182">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="e5867-183">Azonban hello virtuális gép csak társítható egy házirend egyszerre.</span><span class="sxs-lookup"><span data-stu-id="e5867-183">However, hello virtual machine can only be associated with one policy at a time.</span></span>

    ![Védelem új házirenddel](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="e5867-185">A biztonsági mentési házirend hello ütemezett biztonsági mentések megőrzési rendszert tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="e5867-185">A backup policy includes a retention scheme for hello scheduled backups.</span></span> <span data-ttu-id="e5867-186">Ha egy meglévő biztonsági mentési házirend, hello következő lépésben fogja nem toomodify hello adatmegőrzési beállítások.</span><span class="sxs-lookup"><span data-stu-id="e5867-186">If you select an existing backup policy, you will be unable toomodify hello retention options in hello next step.</span></span>
   >
   >
6. <span data-ttu-id="e5867-187">A **megőrzési időtartam** határozza meg az adott biztonsági mentési pontok hello napi, heti, havi vagy éves hatókör hello.</span><span class="sxs-lookup"><span data-stu-id="e5867-187">On **Retention Range** define hello daily, weekly, monthly, and yearly scope for hello specific backup points.</span></span>

    ![A virtuális gép helyreállítási ponttal van mentve](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="e5867-189">Adatmegőrzési idő másolatának tárolására szolgáló hello hosszát adja meg.</span><span class="sxs-lookup"><span data-stu-id="e5867-189">Retention policy specifies hello length of time for storing a backup.</span></span> <span data-ttu-id="e5867-190">Az eltérő megőrzési házirendek hello biztonsági mentés időpontjában alapján is megadhat.</span><span class="sxs-lookup"><span data-stu-id="e5867-190">You can specify different retention policies based on when hello backup is taken.</span></span>
7. <span data-ttu-id="e5867-191">Kattintson a **feladatok** tooview hello listája **Védelemkonfigurálási** feladatok.</span><span class="sxs-lookup"><span data-stu-id="e5867-191">Click **Jobs** tooview hello list of **Configure Protection** jobs.</span></span>

    ![Védelemkonfigurálási feladat](./media/backup-azure-vms/protect-configureprotection.png)

    <span data-ttu-id="e5867-193">Most, hogy létrehozta a hello házirend, toohello következő lépést, és hello kezdeti biztonsági mentés futtatására.</span><span class="sxs-lookup"><span data-stu-id="e5867-193">Now that you've established hello policy, go toohello next step and run hello initial backup.</span></span>

## <a name="initial-backup"></a><span data-ttu-id="e5867-194">Kezdeti biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="e5867-194">Initial backup</span></span>
<span data-ttu-id="e5867-195">Ha egy virtuális gép védett, házirend, megtekintheti a hello kapcsolathoz **védett elemek** fülre. Amíg nem hello kezdeti biztonsági másolatot történik, hello **védelmi állapot** jeleníti meg, mint a **védett - (függőben lévő kezdeti biztonsági másolatot)**.</span><span class="sxs-lookup"><span data-stu-id="e5867-195">Once a virtual machine has been protected with a policy, you can view that relationship on hello **Protected Items** tab. Until hello initial backup occurs, hello **Protection Status** shows as **Protected - (pending initial backup)**.</span></span> <span data-ttu-id="e5867-196">Alapértelmezés szerint hello első ütemezett biztonsági mentés hello *kezdeti biztonsági másolatot*.</span><span class="sxs-lookup"><span data-stu-id="e5867-196">By default, hello first scheduled backup is hello *initial backup*.</span></span>

![Biztonsági mentés függőben](./media/backup-azure-vms-first-look/protection-pending-border.png)

<span data-ttu-id="e5867-198">toostart hello kezdeti biztonsági mentés most:</span><span class="sxs-lookup"><span data-stu-id="e5867-198">toostart hello initial backup now:</span></span>

1. <span data-ttu-id="e5867-199">A hello **védett elemek** kattintson **biztonsági mentés most** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="e5867-199">On hello **Protected Items** page, click **Backup Now** at hello bottom of hello page.</span></span>
    <span data-ttu-id="e5867-200">![Biztonsági mentés ikon](./media/backup-azure-vms-first-look/backup-now-icon.png)</span><span class="sxs-lookup"><span data-stu-id="e5867-200">![Backup Now icon](./media/backup-azure-vms-first-look/backup-now-icon.png)</span></span>

    <span data-ttu-id="e5867-201">hello Azure Backup szolgáltatás létrehoz egy biztonsági mentési feladatot az hello kezdeti biztonsági mentési műveletet.</span><span class="sxs-lookup"><span data-stu-id="e5867-201">hello Azure Backup service creates a backup job for hello initial backup operation.</span></span>
2. <span data-ttu-id="e5867-202">Kattintson a hello **feladatok** lapon tooview hello feladatok listája.</span><span class="sxs-lookup"><span data-stu-id="e5867-202">Click hello **Jobs** tab tooview hello list of jobs.</span></span>

    ![Biztonsági mentés folyamatban](./media/backup-azure-vms-first-look/protect-inprogress.png)

    <span data-ttu-id="e5867-204">Ha a kezdeti biztonsági mentés befejeződött, a hello állapot hello virtuális gép hello **védett elemek** lap *védett*.</span><span class="sxs-lookup"><span data-stu-id="e5867-204">When initial backup is complete, hello status of hello virtual machine in hello **Protected Items** tab is *Protected*.</span></span>

    ![A virtuális gép helyreállítási ponttal van mentve](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > <span data-ttu-id="e5867-206">A virtuális gépek biztonsági mentése egy helyi folyamat.</span><span class="sxs-lookup"><span data-stu-id="e5867-206">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="e5867-207">Nem készíthet biztonsági másolatot a virtuális gépek az egy régióban tooa biztonsági mentési tárolóból egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="e5867-207">You cannot back up virtual machines from one region tooa backup vault in another region.</span></span> <span data-ttu-id="e5867-208">Igen minden Azure-régió, amely rendelkezik a virtuális gépek biztonsági mentése toobe igénylő, legalább egy mentési tárolóból, amelyben létre kell hozni az adott régióban.</span><span class="sxs-lookup"><span data-stu-id="e5867-208">So, for every Azure region that has VMs that need toobe backed up, at least one backup vault must be created in that region.</span></span>
   >
   >

## <a name="next-steps"></a><span data-ttu-id="e5867-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e5867-209">Next steps</span></span>
<span data-ttu-id="e5867-210">Most, hogy sikeresen készített biztonsági mentést egy virtuális gépről, számos további lépés végezhető.</span><span class="sxs-lookup"><span data-stu-id="e5867-210">Now that you have successfully backed up a VM, there are several next steps that could be of interest.</span></span> <span data-ttu-id="e5867-211">hello legtöbb logikai lépésre toofamiliarize saját magának az adatok tooa virtuális gép visszaállítására.</span><span class="sxs-lookup"><span data-stu-id="e5867-211">hello most logical step is toofamiliarize yourself with restoring data tooa VM.</span></span> <span data-ttu-id="e5867-212">Van azonban feladat, amely segít megérteni hogyan tookeep biztonságos adatait és a költségek minimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="e5867-212">However, there are management tasks that will help you understand how tookeep your data safe and minimize costs.</span></span>

* [<span data-ttu-id="e5867-213">A virtuális gépek kezelése és figyelése</span><span class="sxs-lookup"><span data-stu-id="e5867-213">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="e5867-214">Virtuális gépek visszaállítása</span><span class="sxs-lookup"><span data-stu-id="e5867-214">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
* [<span data-ttu-id="e5867-215">Hibaelhárítási útmutató</span><span class="sxs-lookup"><span data-stu-id="e5867-215">Troubleshooting guidance</span></span>](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a><span data-ttu-id="e5867-216">Kérdései vannak?</span><span class="sxs-lookup"><span data-stu-id="e5867-216">Questions?</span></span>
<span data-ttu-id="e5867-217">Ha kérdése van, vagy ha bármely új szolgáltatása része, toosee szeretné [visszajelzést küldhet](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="e5867-217">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>
