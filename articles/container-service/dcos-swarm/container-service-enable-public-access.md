---
title: "aaaEnable access tooAzure DC/OS tároló alkalmazásnak |} Microsoft Docs"
description: "Tooenable nyilvános miként férhetnek hozzá az Azure Tárolószolgáltatás tooDC/OS tárolók."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 1ba251ba5a176a6a5e1c7831655164e380a62b27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-public-access-tooan-azure-container-service-application"></a><span data-ttu-id="9dba1-104">Nyilvános hozzáférés tooan Azure Tárolószolgáltatás-alkalmazás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9dba1-104">Enable public access tooan Azure Container Service application</span></span>
<span data-ttu-id="9dba1-105">A DC/OS tárolóhoz hello ACS [nyilvános ügynök készlet](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) automatikusan elérhetővé toohello van internet.</span><span class="sxs-lookup"><span data-stu-id="9dba1-105">Any DC/OS container in hello ACS [public agent pool](container-service-mesos-marathon-ui.md#deploy-a-docker-formatted-container) is automatically exposed toohello internet.</span></span> <span data-ttu-id="9dba1-106">Alapértelmezés szerint portok **80-as**, **443-as**, **8080** nyitva vannak, és bármilyen (nyilvános) tároló, ezeket a portokat figyeli érhetők el.</span><span class="sxs-lookup"><span data-stu-id="9dba1-106">By default, ports **80**, **443**, **8080** are opened, and any (public) container listening on those ports are accessible.</span></span> <span data-ttu-id="9dba1-107">Ez a cikk bemutatja, hogyan tooopen több portok az alkalmazások az Azure Tárolószolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="9dba1-107">This article shows you how tooopen more ports for your applications in Azure Container Service.</span></span>

## <a name="open-a-port-portal"></a><span data-ttu-id="9dba1-108">Nyisson meg egy portot (portál)</span><span class="sxs-lookup"><span data-stu-id="9dba1-108">Open a port (portal)</span></span>
<span data-ttu-id="9dba1-109">Először létre kell azt szeretnénk, ha tooopen hello port.</span><span class="sxs-lookup"><span data-stu-id="9dba1-109">First, we need tooopen hello port we want.</span></span>

1. <span data-ttu-id="9dba1-110">Jelentkezzen be toohello portálon.</span><span class="sxs-lookup"><span data-stu-id="9dba1-110">Log in toohello portal.</span></span>
2. <span data-ttu-id="9dba1-111">Keresés hello erőforráscsoport központilag telepített hello Azure Tárolószolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="9dba1-111">Find hello resource group that you deployed hello Azure Container Service to.</span></span>
3. <span data-ttu-id="9dba1-112">Válassza ki az ügynök terheléselosztó hello (amely nevű hasonló túl**XXXX-ügynök-lb-XXXX**).</span><span class="sxs-lookup"><span data-stu-id="9dba1-112">Select hello agent load balancer (which is named similar too**XXXX-agent-lb-XXXX**).</span></span>
   
    ![Azure-tárolót szolgáltatás terheléselosztó](./media/container-service-enable-public-access/agent-load-balancer.png)
4. <span data-ttu-id="9dba1-114">Kattintson a **vizsgálat** , majd **hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="9dba1-114">Click **Probes** and then **Add**.</span></span>
   
    ![Azure-tárolót szolgáltatás terheléselosztó-mintavétel](./media/container-service-enable-public-access/add-probe.png)
5. <span data-ttu-id="9dba1-116">Töltse ki hello mintavételi űrlapot, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="9dba1-116">Fill out hello probe form and click **OK**.</span></span>
   
   | <span data-ttu-id="9dba1-117">Mező</span><span class="sxs-lookup"><span data-stu-id="9dba1-117">Field</span></span> | <span data-ttu-id="9dba1-118">Leírás</span><span class="sxs-lookup"><span data-stu-id="9dba1-118">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="9dba1-119">Név</span><span class="sxs-lookup"><span data-stu-id="9dba1-119">Name</span></span> |<span data-ttu-id="9dba1-120">Hello mintavétel egy leíró nevet.</span><span class="sxs-lookup"><span data-stu-id="9dba1-120">A descriptive name of hello probe.</span></span> |
   | <span data-ttu-id="9dba1-121">Port</span><span class="sxs-lookup"><span data-stu-id="9dba1-121">Port</span></span> |<span data-ttu-id="9dba1-122">hello tároló tootest hello portja.</span><span class="sxs-lookup"><span data-stu-id="9dba1-122">hello port of hello container tootest.</span></span> |
   | <span data-ttu-id="9dba1-123">Elérési út</span><span class="sxs-lookup"><span data-stu-id="9dba1-123">Path</span></span> |<span data-ttu-id="9dba1-124">(Ha HTTP módban) relatív webhely elérési útja tooprobe hello.</span><span class="sxs-lookup"><span data-stu-id="9dba1-124">(When in HTTP mode) hello relative website path tooprobe.</span></span> <span data-ttu-id="9dba1-125">A HTTPS nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="9dba1-125">HTTPS not supported.</span></span> |
   | <span data-ttu-id="9dba1-126">időköz</span><span class="sxs-lookup"><span data-stu-id="9dba1-126">Interval</span></span> |<span data-ttu-id="9dba1-127">hello időn közötti mintavételi kísérletek, másodpercben.</span><span class="sxs-lookup"><span data-stu-id="9dba1-127">hello amount of time between probe attempts, in seconds.</span></span> |
   | <span data-ttu-id="9dba1-128">Sérült küszöbérték</span><span class="sxs-lookup"><span data-stu-id="9dba1-128">Unhealthy threshold</span></span> |<span data-ttu-id="9dba1-129">Próbálkozások száma egymást követő mintavétel az előtt annak eldöntéséhez, hogy hello tároló nem kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="9dba1-129">Number of consecutive probe attempts before considering hello container unhealthy.</span></span> |
6. <span data-ttu-id="9dba1-130">Eközben hello ügynök terheléselosztójának hello tulajdonságait, kattintson a **terheléselosztási szabályok** , majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9dba1-130">Back at hello properties of hello agent load balancer, click **Load balancing rules** and then **Add**.</span></span>
   
    ![Azure-tárolót szolgáltatás terheléselosztási szabály](./media/container-service-enable-public-access/add-balancer-rule.png)
7. <span data-ttu-id="9dba1-132">Töltse ki hello load balancer űrlapot, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="9dba1-132">Fill out hello load balancer form and click **OK**.</span></span>
   
   | <span data-ttu-id="9dba1-133">Mező</span><span class="sxs-lookup"><span data-stu-id="9dba1-133">Field</span></span> | <span data-ttu-id="9dba1-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="9dba1-134">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="9dba1-135">Név</span><span class="sxs-lookup"><span data-stu-id="9dba1-135">Name</span></span> |<span data-ttu-id="9dba1-136">Hello terheléselosztó egy leíró nevet.</span><span class="sxs-lookup"><span data-stu-id="9dba1-136">A descriptive name of hello load balancer.</span></span> |
   | <span data-ttu-id="9dba1-137">Port</span><span class="sxs-lookup"><span data-stu-id="9dba1-137">Port</span></span> |<span data-ttu-id="9dba1-138">hello nyilvános bejövő portot.</span><span class="sxs-lookup"><span data-stu-id="9dba1-138">hello public incoming port.</span></span> |
   | <span data-ttu-id="9dba1-139">Háttér-port</span><span class="sxs-lookup"><span data-stu-id="9dba1-139">Backend port</span></span> |<span data-ttu-id="9dba1-140">hello belső nyilvános port hello tároló tooroute forgalom számára.</span><span class="sxs-lookup"><span data-stu-id="9dba1-140">hello internal-public port of hello container tooroute traffic to.</span></span> |
   | <span data-ttu-id="9dba1-141">Háttérkészlet</span><span class="sxs-lookup"><span data-stu-id="9dba1-141">Backend pool</span></span> |<span data-ttu-id="9dba1-142">Ebben a készletben hello tárolók lesz hello célja a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="9dba1-142">hello containers in this pool will be hello target for this load balancer.</span></span> |
   | <span data-ttu-id="9dba1-143">Hálózatfigyelő</span><span class="sxs-lookup"><span data-stu-id="9dba1-143">Probe</span></span> |<span data-ttu-id="9dba1-144">hello használt mintavételi toodetermine, ha a célként a hello **háttérkészlet** működik megfelelően.</span><span class="sxs-lookup"><span data-stu-id="9dba1-144">hello probe used toodetermine if a target in hello **Backend pool** is healthy.</span></span> |
   | <span data-ttu-id="9dba1-145">Munkamenet megőrzését</span><span class="sxs-lookup"><span data-stu-id="9dba1-145">Session persistence</span></span> |<span data-ttu-id="9dba1-146">Meghatározza, hogy egy ügyféltől érkező forgalmat hello munkamenet idejére hello kezelésének módját.</span><span class="sxs-lookup"><span data-stu-id="9dba1-146">Determines how traffic from a client should be handled for hello duration of hello session.</span></span><br><br><span data-ttu-id="9dba1-147">**Nincs**: hello ugyanazt az ügyfelet bármely tárolóban kell kezelnie az egymást követő kéréseket.</span><span class="sxs-lookup"><span data-stu-id="9dba1-147">**None**: Successive requests from hello same client can be handled by any container.</span></span><br><span data-ttu-id="9dba1-148">**Ügyfél IP**: hello azonos ügyfél IP kezeli az egymást követő kéréseket hello ugyanabban a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="9dba1-148">**Client IP**: Successive requests from hello same client IP are handled by hello same container.</span></span><br><span data-ttu-id="9dba1-149">**Ügyfél IP protokoll és**: hello azonos ügyfél IP-protokoll kombináció kezeli az egymást követő kéréseket hello ugyanabban a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="9dba1-149">**Client IP and protocol**: Successive requests from hello same client IP and protocol combination are handled by hello same container.</span></span> |
   | <span data-ttu-id="9dba1-150">Üresjárati időtúllépés</span><span class="sxs-lookup"><span data-stu-id="9dba1-150">Idle timeout</span></span> |<span data-ttu-id="9dba1-151">(Csak a TCP) (Percben), a TCP/HTTP-ügyfél nyissa meg a anélkül, hogy az idő tookeep hello *életben tartási* üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="9dba1-151">(TCP only) In minutes, hello time tookeep a TCP/HTTP client open without relying on *keep-alive* messages.</span></span> |

## <a name="add-a-security-rule-portal"></a><span data-ttu-id="9dba1-152">Adja hozzá a biztonsági szabály (portál)</span><span class="sxs-lookup"><span data-stu-id="9dba1-152">Add a security rule (portal)</span></span>
<span data-ttu-id="9dba1-153">Ezt követően kell tooadd egy biztonsági szabályt, amely a megnyitott portról hello tűzfalon keresztül irányítja a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="9dba1-153">Next, we need tooadd a security rule that routes traffic from our opened port through hello firewall.</span></span>

1. <span data-ttu-id="9dba1-154">Jelentkezzen be toohello portálon.</span><span class="sxs-lookup"><span data-stu-id="9dba1-154">Log in toohello portal.</span></span>
2. <span data-ttu-id="9dba1-155">Keresés hello erőforráscsoport központilag telepített hello Azure Tárolószolgáltatás számára.</span><span class="sxs-lookup"><span data-stu-id="9dba1-155">Find hello resource group that you deployed hello Azure Container Service to.</span></span>
3. <span data-ttu-id="9dba1-156">Jelölje be hello **nyilvános** ügynök hálózati biztonsági csoport (amely nevű hasonló túl**XXXX-ügynök – nyilvános-nsg-XXXX**).</span><span class="sxs-lookup"><span data-stu-id="9dba1-156">Select hello **public** agent network security group (which is named similar too**XXXX-agent-public-nsg-XXXX**).</span></span>
   
    ![Azure-tárolót szolgáltatás hálózati biztonsági csoport](./media/container-service-enable-public-access/agent-nsg.png)
4. <span data-ttu-id="9dba1-158">Válassza ki **bejövő biztonsági szabályok** , majd **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="9dba1-158">Select **Inbound security rules** and then **Add**.</span></span>
   
    ![Azure-tárolót szolgáltatás hálózati biztonsági csoportszabályok](./media/container-service-enable-public-access/add-firewall-rule.png)
5. <span data-ttu-id="9dba1-160">A nyilvános port hello tűzfal szabály tooallow kitöltése, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="9dba1-160">Fill out hello firewall rule tooallow your public port and click **OK**.</span></span>
   
   | <span data-ttu-id="9dba1-161">Mező</span><span class="sxs-lookup"><span data-stu-id="9dba1-161">Field</span></span> | <span data-ttu-id="9dba1-162">Leírás</span><span class="sxs-lookup"><span data-stu-id="9dba1-162">Description</span></span> |
   | --- | --- |
   | <span data-ttu-id="9dba1-163">Név</span><span class="sxs-lookup"><span data-stu-id="9dba1-163">Name</span></span> |<span data-ttu-id="9dba1-164">Egy tűzfalszabály hello leíró nevet.</span><span class="sxs-lookup"><span data-stu-id="9dba1-164">A descriptive name of hello firewall rule.</span></span> |
   | <span data-ttu-id="9dba1-165">Prioritás</span><span class="sxs-lookup"><span data-stu-id="9dba1-165">Priority</span></span> |<span data-ttu-id="9dba1-166">Prioritás dimenziószáma hello szabályhoz.</span><span class="sxs-lookup"><span data-stu-id="9dba1-166">Priority rank for hello rule.</span></span> <span data-ttu-id="9dba1-167">hello alacsonyabb hello számú hello hello elsőbbséget.</span><span class="sxs-lookup"><span data-stu-id="9dba1-167">hello lower hello number hello higher hello priority.</span></span> |
   | <span data-ttu-id="9dba1-168">Forrás</span><span class="sxs-lookup"><span data-stu-id="9dba1-168">Source</span></span> |<span data-ttu-id="9dba1-169">Korlátozza a hello bejövő IP cím tartomány toobe amelyet e szabály engedélyez vagy elutasít.</span><span class="sxs-lookup"><span data-stu-id="9dba1-169">Restrict hello incoming IP address range toobe allowed or denied by this rule.</span></span> <span data-ttu-id="9dba1-170">Használjon **bármely** toonot korlátozás adja meg.</span><span class="sxs-lookup"><span data-stu-id="9dba1-170">Use **Any** toonot specify a restriction.</span></span> |
   | <span data-ttu-id="9dba1-171">Szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="9dba1-171">Service</span></span> |<span data-ttu-id="9dba1-172">Válassza ki a biztonsági szabály csak az előre meghatározott szolgáltatásokat foglal.</span><span class="sxs-lookup"><span data-stu-id="9dba1-172">Select a set of predefined services this security rule is for.</span></span> <span data-ttu-id="9dba1-173">Ellenkező esetben használja **egyéni** toocreate saját.</span><span class="sxs-lookup"><span data-stu-id="9dba1-173">Otherwise use **Custom** toocreate your own.</span></span> |
   | <span data-ttu-id="9dba1-174">Protokoll</span><span class="sxs-lookup"><span data-stu-id="9dba1-174">Protocol</span></span> |<span data-ttu-id="9dba1-175">Korlátozzák a forgalmat a alapján **TCP** vagy **UDP**.</span><span class="sxs-lookup"><span data-stu-id="9dba1-175">Restrict traffic based on **TCP** or **UDP**.</span></span> <span data-ttu-id="9dba1-176">Használjon **bármely** toonot korlátozás adja meg.</span><span class="sxs-lookup"><span data-stu-id="9dba1-176">Use **Any** toonot specify a restriction.</span></span> |
   | <span data-ttu-id="9dba1-177">Porttartomány</span><span class="sxs-lookup"><span data-stu-id="9dba1-177">Port range</span></span> |<span data-ttu-id="9dba1-178">Ha **szolgáltatás** van **egyéni**, adja meg a hello azon portok tartományát, ez a szabály vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="9dba1-178">When **Service** is **Custom**, specifies hello range of ports that this rule affects.</span></span> <span data-ttu-id="9dba1-179">Használhatja például egyetlen port, **80**, vagy egy tartományt, például **1024-1500**.</span><span class="sxs-lookup"><span data-stu-id="9dba1-179">You can use a single port, such as **80**, or a range like **1024-1500**.</span></span> |
   | <span data-ttu-id="9dba1-180">Műveletek</span><span class="sxs-lookup"><span data-stu-id="9dba1-180">Action</span></span> |<span data-ttu-id="9dba1-181">Adatforgalom engedélyezéséhez vagy letiltásához hello feltételeknek megfelelő.</span><span class="sxs-lookup"><span data-stu-id="9dba1-181">Allow or deny traffic that meets hello criteria.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="9dba1-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9dba1-182">Next steps</span></span>
<span data-ttu-id="9dba1-183">További tudnivalók: hello különbségének [nyilvános és titkos DC/OS-ügynökök](container-service-dcos-agents.md).</span><span class="sxs-lookup"><span data-stu-id="9dba1-183">Learn about hello difference between [public and private DC/OS agents](container-service-dcos-agents.md).</span></span>

<span data-ttu-id="9dba1-184">További információt olvashat [a DC/OS-tárolók kezelése](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="9dba1-184">Read more information about [managing your DC/OS containers](container-service-mesos-marathon-ui.md).</span></span>

