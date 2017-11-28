---
title: "Azure Application Insights Telemetria adatmodell - Telemetria környezetben |} Microsoft Docs"
description: "Application Insights telemetria környezetben adatmodell"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/15/2017
ms.author: sergkanz
ms.openlocfilehash: d6a0cad8bda6ca68aa691867e84f540c5ac9f6f3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="telemetry-context-application-insights-data-model"></a><span data-ttu-id="ab9ec-103">Telemetria a környezetben: Application Insights adatmodell</span><span class="sxs-lookup"><span data-stu-id="ab9ec-103">Telemetry context: Application Insights data model</span></span>

<span data-ttu-id="ab9ec-104">Minden telemetriai elem lehet egy szigorú típusmegadású környezet mezőket.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-104">Every telemetry item may have a strongly typed context fields.</span></span> <span data-ttu-id="ab9ec-105">Minden mező lehetővé teszi, hogy egy adott figyelési forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-105">Every field enables a specific monitoring scenario.</span></span> <span data-ttu-id="ab9ec-106">Az egyéni tulajdonságok gyűjteményén segítségével egyéni vagy az alkalmazás-specifikus környezetfüggő információk tárolására.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-106">Use the custom properties collection to store custom or application-specific contextual information.</span></span>


##<a name="application-version"></a><span data-ttu-id="ab9ec-107">Alkalmazás verziószáma</span><span class="sxs-lookup"><span data-stu-id="ab9ec-107">Application version</span></span>

<span data-ttu-id="ab9ec-108">Amely a telemetriai adatokat küld az alkalmazással kapcsolatos információkat az alkalmazás helyi mezőket mindig van.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-108">Information in the application context fields is always about the application that is sending the telemetry.</span></span> <span data-ttu-id="ab9ec-109">Alkalmazásverzió segítségével elemezheti az alkalmazás viselkedését, és a központi telepítésekre a korrelációs trend változásai.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-109">Application version is used to analyze trend changes in the application behavior and its correlation to the deployments.</span></span>

<span data-ttu-id="ab9ec-110">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="ab9ec-110">Max length: 1024</span></span>


##<a name="client-ip-address"></a><span data-ttu-id="ab9ec-111">Ügyfél IP-címe</span><span class="sxs-lookup"><span data-stu-id="ab9ec-111">Client IP address</span></span>

<span data-ttu-id="ab9ec-112">Az ügyféleszköz IP-címét.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-112">The IP address of the client device.</span></span> <span data-ttu-id="ab9ec-113">IPv4 és IPv6 használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-113">IPv4 and IPv6 are supported.</span></span> <span data-ttu-id="ab9ec-114">Telemetriai adatokat küldött a szolgáltatás, a hely környezetben tárgya a felhasználót, hogy a szolgáltatási műveletet kezdeményezett.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-114">When telemetry is sent from a service, the location context is about the user that initiated the operation in the service.</span></span> <span data-ttu-id="ab9ec-115">Az Application Insights csomagolja ki a földrajzihely-információ az ügyfél IP-cím, és majd csonkolja.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-115">Application Insights extract the geo-location information from the client IP and then truncate it.</span></span> <span data-ttu-id="ab9ec-116">Ezért ügyfél IP-önmagában nem használható azonosításra alkalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-116">So client IP by itself cannot be used as end-user identifiable information.</span></span> 

<span data-ttu-id="ab9ec-117">Maximális hossz: 46</span><span class="sxs-lookup"><span data-stu-id="ab9ec-117">Max length: 46</span></span>


##<a name="device-type"></a><span data-ttu-id="ab9ec-118">Eszköz típusa</span><span class="sxs-lookup"><span data-stu-id="ab9ec-118">Device type</span></span>

<span data-ttu-id="ab9ec-119">Ez a mező eredetileg jelzi a felhasználó az alkalmazás által használt eszköz típusának lett megadva.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-119">Originally this field was used to indicate the type of the device the end user of the application is using.</span></span> <span data-ttu-id="ab9ec-120">Ma használt elsősorban különböztetheti meg egymástól a JavaScript telemetriai eszköz típusú "Browser" kiszolgálóoldali a telemetriai adatok az eszköz nem írja be a "PC".</span><span class="sxs-lookup"><span data-stu-id="ab9ec-120">Today used primarily to distinguish JavaScript telemetry with the device type 'Browser' from server-side telemetry with the device type 'PC'.</span></span>

<span data-ttu-id="ab9ec-121">Maximális hossz: 64</span><span class="sxs-lookup"><span data-stu-id="ab9ec-121">Max length: 64</span></span>


##<a name="operation-id"></a><span data-ttu-id="ab9ec-122">A művelet azonosítója</span><span class="sxs-lookup"><span data-stu-id="ab9ec-122">Operation id</span></span>

<span data-ttu-id="ab9ec-123">A legfelső szintű tevékenység egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-123">A unique identifier of the root operation.</span></span> <span data-ttu-id="ab9ec-124">Ez az azonosító lehetővé teszi, hogy a csoport telemetriai több összetevői között.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-124">This identifier allows to group telemetry across multiple components.</span></span> <span data-ttu-id="ab9ec-125">Lásd: [telemetriai korrelációs](application-insights-correlation.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-125">See [telemetry correlation](application-insights-correlation.md) for details.</span></span> <span data-ttu-id="ab9ec-126">A művelet azonosítója vagy egy kérelem, vagy egy nézet hozta létre.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-126">The operation id is created by either a request or a page view.</span></span> <span data-ttu-id="ab9ec-127">Minden egyéb telemetriai adat ebben a mezőben a tartalmazó kérelem vagy a lap nézet értéket állítja be.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-127">All other telemetry sets this field to the value for the containing request or page view.</span></span> 

<span data-ttu-id="ab9ec-128">Maximális hossz: 128</span><span class="sxs-lookup"><span data-stu-id="ab9ec-128">Max length: 128</span></span>


##<a name="parent-operation-id"></a><span data-ttu-id="ab9ec-129">Szülő művelet azonosítója</span><span class="sxs-lookup"><span data-stu-id="ab9ec-129">Parent operation ID</span></span>

<span data-ttu-id="ab9ec-130">A telemetriai adatok elem azonnali szülő egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-130">The unique identifier of the telemetry item's immediate parent.</span></span> <span data-ttu-id="ab9ec-131">Lásd: [telemetriai korrelációs](application-insights-correlation.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-131">See [telemetry correlation](application-insights-correlation.md) for details.</span></span>

<span data-ttu-id="ab9ec-132">Maximális hossz: 128</span><span class="sxs-lookup"><span data-stu-id="ab9ec-132">Max length: 128</span></span>


##<a name="operation-name"></a><span data-ttu-id="ab9ec-133">A művelet neve</span><span class="sxs-lookup"><span data-stu-id="ab9ec-133">Operation name</span></span>

<span data-ttu-id="ab9ec-134">A (csoport) a művelet neve.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-134">The name (group) of the operation.</span></span> <span data-ttu-id="ab9ec-135">A művelet neve vagy a kérelmet, vagy a lap nézet hozta létre.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-135">The operation name is created by either a request or a page view.</span></span> <span data-ttu-id="ab9ec-136">Minden egyéb telemetriai elem ebben a mezőben a tartalmazó kérelem vagy a lap nézet értékre állítva.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-136">All other telemetry items set this field to the value for the containing request or page view.</span></span> <span data-ttu-id="ab9ec-137">Műveletek (például "GET otthoni/Index") egy csoportja számára a telemetriai adatok elemek kereséséhez használt művelet neve.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-137">Operation name is used for finding all the telemetry items for a group of operations (for example 'GET Home/Index').</span></span> <span data-ttu-id="ab9ec-138">A context tulajdonság segítségével fogadja a hívást kérdéseket, például "Mik a tipikus kivételek, ezen az oldalon."</span><span class="sxs-lookup"><span data-stu-id="ab9ec-138">This context property is used to answer questions like "what are the typical exceptions thrown on this page."</span></span>

<span data-ttu-id="ab9ec-139">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="ab9ec-139">Max length: 1024</span></span>


##<a name="synthetic-source-of-the-operation"></a><span data-ttu-id="ab9ec-140">A művelet szintetikus forrása</span><span class="sxs-lookup"><span data-stu-id="ab9ec-140">Synthetic source of the operation</span></span>

<span data-ttu-id="ab9ec-141">Szintetikus forrás nevére.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-141">Name of synthetic source.</span></span> <span data-ttu-id="ab9ec-142">Az alkalmazás egyes telemetriai szintetikus forgalom jelöl.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-142">Some telemetry from the application may represent synthetic traffic.</span></span> <span data-ttu-id="ab9ec-143">A webhely, a webhely rendelkezésreállás figyelésére szolgáló tesztek vagy a nyomkövetések az Application Insights SDK maga például diagnosztikai szalagtárak indexelő webbejáró lehet.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-143">It may be web crawler indexing the web site, site availability tests, or traces from diagnostic libraries like Application Insights SDK itself.</span></span>

<span data-ttu-id="ab9ec-144">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="ab9ec-144">Max length: 1024</span></span>


##<a name="session-id"></a><span data-ttu-id="ab9ec-145">munkamenet-azonosító</span><span class="sxs-lookup"><span data-stu-id="ab9ec-145">Session id</span></span>

<span data-ttu-id="ab9ec-146">Munkamenet-azonosító – a felhasználó és az alkalmazás közötti interakció példányát.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-146">Session ID - the instance of the user's interaction with the app.</span></span> <span data-ttu-id="ab9ec-147">Információk a munkamenet-környezet mezőket a végfelhasználó mindig tárgya.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-147">Information in the session context fields is always about the end user.</span></span> <span data-ttu-id="ab9ec-148">Telemetriai adatokat küldött a szolgáltatás, a munkamenet környezetében tárgya a felhasználót, hogy a szolgáltatási műveletet kezdeményezett.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-148">When telemetry is sent from a service, the session context is about the user that initiated the operation in the service.</span></span>

<span data-ttu-id="ab9ec-149">Maximális hossz: 64</span><span class="sxs-lookup"><span data-stu-id="ab9ec-149">Max length: 64</span></span>


##<a name="anonymous-user-id"></a><span data-ttu-id="ab9ec-150">A névtelen felhasználói azonosító</span><span class="sxs-lookup"><span data-stu-id="ab9ec-150">Anonymous user id</span></span>

<span data-ttu-id="ab9ec-151">A névtelen felhasználói azonosító.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-151">Anonymous user id.</span></span> <span data-ttu-id="ab9ec-152">Az alkalmazás a végfelhasználó jelenti.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-152">Represents the end user of the application.</span></span> <span data-ttu-id="ab9ec-153">Telemetriai adatokat küldött a szolgáltatás, a felhasználói környezet tárgya a felhasználót, hogy a szolgáltatási műveletet kezdeményezett.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-153">When telemetry is sent from a service, the user context is about the user that initiated the operation in the service.</span></span>

<span data-ttu-id="ab9ec-154">[A mintavételi](app-insights-sampling.md) csökkentése érdekében az összegyűjtött telemetrikus módszerek egyike.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-154">[Sampling](app-insights-sampling.md) is one of the techniques to minimize the amount of collected telemetry.</span></span> <span data-ttu-id="ab9ec-155">Mintavételi algoritmus megkísérli a bejövő vagy kimenő a kapcsolódó telemetriai vagy minta.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-155">Sampling algorithm attempts to either sample in or out all the correlated telemetry.</span></span> <span data-ttu-id="ab9ec-156">A névtelen felhasználói azonosítót használja a pontszám generációs mintavétel.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-156">Anonymous user id is used for sampling score generation.</span></span> <span data-ttu-id="ab9ec-157">Így a névtelen felhasználói azonosító elég véletlenszerű értéknek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-157">So anonymous user id should be a random enough value.</span></span> 

<span data-ttu-id="ab9ec-158">Névtelen felhasználói azonosítóját tárolja a felhasználónév használata a visszaélés miatt, a mező.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-158">Using anonymous user id to store user name is a misuse of the field.</span></span> <span data-ttu-id="ab9ec-159">Hitelesített felhasználói azonosítóját használnia.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-159">Use Authenticated user id.</span></span>

<span data-ttu-id="ab9ec-160">Maximális hossz: 128</span><span class="sxs-lookup"><span data-stu-id="ab9ec-160">Max length: 128</span></span>


##<a name="authenticated-user-id"></a><span data-ttu-id="ab9ec-161">Hitelesített felhasználó azonosítója</span><span class="sxs-lookup"><span data-stu-id="ab9ec-161">Authenticated user id</span></span>

<span data-ttu-id="ab9ec-162">Hitelesített felhasználó azonosítója.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-162">Authenticated user id.</span></span> <span data-ttu-id="ab9ec-163">A névtelen felhasználói azonosító ellentéte, ez a mező képviseli a felhasználót egy rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-163">The opposite of anonymous user id, this field represents the user with a friendly name.</span></span> <span data-ttu-id="ab9ec-164">Óta a PII adatok akkor van alapértelmezés szerint nem gyűjt a legtöbb SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-164">Since its PII information it is not collected by default by most SDK.</span></span>

<span data-ttu-id="ab9ec-165">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="ab9ec-165">Max length: 1024</span></span>


##<a name="account-id"></a><span data-ttu-id="ab9ec-166">Futtatófiók-azonosítóvá</span><span class="sxs-lookup"><span data-stu-id="ab9ec-166">Account id</span></span>

<span data-ttu-id="ab9ec-167">A több-bérlős alkalmazásokhoz Ez a futtatófiók-Azonosítóvá vagy nevét, amely a felhasználó működhet együtt.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-167">In multi-tenant applications this is the account ID or name, which the user is acting with.</span></span> <span data-ttu-id="ab9ec-168">Példák az Azure portál vagy blog neve bloggolás platform előfizetés-azonosító lehet.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-168">Examples may be subscription ID for Azure portal or blog name blogging platform.</span></span>

<span data-ttu-id="ab9ec-169">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="ab9ec-169">Max length: 1024</span></span>


##<a name="cloud-role"></a><span data-ttu-id="ab9ec-170">Felhő szerepkör</span><span class="sxs-lookup"><span data-stu-id="ab9ec-170">Cloud role</span></span>

<span data-ttu-id="ab9ec-171">A szerepkör az alkalmazás nevének egy részét képezi.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-171">Name of the role the application is a part of.</span></span> <span data-ttu-id="ab9ec-172">A szerepkör nevét, az azure-ban közvetlenül leképezve.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-172">Maps directly to the role name in azure.</span></span> <span data-ttu-id="ab9ec-173">Is segítségével különböztetheti meg egymástól micro szolgáltatásokat, amelyek egyetlen alkalmazás részét képezik.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-173">Can also be used to distinguish micro services, which are part of a single application.</span></span>

<span data-ttu-id="ab9ec-174">Maximális hossz: 256</span><span class="sxs-lookup"><span data-stu-id="ab9ec-174">Max length: 256</span></span>


##<a name="cloud-role-instance"></a><span data-ttu-id="ab9ec-175">Felhő szerepkör példánya</span><span class="sxs-lookup"><span data-stu-id="ab9ec-175">Cloud role instance</span></span>

<span data-ttu-id="ab9ec-176">Az alkalmazást futtató-példány nevét.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-176">Name of the instance where the application is running.</span></span> <span data-ttu-id="ab9ec-177">Számítógép neve a helyszíni, az Azure-példány nevét.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-177">Computer name for on-premises, instance name for Azure.</span></span>

<span data-ttu-id="ab9ec-178">Maximális hossz: 256</span><span class="sxs-lookup"><span data-stu-id="ab9ec-178">Max length: 256</span></span>


##<a name="internal-sdk-version"></a><span data-ttu-id="ab9ec-179">Belső: SDK-verzió</span><span class="sxs-lookup"><span data-stu-id="ab9ec-179">Internal: SDK version</span></span>

<span data-ttu-id="ab9ec-180">SDK-verzió.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-180">SDK version.</span></span> <span data-ttu-id="ab9ec-181">Lásd: https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification információt.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-181">See https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification for information.</span></span>

<span data-ttu-id="ab9ec-182">Maximális hossz: 64</span><span class="sxs-lookup"><span data-stu-id="ab9ec-182">Max length: 64</span></span>


##<a name="internal-node-name"></a><span data-ttu-id="ab9ec-183">Belső: A csomópont neve</span><span class="sxs-lookup"><span data-stu-id="ab9ec-183">Internal: Node name</span></span>

<span data-ttu-id="ab9ec-184">Ez a mező számlázási célokra csomópont nevét jelöli.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-184">This field represents the node name used for billing purposes.</span></span> <span data-ttu-id="ab9ec-185">Akkor bírálja felül a csomópontok szabványos észlelését.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-185">Use it to override the standard detection of nodes.</span></span>

<span data-ttu-id="ab9ec-186">Maximális hossz: 256</span><span class="sxs-lookup"><span data-stu-id="ab9ec-186">Max length: 256</span></span>


## <a name="next-steps"></a><span data-ttu-id="ab9ec-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ab9ec-187">Next steps</span></span>

- <span data-ttu-id="ab9ec-188">Megtudhatja, hogyan [kiterjesztése és a telemetriai adatok szűrése](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="ab9ec-188">Learn how to [extend and filter telemetry](app-insights-api-filtering-sampling.md).</span></span>
- <span data-ttu-id="ab9ec-189">Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="ab9ec-189">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="ab9ec-190">Tekintse meg a standard környezetben tulajdonsággyűjteményében [konfigurációs](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span><span class="sxs-lookup"><span data-stu-id="ab9ec-190">Check out standard context properties collection [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span></span>
