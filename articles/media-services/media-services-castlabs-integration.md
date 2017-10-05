---
title: "Widevine-licencek kézbesíthet Azure Media Services castLabs használatával |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan használható az Azure Media Services (AMS) által a PlayReady vagy a Widevine DRMs AMS dinamikusan titkosított adatfolyam továbbítására. A PlayReady-licenc Media Services PlayReady licenckiszolgáló származik, és Widevine-licenc castLabs licenckiszolgáló hozta."
services: media-services
documentationcenter: 
author: Mingfeiy
manager: cfowler
editor: 
ms.assetid: 2a9a408a-a995-49e1-8d8f-ac5b51e17d40
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: Mingfeiy;willzhan;Juliako
ms.openlocfilehash: 5b69e804809f834e81221fb2787a997a52dbe286
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="using-castlabs-to-deliver-widevine-licenses-to-azure-media-services"></a><span data-ttu-id="81970-104">A castLabs használata a Widevine-licencek közvetítéséhez az Azure Media Servicesbe</span><span class="sxs-lookup"><span data-stu-id="81970-104">Using castLabs to deliver Widevine licenses to Azure Media Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="81970-105">Axinom</span><span class="sxs-lookup"><span data-stu-id="81970-105">Axinom</span></span>](media-services-axinom-integration.md)
> * [<span data-ttu-id="81970-106">castLabs</span><span class="sxs-lookup"><span data-stu-id="81970-106">castLabs</span></span>](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="81970-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="81970-107">Overview</span></span>
<span data-ttu-id="81970-108">Ez a cikk ismerteti, hogyan használható az Azure Media Services (AMS) által a PlayReady vagy a Widevine DRMs AMS dinamikusan titkosított adatfolyam továbbítására.</span><span class="sxs-lookup"><span data-stu-id="81970-108">This article describes how you can use Azure Media Services (AMS) to deliver a stream that is dynamically encrypted by AMS with both PlayReady and Widevine DRMs.</span></span> <span data-ttu-id="81970-109">A PlayReady-licenc származik a licenckiszolgáló Media Services PlayReady és Widevine-licenc hozta **castLabs** licenckiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="81970-109">The PlayReady license comes from Media Services PlayReady license server and Widevine license is delivered by **castLabs** license server.</span></span>

<span data-ttu-id="81970-110">Adatfolyam-CENC (PlayReady és/vagy Widevine) által védett tartalmak lejátszásához, használhatja a [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="81970-110">To playback streaming content protected by CENC (PlayReady and/or Widevine), you can use  [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span> <span data-ttu-id="81970-111">Lásd: [AMP dokumentum](http://amp.azure.net/libs/amp/latest/docs/) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="81970-111">See [AMP document](http://amp.azure.net/libs/amp/latest/docs/) for details.</span></span>

<span data-ttu-id="81970-112">A következő ábra azt mutatja be, a magas szintű Azure Media Services és a castLabs integrációs architektúra.</span><span class="sxs-lookup"><span data-stu-id="81970-112">The following diagram demonstrates a high-level Azure Media Services and castLabs integration architecture.</span></span>

![Integráció](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a><span data-ttu-id="81970-114">Tipikus rendszer beállítása</span><span class="sxs-lookup"><span data-stu-id="81970-114">Typical system set up</span></span>
* <span data-ttu-id="81970-115">A médiatartalom AMS tárolja.</span><span class="sxs-lookup"><span data-stu-id="81970-115">Media content is stored in AMS.</span></span>
* <span data-ttu-id="81970-116">Kulcs azonosítók tartalom kulcsok castLabs, mind az AMS vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="81970-116">Key IDs of content keys are stored in both castLabs and AMS.</span></span>
* <span data-ttu-id="81970-117">castLabs és AMS egyaránt rendelkezik beépített tokent használó hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="81970-117">castLabs and AMS both have token authentication built in.</span></span> <span data-ttu-id="81970-118">Az alábbi szakaszok ismertetik a hitelesítési tokenek.</span><span class="sxs-lookup"><span data-stu-id="81970-118">The following sections discuss authentication tokens.</span></span> 
* <span data-ttu-id="81970-119">Amikor egy ügyfél történő folyamatos igényel, a tartalom dinamikusan titkosított rendelkező **Common Encryption** (CENC) és a Smooth Streaming, valamint a kötőjel AMS által dinamikusan csomagolni.</span><span class="sxs-lookup"><span data-stu-id="81970-119">When a client requests to stream the video, the content is dynamically encrypted with **Common Encryption** (CENC) and dynamically packaged by AMS to Smooth Streaming and DASH.</span></span> <span data-ttu-id="81970-120">Azt is biztosítanak a HLS streamelési protokoll PlayReady M2TS elemi adatfolyam titkosítását.</span><span class="sxs-lookup"><span data-stu-id="81970-120">We also deliver PlayReady M2TS elementary stream encryption for HLS streaming protocol.</span></span>
* <span data-ttu-id="81970-121">PlayReady-licenc AMS licenckiszolgáló kéri le a rendszer, és Widevine-licenc castLabs licenckiszolgáló kéri le a rendszer.</span><span class="sxs-lookup"><span data-stu-id="81970-121">PlayReady license is retrieved from AMS license server and Widevine license is retrieved from castLabs license server.</span></span> 
* <span data-ttu-id="81970-122">Media Player automatikusan eldönti, mely licenc beolvasni az ügyfél platform képességei alapján.</span><span class="sxs-lookup"><span data-stu-id="81970-122">Media Player automatically decides which license to fetch based on the client platform capability.</span></span> 

## <a name="authentication-token-generation-for-getting-a-license"></a><span data-ttu-id="81970-123">Hitelesítési jogkivonatok létrehozásához a licenc beolvasásakor</span><span class="sxs-lookup"><span data-stu-id="81970-123">Authentication token generation for getting a license</span></span>
<span data-ttu-id="81970-124">CastLabs, mind az AMS támogatja (JSON Web Token) JWT jogkivonat formátumának engedélyezése licenccel.</span><span class="sxs-lookup"><span data-stu-id="81970-124">Both castLabs and AMS support JWT (JSON Web Token) token format used to authorize a license.</span></span> 

### <a name="jwt-token-in-ams"></a><span data-ttu-id="81970-125">Az AMS JWT jogkivonat</span><span class="sxs-lookup"><span data-stu-id="81970-125">JWT token in AMS</span></span>
<span data-ttu-id="81970-126">A következő táblázat ismerteti az AMS JWT jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="81970-126">The following table describes JWT token in AMS.</span></span> 

| <span data-ttu-id="81970-127">Kiállító</span><span class="sxs-lookup"><span data-stu-id="81970-127">Issuer</span></span> | <span data-ttu-id="81970-128">A választott kibocsátó karakterláncot Secure Token Service (STS)</span><span class="sxs-lookup"><span data-stu-id="81970-128">Issuer string from the chosen Secure Token Service (STS)</span></span> |
| --- | --- |
| <span data-ttu-id="81970-129">Célközönség</span><span class="sxs-lookup"><span data-stu-id="81970-129">Audience</span></span> |<span data-ttu-id="81970-130">A használt STS a célközönség karakterlánc</span><span class="sxs-lookup"><span data-stu-id="81970-130">Audience string from the used STS</span></span> |
| <span data-ttu-id="81970-131">Jogcímek</span><span class="sxs-lookup"><span data-stu-id="81970-131">Claims</span></span> |<span data-ttu-id="81970-132">Jogcímek egy készletét</span><span class="sxs-lookup"><span data-stu-id="81970-132">A set of claims</span></span> |
| <span data-ttu-id="81970-133">NotBefore</span><span class="sxs-lookup"><span data-stu-id="81970-133">NotBefore</span></span> |<span data-ttu-id="81970-134">A token érvényességének kezdő</span><span class="sxs-lookup"><span data-stu-id="81970-134">Start validity of the token</span></span> |
| <span data-ttu-id="81970-135">Lejár</span><span class="sxs-lookup"><span data-stu-id="81970-135">Expires</span></span> |<span data-ttu-id="81970-136">A token érvényességének vége</span><span class="sxs-lookup"><span data-stu-id="81970-136">End validity of the token</span></span> |
| <span data-ttu-id="81970-137">SigningCredentials</span><span class="sxs-lookup"><span data-stu-id="81970-137">SigningCredentials</span></span> |<span data-ttu-id="81970-138">A kulcs, amelyet használ PlayReady licenckiszolgáló, castLabs licenckiszolgáló és STS, annak oka lehet szimmetrikus vagy aszimmetrikus kulcs.</span><span class="sxs-lookup"><span data-stu-id="81970-138">The key that is shared among PlayReady License Server, castLabs License Server and STS, it could be either symmetric or asymmetric key.</span></span> |

### <a name="jwt-token-in-castlabs"></a><span data-ttu-id="81970-139">A castLabs JWT jogkivonat</span><span class="sxs-lookup"><span data-stu-id="81970-139">JWT token in castLabs</span></span>
<span data-ttu-id="81970-140">A következő táblázat ismerteti a castLabs JWT jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="81970-140">The following table describes JWT token in castLabs.</span></span> 

| <span data-ttu-id="81970-141">Név</span><span class="sxs-lookup"><span data-stu-id="81970-141">Name</span></span> | <span data-ttu-id="81970-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="81970-142">Description</span></span> |
| --- | --- |
| <span data-ttu-id="81970-143">optData</span><span class="sxs-lookup"><span data-stu-id="81970-143">optData</span></span> |<span data-ttu-id="81970-144">Egy adatokat tartalmazó JSON-karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="81970-144">A JSON string containing information about you.</span></span> |
| <span data-ttu-id="81970-145">CRT</span><span class="sxs-lookup"><span data-stu-id="81970-145">crt</span></span> |<span data-ttu-id="81970-146">Az eszköz adatainak tartalmazó JSON karakterláncnak a licencelési adatokat, és a lejátszás jogok.</span><span class="sxs-lookup"><span data-stu-id="81970-146">A JSON string containing information about the asset, its license info and playback rights.</span></span> |
| <span data-ttu-id="81970-147">IAT</span><span class="sxs-lookup"><span data-stu-id="81970-147">iat</span></span> |<span data-ttu-id="81970-148">Az aktuális dátum és idő a epoch.</span><span class="sxs-lookup"><span data-stu-id="81970-148">The current datetime in epoch.</span></span> |
| <span data-ttu-id="81970-149">jti</span><span class="sxs-lookup"><span data-stu-id="81970-149">jti</span></span> |<span data-ttu-id="81970-150">Ez a token (összes jogkivonat csak egyszer használható a castLabs rendszerben) kapcsolatos egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="81970-150">A unique identifier about this token (every token can only be used once in the castLabs system).</span></span> |

## <a name="sample-solution-set-up"></a><span data-ttu-id="81970-151">A minta megoldás beállítása</span><span class="sxs-lookup"><span data-stu-id="81970-151">Sample solution set up</span></span>
<span data-ttu-id="81970-152">A [megoldás minta](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) két projektet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="81970-152">The [sample solution](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) consists of two projects:</span></span>

* <span data-ttu-id="81970-153">Egy konzolalkalmazás használható egy már feldolgozott eszköz, mind a PlayReady, mind a Widevine DRM korlátozások beállítása.</span><span class="sxs-lookup"><span data-stu-id="81970-153">A console app that can be used to set DRM restrictions on an already ingested asset, for both PlayReady and Widevine.</span></span>
* <span data-ttu-id="81970-154">A webes alkalmazás, amely átadja a jogkivonatokat, ami az STS szolgáltatással nagyon egyszerűsített verziója sikerült tekinthető meg.</span><span class="sxs-lookup"><span data-stu-id="81970-154">A Web Application that hands out tokens, which could be seen as a VERY SIMPLIFIED version of an STS.</span></span>

<span data-ttu-id="81970-155">A Konzolalkalmazás használata:</span><span class="sxs-lookup"><span data-stu-id="81970-155">To use the console application:</span></span>

1. <span data-ttu-id="81970-156">Módosítsa az app.config AMS hitelesítő adatokat, a castLabs hitelesítő adatokat, a STS konfigurációs és a megosztott kulcs beállítása.</span><span class="sxs-lookup"><span data-stu-id="81970-156">Change the app.config to setup AMS credentials, castLabs credentials, STS configuration and shared key.</span></span>
2. <span data-ttu-id="81970-157">Töltse fel az eszköz az AMS.</span><span class="sxs-lookup"><span data-stu-id="81970-157">Upload an Asset into AMS.</span></span>
3. <span data-ttu-id="81970-158">Az UUID azonosító beszerzése a feltöltött eszköz, és módosítsa a sor 32 a Program.cs fájlban:</span><span class="sxs-lookup"><span data-stu-id="81970-158">Get the UUID from the uploaded Asset, and change Line 32 in the Program.cs file:</span></span>
   
      <span data-ttu-id="81970-159">var objIAsset = _context. Assets.Where (x = > x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf"). FirstOrDefault();</span><span class="sxs-lookup"><span data-stu-id="81970-159">var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();</span></span>
4. <span data-ttu-id="81970-160">Használjon egy AssetId elnevezési az eszköz a castLabs rendszerben (sor 44 a Program.cs fájlban).</span><span class="sxs-lookup"><span data-stu-id="81970-160">Use an AssetId for naming the asset in the castLabs system (Line 44 in the Program.cs file).</span></span>
   
   <span data-ttu-id="81970-161">Meg kell adni a AssetId **castLabs**; kell lennie egy egyedi alfanumerikus karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="81970-161">You must set AssetId for **castLabs**; it needs to be a unique alphanumeric string.</span></span>
5. <span data-ttu-id="81970-162">Futtassa a programot.</span><span class="sxs-lookup"><span data-stu-id="81970-162">Run the program.</span></span>

<span data-ttu-id="81970-163">A webes alkalmazás (STS) használata:</span><span class="sxs-lookup"><span data-stu-id="81970-163">To use the Web Application (STS):</span></span>

1. <span data-ttu-id="81970-164">Módosítsa a web.config telepítő castlabs kereskedelmi azonosítója, az STS-konfiguráció és a megosztott kulcsot.</span><span class="sxs-lookup"><span data-stu-id="81970-164">Change the web.config to setup castlabs merchant ID, the STS configuration and the shared key.</span></span>
2. <span data-ttu-id="81970-165">Telepítse az Azure-webhelyekre.</span><span class="sxs-lookup"><span data-stu-id="81970-165">Deploy to Azure Websites.</span></span>
3. <span data-ttu-id="81970-166">Nyissa meg a webhelyet.</span><span class="sxs-lookup"><span data-stu-id="81970-166">Navigate to the website.</span></span>

## <a name="playing-back-a-video"></a><span data-ttu-id="81970-167">Vissza a videó lejátszása</span><span class="sxs-lookup"><span data-stu-id="81970-167">Playing back a video</span></span>
<span data-ttu-id="81970-168">(PlayReady és/vagy Widevine) közös titkosítással titkosított videó lejátszása, használhatja a [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="81970-168">To playback a video encrypted with common encryption (PlayReady and/or Widevine), you can use the [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span> <span data-ttu-id="81970-169">Ha fut a Konzolalkalmazás, annál a tartalom kulcs azonosítója és a jegyzékfájl URL-címet.</span><span class="sxs-lookup"><span data-stu-id="81970-169">When running the console app, the Content Key ID and the Manifest URL are echoed.</span></span>

1. <span data-ttu-id="81970-170">Nyisson meg egy új lapot, és indítsa el a STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].</span><span class="sxs-lookup"><span data-stu-id="81970-170">Open a new tab and launch your STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].</span></span>
2. <span data-ttu-id="81970-171">Ugrás a [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="81970-171">Go to [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>
3. <span data-ttu-id="81970-172">Illessze be a streamelési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="81970-172">Paste in the streaming URL.</span></span>
4. <span data-ttu-id="81970-173">Kattintson a **speciális beállítások** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="81970-173">Click the **Advanced Options** checkbox.</span></span>
5. <span data-ttu-id="81970-174">Az a **védelmi** legördülő menüben válassza a PlayReady és/vagy Widevine.</span><span class="sxs-lookup"><span data-stu-id="81970-174">In the **Protection** dropdown, select PlayReady and/or Widevine.</span></span>
6. <span data-ttu-id="81970-175">Illessze be a származik a jogkivonat szövegmezőjének az STS-tokent.</span><span class="sxs-lookup"><span data-stu-id="81970-175">Paste the token that you got from your STS in the Token textbox.</span></span> 
   
   <span data-ttu-id="81970-176">A castLab licenckiszolgáló nem kell a "tulajdonosi =" előtag előtt a jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="81970-176">The castLab license server does not need the “Bearer=” prefix in front of the token.</span></span> <span data-ttu-id="81970-177">Ezért távolítsa el, amely a token elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="81970-177">So please remove that before submitting the token.</span></span>
7. <span data-ttu-id="81970-178">A Windows Media player frissítése.</span><span class="sxs-lookup"><span data-stu-id="81970-178">Update the player.</span></span>
8. <span data-ttu-id="81970-179">A videó lejátszása kell lehet.</span><span class="sxs-lookup"><span data-stu-id="81970-179">The video should be playing.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="81970-180">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="81970-180">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="81970-181">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="81970-181">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

