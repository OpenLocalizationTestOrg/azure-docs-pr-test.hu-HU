---
title: "adatfolyam-optimalizálás keresztül hello Azure Content Delivery Network aaaMedia"
description: "Adatfolyam-továbbítási médiafájlok zökkenőmentes kézbesítésre optimalizálása"
services: cdn
documentationcenter: 
author: smcevoy
manager: erikre
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: v-semcev
ms.openlocfilehash: a05a86204708c7ea7ef1f9be04323cdda6a2d403
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="media-streaming-optimization-via-hello-azure-content-delivery-network"></a>Médiaadatfolyam-továbbítást keresztül hello Azure Content Delivery Network optimalizálása 
 
A nagy felbontású video egyre gyakoribbá válik az internethez, ami hoz létre a nagy fájlok kézbesítését nehézségek hello. Az igény szerinti videó egyenletes lejátszható várt vagy a hálózatok és ügyfelek különböző video eszközök élő hello világ számos országában dolgoznak. A médiaadatfolyam-továbbítást fájlok gyors és hatékony mechanizmus kritikus tooensure egy élvezetesebbé és zökkenőmentes felhasználói élmény.  

Élő adatfolyamokat különösen nehezen toodeliver hello nagy méretű és száma párhuzamos megjelenítők miatt. Nagy késleltetéseket okoz a felhasználók tooleave. Élő adatfolyamok időben nem gyorsítótárazható, és nagy késleltetésű nem elfogadható tooviewers, mert videó töredék időben érkeznek. 

hello kérelem mintákat keressen az adatfolyam-is nyújt néhány új kihívásokat. Ha népszerű élő adatfolyam vagy új több felszabadul az igény szerinti, akár több ezer videót a megjelenítők toomillions kérhetnek hello adatfolyam: hello ugyanannyi időt vesz igénybe. Ebben az esetben intelligens kérelmeket összevonása létfontosságú toonot ne terhelje tovább hello származási kiszolgálók Ha hello eszközök még nincsenek gyorsítótárazva.
 
hello Azure Content Delivery Network Akamai kínál: a szolgáltatás letölti a folyamatos átviteli adathordozó eszközök hatékony toousers hello földgömb léptékű között. hello szolgáltatás csökkenti késések fordulnak elő, mert hello származási kiszolgálók hello terhelése csökkenti. Ez a szolgáltatás hello Standard Akamai tarifacsomag érhető el. 

hello Azure Content Delivery Network verizon médiafolyamot nyújt, közvetlenül a hello általános webes kézbesítési optimalizálási típusa.
 
## <a name="configure-an-endpoint-toooptimize-media-streaming-in-hello-azure-content-delivery-network-from-akamai"></a>Egy végpont toooptimize médiaadatfolyam-továbbítást az Azure Content Delivery Network Akamai hello konfigurálása
 
Konfigurálhatja a tartalomkézbesítési hálózat (CDN) végpont toooptimize kézbesítési hello Azure-portálon keresztül nagy fájlok esetében. Használhatja a REST API-kat is, vagy bármely ez hello ügyfél SDK-k toodo. hello következő lépések bemutatják hello folyamat hello Azure-portálon keresztül:

1. egy új végponton, hello tooadd **CDN-profil** lapon jelölje be **végpont**.
  
    ![Új végpont](./media/cdn-media-streaming-optimization/01_Adding.png)

2. A hello **optimalizálva** legördülő listában válassza **videó igény szerinti médiaadatfolyam** videotartalom eszközök. Ha egy élő kombinációja és a videotartalom adatfolyamként történő továbbítását, válassza ki a **általános médiaadatfolyam**.

    ![Adatfolyam-kiválasztva](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Hello végpont létrehozása után hello optimalizálási feltételeknek megfelelő összes fájl vonatkozik. a következő szakasz hello ezt a folyamatot ismerteti. 
 
## <a name="media-streaming-optimizations-for-hello-azure-content-delivery-network-from-akamai"></a>Médiaadatfolyam-továbbítást az Azure Content Delivery Network Akamai hello optimalizálása
 
Adatfolyam-továbbítási optimalizálási Akamai érvényben működés közbeni adathordozóról vagy adathordozót használó médiafolyamot videotartalom töredékei kézbesítését. Ez a folyamat nem azonos a progresszív letöltésen keresztül vagy a bájttartomány kérelmek használatával át egyetlen nagy eszköz. Információ a adott stílus media szállítási: [nagy méretű fájlok optimalizálási](cdn-large-file-optimization.md).


hello általános media kézbesítési vagy videotartalom media kézbesítési optimalizálási típusok használata CDN háttér-optimalizálást toodeliver adathordozó eszközök gyorsabb. Megtudta, időbeli ajánlott eljárásai alapján adathordozó eszközök használata konfigurációk is.

### <a name="caching"></a>Gyorsítótárazás

Hello Azure Content Delivery Network Akamai azt észleli, hogy hello eszköz egy adatfolyam-továbbítási jegyzékfájl vagy töredék, ha különböző gyorsítótár lejárati időpontjait a általános webes kézbesítési használ. (Lásd: hello teljes listáját a következő táblázat hello.) Ennek mindig a cache-control vagy Expires fejléc küldi hello forrásból is figyelembe véve. Ha nem a media eszköz hello eszköz, általános webes kézbesítésre hello lejárati időpontjait használatával gyorsítótárazza.

hello rövid negatív gyorsítótárazási, hasznos, ha a forrás kiszervezési amikor sok felhasználók kérnek egy kódrészletet, amely még nem létezik. Példa: Ha hello csomagok nem érhetők el hello a forrásból, hogy a második élő adatfolyam. hello gyorsítótárazási már intervallum is lehetővé teszi, mert videotartalom általában nem módosította-kiszervezés hello származási érkező kérelmeket.
 

|    | Általános kérdések<br> webalkalmazás<br>teljesítéssel | Általános kérdések<br> média<br> adatfolyam- | Videotartalom <br>média<br> adatfolyam-  
--- | --- | --- | ---
Gyorsítótárazás: pozitív <br> 200-AS, 203, 300, HTTP <br> 301, 302, és 410 | 7 nap |365 nap | 365 nap   
Gyorsítótárazás: negatív. <br> HTTP 204, 305, 404, <br> és 405 | None | 1 másodperc | 1 másodperc
 
### <a name="deal-with-origin-failure"></a>Az eredeti hiba kezelésére  

Általános media kézbesítési és videotartalom media kézbesítési is származási időtúllépési és ajánlott eljárások a tipikus kérelem minták alapján újrapróbálkozási naplót. Például mert általános media kézbesítési a működés közbeni és videotartalom media kézbesítési, használ egy rövidebb kapcsolat időtúllépés miatt toohello időérzékeny jellege live streaming.

Ha a kapcsolat időtúllépés miatt megszakadt, hello CDN újrapróbálja többször "504 - átjáró időtúllépése" hiba toohello ügyfél küldése előtt. 

Ha egy fájl megfelel a hello fájlok típusát és méretét feltételek listája, hello CDN hello viselkedés médiaadatfolyam-továbbítást használ. Ellenkező esetben általános webes kézbesítési használ.
   
### <a name="conditions-for-media-streaming-optimization"></a>Médiaadatfolyam-továbbítást optimalizálási feltételei 

a következő táblázat hello feltételek toobe elégedett a médiaadatfolyam-továbbítást optimalizálási hello készletét tartalmazza: 
 
Támogatott adatfolyam-továbbítási típusok | Fájlkiterjesztések  
--- | ---  
Apple HLS | m3u8, m3u, m3ub, kulcsot ts, aac
Az Adobe HDS | f4m, f4x, drmmeta, a rendszerindítás, f4f,<br>Seg-illetheti URL-cím szerkezete <br> (reguláris kifejezéssel egyező: ^(/.*)Seq(\d+)-Frag(\d+)
KÖTŐJEL | mpd, kötőjelet, divx, ismv, m4s, m4v, mp4, mp4v, <br> sidx, webm, mp4a, m4a, isma
Smooth streaming | / jegyzékfájl /, töredék/QualityLevels / /
  

 
## <a name="media-streaming-optimizations-for-hello-azure-content-delivery-network-from-verizon"></a>Médiaadatfolyam-továbbítást az Azure Content Delivery Network verizon hello optimalizálása

hello Azure Content Delivery Network verizon adatfolyam adathordozó eszközök hello általános webes optimalizálási típusú segítségével közvetlenül továbbítja. Néhány funkcióinak hello CDN közvetlenül segítse a postai adathordozó eszközök alapértelmezés szerint.

### <a name="partial-cache-sharing"></a>A részleges gyorsítótári megosztása

A részleges gyorsítótári megosztása lehetővé teszi a hello CDN tooserve részlegesen gyorsítótárazott tartalom toonew kérelmek. Például ha hello első kérelem toohello CDN gyorsítótár-tévesztései eredményez, hello kérelem küld toohello forrása. Bár a nem teljes tartalom hello CDN gyorsítótár tölti be, más kérelmek toohello CDN megkezdheti az adatok beolvasása. 

### <a name="cache-fill-wait-time"></a>Gyorsítótár kitöltés várakozási idő

 hello gyorsítótár kitöltés várakozási idő a szolgáltatás kényszeríti hello peremhálózati kiszolgáló toohold a későbbi kéréseit hello azonos erőforrás csak fejlécek hello eredeti kiszolgálóra érkező HTTP-válasz. HTTP-válaszfejlécek hello forrásból érkezik hello időzítő lejárata előtt, ha volt függeszthetők összes kérelem szolgáltatott gyorsítótár növekvő hello kívül. A hello azonos időben, hello gyorsítótár tölti ki adatokat hello a forrásból. Alapértelmezés szerint a hello gyorsítótár kitöltés várakozási idő too3, 000 ezredmásodperc van beállítva. 

