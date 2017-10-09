---
title: "aaaPrepare gépek tooset mentése után a Site Recovery segítségével áttelepítési tooAzure Azure-régiók közötti vész-helyreállítási |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooprepare gépek tooset katasztrófa utáni helyreállítás után áttelepítési tooAzure Azure Site Recovery segítségével Azure-régiók közötti fel."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: ponatara
ms.openlocfilehash: b6274e3df210c1d8a7b8289cc85868ee6414e523
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-tooanother-region-after-migration-tooazure-by-using-azure-site-recovery"></a><span data-ttu-id="15f55-103">Azure virtuális gépek tooanother régió replikálása után áttelepítési tooAzure Azure Site Recovery segítségével</span><span class="sxs-lookup"><span data-stu-id="15f55-103">Replicate Azure VMs tooanother region after migration tooAzure by using Azure Site Recovery</span></span>

>[!NOTE]
> <span data-ttu-id="15f55-104">Az Azure virtuális gépek (VM) az Azure Site Recovery replikációs jelenleg előzetes verzió.</span><span class="sxs-lookup"><span data-stu-id="15f55-104">Azure Site Recovery replication for Azure virtual machines (VMs) is currently in preview.</span></span>

## <a name="overview"></a><span data-ttu-id="15f55-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="15f55-105">Overview</span></span>

<span data-ttu-id="15f55-106">Ez a cikk segít az Azure virtuális gépek előkészítése után ezek a gépek áttelepített egy a helyszíni környezet tooAzure Azure Site Recovery segítségével két Azure régiók közötti replikáció.</span><span class="sxs-lookup"><span data-stu-id="15f55-106">This article helps you prepare Azure virtual machines for replication between two Azure regions after these machines have been migrated from an on-premises environment tooAzure by using Azure Site Recovery.</span></span>

## <a name="disaster-recovery-and-compliance"></a><span data-ttu-id="15f55-107">Vész-helyreállítási és megfelelőség</span><span class="sxs-lookup"><span data-stu-id="15f55-107">Disaster recovery and compliance</span></span>
<span data-ttu-id="15f55-108">Napjainkban egyre több vállalatok áthelyezi a munkaterhelések tooAzure.</span><span class="sxs-lookup"><span data-stu-id="15f55-108">Today, more and more enterprises are moving their workloads tooAzure.</span></span> <span data-ttu-id="15f55-109">A kritikus fontosságú helyszíni éles áthelyezése vállalatok munkaterhelések tooAzure, az ilyen terhelések vész-helyreállítási beállítása megadása kötelező, megfelelőségi és toosafeguard szemben Azure-régióban esetleges megszakadását.</span><span class="sxs-lookup"><span data-stu-id="15f55-109">With enterprises moving mission-critical on-premises production workloads tooAzure, setting up disaster recovery for these workloads is mandatory for compliance and toosafeguard against any disruptions in an Azure region.</span></span>

## <a name="steps-for-preparing-migrated-machines-for-replication"></a><span data-ttu-id="15f55-110">Felkészülés az áttelepített gépek replikációjának lépései</span><span class="sxs-lookup"><span data-stu-id="15f55-110">Steps for preparing migrated machines for replication</span></span>
<span data-ttu-id="15f55-111">tooprepare állíthatja be az Azure-régió replikációs tooanother gépeket telepített át:</span><span class="sxs-lookup"><span data-stu-id="15f55-111">tooprepare migrated machines for setting up replication tooanother Azure region:</span></span>

1. <span data-ttu-id="15f55-112">Az áttelepítés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="15f55-112">Complete migration.</span></span>
2. <span data-ttu-id="15f55-113">Hello Azure ügynök szükség esetén telepítse.</span><span class="sxs-lookup"><span data-stu-id="15f55-113">Install hello Azure agent if needed.</span></span>
3. <span data-ttu-id="15f55-114">Távolítsa el a mobilitási szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="15f55-114">Remove hello Mobility service.</span></span>  
4. <span data-ttu-id="15f55-115">Indítsa újra a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="15f55-115">Restart hello VM.</span></span>

<span data-ttu-id="15f55-116">Bővebben a következő részekben hello ezeket a lépéseket ismerteti.</span><span class="sxs-lookup"><span data-stu-id="15f55-116">These steps are described in more detail in hello following sections.</span></span>

### <a name="step-1-migrate-workloads-running-on-hyper-v-vms-vmware-vms-and-physical-servers-toorun-on-azure-vms"></a><span data-ttu-id="15f55-117">1. lépés:, Telepítse át a Hyper-V virtuális gépek, a VMware virtuális gépek és fizikai kiszolgálók toorun Azure virtuális gépeken futó számítási feladatok</span><span class="sxs-lookup"><span data-stu-id="15f55-117">Step 1: Migrate workloads running on Hyper-V VMs, VMware VMs, and physical servers toorun on Azure VMs</span></span>

<span data-ttu-id="15f55-118">tooset replikációs kialakításához, és telepítse át a helyszíni Hyper-V, VMware és fizikai munkaterhelésekhez tooAzure kövesse hello lépéseit hello [át Azure IaaS virtuális gépeket az Azure Site Recovery Azure-régiók közötti](site-recovery-migrate-to-azure.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="15f55-118">tooset up replication and migrate your on-premises Hyper-V, VMware, and physical workloads tooAzure, follow hello steps in hello [Migrate Azure IaaS virtual machines between Azure regions with Azure Site Recovery](site-recovery-migrate-to-azure.md) article.</span></span> 

<span data-ttu-id="15f55-119">Az áttelepítés után nem toocommit kell, vagy törölje a feladatátvétel.</span><span class="sxs-lookup"><span data-stu-id="15f55-119">After migration, you don't need toocommit or delete a failover.</span></span> <span data-ttu-id="15f55-120">Ehelyett válassza ki a hello **az áttelepítés végrehajtásához** lehetőséget az egyes gépek toomigrate szeretne:</span><span class="sxs-lookup"><span data-stu-id="15f55-120">Instead, select hello **Complete Migration** option for each machine you want toomigrate:</span></span>
1. <span data-ttu-id="15f55-121">A **replikált elemek**, kattintson a jobb gombbal a virtuális gép hello, és kattintson a **az áttelepítés végrehajtásához**.</span><span class="sxs-lookup"><span data-stu-id="15f55-121">In **Replicated Items**, right-click hello VM, and click **Complete Migration**.</span></span> <span data-ttu-id="15f55-122">Kattintson a **OK** toocomplete hello lépés.</span><span class="sxs-lookup"><span data-stu-id="15f55-122">Click **OK** toocomplete hello step.</span></span> <span data-ttu-id="15f55-123">A folyamat állapotát a virtuális gép tulajdonságok hello nyomon követéséhez hello teljes áttelepítési feladatot a figyelés **Site Recovery-feladatok**.</span><span class="sxs-lookup"><span data-stu-id="15f55-123">You can track progress in hello VM properties by monitoring hello Complete Migration job in **Site Recovery jobs**.</span></span>
2. <span data-ttu-id="15f55-124">Hello **az áttelepítés végrehajtásához** művelet hello áttelepítési folyamat befejeződik, hello gép replikálása eltávolítja, és leállítja a Site Recovery számlázási hello gép.</span><span class="sxs-lookup"><span data-stu-id="15f55-124">hello **Complete Migration** action completes hello migration process, removes replication for hello machine, and stops Site Recovery billing for hello machine.</span></span>

   ![teljesáttelepítés](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

### <a name="step-2-install-hello-azure-vm-agent-on-hello-virtual-machine"></a><span data-ttu-id="15f55-126">2. lépés: Hello Azure Virtuálisgép-ügynök telepítése hello virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="15f55-126">Step 2: Install hello Azure VM agent on hello virtual machine</span></span>
<span data-ttu-id="15f55-127">hello Azure [Virtuálisgép-ügynök](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) hello virtuális gépen telepítve kell, a hello Site Recovery bővítmény toowork és toohelp hello virtuális gép védelmét.</span><span class="sxs-lookup"><span data-stu-id="15f55-127">hello Azure [VM agent](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux) must be installed on hello virtual machine for hello Site Recovery extension toowork and toohelp protect hello VM.</span></span>

>[!IMPORTANT]
><span data-ttu-id="15f55-128">A Windows virtuális gépek 9.7.0.0, verziójától hello mobilitási szolgáltatások telepítőjének is telepíti hello legújabb elérhető Azure Virtuálisgép-ügynök.</span><span class="sxs-lookup"><span data-stu-id="15f55-128">Beginning with version 9.7.0.0, on Windows virtual machines, hello Mobility service installer also installs hello latest available Azure VM agent.</span></span> <span data-ttu-id="15f55-129">Az áttelepítési hello virtuális gép megfelel-e a bármely Virtuálisgép-bővítmény, belefoglalja hello Site Recovery használatára vonatkozó előfeltételek telepítése.</span><span class="sxs-lookup"><span data-stu-id="15f55-129">On migration, hello virtual machine meets the agent installation prerequisite for using any VM extension, including hello Site Recovery extension.</span></span> <span data-ttu-id="15f55-130">hello Azure virtuális gép ügynök igények toobe manuálisan telepíteni, csak akkor, ha hello hello a mobilitási szolgáltatás áttelepített gépet 9.6 vagy korábbi verziója.</span><span class="sxs-lookup"><span data-stu-id="15f55-130">hello Azure VM agent needs toobe manually installed only if hello Mobility service installed on hello migrated machine is version 9.6 or earlier.</span></span>

<span data-ttu-id="15f55-131">hello következő táblázat további információt tartalmaz hello Virtuálisgép-ügynök telepítése és az ellenőrzése, hogy telepítette:</span><span class="sxs-lookup"><span data-stu-id="15f55-131">hello following table provides additional information about installing hello VM agent and validating that it was installed:</span></span>

| <span data-ttu-id="15f55-132">**Művelet**</span><span class="sxs-lookup"><span data-stu-id="15f55-132">**Operation**</span></span> | <span data-ttu-id="15f55-133">**Windows**</span><span class="sxs-lookup"><span data-stu-id="15f55-133">**Windows**</span></span> | <span data-ttu-id="15f55-134">**Linux**</span><span class="sxs-lookup"><span data-stu-id="15f55-134">**Linux**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="15f55-135">Hello Virtuálisgép-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="15f55-135">Installing hello VM agent</span></span> |<span data-ttu-id="15f55-136">Töltse le és telepítse a hello [ügynök MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="15f55-136">Download and install hello [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).</span></span> <span data-ttu-id="15f55-137">Rendszergazdai jogosultságok toocomplete hello telepítési van szüksége.</span><span class="sxs-lookup"><span data-stu-id="15f55-137">You need administrator privileges toocomplete hello installation.</span></span> |<span data-ttu-id="15f55-138">Telepítse legújabb hello [Linux-ügynök](../virtual-machines/linux/agent-user-guide.md).</span><span class="sxs-lookup"><span data-stu-id="15f55-138">Install hello latest [Linux agent](../virtual-machines/linux/agent-user-guide.md).</span></span> <span data-ttu-id="15f55-139">Rendszergazdai jogosultságok toocomplete hello telepítési van szüksége.</span><span class="sxs-lookup"><span data-stu-id="15f55-139">You need administrator privileges toocomplete hello installation.</span></span> <span data-ttu-id="15f55-140">Azt javasoljuk, hogy a terjesztési adattárból hello ügynök telepítése.</span><span class="sxs-lookup"><span data-stu-id="15f55-140">We recommend installing hello agent from your distribution repository.</span></span> <span data-ttu-id="15f55-141">A Microsoft *nem javasoljuk* közvetlenül a Githubból hello Linux Virtuálisgép-ügynök telepítése.</span><span class="sxs-lookup"><span data-stu-id="15f55-141">We *do not recommend* installing hello Linux VM agent directly from GitHub.</span></span>  |
| <span data-ttu-id="15f55-142">Hello Virtuálisgép-ügynök telepítésének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="15f55-142">Validating hello VM agent installation</span></span> |<span data-ttu-id="15f55-143">1. Keresse meg a toohello C:\WindowsAzure\Packages mappájában hello Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="15f55-143">1. Browse toohello C:\WindowsAzure\Packages folder in hello Azure VM.</span></span> <span data-ttu-id="15f55-144">Megtekintheti az hello WaAppAgent.exe fájlt.</span><span class="sxs-lookup"><span data-stu-id="15f55-144">You should see hello WaAppAgent.exe file.</span></span> <br><span data-ttu-id="15f55-145">2. Kattintson a jobb gombbal a hello fájlt, nyissa meg túl**tulajdonságok**, majd hello **részletek** külön-külön hello **termékverzió** mező lehet 2.6.1198.718 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="15f55-145">2. Right-click hello file, go too**Properties**, and then select hello **Details** tab. hello **Product Version** field should be 2.6.1198.718 or higher.</span></span> |<span data-ttu-id="15f55-146">N/A</span><span class="sxs-lookup"><span data-stu-id="15f55-146">N/A</span></span> |


### <a name="step-3-remove-hello-mobility-service-from-hello-migrated-virtual-machine"></a><span data-ttu-id="15f55-147">3. lépés: Az áttelepített virtuális gép eltávolítása hello hello a mobilitási szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="15f55-147">Step 3: Remove hello Mobility service from hello migrated virtual machine</span></span>

<span data-ttu-id="15f55-148">Ha áttelepítette a helyszíni VMware gépek vagy windowsos/Linuxos fizikai kiszolgálók, akkor toomanually távolítsa el, illetve az Eltávolítás hello mobilitásiszolgáltatás hello áttelepített virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="15f55-148">If you have migrated your on-premises VMware machines or physical servers on Windows/Linux, you need toomanually remove/uninstall hello Mobility service from hello migrated virtual machine.</span></span>

>[!IMPORTANT]
><span data-ttu-id="15f55-149">Ez a lépés nincs szükség a Hyper-V virtuális gépek áttelepített tooAzure.</span><span class="sxs-lookup"><span data-stu-id="15f55-149">This step is not required for Hyper-V VMs migrated tooAzure.</span></span>

#### <a name="uninstall-hello-mobility-service-on-a-windows-server-vm"></a><span data-ttu-id="15f55-150">Távolítsa el a hello mobilitási szolgáltatás egy Windows Server virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="15f55-150">Uninstall hello Mobility service on a Windows Server VM</span></span>
<span data-ttu-id="15f55-151">Hello módszerek toouninstall hello mobilitási szolgáltatás egy Windows Server-számítógépen a következő egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="15f55-151">Use one of hello following methods toouninstall hello Mobility service on a Windows Server computer.</span></span>

##### <a name="uninstall-by-using-hello-windows-ui"></a><span data-ttu-id="15f55-152">Távolítsa el a Windows felhasználói felületének hello használatával</span><span class="sxs-lookup"><span data-stu-id="15f55-152">Uninstall by using hello Windows UI</span></span>
1. <span data-ttu-id="15f55-153">Hello Vezérlőpultot, válassza ki **programok**.</span><span class="sxs-lookup"><span data-stu-id="15f55-153">In hello Control Panel, select **Programs**.</span></span>
2. <span data-ttu-id="15f55-154">Válassza ki **Microsoft Azure Site helyreállítási mobilitási szolgáltatás/fő célkiszolgáló**, majd válassza ki **Eltávolítás**.</span><span class="sxs-lookup"><span data-stu-id="15f55-154">Select **Microsoft Azure Site Recovery Mobility Service/Master Target server**, and then select **Uninstall**.</span></span>

##### <a name="uninstall-at-a-command-prompt"></a><span data-ttu-id="15f55-155">Távolítsa el a parancssorba</span><span class="sxs-lookup"><span data-stu-id="15f55-155">Uninstall at a command prompt</span></span>
1. <span data-ttu-id="15f55-156">Nyisson meg egy parancssori ablakot rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="15f55-156">Open a Command Prompt window as an administrator.</span></span>
2. <span data-ttu-id="15f55-157">toouninstall hello mobilitási szolgáltatást, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="15f55-157">toouninstall hello Mobility service, run hello following command:</span></span>

   ```
   MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
   ```

#### <a name="uninstall-hello-mobility-service-on-a-linux-computer"></a><span data-ttu-id="15f55-158">Távolítsa el a mobilitási szolgáltatás hello egy Linux-számítógép</span><span class="sxs-lookup"><span data-stu-id="15f55-158">Uninstall hello Mobility service on a Linux computer</span></span>
1. <span data-ttu-id="15f55-159">A Linux-kiszolgálón jelentkezzen be egy **legfelső szintű** felhasználó.</span><span class="sxs-lookup"><span data-stu-id="15f55-159">On your Linux server, sign in as a **root** user.</span></span>
2. <span data-ttu-id="15f55-160">A Terminálszolgáltatások lépjen túl/felhasználó/helyi/automatikus rendszer-Helyreállítás.</span><span class="sxs-lookup"><span data-stu-id="15f55-160">In a terminal, go too/user/local/ASR.</span></span>
3. <span data-ttu-id="15f55-161">toouninstall hello mobilitási szolgáltatást, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="15f55-161">toouninstall hello Mobility service, run hello following command:</span></span>

   ```
   uninstall.sh -Y
   ```

### <a name="step-4-restart-hello-vm"></a><span data-ttu-id="15f55-162">4. lépés: Indítsa újra a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="15f55-162">Step 4: Restart hello VM</span></span>

<span data-ttu-id="15f55-163">Hello mobilitási szolgáltatás eltávolítása után előtt a virtuális gép újraindítása hello beállítása replikációs tooanother Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="15f55-163">After you uninstall hello Mobility service, restart hello VM before you set up replication tooanother Azure region.</span></span>


## <a name="next-steps"></a><span data-ttu-id="15f55-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="15f55-164">Next steps</span></span>
- <span data-ttu-id="15f55-165">A munkaterhelések számára a védelmének megkezdéséhez [Azure virtuális gépek replikálásához](site-recovery-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="15f55-165">Start protecting your workloads by [replicating Azure virtual machines](site-recovery-azure-to-azure.md).</span></span>
- <span data-ttu-id="15f55-166">További információ [útmutató az Azure virtuális gépek replikálása a hálózat](site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="15f55-166">Learn more about [networking guidance for replicating Azure virtual machines](site-recovery-azure-to-azure-networking-guidance.md).</span></span>
