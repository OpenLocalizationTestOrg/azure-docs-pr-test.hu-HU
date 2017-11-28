---
title: "Gyakori kérdések az Azure Application Gateway |} Microsoft Docs"
description: "Ezen a lapon biztosít Azure Application Gateway gyakran feltett kérdésekre adott válaszok"
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: d54ee7ec-4d6b-4db7-8a17-6513fda7e392
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/19/2017
ms.author: gwallace
ms.openlocfilehash: 4e6244d92f41e0aa5c8a70db0db2881036984247
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a><span data-ttu-id="f850d-103">Az Alkalmazásátjáró gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="f850d-103">Frequently asked questions for Application Gateway</span></span>

## <a name="general"></a><span data-ttu-id="f850d-104">Általános kérdések</span><span class="sxs-lookup"><span data-stu-id="f850d-104">General</span></span>

<span data-ttu-id="f850d-105">**Q. Mi az Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="f850d-105">**Q. What is Application Gateway?**</span></span>

<span data-ttu-id="f850d-106">Azure Application Gateway egy alkalmazás kézbesítési vezérlő LÉPETT szolgáltatásként, az alkalmazások képességei terheléselosztási különböző réteg 7 kínál.</span><span class="sxs-lookup"><span data-stu-id="f850d-106">Azure Application Gateway is an Application Delivery Controller (ADC) as a service, offering various layer 7 load balancing capabilities for your applications.</span></span> <span data-ttu-id="f850d-107">Magas rendelkezésre állású és méretezhető szolgáltatást, amely teljes mértékben kezeli az Azure biztosít.</span><span class="sxs-lookup"><span data-stu-id="f850d-107">It offers highly available and scalable service, which is fully managed by Azure.</span></span>

<span data-ttu-id="f850d-108">**Q. Milyen funkciókat támogatja az Alkalmazásátjáró?**</span><span class="sxs-lookup"><span data-stu-id="f850d-108">**Q. What features does Application Gateway support?**</span></span>

<span data-ttu-id="f850d-109">Alkalmazásátjáró támogatja SSL-feladatkiszervezést és végpontok közötti SSL, webalkalmazási tűzfal, munkamenet cookie-alapú kapcsolat, URL-cím elérési út-alapú útválasztási, több helyet üzemeltető és mások.</span><span class="sxs-lookup"><span data-stu-id="f850d-109">Application Gateway supports SSL offloading and end to end SSL, Web Application Firewall, cookie-based session affinity, url path-based routing, multi site hosting, and others.</span></span> <span data-ttu-id="f850d-110">Támogatott szolgáltatások teljes listájának megtekintéséhez keresse fel a [Alkalmazásátjáró bemutatása](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="f850d-110">For a full list of supported features, visit [Introduction to Application Gateway](application-gateway-introduction.md)</span></span>

<span data-ttu-id="f850d-111">**Q. Mi az a különbség az Alkalmazásátjáró és az Azure Load Balancer?**</span><span class="sxs-lookup"><span data-stu-id="f850d-111">**Q. What is the difference between Application Gateway and Azure Load Balancer?**</span></span>

<span data-ttu-id="f850d-112">Alkalmazásátjáró 7 réteg terheléselosztó, amely azt jelenti, hogy működik együtt a csak internetes forgalmat (HTTP/HTTPS/WebSocket).</span><span class="sxs-lookup"><span data-stu-id="f850d-112">Application Gateway is a layer 7 load balancer, which means it works with web traffic only (HTTP/HTTPS/WebSocket).</span></span> <span data-ttu-id="f850d-113">Például az SSL-lezárást, a munkamenet cookie-alapú kapcsolat és a ciklikus multiplexelés képességek terheléselosztási forgalom támogatja.</span><span class="sxs-lookup"><span data-stu-id="f850d-113">It supports capabilities such as SSL termination, cookie-based session affinity, and round robin for load balancing traffic.</span></span> <span data-ttu-id="f850d-114">Terheléselosztó, kiegyensúlyozza forgalom rétegben 4 (TCP/UDP).</span><span class="sxs-lookup"><span data-stu-id="f850d-114">Load Balancer, load balances traffic at layer 4 (TCP/UDP).</span></span>

<span data-ttu-id="f850d-115">**Q. Milyen protokollokat támogatja az Alkalmazásátjáró?**</span><span class="sxs-lookup"><span data-stu-id="f850d-115">**Q. What protocols does Application Gateway support?**</span></span>

<span data-ttu-id="f850d-116">Alkalmazás-átjáró támogatja a HTTP, HTTPS és WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f850d-116">Application Gateway supports HTTP, HTTPS, and WebSocket.</span></span>

<span data-ttu-id="f850d-117">**Q. Által támogatott ma háttérkészlet részeként?**</span><span class="sxs-lookup"><span data-stu-id="f850d-117">**Q. What resources are supported today as part of backend pool?**</span></span>

<span data-ttu-id="f850d-118">Háttérkészlet állhat hálózati adapter virtuálisgép-méretezési csoportok, nyilvános IP-címek, belső IP-címek, teljes tartománynév (FQDN) neve, és több-bérlős vissza-végpontok, például az Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="f850d-118">Backend pools can be composed of NICs, virtual machine scale sets, public IPs, internal IPs, fully qualified domain names (FQDN), and multi-tenant back-ends like Azure Web Apps.</span></span> <span data-ttu-id="f850d-119">Alkalmazás átjáró háttér készlettag nem rendelkezésre állási csoportok vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="f850d-119">Application Gateway backend pool members are not tied to an availability set.</span></span> <span data-ttu-id="f850d-120">Háttér-készletek tagjai között, fürtök és adatközpontok, vagy lehet Azure-on kívüli mindaddig, amíg az IP-kapcsolattal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="f850d-120">Members of backend pools can be across clusters, data centers, or outside of Azure as long as they have IP connectivity.</span></span>

<span data-ttu-id="f850d-121">**Q. Milyen régiók érhető el a szolgáltatást?**</span><span class="sxs-lookup"><span data-stu-id="f850d-121">**Q. What regions is the service available in?**</span></span>

<span data-ttu-id="f850d-122">Alkalmazásátjáró globális Azure minden területen érhető el.</span><span class="sxs-lookup"><span data-stu-id="f850d-122">Application Gateway is available in all regions of global Azure.</span></span> <span data-ttu-id="f850d-123">Rendszerben is elérhető [Azure Kína](https://www.azure.cn/) és [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)</span><span class="sxs-lookup"><span data-stu-id="f850d-123">It is also available in [Azure China](https://www.azure.cn/) and [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)</span></span>

<span data-ttu-id="f850d-124">**Q. Ez az előfizetésem dedikált telepítésének vagy azt megoszthatja ügyfelek?**</span><span class="sxs-lookup"><span data-stu-id="f850d-124">**Q. Is this a dedicated deployment for my subscription or is it shared across customers?**</span></span>

<span data-ttu-id="f850d-125">Alkalmazásátjáró egy dedikált központi telepítés a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="f850d-125">Application Gateway is a dedicated deployment in your virtual network.</span></span>

<span data-ttu-id="f850d-126">**Q. Van HTTP -> támogatott HTTPS átirányítása?**</span><span class="sxs-lookup"><span data-stu-id="f850d-126">**Q. Is HTTP->HTTPS redirection supported?**</span></span>

<span data-ttu-id="f850d-127">Átirányítás használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="f850d-127">Redirection is supported.</span></span> <span data-ttu-id="f850d-128">Látogasson el [Alkalmazásátjáró átirányítási áttekintése](application-gateway-redirect-overview.md) további.</span><span class="sxs-lookup"><span data-stu-id="f850d-128">Visit [Application Gateway redirect overview](application-gateway-redirect-overview.md) to learn more.</span></span>

<span data-ttu-id="f850d-129">**Q. Milyen sorrendben figyelői feldolgozása?**</span><span class="sxs-lookup"><span data-stu-id="f850d-129">**Q. In what order are listeners processed?**</span></span>

<span data-ttu-id="f850d-130">Figyelők dolgoznak fel a rendszer a sorrendben.</span><span class="sxs-lookup"><span data-stu-id="f850d-130">Listeners are processed in the order they are shown.</span></span> <span data-ttu-id="f850d-131">Ezért ha egy alapszintű figyelő egy bejövő kérelem megfelel feldolgozza azt először.</span><span class="sxs-lookup"><span data-stu-id="f850d-131">For that reason if a basic listener matches an incoming request it processes it first.</span></span>  <span data-ttu-id="f850d-132">Többhelyes figyelők egy alapszintű figyelő annak biztosítására, hogy a megfelelő háttér-forgalom érdekében előtt úgy kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="f850d-132">Multi-site listeners should be configured before a basic listener to ensure traffic is routed to the correct back-end.</span></span>

<span data-ttu-id="f850d-133">**Q. Hol található Application Gateway IP- és DNS?**</span><span class="sxs-lookup"><span data-stu-id="f850d-133">**Q. Where do I find Application Gateway’s IP and DNS?**</span></span>

<span data-ttu-id="f850d-134">A végpont egy nyilvános IP-címet használ, ha ez az információ található a nyilvános IP-cím erőforrás vagy a – áttekintés oldalra az Alkalmazásátjáró a portálon.</span><span class="sxs-lookup"><span data-stu-id="f850d-134">When using a public IP address as an endpoint, this information can be found on the public IP address resource or on the Overview page for the Application Gateway in the portal.</span></span> <span data-ttu-id="f850d-135">A belső IP-címek ez található Áttekintés lap.</span><span class="sxs-lookup"><span data-stu-id="f850d-135">For internal IP addresses, this can be found on the Overview page.</span></span>

<span data-ttu-id="f850d-136">**Q. Az IP- vagy DNS változik az Alkalmazásátjáró életciklusa alatt?**</span><span class="sxs-lookup"><span data-stu-id="f850d-136">**Q. Does the IP or DNS change over the lifetime of the Application Gateway?**</span></span>

<span data-ttu-id="f850d-137">A VIP módosíthatja, ha az átjáró leállt, és az ügyfél által indított.</span><span class="sxs-lookup"><span data-stu-id="f850d-137">The VIP can change if the gateway is stopped and started by the customer.</span></span> <span data-ttu-id="f850d-138">Alkalmazásátjáró társított DNS nem változtatja meg az átjáró életciklusa alatt.</span><span class="sxs-lookup"><span data-stu-id="f850d-138">The DNS associated with Application Gateway does not change over the lifecycle of the gateway.</span></span> <span data-ttu-id="f850d-139">Ezért ajánlott CNAME alias használja, és mutasson a az alkalmazás-átjáró a DNS-címét.</span><span class="sxs-lookup"><span data-stu-id="f850d-139">For this reason, it is recommended to use a CNAME alias and point it to the DNS address of the Application Gateway.</span></span>

<span data-ttu-id="f850d-140">**Q. Alkalmazásátjáró támogatja a statikus IP-címet?**</span><span class="sxs-lookup"><span data-stu-id="f850d-140">**Q. Does Application Gateway support static IP?**</span></span>

<span data-ttu-id="f850d-141">Nem, az Alkalmazásátjáró nem támogatja a statikus nyilvános IP-címek, de statikus belső IP-címek támogatja.</span><span class="sxs-lookup"><span data-stu-id="f850d-141">No, Application Gateway does not support static public IP addresses, but it does support static internal IPs.</span></span>

<span data-ttu-id="f850d-142">**Q. Alkalmazásátjáró támogatja a több nyilvános IP-cím az átjárón?**</span><span class="sxs-lookup"><span data-stu-id="f850d-142">**Q. Does Application Gateway support multiple public IPs on the gateway?**</span></span>

<span data-ttu-id="f850d-143">Csak egy nyilvános IP-cím egy Application Gateway esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="f850d-143">Only one public IP address is supported on an Application Gateway.</span></span>

<span data-ttu-id="f850d-144">**Q. Támogatja az Alkalmazásátjáró x-továbbított-a fejlécek?**</span><span class="sxs-lookup"><span data-stu-id="f850d-144">**Q. Does Application Gateway support x-forwarded-for headers?**</span></span>

<span data-ttu-id="f850d-145">Igen, az Alkalmazásátjáró x-továbbított – az x továbbított protokoll és x továbbított port fejlécek szúr be a kérést továbbítja a háttérkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="f850d-145">Yes, Application Gateway inserts x-forwarded-for, x-forwarded-proto, and x-forwarded-port headers into the request forwarded to the backend.</span></span> <span data-ttu-id="f850d-146">Az x-továbbított-a fejléc formátuma IP:Port vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="f850d-146">The format for x-forwarded-for header is a comma-separated list of IP:Port.</span></span> <span data-ttu-id="f850d-147">X továbbított protokoll érvényes értékei http vagy HTTPS protokollt.</span><span class="sxs-lookup"><span data-stu-id="f850d-147">The valid values for x-forwarded-proto are http or https.</span></span> <span data-ttu-id="f850d-148">X-továbbított-port, amelyen a kérelmet az Alkalmazásátjáró címen érhető portot határozza meg.</span><span class="sxs-lookup"><span data-stu-id="f850d-148">X-forwarded-port specifies the port at which the request reached at the Application Gateway.</span></span>

<span data-ttu-id="f850d-149">**Q. Mennyi időt vesz igénybe egy alkalmazás-átjáró üzembe helyezéséhez? Az Alkalmazásátjáró továbbra is működik, ha frissítése során?**</span><span class="sxs-lookup"><span data-stu-id="f850d-149">**Q. How long does it take to deploy an Application Gateway? Does my Application Gateway still work when being updated?**</span></span>

<span data-ttu-id="f850d-150">Új Alkalmazásátjáró telepítések esetén is igénybe vehet akár 20 percig kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f850d-150">New Application Gateway deployments can take up to 20 minutes to provision.</span></span> <span data-ttu-id="f850d-151">Mérete/példányszám módosításai nem zavaró, és ez alatt az idő az átjáró aktív marad.</span><span class="sxs-lookup"><span data-stu-id="f850d-151">Changes to instance size/count are not disruptive, and the gateway remains active during this time.</span></span>

## <a name="configuration"></a><span data-ttu-id="f850d-152">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="f850d-152">Configuration</span></span>

<span data-ttu-id="f850d-153">**Q. Alkalmazásátjáró mindig telepítve van a virtuális hálózaton?**</span><span class="sxs-lookup"><span data-stu-id="f850d-153">**Q. Is Application Gateway always deployed in a virtual network?**</span></span>

<span data-ttu-id="f850d-154">Igen, az Alkalmazásátjáró mindig a rendszer a virtuális hálózati alhálózat.</span><span class="sxs-lookup"><span data-stu-id="f850d-154">Yes, Application Gateway is always deployed in a virtual network subnet.</span></span> <span data-ttu-id="f850d-155">Ez az alhálózat csak tartalmazhat Alkalmazásátjárót.</span><span class="sxs-lookup"><span data-stu-id="f850d-155">This subnet can only contain Application Gateways.</span></span>

<span data-ttu-id="f850d-156">**Q. A virtuális hálózaton kívüli példányokhoz működik Alkalmazásátjáró?**</span><span class="sxs-lookup"><span data-stu-id="f850d-156">**Q. Can Application Gateway talk to instances outside its virtual network?**</span></span>

<span data-ttu-id="f850d-157">Alkalmazásátjáró működik, hogy a mindaddig, amíg nincs IP-kapcsolatot a virtuális hálózaton kívüli példányára.</span><span class="sxs-lookup"><span data-stu-id="f850d-157">Application Gateway can talk to instances outside of the virtual network that it is in as long as there is IP connectivity.</span></span> <span data-ttu-id="f850d-158">Ha a háttér címkészletet tagként belső IP-címek használatát tervezi, akkor van szükség [VNETBEN társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md) vagy [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="f850d-158">If you plan to use internal IPs as backend pool members, then it requires [VNET Peering](../virtual-network/virtual-network-peering-overview.md) or [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>

<span data-ttu-id="f850d-159">**Q. Központi telepítését az alkalmazás átjáróalhálózatot dolgozott?**</span><span class="sxs-lookup"><span data-stu-id="f850d-159">**Q. Can I deploy anything else in the Application Gateway subnet?**</span></span>

<span data-ttu-id="f850d-160">Nem, de telepítheti az alhálózat más alkalmazásátjárót.</span><span class="sxs-lookup"><span data-stu-id="f850d-160">No, but you can deploy other application gateways in the subnet.</span></span>

<span data-ttu-id="f850d-161">**Q. Hálózati biztonsági csoportok az Alkalmazásátjáró alhálózat támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="f850d-161">**Q. Are Network Security Groups supported on the Application Gateway subnet?**</span></span>

<span data-ttu-id="f850d-162">Hálózati biztonsági csoportok az Alkalmazásátjáró alhálózat, a következő korlátozásokkal támogatottak:</span><span class="sxs-lookup"><span data-stu-id="f850d-162">Network Security Groups are supported on the Application Gateway subnet with the following restrictions:</span></span>

* <span data-ttu-id="f850d-163">Kivételek kell elhelyezni, a bejövő forgalmat portokon 65503-65534 az háttér health megfelelő működéséhez.</span><span class="sxs-lookup"><span data-stu-id="f850d-163">Exceptions must be put in for incoming traffic on ports 65503-65534 for backend health to work correctly.</span></span>

* <span data-ttu-id="f850d-164">Nem blokkolható a kimenő internetkapcsolat.</span><span class="sxs-lookup"><span data-stu-id="f850d-164">Outbound internet connectivity can not be blocked.</span></span>

* <span data-ttu-id="f850d-165">Engedélyezni kell az AzureLoadBalancer címke forgalmát.</span><span class="sxs-lookup"><span data-stu-id="f850d-165">Traffic from the AzureLoadBalancer tag must be allowed.</span></span>

<span data-ttu-id="f850d-166">**Q. Az Alkalmazásátjáró korlátai Tudom növelni a működés felső korlátjának?**</span><span class="sxs-lookup"><span data-stu-id="f850d-166">**Q. What are the limits on Application Gateway? Can I increase these limits?**</span></span>

<span data-ttu-id="f850d-167">Látogasson el [alkalmazás átjáró korlátok](../azure-subscription-service-limits.md#application-gateway-limits) korlátokat megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="f850d-167">Visit [Application Gateway Limits](../azure-subscription-service-limits.md#application-gateway-limits) to view the limits.</span></span>

<span data-ttu-id="f850d-168">**Q. Használhatok Alkalmazásátjáró külső és belső forgalmát egyidejűleg?**</span><span class="sxs-lookup"><span data-stu-id="f850d-168">**Q. Can I use Application Gateway for both external and internal traffic simultaneously?**</span></span>

<span data-ttu-id="f850d-169">Alkalmazásátjáró Igen, támogatja az egy belső IP-cím és egy külső IP-cím az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="f850d-169">Yes, Application Gateway supports having one internal IP and one external IP per Application Gateway.</span></span>

<span data-ttu-id="f850d-170">**Q. Támogatott Vnetben társviszony-létesítés?**</span><span class="sxs-lookup"><span data-stu-id="f850d-170">**Q. Is VNet peering supported?**</span></span>

<span data-ttu-id="f850d-171">Igen, Vnetben társviszony-létesítés támogatott és hasznos a terheléselosztás forgalom más virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="f850d-171">Yes, VNet peering is supported and is beneficial for load balancing traffic in other virtual networks.</span></span>

<span data-ttu-id="f850d-172">**Q. I kommunikálhat a helyszíni kiszolgálók expressroute-on vagy VPN-alagutat csatlakozáskor?**</span><span class="sxs-lookup"><span data-stu-id="f850d-172">**Q. Can I talk to on-premises servers when they are connected by ExpressRoute or VPN tunnels?**</span></span>

<span data-ttu-id="f850d-173">Igen, mindaddig, amíg forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="f850d-173">Yes, as long as traffic is allowed.</span></span>

<span data-ttu-id="f850d-174">**Q. Rendelkezhet eltérő portokon számos alkalmazás szolgál egy háttérkészletéből?**</span><span class="sxs-lookup"><span data-stu-id="f850d-174">**Q. Can I have one backend pool serving many applications on different ports?**</span></span>

<span data-ttu-id="f850d-175">Micro service-architektúra esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="f850d-175">Micro service architecture is supported.</span></span> <span data-ttu-id="f850d-176">Kell több különböző portokon mintavétel konfigurálva http-beállítások.</span><span class="sxs-lookup"><span data-stu-id="f850d-176">You would need multiple http settings configured to probe on different ports.</span></span>

<span data-ttu-id="f850d-177">**Q. Támogatják egyéni mintavételt helyettesítő karakterekkel vagy reguláris kifejezéssel az érkezett válasz adatait?**</span><span class="sxs-lookup"><span data-stu-id="f850d-177">**Q. Do custom probes support wildcards/regex on response data?**</span></span>

<span data-ttu-id="f850d-178">Egyéni mintavételt nem támogatják a helyettesítő karakteres vagy regex érkezett válasz adatait a.</span><span class="sxs-lookup"><span data-stu-id="f850d-178">Custom probes do not support wildcard or regex on response data.</span></span> 

<span data-ttu-id="f850d-179">**Q. Szabályok feldolgozásának módja?**</span><span class="sxs-lookup"><span data-stu-id="f850d-179">**Q. How are rules processed?**</span></span>

<span data-ttu-id="f850d-180">Szabályok feldolgozása a sorrendben vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f850d-180">Rules are processed in the order they are configured.</span></span> <span data-ttu-id="f850d-181">Javasoljuk, hogy többhelyes szabályok konfigurálva vannak-e, mielőtt alapvető szabályok csökkenti annak esélyét, hogy forgalom annak biztosítására, hogy a megfelelő háttér, az alapszintű szabály megfelelő forgalmat a többhelyes szabály értékelt előtt port alapján.</span><span class="sxs-lookup"><span data-stu-id="f850d-181">It is recommended that multi-site rules are configured before basic rules to reduce the chance that traffic is routed to the inappropriate backend as the basic rule would match traffic based on port prior to the multi-site rule being evaluated.</span></span>

<span data-ttu-id="f850d-182">**Q. Szabályok feldolgozásának módja?**</span><span class="sxs-lookup"><span data-stu-id="f850d-182">**Q. How are rules processed?**</span></span>

<span data-ttu-id="f850d-183">Szabályok feldolgozása a létrehozásuk sorrendjében.</span><span class="sxs-lookup"><span data-stu-id="f850d-183">Rules are processed in the order they are created.</span></span> <span data-ttu-id="f850d-184">Javasoljuk, hogy a többhelyes szabályok előtt alapvető szabályok vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f850d-184">It is recommended that multi-site rules are configured before basic rules.</span></span> <span data-ttu-id="f850d-185">Többhelyes figyelői először konfigurálásával, ez a konfiguráció csökkentheti annak lehetőségét annak biztosítására, hogy a megfelelő háttér forgalom.</span><span class="sxs-lookup"><span data-stu-id="f850d-185">By configuring multi-site listeners first, this configuration reduces the chance that traffic is routed to the inappropriate backend.</span></span> <span data-ttu-id="f850d-186">Az alapvető szabály megfelelő előtt a többhelyes szabály értékelt port alapján forgalom útválasztási probléma fordulhatnak elő.</span><span class="sxs-lookup"><span data-stu-id="f850d-186">This routing issue can occur as the basic rule would match traffic based on port prior to the multi-site rule being evaluated.</span></span>

<span data-ttu-id="f850d-187">**Q. Mi a gazdagép mezőt az egyéni mintavételt jelölésére?**</span><span class="sxs-lookup"><span data-stu-id="f850d-187">**Q. What does the Host field for custom probes signify?**</span></span>

<span data-ttu-id="f850d-188">A gazdagép mező neve a mintavétel történő küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="f850d-188">Host field specifies the name to send the probe to.</span></span> <span data-ttu-id="f850d-189">Alkalmazandó csak akkor, ha több hely van beállítva az alkalmazás-átjárón, ellenkező esetben használja a "127.0.0.1".</span><span class="sxs-lookup"><span data-stu-id="f850d-189">Applicable only when multi-site is configured on Application Gateway, otherwise use '127.0.0.1'.</span></span> <span data-ttu-id="f850d-190">Ez az érték eltér a virtuális gép állomásnevét, és formátumú \<protokoll\>://\<állomás\>:\<port\>\<elérési\>.</span><span class="sxs-lookup"><span data-stu-id="f850d-190">This value is different from VM host name and is in format \<protocol\>://\<host\>:\<port\>\<path\>.</span></span>

<span data-ttu-id="f850d-191">**Q. Telepíthetek engedélyezett néhány forrás IP-címek Alkalmazásátjáró elérésére?**</span><span class="sxs-lookup"><span data-stu-id="f850d-191">**Q. Can I whitelist Application Gateway access to a few source IPs?**</span></span>

<span data-ttu-id="f850d-192">Ebben a forgatókönyvben végezhető Alkalmazásátjáró alhálózaton NSG-ket használ.</span><span class="sxs-lookup"><span data-stu-id="f850d-192">This scenario can be done using NSGs on Application Gateway subnet.</span></span> <span data-ttu-id="f850d-193">A következő korlátozásokat a prioritásuk szerinti a listában szereplő sorrendben az alhálózaton kell rendezni:</span><span class="sxs-lookup"><span data-stu-id="f850d-193">The following restrictions should be put on the subnet in the listed order of priority:</span></span>

* <span data-ttu-id="f850d-194">Engedélyezi a bejövő forgalom forrás IP-/ IP-címtartomány.</span><span class="sxs-lookup"><span data-stu-id="f850d-194">Allow incoming traffic from source IP/IP range.</span></span>

* <span data-ttu-id="f850d-195">Bejövő kérelmek forrásokból származó 65503-65534 portok engedélyezése [háttér állapotfigyelő kommunikációja](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="f850d-195">Allow incoming requests from all sources to ports 65503-65534 for [backend health communication](application-gateway-diagnostics.md).</span></span>

* <span data-ttu-id="f850d-196">Bejövő Azure Load Balancer mintavételt (AzureLoadBalancer címke) és a bejövő virtuális hálózati forgalmat (VirtualNetwork címke) engedélyezése a [NSG](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="f850d-196">Allow incoming Azure Load Balancer probes (AzureLoadBalancer tag) and inbound virtual network traffic (VirtualNetwork tag) on the [NSG](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="f850d-197">Megtagadási minden egyéb bejövő forgalom blokkolása minden szabály.</span><span class="sxs-lookup"><span data-stu-id="f850d-197">Block all other incoming traffic with a Deny all rule.</span></span>

* <span data-ttu-id="f850d-198">Kimenő forgalom az internethez, az összes cél engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f850d-198">Allow outbound traffic to the internet for all destinations.</span></span>

## <a name="performance"></a><span data-ttu-id="f850d-199">Teljesítmény</span><span class="sxs-lookup"><span data-stu-id="f850d-199">Performance</span></span>

<span data-ttu-id="f850d-200">**Q. Hogyan támogatja a Alkalmazásátjáró a magas rendelkezésre állás és méretezhetőség?**</span><span class="sxs-lookup"><span data-stu-id="f850d-200">**Q. How does Application Gateway support high availability and scalability?**</span></span>

<span data-ttu-id="f850d-201">Alkalmazásátjáró támogatja a magas rendelkezésre állás elérésére, ha két vagy több példányt.</span><span class="sxs-lookup"><span data-stu-id="f850d-201">Application Gateway supports high availability scenarios when you have two or more instances deployed.</span></span> <span data-ttu-id="f850d-202">Azure ezek a példányok elosztása az update és a tartalék tartományok győződjön meg arról, hogy minden példány nem egy időben.</span><span class="sxs-lookup"><span data-stu-id="f850d-202">Azure distributes these instances across update and fault domains to ensure that all instances do not fail at the same time.</span></span> <span data-ttu-id="f850d-203">Alkalmazásátjáró méretezhetőség támogatja több példányát ugyanahhoz az átjáróhoz a terhelés hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="f850d-203">Application Gateway supports scalability by adding multiple instances of the same gateway to share the load.</span></span>

<span data-ttu-id="f850d-204">**Q. Hogyan érhetők el vész-Helyreállítási forgatókönyv Alkalmazásátjáró adatközpontjaiban között?**</span><span class="sxs-lookup"><span data-stu-id="f850d-204">**Q. How do I achieve DR scenario across data centers with Application Gateway?**</span></span>

<span data-ttu-id="f850d-205">Ügyfelek használhatják a Traffic Manager forgalom szét több alkalmazás átjáró különböző adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="f850d-205">Customers can use Traffic Manager to distribute traffic across multiple Application Gateways in different datacenters.</span></span>

<span data-ttu-id="f850d-206">**Q. Automatikus skálázással támogatva van?**</span><span class="sxs-lookup"><span data-stu-id="f850d-206">**Q. Is auto scaling supported?**</span></span>

<span data-ttu-id="f850d-207">Nem, de Alkalmazásátjáró riasztást küldjön, amikor a küszöbérték elérésekor használt átviteli sebesség metrikát.</span><span class="sxs-lookup"><span data-stu-id="f850d-207">No, but Application Gateway has a throughput metric that can be used to alert you when a threshold is reached.</span></span> <span data-ttu-id="f850d-208">Manuális hozzáadása példányok vagy méretének módosítása nem indítja újra az átjárót, és nem befolyásolja a meglévő forgalom.</span><span class="sxs-lookup"><span data-stu-id="f850d-208">Manually adding instances or changing size does not restart the gateway and does not impact existing traffic.</span></span>

<span data-ttu-id="f850d-209">**Q. Fel/le OK állásidő, nem manuális méretezési?**</span><span class="sxs-lookup"><span data-stu-id="f850d-209">**Q. Does manual scale up/down cause downtime?**</span></span>

<span data-ttu-id="f850d-210">Állásidő nélkül, a frissítési tartományok és a tartalék tartományok között elosztott példányok.</span><span class="sxs-lookup"><span data-stu-id="f850d-210">There is no downtime, instances are distributed across upgrade domains and fault domains.</span></span>

<span data-ttu-id="f850d-211">**Q. Módosítható példányméretének a közepes vagy nagyméretű megszakítása nélkül?**</span><span class="sxs-lookup"><span data-stu-id="f850d-211">**Q. Can I change instance size from medium to large without disruption?**</span></span>

<span data-ttu-id="f850d-212">Igen, Azure példányok elosztása frissítés és a tartalék tartományok győződjön meg arról, hogy minden példány nem egy időben.</span><span class="sxs-lookup"><span data-stu-id="f850d-212">Yes, Azure distributes instances across update and fault domains to ensure that all instances do not fail at the same time.</span></span> <span data-ttu-id="f850d-213">Alkalmazásátjáró támogatja, több példányát ugyanahhoz az átjáróhoz a terhelés hozzáadásával méretezés.</span><span class="sxs-lookup"><span data-stu-id="f850d-213">Application Gateway supports scaling by adding multiple instances of the same gateway to share the load.</span></span>

## <a name="ssl-configuration"></a><span data-ttu-id="f850d-214">SSL-beállítása</span><span class="sxs-lookup"><span data-stu-id="f850d-214">SSL Configuration</span></span>

<span data-ttu-id="f850d-215">**Q. Milyen tanúsítványok Alkalmazásátjáró támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="f850d-215">**Q. What certificates are supported on Application Gateway?**</span></span>

<span data-ttu-id="f850d-216">Önaláírt tanúsítványok, a CA-tanúsítványok, és a helyettesítő tanúsítványok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="f850d-216">Self signed certs, CA certs, and wild-card certs are supported.</span></span> <span data-ttu-id="f850d-217">EV tanúsítványok használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="f850d-217">EV certs are not supported.</span></span>

<span data-ttu-id="f850d-218">**Q. Mik az aktuális alkalmazás-átjáró által támogatott titkosító csomagok?**</span><span class="sxs-lookup"><span data-stu-id="f850d-218">**Q. What are the current cipher suites supported by Application Gateway?**</span></span>

<span data-ttu-id="f850d-219">Az aktuális alkalmazás-átjáró által támogatott titkosító csomagok a következők:</span><span class="sxs-lookup"><span data-stu-id="f850d-219">The following are the current cipher suites supported by application gateway.</span></span> <span data-ttu-id="f850d-220">Látogasson el: [SSL konfigurálása házirend verziója és az Application Gateway titkosító csomagok](application-gateway-configure-ssl-policy-powershell.md) megtudhatja, hogyan szabhatja testre az SSL-beállítások.</span><span class="sxs-lookup"><span data-stu-id="f850d-220">Visit: [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) to learn how to customize SSL options.</span></span>

- <span data-ttu-id="f850d-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="f850d-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="f850d-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="f850d-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="f850d-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="f850d-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="f850d-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="f850d-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="f850d-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="f850d-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="f850d-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="f850d-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="f850d-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="f850d-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="f850d-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="f850d-233">TLS_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-233">TLS_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="f850d-234">TLS_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-234">TLS_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="f850d-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="f850d-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="f850d-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="f850d-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="f850d-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="f850d-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="f850d-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="f850d-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="f850d-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="f850d-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="f850d-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="f850d-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="f850d-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span></span>
- <span data-ttu-id="f850d-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="f850d-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span></span>

<span data-ttu-id="f850d-247">**Q. Alkalmazásátjáró is támogatja a háttér-forgalom újbóli titkosítása?**</span><span class="sxs-lookup"><span data-stu-id="f850d-247">**Q. Does Application Gateway also support re-encryption of traffic to the backend?**</span></span>

<span data-ttu-id="f850d-248">Igen, a Application Gateway SSL kiszervezési, és a végpontok közötti SSL, amely újból titkosítja a forgalmat a háttérrendszer támogatja.</span><span class="sxs-lookup"><span data-stu-id="f850d-248">Yes, Application Gateway supports SSL offload, and end to end SSL, which re-encrypts the traffic to the backend.</span></span>

<span data-ttu-id="f850d-249">**Q. Konfigurálhatja a SSL házirendet, amellyel szabályozhatja az SSL protokoll verziója?**</span><span class="sxs-lookup"><span data-stu-id="f850d-249">**Q. Can I configure SSL policy to control SSL Protocol versions?**</span></span>

<span data-ttu-id="f850d-250">Igen, Alkalmazásátjáró TLS1.0 TLS1.1 és TLS1.2 megtagadni konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="f850d-250">Yes, you can configure Application Gateway to deny TLS1.0, TLS1.1, and TLS1.2.</span></span> <span data-ttu-id="f850d-251">Az SSL 2.0 és 3.0 vannak már le van tiltva alapértelmezés szerint, és amelyek nem konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="f850d-251">SSL 2.0 and 3.0 are already disabled by default and are not configurable.</span></span>

<span data-ttu-id="f850d-252">**Q. Titkosítási csomagok és a házirendek sorrendjének is konfigurálni?**</span><span class="sxs-lookup"><span data-stu-id="f850d-252">**Q. Can I configure cipher suites and policy order?**</span></span>

<span data-ttu-id="f850d-253">Igen, [titkosító csomagok konfigurációjának](application-gateway-ssl-policy-overview.md) esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="f850d-253">Yes, [configuration of cipher suites](application-gateway-ssl-policy-overview.md) is supported.</span></span> <span data-ttu-id="f850d-254">Egyéni házirend meghatározása esetén a következő titkosító csomagok legalább egyikét engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="f850d-254">When defining a custom policy, at least one of the following cipher suites must be enabled.</span></span> <span data-ttu-id="f850d-255">Alkalmazásátjáró háttérbeli felügyeleti az SHA256 titkosítást használ.</span><span class="sxs-lookup"><span data-stu-id="f850d-255">Application gateway uses SHA256 to for backend management.</span></span>

* <span data-ttu-id="f850d-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span></span> 
* <span data-ttu-id="f850d-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
* <span data-ttu-id="f850d-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="f850d-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="f850d-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
* <span data-ttu-id="f850d-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="f850d-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>

<span data-ttu-id="f850d-262">**Q. Hány SSL-tanúsítványok támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="f850d-262">**Q. How many SSL certificates are supported?**</span></span>

<span data-ttu-id="f850d-263">Legfeljebb 20 SSL támogatottak.</span><span class="sxs-lookup"><span data-stu-id="f850d-263">Up to 20 SSL certificates are supported.</span></span>

<span data-ttu-id="f850d-264">**Q. A háttérrendszer újbóli titkosítása hány tanúsítványhitelesítést biztosítsanak támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="f850d-264">**Q. How many authentication certificates for backend re-encryption are supported?**</span></span>

<span data-ttu-id="f850d-265">Legfeljebb 10 hitelesítési tanúsítványok az alapértelmezett érték 5 használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="f850d-265">Up to 10 authentication certificates are supported with a default of 5.</span></span>

<span data-ttu-id="f850d-266">**Q. Nem Alkalmazásátjáró integrálása az Azure Key Vault natív módon?**</span><span class="sxs-lookup"><span data-stu-id="f850d-266">**Q. Does Application Gateway integrate with Azure Key Vault natively?**</span></span>

<span data-ttu-id="f850d-267">Nem, nem integrálva van az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="f850d-267">No, it is not integrated with Azure Key Vault.</span></span>

## <a name="web-application-firewall-waf-configuration"></a><span data-ttu-id="f850d-268">Webes alkalmazás tűzfalat (WAF) konfigurációja</span><span class="sxs-lookup"><span data-stu-id="f850d-268">Web Application Firewall (WAF) Configuration</span></span>

<span data-ttu-id="f850d-269">**Q. Nem a WAF SKU kínálnak a Standard Termékváltozat elérhető összes szolgáltatások?**</span><span class="sxs-lookup"><span data-stu-id="f850d-269">**Q. Does the WAF SKU offer all the features available with the Standard SKU?**</span></span>

<span data-ttu-id="f850d-270">Igen, WAF összes funkcióját támogatja a Standard Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="f850d-270">Yes, WAF supports all the features in the Standard SKU.</span></span>

<span data-ttu-id="f850d-271">**Q. Mi az Application Gateway CRS verzió támogatja?**</span><span class="sxs-lookup"><span data-stu-id="f850d-271">**Q. What is the CRS version Application Gateway supports?**</span></span>

<span data-ttu-id="f850d-272">Alkalmazásátjáró támogatja CRS [program 2.2.9-es](application-gateway-crs-rulegroups-rules.md#owasp229) és CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span><span class="sxs-lookup"><span data-stu-id="f850d-272">Application Gateway supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span></span>

<span data-ttu-id="f850d-273">**Q. Hogyan figyelhetek WAF?**</span><span class="sxs-lookup"><span data-stu-id="f850d-273">**Q. How do I monitor WAF?**</span></span>

<span data-ttu-id="f850d-274">WAF keresztül diagnosztikai naplózás felügyelet alatt, további információt a diagnosztikai naplózás helyen találhatók [diagnosztikai naplózás és az Alkalmazásátjáró metrikák](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="f850d-274">WAF is monitored through diagnostic logging, more information on diagnostic logging can be found at [Diagnostics Logging and Metrics for Application Gateway](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="f850d-275">**Q. Blokkolja az észlelési mód forgalom?**</span><span class="sxs-lookup"><span data-stu-id="f850d-275">**Q. Does detection mode block traffic?**</span></span>

<span data-ttu-id="f850d-276">Nem, a észlelési mód csak naplózza a forgalmat, amely egy WAF szabály elindul.</span><span class="sxs-lookup"><span data-stu-id="f850d-276">No, detection mode only logs traffic, which triggered a WAF rule.</span></span>

<span data-ttu-id="f850d-277">**Q. Hogyan testre szabhatja a WAF szabályokat?**</span><span class="sxs-lookup"><span data-stu-id="f850d-277">**Q. How do I customize WAF rules?**</span></span>

<span data-ttu-id="f850d-278">Igen, testre szabható testre szabhatók találhat további tájékoztatást a-e WAF szabályok [testreszabása WAF csoportok és szabályok](application-gateway-customize-waf-rules-portal.md)</span><span class="sxs-lookup"><span data-stu-id="f850d-278">Yes, WAF rules are customizable, for more information on how to customize them visit [Customize WAF rule groups and rules](application-gateway-customize-waf-rules-portal.md)</span></span>

<span data-ttu-id="f850d-279">**Q. Milyen szabályok jelenleg érhetők el?**</span><span class="sxs-lookup"><span data-stu-id="f850d-279">**Q. What rules are currently available?**</span></span>

<span data-ttu-id="f850d-280">WAF jelenleg CRS [program 2.2.9-es](application-gateway-crs-rulegroups-rules.md#owasp229) és [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), szemben az első 10 biztonsági réseit által a nyitott webes alkalmazás biztonsági Project (OWASP) többségét eredeti biztonsági nyújt található itt [ OWASP első 10 biztonsági réseket](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span><span class="sxs-lookup"><span data-stu-id="f850d-280">WAF currently supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), which provide baseline security against most of the top 10 vulnerabilities identified by the Open Web Application Security Project (OWASP) found here [OWASP top 10 Vulnerabilities](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span></span>

* <span data-ttu-id="f850d-281">SQL-injektálás elleni védelem</span><span class="sxs-lookup"><span data-stu-id="f850d-281">SQL injection protection</span></span>

* <span data-ttu-id="f850d-282">Webhelyek közötti, parancsprogramot alkalmazó támadások elleni védelem</span><span class="sxs-lookup"><span data-stu-id="f850d-282">Cross site scripting protection</span></span>

* <span data-ttu-id="f850d-283">Gyakori webes támadások (például parancsinjektálás, HTTP-kéréscsempészet, HTTP-válaszfelosztás és távolifájl-beszúrásos támadás) elleni védelem</span><span class="sxs-lookup"><span data-stu-id="f850d-283">Common Web Attacks Protection such as command injection, HTTP request smuggling, HTTP response splitting, and remote file inclusion attack</span></span>

* <span data-ttu-id="f850d-284">HTTP protokoll megsértése elleni védelem</span><span class="sxs-lookup"><span data-stu-id="f850d-284">Protection against HTTP protocol violations</span></span>

* <span data-ttu-id="f850d-285">HTTP protokollanomáliák (például hiányzó gazdagép-felhasználói ügynök és Accept (Elfogadás) fejlécek) elleni védelem</span><span class="sxs-lookup"><span data-stu-id="f850d-285">Protection against HTTP protocol anomalies such as missing host user-agent and accept headers</span></span>

* <span data-ttu-id="f850d-286">Robotprogramok, webbejárók és képolvasók elleni védelem</span><span class="sxs-lookup"><span data-stu-id="f850d-286">Prevention against bots, crawlers, and scanners</span></span>

* <span data-ttu-id="f850d-287">A gyakori alkalmazás konfigurációs hibák (Ez azt jelenti, hogy Apache, az IIS, stb.) észlelése</span><span class="sxs-lookup"><span data-stu-id="f850d-287">Detection of common application misconfigurations (that is, Apache, IIS, etc.)</span></span>

<span data-ttu-id="f850d-288">**Q. WAF is támogatja a DDoS megelőzési?**</span><span class="sxs-lookup"><span data-stu-id="f850d-288">**Q. Does WAF also support DDoS prevention?**</span></span>

<span data-ttu-id="f850d-289">WAF nem, nem biztosít DDoS megelőzése.</span><span class="sxs-lookup"><span data-stu-id="f850d-289">No, WAF does not provide DDoS prevention.</span></span>

## <a name="diagnostics-and-logging"></a><span data-ttu-id="f850d-290">Diagnosztika és naplózás</span><span class="sxs-lookup"><span data-stu-id="f850d-290">Diagnostics and Logging</span></span>

<span data-ttu-id="f850d-291">**Q. Milyen típusú naplók az Alkalmazásátjáró érhetők el?**</span><span class="sxs-lookup"><span data-stu-id="f850d-291">**Q. What types of logs are available with Application Gateway?**</span></span>

<span data-ttu-id="f850d-292">Nincsenek elérhető az Alkalmazásátjáró három naplókat.</span><span class="sxs-lookup"><span data-stu-id="f850d-292">There are three logs available for Application Gateway.</span></span> <span data-ttu-id="f850d-293">Ezek a naplók és más diagnosztikai képességek a további tudnivalókért keresse fel [háttér állapot, a diagnosztikai naplók és a metrikák az Alkalmazásátjáró](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="f850d-293">For more information on these logs and other diagnostic capabilities, visit [Backend health, diagnostics logs, and metrics for Application Gateway](application-gateway-diagnostics.md).</span></span>

- <span data-ttu-id="f850d-294">**ApplicationGatewayAccessLog** -a hozzáférési napló minden egyes kérelem elküldve az Alkalmazásátjáró előtér tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f850d-294">**ApplicationGatewayAccessLog** - The access log contains each request submitted to the Application Gateway frontend.</span></span> <span data-ttu-id="f850d-295">Az adatok a hívó IP, kért, URL-cím válasz késés, bejövő és kimenő adatforgalma visszatérési kód, bájt. Hozzáférési napló gyűjtése 300 másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="f850d-295">The data includes the caller's IP, URL requested, response latency, return code, bytes in and out. Access log is collected every 300 seconds.</span></span> <span data-ttu-id="f850d-296">Ez a napló Application Gateway-példányonként egy bejegyzést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="f850d-296">This log contains one record per instance of Application Gateway.</span></span>
- <span data-ttu-id="f850d-297">**ApplicationGatewayPerformanceLog** – a teljesítmény naplóban rögzíti alapon / példány teljes kérelem kiszolgálása, beleértve az átviteli sebesség bájtban teljesítményadatok, kérelmek teljes száma a kiszolgált egy megfelelő és nem megfelelő háttér-, sikertelen kérelmek száma a példányok száma.</span><span class="sxs-lookup"><span data-stu-id="f850d-297">**ApplicationGatewayPerformanceLog** - The performance log captures performance information on per instance basis including total request served, throughput in bytes, total requests served, failed request count, healthy and unhealthy back-end instance count.</span></span>
- <span data-ttu-id="f850d-298">**ApplicationGatewayFirewallLog** -a tűzfal a napló tartalmazza, amelyeket a rendszer a webalkalmazási tűzfal a konfigurált Alkalmazásátjáró észlelési vagy megelőzési módban kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="f850d-298">**ApplicationGatewayFirewallLog** - The firewall log contains requests that are logged through either detection or prevention mode of an application gateway that is configured with web application firewall.</span></span>

<span data-ttu-id="f850d-299">**Q. Hogyan állapítható meg, ha a háttérkiszolgáló készlettagra megfelelő?**</span><span class="sxs-lookup"><span data-stu-id="f850d-299">**Q. How do I know if my backend pool members are healthy?**</span></span>

<span data-ttu-id="f850d-300">A PowerShell-parancsmag `Get-AzureRmApplicationGatewayBackendHealth` vagy ellenőrizze a portálon keresztül állapotfigyelő ellátogatva [Alkalmazásdiagnosztika átjáró](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="f850d-300">You can use the PowerShell cmdlet `Get-AzureRmApplicationGatewayBackendHealth` or verify health through the portal by visiting [Application Gateway Diagnostics](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="f850d-301">**Q. Mi az a diagnosztikai naplókat a megőrzési házirend?**</span><span class="sxs-lookup"><span data-stu-id="f850d-301">**Q. What is the retention policy on the diagnostics logs?**</span></span>

<span data-ttu-id="f850d-302">Diagnosztikai naplók folyamata az ügyfelek tárfiókba, és ügyfelek állíthatja be az adatmegőrzési, a beállítás alapján.</span><span class="sxs-lookup"><span data-stu-id="f850d-302">Diagnostic logs flow to the customers storage account and customers can set the retention policy based on their preference.</span></span> <span data-ttu-id="f850d-303">Diagnosztikai naplók is lehet küldeni az Eseményközpont vagy Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="f850d-303">Diagnostic logs can also be sent to an Event Hub or Log Analytics.</span></span> <span data-ttu-id="f850d-304">Látogasson el [átjáró Alkalmazásdiagnosztika](application-gateway-diagnostics.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="f850d-304">Visit [Application Gateway Diagnostics](application-gateway-diagnostics.md) for more details.</span></span>

<span data-ttu-id="f850d-305">**Q. Hogyan szerezhetek naplók az Alkalmazásátjáró?**</span><span class="sxs-lookup"><span data-stu-id="f850d-305">**Q. How do I get audit logs for Application Gateway?**</span></span>

<span data-ttu-id="f850d-306">Az Alkalmazásátjáró naplók érhetők el.</span><span class="sxs-lookup"><span data-stu-id="f850d-306">Audit logs are available for Application Gateway.</span></span> <span data-ttu-id="f850d-307">Kattintson a portál **tevékenységnapló** a menü paneljén az Alkalmazásátjáró a napló elérésére.</span><span class="sxs-lookup"><span data-stu-id="f850d-307">In the portal, click **Activity Log** in the menu blade of an Application Gateway to access the audit log.</span></span> 

<span data-ttu-id="f850d-308">**Q. Beállíthatja a Alkalmazásátjáró riasztások?**</span><span class="sxs-lookup"><span data-stu-id="f850d-308">**Q. Can I set alerts with Application Gateway?**</span></span>

<span data-ttu-id="f850d-309">Igen, Alkalmazásátjáró támogatja a riasztások, értesítések metrikák ki vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f850d-309">Yes, Application Gateway does support alerts, alerts are configured off metrics.</span></span>  <span data-ttu-id="f850d-310">Alkalmazásátjáró "átviteli", amely konfigurálható egy metrika van riasztást.</span><span class="sxs-lookup"><span data-stu-id="f850d-310">Application Gateway currently has a metric of "throughput", which can be configured to alert.</span></span> <span data-ttu-id="f850d-311">Riasztások kapcsolatos további információkért látogasson el a [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="f850d-311">To learn more about alerts, visit [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="f850d-312">**Q. Háttér állapotfigyelő adja vissza állapota ismeretlen, mi okozza ezt az állapotot?**</span><span class="sxs-lookup"><span data-stu-id="f850d-312">**Q. Backend health returns unknown status, what could be causing this status?**</span></span>

<span data-ttu-id="f850d-313">A leggyakoribb oka a háttérkiszolgálón a hozzáférést egy NSG-t vagy egyéni DNS-megjelenítését blokkolják.</span><span class="sxs-lookup"><span data-stu-id="f850d-313">The most common reason is access to the backend is being blocked by an NSG or custom DNS.</span></span> <span data-ttu-id="f850d-314">Látogasson el [háttér állapot, a diagnosztikai naplózás és a metrikák az Alkalmazásátjáró](application-gateway-diagnostics.md) további.</span><span class="sxs-lookup"><span data-stu-id="f850d-314">Visit [Backend health, diagnostics logging, and metrics for Application Gateway](application-gateway-diagnostics.md) to learn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f850d-315">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f850d-315">Next Steps</span></span>

<span data-ttu-id="f850d-316">További információt az Alkalmazásátjáró látogasson el [Alkalmazásátjáró bemutatása](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="f850d-316">To learn more about Application Gateway visit [Introduction to Application Gateway](application-gateway-introduction.md).</span></span>
