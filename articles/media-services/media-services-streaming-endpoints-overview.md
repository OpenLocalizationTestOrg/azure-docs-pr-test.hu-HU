---
title: "aaaAzure Media Services Streaming Endpoint áttekintése |} Microsoft Docs"
description: "Ez a témakör áttekintést nyújt az Azure Media Services adatfolyam-végpontok."
services: media-services
documentationcenter: 
author: Juliako
writer: juliako
manager: cfowler
editor: 
ms.assetid: 097ab5e5-24e1-4e8e-b112-be74172c2701
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: f27f590175dcc945d1d3299fc0cae5a68cfbf4e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="streaming-endpoints-overview"></a>Adatfolyam-továbbítási végpontok áttekintése 

##<a name="overview"></a>Áttekintés

A Microsoft Azure Media Services (AMS), egy **Streamvégponton** egy adatfolyam-szolgáltatás által biztosított tartalom közvetlenül tooa player ügyfélalkalmazást, vagy későbbi terjesztés Content Delivery Network (CDN) tooa jelöli. A Media Services is biztosít az Azure CDN való zökkenőmentes integrációt. élő stream, igény szerint, vagy az eszköz a Media Services-fiók a progresszív letöltés videó hello kimenő adatfolyam egy StreamingEndpoint szolgáltatásból lehet. Minden Azure Media Services-fiók egy alapértelmezett StreamingEndpoint tartalmazza. További Streamvégpontok hello fiókkal hozhatók létre. Streamvégpontok, 1.0-s és 2.0 két verziója van. 10 2017. január verziótól kezdődően az újonnan létrehozott AMS számlákat tartalmazza 2.0-s verziójának **alapértelmezett** StreamingEndpoint. További végpontok hozzáadása toothis fiók streaming is 2.0-s verziójában. Ez a változás nem befolyásolja a meglévő fiókokat hello; meglévő Streamvégpontok 1.0-s verziója és frissített tooversion 2.0 lehet. A módosítás nem lesznek viselkedés, a számlázásra és a szolgáltatás módosításokat (további információkért lásd: hello **típusai és verziói** szakasz leírása az alábbiakban).

Ezenkívül a következő tulajdonságok toohello Streamvégponton entitás hello kezdve hello 2.15 verzió (2017. január jelent meg), Azure Media Services hozzáadott: **CdnProvider**, **CdnProfile**,  **FreeTrialEndTime**, **StreamingEndpointVersion**. Részletes megtudhatja, ezeket a tulajdonságokat, [ez](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

Amikor hoz létre egy Azure Media Services-fiók egy szabványos streamvégpont jön létre a hello alapértelmezett **leállítva** állapotát. Hello alapértelmezett streamvégpontból nem törölhető. Attól függően, hogy hello Azure CDN rendelkezésre állási megcélzott hello régióban, az újonnan létrehozott alapértelmezett alapértelmezés szerint adatfolyam-továbbítási végpontra is "StandardVerizon" CDN szolgáltató integráció. 

>[!NOTE]
>Azure CDN-integráció le kell tiltani adatfolyam-továbbítási végpontra hello megkezdése előtt.

Ez a témakör áttekintést hello fő funkciói adatfolyam-végpontok által biztosított.

## <a name="streaming-types-and-versions"></a>Adatfolyam-típusai és verziói

### <a name="standardpremium-types-version-20"></a>Standard vagy prémium típusok (verzió: 2.0-s)

A Media Services hello 2017. január kiadástól kezdve, még a két adatfolyam típusa: **szabványos** és **prémium**. Ezek a típusok hello Streaming endpoint 2.0-s verziójában "" részét képezik.

Típus|Leírás
---|---
**Standard**|Ez a lehetőség hello alapértelmezett hello forgatókönyvek hello többsége használható lenne.<br/>Ezzel a lehetőséggel rögzített/korlátozott SLA-t kap, első 15 nap elindítása után adatfolyam-továbbítási végpontra hello szabad.<br/>Ha egynél több adatfolyam-végpontot hoz létre, csak hello először egyik szabad hello az első 15 nap, hello mások számlázása, amint elindítani ezeket. <br/>Vegye figyelembe, hogy az ingyenes próbaverzió csak létrehozott media services-fiókok és az alapértelmezett streamvégpontból toonewly alkalmazza. Meglévő streamvégpontok és továbbá létrehozott streamvégpontok nem tartalmazza az ingyenes próbaverzió időszak még vannak frissítve tooversion 2.0-s vagy 2.0-s verzióját, hozza létre őket.
**Prémium**|Ez a beállítás akkor megfelelő szakmai nagyobb skálázási vagy vezérlő igénylő forgatókönyvek esetén.<br/>Változó SLA-t, amely prémium vásárolt streamelési egységet (SU) kapacitás alapul, a dedikált streamvégpontok élő elkülönített környezetben, és nem "versenyeznek" az erőforrásokat.

További részletekért lásd: hello **összehasonlítása adatfolyam-típusok** a következő szakaszban.

### <a name="classic-type-version-10"></a>Klasszikus típusú (1.0-s verzió)

AMS fiókok előzetes toohello 10 2017. január kiadás létrehozó felhasználók, meg kell adni egy **klasszikus** egy streamvégpont típusú. Ez a típus streaming endpoint verziója "1.0" hello részét képezi.

Ha a **verzió "1.0"** adatfolyam-továbbítási végpontra rendelkezik > = 1 premium adatfolyam-egységek (SU), prémium streamvégpont lesz, és minden AMS-szolgáltatásokat biztosít (hasonlóan hello **Standard vagy prémium** típusa) nélkül további konfigurációs lépéseket.

>[!NOTE]
>**Klasszikus** végpontok streaming ("1.0" és a 0 SU), korlátozott szolgáltatásokat biztosít, és nem tartalmaz egy SLA-t. Ajánlott toomigrate túl**szabványos** írja be a dinamikus becsomagolás vagy titkosítás és egyéb szolgáltatások hello jobb felhasználói élmény és toouse funkciók, például tooget **szabványos** típusa. toomigrate toohello **szabványos** írja be, lépjen a toohello [Azure-portálon](https://portal.azure.com/) válassza **Opt-in tooStandard**. Az áttelepítéssel kapcsolatos további információkért lásd: hello [áttelepítési](#migration-between-types) szakasz.
>
>Ügyeljen arra, hogy ez a művelet nem állítható vissza, és egy árképzési hatással van.
>
 
## <a name="comparing-streaming-types"></a>Adatfolyam-továbbítási típusok összehasonlítása

### <a name="versions"></a>Verziók

|Típus|StreamingEndpointVersion|ScaleUnits|Tartalomkézbesítési hálózat (CDN)|Számlázás|SLA| 
|--------------|----------|-----------------|-----------------|-----------------|-----------------|    
|Klasszikus|1.0|0|NA|Ingyenes|NA|
|Standard szintű streamvégpont|2.0|0|Igen|Díjköteles|Igen|
|Prémium szintű streamelési egységek|1.0|>0|Igen|Díjköteles|Igen|
|Prémium szintű streamelési egységek|2.0|>0|Igen|Díjköteles|Igen|

### <a name="features"></a>Szolgáltatások

Szolgáltatás|Standard|Prémium
---|---|---
Szabad első 15 nap| Igen |Nem
Teljesítmény |Másolatot too600 Mbps Azure CDN szolgáltatás használata nem szolgál. CDN bevonásával méretezhető.|200 MB / adatfolyam-egységek (SU). CDN bevonásával méretezhető.
SLA | 99.9|99,9 (200 MB/s / SU).
Tartalomkézbesítési hálózat (CDN)|Az Azure CDN, harmadik féltől származó CDN vagy nem CDN.|Az Azure CDN, harmadik féltől származó CDN vagy nem CDN.
Számlázási arányosítva van| Napi|Napi
A dinamikus titkosítás|Igen|Igen
Dinamikus csomagolás|Igen|Igen
Méretezés|Az automatikus méretezés során toohello céloz átviteli sebességet.|További streamelési egység
IP-szűrés/G20/egyéni állomás|Igen|Igen
Progresszív letöltés|Igen|Igen
Ajánlott kihasználtsága |Ajánlott hello többsége adatfolyam-forgatókönyveket.|Professional használat.<br/>Ha úgy gondolja, hogy előfordulhat, hogy túl Standard igényeinek. Kapcsolatfelvétel (a microsoft.com amsstreaming) Ha a párhuzamos célközönség mérete nagyobb, mint 50 000 megjelenítők várja.


## <a name="migration-between-types"></a>Áttelepítési típus között

A | túl| Műveletek
---|---|---
Klasszikus|Standard|Tooopt a szükséges
Klasszikus|Prémium| A skála (További adatfolyam-egységek)
Standard vagy Premium|Klasszikus|Nem érhető el (Ha a folyamatos átviteli végpont verziószáma 1.0. Engedélyezve van a scaleunits beállítás toochange tooclassic túl "0")
Standard (a/CDN nélkül)|Premium hello ugyanazokat a konfigurációkat|Hello engedélyezett **lépések** állapotát. (az Azure portálon)
Prémium (a/CDN nélkül)|Hello szabványosnak ugyanazokat a konfigurációkat|Hello engedélyezett **lépések** állapot (Azure-portálon)
Standard (a/CDN nélkül)|A különböző config Premium|Hello engedélyezett **leállt** (Azure-portálon) állapot. Hello futtatási állapota nem engedélyezett.
Prémium (a/CDN nélkül)|A különböző config standard|Hello engedélyezett **leállt** (Azure-portálon) állapot. Hello futtatási állapota nem engedélyezett.
SU az 1.0-s verzió > = 1 CDN|Standard vagy prémium nem CDN|Hello engedélyezett **leállt** állapotát. Nem engedélyezett a hello **lépések** állapotát.
SU az 1.0-s verzió > = 1 CDN|A/nélkül CDN standard|Hello engedélyezett **leállt** állapotát. Nem engedélyezett a hello **lépések** állapotát. CDN 1.0-s verziója lesz törölt és új egyik létrehozott, és elindult.
SU az 1.0-s verzió > = 1 CDN|A/nélkül CDN Premium|Hello engedélyezett **leállt** állapotát. Nem engedélyezett a hello **lépések** állapotát. Klasszikus CDN lesz törölve, és új egyik létrehozott, és elindult.

## <a name="next-steps"></a>Következő lépések
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

