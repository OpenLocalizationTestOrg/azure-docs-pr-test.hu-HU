---
title: "aaaWidevine licenc sablon áttekintése |} Microsoft Docs"
description: "Ez a témakör áttekintést tooconfigure Widevine-licencek használt Widevine-licencsablon."
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
ms.openlocfilehash: 67a6ae38cf3d3c21e1b7282aef15f79b21776414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="widevine-license-template-overview"></a><span data-ttu-id="4d010-103">Widevine-licenc sablon – áttekintés</span><span class="sxs-lookup"><span data-stu-id="4d010-103">Widevine license template overview</span></span>
## <a name="overview"></a><span data-ttu-id="4d010-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4d010-104">Overview</span></span>
<span data-ttu-id="4d010-105">Az Azure Media Services mostantól lehetővé teszi tooconfigure és kérelem Widevine-licencek.</span><span class="sxs-lookup"><span data-stu-id="4d010-105">Azure Media Services now enables you tooconfigure and request Widevine licenses.</span></span> <span data-ttu-id="4d010-106">Amikor hello végfelhasználói player tooplay a védett Widevine, egy kérelem tartalma elküldött toohello licenc kézbesítési szolgáltatás tooobtain licencet.</span><span class="sxs-lookup"><span data-stu-id="4d010-106">When hello end user player tries tooplay your Widevine protected content, a request is sent toohello license delivery service tooobtain a license.</span></span> <span data-ttu-id="4d010-107">Hello licencszolgáltatás hello kérelmet jóváhagyja, ha hello licenc, amely elküldött toohello ügyfél kapcsolatos, és képes lehet használt toodecrypt és play hello megadott tartalom.</span><span class="sxs-lookup"><span data-stu-id="4d010-107">If hello license service approves hello request, it issues hello license which is sent toohello client and can be used toodecrypt and play hello specified content.</span></span>

<span data-ttu-id="4d010-108">Widevine-licenc kérelem JSON üzenet formátuma.</span><span class="sxs-lookup"><span data-stu-id="4d010-108">Widevine license request is formatted as a JSON message.</span></span>  

>[!NOTE]
> <span data-ttu-id="4d010-109">Használhatja a toocreate üres üzenetben nincs érték csak "{}" és a licencsablon alapbeállításokkal jön létre.</span><span class="sxs-lookup"><span data-stu-id="4d010-109">You can choose toocreate an empty message with no values just "{}" and a license template will be created with all defaults.</span></span> <span data-ttu-id="4d010-110">hello alapértelmezett működik a legtöbb esetben.</span><span class="sxs-lookup"><span data-stu-id="4d010-110">hello default works for most cases.</span></span> <span data-ttu-id="4d010-111">Például a MS-alapú licenc kézbesítési esetében, amely alapértelmezett mindig kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4d010-111">For example, for MS based license delivery scenarios that should always be default.</span></span> <span data-ttu-id="4d010-112">Tooset hello "provider" és "content_id" értékek van szükség, ha egy szolgáltató egyeznie kell a Google Widevine hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="4d010-112">If you do need tooset hello "provider" and "content_id" values, a provider must match Google's Widevine credentials.</span></span>

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

## <a name="json-message"></a><span data-ttu-id="4d010-113">JSON-üzenet</span><span class="sxs-lookup"><span data-stu-id="4d010-113">JSON message</span></span>
| <span data-ttu-id="4d010-114">Név</span><span class="sxs-lookup"><span data-stu-id="4d010-114">Name</span></span> | <span data-ttu-id="4d010-115">Érték</span><span class="sxs-lookup"><span data-stu-id="4d010-115">Value</span></span> | <span data-ttu-id="4d010-116">Leírás</span><span class="sxs-lookup"><span data-stu-id="4d010-116">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4d010-117">tartalom</span><span class="sxs-lookup"><span data-stu-id="4d010-117">payload</span></span> |<span data-ttu-id="4d010-118">A Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4d010-118">Base64 encoded string</span></span> |<span data-ttu-id="4d010-119">ügyfél által küldött hello licenc kérelem.</span><span class="sxs-lookup"><span data-stu-id="4d010-119">hello license request sent by a client.</span></span> |
| <span data-ttu-id="4d010-120">content_id</span><span class="sxs-lookup"><span data-stu-id="4d010-120">content_id</span></span> |<span data-ttu-id="4d010-121">A Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4d010-121">Base64 encoded string</span></span> |<span data-ttu-id="4d010-122">Azonosítót minden content_key_specs.track_type tooderive KeyId(s) és a tartalom kulcsoknak használni.</span><span class="sxs-lookup"><span data-stu-id="4d010-122">Identifier used tooderive KeyId(s) and Content Key(s) for each content_key_specs.track_type.</span></span> |
| <span data-ttu-id="4d010-123">Szolgáltató</span><span class="sxs-lookup"><span data-stu-id="4d010-123">provider</span></span> |<span data-ttu-id="4d010-124">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4d010-124">string</span></span> |<span data-ttu-id="4d010-125">Tartalom kulcsok és a házirendek használt toolook.</span><span class="sxs-lookup"><span data-stu-id="4d010-125">Used toolook up content keys and policies.</span></span> <span data-ttu-id="4d010-126">MS kulcs kézbesítési Widevine-licenc kézbesítésre használata esetén a rendszer figyelmen kívül hagyja ezt a paramétert.</span><span class="sxs-lookup"><span data-stu-id="4d010-126">If MS key delivery is used for Widevine license delivery, this parameter is ignored.</span></span> |
| <span data-ttu-id="4d010-127">házirendnév</span><span class="sxs-lookup"><span data-stu-id="4d010-127">policy_name</span></span> |<span data-ttu-id="4d010-128">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4d010-128">string</span></span> |<span data-ttu-id="4d010-129">Egy korábban regisztrált házirend nevét.</span><span class="sxs-lookup"><span data-stu-id="4d010-129">Name of a previously registered policy.</span></span> <span data-ttu-id="4d010-130">Optional</span><span class="sxs-lookup"><span data-stu-id="4d010-130">Optional</span></span> |
| <span data-ttu-id="4d010-131">allowed_track_types</span><span class="sxs-lookup"><span data-stu-id="4d010-131">allowed_track_types</span></span> |<span data-ttu-id="4d010-132">Enum</span><span class="sxs-lookup"><span data-stu-id="4d010-132">enum</span></span> |<span data-ttu-id="4d010-133">SD_ONLY vagy SD_HD.</span><span class="sxs-lookup"><span data-stu-id="4d010-133">SD_ONLY or SD_HD.</span></span> <span data-ttu-id="4d010-134">Vezérlők, amelyek kulcsok tartalom szerepelnie kell egy licencet</span><span class="sxs-lookup"><span data-stu-id="4d010-134">Controls which content keys should be included in a license</span></span> |
| <span data-ttu-id="4d010-135">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="4d010-135">content_key_specs</span></span> |<span data-ttu-id="4d010-136">a tömb JSON struktúrák, lásd: **tartalom kulcs specifikációk** alatt</span><span class="sxs-lookup"><span data-stu-id="4d010-136">array of JSON structures, see **Content Key Specs** below</span></span> |<span data-ttu-id="4d010-137">Egy egyeztetését megbízhatatlanná a tartalmat a kulcsok tooreturn vezérlő.</span><span class="sxs-lookup"><span data-stu-id="4d010-137">A finer grained control on what content keys tooreturn.</span></span> <span data-ttu-id="4d010-138">Tartalom kulcs Spec alatt talál információt.</span><span class="sxs-lookup"><span data-stu-id="4d010-138">See Content Key Spec below for details.</span></span>  <span data-ttu-id="4d010-139">Allowed_track_types és content_key_specs csak egy adható meg.</span><span class="sxs-lookup"><span data-stu-id="4d010-139">Only one of allowed_track_types and content_key_specs can be specified.</span></span> |
| <span data-ttu-id="4d010-140">use_policy_overrides_exclusively</span><span class="sxs-lookup"><span data-stu-id="4d010-140">use_policy_overrides_exclusively</span></span> |<span data-ttu-id="4d010-141">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="4d010-141">boolean.</span></span> <span data-ttu-id="4d010-142">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="4d010-142">true or false</span></span> |<span data-ttu-id="4d010-143">Policy_overrides által megadott házirend attribútumok használata, és hagyja el az összes korábban tárolt házirend.</span><span class="sxs-lookup"><span data-stu-id="4d010-143">Use policy attributes specified by policy_overrides and omit all previously stored policy.</span></span> |
| <span data-ttu-id="4d010-144">policy_overrides</span><span class="sxs-lookup"><span data-stu-id="4d010-144">policy_overrides</span></span> |<span data-ttu-id="4d010-145">JSON-struktúrát, lásd: **házirend felülírja** alatt</span><span class="sxs-lookup"><span data-stu-id="4d010-145">JSON structure, see **Policy Overrides** below</span></span> |<span data-ttu-id="4d010-146">Ez a licenc házirend beállításait.</span><span class="sxs-lookup"><span data-stu-id="4d010-146">Policy settings for this license.</span></span>  <span data-ttu-id="4d010-147">Hello esetben ez az eszköz egy előre definiált szabályzattal rendelkezik, a megadott értékeket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="4d010-147">In hello event this asset has a pre-defined policy, these specified values will be used.</span></span> |
| <span data-ttu-id="4d010-148">session_init</span><span class="sxs-lookup"><span data-stu-id="4d010-148">session_init</span></span> |<span data-ttu-id="4d010-149">JSON-struktúrát, lásd: **munkamenet inicializálási** alatt</span><span class="sxs-lookup"><span data-stu-id="4d010-149">JSON structure, see **Session Initialization** below</span></span> |<span data-ttu-id="4d010-150">Nem kötelező adatokat kapott toolicense.</span><span class="sxs-lookup"><span data-stu-id="4d010-150">Optional data passed toolicense.</span></span> |
| <span data-ttu-id="4d010-151">parse_only</span><span class="sxs-lookup"><span data-stu-id="4d010-151">parse_only</span></span> |<span data-ttu-id="4d010-152">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="4d010-152">boolean.</span></span> <span data-ttu-id="4d010-153">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="4d010-153">true or false</span></span> |<span data-ttu-id="4d010-154">hello licenc kérelem elemzi, de licenc nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="4d010-154">hello license request is parsed but no license is issued.</span></span> <span data-ttu-id="4d010-155">Azonban a értékek űrlap hello licenc kérelem visszaadott hello válaszként.</span><span class="sxs-lookup"><span data-stu-id="4d010-155">However, values form hello license request are returned in hello response.</span></span> |

## <a name="content-key-specs"></a><span data-ttu-id="4d010-156">Tartalomkulcs specifikációk</span><span class="sxs-lookup"><span data-stu-id="4d010-156">Content Key Specs</span></span>
<span data-ttu-id="4d010-157">Ha egy már meglévő házirend létezik, nincs nincs szükség toospecify hello bármelyikét hello tartalom kulcs Spec értékei.  Ehhez a tartalomhoz társított hello már meglévő házirend használt toodetermine hello kimeneti védelmi például HDCP és CGMS lesz.</span><span class="sxs-lookup"><span data-stu-id="4d010-157">If a pre-existing policy exist, there is no need toospecify any of hello values in hello Content Key Spec.  hello pre-existing policy associated with this content will be used toodetermine hello output protection such as HDCP and CGMS.</span></span>  <span data-ttu-id="4d010-158">Ha egy már meglévő házirend hello Widevine-licenc kiszolgáló nincs regisztrálva, hello tartalomszolgáltató is behelyezése hello értékek hello licenc kérelem.</span><span class="sxs-lookup"><span data-stu-id="4d010-158">If a pre-existing policy is not registered with hello Widevine License Server, hello content provider can inject hello values into hello license request.</span></span>   

<span data-ttu-id="4d010-159">Minden egyes content_key_specs meg kell adni az összes nyomon követi, hello beállítás use_policy_overrides_exclusively függetlenül.</span><span class="sxs-lookup"><span data-stu-id="4d010-159">Each content_key_specs must be specified for all tracks, regardless of hello option use_policy_overrides_exclusively.</span></span> 

| <span data-ttu-id="4d010-160">Név</span><span class="sxs-lookup"><span data-stu-id="4d010-160">Name</span></span> | <span data-ttu-id="4d010-161">Érték</span><span class="sxs-lookup"><span data-stu-id="4d010-161">Value</span></span> | <span data-ttu-id="4d010-162">Leírás</span><span class="sxs-lookup"><span data-stu-id="4d010-162">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4d010-163">content_key_specs.</span><span class="sxs-lookup"><span data-stu-id="4d010-163">content_key_specs.</span></span> <span data-ttu-id="4d010-164">track_type</span><span class="sxs-lookup"><span data-stu-id="4d010-164">track_type</span></span> |<span data-ttu-id="4d010-165">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4d010-165">string</span></span> |<span data-ttu-id="4d010-166">Track adattípus neve.</span><span class="sxs-lookup"><span data-stu-id="4d010-166">A track type name.</span></span> <span data-ttu-id="4d010-167">Ha content_key_specs hello licenc kérés van beállítva, győződjön meg arról, hogy az összes nyomon toospecify típusok explicit módon.</span><span class="sxs-lookup"><span data-stu-id="4d010-167">If content_key_specs is specified in hello license request, make sure toospecify all track types explicitly.</span></span> <span data-ttu-id="4d010-168">Hiba toodo így eredményez hiba tooplayback elmúlt 10 másodperc.</span><span class="sxs-lookup"><span data-stu-id="4d010-168">Failure toodo so will result in failure tooplayback past 10 seconds.</span></span> |
| <span data-ttu-id="4d010-169">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="4d010-169">content_key_specs</span></span>  <br/> <span data-ttu-id="4d010-170">security_level</span><span class="sxs-lookup"><span data-stu-id="4d010-170">security_level</span></span> |<span data-ttu-id="4d010-171">UInt32</span><span class="sxs-lookup"><span data-stu-id="4d010-171">uint32</span></span> |<span data-ttu-id="4d010-172">Meghatározza a lejátszás ügyfél szolgáltatás megbízhatóságára követelményeit.</span><span class="sxs-lookup"><span data-stu-id="4d010-172">Defines client robustness requirements for playback.</span></span> <br/> <span data-ttu-id="4d010-173">1 - szoftveres whitebox titkosítási szükség.</span><span class="sxs-lookup"><span data-stu-id="4d010-173">1 - Software-based whitebox crypto is required.</span></span> <br/> <span data-ttu-id="4d010-174">2 - szoftver titkosítási és egy rejtjelezett dekóder szükség.</span><span class="sxs-lookup"><span data-stu-id="4d010-174">2 - Software crypto and an obfuscated decoder is required.</span></span> <br/> <span data-ttu-id="4d010-175">3 - a hardveres biztonsági megbízható végrehajtási környezetekben hello kulcs jelentős és titkosítási műveleteket kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="4d010-175">3 - hello key material and crypto operations must be performed within a hardware backed trusted execution environment.</span></span> <br/> <span data-ttu-id="4d010-176">4 - hello titkosítási és dekódolási tartalom hardver megbízható végrehajtási biztonsági környezetben kell végrehajtani.</span><span class="sxs-lookup"><span data-stu-id="4d010-176">4 - hello crypto and decoding of content must be performed within a hardware backed trusted execution environment.</span></span>  <br/> <span data-ttu-id="4d010-177">5 - hello titkosítási, dekódolási és az összes kezelési hello adathordozó (tömörített és tömörítetlen) belül egy hardveres biztonsági megbízható végrehajtási környezet kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="4d010-177">5 - hello crypto, decoding and all handling of hello media (compressed and uncompressed) must be handled within a hardware backed trusted execution environment.</span></span> |
| <span data-ttu-id="4d010-178">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="4d010-178">content_key_specs</span></span> <br/> <span data-ttu-id="4d010-179">required_output_protection.hDC</span><span class="sxs-lookup"><span data-stu-id="4d010-179">required_output_protection.hdc</span></span> |<span data-ttu-id="4d010-180">karakterlánc - egyikét: HDCP_NONE, HDCP_V1, HDCP_V2</span><span class="sxs-lookup"><span data-stu-id="4d010-180">string - one of: HDCP_NONE, HDCP_V1, HDCP_V2</span></span> |<span data-ttu-id="4d010-181">Azt jelzi, hogy szükség van-e a HDCP</span><span class="sxs-lookup"><span data-stu-id="4d010-181">Indicates whether HDCP is require</span></span> |
| <span data-ttu-id="4d010-182">content_key_specs</span><span class="sxs-lookup"><span data-stu-id="4d010-182">content_key_specs</span></span> <br/><span data-ttu-id="4d010-183">kulcs</span><span class="sxs-lookup"><span data-stu-id="4d010-183">key</span></span> |<span data-ttu-id="4d010-184">a Base64</span><span class="sxs-lookup"><span data-stu-id="4d010-184">Base64</span></span> <br/><span data-ttu-id="4d010-185">kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4d010-185">encoded string</span></span> |<span data-ttu-id="4d010-186">A szám tartalom kulcs toouse. Ha meg van adva, hello track_type, vagy key_id szükség.</span><span class="sxs-lookup"><span data-stu-id="4d010-186">Content key toouse for this track. If specified, hello track_type or key_id is required.</span></span>  <span data-ttu-id="4d010-187">Ez a beállítás lehetővé teszi, hogy hello tartalomszolgáltató tooinject hello tartalomkulcsot a track ahelyett, hogy a Widevine licenckiszolgáló létrehozni, vagy keresési kulcs.</span><span class="sxs-lookup"><span data-stu-id="4d010-187">This option allows hello content provider tooinject hello content key for this track instead of letting Widevine license server generate or lookup a key.</span></span> |
| <span data-ttu-id="4d010-188">content_key_specs.key_id</span><span class="sxs-lookup"><span data-stu-id="4d010-188">content_key_specs.key_id</span></span> |<span data-ttu-id="4d010-189">Base64 kódolású karakterlánc bináris, 16 bájt</span><span class="sxs-lookup"><span data-stu-id="4d010-189">Base64 encoded string  binary, 16 bytes</span></span> |<span data-ttu-id="4d010-190">Hello kulcs egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="4d010-190">Unique identifier for hello key.</span></span> |

## <a name="policy-overrides"></a><span data-ttu-id="4d010-191">Házirend felülbírálása</span><span class="sxs-lookup"><span data-stu-id="4d010-191">Policy Overrides</span></span>
| <span data-ttu-id="4d010-192">Név</span><span class="sxs-lookup"><span data-stu-id="4d010-192">Name</span></span> | <span data-ttu-id="4d010-193">Érték</span><span class="sxs-lookup"><span data-stu-id="4d010-193">Value</span></span> | <span data-ttu-id="4d010-194">Leírás</span><span class="sxs-lookup"><span data-stu-id="4d010-194">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4d010-195">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-195">policy_overrides.</span></span> <span data-ttu-id="4d010-196">can_play</span><span class="sxs-lookup"><span data-stu-id="4d010-196">can_play</span></span> |<span data-ttu-id="4d010-197">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="4d010-197">boolean.</span></span> <span data-ttu-id="4d010-198">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="4d010-198">true or false</span></span> |<span data-ttu-id="4d010-199">Azt jelzi, hogy a lejátszás hello a tartalom engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="4d010-199">Indicates that playback of hello content is allowed.</span></span> <span data-ttu-id="4d010-200">Alapértelmezett értéke false.</span><span class="sxs-lookup"><span data-stu-id="4d010-200">Default is false.</span></span> |
| <span data-ttu-id="4d010-201">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-201">policy_overrides.</span></span> <span data-ttu-id="4d010-202">can_persist</span><span class="sxs-lookup"><span data-stu-id="4d010-202">can_persist</span></span> |<span data-ttu-id="4d010-203">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="4d010-203">boolean.</span></span> <span data-ttu-id="4d010-204">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="4d010-204">true or false</span></span> |<span data-ttu-id="4d010-205">Azt jelzi, lehet, hogy az adott hello licenc megőrzött toonon felejtő kapcsolat nélküli használatra.</span><span class="sxs-lookup"><span data-stu-id="4d010-205">Indicates that hello license may be persisted toonon-volatile storage for offline use.</span></span> <span data-ttu-id="4d010-206">Alapértelmezett értéke false.</span><span class="sxs-lookup"><span data-stu-id="4d010-206">Default is false.</span></span> |
| <span data-ttu-id="4d010-207">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-207">policy_overrides.</span></span> <span data-ttu-id="4d010-208">can_renew</span><span class="sxs-lookup"><span data-stu-id="4d010-208">can_renew</span></span> |<span data-ttu-id="4d010-209">logikai változó értéke IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="4d010-209">boolean true or false</span></span> |<span data-ttu-id="4d010-210">Azt jelzi, hogy ez a licenc megújítása.</span><span class="sxs-lookup"><span data-stu-id="4d010-210">Indicates that renewal of this license is allowed.</span></span> <span data-ttu-id="4d010-211">Igaz értéke esetén hello licenc hello időtartama szívverés lehet kiterjeszteni.</span><span class="sxs-lookup"><span data-stu-id="4d010-211">If true, hello duration of hello license can be extended by heartbeat.</span></span> <span data-ttu-id="4d010-212">Alapértelmezett értéke false.</span><span class="sxs-lookup"><span data-stu-id="4d010-212">Default is false.</span></span> |
| <span data-ttu-id="4d010-213">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-213">policy_overrides.</span></span> <span data-ttu-id="4d010-214">license_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="4d010-214">license_duration_seconds</span></span> |<span data-ttu-id="4d010-215">Int64</span><span class="sxs-lookup"><span data-stu-id="4d010-215">int64</span></span> |<span data-ttu-id="4d010-216">Azt jelzi, hogy az adott licenccsoport hello időablakot.</span><span class="sxs-lookup"><span data-stu-id="4d010-216">Indicates hello time window for this specific license.</span></span> <span data-ttu-id="4d010-217">A 0 érték azt jelzi, hogy nincs-e nincs korlát toohello időtartam.</span><span class="sxs-lookup"><span data-stu-id="4d010-217">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="4d010-218">Alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="4d010-218">Default is 0.</span></span> |
| <span data-ttu-id="4d010-219">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-219">policy_overrides.</span></span> <span data-ttu-id="4d010-220">rental_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="4d010-220">rental_duration_seconds</span></span> |<span data-ttu-id="4d010-221">Int64</span><span class="sxs-lookup"><span data-stu-id="4d010-221">int64</span></span> |<span data-ttu-id="4d010-222">Azt jelzi, hello időkerete, amíg a lejátszás engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="4d010-222">Indicates hello time window while playback is permitted.</span></span> <span data-ttu-id="4d010-223">A 0 érték azt jelzi, hogy nincs-e nincs korlát toohello időtartam.</span><span class="sxs-lookup"><span data-stu-id="4d010-223">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="4d010-224">Alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="4d010-224">Default is 0.</span></span> |
| <span data-ttu-id="4d010-225">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-225">policy_overrides.</span></span> <span data-ttu-id="4d010-226">playback_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="4d010-226">playback_duration_seconds</span></span> |<span data-ttu-id="4d010-227">Int64</span><span class="sxs-lookup"><span data-stu-id="4d010-227">int64</span></span> |<span data-ttu-id="4d010-228">hello megtekintése a lejátszás elindítása belül hello licenc időtartam után eltelt időszakot.</span><span class="sxs-lookup"><span data-stu-id="4d010-228">hello viewing window of time once playback starts within hello license duration.</span></span> <span data-ttu-id="4d010-229">A 0 érték azt jelzi, hogy nincs-e nincs korlát toohello időtartam.</span><span class="sxs-lookup"><span data-stu-id="4d010-229">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="4d010-230">Alapértelmezett érték 0.</span><span class="sxs-lookup"><span data-stu-id="4d010-230">Default is 0.</span></span> |
| <span data-ttu-id="4d010-231">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-231">policy_overrides.</span></span> <span data-ttu-id="4d010-232">renewal_server_url</span><span class="sxs-lookup"><span data-stu-id="4d010-232">renewal_server_url</span></span> |<span data-ttu-id="4d010-233">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4d010-233">string</span></span> |<span data-ttu-id="4d010-234">Ez a licenc vonatkozó összes szívverés (megújítási) toohello megadott URL-címet kell irányítani.</span><span class="sxs-lookup"><span data-stu-id="4d010-234">All heartbeat (renewal) requests for this license shall be directed toohello specified URL.</span></span> <span data-ttu-id="4d010-235">Ha igaz can_renew csak használja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="4d010-235">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="4d010-236">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-236">policy_overrides.</span></span> <span data-ttu-id="4d010-237">renewal_delay_seconds</span><span class="sxs-lookup"><span data-stu-id="4d010-237">renewal_delay_seconds</span></span> |<span data-ttu-id="4d010-238">Int64</span><span class="sxs-lookup"><span data-stu-id="4d010-238">int64</span></span> |<span data-ttu-id="4d010-239">Hány másodperc után license_start_time, megújítási először megkísérlése előtt.</span><span class="sxs-lookup"><span data-stu-id="4d010-239">How many seconds after license_start_time, before renewal is first attempted.</span></span> <span data-ttu-id="4d010-240">Ha igaz can_renew csak használja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="4d010-240">This field is only used if can_renew is true.</span></span> <span data-ttu-id="4d010-241">Az alapértelmezett érték: 0</span><span class="sxs-lookup"><span data-stu-id="4d010-241">Default is 0</span></span> |
| <span data-ttu-id="4d010-242">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-242">policy_overrides.</span></span> <span data-ttu-id="4d010-243">renewal_retry_interval_seconds</span><span class="sxs-lookup"><span data-stu-id="4d010-243">renewal_retry_interval_seconds</span></span> |<span data-ttu-id="4d010-244">Int64</span><span class="sxs-lookup"><span data-stu-id="4d010-244">int64</span></span> |<span data-ttu-id="4d010-245">Másodpercben hello késleltetés között további licenc megújítási kérelmeket, hiba esetén.</span><span class="sxs-lookup"><span data-stu-id="4d010-245">Specifies hello delay in seconds between subsequent license renewal requests, in case of failure.</span></span> <span data-ttu-id="4d010-246">Ha igaz can_renew csak használja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="4d010-246">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="4d010-247">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-247">policy_overrides.</span></span> <span data-ttu-id="4d010-248">renewal_recovery_duration_seconds</span><span class="sxs-lookup"><span data-stu-id="4d010-248">renewal_recovery_duration_seconds</span></span> |<span data-ttu-id="4d010-249">Int64</span><span class="sxs-lookup"><span data-stu-id="4d010-249">int64</span></span> |<span data-ttu-id="4d010-250">az időt, amelyben a lejátszás során megújítási toocontinue hello ablak megkísérelt, még a hello licenckiszolgáló toobackend kapcsolatos problémák miatt sikertelen.</span><span class="sxs-lookup"><span data-stu-id="4d010-250">hello window of time, in which playback is allowed toocontinue while renewal is attempted, yet unsuccessful due toobackend problems with hello license server.</span></span> <span data-ttu-id="4d010-251">A 0 érték azt jelzi, hogy nincs-e nincs korlát toohello időtartam.</span><span class="sxs-lookup"><span data-stu-id="4d010-251">A value of 0 indicates that there is no limit toohello duration.</span></span> <span data-ttu-id="4d010-252">Ha igaz can_renew csak használja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="4d010-252">This field is only used if can_renew is true.</span></span> |
| <span data-ttu-id="4d010-253">policy_overrides.</span><span class="sxs-lookup"><span data-stu-id="4d010-253">policy_overrides.</span></span> <span data-ttu-id="4d010-254">renew_with_usage</span><span class="sxs-lookup"><span data-stu-id="4d010-254">renew_with_usage</span></span> |<span data-ttu-id="4d010-255">logikai változó értéke IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="4d010-255">boolean true or false</span></span> |<span data-ttu-id="4d010-256">Azt jelzi, hogy hello licenc küldik a megújítási használat indításakor.</span><span class="sxs-lookup"><span data-stu-id="4d010-256">Indicates that hello license shall be sent for renewal when usage is started.</span></span> <span data-ttu-id="4d010-257">Ha igaz can_renew csak használja ezt a mezőt.</span><span class="sxs-lookup"><span data-stu-id="4d010-257">This field is only used if can_renew is true.</span></span> |

## <a name="session-initialization"></a><span data-ttu-id="4d010-258">Munkamenet-inicializálása</span><span class="sxs-lookup"><span data-stu-id="4d010-258">Session Initialization</span></span>
| <span data-ttu-id="4d010-259">Név</span><span class="sxs-lookup"><span data-stu-id="4d010-259">Name</span></span> | <span data-ttu-id="4d010-260">Érték</span><span class="sxs-lookup"><span data-stu-id="4d010-260">Value</span></span> | <span data-ttu-id="4d010-261">Leírás</span><span class="sxs-lookup"><span data-stu-id="4d010-261">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="4d010-262">provider_session_token</span><span class="sxs-lookup"><span data-stu-id="4d010-262">provider_session_token</span></span> |<span data-ttu-id="4d010-263">A Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4d010-263">Base64 encoded string</span></span> |<span data-ttu-id="4d010-264">A munkamenet jogkivonatából átadott vissza hello licenc, és ezt követő megújításokat van jelen.</span><span class="sxs-lookup"><span data-stu-id="4d010-264">This session token is passed back in hello license and will exist in subsequent renewals.</span></span>  <span data-ttu-id="4d010-265">hello munkameneti jogkivonat nem fogja megőrizni munkamenetek túl.</span><span class="sxs-lookup"><span data-stu-id="4d010-265">hello session token will not persist beyond sessions.</span></span> |
| <span data-ttu-id="4d010-266">provider_client_token</span><span class="sxs-lookup"><span data-stu-id="4d010-266">provider_client_token</span></span> |<span data-ttu-id="4d010-267">A Base64 kódolású karakterlánc</span><span class="sxs-lookup"><span data-stu-id="4d010-267">Base64 encoded string</span></span> |<span data-ttu-id="4d010-268">Ügyfél token toosend vissza válaszként hello licenc.</span><span class="sxs-lookup"><span data-stu-id="4d010-268">Client token toosend back in hello license response.</span></span>  <span data-ttu-id="4d010-269">Ha hello licenc kérelem ügyfél jogkivonatot tartalmaz, a rendszer figyelmen kívül hagyja ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="4d010-269">If hello license request contains a client token, this value is ignored.</span></span> <span data-ttu-id="4d010-270">hello ügyfél jogkivonatának licenc munkamenetek túl fogja is.</span><span class="sxs-lookup"><span data-stu-id="4d010-270">hello client token will persist beyond license sessions.</span></span> |
| <span data-ttu-id="4d010-271">override_provider_client_token</span><span class="sxs-lookup"><span data-stu-id="4d010-271">override_provider_client_token</span></span> |<span data-ttu-id="4d010-272">Logikai érték.</span><span class="sxs-lookup"><span data-stu-id="4d010-272">boolean.</span></span> <span data-ttu-id="4d010-273">IGAZ vagy HAMIS eredményt ad</span><span class="sxs-lookup"><span data-stu-id="4d010-273">true or false</span></span> |<span data-ttu-id="4d010-274">Ha hamis, valamint a hello licenc kérelem ügyfél jogkivonatot tartalmaz, akkor is, ha egy ügyfél jogkivonatának volt megadva, ez a struktúra a használja hello token hello kérelemből.</span><span class="sxs-lookup"><span data-stu-id="4d010-274">If false and hello license request contains a client token, use hello token from hello request even if a client token was specified in this structure.</span></span>  <span data-ttu-id="4d010-275">Igaz értéke esetén mindig használjon hello lexikális eleme van megadva a struktúrában.</span><span class="sxs-lookup"><span data-stu-id="4d010-275">If true, always use hello token specified in this structure.</span></span> |

## <a name="configure-your-widevine-licenses-using-net-types"></a><span data-ttu-id="4d010-276">A .NET-típus használatával Widevine-licencek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4d010-276">Configure your Widevine licenses using .NET types</span></span>
<span data-ttu-id="4d010-277">A Media Services .NET API-k, amelyek lehetővé teszik a Widevine-licencek konfigurálása biztosít.</span><span class="sxs-lookup"><span data-stu-id="4d010-277">Media Services provides .NET APIs that let you configure your Widevine licenses.</span></span> 

### <a name="classes-as-defined-in-hello-media-services-net-sdk"></a><span data-ttu-id="4d010-278">A Media Services .NET SDK hello osztályok</span><span class="sxs-lookup"><span data-stu-id="4d010-278">Classes as defined in hello Media Services .NET SDK</span></span>
<span data-ttu-id="4d010-279">Az alábbiakban hello hello definíciókat a következő típusú.</span><span class="sxs-lookup"><span data-stu-id="4d010-279">hello following are hello definitions of these types.</span></span>

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

### <a name="example"></a><span data-ttu-id="4d010-280">Példa</span><span class="sxs-lookup"><span data-stu-id="4d010-280">Example</span></span>
<span data-ttu-id="4d010-281">a következő példa azt mutatja meg hogyan hello toouse .NET API-k tooconfigure egy egyszerű Widevine-licenc.</span><span class="sxs-lookup"><span data-stu-id="4d010-281">hello following example shows how toouse .NET APIs tooconfigure  a simple Widevine license.</span></span>

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


## <a name="media-services-learning-paths"></a><span data-ttu-id="4d010-282">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="4d010-282">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="4d010-283">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="4d010-283">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="4d010-284">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="4d010-284">See also</span></span>
[<span data-ttu-id="4d010-285">PlayReady és/vagy Widevine Dynamic Common Encryption használatával</span><span class="sxs-lookup"><span data-stu-id="4d010-285">Using PlayReady and/or Widevine Dynamic Common Encryption</span></span>](media-services-protect-with-drm.md)

