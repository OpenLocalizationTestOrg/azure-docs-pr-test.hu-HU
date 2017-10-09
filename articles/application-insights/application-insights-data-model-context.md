---
title: "Application Insights Telemetria adatmodell - aaaAzure Telemetriai környezetben |} Microsoft Docs"
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
ms.openlocfilehash: 6cdd6240d1c448f883d104a871ee9d5f6b5af2ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="telemetry-context-application-insights-data-model"></a><span data-ttu-id="0d406-103">Telemetria a környezetben: Application Insights adatmodell</span><span class="sxs-lookup"><span data-stu-id="0d406-103">Telemetry context: Application Insights data model</span></span>

<span data-ttu-id="0d406-104">Minden telemetriai elem lehet egy szigorú típusmegadású környezet mezőket.</span><span class="sxs-lookup"><span data-stu-id="0d406-104">Every telemetry item may have a strongly typed context fields.</span></span> <span data-ttu-id="0d406-105">Minden mező lehetővé teszi, hogy egy adott figyelési forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="0d406-105">Every field enables a specific monitoring scenario.</span></span> <span data-ttu-id="0d406-106">Hello egyéni tulajdonságok gyűjtemény toostore egyéni vagy az alkalmazás-specifikus környezetfüggő információkat használja.</span><span class="sxs-lookup"><span data-stu-id="0d406-106">Use hello custom properties collection toostore custom or application-specific contextual information.</span></span>


##<a name="application-version"></a><span data-ttu-id="0d406-107">Alkalmazás verziószáma</span><span class="sxs-lookup"><span data-stu-id="0d406-107">Application version</span></span>

<span data-ttu-id="0d406-108">Információk hello alkalmazás környezet mezőiben mindig tárgya hello hello telemetriai adatokat küldő alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0d406-108">Information in hello application context fields is always about hello application that is sending hello telemetry.</span></span> <span data-ttu-id="0d406-109">Alkalmazás verziószáma használt tooanalyze trend változásait hello alkalmazás viselkedését, valamint a korrelációs toohello központi telepítései.</span><span class="sxs-lookup"><span data-stu-id="0d406-109">Application version is used tooanalyze trend changes in hello application behavior and its correlation toohello deployments.</span></span>

<span data-ttu-id="0d406-110">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="0d406-110">Max length: 1024</span></span>


##<a name="client-ip-address"></a><span data-ttu-id="0d406-111">Ügyfél IP-címe</span><span class="sxs-lookup"><span data-stu-id="0d406-111">Client IP address</span></span>

<span data-ttu-id="0d406-112">hello hello ügyféleszköz IP-címe.</span><span class="sxs-lookup"><span data-stu-id="0d406-112">hello IP address of hello client device.</span></span> <span data-ttu-id="0d406-113">IPv4 és IPv6 használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="0d406-113">IPv4 and IPv6 are supported.</span></span> <span data-ttu-id="0d406-114">Telemetriai adatokat küldött a szolgáltatásból, hello hely környezetben hello szolgáltatásban hello műveletet kezdeményezett hello felhasználóról van.</span><span class="sxs-lookup"><span data-stu-id="0d406-114">When telemetry is sent from a service, hello location context is about hello user that initiated hello operation in hello service.</span></span> <span data-ttu-id="0d406-115">Az Application Insights hello földrajzihely-információk kinyerése hello ügyfél IP-címről, és majd csonkolja azt.</span><span class="sxs-lookup"><span data-stu-id="0d406-115">Application Insights extract hello geo-location information from hello client IP and then truncate it.</span></span> <span data-ttu-id="0d406-116">Ezért ügyfél IP-önmagában nem használható azonosításra alkalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="0d406-116">So client IP by itself cannot be used as end-user identifiable information.</span></span> 

<span data-ttu-id="0d406-117">Maximális hossz: 46</span><span class="sxs-lookup"><span data-stu-id="0d406-117">Max length: 46</span></span>


##<a name="device-type"></a><span data-ttu-id="0d406-118">Eszköz típusa</span><span class="sxs-lookup"><span data-stu-id="0d406-118">Device type</span></span>

<span data-ttu-id="0d406-119">Ez a mező eredetileg hello eszköz hello végfelhasználói hello alkalmazás tooindicate hello típusát.</span><span class="sxs-lookup"><span data-stu-id="0d406-119">Originally this field was used tooindicate hello type of hello device hello end user of hello application is using.</span></span> <span data-ttu-id="0d406-120">Ma hello eszköz típushoz "Browser" a kiszolgálóoldali telemetriai adatokból a hello eszköztípus "PC" használt elsősorban a toodistinguish JavaScript telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="0d406-120">Today used primarily toodistinguish JavaScript telemetry with hello device type 'Browser' from server-side telemetry with hello device type 'PC'.</span></span>

<span data-ttu-id="0d406-121">Maximális hossz: 64</span><span class="sxs-lookup"><span data-stu-id="0d406-121">Max length: 64</span></span>


##<a name="operation-id"></a><span data-ttu-id="0d406-122">A művelet azonosítója</span><span class="sxs-lookup"><span data-stu-id="0d406-122">Operation id</span></span>

<span data-ttu-id="0d406-123">Hello legfelső szintű tevékenység egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0d406-123">A unique identifier of hello root operation.</span></span> <span data-ttu-id="0d406-124">Ez az azonosító lehetővé teszi, hogy toogroup telemetriai több összetevői között.</span><span class="sxs-lookup"><span data-stu-id="0d406-124">This identifier allows toogroup telemetry across multiple components.</span></span> <span data-ttu-id="0d406-125">Lásd: [telemetriai korrelációs](application-insights-correlation.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="0d406-125">See [telemetry correlation](application-insights-correlation.md) for details.</span></span> <span data-ttu-id="0d406-126">hello műveletazonosító kérelmet vagy a lap nézet hozta létre.</span><span class="sxs-lookup"><span data-stu-id="0d406-126">hello operation id is created by either a request or a page view.</span></span> <span data-ttu-id="0d406-127">Egyéb telemetriai beállítja a mező toohello tartozó kérelem vagy a lap nézetet tartalmazó hello.</span><span class="sxs-lookup"><span data-stu-id="0d406-127">All other telemetry sets this field toohello value for hello containing request or page view.</span></span> 

<span data-ttu-id="0d406-128">Maximális hossz: 128</span><span class="sxs-lookup"><span data-stu-id="0d406-128">Max length: 128</span></span>


##<a name="parent-operation-id"></a><span data-ttu-id="0d406-129">Szülő művelet azonosítója</span><span class="sxs-lookup"><span data-stu-id="0d406-129">Parent operation ID</span></span>

<span data-ttu-id="0d406-130">hello azonnali szülő hello telemetriai elem egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0d406-130">hello unique identifier of hello telemetry item's immediate parent.</span></span> <span data-ttu-id="0d406-131">Lásd: [telemetriai korrelációs](application-insights-correlation.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="0d406-131">See [telemetry correlation](application-insights-correlation.md) for details.</span></span>

<span data-ttu-id="0d406-132">Maximális hossz: 128</span><span class="sxs-lookup"><span data-stu-id="0d406-132">Max length: 128</span></span>


##<a name="operation-name"></a><span data-ttu-id="0d406-133">A művelet neve</span><span class="sxs-lookup"><span data-stu-id="0d406-133">Operation name</span></span>

<span data-ttu-id="0d406-134">hello (csoport) hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="0d406-134">hello name (group) of hello operation.</span></span> <span data-ttu-id="0d406-135">hello művelet neve vagy a kérelmet, vagy a lap nézet hozta létre.</span><span class="sxs-lookup"><span data-stu-id="0d406-135">hello operation name is created by either a request or a page view.</span></span> <span data-ttu-id="0d406-136">Telemetriai minden más elemet a hello kérelemmel vagy a lap nézetet tartalmazó mező toohello értékének beállítása.</span><span class="sxs-lookup"><span data-stu-id="0d406-136">All other telemetry items set this field toohello value for hello containing request or page view.</span></span> <span data-ttu-id="0d406-137">Műveletek (például "GET otthoni/Index") a csoport összes hello telemetriai elemek kereséséhez használt művelet neve.</span><span class="sxs-lookup"><span data-stu-id="0d406-137">Operation name is used for finding all hello telemetry items for a group of operations (for example 'GET Home/Index').</span></span> <span data-ttu-id="0d406-138">A context tulajdonság használt tooanswer kérdéseket, például "Mik hello tipikus kivételek ezen az oldalon."</span><span class="sxs-lookup"><span data-stu-id="0d406-138">This context property is used tooanswer questions like "what are hello typical exceptions thrown on this page."</span></span>

<span data-ttu-id="0d406-139">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="0d406-139">Max length: 1024</span></span>


##<a name="synthetic-source-of-hello-operation"></a><span data-ttu-id="0d406-140">Szintetikus forrás hello művelet</span><span class="sxs-lookup"><span data-stu-id="0d406-140">Synthetic source of hello operation</span></span>

<span data-ttu-id="0d406-141">Szintetikus forrás nevére.</span><span class="sxs-lookup"><span data-stu-id="0d406-141">Name of synthetic source.</span></span> <span data-ttu-id="0d406-142">Néhány telemetriai hello alkalmazásból szintetikus forgalom jelöl.</span><span class="sxs-lookup"><span data-stu-id="0d406-142">Some telemetry from hello application may represent synthetic traffic.</span></span> <span data-ttu-id="0d406-143">Lehet, hogy webes webbejáró indexelési hello webhely, hely rendelkezésreállás figyelésére szolgáló tesztek, például maga Application Insights SDK diagnosztikai szalagtárak nyomkövetéseit.</span><span class="sxs-lookup"><span data-stu-id="0d406-143">It may be web crawler indexing hello web site, site availability tests, or traces from diagnostic libraries like Application Insights SDK itself.</span></span>

<span data-ttu-id="0d406-144">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="0d406-144">Max length: 1024</span></span>


##<a name="session-id"></a><span data-ttu-id="0d406-145">munkamenet-azonosító</span><span class="sxs-lookup"><span data-stu-id="0d406-145">Session id</span></span>

<span data-ttu-id="0d406-146">Munkamenet-azonosító - hello felhasználói beavatkozás hello alkalmazással hello példányát.</span><span class="sxs-lookup"><span data-stu-id="0d406-146">Session ID - hello instance of hello user's interaction with hello app.</span></span> <span data-ttu-id="0d406-147">Információk hello munkamenet környezetben mezőiben mindig tárgya hello végfelhasználói.</span><span class="sxs-lookup"><span data-stu-id="0d406-147">Information in hello session context fields is always about hello end user.</span></span> <span data-ttu-id="0d406-148">Telemetriai adatokat küldött a szolgáltatás, a munkamenet-környezet hello hello szolgáltatásban hello műveletet kezdeményezett hello felhasználóról van.</span><span class="sxs-lookup"><span data-stu-id="0d406-148">When telemetry is sent from a service, hello session context is about hello user that initiated hello operation in hello service.</span></span>

<span data-ttu-id="0d406-149">Maximális hossz: 64</span><span class="sxs-lookup"><span data-stu-id="0d406-149">Max length: 64</span></span>


##<a name="anonymous-user-id"></a><span data-ttu-id="0d406-150">A névtelen felhasználói azonosító</span><span class="sxs-lookup"><span data-stu-id="0d406-150">Anonymous user id</span></span>

<span data-ttu-id="0d406-151">A névtelen felhasználói azonosító. Hello végfelhasználói hello alkalmazás jelöli.</span><span class="sxs-lookup"><span data-stu-id="0d406-151">Anonymous user id. Represents hello end user of hello application.</span></span> <span data-ttu-id="0d406-152">Telemetriai adatokat küldött a szolgáltatásból, hello felhasználói környezet hello szolgáltatásban hello műveletet kezdeményezett hello felhasználóról van.</span><span class="sxs-lookup"><span data-stu-id="0d406-152">When telemetry is sent from a service, hello user context is about hello user that initiated hello operation in hello service.</span></span>

<span data-ttu-id="0d406-153">[A mintavételi](app-insights-sampling.md) hello technikák toominimize hello mennyisége gyűjtött telemetriai egyike.</span><span class="sxs-lookup"><span data-stu-id="0d406-153">[Sampling](app-insights-sampling.md) is one of hello techniques toominimize hello amount of collected telemetry.</span></span> <span data-ttu-id="0d406-154">A mintavételi kísérletek tooeither algoritmus minta bejövő vagy kimenő összes hello korrelált telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="0d406-154">Sampling algorithm attempts tooeither sample in or out all hello correlated telemetry.</span></span> <span data-ttu-id="0d406-155">A névtelen felhasználói azonosítót használja a pontszám generációs mintavétel.</span><span class="sxs-lookup"><span data-stu-id="0d406-155">Anonymous user id is used for sampling score generation.</span></span> <span data-ttu-id="0d406-156">Így a névtelen felhasználói azonosító elég véletlenszerű értéknek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0d406-156">So anonymous user id should be a random enough value.</span></span> 

<span data-ttu-id="0d406-157">A névtelen felhasználói azonosító toostore felhasználónév használata a hello mező való visszaélés.</span><span class="sxs-lookup"><span data-stu-id="0d406-157">Using anonymous user id toostore user name is a misuse of hello field.</span></span> <span data-ttu-id="0d406-158">Hitelesített felhasználói azonosítóját használnia.</span><span class="sxs-lookup"><span data-stu-id="0d406-158">Use Authenticated user id.</span></span>

<span data-ttu-id="0d406-159">Maximális hossz: 128</span><span class="sxs-lookup"><span data-stu-id="0d406-159">Max length: 128</span></span>


##<a name="authenticated-user-id"></a><span data-ttu-id="0d406-160">Hitelesített felhasználó azonosítója</span><span class="sxs-lookup"><span data-stu-id="0d406-160">Authenticated user id</span></span>

<span data-ttu-id="0d406-161">Hitelesített felhasználó azonosítója. a névtelen felhasználói azonosítóját, ez a mező ellentétes hello hello felhasználót egy rövid nevet jelöli.</span><span class="sxs-lookup"><span data-stu-id="0d406-161">Authenticated user id. hello opposite of anonymous user id, this field represents hello user with a friendly name.</span></span> <span data-ttu-id="0d406-162">Óta a PII adatok akkor van alapértelmezés szerint nem gyűjt a legtöbb SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="0d406-162">Since its PII information it is not collected by default by most SDK.</span></span>

<span data-ttu-id="0d406-163">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="0d406-163">Max length: 1024</span></span>


##<a name="account-id"></a><span data-ttu-id="0d406-164">Futtatófiók-azonosítóvá</span><span class="sxs-lookup"><span data-stu-id="0d406-164">Account id</span></span>

<span data-ttu-id="0d406-165">A több-bérlős alkalmazásokhoz Ez a hello futtatófiók-Azonosítóvá vagy nevét, mely hello felhasználó működhet együtt.</span><span class="sxs-lookup"><span data-stu-id="0d406-165">In multi-tenant applications this is hello account ID or name, which hello user is acting with.</span></span> <span data-ttu-id="0d406-166">Példák az Azure portál vagy blog neve bloggolás platform előfizetés-azonosító lehet.</span><span class="sxs-lookup"><span data-stu-id="0d406-166">Examples may be subscription ID for Azure portal or blog name blogging platform.</span></span>

<span data-ttu-id="0d406-167">Maximális hossz: 1024</span><span class="sxs-lookup"><span data-stu-id="0d406-167">Max length: 1024</span></span>


##<a name="cloud-role"></a><span data-ttu-id="0d406-168">Felhő szerepkör</span><span class="sxs-lookup"><span data-stu-id="0d406-168">Cloud role</span></span>

<span data-ttu-id="0d406-169">Hello szerepkör hello alkalmazás nevének egy részét képezi.</span><span class="sxs-lookup"><span data-stu-id="0d406-169">Name of hello role hello application is a part of.</span></span> <span data-ttu-id="0d406-170">A Maps közvetlenül toohello szerepkör neve az azure-ban.</span><span class="sxs-lookup"><span data-stu-id="0d406-170">Maps directly toohello role name in azure.</span></span> <span data-ttu-id="0d406-171">Akkor is használt toodistinguish micro szolgáltatásokat, amelyek egyetlen alkalmazás részét képezik.</span><span class="sxs-lookup"><span data-stu-id="0d406-171">Can also be used toodistinguish micro services, which are part of a single application.</span></span>

<span data-ttu-id="0d406-172">Maximális hossz: 256</span><span class="sxs-lookup"><span data-stu-id="0d406-172">Max length: 256</span></span>


##<a name="cloud-role-instance"></a><span data-ttu-id="0d406-173">Felhő szerepkör példánya</span><span class="sxs-lookup"><span data-stu-id="0d406-173">Cloud role instance</span></span>

<span data-ttu-id="0d406-174">Hello alkalmazást futtató hello példány nevét.</span><span class="sxs-lookup"><span data-stu-id="0d406-174">Name of hello instance where hello application is running.</span></span> <span data-ttu-id="0d406-175">Számítógép neve a helyszíni, az Azure-példány nevét.</span><span class="sxs-lookup"><span data-stu-id="0d406-175">Computer name for on-premises, instance name for Azure.</span></span>

<span data-ttu-id="0d406-176">Maximális hossz: 256</span><span class="sxs-lookup"><span data-stu-id="0d406-176">Max length: 256</span></span>


##<a name="internal-sdk-version"></a><span data-ttu-id="0d406-177">Belső: SDK-verzió</span><span class="sxs-lookup"><span data-stu-id="0d406-177">Internal: SDK version</span></span>

<span data-ttu-id="0d406-178">SDK-verzió.</span><span class="sxs-lookup"><span data-stu-id="0d406-178">SDK version.</span></span> <span data-ttu-id="0d406-179">Lásd: https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification információt.</span><span class="sxs-lookup"><span data-stu-id="0d406-179">See https://github.com/Microsoft/ApplicationInsights-Home/blob/master/SDK-AUTHORING.md#sdk-version-specification for information.</span></span>

<span data-ttu-id="0d406-180">Maximális hossz: 64</span><span class="sxs-lookup"><span data-stu-id="0d406-180">Max length: 64</span></span>


##<a name="internal-node-name"></a><span data-ttu-id="0d406-181">Belső: A csomópont neve</span><span class="sxs-lookup"><span data-stu-id="0d406-181">Internal: Node name</span></span>

<span data-ttu-id="0d406-182">Ez a mező képviseli hello csomópontnév számlázási célokat szolgál.</span><span class="sxs-lookup"><span data-stu-id="0d406-182">This field represents hello node name used for billing purposes.</span></span> <span data-ttu-id="0d406-183">Toooverride hello szabványos észlelési csomópontok használni.</span><span class="sxs-lookup"><span data-stu-id="0d406-183">Use it toooverride hello standard detection of nodes.</span></span>

<span data-ttu-id="0d406-184">Maximális hossz: 256</span><span class="sxs-lookup"><span data-stu-id="0d406-184">Max length: 256</span></span>


## <a name="next-steps"></a><span data-ttu-id="0d406-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0d406-185">Next steps</span></span>

- <span data-ttu-id="0d406-186">Ismerje meg, hogyan túl[kiterjesztése és a telemetriai adatok szűrése](app-insights-api-filtering-sampling.md).</span><span class="sxs-lookup"><span data-stu-id="0d406-186">Learn how too[extend and filter telemetry](app-insights-api-filtering-sampling.md).</span></span>
- <span data-ttu-id="0d406-187">Lásd: [adatmodell](application-insights-data-model.md) Application Insights-típusok és az adatok modell.</span><span class="sxs-lookup"><span data-stu-id="0d406-187">See [data model](application-insights-data-model.md) for Application Insights types and data model.</span></span>
- <span data-ttu-id="0d406-188">Tekintse meg a standard környezetben tulajdonsággyűjteményében [konfigurációs](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span><span class="sxs-lookup"><span data-stu-id="0d406-188">Check out standard context properties collection [configuration](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet).</span></span>
