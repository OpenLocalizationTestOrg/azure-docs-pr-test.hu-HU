---
title: "az Azure Site Recovery szolgáltatással a Hyper-V (withSystem Center VMM) tooAzure replikációhoz hálózati aaaPlan |} Microsoft Docs"
description: "Ez a cikk ismerteti az alaphálózati topológia tervezése megadása kötelező, ha a Hyper-V virtuális gépek (VMM) tooAzure replikálása"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 41c3c83e-6b1a-496a-8179-498c552ef0c7
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/23/2017
ms.author: raynew
ms.openlocfilehash: 51ca8b939b6f96880f83599ea8009eb0bfa5b957
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="step-4-plan-networking-for-hyper-v-with-vmm-tooazure-replication"></a><span data-ttu-id="f4908-103">4. lépés: A Hyper-V (a VMM-mel) tooAzure replikáció hálózatkezelés tervezése</span><span class="sxs-lookup"><span data-stu-id="f4908-103">Step 4: Plan networking for Hyper-V (with VMM) tooAzure replication</span></span>

<span data-ttu-id="f4908-104">Végrehajtása után [kapacitástervezés](vmm-to-azure-walkthrough-capacity.md) (Ha végzett a teljes telepítés), ez a cikk toolearn tervezési szempontok, ha replikálása a helyszíni Hyper-V virtuális gépek System Center Virtual Machine Manager (VMM) a hálózattal kapcsolatos olvasása felhők tooAzure hello segítségével [Azure Site Recovery](site-recovery-overview.md) szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f4908-104">After performing [capacity planning](vmm-to-azure-walkthrough-capacity.md) (if you're doing a full deployment), read this article toolearn about network planning considerations when replicating on-premises Hyper-V VMs in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service.</span></span>

<span data-ttu-id="f4908-105">Ez a cikk hello alján a megjegyzéseket, vagy kérdései vannak a hello [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="f4908-105">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="network-mapping-for-replication-tooazure"></a><span data-ttu-id="f4908-106">A replikációs tooAzure hálózatleképezés</span><span class="sxs-lookup"><span data-stu-id="f4908-106">Network mapping for replication tooAzure</span></span>

<span data-ttu-id="f4908-107">A hálózatleképezés akkor használatos, ha a Hyper-V virtuális gépek (a VMM felügyelje) tooAzure replikálása.</span><span class="sxs-lookup"><span data-stu-id="f4908-107">Network mapping is used when replicating Hyper-V VMs (managed in VMM) tooAzure.</span></span> <span data-ttu-id="f4908-108">A hálózati forrás VMM-kiszolgálón a Virtuálisgép-hálózatok között, a hálózatleképezés kapcsolatot hoz létre, és a cél Azure-hálózatok.</span><span class="sxs-lookup"><span data-stu-id="f4908-108">Network mapping maps between VM networks on a source VMM server, and target Azure networks.</span></span> <span data-ttu-id="f4908-109">Leképezési hello a következő:</span><span class="sxs-lookup"><span data-stu-id="f4908-109">Mapping does hello following:</span></span>

- <span data-ttu-id="f4908-110">**Hálózati kapcsolat**– feladatátvétel után minden replikált Azure virtuális gépek a csatlakoztatott toohello leképezett Azure-hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="f4908-110">**Network connection**—After failover, all replicated Azure VMs are connected toohello mapped Azure network.</span></span> <span data-ttu-id="f4908-111">Feladatok átadása a hello azonos gépeire hálózati is csatlakozhat az egyéb tooeach, ha azokat a különböző helyreállítási tervben feladatátvételt.</span><span class="sxs-lookup"><span data-stu-id="f4908-111">All machines which fail over on hello same network can connect tooeach other, even if they failed over in different recovery plans.</span></span>
- <span data-ttu-id="f4908-112">**Hálózati átjáró**– hello cél Azure-hálózatban hálózati átjáró be van állítva, ha a virtuális gépek csatlakozni tud tooother a helyszíni virtuális gépek hello leképezve az átmeneti hálózati.</span><span class="sxs-lookup"><span data-stu-id="f4908-112">**Network gateway**—If a network gateway is set up on hello target Azure network, VMs can connect tooother on-premises virtual machines in hello mapped on-premised network.</span></span>

### <a name="prepare-vmm-for-network-mapping"></a><span data-ttu-id="f4908-113">A VMM hálózatleképezés előkészítése</span><span class="sxs-lookup"><span data-stu-id="f4908-113">Prepare VMM for network mapping</span></span>

<span data-ttu-id="f4908-114">Készítse elő a VMM a hálózatleképezés az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f4908-114">Prepare VMM for network mapping as follows:</span></span>

1. <span data-ttu-id="f4908-115">Ha még nincs fiókja, készítse elő a [VMM logikai hálózata](https://docs.microsoft.com/system-center/vmm/network-logical) , hogy mely hello Hyper-V gazdagépek találhatók hello felhő van társítva.</span><span class="sxs-lookup"><span data-stu-id="f4908-115">If you don't have one, prepare a [VMM logical network](https://docs.microsoft.com/system-center/vmm/network-logical) that's associated with hello cloud in which hello Hyper-V hosts are located.</span></span>
2. <span data-ttu-id="f4908-116">Ha még nincs fiókja, létrehoz egy [Virtuálisgép-hálózat](https://docs.microsoft.com/system-center/vmm/network-virtual) csatolt toohello logikai hálózat felett előkészített.</span><span class="sxs-lookup"><span data-stu-id="f4908-116">If you don't have one, created a [VM network](https://docs.microsoft.com/system-center/vmm/network-virtual) linked toohello logical network you prepared above.</span></span>
3. <span data-ttu-id="f4908-117">Csatlakoztassa a virtuális gépek a Hyper-V server/gazdagépfürt a VMM-felhő, toohello Virtuálisgép-hálózat hello hello.</span><span class="sxs-lookup"><span data-stu-id="f4908-117">Connect VMs on hello Hyper-V host server/cluster in hello VMM cloud, toohello VM network.</span></span>

 
<span data-ttu-id="f4908-118">Vegye figyelembe:</span><span class="sxs-lookup"><span data-stu-id="f4908-118">Note that:</span></span> 
- <span data-ttu-id="f4908-119">Új virtuális gépeket ad hozzá toohello forrás Virtuálisgép-hálózathoz kapcsolódó replikáláskor toohello leképezve az Azure-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="f4908-119">New VMs are added toohello source VM network are connected toohello mapped Azure network when replication occurs.</span></span>
- <span data-ttu-id="f4908-120">Ha hello célhálózatban már több alhálózat működik, és ezek egyikének rendelkezik hello neve megegyezik a virtual machine hello forrás alhálózati található, akkor hello replika virtuális gép a feladatátvételt követően toothat cél alhálózathoz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="f4908-120">If hello target network has multiple subnets, and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine connects toothat target subnet after failover.</span></span>
- <span data-ttu-id="f4908-121">Ha nincsenek egyező nevű cél alhálózathoz, hello virtuálisgép kapcsolódik toohello hello hálózat első alhálózatát.</span><span class="sxs-lookup"><span data-stu-id="f4908-121">If there’s no target subnet with a matching name, hello virtual machine connects toohello first subnet in hello network.</span></span>
- <span data-ttu-id="f4908-122">Készítse elő az Azure-hálózatot, és beállítása a hálózatok leképezését, hello forgatókönyv központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="f4908-122">You prepare an Azure network, and set up network mappings, as you deploy hello scenario.</span></span>

## <a name="connecting-tooazure-vms-after-failover"></a><span data-ttu-id="f4908-123">Csatlakozás tooAzure virtuális gépek a feladatátvételt követően</span><span class="sxs-lookup"><span data-stu-id="f4908-123">Connecting tooAzure VMs after failover</span></span>

<span data-ttu-id="f4908-124">A replikációs és feladatátvételi stratégiát megtervezésekor fontos kérdések hello egyik hogyan tooconnect toohello Azure virtuális gép a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="f4908-124">When planning your replication and failover strategy, one of hello key questions is how tooconnect toohello Azure VM after failover.</span></span> <span data-ttu-id="f4908-125">Számos több lehetősége a replika Azure virtuális gépek hálózati stratégia tervezésekor.</span><span class="sxs-lookup"><span data-stu-id="f4908-125">There are a couple of choices when designing your network strategy for replica Azure VMs:</span></span>

- <span data-ttu-id="f4908-126">**Különböző IP-cím**: kiválaszthatja toouse egy másik IP-címtartomány a hello replikált Azure Virtuálisgép-hálózatot.</span><span class="sxs-lookup"><span data-stu-id="f4908-126">**Use a different IP address**: You can select toouse a different IP address range for hello replicated Azure VM network.</span></span> <span data-ttu-id="f4908-127">Ez a forgatókönyv hello a virtuális gép a feladatátvételt követően lekérdezi a egy új IP-címet, és DNS-frissítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="f4908-127">In this scenario hello VM gets a new IP address after failover, and a DNS update is required.</span></span>
- <span data-ttu-id="f4908-128">**Tartsa meg ugyanazt a címet hello**: érdemes toouse hello azonos IP-címtartományt, az elsődleges helyszíni hálózati, a hello Azure-hálózatot a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="f4908-128">**Retain hello same IP address**: You might want toouse hello same IP address range as that in your primary on-premises network, for hello Azure network after failover.</span></span>  <span data-ttu-id="f4908-129">Egyszerűbbé teszi a ugyanazon IP-címeket való tartása hello hello helyreállítási csökkentésével kapcsolatos probléma hálózati feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="f4908-129">Keeping hello same IP addresses simplifies hello recovery by reducing network related issues after failover.</span></span> <span data-ttu-id="f4908-130">Azonban ha tooAzure replikál, szüksége lesz tooupdate útvonalak hello új hellyel hello IP-címek a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="f4908-130">However, when you're replicating tooAzure, you will need tooupdate routes with hello new location of hello IP addresses after failover.</span></span>


## <a name="retain-ip-addresses"></a><span data-ttu-id="f4908-131">Tartsa meg az IP-címek</span><span class="sxs-lookup"><span data-stu-id="f4908-131">Retain IP addresses</span></span>

<span data-ttu-id="f4908-132">A Site Recovery biztosít hello funkció tooretain rögzített IP-címek, amikor tooAzure egy alhálózat feladatátvételi feladatátvétele.</span><span class="sxs-lookup"><span data-stu-id="f4908-132">Site Recovery provides hello capability tooretain fixed IP addresses when failing over tooAzure, with a subnet failover.</span></span>

<span data-ttu-id="f4908-133">Alhálózati feladatátvétellel egy bizonyos alhálózat jelen webhely 1 vagy 2. hely, de soha nem mindkét helyen egyszerre.</span><span class="sxs-lookup"><span data-stu-id="f4908-133">With subnet failover, a specific subnet is present at Site 1 or Site 2, but never at both sites simultaneously.</span></span> <span data-ttu-id="f4908-134">Rendelés toomaintain hello IP-címterének hello esemény a feladatátvétel, a programozott módon el rendezése hello útválasztó infrastruktúra toomove hello alhálózatok az egyik hely tooanother.</span><span class="sxs-lookup"><span data-stu-id="f4908-134">In order toomaintain hello IP address space in hello event of a failover, you programmatically arrange for hello router infrastructure toomove hello subnets from one site tooanother.</span></span> <span data-ttu-id="f4908-135">A feladatátvételi hello alhálózatok áthelyezése az hello társított védett virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="f4908-135">During failover, hello subnets move with hello associated protected VMs.</span></span> <span data-ttu-id="f4908-136">hello fő hátránya, hogy a hello esetre, ha nem, akkor toomove hello teljes alhálózattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f4908-136">hello main drawback is that in hello event of a failure, you have toomove hello whole subnet.</span></span>



### <a name="failover-example"></a><span data-ttu-id="f4908-137">Feladatátvétel – példa</span><span class="sxs-lookup"><span data-stu-id="f4908-137">Failover example</span></span>

<span data-ttu-id="f4908-138">Egy példa a feladatátvevő tooAzure vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="f4908-138">Let's look at an example for failover tooAzure.</span></span>

- <span data-ttu-id="f4908-139">Egy ficticious vállalati, a Woodgrove Bank üzemeltető üzleti alkalmazások helyszíni infrastruktúra van.</span><span class="sxs-lookup"><span data-stu-id="f4908-139">A ficticious company, Woodgrove Bank, has an on-premises infrastructure hosting their business apps.</span></span> <span data-ttu-id="f4908-140">A mobilalkalmazás Azure üzemelteti.</span><span class="sxs-lookup"><span data-stu-id="f4908-140">Their mobile applications are hosted on Azure.</span></span>
- <span data-ttu-id="f4908-141">Hello a helyszíni peremhálózati hálózati és hello Azure-beli virtuális hálózat közötti pont-pont (VPN) kapcsolatot biztosít a Woodgrove Bank virtuális gépeket az Azure és a helyszíni kiszolgálók közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="f4908-141">Connectivity between Woodgrove Bank VMs in Azure and on-premises servers is provided by a site-to-site (VPN) connection between hello on-premises edge network and hello Azure virtual network.</span></span>
- <span data-ttu-id="f4908-142">A VPN azt jelenti, hogy a vállalati virtuális hálózat az Azure-ban hello jelenik meg a helyszíni hálózat kiterjesztése.</span><span class="sxs-lookup"><span data-stu-id="f4908-142">This VPN means that hello company's virtual network in Azure appears as an extension of their on-premises network.</span></span>
- <span data-ttu-id="f4908-143">Woodgrove toouse Site Recovery tooreplicate helyszíni munkaterhelések tooAzure szeretne.</span><span class="sxs-lookup"><span data-stu-id="f4908-143">Woodgrove wants toouse Site Recovery tooreplicate on-premises workloads tooAzure.</span></span>
 - <span data-ttu-id="f4908-144">Woodgrove toodeal az alkalmazások és konfigurációk, amely IP-címek kódolt függ, és így kell tooretain IP-címek az alkalmazásuk kezelésére feladatátvételi tooAzure után van.</span><span class="sxs-lookup"><span data-stu-id="f4908-144">Woodgrove has toodeal with applications and configurations which depend on hard-coded IP addresses, and thus need tooretain IP addresses for their applications after failover tooAzure.</span></span>
 - <span data-ttu-id="f4908-145">Woodgrove rendelkezik hozzárendelt IP-címek tartomány 172.16.1.0/24 172.16.2.0/24 tooits erőforrásaihoz, az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f4908-145">Woodgrove has assigned IP addresses from range 172.16.1.0/24, 172.16.2.0/24 tooits resources running in Azure.</span></span>


<span data-ttu-id="f4908-146">A Woodgrove toobe képes tooreplicate a virtuális gépek tooAzure, miközben megtartja hello IP-címek ez milyen hello vállalatának szüksége toodo:</span><span class="sxs-lookup"><span data-stu-id="f4908-146">For Woodgrove toobe able tooreplicate its VMs tooAzure while retaining hello IP addresses, here's what hello company needs toodo:</span></span>

1. <span data-ttu-id="f4908-147">Hozzon létre egy Azure virtuális hálózatra.</span><span class="sxs-lookup"><span data-stu-id="f4908-147">Create an Azure virtual network.</span></span> <span data-ttu-id="f4908-148">Hello kiterjesztése a helyszíni hálózat, így az alkalmazások zökkenőmentesen feladatátvétel kell tenni.</span><span class="sxs-lookup"><span data-stu-id="f4908-148">It should be an extension of hello on-premises network, so that applications can fail over seamlessly.</span></span>
2. <span data-ttu-id="f4908-149">Azure lehetővé teszi, hogy Ön tooadd pont-pont VPN-kapcsolatot, továbbá toopoint-hely kapcsolatot toohello létrehozott virtuális hálózatokat az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f4908-149">Azure allows you tooadd site-to-site VPN connectivity, in addition toopoint-to-site connectivity toohello virtual networks created in Azure.</span></span>
3. <span data-ttu-id="f4908-150">Hello pont-pont kapcsolat beállításakor a hello Azure hálózati, csak akkor, ha hello IP-címtartomány eltér a helyi IP-címtartomány hello irányítani tudja forgalom toohello helyszíni hely (helyi hálózati).</span><span class="sxs-lookup"><span data-stu-id="f4908-150">When setting up hello site-to-site connection, in hello Azure network, you can route traffic toohello on-premises location (local-network) only if hello IP address range is different from hello on-premises IP address range.</span></span>
    - <span data-ttu-id="f4908-151">Ennek az az oka Azure felhőbe archivált alhálózatok nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="f4908-151">This is because Azure doesn’t support stretched subnets.</span></span> <span data-ttu-id="f4908-152">Ezért ha alhálózati 192.168.1.0/24 helyi, nem adhat hozzá egy helyi hálózati 192.168.1.0/24 hello Azure-hálózatot a.</span><span class="sxs-lookup"><span data-stu-id="f4908-152">So if you have subnet 192.168.1.0/24 on-premises, you can’t add a local-network 192.168.1.0/24 in hello Azure network.</span></span>
    - <span data-ttu-id="f4908-153">Ez elvárható, hiszen az Azure nem tudja, hogy nincs aktív virtuális gépek szerepelnek hello alhálózati, és csak a vész-helyreállítási hello alhálózaton kerül létrehozásra.</span><span class="sxs-lookup"><span data-stu-id="f4908-153">This is expected because Azure doesn’t know that there are no active VMs in hello subnet, and that hello subnet is being created for disaster recovery only.</span></span>
    - <span data-ttu-id="f4908-154">toobe képes toocorrectly hálózati forgalom kívül egy Azure-hálózat hello alhálózatai hello és hello helyi-hálózat nem ütköznek.</span><span class="sxs-lookup"><span data-stu-id="f4908-154">toobe able toocorrectly route network traffic out of an Azure network hello subnets in hello network and hello local-network mustn't conflict.</span></span>

![Alhálózati feladatátvétel előtt](./media/vmm-to-azure-walkthrough-network/network-design7.png)

### <a name="before-failover"></a><span data-ttu-id="f4908-156">Feladatátvétel előtt</span><span class="sxs-lookup"><span data-stu-id="f4908-156">Before failover</span></span>

1. <span data-ttu-id="f4908-157">Hozzon létre egy további hálózati (például a helyreállítási hálózati).</span><span class="sxs-lookup"><span data-stu-id="f4908-157">Create an additional network (for example Recovery Network).</span></span> <span data-ttu-id="f4908-158">Ez az hello hálózati amely átadja a virtuális gépek jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="f4908-158">This is hello network in which failed over VMs are created.</span></span>
2. <span data-ttu-id="f4908-159">egy feladatátvétel után a virtuális gép tulajdonságai hello őrzi meg, hogy a virtuális gépek IP-cím hello tooensure > **konfigurálása**, adja meg az azonos IP-cím virtuális gép rendelkezik a helyszínen, majd kattintson a hello hello **mentése**.</span><span class="sxs-lookup"><span data-stu-id="f4908-159">tooensure that hello IP address for a VM is retained after a failover, in hello VM properties > **Configure**, specify hello same IP address that hello VM has on-premises, and click **Save**.</span></span>
3. <span data-ttu-id="f4908-160">Hello VM feladatátvételt, ha az Azure Site Recovery rendeli a megadott IP-cím tooit hello.</span><span class="sxs-lookup"><span data-stu-id="f4908-160">When hello VM is failed over, Azure Site Recovery will assign hello provided IP address tooit.</span></span>

    ![Hálózat tulajdonságai](./media/vmm-to-azure-walkthrough-network/network-design8.png)

4. <span data-ttu-id="f4908-162">Miután a feladatátvétel eseményindító aktiválódik, és hello virtuális gépeket hoz létre az Azure-ban szükséges hello IP-címmel, toohello hálózati használatával csatlakoztathatja a [Vnet tooVnet vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="f4908-162">After failover is trigger is triggered, and hello VMs are created in Azure with hello required IP address, you can connect toohello network using a [Vnet tooVnet vonnection](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span> <span data-ttu-id="f4908-163">Ez a művelet parancsfájlalapú lehet.</span><span class="sxs-lookup"><span data-stu-id="f4908-163">This action can be scripted.</span></span>
5. <span data-ttu-id="f4908-164">Útvonalak toobe megfelelően módosítják, tooreflect kell, hogy a 192.168.1.0/24 tooAzure most már át lett helyezve.</span><span class="sxs-lookup"><span data-stu-id="f4908-164">Routes need toobe appropriately modified, tooreflect that 192.168.1.0/24 has now moved tooAzure.</span></span>

    ![Alhálózati feladatátvételt követően](./media/vmm-to-azure-walkthrough-network/network-design9.png)

### <a name="after-failover"></a><span data-ttu-id="f4908-166">A feladatátvételt követően</span><span class="sxs-lookup"><span data-stu-id="f4908-166">After failover</span></span>

<span data-ttu-id="f4908-167">Ha a fenti módon még nem rendelkezik Azure-hálózat, létrehozhat egy-helyek közti VPN-kapcsolat az elsődleges hely és az Azure, failvoer után.</span><span class="sxs-lookup"><span data-stu-id="f4908-167">If you don't have an Azure network as illustrated above, you can create a site-to-site VPN connection between your primary site and Azure, after failvoer.</span></span>

## <a name="change-ip-addresses"></a><span data-ttu-id="f4908-168">IP-címek módosítása</span><span class="sxs-lookup"><span data-stu-id="f4908-168">Change IP addresses</span></span>

<span data-ttu-id="f4908-169">Ez [blogbejegyzés](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) azt ismerteti, hogyan tooset be hello Azure hálózati infrastruktúra, ha már nincs szükség tooretain IP-címek a feladatátvételt követően.</span><span class="sxs-lookup"><span data-stu-id="f4908-169">This [blog post](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) explains how tooset up hello Azure networking infrastructure when you don't need tooretain IP addresses after failover.</span></span> <span data-ttu-id="f4908-170">Ez a kezdődik-e az alkalmazás leírása, néz ki, hogyan tooset a hálózatkezelés a helyszíni és az Azure-ban, és folyamat végén a feladatátvételeket a futtatásával kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="f4908-170">It starts with an application description, looks at how tooset up networking on-premises and in Azure, and concludes with information about running failovers.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="f4908-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f4908-171">Next steps</span></span>

<span data-ttu-id="f4908-172">Nyissa meg túl[5. lépés: Azure előkészítése](vmm-to-azure-walkthrough-prepare-azure.md)</span><span class="sxs-lookup"><span data-stu-id="f4908-172">Go too[Step 5: Prepare Azure](vmm-to-azure-walkthrough-prepare-azure.md)</span></span>