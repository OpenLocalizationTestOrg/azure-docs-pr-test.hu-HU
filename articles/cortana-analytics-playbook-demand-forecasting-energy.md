---
title: "az igény szerinti előrejelzési energia Eszközintelligencia megoldás sablon alkalmazástervezési aaaCortana |} Microsoft Docs"
description: "A Microsoft a Cortana Intelligence, amelyek segítségével igény szerint egy energia segédprogram vállalat előrejelzési megoldás sablon."
services: cortana-analytics
documentationcenter: 
author: ilanr9
manager: ilanr9
editor: yijichen
ms.assetid: 8855dbb9-8543-45b9-b4c6-aa743a04d547
ms.service: cortana-analytics
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2016
ms.author: ilanr9;yijichen;garye
ms.openlocfilehash: 32fc6ece7e24ced34282baf107548039694a38b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-solution-template-playbook-for-demand-forecasting-of-energy"></a>A Cortana Intelligence megoldás sablon alkalmazástervezési energia előrejelzési igény szerint
## <a name="executive-summary"></a>Vezetői összefoglaló
Az elmúlt néhány évben, az eszközök internetes hálózatát (IoT) hello alternatív energia források és a big Data típusú adatok lehet egyesített toocreate túlnyomó lehetőségek hello segédprogram és energia tartományban. A hello ugyanannyi időt vesz igénybe, hello segédprogram és hello teljes energia szektorokkal rendelkező láthatta fogyasztás egybesimítását a fogyasztók jobb módon toocontrol igényelnek energia használatát. Emiatt hello segédprogram és az intelligens rács vállalatok nagy szükség van a tooinnovate, majd újítsa meg magukat. Ezenkívül számos energiagazdálkodási és segédprogram rácsok elavult és nagyon költséges toomaintain egyre, és kezelése. Hello az elmúlt évben, során hello team hello energia tartományon belül kapcsolattartás során számos dolgozott. A kapcsolattartás során során történt sok esetben a mely hello segédprogramok vagy a független szoftverszállítók (független szoftvergyártók) rendelkezik lett keresése az előrejelzés a jövőbeli energia igény szerint. Az ilyen előrejelzések fontos szerepet játszanak az jelenlegi és jövőbeli üzleti, és különböző használatából adódó hello foundation váltak. Ezek közé tartozik a rövid és hosszú távú power terhelés előrejelzés, kereskedelmi, terheléselosztás, rács optimalizálás stb. Big Data típusú adatok és a speciális elemzés AA módszerek, például a Machine Learning (ML) is hello kulcs enablers pontos és megbízható előrejelzések készítésére.  

Ez a forgatókönyv a azt hozzáfoghat hello üzleti és analitikai irányelvek sikeres fejlesztői szükséges, és energia igény szerinti telepítését előrejelzési megoldás. Javasolt iránymutatás segítséget, segédprogramok, az adatelemzők és az adatok mérnökök teljesen operationalized, felhőalapú, igény szerinti előrejelzés megoldások kialakítása során. A vállalatok számára, a big Data típusú adatok és a speciális elemzés út csak indító ilyen megoldást jelenthet a hosszú távú intelligens rács stratégia hello kezdeti leképezésben.

> [!TIP]
> a diagramban, ez a sablon az architektúra áttekintése toodownload lásd: [Cortana Intelligence Megoldássablonban architektúra az igény szerinti előrejelzési energia](cortana-analytics-architecture-demand-forecasting-energy.md).  
> 
> 

## <a name="overview"></a>Áttekintés
Ez a dokumentum hello business, az adatok és a Cortana Intelligence használatával és az adott Azure Machine Learning (AML) hello végrehajtása és a központi telepítés energia előrejelzés megoldások technikai szempontokat ismerteti. hello dokumentum három fő részből áll:  

1. Üzleti ismertetés  
2. Adatok ismertetése  
3. Technikai kivitelezés

Hello **üzleti ismertetése** rész vázol fel hello üzleti aspektus egy igények toounderstand, és vegye figyelembe a korábbi toomaking befektetési döntés. Ismerteti, hogyan tooqualify hello üzleti probléma: az aktuális tooensure, hogy a prediktív elemzés és a gépi tanulás valóban hatékony és megfelelő. hello dokumentum további gépi tanulás, és hogyan használt tooaddress energia-előrejelzés problémák hello alapjait ismerteti. Azt ismerteti, hello Előfeltételek és a használati esetek feltételeinek hello a feltételnek. Néhány példa használja esetek és üzleti eset forgatókönyvek is megadja.

A adata hello fő alkotóeleme a gépi tanulási megoldás. Hello **adatok ismertetése** Ez a dokumentum része ismertet néhány fontos szempontja a hello adatok. Azt ismerteti, hogy hello típusú adatokhoz, energia előrejelzés, minőségi követelményeit, és milyen adatforrásokat általában létezik. Azt is ismertetik, hogyan hello nyers adatok ténylegesen meghajtó hello modellezési része, a használt tooprepare adatokkal kapcsolatos funkciókkal.

hello hello dokumentum harmadik része foglalkozik hello **technikai kivitelezés** szempontja, hogy a megoldás. A szolgáltatás műszaki osztály és modellezési a hello fő hello adatok tudományos folyamat, és részletesen bemutatható ezért tárgyalt. Webszolgáltatások, amelyek egy prediktív elemzési megoldások felhőben telepítését fontos vehicle hello fogalma magában foglalja. Azt is szerkezeti egy tipikus architektúra egy végpontok közötti operationalized megoldás.

Hello dokumentum tartalmazza továbbá a további tudnivalók a hello tartomány és a technológia toogain használható referenciaanyag.

Fontos, hogy azt nem állt szándékában toocover a dokumentum hello mélyebb adatok tudományos folyamat, matematikai és műszaki szempontokból toonote. A részletek megtalálhatók a [Azure ML dokumentáció](http://azure.microsoft.com/services/machine-learning/) és [blogok](http://blogs.microsoft.com/blog/tag/azure-machine-learning/).

### <a name="target-audience"></a>Célközönség
Ez a dokumentum célközönsége hello egyben üzleti és műszaki személyzet szeretné toogain Tudásbázis, és ismeri a Machine Learning-alapú megoldások, és hogyan ezeket használják-e kifejezetten hello energia-előrejelzés tartományon belül.

Adatszakértőkön is kihasználhassa olvasva a dokumentum toogain jobb megértése hello magas szintű folyamata, hogy a meghajtók hello egy havi előrejelzési megoldás központi telepítését. Ebben a környezetben is lehet használt tooestablish jó alapkonfigurációt, és kiindulási pontjaként további részletes, és speciális anyagot.

### <a name="industry-trends"></a>Trendek
Az elmúlt néhány évben hello IoT, a másik energia források és a big Data típusú adatok lehet egyesített toocreate túlnyomó lehetőségek hello segédprogram és energia. Hello: ugyanannyi időt vesz igénybe, hello segédprogram és hello teljes energia szektorok láthatta fogyasztás egybesimítását a fogyasztók jobb módon toocontrol igényelnek energia használatát.

Számos segédprogram és intelligens energiavállalatok rendelkezik lett pioneering hello [intelligens rács](https://en.wikipedia.org/wiki/Smart_grid) használja több telepítésével esetek, amelyek használni hello rács által generált hello adatait. Belső tulajdonságai hello elektromos áram köré szerveződik sok használati eset: nem lehet halmozott sem készlet fenntartott tárolja. Igen milyen hozzák kell használni. Kívánt hatékonyabb toobecome segédprogramok kell tooforecast energiafogyasztását egyszerűen mert, amely ad nekik a nagyobb lehetőséget túl**ellátásának és igényének**, megelőzve az energia veszteség **üvegházhatású csökkentése kibocsátási gáz**, és a költségek szabályozásához.

Ha van szó, a költségek, akkor egy másik fontos szempont, amely ár. Új képességek között segédprogramok tootrade power léptetik nagy szükség van a túl**előrejelzési jövőbeli igények és az elektromos áram jövőbeli ár**. Ez segít a vállalatoknak az éles kötetek meghatározása.

Hello word "intelligens" használatakor ténylegesen irányítjuk ismerje meg, és végezze el előrejelzéseket tooa rács. Azt is, amelyek várhatóan fogyasztás határozza változásainak, valamint **várható ideiglenes túlterhelés olyan helyzetekben, és az automatikus igazítása**. Távolról szabályozásához-felhasználást (ezek intelligens mérőszámok hello segítségével), amelyet honosított túlterhelési olyan helyzetekben lehet kezelni. **Először előrejelzésére és majd működő**, hello teszi maga intelligensebb adott idő alatt.

Ez a dokumentum egy adott családba tartozó fedik le a jövőbeli, előrejelzés használati esethez tárgyaljuk hello többi rövid távú és hosszú távú energia igény szerint. Jelenleg néhány hónapig évi működik, és néhány Tudásbázis és szakértelem, amelyek lehetővé teszik a számunkra tooproduce iparági osztályú eredmények szerzett. Más használati eseteket hello dokumentum a jövőben közelében hello is szerepelnek.

## <a name="business-understanding"></a>Az üzleti igények felmérése
### <a name="business-goals"></a>Üzleti céljaihoz
Hello **energia bemutató** célja toodemonstrate tipikus prediktív elemzési és gépi tanulási megoldás, amely egy nagyon rövid időkereten belül is telepíthető. Pontosabban az aktuális elsősorban energia igény szerinti előrejelzési megoldások engedélyezése, hogy az üzleti értékét gyorsan kell is tudjuk és esetén alkalmazhatók. Ez a forgatókönyv a hello információk segítségével a következő célok hello parancsokról hello ügyfél:

* Rövid idő toovalue machine learning-alapú megoldás
* Képes tooexpand egy kísérleti használható case tooother használja az esetek vagy tooa az üzleti igények alapján szélesebb körű hatókör
* Gyorsan hozzáférni a Cortana Intelligence Suite termékismeretek

Az ezen célok szem előtt a forgatókönyv célja, hogy hello üzleti és műszaki ismereteket, amely segít ezen célok eléréséhez.

### <a name="power-load-and-demand-forecasting"></a>Energiagazdálkodási terhelés és igény szerint előrejelzés
Hello energia szektor, belül lehetnek az sokféleképpen mely igény szerinti előrejelzés segíthet kritikus üzleti problémák megoldásához. Valójában igény szerinti előrejelzés tekinthető hello foundation sok core használatából adódó hello iparágban. Általában az adatraktárakban az energia igény szerinti előrejelzések kétféle: rövid távú és hosszú távú. Minden egyes előfordulhat, hogy más célt szolgálnak, és egy másik módszert használják. hello a fő különbség a között két hello horizon, azaz hello időtartományt jövőbeli hello be, amelynek azt kellene előrejelzési előrejelzés hello.

#### <a name="short-term-load-forecasting"></a>Rövid távú terhelés előrejelzése
Hello környezetben energiát igényű a rövid távú betölteni az előrejelzés (STLF) hello összesített terhelést a hello közelében különböző részeivel hello rács (vagy egy teljes hello rács) a jövőben van előrejelzett típusúként van definiálva. Ebben a környezetben a rövid távú meghatározott toobe idő horizon 1 óra too24 óra hello tartományán belül. Bizonyos esetekben egy horizon 48 órán keresztül is lehetőség. Ezért STLF, az operatív használati eset hello rács nagyon gyakori. Íme néhány példa a használati esetek vezérelt STLF:

* Terheléselosztás készletek és igények
* Energiagazdálkodási kereskedelmi támogatása
* Piaci elvégzése (a beállítás kiemelt ár)
* Rács működési optimalizálása
* [Igény szerinti válasz](https://en.wikipedia.org/wiki/Demand_response)
* Igény szerinti előrejelzés maximális
* Igény szerinti ügyféloldali kezelése
* Terheléselosztás és megelőzési túlterhelés
* Hosszú távon terhelés előrejelzése
* Hiba és anomáliadetektálási észlelése
* Csúcsidőszak megszorítás/simítás 

STLF modell főként hello közelében (utolsó nap vagy hét) múltbeli adatokkal és -felhasználási előrejelzett hőmérséklet, egy fontos előrejelzőjének. Megszerezni a következő órában hello és too24 órában előrejelzési pontos hőmérséklet egyre nagyobb kihívást most nap. Ezek a modellek a következők: kevésbé érzékeny tooseasonal minták vagy hosszú távú fogyasztási trendjeinek.

SLTF megoldások megtalálhatók valószínűleg toogenerate nagy mennyiségű előrejelzés hívások (Szolgáltatáskérés), mert azok vannak meghívott óránként, és egyes esetekben még nagyobb gyakorisággal. Ha egy önálló modell ki minden egyes állomás vagy átalakító, és ezért hello mennyiségi előrejelzés kérelmek még nagyobb nagyon gyakori toosee beültetésre egyben.

#### <a name="long-term-load-forecasting"></a>Hosszú távon terhelés előrejelzése
hello a hosszú távú terhelés előrejelzés (LTLF) célja egy időbeli határait, a beállításnak 1 hét toomultiple hónapok (és egyes esetekben az évek) rendelkező tooforecast power igény szerint. Ezt a tartományt az időhatár többnyire alkalmazható a tervezési és befektetési használati esetekben.

A hosszú távú forgatókönyvek esetén fontos toohave kiváló minőségű adat, amely lefedi a span több éves (minimális 3 év). Ezek a modellek fog általában minták szezonalitás értékének kinyerése hello előzményadatokat és lehetővé teszik a külső predicators például időjárása és a környezet minták szerint.

Fontos, hogy az előrejelzés horizon hosszabb hello hello tooclarify, lehet, hogy kevésbé pontos hello előrejelzés hello. Éppen ezért bizonyos abban, hogy intervallumok együtt tényleges hello előrejelzési, amely lehetővé teszi az emberek toofactor hello lehetséges variation azokat a tervezési folyamat fontos tooproduce.

Hello felhasználási forgatókönyve LTLF többnyire tervezési, mivel azt várható sokkal alacsonyabb előrejelzés kötetek (a összehasonlított tooSTLF). Azt a képi megjelenítés eszközök, például Excel vagy a Power bi beilleszthető előrejelzéseket általában jelenik meg, és hello felhasználó manuálisan elindítható.

### <a name="short-term-vs-long-term-prediction"></a>Rövid távú vs. Hosszú távú előrejelzés
a következő táblázat hello összehasonlítja STLF és LTLF tekintetben toohello legfontosabb attribútumok:

| Attribútum | Rövid távú terhelés előrejelzése | Hosszú távon terhelés előrejelzés |
| --- | --- | --- |
| Horizon előrejelzés |1 óra too48 óra |Az 1 too6 hónapos |
| Adatok granularitási |Óradíj |Óránként vagy naponta |
| A tipikus használati esetek |<ul><li>Igény szerinti/ellátási terheléselosztás</li><li>Válassza ki az óra előrejelzés</li><li>Igény szerinti válasz</li></ul> |<ul><li>Hosszú távon tervezési</li><li>Rács eszközök tervezése</li><li>Erőforrás tervezése</li></ul> |
| Tipikus előre |<ul><li>Nap vagy hét</li><li>Nap órája</li><li>Óránkénti hőmérséklet</li></ul> |<ul><li>Hónap év</li><li>Hónap napja</li><li>Hosszú távú hőmérséklet és a környezet</li></ul> |
| Előzményadatokat tartomány |Két toothree év értékelésével |Öt too10 év értékelésével |
| Tipikus pontossága |MAPE * 5 % vagy alacsonyabb |MAPE * 25 %-os vagy alacsonyabb |
| Előrejelzési gyakorisága |Óránként vagy 24 óránként |Miután a havi, negyedéves és éves előállított |

\*[MAPE](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error) – témakörök átlagos százalékos hiba

Mivel ebben a táblázatban is látható, elég fontos toodistinguish hello rövid és hosszú távú hello előrejelzés forgatókönyvek, mivel határoz meg különböző üzleti igényeknek és különböző központi telepítési és felhasználási minták között.

### <a name="example-use-case-1-esmart-systems--overload-optimization"></a>Példa használati eset 1: eSmart rendszerek – túlterhelési optimalizálása
A fontos szerepet egy [intelligens rács](https://en.wikipedia.org/wiki/Smart_grid) toodynamically és folyamatosan optimalizálása és hello fogyasztási módosításával igazíthatja. Energiafogyasztás befolyásolhatja, főleg hőmérséklet ingadozását által okozott rövid távú módosításokkal (*pl.*, nagyobb teljesítmény vezeték nélkül regisztrálja az állapot vagy a fűtésrendszerek használatos). At hello azonos időben, a gépi fogyasztás is befolyásol hosszú távú trendeket. Ezek közé tartozhatnak szezonalitás értékének hatások, nemzeti ünnepek, hosszú távú használat növekedés és fogyasztói index, olaj ár és GDP akár gazdasági tényezők.

A használati eset a [eSmart](http://www.esmartsystems.com/) szeretett volna, amely lehetővé teszi a hello innovációkká alakuljon előrejelzésére egy túlterhelési helyzetét a bármely adott állomás hello rács toodeploy egy felhőalapú megoldás. Különösen eSmart kívánta tooidentify alállomások, amelyek a következő órán belül hello valószínűleg toooverload úgy azonnali művelet tooavoid tett, vagy hárítsa el az adott helyzetben.

Pontos és gyors a előrejelzésének végrehajtása szükséges három prediktív modelleket végrehajtását:

* Hosszú távú modellt, hogy lehetővé teszi, hogy az előrejelzés során minden egyes állomás a energiafogyasztás hello a következő néhány hét és hónap
* Rövid távú modell, amely lehetővé teszi, hogy a következő órában hello során egy adott állomás túlterhelési helyzet előrejelzését
* Hőmérséklet-modell, amely több forgatókönyv keresztül jövőbeli hőmérséklet előrejelzés

hello hello hosszú távú modell célja toorank hello alállomások által a innovációkká alakuljon toooverload (a power transmission kapacitásának megadott) hello következő hét vagy havi során. Ez lehetővé teszi a hello létrehozása, amely rövid távú előrejelzés hello bemenetként szolgál alállomások rövid listáját. Mivel hőmérséklet egy fontos előrejelzőjének hello hosszú távú modell, nincs szükség tooconstantly által előállított több forgatókönyv előrejelzések és toohello hosszú távú modellbe bemenetként hírcsatorna őket. rövid távú hello előrejelzési majd meghívott toopredict mely állomás valószínűleg toooverload felett hello következő órában.

hello rövid és hosszú távú modellek minden állomás egy külön-külön vannak telepítve. Ezért hello gyakorlati ezek a modellek végrehajtásához kiterjedt vezénylési. toogain magasabb előrejelzés pontosságát hello rövid távon, részletesebb modell minden órában hello nap van kijelölve. Ezek a modellek hajtja végre a rendszer minden órában, és fejezze be a végrehajtása néhány percig tooallow elegendő idő toorespond belül, és megelőző műveletek szükség esetén. Ez a témakörgyűjtemény a modellek frissül folyamatosan által időszakos átképezési hello legújabb adatokkal.

További információ a használati eset található [Itt](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18945).

#### <a name="use-case-qualification-criteria--prerequisites"></a>Használja a nagybetűk minősítési feltételek – Előfeltételek
hello fő Cortana Intelligence erőssége hatékony képességét toodeploy és skálázási machine learning központú megoldások. Tervezett toosupport több ezer előrejelzés párhuzamosan végrehajtott. Az automatikusan át tudja méretezni toomeet egy változó fogyasztás mintát. A megoldás ezért elsősorban pontosságát és számítási teljesítményt. Például a vállalat olyan iránt érdeklődik előállító hello következő órában, és minden órában hello nap előrejelzési pontos energia igény szerint. A hello ugyanakkor, azt is kevesebb miért hello igény szerint-e előre jelzett toobe, mert az hello kérdését megválaszolását (hello modell maga kezeli, amely).

Éppen ezért fontos toorealize, hogy nem minden esetben használja, és üzleti problémák hatékonyan megoldhatók gépi tanulás használatával.

A Cortana Intelligence és a gépi tanulás lehet nagyon hatékony egy adott üzleti probléma megoldásában hello a következő feltételek teljesülése esetén:

* hello az aktuális üzleti probléma van **prediktív** jellegűek. A prediktív használata eset példa egy vállalat, amit toopredict power terhelése, egy adott állomás hello következő óra során. A hello ugyanakkor, elemzése és illesztőprogramok korábbi igényű prioritás lenne **leíró** jellegű, ezért kevesebb alkalmazható.
* Nyilvánvaló **elérési útja művelet** végrehajtott egyszer hello előrejelzés toobe érhető el. Például egy állomás a túlterhelés előrejelzésére hello következő óra során is elindíthatja egy adott állomás társított terhelés csökkentését, és így potenciálisan megakadályozza az olyan túlterhelést proaktív műveletet.
* hello használati eset jelöli egy **tipikus típusú probléma** úgy, hogy ha orvosolhatók azt is pave hello módon toosolving más hasonló használati eseteket.
* hello ügyfél állíthatja be **mennyiségi és minőségi célok** toodemonstrate a sikeres megoldás megvalósítása. Például az energia igény szerinti előrejelzés helyes mennyiségi cél lenne hello szükséges pontosságot küszöbértéket (*pl.*, too5 mentése % hiba engedélyezett), vagy ha az állomás túlterhelési majd hello pontosság (true figyelmeztetéséket aránya) előrejelzésére és visszaírási (true figyelmeztetéséket mértékének) egy adott küszöbérték feletti kell lennie. Ezen célok hello ügyfél üzleti céljainak kell származnia.
* Nyilvánvaló **integrációs forgatókönyv** hello a vállalat üzleti a munkafolyamathoz. Például hello állomás terhelés előrejelzés integrálhatják hello rács control center tooallow túlterhelési megelőzési tevékenységek.
* hello ügyfélnek van kész toouse **megfelelő minőségű adatok** toosupport hello-és nagybetűhasználattal (bővebb információt a következő szakaszban hello, **az adatminőségi**, ez a forgatókönyv az).
* ügyfél támogatja a központú adatok architektúra hello vagy **felhőalapú gépi tanulás**, beleértve az Azure ML és egyéb Cortana Intelligence Suite összetevői.
* hello ügyfél hajlandó tooestablish **egy záró tooend adatfolyama** létesítményekben hello hello felhőbe helyezni az adatokat kézbesítését, és kész túl**azok** hello megoldás.
* hello ügyfél készen áll a túl**erőforrást fordítsanak** ki lesz kell bevonása hello kezdeti kísérleti a végrehajtás során, hogy a Tudásbázis és hello megoldás tulajdonjogát lehet átvitt toohello ügyfelének sikeres létrehozása után.
* hello felhasználói erőforrás kell lennie egy **gyakorlott adatok professional**, lehetőség szerint olyan adatok tudósok.

Minősítési feltételek fent hello alapján használati eset a nagy mértékben javíthatják a használati esetek hello sikerességi arányát és létrehozni egy jó beachhead jövőbeli használati esetek hello végrehajtásához.

### <a name="cloud-based-solutions"></a>Felhő alapú megoldások
Az Azure Cortana Intelligence Suite integrált környezetet, amely hello felhőben található. hello központi telepítése egy speciális elemzési megoldások fontosságú egy felhőalapú környezetben, amely tárolja a vállalkozások számára jelentős előnyt, és a hello ugyanannyi időt vesz igénybe azt jelentheti nagy változást a vállalatok számára, hogy továbbra is használja a helyszíni IT-megoldásokhoz. Hello energia szektor, belül nincs fokozatos áttelepítés hello felhő műveletek egyértelmű tendenciát. Erre az irányra kerül jár együtt hello fejlesztési hello intelligens rács című szakaszban leírtaknak megfelelően a fenti a **trendek**. Mivel ez a forgatókönyv egy felhőalapú megoldás hello energia tartomány összpontosít, is fontos tooexplain hello előnyei és egyéb szempontok a felhőalapú megoldás.

Lehet, hogy hello legnagyobb előnye egy felhőalapú megoldáson, hello költség. Megoldás él felhő telepített összetevők nincs társaságuk költségek vagy (az árukat eladott) ELÁBÉ összetevők költsége helyett társítva. Ez azt jelenti, hogy a hardverek, szoftverek és informatikai karbantartási nincs szükség tooinvest van, és ezért nem jelentős üzleti kockázat csökkentése.

Egy másik előnye, hello használatalapú költség szerkezete felhőalapú megoldásokhoz. Kiszolgálók felhőalapú számítási és tárolási telepített, és most-szükség szerint alapon méretezhető. Hello költség hatékonyságát kihasználása egy felhőalapú megoldás jelképez.

Végül, ez hello felhőalapú ajánlat része informatikai karbantartás vagy későbbi infrastruktúra fejlesztési terhelésnél nincs szükség van. toothat mértékben, Cortana Intelligence Suite tartalmaz hello legjobb osztály szolgáltatások és az ütemterv fejlődnek tartja. Új szolgáltatások, összetevők és képességek folyamatosan bevezetni, és fejleszteni.

Egy olyan vállalat csak megkezdi annak átmenetében hello felhőbe magas javasoljuk tootake fokozatos megközelítés a felhő áttelepítési ütemterv alkalmazásával. Biztosak vagyunk abban, hogy segédprogramok és vállalatok hello energia tartományban, a forgatókönyv ismertetett hello használati esetek prediktív elemzési megoldások felhőben hello ügyfélteszteléssel kiváló lehetőséget jelképezi.

#### <a name="business-case-justification-considerations"></a>Üzleti eset indoklás kapcsolatos szempontok
Sok esetben hello ügyfél létrehozása során egy adott használati eset, amelyben egy felhőalapú megoldás és a gépi tanulás olyan fontos összetevők az üzleti indoklásának érdekelt lehet. Egy helyszíni megoldás hello esetében egy felhőalapú megoldáson eltérően hello társaságuk költség összetevője minimális, és a legtöbb hello költség elemek társított valós használatot. Az előrejelzés megoldás a Cortana Intelligence Suite energia ismét toodeploying, több szolgáltatás integrálható az egyetlen közös költség struktúrára. Például adatbázisok (*pl.*, az SQL Azure) használt toostore hello nyers adatok, valamint az majd a tényleges hello Azure ML előrejelzések van használt toohost hello előrejelzés szolgáltatások. Ebben a példában a struktúra költség hello tartalmazhatnak, tárolás és a tranzakciós összetevők.

A hello, ugyanakkor egy kell hello üzleti értéket egy energia igény szerinti (rövid és hosszú távú) előrejelzés működési beható ismerete. Ez valójában fontos toorealize hello üzleti értéket minden egyes előrejelzési művelet. Például pontosan előrejelzés hello power betöltésének 24 órán át megakadályozhatja, hogy túltermelés vagy megakadályozhatja a hello rács túlterhelések és ez mennyiségi kell naponta pénzügyi megtakarítások tekintetében.

Alapszintű hello pénzügyi előnye, hogy igény kiszámításához képlet előrejelzési megoldás: ![hello pénzügyi előnye, hogy igény szerinti számítási alapvető képlete előrejelzési megoldás](media/cortana-analytics-playbook-demand-forecasting-energy/financial-benefit-formula.png)

Cortana Intelligence Suite használatalapú árképzési modellt biztosít, mivel nincs szükség van egy fix költség összetevő toothis képlet nélül. A képlet kerülhet sor, naponta, havi vagy éves alapon.

Aktuális Cortana Intelligence Suite és Azure ML díjszabások található [Itt](http://azure.microsoft.com/pricing/details/machine-learning/).

### <a name="solution-development-process"></a>A megoldás fejlesztési folyamata
hello fejlesztési ciklus egy energia igényű általában szükség 4 fázisra, biztosítjuk, amelyek mindegyikét az előrejelzés technológiák felhőalapú használja, és -szolgáltatások üzemeltetését hello Cortana Intelligence Suite.

Ezt mutatja be a következő diagram hello:

![Intelligens rács ciklus](media/cortana-analytics-playbook-demand-forecasting-energy/smart-grid-cycle.png)

a következő bekezdés hello a 4. lépés folyamat ismerteti:

1. **Adatgyűjtés** – minden speciális elemzési alapú megoldás adatok támaszkodik (lásd: **adatok ismertetése**). Pontosabban ismét toopredictive elemzés és előrejelzését, azt használja a folyamatban lévő, dinamikus az adatok áramlását. Az energia igény szerinti előrejelzés hello esetben ezeket az adatokat közvetlenül az intelligens mérőszámok forrása is lehet, vagy egy helyszíni adatbázisban már összesíteni. Azt is más külső adatforrások például időjárása és a hőmérséklet támaszkodnak. A folyamatban lévő adatok és kell lennie vezénylését, ütemezett, tárolja. [Az Azure Data Factory](http://azure.microsoft.com/services/data-factory/) (ADF) a fő workhorse való elvégzéséhez szükséges parancsokról ezt a feladatot.
2. **Modellezési** – pontos és megbízható energia előrejelzéseket, egy kell fejlesztése (szerelvény) és nagy modell, amely lehetővé teszi a hello előzményadatokat, illetve kivonatok hello jelentéssel bíró karbantartása és prediktív minták hello adatokat. a növekvő gyorsan lett rendelkezik a hello terület a Machine Learning (ML) speciális algoritmusok rendszeresen fejlesztés alatt áll. Az Azure ML Studio, amely segít a legtöbb speciális ML algoritmusok belül a teljes munkafolyamat hello használata kiváló felhasználói élményt nyújt. A munkafolyamat mutatja be egy egyszerűen elsajátítható folyamatábrája, és tartalmazza a hello adatok előkészítése, a szolgáltatás kivonása, a modellezési és a modell kiértékelése. Ebben a környezetben található különböző modellek több száz hello felhasználói is bekérésére. Ebben a fázisban hello végére egy adatok tudósok lesz működő modell, amely teljes kiértékelt, készen áll a központi telepítés.
   
   hello a következő ábrán egy tipikus munkafolyamat ábrája:
   
   ![Modellezési munkafolyamat](media/cortana-analytics-playbook-demand-forecasting-energy/modeling-workflow.png)
3. **Központi telepítés** – működő modell, hello következő lépésre a központi telepítés. Itt hello modell alakítja egy webszolgáltatás, amelyet egy RESTful API, egyidejűleg keresztül ér el különböző felhasználási ügyfelektől érkező internetes hello mutatja be. Az Azure ML egy modell, közvetlenül az Azure ML Studio hello gomb egyetlen kattintással üzembe egyszerű módszert kínál. hello teljes telepítési folyamatának hello technikai részletek alapján történik. Ez a megoldás automatikusan át tudja méretezni toomeet szükséges hello fogyasztás.
4. **Felhasználás** – ebben a fázisban ténylegesen biztosítjuk előrejelzési modell tooproduce előrejelzéseket hello használata. hello fogyasztás is alapú, felhasználó-alkalmazás (*pl.*, irányítópult), vagy közvetlenül például működési rendszerből igény szerinti/rendszer és a rács optimalizálási megoldást. Több használati esetek is vezeti egyetlen modellből.

## <a name="data-understanding"></a>Adatok ismertetése
Hello üzleti szempontok kiterjedő után (lásd: **üzleti ismertetése**) egy energia igényű előrejelzés megoldás, a most már készen áll a toodiscuss hello adatok része dolgozunk. A prediktív elemzési megoldások megbízható adatok támaszkodik. Az előrejelzés igény szerinti energia, azt támaszkodhat különböző szintű részletességű múltbeli adatokkal. A korábbi adatok hello raw anyagot szolgál. Azt is változni fog mely hello adatok tudósok azonosítja majd előre (hivatkozott tooas funkciókat is), végül hoz létre a szükséges hello előrejelzések modellt helyezhetők gondos elemzést.

Ez a szakasz többi hello, azt ismerteti, különféle lépéseit és szempontjait hello adatok megértéséhez hello és hogyan toobring azt tooa használható formában.

### <a name="hello-model-development-cycle"></a>hello modell fejlesztési ciklus
Jó előrejelzési modellek bizonyos gondos előkészületeket és tervezési előállító. Hello modellezési folyamatának be több lépést, és egyszerre csak egy lépésben összpontosító bontásához jelentősen javíthatja hello teljes folyamat hello eredményeit.

hello következő diagram azt ábrázolja, hogyan modellezési folyamatának hello sikerült oszlanak több lépést:

![Modell fejlesztési ciklus](media/cortana-analytics-playbook-demand-forecasting-energy/model-development-cycle.png)

Mivel látható hello ciklus hat lépésekből áll:

* Probléma létrehozását
* Adatfeldolgozást és az adatok feltárása
* Adatok előkészítése és a szolgáltatás műszaki osztály
* Modellezés
* Modell kiértékelése
* Fejlesztés

Hello többi Ez a szakasz azt ismerteti, hello egyes lépéseit, valamint minden egyes lépésnél elemek tooconsider.

### <a name="problem-formulation"></a>Probléma létrehozását
Azt is érdemes lehet hello probléma létrehozását hello egyik legfontosabb lépés igények tootake előzetes tooimplementing bármely prediktív elemzési megoldás. Itt azt kellene átalakítási hello üzleti probléma, és felbontani azt toospecific elemeket, amelyeket adatok használatával, és technikák modellezési megoldhatók. Jó gyakorlat tooformulate hello hiba kérdések készletként tooanswer tapasztalatairól. Az alábbiakban néhány lehetséges kérdésre, előfordulhat, hogy alkalmazhatók az energia igény szerinti előrejelzés hello hatókörén belül:

* Mi az az egyes állomás a következő óránként vagy naponta hello hello várható terhelése?
* Milyen időpontban hello nap a rács tapasztal csúcs igény szerint?
* A rács toosustain várt hello csúcsterhelés valószínűleg hogyan van?
* Hogy mekkora teljesítményre létrehozzon hello power állomás minden órában hello nap során?

Ezeket a kérdéseket kialakítása lehetővé teszi toofocus hello megfelelő adatok és a megoldás, amely teljesen összhangban az elvégzendő hello üzleti probléma. Majd továbbá azt állíthatja be néhány alapvető metrikákat, amelyek lehetővé teszik a számunkra hello modell teljesítményétől tooevaluate hello. Például hogyan pontos kell hello előrejelzési kell, és mi hiba, amely elfogadható hello vállalat által számos hello?

### <a name="data-sources"></a>Adatforrások
hello modern intelligens rács különböző részeit és összetevők hello rács adatait gyűjti. Ezek az adatok hello működési különböző szempontok és hello kihasználtsági hello power rács jelöli. Hello energia igényű előrejelzési hello hatókörbe azt hello tényleges igény szerinti felhasználása tükröző adatforrások hello döntéseken vannak korlátozza. Energiafogyasztás egyik fontos forrása intelligens mérőszámok. Hello földgömb körül segédprogramok intelligens mérőszámok gyorsan telepíti a fogyasztók számára. Intelligens mérőszámok hello tényleges energiafogyasztását rögzítése, és ezen adatok hátsó toohello segédprogram vállalati folyamatosan továbbítják. Adatok gyűjtése és küldött vissza rögzített időközönként, 5 perc too1 óránként kezdve. Speciális intelligens mérőszámok távolról programozása toocontrol és egyenleg hello fogyasztás háztartását belül. Intelligens mérési adatok viszonylag megbízható, és magában foglalja az időbélyegző. Így az előrejelzés igény fontos összetevőként. A mérési adatok összesíthetők (összegzett) hello rács topológia belül különböző szinteken: átalakító, állomás, régió, *stb*. A Microsoft majd kiválaszthatja a szükséges hello összesítési szintű toobuild előrejelzési modell hozzá. Például ha hello segédprogram vállalati volna jövőbeli tooforecast például betölteni a rács alállomások egyes majd összes mérőszámok-adatokat is lehet összesítve van minden egyes állomás és az előrejelzési modell hello bemenetként használja. Belső adatforrásként irányítjuk toosmart mérőszámok.

Megbízható energia igény szerinti előrejelzés más külső adatforrások is támaszkodik. Egy fontos tényező, amely befolyásolja az energiafogyasztás hello időjárási vagy pontosabban hello hőmérséklet. Előzményadatokat erős korrelációs között külső hőmérséklet és energiafogyasztását jeleníti meg. Működés közbeni nyári napban fogyasztókat abban, hogy azok légkondicionálók és hello téli bekapcsolása fűtésrendszerek közben. Egy megbízható forrás hello rács helyen korábbi hőmérséklet, ezért kulcs. Ezenkívül azt is támaszkodniuk hőmérséklet pontos előrejelzés egy előrejelzőjének energiafogyasztás.

Egyéb külső forrásból is hozzájárulhat a energia igény szerinti előrejelzési modellek fejlesztése során. Ezek közé tartozhatnak a hosszú távú ideértve az éghajlatból módosítások gazdaságos indexek (*pl.*, GDP), és másokkal. A jelen dokumentum nem közzétesszük ezek más adatforrásokhoz.

### <a name="data-structure"></a>Adatok szerkezete
Miután az adatforrások azonosító hello szükséges, azt például, hogy a nyers adatokat gyűjtött hello tartalmaz tooensure kijavítja a adatokkal kapcsolatos funkciókkal. az előrejelzési modell toobuild megbízható igény szerinti, igazolnia kell, hogy az összegyűjtött adatok hello tooensure tartalmaz, amelyek segítségével előre jelezni jövőbeli igények hello adatelemek. Az alábbiakban néhány alapvető követelményeivel kapcsolatos hello adatok szerkezete (séma) hello nyers adatok.

hello nyers adatok sorok és oszlopok áll. Minden egyes mérési adatok egyetlen sor jelzi. Minden egyes soraiban levő adatok (is hivatkozott tooas szolgáltatásokat vagy mezők) több oszlopot tartalmaz.

1. **Időbélyeg** – hello Timestamp típusú mező képviseli hello tényleges időpontot, amikor hello mérési rögzítve lett-e. Akkor kell tartania hello közös dátum/idő formátumok egyikét. Dátum és idő is részei tartalmaznia kell. A legtöbb esetben nincs szükség a hello idő toobe rögzített hello második szintű részletesség. Fontos fontos toospecify hello időzóna hello adatok rögzítése történik.
2. **Azonosító mérni** – Ez a mező hello mérő vagy hello készülék azonosítja. Egy kategorikus változó, és számjegyek és karakterek kombinációja lehet.
3. **Felhasználás értéke** – Ez az adott dátum/idő hello fogyasztás. hello fogyasztás mérhető kWh-ban (kilowatt-hour), vagy bármely más elsődleges egység. Fontos kell, amely a következő mértékegység hello toonote között az összes mérték hello adatok konzisztens marad. Bizonyos esetekben fogyasztás lehet biztosítani a több mint 3 power fázisban. Ebben az esetben azt kell toocollect összes hello független fogyasztás fázisban.
4. **Hőmérséklet** – hello hőmérséklet általában gyűjtött független forrásból. Azonban meg kell kompatibilis hello fogyasztási adatokhoz. Szerepelnie kell benne, amelyek lehetővé teszik az időbélyeg fent leírt módon toobe hello fogyasztás adatok szinkronizálva. hello hőmérséklet érték fok Celsius vagy Fahrenheit adható meg, de kell az összes mérték között konzisztens marad.
5. **Hely –** hello a location mező általában hello hely, ahol hello hőmérséklet adatokat összegyűjtötte-e társítva. Irányítószám számként vagy szélességi/hosszúság (lat/hosszú) formátumban ábrázolhatók.

a következő táblák hello mutat egy jó használati és hőmérséklet adatformátum példákat:

| **Dátum** | **Idő** | **A mérési azonosítója** | **1. fázis** | **2. fázis** | **3. fázis** |
| --- | --- | --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |ABC1234 |7.0 |2.1 |5.3 |
| 7/1/2015 |10:00:01 |ABC1234 |7.1 |2.2 |4.3 |
| 7/1/2015 |10:00:02 |ABC1234 |6.0 |2.1 |4.0 |

| **Dátum** | **Idő** | **Hely** | **Hőmérséklet** |
| --- | --- | --- | --- |
| 7/1/2015 |10:00:00 |11242 |24.4 |
| 7/1/2015 |10:00:01 |11242 |24.4 |
| 7/1/2015 |10:00:02 |11242 |24.5 |

Mivel a fent látható, akkor ebben a példában a 3 power fázisaihoz tartozó megjeleníthető 3 eltérő értékeket tartalmaz. Vegye figyelembe azt is, hogy hello dátumot és időpontot tartalmazó mezői elkülönül egymástól, azonban is kombinált egyetlen oszlopba. Ebben az esetben hello hely oszlopban szerepel az 5 jegyű irányítószám formátum és hello hőmérséklet mértékben Celsius formátumban.

### <a name="data-format"></a>Adatok formátuma
Cortana Intelligence Suite támogathatja a leggyakrabban használt adatokat formátumok hello CSV, TSV, JSON, például *stb*. Fontos, hogy hello adatformátum hello hello projekt teljes életciklusát konzisztens marad.

### <a name="data-ingestion"></a>Adatfeldolgozás
Mivel energia igény szerinti előrejelzés folyamatosan és gyakran várhatóan, gondoskodunk róla, hogy hello nyers adat áramlik teli és megbízható adatfeldolgozást eljárással. hello adatfeldolgozást folyamat kell garantálható, hogy hello nyers adatok folyamat előrejelzés szükséges hello időpontban hello érhető el. Ez azt jelenti, hogy a hello adatok adatfeldolgozást gyakoriság gyakoriság előrejelzés hello nagyobbnak kell lennie.

Példa: Ha az előrejelzés megoldást hoz létre egy új előrejelzés naponta 8:00 órakor, akkor szükséges, amely hello során gyűjtött összes adat hello utolsó tooensure igény szerinti 24 órában teljes okozhatnak: adott pontra, és tooeven hello tartalmazza adatok az elmúlt órában.

A rendezés tooaccomplish, Cortana Intelligence Suite kínál különböző módokon toosupport egy megbízható az adatfeldolgozást folyamata. Ez további tárgyalja hello **telepítési** szakasz ebben a dokumentumban.

### <a name="data-quality"></a>Az adatminőségi
a megbízható és pontos igény szerinti előrejelzés végrehajtásához szükséges hello nyers adatforrás néhány alapvető adatok minőségi feltételeknek kell megfelelnie. Bár speciális statisztikai módszerek lehet néhány lehetséges minőségi probléma használt toocompensate, továbbra is szükséges, hogy néhány alapadatokhoz minőségi küszöbérték amikor új adatok bevitele vannak meghaladó tooensure. Az alábbiakban néhány nyers adatok minőségi vonatkozó szempontok:

* **Hiányzó érték** – ez hivatkozik toohello helyzet, amikor adott mérési gyűjtése nem történt meg. hello alapvető itt mérete, hogy hiányzó érték arány hello nem lehet nagyobb, mint 10 % bármely adott időszakban. Esetben egyetlen érték hiányzik, fel kell tüntetni egy előre definiált érték használatával (például: "9999"), és nem "0", amely érvényes érték lehet.
* **Mérési pontosság** – hello tényleges fogyasztás vagy hőmérséklet értéke pontosan feljegyzik. Pontos mérések pontos előrejelzéseket eredményez. Hello mérési hiba általában 1 % relatív toohello IGAZ érték alacsonyabbnak kell lennie.
* **Mérési idő** – megadása kötelező, ezért az hello tényleges időbélyeg hello adatgyűjtés fog nem térhet el több mint 10 másodperc relatív toohello igaz idő hello tényleges mérési által.
* **Szinkronizálás** – Ha több adatforrást használják (*pl.*, felhasználása és hőmérséklet) azt győződjön meg arról, hogy vannak-e nem időszinkronizálást problémák közöttük. Ez azt jelenti, hogy bármely két független adatforrásokból gyűjtött hello időbélyeg közötti különbség nem haladhatja meg a 10 másodpercnél hello ideje.
* **Késés** - kapcsolatban a fentiekben ismertetett, a **adatfeldolgozást**, azt a megbízható adatok folyamata és feldolgozási folyamat függenek. toocontrol, hogy azt győződjön meg arról, hogy azt szabályozzák hello adatkésleltetést. Ez egy megadva hello időeltérése hello hello tényleges mérési készült és hello idő, ahol a Cortana Intelligence Suite tárolási hello betöltése megtörtént, és használatra kész. Rövid távú terhelést előrejelzési hello teljes késést nem lehet nagyobb, mint 30 perc. Hosszú távú terhelést előrejelzési hello teljes késést nem lehet nagyobb, mint 1 nap.

### <a name="data-preparation-and-feature-engineering"></a>Adatok előkészítése és a szolgáltatás műszaki osztály
Miután hello nyers adatok keresztül a szervezetbe (lásd: **adatfeldolgozást**), és biztonságosan tárolt, készen áll a toobe feldolgozott. hello adatok előkészítési fázis alapvetően hello nyers adatok véve, és (átalakítása, átalakításakor) konvertálása hello modellezési fázisban űrlap be azt. Előfordulhat, hogy egyszerű olyan műveleteket tartalmaznak, mint hello nyers adatok oszlop, mert a tényleges mért érték, szabványosított értékek, összetettebb műveleteket, mint az [idő elmaradt](https://en.wikipedia.org/wiki/Lag_operator), stb. az újonnan létrehozott hello adatoszlopok hivatkozott tooas adatokkal kapcsolatos funkciókkal és hello folyamat ezek hivatkozott tooas szolgáltatás mérnöki csapathoz. Ez a folyamat hello végére egy új adatkészlettel kinyert hello nyers adatokat, és nem használható modellezési kellene azt. Ezenkívül hello adatok előkészítési fázis kell tootake hiányzó értékek kiszolgálásához (lásd: **az adatminőségi**) és ellensúlyozza a őket. Néhány esetben azt kell toonormalize hello adatok tooensure jelennek meg minden értékek hello ugyanolyan skála.

Ebben a szakaszban látható néhány hello közös adatok funkciója, amely hello energia meg igény szerint előrejelzési modellek.

**Idő áll a szolgáltatások:** hello dátuma vagy időbélyege adatok származó ezeket a szolgáltatásokat. Ezek kibontott és konvertálni kategorikus szolgáltatások, mint:

* Idő nap – ezt az értékek 0 too23 túlterhelésével hello nap hello órája
* Nap hét – ez hello hello hét napját jelöli, és 1 (vasárnap) too7 (szombat) értékeket fogad
* Nap hónap – ez hello tényleges dátumot jelöli, és 1 too31 értéket vehet
* Hónap év – ez hello hónap jelöli, és 1 (január) too12 (December) értékeket fogad
* Hétvégi – Ez a tulajdonság bináris érték, amely hello értékek 0 létrehozását vagy 1-es a hétvégi
* Szünnap - typedValue hello értékek 0 rendszeres napi vagy 1-es szabadnap a bináris érték funkciója
* Fourier – hello Fourier feltételek súlyok hello időbélyeg származó használati használt toocapture hello szezonalitás értékének (ciklus) hello adatokat a rendszer. Mivel jelenleg több évszak előfordulhat, hogy rendelkezik az adatok több Fourier feltételek előfordulhat, hogy kell. Igény szerinti értékek Előfordulhat például, éves, heti és napi évszak/ciklusok 3 Fourier feltételek eredménye.

**Független mérési funkciókat:** hello független is, hogy szeretnénk toouse kiszámítására, a modellben minden hello adatelemek. Itt azt kizárása hello függő szolgáltatás, amely azt kellene toopredict.

* Lag szolgáltatás – ezek a vette idő hello tényleges igény szerinti értékeit. Lag 1 szolgáltatások például tartsa a hello igény szerinti érték a hello előző órában (óránkénti adatok feltételezve) relatív toohello aktuális időbélyeg. Hasonlóképpen, a lag 3, azt, adja hozzá a lag 2 *stb*. hello lag használt szolgáltatásai minőségén tényleges kombinációja határozza meg hello modellezési fázis során hello modell eredmények értékelése.
* Hosszú távon trendekkel – Ez a funkció igény szerinti év közötti lineáris növekedése hello jelöli.

**A függő szolgáltatás:** hello függő szolgáltatás akkor hello adatok oszlop, amely azt a modell toopredict. A [felügyelt gépi tanulás](https://en.wikipedia.org/wiki/Supervised_learning), igazolnia kell, először a hello modell hello függő funkciókat használ (amely szintén hivatkozott tooas címkék) betanításához. Így hello modell toolearn hello minták hello függő szolgáltatás a hello adatait. Az előrejelzés energia igény szeretnénk általában toopredict hello tényleges igény szerint, és ezért azt használhatja azt hello függő szolgáltatás.

**A hiányzó értékeket kezelése:** hello adatok előkészítési fázis során toodetermine hello legjobb stratégia toohandle hiányzó értékeket kell azt. Ez főleg az használatával hello különféle statisztikai [adatok imputálási módszerek](https://en.wikipedia.org/wiki/Imputation_\(statistics\)). Az előrejelzés energia igény szerinti hello esetben azt általában imputálására hiányzó értékeket az előző elérhető adatpontok mozgóátlag használatával.

**Adatok normalizálási:** adatok normalizálási egy másik típusú átalakítása, amely használt toobring hasonló méretezési az előrejelzési összes numerikus adatok, például igény szerint. Ez általában növeli a pontosság és hello modell pontosságát. Azt kellene általában ehhez hello adattartomány hello hello tényleges érték elosztásával.
Ez fogja csökkentheti hello eredeti érték kisebb tartományba, általában a -1 és 1 között.

## <a name="modeling"></a>Modellezés
hello modellezési fázisban, ha a modell a hello hello adatok átalakításhoz történik. A hello core hiba a folyamat speciális vizsgálati hello előzményadatok (betanítási adatok), bontsa ki a mintákat és a modell létrehozása algoritmusokat. A modell később új adatok, amelyeket nem használt toobuild hello modell használt toopredict lehet.

Miután egy megbízható működő kell majd használhatjuk tooscore új adatok strukturált tooinclude hello modell szükséges szolgáltatásokat (X). hello pontszámításakor hello megőrzött modell (objektum: hello képzési fázisban) használja, és előre jelezni Ŷ megjelölt hello célváltozó biztosítják.

### <a name="demand-forecasting-modeling-techniques"></a>Az előrejelzés modellezési módszereket igény szerint
Hello esetében az előrejelzés biztosítjuk igény szerint van rendezve idő előzményadatoknak használja. A Microsoft általában hivatkozik, amely tartalmazza az idődimenzió hello toodata [time series](https://en.wikipedia.org/wiki/Time_series). hello cél időpont adatsorozat modellezési az toofind idő kapcsolatos trendeket, szezonalitás értékének, automatikus-korrelációs (idővel korrelációs), és állítson össze azokat a modellbe.

Az elmúlt években speciális algoritmusok már fejlett tooaccommodate idő series előrejelzési és tooimprove az előrejelzés pontosságát. Röviden arról lesz szó néhány őket itt.

> [!NOTE]
> Ez a szakasz nincs tervezett toobe használt, betanítási és az előrejelzés – áttekintés egy gép hanem modellezési módszereket, amelyek gyakran használják az igény szerinti előrejelzési rövid áttekintése. További információk és kapcsolatos idő series előrejelzési oktatási anyagok, erősen ajánlott hello online könyv [előrejelzés: alapelvek és eljárás](https://www.otexts.org/book/fpp).
> 
> 

#### <a name="ma-moving-averagehttpswwwotextsorgfpp62"></a>[**MA (mozgóátlag)**](https://www.otexts.org/fpp/6/2)
Mozgóátlag egyike hello első analitikai módszereket is, az idő series előrejelzési használatban van, és továbbra is mai hello leggyakrabban használt módszerek valamelyikét. Egyben a hello alapját fejlettebb, technikák előrejelzés. A mozgóátlag azt vannak előrejelzés hello következő adatpont által átlagosan KB-os hello legutóbbi pontokon – azt jelzi, ahol K mozgóátlaga hello hello sorrendjének keresztül.

hello áthelyezése átlagos módszer az előrejelzési hello simítás hello hatása, és ezért előfordulhat, hogy nem kezeli a hello adatokat is nagy illékonyság.

#### <a name="ets-exponential-smoothinghttpswwwotextsorgfpp75"></a>[**ETS (beállításáról)**](https://www.otexts.org/fpp/7/5)
Az exponenciális simítás (ETS) családba tartozó order toopredict hello következő adatpont legutóbbi adatpontok súlyozott átlagát használó különböző módszer. hello ötlet tooassign nagyobb súlyt toomore legutóbbi értékeket, és fokozatosan csökkentse a régebbi mért értékek súlya. Számos különböző módszer a termékcsalád is van, ezek közé tartoznak a szezonalitás értékének hello adatok kezelését, mint [Vilmos-telek jellemzők határozza metódus](https://www.otexts.org/fpp/7/5).

Ezek a módszerek közül is figyelembe a hello szezonalitás értékének hello adatok.

#### <a name="arima-auto-regression-integrated-moving-averagehttpswwwotextsorgfpp8"></a>[**Elemhez tartozó ARIMA (automatikus regressziós integrált átlagos áthelyezése)**](https://www.otexts.org/fpp/8)
Automatikus regressziós integrált áthelyezése átlagos (ARIMA) egy másik család módszerek a time series előrejelzési általánosan használt. Ez gyakorlatilag egyesíti az auto-regressziós módszerek mozgóátlaga. Automatikus-regressziós módszerek regressziós modell használják az előző idő sorozatértékek megtételével rendelés toocompute hello tovább dátum pontban. Elemhez tartozó ARIMA módszerek is érvényesek a különböző módszereket, amelyek használatával kiszámítása adatpontok hello különbségének és a használata azokat hello eredeti mért érték helyett. Végül, az ARIMA is elérhetővé válnak a fent említett átlagos technikák áthelyezése hello használata. az összes, az alábbi módszerek különböző módokon hello kombináció milyen szerkezetek hello családba tartozó ARIMA módszerek.

ETS mind az ARIMA széles körben használják ma energia igény szerinti előrejelzés és sok más előrejelzési problémákat. Sok esetben ezek a kvóták együtt toodeliver nagyon pontos eredményeket.

**Általános többváltozós regresszió** regressziós modell lehet hello legfontosabb modellezési megközelítés és a gépi tanulás hello tartományon belül. A time series hello környezetében regressziós toopredict hello jövőbeni értékeket használjuk (*pl.*, az igény szerinti). A regressziós azt hello előre lineáris kombinációja igénybe vehet, és ismerje meg, ezek előre hello súlyozását (is hivatkozott tooas együttható) hello képzési folyamat során. hello célja tooproduce az előre jelzett érték lesz előrejelzési regressziós sorra. Regressziós módszerek alkalmasak, ha a célváltozót hello numerikus, és ezért is megfelelő idő series előrejelzési. Nincs regressziós metódusok, mint például a nagyon egyszerű regressziós modell számos [lineáris regressziós](https://en.wikipedia.org/wiki/Linear_regression) és azokat, például a döntési fák, további speciális [véletlenszerű erdők](https://en.wikipedia.org/wiki/Random_forest), [Neural Hálózatok](https://en.wikipedia.org/wiki/Artificial_neural_network), és a súlyozott döntési fák.

Hozhat létre, energia igény szerinti regressziós hibát előrejelzés biztosítanak számunkra a magas fokú rugalmasságot biztosít az adatok egyesíthetők funkciók kiválasztása külső tényezőkkel, például a hőmérséklet és hello tényleges igény szerinti idő adatsorozat adatokból. Kijelölt hello funkciókról bővebb tájékoztatást hello szolgáltatás mérnöki esik szó (lásd: **adatok előkészítése és a szolgáltatás mérnöki**) Ez a forgatókönyv részét.

A megvalósítás és a telepítési energia igény szerinti előrejelzések próbaüzem tapasztalatunk találtunk, amelyeknek hello Azure ml elérhető speciális regressziós modell általában tooproduce hello legjobb eredmények elérése érdekében, és biztosítjuk is.

## <a name="model-evaluation"></a>Modell kiértékelése
Modell kiértékelése rendelkezik belül hello kulcsfontosságú szerepet **modell fejlesztési ciklus**. Ebben a lépésben azt megismerhetők hello modell és a teljesítmény valós adatok ellenőrzése. Hello modellezési lépés során a hello rendelkezésre álló adatok egy részét a hello modell betanítása használjuk. Hello termékértékelési fázisban vesszük hello fennmaradó hello tootest hello adatmodell. Gyakorlatilag azt jelenti, hogy azt vannak etetési hello új modelladatok, amely rendelkezik szerkezetátalakítás lett, és azonos szolgáltatásokat, mint hello képzési dataset hello tartalmazza. Azonban hello érvényesítési folyamat során a Microsoft hello modell toopredict hello cél változóval helyett hello az elérhető célkiszolgálók változó adja meg. Gyakran irányítjuk, mint a modell pontozása toothis folyamat. Hello igaz célértékek, és hasonlítsa össze azokat a hello előre jelezni azokat, majd volna használjuk. hello cél toomeasure és hello előrejelzési hiba, tehát hello előrejelzéseket és hello IGAZ érték hello különbségének minimalizálása érdekében. Kulcs mennyiségi meghatározására hello hiba mérési azért, mert azt kívánja toofine-hangolási hello modellt, majd ellenőrizze, hogy hello hiba ténylegesen csökken. Tanulási folyamat hello szabályozó Modellparaméterek módosításával, vagy hozzáadásával vagy eltávolításával az adatokkal kapcsolatos funkciókkal végezhető finomhangolási hello modell (tooas említett [paraméterek ismétlés](https://channel9.msdn.com/Blogs/Azure/Data-Science-Series-Building-an-Optimal-Model-With-Parameter-Sweep)). Gyakorlatilag, amely azt jelenti, hogy azt is kell tooiterate közötti hello szolgáltatás mérnöki csapathoz, modellezését, a modell kiértékelése fázisok többször mindaddig, amíg folyamatban, képes tooreduce hello hibaszintet toohello szükséges.

Fontos, hogy hello előrejelzési hiba soha nem lesznek nulla is vannak tooemphasis soha nem modell, amely tökéletesen képes előre jelezni minden eredménye. Van azonban bizonyos nagysága hello üzleti által elfogadható hiba. Hello érvényesítési folyamat során, hogy a modell előrejelzéses hiba hello szint, vagy jobban hello üzleti tolerancia szinten tooensure tapasztalatairól. Ezért fontos tooset hello szintű hello megengedhető hiba hello hello elején ciklus során hello **probléma létrehozását** fázisban.

### <a name="typical-evaluation-techniques"></a>Tipikus értékelési eljárások
A mely előrejelzési hiba mérhető és mennyiségi különböző módja van. Ebben a szakaszban tárgyaljuk hello vitafórum értékelési technikák vonatkozó tootime adatsorozat és a vonatkozó előrejelzési energia igény.

#### <a name="mapehttpsenwikipediaorgwikimeanabsolutepercentageerror"></a>[**MAPE**](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)
MAPE rövidítése abszolút százalékos hiba jelenti. MAPE azt a hello különbség minden előre jelzett pont és a tényleges érték hello, az adott pont vannak számítástechnikai. Azt majd számlálása hello hiba pontonkénti által hello hányadát hello különbség relatív toohello tényleges érték kiszámításakor. Az utolsó lépésben azt átlagos ezeket az értékeket. hello matematikai MAPE használt képlete hello következő:

![MAPE képlet](media/cortana-analytics-playbook-demand-forecasting-energy/mape-formula.png)
*ahol A<sub>t</sub> hello tényleges érték, F<sub>t</sub> hello előre jelzett érték, és n hello előrejelzési horizon.*

## <a name="deployment"></a>Környezet
Amennyiben azt le fázis modellezési hello rögzített, és hello modell teljesítmény érvényesítve vagy történő hello központi telepítési fázis kész toogo folyamatban. Ebben a környezetben központi telepítés azt jelenti, hogy hello ügyfél engedélyezése tooconsume hello modell futtatja a tényleges előrejelzéseket, nagy méretekben. hello telepítési fogalma kulcsot az Azure ML óta fő célunk tooconstantly meghívása előrejelzéseket, megakadályozását toojust hello insight beszerzése hello adatok. hello központi telepítése fázisban megtörténik hello részét, ahol engedélyezzük hello modell toobe nagy léptékű felhasznált.

Előrejelzési energia igényű hello környezeten belül a célja tooinvoke folyamatos és rendszeres időközönként előrejelzések úgy, hogy friss adatokat hello modell érhető el, és adott hello előrejelzett hátsó toohello fogyasztó ügyfél adatküldés közben.

### <a name="web-services-deployment"></a>A webes szolgáltatások telepítése
hello fő telepíthető építőelem Azure ml egy hello webes szolgáltatás. Ez a hello leghatékonyabb módon tooenable fogyasztásának a prediktív modell hello felhőben. hello webszolgáltatás magában foglalja a hello modell és foglalja össze az olyan [RESTful](http://www.restapitutorial.com/) API (Application Programming Interface). hello API bármely ügyfélkódot, amint az alábbi ábrán hello részeként használható.

![Azt a szolgáltatás telepítését és használat](media/cortana-analytics-playbook-demand-forecasting-energy/web-service-deployment-and-consumption.png)

Amint is látható, a hello webszolgáltatás hello Cortana Intelligence Suite felhőben üzemel, és az elérhetőségi REST API végpontra is elindítható. Különböző típusú ügyfeleket különböző tartományok közötti hívhat meg egyidejűleg hello szolgáltatást hello Web API keresztül. hello webszolgáltatás támogatja az egyidejű hívások ezer is méretezhető.

### <a name="a-typical-solution-architecture"></a>Egy tipikus megoldás architektúrája
Az előrejelzés megoldás energia igény szerinti telepítésekor dolgozunk egy záró tooend megoldás, amely túllép hello előrejelzés webes szolgáltatás, és elősegíti a hello teljes adatfolyama üzembe helyezése iránt érdeklődik. Új előrejelzés meghívása a Microsoft hello időpontban toomake meg arról, hogy hello modell táplált a naprakész adatokkal kapcsolatos funkciókkal rendelkező kellene azt. Ez azt jelenti, hogy hello újonnan összegyűjtött nyers adatok rendszer folyamatosan okozhatnak, dolgoz fel, és átalakítja az hello szükséges szolgáltatáskészletet a hello modellt lett létrehozva. At hello azonos időben, azt szeretné, például toomake hello előrejelzett érhető el adat a hello end fogyasztó ügyfelek. Egy példa adatok folyamata ciklus (vagy adatok pipeline) mutatja be az alábbi ábrán hello:

![Data Flow energia igény szerinti előrejelzési End tooEnd](media/cortana-analytics-playbook-demand-forecasting-energy/energy-demand-forecase-end-data-flow.png)

Hello lépéseket hello energia igény szerinti előrejelzési ciklus részeként kerül sor az alábbiak:

1. Központilag telepített adatok mérőszámok több millió valós idejű energiafogyasztási adatokkal folyamatosan generál.
2. Folyamatban van a ezeket az adatokat gyűjt, és egy felhőalapú-tárházat töltve (*pl.*, Azure Blob).
3. Mielőtt feldolgozhatóvá válna, a nyers adatok hello összesített tooa állomás vagy regionális szintű hello üzleti által meghatározott.
4. hello szolgáltatás feldolgozás (lásd: **adatok előkészítése és a szolgáltatás feldolgozási**) majd kerül sor, és szükséges előállított hello adatok modell betanítása vagy pontozási – hello szolgáltatás adatokat adatbázisban tárolja (*pl.*, Az SQL Azure).
5. hello szolgáltatás újra betanítása meghívták toore-vonat hello előrejelzési modell – frissített hello modell verzióját is, hogy a webszolgáltatás pontozási hello használható megőrződjenek.
6. webszolgáltatás pontozási hello meghívták, amely megfelel a szükséges hello előrejelzési gyakoriság ütemezés szerint.
7. hello előrejelzett adatok hello end fogyasztás ügyfél által elérhető adatbázisban tárolja.
8. hello fogyasztás ügyfél hello előrejelzések lekéri, alkalmazza azt hello rács programba, és fel azt szükséges hello használati eset megfelelően.

Fontos, hogy a teljes ciklus teljesen automatizált, és ütemezés szerint fut, fontos toonote. az adatciklus hello teljes összehangolását eszközök segítségével végezhető [Azure Data Factory](http://azure.microsoft.com/services/data-factory/).

### <a name="end-tooend-deployment-architecture"></a>Befejező tooEnd üzembe helyezési architektúrája
A sorrend toopractically központilag telepíteni egy energia igény szerinti előrejelzési megoldás a Cortana Intelligence, igazolnia kell, hogy a szükséges hello összetevők létrehozott és megfelelően konfigurálva tooensure.

hello következő diagram azt ábrázolja, egy tipikus Cortana Intelligence alapú architektúra, amely megvalósítja és koordinálja a fent ismertetett hello-folyamat ciklus:

![Befejező tooEnd üzembe helyezési architektúrája](media/cortana-analytics-playbook-demand-forecasting-energy/architecture.png)

Hello összetevők és hello teljes architektúra kapcsolatos információkért tekintse meg az toohello energia Megoldássablonban.

