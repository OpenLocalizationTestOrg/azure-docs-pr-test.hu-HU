---
title: "A gyorsítótárazás működése |} Microsoft Docs"
description: "Gyorsítótárazás adatok helyi tárolása, hogy a jövőben kéri az adatok elérése gyorsabban során a rendszer."
services: cdn
documentationcenter: 
author: dksimpson
manager: 
editor: 
ms.assetid: 
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/23/2017
ms.author: v-deasim
ms.openlocfilehash: 284b4bcbeafc422a2ed91cec00a5b5b83bb37b7b
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/25/2018
---
# <a name="how-caching-works"></a>A gyorsítótárazás működése

Ez a cikk általános gyorsítótárazási fogalmak áttekintése és hogyan [Azure Content Delivery Network (CDN)](cdn-overview.md) gyorsítótárazást használ a teljesítmény javítása érdekében. Ha szeretne további információk a CDN-végpontot a gyorsítótárazási viselkedés testreszabásához tudnivalókat [vezérlő Azure CDN szolgáltatás használata a szabályok gyorsítótárazással gyorsítótárazásának](cdn-caching-rules.md) és [vezérlő Azure CDN a lekérdezési karakterláncok gyorsítótárazásának](cdn-query-string.md).

## <a name="introduction-to-caching"></a>A gyorsítótárazás bemutatása

Gyorsítótárazás adatok helyi tárolása, hogy a jövőben kéri az adatok elérése gyorsabban során a rendszer. A leggyakoribb típus gyorsítótárazását, webes böngésző gyorsítótár, a webes böngésző statikus adatok másolatát helyben tárol a helyi merevlemez-meghajtón. Gyorsítótárazás használatával a webböngésző elkerüli a kiszolgálóval végzett több adatváltásra a kiszolgálóra, és ehelyett ugyanazon adatokat érik el helyben, így mentése időt és erőforrásokat. Gyorsítótárazás jól alkalmazható helyileg kezeléséhez kicsi, statikus adatok, például a statikus képek, CSS fájlok és JavaScript-fájlt.

Hasonlóképpen gyorsítótárazás használják egy tartalomkézbesítési hálózatra, a peremhálózati kiszolgálóinak megközelíti a felhasználói kérelmek vissza az eredeti utazik, és csökkenti a késéseket végfelhasználói elkerülése érdekében. Egy webes böngésző gyorsítótárat, amely csak egy-egy felhasználóhoz, ellentétben a CDN megosztott gyorsítótárával rendelkezik. A megosztott CDN-gyorsítótár a fájl egy felhasználó által kért elérhető később más felhasználók által amely jelentősen csökkenti az eredeti kiszolgálóra kérelmek számát jelenti.

Dinamikus erőforrásokat, amelyek gyakran módosítják, vagy egy adott felhasználóhoz egyedi nem gyorsítótárazható. Ilyen típusú erőforrások, azonban kihasználhatják dinamikus acceleration (DSA) optimalizálási az Azure Content Delivery Network a teljesítménnyel kapcsolatos fejlesztések a.

Gyorsítótárazás akkor fordulhat elő, több szinten, a forráskiszolgáló és a végfelhasználók között:

- Webkiszolgáló: használ a megosztott gyorsítótárával (több felhasználó).
- Tartalomkézbesítési hálózat: használ a megosztott gyorsítótárával (több felhasználó).
- Internetszolgáltató (ISP): (a több felhasználó) egy megosztott gyorsítótárát használja.
- Webböngésző: használ a saját gyorsítótár (egy felhasználó).

Minden egyes gyorsítótár általában kezeli a saját erőforrás frissesség és érvényesítést végez, amikor egy fájl elavult. Ez a viselkedés definiálva van a HTTP-specifikáció, gyorsítótárazás [RFC 7234](https://tools.ietf.org/html/rfc7234).

### <a name="resource-freshness"></a>Erőforrás frissesség

A gyorsítótárazott erőforrás elavult, vagy elavult (mint a forrás kiszolgálón a megfelelő erőforrás) is lehet, mert fontos bármely gyorsítótárazást szabályozhatja a tartalom frissítésekor. Idő és a sávszélesség-felhasználás mentéséhez egy gyorsítótárazott erőforrás nem összeveti a verziót a forrás kiszolgálón minden alkalommal, amikor érhető el. Ehelyett mindaddig, amíg egy gyorsítótárazott erőforrás friss tekinthető, feltételezett, hogy a legújabb verziójú és közvetlenül az ügyfélnek küldött. A gyorsítótárazott erőforrás friss minősül, ha korát kisebb, mint a kora vagy egy gyorsítótár-beállításban megadott időtartam. Például ha egy böngészőben egy weblapot Újratölti, azt ellenőrzi, hogy a merevlemez-meghajtóról minden gyorsítótárazott erőforrás friss és betölti azt. Ha az erőforrás nem friss (elavult), egy friss másolatot be van töltve a kiszolgálóról.

### <a name="validation"></a>Ellenőrzés

Ha egy erőforrás elavult tekinthető, az eredeti kiszolgálóra kapcsolatba kell érvényesíti, ez azt jelenti, határozza meg, hogy a gyorsítótárban lévő adatok továbbra is megegyezik az eredeti kiszolgálón. Ha a fájl módosult a forrás kiszolgálón, a gyorsítótár frissíti a verziót az erőforrás. Ellenkező esetben ha az erőforrás friss, a kerülnek az adatok közvetlenül a gyorsítótárból érvényességének ellenőrzése először nélkül.

### <a name="cdn-caching"></a>CDN-gyorsítótárazás

Gyorsítótárazás szerves ahhoz, ahogy a CDN kézbesítési felgyorsítása és statikus erőforrásokat, például a képek, betűtípusok és videók származási betöltésének működik. A CDN gyorsítótárazás, statikus erőforrások stratégiailag kiszolgálók, amelyek a felhasználó több helyi szelektív módon tárolják, és az alábbi előnyöket nyújtja:

- Mivel a webes forgalom (a példában, képek, betűtípusok és videók) statikus, CDN gyorsítótárazását csökkenti a hálózati késést mozgatásával tartalom közelebb a felhasználónak, így csökkentve az adatok választ távolságot.

- Történő kiszervezésével a munkahelyi és a CDN gyorsítótárazás csökkentheti a hálózati forgalom és a forrás kiszolgálót érő terhelést. Így csökkenti az alkalmazás költség- és erőforrás követelményei, akkor is, ha nagyszámú felhasználó.

Hogyan gyorsítótárazás van megvalósítva hasonló webböngészőben, szabályozhatja hogyan gyorsítótárazását végzi el a CDN a gyorsítótár-irányelv fejlécek küldését. Gyorsítótár-irányelv fejlécek olyan HTTP-fejlécek, amely az eredeti kiszolgálóra általában adhatók hozzá. Bár ezek a fejlécek a legtöbb eredetileg is cím gyorsítótárazása az ügyfélböngészők, most is használják az összes köztes gyorsítótárak, például a tartalomtovábbító. 

Két fejléc segítségével határozza meg a gyorsítótár frissesség: `Cache-Control` és `Expires`. `Cache-Control`több naprakész, és elsőbbséget élvez `Expires`, ha teljesül. Is két típusa van (más néven érvényesség-ellenőrzők) érvényesítéséhez használt fejlécek: `ETag` és `Last-Modified`. `ETag`több naprakész, és elsőbbséget élvez `Last-Modified`, ha mindkét vannak meghatározva.  

## <a name="cache-directive-headers"></a>Gyorsítótár-irányelv fejlécek

> [!IMPORTANT]
> Alapértelmezés szerint az Azure CDN-végpont DSA optimalizált figyelmen kívül hagyja a gyorsítótár-irányelv fejlécek és megkerüli a gyorsítótárazást. Beállíthatja, hogyan Azure CDN-végpont értékként kezelje-e ezek a fejlécek gyorsítótárazása szabályok gyorsítótárazás CDN használatával. További információkért lásd: [vezérlő Azure CDN szolgáltatás használata a szabályok gyorsítótárazással gyorsítótárazásának](cdn-caching-rules.md).

Az Azure CDN támogatja a következő HTTP-gyorsítótár-irányelv fejlécek, gyorsítótárazás időtartama és a gyorsítótár megosztása meghatározó: 

`Cache-Control`
- A HTTP 1.1 webes közzétevők tartalmuk teljesebb körű vezérlése és vonatkozó korlátozások elhárítása bevezetett a `Expires` fejléc.
- Felülbírálja a `Expires` fejléc, ha mindkét azt és `Cache-Control` vannak definiálva.
- A kérelem fejlécében használatakor `Cache-Control` figyelmen kívül hagyják Azure CDN alapértelmezés szerint.
- Ha egy válaszfejléc használják, Azure CDN támogatja-e a következő `Cache-Control` irányelveknek megfelelően termék: 
   - **Verizon Azure CDN**: összes támogatja `Cache-Control` irányelvek. 
   - **Akamai Azure CDN**: csak a következő támogatja `Cache-Control` irányelvek; minden más a rendszer nem veszi figyelembe: 
      - `max-age`: A gyorsítótár másodpercben megadott tud tárolni a tartalmat. Például: `Cache-Control: max-age=5`. Ez a direktíva meghatározza a maximális időt a tartalom friss tekinthető.
      - `no-cache`: A tartalom gyorsítótárazása, de a tartalom ellenőrzésének, minden alkalommal, mielőtt továbbítása a gyorsítótárból. Egyenértékű `Cache-Control: max-age=0`.
      - `no-store`: Soha ne gyorsítótár tartalma. Távolítsa el a tartalmat, ha korábban már tárolták.

`Expires`
- A HTTP 1.0; bevezetett örökölt fejléc támogatott a visszamenőleges kompatibilitás.
- A dátum-alapú időkorlátját pontossága protokollt használja. 
- Hasonló `Cache-Control: max-age`.
- Ha `Cache-Control` nem létezik.

`Pragma`
   - Nem fogadja el a Azure CDN alapértelmezés szerint.
   - A HTTP 1.0; bevezetett örökölt fejléc támogatott a visszamenőleges kompatibilitás.
   - Használja a következő irányelvnek ügyfél fejléc: `no-cache`. Ez a direktíva arra utasítja a kiszolgálót az erőforrás egy frissen telepített verzióját.
   - `Pragma: no-cache`egyenértékű `Cache-Control: no-cache`.

## <a name="validators"></a>Érvényesség-ellenőrzők

Ha a gyorsítótár elavult, HTTP-gyorsítótár érvényesítők segítségével a gyorsítótárazott egy fájl és összehasonlítása a verziót a forrás-kiszolgálón. **Verizon Azure CDN** alapértelmezés szerint támogatja az ETag és Last-Modified érvényesség-ellenőrzők közben **Akamai Azure CDN** csak Last-Modified alapértelmezés szerint támogatja.

`ETag`
- **Verizon Azure CDN** használ `ETag` során alapértelmezés szerint **Akamai Azure CDN** viszont nem.
- `ETag`karakterlánc, amely egyedi a minden fájl- és a fájl verziója határozza meg. Például: `ETag: "17f0ddd99ed5bbe4edffdd6496d7131f"`.
- A HTTP 1.1-ben bevezetett, és több mint aktuális `Last-Modified`. Akkor hasznos, ha az utolsó módosítási dátumának nehéz lenne meghatározni.
- Támogatja az erős érvényesítése és a gyenge érvényesítési; Azure CDN azonban csak az erős érvényesítési támogatja. Erős ellenőrzése, a két erőforrás felelősséget kell bájt a byte azonos. 
- A gyorsítótár ellenőrzi a fájl által használt `ETag` küldésével egy `If-None-Match` egy vagy több fejlécet `ETag` érvényesség-ellenőrzők a kérelemben. Például: `If-None-Match: "17f0ddd99ed5bbe4edffdd6496d7131f"`. Ha a kiszolgáló verziója megegyezik egy `ETag` érvényesítő szerepel a listán, elküldi állapotkódot (nem módosított) 304 válaszában. Ha másik verziószáma, a kiszolgáló válaszol állapotkód 200 (OK) és a frissített erőforrása.

`Last-Modified`
- A **csak Verizon Azure CDN**Last-Modified, akkor használatos, ha az ETag nem része a HTTP-válasz. 
- A dátum és idő, a forráskiszolgáló által meghatározott, az erőforrás utolsó módosításának határozza meg. Például: `Last-Modified: Thu, 19 Oct 2017 09:28:00 GMT`.
- A gyorsítótár ellenőrzi, hogy egy fájl segítségével `Last-Modified` küldésével egy `If-Modified-Since` fejléc a dátumot és időpontot a kérelemben. A forráskiszolgáló hasonlítja össze, hogy a dátum a `Last-Modified` a legújabb erőforrás fejléc. Ha az erőforrás a megadott idő óta nem módosult, a kiszolgáló válaszában visszaküldi állapotkód 304 (nem módosított). Ha az erőforrás módosítva lett, a kiszolgáló visszaadja-e az állapot kód 200 (OK) és a frissített erőforrása.

## <a name="determining-which-files-can-be-cached"></a>Mely fájlok gyorsítótárazható meghatározása

Nem minden erőforrások gyorsítótárazható. Az alábbi táblázat bemutatja, milyen erőforrásokat gyorsítótárazható, HTTP-válaszok típusa alapján. Erőforrások fel a HTTP-válaszok, amelyek nem felelnek meg ezek a feltételek nem gyorsítótárazható. A **Verizon Premium Azure CDN** csak, használhatja a szabálymotor testreszabásához néhány ezeket a feltételeket.

|                   | Verizon Azure CDN | Akamai Azure CDN            |
|------------------ |------------------------|----------------------------------|
| HTTP-állapotkódok | 200                    | 200-as, 203, 300, 301, 302 és 401-es |
| HTTP-metódus       | GET                    | GET                              |
| Fájlméret         | 300 GB                 | -Általános webes kézbesítési optimalizálási: 1,8 GB<br />-Médiaadatfolyam-továbbítást optimalizálásokat: 1,8 GB<br />-Nagy méretű fájlok optimalizálási: 150 GB |

## <a name="default-caching-behavior"></a>Alapértelmezett gyorsítótárazási viselkedés

A következő táblázat ismerteti az alapértelmezett gyorsítótárazásának az Azure CDN-termékek és az optimalizálás.

|                    | Verizon - általános webes kézbesítés | Verizon – DSA | Akamai - általános webes kézbesítés | Akamai - DSA | Akamai - nagy méretű fájl letöltése | Akamai - általános vagy VOD médiaadatfolyam |
|--------------------|--------|------|-----|----|-----|-----|
| **Tiszteletben forrása**   | Igen    | Nem   | Igen | Nem | Igen | Igen |
| **CDN-gyorsítótárazás időtartama** | 7 nap | Nincs | 7 nap | Nincs | 1 nap | 1 év |

**Forrás tiszteletben**: Megadja, hogy elfogadja a [gyorsítótár-irányelv fejlécek támogatott](#http-cache-directive-headers) Ha léteznek az a HTTP-válasz a forráskiszolgálóról.

**CDN-gyorsítótárazás időtartama**: meghatározza, hogy mennyi ideig erőforrás gyorsítótárazza, amelyeket az Azure CDN. Azonban ha **származási tiszteletben** kérdésre Igen, és a forráskiszolgálóról a HTTP-válasz tartalmazza a gyorsítótár-irányelv fejléc `Expires` vagy `Cache-Control: max-age`, Azure CDN időtartama értéket a fejléc meg helyette használja. 

## <a name="next-steps"></a>További lépések

- Bírálja felül az alapértelmezett szabályok gyorsítótárazással a CDN a gyorsítótárazásának és testreszabásáról további tudnivalókért lásd: [vezérlő Azure CDN szolgáltatás használata a szabályok gyorsítótárazással gyorsítótárazásának](cdn-caching-rules.md). 
- Lekérdezési karakterláncok gyorsítótárazásának működése vezérlőhöz használatával kapcsolatban lásd: [vezérlő Azure CDN a lekérdezési karakterláncok gyorsítótárazásának](cdn-query-string.md).



