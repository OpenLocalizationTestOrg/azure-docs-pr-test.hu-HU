---
title: "Belső terheléselosztó áttekintése |} Microsoft Docs"
description: "Belső terheléselosztó és a szolgáltatások áttekintése. A terheléselosztó belső végpont konfigurálása az Azure és a lehetséges forgatókönyvek működése"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 36065bfe-0ef1-46f9-a9e1-80b229105c85
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: d324aaf8ec2c8766d5cf11452158d14c19cba4d9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="internal-load-balancer-overview"></a><span data-ttu-id="0af8c-103">Belső terheléselosztó áttekintése</span><span class="sxs-lookup"><span data-stu-id="0af8c-103">Internal load balancer overview</span></span>

<span data-ttu-id="0af8c-104">A belső terheléselosztó (ILB) eltérően az internetre irányuló a terheléselosztóhoz, arra utasítja a forgalom csak az erőforráshoz, a felhőalapú szolgáltatás, vagy az Azure-infrastruktúra eléréséhez a VPN-kapcsolattal.</span><span class="sxs-lookup"><span data-stu-id="0af8c-104">Unlike the Internet facing load balancer, the internal load balancer (ILB) directs traffic only to resources inside the cloud service or using VPN to access the Azure infrastructure.</span></span> <span data-ttu-id="0af8c-105">Az infrastruktúra korlátozza a hozzáférést a terheléselosztó virtuális IP-címek (VIP) egy felhőalapú szolgáltatás, vagy egy virtuális hálózatot, hogy azok soha nem közvetlenül számára megjelenik egy Internet-végpontot.</span><span class="sxs-lookup"><span data-stu-id="0af8c-105">The infrastructure restricts access to the load balanced virtual IP addresses (VIPs) of a Cloud Service or a Virtual Network so that they will never be directly exposed to an Internet endpoint.</span></span> <span data-ttu-id="0af8c-106">Ez lehetővé teszi a belső üzletági üzletági (LOB) alkalmazások futtatása az Azure-ban, és elérhető a felhő vagy a helyszíni erőforrások.</span><span class="sxs-lookup"><span data-stu-id="0af8c-106">This enables internal line of business (LOB) applications to run in Azure and be accessed from within the cloud or from resources on-premises.</span></span>

## <a name="why-you-may-need-an-internal-load-balancer"></a><span data-ttu-id="0af8c-107">Miért szükség lehet egy belső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="0af8c-107">Why you may need an internal load balancer</span></span>

<span data-ttu-id="0af8c-108">Az Azure belső betöltése terheléselosztás (ILB) belül egy felhőalapú szolgáltatás, vagy egy virtuális hálózat regionális hatókörbe telepített virtuális gépek közötti terheléselosztást biztosít.</span><span class="sxs-lookup"><span data-stu-id="0af8c-108">Azure Internal Load Balancing (ILB) provides load balancing between virtual machines that reside inside of a cloud service or a virtual network with a regional scope.</span></span> <span data-ttu-id="0af8c-109">A használat és a virtuális hálózatokat regionális hatókörbe és konfigurálásával kapcsolatos további információkért lásd: [regionális virtuális hálózatokba](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) az Azure blogján.</span><span class="sxs-lookup"><span data-stu-id="0af8c-109">For information about the use and configuration of virtual networks with a regional scope, see [Regional Virtual Networks](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) in the Azure blog.</span></span> <span data-ttu-id="0af8c-110">Az affinitáscsoporthoz konfigurált meglévő virtuális hálózatok nem használhatják az ILB-t.</span><span class="sxs-lookup"><span data-stu-id="0af8c-110">Existing virtual networks that have been configured for an affinity group cannot use ILB.</span></span>

<span data-ttu-id="0af8c-111">ILB lehetővé teszi, hogy a következő típusú terheléselosztási:</span><span class="sxs-lookup"><span data-stu-id="0af8c-111">ILB enables the following types of load balancing:</span></span>

* <span data-ttu-id="0af8c-112">Egy felhőalapú szolgáltatás, a virtuális gépek számára a virtuális gépek csoportja tartalmazza az ugyanazon a felhőalapú szolgáltatás belüli belül (lásd az 1. ábra).</span><span class="sxs-lookup"><span data-stu-id="0af8c-112">Within a cloud service, from virtual machines to a set of virtual machines that reside within the same cloud service (see Figure 1).</span></span>
* <span data-ttu-id="0af8c-113">A virtuális hálózaton belül a virtuális hálózat a virtuális gépek csoportja tartalmazza az ugyanazon a felhőalapú szolgáltatás, a virtuális található virtuális gépekről származó (lásd a 2. ábra) hálózati.</span><span class="sxs-lookup"><span data-stu-id="0af8c-113">Within a virtual network, from virtual machines in the virtual network to a set of virtual machines that reside within the same cloud service of the virtual network (see Figure 2).</span></span>
* <span data-ttu-id="0af8c-114">A létesítmények közötti virtuális hálózat a helyszíni számítógépek számára a virtuális gépek csoportja tartalmazza az ugyanazon a felhőalapú szolgáltatás, a virtuális belüli (lásd a 3. ábra) hálózati.</span><span class="sxs-lookup"><span data-stu-id="0af8c-114">For a cross-premises virtual network, from on-premises computers to a set of virtual machines that reside within the same cloud service of the virtual network (see Figure 3).</span></span>
* <span data-ttu-id="0af8c-115">Az Internet felé néző, a többrétegű alkalmazások, amelyben a háttér-rétegek nincsenek internetre irányuló, de szükséges hálózati terheléselosztást a forgalmat az Internet felé néző réteg alapján.</span><span class="sxs-lookup"><span data-stu-id="0af8c-115">Internet-facing, multi-tier applications in which the back-end tiers are not Internet-facing but require load balancing for traffic from the Internet-facing tier.</span></span>
* <span data-ttu-id="0af8c-116">A LOB-alkalmazások anélkül, hogy további load balancer hardver- vagy Azure-ban üzemeltetett terheléselosztást.</span><span class="sxs-lookup"><span data-stu-id="0af8c-116">Load balancing for LOB applications hosted in Azure without requiring additional load balancer hardware or software.</span></span> <span data-ttu-id="0af8c-117">Elosztott terhelésű többek között a helyszíni kiszolgálók a számítógépet, amelynek a forgalmát terhelés készletében.</span><span class="sxs-lookup"><span data-stu-id="0af8c-117">Including on-premises servers in the set of computers whose traffic is load balanced.</span></span>

## <a name="internet-facing-multi-tier-applications"></a><span data-ttu-id="0af8c-118">Többrétegű alkalmazások internetes</span><span class="sxs-lookup"><span data-stu-id="0af8c-118">Internet facing multi-tier applications</span></span>

<span data-ttu-id="0af8c-119">A webes réteg Internet felé néző végpontok van az internetes ügyfeleket, és egy elosztott terhelésű készlet része.</span><span class="sxs-lookup"><span data-stu-id="0af8c-119">The web tier has Internet facing endpoints for Internet clients and is part of a load-balanced set.</span></span> <span data-ttu-id="0af8c-120">A load balancer továbbítja a webkiszolgálók TCP port 443-as (HTTPS) webalkalmazás-ügyfelekről érkező bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="0af8c-120">The load balancer  distributes incoming traffic from web clients for TCP port 443 (HTTPS) to the web servers.</span></span>

<span data-ttu-id="0af8c-121">Az adatbázis-kiszolgálók a webkiszolgálók használó tárolási ILB végpont mögött találhatók.</span><span class="sxs-lookup"><span data-stu-id="0af8c-121">The database servers are behind an ILB endpoint which the web servers use for storage.</span></span> <span data-ttu-id="0af8c-122">Az adatbázis szolgáltatás az elosztott terhelésű végpont, mely forgalom az adatbázis-kiszolgálóin ILB készletében betöltése.</span><span class="sxs-lookup"><span data-stu-id="0af8c-122">This database service load balanced endpoint, which traffic is load balanced across the database servers in the ILB set.</span></span>

<span data-ttu-id="0af8c-123">A következő kép bemutatja, az internetre irányuló a többrétegű alkalmazást belül az azonos felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0af8c-123">The following image shows the Internet facing multi-tier application within the same cloud service.</span></span>

![Belső terheléselosztási egyetlen felhőszolgáltatás](./media/load-balancer-internal-overview/IC736321.png)

<span data-ttu-id="0af8c-125">1. ábra – internetre irányuló többrétegű alkalmazást</span><span class="sxs-lookup"><span data-stu-id="0af8c-125">Figure 1 - Internet facing multi-tier application</span></span>

<span data-ttu-id="0af8c-126">Egy másik lehetséges Többrétegű alkalmazások használata a ILB központi telepítési fel a Példánynak a szolgáltatás egy másik felhőalapú szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0af8c-126">Another possible use for a multi-tier application is when the ILB deployed to a different cloud service than the one consuming the service for the ILB.</span></span>

<span data-ttu-id="0af8c-127">Felhőszolgáltatások a azonos virtuális hálózaton keresztül hozzáférhet a Példánynak a végponthoz.</span><span class="sxs-lookup"><span data-stu-id="0af8c-127">Cloud services using the same virtual network will have access to the ILB endpoint.</span></span> <span data-ttu-id="0af8c-128">A következő kép bemutatja az előtér-webkiszolgáló egy másik felhőalapú szolgáltatást, az adatbázis-háttér és használata a ILB végpontot a virtuális hálózaton belül vannak.</span><span class="sxs-lookup"><span data-stu-id="0af8c-128">The following image shows front-end web servers are in a different cloud service from the database back-end and using the ILB endpoint within the same virtual network.</span></span>

![Felhőszolgáltatások közötti belső terheléselosztás](./media/load-balancer-internal-overview/IC744147.png)

<span data-ttu-id="0af8c-130">2. ábra – másik felhőalapú szolgáltatást az előtér-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="0af8c-130">Figure 2 - Front-end servers in a different cloud service</span></span>

## <a name="intranet-line-of-business-applications"></a><span data-ttu-id="0af8c-131">Intranetes üzletági alkalmazásokat</span><span class="sxs-lookup"><span data-stu-id="0af8c-131">Intranet line of business applications</span></span>

<span data-ttu-id="0af8c-132">A helyszíni hálózaton-ügyfelektől érkező forgalom beolvasása terhelésű közötti VPN-kapcsolat az Azure-hálózat használatával LOB-kiszolgálók készlete.</span><span class="sxs-lookup"><span data-stu-id="0af8c-132">Traffic from clients on the on-premises network get load-balanced across the set of LOB servers using VPN connection to Azure network.</span></span>

<span data-ttu-id="0af8c-133">Az ügyfélszámítógép hozzáférhet a IP-cím pont közötti VPN használatával Azure VPN szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="0af8c-133">The client machine will have access to an IP address from Azure VPN service using point to site VPN.</span></span> <span data-ttu-id="0af8c-134">A LOB-alkalmazások a ILB végpont mögött található engedélyezze a használatát.</span><span class="sxs-lookup"><span data-stu-id="0af8c-134">It allows the use the LOB application hosted behind the ILB endpoint.</span></span>

![Belső terheléselosztás pont közötti VPN használatával](./media/load-balancer-internal-overview/IC744148.png)

<span data-ttu-id="0af8c-136">3. ábra - végpont LB mögött található, a LOB-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="0af8c-136">Figure 3 - LOB applications hosted behind the LB endpoint</span></span>

<span data-ttu-id="0af8c-137">Az üzleti egy másik forgatókönyve, hogy a webhelyek közötti VPN a virtuális hálózathoz, ahol a ILB végpont van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="0af8c-137">Another scenario for the LOB is to have a site to site VPN to the virtual network where the ILB endpoint is configured.</span></span> <span data-ttu-id="0af8c-138">Ez lehetővé teszi a helyszíni hálózati forgalom átirányítását a ILB végpont.</span><span class="sxs-lookup"><span data-stu-id="0af8c-138">This allows on-premises network traffic to be routed to the ILB endpoint.</span></span>

![Belső terheléselosztás webhelyek közötti VPN használatával](./media/load-balancer-internal-overview/IC744150.png)

<span data-ttu-id="0af8c-140">4. ábra – a helyi hálózati forgalom irányítja át a ILB végpont</span><span class="sxs-lookup"><span data-stu-id="0af8c-140">Figure 4 - On-premises network traffic routed to the ILB endpoint</span></span>

## <a name="limitations"></a><span data-ttu-id="0af8c-141">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="0af8c-141">Limitations</span></span>

<span data-ttu-id="0af8c-142">Belső terheléselosztó nem támogatja a SNAT.</span><span class="sxs-lookup"><span data-stu-id="0af8c-142">Internal Load Balancer configurations do not support SNAT.</span></span> <span data-ttu-id="0af8c-143">Ez a dokumentum összefüggésében SNAT port színleg forrás hálózati címfordítás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="0af8c-143">In the context of this document, SNAT refers to port masquerading source  network address translation.</span></span>  <span data-ttu-id="0af8c-144">Ez vonatkozik helyzetek, amikor egy virtuális Gépet a load balancer készlet kell-e a megfelelő belső terheléselosztó tartozó előtérbeli IP-cím elérésére.</span><span class="sxs-lookup"><span data-stu-id="0af8c-144">This applies to scenarios where a VM in a load balancer pool needs to reach the respective internal Load Balancer's frontend IP address.</span></span> <span data-ttu-id="0af8c-145">Ez a forgatókönyv nem támogatott belső terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="0af8c-145">This scenario is not supported for internal Load Balancer.</span></span> <span data-ttu-id="0af8c-146">Kapcsolódási hibák történik, ha a folyamat a virtuális gépre, a folyamat szolgáltatásoktól elosztott terhelésű.</span><span class="sxs-lookup"><span data-stu-id="0af8c-146">Connection failures will occur when the flow is load balanced to the VM which originated the flow.</span></span> <span data-ttu-id="0af8c-147">Ilyen helyzetekben a proxy stílus terheléselosztó kell használnia.</span><span class="sxs-lookup"><span data-stu-id="0af8c-147">You must use a proxy style load balancer for such scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0af8c-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0af8c-148">Next Steps</span></span>

[<span data-ttu-id="0af8c-149">Azure Load Balancer Azure Resource Manager támogatása</span><span class="sxs-lookup"><span data-stu-id="0af8c-149">Azure Resource Manager support for Azure Load Balancer</span></span>](load-balancer-arm.md)

[<span data-ttu-id="0af8c-150">Ismerkedés az Internet felé néző terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0af8c-150">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="0af8c-151">Ismerkedés a belső terheléselosztók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0af8c-151">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="0af8c-152">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0af8c-152">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="0af8c-153">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0af8c-153">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
