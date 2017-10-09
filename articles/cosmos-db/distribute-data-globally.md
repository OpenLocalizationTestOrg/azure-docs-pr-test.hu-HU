---
title: "globális Azure Cosmos DB aaaDistribute adatok |} Microsoft Docs"
description: "Ismerje meg a globális adatbázisokat az Azure Cosmos Adatbázisból, egy globálisan elosztott, mutli-modell dokumentumadatbázis-szolgáltatás segítségével bolygónk méretű georeplikáció, a feladatátvételt és az adatok helyreállítás."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: ba5ad0cc-aa1f-4f40-aee9-3364af070725
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/14/2017
ms.author: arramac
ms.openlocfilehash: b50e8433dc7e70c54d68c4c2f99954a13f4951f4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodistribute-data-globally-with-azure-cosmos-db"></a>Hogyan globálisan Azure Cosmos DB toodistribute adatok
Azure a széles körű – folyamatosan bővülő, és egy globális erőforrásigényét tért 30 + földrajzi régiók között. A világ jelenlétét megkülönböztetett hello képességek az Azure kínál tooits fejlesztők egyik hello képességét toobuild, telepítéséhez és felügyeletéhez a globálisan elosztott alkalmazások könnyen. 

Az [Azure Cosmos DB](../cosmos-db/introduction.md) a Microsoft globálisan elosztott, többmodelles adatbázis-szolgáltatása az alapvető fontosságú alkalmazásokhoz. Az Azure Cosmos DB biztosít kulcsrakész globális terjesztési, [átviteli sebesség és tárterület a rugalmas méretezést](../cosmos-db/partition-data.md) világszerte, egyjegyű ezredmásodperces késések: hello 99th PERCENTILIS, [öt jól meghatározott konzisztenciaszintek ](consistency-levels.md), és magas rendelkezésre állás érdekében minden biztonsági mentés által garantált [iparágvezető SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Az Azure Cosmos DB [automatikusan elvégzi az adatok](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) anélkül, hogy Ön séma- és index felügyeleti toodeal. Többmodelles szolgáltatás, amely támogatja a dokumentumokat, a kulcs-értékeket, a diagramokat és az oszlopos adatmodelleket. Szolgáltatásként felhő született Azure Cosmos DB van gondosan fejthetők vissza a több-bérlős és a globális terjesztése szabad hello.

**Egyetlen Azure Cosmos DB gyűjtemény particionálta és elosztva a több Azure-régiók**

![Az Azure Cosmos DB gyűjtemény particionálta és elosztva három régiók](./media/distribute-data-globally/global-apps.png)

Ahogy rendelkezik Azure Cosmos DB felépítésekor, globális terjesztési nem lehet utólag – nem lehet "menetes-on" egy "egyszeri" adatbázis helyrendszer elveire. globálisan elosztott adatbázis által biztosított hello képességeket span felett marad, hogy a hagyományos földrajzi katasztrófa földrajzi--helyreállítási által kínált "egyhelyes" adatbázisok. Egyetlen hely adatbázisok ajánlat földrajzi-vész-Helyreállítási képességet globálisan elosztott adatbázisok szigorú részhalmazai. 

Azure Cosmos DB kulcsrakész globális terjesztési, fejlesztőknek nem kell toobuild saját replikációs állványok vagy hello Lambda mintát alkalmazásával (például [AWS DynamoDB replikációs](https://github.com/awslabs/dynamodb-cross-region-library/blob/master/README.md)) hello adatbázis naplója keresztül vagy a Ennek során "dupla írások" különféle régiókban. Jelenleg nem javasoljuk ezen módszerek, mert ilyen megközelítések lehetetlen tooensure helyességét, és adja meg a hang SLA-k. 

Ebben a cikkben nyújtunk Azure Cosmos DB globális terjesztési funkcióiról. Azt is ismertetik Azure Cosmos DB egyedi megközelítés tooproviding átfogó SLA-k. 

## <a id="EnableGlobalDistribution"></a>Kulcsrakész globális terjesztési engedélyezése
Az Azure Cosmos DB hello következőket biztosítja képességek tooenable meg tooeasily írási bolygónk méretű alkalmazások. Ezek a képességek érhetők el hello Azure Cosmos DB erőforrás-szolgáltató alapú [REST API-k](https://docs.microsoft.com/rest/api/documentdbresourceprovider/) valamint hello Azure-portálon.

### <a id="RegionalPresence"></a>A széles körű regionális jelenléte 
Azure folyamatosan nő a földrajzi jelenléte hozásával [új régiók](https://azure.microsoft.com/regions/) online. Alapértelmezés szerint minden új Azure régiók Cosmos. Azure-adatbázis nem áll rendelkezésre. Ez lehetővé teszi tooassociate Azure Cosmos DB adatbázis fiókjához egy földrajzi régiót, amint Azure hello új régió vállalati nyílik meg.

**Alapértelmezés szerint az összes Azure-régiók Azure Cosmos DB áll rendelkezésre**

![Az Azure Cosmos DB érhető el az összes Azure-régiók](./media/distribute-data-globally/azure-regions.png)

### <a id="UnlimitedRegionsPerAccount"></a>Az Azure Cosmos DB adatbázisfiók régiók korlátlan számú társítása
Az Azure Cosmos DB tooassociate lehetővé teszi tetszőleges számú Azure-régiók Cosmos helyen az Azure-adatbázis adatbázis-fiók. Geokerítések korlátozások (például a kínai, a németországi) kívül vannak, amely Azure Cosmos DB adatbázis fiókhoz tartozó régiók hello száma nincs korlátozva. a következő ábra hello egy adatbázis konfigurált fiók toospan 25 Azure-régiók közötti jeleníti meg.  

**A bérlő Azure Cosmos-adatbázis adatbázis-fiók feszítőfa 25 Azure-régiók**

![25 Azure-régiók átfedés Azure Cosmos DB adatbázisfiók](./media/distribute-data-globally/spanning-regions.png)

### <a id="PolicyBasedGeoFencing"></a>Csoportházirend-alapú geokerítések
Azure Cosmos DB tervezett toohave csoportházirend-alapú geokerítések képességek. Geokerítések egy fontos összetevő tooensure irányítási és a megfelelőségi korlátozásokat, és előfordulhat, hogy egy adott területre társít a fiókját. Példák a geokerítések (többek korlátozni), hatókör a globális terjesztési toohello régiók szuverén felhő (például a kínai és a németországi), vagy a kormányzati adózás határ (például Ausztrália) belül. hello házirendek az Azure-előfizetés hello metaadatok segítségével szabályozza.

### <a id="DynamicallyAddRegions"></a>Dinamikus hozzáadása és eltávolítása a régiók
Azure Cosmos DB lehetővé teszi a tooadd (associate), vagy távolítsa el (a társítást) régiók tooyour adatbázisfiók idő bármely pontján (lásd: [előző ábra](#UnlimitedRegionsPerAccount)). Címtár adatok replikálása partíció párhuzamosan, Azure Cosmos DB biztosítja, hogy egy új régió online állapotba kerül, ha Azure Cosmos DB elérhető 30 percen belül bárhol a hello world az too100 több TB-nyi fel. 

### <a id="FailoverPriorities"></a>Feladatátvételi prioritások
pontos sorrendjét toocontrol regionális feladatátvétel több regionális kimaradás, Azure Cosmos DB esetén lehetővé teszi a tooassociate hello kiemelt toovarious régiók hello adatbázis fiókhoz társított (lásd a következő ábra hello). Azure Cosmos DB biztosítja, hogy hello automatikus feladatátvételre kerül sor a megadott hello prioritási sorrendben. Regionális feladatátvétel kapcsolatos további információkért lásd: [az üzletmenet folytonossága érdekében az Azure Cosmos Adatbázisba automatikus regionális feladatátvétel](regional-failover.md).

**A bérlő Azure Cosmos-adatbázis hello feladatátvételi prioritási sorrendben (jobb oldali ablaktábla) konfigurálhatja egy adatbázis-fiókjához társított régiókhoz**

![Feladatátvételi prioritások konfigurálása az Azure Cosmos DB](./media/distribute-data-globally/failover-priorities.png)

### <a id="OfflineRegions"></a>Dinamikusan amelynél "elérhető" terület
Azure Cosmos DB teszi lehetővé az adatbázis offline fiók egy adott régióban, és újra elérhetővé később tootake. Megjelölt offline régiók nem aktívan részt vesz a replikációban, és nem hello feladatátvételi feladatütemezési részét képezik. Ez lehetővé teszi toofreeze hello legutóbbi ismert helyes adatbázis kép valamelyik hello olvassa el a régiókban, mielőtt végrehajtaná potenciálisan veszélyes tooyour alkalmazás frissíti.

### <a id="ConsistencyLevels"></a>Több, jól meghatározott konzisztencia modellek globálisan replikált adatbázisok
Azure Cosmos-adatbázis közzététele [több jól meghatározott konzisztenciaszintek](consistency-levels.md) SLA-k által támogatott. Kiválaszthatja, hogy egy adott konzisztencia modell (a beállítások listájának elérhető hello) függően hello munkaterhelés/forgatókönyvek. 

### <a id="TunableConsistency"></a>Globálisan replikált adatbázisok hangolható konzisztencia
Azure Cosmos DB tooprogrammatically felülbírálás lehetővé teszi, és hello alapértelmezett konzisztencia választás / kérés alapon futásidőben enyhíteni. 

### <a id="DynamicallyConfigurableReadWriteRegions"></a>Dinamikusan konfigurálható olvasási és írási régiók
Azure Cosmos DB lehetővé teszi tooconfigure hello régiók (hello adatbázishoz tartozó) "Olvasás", "write" vagy "olvasási/írási" régiók. 

### <a id="ElasticallyScaleThroughput"></a>Azure-régiók közötti rugalmasan méretezési átviteli sebesség
Rugalmasan méretezhető egy Azure Cosmos DB gyűjtemény által létesítési átviteli programozott módon. hello átviteli alkalmazott tooall hello régiók hello gyűjtemény terjesztése.

### <a id="GeoLocalReadsAndWrites"></a>Földrajzi helyi olvasása és írása
hello globálisan elosztott adatbázis legfontosabb előnye toooffer kis késésű hozzáférést toohello adatok bárhol hello world. Azure Cosmos DB kínál a kis késleltetésű garanciák, P99 különböző adatbázis-műveleteknél. Azt biztosítja, hogy az összes olvasási irányított toohello legközelebbi helyi olvasási régióban. tooserve egy olvasási kérést hello kvórum helyi toohello régióban, amelyben olvasási hello kiadott szolgál; hello Ugyanez vonatkozik toohello írási műveleteket. Az írási elfogadott, csak azt követően replikák többsége tartósan véglegesítette hello írási helyileg, de éppen által kezdeményezett távoli replikák tooacknowledge hello írására nélkül. Hello replikációs protokoll Azure Cosmos-adatbázis alatt hello feltételezve, hogy a hello olvasási és írási határozatképességére a rendszer mindig a helyi toohello olvasható, és írja, régiók, mely hello kérés kiadása másképp helyezéséhez működik.

### <a id="ManualFailover"></a>Manuális kezdeményezése a regionális feladatátvétel
Azure Cosmos DB lehetővé teszi a tootrigger hello feladatátvétele hello adatbázis fiók toovalidate hello *end tooend* hello teljes alkalmazás (túl hello adatbázis) rendelkezésre állási tulajdonságait. Mivel mindkét hello biztonsági és hello hiba észlelése és a vezető választás liveness tulajdonságainak garantáltan, biztosítja, hogy az Azure Cosmos DB *nulla adatvesztés* kézi feladatátvételre bérlői által kezdeményezett művelet.

### <a id="AutomaticFailover"></a>Automatikus feladatátvétel
Azure Cosmos-adatbázis támogatja az automatikus feladatátvételt egy vagy több regionális kimaradások esetén. Egy regionális feladatátvétel során Azure Cosmos DB olvasási késése, hasznos üzemidő elérhetőségét, konzisztencia, és a szolgáltatásiszint-szerződések átviteli tart fenn. Azure Cosmos-adatbázis egy automatikus feladatátvételi művelet toocomplete hello időtartama Ez egy felső határa. Ez az esetleges adatvesztés hello ablakának hello regionális kimaradás során.

### <a id="GranularFailover"></a>Másik feladatátvevő Granularitás van tervezve
Jelenleg a automatikus hello és kézi feladatátvételre képességek érhetők el hello részletességű hello adatbázis-fiók. Vegye figyelembe, belső Azure Cosmos DB tervezett toooffer *automatikus* feladatátvételi egyeztetését részletességű adatbázis, gyűjtemény vagy akár partíciójának (a gyűjtemény tulajdonos a kulcsokat számos). 

### <a id="MultiHomingAPIs"></a>Az Azure Cosmos DB többhelyű API-k
Azure Cosmos DB lehetővé teszi a toointeract hello Database segítségével logikai (régió független) vagy a fizikai (régióspecifikus) végpontok. A logikai végpontok biztosítja, hogy hello alkalmazás átlátható módon lehet többhelyű feladatátvétel esetén. Ez utóbbi, fizikai végpontok hello, adja meg a részletesebb vezérlés toohello alkalmazás tooredirect beolvassa és toospecific régiók.

Hogyan tooconfigure olvasási hello beállításait a információt [DocumentDB API](../cosmos-db/tutorial-global-distribution-documentdb.md), [Graph API](../cosmos-db/tutorial-global-distribution-graph.md), [tábla API](../cosmos-db/tutorial-global-distribution-table.md), és [MongoDB API](../cosmos-db/tutorial-global-distribution-mongodb.md)azoknak a kapcsolódó cikkeket.

### <a id="TransparentSchemaMigration"></a>Átlátható és konzisztens legyen az adatbázis-séma- és index áttelepítése 
Azure Cosmos-adatbázis található teljesen [sémát független](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf). hello egyedi tervezése az adatbázis-kezelő lehetővé teszi, hogy tooautomatically, és szinkron módon, anélkül, hogy semmilyen sémát, illetve másodlagos indexek, az Ön ingests hello adatok indexelése. Ez lehetővé teszi, hogy Ön tooiterate a globálisan elosztott alkalmazás gyorsan aggódni az adatbázisban séma- és index áttelepítési vagy összehangolása a sémamódosítások több fázisban alkalmazás végrehajtása nélkül. Azure Cosmos DB garantálja, hogy a módosítások tooindexing szabályzatoknak tett, nem lesz a teljesítmény vagy a rendelkezésre állási teljesítménycsökkenése.  

### <a id="ComprehensiveSLAs"></a>Átfogó SLA-k (túl csak magas rendelkezésre állás)
Globálisan elosztott adatbázis szolgáltatásként Azure Cosmos DB kínál jól meghatározott SLA **adatvesztés**, **rendelkezésre állási**, **P99 várakozási**, **átviteli sebesség**  és **konzisztencia** hello adatbázis egészére, függetlenül attól, régiók hello adatbázishoz tartozó hello száma.  

## <a id="LatencyGuarantees"></a>Késés garanciák
a globálisan elosztott adatbázis szolgáltatás a like Azure Cosmos DB toooffer kis késésű hozzáférést tooyour adatok bárhol hello world hello fő előnyt. Azure Cosmos-adatbázis biztosít garantált kis késleltetésű P99, különböző adatbázis-műveleteknél. Azure Cosmos DB által hello replikációs protokoll biztosítja, hogy hello adatbázis-műveletek (ideális esetben olvas és ír) mindig megtörténik hello régió helyi toothat hello ügyfél. SLA-t az Azure Cosmos DB magában foglalja a P99 hello késéssel is tesz az olvasási és írási (szinkron) indexelt, és lekérdezi a kérelem-válasz különböző méretű. hello késés garantálja az írási műveletek közé tartozik a tartós többsége kvórum véglegesítések hello helyi adatközpontban.

### <a id="LatencyAndConsistency"></a>Késleltetés a kapcsolat konzisztencia 
Az egy globálisan elosztott szolgáltatás toooffer erős konzisztenciával globálisan elosztott beállítása, toosynchronously replikálása hello írási műveleteket kell vagy szinkron hajtsa végre a kereszt-régió olvasások – világos és hello nagy kiterjedésű hálózat megbízhatóságát kívánják hello sebessége késések magas és alacsony rendelkezésre állási adatbázis-művelet eredménye, hogy az erős konzisztencia. Emiatt rendelés toooffer garantált alacsony késleltetésű P99 és 99.99 rendelkezésre állása, a hello szolgáltatás foglalkoztat aszinkron replikációját. Ez szolgálna szükséges hello szolgáltatást nyújt továbbá kell [jól meghatározott, laza konzisztencia choice(s)](consistency-levels.md) – mint erős gyengébb (toooffer alacsony késleltetés és a rendelkezésre állási garanciák) és ideális erősebb, mint a "végleges" konzisztencia () toooffer egy egyszerűen elsajátítható programozási modell).

Azure Cosmos DB biztosítja, hogy egy olvasási művelet nem szükséges toocontact replikák több régiók toodeliver hello adott konzisztencia szintű garancia között. Hasonlóképpen biztosítja, hogy egy írási művelet nem get tiltsa le közben hello adatokat a replikált (azaz írási műveleteket aszinkron módon replikálódnak régiók) összes hello régiók között. Több területi adatbázis fiókok több laza konzisztenciaszintek érhetők el. 

### <a id="LatencyAndAvailability"></a>Rendelkezésre állási várakozási ideje a kapcsolattal 
Késleltetés és a rendelkezésre állási hello két oldalán hello azonos érme. Döntésről bővebben hello művelet stabil állapotot és rendelkezésre állása, a hibák hello tapasztalt késést. Hello alkalmazás tükrözik a lassú futó adatbázis-művelet nem különböztethetők meg egy adatbázist, amely nem érhető el. 

toodistinguish nagy késleltetésű elérhetetlensége, Azure Cosmos DB a késés különböző adatbázis-művelet biztosít egy abszolút felső határa. Ha adatbázis-művelet hello hello felső határa toocomplete hosszabb időbe telik, az Azure Cosmos DB időtúllépési hiba adja vissza. hello Azure Cosmos DB SLA-elérhetőséget biztosítja, hogy adott hello időtúllépések számlálási hello rendelkezésre állási SLA ellen. 

### <a id="LatencyAndThroughput"></a>Átviteli várakozási ideje a kapcsolattal
Azure Cosmos-adatbázis nem tesz, válassza ki a késés és átviteli között. Mindkét P99 várakozási hello SLA eleget tegyen, és rendelkezik kiépített hello átviteli sebesség biztosításához. 

## <a id="ConsistencyGuarantees"></a>Konzisztencia biztosítja
Hello közben [az erős konzisztencia modell](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) hello arany szabvány a programozhatóság, ismét hello elsajátíthatják a használatát ára nagy késleltetésű (stabil állapotban) és a rendelkezésre állás (hello tapasztalt hibák) adatvesztés. 

Azure Cosmos-adatbázis egy jól meghatározott programozási modell tooyou tooreason kapcsolatos replikált adatok konzisztencia nyújtja. A rendezés tooenable meg toobuild többhelyű alkalmazások, hello konzisztencia modellek Azure Cosmos Database által elérhetővé tett tervezett toobe régió-függetlenek, és hello régió, ahol hello olvasási és írási is lehetséges a kiszolgálása nem függ. 

Azure Cosmos-adatbázis konzisztencia SLA garantálja, hogy az olvasási kérések 100 %-os hello konzisztencia növekvő (hello alapértelmezett konzisztenciaszint hello adatbázis fiók, vagy felül hello érték hello kérésére kérte hello konzisztenciaszint felel meg ). Egy olvasási kérést teljesül toohave hello konzisztencia SLA-t tekintendő, ha teljesülnek hello konzisztenciaszint társított összes hello konzisztencia biztosítja. hello következő táblázatban rögzíti, amelyek megfelelnek az Azure Cosmos DB által kínált toospecific konzisztenciaszintek hello konzisztencia biztosítja.

**Az Azure Cosmos Adatbázisba adott konzisztenciaszint társított konzisztencia biztosítja**

<table>
    <tr>
        <td><strong>Konzisztenciaszint</strong></td>
        <td><strong>Konzisztencia-jellemzők</strong></td>
        <td><strong>SLA</strong></td>
    </tr>
    <tr>
        <td rowspan="3">Munkamenet</td>
        <td>Olvassa el a saját írási</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Monoton olvasása</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Egységes előtagja</td>
        <td>100%</td>
    </tr>
    <tr>
        <td rowspan="3">A kötött elavulási</td>
        <td>Monoton (belül régiónként) olvasása</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Egységes előtagja</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Kötött elavulási &lt; K, T</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Egységes előtagja</td>
        <td>Egységes előtagja</td>
        <td>100%</td>
    </tr>
    <tr>
        <td>Erős</td>
        <td>Linearizable</td>
        <td>100%</td>
    </tr>
</table>

### <a id="ConsistencyAndAvailability"></a>Rendelkezésre állási konzisztencia a kapcsolattal
Hello [lehetséges eredmény](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf) a hello [CAP tétel](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf) bizonyítja hello rendszer tooremain elérhető és ajánlat linearizable következetes hello tapasztalt hibák valóban nem lehetséges. adatbázis-szolgáltatás hello ki kell választania a toobe CP vagy AP - CP-rendszerek leírtnál kisebb linearizable konzisztencia helyett rendelkezésre állási közben hello AP rendszerek leírtnál kisebb [linearizable konzisztencia](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) rendelkezésre állási helyett. Az Azure Cosmos DB soha nem sérti hello konzisztenciaszint, így a hivatalos CP rendszer kért. Azonban a gyakorlatban konzisztencia nincs egy mindent vagy semmit felkínált – több jól meghatározott konzisztencia modellek mentén hello konzisztencia pontszámot linearizable és a végleges konzisztencia között van. Azure Cosmos DB, a Microsoft kísérelt tooidentify hello számos konzisztencia modellek enyhíteni valós alkalmazhatósági és egy egyszerűen elsajátítható programozási modellt. Azure Cosmos DB hello konzisztencia-rendelkezésre állási mellékhatásokkal navigál 99.99 rendelkezésre állási szolgáltatási szintek, valamint felajánlásával [több enyhíteni még jól meghatározott konzisztenciaszintek](consistency-levels.md). 

### <a id="ConsistencyAndAvailability"></a>Késés konzisztencia a kapcsolattal
Prof. Daniel Abadi által javasolt egy átfogóbb változata kap, és azt nevezzük [PACELC](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf), amely is késleltetés és a konzisztencia mellékhatásokkal stabil állapotban fiókjainak. Hello adatbázisrendszer válasszon, amely meghatározza a stabil állapotban, konzisztencia és a késleltetés között. Több laza konzisztencia modellek (biztonsági aszinkron replikációját, valamint a helyi olvasási, írási határozatképességére) Azure Cosmos DB ellenőrzi, hogy minden olvasási és írási helyi toohello olvasási és régiók rendre írása.  Ez lehetővé teszi, hogy Azure Cosmos DB toooffer kis késleltetésű biztosítja, hogy a hello konzisztenciaszintek hello régión belül.  

### <a id="ConsistencyAndThroughput"></a>Átviteli sebesség konzisztencia a kapcsolattal
Mivel egy adott konzisztencia modell hello végrehajtásának hello választott függ egy [kvórum típus](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf), hello átviteli is konzisztencia hello választott alapján változik. Például az Azure Cosmos Adatbázisba, hello az erős következetes olvasás átviteli idővel konzisztenssé olvasások körülbelül fél toothat. 
 
**Olvasási kapacitást biztosít az Azure Cosmos Adatbázisba meghatározott konzisztenciaszint kapcsolat**

![Konzisztencia- és átviteli közötti kapcsolat](./media/distribute-data-globally/consistency-and-throughput.png)

## <a id="ThroughputGuarantees"></a>Biztosítja, hogy az átviteli sebesség 
Azure Cosmos DB lehetővé teszi tooscale átviteli sebesség (valamint, tárolás), rugalmasan hello igény szerinti függően különböző régiókban között. 

**Egyetlen Azure Cosmos DB gyűjtemény (között három szilánkok) particionált, és ezután elosztva három Azure-régiók**

![Elosztott Cosmos. Azure-adatbázis és a particionált gyűjtemények](../cosmos-db/media/introduction/azure-cosmos-db-global-distribution.png)

Egy Azure Cosmos DB gyűjteményt lekérdezi terjesztéséhez két dimenzió – régión belül és régiók között. Ezt a következőképpen teheti meg: 

* Egyetlen régión belül egy Azure Cosmos DB gyűjteményt van horizontálisan felskálázott erőforrás partíciók tekintetében. Mindegyik erőforrás partíció kulcsok készlete kezeli, és erős következetes és magas rendelkezésre állású virtuálisgép-replikációt állapot replikák egy készlete közötti címtár. Azure Cosmos-adatbázis egy olyan teljes szabályozott erőforrás rendszer, ahol erőforrás-partíció felelős kiveheti a részét a rendszer lefoglalt erőforrások tooit hello költségvetés átviteli kézbesítéséhez. egy Azure Cosmos DB gyűjtemény skálázás hello az teljes mértékben transzparens – Azure Cosmos DB hello erőforrás partíciók kezeli, és felosztja, és egyesíti az igény szerint. 
* Minden olyan hello erőforrás partíció majd több régióba pontjain. Tulajdonos erőforrás partíciók ugyanazokkal a kulcsokkal hello különböző régiókban űrlap partíció set keresztül (lásd: [előző ábra](#ThroughputGuarantees)).  Erőforrás-partíció a partíción belül található koordinált állapot virtuálisgép-replikációt használ hello között több régióba. Attól függően, hogy a konfigurált hello konzisztenciaszint hello erőforrás partíciók a partíción belül dinamikusan használatával konfigurálhatók különböző topológiával (például csillag, lánckapcsolású-lánc, fa stb.). 

Egy válaszidejű partíció felügyeleti, terheléselosztás és szigorú erőforrás irányítás Azure Cosmos DB lehetővé teszi tooelastically méretezési átviteli egy Azure Cosmos DB gyűjteményt a több Azure-régiók között. Egy gyűjtemény változó átviteli sebességet hasonlít egy futtatási művelet az Adatbázisba az Azure Cosmos - egyéb adatbázis műveletek Azure Cosmos DB biztosítja, hogy a toochange hello átviteli sebesség késése hello abszolút felső határa. Tegyük fel a következő ábra hello tartalmazza az ügyfél gyűjtemény rugalmasan kiosztott átviteli sebesség (beállításnak 1M - 10M kérelmek/másodperc közötti két régióban is) hello igény szerint.
 
**Egy felhasználói gyűjteménybe, és rugalmasan kiosztott átviteli sebesség (1M - 10M kérelmek/másodperc)**

![Az Azure Cosmos DB rugalmasan kiosztott átviteli sebesség](./media/distribute-data-globally/elastic-throughput.png)

### <a id="ThroughputAndConsistency"></a>Átviteli sebesség a kapcsolat konzisztencia 
Ugyanaz, mint a [konzisztencia tartozó kapcsolat átviteli](#ConsistencyAndThroughput).

### <a id="ThroughputAndAvailability"></a>Átviteli sebesség a kapcsolatot a rendelkezésre állást
Azure Cosmos-adatbázis továbbra is toomaintain elérhetőségét módosításakor hello toohello átviteli sebességet. Azure Cosmos DB transzparens módon kezeli a partíciók (például megosztott, egyesítési, klónozási műveletek), és biztosítja, hogy hello műveletek nem csökkentheti a teljesítményt és rendelkezésre állás, amíg a hello alkalmazás rugalmasan növeli vagy csökkenti a teljesítményt. 

## <a id="AvailabilityGuarantees"></a>Rendelkezésre állási garanciák
Azure Cosmos DB kínál egy 99,99 % hasznos üzemidő SLA-elérhetőséget az egyes hello adat- és a vezérlősík műveletek. A korábban ismertetett Azure Cosmos DB rendelkezésre állását garantálja tartalmaznia kell egy abszolút felső határa késés minden adat- és a vezérlősík műveletekhez. hello rendelkezésre állását garantálja steadfast és régiók vagy a földrajzi régiók közötti távolság szerint hello száma nem változik. Rendelkezésre állási garanciák egyaránt manuális, valamint automatikus feladatátvételi alkalmazni. Azure Cosmos DB kínál átlátszó többhelyű API-k, győződjön meg arról, hogy az alkalmazás logikai végpontok is működik, és transzparens módon irányíthatja a hello kérelmek toohello új régió feladatátvétel esetén. PUT eltérően, az alkalmazás nem kell újratelepíteni regionális feladatátvétel esetén toobe és hello rendelkezésre állási SLA-k megmaradjanak.

### <a id="AvailabilityAndConsistency"></a>Rendelkezésre állási tartozó kapcsolat konzisztencia, a késés és a teljesítmény
Rendelkezésre állási tartozó kapcsolat konzisztencia, a késés és a teljesítmény ismertetett [konzisztencia tartozó kapcsolatban van-e rendelkezésre állási](#ConsistencyAndAvailability), [várakozási ideje a kapcsolatot a rendelkezésre állást](#LatencyAndAvailability) és [átviteli sebesség a kapcsolatot a rendelkezésre állást](#ThroughputAndAvailability). 

## <a id="GuaranteesAgainstDataLoss"></a>Biztosítja, és a rendszer viselkedését "adatvesztés"
Az Azure Cosmos Adatbázisba mindegyik partíciójához (gyűjtemény) köszönhetően magas rendelkezésre állású replikákat, amely legalább 10-20 tartalék tartományok vannak elosztva a száma. Összes írt szinkron módon történik, és tartósan elkötelezettek által a replikák többsége másodlagosak ahhoz, azok a toohello ügyfél nyugtázott. Aszinkron replikáció közötti partíciók több régióba elosztva a koordinációs lesz alkalmazva. Az Azure Cosmos DB biztosítja, hogy a bérlő által kezdeményezett kézi feladatátvételre adatvesztés nélküli. Automatikus feladatátvétel során Azure Cosmos DB biztosítja, hogy egy konfigurált hello felső határa elavulási időköz amelyet hello adatvesztési időszak a szolgáltatásiszint-szerződés részeként.

## <a id="CustomerFacingSLAMetrics"></a>SLA-metrikáinak ügyfélkapcsolati
Azure Cosmos DB transzparens módon hello átviteli, a késés, a konzisztencia és a rendelkezésre állási metrikák tesz elérhetővé. A metrikák programozott és hello (lásd a következő ábra) Azure-portálon keresztül érhetők el. Különböző küszöbértékek használata Azure Application Insights-riasztások is állíthat be.
 
**Konzisztencia, a késés, az átviteli sebesség és a rendelkezésre állási adatok gyűjtése le transzparens módon elérhető tooeach bérlői**

![Az Azure Cosmos DB felhasználói által látható SLA-metrikáinak](./media/distribute-data-globally/customer-slas.png)

## <a id="Next Steps"></a>Következő lépések
* globális tooimplement a replikáció az Azure Cosmos DB fiók használata az Azure portál, lásd: hello [hogyan tooperform Azure Cosmos DB globális adatbázis replikációs használatával hello Azure-portálon](tutorial-global-distribution-documentdb.md).
* arról, hogyan toolearn tooimplement több főkiszolgálós architektúrák rendelkező Azure Cosmos DB, lásd: [több főkiszolgálós adatbázis architektúrák rendelkező Azure Cosmos DB](multi-region-writers.md).
* További részletek toolearn hogyan automatikus és manuális feladatátvétel működik az Azure Cosmos Adatbázisba, lásd: [regionális feladatátvétel az Azure Cosmos Adatbázisba](regional-failover.md).

## <a id="References"></a>Hivatkozások
1. Eric sörgyár. [Robusztus elosztott rendszerek felé](https://people.eecs.berkeley.edu/~brewer/cs262b-2004/PODC-keynote.pdf)
2. Eric sörgyár. [SAPKA később – az alkalmazásának tizenkét éve hogyan hello szabályok változásai](http://informatik.unibas.ch/fileadmin/Lectures/HS2012/CS341/workshops/reportsAndSlides/PresentationKevinUrban.pdf)
3. Gilbert, Lynch. - [Sörgyár &#39; s feltevésen és egységes, elérhető megvalósíthatóságának, partíció hibatűrő webes szolgáltatások](http://www.glassbeam.com/sites/all/themes/glassbeam/images/blog/10.1.1.67.6951.pdf)
4. Daniel Abadi. [A Modern konzisztencia mellékhatásokkal elosztott adatbázis rendszerek kialakítása](http://cs-www.cs.yale.edu/homes/dna/papers/abadi-pacelc.pdf)
5. János Kleppmann. [Állítsa le az adatbázisok CP vagy a hozzáférési pont hívása](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)
6. Peter Bailis és mások. [A gyakorlati részleges határozatképességére probabilisztikus a kötött elavulási (PBS)](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
7. Naor és gyapjú. [Betöltési, a kapacitás és a rendelkezésre állási kvórum rendszerekben](http://www.cs.utexas.edu/~lorenzo/corsi/cs395t/04S/notes/naor98load.pdf)
8. Herlihy és a következő. [Lineralizability: Helyességét feltételt az egyidejű objektumok](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf)
9. [Az Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/)
