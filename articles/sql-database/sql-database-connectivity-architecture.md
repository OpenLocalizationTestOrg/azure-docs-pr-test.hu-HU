---
title: "SQL adatbázis-kapcsolat architektúra aaaAzure |} Microsoft Docs"
description: "Ez a dokumentum ismerteti a hello Azure SQLDB kapcsolat architektúra az Azure-ban vagy az Azure-on kívüli."
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/05/2017
ms.author: carlrab
ms.openlocfilehash: 917df6d88a16f1b841b617fb2a53025b4d14d034
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-connectivity-architecture"></a><span data-ttu-id="b95a1-103">Az Azure SQL Database kapcsolat architektúrája</span><span class="sxs-lookup"><span data-stu-id="b95a1-103">Azure SQL Database Connectivity Architecture</span></span> 

<span data-ttu-id="b95a1-104">Ez a cikk ismerteti a hello Azure SQL adatbázis-kapcsolat architektúra, és elmagyarázza, hogyan hello különböző összetevői működni toodirect forgalom tooyour példányát az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="b95a1-104">This article explains hello Azure SQL Database connectivity architecture and explains how hello different components function toodirect traffic tooyour instance of Azure SQL Database.</span></span> <span data-ttu-id="b95a1-105">Ezek az Azure SQL Database összetevőit működik toodirect hálózati forgalom toohello Azure-adatbázis, a csatlakozás az Azure ügyfelekkel és az Azure-on kívüli csatlakozó ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="b95a1-105">These Azure SQL Database connectivity components function toodirect network traffic toohello Azure database with clients connecting from within Azure and with clients connecting from outside of Azure.</span></span> <span data-ttu-id="b95a1-106">Ez a cikk a parancsfájl minták toochange kapcsolat módját is tartalmazza, és a hello szempontok kapcsolódó toochanging hello alapértelmezett kapcsolat beállításokat.</span><span class="sxs-lookup"><span data-stu-id="b95a1-106">This article also provides script samples toochange how connectivity occurs, and hello considerations related toochanging hello default connectivity settings.</span></span> <span data-ttu-id="b95a1-107">Ha kérdése van a cikk elolvasása után, forduljon a következő Dhruv dmalik@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="b95a1-107">If there are any questions after reading this article, please contact Dhruv at dmalik@microsoft.com.</span></span> 

## <a name="connectivity-architecture"></a><span data-ttu-id="b95a1-108">Kapcsolati architektúra</span><span class="sxs-lookup"><span data-stu-id="b95a1-108">Connectivity architecture</span></span>

<span data-ttu-id="b95a1-109">a következő diagram hello magas szintű áttekintést nyújt az Azure SQL adatbázis-kapcsolat architektúra hello.</span><span class="sxs-lookup"><span data-stu-id="b95a1-109">hello following diagram provides a high-level overview of hello Azure SQL Database connectivity architecture.</span></span> 

![architektúra áttekintése](./media/sql-database-connectivity-architecture/architecture-overview.png)


<span data-ttu-id="b95a1-111">hello következő lépések bemutatják, hogyan a kapcsolat azért hello Azure SQL Database szoftver betöltési-terheléselosztó Állapotfigyelője és hello Azure SQL Database átjárón keresztül létesített tooan Azure SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b95a1-111">hello following steps describe how a connection is established tooan Azure SQL database through hello Azure SQL Database software load-balancer (SLB) and hello Azure SQL Database gateway.</span></span>

- <span data-ttu-id="b95a1-112">Azure-ban vagy az Azure-on kívüli ügyfelek csatlakozni toohello SLB, amelyek egy nyilvános IP-címmel rendelkezik, és az 1433-as porton figyel.</span><span class="sxs-lookup"><span data-stu-id="b95a1-112">Clients within Azure or outside of Azure connect toohello SLB, which has a public IP address and listens on port 1433.</span></span>
- <span data-ttu-id="b95a1-113">hello SLB irányítja a forgalmat toohello Azure SQL Database átjáró.</span><span class="sxs-lookup"><span data-stu-id="b95a1-113">hello SLB directs traffic toohello Azure SQL Database gateway.</span></span>
- <span data-ttu-id="b95a1-114">hello átjáró hello forgalom toohello megfelelő proxy köztes irányítja át.</span><span class="sxs-lookup"><span data-stu-id="b95a1-114">hello gateway redirects hello traffic toohello correct proxy middleware.</span></span>
- <span data-ttu-id="b95a1-115">hello proxy köztes hello forgalom toohello megfelelő Azure SQL adatbázis irányítja át.</span><span class="sxs-lookup"><span data-stu-id="b95a1-115">hello proxy middleware redirects hello traffic toohello appropriate Azure SQL database.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b95a1-116">Ezeket az összetevőket tartalmaz elosztott szolgáltatásmegtagadásos (DDoS-) szolgáltatás védelmi beépített hello hálózati és hello app réteget.</span><span class="sxs-lookup"><span data-stu-id="b95a1-116">Each of these components has distributed denial of service (DDoS) protection built-in at hello network and hello app layer.</span></span>
>

## <a name="connectivity-from-within-azure"></a><span data-ttu-id="b95a1-117">Kapcsolat az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="b95a1-117">Connectivity from within Azure</span></span>

<span data-ttu-id="b95a1-118">Ha csatlakozik Azure-ban, a kapcsolatokat. a kapcsolat házirenddel rendelkezhetnek a **átirányítási** alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b95a1-118">If you are connecting from within Azure, your connections have a connection policy of **Redirect** by default.</span></span> <span data-ttu-id="b95a1-119">A házirend a **átirányítási** azt jelenti, hogy kapcsolatok hello TCP-munkamenet végeztével a meghatározott toohello Azure SQL database, hello ügyfélmunkamenethez majd toohello proxy köztes módosítása toohello céljának virtuális IP-származó átirányítva amely hello proxy köztes "hello Azure SQL Database" átjáró toothat.</span><span class="sxs-lookup"><span data-stu-id="b95a1-119">A policy of **Redirect** means that connections after hello TCP session is established toohello Azure SQL database, hello client session is then redirected toohello proxy middleware with a change toohello destination virtual IP from that of hello Azure SQL Database gateway toothat of hello proxy middleware.</span></span> <span data-ttu-id="b95a1-120">Ezt követően minden ezt követő csomagok flow hello proxy köztes hello Azure SQL Database átjáró megkerülésével közvetlenül keresztül.</span><span class="sxs-lookup"><span data-stu-id="b95a1-120">Thereafter, all subsequent packets flow directly via hello proxy middleware, bypassing hello Azure SQL Database gateway.</span></span> <span data-ttu-id="b95a1-121">hello a következő ábra szemlélteti a forgalom áramlását.</span><span class="sxs-lookup"><span data-stu-id="b95a1-121">hello following diagram illustrates this traffic flow.</span></span>

![architektúra áttekintése](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a><span data-ttu-id="b95a1-123">Azure-on kívüli kapcsolat</span><span class="sxs-lookup"><span data-stu-id="b95a1-123">Connectivity from outside of Azure</span></span>

<span data-ttu-id="b95a1-124">Ha a külső Azure kapcsolódik, a kapcsolatokat. a kapcsolat házirenddel rendelkezhetnek a **Proxy** alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="b95a1-124">If you are connecting from outside Azure, your connections have a connection policy of **Proxy** by default.</span></span> <span data-ttu-id="b95a1-125">A házirend a **Proxy** azt jelenti, hogy hello TCP munkamenet hello Azure SQL Database átjárón keresztül, és minden későbbi csomagok flow keresztül hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="b95a1-125">A policy of **Proxy** means that hello TCP session is established via hello Azure SQL Database gateway and all subsequent packets flow via hello gateway.</span></span> <span data-ttu-id="b95a1-126">hello a következő ábra szemlélteti a forgalom áramlását.</span><span class="sxs-lookup"><span data-stu-id="b95a1-126">hello following diagram illustrates this traffic flow.</span></span>

![architektúra áttekintése](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a><span data-ttu-id="b95a1-128">Az Azure SQL Database átjáró IP-címek</span><span class="sxs-lookup"><span data-stu-id="b95a1-128">Azure SQL Database gateway IP addresses</span></span>

<span data-ttu-id="b95a1-129">tooconnect tooan Azure SQL-adatbázis a helyszíni erőforrások, szükség van tooallow kimenő hálózati forgalom toohello Azure SQL Database átjáró az Azure-régiót.</span><span class="sxs-lookup"><span data-stu-id="b95a1-129">tooconnect tooan Azure SQL database from on-premises resources, you need tooallow outbound network traffic toohello Azure SQL Database gateway for your Azure region.</span></span> <span data-ttu-id="b95a1-130">A kapcsolatok csak lépjen hello átjárón keresztül Proxy módban, amely hello alapértelmezett esetén a helyszíni erőforrások csatlakoztatása kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="b95a1-130">Your connections only go via hello gateway when connecting in Proxy mode, which is hello default when connecting from on-premises resources.</span></span>

<span data-ttu-id="b95a1-131">a következő táblázat hello hello elsődleges és másodlagos IP-cím, az összes adatterületek hello Azure SQL Database-átjáró.</span><span class="sxs-lookup"><span data-stu-id="b95a1-131">hello following table lists hello primary and secondary IPs of hello Azure SQL Database gateway for all data regions.</span></span> <span data-ttu-id="b95a1-132">Egyes régiókhoz a rendszer két IP-cím.</span><span class="sxs-lookup"><span data-stu-id="b95a1-132">For some regions, there are two IP addresses.</span></span> <span data-ttu-id="b95a1-133">Ezeken a területeken hello elsődleges IP-címét az aktuális IP-cím hello hello átjáró és hello második IP-cím a feladatátvételi IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b95a1-133">In these regions, hello primary IP address is hello current IP address of hello gateway and hello second IP address is a failover IP address.</span></span> <span data-ttu-id="b95a1-134">hello feladatátvételi címe hello cím toowhich előfordulhat, hogy helyezze át a kiszolgáló tookeep hello szolgáltatás rendelkezésre állása magas, azt.</span><span class="sxs-lookup"><span data-stu-id="b95a1-134">hello failover address is hello address toowhich we might move your server tookeep hello service availability high.</span></span> <span data-ttu-id="b95a1-135">Ezek a régiókban javasoljuk, hogy engedélyezi a kimenő tooboth hello IP-címek.</span><span class="sxs-lookup"><span data-stu-id="b95a1-135">For these regions, we recommend that you allow outbound tooboth hello IP addresses.</span></span> <span data-ttu-id="b95a1-136">hello második IP-cím a Microsoft tulajdonában van, és nem figyel a szolgáltatások által az Azure SQL Database tooaccept kapcsolatok aktiválásáig.</span><span class="sxs-lookup"><span data-stu-id="b95a1-136">hello second IP address is owned by Microsoft and does not listen in on any services until it is activated by Azure SQL Database tooaccept connections.</span></span>

| <span data-ttu-id="b95a1-137">Régió neve</span><span class="sxs-lookup"><span data-stu-id="b95a1-137">Region Name</span></span> | <span data-ttu-id="b95a1-138">Elsődleges IP-cím</span><span class="sxs-lookup"><span data-stu-id="b95a1-138">Primary IP address</span></span> | <span data-ttu-id="b95a1-139">Másodlagos IP-cím</span><span class="sxs-lookup"><span data-stu-id="b95a1-139">Secondary IP address</span></span> |
| --- | --- |--- |
| <span data-ttu-id="b95a1-140">Kelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="b95a1-140">Australia East</span></span> | <span data-ttu-id="b95a1-141">191.238.66.109</span><span class="sxs-lookup"><span data-stu-id="b95a1-141">191.238.66.109</span></span> | <span data-ttu-id="b95a1-142">13.75.149.87</span><span class="sxs-lookup"><span data-stu-id="b95a1-142">13.75.149.87</span></span> |
| <span data-ttu-id="b95a1-143">Délkelet-Ausztrália</span><span class="sxs-lookup"><span data-stu-id="b95a1-143">Australia South East</span></span> | <span data-ttu-id="b95a1-144">191.239.192.109</span><span class="sxs-lookup"><span data-stu-id="b95a1-144">191.239.192.109</span></span> | <span data-ttu-id="b95a1-145">13.73.109.251</span><span class="sxs-lookup"><span data-stu-id="b95a1-145">13.73.109.251</span></span> |
| <span data-ttu-id="b95a1-146">Dél-Brazília</span><span class="sxs-lookup"><span data-stu-id="b95a1-146">Brazil South</span></span> | <span data-ttu-id="b95a1-147">104.41.11.5</span><span class="sxs-lookup"><span data-stu-id="b95a1-147">104.41.11.5</span></span> | |    
| <span data-ttu-id="b95a1-148">Közép-Kanada</span><span class="sxs-lookup"><span data-stu-id="b95a1-148">Canada Central</span></span> | <span data-ttu-id="b95a1-149">40.85.224.249</span><span class="sxs-lookup"><span data-stu-id="b95a1-149">40.85.224.249</span></span> | |    
| <span data-ttu-id="b95a1-150">Kelet-Kanada</span><span class="sxs-lookup"><span data-stu-id="b95a1-150">Canada East</span></span> | <span data-ttu-id="b95a1-151">40.86.226.166</span><span class="sxs-lookup"><span data-stu-id="b95a1-151">40.86.226.166</span></span> | |
| <span data-ttu-id="b95a1-152">USA középső régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-152">Central US</span></span> | <span data-ttu-id="b95a1-153">23.99.160.139</span><span class="sxs-lookup"><span data-stu-id="b95a1-153">23.99.160.139</span></span> | <span data-ttu-id="b95a1-154">13.67.215.62</span><span class="sxs-lookup"><span data-stu-id="b95a1-154">13.67.215.62</span></span> |
| <span data-ttu-id="b95a1-155">Kelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="b95a1-155">East Asia</span></span> | <span data-ttu-id="b95a1-156">191.234.2.139</span><span class="sxs-lookup"><span data-stu-id="b95a1-156">191.234.2.139</span></span> | <span data-ttu-id="b95a1-157">52.175.33.150</span><span class="sxs-lookup"><span data-stu-id="b95a1-157">52.175.33.150</span></span> |
| <span data-ttu-id="b95a1-158">1 USA keleti régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-158">East US 1</span></span> | <span data-ttu-id="b95a1-159">191.238.6.43</span><span class="sxs-lookup"><span data-stu-id="b95a1-159">191.238.6.43</span></span> | <span data-ttu-id="b95a1-160">40.121.158.30</span><span class="sxs-lookup"><span data-stu-id="b95a1-160">40.121.158.30</span></span> |
| <span data-ttu-id="b95a1-161">USA 2. keleti régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-161">East US 2</span></span> | <span data-ttu-id="b95a1-162">191.239.224.107</span><span class="sxs-lookup"><span data-stu-id="b95a1-162">191.239.224.107</span></span> | <span data-ttu-id="b95a1-163">40.79.84.180</span><span class="sxs-lookup"><span data-stu-id="b95a1-163">40.79.84.180</span></span> |
| <span data-ttu-id="b95a1-164">Közép-India</span><span class="sxs-lookup"><span data-stu-id="b95a1-164">India Central</span></span> | <span data-ttu-id="b95a1-165">104.211.96.159</span><span class="sxs-lookup"><span data-stu-id="b95a1-165">104.211.96.159</span></span>  | |   
| <span data-ttu-id="b95a1-166">Dél-India</span><span class="sxs-lookup"><span data-stu-id="b95a1-166">India South</span></span> | <span data-ttu-id="b95a1-167">104.211.224.146</span><span class="sxs-lookup"><span data-stu-id="b95a1-167">104.211.224.146</span></span>  | |
| <span data-ttu-id="b95a1-168">Nyugat-India</span><span class="sxs-lookup"><span data-stu-id="b95a1-168">India West</span></span> | <span data-ttu-id="b95a1-169">104.211.160.80</span><span class="sxs-lookup"><span data-stu-id="b95a1-169">104.211.160.80</span></span> | |
| <span data-ttu-id="b95a1-170">Kelet-Japán</span><span class="sxs-lookup"><span data-stu-id="b95a1-170">Japan East</span></span> | <span data-ttu-id="b95a1-171">191.237.240.43</span><span class="sxs-lookup"><span data-stu-id="b95a1-171">191.237.240.43</span></span> | <span data-ttu-id="b95a1-172">13.78.61.196</span><span class="sxs-lookup"><span data-stu-id="b95a1-172">13.78.61.196</span></span> |
| <span data-ttu-id="b95a1-173">Nyugat-Japán</span><span class="sxs-lookup"><span data-stu-id="b95a1-173">Japan West</span></span> | <span data-ttu-id="b95a1-174">191.238.68.11</span><span class="sxs-lookup"><span data-stu-id="b95a1-174">191.238.68.11</span></span> | <span data-ttu-id="b95a1-175">104.214.148.156</span><span class="sxs-lookup"><span data-stu-id="b95a1-175">104.214.148.156</span></span> |
| <span data-ttu-id="b95a1-176">Korea középső régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-176">Korea Central</span></span> | <span data-ttu-id="b95a1-177">52.231.32.42</span><span class="sxs-lookup"><span data-stu-id="b95a1-177">52.231.32.42</span></span> | |
| <span data-ttu-id="b95a1-178">Korea déli régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-178">Korea South</span></span> | <span data-ttu-id="b95a1-179">52.231.200.86</span><span class="sxs-lookup"><span data-stu-id="b95a1-179">52.231.200.86</span></span> |  |
| <span data-ttu-id="b95a1-180">USA északi középső régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-180">North Central US</span></span> | <span data-ttu-id="b95a1-181">23.98.55.75</span><span class="sxs-lookup"><span data-stu-id="b95a1-181">23.98.55.75</span></span> | <span data-ttu-id="b95a1-182">23.96.178.199</span><span class="sxs-lookup"><span data-stu-id="b95a1-182">23.96.178.199</span></span> |
| <span data-ttu-id="b95a1-183">Észak-Európa</span><span class="sxs-lookup"><span data-stu-id="b95a1-183">North Europe</span></span> | <span data-ttu-id="b95a1-184">191.235.193.75</span><span class="sxs-lookup"><span data-stu-id="b95a1-184">191.235.193.75</span></span> | <span data-ttu-id="b95a1-185">40.113.93.91</span><span class="sxs-lookup"><span data-stu-id="b95a1-185">40.113.93.91</span></span> |
| <span data-ttu-id="b95a1-186">USA déli középső régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-186">South Central US</span></span> | <span data-ttu-id="b95a1-187">23.98.162.75</span><span class="sxs-lookup"><span data-stu-id="b95a1-187">23.98.162.75</span></span> | <span data-ttu-id="b95a1-188">13.66.62.124</span><span class="sxs-lookup"><span data-stu-id="b95a1-188">13.66.62.124</span></span> |
| <span data-ttu-id="b95a1-189">Délkelet-Ázsia</span><span class="sxs-lookup"><span data-stu-id="b95a1-189">South East Asia</span></span> | <span data-ttu-id="b95a1-190">23.100.117.95</span><span class="sxs-lookup"><span data-stu-id="b95a1-190">23.100.117.95</span></span> | <span data-ttu-id="b95a1-191">104.43.15.0</span><span class="sxs-lookup"><span data-stu-id="b95a1-191">104.43.15.0</span></span> |
| <span data-ttu-id="b95a1-192">Egyesült Királyság északi régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-192">UK North</span></span> | <span data-ttu-id="b95a1-193">13.87.97.210</span><span class="sxs-lookup"><span data-stu-id="b95a1-193">13.87.97.210</span></span> | |
| <span data-ttu-id="b95a1-194">Egyesült Királyság déli régiója 1</span><span class="sxs-lookup"><span data-stu-id="b95a1-194">UK South 1</span></span> | <span data-ttu-id="b95a1-195">51.140.184.11</span><span class="sxs-lookup"><span data-stu-id="b95a1-195">51.140.184.11</span></span> | |    
| <span data-ttu-id="b95a1-196">Egyesült Királyság 2. déli régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-196">UK South 2</span></span> | <span data-ttu-id="b95a1-197">13.87.34.7</span><span class="sxs-lookup"><span data-stu-id="b95a1-197">13.87.34.7</span></span> | |
| <span data-ttu-id="b95a1-198">Az Egyesült Királyság nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-198">UK West</span></span> | <span data-ttu-id="b95a1-199">51.141.8.11</span><span class="sxs-lookup"><span data-stu-id="b95a1-199">51.141.8.11</span></span>  | |
| <span data-ttu-id="b95a1-200">USA nyugati középső régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-200">West Central US</span></span> | <span data-ttu-id="b95a1-201">13.78.145.25</span><span class="sxs-lookup"><span data-stu-id="b95a1-201">13.78.145.25</span></span> | |
| <span data-ttu-id="b95a1-202">Nyugat-Európa</span><span class="sxs-lookup"><span data-stu-id="b95a1-202">West Europe</span></span> | <span data-ttu-id="b95a1-203">191.237.232.75</span><span class="sxs-lookup"><span data-stu-id="b95a1-203">191.237.232.75</span></span> | <span data-ttu-id="b95a1-204">40.68.37.158</span><span class="sxs-lookup"><span data-stu-id="b95a1-204">40.68.37.158</span></span> |
| <span data-ttu-id="b95a1-205">1 USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="b95a1-205">West US 1</span></span> | <span data-ttu-id="b95a1-206">23.99.34.75</span><span class="sxs-lookup"><span data-stu-id="b95a1-206">23.99.34.75</span></span> | <span data-ttu-id="b95a1-207">104.42.238.205</span><span class="sxs-lookup"><span data-stu-id="b95a1-207">104.42.238.205</span></span> |
| <span data-ttu-id="b95a1-208">USA nyugati régiója, 2.</span><span class="sxs-lookup"><span data-stu-id="b95a1-208">West US 2</span></span> | <span data-ttu-id="b95a1-209">13.66.226.202</span><span class="sxs-lookup"><span data-stu-id="b95a1-209">13.66.226.202</span></span>  | |
||||

## <a name="change-azure-sql-database-connection-policy"></a><span data-ttu-id="b95a1-210">Módosítsa a kapcsolatkezelési házirendet az Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="b95a1-210">Change Azure SQL Database connection policy</span></span>

<span data-ttu-id="b95a1-211">az Azure SQL Database kapcsolatkezelési házirendet az Azure SQL Database-kiszolgáló esetén használjon hello toochange hello [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span><span class="sxs-lookup"><span data-stu-id="b95a1-211">toochange hello Azure SQL Database connection policy for an Azure SQL Database server, use hello [REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span></span> 

- <span data-ttu-id="b95a1-212">Ha a kapcsolat házirend túl**Proxy**, az összes hálózati csomagok folyamata hello Azure SQL Database átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="b95a1-212">If your connection policy is set too**Proxy**, all network packets flow via hello Azure SQL Database gateway.</span></span> <span data-ttu-id="b95a1-213">Ezt a beállítást, a tooallow kimenő tooonly hello Azure SQL Database átjáró IP kell.</span><span class="sxs-lookup"><span data-stu-id="b95a1-213">For this setting, you need tooallow outbound tooonly hello Azure SQL Database gateway IP.</span></span> <span data-ttu-id="b95a1-214">Beállítás használatával **Proxy** rendelkezik további késést biztosít beállítását **átirányítási**.</span><span class="sxs-lookup"><span data-stu-id="b95a1-214">Using a setting of **Proxy** has more latency than a setting of **Redirect**.</span></span> 
- <span data-ttu-id="b95a1-215">Ha a kapcsolat szabályzatot állítja **átirányítási**, az összes hálózati csomag flow közvetlenül toohello köztes proxy.</span><span class="sxs-lookup"><span data-stu-id="b95a1-215">If your connection policy is setting **Redirect**, all network packets flow directly toohello middleware proxy.</span></span> <span data-ttu-id="b95a1-216">Ezt a beállítást kell tooallow kimenő toomultiple IP-címek.</span><span class="sxs-lookup"><span data-stu-id="b95a1-216">For this setting, you need tooallow outbound toomultiple IPs.</span></span> 

## <a name="script-toochange-connection-settings"></a><span data-ttu-id="b95a1-217">Parancsfájl toochange kapcsolatbeállításai</span><span class="sxs-lookup"><span data-stu-id="b95a1-217">Script toochange connection settings</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b95a1-218">Ez a parancsfájl szükséges hello [Azure PowerShell modul](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="b95a1-218">This script requires hello [Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span>
>

<span data-ttu-id="b95a1-219">a következő PowerShell-parancsfájl hello bemutatja, hogyan toochange hello kapcsolatkezelési házirendet.</span><span class="sxs-lookup"><span data-stu-id="b95a1-219">hello following PowerShell script shows how toochange hello connection policy.</span></span>

```powershell
import-module azureRm
Login-AzureRmAccount

$tenantId =  #your AAD tenant ID
$subscriptionId = #Azure SubscriptionID
$uri = #AAD uri
$authUrl = "https://login.microsoftonline.com/$tenantId"
$serverName = #sqldb server name 
$resourceGroupName=#sqldb resource group
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl

$result = $AuthContext.AcquireToken("https://management.core.windows.net/",
$clientId,
[Uri]$uri, 
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#getting hello current connection property
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#setting hello property too‘Proxy’
$connectionType=”Proxy” <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="next-steps"></a><span data-ttu-id="b95a1-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b95a1-220">Next steps</span></span>

- <span data-ttu-id="b95a1-221">Hogyan toochange hello Azure SQL Database kapcsolatkezelési házirendet az Azure SQL Database-kiszolgáló információkért lásd: [REST API létrehozása vagy frissítési kiszolgáló kapcsolatkezelési házirendet az hello](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span><span class="sxs-lookup"><span data-stu-id="b95a1-221">For information on how toochange hello Azure SQL Database connection policy for an Azure SQL Database server, see [Create or Update Server Connection Policy using hello REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).</span></span>
- <span data-ttu-id="b95a1-222">Információ az Azure SQL Database kapcsolat viselkedésről ADO.NET 4.5 vagy újabb verzióját használó ügyfelek számára, lásd: [kívüli ADO.NET 4.5 1433-as portokon](sql-database-develop-direct-route-ports-adonet-v12.md).</span><span class="sxs-lookup"><span data-stu-id="b95a1-222">For information about Azure SQL Database connection behavior for clients that use ADO.NET 4.5 or a later version, see [Ports beyond 1433 for ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).</span></span>
- <span data-ttu-id="b95a1-223">Általános alkalmazás fejlesztési, témakör [SQL adatbázis alkalmazás fejlesztői áttekintés](sql-database-develop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b95a1-223">For general application development overview information, see [SQL Database Application Development Overview](sql-database-develop-overview.md).</span></span>
