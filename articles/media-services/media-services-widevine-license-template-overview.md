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
# <a name="widevine-license-template-overview"></a>Widevine-licenc sablon – áttekintés
## <a name="overview"></a>Áttekintés
Az Azure Media Services mostantól lehetővé teszi tooconfigure és kérelem Widevine-licencek. Amikor hello végfelhasználói player tooplay a védett Widevine, egy kérelem tartalma elküldött toohello licenc kézbesítési szolgáltatás tooobtain licencet. Hello licencszolgáltatás hello kérelmet jóváhagyja, ha hello licenc, amely elküldött toohello ügyfél kapcsolatos, és képes lehet használt toodecrypt és play hello megadott tartalom.

Widevine-licenc kérelem JSON üzenet formátuma.  

>[!NOTE]
> Használhatja a toocreate üres üzenetben nincs érték csak "{}" és a licencsablon alapbeállításokkal jön létre. hello alapértelmezett működik a legtöbb esetben. Például a MS-alapú licenc kézbesítési esetében, amely alapértelmezett mindig kell lennie. Tooset hello "provider" és "content_id" értékek van szükség, ha egy szolgáltató egyeznie kell a Google Widevine hitelesítő adatokat.

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

## <a name="json-message"></a>JSON-üzenet
| Név | Érték | Leírás |
| --- | --- | --- |
| tartalom |A Base64 kódolású karakterlánc |ügyfél által küldött hello licenc kérelem. |
| content_id |A Base64 kódolású karakterlánc |Azonosítót minden content_key_specs.track_type tooderive KeyId(s) és a tartalom kulcsoknak használni. |
| Szolgáltató |Karakterlánc |Tartalom kulcsok és a házirendek használt toolook. MS kulcs kézbesítési Widevine-licenc kézbesítésre használata esetén a rendszer figyelmen kívül hagyja ezt a paramétert. |
| házirendnév |Karakterlánc |Egy korábban regisztrált házirend nevét. Optional |
| allowed_track_types |Enum |SD_ONLY vagy SD_HD. Vezérlők, amelyek kulcsok tartalom szerepelnie kell egy licencet |
| content_key_specs |a tömb JSON struktúrák, lásd: **tartalom kulcs specifikációk** alatt |Egy egyeztetését megbízhatatlanná a tartalmat a kulcsok tooreturn vezérlő. Tartalom kulcs Spec alatt talál információt.  Allowed_track_types és content_key_specs csak egy adható meg. |
| use_policy_overrides_exclusively |Logikai érték. IGAZ vagy HAMIS eredményt ad |Policy_overrides által megadott házirend attribútumok használata, és hagyja el az összes korábban tárolt házirend. |
| policy_overrides |JSON-struktúrát, lásd: **házirend felülírja** alatt |Ez a licenc házirend beállításait.  Hello esetben ez az eszköz egy előre definiált szabályzattal rendelkezik, a megadott értékeket fogja használni. |
| session_init |JSON-struktúrát, lásd: **munkamenet inicializálási** alatt |Nem kötelező adatokat kapott toolicense. |
| parse_only |Logikai érték. IGAZ vagy HAMIS eredményt ad |hello licenc kérelem elemzi, de licenc nem jelenik meg. Azonban a értékek űrlap hello licenc kérelem visszaadott hello válaszként. |

## <a name="content-key-specs"></a>Tartalomkulcs specifikációk
Ha egy már meglévő házirend létezik, nincs nincs szükség toospecify hello bármelyikét hello tartalom kulcs Spec értékei.  Ehhez a tartalomhoz társított hello már meglévő házirend használt toodetermine hello kimeneti védelmi például HDCP és CGMS lesz.  Ha egy már meglévő házirend hello Widevine-licenc kiszolgáló nincs regisztrálva, hello tartalomszolgáltató is behelyezése hello értékek hello licenc kérelem.   

Minden egyes content_key_specs meg kell adni az összes nyomon követi, hello beállítás use_policy_overrides_exclusively függetlenül. 

| Név | Érték | Leírás |
| --- | --- | --- |
| content_key_specs. track_type |Karakterlánc |Track adattípus neve. Ha content_key_specs hello licenc kérés van beállítva, győződjön meg arról, hogy az összes nyomon toospecify típusok explicit módon. Hiba toodo így eredményez hiba tooplayback elmúlt 10 másodperc. |
| content_key_specs  <br/> security_level |UInt32 |Meghatározza a lejátszás ügyfél szolgáltatás megbízhatóságára követelményeit. <br/> 1 - szoftveres whitebox titkosítási szükség. <br/> 2 - szoftver titkosítási és egy rejtjelezett dekóder szükség. <br/> 3 - a hardveres biztonsági megbízható végrehajtási környezetekben hello kulcs jelentős és titkosítási műveleteket kell elvégezni. <br/> 4 - hello titkosítási és dekódolási tartalom hardver megbízható végrehajtási biztonsági környezetben kell végrehajtani.  <br/> 5 - hello titkosítási, dekódolási és az összes kezelési hello adathordozó (tömörített és tömörítetlen) belül egy hardveres biztonsági megbízható végrehajtási környezet kell kezelni. |
| content_key_specs <br/> required_output_protection.hDC |karakterlánc - egyikét: HDCP_NONE, HDCP_V1, HDCP_V2 |Azt jelzi, hogy szükség van-e a HDCP |
| content_key_specs <br/>kulcs |a Base64 <br/>kódolású karakterlánc |A szám tartalom kulcs toouse. Ha meg van adva, hello track_type, vagy key_id szükség.  Ez a beállítás lehetővé teszi, hogy hello tartalomszolgáltató tooinject hello tartalomkulcsot a track ahelyett, hogy a Widevine licenckiszolgáló létrehozni, vagy keresési kulcs. |
| content_key_specs.key_id |Base64 kódolású karakterlánc bináris, 16 bájt |Hello kulcs egyedi azonosítója. |

## <a name="policy-overrides"></a>Házirend felülbírálása
| Név | Érték | Leírás |
| --- | --- | --- |
| policy_overrides. can_play |Logikai érték. IGAZ vagy HAMIS eredményt ad |Azt jelzi, hogy a lejátszás hello a tartalom engedélyezett. Alapértelmezett értéke false. |
| policy_overrides. can_persist |Logikai érték. IGAZ vagy HAMIS eredményt ad |Azt jelzi, lehet, hogy az adott hello licenc megőrzött toonon felejtő kapcsolat nélküli használatra. Alapértelmezett értéke false. |
| policy_overrides. can_renew |logikai változó értéke IGAZ vagy HAMIS eredményt ad |Azt jelzi, hogy ez a licenc megújítása. Igaz értéke esetén hello licenc hello időtartama szívverés lehet kiterjeszteni. Alapértelmezett értéke false. |
| policy_overrides. license_duration_seconds |Int64 |Azt jelzi, hogy az adott licenccsoport hello időablakot. A 0 érték azt jelzi, hogy nincs-e nincs korlát toohello időtartam. Alapértelmezett érték 0. |
| policy_overrides. rental_duration_seconds |Int64 |Azt jelzi, hello időkerete, amíg a lejátszás engedélyezett. A 0 érték azt jelzi, hogy nincs-e nincs korlát toohello időtartam. Alapértelmezett érték 0. |
| policy_overrides. playback_duration_seconds |Int64 |hello megtekintése a lejátszás elindítása belül hello licenc időtartam után eltelt időszakot. A 0 érték azt jelzi, hogy nincs-e nincs korlát toohello időtartam. Alapértelmezett érték 0. |
| policy_overrides. renewal_server_url |Karakterlánc |Ez a licenc vonatkozó összes szívverés (megújítási) toohello megadott URL-címet kell irányítani. Ha igaz can_renew csak használja ezt a mezőt. |
| policy_overrides. renewal_delay_seconds |Int64 |Hány másodperc után license_start_time, megújítási először megkísérlése előtt. Ha igaz can_renew csak használja ezt a mezőt. Az alapértelmezett érték: 0 |
| policy_overrides. renewal_retry_interval_seconds |Int64 |Másodpercben hello késleltetés között további licenc megújítási kérelmeket, hiba esetén. Ha igaz can_renew csak használja ezt a mezőt. |
| policy_overrides. renewal_recovery_duration_seconds |Int64 |az időt, amelyben a lejátszás során megújítási toocontinue hello ablak megkísérelt, még a hello licenckiszolgáló toobackend kapcsolatos problémák miatt sikertelen. A 0 érték azt jelzi, hogy nincs-e nincs korlát toohello időtartam. Ha igaz can_renew csak használja ezt a mezőt. |
| policy_overrides. renew_with_usage |logikai változó értéke IGAZ vagy HAMIS eredményt ad |Azt jelzi, hogy hello licenc küldik a megújítási használat indításakor. Ha igaz can_renew csak használja ezt a mezőt. |

## <a name="session-initialization"></a>Munkamenet-inicializálása
| Név | Érték | Leírás |
| --- | --- | --- |
| provider_session_token |A Base64 kódolású karakterlánc |A munkamenet jogkivonatából átadott vissza hello licenc, és ezt követő megújításokat van jelen.  hello munkameneti jogkivonat nem fogja megőrizni munkamenetek túl. |
| provider_client_token |A Base64 kódolású karakterlánc |Ügyfél token toosend vissza válaszként hello licenc.  Ha hello licenc kérelem ügyfél jogkivonatot tartalmaz, a rendszer figyelmen kívül hagyja ezt az értéket. hello ügyfél jogkivonatának licenc munkamenetek túl fogja is. |
| override_provider_client_token |Logikai érték. IGAZ vagy HAMIS eredményt ad |Ha hamis, valamint a hello licenc kérelem ügyfél jogkivonatot tartalmaz, akkor is, ha egy ügyfél jogkivonatának volt megadva, ez a struktúra a használja hello token hello kérelemből.  Igaz értéke esetén mindig használjon hello lexikális eleme van megadva a struktúrában. |

## <a name="configure-your-widevine-licenses-using-net-types"></a>A .NET-típus használatával Widevine-licencek konfigurálása
A Media Services .NET API-k, amelyek lehetővé teszik a Widevine-licencek konfigurálása biztosít. 

### <a name="classes-as-defined-in-hello-media-services-net-sdk"></a>A Media Services .NET SDK hello osztályok
Az alábbiakban hello hello definíciókat a következő típusú.

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

### <a name="example"></a>Példa
a következő példa azt mutatja meg hogyan hello toouse .NET API-k tooconfigure egy egyszerű Widevine-licenc.

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


## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Lásd még:
[PlayReady és/vagy Widevine Dynamic Common Encryption használatával](media-services-protect-with-drm.md)

