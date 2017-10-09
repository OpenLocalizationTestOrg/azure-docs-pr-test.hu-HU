---
title: Azure Cosmos DB aaaConsistency szintek |} Microsoft Docs
description: "Azure Cosmos DB öt konzisztencia szintek toohelp egyenleg végleges konzisztencia, a rendelkezésre állás és a késleltetés kompromisszumot rendelkezik."
keywords: "az azure végleges konzisztencia cosmos db, azure, a Microsoft azure"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: cgronlun
documentationcenter: 
ms.assetid: 3fe51cfa-a889-4a4a-b320-16bf871fe74c
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: mimig
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac399c229d0856cd811bc81568536e519af3300f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tunable-data-consistency-levels-in-azure-cosmos-db"></a>Az Azure Cosmos Adatbázisba hangolható konzisztencia szintek
Azure Cosmos DB szabad a globális terjesztési szem előtt az összes adatmodell hello a úgy van kialakítva. Tervezett toooffer előre jelezhető késés garanciák, egy 99,99 % rendelkezésre állási SLA-t, és jól meghatározott több enyhíteni konzisztencia modellek. Jelenleg az Azure Cosmos DB biztosít öt konzisztenciaszintek: erős, kötött elavulás, munkamenet, egységes előtag, és végleges. 

Emellett **erős** és **végleges konzisztencia** modellek gyakran által nyújtott elosztott adatbázisok Azure Cosmos DB három további gondosan kódolt és operationalized konzisztencia modellt biztosít, és a valós életben használati esetek elleni hasznosságát érvényesített. Ezek a hello **kötött elavulási**, **munkamenet**, és **konzisztens előtag** konzisztenciaszintek. Együttesen ezek öt konzisztenciaszintek lehetővé teszik a toomake jól indokolt kompromisszumot konzisztencia, a rendelkezésre állás és a késleltetés között. 

## <a name="distributed-databases-and-consistency"></a>Elosztott adatbázisok és a konzisztencia
A kereskedelmi forgalomban elérhető adatbázisok két kategóriába sorolhatók: az egyik egyáltalán nem kínál jól definiált és bizonyítható konzisztencialehetőségeket, a másik pedig két szélsőséges programozási lehetőséget nyújt (erős vagy végleges konzisztencia). 

hello korábbi terheket alkalmazásfejlesztők számára, a replikációs protokollok minutia, és várja őket toomake nehéz mellékhatásokkal közötti konzisztencia, a rendelkezésre állási, a késés és a teljesítményt. Ez utóbbi hello helyezi a terhelés toochoose egyik hello két szélső érték között. Annak ellenére, hogy hello a kutatás és több mint 50 konzisztencia modellek javaslatokat, ott hello elosztott adatbázis közösségi nem volt képes toocommercialize konzisztenciaszintek erős és a végleges konzisztencia túl. A cosmos DB lehetővé teszi a fejlesztők toochoose közötti öt jól meghatározott konzisztencia modellek mentén hello konzisztencia pontszámot – erős, kötött elavulás, [munkamenet](http://dl.acm.org/citation.cfm?id=383631), egységes előtag, és végleges. 

![Az Azure Cosmos DB biztosít több, jól meghatározott (laza) konzisztencia modellek toochoose a](./media/consistency-levels/five-consistency-levels.png)

a következő táblázat hello hello adott garanciák minden konzisztenciaszint biztosít mutatja be.
 
**Konzisztenciaszintek és garanciák**

| Konzisztenciaszint | Garanciák |
| --- | --- |
| Erős | Linearizálhatóság |
| Kötött elavulás | Konzisztens előtag. Az olvasások k verzióval vagy t időközzel követik az írásokat |
| Munkamenet   | Konzisztens előtag. Monoton olvasások, monoton írások, írás utáni visszaolvasás, beolvasás utáni írás |
| Konzisztens előtag | Visszaadott frissítései néhány előtag hello frissítéseinek, nincs szünet a |
| Végleges  | Nem sorrendben történő olvasások |

Konfigurálhatja hello alapértelmezett konzisztenciaszint Cosmos DB fiókja (és később bírálja felül egy adott olvasási kérelem hello konzisztencia). Belső a hello alapértelmezett konzisztenciaszint toodata belül hello partíció-készletek, amelyek régiókban is vonatkozik. A bérlők körülbelül 73 %-át használja a munkamenet-konzisztencia, és a kötött elavulási előnyben részesítik a 20 %-át. Azt láthatja, hogy a felhasználók körülbelül 3 % kísérletezhet különböző konzisztenciaszintek kezdetben előtt az adott konzisztencia téve az alkalmazásokban a rendezése. Azt is láthatja, hogy a bérlők számára csak 2 % felülbírálása konzisztenciaszintek kérelmek száma alapján. 

A Cosmos DB olvasási kiszolgált munkamenetben, egységes előtag és végleges konzisztencia kétszer szerint olcsó, olvasások erős, vagy a kötött elavulási konzisztencia. Cosmos DB rendelkezik iparágvezető átfogó 99,99 % SLA-k többek között a rendelkezésre állás, az átviteli sebesség és a késleltetés mellett konzisztencia biztosítja. Alkalmazott egy [linearizability ellenőrző](http://dl.acm.org/citation.cfm?id=1806634), amely folyamatosan működik a szolgáltatás telemetriai adatokat, és nyilvánosan jelentéseket bármely konzisztencia megsértésének tooyou. A kötött elavulási, a Microsoft figyelésére és jelentésére felmerülő szabálysértéseket tartott, és t határain. Az összes öt laza konzisztenciaszintek, azt is jelentheti hello [probabilisztikus a kötött elavulási metrika](http://dl.acm.org/citation.cfm?id=2212359) közvetlenül tooyou.  

## <a name="scope-of-consistency"></a>Konzisztencia hatóköre
konzisztencia hello granularitása hatókörön belüli tooa egy felhasználói kérelem. Egy írási kérelem tooan insert, replace, upsert tartozik, vagy törölhető a tranzakció. Írási műveleteket, mint a olvasás/lekérdezés tranzakció egyben hatókörön belüli tooa egy felhasználói kérelem. hello felhasználói lehet szükséges toopaginate keresztül egy nagy-eredménykészlet, több partíció-áthidalás, de egyes tranzakció hatókörön belüli tooa egyetlen lap, és a belül egyetlen partícióra kiszolgált olvasása.

## <a name="consistency-levels"></a>Konzisztenciaszintek
A Cosmos DB fiók tooall gyűjtemények (és az adatbázisok) alkalmazó adatbázis fiókja egy alapértelmezett konzisztenciaszint adhat meg. Alapértelmezés szerint minden olvasási és lekérdezések hello állították ki a felhasználói erőforrások hello hello adatbázisfiók beállított alapértelmezett konzisztencia szint használják. Akkor is enyhíteni hello konzisztenciaszint adott olvasás/lekérdezés kérelem API-k hello használatát támogatják. Nincsenek öt típusú konzisztenciaszintek hello Azure Cosmos DB replikációs protokoll által támogatott, amely egy tiszta kompromisszum között adott konzisztencia biztosítja, és a teljesítmény, ebben a szakaszban leírtak szerint.

**Erős**: 

* Erős konzisztenciát biztosít egy [linearizability](https://aphyr.com/posts/313-strong-consistency-models) hello biztosítékot garantált tooreturn hello legújabb verziója található elem beolvasása. 
* Az erős konzisztencia biztosítja, hogy egy írási csak látható után már tartósan által a replikák többsége másodlagosak hello. Írás már vagy szinkron módon tartósan hello elsődleges és a hello kvórum több másodlagos adatbázist, vagy megszakadt az. Egy olvasási mindig vonatkozik, olvassa el a kvórum hello hozza, az ügyfél egy nem véglegesített vagy részleges írási soha nem látható, és mindig garantáltan tooread hello legújabb nyugtázott írási. 
* Azure Cosmos DB fiókok konfigurált toouse az erős konzisztencia nem társítható egynél több Azure-régiót Azure Cosmos DB fiókkal. 
* egy olvasási művelet költségét hello (a [egységek kérelem](request-units.md) felhasznált) erős konzisztencia magasabb, mint a munkamenet és végleges, de ugyanaz, mint a kötött elavulási hello.

**A kötött elavulási**: 

* A kötött elavulási konzisztencia biztosítja, hogy hello olvasások legfeljebb által írt előfordulhat, hogy késés *K* verziója vagy az előtagok elem vagy *t* időköz. 
* Ezért, amikor kiválasztása kötött elavulási, "elavulási" hello konfigurálható kétféleképpen: verziók száma *K* hello elem, amely szerint hello olvasások késés hello alatt az időtartam alatt, és hello írási *t* 
* Kötött elavulási ajánlatok teljes globális sorrendjét kivéve belül hello "ablak elavulási." hello monoton olvasási garanciák létezik belüli és kívüli hello "ablak elavulási." régión belül 
* A kötött elavulási konzisztencia erősebb garancia munkamenet, illetve a végleges konzisztencia biztosítja. Globálisan elosztott alkalmazásokhoz javasoljuk a kötött elavulási használhat olyan helyzetekben, amikor volna, például az erős konzisztencia toohave, de is érdemes a rendelkezésre állás 99,99 % és kis késleltetése. 
* A kötött elavulási konzisztencia konfigurált Azure Cosmos DB fiókok tetszőleges számú Azure-régiók társíthatja Azure Cosmos DB fiókjuk. 
* az olvasási művelet (tekintetében felhasznált RUs) költségét hello a kötött elavulási értéke magasabb, mint a munkamenet és végleges konzisztencia, de ugyanaz, mint az erős konzisztencia hello.

**Munkamenet**: 

* Erős és a kötött elavulási konzisztencia szintek által kínált hello globális konzisztenciahiba modellek eltérően a munkamenet-konzisztencia hatókörön belüli tooa ügyfélmunkamenethez. 
* A munkamenet-konzisztencia minden olyan esetekben, ahol van szó egy eszközre vagy felhasználóra vonatkozik-munkamenetet, mivel azt biztosítja, hogy monoton olvasási, monoton írás és Olvasás saját írási műveletek (RYW) biztosítja, hogy az ideális. 
* Munkamenet-konzisztencia kiszámítható konzisztencia biztosít egy munkamenet esetében, és a maximális átviteli sebesség olvasási hello legalacsonyabb késés írások és olvasások kínálja. 
* A munkamenet-konzisztencia konfigurált Azure Cosmos DB fiókok tetszőleges számú Azure-régiók társíthatja Azure Cosmos DB fiókjuk. 
* egy olvasási művelet (tekintetében felhasznált RUs) költségét hello munkamenet-konzisztencia szintje kisebb, mint az erős és a kötött elavulási, de több mint végleges konzisztencia

<a id="consistent-prefix"></a>
**Egységes előtag**: 

* Egységes előtag garantálja, hogy bármilyen további írás hiányában hello csoporton belüli hello replikák végül össze vonva. 
* Egységes előtag biztosítja, hogy az, hogy olvasási műveletek nem veszi észre üzemen kívüli írási műveleteket. Ha írási műveletek hello sorrendben végrehajtott `A, B, C`, ügyfél látja el, majd `A`, `A,B`, vagy `A,B,C`, de soha nem megfelelő sorrendben például `A,C` vagy `B,A,C`.
* Egységes előtag konzisztencia konfigurált Azure Cosmos DB fiókok tetszőleges számú Azure-régiók társíthatja Azure Cosmos DB fiókjuk. 

**Végső**: 

* Végleges konzisztencia biztosítja, hogy bármilyen további írás hiányában hello csoporton belüli hello replikák végül össze vonva. 
* Végleges konzisztencia formája, amely hello leggyengébb konzisztencia, ahol az adott ügyfél hello értékek, amelyek régebbiek hello néhányat a meglévők közül azt találkozott előtt lehet, hogy le.
* Végleges konzisztencia hello leggyengébb olvasási konzisztenciát, de ajánlatok mind az Olvasás, mind az írás hello legkisebb mértékű késleltetést biztosítja.
* A végleges konzisztencia konfigurált Azure Cosmos DB fiókok tetszőleges számú Azure-régiók társíthatja Azure Cosmos DB fiókjuk. 
* egy olvasási művelet (tekintetében felhasznált RUs) költségét hello hello végleges konzisztencia szintje összes hello Azure Cosmos DB konzisztenciaszintek legalacsonyabb hello.

## <a name="configuring-hello-default-consistency-level"></a>Hello alapértelmezett konzisztencia szint konfigurálása
1. A hello [Azure-portálon](https://portal.azure.com/), a hello Ugrósávon, kattintson a **Azure Cosmos DB**.
2. A hello **Azure Cosmos DB** panelen, jelölje be hello adatbázis fiók toomodify.
3. Hello-fiók panelen kattintson a **alapértelmezett konzisztencia**.
4. A hello **alapértelmezett konzisztencia** panelen, jelölje be hello új konzisztencia szinten, és kattintson **mentése**.
   
    ![Képernyőfelvétel a hello beállítások ikonra, és alapértelmezett konzisztencia bejegyzés kiemelve](./media/consistency-levels/database-consistency-level-1.png)

## <a name="consistency-levels-for-queries"></a>A lekérdezések konzisztenciaszintek
Alapértelmezés szerint a felhasználói erőforrások hello konzisztenciaszint lekérdezések érték hello azonos módon hello konzisztencia szintjének beolvasása. Alapértelmezés szerint hello index frissítése szinkron módon minden insert, replace vagy egy cikk toohello Cosmos DB tároló törlése. Ez lehetővé teszi, hogy hello lekérdezések toohonor hello azonos konzisztencia szinten az olvasási műveletek mutasson. Azure Cosmos DB írási optimalizálva, és írási műveletek, a szinkron index karbantartási és a kiszolgáló konzisztens lekérdezések tartós köteteket támogatja, konfigurálhat bizonyos gyűjtemények tooupdate indexét lazily. A lusta indexelési további hello írási teljesítmény növekedhet és ideális tömeges adatfeldolgozást forgatókönyvek esetén, ha egy munkaterhelés elsősorban olvasási műveleteket.  

| Az indexelő mód | Olvassa be | Lekérdezések |
| --- | --- | --- |
| A CONSISTENT (alapértelmezett) |Válassza ki az erős, kötött elavulási, munkamenet, egységes előtag, vagy végleges |Válassza ki az erős, kötött elavulás, munkamenet, vagy végleges |
| Lassú |Válassza ki az erős, kötött elavulási, munkamenet, egységes előtag, vagy végleges |Végleges |
| None |Válassza ki az erős, kötött elavulási, munkamenet, egységes előtag, vagy végleges |Nem alkalmazható |

Mint az olvasási kéréseket csökkenthető hello konzisztenciaszint minden API-ban meghatározott lekérdezési kérelem.

## <a name="next-steps"></a>Következő lépések
Ha szeretné toodo több olvasásakor konzisztenciaszintek és kompromisszumot kapcsolatos, a következő erőforrások hello javasoljuk:

* Doug Terry. A replikált adatok konzisztencia viszonylag baseball (videó) keresztül.   
  [https://www.YouTube.com/Watch?v=gluIh8zd26I](https://www.youtube.com/watch?v=gluIh8zd26I)
* Doug Terry. A replikált adatok konzisztencia viszonylag baseball keresztül.   
  [http://Research.microsoft.com/Pubs/157411/ConsistencyAndBaseballReport.PDF](http://research.microsoft.com/pubs/157411/ConsistencyAndBaseballReport.pdf)
* Doug Terry. Munkamenet garanciák gyengén konzisztens replikált adatok.   
  [http://DL.ACM.org/CITATION.cfm?ID=383631](http://dl.acm.org/citation.cfm?id=383631)
* Daniel Abadi. Konzisztencia mellékhatásokkal Modern elosztott adatbázis rendszerek kialakításában: CAP csak hello szövegegység része ".   
  [http://Computer.org/CSDL/mags/Co/2012/02/mco2012020037-ABS.HTML](http://computer.org/csdl/mags/co/2012/02/mco2012020037-abs.html)
* Peter Bailis, Shivaram Venkataraman, Michael J. tw, Joseph M. Hellerstein, adatmegőrzési Stoica. Probabilisztikus kötött elavulási (PBS) a gyakorlati részleges határozatképességére.   
  [http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.PDF](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
* Wernernek Vogels. Végső konzisztens - javított változat.    
  [http://allthingsdistributed.com/2008/12/eventually_consistent.HTML](http://allthingsdistributed.com/2008/12/eventually_consistent.html)
* MONi Naor, Avishai gyapjú, hello betöltése, kapacitás és rendelkezésre állási a kvórum rendszerek, a számítási, v.27 n.2, p.423 447, 1998. április SIAM napló.
  [http://epubs.Siam.org/DOI/ABS/10.1137/S0097539795281232](http://epubs.siam.org/doi/abs/10.1137/S0097539795281232)
* Sebastian Burckhardt, Chris Dern, Macanal Musuvathi, Roy Tan, Line-up: a teljes és automatikus linearizability-ellenőrző eljárás hello 2010 ACM SIGPLAN konferencia programozási nyelv tervezési és megvalósítási, június 05-10-es, 2010, Toronto, Ontario, Kanadában [doi > 10.1145/1806596.1806634] [http://dl.acm.org/citation.cfm?id=1806634](http://dl.acm.org/citation.cfm?id=1806634)
* Peter Bailis, Shivaram Venkataraman, Michael J. tw, Joseph M. Hellerstein adatmegőrzési Stoica Probabilistically kötött elavulási gyakorlati részleges határozatképességére, hello VLDB dotációs, még az v.5 n.8, p.776 787, 2012 áprilisi eljárásai az [http:// DL.ACM.org/CITATION.cfm?ID=2212359](http://dl.acm.org/citation.cfm?id=2212359)
