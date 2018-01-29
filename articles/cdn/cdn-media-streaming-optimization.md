---
title: "Médiaadatfolyam-továbbítást az Azure Content Delivery Network keresztül optimalizálása"
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
ms.openlocfilehash: 02cd0fe30a2a14f42a16ed12f714d496bbb23b36
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="media-streaming-optimization-via-the-azure-content-delivery-network"></a>Médiaadatfolyam-továbbítást az Azure Content Delivery Network keresztül optimalizálása 
 
A nagy felbontású video egyre gyakoribbá válik az interneten, és a nagy fájlok kézbesítését nehézségek hoz létre. Az igény szerinti videó egyenletes lejátszható várt vagy a hálózatok és ügyfelek különböző video eszközök élő a világ számos országában dolgoznak. A médiaadatfolyam-továbbítást fájlok gyors és hatékony mechanizmus alapvető fontosságú élvezetesebbé és zökkenőmentes felhasználói élményt nyújtsanak.  

Élő adatfolyamokat nehézségekbe ütközhet a fájlmegosztásba a nagy méretű és száma párhuzamos megjelenítők miatt. Nagy késleltetéseket okoz, hogy a felhasználók. Élő adatfolyamok időben nem gyorsítótárazható, mert a nagy késleltetésű nem teszi a nézők számára elfogadható, videó töredék időben érkeznek. 

A kérelem mintákat keressen az adatfolyam-is nyújt néhány új kihívásokat. Amikor népszerű élő adatfolyam vagy új több megjelenik az igény szerinti videót, több ezer megjelenítők több millió kérhetnek az adatfolyam egy időben. Ebben az esetben intelligens kérelem összevonása nélkülözhetetlen annak nem ne terhelje tovább a származási kiszolgálók Ha az eszközök még nincsenek gyorsítótárazva.
 
Az Azure Content Delivery Network Akamai egy szolgáltatás, amely továbbítja a folyamatos átviteli adathordozó eszközök hatékony felhasználók léptékű világszerte kínál. A szolgáltatással csökken késések fordulnak elő, mivel csökkenti a terhelést a forrás-kiszolgálókon. A szolgáltatás az IP-címek Akamai Standard érhető el. 

Az Azure Content Delivery Network verizon médiafolyamot nyújt, közvetlenül az általános webes optimalizálási típusú.
 
## <a name="configure-an-endpoint-to-optimize-media-streaming-in-the-azure-content-delivery-network-from-akamai"></a>Optimalizálja a médiaadatfolyam-továbbítást az Azure Content Delivery Network Akamai végpont konfigurálása
 
Konfigurálhatja a tartalomkézbesítési hálózat (CDN) végpont nagy fájlok az Azure-portálon kézbesítési optimalizálása érdekében. Ehhez a REST API-k vagy az ügyfél SDK-k bármelyikét is használja. Az alábbi műveletek ismertetik a folyamatot az Azure-portálon:

1. Egy új végpont hozzáadásához a **CDN-profil** lapon jelölje be **végpont**.
  
    ![Új végpont](./media/cdn-media-streaming-optimization/01_Adding.png)

2. Az a **optimalizálva** legördülő listában válassza **videó igény szerinti médiaadatfolyam** videotartalom eszközök. Ha egy élő kombinációja és a videotartalom adatfolyamként történő továbbítását, válassza ki a **általános médiaadatfolyam**.

    ![Adatfolyam-kiválasztva](./media/cdn-media-streaming-optimization/02_Creating.png) 
 
Miután létrehozta a végpontot, a feltételeknek megfelelő összes fájl optimalizálása vonatkozik. Az alábbi szakasz ismerteti a folyamatot. 
 
## <a name="media-streaming-optimizations-for-the-azure-content-delivery-network-from-akamai"></a>Médiaadatfolyam-továbbítást az Azure Content Delivery Network Akamai optimalizálás.
 
Adatfolyam-továbbítási optimalizálási Akamai érvényben működés közbeni adathordozóról vagy adathordozót használó médiafolyamot videotartalom töredékei kézbesítését. Ez a folyamat nem azonos a progresszív letöltésen keresztül vagy a bájttartomány kérelmek használatával át egyetlen nagy eszköz. Információ a adott stílus media szállítási: [nagy méretű fájlok optimalizálási](cdn-large-file-optimization.md).


Az általános media kézbesítési és videotartalom media kézbesítési optimalizálási típusú CDN használata gyorsabb képes biztosítani a media eszközök a háttér-optimalizálást. Megtudta, időbeli ajánlott eljárásai alapján adathordozó eszközök használata konfigurációk is.

### <a name="caching"></a>Gyorsítótárazás

Ha az Azure Content Delivery Network Akamai azt észleli, hogy az eszköz egy adatfolyam-továbbítási jegyzékfájl vagy töredék, általános webes kézbesítési eltérő gyorsítótár lejárati időpontban használ. (Lásd az alábbi táblázatban a teljes listát.) Ennek mindig a cache-control vagy Expires fejléc a forrásnak küldött is figyelembe véve. Ha az eszköz nem egy adathordozó eszköz, a lejárati időpontjait használatával általános webes kézbesítésre gyorsítótárazza.

A rövid negatív gyorsítótárazási idő az eredeti kiszervezési hasznos, sok felhasználó kérése egy kódrészletet, amely még nem létezik. Példa: Ha a csomagok nem érhetők el a forrásból, hogy a második élő adatfolyam. A gyorsítótárazási már intervallum segít kiszervezése a forrás érkező kéréseket, mert videotartalom általában nem módosította.
 

|   | Általános webes kézbesítés | Általános médiaadatfolyam-továbbítást | Videotartalom médiaadatfolyam-továbbítást  
--- | --- | --- | ---
Gyorsítótárazás: pozitív <br> 200-AS, 203, 300, HTTP <br> 301, 302, és 410 | 7 nap |365 nap | 365 nap   
Gyorsítótárazás: negatív. <br> HTTP 204, 305, 404, <br> és 405 | None | 1 másodperc | 1 másodperc
 
### <a name="deal-with-origin-failure"></a>Az eredeti hiba kezelésére  

Általános media kézbesítési és videotartalom media kézbesítési is származási időtúllépési és ajánlott eljárások a tipikus kérelem minták alapján újrapróbálkozási naplót. Például mert általános media kézbesítési a működés közbeni és videotartalom media kézbesítési, használ egy rövidebb kapcsolat időkorlátja időérzékeny jellemzői miatt live streaming.

Ha a kapcsolat időtúllépés miatt megszakadt, a CDN újrapróbálja többször az ügyfél egy "504 - átjáró időtúllépése" hibaüzenet küldése előtt. 

Ha egy fájl megfelel a fájl típusát és méretét feltételek listája, a CDN viselkedésének médiaadatfolyam használja. Ellenkező esetben általános webes kézbesítési használ.
   
### <a name="conditions-for-media-streaming-optimization"></a>Médiaadatfolyam-továbbítást optimalizálási feltételei 

Az alábbi táblázat a médiaadatfolyam-továbbítást optimalizálási teljesítését feltételek csoportja: 
 
Támogatott adatfolyam-továbbítási típusok | Fájlkiterjesztések  
--- | ---  
Apple HLS | m3u8, m3u, m3ub, kulcsot ts, aac
Az Adobe HDS | f4m, f4x, drmmeta, a rendszerindítás, f4f,<br>Seg-illetheti URL-cím szerkezete <br> (reguláris kifejezéssel egyező: ^(/.*)Seq(\d+)-Frag(\d+)
KÖTŐJEL | mpd, kötőjelet, divx, ismv, m4s, m4v, mp4, mp4v, <br> sidx, webm, mp4a, m4a, isma
Smooth streaming | / jegyzékfájl /, töredék/QualityLevels / /
  

 
## <a name="media-streaming-optimizations-for-the-azure-content-delivery-network-from-verizon"></a>Médiaadatfolyam-továbbítást az Azure Content Delivery Network verizon optimalizálás.

Az Azure Content Delivery Network verizon adatfolyam adathordozó eszközök általános webes optimalizálási típusú segítségével közvetlenül továbbítja. A CDN néhány funkcióinak közvetlenül segítse a postai adathordozó eszközök alapértelmezés szerint.

### <a name="partial-cache-sharing"></a>A részleges gyorsítótári megosztása

A részleges gyorsítótári megosztása lehetővé teszi, hogy a CDN és a részlegesen gyorsítótárazott tartalmat rendszerben tett új kérelmekre. Például ha az első kérelem és a CDN egy gyorsítótár-tévesztései eredményez, a kérelem érkezik a forrás. Bár a nem teljes tartalom a CDN-gyorsítótárból tölti be, küldött egyéb kérések számára a CDN megkezdheti az adatok beolvasása. 

### <a name="cache-fill-wait-time"></a>Gyorsítótár kitöltés várakozási idő

 A gyorsítótár kitöltés várakozási idő a szolgáltatás kényszeríti a peremhálózati kiszolgáló erőforrást a következő kéréseit tárolásához, amíg az eredeti kiszolgálóra érkező HTTP-válaszfejlécek. Ha HTTP-válaszfejlécek a forrásból érkeznek, az időzítő lejárata előtt, a növekvő gyorsítótárból szolgáltatott összes kérelem volt helyezhető tartásba. Egy időben a gyorsítótár tölti ki a forrás adatait. Alapértelmezés szerint a gyorsítótár kitöltés várakozási idő értéke 3000 ezredmásodperc. 

