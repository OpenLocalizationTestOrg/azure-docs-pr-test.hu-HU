---
title: "Bevezetés tooAzure Cosmos DB Graph API-k |} Microsoft Docs"
description: "Ismerje meg, hogyan használható Azure Cosmos DB toostore, a lekérdezés és a bejárás nagy diagramokat és kis késésű hello hello Gremlin graph lekérdező nyelve az Apache TinkerPop használatával."
services: cosmos-db
author: dennyglee
documentationcenter: 
ms.assetid: b916644c-4f28-4964-95fe-681faa6d6e08
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/21/2017
ms.author: denlee
ms.openlocfilehash: a4e79a4aa27976966baf0554928026177991ff69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-cosmos-db-graph-api"></a>Bevezetés tooAzure Cosmos DB: Graph API-val

[Az Azure Cosmos DB](introduction.md) van létfontosságú alkalmazások globálisan elosztott, több modellre dokumentumadatbázis-szolgáltatás a Microsoft hello. Az Azure Cosmos DB biztosít [kulcsrakész globális terjesztési](distribute-data-globally.md), [átviteli sebesség és tárterület a rugalmas méretezést](partition-data.md) világszerte, egyjegyű ezredmásodperces késések: hello 99th PERCENTILIS, [öt jól meghatározott konzisztenciaszintek](consistency-levels.md), és magas rendelkezésre állás érdekében minden által támogatott garantált [iparágvezető SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/). Az Azure Cosmos DB [automatikusan elvégzi az adatok](http://www.vldb.org/pvldb/vol8/p1668-shukla.pdf) anélkül, hogy Ön séma- és index felügyeleti toodeal. Többmodelles szolgáltatás, amely támogatja a dokumentumokat, a kulcs-értékeket, a diagramokat és az oszlopos adatmodelleket.

![Gremlin, a graph és az Azure Cosmos DB](./media/graph-introduction/graph-gremlin.png)

hello Azure Cosmos DB Graph API biztosítja:

- Graph modellezési
- Könyvtármegkerülési API-k
- Kulcsrakész globális terjesztési
- A rugalmas méretezést tárolási és átviteli, legfeljebb 10 ms késések olvasása és kisebb, mint 15 ms hello 99th PERCENTILIS:
- Automatikus azonnali lekérdezés elérhető indexelő
- Aprólékosan beállítható konzisztenciaszintek
- Átfogó, beleértve a rendelkezésre állás 99,99 % SLA

Azure Cosmos DB tooquery, hello használható [Apache TinkerPop](http://tinkerpop.apache.org) átjárás nyelvi diagramot [Gremlin](http://tinkerpop.apache.org/docs/current/reference/#graph-traversal-steps), vagy más kompatibilis TinkerPop graph rendszerek, például [Apache Spark GraphX](spark-connector-graph.md).

Ez a cikk áttekintést hello Azure Cosmos DB Graph API, és ismerteti, hogyan használható az toostore nagy diagramjait és a csúcsban milliárd. Hello diagramjait lekérdezéseket az ezredmásodperces késési, és könnyen fejlődnek hello graph struktúra és a séma.

## <a name="graph-database"></a>Graph-adatbázis
Hello valós megjelenő természetes kapcsolódnak. Hagyományos modellezési entitások összpontosít. Több alkalmazás is van a szükséges toomodel vagy toomodel entitásokat és a kapcsolatok természetes.

A [graph](http://mathworld.wolfram.com/Graph.html) áll egy struktúra [csúcsban](http://mathworld.wolfram.com/GraphVertex.html) és [szélek](http://mathworld.wolfram.com/GraphEdge.html). Mind a csúcsban, és a szélek tulajdonságok tetszőleges számú tartozhat. Csúcsban jelöl diszkrét objektumok, például egy személy, egy helyen vagy egy eseményt. Szegélyek csúcsban közötti kapcsolatok jelöl. Például egy személy előfordulhat, hogy tudja, egy másik személyre kell részt az esemény és egy helyen nemrég átállították. Tulajdonságok hello csúcsban és szélek kapcsolatos információk express. Példa tulajdonságai közé tartozik, amelyen a neve, a kora, és a peremhálózati, amelynek időbélyeg és/vagy az egyik súlya csúcspont. Ez a modell több hivatalosan is ismert, egy [tulajdonság graph](http://tinkerpop.apache.org/docs/current/reference/#intro). Azure Cosmos DB hello tulajdonság graph modellt támogatja.

Például hello alábbi példa a diagramon látható személyek, mobileszközök, érdeklődési és operációs rendszerek közötti kapcsolatok szerepelnek.

![Személyek, eszközök és érdeklődési mintaadatbázis](./media/graph-introduction/sample-graph.png)

Diagramokat tudományos, technológia és üzleti adatkészletek számos hasznos toounderstand. Graph adatbázisok lehetővé teszik, hogy a modell és az áruházbeli diagramjait természetesen és hatékonyan, ami hasznos, ha több forgatókönyv. Graph adatbázisok jellemzően NoSQL-adatbázisok, mert ezek használatát is gyakran esetekben a szükséges sémák rugalmasságát és gyors iterációs.

Diagramokat egy új és a hatékony módszer modellezési adatok kínálnak. De a tény önmagában nem elegendő ok toouse egy grafikonon adatbázis. Számos használati esetek és minták graph traversals érintő diagramjait outperform hagyományos SQL és a NoSQL-adatbázisok által nagyságrenddel. A teljesítménybeli eltérést az további bővíteni, amikor egynél több kapcsolattal, például a "Friend"-az-a-barát áthaladó.

Hello graph adatbázisok gyors traversals kombinálhatja graph algoritmusok, pl. mélysége-első keresési, körének-első keresési és Dijkstra tartozó algoritmus, közösségi webhelyek, a tartalom kezelésére, a földrajzi, például különböző tartományokban toosolve problémák és javaslatokat.

## <a name="planet-scale-graphs-with-azure-cosmos-db"></a>Az Azure Cosmos DB bolygónk méretű diagramjait
Azure Cosmos-adatbázis egy teljes körűen felügyelt graph-adatbázist, amely globális terjesztési, rugalmas tárolási és átviteli sebességet, automatikus indexeléshez lekérdezés, konzisztenciaszinteket és hello TinkerPop szabvány támogatása skálázás kínál.  

![Azure Cosmos DB graph-architektúra](./media/graph-introduction/cosmosdb-graph-architecture.png)

Azure Cosmos DB kínál hello következő megkülönböztetett képességek képest tooother graph adatbázisok hello piacon:

* Rugalmasan méretezhető átviteli sebesség és tárterület

 A valós életben hello diagramjait tooscale hello egyetlen kiszolgáló kapacitása túl kell. Az Azure Cosmos DB méretezheti a diagramok zökkenőmentesen több kiszolgáló között. Hello sebességét, a graph-egymástól függetlenül a hozzáférési minták alapján is méretezheti. Azure Cosmos-adatbázis is méretezhető toovirtually graph-adatbázisokat támogatja korlátlan tárterület mérete és a létesített átviteli sebesség.

* Több területi replikáció

 Azure Cosmos DB transzparens módon replikálja a graph tooall adatterületek, amely a fiókjához tartozó. Replikációs globális hozzáférési toodata igénylő alkalmazások toodevelop lehetővé teszi. Nincsenek mellékhatásokkal konzisztencia, a rendelkezésre állás, és a teljesítmény és a megfelelő garanciák hello területéhez. Azure Cosmos DB többhelyű API-khoz transzparens regionális feladatátvétel biztosít. Rugalmasan méretezhető átviteli sebesség és tárterület hello földgömb között.

* Gyors lekérdezéseket és ismerős Gremlin szintaxissal traversals

 Heterogén csúcsban és szélek tárolja, és ezeket a dokumentumokat a megszokott Gremlin szintaxis keresztül lekérdezése. Azure Cosmos-adatbázis egy nagymértékben párhuzamos, zárolás ingyenes, naplószerkezetű indexelési technológiát tooautomatically index összes tartalmát használja. Ez a funkció lehetővé teszi, hogy részletes valós idejű lekérdezéseket és traversals hello nélkül kell toospecify sémamutatók, másodlagos indexek vagy nézetek. További információ: [Gremlin használatával lekérdezés diagramjait](gremlin-support.md).

* Teljes körű felügyelet

 Azure Cosmos-adatbázis beállítást választja, nincs szükség toomanage adatbázis és a gép erőforrásainak hello. Teljes körűen felügyelt Microsoft Azure szolgáltatásként akkor nem kell toomanage virtuális gépek tegye, telepítése és szoftver konfigurálása, méretezés kezelésére vagy összetett adatrétegek frissítésére foglalkozik. Minden graph automatikusan biztonsági másolat és védve legyenek a regionális meghibásodásokkal szemben. Egyszerűen Azure Cosmos DB fiók hozzáadása és mértékben kapacitást kell azt, hogy az alkalmazás üzemeltetése és kezelése az adatbázis helyett összpontosíthat.

* Az automatikus indexeléshez

 Alapértelmezés szerint Azure Cosmos DB automatikusan indexeket csomópontokat és szélek hello grafikon az összes hello tulajdonságokat, és nem várt vagy igényel semmilyen sémát, illetve másodlagos indexek létrehozását.

* Apache TinkerPop való kompatibilitás érdekében

 Azure Cosmos-adatbázis natív módon támogatja hello nyílt forráskódú Apache TinkerPop szabványt, és más diagramhoz TinkerPop-kompatibilis rendszerek integrálható. Igen, egyszerűen telepítse át egy másik graph-adatbázisból, például Titan vagy Neo4j, vagy Azure Cosmos-Adatbázist kíván használni a graph analytics keretrendszerek, például a [Apache Spark GraphX](spark-connector-graph.md).

* Aprólékosan beállítható konzisztenciaszintek

 Válassza ki az öt jól meghatározott konzisztencia szintek tooachieve közötti optimális kompromisszum konzisztencia és a teljesítmény. A lekérdezések és olvasási műveletek esetében az Azure Cosmos DB öt különböző konzisztenciaszintet kínál: erős, kötött elavulás, munkamenet, konzisztens előtag és végleges. A részletes, jól meghatározott konzisztenciaszintek lehetővé teszi hang mellékhatásokkal toomake konzisztencia, a rendelkezésre állás és a késleltetés között. További információ: [toomaximize rendelkezésre állásának és teljesítményének a documentdb használatával konzisztencia szintek](consistency-levels.md).

Azure Cosmos-adatbázis is használhat, több modellek, például a dokumentumok és a graph-belül azonos tárolók/adatbázisok hello. A dokumentum gyűjtemény toostore Diagramadatok dokumentumok és használható. Mindkét SQL-lekérdezések JSON keresztül is használhatja, és Gremlin lekérdezések tooquery hello ugyanazokat az adatokat egy grafikonon.

## <a name="getting-started"></a>Bevezetés
Használhatja a hello Azure parancssori felület (CLI), Azure Powershell vagy Azure portál, amely támogatja a graph API toocreate Azure Cosmos DB fiókok hello. Miután létrehozta a fiókok, hello Azure-portálon biztosít egy végpontot, például `https://<youraccount>.graphs.azure.com`, biztosít egy WebSocket előtér Gremlin számára. Konfigurálhatja a TinkerPop-kompatibilis eszközök, például a hello [Gremin konzol](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console), tooconnect toothis végpont és build Java, Node.js vagy bármely Gremlin ügyfél-illesztőprogram alkalmazások.

hello következő táblázatban népszerű Gremlin illesztőprogramok használható Azure Cosmos DB szemben:

| Letöltés | Dokumentáció |
| --- | --- |
| [Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) |[Gremlin JavaDoc](http://tinkerpop.apache.org/javadocs/current/full/) |
| [Node.js](https://www.npmjs.com/package/gremlin) |[Gremlin-JavaScript a Githubon](https://github.com/jbmusso/gremlin-javascript) |
| [Gremlin konzol](https://tinkerpop.apache.org/downloads.html) |[TinkerPop docs](http://tinkerpop.apache.org/docs/current/reference/#gremlin-console) |

Azure Cosmos-adatbázis is biztosít, amely rendelkezik Gremlin kiterjesztésmetódusok fölött hello .NET könyvtár [Azure Cosmos DB SDK-k](documentdb-sdk-dotnet.md) NuGet útján. Ezt a szalagtárat tartalmaz egy "folyamaton belüli" Gremlin kiszolgáló használható tooconnect közvetlenül tooDocumentDB adatok partíciókat.

| Letöltés | Dokumentáció |
| --- | --- |
| [.NET](https://www.nuget.org/packages/Microsoft.Azure.Graphs/) |[Microsoft.Azure.Graphs](https://msdn.microsoft.com/library/azure/dn948556.aspx) |

Hello segítségével [Azure Cosmos DB emulátor](local-emulator.md), hello Graph API toodevelop használja, és helyi tesztelése egy Azure-előfizetés létrehozása, és ezzel járó költségeket nélkül. Ha elégedett az alkalmazást az emulátor hello alakulását, toousing hello felhőben Azure Cosmos DB fiók válthat.

## <a name="scenarios-for-graph-support-of-azure-cosmos-db"></a>Azure Cosmos DB graph-támogatása forgatókönyvei
Az alábbiakban néhány forgatókönyv graph támogatási Azure Cosmos-adatbázis helyének:

* Közösségi hálózatokkal

 Az ügyfelek és azok interakciók más felhasználókkal kapcsolatos adatok kombinálásával személyre szabott élmény fejlesztése, felhasználói viselkedés előrejelzése vagy személyek csatlakozás másokkal hasonló érdekében. Azure Cosmos-adatbázis is használt toomanage közösségi hálózatokkal és nyomon követheti a felhasználói beállítások és adatok.

* A javaslat motorok

 Ebben a forgatókönyvben hello kereskedelmi iparági gyakran használják. Termékek és felhasználók, felhasználói interakció megvásárlásáról, keresse meg vagy egy elem értékelése kombinálásával hozhat létre testreszabott javaslatok. Kis késés, rugalmasan méretezhető és natív hello Azure Cosmos DB graph-támogatása ezen kapcsolati modellezési ideális.

* Térinformatikai

 Számos alkalmazás távközlési, logisztikai és utazás megtervezése toofind az érdeklődési területen helyre kell, vagy keresse meg a lehető legrövidebb/optimális útvonal hello két hely között. Azure Cosmos-adatbázis a problémák természetes alkalmasnak.

* Eszközök internetes hálózata

 A hello hálózati és egy grafikonon modellezve az IoT-eszközök közötti kapcsolatokat az eszközök és az eszközök állapotának hello jobb megértése létrehozhatja és ismerje meg, hogy egyik részét képező hello hálózati változásai hatással lehet egy másik.

## <a name="next-steps"></a>Következő lépések
További információ az Azure Cosmos DB, a graph-támogatás toolearn lásd:

* Ismerkedés a hello [Azure Cosmos DB graph oktatóanyag](create-graph-dotnet.md).
* Megtudhatja, hogyan túl[lekérdezése az Azure Cosmos Adatbázisba Gremlin használatával diagramjait](gremlin-support.md).
