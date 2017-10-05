---
title: "A Hyper-V (a System Center VMM nélkül) az Azure Site Recovery Azure replikáció hálózati terv |} Microsoft Docs"
description: "Ez a cikk ismerteti az alaphálózati topológia tervezése szükséges, ha (VMM nélkül) a Hyper-V virtuális gépek replikálása az Azure-bA"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5ca12254-975d-48e8-a84d-422825dac9b2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/21/2017
ms.author: raynew
ms.openlocfilehash: 100b9d8a55c2c163e7a04680f0f7d7963315ee73
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="step-4-plan-networking-for-hyper-v-to-azure-replication"></a><span data-ttu-id="4504c-103">4. lépés: Hyper-v hálózatkezelési Azure replikáció tervezése</span><span class="sxs-lookup"><span data-stu-id="4504c-103">Step 4: Plan networking for Hyper-V to Azure replication</span></span>

<span data-ttu-id="4504c-104">Ez a cikk hálózati tervezési szempontok, ha replikálása a helyszíni Hyper-V virtuális gépek (a System Center VMM nélkül) az Azure használatát foglalja össze a [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="4504c-104">This article summarizes network planning considerations when replicating on-premises Hyper-V VMs (without System Center VMM) to Azure using the [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="4504c-105">Megjegyzéseket a cikk alján tehet, kérdéseket pedig az [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti fel.</span><span class="sxs-lookup"><span data-stu-id="4504c-105">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="connecting-to-replica-vms-after-failover"></a><span data-ttu-id="4504c-106">Csatlakozás a replika virtuális gépek a feladatátvételt követően</span><span class="sxs-lookup"><span data-stu-id="4504c-106">Connecting to replica VMs after failover</span></span>

<span data-ttu-id="4504c-107">A replikációs és feladatátvételi stratégiát tervezésekor az alábbiakhoz hasonló fontos kérdések egyike csatlakoztatása az Azure virtuális gép a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="4504c-107">When planning your replication and failover strategy, one of the key questions is how to connect to the Azure VM after failover.</span></span> <span data-ttu-id="4504c-108">Számos több lehetősége a replika Azure virtuális gépek hálózati stratégia tervezésekor.</span><span class="sxs-lookup"><span data-stu-id="4504c-108">There are a couple of choices when designing your network strategy for replica Azure VMs:</span></span>

- <span data-ttu-id="4504c-109">**Másik IP-cím**: egy másik IP-címtartományt használja a replikált Azure Virtuálisgép-hálózat állítható be.</span><span class="sxs-lookup"><span data-stu-id="4504c-109">**Different IP address**: You can select to use a different IP address range for the replicated Azure VM network.</span></span> <span data-ttu-id="4504c-110">Ebben a forgatókönyvben a virtuális Gépet egy új IP-cím beolvasása feladatátvétel után, és egy DNS-frissítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="4504c-110">In this scenario the VM gets a new IP address after failover, and a DNS update is required.</span></span> [<span data-ttu-id="4504c-111">További információ</span><span class="sxs-lookup"><span data-stu-id="4504c-111">Learn more</span></span>](site-recovery-test-failover-vmm-to-vmm.md#prepare-the-infrastructure-for-test-failover)
- <span data-ttu-id="4504c-112">**IP-cím**: Előfordulhat, hogy szeretné használják az azonos IP-címtartományt, amely a helyszíni elsődleges hálózat, a feladatátvételt követően az Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="4504c-112">**Same IP address**: You might want to use the same IP address range as that in your primary on-premises network, for the Azure network after failover.</span></span>  <span data-ttu-id="4504c-113">Megőrzi az azonos IP-cím címek egyszerűbbé teszi a helyreállítást csökkentésével hálózattal kapcsolatos problémákat a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="4504c-113">Keeping the same IP addresses simplifies the recovery by reducing network related issues after failover.</span></span> <span data-ttu-id="4504c-114">Azonban ha az Azure-bA replikál, szüksége lesz az útvonalak frissítése az új hellyel a IP-címek a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="4504c-114">However, when you're replicating to Azure, you will need to update routes with the new location of the IP addresses after failover.</span></span>


## <a name="retain-ip-addresses"></a><span data-ttu-id="4504c-115">Tartsa meg az IP-címek</span><span class="sxs-lookup"><span data-stu-id="4504c-115">Retain IP addresses</span></span>

<span data-ttu-id="4504c-116">A Site Recovery biztosít a funkció megőrzését rögzített IP oldja meg, amikor az Azure-ba, az alhálózati feladatátvevő feladatátvétele.</span><span class="sxs-lookup"><span data-stu-id="4504c-116">Site Recovery provides the capability to retain fixed IP addresses when failing over to Azure, with a subnet failover.</span></span>

<span data-ttu-id="4504c-117">Alhálózati feladatátvétellel egy bizonyos alhálózat jelen webhely 1 vagy 2. hely, de soha nem mindkét helyen egyszerre.</span><span class="sxs-lookup"><span data-stu-id="4504c-117">With subnet failover, a specific subnet is present at Site 1 or Site 2, but never at both sites simultaneously.</span></span> <span data-ttu-id="4504c-118">Ahhoz, hogy a feladatátvétel esetén az IP-címtér kezelése, programozott módon el rendezése az útválasztó-infrastruktúra az alhálózatok áthelyezése egyik helyről egy másikra.</span><span class="sxs-lookup"><span data-stu-id="4504c-118">In order to maintain the IP address space in the event of a failover, you programmatically arrange for the router infrastructure to move the subnets from one site to another.</span></span> <span data-ttu-id="4504c-119">A feladatátvételi az alhálózatok a társított védett virtuális gépek áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="4504c-119">During failover, the subnets move with the associated protected VMs.</span></span> <span data-ttu-id="4504c-120">A fő hátránya, hogy hiba esetén, hogy a teljes alhálózat áthelyezése.</span><span class="sxs-lookup"><span data-stu-id="4504c-120">The main drawback is that in the event of a failure, you have to move the whole subnet.</span></span>


### <a name="failover-example"></a><span data-ttu-id="4504c-121">Feladatátvétel – példa</span><span class="sxs-lookup"><span data-stu-id="4504c-121">Failover example</span></span>

<span data-ttu-id="4504c-122">Nézzük például feladatátvétel az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="4504c-122">Let's look at an example for failover to Azure.</span></span>

- <span data-ttu-id="4504c-123">Egy ficticious vállalati, a Woodgrove Bank üzemeltető üzleti alkalmazások helyszíni infrastruktúra van.</span><span class="sxs-lookup"><span data-stu-id="4504c-123">A ficticious company, Woodgrove Bank, has an on-premises infrastructure hosting their business apps.</span></span> <span data-ttu-id="4504c-124">A mobilalkalmazás Azure üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="4504c-124">Their mobile applications are hosted on Azure.</span></span>
- <span data-ttu-id="4504c-125">A helyszíni peremhálózati hálózat és az Azure virtuális hálózat közötti pont-pont (VPN) kapcsolatot biztosít a Woodgrove Bank virtuális gépeket az Azure és a helyszíni kiszolgálók közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="4504c-125">Connectivity between Woodgrove Bank VMs in Azure and on-premises servers is provided by a site-to-site (VPN) connection between the on-premises edge network and the Azure virtual network.</span></span>
- <span data-ttu-id="4504c-126">A VPN-t, az azt jelenti, hogy a vállalat virtuális hálózat az Azure-ban jelenik meg a helyszíni hálózat kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="4504c-126">This VPN means that the company's virtual network in Azure appears as an extension of their on-premises network.</span></span>
- <span data-ttu-id="4504c-127">Woodgrove szeretné replikálni a helyszíni munkaterhelések az Azure Site Recovery segítségével.</span><span class="sxs-lookup"><span data-stu-id="4504c-127">Woodgrove wants to use Site Recovery to replicate on-premises workloads to Azure.</span></span>
 - <span data-ttu-id="4504c-128">Woodgrove rendelkezik az alkalmazások és konfigurációk, amely IP-címek kódolt függ, és így meg kell őriznie az IP-címet az alkalmazások az Azure-bA a feladatátvételt követően kezelésére.</span><span class="sxs-lookup"><span data-stu-id="4504c-128">Woodgrove has to deal with applications and configurations which depend on hard-coded IP addresses, and thus need to retain IP addresses for their applications after failover to Azure.</span></span>
 - <span data-ttu-id="4504c-129">Woodgrove tartomány 172.16.1.0/24, Azure-beli erőforrásaihoz 172.16.2.0/24 rendelkezik hozzárendelt IP-címeket.</span><span class="sxs-lookup"><span data-stu-id="4504c-129">Woodgrove has assigned IP addresses from range 172.16.1.0/24, 172.16.2.0/24 to its resources running in Azure.</span></span>


<span data-ttu-id="4504c-130">A Woodgrove tenni a virtuális gépek replikálása az Azure-ba, megtartja az IP-címek, itt meg Mi az a vállalati kell tennie:</span><span class="sxs-lookup"><span data-stu-id="4504c-130">For Woodgrove to be able to replicate its VMs to Azure while retaining the IP addresses, here's what the company needs to do:</span></span>

1. <span data-ttu-id="4504c-131">Hozzon létre egy Azure virtuális hálózatra.</span><span class="sxs-lookup"><span data-stu-id="4504c-131">Create an Azure virtual network.</span></span> <span data-ttu-id="4504c-132">A helyszíni hálózat kiterjesztése kell, hogy az alkalmazások zökkenőmentesen feladatátvétel.</span><span class="sxs-lookup"><span data-stu-id="4504c-132">It should be an extension of the on-premises network, so that applications can fail over seamlessly.</span></span>
2. <span data-ttu-id="4504c-133">Azure-helyek VPN-kapcsolat, az Azure-ban létrehozott virtuális hálózatok pont – hely kapcsolat mellett hozzáadását teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="4504c-133">Azure allows you to add site-to-site VPN connectivity, in addition to point-to-site connectivity to the virtual networks created in Azure.</span></span>
3. <span data-ttu-id="4504c-134">A pont-pont kapcsolat az Azure-hálózat beállítása során Ön irányíthatja a forgalmat a helyszíni helyre (helyi hálózati) csak akkor, ha az IP-címtartomány eltér a helyi IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="4504c-134">When setting up the site-to-site connection, in the Azure network, you can route traffic to the on-premises location (local-network) only if the IP address range is different from the on-premises IP address range.</span></span>
    - <span data-ttu-id="4504c-135">Ennek az az oka Azure felhőbe archivált alhálózatok nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="4504c-135">This is because Azure doesn’t support stretched subnets.</span></span> <span data-ttu-id="4504c-136">Ezért ha alhálózati 192.168.1.0/24 helyi, nem adhat hozzá egy helyi hálózati 192.168.1.0/24 az Azure-hálózat.</span><span class="sxs-lookup"><span data-stu-id="4504c-136">So if you have subnet 192.168.1.0/24 on-premises, you can’t add a local-network 192.168.1.0/24 in the Azure network.</span></span>
    - <span data-ttu-id="4504c-137">Ez elvárható, hiszen az Azure nem tudja, hogy nincs aktív virtuális gépek nincsenek-e az alhálózatot, és csak a vész-helyreállítási létre az alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="4504c-137">This is expected because Azure doesn’t know that there are no active VMs in the subnet, and that the subnet is being created for disaster recovery only.</span></span>
    - <span data-ttu-id="4504c-138">A fogja tudni megfelelően irányítható a hálózati forgalom kívül az Azure-hálózatot a hálózati és a helyi hálózat alhálózatai nem ütköznek-e.</span><span class="sxs-lookup"><span data-stu-id="4504c-138">To be able to correctly route network traffic out of an Azure network the subnets in the network and the local-network mustn't conflict.</span></span>

![Alhálózati feladatátvétel előtt](./media/hyper-v-site-walkthrough-network/network-design7.png)

### <a name="before-failover"></a><span data-ttu-id="4504c-140">Feladatátvétel előtt</span><span class="sxs-lookup"><span data-stu-id="4504c-140">Before failover</span></span>

1. <span data-ttu-id="4504c-141">Hozzon létre egy további hálózati (például a helyreállítási hálózati).</span><span class="sxs-lookup"><span data-stu-id="4504c-141">Create an additional network (for example Recovery Network).</span></span> <span data-ttu-id="4504c-142">Ez az a hálózaton található, amely átadja a virtuális gépek jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="4504c-142">This is the network in which failed over VMs are created.</span></span>
2. <span data-ttu-id="4504c-143">Győződjön meg arról, hogy az IP-címet a virtuális gépek a feladatátvétel után a virtuális gép tulajdonságai őrzi meg a > **konfigurálása**, adja meg, hogy a virtuális gép rendelkezik-e a helyszíni ugyanazt a címet, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="4504c-143">To ensure that the IP address for a VM is retained after a failover, in the VM properties > **Configure**, specify the same IP address that the VM has on-premises, and click **Save**.</span></span>
3. <span data-ttu-id="4504c-144">A virtuális gép feladatátvételt, ha az Azure Site Recovery rendeli hozzá a megadott IP-cím.</span><span class="sxs-lookup"><span data-stu-id="4504c-144">When the VM is failed over, Azure Site Recovery will assign the provided IP address to it.</span></span>

    ![Hálózat tulajdonságai](./media/hyper-v-site-walkthrough-network/network-design8.png)

4. <span data-ttu-id="4504c-146">Miután a feladatátvétel eseményindító aktiválódik, és a virtuális gépeket az Azure-hoz létre a szükséges IP-címmel, csatlakozhat a hálózati használatával egy [Vnet hálózatok vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="4504c-146">After failover is trigger is triggered, and the VMs are created in Azure with the required IP address, you can connect to the network using a [Vnet to Vnet vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span> <span data-ttu-id="4504c-147">Ez a művelet parancsfájlalapú lehet.</span><span class="sxs-lookup"><span data-stu-id="4504c-147">This action can be scripted.</span></span>
5. <span data-ttu-id="4504c-148">Útvonalak megfelelően módosítani kell, hogy tükrözze a 192.168.1.0/24 most áthelyezte az Azure kell.</span><span class="sxs-lookup"><span data-stu-id="4504c-148">Routes need to be appropriately modified, to reflect that 192.168.1.0/24 has now moved to Azure.</span></span>

    ![Alhálózati feladatátvételt követően](./media/hyper-v-site-walkthrough-network/network-design9.png)

### <a name="after-failover"></a><span data-ttu-id="4504c-150">A feladatátvételt követően</span><span class="sxs-lookup"><span data-stu-id="4504c-150">After failover</span></span>

<span data-ttu-id="4504c-151">Ha a fenti módon még nem rendelkezik Azure-hálózat, létrehozhat egy-helyek közti VPN-kapcsolat az elsődleges hely és az Azure, failvoer után.</span><span class="sxs-lookup"><span data-stu-id="4504c-151">If you don't have an Azure network as illustrated above, you can create a site-to-site VPN connection between your primary site and Azure, after failvoer.</span></span>

## <a name="change-ip-addresses"></a><span data-ttu-id="4504c-152">IP-címek módosítása</span><span class="sxs-lookup"><span data-stu-id="4504c-152">Change IP addresses</span></span>

<span data-ttu-id="4504c-153">Ez [blogbejegyzés](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) ismerteti, hogyan állíthatja be az Azure hálózati infrastruktúra, ha már nincs szükség IP-címek megőrizni a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="4504c-153">This [blog post](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) explains how to set up the Azure networking infrastructure when you don't need to retain IP addresses after failover.</span></span> <span data-ttu-id="4504c-154">Az kezdődik-e az alkalmazás leírása, hogyan állíthatja be a helyszíni hálózat és az Azure-ban megvizsgálja, és folyamat végén a feladatátvételeket a futtatásával kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="4504c-154">It starts with an application description, looks at how to set up networking on-premises and in Azure, and concludes with information about running failovers.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="4504c-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4504c-155">Next steps</span></span>

<span data-ttu-id="4504c-156">Ugrás a [5. lépés: Azure előkészítése](hyper-v-site-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="4504c-156">Go to [Step 5: Prepare Azure](hyper-v-site-walkthrough-prepare-azure.md)</span></span>
