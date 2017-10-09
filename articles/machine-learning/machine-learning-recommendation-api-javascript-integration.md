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
redirect_document_id: True
ms.openlocfilehash: 4c5f0eee4aa04ce823321d52985374c52850f0d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-recommendations---javascript-integration"></a>Azure Machine Learning Recommendations – JavaScript-integráció
> [!NOTE]
> El kell kezdenie hello javaslatok API kognitív szolgáltatás helyett jelen verziójában. hello javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és minden hello új szolgáltatások nincs fejlesztik ki. Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.
> További információ [áttelepítése toohello új kognitív szolgáltatás](http://aka.ms/recomigrate)
> 
> 

Ez a dokumentum jelzik a hogyan toointegrate a webhely JavaScript használatával. hello JavaScript lehetővé teszi toosend adatgyűjtést események és tooconsume javaslatokat a javaslat modell létrehozása után. JS keresztül végzett összes műveletet is megteheti a kiszolgálóoldali.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a>1. Általános – áttekintés
A 2 fázisok a webhely integrálása az Azure ML javaslatok állnak:

1. Események küldése az Azure ML javaslatokat. Ezzel a lépéssel engedélyezi a javaslat modell toobuild.
2. Hello javaslatok felhasználását. Miután hello modelljei épülnek hello javaslatok felhasználhat. (Ez a dokumentum nem azt ismertetik, hogyan toobuild modell, olvassa el hello gyors üzembe helyezési útmutató tooget olvashat).

<ins>Első</ins>

Hello az első fázis behelyezi a html-lapok egy kis JavaScript-függvénytárat, amely lehetővé teszi azok bekövetkezésekor hello html-lapot az Azure ML javaslatok kiszolgálók (az adatok piaci) keresztül történő hello toosend események:

![Drawing1][1]

<ins>Fázis</ins>

Hello második fázisában, ha azt szeretné, hogy tooshow hello javaslatok hello lapon, válassza ki az alábbi beállítások hello:

1. a kiszolgáló (az oldal megjelenítési hello fázisában) meghívja az Azure ML javaslatok kiszolgáló (az adatok piaci) keresztül tooget javaslatok. hello eredmények elemek azonosító listáját tartalmazza. A kiszolgáló tooenrich hello eredmények hello cikkek metaadatai (pl. képek, leírás) kell, és létre lap toohello böngésző hello küldése.

![Drawing2][2]

2 hello másik lehetőség egy toouse hello kis JavaScript-fájlt a fázis egy tooget ajánlott elemek egyszerű listája. Itt fogadott hello adatok Karcsúbb mint hello hello első beállításban.

![Drawing3][3]

## <a name="2-prerequisites"></a>2. Előfeltételek
1. Hozzon létre egy új modell hello API-k használatával. Hello – első lépések útmutató meg, hogyan toodo azt.
2. Kódolja a &lt;dataMarketUser&gt;:&lt;dataMarketKey&gt; a Base64 kódolású. (Ez használandó hello az egyszerű hitelesítés tooenable hello JS kód toocall hello API-k).

## <a name="3-send-data-acquisition-events-using-javascript"></a>3. A JavaScript használatával adatgyűjtést események küldése
a lépéseket követve hello küldő események elősegítése:

1. Vegye fel a kód JQuery könyvtár. Azt az URL-cím a következő hello nuget-ből tölthetik le.
   
     http://www.nuget.org/Packages/jQuery/1.8.2
2. Hello javaslatok Java parancsfájl kódtárat hello a következő URL-címet tartalmaz: http://aka.ms/RecoJSLib1
3. Hello megfelelő paraméterekkel rendelkező Azure ML javaslatok kódtár inicializálása.
   
     <script>AzureMLRecommendationsStart ("<base64encoding of username:key>", "< model_id >"); </script> 
4. Küldési hello megfelelő esemény. Tekintse meg a részletes című szakaszt az összes típusú eseményeket (például kattintson esemény) <script> Ha (typeof AzureMLRecommendationsEvent == "nincs definiálva") {         
                     AzureMLRecommendationsEvent = []; } AzureMLRecommendationsEvent.push({event: "click", item: "18321116"});</script>

### <a name="31----limitations-and-browser-support"></a>3.1.    A korlátozásokkal, valamint a támogatott böngésző
Ez a hivatkozás megvalósítása és kap, mert a. Az összes ismertebb böngésző támogatniuk kell.

### <a name="32----type-of-events"></a>3.2.    Események
Esemény 5 típusú hello könyvtár támogatja: kattintson a gombra, kattintson a javaslat, vegye fel a bevásárlókocsiból, tooShop eltávolítása üzemi bevásárlókocsiból és beszerzési. Nincs olyan további esemény miatt használt tooset hello felhasználói környezet bejelentkezési neve.

#### <a name="321-click-event"></a>3.2.1. Kattintson az esemény
Ez az esemény minden alkalommal elem kattintott kell használni. Általában akkor, ha a felhasználó kattint egy elemet egy új lap megnyitása hello elem adatokkal; Ezen a lapon az esemény megtörténjen.

Paraméterek:

* "kattintson" (karakterlánc, kötelező) - esemény
* elem (karakterlánc, kötelező) – hello elem egyedi azonosítója
* itemName (karakterlánc, nem kötelező) – hello hello elem neve
* itemDescription (karakterlánc, nem kötelező) – hello elem hello leírása
* itemCategory (karakterlánc, nem kötelező) – hello kategória hello elem
  
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
Ez az esemény minden alkalommal Azure ML javaslatok az ajánlott elemet fogadott elem kattintott kell használni. Általában akkor, ha a felhasználó kattint egy elemet egy új lap megnyitása hello elem adatokkal; Ezen a lapon az esemény megtörténjen.

Paraméterek:

* "recommendationclick" (karakterlánc, kötelező) - esemény
* elem (karakterlánc, kötelező) – hello elem egyedi azonosítója
* itemName (karakterlánc, nem kötelező) – hello hello elem neve
* itemDescription (karakterlánc, nem kötelező) – hello elem hello leírása
* itemCategory (karakterlánc, nem kötelező) – hello kategória hello elem
* (karakterlánc-tömbben, nem kötelező) - magok hello mag, ami hello javaslat lekérdezés jön létre.
* (karakterlánc-tömbben, nem kötelező) - recoList hello kattintott hello elemet létrehozó hello javaslat kérelem eredményét.
  
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
Ezt az eseményt kell használni. Ha a hello felhasználó hozzáadása egy elem toohello bevásárlókocsiból vásárlás.
Paraméterek:

* "addshopcart" (karakterlánc, kötelező) - esemény
* elem (karakterlánc, kötelező) – hello elem egyedi azonosítója
* itemName (karakterlánc, nem kötelező) – hello hello elem neve
* itemDescription (karakterlánc, nem kötelező) – hello elem hello leírása
* itemCategory (karakterlánc, nem kötelező) – hello kategória hello elem
  
        <script>
            if (typeof AzureMLRecommendationsEvent == "undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "addshopcart", item: "13221118" });
        </script>

#### <a name="324-remove-shopping-cart-event"></a>3.2.4. Eltávolítása a bevásárlókocsiból esemény vásárlás
Ez az esemény akkor kell használni, amikor hello felhasználó eltávolítja az elemet toohello kosár.

Paraméterek:

* "removeshopcart" (karakterlánc, kötelező) - esemény
* elem (karakterlánc, kötelező) – hello elem egyedi azonosítója
* itemName (karakterlánc, nem kötelező) – hello hello elem neve
* itemDescription (karakterlánc, nem kötelező) – hello elem hello leírása
* itemCategory (karakterlánc, nem kötelező) – hello kategória hello elem
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "removeshopcart", item: "111118" });
        </script>

#### <a name="325-purchase-event"></a>3.2.5. A vásárlás esemény
Ez az esemény akkor kell használni, amikor hello felhasználói vásárolt annak kosár.

Paraméterek:

* "beszerzési" (karakterlánc) - esemény
* ([] beszerzett) cikkek - bejegyzést birtokló minden elem vásárolt tömb.<br><br>
  Megvásárolt formátuma:
  * elem (karakterlánc) – hello elem egyedi azonosítója.
  * megadott számú (egész szám vagy karakterlánc) - beszerzett cikkek.
  * Árlista (float vagy karakterlánc) – nem kötelező mező – hello ára hello elemet.

hello az alábbi példában beszerzési 3 (33, 34, 35) elemek kettő fel mezők (elem, count, ár) és egy (elem 34) ár nélkül.

        <script>
            if ( typeof AzureMLRecommendationsEvent == "undefined"){ AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({ event: "purchase", items: [{ item: "33", count: "1", price: "10" }, { item: "34", count: "2" }, { item: "35", count: "1", price: "210" }] });
        </script>

#### <a name="326-user-login-event"></a>3.2.6. Felhasználói bejelentkezési esemény
Az Azure ML javaslatok esemény könyvtár hoz létre és használja a cookie-k rendelés tooidentify esemény, amely innen származik: hello ugyanazt a böngészőt. Eredmények Azure ML javaslatok rendelés tooimprove hello modell lehetővé teszi a felhasználó egyedi azonosítóját, amelyek felülírják hello cookie-k használatának tooset.

Ez az esemény után hello felhasználói bejelentkezési tooyour helyet kell használni.

Paraméterek:

* "userlogin" (karakterlánc) - esemény
* felhasználó (karakterlánc) – hello felhasználó egyedi azonosítóját.
  
        <script>
            if (typeof AzureMLRecommendationsEvent=="undefined") { AzureMLRecommendationsEvent = []; }
            AzureMLRecommendationsEvent.push({event: "userlogin", user: “ABCD10AA” });
        </script>

## <a name="4-consume-recommendations-via-javascript"></a>4. Javaslatok JavaScripttel felhasználása
hello kódot, amely akkor hello javaslat váltja ki, néhány JavaScript esemény hello ügyfél weblap. hello javaslat válasz ajánlott elemek van, a nevek és azok minősítései hello tartalmazza. Akkor ajánlott toouse ezt a beállítást ajánlott elemek - összetettebb (mint például a hello elem metaadatok hozzáadása) kezelése hello csak a lista megjeleníti a hello server ügyféloldali integrációs kell elvégezni.

### <a name="41-consume-recommendations"></a>4.1 felhasználásához javaslatok
tooconsume javaslatok tooinclude hello van szüksége a lap és toocall AzureMLRecommendationsStart JavaScript-tárak szükséges. Lásd a 2. szakasz.

egy vagy több elem tooconsume javaslatok van szüksége egy metódus hívása toocall: AzureMLRecommendationsGetI2IRecommendation.

Paraméterek:

* (karakterlánc tömbje) - elemek egy vagy több elem tooget javaslatok. Ha egy Fbt build felhasznált, beállíthat Itt csak egy elemet.
* numberOfResults (int) - szükséges eredmények száma.
* includeMetadata (logikai érték, nem kötelező) – Ha too'true beállítása "hello metaadatok mező ki kell tölteni a hello eredmény jelzi.
* Függvény - egy függvény kezelésére hello javaslatok feldolgozása adott vissza. hello adatok tömbjét adja vissza a rendszer:
  * Konfigurációelem - elem egyedi azonosítója
  * név - elem neve (ha létezik a katalógus)
  * minősítés - javaslat minősítése
  * metaadat - hello metaadatok hello elem jelölő karakterlánccá

Példa: a következő kód hello kérelmek 8 javaslatok elem "64f6eb0d-947a-4c18-a16c-888da9e228ba" (és nem ad meg a includeMetadata - implicit módon felirat látható, hogy nincsenek metaadatok szükség), azt a puffer majd összefűzésére hello eredmények.

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
