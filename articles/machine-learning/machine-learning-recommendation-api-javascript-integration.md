---
title: "Machine Learning javaslatok: JavaScript integrációs |} Microsoft Docs"
description: "Az Azure Machine Learning javaslatok - integráció JavaScript - dokumentációja"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: bbbb5bb6-489d-4a62-a2ae-f36237e9e2e1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 8f27962d097bffc2a03de80244ae41d6573a4bf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure Machine Learning Recommendations – JavaScript-integráció
> [!NOTE]
> El kell kezdenie a javaslatok API kognitív szolgáltatás helyett jelen verziójában. A javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és az új szolgáltatások nincs fejlesztik ki. Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.
> További információ [áttelepítése az új kognitív szolgáltatáshoz](http://aka.ms/recomigrate)
> 
> 

Ez a dokumentum jelzik, hogyan integrálható a JavaScript használó webhelyet. A JavaScript lehetővé teszi a adatgyűjtést események küldésére, valamint a javaslatok felhasználását, miután egy javaslat modell létrehozása. JS keresztül végzett összes műveletet is megteheti a kiszolgálóoldali.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a>1. Általános – áttekintés
A 2 fázisok a webhely integrálása az Azure ML javaslatok állnak:

1. Események küldése az Azure ML javaslatokat. Ez lehetővé teszi a javaslat modellek létrehozásához.
2. A javaslatok felhasználását. Miután összeállította a modell használatba vehetné a javaslatokat. (Ez a dokumentum nem ismerteti a modell létrehozásához olvassa el a gyors üzembe helyezési útmutatót kapcsolatos további információkért).

<ins>Első</ins>

Az első fázisban behelyezi a html-lapok egy kis JavaScript-függvénytárat, amely lehetővé teszi az események küldése az Azure ML javaslatok kiszolgálók (az adatok piaci) keresztül előforduló a html-weblap lapján:

![Drawing1][1]

<ins>Fázis</ins>

A második fázisában, ha szeretne a javaslatok megjelenítése a lapon, válassza ki a következő lehetőségek közül:

1. a kiszolgáló (az oldal megjelenítési fázisa) meghívja az Azure ML javaslatok kiszolgáló (az adatok piaci) keresztül javaslatok eléréséhez. Az eredmények tartalmazzák a cikkek azonosítójának listáját. A cikkek metaadatai (pl. képek, leírás) az eredmények kiegészítése, és a létrehozott lapra kapjon a böngésző kell a kiszolgálót.

![Drawing2][2]

2 a másik lehetőség egy ajánlott elemek egyszerű listájának első lépése az kis JavaScript-fájl használatára. Itt kapott adatok Karcsúbb a az első lehetőség.

![Drawing3][3]

## <a name="2-prerequisites"></a>2. Előfeltételek
1. Hozzon létre egy új modell API-val. A gyors üzembe helyezési útmutatójában megtudhatja, hogyan teheti meg.
2. Kódolja a &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; a Base64 kódolású. (Ez lesz használható az egyszerű hitelesítés esetén a JS kódot annak az API-k engedélyezése).

## <a name="3-send-data-acquisition-events-using-javascript"></a>3. A JavaScript használatával adatgyűjtést események küldése
Az alábbi lépéseket a küldő események elősegítése:

1. Vegye fel a kód JQuery könyvtár. Letöltheti a nugetből a következő URL-címben.
   
     http://www.nuget.org/Packages/jQuery/1.8.2
2. A következő URL-címet a javaslatok Java parancsfájl kódtárat tartalmaz: http://aka.ms/RecoJSLib1
3. A megfelelő paraméterekkel rendelkező Azure ML javaslatok kódtár inicializálása.
   
     <script>AzureMLRecommendationsStart ("<base64encoding of username:key>", "< model_id >"); </script> 
4. Elküldeni a megfelelő eseményt. Tekintse meg a részletes című szakaszt az összes típusú eseményeket (például kattintson esemény) <script> Ha (typeof AzureMLRecommendationsEvent == "nincs definiálva") {         
                     AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script>

### <a name="31----limitations-and-browser-support"></a>3.1.    A korlátozásokkal, valamint a támogatott böngésző
Ez a hivatkozás megvalósítása és kap, mert a. Az összes ismertebb böngésző támogatniuk kell.

### <a name="32----type-of-events"></a>3.2.    Események
5 különböző típusú eseményt, amely támogatja a tár:, kattintson a javaslat, kattintson a Hozzáadás gombra üzemi kosárhoz, távolítsa el az üzemi bevásárlókocsiból és beszerzési. Nincs olyan további esemény miatt lehet beállítani a felhasználói környezet bejelentkezési neve.

#### <a name="321-click-event"></a>3.2.1. Kattintson az esemény
Ez az esemény minden alkalommal elem kattintott kell használni. Általában akkor, ha a felhasználó kattint egy elemet egy új lap megnyitása a cikk adatokkal; Ezen a lapon az esemény megtörténjen.

Paraméterek:

* "kattintson" (karakterlánc, kötelező) - esemény
* elem (karakterlánc, kötelező) – az elem egyedi azonosítója
* itemName (karakterlánc, nem kötelező) – az elem neve
* itemDescription (karakterlánc, nem kötelező) – a cikk leírása
* itemCategory (karakterlánc, nem kötelező) – a cikk a kategória
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "click", item: "3111718" });
        </script>

Vagy nem kötelező adatokkal:

        <script>
            if (typeof AzureMLRecommendationsEvent === "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "click", item: "3111718", itemName: "Plane", itemDescription: "It is a big plane", itemCategory: "Aviation"});
        </script>


#### <a name="322-recommendation-click-event"></a>3.2.2. A javaslat kattintson esemény
Ez az esemény minden alkalommal Azure ML javaslatok az ajánlott elemet fogadott elem kattintott kell használni. Általában akkor, ha a felhasználó kattint egy elemet egy új lap megnyitása a cikk adatokkal; Ezen a lapon az esemény megtörténjen.

Paraméterek:

* "recommendationclick" (karakterlánc, kötelező) - esemény
* elem (karakterlánc, kötelező) – az elem egyedi azonosítója
* itemName (karakterlánc, nem kötelező) – az elem neve
* itemDescription (karakterlánc, nem kötelező) – a cikk leírása
* itemCategory (karakterlánc, nem kötelező) – a cikk a kategória
* magok (karakterlánc-tömbben, nem kötelező) – a mag, ami a javaslat lekérdezés jön létre.
* (karakterlánc-tömbben, nem kötelező) - recoList kattintott elemet létrehozó javaslat kérelem eredményét.
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "recommendationclick", item: "18899918" });
        </script>

Vagy nem kötelező adatokkal:

        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: eventName, item: "198", itemName: "Plane2", itemDescription: "It is a big plane2", itemCategory: "Default2", seeds: ["Seed1", "Seed2"], recoList: ["199", "198", "197"]                 });
        </script>


#### <a name="323-add-shopping-cart-event"></a>3.2.3. Vásárlási bevásárlókocsiból esemény hozzáadása
Ezt az eseményt kell használni. Ha a felhasználó vegyen fel egy elemet a kosár.
Paraméterek:

* "addshopcart" (karakterlánc, kötelező) - esemény
* elem (karakterlánc, kötelező) – az elem egyedi azonosítója
* itemName (karakterlánc, nem kötelező) – az elem neve
* itemDescription (karakterlánc, nem kötelező) – a cikk leírása
* itemCategory (karakterlánc, nem kötelező) – a cikk a kategória
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a>3.2.4. Eltávolítása a bevásárlókocsiból esemény vásárlás
Ez az esemény akkor kell használni, amikor a felhasználó eltávolítja a kosár elemet.

Paraméterek:

* "removeshopcart" (karakterlánc, kötelező) - esemény
* elem (karakterlánc, kötelező) – az elem egyedi azonosítója
* itemName (karakterlánc, nem kötelező) – az elem neve
* itemDescription (karakterlánc, nem kötelező) – a cikk leírása
* itemCategory (karakterlánc, nem kötelező) – a cikk a kategória
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a>3.2.5. A vásárlás esemény
Ez az esemény akkor kell használni, amikor a felhasználó a kosár vásárolt.

Paraméterek:

* "beszerzési" (karakterlánc) - esemény
* ([] beszerzett) cikkek - bejegyzést birtokló minden elem vásárolt tömb.<br><br>
  Megvásárolt formátuma:
  * elem (karakterlánc) – az elem egyedi azonosítója.
  * megadott számú (egész szám vagy karakterlánc) - beszerzett cikkek.
  * ár (float vagy karakterlánc) - nem kötelező mező – a cikk árát.

Az alábbi példában látható 3 beszerzési elemek (33, 34, 35), kettő fel mezők (elem, count, ár) és egy (elem 34) ár nélkül.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a>3.2.6. Felhasználói bejelentkezési esemény
Az Azure ML javaslatok esemény könyvtár hoz létre, és a cookie-t használja, amely innen származik: ugyanazzal a böngészővel események megismerése érdekében. A modell eredmények Azure ML javaslatok javítása érdekében lehetővé teszi a felhasználó egyedi azonosítóját, amelyek felülírják a cookie-k használatának beállításához.

Ez az esemény után a felhasználói bejelentkezési és a hely számára használható.

Paraméterek:

* "userlogin" (karakterlánc) - esemény
* felhasználó (karakterlánc) – a felhasználó egyedi azonosítóját.
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a>4. Javaslatok JavaScripttel felhasználása
A kódot, amely a javaslat akkor váltja ki, néhány JavaScript esemény által az ügyfél weblap. A javaslat válasz tartalmazza, az ajánlott elemek van, a nevek és azok minősítése. Legjobb, ha használja ezt a beállítást csak a lista megjeleníti a javasolt elemeket - összetettebb kezelése (például a cikk metaadatok hozzáadása) el kell végezni a kiszolgáló oldalán integrálását.

### <a name="41-consume-recommendations"></a>4.1 felhasználásához javaslatok
Felhasználását ajánlásokat kell tartalmaznia a JavaScript szükséges kódtárak és AzureMLRecommendationsStart hívása a lap a. Lásd a 2. szakasz.

Meg kell hívnia a hívott metódus egy vagy több elem javaslatok felhasználásához: AzureMLRecommendationsGetI2IRecommendation.

Paraméterek:

* elemek (karakterláncokból álló tömb) - javaslatok segítségével egy vagy több elemét. Ha egy Fbt build felhasznált, beállíthat Itt csak egy elemet.
* numberOfResults (int) - szükséges eredmények száma.
* includeMetadata (logikai érték, nem kötelező) – Ha értéke "true" azt jelzi, hogy a metaadatok mezőt ki kell tölteni az eredményben.
* Függvény - egy függvény kezelésére a javaslatok feldolgozása adott vissza. Az adatok egy tömbjét adja vissza a rendszer:
  * Konfigurációelem - elem egyedi azonosítója
  * név - elem neve (ha létezik a katalógus)
  * minősítés - javaslat minősítése
  * metaadat - karakterlánc, amely a metaadatokat az elem jelöli

Példa: A következő kódot kér 8 javaslatok elem "64f6eb0d-947a-4c18-a16c-888da9e228ba" (és nem ad meg a includeMetadata - implicit módon felirat látható, hogy nincsenek metaadatok szükség), azt a puffer majd összefűzésére az eredményeket.

        <script>
             var reco = AzureMLRecommendationsGetI2IRecommendation(["64f6eb0d-947a-4c18-a16c-888da9e228ba"], 8, false, function (reco) {
                 var buff = "";
                 for (var ii = 0; ii < reco.length; ii++) {
                       buff += reco[ii].item + "," + reco[ii].name + "," + reco[ii].rating + "\n";
                 }
                 alert(buff);
            });
        </script>


[1]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing1.png
[2]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing2.png
[3]: ./media/machine-learning-recommendation-api-javascript-integration/Drawing3.png
