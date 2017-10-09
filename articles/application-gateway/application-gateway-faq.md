---
title: "Gyakori kérdések az Azure Application Gateway aaaFrequently |} Microsoft Docs"
description: "Ezen a lapon választ ad toofrequently Azure Application Gateway kapcsolatos kérdések"
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
ms.openlocfilehash: b2df3a82a71a3264d3d34d317d08e4b4f72c6e3e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-for-application-gateway"></a><span data-ttu-id="81651-103">Az Alkalmazásátjáró gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="81651-103">Frequently asked questions for Application Gateway</span></span>

## <a name="general"></a><span data-ttu-id="81651-104">Általános kérdések</span><span class="sxs-lookup"><span data-stu-id="81651-104">General</span></span>

<span data-ttu-id="81651-105">**Q. Mi az Application Gateway?**</span><span class="sxs-lookup"><span data-stu-id="81651-105">**Q. What is Application Gateway?**</span></span>

<span data-ttu-id="81651-106">Azure Application Gateway egy alkalmazás kézbesítési vezérlő LÉPETT szolgáltatásként, az alkalmazások képességei terheléselosztási különböző réteg 7 kínál.</span><span class="sxs-lookup"><span data-stu-id="81651-106">Azure Application Gateway is an Application Delivery Controller (ADC) as a service, offering various layer 7 load balancing capabilities for your applications.</span></span> <span data-ttu-id="81651-107">Magas rendelkezésre állású és méretezhető szolgáltatást, amely teljes mértékben kezeli az Azure biztosít.</span><span class="sxs-lookup"><span data-stu-id="81651-107">It offers highly available and scalable service, which is fully managed by Azure.</span></span>

<span data-ttu-id="81651-108">**Q. Milyen funkciókat támogatja az Alkalmazásátjáró?**</span><span class="sxs-lookup"><span data-stu-id="81651-108">**Q. What features does Application Gateway support?**</span></span>

<span data-ttu-id="81651-109">Alkalmazásátjáró támogatja SSL kiszervezésével és a záró tooend SSL, webalkalmazási tűzfal, munkamenet cookie-alapú kapcsolat, URL-cím elérési út-alapú útválasztási, több hely üzemeltetéséhez és mások számára.</span><span class="sxs-lookup"><span data-stu-id="81651-109">Application Gateway supports SSL offloading and end tooend SSL, Web Application Firewall, cookie-based session affinity, url path-based routing, multi site hosting, and others.</span></span> <span data-ttu-id="81651-110">Támogatott szolgáltatások teljes listájának megtekintéséhez keresse fel a [bemutatása tooApplication átjáró](application-gateway-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="81651-110">For a full list of supported features, visit [Introduction tooApplication Gateway](application-gateway-introduction.md)</span></span>

<span data-ttu-id="81651-111">**Q. Mi az Application Gateway és az Azure Load Balancer hello különbségének?**</span><span class="sxs-lookup"><span data-stu-id="81651-111">**Q. What is hello difference between Application Gateway and Azure Load Balancer?**</span></span>

<span data-ttu-id="81651-112">Alkalmazásátjáró 7 réteg terheléselosztó, amely azt jelenti, hogy működik együtt a csak internetes forgalmat (HTTP/HTTPS/WebSocket).</span><span class="sxs-lookup"><span data-stu-id="81651-112">Application Gateway is a layer 7 load balancer, which means it works with web traffic only (HTTP/HTTPS/WebSocket).</span></span> <span data-ttu-id="81651-113">Például az SSL-lezárást, a munkamenet cookie-alapú kapcsolat és a ciklikus multiplexelés képességek terheléselosztási forgalom támogatja.</span><span class="sxs-lookup"><span data-stu-id="81651-113">It supports capabilities such as SSL termination, cookie-based session affinity, and round robin for load balancing traffic.</span></span> <span data-ttu-id="81651-114">Terheléselosztó, kiegyensúlyozza forgalom rétegben 4 (TCP/UDP).</span><span class="sxs-lookup"><span data-stu-id="81651-114">Load Balancer, load balances traffic at layer 4 (TCP/UDP).</span></span>

<span data-ttu-id="81651-115">**Q. Milyen protokollokat támogatja az Alkalmazásátjáró?**</span><span class="sxs-lookup"><span data-stu-id="81651-115">**Q. What protocols does Application Gateway support?**</span></span>

<span data-ttu-id="81651-116">Alkalmazás-átjáró támogatja a HTTP, HTTPS és WebSocket.</span><span class="sxs-lookup"><span data-stu-id="81651-116">Application Gateway supports HTTP, HTTPS, and WebSocket.</span></span>

<span data-ttu-id="81651-117">**Q. Által támogatott ma háttérkészlet részeként?**</span><span class="sxs-lookup"><span data-stu-id="81651-117">**Q. What resources are supported today as part of backend pool?**</span></span>

<span data-ttu-id="81651-118">Háttérkészlet állhat hálózati adapter virtuálisgép-méretezési csoportok, nyilvános IP-címek, belső IP-címek, teljes tartománynév (FQDN) neve, és több-bérlős vissza-végpontok, például az Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="81651-118">Backend pools can be composed of NICs, virtual machine scale sets, public IPs, internal IPs, fully qualified domain names (FQDN), and multi-tenant back-ends like Azure Web Apps.</span></span> <span data-ttu-id="81651-119">Az Alkalmazásátjáró háttér címkészletet tagjai nem tooan rendelkezésre állási csoport társítva.</span><span class="sxs-lookup"><span data-stu-id="81651-119">Application Gateway backend pool members are not tied tooan availability set.</span></span> <span data-ttu-id="81651-120">Háttér-készletek tagjai között, fürtök és adatközpontok, vagy lehet Azure-on kívüli mindaddig, amíg az IP-kapcsolattal rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="81651-120">Members of backend pools can be across clusters, data centers, or outside of Azure as long as they have IP connectivity.</span></span>

<span data-ttu-id="81651-121">**Q. Milyen régiók érhető el hello szolgáltatást?**</span><span class="sxs-lookup"><span data-stu-id="81651-121">**Q. What regions is hello service available in?**</span></span>

<span data-ttu-id="81651-122">Alkalmazásátjáró globális Azure minden területen érhető el.</span><span class="sxs-lookup"><span data-stu-id="81651-122">Application Gateway is available in all regions of global Azure.</span></span> <span data-ttu-id="81651-123">Rendszerben is elérhető [Azure Kína](https://www.azure.cn/) és [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)</span><span class="sxs-lookup"><span data-stu-id="81651-123">It is also available in [Azure China](https://www.azure.cn/) and [Azure Government](https://azure.microsoft.com/en-us/overview/clouds/government/)</span></span>

<span data-ttu-id="81651-124">**Q. Ez az előfizetésem dedikált telepítésének vagy azt megoszthatja ügyfelek?**</span><span class="sxs-lookup"><span data-stu-id="81651-124">**Q. Is this a dedicated deployment for my subscription or is it shared across customers?**</span></span>

<span data-ttu-id="81651-125">Alkalmazásátjáró egy dedikált központi telepítés a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="81651-125">Application Gateway is a dedicated deployment in your virtual network.</span></span>

<span data-ttu-id="81651-126">**Q. Van HTTP -> támogatott HTTPS átirányítása?**</span><span class="sxs-lookup"><span data-stu-id="81651-126">**Q. Is HTTP->HTTPS redirection supported?**</span></span>

<span data-ttu-id="81651-127">Átirányítás használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="81651-127">Redirection is supported.</span></span> <span data-ttu-id="81651-128">Látogasson el [Alkalmazásátjáró átirányítási áttekintése](application-gateway-redirect-overview.md) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="81651-128">Visit [Application Gateway redirect overview](application-gateway-redirect-overview.md) toolearn more.</span></span>

<span data-ttu-id="81651-129">**Q. Milyen sorrendben figyelői feldolgozása?**</span><span class="sxs-lookup"><span data-stu-id="81651-129">**Q. In what order are listeners processed?**</span></span>

<span data-ttu-id="81651-130">Figyelők feldolgozása hello ahhoz, azok láthatók.</span><span class="sxs-lookup"><span data-stu-id="81651-130">Listeners are processed in hello order they are shown.</span></span> <span data-ttu-id="81651-131">Ezért ha egy alapszintű figyelő egy bejövő kérelem megfelel feldolgozza azt először.</span><span class="sxs-lookup"><span data-stu-id="81651-131">For that reason if a basic listener matches an incoming request it processes it first.</span></span>  <span data-ttu-id="81651-132">Többhelyes figyelők úgy kell konfigurálni, mielőtt egy alapszintű figyelő tooensure forgalom irányított toohello megfelelő háttér.</span><span class="sxs-lookup"><span data-stu-id="81651-132">Multi-site listeners should be configured before a basic listener tooensure traffic is routed toohello correct back-end.</span></span>

<span data-ttu-id="81651-133">**Q. Hol található Application Gateway IP- és DNS?**</span><span class="sxs-lookup"><span data-stu-id="81651-133">**Q. Where do I find Application Gateway’s IP and DNS?**</span></span>

<span data-ttu-id="81651-134">A végpont egy nyilvános IP-címet használ, ha ezek az információk található hello nyilvános IP-cím erőforrás vagy hello – áttekintés oldalra hello Alkalmazásátjáró hello portálon.</span><span class="sxs-lookup"><span data-stu-id="81651-134">When using a public IP address as an endpoint, this information can be found on hello public IP address resource or on hello Overview page for hello Application Gateway in hello portal.</span></span> <span data-ttu-id="81651-135">A belső IP-címek ez található hello áttekintése lapon.</span><span class="sxs-lookup"><span data-stu-id="81651-135">For internal IP addresses, this can be found on hello Overview page.</span></span>

<span data-ttu-id="81651-136">**Q. Hello IP- vagy DNS változik az Alkalmazásátjáró hello hello élettartamuk során?**</span><span class="sxs-lookup"><span data-stu-id="81651-136">**Q. Does hello IP or DNS change over hello lifetime of hello Application Gateway?**</span></span>

<span data-ttu-id="81651-137">hello VIP módosíthatja, ha hello átjáró leáll, majd hello ügyfél által indított.</span><span class="sxs-lookup"><span data-stu-id="81651-137">hello VIP can change if hello gateway is stopped and started by hello customer.</span></span> <span data-ttu-id="81651-138">hello Alkalmazásátjáró társított DNS hello átjáró hello életciklusa során nem változik.</span><span class="sxs-lookup"><span data-stu-id="81651-138">hello DNS associated with Application Gateway does not change over hello lifecycle of hello gateway.</span></span> <span data-ttu-id="81651-139">Ezért az ajánlott toouse CNAME alias, majd mutasson az Alkalmazásátjáró hello toohello DNS-címét.</span><span class="sxs-lookup"><span data-stu-id="81651-139">For this reason, it is recommended toouse a CNAME alias and point it toohello DNS address of hello Application Gateway.</span></span>

<span data-ttu-id="81651-140">**Q. Alkalmazásátjáró támogatja a statikus IP-címet?**</span><span class="sxs-lookup"><span data-stu-id="81651-140">**Q. Does Application Gateway support static IP?**</span></span>

<span data-ttu-id="81651-141">Nem, az Alkalmazásátjáró nem támogatja a statikus nyilvános IP-címek, de statikus belső IP-címek támogatja.</span><span class="sxs-lookup"><span data-stu-id="81651-141">No, Application Gateway does not support static public IP addresses, but it does support static internal IPs.</span></span>

<span data-ttu-id="81651-142">**Q. Alkalmazásátjáró támogatja a több nyilvános IP-cím hello átjárón?**</span><span class="sxs-lookup"><span data-stu-id="81651-142">**Q. Does Application Gateway support multiple public IPs on hello gateway?**</span></span>

<span data-ttu-id="81651-143">Csak egy nyilvános IP-cím egy Application Gateway esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="81651-143">Only one public IP address is supported on an Application Gateway.</span></span>

<span data-ttu-id="81651-144">**Q. Támogatja az Alkalmazásátjáró x-továbbított-a fejlécek?**</span><span class="sxs-lookup"><span data-stu-id="81651-144">**Q. Does Application Gateway support x-forwarded-for headers?**</span></span>

<span data-ttu-id="81651-145">Igen, Alkalmazásátjáró beszúrása x-továbbított- esetén az x továbbított protokoll és az x továbbított port fejlécek hello kérelem a továbbított toohello háttér.</span><span class="sxs-lookup"><span data-stu-id="81651-145">Yes, Application Gateway inserts x-forwarded-for, x-forwarded-proto, and x-forwarded-port headers into hello request forwarded toohello backend.</span></span> <span data-ttu-id="81651-146">hello x-továbbított-a fejléc formátuma IP:Port vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="81651-146">hello format for x-forwarded-for header is a comma-separated list of IP:Port.</span></span> <span data-ttu-id="81651-147">hello érvényes x továbbított protokoll értékei http vagy HTTPS protokollt.</span><span class="sxs-lookup"><span data-stu-id="81651-147">hello valid values for x-forwarded-proto are http or https.</span></span> <span data-ttu-id="81651-148">X továbbított port hello portot mely hello kérésére hello Alkalmazásátjáró címen érhető határozza meg.</span><span class="sxs-lookup"><span data-stu-id="81651-148">X-forwarded-port specifies hello port at which hello request reached at hello Application Gateway.</span></span>

<span data-ttu-id="81651-149">**Q. Mennyi ideig tart toodeploy olyan átjárót? Az Alkalmazásátjáró továbbra is működik, ha frissítése során?**</span><span class="sxs-lookup"><span data-stu-id="81651-149">**Q. How long does it take toodeploy an Application Gateway? Does my Application Gateway still work when being updated?**</span></span>

<span data-ttu-id="81651-150">Új Alkalmazásátjáró telepítések too20 perc tooprovision is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="81651-150">New Application Gateway deployments can take up too20 minutes tooprovision.</span></span> <span data-ttu-id="81651-151">Nincsenek zavaró módosításokat tooinstance mérete és száma, és ebben az időszakban hello átjáró aktív marad.</span><span class="sxs-lookup"><span data-stu-id="81651-151">Changes tooinstance size/count are not disruptive, and hello gateway remains active during this time.</span></span>

## <a name="configuration"></a><span data-ttu-id="81651-152">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="81651-152">Configuration</span></span>

<span data-ttu-id="81651-153">**Q. Alkalmazásátjáró mindig telepítve van a virtuális hálózaton?**</span><span class="sxs-lookup"><span data-stu-id="81651-153">**Q. Is Application Gateway always deployed in a virtual network?**</span></span>

<span data-ttu-id="81651-154">Igen, az Alkalmazásátjáró mindig a rendszer a virtuális hálózati alhálózat.</span><span class="sxs-lookup"><span data-stu-id="81651-154">Yes, Application Gateway is always deployed in a virtual network subnet.</span></span> <span data-ttu-id="81651-155">Ez az alhálózat csak tartalmazhat Alkalmazásátjárót.</span><span class="sxs-lookup"><span data-stu-id="81651-155">This subnet can only contain Application Gateways.</span></span>

<span data-ttu-id="81651-156">**Q. Alkalmazásátjáró működik a virtuális hálózaton kívüli tooinstances?**</span><span class="sxs-lookup"><span data-stu-id="81651-156">**Q. Can Application Gateway talk tooinstances outside its virtual network?**</span></span>

<span data-ttu-id="81651-157">Alkalmazásátjáró hello mindaddig, amíg nincs IP-kapcsolat van virtuális hálózaton kívüli tooinstances működik.</span><span class="sxs-lookup"><span data-stu-id="81651-157">Application Gateway can talk tooinstances outside of hello virtual network that it is in as long as there is IP connectivity.</span></span> <span data-ttu-id="81651-158">Ha azt tervezi, toouse belső IP-címek, a háttér a készlet tagjainak, akkor azt igényli [VNETBEN társviszony-létesítés](../virtual-network/virtual-network-peering-overview.md) vagy [VPN-átjáró](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span><span class="sxs-lookup"><span data-stu-id="81651-158">If you plan toouse internal IPs as backend pool members, then it requires [VNET Peering](../virtual-network/virtual-network-peering-overview.md) or [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md).</span></span>

<span data-ttu-id="81651-159">**Q. Központi telepítését bármi más hello Alkalmazásátjáró alhálózat?**</span><span class="sxs-lookup"><span data-stu-id="81651-159">**Q. Can I deploy anything else in hello Application Gateway subnet?**</span></span>

<span data-ttu-id="81651-160">Nem, de telepítheti a más alkalmazásátjárót hello alhálózat.</span><span class="sxs-lookup"><span data-stu-id="81651-160">No, but you can deploy other application gateways in hello subnet.</span></span>

<span data-ttu-id="81651-161">**Q. Hálózati biztonsági csoportok hello Alkalmazásátjáró alhálózaton támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="81651-161">**Q. Are Network Security Groups supported on hello Application Gateway subnet?**</span></span>

<span data-ttu-id="81651-162">A következő korlátozások hello hello Alkalmazásátjáró alhálózaton hálózati biztonsági csoportok használata támogatott:</span><span class="sxs-lookup"><span data-stu-id="81651-162">Network Security Groups are supported on hello Application Gateway subnet with hello following restrictions:</span></span>

* <span data-ttu-id="81651-163">Kivételek kell elhelyezni, a bejövő forgalmat a háttérrendszer állapotfigyelő toowork 65503-65534 portok megfelelően.</span><span class="sxs-lookup"><span data-stu-id="81651-163">Exceptions must be put in for incoming traffic on ports 65503-65534 for backend health toowork correctly.</span></span>

* <span data-ttu-id="81651-164">Nem blokkolható a kimenő internetkapcsolat.</span><span class="sxs-lookup"><span data-stu-id="81651-164">Outbound internet connectivity can not be blocked.</span></span>

* <span data-ttu-id="81651-165">Engedélyezni kell az AzureLoadBalancer címke hello forgalmát.</span><span class="sxs-lookup"><span data-stu-id="81651-165">Traffic from hello AzureLoadBalancer tag must be allowed.</span></span>

<span data-ttu-id="81651-166">**Q. Az Alkalmazásátjáró hello korlátai Tudom növelni a működés felső korlátjának?**</span><span class="sxs-lookup"><span data-stu-id="81651-166">**Q. What are hello limits on Application Gateway? Can I increase these limits?**</span></span>

<span data-ttu-id="81651-167">Látogasson el [alkalmazás átjáró korlátok](../azure-subscription-service-limits.md#application-gateway-limits) tooview hello korlátok.</span><span class="sxs-lookup"><span data-stu-id="81651-167">Visit [Application Gateway Limits](../azure-subscription-service-limits.md#application-gateway-limits) tooview hello limits.</span></span>

<span data-ttu-id="81651-168">**Q. Használhatok Alkalmazásátjáró külső és belső forgalmát egyidejűleg?**</span><span class="sxs-lookup"><span data-stu-id="81651-168">**Q. Can I use Application Gateway for both external and internal traffic simultaneously?**</span></span>

<span data-ttu-id="81651-169">Alkalmazásátjáró Igen, támogatja az egy belső IP-cím és egy külső IP-cím az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="81651-169">Yes, Application Gateway supports having one internal IP and one external IP per Application Gateway.</span></span>

<span data-ttu-id="81651-170">**Q. Támogatott Vnetben társviszony-létesítés?**</span><span class="sxs-lookup"><span data-stu-id="81651-170">**Q. Is VNet peering supported?**</span></span>

<span data-ttu-id="81651-171">Igen, Vnetben társviszony-létesítés támogatott és hasznos a terheléselosztás forgalom más virtuális hálózatok.</span><span class="sxs-lookup"><span data-stu-id="81651-171">Yes, VNet peering is supported and is beneficial for load balancing traffic in other virtual networks.</span></span>

<span data-ttu-id="81651-172">**Q. I működik tooon helyszíni kiszolgálók expressroute-on vagy VPN-alagutat csatlakozáskor?**</span><span class="sxs-lookup"><span data-stu-id="81651-172">**Q. Can I talk tooon-premises servers when they are connected by ExpressRoute or VPN tunnels?**</span></span>

<span data-ttu-id="81651-173">Igen, mindaddig, amíg forgalom engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="81651-173">Yes, as long as traffic is allowed.</span></span>

<span data-ttu-id="81651-174">**Q. Rendelkezhet eltérő portokon számos alkalmazás szolgál egy háttérkészletéből?**</span><span class="sxs-lookup"><span data-stu-id="81651-174">**Q. Can I have one backend pool serving many applications on different ports?**</span></span>

<span data-ttu-id="81651-175">Micro service-architektúra esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="81651-175">Micro service architecture is supported.</span></span> <span data-ttu-id="81651-176">Több http konfigurált beállítások tooprobe eltérő portokon kellene.</span><span class="sxs-lookup"><span data-stu-id="81651-176">You would need multiple http settings configured tooprobe on different ports.</span></span>

<span data-ttu-id="81651-177">**Q. Támogatják egyéni mintavételt helyettesítő karakterekkel vagy reguláris kifejezéssel az érkezett válasz adatait?**</span><span class="sxs-lookup"><span data-stu-id="81651-177">**Q. Do custom probes support wildcards/regex on response data?**</span></span>

<span data-ttu-id="81651-178">Egyéni mintavételt nem támogatják a helyettesítő karakteres vagy regex érkezett válasz adatait a.</span><span class="sxs-lookup"><span data-stu-id="81651-178">Custom probes do not support wildcard or regex on response data.</span></span> 

<span data-ttu-id="81651-179">**Q. Szabályok feldolgozásának módja?**</span><span class="sxs-lookup"><span data-stu-id="81651-179">**Q. How are rules processed?**</span></span>

<span data-ttu-id="81651-180">Szabályok feldolgozása hello sorrendben vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="81651-180">Rules are processed in hello order they are configured.</span></span> <span data-ttu-id="81651-181">Javasoljuk, hogy többhelyes szabályok konfigurálva vannak-e, mielőtt alapvető szabályok tooreduce hello esélye annak, hogy az adatforgalom irányított toohello nem megfelelő háttér alapszintű hello szabály alapján előzetes toohello többhelyes portszabály értékelt forgalom megfelelő módon.</span><span class="sxs-lookup"><span data-stu-id="81651-181">It is recommended that multi-site rules are configured before basic rules tooreduce hello chance that traffic is routed toohello inappropriate backend as hello basic rule would match traffic based on port prior toohello multi-site rule being evaluated.</span></span>

<span data-ttu-id="81651-182">**Q. Szabályok feldolgozásának módja?**</span><span class="sxs-lookup"><span data-stu-id="81651-182">**Q. How are rules processed?**</span></span>

<span data-ttu-id="81651-183">Szabályok feldolgozása hello ahhoz, azok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="81651-183">Rules are processed in hello order they are created.</span></span> <span data-ttu-id="81651-184">Javasoljuk, hogy a többhelyes szabályok előtt alapvető szabályok vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="81651-184">It is recommended that multi-site rules are configured before basic rules.</span></span> <span data-ttu-id="81651-185">Többhelyes figyelői először konfigurálásával, ez a konfiguráció csökkenti a hello esélye annak, hogy a forgalom irányított toohello nem megfelelő háttér legyen.</span><span class="sxs-lookup"><span data-stu-id="81651-185">By configuring multi-site listeners first, this configuration reduces hello chance that traffic is routed toohello inappropriate backend.</span></span> <span data-ttu-id="81651-186">Alapszintű hello szabály alapján előzetes toohello többhelyes portszabály értékelt forgalom megfelelő útválasztási probléma fordulhatnak elő.</span><span class="sxs-lookup"><span data-stu-id="81651-186">This routing issue can occur as hello basic rule would match traffic based on port prior toohello multi-site rule being evaluated.</span></span>

<span data-ttu-id="81651-187">**Q. Mi az egyéni mintavételt hello a gazdagép mező jelölésére?**</span><span class="sxs-lookup"><span data-stu-id="81651-187">**Q. What does hello Host field for custom probes signify?**</span></span>

<span data-ttu-id="81651-188">A gazdagép mező hello neve toosend hello mintavételi a határozza meg.</span><span class="sxs-lookup"><span data-stu-id="81651-188">Host field specifies hello name toosend hello probe to.</span></span> <span data-ttu-id="81651-189">Alkalmazandó csak akkor, ha több hely van beállítva az alkalmazás-átjárón, ellenkező esetben használja a "127.0.0.1".</span><span class="sxs-lookup"><span data-stu-id="81651-189">Applicable only when multi-site is configured on Application Gateway, otherwise use '127.0.0.1'.</span></span> <span data-ttu-id="81651-190">Ez az érték eltér a virtuális gép állomásnevét, és formátumú \<protokoll\>://\<állomás\>:\<port\>\<elérési\>.</span><span class="sxs-lookup"><span data-stu-id="81651-190">This value is different from VM host name and is in format \<protocol\>://\<host\>:\<port\>\<path\>.</span></span>

<span data-ttu-id="81651-191">**Q. Engedélyezési lista Alkalmazásátjáró hozzáférés tooa telepíthetek kevés forrás IP-cím?**</span><span class="sxs-lookup"><span data-stu-id="81651-191">**Q. Can I whitelist Application Gateway access tooa few source IPs?**</span></span>

<span data-ttu-id="81651-192">Ebben a forgatókönyvben végezhető Alkalmazásátjáró alhálózaton NSG-ket használ.</span><span class="sxs-lookup"><span data-stu-id="81651-192">This scenario can be done using NSGs on Application Gateway subnet.</span></span> <span data-ttu-id="81651-193">a következő korlátozások hello a prioritásuk szerinti sorrendben felsorolva hello hello alhálózaton kell rendezni:</span><span class="sxs-lookup"><span data-stu-id="81651-193">hello following restrictions should be put on hello subnet in hello listed order of priority:</span></span>

* <span data-ttu-id="81651-194">Engedélyezi a bejövő forgalom forrás IP-/ IP-címtartomány.</span><span class="sxs-lookup"><span data-stu-id="81651-194">Allow incoming traffic from source IP/IP range.</span></span>

* <span data-ttu-id="81651-195">Az összes források tooports 65503-65534 a bejövő kérések engedélyezése [háttér állapotfigyelő kommunikációja](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="81651-195">Allow incoming requests from all sources tooports 65503-65534 for [backend health communication](application-gateway-diagnostics.md).</span></span>

* <span data-ttu-id="81651-196">Annak engedélyezése, hogy a bejövő Azure Load Balancer mintavételt (AzureLoadBalancer címke) és a bejövő virtuális hálózati forgalom (VirtualNetwork címke) hello [NSG](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="81651-196">Allow incoming Azure Load Balancer probes (AzureLoadBalancer tag) and inbound virtual network traffic (VirtualNetwork tag) on hello [NSG](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="81651-197">Megtagadási minden egyéb bejövő forgalom blokkolása minden szabály.</span><span class="sxs-lookup"><span data-stu-id="81651-197">Block all other incoming traffic with a Deny all rule.</span></span>

* <span data-ttu-id="81651-198">Engedélyezze a kimenő forgalom toohello internet összes célhoz.</span><span class="sxs-lookup"><span data-stu-id="81651-198">Allow outbound traffic toohello internet for all destinations.</span></span>

## <a name="performance"></a><span data-ttu-id="81651-199">Teljesítmény</span><span class="sxs-lookup"><span data-stu-id="81651-199">Performance</span></span>

<span data-ttu-id="81651-200">**Q. Hogyan támogatja a Alkalmazásátjáró a magas rendelkezésre állás és méretezhetőség?**</span><span class="sxs-lookup"><span data-stu-id="81651-200">**Q. How does Application Gateway support high availability and scalability?**</span></span>

<span data-ttu-id="81651-201">Alkalmazásátjáró támogatja a magas rendelkezésre állás elérésére, ha két vagy több példányt.</span><span class="sxs-lookup"><span data-stu-id="81651-201">Application Gateway supports high availability scenarios when you have two or more instances deployed.</span></span> <span data-ttu-id="81651-202">Azure ezek a példányok elosztása az összes példány nem meghiúsulhatnak hello frissítés és a tartalék tartományok tooensure ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="81651-202">Azure distributes these instances across update and fault domains tooensure that all instances do not fail at hello same time.</span></span> <span data-ttu-id="81651-203">Alkalmazásátjáró méretezhetőség támogatja több példány hello hozzáadásával azonos átjáró tooshare hello terhelés.</span><span class="sxs-lookup"><span data-stu-id="81651-203">Application Gateway supports scalability by adding multiple instances of hello same gateway tooshare hello load.</span></span>

<span data-ttu-id="81651-204">**Q. Hogyan érhetők el vész-Helyreállítási forgatókönyv Alkalmazásátjáró adatközpontjaiban között?**</span><span class="sxs-lookup"><span data-stu-id="81651-204">**Q. How do I achieve DR scenario across data centers with Application Gateway?**</span></span>

<span data-ttu-id="81651-205">Az ügyfelek használhatják a Traffic Manager toodistribute forgalom különböző adatközpontokban több alkalmazás-átjáró között.</span><span class="sxs-lookup"><span data-stu-id="81651-205">Customers can use Traffic Manager toodistribute traffic across multiple Application Gateways in different datacenters.</span></span>

<span data-ttu-id="81651-206">**Q. Automatikus skálázással támogatva van?**</span><span class="sxs-lookup"><span data-stu-id="81651-206">**Q. Is auto scaling supported?**</span></span>

<span data-ttu-id="81651-207">Nem, de Application Gateway használható használt tooalert átviteli metrika, amikor egy küszöbértéket.</span><span class="sxs-lookup"><span data-stu-id="81651-207">No, but Application Gateway has a throughput metric that can be used tooalert you when a threshold is reached.</span></span> <span data-ttu-id="81651-208">Példányok hozzáadásának vagy kicserélésének mérete manuálisan hello átjáró nem indul újra, és nem befolyásolja a meglévő forgalom.</span><span class="sxs-lookup"><span data-stu-id="81651-208">Manually adding instances or changing size does not restart hello gateway and does not impact existing traffic.</span></span>

<span data-ttu-id="81651-209">**Q. Fel/le OK állásidő, nem manuális méretezési?**</span><span class="sxs-lookup"><span data-stu-id="81651-209">**Q. Does manual scale up/down cause downtime?**</span></span>

<span data-ttu-id="81651-210">Állásidő nélkül, a frissítési tartományok és a tartalék tartományok között elosztott példányok.</span><span class="sxs-lookup"><span data-stu-id="81651-210">There is no downtime, instances are distributed across upgrade domains and fault domains.</span></span>

<span data-ttu-id="81651-211">**Q. Módosítható a közepes toolarge megszakítása nélkül példányméretének?**</span><span class="sxs-lookup"><span data-stu-id="81651-211">**Q. Can I change instance size from medium toolarge without disruption?**</span></span>

<span data-ttu-id="81651-212">Igen, Azure elosztja példányok nem meghiúsulhatnak, minden példány hello frissítés és a tartalék tartományok tooensure ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="81651-212">Yes, Azure distributes instances across update and fault domains tooensure that all instances do not fail at hello same time.</span></span> <span data-ttu-id="81651-213">Alkalmazás átjáró támogatja a több példánya hozzáadásával skálázás hello azonos átjáró tooshare hello terhelés.</span><span class="sxs-lookup"><span data-stu-id="81651-213">Application Gateway supports scaling by adding multiple instances of hello same gateway tooshare hello load.</span></span>

## <a name="ssl-configuration"></a><span data-ttu-id="81651-214">SSL-beállítása</span><span class="sxs-lookup"><span data-stu-id="81651-214">SSL Configuration</span></span>

<span data-ttu-id="81651-215">**Q. Milyen tanúsítványok Alkalmazásátjáró támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="81651-215">**Q. What certificates are supported on Application Gateway?**</span></span>

<span data-ttu-id="81651-216">Önaláírt tanúsítványok, a CA-tanúsítványok, és a helyettesítő tanúsítványok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="81651-216">Self signed certs, CA certs, and wild-card certs are supported.</span></span> <span data-ttu-id="81651-217">EV tanúsítványok használata nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="81651-217">EV certs are not supported.</span></span>

<span data-ttu-id="81651-218">**Q. Mik azok a hello alkalmazás átjáró által támogatott aktuális titkosító csomagok?**</span><span class="sxs-lookup"><span data-stu-id="81651-218">**Q. What are hello current cipher suites supported by Application Gateway?**</span></span>

<span data-ttu-id="81651-219">Az alábbiakban hello hello alkalmazás átjáró által támogatott aktuális titkosító csomagok.</span><span class="sxs-lookup"><span data-stu-id="81651-219">hello following are hello current cipher suites supported by application gateway.</span></span> <span data-ttu-id="81651-220">Látogasson el: [SSL konfigurálása házirend verziója és az Application Gateway titkosító csomagok](application-gateway-configure-ssl-policy-powershell.md) toolearn hogyan toocustomize SSL-beállítások.</span><span class="sxs-lookup"><span data-stu-id="81651-220">Visit: [Configure SSL policy versions and cipher suites on Application Gateway](application-gateway-configure-ssl-policy-powershell.md) toolearn how toocustomize SSL options.</span></span>

- <span data-ttu-id="81651-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="81651-221">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="81651-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-222">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="81651-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-223">TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="81651-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-224">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="81651-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="81651-225">TLS_DHE_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="81651-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-226">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="81651-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-227">TLS_DHE_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="81651-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-228">TLS_DHE_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="81651-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="81651-229">TLS_RSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="81651-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-230">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="81651-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-231">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="81651-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-232">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="81651-233">TLS_RSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-233">TLS_RSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="81651-234">TLS_RSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-234">TLS_RSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="81651-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span><span class="sxs-lookup"><span data-stu-id="81651-235">TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384</span></span>
- <span data-ttu-id="81651-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-236">TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256</span></span>
- <span data-ttu-id="81651-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span><span class="sxs-lookup"><span data-stu-id="81651-237">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384</span></span>
- <span data-ttu-id="81651-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-238">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="81651-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-239">TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="81651-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-240">TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="81651-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-241">TLS_DHE_DSS_WITH_AES_256_CBC_SHA256</span></span>
- <span data-ttu-id="81651-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-242">TLS_DHE_DSS_WITH_AES_128_CBC_SHA256</span></span>
- <span data-ttu-id="81651-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-243">TLS_DHE_DSS_WITH_AES_256_CBC_SHA</span></span>
- <span data-ttu-id="81651-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-244">TLS_DHE_DSS_WITH_AES_128_CBC_SHA</span></span>
- <span data-ttu-id="81651-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-245">TLS_RSA_WITH_3DES_EDE_CBC_SHA</span></span>
- <span data-ttu-id="81651-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span><span class="sxs-lookup"><span data-stu-id="81651-246">TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA</span></span>

<span data-ttu-id="81651-247">**Q. Alkalmazásátjáró is támogatja a forgalom toohello háttér újbóli titkosítása?**</span><span class="sxs-lookup"><span data-stu-id="81651-247">**Q. Does Application Gateway also support re-encryption of traffic toohello backend?**</span></span>

<span data-ttu-id="81651-248">Igen, Alkalmazásátjáró támogatja az SSL kiszervezési és end tooend SSL, amely hello forgalom toohello háttér újra titkosítja.</span><span class="sxs-lookup"><span data-stu-id="81651-248">Yes, Application Gateway supports SSL offload, and end tooend SSL, which re-encrypts hello traffic toohello backend.</span></span>

<span data-ttu-id="81651-249">**Q. Konfigurálhatja a SSL házirend toocontrol SSL protokoll verziója?**</span><span class="sxs-lookup"><span data-stu-id="81651-249">**Q. Can I configure SSL policy toocontrol SSL Protocol versions?**</span></span>

<span data-ttu-id="81651-250">Igen, konfigurálhatja az Application Gateway toodeny TLS1.0 TLS1.1 és TLS1.2.</span><span class="sxs-lookup"><span data-stu-id="81651-250">Yes, you can configure Application Gateway toodeny TLS1.0, TLS1.1, and TLS1.2.</span></span> <span data-ttu-id="81651-251">Az SSL 2.0 és 3.0 vannak már le van tiltva alapértelmezés szerint, és amelyek nem konfigurálhatók.</span><span class="sxs-lookup"><span data-stu-id="81651-251">SSL 2.0 and 3.0 are already disabled by default and are not configurable.</span></span>

<span data-ttu-id="81651-252">**Q. Titkosítási csomagok és a házirendek sorrendjének is konfigurálni?**</span><span class="sxs-lookup"><span data-stu-id="81651-252">**Q. Can I configure cipher suites and policy order?**</span></span>

<span data-ttu-id="81651-253">Igen, [titkosító csomagok konfigurációjának](application-gateway-ssl-policy-overview.md) esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="81651-253">Yes, [configuration of cipher suites](application-gateway-ssl-policy-overview.md) is supported.</span></span> <span data-ttu-id="81651-254">Egyéni házirend meghatározása esetén titkosító csomagok a következő hello legalább egyikét engedélyezni kell.</span><span class="sxs-lookup"><span data-stu-id="81651-254">When defining a custom policy, at least one of hello following cipher suites must be enabled.</span></span> <span data-ttu-id="81651-255">Alkalmazásátjáró SHA256 toofor háttér-kezelést használ.</span><span class="sxs-lookup"><span data-stu-id="81651-255">Application gateway uses SHA256 toofor backend management.</span></span>

* <span data-ttu-id="81651-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-256">TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256</span></span> 
* <span data-ttu-id="81651-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-257">TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256</span></span>
* <span data-ttu-id="81651-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-258">TLS_DHE_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="81651-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-259">TLS_RSA_WITH_AES_128_GCM_SHA256</span></span>
* <span data-ttu-id="81651-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-260">TLS_RSA_WITH_AES_256_CBC_SHA256</span></span>
* <span data-ttu-id="81651-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span><span class="sxs-lookup"><span data-stu-id="81651-261">TLS_RSA_WITH_AES_128_CBC_SHA256</span></span>

<span data-ttu-id="81651-262">**Q. Hány SSL-tanúsítványok támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="81651-262">**Q. How many SSL certificates are supported?**</span></span>

<span data-ttu-id="81651-263">Másolatot too20 SSL támogatottak.</span><span class="sxs-lookup"><span data-stu-id="81651-263">Up too20 SSL certificates are supported.</span></span>

<span data-ttu-id="81651-264">**Q. A háttérrendszer újbóli titkosítása hány tanúsítványhitelesítést biztosítsanak támogatottak?**</span><span class="sxs-lookup"><span data-stu-id="81651-264">**Q. How many authentication certificates for backend re-encryption are supported?**</span></span>

<span data-ttu-id="81651-265">Too10 be a hitelesítési tanúsítványok az alapértelmezett érték 5 támogatottak.</span><span class="sxs-lookup"><span data-stu-id="81651-265">Up too10 authentication certificates are supported with a default of 5.</span></span>

<span data-ttu-id="81651-266">**Q. Nem Alkalmazásátjáró integrálása az Azure Key Vault natív módon?**</span><span class="sxs-lookup"><span data-stu-id="81651-266">**Q. Does Application Gateway integrate with Azure Key Vault natively?**</span></span>

<span data-ttu-id="81651-267">Nem, nem integrálva van az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="81651-267">No, it is not integrated with Azure Key Vault.</span></span>

## <a name="web-application-firewall-waf-configuration"></a><span data-ttu-id="81651-268">Webes alkalmazás tűzfalat (WAF) konfigurációja</span><span class="sxs-lookup"><span data-stu-id="81651-268">Web Application Firewall (WAF) Configuration</span></span>

<span data-ttu-id="81651-269">**Q. Hello WAF SKU nyújtja a hello Standard Termékváltozat elérhető összes hello szolgáltatások?**</span><span class="sxs-lookup"><span data-stu-id="81651-269">**Q. Does hello WAF SKU offer all hello features available with hello Standard SKU?**</span></span>

<span data-ttu-id="81651-270">WAF Igen, a Standard Termékváltozat hello támogatja az összes hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="81651-270">Yes, WAF supports all hello features in hello Standard SKU.</span></span>

<span data-ttu-id="81651-271">**Q. Mi az a hello CRS verziója támogatja a Alkalmazásátjáró?**</span><span class="sxs-lookup"><span data-stu-id="81651-271">**Q. What is hello CRS version Application Gateway supports?**</span></span>

<span data-ttu-id="81651-272">Alkalmazásátjáró támogatja CRS [program 2.2.9-es](application-gateway-crs-rulegroups-rules.md#owasp229) és CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span><span class="sxs-lookup"><span data-stu-id="81651-272">Application Gateway supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and CRS [3.0](application-gateway-crs-rulegroups-rules.md#owasp30).</span></span>

<span data-ttu-id="81651-273">**Q. Hogyan figyelhetek WAF?**</span><span class="sxs-lookup"><span data-stu-id="81651-273">**Q. How do I monitor WAF?**</span></span>

<span data-ttu-id="81651-274">WAF keresztül diagnosztikai naplózás felügyelet alatt, további információt a diagnosztikai naplózás helyen találhatók [diagnosztikai naplózás és az Alkalmazásátjáró metrikák](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="81651-274">WAF is monitored through diagnostic logging, more information on diagnostic logging can be found at [Diagnostics Logging and Metrics for Application Gateway](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="81651-275">**Q. Blokkolja az észlelési mód forgalom?**</span><span class="sxs-lookup"><span data-stu-id="81651-275">**Q. Does detection mode block traffic?**</span></span>

<span data-ttu-id="81651-276">Nem, a észlelési mód csak naplózza a forgalmat, amely egy WAF szabály elindul.</span><span class="sxs-lookup"><span data-stu-id="81651-276">No, detection mode only logs traffic, which triggered a WAF rule.</span></span>

<span data-ttu-id="81651-277">**Q. Hogyan testre szabhatja a WAF szabályokat?**</span><span class="sxs-lookup"><span data-stu-id="81651-277">**Q. How do I customize WAF rules?**</span></span>

<span data-ttu-id="81651-278">Igen, WAF szabályok lettek testre szabható, hogyan toocustomize őket látogasson el a további információt [testreszabása WAF csoportok és szabályok](application-gateway-customize-waf-rules-portal.md)</span><span class="sxs-lookup"><span data-stu-id="81651-278">Yes, WAF rules are customizable, for more information on how toocustomize them visit [Customize WAF rule groups and rules](application-gateway-customize-waf-rules-portal.md)</span></span>

<span data-ttu-id="81651-279">**Q. Milyen szabályok jelenleg érhetők el?**</span><span class="sxs-lookup"><span data-stu-id="81651-279">**Q. What rules are currently available?**</span></span>

<span data-ttu-id="81651-280">WAF jelenleg CRS [program 2.2.9-es](application-gateway-crs-rulegroups-rules.md#owasp229) és [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), első 10 biztonsági réseit által megnyitott webes alkalmazás biztonsági Project (OWASP) itt található hello nyújt elleni hello többségét eredeti biztonsági [OWASP felső 10 biztonsági réseket](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span><span class="sxs-lookup"><span data-stu-id="81651-280">WAF currently supports CRS [2.2.9](application-gateway-crs-rulegroups-rules.md#owasp229) and [3.0](application-gateway-crs-rulegroups-rules.md#owasp30), which provide baseline security against most of hello top 10 vulnerabilities identified by hello Open Web Application Security Project (OWASP) found here [OWASP top 10 Vulnerabilities](https://www.owasp.org/index.php/Top10#OWASP_Top_10_for_2013)</span></span>

* <span data-ttu-id="81651-281">SQL-injektálás elleni védelem</span><span class="sxs-lookup"><span data-stu-id="81651-281">SQL injection protection</span></span>

* <span data-ttu-id="81651-282">Webhelyek közötti, parancsprogramot alkalmazó támadások elleni védelem</span><span class="sxs-lookup"><span data-stu-id="81651-282">Cross site scripting protection</span></span>

* <span data-ttu-id="81651-283">Gyakori webes támadások (például parancsinjektálás, HTTP-kéréscsempészet, HTTP-válaszfelosztás és távolifájl-beszúrásos támadás) elleni védelem</span><span class="sxs-lookup"><span data-stu-id="81651-283">Common Web Attacks Protection such as command injection, HTTP request smuggling, HTTP response splitting, and remote file inclusion attack</span></span>

* <span data-ttu-id="81651-284">HTTP protokoll megsértése elleni védelem</span><span class="sxs-lookup"><span data-stu-id="81651-284">Protection against HTTP protocol violations</span></span>

* <span data-ttu-id="81651-285">HTTP protokollanomáliák (például hiányzó gazdagép-felhasználói ügynök és Accept (Elfogadás) fejlécek) elleni védelem</span><span class="sxs-lookup"><span data-stu-id="81651-285">Protection against HTTP protocol anomalies such as missing host user-agent and accept headers</span></span>

* <span data-ttu-id="81651-286">Robotprogramok, webbejárók és képolvasók elleni védelem</span><span class="sxs-lookup"><span data-stu-id="81651-286">Prevention against bots, crawlers, and scanners</span></span>

* <span data-ttu-id="81651-287">A gyakori alkalmazás konfigurációs hibák (Ez azt jelenti, hogy Apache, az IIS, stb.) észlelése</span><span class="sxs-lookup"><span data-stu-id="81651-287">Detection of common application misconfigurations (that is, Apache, IIS, etc.)</span></span>

<span data-ttu-id="81651-288">**Q. WAF is támogatja a DDoS megelőzési?**</span><span class="sxs-lookup"><span data-stu-id="81651-288">**Q. Does WAF also support DDoS prevention?**</span></span>

<span data-ttu-id="81651-289">WAF nem, nem biztosít DDoS megelőzése.</span><span class="sxs-lookup"><span data-stu-id="81651-289">No, WAF does not provide DDoS prevention.</span></span>

## <a name="diagnostics-and-logging"></a><span data-ttu-id="81651-290">Diagnosztika és naplózás</span><span class="sxs-lookup"><span data-stu-id="81651-290">Diagnostics and Logging</span></span>

<span data-ttu-id="81651-291">**Q. Milyen típusú naplók az Alkalmazásátjáró érhetők el?**</span><span class="sxs-lookup"><span data-stu-id="81651-291">**Q. What types of logs are available with Application Gateway?**</span></span>

<span data-ttu-id="81651-292">Nincsenek elérhető az Alkalmazásátjáró három naplókat.</span><span class="sxs-lookup"><span data-stu-id="81651-292">There are three logs available for Application Gateway.</span></span> <span data-ttu-id="81651-293">Ezek a naplók és más diagnosztikai képességek a további tudnivalókért keresse fel [háttér állapot, a diagnosztikai naplók és a metrikák az Alkalmazásátjáró](application-gateway-diagnostics.md).</span><span class="sxs-lookup"><span data-stu-id="81651-293">For more information on these logs and other diagnostic capabilities, visit [Backend health, diagnostics logs, and metrics for Application Gateway](application-gateway-diagnostics.md).</span></span>

- <span data-ttu-id="81651-294">**ApplicationGatewayAccessLog** -hello hozzáférési napló kérelmet tartalmaz, minden küldött toohello Alkalmazásátjáró előtér.</span><span class="sxs-lookup"><span data-stu-id="81651-294">**ApplicationGatewayAccessLog** - hello access log contains each request submitted toohello Application Gateway frontend.</span></span> <span data-ttu-id="81651-295">hello adatok hello hívó IP, kért, URL-cím válasz késés, bejövő és kimenő adatforgalma visszatérési kód, bájt. Hozzáférési napló gyűjtése 300 másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="81651-295">hello data includes hello caller's IP, URL requested, response latency, return code, bytes in and out. Access log is collected every 300 seconds.</span></span> <span data-ttu-id="81651-296">Ez a napló Application Gateway-példányonként egy bejegyzést tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="81651-296">This log contains one record per instance of Application Gateway.</span></span>
- <span data-ttu-id="81651-297">**ApplicationGatewayPerformanceLog** -hello teljesítmény naplófájl rögzíti a teljesítményadatok alapon / példány teljes kérelem kiszolgálása, beleértve az átviteli sebesség (bájt), a kiszolgált kérelmek teljes száma a sikertelen kérelmek száma, a megfelelő és nem kifogástalan háttér-példányok száma.</span><span class="sxs-lookup"><span data-stu-id="81651-297">**ApplicationGatewayPerformanceLog** - hello performance log captures performance information on per instance basis including total request served, throughput in bytes, total requests served, failed request count, healthy and unhealthy back-end instance count.</span></span>
- <span data-ttu-id="81651-298">**ApplicationGatewayFirewallLog** -hello tűzfal a napló tartalmazza, amelyeket a rendszer a webalkalmazási tűzfal a konfigurált Alkalmazásátjáró észlelési vagy megelőzési módban kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="81651-298">**ApplicationGatewayFirewallLog** - hello firewall log contains requests that are logged through either detection or prevention mode of an application gateway that is configured with web application firewall.</span></span>

<span data-ttu-id="81651-299">**Q. Hogyan állapítható meg, ha a háttérkiszolgáló készlettagra megfelelő?**</span><span class="sxs-lookup"><span data-stu-id="81651-299">**Q. How do I know if my backend pool members are healthy?**</span></span>

<span data-ttu-id="81651-300">Hello PowerShell-parancsmag `Get-AzureRmApplicationGatewayBackendHealth` vagy hello portálon keresztül állapotát ellenőrizni ellátogatva [Alkalmazásdiagnosztika átjáró](application-gateway-diagnostics.md)</span><span class="sxs-lookup"><span data-stu-id="81651-300">You can use hello PowerShell cmdlet `Get-AzureRmApplicationGatewayBackendHealth` or verify health through hello portal by visiting [Application Gateway Diagnostics](application-gateway-diagnostics.md)</span></span>

<span data-ttu-id="81651-301">**Q. Mi az az adatmegőrzési hello hello diagnosztika napló?**</span><span class="sxs-lookup"><span data-stu-id="81651-301">**Q. What is hello retention policy on hello diagnostics logs?**</span></span>

<span data-ttu-id="81651-302">Diagnosztikai naplók folyamat toohello ügyfelek tárfiók, és az ügyfelek hello megőrzési házirend alapján választaniuk be.</span><span class="sxs-lookup"><span data-stu-id="81651-302">Diagnostic logs flow toohello customers storage account and customers can set hello retention policy based on their preference.</span></span> <span data-ttu-id="81651-303">Diagnosztikai naplók is küldhetők tooan Eseményközpont vagy Naplóelemzési.</span><span class="sxs-lookup"><span data-stu-id="81651-303">Diagnostic logs can also be sent tooan Event Hub or Log Analytics.</span></span> <span data-ttu-id="81651-304">Látogasson el [átjáró Alkalmazásdiagnosztika](application-gateway-diagnostics.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="81651-304">Visit [Application Gateway Diagnostics](application-gateway-diagnostics.md) for more details.</span></span>

<span data-ttu-id="81651-305">**Q. Hogyan szerezhetek naplók az Alkalmazásátjáró?**</span><span class="sxs-lookup"><span data-stu-id="81651-305">**Q. How do I get audit logs for Application Gateway?**</span></span>

<span data-ttu-id="81651-306">Az Alkalmazásátjáró naplók érhetők el.</span><span class="sxs-lookup"><span data-stu-id="81651-306">Audit logs are available for Application Gateway.</span></span> <span data-ttu-id="81651-307">Hello portálon kattintson **tevékenységnapló** hello menü panelen a naplók Alkalmazásátjáró tooaccess hello.</span><span class="sxs-lookup"><span data-stu-id="81651-307">In hello portal, click **Activity Log** in hello menu blade of an Application Gateway tooaccess hello audit log.</span></span> 

<span data-ttu-id="81651-308">**Q. Beállíthatja a Alkalmazásátjáró riasztások?**</span><span class="sxs-lookup"><span data-stu-id="81651-308">**Q. Can I set alerts with Application Gateway?**</span></span>

<span data-ttu-id="81651-309">Igen, Alkalmazásátjáró támogatja a riasztások, értesítések metrikák ki vannak konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="81651-309">Yes, Application Gateway does support alerts, alerts are configured off metrics.</span></span>  <span data-ttu-id="81651-310">Alkalmazásátjáró "átviteli", amely lehet konfigurált tooalert metrika van.</span><span class="sxs-lookup"><span data-stu-id="81651-310">Application Gateway currently has a metric of "throughput", which can be configured tooalert.</span></span> <span data-ttu-id="81651-311">További információ a riasztások, toolearn látogasson el [riasztási értesítéseket](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="81651-311">toolearn more about alerts, visit [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>

<span data-ttu-id="81651-312">**Q. Háttér állapotfigyelő adja vissza állapota ismeretlen, mi okozza ezt az állapotot?**</span><span class="sxs-lookup"><span data-stu-id="81651-312">**Q. Backend health returns unknown status, what could be causing this status?**</span></span>

<span data-ttu-id="81651-313">hello leggyakoribb oka hozzáférés toohello háttér egy NSG-t vagy egyéni DNS-megjelenítését blokkolják.</span><span class="sxs-lookup"><span data-stu-id="81651-313">hello most common reason is access toohello backend is being blocked by an NSG or custom DNS.</span></span> <span data-ttu-id="81651-314">Látogasson el [háttér állapot, a diagnosztikai naplózás és a metrikák az Alkalmazásátjáró](application-gateway-diagnostics.md) további toolearn.</span><span class="sxs-lookup"><span data-stu-id="81651-314">Visit [Backend health, diagnostics logging, and metrics for Application Gateway](application-gateway-diagnostics.md) toolearn more.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81651-315">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81651-315">Next Steps</span></span>

<span data-ttu-id="81651-316">Alkalmazásátjáró olvashat toolearn [bemutatása tooApplication átjáró](application-gateway-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="81651-316">toolearn more about Application Gateway visit [Introduction tooApplication Gateway](application-gateway-introduction.md).</span></span>
