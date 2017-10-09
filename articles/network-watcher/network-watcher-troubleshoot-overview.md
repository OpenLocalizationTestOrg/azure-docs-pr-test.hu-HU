---
title: "hibaelhárítás a Azure hálózati figyelőt aaaIntroduction tooresource |} Microsoft Docs"
description: "Ezen a lapon hello hálózati figyelőt erőforrás hibaelhárítási képességei áttekintése"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: ccbe4c1c2364473aba06e709460d67c773cf25ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooresource-troubleshooting-in-azure-network-watcher"></a><span data-ttu-id="41c79-103">Hibaelhárítás a Azure hálózati figyelőt bemutatása tooresource</span><span class="sxs-lookup"><span data-stu-id="41c79-103">Introduction tooresource troubleshooting in Azure Network Watcher</span></span>

<span data-ttu-id="41c79-104">Virtuális hálózati átjárók biztosít a helyi erőforrások és más Azure-ban virtuális hálózatok közötti kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="41c79-104">Virtual Network Gateways provide connectivity between on-premises resources and other virtual networks within Azure.</span></span> <span data-ttu-id="41c79-105">Ezek átjárók és a kapcsolatok figyelés létfontosságú tooensuring kommunikáció nem megszakad.</span><span class="sxs-lookup"><span data-stu-id="41c79-105">Monitoring these gateways and their Connections is critical tooensuring communication is not broken.</span></span> <span data-ttu-id="41c79-106">Hálózati figyelőt hello funkció tootroubleshoot biztosít a virtuális hálózati átjárók és kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="41c79-106">Network Watcher provides hello capability tootroubleshoot Virtual Network Gateways and Connections.</span></span> <span data-ttu-id="41c79-107">Ez hello portál, a PowerShell, a CLI vagy a REST API-n keresztül kell meghívni.</span><span class="sxs-lookup"><span data-stu-id="41c79-107">This can be called through hello portal, PowerShell, CLI, or REST API.</span></span> <span data-ttu-id="41c79-108">Meghívásakor, a hálózati figyelőt diagnosztizálja a virtuális hálózati átjáró hello vagy a kapcsolat és a visszatérési hello megfelelő eredmények hello állapotának.</span><span class="sxs-lookup"><span data-stu-id="41c79-108">When called, Network Watcher diagnoses hello health of hello virtual network gateway or connection and return hello appropriate results.</span></span> <span data-ttu-id="41c79-109">A kérelem egy hosszú ideig futó tranzakció, hello eredményeinek hello diagnosztikai végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="41c79-109">This request is a long running transaction, hello results are returned once hello diagnosis is complete.</span></span>

![portal][2]

## <a name="results"></a><span data-ttu-id="41c79-111">Results (Eredmények)</span><span class="sxs-lookup"><span data-stu-id="41c79-111">Results</span></span>

<span data-ttu-id="41c79-112">hello előzetes eredményének hello rendszerállapot hello erőforrás átfogó képet ad.</span><span class="sxs-lookup"><span data-stu-id="41c79-112">hello preliminary results returned give an overall picture of hello health of hello resource.</span></span> <span data-ttu-id="41c79-113">Mélyebb információk biztosítható, hogy az erőforrások hello a következő szakaszban ismertetett módon:</span><span class="sxs-lookup"><span data-stu-id="41c79-113">Deeper information can be provided for resources as shown in hello following section:</span></span>

<span data-ttu-id="41c79-114">hello alábbi lista a következő hello értékeket ad vissza, amelyben hello API hibáinak elhárítása:</span><span class="sxs-lookup"><span data-stu-id="41c79-114">hello following list is hello values returned with hello troubleshoot API:</span></span>

* <span data-ttu-id="41c79-115">**startTime** -ezt az értéket az hello idő hello hibaelhárítása API-hívások indítása.</span><span class="sxs-lookup"><span data-stu-id="41c79-115">**startTime** - This value is hello time hello troubleshoot API call started.</span></span>
* <span data-ttu-id="41c79-116">**endTime** -ezt az értéket az hello idő, amikor hello hibaelhárítás befejeződött.</span><span class="sxs-lookup"><span data-stu-id="41c79-116">**endTime** - This value is hello time when hello troubleshooting ended.</span></span>
* <span data-ttu-id="41c79-117">**kód** -ezt az értéket nem kifogástalan állapotú, akkor, ha sikertelen egy egyetlen elemzés céljából.</span><span class="sxs-lookup"><span data-stu-id="41c79-117">**code** - This value is UnHealthy, if there is a single diagnosis failure.</span></span>
* <span data-ttu-id="41c79-118">**eredmények** -eredmények hello kapcsolat vagy hello virtuális hálózati átjáró visszaadott eredmények gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="41c79-118">**results** - Results is a collection of results returned on hello Connection or hello virtual network gateway.</span></span>
    * <span data-ttu-id="41c79-119">**azonosító** -hello hibatípus értéke.</span><span class="sxs-lookup"><span data-stu-id="41c79-119">**id** - This value is hello fault type.</span></span>
    * <span data-ttu-id="41c79-120">**összegző** -e értéke hello tartalék összegzését.</span><span class="sxs-lookup"><span data-stu-id="41c79-120">**summary** - This value is a summary of hello fault.</span></span>
    * <span data-ttu-id="41c79-121">**részletes** -ezt az értéket hello hiba részletes leírását tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41c79-121">**detailed** - This value provides a detailed description of hello fault.</span></span>
    * <span data-ttu-id="41c79-122">**recommendedActions** – Ez a tulajdonság műveleteket tootake gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="41c79-122">**recommendedActions** - This property is a collection of recommended actions tootake.</span></span>
      * <span data-ttu-id="41c79-123">**actionText** -Ez az érték milyen művelet tootake leíró hello szöveget tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="41c79-123">**actionText** - This value contains hello text describing what action tootake.</span></span>
      * <span data-ttu-id="41c79-124">**actionUri** -Ez az érték szolgál az hello URI toodocumentation tooact.</span><span class="sxs-lookup"><span data-stu-id="41c79-124">**actionUri** - This value provides hello URI toodocumentation on how tooact.</span></span>
      * <span data-ttu-id="41c79-125">**actionUriText** -e értéke hello művelet szöveg rövid leírása.</span><span class="sxs-lookup"><span data-stu-id="41c79-125">**actionUriText** - This value is a short description of hello action text.</span></span>

<span data-ttu-id="41c79-126">hello a következő táblák megjelenítése hello különböző tartalék típusok (azonosító: hello listát megelőző eredményei alapján), és ha hello hibát hoz létre a naplókat.</span><span class="sxs-lookup"><span data-stu-id="41c79-126">hello following tables show hello different fault types (id under results from hello preceding list) that are available and if hello fault creates logs.</span></span>

### <a name="gateway"></a><span data-ttu-id="41c79-127">Átjáró</span><span class="sxs-lookup"><span data-stu-id="41c79-127">Gateway</span></span>

| <span data-ttu-id="41c79-128">Hibatípus</span><span class="sxs-lookup"><span data-stu-id="41c79-128">Fault Type</span></span> | <span data-ttu-id="41c79-129">Ok</span><span class="sxs-lookup"><span data-stu-id="41c79-129">Reason</span></span> | <span data-ttu-id="41c79-130">Napló</span><span class="sxs-lookup"><span data-stu-id="41c79-130">Log</span></span>|
|---|---|---|
| <span data-ttu-id="41c79-131">NoFault</span><span class="sxs-lookup"><span data-stu-id="41c79-131">NoFault</span></span> | <span data-ttu-id="41c79-132">Ha nincs hiba észlelhető.</span><span class="sxs-lookup"><span data-stu-id="41c79-132">When no error is detected.</span></span> |<span data-ttu-id="41c79-133">Igen</span><span class="sxs-lookup"><span data-stu-id="41c79-133">Yes</span></span>|
| <span data-ttu-id="41c79-134">GatewayNotFound</span><span class="sxs-lookup"><span data-stu-id="41c79-134">GatewayNotFound</span></span> | <span data-ttu-id="41c79-135">Nem található átjáró vagy az átjáró nem lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="41c79-135">Cannot find Gateway or Gateway is not provisioned.</span></span> |<span data-ttu-id="41c79-136">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-136">No</span></span>|
| <span data-ttu-id="41c79-137">PlannedMaintenance</span><span class="sxs-lookup"><span data-stu-id="41c79-137">PlannedMaintenance</span></span> |  <span data-ttu-id="41c79-138">Átjárópéldány karbantartás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="41c79-138">Gateway instance is under maintenance.</span></span>  |<span data-ttu-id="41c79-139">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-139">No</span></span>|
| <span data-ttu-id="41c79-140">UserDrivenUpdate</span><span class="sxs-lookup"><span data-stu-id="41c79-140">UserDrivenUpdate</span></span> | <span data-ttu-id="41c79-141">Amikor a felhasználó frissítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="41c79-141">When a user update is in progress.</span></span> <span data-ttu-id="41c79-142">Ennek oka lehet egy átméretezés.</span><span class="sxs-lookup"><span data-stu-id="41c79-142">This could be a resize operation.</span></span> | <span data-ttu-id="41c79-143">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-143">No</span></span> |
| <span data-ttu-id="41c79-144">VipUnResponsive</span><span class="sxs-lookup"><span data-stu-id="41c79-144">VipUnResponsive</span></span> | <span data-ttu-id="41c79-145">Hello elsődleges hello átjáró példánya nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="41c79-145">Cannot reach hello primary instance of hello Gateway.</span></span> <span data-ttu-id="41c79-146">Ez akkor fordul elő, amikor hello állapotmintáihoz sikertelen.</span><span class="sxs-lookup"><span data-stu-id="41c79-146">This happens when hello health probe fails.</span></span> | <span data-ttu-id="41c79-147">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-147">No</span></span> |
| <span data-ttu-id="41c79-148">PlatformInActive</span><span class="sxs-lookup"><span data-stu-id="41c79-148">PlatformInActive</span></span> | <span data-ttu-id="41c79-149">A hello platformmal probléma van.</span><span class="sxs-lookup"><span data-stu-id="41c79-149">There is an issue with hello platform.</span></span> | <span data-ttu-id="41c79-150">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-150">No</span></span>|
| <span data-ttu-id="41c79-151">ServiceNotRunning</span><span class="sxs-lookup"><span data-stu-id="41c79-151">ServiceNotRunning</span></span> | <span data-ttu-id="41c79-152">hello alapul szolgáló szolgáltatás nem fut.</span><span class="sxs-lookup"><span data-stu-id="41c79-152">hello underlying service is not running.</span></span> | <span data-ttu-id="41c79-153">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-153">No</span></span>|
| <span data-ttu-id="41c79-154">NoConnectionsFoundForGateway</span><span class="sxs-lookup"><span data-stu-id="41c79-154">NoConnectionsFoundForGateway</span></span> | <span data-ttu-id="41c79-155">Hello átjáró kapcsolat nem létezik.</span><span class="sxs-lookup"><span data-stu-id="41c79-155">No Connections exists on hello gateway.</span></span> <span data-ttu-id="41c79-156">Ez a figyelmeztetés csak.</span><span class="sxs-lookup"><span data-stu-id="41c79-156">This is only a warning.</span></span>| <span data-ttu-id="41c79-157">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-157">No</span></span>|
| <span data-ttu-id="41c79-158">ConnectionsNotConnected</span><span class="sxs-lookup"><span data-stu-id="41c79-158">ConnectionsNotConnected</span></span> | <span data-ttu-id="41c79-159">Kapcsolatok nem kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="41c79-159">Connections are not connected.</span></span> <span data-ttu-id="41c79-160">Ez a figyelmeztetés csak.</span><span class="sxs-lookup"><span data-stu-id="41c79-160">This is only a warning.</span></span>| <span data-ttu-id="41c79-161">Igen</span><span class="sxs-lookup"><span data-stu-id="41c79-161">Yes</span></span>|
| <span data-ttu-id="41c79-162">GatewayCPUUsageExceeded</span><span class="sxs-lookup"><span data-stu-id="41c79-162">GatewayCPUUsageExceeded</span></span> | <span data-ttu-id="41c79-163">hello aktuális átjáró CPU-használat > 95 %.</span><span class="sxs-lookup"><span data-stu-id="41c79-163">hello current Gateway CPU usage is > 95%.</span></span> | <span data-ttu-id="41c79-164">Igen</span><span class="sxs-lookup"><span data-stu-id="41c79-164">Yes</span></span> |

### <a name="connection"></a><span data-ttu-id="41c79-165">Kapcsolat</span><span class="sxs-lookup"><span data-stu-id="41c79-165">Connection</span></span>

| <span data-ttu-id="41c79-166">Hibatípus</span><span class="sxs-lookup"><span data-stu-id="41c79-166">Fault Type</span></span> | <span data-ttu-id="41c79-167">Ok</span><span class="sxs-lookup"><span data-stu-id="41c79-167">Reason</span></span> | <span data-ttu-id="41c79-168">Napló</span><span class="sxs-lookup"><span data-stu-id="41c79-168">Log</span></span>|
|---|---|---|
| <span data-ttu-id="41c79-169">NoFault</span><span class="sxs-lookup"><span data-stu-id="41c79-169">NoFault</span></span> | <span data-ttu-id="41c79-170">Ha nincs hiba észlelhető.</span><span class="sxs-lookup"><span data-stu-id="41c79-170">When no error is detected.</span></span> |<span data-ttu-id="41c79-171">Igen</span><span class="sxs-lookup"><span data-stu-id="41c79-171">Yes</span></span>|
| <span data-ttu-id="41c79-172">GatewayNotFound</span><span class="sxs-lookup"><span data-stu-id="41c79-172">GatewayNotFound</span></span> | <span data-ttu-id="41c79-173">Nem található átjáró vagy az átjáró nem lett beállítva.</span><span class="sxs-lookup"><span data-stu-id="41c79-173">Cannot find Gateway or Gateway is not provisioned.</span></span> |<span data-ttu-id="41c79-174">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-174">No</span></span>|
| <span data-ttu-id="41c79-175">PlannedMaintenance</span><span class="sxs-lookup"><span data-stu-id="41c79-175">PlannedMaintenance</span></span> | <span data-ttu-id="41c79-176">Átjárópéldány karbantartás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="41c79-176">Gateway instance is under maintenance.</span></span>  |<span data-ttu-id="41c79-177">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-177">No</span></span>|
| <span data-ttu-id="41c79-178">UserDrivenUpdate</span><span class="sxs-lookup"><span data-stu-id="41c79-178">UserDrivenUpdate</span></span> | <span data-ttu-id="41c79-179">Amikor a felhasználó frissítése folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="41c79-179">When a user update is in progress.</span></span> <span data-ttu-id="41c79-180">Ennek oka lehet egy átméretezés.</span><span class="sxs-lookup"><span data-stu-id="41c79-180">This could be a resize operation.</span></span>  | <span data-ttu-id="41c79-181">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-181">No</span></span> |
| <span data-ttu-id="41c79-182">VipUnResponsive</span><span class="sxs-lookup"><span data-stu-id="41c79-182">VipUnResponsive</span></span> | <span data-ttu-id="41c79-183">Hello elsődleges hello átjáró példánya nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="41c79-183">Cannot reach hello primary instance of hello Gateway.</span></span> <span data-ttu-id="41c79-184">Akkor történik, ha hello állapotmintáihoz sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="41c79-184">It happens when hello health probe fails.</span></span> | <span data-ttu-id="41c79-185">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-185">No</span></span> |
| <span data-ttu-id="41c79-186">ConnectionEntityNotFound</span><span class="sxs-lookup"><span data-stu-id="41c79-186">ConnectionEntityNotFound</span></span> | <span data-ttu-id="41c79-187">Kapcsolat konfigurációja hiányzik.</span><span class="sxs-lookup"><span data-stu-id="41c79-187">Connection configuration is missing.</span></span> | <span data-ttu-id="41c79-188">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-188">No</span></span> |
| <span data-ttu-id="41c79-189">ConnectionIsMarkedDisconnected</span><span class="sxs-lookup"><span data-stu-id="41c79-189">ConnectionIsMarkedDisconnected</span></span> | <span data-ttu-id="41c79-190">hello kapcsolat "leválasztott" van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="41c79-190">hello Connection is marked "disconnected".</span></span> |<span data-ttu-id="41c79-191">Nem</span><span class="sxs-lookup"><span data-stu-id="41c79-191">No</span></span>|
| <span data-ttu-id="41c79-192">ConnectionNotConfiguredOnGateway</span><span class="sxs-lookup"><span data-stu-id="41c79-192">ConnectionNotConfiguredOnGateway</span></span> | <span data-ttu-id="41c79-193">hello mögöttes szolgáltatás nincs kapcsolat konfigurálva hello.</span><span class="sxs-lookup"><span data-stu-id="41c79-193">hello underlying service does not have hello Connection configured.</span></span> | <span data-ttu-id="41c79-194">Igen</span><span class="sxs-lookup"><span data-stu-id="41c79-194">Yes</span></span> |
| <span data-ttu-id="41c79-195">ConnectionMarkedStandy</span><span class="sxs-lookup"><span data-stu-id="41c79-195">ConnectionMarkedStandy</span></span> | <span data-ttu-id="41c79-196">az alapul szolgáló szolgáltatás hello készenléti jelölésű.</span><span class="sxs-lookup"><span data-stu-id="41c79-196">hello underlying service is marked as standby.</span></span>| <span data-ttu-id="41c79-197">Igen</span><span class="sxs-lookup"><span data-stu-id="41c79-197">Yes</span></span>|
| <span data-ttu-id="41c79-198">Authentication</span><span class="sxs-lookup"><span data-stu-id="41c79-198">Authentication</span></span> | <span data-ttu-id="41c79-199">Előmegosztott kulcs nem megfelelő.</span><span class="sxs-lookup"><span data-stu-id="41c79-199">Preshared Key mismatch.</span></span> | <span data-ttu-id="41c79-200">Igen</span><span class="sxs-lookup"><span data-stu-id="41c79-200">Yes</span></span>|
| <span data-ttu-id="41c79-201">PeerReachability</span><span class="sxs-lookup"><span data-stu-id="41c79-201">PeerReachability</span></span> | <span data-ttu-id="41c79-202">hello társ átjáró nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="41c79-202">hello peer gateway is not reachable.</span></span> | <span data-ttu-id="41c79-203">Igen</span><span class="sxs-lookup"><span data-stu-id="41c79-203">Yes</span></span>|
| <span data-ttu-id="41c79-204">IkePolicyMismatch</span><span class="sxs-lookup"><span data-stu-id="41c79-204">IkePolicyMismatch</span></span> | <span data-ttu-id="41c79-205">hello társ átjáró rendelkezik IKE szabályzatok Azure által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="41c79-205">hello peer gateway has IKE policies that are not supported by Azure.</span></span> | <span data-ttu-id="41c79-206">Igen</span><span class="sxs-lookup"><span data-stu-id="41c79-206">Yes</span></span>|
| <span data-ttu-id="41c79-207">WfpParse hiba</span><span class="sxs-lookup"><span data-stu-id="41c79-207">WfpParse Error</span></span> | <span data-ttu-id="41c79-208">Hiba történt az elemzési hello WFP napló.</span><span class="sxs-lookup"><span data-stu-id="41c79-208">An error occurred parsing hello WFP log.</span></span> |<span data-ttu-id="41c79-209">Igen</span><span class="sxs-lookup"><span data-stu-id="41c79-209">Yes</span></span>|

## <a name="supported-gateway-types"></a><span data-ttu-id="41c79-210">Támogatott átjáró típusok</span><span class="sxs-lookup"><span data-stu-id="41c79-210">Supported Gateway types</span></span>

<span data-ttu-id="41c79-211">hello alábbi lista mutatja azokat hello támogatási jeleníti meg, melyik átjárót és a kapcsolatok támogatottak a hibaelhárításban hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="41c79-211">hello following list shows hello support shows which gateways and connections are supported with Network Watcher troubleshooting.</span></span>
|  |  |
|---------|---------|
|<span data-ttu-id="41c79-212">**Átjáró típusa**</span><span class="sxs-lookup"><span data-stu-id="41c79-212">**Gateway types**</span></span>   |         |
|<span data-ttu-id="41c79-213">VPN</span><span class="sxs-lookup"><span data-stu-id="41c79-213">VPN</span></span>      | <span data-ttu-id="41c79-214">Támogatott</span><span class="sxs-lookup"><span data-stu-id="41c79-214">Supported</span></span>        |
|<span data-ttu-id="41c79-215">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="41c79-215">ExpressRoute</span></span> | <span data-ttu-id="41c79-216">Nem támogatott</span><span class="sxs-lookup"><span data-stu-id="41c79-216">Not Supported</span></span> |
|<span data-ttu-id="41c79-217">Hypernet</span><span class="sxs-lookup"><span data-stu-id="41c79-217">Hypernet</span></span> | <span data-ttu-id="41c79-218">Nem támogatott</span><span class="sxs-lookup"><span data-stu-id="41c79-218">Not Supported</span></span>|
|<span data-ttu-id="41c79-219">**VPN-típusai**</span><span class="sxs-lookup"><span data-stu-id="41c79-219">**VPN types**</span></span> | |
|<span data-ttu-id="41c79-220">Útvonal alapján</span><span class="sxs-lookup"><span data-stu-id="41c79-220">Route Based</span></span> | <span data-ttu-id="41c79-221">Támogatott</span><span class="sxs-lookup"><span data-stu-id="41c79-221">Supported</span></span>|
|<span data-ttu-id="41c79-222">Csoportházirend-alapú</span><span class="sxs-lookup"><span data-stu-id="41c79-222">Policy Based</span></span> | <span data-ttu-id="41c79-223">Nem támogatott</span><span class="sxs-lookup"><span data-stu-id="41c79-223">Not Supported</span></span>|
|<span data-ttu-id="41c79-224">**Kapcsolat típusai**</span><span class="sxs-lookup"><span data-stu-id="41c79-224">**Connection types**</span></span>||
|<span data-ttu-id="41c79-225">IPSec</span><span class="sxs-lookup"><span data-stu-id="41c79-225">IPSec</span></span>| <span data-ttu-id="41c79-226">Támogatott</span><span class="sxs-lookup"><span data-stu-id="41c79-226">Supported</span></span>|
|<span data-ttu-id="41c79-227">VNet2Vnet</span><span class="sxs-lookup"><span data-stu-id="41c79-227">VNet2Vnet</span></span>| <span data-ttu-id="41c79-228">Támogatott</span><span class="sxs-lookup"><span data-stu-id="41c79-228">Supported</span></span>|
|<span data-ttu-id="41c79-229">ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="41c79-229">ExpressRoute</span></span>| <span data-ttu-id="41c79-230">Nem támogatott</span><span class="sxs-lookup"><span data-stu-id="41c79-230">Not Supported</span></span>|
|<span data-ttu-id="41c79-231">Hypernet</span><span class="sxs-lookup"><span data-stu-id="41c79-231">Hypernet</span></span>| <span data-ttu-id="41c79-232">Nem támogatott</span><span class="sxs-lookup"><span data-stu-id="41c79-232">Not Supported</span></span>|
|<span data-ttu-id="41c79-233">VPNClient</span><span class="sxs-lookup"><span data-stu-id="41c79-233">VPNClient</span></span>| <span data-ttu-id="41c79-234">Nem támogatott</span><span class="sxs-lookup"><span data-stu-id="41c79-234">Not Supported</span></span>|

## <a name="log-files"></a><span data-ttu-id="41c79-235">Naplófájlok</span><span class="sxs-lookup"><span data-stu-id="41c79-235">Log files</span></span>

<span data-ttu-id="41c79-236">hello erőforrás hibaelhárítás naplófájljai vannak tárolva egy tárfiókot, erőforrás hibakeresési befejeződése után.</span><span class="sxs-lookup"><span data-stu-id="41c79-236">hello resource troubleshooting log files are stored in a storage account after resource troubleshooting is finished.</span></span> <span data-ttu-id="41c79-237">hello következő kép bemutatja, hibát eredményezett hívás hello példa tartalmát.</span><span class="sxs-lookup"><span data-stu-id="41c79-237">hello following image shows hello example contents of a call that resulted in an error.</span></span>

![zip-fájl][1]

> [!NOTE]
> <span data-ttu-id="41c79-239">Bizonyos esetekben csak egy részhalmazát hello naplófájlokat toostorage írása.</span><span class="sxs-lookup"><span data-stu-id="41c79-239">In some cases, only a subset of hello logs files is written toostorage.</span></span>

<span data-ttu-id="41c79-240">A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg túl[az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span><span class="sxs-lookup"><span data-stu-id="41c79-240">For instructions on downloading files from azure storage accounts, refer too[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md).</span></span> <span data-ttu-id="41c79-241">Egy másik eszköz, amely használható a Tártallózó.</span><span class="sxs-lookup"><span data-stu-id="41c79-241">Another tool that can be used is Storage Explorer.</span></span> <span data-ttu-id="41c79-242">Tártallózó további információt itt található: a következő hivatkozás hello: [Tártallózó](http://storageexplorer.com/)</span><span class="sxs-lookup"><span data-stu-id="41c79-242">More information about Storage Explorer can be found here at hello following link: [Storage Explorer](http://storageexplorer.com/)</span></span>

### <a name="connectionstatstxt"></a><span data-ttu-id="41c79-243">ConnectionStats.txt</span><span class="sxs-lookup"><span data-stu-id="41c79-243">ConnectionStats.txt</span></span>

<span data-ttu-id="41c79-244">Hello **ConnectionStats.txt** fájl hello kapcsolat, beleértve a bemenő és kimenő bájtokat, a kapcsolat állapota és a hello idő hello kapcsolat jött létre az összesített statisztikák tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="41c79-244">hello **ConnectionStats.txt** file contains overall stats of hello Connection, including ingress and egress bytes, Connection status, and hello time hello Connection was established.</span></span>

> [!NOTE]
> <span data-ttu-id="41c79-245">Hello hívás toohello API hibaelhárítási kifogástalan adja vissza, ha van-e a visszaadott hello zip-fájlban szereplő hello egyedül a **ConnectionStats.txt** fájlt.</span><span class="sxs-lookup"><span data-stu-id="41c79-245">If hello call toohello troubleshooting API returns healthy, hello only thing returned in hello zip file is a **ConnectionStats.txt** file.</span></span>

<span data-ttu-id="41c79-246">hello a fájl tartalma a következő példa hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="41c79-246">hello contents of this file are similar toohello following example:</span></span>

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a><span data-ttu-id="41c79-247">CPUStats.txt</span><span class="sxs-lookup"><span data-stu-id="41c79-247">CPUStats.txt</span></span>

<span data-ttu-id="41c79-248">Hello **CPUStats.txt** CPU-használat és a teszteléshez hello időpontjában elérhető memória-fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41c79-248">hello **CPUStats.txt** file contains CPU usage and memory available at hello time of testing.</span></span>  <span data-ttu-id="41c79-249">a fájl hello tartalmát a következő példa hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="41c79-249">hello contents of this file is similar toohello following example:</span></span>

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a><span data-ttu-id="41c79-250">IKEErrors.txt</span><span class="sxs-lookup"><span data-stu-id="41c79-250">IKEErrors.txt</span></span>

<span data-ttu-id="41c79-251">Hello **IKEErrors.txt** fájl bármely ellenőrzése során talált IKE hibákat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41c79-251">hello **IKEErrors.txt** file contains any IKE errors that were found during monitoring.</span></span>

<span data-ttu-id="41c79-252">hello következő példa bemutatja egy IKEErrors.txt fájl hello tartalmát.</span><span class="sxs-lookup"><span data-stu-id="41c79-252">hello following example shows hello contents of an IKEErrors.txt file.</span></span> <span data-ttu-id="41c79-253">A hibák hello probléma függően eltérő lehet.</span><span class="sxs-lookup"><span data-stu-id="41c79-253">Your errors may be different depending on hello issue.</span></span>

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a><span data-ttu-id="41c79-254">Törlődik wfpdiag.txt</span><span class="sxs-lookup"><span data-stu-id="41c79-254">Scrubbed-wfpdiag.txt</span></span>

<span data-ttu-id="41c79-255">Hello **Scrubbed-wfpdiag.txt** naplófájl hello wfp naplófájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="41c79-255">hello **Scrubbed-wfpdiag.txt** log file contains hello wfp log.</span></span> <span data-ttu-id="41c79-256">Ez a napló tartalmaz csomagjai és IKE/AuthIP hibák naplózása.</span><span class="sxs-lookup"><span data-stu-id="41c79-256">This log contains logging of packet drop and IKE/AuthIP failures.</span></span>

<span data-ttu-id="41c79-257">hello következő példa bemutatja hello hello Scrubbed-wfpdiag.txt fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="41c79-257">hello following example shows hello contents of hello Scrubbed-wfpdiag.txt file.</span></span> <span data-ttu-id="41c79-258">Ebben a példában az hello megosztott kulcs kapcsolat nem volt pontosak, mivel a 3. sorából hello hello aljáról látható.</span><span class="sxs-lookup"><span data-stu-id="41c79-258">In this example, hello shared key of a Connection was not correct as can be seen from hello 3rd line from hello bottom.</span></span> <span data-ttu-id="41c79-259">a következő példa hello csak hello teljes napló, a részlet esetén, mert a hello napló megnőhet, attól függően, hogy hello probléma.</span><span class="sxs-lookup"><span data-stu-id="41c79-259">hello following example is just a snippet of hello entire log, as hello log can be lengthy depending on hello issue.</span></span>

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from hello high priority thread pool list
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|IKE diagnostic event:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Event Header:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Timestamp: 1601-01-01T00:00:00.000Z
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Flags: 0x00000106
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Local address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Remote address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IP version field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP version: IPv4
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP protocol: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local address: 13.78.238.92
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote address: 52.161.24.36
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Application ID:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  User SID: <invalid>
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Failure type: IKE/Authip Main Mode Failure
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Type specific info:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure error code:0x000035e9
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IKE authentication credentials are unacceptable
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure point: Remote
...
```

### <a name="wfpdiagtxtsum"></a><span data-ttu-id="41c79-260">wfpdiag.txt.Sum</span><span class="sxs-lookup"><span data-stu-id="41c79-260">wfpdiag.txt.sum</span></span>

<span data-ttu-id="41c79-261">Hello **wfpdiag.txt.sum** fájl a napló hello buffers és a feldolgozott események megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="41c79-261">hello **wfpdiag.txt.sum** file is a log showing hello buffers and events processed.</span></span>

<span data-ttu-id="41c79-262">hello következő példa egy hello wfpdiag.txt.sum fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="41c79-262">hello following example is hello contents of a wfpdiag.txt.sum file.</span></span>
```
Files Processed:
    C:\Resources\directory\924336c47dd045d5a246c349b8ae57f2.GatewayTenantWorker.DiagnosticsStorage\2017-02-02T17-34-23\wfpdiag.etl
Total Buffers Processed 8
Total Events  Processed 2169
Total Events  Lost      0
Total Format  Errors    0
Total Formats Unknown   486
Elapsed Time            330 sec
+-----------------------------------------------------------------------------------+
|EventCount    EventName            EventType   TMF                                 |
+-----------------------------------------------------------------------------------+
|        36    ikeext               ike_addr_utils_c844  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        12    ikeext               ike_addr_utils_c857  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        96    ikeext               ike_addr_utils_c832  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|         6    ikeext               ike_bfe_callbacks_c133  1dc2d67f-8381-6303-e314-6c1452eeb529|
|         6    ikeext               ike_bfe_callbacks_c61  1dc2d67f-8381-6303-e314-6c1452eeb529|
|        12    ikeext               ike_sa_management_c5698  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c8447  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c494  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c642  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c3162  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c3307  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
```

## <a name="next-steps"></a><span data-ttu-id="41c79-263">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41c79-263">Next steps</span></span>

<span data-ttu-id="41c79-264">Ismerje meg, hogyan toodiagnose VPN-átjárók és kapcsolatok keresztül hello portal ellátogatva [átjáró hibaelhárítás – Azure-portálon](network-watcher-troubleshoot-manage-portal.md).</span><span class="sxs-lookup"><span data-stu-id="41c79-264">Learn how toodiagnose VPN Gateways and Connections through hello portal by visiting [Gateway troubleshooting - Azure portal](network-watcher-troubleshoot-manage-portal.md).</span></span>
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
