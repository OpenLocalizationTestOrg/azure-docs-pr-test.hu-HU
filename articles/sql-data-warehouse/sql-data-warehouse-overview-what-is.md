---
title: aaaWhat az Azure SQL Data Warehouse? | Microsoft Docs
description: "Vállalati szintű elosztott adatbázis, amely petabájtnyi mennyiségű relációs és nem relációs adatot képes feldolgozni. Hello iparág első felhőalapú adatraktára nő a, és zsugorítása másodpercek alatt szüneteltethető."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: bjhubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 2/28/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 5fefe40879230f123c2e4a90b9c20a35779cf711
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-sql-data-warehouse"></a>Mi az Azure SQL Data Warehouse?
Az Azure SQL Data Warehouse egy nagymértékben párhuzamos feldolgozási (MPP) kialakítású, felhőalapú, horizontálisan felskálázható relációs adatbázis, amely nagy mennyiségű adatot képes feldolgozni. 

SQL Data Warehouse:

* Hello SQL Server relációsadatbázis egyesíti az Azure-felhőbe kibővített képességeit. 
* Elválasztja a tárterületet a számítási műveletektől.
* Lehetővé teszi a számítási műveletek növelését, csökkentését, szüneteltetését vagy folytatását. 
* Egyesíti a hello Azure platformon keresztül.
* Az SQL Server Transact-SQL (T-SQL) nyelvét és eszközeit használja.
* Megfelel különböző jogi és vállalati biztonsági követelményeknek (például SOC és ISO).

Ez a cikk ismerteti az SQL Data Warehouse hello funkciói.

## <a name="massively-parallel-processing-architecture"></a>Nagymértékben párhuzamos feldolgozási architektúra
Az SQL Data Warehouse egy nagymértékben párhuzamos feldolgozási (MPP) elosztott adatbázisrendszerre épül. Hello háttérben SQL Data Warehouse között osztja el az adatok sok osztott tárfiók és feldolgozóegység között. hello adatok tárolódnak a prémium szintű helyileg redundáns tárolás réteg fölött, amely dinamikusan csatolt számítási csomópontok-lekérdezéseket hajt végre. SQL Data Warehouse vesz egy "osztani és uralkodj" megközelítés toorunning betölti és összetett lekérdezések. A vezérlő csomópont, terjesztési optimalizálva, és majd átadott tooCompute csomópontok toodo munkavégzést párhuzamosan érkezik kérelem.

Az SQL Data Warehouse a tárterület és a számítási műveletek elkülönítésével a következőkre képes:

* Növelheti és csökkentheti a tárterület a számítási kapacitástól függetlenül.
* Növelheti vagy csökkentheti a számítási kapacitást az adatok áthelyezése nélkül.
* Szüneteltetheti a számítási kapacitást az adatok sérülése nélkül, így csak a tárterületért kell fizetnie.
* A működési időn belül folytatni tudja a számítási kapacitást.

hello alábbi ábrán látható hello architektúra részletesebben.

![Az SQL Data Warehouse architektúrája][1]

**Vezérlő csomópont:** hello vezérlő csomópont kezeli, és optimalizálja a lekérdezések. Hello előtér, amely az összes alkalmazással és kapcsolattal együttműködik. Az SQL Data Warehouse hello vezérlő csomópont SQL Database működteti, és csatlakozás tooit keres, és érzi hello azonos. Hello felszín alatt hello vezérlő csomópont koordinálja az összes hello adatmozgatást és a szükséges számítási toorun párhuzamos lekérdezések elosztott adatokon. Amikor egy T-SQL-lekérdezés tooSQL Data warehouse-ba, hello vezérlő csomópont alakítja külön minden számítási csomópont párhuzamosan futó lekérdezések.

**Számítási csomópontok:** hello teljesítménye mögött SQL Data Warehouse hello számítási csomópontok állnak. Ezek olyan SQL-adatbázisok, amelyek tárolják az adatokat, és feldolgozzák a lekérdezést. Amikor adatokat, az SQL Data Warehouse hello sorok tooyour számítási csomópontok osztja el. hello számítási csomópontok hello munkavállalók, amely hello párhuzamos lekérdezéseket futtathat az adatokat a rendszer. A feldolgozás után ezek a csomópontok visszaküldik hello eredmények hátsó toohello vezérlő csomópont. toofinish hello lekérdezés, hello vezérlő csomópont aggregátumok hello eredményeit, és vissza hello végeredményt.

**Tárolás:** a rendszer az adatokat az Azure Blob szolgáltatásban tárolja. Ha a számítási csomópontok kommunikálnak az adatokkal, írása, és közvetlenül tooand olvasni a blob storage. Az Azure storage átlátható és jelentősen bővíti, mert az SQL Data Warehouse teheti hello azonos. Mivel a számítás és a tárolás nem függ egymástól, az SQL Data Warehouse automatikusan át tudja méretezni a tárolást, külön, a számítás méretezése nélkül, és fordítva. Az Azure Blob storage emellett teljesen hibatűrő, és leegyszerűsíti hello biztonsági mentési és visszaállítási folyamat.

**Adatátviteli szolgáltatás:** adatok adatátviteli szolgáltatás (DMS) hello csomópontok között mozgatja az adatokat. DMS ad hello számítási csomópontok hozzáférés toodata az összekapcsolásokhoz és az összesítések van szükségük. A DMS nem Azure-szolgáltatás. Windows szolgáltatás, amely az SQL-adatbázis mellett minden hello csomópontján is. A DMS egy háttérfolyamat, amellyel nem lehet közvetlenül kapcsolatba lépni. Vessen egy pillantást lekérdezés tervek toosee esetén DMS-művelet, mert az adatmozgatás szükséges toorun minden egyes párhuzamos lekérdezés.

## <a name="optimized-for-data-warehouse-workloads"></a>Az adatraktár munkaterhelésére optimalizálva
hello MPP megközelítést több teljesítményoptimalizálás, beleértve az adatok alapján születik:

* Egy elosztott lekérdezésoptimalizáló és az összes adatra vonatkozó összetett statisztikák készlete. Hello szolgáltatást a információk adatok méretével és a terjesztési, mivel felméri az elosztott lekérdezési műveletek költségét hello használata képes toooptimize lekérdezések.
* Speciális algoritmusok és technikák integrált hello adatok adatátviteli tooefficiently move data szükséges tooperform hello lekérdezés számítási erőforrások között. Ezek az adatátviteli műveletek beépített, és minden optimalizálás toohello adatátviteli szolgáltatás automatikusan megtörténik.
* Alapértelmezés szerint fürtözött **oszlopcentrikus** indexek. Az Oszlopalapú tárolás használatával az SQL Data Warehouse lekérdezi átlagosan 5 x nagyobb szintű tömörítésre képes azt hagyományos, soros tárolással, és be too10x vagy több lekérdezési teljesítménynövekedést érhet el. Tooscan elemzési lekérdezések nagy mennyiségű sort jobban oszloptárindexekkel használható.


## <a name="predictable-and-scalable-performance-with-data-warehouse-units"></a>Kiszámítható és méretezhető teljesítmény adattárházegységekkel
Az SQL Data Warehouse ugyanolyan technológiával készült, mint az SQL Database, így a felhasználók egységes és kiszámítható teljesítményt várhatnak el az elemzési lekérdezéseknél. Felhasználók hozzáadása, illetve a számítási csomópontok kivonás lineárisan látnia toosee teljesítmény skála. Erőforrások tooyour SQL Data Warehouse allokációja másodpercenkénti Adattárházegységek (dwu-k). Dwu-k olyan alapul szolgáló erőforrásokhoz, mint a Processzor, memória, iops-érték, amely hozzá az SQL Data Warehouse tooyour biztosítása. Hello dwu-k számának növelése növeli, erőforrások és teljesítményét. A DWU-k a következőket biztosítják:

* Az adatraktár-anélkül, hogy hello alapul szolgáló hardver- vagy képes tooscale áll.
* A teljesítmény javítása a DWU szint hello számítási az adatraktár módosítása előtt képes előre jelezni.
* hello alapul szolgáló hardver- és a példány lehet változtatni vagy át anélkül, hogy az hatással lenne a számítási feladat teljesítményére.
* A Microsoft hello alapjául szolgáló hello szolgáltatás anélkül, hogy az hatással lenne a számítási feladatok teljesítményére hello javíthatja.
* Microsoft gyorsan a jobb teljesítmény érdekében az SQL Data Warehouse, úgy, hogy az méretezhető és egyenletesen hatások hello rendszer.

Az adattárházegységek három mérőszámból álló mértéknek tekinthetők, amelyek szorosan összefüggnek az adattárház számításifeladat-teljesítményével. hello következő terhelési metrika méretezéssel lineárisan hello dwu-k, billentyűt.

**Vizsgálat/aggregáció:** Egy standard adattárház-lekérdezés, amely számos sort átvizsgál, majd komplex összesítést készít. Ez egy I/O- és processzorigényes művelet.

**Betöltés:** képességét tooingest adatok hello hello szolgáltatásban. A betöltés az Azure Storage Blobból vagy az Azure Data Lake-ből végezhető el a legjobban a PolyBase használatával. Ez a metrika, tervezett toostress hálózati és a CPU aspektusainak hello szolgáltatást.

**Tábla létrehozása Select (CTAS):** CTAS méri hello képességét toocopy egy tábla. Ebbe beletartozik az adatok olvasása a tárból, szétosztása a készülék hello hello-csomópont között, és újra írása toostorage. Ez egy processzor-, I/O- és hálózatigényes művelet.

## <a name="built-on-sql-server"></a>Alapja az SQL Server
Az SQL Data Warehouse hello SQL Server relációs adatbázis-kezelő alapul, és számos vállalati adatraktáraktól elvárt hello szolgáltatást tartalmazza. Ha már ismeri a T-SQL, a rendszer könnyen tootransfer a Tudásbázis tooSQL Data warehouse-bA. E speciális vagy csak az első lépések, hello dokumentáció hello példák segítenek a kezdésben. Általános azt is gondolja át hello úgy, hogy épülnek hello SQL Data Warehouse nyelvi elemei az alábbiak szerint:

* Az SQL Data Warehouse számos művelethez a T-SQL szintaxisát használja. Ezenkívül a hagyományos SQL-szerkezetek széles körét támogatja, például a tárolt eljárásokat, a felhasználó által definiált függvényeket, a táblaparticionálást, az indexeket és a rendezéseket.
* Az SQL Data Warehouse emellett számos újabb SQL Server szolgáltatást is tartalmaz, beleérve a fürtözött **oszlopcentrikus** indexeket, a PolyBase-integrációt és az adatok naplózását (fenyegetésértékeléssel).
* Bizonyos T-SQL nyelvi elemek, amelyek kevésbé gyakran adatraktározás számítási feladatainál vagy újabb tooSQL kiszolgáló, nem lehet érhető el. További információkért lásd: hello [az áttelepítési dokumentáció][Migration documentation].

Hello Transact-SQL és az SQL Server, az SQL Data Warehouse, az SQL-adatbázis és a Analytics Platform System szolgáltatás tartománynevük létrehozhat olyan megoldás, amely megfelel az adattárolási igényeinek. Eldöntheti, ahol tookeep az adatok alapján teljesítmény biztonsági, és a méretezési követelmények és adatátviteli különböző rendszerek között szükség szerint.

## <a name="data-protection"></a>Adatvédelem
Az SQL Data Warehouse minden adatot az Azure Premium helyileg redundáns tárolóban tárol. Több szinkron másolatot hello adatok karbantartása hello helyi data center tooguarantee transzparens adatvédelmet honosított-hibákkal szemben. Emellett az SQL Data Warehouse rendszeres időközönként automatikusan végrehajtja az aktív (nem szüneteltetett) adatbázisok biztonsági mentését az Azure Storage Snapshots használatával. toolearn kapcsolatos biztonsági mentése és visszaállítása működését, tekintse meg a hello [biztonsági mentés és helyreállítás áttekintése][Backup and restore overview].

## <a name="integrated-with-microsoft-tools"></a>Integráció a Microsoft eszközeivel
Az SQL Data Warehouse együttműködik hello eszközök többsége a felhasználók ismerős lehet az SQL Server kiszolgálót. Ezek az eszközök a következőket foglalják magukban:

**Hagyományos SQL Server-eszközök:** az SQL Data Warehouse szolgáltatás teljesen integrálható az SQL Server Analysis Services, az Integration Services és a Reporting Services szolgáltatással.

**Felhőalapú eszközök:** az SQL Data Warehouse különböző szolgáltatásokkal integrálható az Azure-ban, beleértve az Azure Data Factory, a Stream Analytics, a Machine Learning és a Power BI szolgáltatást. A teljes listát lásd: [Az integrált eszközök áttekintése][Integrated tools overview].

**Harmadik felektől származó eszközök:** Számos külső eszközszolgáltató tanúsított módon integrálta az eszközeit az SQL Data Warehouse szolgáltatással. A teljes listát lásd: [Az SQL Data Warehouse megoldáspartnerei][SQL Data Warehouse solution partners].

## <a name="hybrid-data-sources-scenarios"></a>Hibrid adatforrások forgatókönyvei
A Polybase lehetővé teszi a tooleverage a jól ismert T-SQL-parancsok segítségével különböző forrásokból származó adatok. A Polybase lehetővé teszi, hogy a normál táblákhoz az Azure Blob storage szolgáltatásban tárolt nem relációs adatokat tooquery. Az SQL Data Warehouse Polybase tooquery nem relációs adatokat, vagy tooimport nem relációs adatok használata

* A polybase külső táblák tooaccess nem relációs adatokat. hello tábla tárolja az SQL Data Warehouse, és SQL használatával végezheti el őket, és eszközök hasonló szokványos relációs adatok elérésére.
* A PolyBase integrációs szempontból rendszerfüggetlen. Az tesz elérhetővé hello szolgáltatást és funkciót tooall hello adatforrások, hogy az támogatja-e. a Polybase által beolvasott hello adatok különböző formátumokba, beleértve a tagolt fájlokat és az ORC fájlokat is szerepelhet.
* A PolyBase is használatos storage a HDInsight-fürtök használt tooaccess blobtárolóba lehet. Ez hozzáférést tud biztosítani, hogy toohello ugyanazokat az adatokat a relációs és nem relációs eszközökkel.

## <a name="sla"></a>SLA
Az SQL Data Warehouse a Microsoft Online Services SLA részeként termékszintű szolgáltatói szerződést biztosít. További információ: [SQL Data Warehouse SLA][SLA for SQL Data Warehouse]. Minden egyéb termékekkel kapcsolatos SLA-információk hello alkalmazásról [szolgáltatói szerződések] Azure lapon, vagy letöltheti a fájlokat a hello [mennyiségi licencelés] [ Volume Licensing] lap. 

## <a name="next-steps"></a>Következő lépések
Most, hogy jobban megismerte az SQL Data Warehouse, megtudhatja, hogyan tooquickly [SQL Data Warehouse létrehozása] [ create a SQL Data Warehouse] és [mintaadatokat tölthet be][load sample data]. Ha új tooAzure, azt tapasztalhatja hello [Azure szószedet] [ Azure glossary] hasznos, új terminológia észlel. Vagy tekintsen meg néhányat a többi SQL Data Warehouse-erőforrás közül.  

* [Ügyfelek sikertörténetei]
* [Blogok]
* [Funkciókérések]
* [Videók]
* [Az ügyféltanácsadói csapat blogjai]
* [Támogatási jegy létrehozása]
* [MSDN-fórum]
* [Stack Overflow-fórum]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Támogatási jegy létrehozása]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Ügyfelek sikertörténetei]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Blogok]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Az ügyféltanácsadói csapat blogjai]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funkciókérések]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-fórum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow-fórum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videók]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[szolgáltatói szerződések]: https://azure.microsoft.com/en-us/support/legal/sla/
