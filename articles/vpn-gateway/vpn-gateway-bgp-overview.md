---
title: a BGP az Azure VPN Gatewayek aaaOverview |} Microsoft Docs
description: "Ez a cikk bemutatja, hogyan használható a BGP az Azure VPN Gateway megoldással együtt."
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
tags: 
ms.assetid: f8c3985c-c128-4f34-835c-0e88742bf36e
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/12/2017
ms.author: yushwang
ms.openlocfilehash: ced3f77ecd791c84fb72b96447e839be3bf94846
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-bgp-with-azure-vpn-gateways"></a><span data-ttu-id="d18ad-103">A BGP és az Azure VPN Gateway együttműködésének áttekintése</span><span class="sxs-lookup"><span data-stu-id="d18ad-103">Overview of BGP with Azure VPN Gateways</span></span>
<span data-ttu-id="d18ad-104">Ez a cikk ismerteti az Azure VPN Gateway megoldásban a BGP (Border Gateway Protocol) támogatását.</span><span class="sxs-lookup"><span data-stu-id="d18ad-104">This article provides an overview of BGP (Border Gateway Protocol) support in Azure VPN Gateways.</span></span>

<span data-ttu-id="d18ad-105">A BGP egy szabványos útválasztási protokoll hello általánosan használt az hello Internet tooexchange az Útválasztás és reachability information legalább két hálózat között.</span><span class="sxs-lookup"><span data-stu-id="d18ad-105">BGP is hello standard routing protocol commonly used in hello Internet tooexchange routing and reachability information between two or more networks.</span></span> <span data-ttu-id="d18ad-106">Ha az Azure virtuális hálózatok hello környezetben használják, BGP engedélyezi hello Azure VPN-átjáróként és BGP-társakat nevű a helyi VPN-eszközök vagy szomszédok tooexchange "irányítja a", amely mindkét átjárókat a hello rendelkezésre állását és elérhetőség tájékoztatja azok előtagok toogo hello átjárók és az érintett útválasztók keresztül.</span><span class="sxs-lookup"><span data-stu-id="d18ad-106">When used in hello context of Azure Virtual Networks, BGP enables hello Azure VPN Gateways and your on-premises VPN devices, called BGP peers or neighbors, tooexchange "routes" that will inform both gateways on hello availability and reachability for those prefixes toogo through hello gateways or routers involved.</span></span> <span data-ttu-id="d18ad-107">BGP is engedélyezhető tranzit útválasztást több hálózat között útvonalak BGP átjáró egy BGP-társ tooall való megtanulja propagálására más BGP-társak.</span><span class="sxs-lookup"><span data-stu-id="d18ad-107">BGP can also enable transit routing among multiple networks by propagating routes a BGP gateway learns from one BGP peer tooall other BGP peers.</span></span> 

## <a name="why-use-bgp"></a><span data-ttu-id="d18ad-108">Miért érdemes a BGP-t használni?</span><span class="sxs-lookup"><span data-stu-id="d18ad-108">Why use BGP?</span></span>
<span data-ttu-id="d18ad-109">A BGP egy olyan opcionális szolgáltatás, amely az Azure útvonalalapú VPN-átjáróival együtt használható.</span><span class="sxs-lookup"><span data-stu-id="d18ad-109">BGP is an optional feature you can use with Azure Route-Based VPN gateways.</span></span> <span data-ttu-id="d18ad-110">Is győződjön meg arról, hogy a helyszíni VPN-eszközök támogatják a BGP hello szolgáltatás engedélyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="d18ad-110">You should also make sure your on-premises VPN devices support BGP before you enable hello feature.</span></span> <span data-ttu-id="d18ad-111">Folytathatja toouse Azure VPN-átjáróként és BGP nélkül a helyszíni VPN-eszközök.</span><span class="sxs-lookup"><span data-stu-id="d18ad-111">You can continue toouse Azure VPN gateways and your on-premises VPN devices without BGP.</span></span> <span data-ttu-id="d18ad-112">Hello (BGP) nélkül statikus útvonalak használatával egyenértékű *és* használata a dinamikus útválasztási BGP a hálózatok és az Azure között.</span><span class="sxs-lookup"><span data-stu-id="d18ad-112">It is hello equivalent of using static routes (without BGP) *vs.* using dynamic routing with BGP between your networks and Azure.</span></span>

<span data-ttu-id="d18ad-113">A BGP használata számos előnyt és új képességet biztosít:</span><span class="sxs-lookup"><span data-stu-id="d18ad-113">There are several advantages and new capabilities with BGP:</span></span>

### <a name="support-automatic-and-flexible-prefix-updates"></a><span data-ttu-id="d18ad-114">Az automatikus és rugalmas előtagfrissítések támogatása</span><span class="sxs-lookup"><span data-stu-id="d18ad-114">Support automatic and flexible prefix updates</span></span>
<span data-ttu-id="d18ad-115">A BGP-vel csak akkor kell toodeclare előtag minimális tooa adott BGP társ hello IPsec S2S VPN-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="d18ad-115">With BGP, you only need toodeclare a minimum prefix tooa specific BGP peer over hello IPsec S2S VPN tunnel.</span></span> <span data-ttu-id="d18ad-116">Ez lehet a mérete legyen a gazdagép előtag (/ 32) a BGP társ IP-címét a helyszíni VPN-eszköz hello.</span><span class="sxs-lookup"><span data-stu-id="d18ad-116">It can be as small as a host prefix (/32) of hello BGP peer IP address of your on-premises VPN device.</span></span> <span data-ttu-id="d18ad-117">Azt is szabályozhatja, amely a helyszíni hálózati előtagok az Azure Virtual Network tooaccess tooadvertise tooAzure tooallow szeretné.</span><span class="sxs-lookup"><span data-stu-id="d18ad-117">You can control which on-premises network prefixes you want tooadvertise tooAzure tooallow your Azure Virtual Network tooaccess.</span></span>

<span data-ttu-id="d18ad-118">Is előfordulhat, hogy tartalmazza a VNet-címelőtagjait, például a nagy privát IP-címterület (például: 10.0.0.0/8) némelyike nagyobb előtagok hirdethető meg.</span><span class="sxs-lookup"><span data-stu-id="d18ad-118">You can also advertise larger prefixes that may include some of your VNet address prefixes, such as a large private IP address space (for example, 10.0.0.0/8).</span></span> <span data-ttu-id="d18ad-119">Vegye figyelembe, ha hello előtagok nem lehet azonos a VNet-előtagok egyike.</span><span class="sxs-lookup"><span data-stu-id="d18ad-119">Note though hello prefixes cannot be identical with any one of your VNet prefixes.</span></span> <span data-ttu-id="d18ad-120">A program elutasítja azonos tooyour VNet-előtagok útvonalak.</span><span class="sxs-lookup"><span data-stu-id="d18ad-120">Those routes identical tooyour VNet prefixes will be rejected.</span></span>

### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a><span data-ttu-id="d18ad-121">Többcsatornás üzemeltetés támogatása BGP-alapú automatikus feladatátvétellel, egy virtuális hálózat és egy helyszín között</span><span class="sxs-lookup"><span data-stu-id="d18ad-121">Support multiple tunnels between a VNet and an on-premises site with automatic failover based on BGP</span></span>
<span data-ttu-id="d18ad-122">Létrehozhat több kapcsolatot az Azure virtuális hálózat és a helyszíni VPN-eszközök közötti hello az ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="d18ad-122">You can establish multiple connections between your Azure VNet and your on-premises VPN devices in hello same location.</span></span> <span data-ttu-id="d18ad-123">Ez a funkció aktív-aktív konfigurációban hello a két hálózat között, több alagutat (elérési út) biztosít.</span><span class="sxs-lookup"><span data-stu-id="d18ad-123">This capability provides multiple tunnels (paths) between hello two networks in an active-active configuration.</span></span> <span data-ttu-id="d18ad-124">Ha valamelyik hello alagutak le van választva, hello megfelelő útvonalak BGP keresztül visszavonja, és hello forgalom automatikusan toohello fennmaradó alagutak lesz.</span><span class="sxs-lookup"><span data-stu-id="d18ad-124">If one of hello tunnels is disconnected, hello corresponding routes will be withdrawn via BGP and hello traffic automatically shifts toohello remaining tunnels.</span></span>

<span data-ttu-id="d18ad-125">a következő diagram hello a magas rendelkezésre állású telepítés egy egyszerű példa látható:</span><span class="sxs-lookup"><span data-stu-id="d18ad-125">hello following diagram shows a simple example of this highly available setup:</span></span>

![Több aktív elérési út](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a><span data-ttu-id="d18ad-127">Tranzit jellegű útválasztás támogatása a helyszíni hálózatok és több Azure virtuális hálózat között</span><span class="sxs-lookup"><span data-stu-id="d18ad-127">Support transit routing between your on-premises networks and multiple Azure VNets</span></span>
<span data-ttu-id="d18ad-128">A BGP lehetővé teszi több átjárók toolearn, és különböző hálózatokon előtagjait propagálására, hogy közvetlenül vagy közvetve csatlakoznak.</span><span class="sxs-lookup"><span data-stu-id="d18ad-128">BGP enables multiple gateways toolearn and propagate prefixes from different networks, whether they are directly or indirectly connected.</span></span> <span data-ttu-id="d18ad-129">Ezzel engedélyezhető az Azure VPN-átjárók használatával történő tranzit útválasztás a helyszínek között vagy több Azure virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="d18ad-129">This can enable transit routing with Azure VPN gateways between your on-premises sites or across multiple Azure Virtual Networks.</span></span>

<span data-ttu-id="d18ad-130">hello alábbi ábrán egy példa egy Többugrásos topológia több elérési utat, amely lehet átmenő forgalom hello két helyszíni hálózat között keresztül Azure VPN gatewayek hello Microsoft Networks belül:</span><span class="sxs-lookup"><span data-stu-id="d18ad-130">hello following diagram shows an example of a multi-hop topology with multiple paths that can transit traffic between hello two on-premises networks through Azure VPN gateways within hello Microsoft Networks:</span></span>

![Többugrásos átvitel](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faq"></a><span data-ttu-id="d18ad-132">A BGP – GYAKORI KÉRDÉSEK</span><span class="sxs-lookup"><span data-stu-id="d18ad-132">BGP FAQ</span></span>
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="next-steps"></a><span data-ttu-id="d18ad-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d18ad-133">Next steps</span></span>
<span data-ttu-id="d18ad-134">Lásd: [BGP-első lépések az Azure VPN gatewayek](vpn-gateway-bgp-resource-manager-ps.md) vonatkozó lépéseket tooconfigure BGP a létesítmények közötti és VNet – VNet kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="d18ad-134">See [Getting started with BGP on Azure VPN gateways](vpn-gateway-bgp-resource-manager-ps.md) for steps tooconfigure BGP for your cross-premises and VNet-to-VNet connections.</span></span>

