---
title: "Hyper-V virtuális gépek VMM tooa másodlagos hely PowerShell (Azure Resource Manager) aaaReplicate |} Microsoft Docs"
description: "Ismerteti, hogyan toodeploy Azure Site Recovery tooorchestrate replikáció, feladatátvétel és a VMM-ben a Hyper-V virtuális gépek helyreállítási felhők tooa VMM másodlagos hely powershellel (Resource Manager)"
services: site-recovery
documentationcenter: 
author: sujaytalasila
manager: rochakm
editor: raynew
ms.assetid: 9d38e9c3-217c-4e44-830c-575e9a4141f2
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: a769dcc68d66c18b9dc47539071f4d0e0f1db70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site-using-powershell-resource-manager"></a><span data-ttu-id="381ac-103">A VMM felhők tooa VMM másodlagos hely PowerShell (Resource Manager) segítségével a Hyper-V virtuális gépek replikálása</span><span class="sxs-lookup"><span data-stu-id="381ac-103">Replicate Hyper-V virtual machines in VMM clouds tooa secondary VMM site using PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="381ac-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="381ac-104">Azure Portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="381ac-105">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="381ac-105">Classic Portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="381ac-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="381ac-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="381ac-107">Üdvözli a Site Recovery tooAzure!</span><span class="sxs-lookup"><span data-stu-id="381ac-107">Welcome tooAzure Site Recovery!</span></span> <span data-ttu-id="381ac-108">Ez a cikk akkor használja, ha azt szeretné, hogy tooreplicate helyszíni Hyper-V virtuális gépek kezelése a System Center Virtual Machine Manager (VMM) felhők tooa másodlagos helyen.</span><span class="sxs-lookup"><span data-stu-id="381ac-108">Use this article if you want tooreplicate on-premises Hyper-V  virtual machines managed in System Center Virtual Machine Manager (VMM) clouds tooa secondary site.</span></span>

<span data-ttu-id="381ac-109">Ez a cikk bemutatja, hogyan toouse PowerShell tooautomate gyakori feladatokat kell tooperform beállításakor Azure Site Recovery tooreplicate Hyper-V virtuális gépek VMM-felhőkben System Center VMM felhők tooSystem Center másodlagos helyen.</span><span class="sxs-lookup"><span data-stu-id="381ac-109">This article shows you how toouse PowerShell tooautomate common tasks you need tooperform when you set up Azure Site Recovery tooreplicate Hyper-V virtual machines in System Center VMM clouds tooSystem Center VMM clouds in secondary site.</span></span>

<span data-ttu-id="381ac-110">hello cikk hello forgatókönyv előfeltételei tartalmazza, és megjeleníti a</span><span class="sxs-lookup"><span data-stu-id="381ac-110">hello article includes prerequisites for hello scenario, and shows you</span></span>

* <span data-ttu-id="381ac-111">Hogyan tooset mentést a Recovery Services-tároló</span><span class="sxs-lookup"><span data-stu-id="381ac-111">How tooset up a Recovery Services Vault</span></span>
* <span data-ttu-id="381ac-112">Hello Azure Site Recovery Provider telepítése hello forrás VMM-kiszolgálón, és hello cél VMM-kiszolgálóval</span><span class="sxs-lookup"><span data-stu-id="381ac-112">Install hello Azure Site Recovery Provider on hello source VMM server and hello target VMM server</span></span>
* <span data-ttu-id="381ac-113">Regisztrálja hello tárolóban hello VMM kiszolgáló (k)</span><span class="sxs-lookup"><span data-stu-id="381ac-113">Register hello VMM server(s) in hello vault</span></span>
* <span data-ttu-id="381ac-114">Replikációs szabályzat hello VMM-felhő konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="381ac-114">Configure replication policy for hello VMM Cloud.</span></span> <span data-ttu-id="381ac-115">hello replikációs beállítások vannak megadva hello házirend lesz alkalmazott tooall védett virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="381ac-115">hello replication settings in hello policy will be applied tooall protected virtual machines</span></span>
* <span data-ttu-id="381ac-116">Hello virtuális gépek védelmének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="381ac-116">Enable protection for hello virtual machines.</span></span>
* <span data-ttu-id="381ac-117">Hello feladatátvételi teszt virtuális gépek egyenként vagy a helyreállítási terv toomake meg arról, hogy minden a várt módon működik.</span><span class="sxs-lookup"><span data-stu-id="381ac-117">Test hello failover of VMs individually or as part of a recovery plan toomake sure everything is working as expected.</span></span>
* <span data-ttu-id="381ac-118">Hajtsa végre a tervezett vagy nem tervezett feladatátvételt a virtuális gépek egyenként vagy a helyreállítási terv toomake meg arról, hogy minden a várt módon működik részeként.</span><span class="sxs-lookup"><span data-stu-id="381ac-118">Perform a planned or an unplanned failover of VMs individually or as part of a recovery plan toomake sure everything is working as expected.</span></span>

<span data-ttu-id="381ac-119">Ha ez a forgatókönyv beállítása problémát tapasztal, kérdéseit a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="381ac-119">If you run into problems setting up this scenario, post your questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="381ac-120">Az Azure két különböző [üzembe helyezési modellt](../azure-resource-manager/resource-manager-deployment-model.md) kínál az erőforrások létrehozására és kezelésére: az Azure Resource Manager-modellt és a klasszikus modellt.</span><span class="sxs-lookup"><span data-stu-id="381ac-120">Azure has two different [deployment models](../azure-resource-manager/resource-manager-deployment-model.md) for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="381ac-121">Azure a két portál – hello a klasszikus Azure portálra, hello klasszikus üzembe helyezési modellt támogató, és a két üzembe helyezési modell támogató Azure-portálon hello is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="381ac-121">Azure also has two portals – hello Azure classic portal that supports hello classic deployment model, and hello Azure portal with support for both deployment models.</span></span> <span data-ttu-id="381ac-122">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="381ac-122">This article covers hello Resource Manager deployment model.</span></span>
>
>

## <a name="on-premises-prerequisites"></a><span data-ttu-id="381ac-123">Helyszíni előfeltételek</span><span class="sxs-lookup"><span data-stu-id="381ac-123">On-premises prerequisites</span></span>
<span data-ttu-id="381ac-124">Íme a következőkre lesz szüksége a hello elsődleges és másodlagos helyszíni helyek toodeploy ebben a forgatókönyvben:</span><span class="sxs-lookup"><span data-stu-id="381ac-124">Here's what you'll need in hello primary and secondary on-premises sites toodeploy this scenario:</span></span>

| <span data-ttu-id="381ac-125">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="381ac-125">**Prerequisites**</span></span> | <span data-ttu-id="381ac-126">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="381ac-126">**Details**</span></span> |
| --- | --- |
| <span data-ttu-id="381ac-127">**VMM**</span><span class="sxs-lookup"><span data-stu-id="381ac-127">**VMM**</span></span> |<span data-ttu-id="381ac-128">Azt javasoljuk, hogy a VMM-kiszolgáló hello elsődleges hely és a VMM-kiszolgáló hello másodlagos hely telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="381ac-128">We recommend you deploy a VMM server in hello primary site and a VMM server in hello secondary site.</span></span><br/><br/> <span data-ttu-id="381ac-129">Emellett [replikálása egy VMM-kiszolgálón felhők közötti](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span><span class="sxs-lookup"><span data-stu-id="381ac-129">You can also [replicate between clouds on a single VMM server](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span></span> <span data-ttu-id="381ac-130">toodo ez szüksége lesz legalább két hello VMM-kiszolgálón konfigurált felhők.</span><span class="sxs-lookup"><span data-stu-id="381ac-130">toodo this you'll need at least two clouds configured on hello VMM server.</span></span><br/><br/> <span data-ttu-id="381ac-131">VMM-kiszolgálókon legalább futnia kell a System Center 2012 SP1 hello legújabb frissítéseit.</span><span class="sxs-lookup"><span data-stu-id="381ac-131">VMM servers should be running at least System Center 2012 SP1 with hello latest updates.</span></span><br/><br/> <span data-ttu-id="381ac-132">Minden VMM-kiszolgálót rendelkeznie kell egy vagy több konfigurált felhők és az összes felhő beállítása hello Hyper-V kapacitás profillal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="381ac-132">Each VMM server must have at one or more clouds configured and all clouds must have hello Hyper-V Capacity profile set.</span></span> <br/><br/><span data-ttu-id="381ac-133">Felhő legalább egy VMM-gazdagépcsoportot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="381ac-133">Clouds must contain one or more VMM host groups.</span></span><br/><br/><span data-ttu-id="381ac-134">További információk a VMM-felhőkről beállításáról [konfigurálása hello VMM felhőbeli háló](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), és [bemutató: magánfelhők létrehozása a System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span><span class="sxs-lookup"><span data-stu-id="381ac-134">Learn more about setting up VMM clouds in [Configuring hello VMM cloud fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), and [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span></span><br/><br/> <span data-ttu-id="381ac-135">VMM-kiszolgálókon internet-hozzáféréssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="381ac-135">VMM servers should have internet access.</span></span> |
| <span data-ttu-id="381ac-136">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="381ac-136">**Hyper-V**</span></span> |<span data-ttu-id="381ac-137">Hyper-V kiszolgálók legalább futnia kell hello Hyper-V szerepkör és a Windows Server 2012 és a legújabb frissítések telepítve rendelkezik hello.</span><span class="sxs-lookup"><span data-stu-id="381ac-137">Hyper-V servers must be running at least Windows Server 2012 with hello Hyper-V role and have hello latest updates installed.</span></span><br/><br/> <span data-ttu-id="381ac-138">Minden Hyper-V kiszolgáló tartalmazzon legalább egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="381ac-138">A Hyper-V server should contain one or more VMs.</span></span><br/><br/>  <span data-ttu-id="381ac-139">Hyper-V gazdakiszolgálók gazdagépcsoportok hello elsődleges és másodlagos VMM-felhőkben kell elhelyezkedniük.</span><span class="sxs-lookup"><span data-stu-id="381ac-139">Hyper-V host servers should be located in host groups in hello primary and secondary VMM clouds.</span></span><br/><br/> <span data-ttu-id="381ac-140">Ha futtatja a Hyper-V fürt Windows Server 2012 R2 rendszeren kell telepítenie [2961977 frissítése](https://support.microsoft.com/kb/2961977)</span><span class="sxs-lookup"><span data-stu-id="381ac-140">If you're running Hyper-V in a cluster on Windows Server 2012 R2 you should install [update 2961977](https://support.microsoft.com/kb/2961977)</span></span><br/><br/> <span data-ttu-id="381ac-141">Ha futtatja a Hyper-V fürt Windows Server 2012 Megjegyzés: a fürtszervező nem hozza létre automatikusan Ha egy statikus IP cím alapú fürtöt.</span><span class="sxs-lookup"><span data-stu-id="381ac-141">If you're running Hyper-V in a cluster on Windows Server 2012 note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="381ac-142">Manuálisan kell tooconfigure hello fürtszervező.</span><span class="sxs-lookup"><span data-stu-id="381ac-142">You'll need tooconfigure hello cluster broker manually.</span></span> <span data-ttu-id="381ac-143">[További információk](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span><span class="sxs-lookup"><span data-stu-id="381ac-143">[Read more](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span></span> |
| <span data-ttu-id="381ac-144">**Szolgáltató**</span><span class="sxs-lookup"><span data-stu-id="381ac-144">**Provider**</span></span> |<span data-ttu-id="381ac-145">Site Recovery üzembe helyezése során hello Azure Site Recovery Providert a VMM-kiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="381ac-145">During Site Recovery deployment you install hello Azure Site Recovery Provider on VMM servers.</span></span> <span data-ttu-id="381ac-146">hello szolgáltató HTTPS 443-as tooorchestrate replikációs keresztül kommunikál a Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="381ac-146">hello Provider communicates with Site Recovery over HTTPS 443 tooorchestrate replication.</span></span> <span data-ttu-id="381ac-147">Adatok replikáció hello elsődleges és másodlagos Hyper-V kiszolgálók hello LAN keresztül vagy a VPN-kapcsolat között.</span><span class="sxs-lookup"><span data-stu-id="381ac-147">Data replication occurs between hello primary and secondary Hyper-V servers over hello LAN or a VPN connection.</span></span><br/><br/> <span data-ttu-id="381ac-148">hello hello VMM-kiszolgálón futó Provider kell elérni toothese URL-címek: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="381ac-148">hello Provider running on hello VMM server needs access toothese URLs: *.hypervrecoverymanager.windowsazure.com; *.accesscontrol.windows.net; *.backup.windowsazure.com; *.blob.core.windows.net; *.store.core.windows.net.</span></span><br/><br/> <span data-ttu-id="381ac-149">Ezenkívül használhatja a tűzfal kommunikációra a VMM-kiszolgálók toohello hello [Azure datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és hello HTTPS (443) protokollt engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="381ac-149">In addition allow firewall communication from hello VMM servers toohello [Azure datacenter IP ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) and allow hello HTTPS (443) protocol.</span></span> |

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="381ac-150">Hálózatleképezési előfeltételek</span><span class="sxs-lookup"><span data-stu-id="381ac-150">Network mapping prerequisites</span></span>
<span data-ttu-id="381ac-151">Hálózatleképezés kapcsolatot hoz létre hello elsődleges és másodlagos VMM-kiszolgálókon a VMM-Virtuálisgép-hálózatok között:</span><span class="sxs-lookup"><span data-stu-id="381ac-151">Network mapping maps between VMM VM networks on hello primary and secondary VMM servers to:</span></span>

* <span data-ttu-id="381ac-152">Optimális helyezze el a replika virtuális gépek másodlagos Hyper-V-gazdagépek a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="381ac-152">Optimally place replica VMs on secondary Hyper-V hosts after failover.</span></span>
* <span data-ttu-id="381ac-153">Csatlakoztassa a replika virtuális gépek tooappropriate Virtuálisgép-hálózatok.</span><span class="sxs-lookup"><span data-stu-id="381ac-153">Connect replica VMs tooappropriate VM networks.</span></span>
* <span data-ttu-id="381ac-154">Ha nem konfigurálja a hálózati leképezése a replika virtuális gépek nem csatlakoztatott tooany hálózati feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="381ac-154">If you don't configure network mapping replica VMs won't be connected tooany network after failover.</span></span>
* <span data-ttu-id="381ac-155">Ha azt szeretné, hogy a hálózat tooset leképezése során a Site Recovery telepítési itt szolgáltatás következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="381ac-155">If you want tooset up network mapping during Site Recovery deployment here's what you'll need:</span></span>

  * <span data-ttu-id="381ac-156">Győződjön meg arról, hogy hello forrás Hyper-V gazdakiszolgálón futó virtuális gépek csatlakoztatott tooa VMM Virtuálisgép-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="381ac-156">Make sure that VMs on hello source Hyper-V host server are connected tooa VMM VM network.</span></span> <span data-ttu-id="381ac-157">Erre a hálózatra kell csatolt tooa hello felhő társított logikai hálózat.</span><span class="sxs-lookup"><span data-stu-id="381ac-157">That network should be linked tooa logical network that is associated with hello cloud.</span></span>
  * <span data-ttu-id="381ac-158">Győződjön meg arról, hogy hello fogja használni a helyreállításhoz másodlagos felhő rendelkezik-e a megfelelő Virtuálisgép-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="381ac-158">Verify that hello secondary cloud that you'll use for recovery has a corresponding VM network configured.</span></span> <span data-ttu-id="381ac-159">A Virtuálisgép-hálózatnak hello másodlagos felhőhöz társított logikai hálózati csatolt tooa kell lennie.</span><span class="sxs-lookup"><span data-stu-id="381ac-159">That VM network should be linked tooa logical network that's associated with hello secondary cloud.</span></span>

<span data-ttu-id="381ac-160">További információ az alábbi cikkek hello VMM hálózatok konfigurálásáról</span><span class="sxs-lookup"><span data-stu-id="381ac-160">Learn more about configuring VMM networks in hello below articles</span></span>

* [<span data-ttu-id="381ac-161">Hogyan tooconfigure logikai hálózatok a VMM Alkalmazásban</span><span class="sxs-lookup"><span data-stu-id="381ac-161">How tooconfigure logical networks in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [<span data-ttu-id="381ac-162">Hogyan tooconfigure Virtuálisgép-hálózatokat és átjárókat a VMM-ben</span><span class="sxs-lookup"><span data-stu-id="381ac-162">How tooconfigure VM networks and gateways in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386308)

<span data-ttu-id="381ac-163">[További információk](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) a hálózatleképezés működéséről.</span><span class="sxs-lookup"><span data-stu-id="381ac-163">[Learn more](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) about how network mapping works.</span></span>

### <a name="powershell-prerequisites"></a><span data-ttu-id="381ac-164">PowerShell-Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="381ac-164">PowerShell prerequisites</span></span>
<span data-ttu-id="381ac-165">Győződjön meg arról, hogy készen áll az Azure PowerShell toogo.</span><span class="sxs-lookup"><span data-stu-id="381ac-165">Make sure you have Azure PowerShell ready toogo.</span></span> <span data-ttu-id="381ac-166">Ha már használ PowerShell, szüksége lesz a tooupgrade tooversion 0.8.10 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="381ac-166">If you are already using PowerShell, you'll need tooupgrade tooversion 0.8.10 or later.</span></span> <span data-ttu-id="381ac-167">PowerShell beállításával kapcsolatos információkért lásd: hello [tooinstall útmutató, és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="381ac-167">For information about setting up PowerShell, see hello [Guide tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="381ac-168">Miután beállítása és PowerShell konfigurálva, megtekintheti az összes rendelkezésre álló parancsmagok hello hello szolgáltatás [Itt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="381ac-168">Once you have set up and configured PowerShell, you can view all of hello available cmdlets for hello service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="381ac-169">toolearn tippeket, amelyik segíthet a hello parancsmag például paraméterértékeket, bemenetekhez és kimenetekhez általában kezelésének módja az Azure PowerShell használatával kapcsolatban lásd: hello [tooget Started with Azure parancsmagok útmutató](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="381ac-169">toolearn about tips that can help you use hello cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see hello [Guide tooget Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-hello-subscription"></a><span data-ttu-id="381ac-170">1. lépés: Hello előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="381ac-170">Step 1: Set hello subscription</span></span>
1. <span data-ttu-id="381ac-171">Az Azure powershell, Azure-fiók bejelentkezési tooyour: hello a következő parancsmagok használatával</span><span class="sxs-lookup"><span data-stu-id="381ac-171">From Azure powershell, login tooyour Azure account: using hello following cmdlets</span></span>

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. <span data-ttu-id="381ac-172">Az előfizetések listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="381ac-172">Get a list of your subscriptions.</span></span> <span data-ttu-id="381ac-173">Ez is kilistázza hello subscriptionIDs az egyes hello előfizetések.</span><span class="sxs-lookup"><span data-stu-id="381ac-173">This will also list hello subscriptionIDs for each of hello subscriptions.</span></span> <span data-ttu-id="381ac-174">Jegyezze fel, amelyben toocreate hello recovery services-tároló kívánja hello előfizetés hello előfizetés-azonosító</span><span class="sxs-lookup"><span data-stu-id="381ac-174">Note down hello subscriptionID of hello subscription in which you wish toocreate hello recovery services vault</span></span>    

        Get-AzureRmSubscription
3. <span data-ttu-id="381ac-175">Mely hello a recovery services-tároló: megemlíteni hello előfizetés-azonosító által létrehozott toobe hello előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="381ac-175">Set hello subscription in which hello recovery services vault is toobe created by mentioning hello subscription ID</span></span>

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a><span data-ttu-id="381ac-176">2. lépés: Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="381ac-176">Step 2: Create a Recovery Services vault</span></span>
1. <span data-ttu-id="381ac-177">Ha már nincs ilyen, hozzon létre egy Azure Resource Manager erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="381ac-177">Create an Azure Resource Manager resource group if you don't have one already</span></span>

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. <span data-ttu-id="381ac-178">Hozzon létre egy új Recovery Services-tároló, és mentse a létrehozott automatikus rendszer-Helyreállítás tároló objektumhoz (használandó később) változó hello.</span><span class="sxs-lookup"><span data-stu-id="381ac-178">Create a new Recovery Services vault and save hello created ASR vault object in a variable (will be used later).</span></span> <span data-ttu-id="381ac-179">Beolvasni a hello ASR tároló objektum post létrehozása hello Get-AzureRMRecoveryServicesVault parancsmag segítségével:-</span><span class="sxs-lookup"><span data-stu-id="381ac-179">You can also retrieve hello ASR vault object post creation using hello Get-AzureRMRecoveryServicesVault cmdlet:-</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a><span data-ttu-id="381ac-180">3. lépés: Hello Recovery Services-tároló környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="381ac-180">Step 3: Set hello Recovery Services Vault context</span></span>
1. <span data-ttu-id="381ac-181">Ha egy már létrehozott tároló, futtassa az alábbi parancs tooget hello tároló hello.</span><span class="sxs-lookup"><span data-stu-id="381ac-181">If you have a vault already created, run hello below command tooget hello vault.</span></span>

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. <span data-ttu-id="381ac-182">Hello tároló környezet beállítása hello alábbi parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="381ac-182">Set hello vault context by running hello below command.</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-hello-azure-site-recovery-provider"></a><span data-ttu-id="381ac-183">4. lépés: Hello Azure Site Recovery Provider telepítése</span><span class="sxs-lookup"><span data-stu-id="381ac-183">Step 4: Install hello Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="381ac-184">Hello VMM-gépen hozzon létre egy könyvtárat hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-184">On hello VMM machine, create a directory by running hello following command:</span></span>

       New-Item c:\ASR -type directory
2. <span data-ttu-id="381ac-185">Bontsa ki a letöltött hello szolgáltató használatával hello a következő parancs futtatásával hello fájlt</span><span class="sxs-lookup"><span data-stu-id="381ac-185">Extract hello files using hello downloaded provider by running hello following command</span></span>

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. <span data-ttu-id="381ac-186">Hello szolgáltató a következő parancsok hello segítségével telepíti:</span><span class="sxs-lookup"><span data-stu-id="381ac-186">Install hello provider using hello following commands:</span></span>

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

   <span data-ttu-id="381ac-187">Várjon, amíg hello telepítési toofinish.</span><span class="sxs-lookup"><span data-stu-id="381ac-187">Wait for hello installation toofinish.</span></span>
4. <span data-ttu-id="381ac-188">Hello kiszolgáló regisztrálása hello tárolónál hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="381ac-188">Register hello server in hello vault using hello following command:</span></span>

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-and-associate-a-replication-policy"></a><span data-ttu-id="381ac-189">5. lépés: Házirend létrehozása és társítása a replikációs</span><span class="sxs-lookup"><span data-stu-id="381ac-189">Step 5: Create and associate a replication policy</span></span>
1. <span data-ttu-id="381ac-190">Hozzon létre egy Hyper-V 2012 R2-ben replikációs házirendet hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-190">Create a Hyper-V 2012 R2 replication policy by running hello following command:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify hello number of hours tooretain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify hello frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify hello port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > <span data-ttu-id="381ac-191">VMM-felhő hello (ahogy azt hello Hyper-V előfeltételei) Windows Server különböző verzióit futtató Hyper-V-gazdagépek is tartalmazhat, de hello replikációs házirend adott operációsrendszer-verzió.</span><span class="sxs-lookup"><span data-stu-id="381ac-191">hello VMM cloud can contain Hyper-V hosts running different versions of Windows Server (as mentioned in hello Hyper-V prerequisites), but hello replication policy is OS version specific.</span></span> <span data-ttu-id="381ac-192">Ha a különböző operációs rendszereken futó különböző gazdagépeken, majd hozza létre külön replikációs házirendet minden operációs rendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="381ac-192">If you have different hosts running on different operating system versions, then create separate replication policies for each type of OS version.</span></span> <span data-ttu-id="381ac-193">A pl.: Ha öt gazdagépeken futó Windows-kiszolgálók 2012 és Windows Server 2012 R2 három, létre kell hoznia két replikációs házirendek – egyet az egyes operációs rendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="381ac-193">For eg: If you have five hosts running on Windows Servers 2012 and three on Windows Server 2012 R2, create two replication polices – one for each type of operating system versions.</span></span>

1. <span data-ttu-id="381ac-194">Töltse le a hello elsődleges védelmi tároló (elsődleges VMM-felhőben) és a helyreállítási védelmi tároló (helyreállítási VMM-felhőben) hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-194">Get hello primary protection container (primary VMM Cloud) and recovery protection container (recovery VMM Cloud) by running hello following commands:</span></span>

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. <span data-ttu-id="381ac-195">1. lépés használatával létrehozott hello házirend beolvasása hello hello házirend rövid neve</span><span class="sxs-lookup"><span data-stu-id="381ac-195">Retrieve hello policy you created in step 1 using hello friendly name of hello policy</span></span>

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. <span data-ttu-id="381ac-196">Hello társítás hello védelmi tároló (VMM-felhőben) kezdődnie hello replikációs házirendhez:</span><span class="sxs-lookup"><span data-stu-id="381ac-196">Start hello association of hello protection container (VMM Cloud) with hello replication policy:</span></span>

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. <span data-ttu-id="381ac-197">Várjon, amíg hello házirend társítása feladat toocomplete.</span><span class="sxs-lookup"><span data-stu-id="381ac-197">Wait for hello policy association job toocomplete.</span></span> <span data-ttu-id="381ac-198">Ha a hello feladat befejeződött, a következő PowerShell-részlet hello segítségével ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="381ac-198">You can check if hello job has completed using hello following PowerShell snippet.</span></span>

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   <span data-ttu-id="381ac-199">Hello feladat feldolgozása után futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="381ac-199">After hello job has finished processing, run hello following command:</span></span>

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

<span data-ttu-id="381ac-200">hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).</span><span class="sxs-lookup"><span data-stu-id="381ac-200">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-6-configure-network-mapping"></a><span data-ttu-id="381ac-201">6. lépés: Hálózatleképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="381ac-201">Step 6: Configure network mapping</span></span>
1. <span data-ttu-id="381ac-202">hello első parancs beolvasása kiszolgálók hello aktuális Azure Site Recovery-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="381ac-202">hello first command gets servers for hello current Azure Site Recovery vault.</span></span> <span data-ttu-id="381ac-203">hello parancs hello Microsoft Azure Site Recovery kiszolgálók hello $Servers tömbváltozó tárolja.</span><span class="sxs-lookup"><span data-stu-id="381ac-203">hello command stores hello Microsoft Azure Site Recovery servers in hello $Servers array variable.</span></span>

        $Servers = Get-AzureRmSiteRecoveryServer
2. <span data-ttu-id="381ac-204">alább parancsok hello futtasson hello hely helyreállítási hálózati hello forrás VMM-kiszolgálót és hello cél VMM-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="381ac-204">hello below commands get hello site recovery network for hello source VMM server and hello target VMM server.</span></span>

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > <span data-ttu-id="381ac-205">hello forrás VMM-kiszolgálót is kell hello elsőt vagy hello második egy a hello kiszolgálók tömb.</span><span class="sxs-lookup"><span data-stu-id="381ac-205">hello source VMM server can be hello first one or hello second one in hello servers array.</span></span> <span data-ttu-id="381ac-206">Ellenőrizze a hello hello VMM-kiszolgáló nevét, és hello hálózatok megfelelően beolvasása</span><span class="sxs-lookup"><span data-stu-id="381ac-206">Check hello names of hello VMM servers and get hello networks appropriately</span></span>


1. <span data-ttu-id="381ac-207">hello végső parancsmag hello elsődleges és hello helyreállítási hálózat közötti leképezést hoz létre.</span><span class="sxs-lookup"><span data-stu-id="381ac-207">hello final cmdlet creates a mapping between hello primary network and hello recovery network.</span></span> <span data-ttu-id="381ac-208">hello parancsmag hello elsődleges hálózati hello $PrimaryNetworks és hello helyreállítási hálózaton: hello $RecoveryNetworks első elemének első eleme határozza meg.</span><span class="sxs-lookup"><span data-stu-id="381ac-208">hello cmdlet specifies hello primary network as hello first element of $PrimaryNetworks and hello recovery network as hello first element of $RecoveryNetworks.</span></span>

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a><span data-ttu-id="381ac-209">7. lépés: Konfigurálja a tárolási leképezése</span><span class="sxs-lookup"><span data-stu-id="381ac-209">Step 7: Configure storage mapping</span></span>
1. <span data-ttu-id="381ac-210">hello alábbi parancs beolvassa a $storageclassifications változó tárhelybesorolások hello listáját.</span><span class="sxs-lookup"><span data-stu-id="381ac-210">hello below command gets hello list of storage classifications into $storageclassifications variable.</span></span>

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. <span data-ttu-id="381ac-211">alább parancsok hello hello Forrás besorolás $SourceClassificaion változó be és a cél besorolás $TargetClassification változó be kapják meg.</span><span class="sxs-lookup"><span data-stu-id="381ac-211">hello below commands get hello source classification into $SourceClassificaion variable and target classification into $TargetClassification variable.</span></span>

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > <span data-ttu-id="381ac-212">hello forrású és besorolásokat is lehet bármely hello tömb elemei.</span><span class="sxs-lookup"><span data-stu-id="381ac-212">hello source and target classifications can be any element in hello array.</span></span> <span data-ttu-id="381ac-213">Tekintse meg az alábbi parancs toofigure hello forrása és célja besorolások $storageclassifications tömb indexe hello toohello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="381ac-213">Refer toohello output of hello below command toofigure hello index of source and target classifications in $storageclassifications array.</span></span>

    > <span data-ttu-id="381ac-214">Get-AzureRmSiteRecoveryStorageClassification |} Select-Object - tulajdonság FriendlyName, azonosító |} Táblázat formázása</span><span class="sxs-lookup"><span data-stu-id="381ac-214">Get-AzureRmSiteRecoveryStorageClassification | Select-Object -Property FriendlyName, Id | Format-Table</span></span>


1. <span data-ttu-id="381ac-215">alább parancsmag hello hello forrás besorolási és hello cél egymáshoz leképezéseket hoz létre.</span><span class="sxs-lookup"><span data-stu-id="381ac-215">hello below cmdlet creates a mapping between hello source classification and hello target classification.</span></span>

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a><span data-ttu-id="381ac-216">8. lépés: A virtuális gépek védelmének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="381ac-216">Step 8: Enable protection for virtual machines</span></span>
<span data-ttu-id="381ac-217">Hello kiszolgálók, felhők és hálózatok megfelelő konfigurálását követően engedélyezheti a hello felhő virtuális gépek védelmét.</span><span class="sxs-lookup"><span data-stu-id="381ac-217">After hello servers, clouds and networks are configured correctly, you can enable protection for virtual machines in hello cloud.</span></span>

1. <span data-ttu-id="381ac-218">Futtassa a következő parancs tooget hello védelmi tároló hello tooenable védelem:</span><span class="sxs-lookup"><span data-stu-id="381ac-218">tooenable protection, run hello following command tooget hello protection container:</span></span>

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. <span data-ttu-id="381ac-219">Töltse le a hello védelmi entitás (VM) hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-219">Get hello protection entity (VM) by running hello following command:</span></span>

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. <span data-ttu-id="381ac-220">Engedélyezze a replikációját hello VM hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-220">Enable replication for hello VM by running hello following command:</span></span>

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a><span data-ttu-id="381ac-221">A központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="381ac-221">Test your deployment</span></span>
<span data-ttu-id="381ac-222">Tervezze meg a központi telepítés futtathat egyetlen virtuális gép feladatátvételi tesztjét, vagy több virtuális gép álló és hello a feladatátvételi teszt futtatása helyreállítási terv létrehozása tootest.</span><span class="sxs-lookup"><span data-stu-id="381ac-222">tootest your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for hello plan.</span></span> <span data-ttu-id="381ac-223">A feladatátvételi teszt segítségével elkülönített hálózatban próbálhatja ki a feladatátvételi és helyreállítási mechanizmusokat.</span><span class="sxs-lookup"><span data-stu-id="381ac-223">Test failover simulates your failover and recovery mechanism in an isolated network.</span></span>

> [!NOTE]
> <span data-ttu-id="381ac-224">Az alkalmazás helyreállítási tervet hozhat létre Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="381ac-224">You can create a recovery plan for your application in Azure portal.</span></span>
>
>

<span data-ttu-id="381ac-225">hello művelet, toocheck hello befejezése kövesse hello [figyelési tevékenység](#monitor).</span><span class="sxs-lookup"><span data-stu-id="381ac-225">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

### <a name="run-a-test-failover"></a><span data-ttu-id="381ac-226">Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="381ac-226">Run a test failover</span></span>
1. <span data-ttu-id="381ac-227">Futtassa az alábbi parancsmagok tooget hello VM hálózati toowhich hello kívánt tootest feladatátvételt a virtuális gépek számára.</span><span class="sxs-lookup"><span data-stu-id="381ac-227">Run hello below cmdlets tooget hello VM network toowhich you want tootest failover your VMs to.</span></span>

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. <span data-ttu-id="381ac-228">A virtuális gépek feladatátvételi teszt végrehajtása hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-228">Perform a test failover of a VM by doing hello following:</span></span>

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. <span data-ttu-id="381ac-229">A helyreállítási terv feladatátvételi teszt végrehajtása hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-229">Perform a test failover of a recovery plan by doing hello following:</span></span>

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a><span data-ttu-id="381ac-230">Tervezett feladatátvétel futtatása</span><span class="sxs-lookup"><span data-stu-id="381ac-230">Run a planned failover</span></span>
1. <span data-ttu-id="381ac-231">A virtuális gépek tervezett feladatátvétel végrehajtása hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-231">Perform a planned failover of a VM by doing hello following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. <span data-ttu-id="381ac-232">A helyreállítási terv tervezett feladatátvétel végrehajtása hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-232">Perform a planned failover of a recovery plan by doing hello following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a><span data-ttu-id="381ac-233">A nem tervezett feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="381ac-233">Run an unplanned failover</span></span>
1. <span data-ttu-id="381ac-234">Hajtsa végre a nem tervezett feladatátvételt a virtuális gépek hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-234">Perform an unplanned failover of a VM by doing hello following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

<span data-ttu-id="381ac-235">2 a helyreállítási terv nem tervezett feladatátvétel végrehajtása hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="381ac-235">2.Perform an unplanned failover of a recovery plan by doing hello following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <span data-ttu-id="381ac-236"><a name=monitor></a>Figyelési tevékenység</span><span class="sxs-lookup"><span data-stu-id="381ac-236"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="381ac-237">A következő parancsok toomonitor hello tevékenység hello használata.</span><span class="sxs-lookup"><span data-stu-id="381ac-237">Use hello following commands toomonitor hello activity.</span></span> <span data-ttu-id="381ac-238">Vegye figyelembe, hogy rendelkezik-e toowait Between hello feldolgozási toofinish feladataihoz.</span><span class="sxs-lookup"><span data-stu-id="381ac-238">Note that you have toowait in between jobs for hello processing toofinish.</span></span>

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



## <a name="next-steps"></a><span data-ttu-id="381ac-239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="381ac-239">Next steps</span></span>
<span data-ttu-id="381ac-240">[További](/powershell/module/azurerm.recoveryservices.backup/#recovery) Azure Site Recovery Azure Resource Manager PowerShell-parancsmagokkal kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="381ac-240">[Read more](/powershell/module/azurerm.recoveryservices.backup/#recovery) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
