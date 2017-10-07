---
title: "aaaAzure Team adatok tudományos folyamat életciklus |} Microsoft Docs"
description: "Lépésre szükség tooexecute tudományos adatok projektjeikbe."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b1f677bb-eef5-4acb-9b3b-8a5819fb0e78
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev;
ms.openlocfilehash: 58114a1c2d3289d1c4b2781219d0bf9647dbccd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="team-data-science-process-lifecycle"></a>Team adatok tudományos folyamat életciklusa

hello Team adatok tudományos folyamat (TDSP) biztosít egy ajánlott életciklussal használható toostructure tudományos adatok projektjeikbe. hello életciklus hello lépéseit, a start toofinish, amelyek projektek általában végrehajtás sorolja fel. Ha az adatok egy másik tudományos életciklusát, például a [SZÍNELOSZLÁS-DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) vagy a szervezet saját egyéni folyamat továbbra is használhatja az alábbi fejlesztési életciklusának feladatalapú TDSP hello. 

Életciklus úgy tervezték, az adatok tudományos projektet tervezett tooship intelligens alkalmazások részeként. Ezek az alkalmazások központi telepítése a machine learning vagy mesterséges eszközintelligencia modellek prediktív elemzési. Felderítési adatok tudományos és alkalmi analytics projektek is is előnyt az ezt a folyamatot. Azonban ebben az esetben egyes lépéseit nem is szükséges.    

Íme hello vizuális ábrázolását **Team adatok tudományos folyamat életciklus**. 

![TDSP-életciklus](./media/data-science-process-overview/tdsp-lifecycle.png) 

hello TDSP életciklus ismételt végrehajtott öt fő szakaszból áll. Ezek a következők:

* **Üzleti ismertetése**
* **Adatok megszerzését és ismertetése**
* **Modellezési**
* **Üzembe helyezés**
* **Ügyfelek**

Minden szakaszra nyújtunk hello a következő információkat:

* **Célok**: hello meghatározott célkitűzések.
* **Hogyan toodo azt**: hello vázolt meghatározott feladatok és útmutatást elvégzését őket.
* **Az összetevők**: hello termékek és hello támogatása előállító őket.


## <a name="1-business-understanding"></a>1. Üzleti ismertetés

### <a name="goals"></a>Célok
* Hello **változók kulcs** , amelyek szerint hello tooserve megadott **célok modell** és amelynek a kapcsolódó metrikák használt hello projekt hello sikerességének meghatározásához.
* hello vonatkozó **adatforrások** azonosítja, hogy rendelkezik-e hozzáférési tooor igényeinek tooobtain hello üzleti.

### <a name="how-toodo-it"></a>Hogyan toodo azt
Ebben a szakaszban tárgyalt két fő feladatok van: 

* **Adja meg a célkitűzés**: az ügyfél és más érdekelt felek toounderstand és hello üzleti problémák azonosításához. Állítson össze, amelyek meghatározzák a hello üzleti céljaihoz és, hogy az adatok tudományos technikák célba kérdéseket.
* **Adatforrások azonosítása**: keresés hello a vonatkozó adatokat tartalmaz, amelyek megkönnyítik a hello projekt hello céljainak meghatározó hello-kérdések megválaszolása.

#### <a name="11-define-objectives"></a>1.1 a célok meghatározása

1. Ebben a lépésben a központi célkitűzése tooidentify hello kulcs **üzleti változók** , hogy hello elemzés toopredict kell-e. Ezek a változók hivatkozott tooas hello **célok modell** , és a hozzájuk társított runbookokat hello metrikák használt toodetermine hello sikeres hello projekt. Ilyen célok két példák alatt csalárd rendelés értékesítési előrejelzés vagy hello valószínűségét.

2. Adja meg a hello **célok projekt** kérése és a "éles" kérdésekre vonatkozó és adott és egyértelműen szűkebbé tenni. Adattudomány hello folyamata nevekkel, és számokat tartalmazhat tooanswer ilyen kérdéseket. Az éles kérdések további információkért lásd: [hogyan toodo Adattudomány](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blog. Adattudomány / gépi tanulás általánosan használt tooanswer öt típusú kérdésekre:
 
   * Mennyi vagy hány? (regresszió)
   * Mely kategóriát? (besorolási)
   * Melyik csoportot? (fürtökkel)
   * Az Ez furcsának? (anomáliadetektálás)
   * Melyik lehetőség kell tenni? (ajánlott)

    Határozza meg, amely ezeket a kérdéseket arra utasítja, és hogyan fogadó Ez a funkció az üzleti céljaihoz.

3. Adja meg a hello **projekt csapata** hello szerepkörök és felelősségek meghatározása, a tagjai megadásával. Amely akkor többször további információt ismertté mérföldkő magas szintű tervének kialakításához.  

4. **Adja meg a sikerkritériumok metrikái**. Példa: ügyfél forgalom előrejelzés pontosságát X % elérése a 3 hónapos projekt hello végére, úgy, hogy azt kínálhat előléptetések tooreduce forgalmának kezeléséhez. hello metrikák kell **INTELLIGENS**: 
   * **S**ütemezésből 
   * **M**easurable
   * **A**chievable 
   * **R**elevant 
   * **T**ime adathoz kötött 

#### <a name="12-identify-data-sources"></a>1.2 az adatforrások azonosítása
A válaszok tooyour éles kérdések ismert példák tartalmazó adatforrások azonosítása. Keresse meg a következő adatok hello:

* Adatok **érintett** toohello kérdést. Tudunk hello cél és a kapcsolódó toohello cél funkciókat?
* Adatok egy **pontos** a modell cél- és hello szolgáltatásai közül egyik fontos.

Nem ritka, például toofind, hogy a meglévő rendszerek toocollect, és további típusú adatok tooaddress jelentkezzen probléma hello és hello projekt céljának eléréséhez. Ebben az esetben előfordulhat, hogy szeretné toolook külső adatforrások, vagy frissítse az rendszerek toocollect új adatait.

### <a name="artifacts"></a>Összetevők
Ebben a szakaszban az alábbiakban hello termékek esetében:

* [**A dokumentum charter**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): szabványos sablon hello TDSP projekt struktúra definíciójának találhatók. Ez az élő dokumentum hello projekt alatt frissített, új felderítések válnak, és üzleti követelmények változnak. hello kulcsa tooiterate esetén ez a dokumentum részletesen, hozzáadása hello felderítési folyamat során. Tartsa hello felhasználói és más érdekelt felek hello módosítása részt, és egyértelműen kommunikációhoz hello módosítások toothem hello okait.  
* [**Adatforrások**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): Ez a hello **nyers adatforrások** hello szakasza **Adatmeghatározások** jelentést, amely megtalálható a hello TDSP projekt **jelentés**  mappa. Azt adja meg az eredeti hello és hello nyers adatok a célhelyekkel. Későbbi szakaszaiban ki további adatait, például parancsfájlok toomove hello adatok tooyour elemzési környezetben.  
* [**Adatok szótárak**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries): Ez a dokumentum leírást ad a hello hello ügyfél által adott adatokat. A leírások hello séma (adattípusok, információkat a ellenőrzési szabályok, ha van ilyen) információkat tartalmaznak, és hello entitás-kapcsolat diagramokat, ha elérhető.


## <a name="2-data-acquisition-and-understanding"></a>2. Adatgyűjtés és adatértelmezés

### <a name="goals"></a>Célok
* Egy tiszta, kiváló minőségű dataset amelynek kapcsolatok toohello cél változók értendők hello megfelelő analytics környezetben, készen áll a toomodel található.
* A megoldásarchitektúra hello adatok adatcsatorna toorefresh és pontszámmal adatok rendszeresen fejlesztettek ki.

### <a name="how-toodo-it"></a>Hogyan toodo azt
Az ebben a szakaszban leírt három fő feladat van:

* **Hello adatok** hello cél elemzési környezetbe.
* **Hello adatokba** toodetermine hello az adatminőségi megfelelő tooanswer hello kérdés esetén. 
* **Állítson be egy adatok folyamat** tooscore új vagy rendszeresen frissíteni az adatokat.

#### <a name="21-ingest-hello-data"></a>2.1 betöltési hello adatok
Hello folyamat toomove hello adatok beállítása az adatforrás helyének toohello célhelyeket, ahol analytics műveletek, például képzési és előrejelzéseket toobe végre. A műszaki adatok és hogyan toodo ez különböző adatok Azure-szolgáltatásokkal, tekintse meg a beállítások [adatok betöltése az analytics tárolási környezeteket](machine-learning-data-science-ingest-data.md). 

#### <a name="22-explore-hello-data"></a>2.2 hello adatokba
A modell betanításához kell toodevelop hello adatok hang ismertetése. Valós adatkészletek általában zajos vagy értékek hiányzik, vagy más eltérések állomással rendelkezzenek. Adatok összegzési és a képi megjelenítés kell az adatok használt tooaudit hello minőségének és információkkal hello szükséges tooprocess hello adatokat ahhoz, hogy készen áll a modellezési. Ez a folyamat gyakran ismétlődő.

TDSP biztosít egy nevezik automatizált segédprogram [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) toohelp hello adatok megjelenítése, és készítse elő az adatok összegző jelentéseket. Javasoljuk, hogy IDEAR első tooexplore hello adatok toohelp kezdve interaktívan nem kódolási a kezdeti adatok ismertetése fejlesztése és az adatok feltárása és a képi megjelenítés egyéni kód írását, majd. A hello adattisztításon útmutatóért lásd: [tooprepare adatainak továbbfejlesztett gépi tanulási feladatok](machine-learning-data-science-prepare-data.md).  

Ha elégedett tisztítani hello adatok hello minőségének, hello következő lépésre-e toobetter hello minták hello adatok, amelyek segítenek rejt válassza ki, és a cél egy megfelelő prediktív modellek fejlesztése. Keressen bizonyító adatok, milyen mértékben csatlakoztatott hello adatok érték toohello cél és a megfelelő adatok toomove előre a hello modellezési lépések van-e. Ebben az esetben a folyamat be nem gyakran ismétlődő. Pontosabb vagy több megfelelő adatok tooaugment hello adatkészletben kezdetben hello előző szakaszban azonosított szükség lehet toofind új adatforrásokat.  

#### <a name="23-set-up-a-data-pipeline"></a>2.3 adatok adatcsatorna beállítása
Ezenkívül toohello kezdeti adatfeldolgozást és a rendszer törölné az hello adatok, általában szükséges egy folyamat tooscore új adatok vagy a frissítési hello adatok tooset rendszeresen egy folyamatos tanulás folyamat részeként. Ezt megteheti a adatok folyamat vagy munkafolyamat beállításával. Íme egy [példa](machine-learning-data-science-move-sql-azure-adf.md) , hogy hogyan tooset fel az adatcsatorna [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

Ebben a szakaszban egy megoldásarchitektúra hello adatok feldolgozási folyamat fejlesztése. a következő szakaszok hello adatok tudományos projekt hello párhuzamosan is fejlesztése hello folyamat. hello csővezeték batch-alapú vagy streaming/real-egyszeri vagy egy hibrid attól függően, hogy az üzleti kell, és a meglévő rendszerek, amelybe ez a megoldás integrált megkötések hello. 

### <a name="artifacts"></a>Összetevők
hello alábbiakban hello termékek ebben a szakaszban.

* [**Minőségi jelentés**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Ez a jelentés tartalmaz ezekkel az adatokat, minden egyes attribútum és a cél, a prioritás stb hello változó közötti kapcsolatok [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) TDSP részét gyorsan hozhat létre a megadott eszköz Ez a jelentés például egy CSV-fájl vagy egy relációs tábla bármely táblázatos adatkészlethez. 
* **Megoldás architektúrája**: Ez lehet egy diagram vagy a leírást a adatok használt folyamat toorun pontozási vagy az új adatok előrejelzéseket a modell létrehozása után. Új adatok alapján a modell hello adatcsatorna tooretrain is tartalmaz. hello dokumentum tárolása hello [projekt](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) directory hello TDSP directory struktúra sablon használatakor.
* **Ellenőrzőpont döntési**: teljes szolgáltatás termékgondozó csoportja és a modell létrehozásának megkezdése előtt meg is újra kiértékelje hello projekt toodetermine hogy hello érték várt elegendő toocontinue elméletben a minden esetben hasznos azt. Akkor lehet, hogy, például készen tooproceed, szükség toocollect több adatot kell, vagy szakítsa hello projekt hello adatok nem létezik tooanswer hello kérdést.


## <a name="3-modeling"></a>3. Modellezés

### <a name="goals"></a>Célok
* Hello gépi tanulási modellek optimális adatok funkciói.
* Olyan informatív ML-modell, amely a leginkább előrejelzésre hello cél.
* Olyan ML-modell, amely a termelési alkalmas.

### <a name="how-toodo-it"></a>Hogyan toodo azt
Az ebben a szakaszban leírt három fő feladat van:

* **Jellemzőkiemelés**: hello nyers adatok toofacilitate modell betanítási adatok szolgáltatások készíteni.
* **A modell betanítási**: található hello modell adott válaszok hello kérdés legpontosabban a sikerkritériumokat összehasonlítva.
* Annak megállapítása, hogy a modell **éles alkalmas**.

#### <a name="31-feature-engineering"></a>3.1 jellemzőkiemelés
A szolgáltatás műszaki osztály magában foglalja a befoglalási, összesítési és nyers változók toocreate átalakítása hello hello elemzés használt szolgáltatásait. Ha azt szeretné, hogy a modell Előfeltételek betekintést, akkor szüksége toounderstand hogyan jellemzői a következők egyéb kapcsolódó tooeach, és hogyan hello gépi tanulási algoritmusok toouse azokat a funkciókat. Ez a lépés szükséges tartomány szakértelmét és elemzések hello adatok feltárása lépésben beszerzett kreatív kombinációja. Ez a Keresés és informatív változóval együtt túl sok független változók elkerülésével egy egyensúlyt igényel. Informatív változók javítása az eredmény; független változók bevezetéséhez hello modell nélküli "zajt". Szüksége is toogenerate ezeket a funkciókat pontozási során kapott új adatokat. Úgy hello generációja ezeket a funkciókat csak függőségi viszonyban lehet adatok pontozás hello időpontban is érhető el. A szolgáltatás műszaki osztály különböző az Azure data technológiákhoz műszaki útmutató: [Jellemzőkiemelés hello tudományos folyamata a](machine-learning-data-science-create-features.md). 

#### <a name="32-model-training"></a>3.2 a modellek betanítása
Kérdés-válasz próbált típusától függően érhetők el számos modellezési algoritmus. Hello algoritmusok kiválasztásáról útmutatóért lásd: [hogyan toochoose algoritmusok használata a Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md). Bár ez a cikk az Azure Machine Learning, hello akkor hasznos, ha bármely gépi tanulási a projektek nyújt útmutatást. 

hello folyamat modell képzési hello a következő lépéseket tartalmazza: 

* **Vegyes hello bemeneti adatai** véletlenszerűen a modellezési a tanítási adathalmazt és a vizsgálati adatkészletet.
* **Hello modellek létrehozása** hello tanítási adathalmazt használatával.
* **Értékelje ki** (tanítási és tesztelési dataset) tanulási algoritmus együtt hello különböző társított hangolási paraméterek (más néven paraméter ismétlés), amely a hello érdeklő hello kérdés megválaszolásával célközönségre versengő gép sorozata aktuális adatokat.
* **Hello "ajánlott" megoldás meghatározásához** tooanswer hello kérdés összehasonlítva hello sikeres metrika alternatív módszerek között.

> [!NOTE]
> **Adatszivárgás megakadályozásához**: adatok hello felvételét az adatszivárgás okozhatja adatkészletből külső hello képzési, amely lehetővé teszi, hogy a modell vagy a gépi tanulási algoritmus toomake irreálisan jó előrejelzéseket. Adatszivárgás miért adatszakértőkön beolvasása ideg, ha csak prediktív eredmények, amelyek az adatok túl jó toobe igaz gyakori oka. Ezek a függőségi rögzített toodetect lehet. tooavoid kiszivárgásának gyakran szükséges a léptetés egy elemző adatkészlet létrehozása, a modell létrehozása és hello pontossága kiértékelése között. 
> 
> 

Azt adja meg egy [automatikus modellezéséhez és jelentéskészítési eszköz](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) TDSP rendelkező, amely képes toorun keresztül több algoritmusok és paraméter halmokat tooproduce egy modell. A jelentés minden modell és paraméter kombináció, többek között a változó fontosság teljesítményének összefoglalójához modellezési alapterv is képes. Ez a folyamat esetén is iteratív, mert azt is meghajtó további szolgáltatás mérnöki csapathoz. 

### <a name="artifacts"></a>Összetevők
Ebben a szakaszban létrehozott hello összetevők a következők:

* [**Szolgáltatáskészletek**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): hello modellezési kifejlesztett hello funkciókat ismerteti a hello **szolgáltatáskészletek** hello szakasza **adatainak definíciója** jelentés. Mutatók toohello kód toogenerate hello szolgáltatásait és hello szolgáltatás létrehozásáról leírása tartalmazza.
* [**A jelentés a modell**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): minden modell, amely próbálkozik, egy szabványos, amely adatokat minden egyes kísérlet a sablon-alapú jelentés hozott létre.
* **Ellenőrzőpont döntési**: mérlegelje, hogy hello modell működik jól elég toodeploy azt tooa éles rendszer. Néhány fontos kérdések tooask a következők:
  * Nem hello modell válasz hello kérdés elegendő abban, hogy a megadott hello Tesztadatok? 
  * Érdemes kipróbálni! bármely alternatív módszerek: további adatokat gyűjtsön, ne további szolgáltatás mérnöki csapathoz, vagy más algoritmusok kísérletezhet?


## <a name="4-deployment"></a>4. Környezet

### <a name="goal"></a>Cél
* Adatok adatcsatorna modellek a következők: telepített tooa éles vagy hasonló környezetet a végfelhasználó elfogadását. 

### <a name="how-toodo-it"></a>Hogyan toodo azt
hello fő feladat ebben a szakaszban tárgyalt:

* **Azok a hello modell**: hello modell, mind az adatcsatorna tooa éles vagy alkalmazás felhasználásra hasonló környezet telepítése.

#### <a name="41-operationalize-a-model"></a>4.1-es vagy azok egy modell
Miután egy modellt a hatékony készletét, azokat is lehet operationalized az egyéb alkalmazások tooconsume. Attól függően, hogy hello üzleti követelmények a előrejelzéseket valós időben vagy kötegelt alapon történik. toobe operationalized, hello modellek elérhetővé tett egy nyitott API felülettel online webhelyek, táblázatkezelő programok, irányítópultok vagy üzleti és a háttérkiszolgáló alkalmazások például különböző alkalmazásokból könnyen felhasznált toobe rendelkezik. Az Azure Machine Learning webszolgáltatás a modell operationalization példákért lásd [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md). Egyben a bevált gyakorlat toobuild telemetriai és figyelést üzemi modell hello és adatok telepített adatcsatorna toohelp hello jelentések és hibaelhárítás későbbi rendszer állapotú.  

### <a name="artifacts"></a>Összetevők
* A rendszer állapotának és a kulcs mérőszámok irányítópult állapotát.
* Végső modellezési központi telepítésének részletei jelentés.
* Végső megoldás architektúrája dokumentum.

## <a name="5-customer-acceptance"></a>5. Felhasználói elfogadás

### <a name="goal"></a>Cél
* **Hello projekt termékek véglegesítése**: Ellenőrizze, hogy hello csővezeték hello modell és éles környezetben a központi telepítés amelyek ügyfél céljait.

### <a name="how-toodo-it"></a>Hogyan toodo azt
Ebben a szakaszban tárgyalt két fő feladatok van:

* **Rendszer érvényesítési**: Ellenőrizze a központilag telepített hello modell, mind az adatcsatorna teljesítésének ügyfelek igényeinek megfelelően.
* **Az aktuális ki a projekt**: toohello entitás toorun hello rendszer éles környezetben.

hello ügyfél érdemes ellenőrizni, hogy hello rendszer megfelel az üzleti igényeiknek és hello válaszok hello elfogadható pontossága toodeploy hello rendszer tooproduction használatra kérdéseket az ügyfélalkalmazás által. Minden hello dokumentációt fejeződik be, és tekintse át. Egy az aktuális indító hello projekt toohello entitás műveletek felelős befejeződött. Ehhez az entitáshoz lehet, például az informatikai vagy felhasználói adatok tudományos team vagy hello ügyfél, amely felelős az éles környezetben hello rendszert futtató ügynök. 

### <a name="artifacts"></a>Összetevők
hello fő összetevő előállított végső ebben a szakaszban hello **kilépési jelentés a projekt ügyfél**. Ez hello hello projekt összes részleteit tartalmazó adott hasznos toolearn kapcsolatos műszaki jelentést, és hello rendszer működik. Egy [kilépési jelentés](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) sablont használja, vagy az adott ügyfélnek kell testre szabott TDSP biztosítja. 

## <a name="summary"></a>Összefoglalás
Hello [Team adatok tudományos folyamat életciklus](http://aka.ms/datascienceprocess) többször is lépések, amely útmutatást nyújtanak hello feladatok sorozatát toouse prediktív modelleket igény szerint van modellezve. Ezek a modellek egy éles környezetben kihasználhatók toobe toobuild intelligens alkalmazások üzembe helyezhető. hello folyamat életciklus célja toocontinue toomove adatok tudományos projekt előre egy tiszta engagement végpont felé. Amíg az igaz értékű, adattudomány egy gyakorlatot kutatási és a felderítési folyamatban képes tooclearly kommunikációhoz ezen feladatok tooyour csoportját, és az ügyfelek összetevők, amelyek az alkalmazottak szabványosított sablonok segítségével jól meghatározott félreértés elkerülése és egy összetett adatokat tudományos projekt sikeres befejezésének hello esélyét növelése.

## <a name="next-steps"></a>Következő lépések
Végpont forgatókönyvek, amelyek bemutatják, hello lépéseket a hello folyamata teljes **meghatározott forgatókönyvek** is rendelkezésre állnak. Szerepel a listában, és kapcsolódik a miniatűr leírásokat hello [Team adatok tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md) témakör.

