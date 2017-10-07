---
title: aaaUsing castLabs toodeliver Widevine-licencek tooAzure Media Services |} Microsoft Docs
description: "Ez a cikk ismerteti, hogyan használhatja az Azure Media Services (AMS) toodeliver olyan adatfolyamra, amely dinamikusan titkosítja a PlayReady vagy a Widevine DRMs AMS. hello PlayReady licenc Media Services PlayReady licenckiszolgáló származik, és Widevine-licenc castLabs licenckiszolgáló hozta."
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
ms.openlocfilehash: 80d2778fb283a96361e7e511990a36c2f551a310
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-castlabs-toodeliver-widevine-licenses-tooazure-media-services"></a><span data-ttu-id="2ae24-104">CastLabs toodeliver Widevine-licencek tooAzure Media Services használatával</span><span class="sxs-lookup"><span data-stu-id="2ae24-104">Using castLabs toodeliver Widevine licenses tooAzure Media Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="2ae24-105">Axinom</span><span class="sxs-lookup"><span data-stu-id="2ae24-105">Axinom</span></span>](media-services-axinom-integration.md)
> * [<span data-ttu-id="2ae24-106">castLabs</span><span class="sxs-lookup"><span data-stu-id="2ae24-106">castLabs</span></span>](media-services-castlabs-integration.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="2ae24-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2ae24-107">Overview</span></span>
<span data-ttu-id="2ae24-108">Ez a cikk ismerteti, hogyan használhatja az Azure Media Services (AMS) toodeliver olyan adatfolyamra, amely dinamikusan titkosítja a PlayReady vagy a Widevine DRMs AMS.</span><span class="sxs-lookup"><span data-stu-id="2ae24-108">This article describes how you can use Azure Media Services (AMS) toodeliver a stream that is dynamically encrypted by AMS with both PlayReady and Widevine DRMs.</span></span> <span data-ttu-id="2ae24-109">hello PlayReady licenc származik a licenckiszolgáló Media Services PlayReady és Widevine-licenc hozta **castLabs** licenckiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="2ae24-109">hello PlayReady license comes from Media Services PlayReady license server and Widevine license is delivered by **castLabs** license server.</span></span>

<span data-ttu-id="2ae24-110">adatfolyam-védett tartalom tooplayback CENC (PlayReady és/vagy Widevine), használható [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="2ae24-110">tooplayback streaming content protected by CENC (PlayReady and/or Widevine), you can use  [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span> <span data-ttu-id="2ae24-111">Lásd: [AMP dokumentum](http://amp.azure.net/libs/amp/latest/docs/) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="2ae24-111">See [AMP document](http://amp.azure.net/libs/amp/latest/docs/) for details.</span></span>

<span data-ttu-id="2ae24-112">hello a következő ábra azt mutatja be, a magas szintű Azure Media Services és a castLabs integrációs architektúra.</span><span class="sxs-lookup"><span data-stu-id="2ae24-112">hello following diagram demonstrates a high-level Azure Media Services and castLabs integration architecture.</span></span>

![Integráció](./media/media-services-castlabs-integration/media-services-castlabs-integration.png)

## <a name="typical-system-set-up"></a><span data-ttu-id="2ae24-114">Tipikus rendszer beállítása</span><span class="sxs-lookup"><span data-stu-id="2ae24-114">Typical system set up</span></span>
* <span data-ttu-id="2ae24-115">A médiatartalom AMS tárolja.</span><span class="sxs-lookup"><span data-stu-id="2ae24-115">Media content is stored in AMS.</span></span>
* <span data-ttu-id="2ae24-116">Kulcs azonosítók tartalom kulcsok castLabs, mind az AMS vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="2ae24-116">Key IDs of content keys are stored in both castLabs and AMS.</span></span>
* <span data-ttu-id="2ae24-117">castLabs és AMS egyaránt rendelkezik beépített tokent használó hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="2ae24-117">castLabs and AMS both have token authentication built in.</span></span> <span data-ttu-id="2ae24-118">hello a következő szakaszok ismertetik a hitelesítési tokenek.</span><span class="sxs-lookup"><span data-stu-id="2ae24-118">hello following sections discuss authentication tokens.</span></span> 
* <span data-ttu-id="2ae24-119">Amikor egy ügyfél toostream hello videó, dinamikusan hello tartalom titkosítása a **Common Encryption** (CENC) és AMS tooSmooth dinamikusan csomagolni, Streaming és kötőjel.</span><span class="sxs-lookup"><span data-stu-id="2ae24-119">When a client requests toostream hello video, hello content is dynamically encrypted with **Common Encryption** (CENC) and dynamically packaged by AMS tooSmooth Streaming and DASH.</span></span> <span data-ttu-id="2ae24-120">Azt is biztosítanak a HLS streamelési protokoll PlayReady M2TS elemi adatfolyam titkosítását.</span><span class="sxs-lookup"><span data-stu-id="2ae24-120">We also deliver PlayReady M2TS elementary stream encryption for HLS streaming protocol.</span></span>
* <span data-ttu-id="2ae24-121">PlayReady-licenc AMS licenckiszolgáló kéri le a rendszer, és Widevine-licenc castLabs licenckiszolgáló kéri le a rendszer.</span><span class="sxs-lookup"><span data-stu-id="2ae24-121">PlayReady license is retrieved from AMS license server and Widevine license is retrieved from castLabs license server.</span></span> 
* <span data-ttu-id="2ae24-122">Media Player automatikusan eldönti, milyen licenc toofetch hello ügyfél platform képességei alapján.</span><span class="sxs-lookup"><span data-stu-id="2ae24-122">Media Player automatically decides which license toofetch based on hello client platform capability.</span></span> 

## <a name="authentication-token-generation-for-getting-a-license"></a><span data-ttu-id="2ae24-123">Hitelesítési jogkivonatok létrehozásához a licenc beolvasásakor</span><span class="sxs-lookup"><span data-stu-id="2ae24-123">Authentication token generation for getting a license</span></span>
<span data-ttu-id="2ae24-124">CastLabs, mind az AMS támogatja (JSON Web Token) JWT jogkivonat formátumától tooauthorize egy licencet.</span><span class="sxs-lookup"><span data-stu-id="2ae24-124">Both castLabs and AMS support JWT (JSON Web Token) token format used tooauthorize a license.</span></span> 

### <a name="jwt-token-in-ams"></a><span data-ttu-id="2ae24-125">Az AMS JWT jogkivonat</span><span class="sxs-lookup"><span data-stu-id="2ae24-125">JWT token in AMS</span></span>
<span data-ttu-id="2ae24-126">hello a következő táblázat ismerteti az AMS JWT jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="2ae24-126">hello following table describes JWT token in AMS.</span></span> 

| <span data-ttu-id="2ae24-127">Kiállító</span><span class="sxs-lookup"><span data-stu-id="2ae24-127">Issuer</span></span> | <span data-ttu-id="2ae24-128">A kiválasztott kibocsátó karakterláncnak a következőről hello Secure Token Service (STS)</span><span class="sxs-lookup"><span data-stu-id="2ae24-128">Issuer string from hello chosen Secure Token Service (STS)</span></span> |
| --- | --- |
| <span data-ttu-id="2ae24-129">Célközönség</span><span class="sxs-lookup"><span data-stu-id="2ae24-129">Audience</span></span> |<span data-ttu-id="2ae24-130">A célközönség-karakterláncnak a következőről hello használt STS</span><span class="sxs-lookup"><span data-stu-id="2ae24-130">Audience string from hello used STS</span></span> |
| <span data-ttu-id="2ae24-131">Jogcímek</span><span class="sxs-lookup"><span data-stu-id="2ae24-131">Claims</span></span> |<span data-ttu-id="2ae24-132">Jogcímek egy készletét</span><span class="sxs-lookup"><span data-stu-id="2ae24-132">A set of claims</span></span> |
| <span data-ttu-id="2ae24-133">NotBefore</span><span class="sxs-lookup"><span data-stu-id="2ae24-133">NotBefore</span></span> |<span data-ttu-id="2ae24-134">Hello token érvényességének kezdő</span><span class="sxs-lookup"><span data-stu-id="2ae24-134">Start validity of hello token</span></span> |
| <span data-ttu-id="2ae24-135">Lejár</span><span class="sxs-lookup"><span data-stu-id="2ae24-135">Expires</span></span> |<span data-ttu-id="2ae24-136">Hello token érvényességének vége</span><span class="sxs-lookup"><span data-stu-id="2ae24-136">End validity of hello token</span></span> |
| <span data-ttu-id="2ae24-137">SigningCredentials</span><span class="sxs-lookup"><span data-stu-id="2ae24-137">SigningCredentials</span></span> |<span data-ttu-id="2ae24-138">hello kulcs, amelyet használ PlayReady licenckiszolgáló, castLabs licenckiszolgáló és STS, akkor lehet, hogy szimmetrikus vagy aszimmetrikus kulcs.</span><span class="sxs-lookup"><span data-stu-id="2ae24-138">hello key that is shared among PlayReady License Server, castLabs License Server and STS, it could be either symmetric or asymmetric key.</span></span> |

### <a name="jwt-token-in-castlabs"></a><span data-ttu-id="2ae24-139">A castLabs JWT jogkivonat</span><span class="sxs-lookup"><span data-stu-id="2ae24-139">JWT token in castLabs</span></span>
<span data-ttu-id="2ae24-140">a következő táblázat hello castLabs a JWT jogkivonat ismerteti.</span><span class="sxs-lookup"><span data-stu-id="2ae24-140">hello following table describes JWT token in castLabs.</span></span> 

| <span data-ttu-id="2ae24-141">Név</span><span class="sxs-lookup"><span data-stu-id="2ae24-141">Name</span></span> | <span data-ttu-id="2ae24-142">Leírás</span><span class="sxs-lookup"><span data-stu-id="2ae24-142">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2ae24-143">optData</span><span class="sxs-lookup"><span data-stu-id="2ae24-143">optData</span></span> |<span data-ttu-id="2ae24-144">Egy adatokat tartalmazó JSON-karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="2ae24-144">A JSON string containing information about you.</span></span> |
| <span data-ttu-id="2ae24-145">CRT</span><span class="sxs-lookup"><span data-stu-id="2ae24-145">crt</span></span> |<span data-ttu-id="2ae24-146">A JSON karakterlánc kapcsolatos információkat tartalmazó hello eszköz, a licencelési adatokat, és a lejátszás jogok.</span><span class="sxs-lookup"><span data-stu-id="2ae24-146">A JSON string containing information about hello asset, its license info and playback rights.</span></span> |
| <span data-ttu-id="2ae24-147">IAT</span><span class="sxs-lookup"><span data-stu-id="2ae24-147">iat</span></span> |<span data-ttu-id="2ae24-148">hello aktuális datetime epoch.</span><span class="sxs-lookup"><span data-stu-id="2ae24-148">hello current datetime in epoch.</span></span> |
| <span data-ttu-id="2ae24-149">jti</span><span class="sxs-lookup"><span data-stu-id="2ae24-149">jti</span></span> |<span data-ttu-id="2ae24-150">Ez a token (összes jogkivonat csak egyszer használható hello castLabs rendszerben) kapcsolatos egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2ae24-150">A unique identifier about this token (every token can only be used once in hello castLabs system).</span></span> |

## <a name="sample-solution-set-up"></a><span data-ttu-id="2ae24-151">A minta megoldás beállítása</span><span class="sxs-lookup"><span data-stu-id="2ae24-151">Sample solution set up</span></span>
<span data-ttu-id="2ae24-152">Hello [megoldás minta](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) két projektet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="2ae24-152">hello [sample solution](https://github.com/AzureMediaServicesSamples/CastlabsIntegration) consists of two projects:</span></span>

* <span data-ttu-id="2ae24-153">Egy konzolalkalmazást, amely a PlayReady és Widevine már feldolgozott eszközként használt tooset DRM korlátozásait.</span><span class="sxs-lookup"><span data-stu-id="2ae24-153">A console app that can be used tooset DRM restrictions on an already ingested asset, for both PlayReady and Widevine.</span></span>
* <span data-ttu-id="2ae24-154">A webes alkalmazás, amely átadja a jogkivonatokat, ami az STS szolgáltatással nagyon egyszerűsített verziója sikerült tekinthető meg.</span><span class="sxs-lookup"><span data-stu-id="2ae24-154">A Web Application that hands out tokens, which could be seen as a VERY SIMPLIFIED version of an STS.</span></span>

<span data-ttu-id="2ae24-155">toouse hello konzolalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="2ae24-155">toouse hello console application:</span></span>

1. <span data-ttu-id="2ae24-156">Hello app.config toosetup AMS hitelesítő adatokat, a castLabs hitelesítő adatokat, a STS konfigurációs és a megosztott kulcs módosítása.</span><span class="sxs-lookup"><span data-stu-id="2ae24-156">Change hello app.config toosetup AMS credentials, castLabs credentials, STS configuration and shared key.</span></span>
2. <span data-ttu-id="2ae24-157">Töltse fel az eszköz az AMS.</span><span class="sxs-lookup"><span data-stu-id="2ae24-157">Upload an Asset into AMS.</span></span>
3. <span data-ttu-id="2ae24-158">Get hello UUID hello a feltöltött eszköz, és módosítsa a sor 32 hello Program.cs fájlban:</span><span class="sxs-lookup"><span data-stu-id="2ae24-158">Get hello UUID from hello uploaded Asset, and change Line 32 in hello Program.cs file:</span></span>
   
      <span data-ttu-id="2ae24-159">var objIAsset = _context. Assets.Where (x = > x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf"). FirstOrDefault();</span><span class="sxs-lookup"><span data-stu-id="2ae24-159">var objIAsset = _context.Assets.Where(x => x.Id == "nb:cid:UUID:dac53a5d-1500-80bd-b864-f1e4b62594cf").FirstOrDefault();</span></span>
4. <span data-ttu-id="2ae24-160">Használjon egy AssetId hello eszköz elnevezési hello castLabs rendszerben (sor 44 hello Program.cs fájlban).</span><span class="sxs-lookup"><span data-stu-id="2ae24-160">Use an AssetId for naming hello asset in hello castLabs system (Line 44 in hello Program.cs file).</span></span>
   
   <span data-ttu-id="2ae24-161">Meg kell adni a AssetId **castLabs**; toobe egyedi alfanumerikus karakterlánc szükséges.</span><span class="sxs-lookup"><span data-stu-id="2ae24-161">You must set AssetId for **castLabs**; it needs toobe a unique alphanumeric string.</span></span>
5. <span data-ttu-id="2ae24-162">Hello program futtatása.</span><span class="sxs-lookup"><span data-stu-id="2ae24-162">Run hello program.</span></span>

<span data-ttu-id="2ae24-163">toouse hello webes alkalmazás (STS):</span><span class="sxs-lookup"><span data-stu-id="2ae24-163">toouse hello Web Application (STS):</span></span>

1. <span data-ttu-id="2ae24-164">Változás hello web.config toosetup castlabs kereskedelmi Azonosítót, hello STS konfigurációs és hello megosztott kulcsot.</span><span class="sxs-lookup"><span data-stu-id="2ae24-164">Change hello web.config toosetup castlabs merchant ID, hello STS configuration and hello shared key.</span></span>
2. <span data-ttu-id="2ae24-165">Webhelyek tooAzure telepítése.</span><span class="sxs-lookup"><span data-stu-id="2ae24-165">Deploy tooAzure Websites.</span></span>
3. <span data-ttu-id="2ae24-166">Keresse meg a toohello webhelyet.</span><span class="sxs-lookup"><span data-stu-id="2ae24-166">Navigate toohello website.</span></span>

## <a name="playing-back-a-video"></a><span data-ttu-id="2ae24-167">Vissza a videó lejátszása</span><span class="sxs-lookup"><span data-stu-id="2ae24-167">Playing back a video</span></span>
<span data-ttu-id="2ae24-168">tooplayback common encryption (PlayReady és/vagy Widevine) titkosítva videó, használhatja a hello [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="2ae24-168">tooplayback a video encrypted with common encryption (PlayReady and/or Widevine), you can use hello [Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span> <span data-ttu-id="2ae24-169">Hello Konzolalkalmazás futtatásakor annál hello Tartalomazonosítót kulcs és hello Manifest URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="2ae24-169">When running hello console app, hello Content Key ID and hello Manifest URL are echoed.</span></span>

1. <span data-ttu-id="2ae24-170">Nyisson meg egy új lapot, és indítsa el a STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].</span><span class="sxs-lookup"><span data-stu-id="2ae24-170">Open a new tab and launch your STS: http://[yourStsName].azurewebsites.net/api/token/assetid/[yourCastLabsAssetId]/contentkeyid/[thecontentkeyid].</span></span>
2. <span data-ttu-id="2ae24-171">Nyissa meg túl[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span><span class="sxs-lookup"><span data-stu-id="2ae24-171">Go too[Azure Media Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html).</span></span>
3. <span data-ttu-id="2ae24-172">Illessze be a streamelési URL-cím hello.</span><span class="sxs-lookup"><span data-stu-id="2ae24-172">Paste in hello streaming URL.</span></span>
4. <span data-ttu-id="2ae24-173">Kattintson a hello **speciális beállítások** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="2ae24-173">Click hello **Advanced Options** checkbox.</span></span>
5. <span data-ttu-id="2ae24-174">A hello **védelmi** legördülő menüben válassza a PlayReady és/vagy Widevine.</span><span class="sxs-lookup"><span data-stu-id="2ae24-174">In hello **Protection** dropdown, select PlayReady and/or Widevine.</span></span>
6. <span data-ttu-id="2ae24-175">Illessze be a portáltól kapott az STS hello Token szövegmezőjének hello jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="2ae24-175">Paste hello token that you got from your STS in hello Token textbox.</span></span> 
   
   <span data-ttu-id="2ae24-176">hello castLab licenckiszolgáló nem kell hello "tulajdonosi =" hello token elé előtag.</span><span class="sxs-lookup"><span data-stu-id="2ae24-176">hello castLab license server does not need hello “Bearer=” prefix in front of hello token.</span></span> <span data-ttu-id="2ae24-177">Ezért távolítsa el, amely hello token elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="2ae24-177">So please remove that before submitting hello token.</span></span>
7. <span data-ttu-id="2ae24-178">Hello player frissítése.</span><span class="sxs-lookup"><span data-stu-id="2ae24-178">Update hello player.</span></span>
8. <span data-ttu-id="2ae24-179">videó hello kell játszott.</span><span class="sxs-lookup"><span data-stu-id="2ae24-179">hello video should be playing.</span></span>

## <a name="media-services-learning-paths"></a><span data-ttu-id="2ae24-180">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="2ae24-180">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="2ae24-181">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="2ae24-181">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

