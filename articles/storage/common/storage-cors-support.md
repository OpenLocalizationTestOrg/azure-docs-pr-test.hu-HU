---
title: "aaaCross eredetű erőforrás-megosztás (CORS) támogatást |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable CORS-támogatás a Microsoft Azure Storage szolgáltatásainak hello."
services: storage
documentationcenter: .net
author: cbrooksmsft
manager: carmonm
editor: tysonn
ms.assetid: a0229595-5b64-4898-b8d6-fa2625ea6887
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 2/22/2017
ms.author: cbrooks
ms.openlocfilehash: 0a6ec3bf6999c5faa7f0912dc2a47921aa01d3d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cross-origin-resource-sharing-cors-support-for-hello-azure-storage-services"></a>Hello Azure Storage szolgáltatásainak Cross-Origin Resource Sharing (CORS) támogatása
2013-08-15 verziójával kezdve hello Azure storage szolgáltatások támogatja az eltérő eredetű erőforrások megosztása (CORS) hello Blob, Table, várólista és fájl számára. A CORS egy HTTP-szolgáltatás, amely lehetővé teszi egy webalkalmazást az egyik tartomány tooaccess erőforrásainak fut egy másik tartományban. Webböngészők néven ismert biztonsági korlátozások megvalósítása [azonos eredetű házirend](http://www.w3.org/Security/wiki/Same_Origin_Policy) , amely megakadályozza, hogy egy másik tartományban; hívási API-weblap A CORS biztosít egy biztonságos módon tooallow tartománya (hello forrástartományt) toocall API-k egy másik tartományban. Lásd: hello [CORS specification](http://www.w3.org/TR/cors/) CORS leírását.

A CORS szabályainak beállítása külön-külön az egyes hello tárolási szolgáltatások meghívásával [Blob szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452235.aspx), [várólista-tulajdonságok beállítása](https://msdn.microsoft.com/library/hh452232.aspx), és [Table szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452240.aspx). Ha elvégezte a hello CORS-szabályokat hello szolgáltatást, majd más tartományokból hello szolgáltatásra szóló megfelelően hitelesített kérelem lesz kiértékelt toodetermine e toohello szabályokkal, amelyeket megfelelően engedélyezve van.

> [!NOTE]
> Vegye figyelembe, hogy a CORS nem olyan hitelesítési módszert. Kérését a tároló egyik erőforrásához szemben ha engedélyezve van a CORS rendelkeznie kell a megfelelő hitelesítési aláírás, vagy egy nyilvános erőforrás elleni kell tenni.
> 
> 

## <a name="understanding-cors-requests"></a>A CORS kérelmek ismertetése
A CORS kérelem származási tartományból két külön kérelmet állhat:

* A elővizsgálati kérelmet, amely lekérdezi a hello CORS korlátozásai hello szolgáltatást. hello ellenőrzési kérés szükség, ha hello kérelem metódus egy [egyszerű módszer](http://www.w3.org/TR/cors/), ami azt jelenti, GET, HEAD vagy POST.
* a keresett erőforrás hello tényleges kérelem hello ellen.

### <a name="preflight-request"></a>Ellenőrzési kérés
hello ellenőrzési kérés lekérdezések hello CORS korlátozások hello fiók tulajdonosának hello társzolgáltatás létrehoztak. webböngésző hello (vagy egyéb felhasználói ügynök) küld egy beállítások hello kérelemfejléc, tartalmazó metódus és a forrás tartományát. hello társzolgáltatás szánt hello művelet CORS-szabályokat, amelyek adja meg, melyik eredettartományból, módszerek kérelem előre konfigurált készlete alapján kiértékeli, és kérelemfejléc adható meg a tároló egyik erőforrásához egy tényleges kérelmet.

Ha CORS hello szolgáltatás engedélyezve van, és a CORS szabályt, amely megegyezik a hello ellenőrzési kérés, hello szolgáltatás válaszol, állapotkód: 200 (OK), és hello szükséges hozzáférési fejlécek hello válaszként.

Ha CORS hello szolgáltatás számára nem engedélyezett, vagy nincs CORS szabály megegyezik a hello ellenőrzési kérés, hello szolgáltatás válaszol, állapotkód: 403 (tiltott).

Ha hello beállítások kérelem nem tartalmazza a szükséges CORS fejlécek (hello származási és hozzáférés-vezérlési-kérelem-metódus fejlécekkel együtt) hello, hello szolgáltatás válaszol, állapotkód: 400 (hibás kérés).

Vegye figyelembe, hogy az ellenőrzési kérés ki lesz értékelve hello szolgáltatás (Blob, Queue és Table) és hello elleni nem kért erőforrás. hello fiók tulajdonosának kell engedélyezte a CORS szolgáltatást fióktulajdonságok hello ahhoz, hogy hello kérelem toosucceed részeként.

### <a name="actual-request"></a>Tényleges kérelem
Miután hello elővizsgálati kérelem elfogadása és hello választ ad vissza, hello böngésző csatolva az hello hello tárolási erőforrások tényleges kérelmet. hello böngésző megtagadja hello tényleges kérelem azonnal Ha hello elővizsgálati vonatkozó kérés elutasítva.

hello tényleges kérelem hello társzolgáltatás normál kérelmet a rendszer. hello származási fejléc hello jelenléte azt jelzi, hogy hello kérelme, mert a CORS kérelem hello szolgáltatás ellenőrzi a megfeleltetési szabályokról a CORS hello. Ha van egyezés, hello hozzáférési fejlécek felvett toohello választ, és hátsó toohello ügyfél küldött. Nem található egyezés, ha hello CORS hozzáférési fejlécek nem lehet megjeleníteni.

## <a name="enabling-cors-for-hello-azure-storage-services"></a>A CORS engedélyezése hello Azure Storage szolgáltatás
CORS-szabályokat úgy van beállítva, hello szolgáltatás szintjén kell tooenable, vagy tiltsa le a CORS az egyes szolgáltatásokhoz (Blob, várólista és tábla) külön-külön. A CORS minden egyes szolgáltatás alapértelmezés szerint le van tiltva. tooset hello megfelelő szolgáltatástulajdonságok verziójával 2013-08-15 szüksége tooenable CORS, vagy újabb verzióját, és adja hozzá a CORS szabályok toohello szolgáltatás tulajdonságait. Részletes információt tooenable vagy tiltsa le a CORS egy szolgáltatáshoz, és hogyan tooset CORS szabályok, adjon tekintse meg a túl[Blob szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452235.aspx), [várólista-tulajdonságok beállítása](https://msdn.microsoft.com/library/hh452232.aspx), és [tábla beállítása Szolgáltatás tulajdonságait,](https://msdn.microsoft.com/library/hh452240.aspx).

Itt látható egy minta egy CORS szabályt, a szolgáltatás tulajdonságainak beállítása művelet keresztül:

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

Minden elem hello CORS szabály szerepel az alábbiakban olvasható:

* **AllowedOrigins**: hello eredettartományból toomake engedélyezett egy kérelmet hello tárolási szolgáltatás CORS keresztül. hello forrástartományt az hello tartomány mely hello a kérelem származik. Vegye figyelembe, hogy hello származási hello forrása, hogy a hello felhasználói kora toohello szolgáltatás elküldi a kis-és nagybetűket pontosan egyeznie kell lennie. Hello helyettesítő karakter is használható ' *' tooallow minden eredet tartományok toomake kérelmek CORS keresztül. Hello a fenti példában a hello tartományok [http://www.contoso.com](http://www.contoso.com) és [http://www.fabrikam.com](http://www.fabrikam.com) kéréseket a meghatározott hello szolgáltatást a CORS használatával végezheti.
* **AllowedMethods**: hello módszert (HTTP-kérelem műveletek), hogy hello forrástartományt használhatja a CORS kérelmek. A hello a fenti példában csak a PUT és a GET kérelmek engedélyezettek.
* **AllowedHeaders**: hello kérelemfejléc adott hello forrástartományt adhatnak meg hello CORS kérésre. Hello a fenti példában az x-ms-metaadatok, x-ms-metaadat-tároló és az x-ms-meta-abc kezdve minden metaadat fejléc engedélyezett. Vegye figyelembe, hogy hello helyettesítő karakter "*" azt jelzi, hogy bármely hello fejléc kezdődő megadott előtag engedélyezett.
* **ExposedHeaders**: hello válaszfejlécek hello válasz toohello CORS kérelemben küldött és hello böngésző toohello kérelmet kibocsátó által elérhetővé tett tárolókra. A fenti hello böngésző hello példában van bármely x-ms-meta fejléc kezdődő utasításai tooexpose.
* **MaxAgeInSeconds**: hello maximális ideje, hogy a böngésző hello ellenőrzési beállítások kérelem kell gyorsítótárazza.

hello Azure storage szolgáltatások támogatása megadását oszloplistájában fejlécek mindkét hello **AllowedHeaders** és **ExposedHeaders** elemek. tooallow egy kategóriát a fejlécek, megadhat egy közös előtag toothat kategóriát. Például megadó *x-ms-meta** oszloplistájában fejléc alapján lesznek meghatározva azok egy szabályt, amely az összes fejléc x-ms-meta kezdődő fog egyezni.

a következő korlátozások hello tooCORS szabályok vonatkoznak:

* Másolatot toofive megadhat tárolási szolgáltatás (Blob, Table és Queue) esetében a CORS-szabályokat.
* minden CORS szabályok beállítások hello kérés XML-címkék nélkül hello maximális mérete nem haladhatja meg a 2 KB lehet.
* engedélyezett fejléc, kitett fejléc, vagy engedélyezett származási hello hossza nem haladhatja meg a 256 karaktert.
* Megengedett fejlécek és elérhetőségi fejlécek lehetnek:
  * Literális fejlécek, ahol hello pontos fejlécnév valósul meg, például a **x-ms-meta-feldolgozott**. Legfeljebb 64 literális fejlécek hello kérésre adható meg.
  * Fejlécek, ahol hello fejléc előtag valósul meg, például a következő előtaggal **x-ms-metaadatok***. Adja meg a előtag ily módon lehetővé teszi, hogy, és elérhetővé teszi a bármely fejlécet, amely a megadott előtag hello kezdődik. Legfeljebb két oszloplistájában fejléc hello kérésre adható meg.
* a hello megadott módszerek (vagy HTTP-műveletek) hello **AllowedMethods** elemet meg kell felelnie az Azure storage szolgáltatás API-k által támogatott toohello módszerek. Támogatott módszereket törlése, GET, HEAD, egyesítési, POST, beállítások és a PUT.

## <a name="understanding-cors-rule-evaluation-logic"></a>CORS értékelési szabálylogikával ismertetése
Egy tároló szolgáltatás előzetes vagy tényleges kérelmet kap, ha a kérésre hello szolgáltatás keresztül hello szolgáltatás tulajdonságainak beállítása művelet létesítése hello CORS szabályok alapján értékeli ki. CORS-szabályokat, amelyben voltak beállítva a kérelem törzse hello hello szolgáltatás tulajdonságainak beállítása művelet hello sorrendben értékeli ki a rendszer.

CORS-szabályokat az alábbiak szerint értékeli:

1. Első lépésként hello forrástartományt hello kérelem hello felsorolt hello tartományok ellenőrizi **AllowedOrigins** elemet. Ha hello forrástartományt szerepel hello listájában, illetve minden engedélyezett hello helyettesítő karakterrel ' *', majd a kiértékelési hibákra vonatkozó szabályokat. Ha hello forrástartományt nincs megadva, akkor hello kérelem sikertelen lesz.
2. A következő metódus hello (vagy HTTP-műveletet) hello kérelem ellenőrizi hello felsorolt hello módszerek **AllowedMethods** elemet. Ha hello metódus szerepel hello listájában, majd szabályok értékelésének folytatja; Ha nem, akkor a hello kérelem sikertelen lesz.
3. Hello kérelem megfelel egy szabályt a forrástartomány és az metódust, ha ez a szabály kijelölt tooprocess hello kérelem és nincsenek további szabályok kiértékelése. Mielőtt hello kérés sikeres lehet, azonban bármely hello kérésre megadott fejlécek veti össze hello felsorolt hello fejléceket **AllowedHeaders** elemet. Ha küldi hello fejlécek nem felelnek meg a fejléc engedélyezett hello, hello kérelem sikertelen lesz.

Hello szabályok feldolgozása hello ahhoz, azok szerepelnek a hello kérelemtörzset, mivel a bevált gyakorlat része, meg kell adnia hello szigorúbb szabályaival tiszteletben tooorigins először hello listájában, így ezek értékeli ki a rendszer először. Adja meg a szabályokat, amelyek kevésbé korlátozó – például egy szabály tooallow minden eredet – hello lista hello végén.

### <a name="example--cors-rules-evaluation"></a>Példa – CORS szabályok kiértékelése
hello következő példa bemutatja egy részleges kérelemtörzset egy művelet tooset CORS szabályok hello tárolási szolgáltatásokhoz. Lásd: [Blob szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452235.aspx), [várólista-tulajdonságok beállítása](https://msdn.microsoft.com/library/hh452232.aspx), és [Table szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452240.aspx) hello kérés létrehozásával kapcsolatos részletekért.

```xml
<Cors>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>PUT,HEAD</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>*</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
    </CorsRule>
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
        <AllowedMethods>GET</AllowedMethods>
        <MaxAgeInSeconds>5</MaxAgeInSeconds>
        <ExposedHeaders>x-ms-*</ExposedHeaders>
        <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
    </CorsRule>
</Cors>
```

Ezt követően vegye figyelembe a következő CORS kérelmek hello:

| Kérés |  |  | Válasz |  |
| --- | --- | --- | --- | --- |
| **Módszer** |**Forrás** |**Kérelem fejlécei** |**A szabály egyezés** |**Eredménye** |
| **A PUT** |http://www.contoso.com |x-ms-blob-tartalomtípus |Első szabály |Sikeres |
| **GET** |http://www.contoso.com |x-ms-blob-tartalomtípus |Második szabály |Sikeres |
| **GET** |http://www.contoso.com |x-ms-client-request-id |Második szabály |Hiba |

hello első kérelem megfelel az első szabály hello – hello forrástartományt megegyezik az engedélyezett eredeteket hello hello metódus felel meg módszerek engedélyezett hello és hello fejléc engedélyezett fejlécek – hello megegyezik, és így képes lesz.

mivel hello metódus nem felel meg a módszerek engedélyezett hello hello második kérelem nem egyezik meg hello első szabály. Azonban egyezik hello második szabály, ezért ez sikeres.

hello harmadik távelérésének hello második szabály forrástartományt és metódust, így nincsenek további szabályok kiértékelése. Azonban hello *x-ms-client-request-id fejléc* , hello kérés nem teljesíthető, hogy hello harmadik szabály hello szemantikáját használhatott volna toosucceed hello ellenére hello második szabály, nem engedélyezi.

> [!NOTE]
> Bár ez a példa bemutatja egy kevésbé korlátozó szabály szigorúbb egy előtt általában hello ajánlott toolist hello szigorúbb szabályok először.
> 
> 

## <a name="understanding-how-hello-vary-header-is-set"></a>Hogyan van beállítva a hello Vary fejléce ismertetése
Hello *Vary* fejléc tanácsadás hello böngésző vagy felhasználói ügynök hello tooprocess hello kiszolgálókérése kiválasztott hello feltételeket vonatkozó kérelem fejlécmezők készlete álló szabványos HTTP/1.1 fejléc. Hello *Vary* fejléc főleg használható gyorsítótárazás proxyk, a böngésző támogatja, és a tartalomtovábbító, amely toodetermine használják, hogyan hello válasz gyorsítótárazza. További információkért lásd: hello hello előírása [Vary fejléce](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Hello böngésző vagy egy másik felhasználói ügynök gyorsítótárazza a CORS kérelmet hello válaszát, amikor a rendszer gyorsítótárazza a hello forrástartományt, eredet engedélyezett hello. Ha egy második problémáira hello kérésben tárolási erőforrásokhoz, amíg aktív hello gyorsítótár, hello felhasználói ügynök lekéri hello gyorsítótárazott forrástartományt. hello második tartomány nem felel meg hello gyorsítótárazott tartományifiók, így hello kérelem sikertelen lesz, ha más módon járnak. Bizonyos esetekben Azure Storage úgy állítja be az hello Vary fejléce túl**származási** tooinstruct hello felhasználói ügynök toosend hello későbbi CORS kérelem toohello szolgáltatás Ha a kért tartomány különbözik hello hello gyorsítótárazva forrása.

Az Azure Storage beállítja hello *Vary* fejléc túl**származási** vonatkozó tényleges GET vagy HEAD kérelmek hello a következő esetekben:

* Ha a hello kérelem származási pontosan egyezik hello forrása a CORS szabály szerint engedélyezett. toobe pontos egyezés, hello CORS szabály nem tartalmazhat helyettesítő karakter "*" karaktert.
* Nincs szabály egyező hello kérelem származási van, de a CORS hello tároló szolgáltatás engedélyezve van.

Ha egy GET vagy HEAD kérelem megfelel-e a CORS szabályt, amely lehetővé teszi, hogy minden eredet hello esetben hello válasz azt jelzi, hogy minden eredet engedélyezett, hello felhasználói ügynök gyorsítótár lehetővé teszi bármely forrástartományt érkező későbbi kérelmeket, amíg aktív hello gyorsítótár.

Vegye figyelembe, hogy a GET vagy HEAD eltérő módszerekkel kéréseket, hello tárolási szolgáltatások nem állítja be hello Vary fejléce, mivel válaszok toothese módszerek nem kerülnek a gyorsítótárba felhasználói ügynök.

hello alábbi táblázat tartalmazza az Azure storage tooGET/HEAD kérelem hello alapján korábban említett eset válaszol:

| Kérés | Fiók beállítás és a szabály kiértékelés eredménye |  |  | Válasz |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Megtalálható a kérelem származási fejléc** |**Ez a szolgáltatás számára megadott CORS szabályok** |**Létezik egyező szabály, amely lehetővé teszi minden eredet (*)** |**Létezik egyező szabály forrás pontos egyezés** |**Válasz Vary fejléce set tooOrigin tartalmazza** |**Válasz tartalmazza a hozzáférés-vezérlési-engedélyezett-forrása: "*"** |**Válasz tartalmazza a hozzáférés-vezérlési-kitett-fejlécek** |
| Nem |Nem |Nem |Nem |Nem |Nem |Nem |
| Nem |Igen |Nem |Nem |Igen |Nem |Nem |
| Nem |Igen |Igen |Nem |Nem |Igen |Igen |
| Igen |Nem |Nem |Nem |Nem |Nem |Nem |
| Igen |Igen |Nem |Igen |Igen |Nem |Igen |
| Igen |Igen |Nem |Nem |Igen |Nem |Nem |
| Igen |Igen |Igen |Nem |Nem |Igen |Igen |

## <a name="billing-for-cors-requests"></a>A CORS kérelmek számlázási
Sikeres ellenőrzés kéri, ha engedélyezte a fiók hello tárolási szolgáltatások bármelyikéhez CORS számlázása (meghívásával [Blob szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452235.aspx), [várólista-tulajdonságok beállítása](https://msdn.microsoft.com/library/hh452232.aspx), vagy [ Table szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452240.aspx)). toominimize költségek, vegye figyelembe, hogy hello beállítása **MaxAgeInSeconds** a CORS elemének szabályok tooa nagy érték, így hello felhasználói ügynök gyorsítótárazza hello kérelem.

Sikertelen ellenőrzés kérelmek nem lesz terhelve.

## <a name="next-steps"></a>Következő lépések
[Blob szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452235.aspx)

[Várólista-szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452232.aspx)

[Table szolgáltatás tulajdonságainak beállítása](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C eltérő eredetű erőforrások megosztása meghatározása](http://www.w3.org/TR/cors/)

