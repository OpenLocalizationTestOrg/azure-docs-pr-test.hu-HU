---
title: "a VMware tooAzure replikáció hálózati aaaPlan |} Microsoft Docs"
description: "Ez a cikk ismerteti, amelyek hálózati tooAzure VMware virtuális gépek replikálása esetén szükséges tervezési"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5578a0b4-0352-41c3-9cce-56beb17a4d97
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/27/2017
ms.author: raynew
ms.openlocfilehash: 2b4f385c768cc7f5e98abae0afb8258b00f3724f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-plan-networking-for-vmware-tooazure-replication"></a><span data-ttu-id="cf24d-103">4. lépés: A VMware tooAzure replikációs hálózat tervezése</span><span class="sxs-lookup"><span data-stu-id="cf24d-103">Step 4: Plan networking for VMware tooAzure replication</span></span>

<span data-ttu-id="cf24d-104">Ez a cikk összegzi a tervezési szempontok, ha replikálása a helyszíni VMware virtuális gépek tooAzure hello használata hálózati [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cf24d-104">This article summarizes network planning considerations when replicating on-premises VMware VMs tooAzure using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="cf24d-105">Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="cf24d-105">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="connect-tooreplica-vms"></a><span data-ttu-id="cf24d-106">Csatlakozás tooreplica virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="cf24d-106">Connect tooreplica VMs</span></span>

<span data-ttu-id="cf24d-107">A replikációs és feladatátvételi stratégiát megtervezésekor fontos kérdések hello egyik hogyan tooconnect toohello Azure virtuális gép a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="cf24d-107">When planning your replication and failover strategy, one of hello key questions is how tooconnect toohello Azure VM after failover.</span></span> <span data-ttu-id="cf24d-108">Számos több lehetősége a replika Azure virtuális gépek hálózati stratégia tervezésekor.</span><span class="sxs-lookup"><span data-stu-id="cf24d-108">There are a couple of choices when designing your network strategy for replica Azure VMs:</span></span>

- <span data-ttu-id="cf24d-109">**Különböző IP-cím**: kiválaszthatja toouse egy másik IP-címtartomány a hello replikált Azure Virtuálisgép-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="cf24d-109">**Use different IP address**: You can select toouse a different IP address range for hello replicated Azure VM network.</span></span> <span data-ttu-id="cf24d-110">Ez a forgatókönyv hello a virtuális gép a feladatátvételt követően lekérdezi a egy új IP-címet, és DNS-frissítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="cf24d-110">In this scenario hello VM gets a new IP address after failover, and a DNS update is required.</span></span>
- <span data-ttu-id="cf24d-111">**Tartsa meg az IP-cím**: érdemes toouse hello azonos IP-címtartomány, mint a helyszíni elsődleges hely, hello Azure-hálózatot a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="cf24d-111">**Retain same IP address**: You might want toouse hello same IP address range as that in your primary on-premises site, for hello Azure network after failover.</span></span> <span data-ttu-id="cf24d-112">Egyszerűbbé teszi a ugyanazon IP-címeket való tartása hello hello helyreállítási csökkentésével kapcsolatos probléma hálózati feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="cf24d-112">Keeping hello same IP addresses simplifies hello recovery by reducing network related issues after failover.</span></span> <span data-ttu-id="cf24d-113">Azonban ha tooAzure replikál, szüksége lesz tooupdate útvonalak hello új hellyel hello IP-címek a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="cf24d-113">However, when you're replicating tooAzure, you will need tooupdate routes with hello new location of hello IP addresses after failover.</span></span> 


## <a name="retain-ip-addresses"></a><span data-ttu-id="cf24d-114">Tartsa meg az IP-címek</span><span class="sxs-lookup"><span data-stu-id="cf24d-114">Retain IP addresses</span></span>

<span data-ttu-id="cf24d-115">A Site Recovery biztosít hello funkció tooretain rögzített IP-címek, amikor tooAzure egy alhálózat feladatátvételi feladatátvétele.</span><span class="sxs-lookup"><span data-stu-id="cf24d-115">Site Recovery provides hello capability tooretain fixed IP addresses when failing over tooAzure, with a subnet failover.</span></span>

<span data-ttu-id="cf24d-116">Alhálózati feladatátvétellel egy bizonyos alhálózat jelen webhely 1 vagy 2. hely, de soha nem mindkét helyen egyszerre.</span><span class="sxs-lookup"><span data-stu-id="cf24d-116">With subnet failover, a specific subnet is present at Site 1 or Site 2, but never at both sites simultaneously.</span></span> <span data-ttu-id="cf24d-117">Rendelés toomaintain hello IP-címterének hello esemény a feladatátvétel, a programozott módon el rendezése hello útválasztó infrastruktúra toomove hello alhálózatok az egyik hely tooanother.</span><span class="sxs-lookup"><span data-stu-id="cf24d-117">In order toomaintain hello IP address space in hello event of a failover, you programmatically arrange for hello router infrastructure toomove hello subnets from one site tooanother.</span></span> <span data-ttu-id="cf24d-118">A feladatátvételi hello alhálózatok áthelyezése az hello társított védett virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="cf24d-118">During failover, hello subnets move with hello associated protected VMs.</span></span> <span data-ttu-id="cf24d-119">hello fő hátránya, hogy a hello esetre, ha nem, akkor toomove hello teljes alhálózattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="cf24d-119">hello main drawback is that in hello event of a failure, you have toomove hello whole subnet.</span></span>


### <a name="failover-example"></a><span data-ttu-id="cf24d-120">Feladatátvétel – példa</span><span class="sxs-lookup"><span data-stu-id="cf24d-120">Failover example</span></span>

<span data-ttu-id="cf24d-121">Egy példa a feladatátvevő tooAzure vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="cf24d-121">Let's look at an example for failover tooAzure.</span></span>

- <span data-ttu-id="cf24d-122">Egy ficticious vállalati, a Woodgrove Bank üzemeltető üzleti alkalmazások helyszíni infrastruktúra van.</span><span class="sxs-lookup"><span data-stu-id="cf24d-122">A ficticious company, Woodgrove Bank, has an on-premises infrastructure hosting their business apps.</span></span> <span data-ttu-id="cf24d-123">A mobilalkalmazás Azure üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="cf24d-123">Their mobile applications are hosted on Azure.</span></span>
- <span data-ttu-id="cf24d-124">Hello a helyszíni peremhálózati hálózati és hello Azure-beli virtuális hálózat közötti pont-pont (VPN) kapcsolatot biztosít a Woodgrove Bank virtuális gépeket az Azure és a helyszíni kiszolgálók közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="cf24d-124">Connectivity between Woodgrove Bank VMs in Azure and on-premises servers is provided by a site-to-site (VPN) connection between hello on-premises edge network and hello Azure virtual network.</span></span>
- <span data-ttu-id="cf24d-125">A VPN azt jelenti, hogy a vállalati virtuális hálózat az Azure-ban hello jelenik meg a helyszíni hálózat kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="cf24d-125">This VPN means that hello company's virtual network in Azure appears as an extension of their on-premises network.</span></span>
- <span data-ttu-id="cf24d-126">Woodgrove toouse Site Recovery tooreplicate helyszíni munkaterhelések tooAzure szeretne.</span><span class="sxs-lookup"><span data-stu-id="cf24d-126">Woodgrove wants toouse Site Recovery tooreplicate on-premises workloads tooAzure.</span></span>
 - <span data-ttu-id="cf24d-127">Woodgrove toodeal az alkalmazások és konfigurációk, amely IP-címek kódolt függ, és így kell tooretain IP-címek az alkalmazásuk kezelésére feladatátvételi tooAzure után van.</span><span class="sxs-lookup"><span data-stu-id="cf24d-127">Woodgrove has toodeal with applications and configurations which depend on hard-coded IP addresses, and thus need tooretain IP addresses for their applications after failover tooAzure.</span></span>
 - <span data-ttu-id="cf24d-128">Woodgrove rendelkezik hozzárendelt IP-címek tartomány 172.16.1.0/24 172.16.2.0/24 tooits erőforrásaihoz, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="cf24d-128">Woodgrove has assigned IP addresses from range 172.16.1.0/24, 172.16.2.0/24 tooits resources running in Azure.</span></span>


<span data-ttu-id="cf24d-129">A Woodgrove toobe képes tooreplicate a virtuális gépek tooAzure, miközben megtartja hello IP-címek ez milyen hello vállalatának szüksége toodo:</span><span class="sxs-lookup"><span data-stu-id="cf24d-129">For Woodgrove toobe able tooreplicate its VMS tooAzure while retaining hello IP addresses, here's what hello company needs toodo:</span></span>

1. <span data-ttu-id="cf24d-130">Hozzon létre egy Azure virtuális hálózatra.</span><span class="sxs-lookup"><span data-stu-id="cf24d-130">Create an Azure virtual network.</span></span> <span data-ttu-id="cf24d-131">Hello kiterjesztése a helyszíni hálózat, így az alkalmazások zökkenőmentesen feladatátvétel kell tenni.</span><span class="sxs-lookup"><span data-stu-id="cf24d-131">It should be an extension of hello on-premises network, so that applications can fail over seamlessly.</span></span>
2. <span data-ttu-id="cf24d-132">Azure lehetővé teszi, hogy Ön tooadd pont-pont VPN-kapcsolatot, továbbá toopoint-hely kapcsolatot toohello létrehozott virtuális hálózatokat az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="cf24d-132">Azure allows you tooadd site-to-site VPN connectivity, in addition toopoint-to-site connectivity toohello virtual networks created in Azure.</span></span>
3. <span data-ttu-id="cf24d-133">Hello pont-pont kapcsolat beállításakor a hello Azure hálózati, csak akkor, ha hello IP-címtartomány eltér a helyi IP-címtartomány hello irányítani tudja forgalom toohello helyszíni hely (helyi hálózati).</span><span class="sxs-lookup"><span data-stu-id="cf24d-133">When setting up hello site-to-site connection, in hello Azure network, you can route traffic toohello on-premises location (local-network) only if hello IP address range is different from hello on-premises IP address range.</span></span>
    - <span data-ttu-id="cf24d-134">Ennek az az oka Azure felhőbe archivált alhálózatok nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="cf24d-134">This is because Azure doesn’t support stretched subnets.</span></span> <span data-ttu-id="cf24d-135">Ezért ha alhálózati 192.168.1.0/24 helyi, nem adhat hozzá egy helyi hálózati 192.168.1.0/24 hello Azure-hálózatot a.</span><span class="sxs-lookup"><span data-stu-id="cf24d-135">So if you have subnet 192.168.1.0/24 on-premises, you can’t add a local-network 192.168.1.0/24 in hello Azure network.</span></span>
    - <span data-ttu-id="cf24d-136">Ez elvárható, hiszen az Azure nem tudja, hogy nincs aktív virtuális gépek szerepelnek hello alhálózati, és csak a vész-helyreállítási hello alhálózaton kerül létrehozásra.</span><span class="sxs-lookup"><span data-stu-id="cf24d-136">This is expected because Azure doesn’t know that there are no active VMs in hello subnet, and that hello subnet is being created for disaster recovery only.</span></span>
    - <span data-ttu-id="cf24d-137">toobe képes toocorrectly hálózati forgalom kívül egy Azure-hálózat hello alhálózatai hello és hello helyi-hálózat nem ütköznek.</span><span class="sxs-lookup"><span data-stu-id="cf24d-137">toobe able toocorrectly route network traffic out of an Azure network hello subnets in hello network and hello local-network mustn't conflict.</span></span>

![Alhálózati feladatátvétel előtt](./media/site-recovery-network-design/network-design7.png)

### <a name="before-failover"></a><span data-ttu-id="cf24d-139">Feladatátvétel előtt</span><span class="sxs-lookup"><span data-stu-id="cf24d-139">Before failover</span></span>

1. <span data-ttu-id="cf24d-140">Hozzon létre egy további hálózati (például a helyreállítási hálózati).</span><span class="sxs-lookup"><span data-stu-id="cf24d-140">Create an additional network (for example Recovery Network).</span></span> <span data-ttu-id="cf24d-141">Ez az hello hálózati amely átadja a virtuális gépek jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="cf24d-141">This is hello network in which failed over VMs are created.</span></span>
2. <span data-ttu-id="cf24d-142">egy feladatátvétel után a virtuális gép tulajdonságai hello őrzi meg, hogy a virtuális gépek IP-cím hello tooensure > **konfigurálása**, adja meg az azonos IP-cím virtuális gép rendelkezik a helyszínen, majd kattintson a hello hello **mentése**.</span><span class="sxs-lookup"><span data-stu-id="cf24d-142">tooensure that hello IP address for a VM is retained after a failover, in hello VM properties > **Configure**, specify hello same IP address that hello VM has on-premises, and click **Save**.</span></span>
3. <span data-ttu-id="cf24d-143">Hello VM feladatátvételt, ha az Azure Site Recovery rendeli a megadott IP-cím tooit hello.</span><span class="sxs-lookup"><span data-stu-id="cf24d-143">When hello VM is failed over, Azure Site Recovery will assign hello provided IP address tooit.</span></span>

    ![Hálózat tulajdonságai](./media/site-recovery-network-design/network-design8.png)

4. <span data-ttu-id="cf24d-145">Miután a feladatátvétel eseményindító aktiválódik, és hello virtuális gépeket hoz létre az Azure-ban szükséges hello IP-címmel, toohello hálózati használatával csatlakoztathatja a [tooVnet kapcsolatcsoporttal](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="cf24d-145">After failover is trigger is triggered, and hello VMs are created in Azure with hello required IP address, you can connect toohello network using a [Vnet tooVnet connection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span> <span data-ttu-id="cf24d-146">Ez a művelet parancsfájlalapú lehet.</span><span class="sxs-lookup"><span data-stu-id="cf24d-146">This action can be scripted.</span></span>
5. <span data-ttu-id="cf24d-147">Útvonalak toobe megfelelően módosítják, tooreflect kell, hogy a 192.168.1.0/24 tooAzure most már át lett helyezve.</span><span class="sxs-lookup"><span data-stu-id="cf24d-147">Routes need toobe appropriately modified, tooreflect that 192.168.1.0/24 has now moved tooAzure.</span></span>

    ![Alhálózati feladatátvételt követően](./media/site-recovery-network-design/network-design9.png)

### <a name="after-failover"></a><span data-ttu-id="cf24d-149">A feladatátvételt követően</span><span class="sxs-lookup"><span data-stu-id="cf24d-149">After failover</span></span>

<span data-ttu-id="cf24d-150">Ha a fenti módon még nem rendelkezik az Azure-hálózatot, létrehozhat egy-helyek közti VPN-kapcsolat az elsődleges hely és az Azure, a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="cf24d-150">If you don't have an Azure network as illustrated above, you can create a site-to-site VPN connection between your primary site and Azure, after failover.</span></span>

## <a name="change-ip-addresses"></a><span data-ttu-id="cf24d-151">IP-címek módosítása</span><span class="sxs-lookup"><span data-stu-id="cf24d-151">Change IP addresses</span></span>

<span data-ttu-id="cf24d-152">Ez [blogbejegyzés](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) azt ismerteti, hogyan tooset be hello Azure hálózati infrastruktúra, ha már nincs szükség tooretain IP-címek a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="cf24d-152">This [blog post](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) explains how tooset up hello Azure networking infrastructure when you don't need tooretain IP addresses after failover.</span></span> <span data-ttu-id="cf24d-153">Ez a kezdődik-e az alkalmazás leírása, néz ki, hogyan tooset a hálózatkezelés a helyszíni és az Azure-ban, és folyamat végén a feladatátvételeket a futtatásával kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="cf24d-153">It starts with an application description, looks at how tooset up networking on-premises and in Azure, and concludes with information about running failovers.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="cf24d-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf24d-154">Next steps</span></span>

<span data-ttu-id="cf24d-155">Nyissa meg túl[5. lépés: Azure előkészítése](vmware-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="cf24d-155">Go too[Step 5: Prepare Azure](vmware-walkthrough-prepare-azure.md)</span></span>
