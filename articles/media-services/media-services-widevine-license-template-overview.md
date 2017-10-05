---
title: "Widevine-licenc sablon áttekintése |} Microsoft Docs"
description: "Ez a témakör áttekintést a Widevine-licencsablon, Widevine-licencek konfigurálására használhatók."
author: juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 667ff16dc7608dab2a5b8b1fd7df715da4620ca1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="widevine-license-template-overview"></a><span data-ttu-id="b2feb-103">Widevine-licenc sablon – áttekintés</span><span class="sxs-lookup"><span data-stu-id="b2feb-103">Widevine license template overview</span></span>
## <a name="overview"></a><span data-ttu-id="b2feb-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="b2feb-104">Overview</span></span>
<span data-ttu-id="b2feb-105">Az Azure Media Services lehetővé teszi konfigurálása és Widevine-licencek kérése most.</span><span class="sxs-lookup"><span data-stu-id="b2feb-105">Azure Media Services now enables you to configure and request Widevine licenses.</span></span> <span data-ttu-id="b2feb-106">Ha a végfelhasználó player próbál Widevine védett tartalom lejátszása, a licenc beszerzésével kérelmet küldött a kézbesítési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b2feb-106">When the end user player tries to play your Widevine protected content, a request is sent to the license delivery service to obtain a license.</span></span> <span data-ttu-id="b2feb-107">Ha a szolgáltatás a kérelem jóváhagyása, a licenc, amely az ügyfélnek küldött és fejti vissza és a megadott tartalom lejátszásához használható ad ki.</span><span class="sxs-lookup"><span data-stu-id="b2feb-107">If the license service approves the request, it issues the license which is sent to the client and can be used to decrypt and play the specified content.</span></span>

<span data-ttu-id="b2feb-108">Widevine-licenc kérelem JSON üzenet formátuma.</span><span class="sxs-lookup"><span data-stu-id="b2feb-108">Widevine license request is formatted as a JSON message.</span></span>  

>[!NOTE]
> <span data-ttu-id="b2feb-109">Ha szeretné, létrehozhat egy üres értékek nélküli csak "{}", és egy licenc-sablon létrehozza az összes alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="b2feb-109">You can choose to create an empty message with no values just "{}" and a license template will be created with all defaults.</span></span> <span data-ttu-id="b2feb-110">Az alapértelmezett működik a legtöbb esetben.</span><span class="sxs-lookup"><span data-stu-id="b2feb-110">The default works for most cases.</span></span> <span data-ttu-id="b2feb-111">Például a MS-alapú licenc kézbesítési esetében, amely alapértelmezett mindig kell lennie.</span><span class="sxs-lookup"><span data-stu-id="b2feb-111">For example, for MS based license delivery scenarios that should always be default.</span></span> <span data-ttu-id="b2feb-112">Állítsa be a "provider" és "content_id" értékeket kell, ha egy szolgáltató Google Widevine hitelesítő adatok meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="b2feb-112">If you do need to set the "provider" and "content_id" values, a provider must match Google's Widevine credentials.</span></span>

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a><span data-ttu-id="b2feb-113">JSON-üzenet</span><span class="sxs-lookup"><span data-stu-id="b2feb-113">JSON message</span></span>
| <span data-ttu-id="b2feb-114">Név</span><span class="sxs-lookup"><span data-stu-id="b2feb-114">Name</span></span> | <span data-ttu-id="b2feb-115">Érték</span><span class="sxs-lookup"><span data-stu-id="b2feb-115">Value</span></span> | <span data-ttu-id="b2feb-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="b2feb-116">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b2feb-117">tartalom</span><span class="sxs-lookup"><span data-stu-id="b2feb-117">payload</span></span> |<span data-ttu-id="b2feb-118">A Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b2feb-118">Base64 encoded string</span></span> |<span data-ttu-id="b2feb-119">Az ügyfelek által küldött licenc kérelem.</span><span class="sxs-lookup"><span data-stu-id="b2feb-119">The license request sent by a client.</span></span> |
| <span data-ttu-id="b2feb-120">content_id</span><span class="sxs-lookup"><span data-stu-id="b2feb-120">content_id</span></span> |<span data-ttu-id="b2feb-121">A Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b2feb-121">Base64 encoded string</span></span> |<span data-ttu-id="b2feb-122">Használható KeyId(s) és a tartalom kulcsoknak az egyes content_key_specs.track_type azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b2feb-122">Identifier used to derive KeyId(s) and Content Key(s) for each content_key_specs.track_type.</span></span> |
| <span data-ttu-id="b2feb-123">Szolgáltató</span><span class="sxs-lookup"><span data-stu-id="b2feb-123">provider</span></span> |<span data-ttu-id="b2feb-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b2feb-124">string</span></span> |<span data-ttu-id="b2feb-125">Tartalom kulcsok és a házirendek kikereséséhez használandó.</span><span class="sxs-lookup"><span data-stu-id="b2feb-125">Used to look up content keys and policies.</span></span> <span data-ttu-id="b2feb-126">MS kulcs kézbesítési Widevine-licenc kézbesítésre használata esetén a rendszer figyelmen kívül hagyja ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="b2feb-126">If MS key delivery is used for Widevine license delivery, this parameter is ignored.</span></span> |
| <span data-ttu-id="b2feb-127">házirendnév</span><span class="sxs-lookup"><span data-stu-id="b2feb-127">policy_name</span></span> |<span data-ttu-id="b2feb-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b2feb-128">string</span></span> |<span data-ttu-id="b2feb-129">Egy korábban regisztrált házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="b2feb-129">Name of a previously registered policy.</span></span> <span data-ttu-id="b2feb-130">Optional</span><span class="sxs-lookup"><span data-stu-id="b2feb-130">Optional</span></span> |
| <span data-ttu-id="b2feb-131">allowed_track_types</span><span class="sxs-lookup"><span data-stu-id="b2feb-131">allowed_track_types</span></span> |<span data-ttu-id="b2feb-132">Enum</span><span class="sxs-lookup"><span data-stu-id="b2feb-132">enum</span></span> |<span data-ttu-id="b2feb-133">SD_ONLY vagy SD_HD.</span><span class="sxs-lookup"><span data-stu-id="b2feb-133">SD_ONLY or SD_HD.</span></span> <span data-ttu-id="b2feb-134">Vezérlők, amelyek kulcsok tartalom szerepelnie kell egy licencet</span><span class="sxs-lookup"><span data-stu-id="b2feb-134">Controls which content keys should be included in a license</span></span> |
| <span data-ttu-id="b2feb-135">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="b2feb-135">content_key_specs</span></span> |<span data-ttu-id="b2feb-136">a tömb JSON struktúrák, lásd: **tartalom kulcs specifikációk** alatt</span><span class="sxs-lookup"><span data-stu-id="b2feb-136">array of JSON structures, see **Content Key Specs** below</span></span> |<span data-ttu-id="b2feb-137">Egy egyeztetését megbízhatatlanná vezérlő a tartalmat a kulcsok való visszatéréshez.</span><span class="sxs-lookup"><span data-stu-id="b2feb-137">A finer grained control on what content keys to return.</span></span> <span data-ttu-id="b2feb-138">Tartalom kulcs Spec alatt talál információt.</span><span class="sxs-lookup"><span data-stu-id="b2feb-138">See Content Key Spec below for details.</span></span>  <span data-ttu-id="b2feb-139">Allowed_track_types és content_key_specs csak egy adható meg.</span><span class="sxs-lookup"><span data-stu-id="b2feb-139">Only one of allowed_track_types and content_key_specs can be specified.</span></span> |
| <span data-ttu-id="b2feb-140">use_policy_overrides_exclusively</span><span class="sxs-lookup"><span data-stu-id="b2feb-140">use_policy_overrides_exclusively</span></span> |<span data-ttu-id="b2feb-141">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="b2feb-141">boolean.</span></span> <span data-ttu-id="b2feb-142">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="b2feb-142">true or false</span></span> |<span data-ttu-id="b2feb-143">Policy_overrides által megadott házirend attribútumok használata, és hagyja el az összes korábban tárolt házirend.</span><span class="sxs-lookup"><span data-stu-id="b2feb-143">Use policy attributes specified by policy_overrides and omit all previously stored policy.</span></span> |
| <span data-ttu-id="b2feb-144">policy_overrides</span><span class="sxs-lookup"><span data-stu-id="b2feb-144">policy_overrides</span></span> |<span data-ttu-id="b2feb-145">JSON-struktúrát, lásd: **házirend felülírja** alatt</span><span class="sxs-lookup"><span data-stu-id="b2feb-145">JSON structure, see **Policy Overrides** below</span></span> |<span data-ttu-id="b2feb-146">Ez a licenc házirend beállításait.</span><span class="sxs-lookup"><span data-stu-id="b2feb-146">Policy settings for this license.</span></span>  <span data-ttu-id="b2feb-147">Abban az esetben, ha ez az eszköz egy előre definiált szabályzattal rendelkezik, a megadott értékeket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="b2feb-147">In the event this asset has a pre-defined policy, these specified values will be used.</span></span> |
| <span data-ttu-id="b2feb-148">session_init</span><span class="sxs-lookup"><span data-stu-id="b2feb-148">session_init</span></span> |<span data-ttu-id="b2feb-149">JSON-struktúrát, lásd: **munkamenet inicializálási** alatt</span><span class="sxs-lookup"><span data-stu-id="b2feb-149">JSON structure, see **Session Initialization** below</span></span> |<span data-ttu-id="b2feb-150">Választható adatok kapott licencet.</span><span class="sxs-lookup"><span data-stu-id="b2feb-150">Optional data passed to license.</span></span> |
| <span data-ttu-id="b2feb-151">parse_only</span><span class="sxs-lookup"><span data-stu-id="b2feb-151">parse_only</span></span> |<span data-ttu-id="b2feb-152">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="b2feb-152">boolean.</span></span> <span data-ttu-id="b2feb-153">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="b2feb-153">true or false</span></span> |<span data-ttu-id="b2feb-154">A licenc kérelem elemzi, de licenc nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b2feb-154">The license request is parsed but no license is issued.</span></span> <span data-ttu-id="b2feb-155">Azonban értékek a licenc-kérelmet a válaszban visszaadott képernyő.</span><span class="sxs-lookup"><span data-stu-id="b2feb-155">However, values form the license request are returned in the response.</span></span> |

## <a name="content-key-specs"></a><span data-ttu-id="b2feb-156">Tartalomkulcs specifikációk</span><span class="sxs-lookup"><span data-stu-id="b2feb-156">Content Key Specs</span></span>
<span data-ttu-id="b2feb-157">Ha egy már meglévő házirend létezik, nincs szükség van valamely értékét adja meg a tartalom kulcs specifikációi.  A már meglévő házirend ehhez a tartalomhoz társított használandó a kimeneti védelmet, például HDCP és CGMS határozza meg.</span><span class="sxs-lookup"><span data-stu-id="b2feb-157">If a pre-existing policy exist, there is no need to specify any of the values in the Content Key Spec.  The pre-existing policy associated with this content will be used to determine the output protection such as HDCP and CGMS.</span></span>  <span data-ttu-id="b2feb-158">Ha egy már meglévő házirend a Widevine-licenc kiszolgáló nincs regisztrálva, a tartalomszolgáltató is behelyezése az értékeket a licenc kérelmet.</span><span class="sxs-lookup"><span data-stu-id="b2feb-158">If a pre-existing policy is not registered with the Widevine License Server, the content provider can inject the values into the license request.</span></span>   

<span data-ttu-id="b2feb-159">Minden egyes content_key_specs összes nyomon követi, függetlenül a beállítás use_policy_overrides_exclusively kell adni.</span><span class="sxs-lookup"><span data-stu-id="b2feb-159">Each content_key_specs must be specified for all tracks, regardless of the option use_policy_overrides_exclusively.</span></span> 

| <span data-ttu-id="b2feb-160">Név</span><span class="sxs-lookup"><span data-stu-id="b2feb-160">Name</span></span> | <span data-ttu-id="b2feb-161">Érték</span><span class="sxs-lookup"><span data-stu-id="b2feb-161">Value</span></span> | <span data-ttu-id="b2feb-162">Leírás</span><span class="sxs-lookup"><span data-stu-id="b2feb-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b2feb-163">content_key_specs.</span><span class="sxs-lookup"><span data-stu-id="b2feb-163">content_key_specs.</span></span> <span data-ttu-id="b2feb-164">track_type</span><span class="sxs-lookup"><span data-stu-id="b2feb-164">track_type</span></span> |<span data-ttu-id="b2feb-165">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b2feb-165">string</span></span> |<span data-ttu-id="b2feb-166">Track adattípus neve.</span><span class="sxs-lookup"><span data-stu-id="b2feb-166">A track type name.</span></span> <span data-ttu-id="b2feb-167">Ha a licenc kérelem content_key_specs van beállítva, ellenőrizze, hogy minden nyomon típusokat explicit módon.</span><span class="sxs-lookup"><span data-stu-id="b2feb-167">If content_key_specs is specified in the license request, make sure to specify all track types explicitly.</span></span> <span data-ttu-id="b2feb-168">Elmulasztása eredményez hiba lejátszásához elmúlt 10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="b2feb-168">Failure to do so will result in failure to playback past 10 seconds.</span></span> |
| <span data-ttu-id="b2feb-169">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="b2feb-169">content_key_specs</span></span>  <br/> <span data-ttu-id="b2feb-170">security_level</span><span class="sxs-lookup"><span data-stu-id="b2feb-170">security_level</span></span> |<span data-ttu-id="b2feb-171">UInt32</span><span class="sxs-lookup"><span data-stu-id="b2feb-171">uint32</span></span> |<span data-ttu-id="b2feb-172">Meghatározza a lejátszás ügyfél szolgáltatás megbízhatóságára követelményeit.</span><span class="sxs-lookup"><span data-stu-id="b2feb-172">Defines client robustness requirements for playback.</span></span> <br/> <span data-ttu-id="b2feb-173">1 - szoftveres whitebox titkosítási szükség.</span><span class="sxs-lookup"><span data-stu-id="b2feb-173">1 - Software-based whitebox crypto is required.</span></span> <br/> <span data-ttu-id="b2feb-174">2 - szoftver titkosítási és egy rejtjelezett dekóder szükség.</span><span class="sxs-lookup"><span data-stu-id="b2feb-174">2 - Software crypto and an obfuscated decoder is required.</span></span> <br/> <span data-ttu-id="b2feb-175">3 - a hardveres biztonsági megbízható végrehajtási környezetekben a kulcsokat tárol, és a titkosítási műveleteket kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="b2feb-175">3 - The key material and crypto operations must be performed within a hardware backed trusted execution environment.</span></span> <br/> <span data-ttu-id="b2feb-176">4 – a titkosítás és a tartalom dekódolási hardver megbízható végrehajtási biztonsági környezetben kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="b2feb-176">4 - The crypto and decoding of content must be performed within a hardware backed trusted execution environment.</span></span>  <br/> <span data-ttu-id="b2feb-177">5 - a titkosítási, dekódolási és az adathordozó (tömörített és tömörítetlen) összes kezelési hardver megbízható végrehajtási biztonsági környezetben kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="b2feb-177">5 - The crypto, decoding and all handling of the media (compressed and uncompressed) must be handled within a hardware backed trusted execution environment.</span></span> |
| <span data-ttu-id="b2feb-178">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="b2feb-178">content_key_specs</span></span> <br/> <span data-ttu-id="b2feb-179">required_output_protection.hDC</span><span class="sxs-lookup"><span data-stu-id="b2feb-179">required_output_protection.hdc</span></span> |<span data-ttu-id="b2feb-180">karakterlánc - egyikét: HDCP_NONE, HDCP_V1, HDCP_V2</span><span class="sxs-lookup"><span data-stu-id="b2feb-180">string - one of: HDCP_NONE, HDCP_V1, HDCP_V2</span></span> |<span data-ttu-id="b2feb-181">Azt jelzi, hogy szükség van-e a HDCP</span><span class="sxs-lookup"><span data-stu-id="b2feb-181">Indicates whether HDCP is require</span></span> |
| <span data-ttu-id="b2feb-182">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="b2feb-182">content_key_specs</span></span> <br/><span data-ttu-id="b2feb-183">kulcs</span><span class="sxs-lookup"><span data-stu-id="b2feb-183">key</span></span> |<span data-ttu-id="b2feb-184">a Base64</span><span class="sxs-lookup"><span data-stu-id="b2feb-184">Base64</span></span> <br/><span data-ttu-id="b2feb-185">kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b2feb-185">encoded string</span></span> |<span data-ttu-id="b2feb-186">Az a szám használandó tartalomkulcsot. Ha meg van adva, a track_type vagy key_id megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="b2feb-186">Content key to use for this track. If specified, the track_type or key_id is required.</span></span>  <span data-ttu-id="b2feb-187">Ez a beállítás lehetővé teszi, hogy a tartalomszolgáltató szúrjon a tartalomkulcsot a track ahelyett, hogy a Widevine licenckiszolgáló létrehozni, vagy keresési kulcs.</span><span class="sxs-lookup"><span data-stu-id="b2feb-187">This option allows the content provider to inject the content key for this track instead of letting Widevine license server generate or lookup a key.</span></span> |
| <span data-ttu-id="b2feb-188">content_key_specs.key_id</span><span class="sxs-lookup"><span data-stu-id="b2feb-188">content_key_specs.key_id</span></span> |<span data-ttu-id="b2feb-189">Base64 kódolású karakterlánc bináris, 16 bájt</span><span class="sxs-lookup"><span data-stu-id="b2feb-189">Base64 encoded string  binary, 16 bytes</span></span> |<span data-ttu-id="b2feb-190">A kulcs egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b2feb-190">Unique identifier for the key.</span></span> |

## <a name="policy-overrides"></a><span data-ttu-id="b2feb-191">Házirend felülbírálása</span><span class="sxs-lookup"><span data-stu-id="b2feb-191">Policy Overrides</span></span>
| <span data-ttu-id="b2feb-192">Név</span><span class="sxs-lookup"><span data-stu-id="b2feb-192">Name</span></span> | <span data-ttu-id="b2feb-193">Érték</span><span class="sxs-lookup"><span data-stu-id="b2feb-193">Value</span></span> | <span data-ttu-id="b2feb-194">Leírás</span><span class="sxs-lookup"><span data-stu-id="b2feb-194">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b2feb-195">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-195">policy_overrides.</span></span> <span data-ttu-id="b2feb-196">can_play</span><span class="sxs-lookup"><span data-stu-id="b2feb-196">can_play</span></span> |<span data-ttu-id="b2feb-197">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="b2feb-197">boolean.</span></span> <span data-ttu-id="b2feb-198">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="b2feb-198">true or false</span></span> |<span data-ttu-id="b2feb-199">Azt jelzi, hogy a tartalom lejátszását engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="b2feb-199">Indicates that playback of the content is allowed.</span></span> <span data-ttu-id="b2feb-200">Alapértelmezett értéke false.</span><span class="sxs-lookup"><span data-stu-id="b2feb-200">Default is false.</span></span> |
| <span data-ttu-id="b2feb-201">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-201">policy_overrides.</span></span> <span data-ttu-id="b2feb-202">can_persist</span><span class="sxs-lookup"><span data-stu-id="b2feb-202">can_persist</span></span> |<span data-ttu-id="b2feb-203">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="b2feb-203">boolean.</span></span> <span data-ttu-id="b2feb-204">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="b2feb-204">true or false</span></span> |<span data-ttu-id="b2feb-205">Azt jelzi, hogy a licenc előfordulhat, hogy sikerült-e megőrizni a nem felejtő kapcsolat nélküli használatra.</span><span class="sxs-lookup"><span data-stu-id="b2feb-205">Indicates that the license may be persisted to non-volatile storage for offline use.</span></span> <span data-ttu-id="b2feb-206">Alapértelmezett értéke false.</span><span class="sxs-lookup"><span data-stu-id="b2feb-206">Default is false.</span></span> |
| <span data-ttu-id="b2feb-207">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-207">policy_overrides.</span></span> <span data-ttu-id="b2feb-208">can_renew</span><span class="sxs-lookup"><span data-stu-id="b2feb-208">can_renew</span></span> |<span data-ttu-id="b2feb-209">logikai változó értéke IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="b2feb-209">boolean true or false</span></span> |<span data-ttu-id="b2feb-210">Azt jelzi, hogy ez a licenc megújítása.</span><span class="sxs-lookup"><span data-stu-id="b2feb-210">Indicates that renewal of this license is allowed.</span></span> <span data-ttu-id="b2feb-211">Amennyiben az értéke igaz, a licenc időtartama szívverés lehet kiterjeszteni.</span><span class="sxs-lookup"><span data-stu-id="b2feb-211">If true, the duration of the license can be extended by heartbeat.</span></span> <span data-ttu-id="b2feb-212">Alapértelmezett értéke false.</span><span class="sxs-lookup"><span data-stu-id="b2feb-212">Default is false.</span></span> |
| <span data-ttu-id="b2feb-213">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-213">policy_overrides.</span></span> <span data-ttu-id="b2feb-214">license_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="b2feb-214">license_duration_seconds</span></span> |<span data-ttu-id="b2feb-215">Int64</span><span class="sxs-lookup"><span data-stu-id="b2feb-215">int64</span></span> |<span data-ttu-id="b2feb-216">Azt jelzi, hogy az adott licenccsoport időablakot.</span><span class="sxs-lookup"><span data-stu-id="b2feb-216">Indicates the time window for this specific license.</span></span> <span data-ttu-id="b2feb-217">A 0 érték azt jelzi, hogy nincs-e korlát időtartama.</span><span class="sxs-lookup"><span data-stu-id="b2feb-217">A value of 0 indicates that there is no limit to the duration.</span></span> <span data-ttu-id="b2feb-218">Alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="b2feb-218">Default is 0.</span></span> |
| <span data-ttu-id="b2feb-219">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-219">policy_overrides.</span></span> <span data-ttu-id="b2feb-220">rental_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="b2feb-220">rental_duration_seconds</span></span> |<span data-ttu-id="b2feb-221">Int64</span><span class="sxs-lookup"><span data-stu-id="b2feb-221">int64</span></span> |<span data-ttu-id="b2feb-222">Azt jelzi, a időszak, amíg a lejátszás engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="b2feb-222">Indicates the time window while playback is permitted.</span></span> <span data-ttu-id="b2feb-223">A 0 érték azt jelzi, hogy nincs-e korlát időtartama.</span><span class="sxs-lookup"><span data-stu-id="b2feb-223">A value of 0 indicates that there is no limit to the duration.</span></span> <span data-ttu-id="b2feb-224">Alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="b2feb-224">Default is 0.</span></span> |
| <span data-ttu-id="b2feb-225">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-225">policy_overrides.</span></span> <span data-ttu-id="b2feb-226">playback_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="b2feb-226">playback_duration_seconds</span></span> |<span data-ttu-id="b2feb-227">Int64</span><span class="sxs-lookup"><span data-stu-id="b2feb-227">int64</span></span> |<span data-ttu-id="b2feb-228">Az idő után a lejátszás elindítása a licenc időtartama belül megtekintése ablak.</span><span class="sxs-lookup"><span data-stu-id="b2feb-228">The viewing window of time once playback starts within the license duration.</span></span> <span data-ttu-id="b2feb-229">A 0 érték azt jelzi, hogy nincs-e korlát időtartama.</span><span class="sxs-lookup"><span data-stu-id="b2feb-229">A value of 0 indicates that there is no limit to the duration.</span></span> <span data-ttu-id="b2feb-230">Alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="b2feb-230">Default is 0.</span></span> |
| <span data-ttu-id="b2feb-231">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-231">policy_overrides.</span></span> <span data-ttu-id="b2feb-232">renewal_server_url</span><span class="sxs-lookup"><span data-stu-id="b2feb-232">renewal_server_url</span></span> |<span data-ttu-id="b2feb-233">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b2feb-233">string</span></span> |<span data-ttu-id="b2feb-234">Ez a licenc vonatkozó összes szívverés (megújítási) kell arra, hogy a megadott URL-cím.</span><span class="sxs-lookup"><span data-stu-id="b2feb-234">All heartbeat (renewal) requests for this license shall be directed to the specified URL.</span></span> <span data-ttu-id="b2feb-235">Ha igaz can_renew csak használja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="b2feb-235">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="b2feb-236">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-236">policy_overrides.</span></span> <span data-ttu-id="b2feb-237">renewal_delay_seconds</span><span class="sxs-lookup"><span data-stu-id="b2feb-237">renewal_delay_seconds</span></span> |<span data-ttu-id="b2feb-238">Int64</span><span class="sxs-lookup"><span data-stu-id="b2feb-238">int64</span></span> |<span data-ttu-id="b2feb-239">Hány másodperc után license_start_time, megújítási először megkísérlése előtt.</span><span class="sxs-lookup"><span data-stu-id="b2feb-239">How many seconds after license_start_time, before renewal is first attempted.</span></span> <span data-ttu-id="b2feb-240">Ha igaz can_renew csak használja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="b2feb-240">This field is only used if can_renew is true.</span></span> <span data-ttu-id="b2feb-241">Alapértelmezett érték a 0</span><span class="sxs-lookup"><span data-stu-id="b2feb-241">Default is 0</span></span> |
| <span data-ttu-id="b2feb-242">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-242">policy_overrides.</span></span> <span data-ttu-id="b2feb-243">renewal_retry_interval_seconds</span><span class="sxs-lookup"><span data-stu-id="b2feb-243">renewal_retry_interval_seconds</span></span> |<span data-ttu-id="b2feb-244">Int64</span><span class="sxs-lookup"><span data-stu-id="b2feb-244">int64</span></span> |<span data-ttu-id="b2feb-245">Adja meg a késleltetés között további licenc megújítási kérelmeket, hiba esetén.</span><span class="sxs-lookup"><span data-stu-id="b2feb-245">Specifies the delay in seconds between subsequent license renewal requests, in case of failure.</span></span> <span data-ttu-id="b2feb-246">Ha igaz can_renew csak használja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="b2feb-246">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="b2feb-247">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-247">policy_overrides.</span></span> <span data-ttu-id="b2feb-248">renewal_recovery_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="b2feb-248">renewal_recovery_duration_seconds</span></span> |<span data-ttu-id="b2feb-249">Int64</span><span class="sxs-lookup"><span data-stu-id="b2feb-249">int64</span></span> |<span data-ttu-id="b2feb-250">Az ablak az időt, amelyben a lejátszás lehet folytatni, amíg a megújítási időtartama a licenckiszolgáló megkísérelt, de sikertelen, mert a háttérrendszer problémáit.</span><span class="sxs-lookup"><span data-stu-id="b2feb-250">The window of time, in which playback is allowed to continue while renewal is attempted, yet unsuccessful due to backend problems with the license server.</span></span> <span data-ttu-id="b2feb-251">A 0 érték azt jelzi, hogy nincs-e korlát időtartama.</span><span class="sxs-lookup"><span data-stu-id="b2feb-251">A value of 0 indicates that there is no limit to the duration.</span></span> <span data-ttu-id="b2feb-252">Ha igaz can_renew csak használja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="b2feb-252">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="b2feb-253">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="b2feb-253">policy_overrides.</span></span> <span data-ttu-id="b2feb-254">renew_with_usage</span><span class="sxs-lookup"><span data-stu-id="b2feb-254">renew_with_usage</span></span> |<span data-ttu-id="b2feb-255">logikai változó értéke IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="b2feb-255">boolean true or false</span></span> |<span data-ttu-id="b2feb-256">Azt jelzi, hogy a licenc küldik a megújítási használat indításakor.</span><span class="sxs-lookup"><span data-stu-id="b2feb-256">Indicates that the license shall be sent for renewal when usage is started.</span></span> <span data-ttu-id="b2feb-257">Ha igaz can_renew csak használja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="b2feb-257">This field is only used if can_renew is true.</span></span> |

## <a name="session-initialization"></a><span data-ttu-id="b2feb-258">Munkamenet-inicializálása</span><span class="sxs-lookup"><span data-stu-id="b2feb-258">Session Initialization</span></span>
| <span data-ttu-id="b2feb-259">Név</span><span class="sxs-lookup"><span data-stu-id="b2feb-259">Name</span></span> | <span data-ttu-id="b2feb-260">Érték</span><span class="sxs-lookup"><span data-stu-id="b2feb-260">Value</span></span> | <span data-ttu-id="b2feb-261">Leírás</span><span class="sxs-lookup"><span data-stu-id="b2feb-261">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b2feb-262">provider_session_token</span><span class="sxs-lookup"><span data-stu-id="b2feb-262">provider_session_token</span></span> |<span data-ttu-id="b2feb-263">A Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b2feb-263">Base64 encoded string</span></span> |<span data-ttu-id="b2feb-264">A munkamenet jogkivonatából átadott vissza a licenc, és ezt követő megújításokat van jelen.</span><span class="sxs-lookup"><span data-stu-id="b2feb-264">This session token is passed back in the license and will exist in subsequent renewals.</span></span>  <span data-ttu-id="b2feb-265">A munkameneti jogkivonat nem fogja megőrizni munkamenetek túl.</span><span class="sxs-lookup"><span data-stu-id="b2feb-265">The session token will not persist beyond sessions.</span></span> |
| <span data-ttu-id="b2feb-266">provider_client_token</span><span class="sxs-lookup"><span data-stu-id="b2feb-266">provider_client_token</span></span> |<span data-ttu-id="b2feb-267">A Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b2feb-267">Base64 encoded string</span></span> |<span data-ttu-id="b2feb-268">Ügyfél jogkivonatának visszaállítása a licenc válaszul küldendő.</span><span class="sxs-lookup"><span data-stu-id="b2feb-268">Client token to send back in the license response.</span></span>  <span data-ttu-id="b2feb-269">Ha a licenc kérelem ügyfél jogkivonatot tartalmaz, a rendszer figyelmen kívül hagyja ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="b2feb-269">If the license request contains a client token, this value is ignored.</span></span> <span data-ttu-id="b2feb-270">Az ügyfél jogkivonatának licenc munkamenetek túl fogja is.</span><span class="sxs-lookup"><span data-stu-id="b2feb-270">The client token will persist beyond license sessions.</span></span> |
| <span data-ttu-id="b2feb-271">override_provider_client_token</span><span class="sxs-lookup"><span data-stu-id="b2feb-271">override_provider_client_token</span></span> |<span data-ttu-id="b2feb-272">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="b2feb-272">boolean.</span></span> <span data-ttu-id="b2feb-273">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="b2feb-273">true or false</span></span> |<span data-ttu-id="b2feb-274">Ha hamis, valamint a licenc kérelem olyan ügyfél-jogkivonatot tartalmaz, akkor is, ha egy ügyfél jogkivonatának volt megadva, ez a struktúra a használja a jogkivonatot a kérelemből.</span><span class="sxs-lookup"><span data-stu-id="b2feb-274">If false and the license request contains a client token, use the token from the request even if a client token was specified in this structure.</span></span>  <span data-ttu-id="b2feb-275">Igaz értéke esetén mindig használja a lexikális elem van megadva a struktúrában.</span><span class="sxs-lookup"><span data-stu-id="b2feb-275">If true, always use the token specified in this structure.</span></span> |

## <a name="configure-your-widevine-licenses-using-net-types"></a><span data-ttu-id="b2feb-276">A .NET-típus használatával Widevine-licencek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b2feb-276">Configure your Widevine licenses using .NET types</span></span>
<span data-ttu-id="b2feb-277">A Media Services .NET API-k, amelyek lehetővé teszik a Widevine-licencek konfigurálása biztosít.</span><span class="sxs-lookup"><span data-stu-id="b2feb-277">Media Services provides .NET APIs that let you configure your Widevine licenses.</span></span> 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a><span data-ttu-id="b2feb-278">A Media Services .NET SDK osztályok</span><span class="sxs-lookup"><span data-stu-id="b2feb-278">Classes as defined in the Media Services .NET SDK</span></span>
<span data-ttu-id="b2feb-279">Az alábbiakban a következő típusú definíciókat.</span><span class="sxs-lookup"><span data-stu-id="b2feb-279">The following are the definitions of these types.</span></span>

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a><span data-ttu-id="b2feb-280">Példa</span><span class="sxs-lookup"><span data-stu-id="b2feb-280">Example</span></span>
<span data-ttu-id="b2feb-281">A következő példa bemutatja, hogyan egy egyszerű Widevine-licenc konfigurálása a .NET API-k segítségével.</span><span class="sxs-lookup"><span data-stu-id="b2feb-281">The following example shows how to use .NET APIs to configure  a simple Widevine license.</span></span>

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


## <a name="media-services-learning-paths"></a><span data-ttu-id="b2feb-282">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="b2feb-282">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b2feb-283">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="b2feb-283">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="b2feb-284">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b2feb-284">See also</span></span>
[<span data-ttu-id="b2feb-285">PlayReady és/vagy Widevine Dynamic Common Encryption használatával</span><span class="sxs-lookup"><span data-stu-id="b2feb-285">Using PlayReady and/or Widevine Dynamic Common Encryption</span></span>](media-services-protect-with-drm.md)

