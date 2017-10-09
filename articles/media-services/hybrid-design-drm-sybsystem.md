---
title: "az Azure Media Services DRM subsystem(s) aaaHybrid kialakítása |} Microsoft Docs"
description: "Ez a témakör ismerteti az Azure Media Services használatával DRM subsystem(s) hibrid kialakítással."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: willzhan;juliako
ms.openlocfilehash: 4206248420ccd4dbfc9a87a86f4763534c6254a1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hybrid-design-of-drm-subsystems"></a><span data-ttu-id="db202-103">DRM subsystem(s) hibrid kialakítása</span><span class="sxs-lookup"><span data-stu-id="db202-103">Hybrid design of DRM subsystem(s)</span></span>

<span data-ttu-id="db202-104">Ez a témakör ismerteti az Azure Media Services használatával DRM subsystem(s) hibrid kialakítással.</span><span class="sxs-lookup"><span data-stu-id="db202-104">This topic discusses hybrid design of DRM subsystem(s) using Azure Media Services.</span></span>

## <a name="overview"></a><span data-ttu-id="db202-105">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="db202-105">Overview</span></span>

<span data-ttu-id="db202-106">Az Azure Media Services támogatást nyújt a következő három DRM rendszer hello:</span><span class="sxs-lookup"><span data-stu-id="db202-106">Azure Media Services provides support for hello following three DRM system:</span></span>

* <span data-ttu-id="db202-107">PlayReady</span><span class="sxs-lookup"><span data-stu-id="db202-107">PlayReady</span></span>
* <span data-ttu-id="db202-108">Widevine (moduláris)</span><span class="sxs-lookup"><span data-stu-id="db202-108">Widevine (Modular)</span></span>
* <span data-ttu-id="db202-109">FairPlay</span><span class="sxs-lookup"><span data-stu-id="db202-109">FairPlay</span></span>

<span data-ttu-id="db202-110">hello DRM-támogatást tartalmaz, amelynek Azure Media Player támogató összes 3 DRMs adott böngésző SDK DRM titkosítás (a dinamikus titkosítás) és a licenc kézbesítési.</span><span class="sxs-lookup"><span data-stu-id="db202-110">hello DRM support includes DRM encryption (dynamic encryption) and license delivery, with Azure Media Player supporting all 3 DRMs as a browser player SDK.</span></span>

<span data-ttu-id="db202-111">Részletes kezelésére DRM/CENC alrendszer megtervezését és megvalósítását, lásd: hello dokumentum című [Multi-DRM-és hozzáférés-vezérlés CENC](media-services-cenc-with-multidrm-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="db202-111">For a detailed treatment of DRM/CENC subsystem design and implementation, please see hello document titled [CENC with Multi-DRM and Access Control](media-services-cenc-with-multidrm-access-control.md).</span></span>

<span data-ttu-id="db202-112">Bár a teljes mértékben támogatják a három DRM rendszerek kínálunk, néha kell toouse hozzáadása tooAzure Media Services toobuild DRM alrendszer hibrid saját infrastruktúra/alrendszereket különböző részeit.</span><span class="sxs-lookup"><span data-stu-id="db202-112">Although we offer complete support for three DRM systems, sometimes customers need toouse various parts of their own infrastructure/subsystems in addition tooAzure Media Services toobuild a hybrid DRM subsystem.</span></span>

<span data-ttu-id="db202-113">Alább néhány általános kérdés felkérést ügyfelek:</span><span class="sxs-lookup"><span data-stu-id="db202-113">Below are some common questions asked by customers:</span></span>

* <span data-ttu-id="db202-114">"Használható saját DRM licenckiszolgálókat?"</span><span class="sxs-lookup"><span data-stu-id="db202-114">"Can I use my own DRM license servers?"</span></span> <span data-ttu-id="db202-115">(Ebben az esetben az ügyfelek befektettek DRM licenc kiszolgálófarm beágyazott üzleti logika).</span><span class="sxs-lookup"><span data-stu-id="db202-115">(In this case, customers have invested in DRM license server farm with embedded business logic).</span></span>
* <span data-ttu-id="db202-116">"Használható csak a DRM licenc kézbesítési Azure Media Services nélkül AMS tartalma üzemeltető?"</span><span class="sxs-lookup"><span data-stu-id="db202-116">"Can I use only your DRM license delivery in Azure Media Services without hosting content in AMS?"</span></span>

## <a name="modularity-of-hello-ams-drm-platform"></a><span data-ttu-id="db202-117">Az AMS DRM platform hello modularitás</span><span class="sxs-lookup"><span data-stu-id="db202-117">Modularity of hello AMS DRM platform</span></span>

<span data-ttu-id="db202-118">Egy kiterjedt funkciókészlettel ellátott felhőbeli videó platform részeként Azure Media Services DRM rugalmasságot és modularitást biztosítson a kialakítás rendelkezik szem előtt.</span><span class="sxs-lookup"><span data-stu-id="db202-118">As part of a comprehensive cloud video platform, Azure Media Services DRM has a design with flexibility and modularity in mind.</span></span> <span data-ttu-id="db202-119">Azure Media Services a következő táblázatban leírt hello az alábbi (annak magyarázatát, hello notation hello tábla szerepel a következő) különböző kombinációkban hello bármelyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="db202-119">You can use Azure Media Services with any of hello following different combinations described in hello table below (an explanation of hello notation used in hello table follows).</span></span> 

|<span data-ttu-id="db202-120">**Tartalom üzemeltető & forrása**</span><span class="sxs-lookup"><span data-stu-id="db202-120">**Content hosting & origin**</span></span>|<span data-ttu-id="db202-121">**Tartalom titkosítása**</span><span class="sxs-lookup"><span data-stu-id="db202-121">**Content encryption**</span></span>|<span data-ttu-id="db202-122">**DRM-licenckézbesítés**</span><span class="sxs-lookup"><span data-stu-id="db202-122">**DRM license delivery**</span></span>|
|---|---|---|
|<span data-ttu-id="db202-123">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-123">AMS</span></span>|<span data-ttu-id="db202-124">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-124">AMS</span></span>|<span data-ttu-id="db202-125">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-125">AMS</span></span>|
|<span data-ttu-id="db202-126">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-126">AMS</span></span>|<span data-ttu-id="db202-127">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-127">AMS</span></span>|<span data-ttu-id="db202-128">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-128">Third-party</span></span>|
|<span data-ttu-id="db202-129">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-129">AMS</span></span>|<span data-ttu-id="db202-130">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-130">Third-party</span></span>|<span data-ttu-id="db202-131">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-131">AMS</span></span>|
|<span data-ttu-id="db202-132">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-132">AMS</span></span>|<span data-ttu-id="db202-133">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-133">Third-party</span></span>|<span data-ttu-id="db202-134">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-134">Third-party</span></span>|
|<span data-ttu-id="db202-135">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-135">Third-party</span></span>|<span data-ttu-id="db202-136">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-136">Third-party</span></span>|<span data-ttu-id="db202-137">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-137">AMS</span></span>|

### <a name="content-hosting--origin"></a><span data-ttu-id="db202-138">Tartalom üzemeltető & forrása</span><span class="sxs-lookup"><span data-stu-id="db202-138">Content hosting & origin</span></span>

* <span data-ttu-id="db202-139">AMS: video asset tárolódik AMS és adatfolyam-AMS streamvégpontok (de nem feltétlenül dinamikus becsomagolás) keresztül.</span><span class="sxs-lookup"><span data-stu-id="db202-139">AMS: video asset is hosted in AMS and streaming is through AMS streaming endpoints (but not necessarily dynamic packaging).</span></span>
* <span data-ttu-id="db202-140">Külső: videó üzemeltetett és egy külső adatfolyam platform kívül AMS-i.</span><span class="sxs-lookup"><span data-stu-id="db202-140">Third-party: video is hosted and delivered on a third-party streaming platform outside of AMS.</span></span>

### <a name="content-encryption"></a><span data-ttu-id="db202-141">Tartalom titkosítása</span><span class="sxs-lookup"><span data-stu-id="db202-141">Content encryption</span></span>

* <span data-ttu-id="db202-142">AMS: tartalomtitkosító elvégezni dinamikusan/igény szerint AMS a dinamikus titkosítás.</span><span class="sxs-lookup"><span data-stu-id="db202-142">AMS: content encryption is performed dynamically/on-demand by AMS dynamic encryption.</span></span>
* <span data-ttu-id="db202-143">Külső: tartalomtitkosító előre feldolgozásra munkafolyamattal AMS kívül történik.</span><span class="sxs-lookup"><span data-stu-id="db202-143">Third-party: content encryption is performed outside of AMS using a pre-processing workflow.</span></span>

### <a name="drm-license-delivery"></a><span data-ttu-id="db202-144">DRM-licenckézbesítés</span><span class="sxs-lookup"><span data-stu-id="db202-144">DRM license delivery</span></span>

* <span data-ttu-id="db202-145">AMS: DRM licenc hozta AMS licenctovábbítási szolgáltatásra.</span><span class="sxs-lookup"><span data-stu-id="db202-145">AMS: DRM license is delivered by AMS license delivery service.</span></span>
* <span data-ttu-id="db202-146">Külső: DRM-licenc egy külső DRM licenckiszolgáló AMS kívül érkeznek.</span><span class="sxs-lookup"><span data-stu-id="db202-146">Third-party: DRM license is delivered by a third-party DRM license server outside of AMS.</span></span>

## <a name="configure-based-on-your-hybrid-scenario"></a><span data-ttu-id="db202-147">Konfigurálja a hibrid forgatókönyvek alapján</span><span class="sxs-lookup"><span data-stu-id="db202-147">Configure based on your hybrid scenario</span></span>

### <a name="content-key"></a><span data-ttu-id="db202-148">Tartalomkulcs</span><span class="sxs-lookup"><span data-stu-id="db202-148">Content key</span></span>

<span data-ttu-id="db202-149">Tartalomkulcs beállításon szabályozhatja az AMS a dinamikus titkosítás és a szállítási AMS-licencelési szolgáltatástól tulajdonságait a következő hello:</span><span class="sxs-lookup"><span data-stu-id="db202-149">Through configuration of a content key, you can control hello following attributes of both AMS dynamic encryption and AMS license delivery service:</span></span>

* <span data-ttu-id="db202-150">hello tartalomkulcsot dinamikus DRM-titkosítást használja.</span><span class="sxs-lookup"><span data-stu-id="db202-150">hello content key used for dynamic DRM encryption.</span></span>
* <span data-ttu-id="db202-151">DRM licenc tartalom toobe licenctovábbítási szolgáltatások kézbesítette: jogosultságokat, tartalomkulcsot és korlátozásokat.</span><span class="sxs-lookup"><span data-stu-id="db202-151">DRM license content toobe delivered by license delivery services: rights, content key and restrictions.</span></span>
* <span data-ttu-id="db202-152">Írja be a **hitelesítési házirend-korlátozás tartalom**: IP- vagy a token korlátozás megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="db202-152">Type of **content key authorization policy restriction**: open, IP, or token restriction.</span></span>
* <span data-ttu-id="db202-153">Ha **token** típusú **szolgál a tartalomkulcs-hitelesítési házirend-korlátozás**, hello **hitelesítési házirend-korlátozás tartalom** teljesülniük kell ahhoz licencet ad ki.</span><span class="sxs-lookup"><span data-stu-id="db202-153">If **token** type of **content key authorization policy restriction is used**, hello **content key authorization policy restriction** must be met before a license is issued.</span></span>

### <a name="asset-delivery-policy"></a><span data-ttu-id="db202-154">Objektumtovábbítási szabályzat</span><span class="sxs-lookup"><span data-stu-id="db202-154">Asset delivery policy</span></span>

<span data-ttu-id="db202-155">Objektumtovábbítási szabályzat konfigurációját, a következő AMS dinamikus csomagoló és egy adatfolyam-továbbítási végpontra AMS a dinamikus titkosítás által használt attribútumok hello szabályozhatja:</span><span class="sxs-lookup"><span data-stu-id="db202-155">Through configuration of an asset delivery policy, you can control hello following attributes used by AMS dynamic packager and dynamic encryption of an AMS streaming endpoint:</span></span>

* <span data-ttu-id="db202-156">Folyamatos átviteli protokoll és DRM titkosítási kombináció, például a CENC (PlayReady és Widevine), KÖTŐJELET sima a PlayReady, alatt áll, a PlayReady vagy Widevine HLS streamelési.</span><span class="sxs-lookup"><span data-stu-id="db202-156">Streaming protocol and DRM encryption combination, such as DASH under CENC (PlayReady and Widevine), smooth streaming under PlayReady, HLS under Widevine or PlayReady.</span></span>
* <span data-ttu-id="db202-157">hello alapértelmezett/beágyazott licenc kézbesítési URL-címeket az egyes hello DRMs szerepet játszanak.</span><span class="sxs-lookup"><span data-stu-id="db202-157">hello default/embedded license delivery URLs for each of hello involved DRMs.</span></span>
* <span data-ttu-id="db202-158">E licenc licenckérési URL-címeit (LA_URLs) DASH MPD vagy HLS lista tartalmaz kulcs azonosítója (KID) lekérdezési karakterláncot a Widevine és FairPlay, illetve.</span><span class="sxs-lookup"><span data-stu-id="db202-158">Whether license acquisition URLs (LA_URLs) in DASH MPD or HLS playlist contain query string of key ID (KID) for Widevine and FairPlay, respectively.</span></span>

## <a name="scenarios-and-samples"></a><span data-ttu-id="db202-159">Forgatókönyvek és minták</span><span class="sxs-lookup"><span data-stu-id="db202-159">Scenarios and samples</span></span>

<span data-ttu-id="db202-160">Hello magyarázatokat hello előző részben leírtak alapján, a következő öt hibrid forgatókönyvek hello használata megfelelő **tartalomkulcs**-**objektumtovábbítási szabályzat** konfigurációkat támogat (hello hello utolsó oszlopában szereplő minták kövesse hello táblázatban):</span><span class="sxs-lookup"><span data-stu-id="db202-160">Based on hello explanations in hello previous section, hello following five hybrid scenarios use respective **Content key**-**Asset delivery policy** configuration combinations (hello samples mentioned in hello last column follow hello table):</span></span>

|<span data-ttu-id="db202-161">**Tartalom üzemeltető & forrása**</span><span class="sxs-lookup"><span data-stu-id="db202-161">**Content hosting & origin**</span></span>|<span data-ttu-id="db202-162">**DRM-titkosítás**</span><span class="sxs-lookup"><span data-stu-id="db202-162">**DRM encryption**</span></span>|<span data-ttu-id="db202-163">**DRM-licenckézbesítés**</span><span class="sxs-lookup"><span data-stu-id="db202-163">**DRM license delivery**</span></span>|<span data-ttu-id="db202-164">**Konfigurálja a tartalomkulcs**</span><span class="sxs-lookup"><span data-stu-id="db202-164">**Configure content key**</span></span>|<span data-ttu-id="db202-165">**Objektumtovábbítási szabályzat konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="db202-165">**Configure asset delivery policy**</span></span>|<span data-ttu-id="db202-166">**Minta**</span><span class="sxs-lookup"><span data-stu-id="db202-166">**Sample**</span></span>|
|---|---|---|---|---|---|
|<span data-ttu-id="db202-167">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-167">AMS</span></span>|<span data-ttu-id="db202-168">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-168">AMS</span></span>|<span data-ttu-id="db202-169">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-169">AMS</span></span>|<span data-ttu-id="db202-170">Igen</span><span class="sxs-lookup"><span data-stu-id="db202-170">Yes</span></span>|<span data-ttu-id="db202-171">Igen</span><span class="sxs-lookup"><span data-stu-id="db202-171">Yes</span></span>|<span data-ttu-id="db202-172">1. példa</span><span class="sxs-lookup"><span data-stu-id="db202-172">Sample 1</span></span>|
|<span data-ttu-id="db202-173">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-173">AMS</span></span>|<span data-ttu-id="db202-174">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-174">AMS</span></span>|<span data-ttu-id="db202-175">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-175">Third-party</span></span>|<span data-ttu-id="db202-176">Igen</span><span class="sxs-lookup"><span data-stu-id="db202-176">Yes</span></span>|<span data-ttu-id="db202-177">Igen</span><span class="sxs-lookup"><span data-stu-id="db202-177">Yes</span></span>|<span data-ttu-id="db202-178">2. példa</span><span class="sxs-lookup"><span data-stu-id="db202-178">Sample 2</span></span>|
|<span data-ttu-id="db202-179">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-179">AMS</span></span>|<span data-ttu-id="db202-180">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-180">Third-party</span></span>|<span data-ttu-id="db202-181">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-181">AMS</span></span>|<span data-ttu-id="db202-182">Igen</span><span class="sxs-lookup"><span data-stu-id="db202-182">Yes</span></span>|<span data-ttu-id="db202-183">Nem</span><span class="sxs-lookup"><span data-stu-id="db202-183">No</span></span>|<span data-ttu-id="db202-184">3. példa</span><span class="sxs-lookup"><span data-stu-id="db202-184">Sample 3</span></span>|
|<span data-ttu-id="db202-185">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-185">AMS</span></span>|<span data-ttu-id="db202-186">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-186">Third-party</span></span>|<span data-ttu-id="db202-187">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-187">Outside</span></span>|<span data-ttu-id="db202-188">Nem</span><span class="sxs-lookup"><span data-stu-id="db202-188">No</span></span>|<span data-ttu-id="db202-189">Nem</span><span class="sxs-lookup"><span data-stu-id="db202-189">No</span></span>|<span data-ttu-id="db202-190">4. példa</span><span class="sxs-lookup"><span data-stu-id="db202-190">Sample 4</span></span>|
|<span data-ttu-id="db202-191">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-191">Third-party</span></span>|<span data-ttu-id="db202-192">Külső</span><span class="sxs-lookup"><span data-stu-id="db202-192">Third-party</span></span>|<span data-ttu-id="db202-193">AMS</span><span class="sxs-lookup"><span data-stu-id="db202-193">AMS</span></span>|<span data-ttu-id="db202-194">Igen</span><span class="sxs-lookup"><span data-stu-id="db202-194">Yes</span></span>|<span data-ttu-id="db202-195">Nem</span><span class="sxs-lookup"><span data-stu-id="db202-195">No</span></span>|    

<span data-ttu-id="db202-196">Hello minták, a PlayReady-védelem működik DASH vagy smooth streaming.</span><span class="sxs-lookup"><span data-stu-id="db202-196">In hello samples, PlayReady protection works for both DASH and smooth streaming.</span></span> <span data-ttu-id="db202-197">hello Videó URL-címeket az alábbi smooth streaming URL-címek.</span><span class="sxs-lookup"><span data-stu-id="db202-197">hello video URLs below are smooth streaming URLs.</span></span> <span data-ttu-id="db202-198">tooget hello megfelelő vonal URL-címek, csak hozzáfűző "(formátum = mpd-idő-csf)".</span><span class="sxs-lookup"><span data-stu-id="db202-198">tooget hello corresponding DASH URLs, just append "(format=mpd-time-csf)".</span></span> <span data-ttu-id="db202-199">Hello használata [azure media player tesztelése](http://aka.ms/amtest) tootest a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="db202-199">You could use hello [azure media test player](http://aka.ms/amtest) tootest in a browser.</span></span> <span data-ttu-id="db202-200">Ez lehetővé teszi tooconfigure amely adatfolyam-protokoll toouse, mely műszaki alatt.</span><span class="sxs-lookup"><span data-stu-id="db202-200">It allows you tooconfigure which streaming protocol toouse, under which tech.</span></span> <span data-ttu-id="db202-201">IE11 és a Windows 10 MS Edge támogatja a PlayReady EME keresztül.</span><span class="sxs-lookup"><span data-stu-id="db202-201">IE11 and MS Edge on Windows 10 support PlayReady through EME.</span></span> <span data-ttu-id="db202-202">További információkért lásd: [hello adatait vizsgálati eszköz](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/).</span><span class="sxs-lookup"><span data-stu-id="db202-202">For more information, see [details about hello test tool](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/).</span></span>

### <a name="sample-1"></a><span data-ttu-id="db202-203">1. példa</span><span class="sxs-lookup"><span data-stu-id="db202-203">Sample 1</span></span>

* <span data-ttu-id="db202-204">Forrás (alap) URL-címe: https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest</span><span class="sxs-lookup"><span data-stu-id="db202-204">Source (base) URL: https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest</span></span> 
* <span data-ttu-id="db202-205">PlayReady-LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span><span class="sxs-lookup"><span data-stu-id="db202-205">PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span></span> 
* <span data-ttu-id="db202-206">Widevine LA_URL (DASH): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4</span><span class="sxs-lookup"><span data-stu-id="db202-206">Widevine LA_URL (DASH): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4</span></span> 
* <span data-ttu-id="db202-207">FairPlay LA_URL (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8</span><span class="sxs-lookup"><span data-stu-id="db202-207">FairPlay LA_URL (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8</span></span> 

### <a name="sample-2"></a><span data-ttu-id="db202-208">2. példa</span><span class="sxs-lookup"><span data-stu-id="db202-208">Sample 2</span></span>

* <span data-ttu-id="db202-209">Forrás (alap) URL-címe: http://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest</span><span class="sxs-lookup"><span data-stu-id="db202-209">Source (base) URL: http://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest</span></span> 
* <span data-ttu-id="db202-210">PlayReady-LA_URL (DASH & smooth): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx</span><span class="sxs-lookup"><span data-stu-id="db202-210">PlayReady LA_URL (DASH & smooth): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx</span></span> 

### <a name="sample-3"></a><span data-ttu-id="db202-211">3. példa</span><span class="sxs-lookup"><span data-stu-id="db202-211">Sample 3</span></span>

* <span data-ttu-id="db202-212">Forrás URL-címe: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest</span><span class="sxs-lookup"><span data-stu-id="db202-212">Source URL: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest</span></span> 
* <span data-ttu-id="db202-213">PlayReady-LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span><span class="sxs-lookup"><span data-stu-id="db202-213">PlayReady LA_URL (DASH & smooth): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/</span></span> 

### <a name="sample-4"></a><span data-ttu-id="db202-214">4. példa</span><span class="sxs-lookup"><span data-stu-id="db202-214">Sample 4</span></span>

* <span data-ttu-id="db202-215">Forrás URL-címe: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest</span><span class="sxs-lookup"><span data-stu-id="db202-215">Source URL: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest</span></span> 
* <span data-ttu-id="db202-216">PlayReady-LA_URL (DASH & smooth): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx</span><span class="sxs-lookup"><span data-stu-id="db202-216">PlayReady LA_URL (DASH & smooth): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx</span></span> 

## <a name="summary"></a><span data-ttu-id="db202-217">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="db202-217">Summary</span></span>

<span data-ttu-id="db202-218">Az összegzés Azure Media Services DRM-összetevők rugalmas, használhatja őket egy hibrid forgatókönyvben megfelelően konfigurálja a tartalomkulcs és az adategység továbbítási házirendjét, ebben a témakörben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="db202-218">In summary, Azure Media Services DRM components are flexible, you can use them in a hybrid scenario by properly configuring content key and asset delivery policy, as described in this topic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="db202-219">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="db202-219">Next steps</span></span>
<span data-ttu-id="db202-220">Nézet Media Services tanulási útvonalai.</span><span class="sxs-lookup"><span data-stu-id="db202-220">View Media Services learning paths.</span></span>

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="db202-221">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="db202-221">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

