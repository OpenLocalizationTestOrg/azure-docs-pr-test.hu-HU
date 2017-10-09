---
title: "aaaPredictive karbantartás miatt az Azure - Cortana Intelligence megoldássablonban űrtechnikai |} Microsoft Docs"
description: "A Microsoft a Cortana Intelligence űrtechnikai, segédprogramok és szállítására prediktív karbantartási megoldás sablon."
services: cortana-analytics
documentationcenter: 
author: fboylu
manager: jhubbard
editor: cgronlun
ms.assetid: 2e8b66db-91eb-432b-b305-6abccca25620
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: fboylu
ms.openlocfilehash: da863a89d2409a8b56b23066f15b4da13e1cd3d0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-solution-template-playbook-for-predictive-maintenance-in-aerospace-and-other-businesses"></a>A Cortana Intelligence megoldás sablon alkalmazástervezési űrtechnikai és más vállalatok prediktív karbantartás
## <a name="executive-summary"></a>Vezetői összefoglaló
A prediktív karbantartási egyike hello legtöbb igényelt költséghatékony nagy mennyiségű unarguable előnyt prediktív elemzési alkalmazásokban fognak szerepelni. Ez a forgatókönyv célja egy hivatkozást a prediktív karbantartási megoldásokat főbb használati esetet hello hangsúlyt.
Előkészített toogive hello olvasó hello leggyakoribb üzleti forgatókönyvek prediktív karbantartás, az ilyen megoldások az üzleti problémák verziójú kihívást ismeretét, az adatok szükséges toosolve üzleti problémákra prediktív modellezési Ezek az adatok és ajánlott eljárások használata minta megoldás architektúrák az technikák toobuild megoldások.
Azt is bemutatja hello rögzítésen hello prediktív modellek fejlesztett például szolgáltatás mérnöki csapathoz, fejlesztési és teljesítmény értékelés modell. Lényegében ez a forgatókönyv egyesíti hello üzleti és egy sikeres fejlesztési és üzembe helyezésére használt prediktív karbantartási megoldásokat szükséges analitikai irányelveket. Ezeket az irányelveket előkészített toohelp hello célközönség Cortana Intelligence Suite segítségével kezdeti megoldás létrehozása és a hosszú távú prediktív karbantartási stratégiai kifejezetten Azure Machine Learning egy kiindulási pont. hello Cortana Intelligence Suite és az Azure Machine Learning dokumentációs található [Cortana Analytics](http://www.microsoft.com/server-cloud/cortana-analytics-suite/overview.aspx) és [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) lapokat.

> [!TIP]
> Műszaki útmutató tooimplementing megoldás sablon, a következő témakörben: [műszaki útmutató toohello Cortana Intelligence Megoldássablonban prediktív karbantartási](cortana-analytics-technical-guide-predictive-maintenance.md).
> a diagramban, ez a sablon az architektúra áttekintése toodownload lásd: [hello Cortana Intelligence Megoldássablonban prediktív karbantartási architektúrájának](cortana-analytics-architecture-predictive-maintenance.md).
> 
> 

## <a name="playbook-overview-and-target-audience"></a>Forgatókönyv áttekintése és azon célközönség
Ez a forgatókönyv szervezett toobenefit műszaki és a nem technikai jellegű célközönség különböző hátterek és a prediktív karbantartási területen érdekében. hello alkalmazástervezési foglalkozik mindkét általános vonatkozásaira hello prediktív karbantartási megoldásokat különböző típusú, és részletesen ismerteti tooimplement őket. hello tartalom kiegyensúlyozott elemeket, mindkét toohello célközönség, akik csak a megoldás a tárhely- és hello típusú alkalmazások, valamint azok számára, akik tooimplement hasznosítani megválaszolásával ezek a megoldások, és ezért a technikai részleteket is.

Ez a forgatókönyv tartalma hello többsége nem feltételezi azt, a korábbi adatok tudományos Tudásbázis vagy szakértői. Azonban néhány hello forgatókönyv része kell némileg tudományos adatok fogalmak megvalósítás részletei követve toobe ismeretét. Szakértelem bevezető szintű adattudomány szükséges toofully juttatás hello anyagból szakaszt.

hello alkalmazástervezési első felében hello hozzá van rendelve egy bevezető toopredictive karbantartási alkalmazások, hogyan tooqualify egy prediktív karbantartási megoldás egy gyakori felhasználási gyűjteménye hello részleteit az üzleti probléma esetben hello körülvevő ezeket használja adatok esetek és hello üzleti előnyt végrehajtási a prediktív karbantartási megoldásokat. Ezek a szakaszok nincs szükség semmilyen technikai tudásszintjük hello prediktív elemzési tartományban.

Hello második felében hello forgatókönyv, a prediktív karbantartási alkalmazások és megvalósításához a hello használati esetek hello alkalmazástervezési első felében hello leírt példák ezek a modellek prediktív modellezési módszereket hello típusú azt foglalkozik. Ezt szemlélteti átnézi adatok előfeldolgozása lépésein, például az adatok címkézés és a szolgáltatás műszaki osztály, a modell kiválasztása, a képzési/tesztelés és a teljesítmény értékelési ajánlott eljárások. Ezek a szakaszok olyan technikai célközönség alkalmas.

## <a name="predictive-maintenance-in-iot"></a>Az IoT prediktív karbantartás
lehet, hogy a nem ütemezett berendezések állásidő hello hatását rendkívül ilyen beállítások mellett pusztító a vállalkozások számára. Kritikus tookeep mező berendezések fut, sorrendben toomaximize kihasználtságát és a teljesítmény szerint minimalizálja a költséges, nem tervezett leállás legyen. Egyszerűen nincs az aktuális üzleti műveletek helyszín megfizethető hello hiba toooccur vár. versenyképes tooremain vállalatok keresse meg az új módon toomaximize eszköz teljesítmény azáltal, hogy a különböző csatornákon hello adatok felhasználása során gyűjtött. Egy módja a tooanalyze Ez az információ nem tooutilize prediktív elemzési módszereket, amelyek segítségével korábbi minták toopredict jövőbeli eredményt. Ezek a megoldások legnépszerűbb hello egyik nevezik prediktív karbantartási, amely általában adható meg, de nem korlátozott toopredicting lehetőségét, hogy egy eszköz a jövőbeli úgy, hogy hello eszközöket lehet figyelni tooproactively közelében hello hiba azonosítása hibák, és hajtsa végre a megfelelő művelet előtt hello hiba történik. Ezek a megoldások hiba minták toodetermine eszközök, amely a legnagyobb veszélyt hello hiba észlelése. A korai problémákat azonosítása korlátozott karbantartási erőforrások telepítése több költséghatékony és minőségének javítása érdekében, valamint ellátási lánc folyamatok segítségével.

Hello megnövekedhet a hello az eszközök internetes hálózatát (IoT) alkalmazások, a prediktív karbantartási való növekvő figyelmet hello iparági hello adatok gyűjtését, és elég toogenerate technológiák feldolgozása van jelen a továbbítani, tárolását és elemzését az összes milyen kötegekben telepítse, vagy a valós idejű adatokat. Ilyen technológiák segítségével könnyen fejlesztési és -végpontok közötti megoldások telepítésének speciális elemzési megoldásairól, így késései hello legnagyobb előny prediktív karbantartási megoldásokat.

Üzleti problémák miatt toounexpected hibák és problémák összetett üzleti környezetekben hello okának korlátozott betekintést magas működési kockázat hello prediktív karbantartási tartomány közé. Ezek a problémák hello többsége lehet a következő üzleti kérdéseket hello kategorizált toofall:

* Mi az a hello annak valószínűségét, hogy berendezés sikertelen, a jövőben közelében hello?
* Mi az a hello hasznos élettartama hello berendezések?
* Mik azok a hello okozza hibák és karbantartási műveleteket kell elvégezni toofix a problémát?

A prediktív karbantartási tooanswer felügyelniük ezeket a kérdéseket vállalatok számára a következőket teheti:

* Működési kockázat csökkentése, és növelheti eszközök megtérülési hibák gyorsabban ahhoz, azok történt
* Csökkentse a szükségtelen időalapú karbantartási műveleteket és a karbantartási költségek szabályozásához
* Általános márka kép javítása, hibás nyilvánosságra vonatkozó és az ügyfél kopásállóság eredményül kapott elveszett értékesítési megszüntetéséhez.
* Készlet által a újrarendelési pont előrejelzésére csökkentésével alacsonyabb készlet költségek
* Minták csatlakoztatott toovarious karbantartási problémák észlelése

Prediktív karbantartási megoldásokat biztosíthat a vállalatok számára a fő teljesítménymutatók, például a health pontszámok toomonitor valós idejű eszköz feltétel, az eszközök, megelőző karbantartás tevékenységek javaslat élettartamot fennmaradó hello becsült és becsült rendelés dátumok részek cserélni.

## <a name="qualification-criteria-for-predictive-maintenance"></a>Minősítési feltételek prediktív karbantartás
Fontos, hogy nem minden esetben használja, vagy üzleti problémák hatékonyan megoldhatók prediktív karbantartási fontos tooemphasize. Fontos minősítési feltételek tartalmazza-e hello probléma prediktív ideiglenesek, esetén állapotúak előzetesen és a legfontosabb, a rendelés tooprevent hibák található egy tiszta elérési útja művelet elegendő minőségi toosupport hello adatok felhasználási eseténél érhető el. Itt azt összpontosítani sikeres prediktív karbantartási megoldás kialakításának hello követelményeit.

A prediktív modellek fejlesztéskor előzményadatokat tootrain hello modell rejtett minták majd azonosítani és további azonosíthatja ezeket a mintákat, hello jövőbeli adatok használjuk. Ezek a modellek képzett példákkal leírt szolgáltatások és előrejelzés hello célját. hello betanított modell várt toomake előrejelzéseket hello cél hello szolgáltatások hello új példák csak megtekintésével. Kritikus fontosságú, hogy hello modell rögzítési hello kapcsolatát szolgáltatásokat, és hello előrejelzés célját. Ahhoz, egy hatékony gépi tanulási a modell, igazolnia kell a betanítási adatok, amelyek ténylegesen előrejelzés jelentéssel hello adatok hello cél felé prediktív power szolgáltatást tartalmaz tootrain vonatkozó toohello előrejelzés cél tooexpect pontos kell lennie előrejelzés.

Például ha hello cél vonat kerekek toopredict hibák, betanítási adatok hello kerék szolgáltatásokkal (pl. telemetriai adat tükröző hello állapotának kerekeket, hello távolság, car terhelés, stb.) kell tartalmaznia. Azonban ha hello cél toopredict vonaton motort hibák, valószínűleg kell egy másik készlet a betanítási adatok, amelyek motor funkcióiról. Létrehozási prediktív modelleket, mielőtt azt várt hello szakértői toounderstand hello adatok alkalmazhatóságát követelmény, és adja meg, amely szükséges tooselect vonatkozó hello analitikai adatok részhalmaza hello tartomány Tudásbázis.

Nincsenek azt keresni, amikor egy üzleti probléma toobe megfelelő-e a prediktív karbantartási megoldás jogosult három alapvető adatforrások:

1. Hiba előzmények: Prediktív karbantartási alkalmazásokban, sikertelen események általában nagyon ritkán fordul elő. Azonban prediktív modelleket, amelyek megjósolható hibák fejlesztéskor hello algoritmus kell toolearn hello normál működés mintát, valamint a hello hiba mintát hello képzési folyamaton keresztül. Ezért fontos, hogy hello betanítási adatok tartalmaz elegendő számú order toolearn mindkét kategóriákban példák ezek két eltérő mintát. Éppen ezért kérjük, hogy rendelkezik-e elegendő számú hibaesemények adatokat. Sikertelen események karbantartási rögzíti és részek helyettesítő előzmények vagy rendellenességeinek hello betanítási adatok is használható hello tartomány szakértők által meghatározott hibák találhatók.
2. Karbantartási javítási előzmények: Egy prediktív karbantartási megoldásokat adatainak alapvető forrását van hello részletes karbantartási előzmények hello eszköz helyett hello összetevőivel kapcsolatos adatokat tartalmazó, megelőző karbantartás céljából aktiválja elvégezni, stb. Rendkívül fontos ezeket az eseményeket fogadhatja ezek hatása hello teljesítménycsökkenést mintákat és ezt az információt hiánya okozza félrevezető toocapture annak az eredménye.
3. Gép feltételek: A rendelés toopredict hány nap (óra, miles, tranzakciók, stb.) a gép tart előtt nem sikerül, feltételezzük, hogy adott idő alatt a működése során csökkenti hello gép állapotát. Ezért elvárjuk hello adatok toocontain idő-változó funkciók a korosodási mintát rögzítő és bármely rendellenességeket, amelyek a toodegradation vezet. Az IoT-alkalmazásokhoz a különböző érzékelők hello telemetriai adatokat egy jó példa jelölik. A sorrend toopredict Ha egy gép toofail egy időkereten belül ideális hello kell adatrögzítő megalázó trend a időtartományban tényleges hello esemény előtt.

Emellett szükséges adatokat, amelyek közvetlenül kapcsolódó működési feltételek hello target eszköz előrejelzési toohello. a cél hello döntési mindkét üzleti igényeinek, valamint az adatok rendelkezésre állását alapul. Véve hello betanítása kerék előrejelzés például azt is előrejelzése "Ha hello kerék folyamatos toohave hiba" vagy "Ha a teljes vonat hello rendszer sikertelen". hello először egy célozza több adott összetevő, míg a második érték hello vonat hiba célozza. hello második mint hello sokkal több szétszórt adatelemek elsőt, így nehezebb toobuild modell szükséges általános kérdést. Ezzel ellentétben toopredict kerék hibák próbált csak megtekintésével hello magas szintű tanítási adatokat nem lehet valósítható meg, nem tartalmaz információt hello összetevő szinten. Ez általában több ésszerű toopredict adott hibaesemények mint általánosabb.

Egy gyakori kérdést, amely általában kapcsolatba kapcsolatos hiba előzményadatok "hogyan sok hibaesemények szükséges tootrain modell, és hány minősül"elegendő"? Nem egyértelmű válasz toothat kérdése hasonlóan a prediktív elemzés számos forgatókönyv, hogy általában hello adatokat határozzák meg, hogy mi az az elfogadható hello minőségét. Ha hello adatkészlet nem tartalmazza a megfelelő toofailure előrejelzés funkciókat, majd akkor sem, ha sok sikertelen események, felépítése helyes modell nem lehet. Hello tapasztalatok azonban, hogy hello további hello sikertelen események hello jobb hello modell, hány sikertelen példák szükségesek durva becslés egy nagyon környezet és az adatok függő mérték. A probléma tárgyalt hello szakaszban imbalanced adatkészletek kezelése ahol azt javasolja, hogy módszerek toocope a következő, nem rendelkezik elegendő hibák hello probléma.

## <a name="sample-use-cases"></a>Példa használati esetek
Ez a szakasz a prediktív karbantartási használhatók a különböző iparágak például űrtechnikai, segédprogramok és szállítására gyűjteménye összpontosít. Minden alszakasz csukja be hello használati esetek ezeknek a területeknek gyűjtött és üzleti probléma, hello adatok környező hello üzleti probléma, és egy prediktív karbantartási megoldás hello előnyeit ismerteti.

### <a name="aerospace"></a>Űrtechnikai
#### <a name="use-case-1-flight-delay-and-cancellations"></a>Használati eset 1: Repülési késleltetés és a sikertelen
##### <a name="business-problem-and-data-sources"></a>*Üzleti probléma és adatforrások*
Hello jelentős üzleti problémák adott légitársaság tapasztalt egyik hello jelentős járó költségek a toomechanical problémák miatt folyamatban késleltetett járatok. Hello gépi hiba nem javítható, ha járatok még érvényteleníthető. Ez az nagyon költséges, késések ütemezés és a műveletek, az okok rossz megbízhatóságibesorolás-kezelési és a felhasználói kapcsolatos mellett sok más problémákat a problémák hozzon létre. Légitársaság különösen érdeklik gépi hibák előre előrejelzésére, így csökkenthetik a felhőszolgáltató közötti átviteléhez működés lelassulhat, vagy a sikertelen. hello hello prediktív karbantartási megoldás ezekben az esetekben célja toopredict hello valószínűségértékének inverzét repülőgép alatt álló késleltetett vagy visszavont, alapján érintett adatforrások, például karbantartási előzményekhez és a felhőszolgáltató közötti átviteléhez az útvonal-információkat. hello két fő adatforrások ebben az esetben használja a következők: hello repülési ágak és a lap naplókat. Repülési engedélyezi adatok hello repülési Útvonaladatok például hello dátuma és időpontja indító és érkezésének, indító és érkezési repülőtéren stb vonatkozó adatokat tartalmazza. Lap naplóadatokat hiba és karbantartási kódok a hello karbantartási személyzet rögzített sorozata tartalmazza.

##### <a name="business-value-of-hello-predictive-model"></a>*A prediktív modell hello üzleti értéket*
Hello elérhető előzményadatok használatával, a prediktív modell lett létrehozva egy gépi probléma, így ez a késleltetés vagy egy repülési belül hello megszakítását 24 órán át több osztályozó algoritmus toopredict hello típusú használ. Azáltal, hogy az előrejelzés, a szükséges karbantartási műveletek toomitigate hello kockázat átvihető, amíg repülőgép javítás alatt áll, és így megakadályozható a lehetséges késést és a sikertelen. Azure Machine Learning webszolgáltatás segítségével, hello prediktív modelleket zökkenőmentesen és egyszerűen integrálhatják légitársaság meglévő operációs rendszerek. 

#### <a name="use-case-2-aircraft-component-failure"></a>Használati eset 2: Repülőgép összetevők hibája esetén.
##### <a name="business-problem-and-data-sources"></a>*Üzleti probléma és adatforrások*
Repülőgép motorok nagyon érzékeny és költséges eszközökre és motor része cserékhez hello általános karbantartási feladatokat hello légitársaság iparágban között. Karbantartási megoldásokat légitársaság gondos felügyeleti összetevő készlet rendelkezésre állásának, és a tervezési igényelnek. A beruházási költségek képes toogather az eszközintelligencia összetevő megbízhatóságáról alatt részletes útmutatást toosubstantial csökkentése érdekében. hello fő adatforrás ebben az esetben használja a hello feltétellel hello repülőgép hello repülőgép olyan információkat az érzékelők számos összegyűjtött telemetrikus adatok. Karbantartási rekordok volt is használt tooidentify összetevő hibák történtek, és csere történt.

##### <a name="business-value-of-hello-predictive-model"></a>A prediktív modell hello üzleti értéket
A többszörös osztály besorolási modell lett létrehozva, amely képes a hibák valószínűségét esedékes tooa bizonyos hello következő hónap összetevőjét. Ezek a megoldások alkalmazásával légitársaság is összetevő javítási költségek csökkentése, összetevő-készlet rendelkezésre állásának javítása, csökkentse a kapcsolódó eszközök készlet szintek és javítása karbantartási tervezési.

### <a name="utilities"></a>Segédprogramok
#### <a name="use-case-1-atm-cash-dispense-failure"></a>Használati eset 1: ATM cash eredményeként hiba
##### <a name="business-problem-and-data-sources"></a>*Üzleti probléma és adatforrások*
Azok a vezetők eszköz intenzív iparágakban gyakran állapot, hogy elsődleges működési kockázat tootheir vállalatok számára az eszközeik váratlan meghibásodások esetén. Tegyük fel gépek adatközponthoz például banki iparági hibája nagyon gyakori probléma gyakran előfordul. Ilyen jellegű problémák készítése prediktív karbantartási megoldásokat nagyon hasznos az operátorok ilyen gépek. A használati eset, az előrejelzési probléma a toocalculate hello valószínűsége, hogy egy ATM cash visszavonása tranzakció megszakadt, lekérdezi miatt tooa hiba hello cash adagoló például egy dokumentum a probléma megoldásáig a vagy egy része sikertelen volt. Ebben az esetben a fő adatforrások mérések összegyűjtése közben cash megjegyzések anyagot mérnek érzékelő szivattyútelepek érzékelőinek adatai, és karbantartási rekordok összegyűjtött adott idő alatt. Érzékelőadatok érzékelő értékek egyes tranzakciónként befejeződött, és minden mellőzni Megjegyzés / érzékelő értékek tartalmazza. hello érzékelő megadott értékek mérések vastagsága, megjegyzések között lévő hézagokat például vegye figyelembe, hogy érkezési távolság stb. Karbantartási hibakódok és javítási információkat tartalmazza. E két használt tooidentify hiba esetekben.

##### <a name="business-value-of-hello-predictive-model"></a>*A prediktív modell hello üzleti értéket*
Két prediktív modelleket hello cash visszavonása tranzakciók toopredict hibáinak és hello egyedi kiegészítő jelentési tranzakció során hibák lett létrehozva. Mivel képes toopredict tranzakció hibák előzetesen, adatközponthoz is lehet szervizelt proaktív tooprevent hibák előfordulását. Is a Megjegyzés előrejelzés, valószínűleg egy tranzakcióban esetén toofail miatt tooa Megjegyzés befejeződése előtt a hiba eredményeként, előfordulhat, hogy ajánlott toostop hello folyamat, és hello ügyfél hiányos tranzakció helyett hello karbantartási kiszolgálásra várakozó figyelmeztetés Miután hello hiba akkor fordul elő, ami azt eredményezheti, toolarger ügyfél kapcsolatos tooarrive.

#### <a name="use-case-2-wind-turbine-failures"></a>Használati eset 2: Szél turbinás hibák
##### <a name="business-problem-and-data-sources"></a>*Üzleti probléma és adatforrások*
Szél turbinák váltak hello fő forrásokból energia generációs hello megjelenítése a környezeti tájékoztatási, és általában költségeket millióira dolláros. Sok érzékelők, amelyek segítségével toomonitor turbinás feltételek és állapot hello generátor motor, amely rendelkezik a fő összetevői hello szél turbinák jelenti. hello érzékelő értékek értékes információkat tartalmaz a prediktív modell toopredict használt toobuild lesz kritikus kulcs teljesítménymutatókat (KPI) például középidő toofailure hello szél turbinás-összetevőihez. Ebben az esetben használja az adatok több szél turbinák három különböző farm helyeken található származik. Mérték zárja be az egyes turbinás száz érzékelők rögzítve 10 másodpercenként egy évig tooa. Ezek az értékek közé tartozik például hőmérséklet, generátor sebességétől, turbinás power és generátor felszámolására mérések.

##### <a name="business-value-of-hello-predictive-model"></a>*A prediktív modell hello üzleti értéket*
A prediktív modellek tooestimate fennmaradó élettartama generátorokat és hőmérséklet-érzékelő lett létrehozva. Által hiba, karbantartási technikusok összpontosíthatnak, amelyek hamarosan valószínűleg toofail gyanús turbinák hello valószínűségét toocomplement időalapú karbantartási rendszerek becslése.
Emellett a prediktív modellek kapcsolja a insight toohello szint hozzájárulás különböző tényezők toohello valószínűségét, a hiba, amely segít az üzleti toohave hello legfelső szintű jobb megértése problémákat okozhat.

#### <a name="use-case-3-circuit-breaker-failures"></a>Használati eset 3: áramköri megszakító hibák
##### <a name="business-problem-and-data-sources"></a>*Üzleti probléma és adatforrások*
Generációs, terjesztési és elektromos energia pénztári elektromosság kialakulása, a gáz műveletek igényelnek, hogy kiemelt sorok működési minden karbantartási jelentős mennyiségű alkalommal fordult elő az energia toohouseholds tooguarantee kézbesítését. Az ilyen műveletek hibája kritikus, szinte minden entitás power problémák jelentkeznek hello régiókban függvénye. Kör szóhatároló kritikusak ilyen műveletekhez szerint berendezés, amely esetén a problémák és rövid Kapcsolatcsoportok tooprevent elektromos áram Kivágás bármely sérülést toopower sorok le. Ebben az esetben használja az üzleti probléma a karbantartási naplókat, a parancselőzményekben és a műszaki adatok toopredict áramköri megszakító hibák.

Ebben az esetben három fő adatforrások karbantartási naplók megoldani a problémát, megelőző és rendszeres műveletek tartalmazó, automatikus és manuális parancsok tartalmazó működési adatok küldése toocircuit szóhatároló, mint a Megnyitás és Bezárás műveletek és a technikai minden áramköri megszakító például év, a helyet, a modell, a stb tulajdonságainak Specification adatait.

##### <a name="business-value-of-hello-predictive-model"></a>*A prediktív modell hello üzleti értéket*
Prediktív karbantartási megoldásokat javítási költségek csökkentése és eszközökre, például a kapcsolatcsoport szóhatároló hello életciklusát növelése érdekében. Ezek a modellek javításához is hozzájárulhat a hello power hálózati hello minőségének mivel modellek biztosítják időben figyelmeztetések, amelyek csökkentik a váratlan meghibásodások esetén, amelyek toofewer megszakításokat toohello szolgáltatás.

#### <a name="use-case-4-elevator-door-failures"></a>Használati eset 4: Foglalhatja ajtó hibák
##### <a name="business-problem-and-data-sources"></a>*Üzleti probléma és adatforrások*
Legtöbb nagy foglalhatja a vállalatok általában még több millió hello world körül fut felvonók. toogain versenyképes él, azok összpontosítani megbízhatóságot, amely legtöbb tootheir ügyfél fontos információk. Rajz hello rejlő hello az eszközök internetes hálózatát, a csatlakozás a felvonók toohello felhő, és adatokat gyűjt foglalhatja érzékelők és rendszerek, azok a értékes üzleti intelligencia, ami jelentősen javítja a műveleteket a képes tootransform adatok az ajánlat preemptive és prediktív karbantartási, amely nincs valami, amely elérhető toohello versenytársak még. Ebben az esetben hello követelményeit egy Tudásbázis prediktív alkalmazás, amely képes hello lehetséges okai ajtó hibák tooprovide. hello szükséges három részből foglalhatja statikus szolgáltatások (pl. azonosítók szerződést kötöttek karbantartás gyakorisága, épület típusát stb.), amelyek használati adatait (pl. száma ajtó ciklusokat, átlagos ajtajának bezárása idő stb.) áll ez a megvalósítás adatait és Hiba (azaz korábbi hiba rögzíti és okaik) előzményeit.

Multiclass logisztikai regresszió modell Azure Machine Learning toosolve hello előrejelzés probléma lett létrehozva, hello integrált statikus szolgáltatásokat és funkciókat, valamint a korábbi hiba rekordok osztály feliratok hello okok használati adatok. A prediktív modell felhasznált mező szakemberek által használt mobileszközökön az alkalmazás által toohelp működő hatékonyság javítása. Hely toorepair egy foglalhatja állapotba technikus, ha többé toothis app ajánlott okok és a karbantartási műveleteket toofix hello foglalhatja ajtók lassabbak, a lehető legjobb tanfolyamokat hivatkozhat.

### <a name="transportation-and-logistics"></a>Szállítási és logisztikai
#### <a name="use-case-1-brake-disc-failures"></a>Használati eset 1: Berendezés lemez hibák
##### <a name="business-problem-and-data-sources"></a>*Üzleti probléma és adatforrások*
Tipikus karbantartási házirendeket járművekről gyűjtött korrekciós és megelőző karbantartás tartalmazza. Javítási karbantartási azt jelenti, hogy hello vehicle javítása után hiba történt, amely eredményeként egy egy látogasson el toomechanic a kihasználatlan váratlan meghibásodása és hello ideje súlyos kellemetlenségért toohello illesztőprogram okozhat. A legtöbb járművekről gyűjtött egyaránt tulajdonos tooa megelőző karbantartás céljából-házirendet, amely egy ütemezést, amely nem veszi figyelembe hello tényleges feltétel hello car alrendszerek bizonyos vizsgáló igényel. Ezek a módszerek sem sikeres teljes mértékben kiküszöbölhetők azon problémák. Itt hello konkrét használati eset lemez előrejelzés berendezés hello kulcsszava rendszerben egy autó, amely nyomon követi a korábbi vezetői minták érzékelők keresztül gyűjtött adatok alapján, és más feltételeket, amelyek a hello car van kitéve. hello ebben az esetben legfontosabb adatforrás hello érzékelőadatait mérő, például gyorsítás, fékezés kombinációját, befolyásoló tényezők távolság, sebességet, stb. Ezt az információt, amelyhez statikus ezeket az adatokat car funkciók, például súgó build jó előre használható prediktív modellben készlete. Egy másik készlet lényeges információk hello hiba adat, amely erről a rész rendelés adatbázis (használt tookeep hello tartalék rész rendelési és a mennyiségek autók hello kereskedők javítás vannak alatt).

##### <a name="business-value-of-hello-predictive-model"></a>*A prediktív modell hello üzleti értéket*
hello üzleti egy prediktív megközelítés értéke jelentős. A prediktív karbantartási rendszer ütemezhet egy látogasson el toohello viszonteladó a prediktív modell alapján. hello modell van hello hello car és ki irányítja az előzmények hello aktuális állapotát jelző csökkent érzékelő adatokat is alapulhat. Ez a megközelítés minimalizálja a váratlan meghibásodások esetén, valamint esetleg bekövetkező hello következő rendszeres karbantartás előtt hello kockázatát.
Felesleges megelőző karbantartás hello mennyisége is csökkentheti. Illesztőprogram is proaktív folyamatosan tisztában lehet azzal, hogy a változás kijelzők szükség lehet néhány héttel és ellátási hello viszonteladó ezzel az információval. előre hello viszonteladó majd sikerült előkészíteni az egyes karbantartása csomag hello illesztőprogram.

#### <a name="use-case-2-subway-train-door-failures"></a>Használati eset 2: Subway vonat ajtó hibák
##### <a name="business-problem-and-data-sources"></a>*Üzleti probléma és adatforrások*
Hello fő oka késést és a problémák subway műveletek egyike vonat autók ajtó hibák. Rendkívül fontos előrelátás előrejelzésére, ha egy vonat autó előfordulhat, hogy ajtó hiba vagy képes tooforecast hello száma nap vége: hello következő door hiba. Hello lehetőség toooptimize vonat ajtó karbantartási biztosít, és csökkentheti a hello szerelvény állásidő.

#### <a name="data-sources"></a>Adatforrások
A használati eset a három adatforrások 

* **eseményadatok betanítása**, vagyis hello előzményrekordjaira vonat események 
* **karbantartási** karbantartási, munkahelyi rendelés és rugalmasságtípusokat, valamint prioritás kódok, például  
* **rögzíti a hibákat**.

##### <a name="business-value-of-hello-predictive-model"></a>*A prediktív modell hello üzleti értéket*
Két modell toopredict következő nap hiba valószínűség bináris osztályozás és nap vége: regressziós használatával hiba segítségével lett létrehozva. Hello hasonló korábbi esetben hello modellek létrehozása rengeteg lehetőséget tooimprove szolgáltatásminőség és ügyfelek elégedettségének növelése hozzájárul hello rendszeres karbantartás rendszerek.

## <a name="data-preparation"></a>Adatok előkészítése
### <a name="data-sources"></a>Adatforrások
közös adatelemek hello a prediktív karbantartási problémákat a következőképpen összegezhető:

* Hiba előzmények: hello egy számítógép vagy hello gépekről összetevő hibája előzményeit.
* Karbantartási napló: a gép, pl. hibakódok, előző karbantartási tevékenységek vagy összetevő cserékhez javítási előzmények hello.
* A gép feltételek és a használati: hello üzemi körülmények között a gépek pl. adatok összegyűjtése az érzékelők.
* Számítógép-szolgáltatások: hello szolgáltatások gép, pl. motor mérete, gyártmány, és a modell, helyét.
* Operátor szolgáltatások: hello szolgáltatások hello operátor, például a nemét, az elmúlt élmény.

Ez nem lehetséges, és általában hello eset, hogy hiba előzmények karbantartási előzmények például különleges hibakódok vagy tartalék részek rendelés dátumai hello űrlapján tartalmazza. Ezekben az esetekben hibák hello karbantartási adatok történő kinyerésének. Emellett különböző üzleti tartományok rendelkezhetnek más adatforrások hiba mintákat, amelyek nem szerepelnek a listán itt kimerítően befolyásoló különböző. Ezek hello tartomány szakértői tanácsadás, amikor a prediktív modellek kell azonosítani.

Néhány példa a fenti adatelemek a használati esetek a következők:

Hiba előzmények: késleltetés dátumok, repülőgép összetevő hibája dátumok és típusok, ATM cash visszavonása tranzakció sikertelen, vonat/foglalhatja ajtó hibák, berendezés lemez helyettesítő rendelés dátumok, szél turbinás hiba dátumok és áramköri megszakító parancs hibák küzdelemre.

Karbantartási előzmények: repülési hibanaplókat, ATM tranzakció hibanaplókat, például karbantartási típusa, rövid leírás stb és áramköri megszakító karbantartási rekordok karbantartási rekordok betanításához.

A gép feltételek és a használati: repülési továbbítja, és időpontokat, repülőgép motorok, érzékelő értékek ATM tranzakciókból származó érzékelő adatokat betanítása események adatai, a szél turbinák, anyag- és csatlakoztatott autók érzékelő értékek.

Számítógép-szolgáltatások: áramköri megszakító műszaki specifikációk például feszültség szintek, ellenőrizze, a model, a motor például földrajzi hely meghatározásának vagy car funkciót mérete, kulcsszava típusok, éles létesítmény stb.

Hello fenti adatforrások, hello két fő adattípusok prediktív karbantartási tartomány azt láthatja a következők historikus és statikus adatok.
Hiba az előzmények gép feltételek javítása előzmények, használati esetek többségében származnak-időbélyeggel ellátott utaló minden adat hello gyűjtemény idejét. Gép operátor szolgáltatásokkal és általában statikusak általában leírják hello a műszaki adatok a gépeket vagy üzemeltető óta. Ezek a funkciók toochange lehetővé felett ideje, és ha így kezelni, időbélyegzővel adatforrások.

### <a name="merging-data-sources"></a>Az egyesítés adatforrások
Szolgáltatás műszaki osztály vagy szalagcímkézési folyamat bármilyen típusú beolvasása, előtt igazolnia kell toofirst készítse elő az adatok a hello tárgyhoz szükséges alakú toocreate szolgáltatásait. hello végleges célja egy olyan rekordot minden eszköz és a szolgáltatások és a feliratok toobe idő tárolóegységekhez táplált be hello gépi tanulási algoritmus toogenerate. A sorrend tooprepare, amely végső adatkészlet törlése néhány előre feldolgozásra lépéseket kell tenni. Első lépés toodivide hello időtartama, ahol minden rekordot tartozik az eszközhöz tartozó tooa időegységét időegységek adatok gyűjteményét. Adatgyűjtés is lehet osztani műveletek, például más egységek azonban az egyszerűség kedvéért hello magyarázatokat hello többi használjuk az egységeket.

hello mérési egysége idő másodpercben, perc, óra, nap, hónap, ciklusok, miles vagy tranzakciók hello hatékonyságát függően az adatok előkészítése és egyéb vagy más hello feltételek hello eszköz egy alkalommal egység toohello a megfigyelt hello változások lehet tényezők adott toohello tartomány. Más szóval hello időegység nincs hello ugyanaz, mint sok esetben adatok adatgyűjtés hello gyakorisága előfordulhat, hogy nem látható a több egység toohello eltérést más toobe. Például ha hőmérséklet értékek 10 másodpercenként volt gyűjtenek, egy 10 másodperces hello teljes elemzés időegységét kiadási inflates példák hello száma további adatok megadása nélkül. Jobb stratégia toouse átlagos lehet akár egy óráig példaként.

Például általános sémák hello lehetséges adatforrások, tekintse meg a hello korábbi szakaszában vannak:

Karbantartási rekordok: ezek a hello rekordok karbantartási műveleteket végrehajtani. hello nyers időbélyeg karbantartási tevékenységeket adatait tartalmazó elvégezte akkori és karbantartási általában tartalmaz egy eszköz azonosítója. Ilyen nyers adatok esetén karbantartási tevékenységek kell toobe kategorikus oszlopok lefordítani az egyes kategória megfelelő tooa karbantartási művelet típusa. Alapszintű Adatséma hello karbantartási rekordok eszköz azonosítója, az idő és a karbantartási művelet oszlopot tartalmazhat.

Hiba a rekordok: ezek a hibák vagy a hiba oka telepítésnek előrejelzés toohello célját tartozó hello rögzíti. Ezek lehetnek egyes hibakódok vagy hibák által meghatározott üzleti feltétel meghatározott események. Bizonyos esetekben az adatok, amelyek megfelelnek a érdeklő toofailures több hibakódok közé tartoznak. Nem minden olyan előrejelzési úgy, hogy más hibák általában célját is összefüggéseket tooconstruct funkciókra használt hibákkal. Alapszintű Adatséma hello sikertelen műveletet tartalmazhat eszköz azonosítója, az idő és a hiba vagy a hiba oszlopok Ha OK érhető el.

Számítógép-feltételek: ezek a lehetőleg valós idejű működési feltételek hello adatok hello adatainak. Például ajtó hibák ajtó megnyitása és záró idő is hello aktuális feltételről ajtók helyes mutatók. Alapszintű Adatséma hello gép feltételek eszköz azonosítója, időt és az állapot értéke oszlopot tartalmazhat.

Számítógép-és operátor: számítógép-és operátor egyesíthető egy séma tooidentify melyik eszköz mely tulajdonságok eszköz és az operátorral együtt üzemeltető azon műveleteket végezni. Például egy autó általában tulajdonában van egy illesztőprogram attribútumokkal kora, például befolyásoló tényezők élmény stb. Ha ezek az adatok idővel változik, az adatok is tartalmaznia kell egy time oszlopa, és adatokat különböző szolgáltatás generációs idő szerint kell kezelni. Alapszintű Adatséma hello gép feltételek eszköz azonosítója, eszköz, operátor azonosító operátor szolgáltatásokkal és tartalmazhat.

gép feltételek tábla hiba a rekordok eszköz azonosítója és időt tartalmazó mezőiben az összekötő balra válthat ki hello végső tábla címkézés és a szolgáltatás létrehozása előtt. Ez a táblázat majd lehetne illeszteni eszköz azonosítója karbantartási rekord és az időt mezők, és végül gép és az operátorral együtt szolgáltatásokat biztosító eszköz azonosítója. hello első bal oldali illesztés elhagyja hiba oszlop null értékeket számítógép esetén a normál működés, normál művelet egy mutató értékének imputálni kell ezeket. A hiba oszlop használt toocreate címkék hello prediktív modell legyen.

### <a name="feature-engineering"></a>Jellemzőkiemelés
hello modellezési első lépése a szolgáltatás műszaki osztály. hello szolgáltatás generációs lényege, tooconceptually írják le, és a készülék állapota feltétel absztrakt toothat pont be időben összegyűjtött előzményadatok használatával egy adott időpontban. Hello a következő szakaszban a prediktív karbantartási és hogyan hello címkézés történik a módszer használható technikák hello típusú áttekintést nyújtunk. hello pontos technika, amely használandó hello adatok és az üzleti probléma függ. Azonban hello szolgáltatás mérnöki az alábbiakban metódus használható alapjául a szolgáltatások létrehozásához. Az alábbi arról lesz szó lag funkciókra adatforrásokból a időbélyegeket és statikus adatforrásokból létrehozott statikus szolgáltatások is, és adja meg a hello említett példákat követik használati esetekben kell kialakítani.

#### <a name="lag-features"></a>Lag szolgáltatások
A korábbiak a prediktív karbantartás előzményadatokat általában tartalmaz minden adat hello gyűjtemény idejét jelző időbélyeg helyi időre. Számos módon szolgáltatások létrehozásának időbélyegzővel adatokkal hello adatokból. Ebben a szakaszban arról lesz szó a prediktív karbantartási használt módszerek közül. Azonban hogy nincs korlátozva ezeket a módszereket. Szolgáltatás mérnöki prediktív modellezési az egyik leginkább kreatív területek hello toobe számít, mivel számos módon toocreate szolgáltatást lehet. Itt nyújtunk néhány általános technikákat.

##### <a name="rolling-aggregates"></a>*Működés közbeni összesítések*
Az eszköz egyes rekordokhoz azt ablakot a működés közbeni méretű "W", amely hello szeretnénk toocompute korábbi összesíti az idő egységek száma. Majd számítási működés közbeni hello W időszakok hello dátum előtt, hogy a rekord összesített funkciók. Működés közbeni összesítések példákat is működés közbeni, számát, azt jelenti, szórás szorzataként, a szórás szorzataként alapján kiugró, CUSUM méri, időszak minimális és maximális értékeket. Egy másik érdekes módszer akkor toocapture trend, teljesítményt és szint módosítása algoritmusokkal, amely adatokat, és anomáliadetektálási észlelési algoritmusokat rendellenességek észlelését.

Bemutató, lásd: 1. ábra ahol határoz meg azt az egyes Munkaegységek idő az adott eszköz számára rögzített érzékelő értékek hello kék sorok, és jelölje be a működés közbeni átlagos szolgáltatás számítását W = 3 hello rekordok t hello<sub>1</sub> és t<sub>2 </sub> csoportosításokat rendre zöld és narancssárga jelzi.

![1. ábra. Működés közbeni összesített szolgáltatások](media/cortana-analytics-playbook-predictive-maintenance/rolling-aggregate-features.png)

1. ábra. Működés közbeni összesített szolgáltatások

Példaként, repülőgép összetevő meghibásodása esetén érzékelő értékeket az előző hét, az elmúlt három napban és utolsó napján volt a működés közbeni azt jelenti, szórás és sum szolgáltatások használt toocreate. Hasonlóképpen a ATM hibák, a nyers érzékelő értékek és a működés közbeni azt jelenti, középérték, a tartomány, a szórás szorzataként használták kiugró túl három szórás szorzataként, a felső és alsó CUMSUM funkciók száma.

Repülési késleltetés előrejelzés számát is az előző hét hibakódok használt toocreate szolgáltatások volt. Vonat ajtó hibák számát is hello hello események utolsó nap száma események keresztül hello előző 2 hét és számát is események hello az elmúlt 15 napban varianciáját használt toocreate lag szolgáltatásokat. Azonos számbavételi karbantartási-események lett megadva.

Emellett W válassza háttérszínnek által megadott nagyon nagy (pl.. év), akkor előfordulhat, hello teljes előzmények egy eszköz, például a leltározási minden karbantartási toolook rögzíti, hiba stb. amíg hello rekord hello ideje. Ez a módszer utolsó három év hello áramköri megszakító sikertelen leltárhoz lett megadva. Is vonat hibák, az összes karbantartási események számlált toocreate egy szolgáltatás toocapture hello hosszú távú karbantartási hatással.

##### <a name="tumbling-aggregates"></a>*Átfedésmentes összesítések*
Minden egyes referenciakonfigurációjához címkézett rekord, azt válasszon egy ablak méretének "W -<sub>k</sub>" k esetén hello számát vagy a windows, hogy szeretnénk toocreate méretű "W" lag funkciói. "-k" egy nagy számú toocapture hosszú távú teljesítménycsökkenést minták vagy egy kis számú toocapture rövid távú hatások is tárolható. Használjuk majd k átfedésmentes windows W -<sub>k</sub> , W -<sub>(k-1)</sub>,..., W -<sub>2</sub> , W -<sub>1</sub> toocreate összesített szolgáltatások hello időszakokra hello rekord előtt dátum és idő (lásd a 2. ábra). Ezek is állapotmodelljeiig windows hello rekord szinten egy időegységet, amely nem rögzíti a 2. ábrán, de hello ötlet hello ugyanaz, mint 1. ábra ahol t<sub>2</sub> is használt toodemonstrate hello folyamatban van hatással.

![2. ábra Összesített szolgáltatások átfedésmentes](media/cortana-analytics-playbook-predictive-maintenance/tumbling-aggregate-features.png)

2. ábra Összesített szolgáltatások átfedésmentes

Tegyük fel a szél turbinák, L = 1 és k = 3 hónapon használt toocreate lag szolgáltatások volt az egyes hello segítségével felső és alsó kiugró utolsó 3 hónapban.

#### <a name="static-features"></a>Statikus szolgáltatások
Ezek a műszaki adatok a hello eszközökre, például a gyártás dátum modellszámát, hely, stb. Míg lag szolgáltatások többnyire numerikus ideiglenesek, statikus szolgáltatás általában válik kategorikus változók hello modellekben. Egy példa, áramköri megszakító tulajdonságait, például feszültség, aktuális és power specifikációk átalakító típusok együtt az áramforrásokból stb használták. Berendezés lemez hibák mintha azok készült vagy acélból használt néhány statikus hello szolgáltatást, hello kulcsszava kerekek ilyen típusú.

Szolgáltatás létrehozása során például a hiányzó értékeket vagy normalizálási kezelése néhány más fontos lépést kell végrehajtani. Számos módon hiányzó érték imputálási, és adatok normalizálási, amelyek itt nem esik szó. Azonban előnyös tootry különböző módszereket toosee Ha előrejelzés teljesítmény növekedését lehetséges.

hello végső szolgáltatás táblában után a szolgáltatás műszaki osztály hello ismertetett lépéseket korábbi szakaszában időegység esetén egy nap a következő példa adatkulcsokat hello kell hasonlítania:

| Eszköz azonosítója | Time | A szolgáltatás oszlopok | Címke |
| --- | --- | --- | --- |
| 1 |Naponta 1 | | |
| 1 |2 nap | | |
| ... |... | | |
| 2 |Naponta 1 | | |
| 2 |2 nap | | |
| ... |... | | |

## <a name="modeling-techniques"></a>Modellezési módszereket
A prediktív karbantartási gyakran alkalmazó üzleti kérdéseket, amely előfordulhat, hogy a hello prediktív modellezési szempontjából a számos különböző szögek kell válaszadásra nagyon hatékony tartomány. A következő szakaszokban hello nyújtunk használt toomodel különböző üzleti kérdéseket is prediktív karbantartási megoldás fő technikákat. Habár az Hasonlóságok, minden modellnek eltérően összeállításával címke, amely részletesen ismerteti. Kísérő erőforrásként olvassa el a toohello prediktív karbantartási sablon hello mintakísérleteket Azure Machine Learning belül található. hello hivatkozások toohello online anyagot sablon hello források szakaszában szerepelnek. Láthatja, hogyan néhány hello szolgáltatás műszaki osztály kapcsolatban a fentiekben ismertetett technikák és hello következő szakaszokban ismertetett eljárás modellezési hello alkalmazza a rendszer toopredict repülőgép motor Azure Machine Learning segítségével hibák.

### <a name="binary-classification-for-predictive-maintenance"></a>A prediktív karbantartási bináris osztályozás
Bináris osztályozási prediktív karbantartási használt toopredict hello annak valószínűségét, hogy a készülék nem sikerül egy jövőbeli időtartamon belül. hello időszak határozza meg, és az üzleti szabályok és az elvégzendő hello adatok alapján. Néhány gyakori időszakokra minimális átfutási szükséges idő toopurchase tartalék részek tooreplace valószínűleg toodamage összetevők vagy idő szükséges toodeploy karbantartási erőforrások tooperform karbantartási rutinok toofix hello probléma, amely valószínűleg toooccur belül, amely adott időszakban. A jövőbeli horizon időszak "X" nevezzük.

Rendelés toouse bináris osztályozás igazolnia kell a pozitív és negatív telepítésnek példákat kétféle tooidentify. Minden egyes példája egy rekordot, amely tooa időegység fogalmilag leíró, és annak időegység toothat keresztül szolgáltatás műszaki osztály korábban leírt korábbi és más adatforrások használata be üzemi körülmények absztrakt eszköz tartozik. A prediktív karbantartási bináris osztályozási hello környezetében pozitív típus jelent a hibák (címke 1), és negatív típus jelent a normál működés (címke = 0) ahol feliratai kategorikus típusú. hello célja toofind modell, amely azonosítja az egyes új példa valószínűleg toofail, vagy tovább X időegységekkel hello belül megfelelően működik.

#### <a name="label-construction"></a>Címke létrehozása
Rendelés toocreate egy prediktív modellt tooanswer hello kérdés "Mi az hello annak valószínűségét, hogy eszköz meghiúsul a következő X időegységekkel hello hello?", a címkézés véve X rekordok előzetes toohello sikertelen volt-e az eszköz és a címkézés őket, "kapcsolatos toofail" végezhető el (címke = 1) "szokásos" módon más rekordok címkézés közben (címke = 0). Ezzel a módszerrel a címkék (lásd a 3. ábra) kategorikus változók.

![3. ábra. A bináris osztályozási címkézés](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-binary-classification.png)

3. ábra. A bináris osztályozási címkézés

Repülési késést és a sikertelen, az X nek hello toopredict késést egy nap mint 24 órán át. Hibák előtt 24 órán belül minden repülőútra 1s lett címkézve. ATM cash eredményeként hibák, két bináris besorolási modell lett létrehozva a toopredict hello hiba valószínűség hello a tranzakció 10 percig, ezért is toopredict hello valószínűségértékének inverzét hiba történt a hello mellőzni következő 100 megjegyzések. Összes tranzakció, és ismételje meg az utolsó 10 percet hello hiba hello belül vannak címkézve hello első modell 1. És minden megjegyzések belül hello mellőzni utolsó hiba 100 megjegyzések hello második modell 1 lett címkézve. Az áramköri megszakító hibák hello feladata toopredict hello annak valószínűségét, hogy a következő áramköri megszakító parancs hello sikertelen ebben az esetben X toobe egy jövőbeli parancsot választja. Hello bináris osztályozási modell vonat ajtó hibák, készült toopredict hibák belül hello következő 7 nap. X szél turbinás hibák, mint 3 hónapos választott.

Szél turbinás és a vonat ajtó esetekben regressziós elemzés toopredict fennmaradó élettartama használatával hello ugyanazokat az adatokat, de egy másik szalagcímkézési stratégia hello a következő szakaszban ismertetett felügyelniük is használhatók.

### <a name="regression-for-predictive-maintenance"></a>A prediktív karbantartási regressziós
Prediktív karbantartási regressziós modell használt toocompute hasznos élettartama (Szabályainak) egy eszköz, amelyeket a hello, hogy mennyi ideig eszköz hello működőképességét, mielőtt hello a következő hiba történik. Ugyanaz, mint a bináris osztályozási, minden egyes példa: egy rekordot, amely tooa időegység "Y" eszköz tartozik. Azonban a regressziós hello környezetében hello célja toofind modell, amely kiszámítja a fennmaradó élettartamuk minden új példa egy folyamatos számot, amely hello meghibásodása előtt hátralévő idő: hello időtartama hello. Ebben az időszakban bizonyos számú többszöröse az Y hívása. Minden példa is rendelkeznek, amelyek számítható ki, hogy például hello következő meghibásodása előtt hátralévő idő hello mennyisége mérésével fennmaradó élettartama.

#### <a name="label-construction"></a>Címke létrehozása
A megadott hello kérdéssel "milyen hello hasznos élettartama hello berendezések?
", felirataihoz, ha a hello regressziós modell minden rekordot előzetes toohello hibát, és hány időegységekkel maradnak hello következő meghibásodása előtt kiszámításával címkézés őket lehet létrehozni. Ezzel a módszerrel a feliratai folyamatos változók (lásd a 4. ábra).

![4. ábra. A regresszió címkézés](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-regression.png)

4. ábra. A regresszió címkézés

Eltér a bináris osztályozási regressziós, az eszközök hello adatok hibák nélkül nem használható modellezési címkézés hivatkozási tooa hiba pont végezheti el, és a számítást nincs lehetőség anélkül, hogy tudnák megélték előtt mennyi ideig hello eszköz Hiba történt. Ezzel a problémával legjobb túlélési Analysis nevű másik statisztikai technikával.
Nem fogjuk toodiscuss túlélési elemzés miatt lehetséges komplikációk hello esetleg felmerülő hello technika toopredictive karbantartási alkalmazásakor a forgatókönyv az esetekben, például az idő-változó adatok rendszeres időközönként használja.

### <a name="multi-class-classification-for-predictive-maintenance"></a>A prediktív karbantartási besorolást több osztály
Több osztály besorolás prediktív karbantartási segítségével előre jelezni jövőbeli két eredményt. hello először egyik tooassign egy eszköz tooone hello az idő toogive idő toofailure minden eszköz számos több lehetséges időszakok. hello második egy későbbi időszak kellő hibák valószínűségét tooidentify hello a hello tooone több okait. Amely lehetővé teszi, hogy a karbantartási személyzet hello problémák kezeléséhez előre a Tudásbázis rendelkeznek. Egy másik osztály több modellezési technika összpontosít hello legvalószínűbb okának meghatározása egy adott hibát. Ez lehetővé teszi a javaslatok toobe hello felső karbantartási műveletek toobe toofix sorrendben hajtja végre, a hiba megadott.
Azzal, hogy a legfelső szintű rangsorolt listáját hatására és a kapcsolódó javítási műveletek  
lehet, hogy hatékonyabb hiba után az első javítási műveletek végrehajtása a technikusok.

#### <a name="label-construction"></a>Címke létrehozása
Hello két kérdéseket, amelyek adott "Újdonságok hello annak valószínűségét, hogy egy eszköz nem sikerül egységekben hello következő"aZ"idő"a"hello időszakok számát az" és "hello annak valószínűségét, hogy eszköz hello Újdonságok sikertelen a következő X időegységekkel hello esedékes tooproblem" P<sub>i </sub>"hol"i"hello számos ok kiválthatja, címkézés van végezheti el a tootechniques módot kínál a következő hello.

Az első kérdés hello, címkézés végezhető el aZ rekordok eszköz hello meghibásodás előtt véve, és a címkézés őket gyűjtő idő (3Z, 2Z, Z) használatával a feliratok "szokásos" módon más rekordok címkézés közben (címke = 0). Ezzel a módszerrel a címkéje kategorikus változó (lásd a 5. ábra).

![5. ábra. Idő előrejelzés multiclass besorolási címkék](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-failure-time-prediction.png)

5. ábra. Idő előrejelzés multiclass besorolási címkék

Az hello második kérdést, címkézés végezhető el egy eszköz és a címkézés azokat hello meghibásodása előtt véve X rekordok "kapcsolatos probléma P miatt toofail<sub>i</sub>" (címke = P<sub>i</sub>), más rekordok címkézés közben " Normál"(címke = 0). Ezzel a módszerrel a feliratai kategorikus változók (lásd a 6. ábrát).

![6. ábra. A legfelső szintű OK előrejelzés multiclass besorolás címkézés](media/cortana-analytics-playbook-predictive-maintenance/labelling-for-multiclass-classification-for-root-cause-prediction.png)

6. ábra. A legfelső szintű OK előrejelzés multiclass besorolás címkézés

hello modell hozzárendel egy hiba valószínűség esedékes tooeach P<sub>i</sub> és hibák nélkül hello valószínűségét. Ezek a valószínűség nagyságrendet tooallow előrejelzését hello problémák, amelyek a jövőbeli hello valószínűleg toooccur által rendelhető. Repülőgép összetevő hibája használati eset multiclass besorolás hibát lett felépítve. Ez lehetővé teszi, hogy a hiba miatt tootwo különböző nyomás szelepösszetevők hello belül előforduló a következő hónap hello valószínűség hello előrejelzését.

Ajánló karbantartási műveletek hiba után, a címkézés nincs szükség egy jövőbeli horizon toobe kivételezett. Ennek az az oka hello modell nem becslése hiba történt a jövőbeli hello, de az imént becslése hello legvalószínűbb okának már elvégzett hello hiba után. Foglalhatja ajtó hibák hello harmadik esetet, amikor hello cél toopredict ebbe tartozik a megadott üzemi körülmények között az előzményadatok hello hiba okát. Ez a modell használja toopredict hello legvalószínűbb okait után hiba történt. Ez a modell egy fő előnyt az jelenti, hogy megkönnyíti tapasztalatlan technikusok tooeasily, amelyek egyébként szükséges év alatt érkezett élmény problémák diagnosztizálásához és megoldásához.

## <a name="training-validation-and-testing-methods-in-predictive-maintenance"></a>Képzési, érvényesítése és a vizsgálati prediktív karbantartás alatt áll
A prediktív karbantartás, hasonló tooany hello tipikus tanítási és tesztelési rutin igények tootake fiók hello idő különböző szempontok toobetter más megoldás terület időbélyegzővel adatokat tartalmazó, generalize nem látható jövőbeli adatokon.

### <a name="cross-validation"></a>Kereszt-ellenőrzési
Sok gépi tanulási algoritmusok hiperparamétereket, amely jelentősen módosíthatja modell teljesítmény számos függ. hello ezek hiperparamétereket optimális értékei vannak nem számított automatikusan hello modell betanításakor, de az adatok tudósok kell megadni. Számos módon, hogy a rendszer hiperparamétereket helyes értékeket. hello leggyakoribb egy "k hajtott kereszt-ellenőrzési", amely felosztja a hello példák véletlenszerűen "-k" modellrészt. Az egyes hiperparamétereket értékek tanulási algoritmus futtatási k alkalommal. Minden egyes ismétlés hello aktuális modellrészek hello példák érvényesítési készletként használt, betanítási készletként használt hello részeinek hello példák. hello algoritmus betanítja példák keresztül, és arra az esetre hello teljesítménymutatók vonatkoznak érvényesítési példák keresztül. Ez a ciklus az egyes hyperparameter értékek hello végén azt hello k teljesítmény metrika értékek átlaga számítási, és kattintson hyperparameter értékek, amelyek hello legjobb átlagos teljesítmény.

Ahogy korábban említettük, a prediktív karbantartási problémákat, adatok, amelyek az adatok több forrásból származó események idősor formájában van. Ezeket a rekordokat a rekord vagy például címkézés toohello ideje szerint kell rendelni. Ezért ha azt felosztása hello dataset véletlenszerűen képzési és érvényesítési, hello példák későbbinek időben érvényesítési példák némelyike. Az eredmény becslése jövőbeli teljesítmény hyperparameter értékek előtt modell betanítása lett érkező hello adatok alapján. Ezek becsléseket túlságosan optimista, lehet, különösen akkor, ha az idősorozat nem álló, és azok viselkedését változnak az idők. Ennek eredményeképpen választott hyperparameter értékek optimális lehet.

A jobb, hogy a helyes értékeket hiperparamétereket módja toosplit hello példák képzési és érvényesítési időfüggő módon állítsa be úgy, hogy az összes érvényesítési többek között az összes példák mint időben későbbi. Ezt követően hiperparamétereket értékének minden készlete esetében azt hello algoritmus betanítása keresztül betanítási készlete, modell teljesítmény mérésére hello azonos érvényesítési, majd válassza a hyperparameter értékek, amelyek megjelenítik a lehető legjobb teljesítményt hello keresztül. Ha időbeli fejlődésének-idősoros adatok nincs álló, hello hyperparameter értékek által tanítási és érvényesítés vágási átfutási tooa jobb jövőbeli "modell teljesítményt által kereszt-ellenőrzési véletlenszerűen választott hello értékekkel.

hello végső modell betanítása tanulási algoritmus segítségével hello legjobb hyperparameter értékek felosztása vagy a kereszt-ellenőrzési képzési és érvényesítés használatával teljes adatok állítja elő.

### <a name="testing-for-model-performance"></a>A modell teljesítmény tesztelése
A modell létrehozása után az új adatok a jövőbeni teljesítményét tooestimate igazolnia kell. hello legegyszerűbb becsült hello betanítási adatok keresztül lehet hello hello modell teljesítményétől. Azonban ez a becslés túlságosan optimista, mert a modell használt tooestimate teljesítmény szabott toohello adatokról. A jobb becsült hyperparameter értékek teljesítmény metrika számított hello érvényesítési készleten, vagy egy átlagos teljesítmény metrika történik a kereszt-ellenőrzési lehet. De hello azonos okai a korábban megadott, az ezeket becsléseket továbbra is túlságosan optimista. Több reális megközelítések modell teljesítményének méréséhez kell.

Egyik módja toosplit hello adatok véletlenszerűen képzési, az érvényesítési és a vizsgálat beállítása. hello képzési és érvényesítési beállítása értékeket használt tooselect hello hiperparamétereket és tanítási modell velük. a modell teljesítményétől hello hello TesztKészlet keresztül mérik.

Egy másik lehetőség, amelyhez megfelelő toopredictive karbantartási, a toosplit betanítása időfüggő módon érvényesítése és a vizsgálat beállítása úgy, hogy az összes teszt példák be például a következők minden képzési és az érvényesítés példák mint időben későbbi. Hello megosztása, után modell és a teljesítmény mérési végzett hello ugyanaz a fentebb leírt módon.

Ha idősorozat helyhez kötött és könnyen toopredict mindkét megközelítés jövőbeli teljesítmény hasonló becsléseket hoz létre. De ha idősorozat nem helyhez kötött és/vagy rögzített toopredict, hello második megközelítést több reális becslése jövőbeli teljesítmény, mint az első hello hoz létre.

### <a name="time-dependent-split"></a>Időfüggő felosztása
Ajánlott eljárásként ebben a szakaszban vesszük időfüggő vegyes megvalósításához részletes bemutatása. Azt írja le között tanítási és tesztelési, azonban pontosan hello ugyanez a logika alkalmazni kell időfüggő felosztása tanítási és érvényesítési beállítása időfüggő kétirányú valószínűségét.

Tegyük fel például különböző érzékelők mérték időbélyegzővel események adatfolyam kell. Tanítási és tesztelési példák, valamint a címkék funkcióit több eseményeket tartalmazó korlátozta keresztül vannak definiálva.
Például a bináris osztályozási szolgáltatás mérnöki és modellezési módszereket szakaszokban ismertetett módon szolgáltatások alapján hozzák létre hello múltbeli események és címkék alapján hozzák létre jövőbeni események típuson belüli "X" a hello időegységekkel jövőbeli. Így például időkeretétől címkézés hello később származik, majd a szolgáltatást időkeretén hello. Az időfüggő valószínűségét időben, ahol azt a modell betanításához bennünket hiperparamétereket az előzményadatok mentése toothat pont használatával azt ki egy olyan pontot. jövőbeli címkék, amelyekre nem térnek hello képzési küszöbérték a betanítási adatok kiszivárgásának tooprevent, választjuk hello legújabb időkeretre toolabel képzési példák toobe X egységek hello képzési határidő előtt. 7. ábra minden egyes teli kör hello végső szolgáltatás adatkészlet mely hello a szolgáltatások és a feliratok arra az esetre vonatkoznak a fent leírt toothe módszer szerint sorában jelöli. Mivel, hello ábrán látható hello rögzíti, amelyek kell képzési és egy tesztelési időfüggő vegyes megvalósításához X = 2 és W = 3:

![7. ábra. Ossza fel a bináris osztályozási időfüggő](media/cortana-analytics-playbook-predictive-maintenance/time-dependent-split-for-binary-classification.png)

7. ábra. Ossza fel a bináris osztályozási időfüggő

hello zöld négyzetes hello rekordot képviselő tartozó toohello mértékegységét képzési használható. Ahogy korábban, a végső hello képzési példákban megnézi által létrehozott-e a szolgáltatás táblában 3 időszakokat a szolgáltatás és a címkézési előtt a képzés nap lezárási 2 jövőbeli időszak lejárt. Példák állítható be, ha bármely hello 2 jövőbeli időszakok adott például része hello képzési küszöbérték felett marad, mivel feltételezzük, hogy nincs látható, a képzési küszöbérték felett marad hello képzés nem használunk. Toothat megkötés miatt fekete példák mutatják be hello rekordjának hello végső adatkészletre mutató nem használható a tanítási adathalmazt hello címkével. Ezeket a rekordokat nem használható az adatok, vagy mert hello képzési lezárási előtt kerül, és azok szalagcímkézési korlátozta részben hello képzési időkeretet, amely nem lehet hello eset, szeretnénk toocompletely függenek külön korlátozta a címkézés tesztelése képzési és tooprevent címke információk kiszivárgásának tesztelése.

Ez a módszer lehetővé teszi, hogy az átfedésben lévő hello adatok között a modell betanítására és tesztelésére példák, amelyek Bezárás toothe képzési lezárási szolgáltatás létrehozásánál használható. Attól függően, hogy az adatok rendelkezésre állását egy még inkább súlyos elválasztási bármelyikével nem hello szereplő példák a TesztKészlet belüli W időegységek hello képzési lezárási valósítható meg.

A munkahelyen észleltünk, hogy élettartama fennmaradó előrejelzéséhez használt regressziós modell hello kiszivárgásának probléma több súlyosan érinti, és tooextreme overfitting véletlenszerű valószínűségét használatával részletes útmutatást. Ehhez hasonlóan regressziós problémákat hello vegyes kell lennie, levágási képzési előtt hibákkal rendelkező eszközök tartozó rekordokat kell használni a képzési és a után hello küszöbérték beállítása tesztelési használandó hibákkal rendelkező eszközök.

Egy általános módszer egy másik fontos vágását meghatározó adatokat a modell betanítására és tesztelésére ajánlott toouse eszköz-azonosító szerint valószínűségét, hogy nincs hello eszközök képzési rendszerben használt tesztelésre, mivel a meghatározni, hogy a tesztelési meg arról, hogy toomake használt, amely egy új eszköz használatakor  az előrejelzés toomake, hello modell reális eredményét mutatja.

### <a name="handling-imbalanced-data"></a>Imbalanced adatok kezelése
A besorolási problémák Ha nincsenek további példákat hello, mint egy osztály másokról, az adatok hello említett toobe imbalanced. Ideális esetben szeretnénk toohave elég képviselői minden egyes osztály a hello betanítási adatok toobe képes toodifferentiate különböző osztályok között. Ha egy osztály hello adatok 10 %-nál kisebb, azt is tegyük fel például, hogy hello adatok imbalanced és hello underrepresented dataset kisebbségi osztály nevezzük. Drasztikusan sok esetben találtunk imbalanced adatkészletek egy osztály esetén súlyosan underrepresented összehasonlított tooothers például csak alkotó 0,001 % hello adatpontok szerint. Osztály egyensúlyhiány hibák belül hol áll általában ritka előfordulások hello élettartama hello eszközök hello kisebbségi osztály példák alkotó csalások felderítéséhez, a hálózati behatolás és a prediktív karbantartási sok tartományokban probléma.

Osztály egyensúlyhiány esetén a legtöbb szabványos tanulási algoritmusok teljesítmény biztonsága sérül, toominimize irányulnak általános Hibaarány hello. Például az adatkészlet 99 % negatív osztály példák és 1 % pozitív osztály példák azt eléréséhez 99 % pontossága egyszerűen címkézés negatív összes példányát. Azonban ez misclassifies meg minden pozitív példák, ezért hello algoritmus nincs hasznos egy hello pontossága metrika azonban nagyon nagy. Hagyományos értékelési metrikák általános pontossága a Hibaarány, például következésképpen imbalanced tanulási esetén nem elegendőek el. Más mutatókat, például a pontosság, visszaírási, F1 pontszámokat és módosított ROC görbék értékelések esetén imbalanced adatkészleteket, amelyről a hello értékelési-metrikák szakaszban használt költség.

Van azonban néhány módszereket, amelyek segítenek a orvoslása érdekében osztály egyensúlyhiány probléma. hello két fő mintavételt technikák és költség-és nagybetűket tanulási.

#### <a name="sampling-methods"></a>Mintavételi
mintavételi módszerek imbalanced szeretnének hello használata hello dataset módosítása által áll néhány mechanizmusok rendelés tooprovide az elosztott terhelésű dataset. Habár számos különböző mintavételi technikák, véletlenszerű-e legtöbb nagyon egyszerű ők oversampling és a mintavételi.

Egyszerűen is említettük, véletlenszerű oversampling véletlenszerűen kijelölése kisebbségi osztályból, ezekben a példákban replikálása, és hozzáadja őket tootraining adatkészlet. A teljes példák kisebbségi osztályban hello száma, végül elosztása a különböző osztályokhoz példák hello száma. A rendszer oversampling egy veszélye, hogy az egyes példák több példánya okozhat hello osztályozó toobecome túl adott vezető toooverfitting.
Magas képzési pontossága eredményezne, de előfordulhat, hogy az adatok tesztelés nem látható a teljesítmény nagyon alacsony. Ezzel ellentétben a mintavételi véletlenszerű kijelölése véletlenszerűen többsége osztály, és ezek a példák eltávolítása a tanítási adathalmazt. Azonban példák eltávolítása a legtöbb osztály okozhat hello osztályozó toomiss fontos fogalmakat toohello többsége osztály alkalmas. Hibrid mintavételi ahol túl van-e mintavételezett kisebbségi osztály és a legtöbb osztály a rendszer mintát hello egyszerre egy másik életképes megközelítést. Nincsenek a sok más kifinomultabb mintavételi módszerek állnak rendelkezésre, és osztály egyensúlyhiány mintavételi módszerei egy állandó figyelmet és a hozzájárulások fogadása sok csatornák népszerű kutatási területre. Döntse el, hogy a hello leghatékonyabb azokat, általában a bal oldali toohello adatok tudósok tooresearch és kísérletek kifejlesztéséhez, és nagymértékben függ hello adattulajdonságainak különböző módszerek használatát. Ezenkívül fontos, hogy mintavételi alkalmazott csak toohello képzési toomake állítja be, de nem hello TesztKészlet.

#### <a name="cost-sensitive-learning"></a>Bizalmas tanulási költsége
A prediktív karbantartás hello kisebbségi osztály alkotó hibák az érdekesebb normál példák mint, és így hello elsősorban a teljesítmény hello hibáiról algoritmus az általában hello fókuszba. Ez általában a egyenlőtlen elvesztése vagy különböző osztályokhoz, amelynek helytelenül előrejelzésére negatív pozitív is költség elemek misclassifying aszimmetrikus költségeinek említett több, mint ez fordítva is igaz. a keresett hello osztályozó kell tudni toogive magas előrejelzés pontosságát hello kisebbségi osztály a hello pontossága hello többsége osztály súlyosan veszélyeztetése nélkül.

Számos módon ez érhető el. A magas hello kisebbségi a téves besorolás az rendelve osztály, és a teljes költség szempontjából, eltérő hello problémáját elveszíti közben toominimize is hatékonyan kezelhető. Néhány gépi tanulási algoritmusok használja ezt az elképzelést eredendően például SVMs (támogatási vektoros gépek) pozitív és negatív példák költségét ahol építhető képzési idő alatt. Ehhez hasonlóan kiemelési módszerek használják, és általában megjelenítése a megfelelő teljesítmény imbalanced adatok, például a súlyozott döntési esetén algoritmus.

## <a name="evaluation-metrics"></a>Értékelési-metrikák
Amint azt korábban említettük, a osztály egyensúlyhiány gyenge teljesítményt okoz a algoritmusok általában tooclassify többsége osztály példák jobb kisebbségi osztály esetek költségeket a hello teljes téves besorolás hiba sokkal akkor javul, ha a legtöbb osztály helytelenül lett címkézve, mivel. Ez kis visszaírási díjszabás okoz, és egy nagyobb probléma lesz, ha nagyon nagy a téves riasztásokat toohello üzleti hello költségét. Pontosság hello legnépszerűbb metrika egy osztályozó teljesítmény leíró használt. Azonban a fentiekben leírtak szerint pontossága nem helyettesíti, és nem tükrözik hello valós teljesítmény egy osztályozó funkciókat biztosítanak, mert nagyon érzékeny toodata terjesztéseket. Ehelyett a más értékelési-metrikák használt tooassess imbalanced tanulási problémák. Ezekben az esetekben pontosság, visszaírási és F1 pontszámok kell hello kezdeti metrikák toolook: prediktív karbantartási modell teljesítmény kiértékelése során. A prediktív karbantartás visszaírási díjszabás jelöl hello TesztKészlet hibáinak sikerült helyesen azonosítani hello modell hello számát. Magasabb visszaírási díjszabás jelenti azt hello modell sikeres hello igaz hibák alatt. Pontosság metrika téves riasztásokat, ha alacsonyabb pontosság díjszabás magasabb téves riasztásokat megfelelnek hello aránya vonatkozik. F1 pontszám pontosság és a visszaírási díjszabás ajánlott érték 1 alatt, és legrosszabb 0 figyelembe veszi.

Ezenkívül bináris osztályozási, decile táblák és növekedési diagram olyan nagyon informatív teljesítmény kiértékelése során. Ezeket csak az a pozitív osztály (hibák) koncentrálhat, és adja meg az algoritmus teljesítmény összetettebb képe mint mi látható megtekintésével, hogy csak egy rögzített működési pont hello: (fogadó működő Fullnosc) ROC-görbe.
Decile táblák nyerhetők rendelési hello tesztelési példák küszöb toodecide hello végső címke előtt hello modell által kiszámított hibák az előre jelzett valószínűség szerint. hello rendezett minták majd deciles (azaz hello 10 % minták a legnagyobb valószínűség és majd 20 %, 30 % és így tovább) vannak csoportosítva. Minden decile igaz pozitív arányát és annak véletlenszerű baseline (azaz 0,1, 0,2..) hello aránya számítástechnikai által egy megbecsülheti, hogyan hello algoritmus teljesítmény minden decile módosulnak. Növekedési diagramok decile igaz pozitív sebessége és a véletlenszerű igaz pozitív arány pár összes deciles ábrázolásához használt tooplot decile értékek közé. Hello első deciles általában hello fókusz hello eredmények óta itt látható a legnagyobb növekedését. Első deciles reprezentatív "veszélyben", a prediktív karbantartási használatakor is látható.

## <a name="sample-solution-architecture"></a>A minta megoldás architektúrája
A prediktív karbantartási megoldás telepítése, amikor azt is egy záró tooend megoldás, amely a folyamatos ciklust adatfeldolgozást, az adatok tárolási modell betanítási, szolgáltatás-létrehozás, előrejelzés és hello eredmények, valamint a képi megjelenítés egy riasztás-előállítási mechanizmus, például a figyelési irányítópult eszközként. Azt szeretnénk, későbbi insights toohello felhasználói biztosít folyamatos automatikus módon adatok folyamat. Az ilyen egy IoT-adatok adatcsatorna példa prediktív karbantartási architektúrája alábbi 8. ábrán látható. Az architektúra valós idejű telemetriai eseményközpontnak streamelési adatok tárolására szolgáló gyűjti. Ezek az adatok a valós idejű feldolgozási adatok, például a szolgáltatás-létrehozás a stream Analytics van okozhatnak. hello majd használják a szolgáltatásokat toocall hello prediktív modellt webszolgáltatás és az eredmények jelennek meg a hello irányítópulton. At hello azonos idő, a feldolgozott adatok is előzmény-adatbázisban tárolja, és egyesíti a külső adatforrások, mint például a helyszíni adatok adatbázisok toocreate példák a modellezési.
Ugyanazt az adatraktárak kötegelt pontozási hello példák, és újra lesz használt tooprovide prediktív jelentések hello irányítópult hello eredmények tárolására használható.

![8. ábra. Példa megoldásarchitektúra prediktív karbantartás](media/cortana-analytics-playbook-predictive-maintenance/example-solution-architecture-for-predictive-maintenance.png)

8. ábra. Példa megoldásarchitektúra prediktív karbantartás

További információ hello összetevői hello architektúra tekintse meg túl[Azure](https://azure.microsoft.com/) dokumentációját.

