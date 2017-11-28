---
title: "aaaReplicate Hyper-V virtuális gépek VMM-felhőkben Azure Site Recovery és a PowerShell használatával (Resource Manager) |} Microsoft Docs"
description: "Hyper-V virtuális gépek replikálása az Azure Site Recovery és a PowerShell használatával a VMM-felhők"
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: raynew
ms.assetid: 6ac509ad-5024-43d8-b621-d8fec019b9a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: b0f641de4e10a600ead415ceb9bd488fb4d1659d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-powershell-and-azure-resource-manager"></a><span data-ttu-id="06e07-103">A VMM-felhők tooAzure PowerShell és az Azure Resource Manager használatával Hyper-V virtuális gépek replikálása</span><span class="sxs-lookup"><span data-stu-id="06e07-103">Replicate Hyper-V virtual machines in VMM clouds tooAzure using PowerShell and Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="06e07-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="06e07-104">Azure Portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="06e07-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="06e07-105">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="06e07-106">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="06e07-106">Classic Portal</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="06e07-107">PowerShell – Klasszikus</span><span class="sxs-lookup"><span data-stu-id="06e07-107">PowerShell - Classic</span></span>](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a><span data-ttu-id="06e07-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="06e07-108">Overview</span></span>
<span data-ttu-id="06e07-109">Az Azure Site Recovery hozzájárul tooyour üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia megvalósításában replikációjának, feladatátvételének és helyreállítási virtuális gép a különböző telepítési forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="06e07-109">Azure Site Recovery contributes tooyour business continuity and disaster recovery (BCDR) strategy by orchestrating replication, failover and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="06e07-110">Egy teljes listája központi telepítési forgatókönyvek: hello [Azure Site Recovery áttekintését](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="06e07-110">For a full list of deployment scenarios see hello [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="06e07-111">Ez a cikk bemutatja, hogyan toouse PowerShell tooautomate gyakori feladatokat kell tooperform Azure Site Recovery tooreplicate Hyper-V virtuális gépek a System Center VMM-felhők tooAzure tárolási beállításakor.</span><span class="sxs-lookup"><span data-stu-id="06e07-111">This article shows you how toouse PowerShell tooautomate common tasks you need tooperform when you set up Azure Site Recovery tooreplicate Hyper-V virtual machines in System Center VMM clouds tooAzure storage.</span></span>

<span data-ttu-id="06e07-112">hello cikk hello forgatókönyv előfeltételei tartalmazza, és megjeleníti a</span><span class="sxs-lookup"><span data-stu-id="06e07-112">hello article includes prerequisites for hello scenario, and shows you</span></span>

* <span data-ttu-id="06e07-113">Hogyan tooset mentést a Recovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="06e07-113">How tooset up a Recovery Services Vault</span></span>
* <span data-ttu-id="06e07-114">A forrás VMM-kiszolgálón hello hello Azure Site Recovery Provider telepítése</span><span class="sxs-lookup"><span data-stu-id="06e07-114">Install hello Azure Site Recovery Provider on hello source VMM server</span></span>
* <span data-ttu-id="06e07-115">Regisztrálja hello kiszolgálót hello tárolóban, az Azure-tárfiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="06e07-115">Register hello server in hello vault, add an Azure storage account</span></span>
* <span data-ttu-id="06e07-116">Hello Azure Recovery Services agent telepítése a Hyper-V gazdakiszolgálókra</span><span class="sxs-lookup"><span data-stu-id="06e07-116">Install hello Azure Recovery Services agent on Hyper-V host servers</span></span>
* <span data-ttu-id="06e07-117">Adja meg a VMM-felhő, lesz alkalmazott tooall védett virtuális gép védelmi beállításait</span><span class="sxs-lookup"><span data-stu-id="06e07-117">Configure protection settings for VMM clouds, that will be applied tooall protected virtual machines</span></span>
* <span data-ttu-id="06e07-118">Ezen virtuális gépek védelmének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="06e07-118">Enable protection for those virtual machines.</span></span>
* <span data-ttu-id="06e07-119">Tesztelje a hello feladatátvételi toomake meg arról, hogy minden a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="06e07-119">Test hello fail-over toomake sure everything's working as expected.</span></span>

<span data-ttu-id="06e07-120">Ha ez a forgatókönyv beállítása problémát tapasztal, kérdéseit a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="06e07-120">If you run into problems setting up this scenario, post your questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="06e07-121">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="06e07-121">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="06e07-122">Ez a cikk hello Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="06e07-122">This article covers using hello Resource Manager deployment model.</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="06e07-123">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="06e07-123">Before you start</span></span>
<span data-ttu-id="06e07-124">Győződjön meg arról, hogy az előfeltételek teljesülnek:</span><span class="sxs-lookup"><span data-stu-id="06e07-124">Make sure you have these prerequisites in place:</span></span>

### <a name="azure-prerequisites"></a><span data-ttu-id="06e07-125">Azure-előfeltételek</span><span class="sxs-lookup"><span data-stu-id="06e07-125">Azure prerequisites</span></span>
* <span data-ttu-id="06e07-126">Szüksége lesz egy [Microsoft Azure](https://azure.microsoft.com/)-fiókra.</span><span class="sxs-lookup"><span data-stu-id="06e07-126">You'll need a [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="06e07-127">Ha még nincs fiókja, kezdje a [ingyenes fiókot](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="06e07-127">If you don't have one, start with a [free account](https://azure.microsoft.com/free).</span></span> <span data-ttu-id="06e07-128">Emellett áttekintheti az hello [Azure Site Recovery Manager árképzési](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="06e07-128">In addition, you can read about hello [Azure Site Recovery Manager pricing](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
* <span data-ttu-id="06e07-129">Hello replikációs tooa CSP előfizetés forgatókönyvben, ha szüksége lesz egy CSP-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="06e07-129">You'll need a CSP subscription if you are trying out hello replication tooa CSP subscription scenario.</span></span> <span data-ttu-id="06e07-130">További információ a hello CSP programról [hogyan hello CSP programban tooenroll](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).</span><span class="sxs-lookup"><span data-stu-id="06e07-130">Learn more about hello CSP program in [how tooenroll in hello CSP program](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).</span></span>
* <span data-ttu-id="06e07-131">Szüksége lesz egy Azure v2 (erőforrás-kezelő) tárolási fiók toostore replikált adatok tooAzure.</span><span class="sxs-lookup"><span data-stu-id="06e07-131">You'll need an Azure v2 storage (Resource Manager) account toostore data replicated tooAzure.</span></span> <span data-ttu-id="06e07-132">hello fiókot kell georeplikáció engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="06e07-132">hello account needs geo-replication enabled.</span></span> <span data-ttu-id="06e07-133">Lehetséges értékek: hello és hello Azure Site Recovery szolgáltatásnak ugyanabban a régióban, és hello társítható ugyanahhoz az előfizetéshez vagy hello CSP előfizetés.</span><span class="sxs-lookup"><span data-stu-id="06e07-133">It should be in hello same region as hello Azure Site Recovery service, and be associated with hello same subscription or hello CSP subscription.</span></span> <span data-ttu-id="06e07-134">További információ az Azure storage beállítása toolearn lásd: hello [Azure Storage bemutatása tooMicrosoft](../storage/common/storage-introduction.md) referenciaként.</span><span class="sxs-lookup"><span data-stu-id="06e07-134">toolearn more about setting up Azure storage, see hello [Introduction tooMicrosoft Azure Storage](../storage/common/storage-introduction.md) for reference.</span></span>
* <span data-ttu-id="06e07-135">Szüksége lesz arra, hogy a virtuális gépeknek meg tooprotect megfelelnek-e hello toomake [Azure virtuális gép Előfeltételek](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="06e07-135">You'll need toomake sure that virtual machines you want tooprotect comply with hello [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

> [!NOTE]
> <span data-ttu-id="06e07-136">Csak a virtuális gép szintű műveletek jelenleg Powershellen keresztül lehetséges.</span><span class="sxs-lookup"><span data-stu-id="06e07-136">Currently, only VM level operations are possible through Powershell.</span></span> <span data-ttu-id="06e07-137">Helyreállítási terv szintű műveletek támogatása hamarosan fog történni.</span><span class="sxs-lookup"><span data-stu-id="06e07-137">Support for recovery plan level operations will be made soon.</span></span>  <span data-ttu-id="06e07-138">Most korlátozott tooperforming sikertelenek-garantál csak a "védett virtuális gép" lépésköz és nem a helyreállítási terv szintű áll.</span><span class="sxs-lookup"><span data-stu-id="06e07-138">For now, you are limited tooperforming fail-overs only at a ‘protected VM’ granularity and not at a Recovery Plan level.</span></span>
>
>

### <a name="vmm-prerequisites"></a><span data-ttu-id="06e07-139">A VMM-Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="06e07-139">VMM prerequisites</span></span>
* <span data-ttu-id="06e07-140">A System Center 2012 R2 rendszeren futó VMM-kiszolgáló lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="06e07-140">You'll need VMM server running on System Center 2012 R2.</span></span>
* <span data-ttu-id="06e07-141">A virtuális gépeket tartalmazó VMM-kiszolgáló kívánt tooprotect hello Azure Site Recovery Provider kell futnia.</span><span class="sxs-lookup"><span data-stu-id="06e07-141">Any VMM server containing virtual machines you want tooprotect must be running hello Azure Site Recovery Provider.</span></span> <span data-ttu-id="06e07-142">Ez hello Azure Site Recovery üzembe helyezése során telepítve van.</span><span class="sxs-lookup"><span data-stu-id="06e07-142">This is installed during hello Azure Site Recovery deployment.</span></span>
* <span data-ttu-id="06e07-143">A VMM-kiszolgálónak tooprotect hello legalább egy felhő lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="06e07-143">You'll need at least one cloud on hello VMM server you want tooprotect.</span></span> <span data-ttu-id="06e07-144">hello felhők tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="06e07-144">hello cloud should contain:</span></span>
  * <span data-ttu-id="06e07-145">Legalább egy VMM-gazdagépcsoport.</span><span class="sxs-lookup"><span data-stu-id="06e07-145">One or more VMM host groups.</span></span>
  * <span data-ttu-id="06e07-146">Minden gazdagépcsoportban legalább egy Hyper-V gazdakiszolgáló vagy fürt.</span><span class="sxs-lookup"><span data-stu-id="06e07-146">One or more Hyper-V host servers or clusters in each host group.</span></span>
  * <span data-ttu-id="06e07-147">Egy vagy több virtuális gépek hello forrás Hyper-V kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="06e07-147">One or more virtual machines on hello source Hyper-V server.</span></span>
* <span data-ttu-id="06e07-148">További információ a VMM-felhőkben beállítása:</span><span class="sxs-lookup"><span data-stu-id="06e07-148">Learn more about setting up VMM clouds:</span></span>
  * <span data-ttu-id="06e07-149">További információk a VMM-magánfelhőkben [újdonságai a System Center 2012 R2 VMM Magánfelhő](http://go.microsoft.com/fwlink/?LinkId=324952) és a [VMM 2012 és a hello](http://go.microsoft.com/fwlink/?LinkId=324956).</span><span class="sxs-lookup"><span data-stu-id="06e07-149">Read more about private VMM clouds in [What’s New in Private Cloud with System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) and in [VMM 2012 and hello clouds](http://go.microsoft.com/fwlink/?LinkId=324956).</span></span>
  * <span data-ttu-id="06e07-150">További tudnivalók [konfigurálása hello VMM felhőbeli háló](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)</span><span class="sxs-lookup"><span data-stu-id="06e07-150">Learn about [Configuring hello VMM cloud fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)</span></span>
  * <span data-ttu-id="06e07-151">A magánfelhők létrehozásával kapcsolatos további követően a felhőbeli háló elemek helyen [magánfelhő létrehozása a VMM Alkalmazásban](http://go.microsoft.com/fwlink/p/?LinkId=324953) és a [bemutató: magánfelhők létrehozása a System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).</span><span class="sxs-lookup"><span data-stu-id="06e07-151">After your cloud fabric elements are in place learn about creating private clouds in [Creating a private cloud in VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) and in [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).</span></span>

### <a name="hyper-v-prerequisites"></a><span data-ttu-id="06e07-152">Hyper-V előfeltételei</span><span class="sxs-lookup"><span data-stu-id="06e07-152">Hyper-V prerequisites</span></span>
* <span data-ttu-id="06e07-153">hello Hyper-V gazdakiszolgálók futnia kell az legalább **Windows Server 2012** Hyper-V szerepkörrel vagy **Microsoft Hyper-V Server 2012** és a legújabb frissítések telepítve rendelkezik hello.</span><span class="sxs-lookup"><span data-stu-id="06e07-153">hello host Hyper-V servers must be running at least **Windows Server 2012** with Hyper-V role or **Microsoft Hyper-V Server 2012** and have hello latest updates installed.</span></span>
* <span data-ttu-id="06e07-154">Ha fürtben futtatja a Hyper-V-t, ne feledje, hogy ha statikus IP-címen alapuló fürtöt használ, a rendszer nem hozza létre automatikusan a fürtszervezőt.</span><span class="sxs-lookup"><span data-stu-id="06e07-154">If you're running Hyper-V in a cluster note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="06e07-155">Manuálisan kell tooconfigure hello fürtszervező.</span><span class="sxs-lookup"><span data-stu-id="06e07-155">You'll need tooconfigure hello cluster broker manually.</span></span> <span data-ttu-id="06e07-156">A</span><span class="sxs-lookup"><span data-stu-id="06e07-156">For</span></span>
* <span data-ttu-id="06e07-157">Útmutatásért lásd: [hogyan tooConfigure Hyper-V Replikaszervező](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).</span><span class="sxs-lookup"><span data-stu-id="06e07-157">For instructions see [How tooConfigure Hyper-V Replica Broker](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).</span></span>
* <span data-ttu-id="06e07-158">Bármely Hyper-V gazdakiszolgáló vagy fürt legyen toomanage védelmi szerepelnie kell egy VMM-felhőben.</span><span class="sxs-lookup"><span data-stu-id="06e07-158">Any Hyper-V host server or cluster for which you want toomanage protection must be included in a VMM cloud.</span></span>

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="06e07-159">Hálózatleképezési előfeltételek</span><span class="sxs-lookup"><span data-stu-id="06e07-159">Network mapping prerequisites</span></span>
<span data-ttu-id="06e07-160">Ha a virtuális gépek Azure-ban, hello hálózatleképezés kapcsolatot hoz létre a virtuálisgép-hálózatok hello hello forrás VMM-kiszolgálón és a cél Azure-hálózatok tooenable hello következő:</span><span class="sxs-lookup"><span data-stu-id="06e07-160">When you protect virtual machines in Azure, hello network mapping  maps hello virtual machine networks on hello source VMM server and target Azure networks tooenable hello following:</span></span>

* <span data-ttu-id="06e07-161">Feladatok átadása a hello azonos gépeire hálózati kapcsolódhatnak tooeach más, függetlenül a helyreállítási terv.</span><span class="sxs-lookup"><span data-stu-id="06e07-161">All machines which fail over on hello same network can connect tooeach other, irrespective of which recovery plan they are in.</span></span>
* <span data-ttu-id="06e07-162">Ha egy hálózati átjáró működik hello cél Azure-hálózatot, virtuális gépek tooother a helyszíni virtuális gépek is elérheti.</span><span class="sxs-lookup"><span data-stu-id="06e07-162">If a network gateway is setup on hello target Azure network, virtual machines can connect tooother on-premises virtual machines.</span></span>
* <span data-ttu-id="06e07-163">Ha nem adja meg a hálózatleképezés, csak hello feladatátvételt hello azonos virtuális gépek helyreállítási terv feladatátvételi tooAzure után lesznek képesek tooconnect tooeach más.</span><span class="sxs-lookup"><span data-stu-id="06e07-163">If you don’t configure network mapping, only hello virtual machines that fail over in hello same recovery plan will be able tooconnect tooeach other after fail-over tooAzure.</span></span>

<span data-ttu-id="06e07-164">Ha azt szeretné, hogy toodeploy hálózatra való leképezés, hello következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="06e07-164">If you want toodeploy network mapping you'll need hello following:</span></span>

* <span data-ttu-id="06e07-165">azt szeretné, hogy a forrás VMM-kiszolgálón hello tooprotect hello virtuális gépek csatlakoztatott tooa Virtuálisgép-hálózathoz kell lennie.</span><span class="sxs-lookup"><span data-stu-id="06e07-165">hello virtual machines you want tooprotect on hello source VMM server should be connected tooa VM network.</span></span> <span data-ttu-id="06e07-166">Erre a hálózatra kell csatolt tooa hello felhő társított logikai hálózat.</span><span class="sxs-lookup"><span data-stu-id="06e07-166">That network should be linked tooa logical network that is associated with hello cloud.</span></span>
* <span data-ttu-id="06e07-167">Egy Azure-hálózat toowhich replikált virtuális gépek feladatátvételi után kapcsolódhatnak.</span><span class="sxs-lookup"><span data-stu-id="06e07-167">An Azure network toowhich replicated virtual machines can connect after fail-over.</span></span> <span data-ttu-id="06e07-168">Ezt a hálózatot a feladatátvételi hello időpontjában válasszon ki.</span><span class="sxs-lookup"><span data-stu-id="06e07-168">You'll select this network at hello time of fail-over.</span></span> <span data-ttu-id="06e07-169">hello hello hálózat legyen ugyanabban a régióban, az Azure Site Recovery-előfizetést.</span><span class="sxs-lookup"><span data-stu-id="06e07-169">hello network should be in hello same region as your Azure Site Recovery subscription.</span></span>

<span data-ttu-id="06e07-170">További információ a hálózatra való leképezés</span><span class="sxs-lookup"><span data-stu-id="06e07-170">Learn more about network mapping in</span></span>

* [<span data-ttu-id="06e07-171">Hogyan tooconfigure logikai hálózatok a VMM Alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="06e07-171">How tooconfigure logical networks in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [<span data-ttu-id="06e07-172">Hogyan tooconfigure Virtuálisgép-hálózatokat és átjárókat a VMM-ben</span><span class="sxs-lookup"><span data-stu-id="06e07-172">How tooconfigure VM networks and gateways in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386308)
* [<span data-ttu-id="06e07-173">Hogyan tooconfigure és a figyelő virtuális hálózatok az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="06e07-173">How tooconfigure and monitor virtual networks in Azure</span></span>](https://azure.microsoft.com/documentation/services/virtual-network/)

### <a name="powershell-prerequisites"></a><span data-ttu-id="06e07-174">PowerShell-Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="06e07-174">PowerShell prerequisites</span></span>
<span data-ttu-id="06e07-175">Győződjön meg arról, hogy készen áll az Azure PowerShell toogo.</span><span class="sxs-lookup"><span data-stu-id="06e07-175">Make sure you have Azure PowerShell ready toogo.</span></span> <span data-ttu-id="06e07-176">Ha már használ PowerShell, szüksége lesz a tooupgrade tooversion 0.8.10 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="06e07-176">If you are already using PowerShell, you'll need tooupgrade tooversion 0.8.10 or later.</span></span> <span data-ttu-id="06e07-177">PowerShell beállításával kapcsolatos információkért lásd: hello [tooinstall útmutató, és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="06e07-177">For information about setting up PowerShell, see hello [Guide tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="06e07-178">Miután beállítása és PowerShell konfigurálva, megtekintheti az összes rendelkezésre álló parancsmagok hello hello szolgáltatás [Itt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="06e07-178">Once you have set up and configured PowerShell, you can view all of hello available cmdlets for hello service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="06e07-179">toolearn tippeket, amelyik segíthet a hello parancsmag például paraméterértékeket, bemenetekhez és kimenetekhez általában kezelésének módja az Azure PowerShell használatával kapcsolatban lásd: hello [tooget Started with Azure parancsmagok útmutató](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="06e07-179">toolearn about tips that can help you use hello cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see hello [Guide tooget Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-hello-subscription"></a><span data-ttu-id="06e07-180">1. lépés: Hello előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="06e07-180">Step 1: Set hello subscription</span></span>
1. <span data-ttu-id="06e07-181">Az Azure powershell, Azure-fiók bejelentkezési tooyour: hello a következő parancsmagok használatával</span><span class="sxs-lookup"><span data-stu-id="06e07-181">From Azure powershell, login tooyour Azure account: using hello following cmdlets</span></span>

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. <span data-ttu-id="06e07-182">Az előfizetések listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="06e07-182">Get a list of your subscriptions.</span></span> <span data-ttu-id="06e07-183">Ez is kilistázza hello subscriptionIDs az egyes hello előfizetések.</span><span class="sxs-lookup"><span data-stu-id="06e07-183">This will also list hello subscriptionIDs for each of hello subscriptions.</span></span> <span data-ttu-id="06e07-184">Jegyezze fel, amelyben toocreate hello recovery services-tároló kívánja hello előfizetés hello előfizetés-azonosító</span><span class="sxs-lookup"><span data-stu-id="06e07-184">Note down hello subscriptionID of hello subscription in which you wish toocreate hello recovery services vault</span></span>

        Get-AzureRmSubscription
3. <span data-ttu-id="06e07-185">Mely hello a recovery services-tároló: megemlíteni hello előfizetés-azonosító által létrehozott toobe hello előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="06e07-185">Set hello subscription in which hello recovery services vault is toobe created by mentioning hello subscription ID</span></span>

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a><span data-ttu-id="06e07-186">2. lépés: Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="06e07-186">Step 2: Create a Recovery Services vault</span></span>
1. <span data-ttu-id="06e07-187">Hozzon létre egy erőforráscsoportot az Azure Resource Manager Ha még nem rendelkezik már</span><span class="sxs-lookup"><span data-stu-id="06e07-187">Create a resource group  in Azure Resource Manager if you don't have one already</span></span>

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. <span data-ttu-id="06e07-188">Hozzon létre egy új Recovery Services-tároló, és mentse a létrehozott automatikus rendszer-Helyreállítás tároló objektumhoz (használandó később) változó hello.</span><span class="sxs-lookup"><span data-stu-id="06e07-188">Create a new Recovery Services vault and save hello created ASR vault object in a variable (will be used later).</span></span> <span data-ttu-id="06e07-189">Beolvasni a hello ASR tároló objektum post létrehozása hello Get-AzureRMRecoveryServicesVault parancsmag segítségével:-</span><span class="sxs-lookup"><span data-stu-id="06e07-189">You can also retrieve hello ASR vault object post creation using hello Get-AzureRMRecoveryServicesVault cmdlet:-</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a><span data-ttu-id="06e07-190">3. lépés: Hello Recovery Services-tároló környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="06e07-190">Step 3: Set hello Recovery Services Vault context</span></span>

<span data-ttu-id="06e07-191">Hello tároló környezet beállítása hello alábbi parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="06e07-191">Set hello vault context by running hello below command.</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-hello-azure-site-recovery-provider"></a><span data-ttu-id="06e07-192">4. lépés: Hello Azure Site Recovery Provider telepítése</span><span class="sxs-lookup"><span data-stu-id="06e07-192">Step 4: Install hello Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="06e07-193">Hello VMM-gépen hozzon létre egy könyvtárat hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="06e07-193">On hello VMM machine, create a directory by running hello following command:</span></span>

       New-Item c:\ASR -type directory
2. <span data-ttu-id="06e07-194">Bontsa ki a letöltött hello szolgáltató használatával hello a következő parancs futtatásával hello fájlt</span><span class="sxs-lookup"><span data-stu-id="06e07-194">Extract hello files using hello downloaded provider by running hello following command</span></span>

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. <span data-ttu-id="06e07-195">Hello szolgáltató a következő parancsok hello segítségével telepíti:</span><span class="sxs-lookup"><span data-stu-id="06e07-195">Install hello provider using hello following commands:</span></span>

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

   <span data-ttu-id="06e07-196">Várjon, amíg hello telepítési toofinish.</span><span class="sxs-lookup"><span data-stu-id="06e07-196">Wait for hello installation toofinish.</span></span>
4. <span data-ttu-id="06e07-197">Hello kiszolgáló regisztrálása hello tárolónál hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="06e07-197">Register hello server in hello vault using hello following command:</span></span>

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-an-azure-storage-account"></a><span data-ttu-id="06e07-198">5. lépés: Az Azure storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="06e07-198">Step 5: Create an Azure storage account</span></span>

<span data-ttu-id="06e07-199">Egy Azure storage-fiók nem rendelkezik, ha engedélyezett a georeplikáció-fiók létrehozása a hello hello azonos tárfiókéval tároló hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="06e07-199">If you don't have an Azure storage account, create a geo-replication enabled account in hello same geo as hello vault by running hello following command:</span></span>

        $StorageAccountName = "teststorageacc1"    #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"     
        $ResourceGroupName =  “myRG”             #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

<span data-ttu-id="06e07-200">Vegye figyelembe, hogy hello tárfiókot kell lennie a hello és hello Azure Site Recovery szolgáltatásnak ugyanabban a régióban, és társítva hello ugyanahhoz az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="06e07-200">Note that hello storage account must be in hello same region as hello Azure Site Recovery service, and be associated with hello same subscription.</span></span>

## <a name="step-6-install-hello-azure-recovery-services-agent"></a><span data-ttu-id="06e07-201">6. lépés: Hello Azure Recovery Services Agent telepítése</span><span class="sxs-lookup"><span data-stu-id="06e07-201">Step 6: Install hello Azure Recovery Services Agent</span></span>
1. <span data-ttu-id="06e07-202">Töltse le a hello Azure Recovery Services Agent ügynököt a [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) , és telepítse azt minden Hyper-V gazdagép-kiszolgálón található hello VMM felhők, tooprotect.</span><span class="sxs-lookup"><span data-stu-id="06e07-202">Download hello Azure Recovery Services agent at [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) and install it on each Hyper-V host server located in hello VMM clouds you want tooprotect.</span></span>
2. <span data-ttu-id="06e07-203">Futtassa az alábbi a parancs minden VMM-gazdagép hello:</span><span class="sxs-lookup"><span data-stu-id="06e07-203">Run hello following command on all VMM hosts:</span></span>

       marsagentinstaller.exe /q /nu

## <a name="step-7-configure-cloud-protection-settings"></a><span data-ttu-id="06e07-204">7. lépés: Konfigurálja a felhő védelmi beállításait</span><span class="sxs-lookup"><span data-stu-id="06e07-204">Step 7: Configure cloud protection settings</span></span>
1. <span data-ttu-id="06e07-205">Hozzon létre egy replikációs házirend tooAzure hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="06e07-205">Create a replication policy tooAzure by running hello following command:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

1. <span data-ttu-id="06e07-206">Töltse le a védelmi tároló hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="06e07-206">Get a protection container by running hello following commands:</span></span>

       $PrimaryCloud = "testcloud"
       $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  
2. <span data-ttu-id="06e07-207">Hello házirend részletek tooa változó feldolgozással hello létrehozott és hello rövid házirendnév megemlíteni olvasható be:</span><span class="sxs-lookup"><span data-stu-id="06e07-207">Get hello policy details tooa variable using hello job that was created and mentioning hello friendly policy name:</span></span>

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. <span data-ttu-id="06e07-208">Hello társítás hello védelmi tároló kezdődnie hello replikációs házirendhez:</span><span class="sxs-lookup"><span data-stu-id="06e07-208">Start hello association of hello protection container with hello replication policy:</span></span>

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  
4. <span data-ttu-id="06e07-209">Hello feladat befejeződését követően futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="06e07-209">After hello job has finished, run hello following command:</span></span>

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }
5. <span data-ttu-id="06e07-210">Hello feladat feldolgozása után futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="06e07-210">After hello job has finished processing, run hello following command:</span></span>

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

<span data-ttu-id="06e07-211">hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).</span><span class="sxs-lookup"><span data-stu-id="06e07-211">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-8-configure-network-mapping"></a><span data-ttu-id="06e07-212">8. lépés: Hálózatleképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="06e07-212">Step 8: Configure network mapping</span></span>
<span data-ttu-id="06e07-213">Mielőtt elkezdené hálózatleképezés ellenőrizze, hogy hello forrás VMM-kiszolgálón található virtuális gépek csatlakoztatott tooa Virtuálisgép-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="06e07-213">Before you begin network mapping verify that virtual machines on hello source VMM server are connected tooa VM network.</span></span> <span data-ttu-id="06e07-214">Továbbá hozzon létre legalább egy Azure virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="06e07-214">In addition, create one or more Azure virtual networks.</span></span>

<span data-ttu-id="06e07-215">További tudnivalók hogyan toocreate a virtuális hálózat Azure Resource Manager és a PowerShell, a [virtuális hálózat létrehozása az Azure Resource Manager és a PowerShell használatával pont-pont VPN-kapcsolattal](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="06e07-215">Learn more about how toocreate a virtual network using Azure Resource Manager and PowerShell, in [Create a virtual network with a site-to-site VPN connection using Azure Resource Manager and PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)</span></span>

<span data-ttu-id="06e07-216">Vegye figyelembe, hogy több virtuálisgép-hálózat is csatlakoztatott tooa egyetlen Azure-hálózatra.</span><span class="sxs-lookup"><span data-stu-id="06e07-216">Note that multiple Virtual Machine networks can be mapped tooa single Azure network.</span></span> <span data-ttu-id="06e07-217">Ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép feladatátvételi után lesz csatlakoztatott toothat cél alhálózathoz.</span><span class="sxs-lookup"><span data-stu-id="06e07-217">If hello target network has multiple subnets and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine will be connected toothat target subnet after fail-over.</span></span> <span data-ttu-id="06e07-218">Ha nincsenek egyező nevű cél alhálózathoz, hello virtuális gép lesz a csatlakoztatott toohello hello hálózat első alhálózatát.</span><span class="sxs-lookup"><span data-stu-id="06e07-218">If there is no target subnet with a matching name, hello virtual machine will be connected toohello first subnet in hello network.</span></span>

1. <span data-ttu-id="06e07-219">hello első parancs beolvasása kiszolgálók hello aktuális Azure Site Recovery-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="06e07-219">hello first command gets servers for hello current Azure Site Recovery vault.</span></span> <span data-ttu-id="06e07-220">hello parancs hello Microsoft Azure Site Recovery kiszolgálók hello $Servers tömbváltozó tárolja.</span><span class="sxs-lookup"><span data-stu-id="06e07-220">hello command stores hello Microsoft Azure Site Recovery servers in hello $Servers array variable.</span></span>

        $Servers = Get-AzureRmSiteRecoveryServer
2. <span data-ttu-id="06e07-221">hello második parancs hello hely helyreállítási hálózati hello első kiszolgáló kéri le a hello $Servers tömbben.</span><span class="sxs-lookup"><span data-stu-id="06e07-221">hello second command gets hello site recovery network for hello first server in hello $Servers array.</span></span> <span data-ttu-id="06e07-222">hello parancs hello hálózatok hello $Networks változó tárolja.</span><span class="sxs-lookup"><span data-stu-id="06e07-222">hello command stores hello networks in hello $Networks variable.</span></span>

        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

1. <span data-ttu-id="06e07-223">hello harmadik parancs a hello $AzureVmNetworks változóval lekérdezi az Azure virtuális hálózataihoz, és ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="06e07-223">hello third command gets Azure virtual networks, and then that value in hello $AzureVmNetworks variable.</span></span>

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork
2. <span data-ttu-id="06e07-224">hello végső parancsmag hello elsődleges hálózati és hello Azure virtuális gép hálózati közötti leképezést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="06e07-224">hello final cmdlet creates a mapping between hello primary network and hello Azure virtual machine network.</span></span> <span data-ttu-id="06e07-225">hello parancsmag hello elsődleges hálózati $Networks hello első eleme határozza meg.</span><span class="sxs-lookup"><span data-stu-id="06e07-225">hello cmdlet specifies hello primary network as hello first element of $Networks.</span></span> <span data-ttu-id="06e07-226">hello parancsmag egy virtuális gép hálózati $AzureVmNetworks hello első eleme határozza meg.</span><span class="sxs-lookup"><span data-stu-id="06e07-226">hello cmdlet specifies a virtual machine network as hello first element of $AzureVmNetworks.</span></span>

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]

## <a name="step-9-enable-protection-for-virtual-machines"></a><span data-ttu-id="06e07-227">9. lépés: Virtuális gépek védelmének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="06e07-227">Step 9: Enable protection for virtual machines</span></span>
<span data-ttu-id="06e07-228">Hello kiszolgálók, felhők és hálózatok megfelelő konfigurálását követően engedélyezheti a hello felhő virtuális gépek védelmét.</span><span class="sxs-lookup"><span data-stu-id="06e07-228">After hello servers, clouds and networks are configured correctly, you can enable protection for virtual machines in hello cloud.</span></span>

 <span data-ttu-id="06e07-229">Vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="06e07-229">Note hello following:</span></span>

* <span data-ttu-id="06e07-230">Virtuális gépek Azure-követelményeknek kell megfelelniük.</span><span class="sxs-lookup"><span data-stu-id="06e07-230">Virtual machines must meet Azure requirements.</span></span> <span data-ttu-id="06e07-231">Ellenőrizze, hogy ezek szerepel [Előfeltételek és a támogatás](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) hello tervezési útmutató.</span><span class="sxs-lookup"><span data-stu-id="06e07-231">Check these in [Prerequisites and Support](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) in hello planning guide.</span></span>
* <span data-ttu-id="06e07-232">hello virtuális gép tooenable védelmi, hello operációsrendszer- és operációsrendszer-lemez tulajdonságait kell állítani.</span><span class="sxs-lookup"><span data-stu-id="06e07-232">tooenable protection, hello operating system and operating system disk properties must be set for hello virtual machine.</span></span> <span data-ttu-id="06e07-233">Ha egy virtuális gép létrehozása a VMM-virtuálisgép-sablon használatával beállíthatja hello tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="06e07-233">When you create a virtual machine in VMM using a virtual machine template you can set hello property.</span></span> <span data-ttu-id="06e07-234">Ezeket a tulajdonságokat a meglévő virtuális gépek állítsa be a hello **általános** és **hardverkonfiguráció** hello virtuális gép tulajdonságok lapján.</span><span class="sxs-lookup"><span data-stu-id="06e07-234">You can also set these properties for existing virtual machines on hello **General** and **Hardware Configuration** tabs of hello virtual machine properties.</span></span> <span data-ttu-id="06e07-235">Ha ezeket a tulajdonságokat a VMM-ben nem állít be fog tudni tooconfigure őket hello Azure Site Recovery portálon.</span><span class="sxs-lookup"><span data-stu-id="06e07-235">If you don't set these properties in VMM you'll be able tooconfigure them in hello Azure Site Recovery portal.</span></span>

1. <span data-ttu-id="06e07-236">Futtassa a következő parancs tooget hello védelmi tároló hello tooenable védelem:</span><span class="sxs-lookup"><span data-stu-id="06e07-236">tooenable protection, run hello following command tooget hello protection container:</span></span>

          $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName
2. <span data-ttu-id="06e07-237">Töltse le a hello védelmi entitás (VM) hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="06e07-237">Get hello protection entity (VM) by running hello following command:</span></span>

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer
3. <span data-ttu-id="06e07-238">A virtuális gép hello vész-Helyreállítási hello engedélyezése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="06e07-238">Enable hello DR for hello VM by running hello following command:</span></span>

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1

## <a name="test-your-deployment"></a><span data-ttu-id="06e07-239">A központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="06e07-239">Test your deployment</span></span>
<span data-ttu-id="06e07-240">tootest a központi telepítés egy vizsgálat futtatása egy egyetlen virtuális gép feladatátvételi hozhat létre több virtuális gépet tartalmazó helyreállítási tervet, illetve a teszt feladatátvételi hello terv futtatása.</span><span class="sxs-lookup"><span data-stu-id="06e07-240">tootest your deployment you can run a test fail-over for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test fail-over for hello plan.</span></span> <span data-ttu-id="06e07-241">Feladatátvételi teszt elszigetelt hálózatot a feladatátvételi és helyreállítási mechanizmusokat szimulálja.</span><span class="sxs-lookup"><span data-stu-id="06e07-241">Test fail-over simulates your fail-over and recovery mechanism in an isolated network.</span></span> <span data-ttu-id="06e07-242">Vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="06e07-242">Note that:</span></span>

* <span data-ttu-id="06e07-243">Ha azt szeretné, hogy a távoli asztali kapcsolat segítségével után hello feladatátvételi tooconnect toohello virtuális gépet, engedélyezze a távoli asztali kapcsolat hello virtuális gépen, hello a feladatátvételi teszt futtatása előtt.</span><span class="sxs-lookup"><span data-stu-id="06e07-243">If you want tooconnect toohello virtual machine in Azure using Remote Desktop after hello fail-over, enable Remote Desktop Connection on hello virtual machine before you run hello test failover.</span></span>
* <span data-ttu-id="06e07-244">Után feladatátvételi egy nyilvános IP cím tooconnect toohello virtuális gépet fog használni a távoli asztali kapcsolat segítségével.</span><span class="sxs-lookup"><span data-stu-id="06e07-244">After fail-over, you'll use a public IP address tooconnect toohello virtual machine in Azure using Remote Desktop.</span></span> <span data-ttu-id="06e07-245">Ha meg akarja toodo, győződjön meg arról, nincs érvényben tartományi szabályzatok, amelyek meggátolják, kapcsolódó tooa virtuális gép nyilvános cím segítségével.</span><span class="sxs-lookup"><span data-stu-id="06e07-245">If you want toodo this, ensure you don't have any domain policies that prevent you from connecting tooa virtual machine using a public address.</span></span>

<span data-ttu-id="06e07-246">hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).</span><span class="sxs-lookup"><span data-stu-id="06e07-246">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

### <a name="run-a-test-failover"></a><span data-ttu-id="06e07-247">Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="06e07-247">Run a test failover</span></span>
- <span data-ttu-id="06e07-248">Indítsa el a hello feladatátvételi teszthez hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="06e07-248">Start hello test failover by running hello following command:</span></span>

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a><span data-ttu-id="06e07-249">Tervezett feladatátvétel futtatása</span><span class="sxs-lookup"><span data-stu-id="06e07-249">Run a planned failover</span></span>
- <span data-ttu-id="06e07-250">Indítsa el a hello tervezett feladatátvétel hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="06e07-250">Start hello planned failover by running hello following command:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a><span data-ttu-id="06e07-251">A nem tervezett feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="06e07-251">Run an unplanned failover</span></span>
- <span data-ttu-id="06e07-252">Indítsa el a nem tervezett feladatátvétel hello hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="06e07-252">Start hello unplanned failover by running hello following command:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

## <span data-ttu-id="06e07-253"><a name=monitor></a>Figyelési tevékenység</span><span class="sxs-lookup"><span data-stu-id="06e07-253"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="06e07-254">A következő parancsok toomonitor hello tevékenység hello használata.</span><span class="sxs-lookup"><span data-stu-id="06e07-254">Use hello following commands toomonitor hello activity.</span></span> <span data-ttu-id="06e07-255">Vegye figyelembe, hogy rendelkezik-e toowait Between hello feldolgozási toofinish feladataihoz.</span><span class="sxs-lookup"><span data-stu-id="06e07-255">Note that you have toowait in between jobs for hello processing toofinish.</span></span>

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a><span data-ttu-id="06e07-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="06e07-256">Next steps</span></span>
<span data-ttu-id="06e07-257">[További](/powershell/module/azurerm.recoveryservices.backup/#recovery) Azure Site Recovery Azure Resource Manager PowerShell-parancsmagokkal kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="06e07-257">[Read more](/powershell/module/azurerm.recoveryservices.backup/#recovery) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
