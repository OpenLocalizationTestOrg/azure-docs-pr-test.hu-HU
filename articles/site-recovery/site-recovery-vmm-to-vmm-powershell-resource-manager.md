---
title: "A VMM-ben a Hyper-V virtuális gépek replikálása egy másodlagos helyre a PowerShell használatával (Azure Resource Manager) |} Microsoft Docs"
description: "Ismerteti, hogyan lehet az Azure Site Recovery-bA replikálása, feladatátvétele és helyreállítása a Hyper-V virtuális gépek VMM-felhőkben a VMM másodlagos hely PowerShell (Resource Manager) használatával történő telepítése"
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
ms.openlocfilehash: 5a6e00877b0a2b139d5322f610c1901ad76a710f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a><span data-ttu-id="fb3fd-103">Hyper-V virtuális gépek VMM-felhőkben replikálása egy másodlagos VMM-hely PowerShell (Resource Manager) használatával</span><span class="sxs-lookup"><span data-stu-id="fb3fd-103">Replicate Hyper-V virtual machines in VMM clouds to a secondary VMM site using PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fb3fd-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fb3fd-104">Azure Portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="fb3fd-105">Klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="fb3fd-105">Classic Portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="fb3fd-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="fb3fd-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="fb3fd-107">Az Azure Site Recovery szolgáltatás üdvözli Önt!</span><span class="sxs-lookup"><span data-stu-id="fb3fd-107">Welcome to Azure Site Recovery!</span></span> <span data-ttu-id="fb3fd-108">Ez a cikk, ha a helyszíni Hyper-V virtuális gépek replikálása egy másodlagos hely System Center Virtual Machine Manager (VMM) felhőkben felügyelt használja.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-108">Use this article if you want to replicate on-premises Hyper-V  virtual machines managed in System Center Virtual Machine Manager (VMM) clouds to a secondary site.</span></span>

<span data-ttu-id="fb3fd-109">Ez a cikk bemutatja, hogyan kell végrehajtani, ha a System Center VMM-felhőkben Hyper-V virtuális gépek replikálása másodlagos hely System Center VMM-felhők beállításakor Azure Site Recovery gyakori feladatok automatizálása a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-109">This article shows you how to use PowerShell to automate common tasks you need to perform when you set up Azure Site Recovery to replicate Hyper-V virtual machines in System Center VMM clouds to System Center VMM clouds in secondary site.</span></span>

<span data-ttu-id="fb3fd-110">A cikk a forgatókönyv előfeltételei tartalmazza, és bemutatja</span><span class="sxs-lookup"><span data-stu-id="fb3fd-110">The article includes prerequisites for the scenario, and shows you</span></span>

* <span data-ttu-id="fb3fd-111">A Recovery Services-tároló beállítása</span><span class="sxs-lookup"><span data-stu-id="fb3fd-111">How to set up a Recovery Services Vault</span></span>
* <span data-ttu-id="fb3fd-112">Az Azure Site Recovery Provider telepítése a forrás VMM-kiszolgálón és a célként megadott VMM-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="fb3fd-112">Install the Azure Site Recovery Provider on the source VMM server and the target VMM server</span></span>
* <span data-ttu-id="fb3fd-113">A VMM-kiszolgáló regisztrálása a tárolóban</span><span class="sxs-lookup"><span data-stu-id="fb3fd-113">Register the VMM server(s) in the vault</span></span>
* <span data-ttu-id="fb3fd-114">Replikációs házirend konfigurálása a VMM-felhő.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-114">Configure replication policy for the VMM Cloud.</span></span> <span data-ttu-id="fb3fd-115">A replikációs beállítások vannak megadva a házirend az összes védett virtuális gépek esetén alkalmazandó</span><span class="sxs-lookup"><span data-stu-id="fb3fd-115">The replication settings in the policy will be applied to all protected virtual machines</span></span>
* <span data-ttu-id="fb3fd-116">A virtuális gépek védelmének engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-116">Enable protection for the virtual machines.</span></span>
* <span data-ttu-id="fb3fd-117">Tesztelése a virtuális gépek egyenként, vagy győződjön meg arról, hogy minden a várt módon működik a helyreállítási terv részeként.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-117">Test the failover of VMs individually or as part of a recovery plan to make sure everything is working as expected.</span></span>
* <span data-ttu-id="fb3fd-118">Hajtsa végre a tervezett vagy nem tervezett feladatátvételt a virtuális gépek egyenként, vagy győződjön meg arról, hogy minden a várt módon működik a helyreállítási terv részeként.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-118">Perform a planned or an unplanned failover of VMs individually or as part of a recovery plan to make sure everything is working as expected.</span></span>

<span data-ttu-id="fb3fd-119">Ha ez a forgatókönyv beállítása problémát tapasztal, kérdéseit a a [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="fb3fd-119">If you run into problems setting up this scenario, post your questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="fb3fd-120">Az Azure két különböző [üzembe helyezési modellt](../azure-resource-manager/resource-manager-deployment-model.md) kínál az erőforrások létrehozására és kezelésére: az Azure Resource Manager-modellt és a klasszikus modellt.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-120">Azure has two different [deployment models](../azure-resource-manager/resource-manager-deployment-model.md) for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="fb3fd-121">Az Azure-ban két különböző portál érhető el: a klasszikus Azure portál, amely a klasszikus üzembe helyezési modellt támogatja, és az Azure portál, amely mindkét modellt támogatja.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-121">Azure also has two portals – the Azure classic portal that supports the classic deployment model, and the Azure portal with support for both deployment models.</span></span> <span data-ttu-id="fb3fd-122">Ez a cikk a Resource Manager-alapú üzemi modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-122">This article covers the Resource Manager deployment model.</span></span>
>
>

## <a name="on-premises-prerequisites"></a><span data-ttu-id="fb3fd-123">Helyszíni előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fb3fd-123">On-premises prerequisites</span></span>
<span data-ttu-id="fb3fd-124">Itt van a következőkre lesz szüksége a az helyszíni elsődleges és másodlagos helyek a forgatókönyv megvalósításához:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-124">Here's what you'll need in the primary and secondary on-premises sites to deploy this scenario:</span></span>

| <span data-ttu-id="fb3fd-125">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="fb3fd-125">**Prerequisites**</span></span> | <span data-ttu-id="fb3fd-126">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="fb3fd-126">**Details**</span></span> |
| --- | --- |
| <span data-ttu-id="fb3fd-127">**VMM**</span><span class="sxs-lookup"><span data-stu-id="fb3fd-127">**VMM**</span></span> |<span data-ttu-id="fb3fd-128">Azt javasoljuk, hogy a VMM-kiszolgáló az elsődleges hely és a VMM-kiszolgálót a másodlagos helyet telepít.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-128">We recommend you deploy a VMM server in the primary site and a VMM server in the secondary site.</span></span><br/><br/> <span data-ttu-id="fb3fd-129">Emellett [replikálása egy VMM-kiszolgálón felhők közötti](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span><span class="sxs-lookup"><span data-stu-id="fb3fd-129">You can also [replicate between clouds on a single VMM server](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span></span> <span data-ttu-id="fb3fd-130">Ehhez szüksége lesz legalább két, a VMM-kiszolgálón konfigurált felhők.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-130">To do this you'll need at least two clouds configured on the VMM server.</span></span><br/><br/> <span data-ttu-id="fb3fd-131">VMM-kiszolgálókon legalább futnia kell a System Center 2012 SP1 a legújabb frissítésekkel.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-131">VMM servers should be running at least System Center 2012 SP1 with the latest updates.</span></span><br/><br/> <span data-ttu-id="fb3fd-132">Minden VMM-kiszolgálót rendelkeznie kell egy vagy több konfigurált felhők és az összes felhő rendelkeznie kell a Hyper-V kapacitásprofilhoz beállítása.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-132">Each VMM server must have at one or more clouds configured and all clouds must have the Hyper-V Capacity profile set.</span></span> <br/><br/><span data-ttu-id="fb3fd-133">Felhő legalább egy VMM-gazdagépcsoportot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-133">Clouds must contain one or more VMM host groups.</span></span><br/><br/><span data-ttu-id="fb3fd-134">További információk a VMM-felhőkről beállításáról [konfigurálása a VMM felhőbeli háló](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), és [bemutató: magánfelhők létrehozása a System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb3fd-134">Learn more about setting up VMM clouds in [Configuring the VMM cloud fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), and [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span></span><br/><br/> <span data-ttu-id="fb3fd-135">VMM-kiszolgálókon internet-hozzáféréssel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-135">VMM servers should have internet access.</span></span> |
| <span data-ttu-id="fb3fd-136">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="fb3fd-136">**Hyper-V**</span></span> |<span data-ttu-id="fb3fd-137">Hyper-V kiszolgálók legalább futnia kell a Hyper-V szerepkör és a Windows Server 2012 és a legújabb frissítések telepítve van.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-137">Hyper-V servers must be running at least Windows Server 2012 with the Hyper-V role and have the latest updates installed.</span></span><br/><br/> <span data-ttu-id="fb3fd-138">Minden Hyper-V kiszolgáló tartalmazzon legalább egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-138">A Hyper-V server should contain one or more VMs.</span></span><br/><br/>  <span data-ttu-id="fb3fd-139">Hyper-V gazdakiszolgálók csoportok tárolása az elsődleges és másodlagos VMM-felhőkben kell elhelyezkedniük.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-139">Hyper-V host servers should be located in host groups in the primary and secondary VMM clouds.</span></span><br/><br/> <span data-ttu-id="fb3fd-140">Ha futtatja a Hyper-V fürt Windows Server 2012 R2 rendszeren kell telepítenie [2961977 frissítése](https://support.microsoft.com/kb/2961977)</span><span class="sxs-lookup"><span data-stu-id="fb3fd-140">If you're running Hyper-V in a cluster on Windows Server 2012 R2 you should install [update 2961977](https://support.microsoft.com/kb/2961977)</span></span><br/><br/> <span data-ttu-id="fb3fd-141">Ha futtatja a Hyper-V fürt Windows Server 2012 Megjegyzés: a fürtszervező nem hozza létre automatikusan Ha egy statikus IP cím alapú fürtöt.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-141">If you're running Hyper-V in a cluster on Windows Server 2012 note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="fb3fd-142">Ebben az esetben manuálisan kell konfigurálnia a fürtszervezőt.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-142">You'll need to configure the cluster broker manually.</span></span> <span data-ttu-id="fb3fd-143">[További](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span><span class="sxs-lookup"><span data-stu-id="fb3fd-143">[Read more](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span></span> |
| <span data-ttu-id="fb3fd-144">**Szolgáltató**</span><span class="sxs-lookup"><span data-stu-id="fb3fd-144">**Provider**</span></span> |<span data-ttu-id="fb3fd-145">A Site Recovery üzembe helyezése során az Azure Site Recovery Providert a VMM-kiszolgáló telepítése.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-145">During Site Recovery deployment you install the Azure Site Recovery Provider on VMM servers.</span></span> <span data-ttu-id="fb3fd-146">A szolgáltató replikációra HTTPS 443-as keresztül kommunikál a Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-146">The Provider communicates with Site Recovery over HTTPS 443 to orchestrate replication.</span></span> <span data-ttu-id="fb3fd-147">Adatok replikáció az elsődleges és másodlagos Hyper-V-kiszolgáló között a helyi vagy a VPN-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-147">Data replication occurs between the primary and secondary Hyper-V servers over the LAN or a VPN connection.</span></span><br/><br/> <span data-ttu-id="fb3fd-148">A VMM-kiszolgálón futó Provider hozzá kell férnie az URL-címek: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-148">The Provider running on the VMM server needs access to these URLs: *.hypervrecoverymanager.windowsazure.com; *.accesscontrol.windows.net; *.backup.windowsazure.com; *.blob.core.windows.net; *.store.core.windows.net.</span></span><br/><br/> <span data-ttu-id="fb3fd-149">Továbbá lehetővé teszi a VMM-kiszolgáló a tűzfal-kommunikációt a [Azure datacenter IP-címtartományok](https://www.microsoft.com/download/confirmation.aspx?id=41653) és a HTTPS (443) protokollt engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-149">In addition allow firewall communication from the VMM servers to the [Azure datacenter IP ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) and allow the HTTPS (443) protocol.</span></span> |

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="fb3fd-150">Hálózatleképezési előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fb3fd-150">Network mapping prerequisites</span></span>
<span data-ttu-id="fb3fd-151">Hálózatleképezés kapcsolatot hoz létre az elsődleges és másodlagos VMM-kiszolgálókon a VMM-Virtuálisgép-hálózatok között:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-151">Network mapping maps between VMM VM networks on the primary and secondary VMM servers to:</span></span>

* <span data-ttu-id="fb3fd-152">Optimális helyezze el a replika virtuális gépek másodlagos Hyper-V-gazdagépek a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-152">Optimally place replica VMs on secondary Hyper-V hosts after failover.</span></span>
* <span data-ttu-id="fb3fd-153">A replika virtuális gépek csatlakozni a megfelelő Virtuálisgép-hálózatok.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-153">Connect replica VMs to appropriate VM networks.</span></span>
* <span data-ttu-id="fb3fd-154">Ha nem konfigurálja a hálózati leképezési replika virtuális gép nem fog csatlakozni egyetlen hálózathoz sem a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-154">If you don't configure network mapping replica VMs won't be connected to any network after failover.</span></span>
* <span data-ttu-id="fb3fd-155">Ha azt szeretné, hogy a hálózat beállítása során a Site Recovery leképezése itt kell telepíteni következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-155">If you want to set up network mapping during Site Recovery deployment here's what you'll need:</span></span>

  * <span data-ttu-id="fb3fd-156">Ellenőrizze, hogy a forrás Hyper-V gazdakiszolgálón futó virtuális gépek csatlakoznak-e egy VMM-virtuálisgép-hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-156">Make sure that VMs on the source Hyper-V host server are connected to a VMM VM network.</span></span> <span data-ttu-id="fb3fd-157">Ezt a hálózatot kösse össze egy, a felhőhöz társított logikai hálózattal.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-157">That network should be linked to a logical network that is associated with the cloud.</span></span>
  * <span data-ttu-id="fb3fd-158">Ellenőrizze, hogy a másodlagos szeretné használni a helyreállítási felhőben van-e a megfelelő Virtuálisgép-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-158">Verify that the secondary cloud that you'll use for recovery has a corresponding VM network configured.</span></span> <span data-ttu-id="fb3fd-159">A Virtuálisgép-hálózatot kösse össze a másodlagos felhőhöz társított logikai hálózatot.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-159">That VM network should be linked to a logical network that's associated with the secondary cloud.</span></span>

<span data-ttu-id="fb3fd-160">További információ a VMM-hálózatok konfigurálásáról az alábbi cikkek</span><span class="sxs-lookup"><span data-stu-id="fb3fd-160">Learn more about configuring VMM networks in the below articles</span></span>

* [<span data-ttu-id="fb3fd-161">Logikai hálózatok konfigurálása a VMM-ben</span><span class="sxs-lookup"><span data-stu-id="fb3fd-161">How to configure logical networks in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [<span data-ttu-id="fb3fd-162">A Virtuálisgép-hálózatok és átjárók konfigurálása a VMM-ben</span><span class="sxs-lookup"><span data-stu-id="fb3fd-162">How to configure VM networks and gateways in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386308)

<span data-ttu-id="fb3fd-163">[További információk](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) a hálózatleképezés működéséről.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-163">[Learn more](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) about how network mapping works.</span></span>

### <a name="powershell-prerequisites"></a><span data-ttu-id="fb3fd-164">PowerShell-Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fb3fd-164">PowerShell prerequisites</span></span>
<span data-ttu-id="fb3fd-165">Győződjön meg arról, hogy készen áll az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-165">Make sure you have Azure PowerShell ready to go.</span></span> <span data-ttu-id="fb3fd-166">Ha már használ PowerShell, szüksége lesz a frissítési verzióra 0.8.10 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-166">If you are already using PowerShell, you'll need to upgrade to version 0.8.10 or later.</span></span> <span data-ttu-id="fb3fd-167">PowerShell beállításával kapcsolatos információkért lásd: a [útmutató az Azure PowerShell telepítése és konfigurálása](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="fb3fd-167">For information about setting up PowerShell, see the [Guide to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="fb3fd-168">Miután beállítása és PowerShell konfigurált, megtekintheti az összes elérhető parancsmagok a szolgáltatás [Itt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="fb3fd-168">Once you have set up and configured PowerShell, you can view all of the available cmdlets for the service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="fb3fd-169">Című témakörben olvashat tippeket, amelyekkel a parancsmagok, például a paraméterértékek, bemenetekhez és kimenetekhez általában kezelésének módja az Azure PowerShell használata a [útmutatóban Ismerkedés az Azure-parancsmagok](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="fb3fd-169">To learn about tips that can help you use the cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see the [Guide to get Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-the-subscription"></a><span data-ttu-id="fb3fd-170">1. lépés: Az előfizetés beállításához</span><span class="sxs-lookup"><span data-stu-id="fb3fd-170">Step 1: Set the subscription</span></span>
1. <span data-ttu-id="fb3fd-171">Az Azure powershell, jelentkezzen be az Azure-fiókjával: a következő parancsmagok használatával</span><span class="sxs-lookup"><span data-stu-id="fb3fd-171">From Azure powershell, login to your Azure account: using the following cmdlets</span></span>

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. <span data-ttu-id="fb3fd-172">Az előfizetések listájának lekérése.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-172">Get a list of your subscriptions.</span></span> <span data-ttu-id="fb3fd-173">Ez is kilistázza a subscriptionIDs az egyes előfizetések.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-173">This will also list the subscriptionIDs for each of the subscriptions.</span></span> <span data-ttu-id="fb3fd-174">Jegyezze fel az előfizetés-azonosító az előfizetés, amelyben létre szeretne hozni a recovery services-tároló</span><span class="sxs-lookup"><span data-stu-id="fb3fd-174">Note down the subscriptionID of the subscription in which you wish to create the recovery services vault</span></span>    

        Get-AzureRmSubscription
3. <span data-ttu-id="fb3fd-175">Az előfizetés, amelyen a recovery services-tároló, hogy hozza létre az előfizetés-azonosító megemlíteni beállításához</span><span class="sxs-lookup"><span data-stu-id="fb3fd-175">Set the subscription in which the recovery services vault is to be created by mentioning the subscription ID</span></span>

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a><span data-ttu-id="fb3fd-176">2. lépés: Recovery Services-tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="fb3fd-176">Step 2: Create a Recovery Services vault</span></span>
1. <span data-ttu-id="fb3fd-177">Ha már nincs ilyen, hozzon létre egy Azure Resource Manager erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="fb3fd-177">Create an Azure Resource Manager resource group if you don't have one already</span></span>

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. <span data-ttu-id="fb3fd-178">Hozzon létre egy új Recovery Services-tároló, és mentse a létrehozott automatikus rendszer-Helyreállítás tároló objektum egy változóban (használandó később).</span><span class="sxs-lookup"><span data-stu-id="fb3fd-178">Create a new Recovery Services vault and save the created ASR vault object in a variable (will be used later).</span></span> <span data-ttu-id="fb3fd-179">Az automatikus rendszer-Helyreállítás tároló objektum post létrehozása a Get-AzureRMRecoveryServicesVault parancsmaggal is lekérhet:-</span><span class="sxs-lookup"><span data-stu-id="fb3fd-179">You can also retrieve the ASR vault object post creation using the Get-AzureRMRecoveryServicesVault cmdlet:-</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a><span data-ttu-id="fb3fd-180">3. lépés: A Recovery Services-tároló környezet beállítása</span><span class="sxs-lookup"><span data-stu-id="fb3fd-180">Step 3: Set the Recovery Services Vault context</span></span>
1. <span data-ttu-id="fb3fd-181">Ha egy már létrehozott tároló, futtassa az alábbi parancs használatával beszerezheti a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-181">If you have a vault already created, run the below command to get the vault.</span></span>

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. <span data-ttu-id="fb3fd-182">A tároló környezet beállítása futtatásával az alábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-182">Set the vault context by running the below command.</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a><span data-ttu-id="fb3fd-183">4. lépés: Az Azure Site Recovery Provider telepítése</span><span class="sxs-lookup"><span data-stu-id="fb3fd-183">Step 4: Install the Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="fb3fd-184">A VMM-gépen hozzon létre egy könyvtárat a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-184">On the VMM machine, create a directory by running the following command:</span></span>

       New-Item c:\ASR -type directory
2. <span data-ttu-id="fb3fd-185">Bontsa ki a fájlokat a letöltött szolgáltató használatával a következő parancs futtatásával</span><span class="sxs-lookup"><span data-stu-id="fb3fd-185">Extract the files using the downloaded provider by running the following command</span></span>

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. <span data-ttu-id="fb3fd-186">Telepítse a szolgáltatót, a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-186">Install the provider using the following commands:</span></span>

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

   <span data-ttu-id="fb3fd-187">Várjon, amíg a telepítés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-187">Wait for the installation to finish.</span></span>
4. <span data-ttu-id="fb3fd-188">Regisztrálja a kiszolgálót a tárolóban, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-188">Register the server in the vault using the following command:</span></span>

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-and-associate-a-replication-policy"></a><span data-ttu-id="fb3fd-189">5. lépés: Házirend létrehozása és társítása a replikációs</span><span class="sxs-lookup"><span data-stu-id="fb3fd-189">Step 5: Create and associate a replication policy</span></span>
1. <span data-ttu-id="fb3fd-190">Hozzon létre egy Hyper-V 2012 R2-ben replikációs házirendet a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-190">Create a Hyper-V 2012 R2 replication policy by running the following command:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > <span data-ttu-id="fb3fd-191">A VMM-felhő (ahogy azt a Hyper-V előfeltételei) Windows Server különböző verzióit futtató Hyper-V-gazdagépek is tartalmazhat, de a replikációs házirend adott operációsrendszer-verzió.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-191">The VMM cloud can contain Hyper-V hosts running different versions of Windows Server (as mentioned in the Hyper-V prerequisites), but the replication policy is OS version specific.</span></span> <span data-ttu-id="fb3fd-192">Ha a különböző operációs rendszereken futó különböző gazdagépeken, majd hozza létre külön replikációs házirendet minden operációs rendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-192">If you have different hosts running on different operating system versions, then create separate replication policies for each type of OS version.</span></span> <span data-ttu-id="fb3fd-193">A pl.: Ha öt gazdagépeken futó Windows-kiszolgálók 2012 és Windows Server 2012 R2 három, létre kell hoznia két replikációs házirendek – egyet az egyes operációs rendszerekhez.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-193">For eg: If you have five hosts running on Windows Servers 2012 and three on Windows Server 2012 R2, create two replication polices – one for each type of operating system versions.</span></span>

1. <span data-ttu-id="fb3fd-194">Töltse le az elsődleges védelmi tároló (elsődleges VMM-felhőben) és a helyreállítási védelmi tároló (helyreállítási VMM-felhőben) a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-194">Get the primary protection container (primary VMM Cloud) and recovery protection container (recovery VMM Cloud) by running the following commands:</span></span>

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. <span data-ttu-id="fb3fd-195">A házirend a rövid név használatával 1. lépésben létrehozott szabályzat beolvasása</span><span class="sxs-lookup"><span data-stu-id="fb3fd-195">Retrieve the policy you created in step 1 using the friendly name of the policy</span></span>

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. <span data-ttu-id="fb3fd-196">Indítsa el a társítást a védelmi tároló (VMM-felhőben) a replikációs házirend:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-196">Start the association of the protection container (VMM Cloud) with the replication policy:</span></span>

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. <span data-ttu-id="fb3fd-197">Várja meg a házirend társítása feladat befejeződik.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-197">Wait for the policy association job to complete.</span></span> <span data-ttu-id="fb3fd-198">Ha a feladat befejeződött a következő PowerShell-részlet használatával ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-198">You can check if the job has completed using the following PowerShell snippet.</span></span>

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   <span data-ttu-id="fb3fd-199">A feladat feldolgozása után futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-199">After the job has finished processing, run the following command:</span></span>

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

<span data-ttu-id="fb3fd-200">A művelet a befejezése ellenőrzéséhez kövesse [figyelési tevékenység](#monitor).</span><span class="sxs-lookup"><span data-stu-id="fb3fd-200">To check the completion of the operation, follow the steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-6-configure-network-mapping"></a><span data-ttu-id="fb3fd-201">6. lépés: Hálózatleképezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fb3fd-201">Step 6: Configure network mapping</span></span>
1. <span data-ttu-id="fb3fd-202">Az első parancs kiszolgálók lekéri a jelenlegi Azure Site Recovery-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-202">The first command gets servers for the current Azure Site Recovery vault.</span></span> <span data-ttu-id="fb3fd-203">A parancs a Microsoft Azure Site Recovery kiszolgálók $Servers tömb változó tárolja.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-203">The command stores the Microsoft Azure Site Recovery servers in the $Servers array variable.</span></span>

        $Servers = Get-AzureRmSiteRecoveryServer
2. <span data-ttu-id="fb3fd-204">Az alábbi parancsok lekérése a helyreállítási helyen lévő hálózat a forrás VMM-kiszolgálón és a cél VMM-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-204">The below commands get the site recovery network for the source VMM server and the target VMM server.</span></span>

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > <span data-ttu-id="fb3fd-205">A forrás VMM-kiszolgálón az elsőt vagy a másikat a kiszolgálók tömb lehet.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-205">The source VMM server can be the first one or the second one in the servers array.</span></span> <span data-ttu-id="fb3fd-206">Ellenőrizze a VMM-kiszolgáló nevét, és a hálózatok megfelelően beolvasása</span><span class="sxs-lookup"><span data-stu-id="fb3fd-206">Check the names of the VMM servers and get the networks appropriately</span></span>


1. <span data-ttu-id="fb3fd-207">A végső parancsmag hoz létre az elsődleges és a helyreállítási hálózat közötti leképezést.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-207">The final cmdlet creates a mapping between the primary network and the recovery network.</span></span> <span data-ttu-id="fb3fd-208">A parancsmag, mint az első elem $PrimaryNetworks és a helyreállítási hálózati $RecoveryNetworks első elemeként adja meg az elsődleges hálózatot.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-208">The cmdlet specifies the primary network as the first element of $PrimaryNetworks and the recovery network as the first element of $RecoveryNetworks.</span></span>

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a><span data-ttu-id="fb3fd-209">7. lépés: Konfigurálja a tárolási leképezése</span><span class="sxs-lookup"><span data-stu-id="fb3fd-209">Step 7: Configure storage mapping</span></span>
1. <span data-ttu-id="fb3fd-210">Az alábbi parancs beolvassa a tárhelybesorolások $storageclassifications változó be listáját.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-210">The below command gets the list of storage classifications into $storageclassifications variable.</span></span>

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. <span data-ttu-id="fb3fd-211">Az alábbi parancsok beolvasása a Forrás besorolás $SourceClassificaion változót és a célként megadott besorolási $TargetClassification változó be azokat.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-211">The below commands get the source classification into $SourceClassificaion variable and target classification into $TargetClassification variable.</span></span>

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > <span data-ttu-id="fb3fd-212">A forrás- és besorolásokat is lehet bármely a tömb elemei.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-212">The source and target classifications can be any element in the array.</span></span> <span data-ttu-id="fb3fd-213">Tekintse meg a kimeneti az alábbi parancsot, mi a forrás és cél besorolások $storageclassifications tömb indexét.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-213">Refer to the output of the below command to figure the index of source and target classifications in $storageclassifications array.</span></span>

    > <span data-ttu-id="fb3fd-214">Get-AzureRmSiteRecoveryStorageClassification |} Select-Object - tulajdonság FriendlyName, azonosító |} Táblázat formázása</span><span class="sxs-lookup"><span data-stu-id="fb3fd-214">Get-AzureRmSiteRecoveryStorageClassification | Select-Object -Property FriendlyName, Id | Format-Table</span></span>


1. <span data-ttu-id="fb3fd-215">Az alábbi parancsmag létrehoz egy leképezésnek a forrás besorolást és a cél egymáshoz.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-215">The below cmdlet creates a mapping between the source classification and the target classification.</span></span>

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a><span data-ttu-id="fb3fd-216">8. lépés: A virtuális gépek védelmének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="fb3fd-216">Step 8: Enable protection for virtual machines</span></span>
<span data-ttu-id="fb3fd-217">A kiszolgálók, felhők és hálózatok megfelelő konfigurálását követően engedélyezheti a felhőben található virtuális gépek védelmét.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-217">After the servers, clouds and networks are configured correctly, you can enable protection for virtual machines in the cloud.</span></span>

1. <span data-ttu-id="fb3fd-218">Védelem engedélyezéséhez futtassa a következő parancsot a védelmi tároló:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-218">To enable protection, run the following command to get the protection container:</span></span>

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. <span data-ttu-id="fb3fd-219">Töltse le a védelmi entitás (VM) a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-219">Get the protection entity (VM) by running the following command:</span></span>

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. <span data-ttu-id="fb3fd-220">A virtuális gép replikációjának engedélyezéséhez a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-220">Enable replication for the VM by running the following command:</span></span>

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a><span data-ttu-id="fb3fd-221">A központi telepítés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fb3fd-221">Test your deployment</span></span>
<span data-ttu-id="fb3fd-222">Az üzemelő példány el egyetlen virtuális gépre, a feladatátvételi teszt futtatása vagy több virtuális gépet tartalmazó helyreállítási tervet létrehozni és futtassa a terv feladatátvételi tesztjét.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-222">To test your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for the plan.</span></span> <span data-ttu-id="fb3fd-223">A feladatátvételi teszt segítségével elkülönített hálózatban próbálhatja ki a feladatátvételi és helyreállítási mechanizmusokat.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-223">Test failover simulates your failover and recovery mechanism in an isolated network.</span></span>

> [!NOTE]
> <span data-ttu-id="fb3fd-224">Az alkalmazás helyreállítási tervet hozhat létre Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-224">You can create a recovery plan for your application in Azure portal.</span></span>
>
>

<span data-ttu-id="fb3fd-225">A művelet a befejezése ellenőrzéséhez kövesse [figyelési tevékenység](#monitor).</span><span class="sxs-lookup"><span data-stu-id="fb3fd-225">To check the completion of the operation, follow the steps in [Monitor Activity](#monitor).</span></span>

### <a name="run-a-test-failover"></a><span data-ttu-id="fb3fd-226">Feladatátvételi teszt futtatása</span><span class="sxs-lookup"><span data-stu-id="fb3fd-226">Run a test failover</span></span>
1. <span data-ttu-id="fb3fd-227">Futtassa az alábbi parancsmagok a Virtuálisgép-hálózatot, amelyhez a feladatátvételi teszt szeretné beolvasni a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-227">Run the below cmdlets to get the VM network to which you want to test failover your VMs to.</span></span>

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. <span data-ttu-id="fb3fd-228">A virtuális gépek feladatátvételi teszt végrehajtása a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-228">Perform a test failover of a VM by doing the following:</span></span>

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. <span data-ttu-id="fb3fd-229">A helyreállítási terv feladatátvételi teszt végrehajtása a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-229">Perform a test failover of a recovery plan by doing the following:</span></span>

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a><span data-ttu-id="fb3fd-230">Tervezett feladatátvétel futtatása</span><span class="sxs-lookup"><span data-stu-id="fb3fd-230">Run a planned failover</span></span>
1. <span data-ttu-id="fb3fd-231">A virtuális gépek tervezett feladatátvétel végrehajtása a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-231">Perform a planned failover of a VM by doing the following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. <span data-ttu-id="fb3fd-232">A helyreállítási terv tervezett feladatátvétel végrehajtása a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-232">Perform a planned failover of a recovery plan by doing the following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a><span data-ttu-id="fb3fd-233">A nem tervezett feladatátvétel</span><span class="sxs-lookup"><span data-stu-id="fb3fd-233">Run an unplanned failover</span></span>
1. <span data-ttu-id="fb3fd-234">Hajtsa végre a nem tervezett feladatátvételt a virtuális gépek a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-234">Perform an unplanned failover of a VM by doing the following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

<span data-ttu-id="fb3fd-235">2 a helyreállítási terv nem tervezett feladatátvétel végrehajtása a következő módon:</span><span class="sxs-lookup"><span data-stu-id="fb3fd-235">2.Perform an unplanned failover of a recovery plan by doing the following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <span data-ttu-id="fb3fd-236"><a name=monitor></a>Figyelési tevékenység</span><span class="sxs-lookup"><span data-stu-id="fb3fd-236"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="fb3fd-237">Az alábbi parancsokkal tevékenységének figyelését.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-237">Use the following commands to monitor the activity.</span></span> <span data-ttu-id="fb3fd-238">Vegye figyelembe, hogy várnia kell, a feldolgozását a Befejezés gombra a feladatok között.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-238">Note that you have to wait in between jobs for the processing to finish.</span></span>

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



## <a name="next-steps"></a><span data-ttu-id="fb3fd-239">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fb3fd-239">Next steps</span></span>
<span data-ttu-id="fb3fd-240">[További](/powershell/module/azurerm.recoveryservices.backup/#recovery) Azure Site Recovery Azure Resource Manager PowerShell-parancsmagokkal kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="fb3fd-240">[Read more](/powershell/module/azurerm.recoveryservices.backup/#recovery) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
