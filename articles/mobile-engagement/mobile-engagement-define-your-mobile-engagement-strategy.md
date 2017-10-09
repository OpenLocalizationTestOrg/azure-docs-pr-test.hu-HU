---
title: "aaaDefine a Mobile Engagement-stratégia |} Microsoft Docs"
description: "Megtudhatja, hogyan tooonboard és optimalizálható a Mobile Engagement az elemzések és leküldéses értesítéseket."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7533e318-81b9-4360-aace-b7be8225985b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: afe32cb71019092eb28f2a8557404d60ad48ada4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="define-your-mobile-engagement-strategy"></a>A Mobile Engagement-stratégia kidolgozása
*Az alkalmazás okokból megírt: toohave a felhasználók használni!*

Úgy véljük, hogy biztosan put nagyszerű kezelésére toomake fektetett erőfeszítéssel azt nagyszerű alkalmazás legyen, hogy a felhasználók szeretni fognak. Az is valószínű marketing költségvetési tooacquire felhasználók jelentős időt fektetett. Azonban a hello kezdeti szívderítő áradata felhasználók lassan abbahagyják az alkalmazás segítségével láthatja. *Minden szól ez az Azure Mobile Engagement!* : rávenni toostick, és lehetővé teszik tooincrementally keresztül teszt fejleszteni az alkalmazást, és ismerje meg.

Alkalmazás felhasználói leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel, azonban kifinomult módon, az üzenetek és a különböző kommunikációs toothem, az alkalmazáson belüli egyes függően tootheir viselkedés amikről a megközelítést tooimproving megőrzési és használati alapul. Célunk toolet hello megfelelő időben és helyen hello hello megfelelő célközönség kommunikálni.

De az adott, konfigurálnia kell a toostart *ismernie a felhasználókat*, majd hozzon létre csoportokat tevékenységük vagy jellemzőik (ezért nevezik szegmensek) alapján, és ezután hozzon létre a megfelelő kommunikációs tooeach szegmens.

## <a name="mobile-engagement-serves-your-objectives"></a>A Mobile Engagement segítséget nyújt a céljai megvalósításához
*Beszéltünk megtartásról és használatról, de mi célból?*

A Mobile Engagement-stratégia kidolgozásához először az alkalmazás céljait és fő teljesítménymutatóit (KPI-k) kell szemügyre vennie.

Első lépésként hello célkitűzések és a KPI-k, amelyek segítenek toodefine a bevonási használati eseteket hello megfelelő megvilágításba helyezik.

A használati esetek is szeretné a felhasználókkal, kezdve a hello egyszerű üdvözléssel, toohello nagyon toomake toocommunicate speciális az informatikai rendszer által indított segédprogram értesítési kampányok egyszerű listája. Egy jól felépített használati esetnek tartalmaznia kell legalább hello kérdéscsoportra adott válaszok alapján *mit-kinek-mikor*:

1. Egy nagyon rövid megjelölést (például: „Üdvözlőkampány”).
2. **Mi**: egy üzenet példa (például "örülünk toohave meg hozzánk! Ne feledje toologin tooget az 1. ingyenes hónap igénybe vételéhez! "). Ez az üzenet nem végleges, be fog tudni toochange eszközökkel, bármikor, de általában segít toostart továbbléphetnek milyen azt szeretné, hogy toosay.
3. **Aki**: hello (például "minden felhasználó először hello hello alkalmazást elindító 3 nappal ezelőtt, ellátogatott a bejelentkezési oldal hello, de rendelkezik nem jelentkezett be") ezt az üzenetet megkapó szegmens.
   * Igen, az Azure Mobile Engagement segítségével ezt nagyon egyszerűen elvégezheti :)
   * Ebben az esetben ez nincs toobe végleges, bármikor meghatározhatja a szegmenseket, de a szegmentálási feltételeket tooensure a korai fontos toodefine gyűjtött hello megfelelő adatok.
4. **Amikor**: a kampány időzítése hello. A kampány indulhat adott napon, egy adott művelet után vagy kiválthatja egy eseményindító. A Mobile Engagement számos lehetőséget kínál lehetőségek toorightly idő a kommunikáció.

Miután használati esetek és a szegmens meghatározása, biztosít egy toodefine iránymutatás-hello az alkalmazáson belül összegyűjtendő adatok. Ez az hello szerepe a *"címkézési terv"*. A címkézési tervvel, tooensure, amely hello adatgyűjtés megadott toohello fejlesztők számára. Emiatt a fejlesztők képes a Mobile Engagement a hello tooembed jobb beállítása meg toowork a kampányok hello megfelelő adatokkal. Azt is nagyon fontos toorun tesztek tooensure hello integráció helyes-e, és összegyűjti az alábbiakra lesz szüksége.

Hello integrációs alapuló alkalmazások közzétételét, akkor forgalmazóként rendszer képes toosee az elemzéseket valós időben, szegmentálhatja a célközönséget, majd indítsa el a toosend intelligens módon célzott leküldéses értesítési tooengage kívül hello alkalmazások vagy végfelhasználók.

### <a name="use-cases-tooget-started"></a>Használati esetek tooget indítása
1. Üdvözlési stratégia: hozzon létre több leküldéses értesítéses kampányokkal hello alkalmazás toore megszólítása: D + 2/5/10/15 sorrendben után hello első munkamenet és növelje az első futtatási megőrzési hello indításakor hello végfelhasználói viselkedés alapján.
2. Népszerűsítsen hello végfelhasználói toosend hello információk csak tooend-felhasználók, akik nagyobb eséllyel tooengage hello viselkedése alapján új tartalmakat (funkció, cikk/videó vagy termék).
3. Hello alkalmazás aránya: kisebb, mint 1 százalékát, a felhasználói bázis, amely valószínűleg toorate hello 5 csillagos értékelést alkalmazásának hello tárolójában lévő cél.
4. Előfizetések növelése: előléptetni értékes tartalmakat tooend – a felhasználóknak, amely nem találkoztak még tooincrease előfizetés.
5. Oktatóanyag: Nincs többé mindenki számára kötelező oktatóanyag. Miért ne hozhatna létre nagyszerű oktatóanyagokat az alkalmazáson belül, és aktiválhatná azokat alkalmazáson belüli üzenetekkel csak akkor, ha a felhasználó hello úgy tűnik, toonot használata révén hello alkalmazást vagy nehézsége támad szolgáltatás segítségével?

## <a name="why-do-you-need-analytics-tooengage"></a>Miért kell analytics tooengage?
Ezen a ponton talán már rájött, hogy csak leküldéses értesítések létrehozása nem elegendő. a Mobile Engagement hello alapvető fogalma toohelp forgalmazók és fejlesztők hello megfelelő időben hello megfelelő végfelhasználók bevonásához, és a jobb oldali hello helyezze el. tooknow ezen három fő alapelv alapvető toogather analytics az alkalmazásból, és toosegment használni a célközönséget. Ez még sokkal hatékonyabb, ha a viselkedési szegmensek kiegészítik a többi adatbázisból, a CRM-rendszerből vagy a keresztcsatornából származó adatokat. A Mobile Engagement lehetővé teszi, hogy az adatok lekérdezése tetszőleges helyre, és tootarget hello megfelelő célközönség használja.

toobe hello leginkább megfelelő lehetséges módon vegye fel a célközönséget, akkor hello végfelhasználó viselkedésének tooknow ismerete döntő toohave hello az állapotuk valós idejű. Az adatgyűjtés lehetővé teszi a forgalmazók toofocus valóban a mi számít, tooplay használati esetek és a mobilmarketing stratégia célok eléréséhez. Korábban hello célok elérésére egyben hello oka hello ajánlott eljárás tulajdonképpen nem toogather semmi, és mindent hello elemzés, de csak azok, amelyek lehetővé teszik toofocus milyen azt szeretné, hogy toolearn és a használati esetek. Ez hello jó módszer toostart, próbálja, tesztelése és megtudhatja, hogyan toouse hello megoldás és a cím intelligens leküldéses értesítési és egy alkalmazás toobring hello megőrzésére növelje az sikertörténet szinten.

> [!NOTE]
> Ne feledje: Túl sok adat használhatatlanná teszi hello adatokat!
> 
> 

### <a name="use-cases-and-best-practices"></a>Használati esetek és ajánlott eljárások
Hello mutatjuk röviden néhány, a következő szakaszok kulcsban használati esetet, amelyekkel az ügyfelek tooget használatba találkoztunk.

#### <a name="media"></a>Média
Hello végfelhasználói által fogyasztott tartalmak típusa hello gyűjt, és szegmentálja a hello célközönséget ezen viselkedés tootarget adott típusú tartalom csak tooan célközönség, amely nagy valószínűséggel tooconsume lesz alapján. Ezzel elkerülheti a teljes felhasználói bázis elárasztását levélszeméttel, és jobb megtartást biztosíthat.

#### <a name="m-commerce"></a>M-kereskedelem
Hello hello alkalmazás és a cél a célközönség toopromote belül a legtöbbet megtekintett termékkategóriákat kedvezményes gyűjtése, vagy új terméket, amely a végfelhasználói hello kategória nagy valószínűséggel toopurchase lesz. Tooboost bevételek célja. Újra hello célja nem toospam!

#### <a name="gaming"></a>Játékok
Game hello szintű gyűjtése egy végfelhasználói és hello időt töltött az egy adott időszak tootarget hello célközönségnek juttathatja el, megakadhattak és várhatóan nagy valószínűséggel toojump tooa következő szintű lépnének egy bónuszajánlat segítségével.

Kommunikáció különleges eseményekről, már nem játszó bizonyos idő tootry tooencourage őket tooreturn ösztönző toothose felhasználókkal.

#### <a name="retail"></a>Kiskereskedelem
Gyűjtsön hello termékeket és márkákat, amelyeket a közönség a Kedvencek vagy a viselkedést és a meghajtó hello célközönség tooyour tároló tooincrease vásárlási bevételek alapján nagyobb eséllyel tooconsume kell lennie.

#### <a name="banking"></a>Banki szolgáltatások
Adatgyűjtés a végfelhasználók számára, amelyekkel hello alkalmazás hello első indításakor hoztak létre fiókot. Egy üdvözlési célzott leküldéses értesítésekkel toodeploy célja, és növelje a fiók-előfizetések hello száma.

### <a name="how-toocreate-a-great-tag-plan"></a>Hogyan toocreate remek címkézési terv?
A címkézési tervnek hello felhasználó által megtett út vagy egyfajta munkafolyamatának hello alkalmazás biztosít minden hello szükséges címkét (adatot), amely az összegyűjtött toohave kell lennie, elég analytics toounderstand felhasználói viselkedést és a megfelelő szegmens hello felhasználói bázis leírásának kell lennie. Ez nem műszaki jellegű feladat. Emiatt a forgalmazók képes toospecify hello adatokat toocollect a Mobile Engagement-stratégiájuk alapján kívánja.

hello minimális van tootag legalább összes hello képernyők (nevű *tevékenységek* a Mobile Engagement) az alkalmazás. Ez segít meghatározni, felhasználó által megtett út hello.

A tevékenység tartalmazhat *eseményeket*, amelyek műveletinformációkat gyűjtenek, ilyen például egy gombra való kattintás. Ez lehetővé teszi, hogy a kapcsolati hello alkalmazáson belül hello gyűjteménye. Ezért a forgalmazók képesek tooknow az milyen képernyő felhasználók, ahol tartózkodik, és milyen tevékenységet folytatnak.

`Jobs`időtartammal rendelkező műveletek. Ez nagyon hasznos a forgalmazók toounderstand mennyi idő alatt az a felhasználó toocreate partner vagy toologin példányhoz. Ez is hasznos lehet a fejlesztők toomonitor mennyi ideig tart toocall, egy webszolgáltatás-bővítmény.

`Errors`akkor is figyelt tooknow Ha felhasználó problémát tapasztal az alkalmazásban. Például: gyakran előforduló kapcsolódási problémák.

Az összes ilyen típusú adat kiegészíthető paraméterekkel (*további adatokat* a Mobile Engagement) lehetővé teszi dinamikus adatok toogather hello alkalmazásból. Ez egy fontos tooallow részletes Szegmentálás. Például forgalmazók szegmentálhatják hello általuk fogyasztott tartalmak típusa alapján. hello típusú tartalom lesz hello dinamikus adatokat tevékenységet vagy egy eseményt.

*Alkalmazásadatok* adat, amely lehetővé teszi a valós idejű hello alkalmazás vagy felhasználó hello tooconfirm hello állapotának. Ez is segít toocategorize egy célközönség talál, és gyorsan megcélzását. Használhat például egy igaz/hamis állapotot e hello felhasználó bejelentkezik-e, vagy az előfizetés lejárati dátuma.

#### <a name="example-of-tags"></a>Példa címkékre
*Használati eset: Szegmens célközönség viselkedésének tootarget hello jobb végfelhasználói hello megfelelő leküldéses értesítési tartalommal*

1. Leküldéses értesítési toopromote küldése adott termékkategória: gyűjtsön viselkedés adatok toosegment célközönség tekintettek hello termékkategória alapján termékkategóriát hányszor egy adott időtartam alatt vagy egy adott tételt hányszor adtak hozzá, a kosárhoz. összegyűjtött hello adatok lehetővé teszik toosegment és elküldheti-e a leküldéses értesítési toohello megfelelő célközönség.
2. Sebesség hello app: hello célközönséget a közösségi hálózat által megosztott hello tartalom alapján adatainak gyűjtéséről. Hello meghatározásával toosegment hello célközönség célja *Nagykövetek* az alkalmazás. Egy leküldéses értesítést az alkalmazáson belüli használ, hello Nagykövetek lesz hello legjobb célközönsége az alkalmazás tooask toorate az alkalmazás 5 csillaggal hello tárolójában.
   
   ![][1]

*Használati eset: Deklaratív adatok*

1. Szegmensnek szóló hírek: deklaratív adatok toosegment célközönség preferenciáik alapján gyűjtése. Ez lehetővé teszi leküldéses értesítések küldését egy olyan témával kapcsolatban, amely ténylegesen érdekel egy adott célközönséget.
2. A célközönség szegmentálása a bejelentkezési állapot alapján. Adatok tooknow gyűjtése, ha a felhasználó van csatlakoztatva, vagy létrehozott-e fiókot. Segítséget nyújt azon végfelhasználókat megcélzásában, akik még nem jelentkeztek a, és elküld egy leküldéses értesítési tooencourage végfelhasználói tooconvert.
   ![][2]

### <a name="next-steps"></a>Következő lépések
* Látogasson el [Mobile Engagement fogalmait] további alapfogalmait a Mobile Engagement toolearn.
* Látogasson el [Mobile Engagement-alkalmazás létrehozása](mobile-engagement-create.md) toocreate egy új Mobile Engagement-Alkalmazásgyűjteménynek az Azure és a start hello Mobile Engagement portálra az alkalmazások kezelését.
* Látogasson el [ajánlott eljárások](mobile-engagement-getting-started-best-practices.md) részleteinek toogo.
* Látogasson el [Játékalkalmazásról szóló forgatókönyvet](mobile-engagement-gaming-scenario.md) toolearn kapcsolatos Mobile Engagement minta játékalkalmazással végrehajtására. 
* Látogasson el [Médiaalkalmazásról szóló forgatókönyvet](mobile-engagement-media-scenario.md) toolearn kapcsolatos Mobile Engagement minta médiaalkalmazással végrehajtására. 
* Látogasson el [oktatóanyagok] hello megvalósításával kapcsolatos további toolearn.

<!-- Images. -->
[1]: ./media/mobile-engagement-define-your-mobile-engagement-strategy/use-case1.png
[2]: ./media/mobile-engagement-define-your-mobile-engagement-strategy/use-case2.png

<!-- URLs. -->
[Mobile Engagement fogalmait]: http://azure.microsoft.com/documentation/articles/mobile-engagement-concepts/
[oktatóanyagok]: http://azure.microsoft.com/documentation/articles/mobile-engagement-ios-get-started/

