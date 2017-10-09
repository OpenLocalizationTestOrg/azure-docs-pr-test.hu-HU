---
title: "aaaInternal terheléselosztó áttekintése |} Microsoft Docs"
description: "Belső terheléselosztó és a szolgáltatások áttekintése. A terheléselosztó működéséről az Azure és a lehetséges forgatókönyvek tooconfigure belső végpont"
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
ms.openlocfilehash: 9a901aad224d8821c154e130e142699d57282b25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="internal-load-balancer-overview"></a><span data-ttu-id="c4b5b-103">Belső terheléselosztó áttekintése</span><span class="sxs-lookup"><span data-stu-id="c4b5b-103">Internal load balancer overview</span></span>

<span data-ttu-id="c4b5b-104">Hello Internet felé néző terheléselosztó eltérően hello belső terheléselosztón (ILB) irányítja a forgalmat csak tooresources hello felhőalapú szolgáltatás, vagy a VPN tooaccess hello Azure-infrastruktúra használatával belül.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-104">Unlike hello Internet facing load balancer, hello internal load balancer (ILB) directs traffic only tooresources inside hello cloud service or using VPN tooaccess hello Azure infrastructure.</span></span> <span data-ttu-id="c4b5b-105">hello infrastruktúra korlátozza a hozzáférési toohello terhelésű virtuális IP-címek (VIP) egy felhőalapú szolgáltatás, vagy egy virtuális hálózatot, hogy soha nem fogja közvetlenül elérhetővé tooan Internet végpont.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-105">hello infrastructure restricts access toohello load balanced virtual IP addresses (VIPs) of a Cloud Service or a Virtual Network so that they will never be directly exposed tooan Internet endpoint.</span></span> <span data-ttu-id="c4b5b-106">Ez lehetővé teszi, hogy a belső üzletági üzletági (LOB) alkalmazások toorun az Azure-ban, és elérhető az hello felhőben, vagy a helyszíni erőforrások.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-106">This enables internal line of business (LOB) applications toorun in Azure and be accessed from within hello cloud or from resources on-premises.</span></span>

## <a name="why-you-may-need-an-internal-load-balancer"></a><span data-ttu-id="c4b5b-107">Miért szükség lehet egy belső terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="c4b5b-107">Why you may need an internal load balancer</span></span>

<span data-ttu-id="c4b5b-108">Az Azure belső betöltése terheléselosztás (ILB) belül egy felhőalapú szolgáltatás, vagy egy virtuális hálózat regionális hatókörbe telepített virtuális gépek közötti terheléselosztást biztosít.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-108">Azure Internal Load Balancing (ILB) provides load balancing between virtual machines that reside inside of a cloud service or a virtual network with a regional scope.</span></span> <span data-ttu-id="c4b5b-109">Hello használata és a virtuális hálózatokat regionális hatókörbe és konfigurálásával kapcsolatos további információkért lásd: [regionális virtuális hálózatokba](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) a hello Azure blog.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-109">For information about hello use and configuration of virtual networks with a regional scope, see [Regional Virtual Networks](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/) in hello Azure blog.</span></span> <span data-ttu-id="c4b5b-110">Az affinitáscsoporthoz konfigurált meglévő virtuális hálózatok nem használhatják az ILB-t.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-110">Existing virtual networks that have been configured for an affinity group cannot use ILB.</span></span>

<span data-ttu-id="c4b5b-111">ILB lehetővé teszi, hogy a következő típusú terheléselosztás hello:</span><span class="sxs-lookup"><span data-stu-id="c4b5b-111">ILB enables hello following types of load balancing:</span></span>

* <span data-ttu-id="c4b5b-112">Belül egy felhőalapú szolgáltatás, a virtuális gépek hello belüli virtuális gépek tooa készletből ugyanaz a felhőalapú szolgáltatás (lásd az 1. ábra).</span><span class="sxs-lookup"><span data-stu-id="c4b5b-112">Within a cloud service, from virtual machines tooa set of virtual machines that reside within hello same cloud service (see Figure 1).</span></span>
* <span data-ttu-id="c4b5b-113">A virtuális hálózaton belül hello virtuális hálózati tooa hello belüli virtuális gépek halmaza virtuális gépekről származó ugyanaz a felhőalapú szolgáltatás, hello virtuális hálózat (lásd a 2. ábra).</span><span class="sxs-lookup"><span data-stu-id="c4b5b-113">Within a virtual network, from virtual machines in hello virtual network tooa set of virtual machines that reside within hello same cloud service of hello virtual network (see Figure 2).</span></span>
* <span data-ttu-id="c4b5b-114">A létesítmények közötti virtuális hálózat, a helyszíni számítógépek tooa hello belüli virtuális gépek halmaza azonos a felhőalapú szolgáltatás a hello virtuális hálózat (lásd a 3. ábra).</span><span class="sxs-lookup"><span data-stu-id="c4b5b-114">For a cross-premises virtual network, from on-premises computers tooa set of virtual machines that reside within hello same cloud service of hello virtual network (see Figure 3).</span></span>
* <span data-ttu-id="c4b5b-115">Internet felé néző, a többrétegű alkalmazások hello háttér-rétegek nincsenek internetre de igénylő terheléselosztás forgalom hello internetre réteg alapján.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-115">Internet-facing, multi-tier applications in which hello back-end tiers are not Internet-facing but require load balancing for traffic from hello Internet-facing tier.</span></span>
* <span data-ttu-id="c4b5b-116">A LOB-alkalmazások anélkül, hogy további load balancer hardver- vagy Azure-ban üzemeltetett terheléselosztást.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-116">Load balancing for LOB applications hosted in Azure without requiring additional load balancer hardware or software.</span></span> <span data-ttu-id="c4b5b-117">A számítógépet, amelynek a forgalmát terhelés hello készletét is beleértve a helyszíni kiszolgálók átgondolni.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-117">Including on-premises servers in hello set of computers whose traffic is load balanced.</span></span>

## <a name="internet-facing-multi-tier-applications"></a><span data-ttu-id="c4b5b-118">Többrétegű alkalmazások internetes</span><span class="sxs-lookup"><span data-stu-id="c4b5b-118">Internet facing multi-tier applications</span></span>

<span data-ttu-id="c4b5b-119">hello webes réteg Internet felé néző végpontok van az internetes ügyfeleket, és egy elosztott terhelésű készlet része.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-119">hello web tier has Internet facing endpoints for Internet clients and is part of a load-balanced set.</span></span> <span data-ttu-id="c4b5b-120">hello terheléselosztó osztja el a TCP port 443 (HTTPS) toohello webkiszolgálók webes ügyfelekről érkező bejövő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-120">hello load balancer  distributes incoming traffic from web clients for TCP port 443 (HTTPS) toohello web servers.</span></span>

<span data-ttu-id="c4b5b-121">adatbázis-kiszolgálók hello hello webkiszolgálók használó tárolási ILB végpont mögött találhatók.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-121">hello database servers are behind an ILB endpoint which hello web servers use for storage.</span></span> <span data-ttu-id="c4b5b-122">Az adatbázis szolgáltatás az elosztott terhelésű végpont, mely akkor hello ILB set hello adatbázis kiszolgálója között elosztott terhelésű.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-122">This database service load balanced endpoint, which traffic is load balanced across hello database servers in hello ILB set.</span></span>

<span data-ttu-id="c4b5b-123">a következő kép bemutatja hello hello internetre irányuló többrétegű alkalmazást hello belül ugyanaz a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-123">hello following image shows hello Internet facing multi-tier application within hello same cloud service.</span></span>

![Belső terheléselosztási egyetlen felhőszolgáltatás](./media/load-balancer-internal-overview/IC736321.png)

<span data-ttu-id="c4b5b-125">1. ábra – internetre irányuló többrétegű alkalmazást</span><span class="sxs-lookup"><span data-stu-id="c4b5b-125">Figure 1 - Internet facing multi-tier application</span></span>

<span data-ttu-id="c4b5b-126">Egy másik lehetséges Többrétegű alkalmazások használata telepítésekor hello ILB tooa másik felhőalapú szolgáltatást, mint a hello hello ILB egy fogyasztó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-126">Another possible use for a multi-tier application is when hello ILB deployed tooa different cloud service than hello one consuming hello service for hello ILB.</span></span>

<span data-ttu-id="c4b5b-127">Felhőalapú szolgáltatások segítségével hello azonos virtuális hálózatban kell elérni toohello ILB végpont.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-127">Cloud services using hello same virtual network will have access toohello ILB endpoint.</span></span> <span data-ttu-id="c4b5b-128">a következő kép azt mutatja be, előtér-webkiszolgálók szerepelnek a hello adatbázis háttér-másik felhőalapú szolgáltatást, és segítségével hello hello ILB végpont hello belül azonos virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-128">hello following image shows front-end web servers are in a different cloud service from hello database back-end and using hello ILB endpoint within hello same virtual network.</span></span>

![Felhőszolgáltatások közötti belső terheléselosztás](./media/load-balancer-internal-overview/IC744147.png)

<span data-ttu-id="c4b5b-130">2. ábra – másik felhőalapú szolgáltatást az előtér-kiszolgálók</span><span class="sxs-lookup"><span data-stu-id="c4b5b-130">Figure 2 - Front-end servers in a different cloud service</span></span>

## <a name="intranet-line-of-business-applications"></a><span data-ttu-id="c4b5b-131">Intranetes üzletági alkalmazásokat</span><span class="sxs-lookup"><span data-stu-id="c4b5b-131">Intranet line of business applications</span></span>

<span data-ttu-id="c4b5b-132">Hello a helyi hálózaton-ügyfelektől érkező forgalom beolvasása terhelésű hello beállítása a VPN-kapcsolat tooAzure hálózat használatával LOB-kiszolgálók között.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-132">Traffic from clients on hello on-premises network get load-balanced across hello set of LOB servers using VPN connection tooAzure network.</span></span>

<span data-ttu-id="c4b5b-133">hello ügyfélszámítógép lesz hozzáférés tooan IP-cím pont toosite VPN-kapcsolattal Azure VPN szolgáltatásból.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-133">hello client machine will have access tooan IP address from Azure VPN service using point toosite VPN.</span></span> <span data-ttu-id="c4b5b-134">Lehetővé teszi a hello használata hello LOB-alkalmazás mögött hello ILB végpont.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-134">It allows hello use hello LOB application hosted behind hello ILB endpoint.</span></span>

![Pont toosite VPN-kapcsolattal belső terheléselosztás](./media/load-balancer-internal-overview/IC744148.png)

<span data-ttu-id="c4b5b-136">3. ábra - végpont hello LB mögött található, a LOB-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="c4b5b-136">Figure 3 - LOB applications hosted behind hello LB endpoint</span></span>

<span data-ttu-id="c4b5b-137">A hello LOB egy másik helyzet lehet toohave hely toosite VPN toohello virtuális hálózat ahol hello ILB végpont van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-137">Another scenario for hello LOB is toohave a site toosite VPN toohello virtual network where hello ILB endpoint is configured.</span></span> <span data-ttu-id="c4b5b-138">Ez lehetővé teszi a helyszíni hálózati forgalom irányított toobe toohello ILB végpontot.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-138">This allows on-premises network traffic toobe routed toohello ILB endpoint.</span></span>

![Hely toosite VPN használatával belső terheléselosztás](./media/load-balancer-internal-overview/IC744150.png)

<span data-ttu-id="c4b5b-140">4. ábra – a helyi hálózati forgalom irányított toohello ILB végpont</span><span class="sxs-lookup"><span data-stu-id="c4b5b-140">Figure 4 - On-premises network traffic routed toohello ILB endpoint</span></span>

## <a name="limitations"></a><span data-ttu-id="c4b5b-141">Korlátozások</span><span class="sxs-lookup"><span data-stu-id="c4b5b-141">Limitations</span></span>

<span data-ttu-id="c4b5b-142">Belső terheléselosztó nem támogatja a SNAT.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-142">Internal Load Balancer configurations do not support SNAT.</span></span> <span data-ttu-id="c4b5b-143">Ez a dokumentum a hello környezetében SNAT tooport színleg forrás hálózati címfordítás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-143">In hello context of this document, SNAT refers tooport masquerading source  network address translation.</span></span>  <span data-ttu-id="c4b5b-144">Amikor egy virtuális Gépet a load balancer készlet kell tooreach hello megfelelő belső terheléselosztó tartozó előtérbeli IP-cím tooscenarios vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-144">This applies tooscenarios where a VM in a load balancer pool needs tooreach hello respective internal Load Balancer's frontend IP address.</span></span> <span data-ttu-id="c4b5b-145">Ez a forgatókönyv nem támogatott belső terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-145">This scenario is not supported for internal Load Balancer.</span></span> <span data-ttu-id="c4b5b-146">Kapcsolódási hibák hello folyamata az elosztott terhelésű toohello hello folyamata szolgáltatásoktól VM történjen.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-146">Connection failures will occur when hello flow is load balanced toohello VM which originated hello flow.</span></span> <span data-ttu-id="c4b5b-147">Ilyen helyzetekben a proxy stílus terheléselosztó kell használnia.</span><span class="sxs-lookup"><span data-stu-id="c4b5b-147">You must use a proxy style load balancer for such scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4b5b-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4b5b-148">Next Steps</span></span>

[<span data-ttu-id="c4b5b-149">Azure Load Balancer Azure Resource Manager támogatása</span><span class="sxs-lookup"><span data-stu-id="c4b5b-149">Azure Resource Manager support for Azure Load Balancer</span></span>](load-balancer-arm.md)

[<span data-ttu-id="c4b5b-150">Ismerkedés az Internet felé néző terheléselosztó konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c4b5b-150">Get started configuring an Internet facing load balancer</span></span>](load-balancer-get-started-internet-arm-ps.md)

[<span data-ttu-id="c4b5b-151">Ismerkedés a belső terheléselosztók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c4b5b-151">Get started configuring an Internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="c4b5b-152">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c4b5b-152">Configure a Load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="c4b5b-153">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c4b5b-153">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
