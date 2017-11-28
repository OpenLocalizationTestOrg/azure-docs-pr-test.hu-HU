---
title: "az Azure Site Recovery két Azure-régiók közötti aaaNetwork leképezési |} Microsoft Docs"
description: "Az Azure Site Recovery koordinálja hello replikációjának, feladatátvételének és helyreállításának virtuális gépek és fizikai kiszolgálók. További információk a feladatátvételi tooAzure vagy egy másodlagos adatközpontba."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 08/11/2017
ms.author: pratshar
ms.openlocfilehash: 4f80c44e3f94eaf446bc01a7041d91fe34aa78d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="network-mapping-between-two-azure-regions"></a><span data-ttu-id="14d0e-104">Két Azure-régiók közötti hálózatleképezés</span><span class="sxs-lookup"><span data-stu-id="14d0e-104">Network mapping between two Azure regions</span></span>


<span data-ttu-id="14d0e-105">Ez a cikk ismerteti, hogyan toomap Azure virtuális hálózatok az két Azure-régiók egymással.</span><span class="sxs-lookup"><span data-stu-id="14d0e-105">This article describes how toomap Azure virtual networks of two Azure regions with each other.</span></span> <span data-ttu-id="14d0e-106">A hálózatleképezés biztosítja, hogy a replikált virtuális gép létrehozásakor hello cél Azure-régiót jön létre, hello virtuális hálózathoz csatlakoztatott toovirtual hálózati hello forrás virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="14d0e-106">Network mapping ensures that when replicated virtual machine is created in hello target Azure region, it is created on hello virtual network that is mapped toovirtual network of hello source virtual machine.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="14d0e-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="14d0e-107">Prerequisites</span></span>
<span data-ttu-id="14d0e-108">Mielőtt leképez hálózatok mindenképpen, létrehozott [Azure virtuális hálózataihoz](../virtual-network/virtual-networks-overview.md) mindkét forrás és cél Azure-régiók.</span><span class="sxs-lookup"><span data-stu-id="14d0e-108">Before you map networks make sure, you have created [Azure virtual networks](../virtual-network/virtual-networks-overview.md) in both source and target Azure regions.</span></span>

## <a name="map-networks"></a><span data-ttu-id="14d0e-109">Hálózatok leképezése</span><span class="sxs-lookup"><span data-stu-id="14d0e-109">Map networks</span></span>

<span data-ttu-id="14d0e-110">toomap egy Azure-régiót tooanother virtuális hálózat egy másik régióban, nyissa meg tooSite Recovery-infrastruktúra az Azure virtuális hálózat -> Hálózatleképezés (az Azure Virtual Machines), és hozzon létre egy hálózatra való leképezés.</span><span class="sxs-lookup"><span data-stu-id="14d0e-110">toomap an Azure virtual network in one Azure region tooanother virtual network in another region, go tooSite Recovery Infrastructure -> Network Mapping (For Azure Virtual Machines) and create a network mapping.</span></span>

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping1.png)


<span data-ttu-id="14d0e-112">Az alábbi példában a virtuális gép Kelet-Ázsia régióban fut, és folyamatban van a hello replikálja a tooSoutheast Ázsia.</span><span class="sxs-lookup"><span data-stu-id="14d0e-112">In hello example below my virtual machine is running in East Asia region and is being replicated tooSoutheast Asia.</span></span>

<span data-ttu-id="14d0e-113">Válassza ki a forrás és cél hálózati hello, és kattintson az OK toocreate a hálózatra való leképezést a Kelet-Ázsia tooSoutheast Ázsia.</span><span class="sxs-lookup"><span data-stu-id="14d0e-113">Select hello source and target network and then click OK toocreate a network mapping from East Asia tooSoutheast Asia.</span></span>

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping2.png)


<span data-ttu-id="14d0e-115">Hello azonos dolog toocreate a hálózatra való leképezést a Délkelet-Ázsia tooEast Ázsia.</span><span class="sxs-lookup"><span data-stu-id="14d0e-115">Do hello same thing toocreate a network mapping from Southeast Asia tooEast Asia.</span></span>  
![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping3.png)


## <a name="mapping-network-when-enabling-replication"></a><span data-ttu-id="14d0e-117">Ha engedélyezve van a replikáció hálózati</span><span class="sxs-lookup"><span data-stu-id="14d0e-117">Mapping network when enabling replication</span></span>

<span data-ttu-id="14d0e-118">Ha hálózatra való leképezés nem történik meg, amikor egy virtuális gép hello első alkalommal űrlap egy Azure-régiót tooanother replikál, akkor választhatja célhálózat hello részeként ugyanazt a folyamatot.</span><span class="sxs-lookup"><span data-stu-id="14d0e-118">If network mapping is not done when you are replicating a virtual machine for hello first time form one Azure region tooanother, then you can choose target network as part of hello same process.</span></span> <span data-ttu-id="14d0e-119">A Site Recovery forrás régió tootarget régióban, és a cél régió toosource régió a kijelölés alapján hoz létre a hálózatok leképezését.</span><span class="sxs-lookup"><span data-stu-id="14d0e-119">Site Recovery creates network mappings from source region tootarget region and from target region toosource region based on this selection.</span></span>   

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping4.png)

<span data-ttu-id="14d0e-121">Alapértelmezés szerint a Site Recovery egy hálózatot hoz létre, amely azonos toohello Forráshálózat hello cél régióban, és adja hozzá a(z)-asr "hello forrás hálózat utótag toohello neve.</span><span class="sxs-lookup"><span data-stu-id="14d0e-121">By default, Site Recovery creates a network in hello target region that is identical toohello source network and by adding '-asr' as a suffix toohello name of hello source network.</span></span> <span data-ttu-id="14d0e-122">Testreszabás kattintva választhat egy már létrehozott hálózati.</span><span class="sxs-lookup"><span data-stu-id="14d0e-122">You can choose an already created network by clicking Customize.</span></span>

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping5.png)


<span data-ttu-id="14d0e-124">Hello hálózatra való leképezés már megtörtént, ha replikáció során hello cél virtuális hálózat nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="14d0e-124">If hello network mapping is already done, you can't change hello target virtual network while enabling replication.</span></span> <span data-ttu-id="14d0e-125">toochange, meglévő hálózatleképezés módosítása.</span><span class="sxs-lookup"><span data-stu-id="14d0e-125">toochange it, modify existing network mapping.</span></span>  

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/network-mapping6.png)

![Hálózatleképezés](./media/site-recovery-network-mapping-azure-to-azure/modify-network-mapping.png)

> [!IMPORTANT]
> <span data-ttu-id="14d0e-128">Ha módosít egy hálózatra való leképezést a terület-1 tooregion-2, ellenőrizze, hogy a terület-2 tooregion-1, valamint a hello hálózatleképezés módosítása.</span><span class="sxs-lookup"><span data-stu-id="14d0e-128">If you modify a network mapping from region-1 tooregion-2, make sure you modify hello network mapping from region-2 tooregion-1 as well.</span></span>
>
>


## <a name="subnet-selection"></a><span data-ttu-id="14d0e-129">Alhálózat kiválasztása</span><span class="sxs-lookup"><span data-stu-id="14d0e-129">Subnet selection</span></span>
<span data-ttu-id="14d0e-130">Alhálózati hello cél virtuális gép kiválasztása hello hello alhálózati hello forrás virtuális gép neve alapján történik.</span><span class="sxs-lookup"><span data-stu-id="14d0e-130">Subnet of hello target virtual machine is selected based on hello name of hello subnet of hello source virtual machine.</span></span> <span data-ttu-id="14d0e-131">Ha hello alhálózatának azonos nevet hello forrás virtuális gép hello célhálózat, elérhető, amely, amely hello cél virtuális gép van kiválasztva, majd.</span><span class="sxs-lookup"><span data-stu-id="14d0e-131">If there is a subnet of hello same name as that of hello source virtual machine available in hello target network, then that is chosen for hello target virtual machine.</span></span> <span data-ttu-id="14d0e-132">Ha nem található hello alhálózat azonos név hello célhálózat, majd betűrendben az első alhálózat van kiválasztva, mert a cél alhálózathoz hello.</span><span class="sxs-lookup"><span data-stu-id="14d0e-132">If there is no subnet with hello same name in hello target network, then alphabetically first subnet is chosen as hello target subnet.</span></span> <span data-ttu-id="14d0e-133">Ez az alhálózat tooCompute és hello virtuális gép hálózati beállításainak megnyitásával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="14d0e-133">You can modify this subnet by going tooCompute and Network settings of hello virtual machine.</span></span>

![Alhálózat módosítása](./media/site-recovery-network-mapping-azure-to-azure/modify-subnet.png)


## <a name="ip-address"></a><span data-ttu-id="14d0e-135">IP-cím</span><span class="sxs-lookup"><span data-stu-id="14d0e-135">IP address</span></span>

<span data-ttu-id="14d0e-136">Az egyes hello hálózati illesztő hello cél virtuális gép IP-cím van kiválasztva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="14d0e-136">IP address for each of hello network interface of hello target virtual machine is chosen as follows:</span></span>

### <a name="dhcp"></a><span data-ttu-id="14d0e-137">DHCP</span><span class="sxs-lookup"><span data-stu-id="14d0e-137">DHCP</span></span>
<span data-ttu-id="14d0e-138">Ha hello hálózati illesztő hello forrás virtuális gép nem használja a DHCP, majd hello cél virtuális gép hálózati adapter is értéke DHCP.</span><span class="sxs-lookup"><span data-stu-id="14d0e-138">If hello network interface of hello source virtual machine is using DHCP, then network interface of hello target virtual machine is also set as DHCP.</span></span>

### <a name="static-ip"></a><span data-ttu-id="14d0e-139">Statikus IP-cím</span><span class="sxs-lookup"><span data-stu-id="14d0e-139">Static IP</span></span>
<span data-ttu-id="14d0e-140">Ha hello hálózati illesztő hello forrás virtuális gép statikus IP-címet használ, majd hello cél virtuális gép hálózati adapter is toouse statikus IP-cím van beállítva.</span><span class="sxs-lookup"><span data-stu-id="14d0e-140">If hello network interface of hello source virtual machine is using Static IP, then network interface of hello target virtual machine is also set toouse Static IP.</span></span> <span data-ttu-id="14d0e-141">Statikus IP-cím van kiválasztva az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="14d0e-141">Static IP is chosen as follows:</span></span>

#### <a name="same-address-space"></a><span data-ttu-id="14d0e-142">Ugyanazt a címtartományt</span><span class="sxs-lookup"><span data-stu-id="14d0e-142">Same address space</span></span>

<span data-ttu-id="14d0e-143">Ha hello forrás alhálózat és a cél alhálózathoz hello hello azonos címtartományon, cél IP-címet ugyanaz, mint a hello IP hello hálózati adapter hello forrás virtuális gép be.</span><span class="sxs-lookup"><span data-stu-id="14d0e-143">If hello source subnet and hello target subnet have hello same address space, then target IP is set same as hello IP of  hello network interface of hello source virtual machine.</span></span> <span data-ttu-id="14d0e-144">Ha ugyanazon IP-cím nem érhető el, majd néhány más elérhető IP értéke hello cél IP-címet.</span><span class="sxs-lookup"><span data-stu-id="14d0e-144">If same IP is not available, then some other available IP is set as hello target IP.</span></span>

#### <a name="different-address-space"></a><span data-ttu-id="14d0e-145">Különböző címterület</span><span class="sxs-lookup"><span data-stu-id="14d0e-145">Different address space</span></span>

<span data-ttu-id="14d0e-146">Ha hello forrás alhálózati és a cél alhálózathoz hello eltérő, cél IP-címet legyen az bármilyen elérhető IP-hello cél alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="14d0e-146">If hello source subnet and hello target subnet have different address space, then target IP is set as any available IP in hello target subnet.</span></span>

<span data-ttu-id="14d0e-147">Minden hálózati illesztőn hello cél IP-címet tooCompute és hello virtuális gép hálózati beállításainak megnyitásával módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="14d0e-147">You can modify hello target IP on each network interface by going tooCompute and Network settings of hello virtual machine.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14d0e-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="14d0e-148">Next steps</span></span>

- <span data-ttu-id="14d0e-149">További tudnivalók [hálózati útmutatót az Azure virtuális gépek replikálása](site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="14d0e-149">Learn about [networking guidance for replicating Azure VMs](site-recovery-azure-to-azure-networking-guidance.md).</span></span>
