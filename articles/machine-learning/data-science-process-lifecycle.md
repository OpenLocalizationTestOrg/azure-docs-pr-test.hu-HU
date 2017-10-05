---
title: "Azure-csapat adatok tudományos folyamat életciklus |} Microsoft Docs"
description: "Az adatok tudományos projektek végrehajtásához szükséges lépéseket."
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
ms.openlocfilehash: 9b8ef4f1165a89fa6ed1b64b44d58bb45f08f232
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="team-data-science-process-lifecycle"></a>Team adatok tudományos folyamat életciklusa

A csapat adatok tudományos folyamat (TDSP) biztosít egy ajánlott életciklussal, amely segítségével az adatok tudományos projektek struktúra. Az életciklus kezdetétől a végéig, projektek általában követve azok végrehajtásakor, lépéseit mutatja be. Ha az adatok egy másik tudományos életciklusát, például a [SZÍNELOSZLÁS-DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) vagy a szervezet saját egyéni folyamat a feladatalapú TDSP továbbra is használható a fejlesztési életciklusának. 

Életciklus úgy tervezték, adatok tudományos projektek célja, hogy intelligens alkalmazások részét képezi. Ezek az alkalmazások központi telepítése a machine learning vagy mesterséges eszközintelligencia modellek prediktív elemzési. Felderítési adatok tudományos és alkalmi analytics projektek is is előnyt az ezt a folyamatot. Azonban ebben az esetben egyes lépéseit nem is szükséges.    

Íme egy vizuális ábrázolását a **Team adatok tudományos folyamat életciklus**. 

![TDSP-életciklus](./media/data-science-process-overview/tdsp-lifecycle.png) 

A TDSP életciklus ismételt végrehajtott öt fő szakaszból áll. Ezek a következők:

* **Üzleti ismertetése**
* **Adatok megszerzését és ismertetése**
* **Modellezési**
* **Üzembe helyezés**
* **Ügyfelek**

Minden szakaszhoz azt adja meg a következő információkat:

* **Célok**: a konkrét célokat.
* **Megtudhatja, hogyan teheti**: a leírt meghatározott feladatok és útmutatást elvégzését őket.
* **Az összetevők**: a termékek és az őket létrehozó támogatását.


## <a name="1-business-understanding"></a>1. Üzleti ismertetés

### <a name="goals"></a>Célok
* A **változók kulcs** megadott szolgál, amelyek a **célok modell** , és amelynek a kapcsolódó metrikák használnak a projekt sikerességének meghatározásához.
* A megfelelő **adatforrások** azonosítja, hogy az üzleti hozzáféréssel rendelkezik, vagy kell beszereznie.

### <a name="how-to-do-it"></a>Menete
Ebben a szakaszban tárgyalt két fő feladatok van: 

* **Adja meg a célkitűzés**: az ügyfél és az egyéb érintett felek megértéséhez, valamint az üzleti problémák azonosításához. Állítson össze, amelyek az üzleti célokat határozza meg, és adatok tudományos technikák célzó kérdésekre.
* **Adatforrások azonosítása**: a Keresés a vonatkozó adatokat tartalmaz, amelyek megkönnyítik a kérdésekre, amelyek meghatározzák a projekt céljainak.

#### <a name="11-define-objectives"></a>1.1 a célok meghatározása

1. Egy központi e lépés célja, hogy azonosítsa a kulcs **üzleti változók** , amelyet az elemzés előre jelezni. Ezek a változók nevezzük a **célok modell** és a hozzájuk társított mérőszámok segítségével meghatározhatja a projekt sikeréhez. Ilyen célok két többek között az értékesítési vagy éppen a csalárd rendelés valószínűségét.

2. Adja meg a **célok projekt** kérése és a "éles" kérdésekre vonatkozó és adott és egyértelműen szűkebbé tenni. Adattudomány ilyen kérdések megválaszolása nevek és számok használata során a rendszer. Az éles kérdések további információkért lásd: [Adattudomány módjáról](https://blogs.technet.microsoft.com/machinelearning/2016/03/28/how-to-do-data-science/) blog. Adattudomány / gépi tanulás általában használt öt típus kérdések megválaszolásához:
 
   * Mennyi vagy hány? (regresszió)
   * Mely kategóriát? (besorolási)
   * Melyik csoportot? (fürtökkel)
   * Az Ez furcsának? (anomáliadetektálás)
   * Melyik lehetőség kell tenni? (ajánlott)

    Határozza meg, amely ezeket a kérdéseket arra utasítja, és hogyan fogadó Ez a funkció az üzleti céljaihoz.

3. Adja meg a **projekt csapata** a szerepkörök és felelősségek meghatározása, a tagjai megadásával. Amely akkor többször további információt ismertté mérföldkő magas szintű tervének kialakításához.  

4. **Adja meg a sikerkritériumok metrikái**. Példa: ügyfél forgalom előrejelzés pontosságát X % elérése a 3 hónapos projekt végén, úgy, hogy azt kínálhat előléptetések forgalom csökkentése érdekében. A metrikák kell **INTELLIGENS**: 
   * **S**ütemezésből 
   * **M**easurable
   * **A**chievable 
   * **R**elevant 
   * **T**ime adathoz kötött 

#### <a name="12-identify-data-sources"></a>1.2 az adatforrások azonosítása
A kérdésekre adott válaszok az éles ismert példák tartalmazó adatforrások azonosítása. Keresse meg a következő adatokat:

* Adatok **érintett** a kérdésre. A cél és a cél kapcsolódó szolgáltatások rendelkezünk?
* Adatok egy **pontos** a modell cél és a lehetőségeket.

Nincs ritka, például, hogy a meglévő rendszerek összegyűjtése és oldja meg a problémát, és a projekt céljainak eléréséhez további adatfajták naplózása kell kereséséhez. Ebben az esetben érdemes lehet külső adatforrások keres, vagy frissítse a rendszer új adatok gyűjtéséért felelős ügyfélfeladatot.

### <a name="artifacts"></a>Összetevők
Ebben a szakaszban az alábbiakban a termékek esetében:

* [**A dokumentum charter**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Charter.md): szabványos sablon megtalálható a TDSP projekt struktúra definíciójának. Ez az egy élő dokumentumot folyamatosan frissített, új felderítések válnak, és üzleti követelmények változnak. A fontos, hogy ez a dokumentum részletesen, hozzáadása során a felderítési folyamat során többször. Megőrzése az ügyfél és egyéb érintett felek részt vesz a módosítások elvégzése, és egyértelműen kommunikálnak a okok miatt a módosítások.  
* [**Adatforrások**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#raw-data-sources): Ez a **nyers adatforrások** szakasza a **Adatmeghatározások** jelentést, amely megtalálható a TDSP projekt **jelentés** mappát. Meghatározza a nyers adatok eredeti és a rendeltetési helyét. Későbbi szakaszaiban ki további adatait, például az adatok áthelyezése az analitikus környezet parancsfájlok.  
* [**Adatok szótárak**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/DataDictionaries): Ez a dokumentum ismerteti az ügyfél által meghatározott adatok. A leírások információkat tartalmaznak a séma (adattípusok, információkat a ellenőrzési szabályok, ha van ilyen) és az entitás-kapcsolat diagramokat, ha elérhető.


## <a name="2-data-acquisition-and-understanding"></a>2. Adatgyűjtés és adatértelmezés

### <a name="goals"></a>Célok
* Tiszta, kiváló minőségű dataset amelynek kapcsolatok a cél változók tisztában vannak a megfelelő analytics környezetben, készen áll a modell található.
* Az adatok folyamatának frissítse és adatok pontozása rendszeresen megoldási fejlesztettek ki.

### <a name="how-to-do-it"></a>Menete
Az ebben a szakaszban leírt három fő feladat van:

* **Az adatok** a cél elemzési környezetbe.
* **Az adatokba** annak megállapításához, hogy a data quality a kérdés megválaszolásához megfelelő. 
* **Állítson be egy adatok folyamat** új vagy rendszeresen pontozása céljából frissítette az adatokat.

#### <a name="21-ingest-the-data"></a>2.1 viszi be az adatokat
Állítsa be a folyamat az adatok áthelyezése Forráshelyek analytics műveletek hova képzési célhelyeket és előrejelzéseket hajthatnak végre. Technikai részletei és a különböző Azure data services rendszerrel ehhez azon beállítások: [adatok betöltése az analytics tárolási környezeteket](machine-learning-data-science-ingest-data.md). 

#### <a name="22-explore-the-data"></a>2.2 ásna az adatokban
A modell betanításához előtt az adatok egy hang ismertetése kifejlesztésére lesz szükség. Valós adatkészletek általában zajos vagy értékek hiányzik, vagy más eltérések állomással rendelkezzenek. Adatainak összefoglalója és a képi megjelenítés segítségével az adatok minőségének naplózási, és adja meg az adatok feldolgozására, ahhoz, hogy készen áll a modellezési szükséges információkat. Ez a folyamat gyakran ismétlődő.

TDSP biztosít egy nevezik automatizált segédprogram [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) megjelenítheti az adatokat, és készítse elő az adatok összesítő jelentések segítségével. Azt javasoljuk, hogy először a kezdeti adatok ismertetése az interaktív módon nem kódolási fejlesztése és az adatok feltárása és a képi megjelenítés egyéni kód írását, majd az adatokba IDEAR kezdve. Az adatok tisztítása útmutatóért lásd: [készíti elő az adatok továbbfejlesztett feladatok gépi tanulás](machine-learning-data-science-prepare-data.md).  

Ha elégedett a kitisztítanak adatok minőségének, a következő lépéssel jobb megértése érdekében jellemzőek, amelyek segítenek az adatok mintázatokat válassza ki, és a cél egy megfelelő prediktív modellek fejlesztése. Keresse meg a bizonyító adatok számára az adatok milyen mértékben csatlakoztatva van a cél, és hogy nincs elegendő adat előre a modellezési lépések együtt mozognak. Ebben az esetben a folyamat be nem gyakran ismétlődő. Szükség lehet a pontosabb vagy több megfelelő adataival, hogy az adatkészlet kezdetben az előző szakaszban azonosított kiegészítheti az új adatforrások keresése.  

#### <a name="23-set-up-a-data-pipeline"></a>2.3 adatok adatcsatorna beállítása
A kezdeti adatfeldolgozást és az adatok tisztítása kívül általában kell állítson be olyan folyamatot, amely új adatok pontozása vagy egy folyamatban lévő tanulási folyamat részeként rendszeresen adatok frissítése. Ezt megteheti a adatok folyamat vagy munkafolyamat beállításával. Íme egy [példa](machine-learning-data-science-move-sql-azure-adf.md) , hogyan állíthat be az adatcsatorna [Azure Data Factory](https://azure.microsoft.com/services/data-factory/). 

Az adatok feldolgozási folyamat megoldási fejlesztése ebben a szakaszban. A folyamat a következő szakaszokra tudományos adatok projekt párhuzamosan is fejlesztése. A feldolgozási sor batch-alapú vagy streaming/real-egyszeri vagy lehet a hibrid attól függően, hogy az üzleti igényeknek és a meglévő rendszerek, amelybe ez a megoldás integrálva van a korlátozásait. 

### <a name="artifacts"></a>Összetevők
A következők a céljai legyenek ebben a szakaszban.

* [**Minőségi jelentés**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/DataSummaryReport.md): Ez a jelentés ezekkel az adatokat, minden egyes attribútum és a cél, közötti kapcsolatokat tartalmaz változó rangsorolási stb. A [IDEAR](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/DataReport-Utils) TDSP részét gyorsan hozhat létre, ez a jelentés például egy CSV-fájl vagy egy relációs tábla bármely táblázatos adatkészlethez megadott eszköz. 
* **Megoldás architektúrája**: Ez lehet egy diagram vagy a leírást a pontozási futtatásához használt adatok folyamat vagy az új adatok előrejelzéseket a modell létrehozása után. A kimenetátirányítási újratanítása a modellt új adatok alapján is tartalmaz. A dokumentum tárolása a [projekt](https://github.com/Azure/Azure-TDSP-ProjectTemplate/tree/master/Docs/Project) directory a TDSP directory struktúra sablon használatakor.
* **Ellenőrzőpont döntési**: a szolgáltatás teljes termékgondozó csoportja és a modell létrehozásának megkezdése előtt is újra kiértékelje a projekt meghatározásához. a várt értéket elméletben a minden esetben hasznos, a folytatáshoz elegendő. Lehet, például lehet készen áll a folytatáshoz további adatokat gyűjteni, vagy szakítsa a projekt, az adatok nem létezik a kérdésre válaszolnia kell.


## <a name="3-modeling"></a>3. Modellezés

### <a name="goals"></a>Célok
* A gépi tanulási modell optimális adatok funkciói.
* Olyan informatív ML-modell, amely a leginkább előrejelzésre a cél.
* Olyan ML-modell, amely a termelési alkalmas.

### <a name="how-to-do-it"></a>Menete
Az ebben a szakaszban leírt három fő feladat van:

* **Jellemzőkiemelés**: adatok funkciók létrehozása a modell betanítási megkönnyítése érdekében a nyers adatoktól.
* **A modell betanítási**: keresse meg az, hogy felveszi a kérdés legpontosabban a sikerkritériumokat összehasonlítva.
* Annak megállapítása, hogy a modell **éles alkalmas**.

#### <a name="31-feature-engineering"></a>3.1 jellemzőkiemelés
A szolgáltatás műszaki osztály magában foglalja a, összesítési és az átalakítási nyers változók létrehozása az elemzés során használt szolgáltatásait. Ha azt szeretné, hogy a modell Előfeltételek betekintést, akkor szüksége tudni, hogyan szolgáltatások kapcsolódnak egymáshoz, és hogyan a gépi tanulási algoritmusok ezen szolgáltatások használatát. Ez a lépés szükséges tartomány szakértelmét és az adatok feltárása lépésben beszerzett insights kreatív kombinációja. Ez a Keresés és informatív változóval együtt túl sok független változók elkerülésével egy egyensúlyt igényel. Informatív változók javítása az eredmény; független változók bevezetéséhez a modell nélküli "zajt". Is kell ezeket a szolgáltatásokat bármely pontozási során kapott új adatok létrehozásához. Ezek a funkciók generációját úgy csak adatok pontozása időpontjában függ. A szolgáltatás műszaki osztály különböző az Azure data technológiákhoz műszaki útmutató: [Jellemzőkiemelés az adatok tudományos folyamatban](machine-learning-data-science-create-features.md). 

#### <a name="32-model-training"></a>3.2 a modellek betanítása
Kérdés-válasz próbált típusától függően érhetők el számos modellezési algoritmus. Az algoritmusok kiválasztásáról útmutatóért lásd: [algoritmusok kiválasztása a Microsoft Azure Machine Learning](machine-learning-algorithm-choice.md). Bár ez a cikk az Azure Machine Learning, a nyújt útmutatást esetén gépi tanulási projektek hasznos. 

A modell betanítási folyamata a következő lépésekből áll: 

* **A bemeneti adatok** véletlenszerűen a modellezési a tanítási adathalmazt és a vizsgálati adatkészletet.
* **A modellek létrehozása** a tanítási adathalmazt használatával.
* **Értékelje ki** (tanítási és tesztelési dataset) együtt a különböző versengő gépi tanulási algoritmusok több kapcsolódó paraméterek (más néven paraméter ismétlés), amely az aktuális adatok érdeklő a kérdés megválaszolásához célközönségre hangolása.
* **Határozza meg a "ajánlott" megoldás** a kérdés megválaszolásához összehasonlítva a sikeres metrika alternatív módszerek között.

> [!NOTE]
> **Adatszivárgás megakadályozásához**: adatszivárgás oka lehet az adatok a kívül a tanítási adathalmazt, amelyet lehetővé teszi, hogy a modell vagy a gépi tanulási algoritmus irreálisan jó előrejelzéseket készítsen a felvételét. Adatszivárgás oka egy közös miért kutatók beolvasása ideg, ha csak prediktív eredmények, amelyek adatokat úgy tűnik, túl alacsony. Ezek a függőségi észleléséhez nehéz lehet. Gyakran az adatszivárgás megakadályozásához között egy elemző adatkészlet létrehozása, a modell létrehozása és pontosságának kiértékelése léptetés igényel. 
> 
> 

Azt adja meg egy [automatikus modellezéséhez és jelentéskészítési eszköz](https://github.com/Azure/Azure-TDSP-Utilities/blob/master/DataScienceUtilities/Modeling) TDSP, amely képes több algoritmusok és paraméter segítségével futtassa a modell létrehozásához el a. A jelentés minden modell és paraméter kombináció, többek között a változó fontosság teljesítményének összefoglalójához modellezési alapterv is képes. Ez a folyamat esetén is iteratív, mert azt is meghajtó további szolgáltatás mérnöki csapathoz. 

### <a name="artifacts"></a>Összetevők
Az ebben a szakaszban létrehozott összetevőket tartalmazza:

* [**Szolgáltatáskészletek**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/DataReport/Data%20Defintion.md#feature-sets): A funkciók a modellezési kifejlesztett ismerteti a **szolgáltatáskészletek** szakasza a **adatainak definíciója** jelentés. A kód a szolgáltatások és a leírást a szolgáltatás létrehozásáról a mutatók tartalmaz.
* [**A jelentés a modell**](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Model/Model%201/Model%20Report.md): minden modell, amely próbálkozik, egy szabványos, amely adatokat minden egyes kísérlet a sablon-alapú jelentés hozott létre.
* **Ellenőrzőpont döntési**: mérlegelje, hogy a modell teljesítménye elegendő telepíteni kell egy éles rendszer. Néhány kulcsfontosságú megválaszolandó kérdések a következők:
  * A modell nem elegendő a vizsgálati adatok megadott megbízhatósággal válaszolja a kérdést? 
  * Érdemes kipróbálni! bármely alternatív módszerek: további adatokat gyűjtsön, ne további szolgáltatás mérnöki csapathoz, vagy más algoritmusok kísérletezhet?


## <a name="4-deployment"></a>4. Környezet

### <a name="goal"></a>Cél
* Adatok adatcsatorna modellek egy éles vagy hasonló környezetet a végfelhasználó elfogadási is települ. 

### <a name="how-to-do-it"></a>Menete
Ebben a szakaszban tárgyalt fő feladat:

* **Azok a modell**: a modell és a kimenetátirányítási telepítése egy éles vagy hasonló környezet alkalmazás felhasználásra.

#### <a name="41-operationalize-a-model"></a>4.1-es vagy azok egy modell
Miután egy modellt a hatékony készletét, azokat is kell operationalized más alkalmazások felhasználását. Attól függően, hogy az üzleti igények előrejelzéseket valós időben vagy kötegelt alapon történik. Operationalized, hogy a modellek rendelkezik, például online webhelyek, táblázatkezelő programok, irányítópultok vagy üzleti és a háttérkiszolgáló alkalmazások különböző alkalmazásokból könnyen felhasznált nyitott API-illesztővel kell tenni. Az Azure Machine Learning webszolgáltatás a modell operationalization példákért lásd [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md). Egyben ajánlott telemetriai adatok és a figyelést az üzemi modell létrehozásához, és az adatok csővezeték telepített későbbi rendszer állapotú jelentések és hibaelhárítás érdekében.  

### <a name="artifacts"></a>Összetevők
* A rendszer állapotának és a kulcs mérőszámok irányítópult állapotát.
* Végső modellezési központi telepítésének részletei jelentés.
* Végső megoldás architektúrája dokumentum.

## <a name="5-customer-acceptance"></a>5. Felhasználói elfogadás

### <a name="goal"></a>Cél
* **A projekt termékek véglegesítése**: Ellenőrizze, hogy a folyamat, a modell és a telepítés éles környezetben amelyek ügyfél céljait.

### <a name="how-to-do-it"></a>Menete
Ebben a szakaszban tárgyalt két fő feladatok van:

* **Rendszer érvényesítési**: Ellenőrizze, hogy a telepített modell és a kimenetátirányítási teljesítésének ügyfelek igényeinek megfelelően.
* **Az aktuális ki a projekt**: az entitás számára a rendszer éles környezetben való futtatásához.

Az ügyfél ellenőrizni kell, hogy a rendszer megfelel-e az üzleti igényeknek és a válaszok az ügyfélalkalmazás által használható a kérdéseket a rendszer telepítéséhez az üzemi környezetben elfogadható pontossága. A dokumentáció fejeződik be, és tekintse át. Egy az aktuális ki a projekt műveletek felelős entitásra befejeződött. Ehhez az entitáshoz lehet, például az informatikai vagy felhasználói adatok tudományos team vagy az ügyfél, amely felelős az éles környezetben a rendszert futtató ügynök. 

### <a name="artifacts"></a>Összetevők
A fő összetevő előállított végső ebben a szakaszban a **kilépési jelentés a projekt ügyfél**. Ez az a műszaki és a rendszert, amely hasznos a projekt összes részleteit tartalmazó jelentést. Egy [kilépési jelentés](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) sablont használja, vagy az adott ügyfélnek kell testre szabott TDSP biztosítja. 

## <a name="summary"></a>Összefoglalás
A [Team adatok tudományos folyamat életciklus](http://aka.ms/datascienceprocess) többször is lépések, amely útmutatást nyújtanak a prediktív modellek használatához szükséges feladatok sorozatát van modellezve. Ezek a modellek javítható, ha intelligens alkalmazásokat szeretne összeállítani az éles környezetben is telepíthető. Folyamat életciklus célja továbbra is helyezze át a tudományos adatprojekthez előre egy tiszta engagement végpont felé. Is igaz, hogy adattudomány-e a kutatás és felderítési egy gyakorlatot, félreértés igényt egyértelműen kommunikációhoz ezeket a feladatokat a csapat és ügyfelei az alábbiakra, összetevők, amelyek az alkalmazottak szabványosított sablonok segítségével jól meghatározott elkerülése és Növelje meg az esélye, egy összetett adatokat tudományos projektet egy sikeres befejezését.

## <a name="next-steps"></a>Következő lépések
Végpont forgatókönyvek, amelyek azt mutatják, a folyamat lépései teljes **meghatározott forgatókönyvek** is rendelkezésre állnak. Szerepel a listában, és kapcsolódik a miniatűr leírásokat a [Team adatok tudományos folyamat forgatókönyvek](data-science-process-walkthroughs.md) témakör.

