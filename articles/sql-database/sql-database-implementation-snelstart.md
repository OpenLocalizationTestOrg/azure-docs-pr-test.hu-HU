---
title: "aaaAzure SQL adatbázis Azure esettanulmány - Snelstart |} Microsoft Docs"
description: "További tudnivalók: hogyan SnelStart által használt SQL-adatbázis toorapidly kibontva 1000 új Azure SQL-adatbázisok havi gyakorisággal az üzleti szolgáltatás"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: fab506b2-439d-4f1a-bdc5-d1d25c80d267
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 69572393f41a9baf44f41b4f5f4ebbef347de851
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a>Az Azure-SnelStart gyorsan bővített 1000 új Azure SQL-adatbázisok havi gyakorisággal az üzleti szolgáltatás
![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

SnelStart hello Hollandia népszerű pénzügyi - és üzleti-kezelési szoftver a kis - és közepes méretű vállalkozások számára (SMB) hoz. A személyzet 110 alkalmazottak, beleértve az informatikai részleg 35 által kiszolgált 55,000 ügyfeleinek. Asztali szoftverek tooa szoftver-,-a-(SaaS) szolgáltatásajánlat épülő Azure mozgatásával SnelStart készített legtöbb hello beépített szolgáltatás, a megszokott környezetben a C# segítségével, és a teljesítmény és méretezhetőség optimalizálása által sem over - felügyeletének automatizálására vagy a kiépítés vállalatok rugalmas készleteket használó. Azure használatával biztosít az ügyfelek a helyszíni és hello közötti áthelyezése SnelStart hello rugalmasság felhő.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-SnelStart/player]
> 
> 

## <a name="why-snelstart-extended-services-from-hello-desktop-toohello-cloud"></a>Miért SnelStart kiterjesztett hello asztali toohello felhőből szolgáltatások
> "Az Azure-ral működő azt jelenti, hogy azt is kézbesíti szoftver gyorsabb, gyorsan reagálni toocustomer iránti igények kielégítése érdekében és méretezhető megoldás, amikor iránti igények kielégítése érdekében."
> 
> – Henry lett, a szoftver felelős mérnök
> 
> 

SnelStart lefutott egy sikeres szoftver üzleti évig, egy hagyományos fejlesztési modellel: code csomagot, küldje el, majd ismételje meg. Az idő múlásával változás lépést hello nőtt, gyorsabb és gyorsabb. Szabályzat gyakran változó, és ügyfelek pénzügyi rekordok könnyebb módon tooprocess szükséges, és együttműködik a könyvelők és kormányzati tookeep be ezeket a módosításokat.

"Szállítási szoftver toocustomers volt, költséges és korlátozó," tooHenry megfelelően lett, a szoftver felelős mérnök. "Éles csomagolás és szállítási költségek korlátozott azt sikerült a gyakoriságát kibocsátási szoftver. Rendszeres szállítására toopackage frissítései volt, de, amely tette rögzített toomeet felhasználóink igények módosítása. A Microsoft sikerült soha nem biztos lehet benne, hogy az ügyfelek frissítése toohello hello termék legújabb verzióját. Ezért azt kellett toosupport több verziója is támogatja a nagyon nehéz feladat toodo hello végrehajtása."

Nincs ad hozzá, "úgy meg akartunk tooprogram és a kiadás frissítéseket, egy gyorsított lépést, így azt nem sikerült innovációját gyorsabb és az ügyfelek érdekében új szolgáltatások létrehozására. Is meg akartunk egy módon tooautomate több dolgozza fel a rendelés toosimplify felhasználóink üzleti-címfelügyeleti szükségletek."

SnelStart, a hello megoldás volt tooextend szolgáltatásai egy felhőalapú Szolgáltatottszoftver-szolgáltató váljon. hello Azure SQL Database platform segített SnelStart juthat el nélkül hello fő az informatikai részleg terhelését, amely egy infrastruktúra--a-szolgáltatásként (IaaS) megoldás lenne szükség.

hello felhős modellnek is lehetővé teszi, hogy a SnelStart toofix hibákat, és új funkciók biztosítására gyorsan, anélkül, hogy az ügyfelek toodownload és frissítési szoftver kellene. Függően tooBeen, "Using Azure felhőszolgáltatások lehetővé teszi, hogy velünk tooquickly act változó követelmények harmadik felek során. Így ahelyett hogy tooship ügyfelek egy új verzió toothousands, azt módosíthatja az asztali alkalmazások toonew formátumokból szükséges harmadik felek által küldött adatokat."

## <a name="a-modern-company-with-traditional-roots"></a>A hagyományos gyökérrel rendelkező modern vállalati
SnelStart too1964, elrendeli indításakor hello is hello vállalati, a gyártó zenei szolgáltatás részek szerény gyökérrel rendelkező modern, a gyors, a csúcstechnológiai üzleti. hello SnelStart szoftver üzleti hello lelke valóban lépések során hello elterjedése hello személyi számítógép hello 1980-as években, a zúzása. hello vállalati szükségesek egy jobb alternatív toohello nyilvántartási szoftverrel hello időpontban, így saját kezébe kérdések tartott. Hello vállalati 1982, mi végül válna SnelStart nyilvántartási szoftver hello rálátással hozott létre. Hello kezdetektől hello szoftver az egyszerűség és sebesség lett admired –, hogy hello vállalati végül fókusz megváltozott, és magát a szoftver vállalatba reinvented túl nagy.

SnelStart 1995-ben jelent meg az első könyvelés alkalmazás Windows. hello alkalmazás, a Microsoft Visual Basic 1.0-s és a Microsoft Access-adatbázisok épül segített hello ügyfél alap toomore mint 5000 felhasználója nő.

Napjainkban SnelStart egy fő szolgáltató holland SMB és az önálló dinamikus irányuló üzleti-felügyeleti alkalmazás és az üzletági (LOB). Carlo Kuip, informatikai rendszerfejlesztők szerint, mert "célunk tooprovide 100 %-os automation üzleti-felügyeleti szolgáltatások adhatunk ügyfeleink."

## <a name="optimizing-performance-and-cost-with-elastic-pools"></a>Teljesítmény és a rugalmas készletek költséget optimalizálása
SnelStart rugalmas készletek nagyméretű korai alkalmazó volt. Rugalmas készletek hello vállalati korlát kapcsolatos költségek, és hatékonyabban kezelheti a teljesítményre vonatkozó követelmények. Szerint tooBeen "rugalmas készletek használatával azt is teljesítményének optimalizálásához hello igények ügyfeleink, alapján túlzott kiosztása nélkül. Ha le kellett tooprovision csúcsterhelés alapján, elég költséges lenne. Ehelyett hello beállítás tooshare erőforrások között, több alacsony kihasználtságú adatbázisok kiválaszthatjuk toocreate is végez, és költséghatékony megoldást. "

## <a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a>Az Azure SQL-adatbázisok súgó containerize elkülönítési és biztonsági adatait
Az Azure SQL Database SnelStart tooeasily lehetővé teszi, és ügyfelek helyszíni üzleti-felügyeleti adatok tooAzure transzparens módon történő áthelyezését. hello Azure SQL Database egy kényelmes tároló, amely elszigetelésére, a határ hitelesítési, engedélyezési és egyszerű biztonsági mentési és visszaállítási képességeket biztosítja. Adatbázisok jól alkalmazható fogalmi modell üzleti felügyeleti adja meg. Függően tooCarlo Kuip, informatikai felelős mérnök, "a tároló határon belül tartalmazhatnak bizalmas adatokat, amely kritikus fontosságú tooa üzleti, és tárolja azokat a cikkeket egy elkülönített adatbázis tartja őket védettek. Azt is fiókonkénti hello adatbázis szintjén, és még automatizálásához hello felügyeleti és ezeknek az adatbázisoknak a kibővített anélkül, hogy az adatbázis-rendszergazdák (DBAs) személyzeti."

Az SQL Data Warehouse is szerepet játszik hello SnelStart biztonságot és felügyeletet szövegegység segíti a hello vállalati gyűjt telemetrikus adatokat, például a behatolás-észlelés, a felhasználó naplózása és a kapcsolatokat.

## <a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a>Azure terhet eltávolítja az, hogy a fejlesztők is időt több érték továbbítása
hello Azure platformon modell infrastruktúra terhelés eltávolítva, és SnelStart tooautomate üzembe helyezése C# kezelési kódtárakat engedélyezve. Kuip szerint, mert "volt a folyamatban lévő műveletek egy nagyon kis személyzet egyidejűleg növekvő méretezhetőséget, a sebesség és a katasztrófa utáni helyreállítás során a beállítások az ügyfelek képesek toogrow. hello shift tooservices fejlesztési felszabadulnak erőforrások toofocus a új szolgáltatások és funkciók helyett csak frissítése meglévő kódot tookeep fel az új szabályzat vagy adó kódot." Hozzáad, "Felügyeletének automatizálására, és hello SaaS ajánlat használatával, dolgozunk több érték, az ügyfelek számára anélkül, hogy a nagy beruházások toomake működési személyzeti képes toodeliver." Például az Azure és a rugalmas készletek használatával SnelStart volt képes tooadd számos új funkciót, beleértve a robusztusabb ügyféladatok integráció a bankokhoz, új számlázási szolgáltatások kisvállalati átvilágítást és e-mailek szolgáltatások.

> "Az első néhány hónappal 2016 hello pedig csak azt az Azure SQL Database 12 000-nél készül 5500 toomore telepítéseit kibontva, és azt jelenleg visszaigazolása havonta körülbelül 1000 adatbázisok."
> 
> – Henry lett, a szoftver felelős mérnök
> 
> 

Adatbázis-felügyeleti további automatizált hello rugalmas feladatok szolgáltatással. Kuip szerint, mert "magas Köszönjük hello automatikus észlelése az adatbázisok SQL DB [kiszolgáló] példányán." Ez lehetővé teszi SnelStart tooexecute felügyeleti műveletek a dinamikusan növekvő ügyfél alap többletterhelést nélkül.

SnelStart is fejleszti az API-k, amelyek a felhasználói adatok és a harmadik féltől származó szoftverek partnerek által létrehozott alkalmazások közötti közvetítőként működik. Kuip állapotok, mint "Ez az API lehetővé teszi más szállítók tooadd funkció tooour szoftverek, például számlákat és egyéb dokumentumokat esetében bevitt adat kiküszöbölése." Az ügyfelek már nem fog rendelkezni toomanually típus számlák azok kisvállalati nyilvántartási szoftver; hello SnelStart szoftver közvetlenül cserélnek hello szükséges információkat. Ez lehetővé teszi az ügyfelek toojoin az üzleti-felügyeleti feladatok és, amely a digitális átalakítása hello iparágban egyre inkább hello-ökoszisztéma.  

## <a name="how-azure-services-enable-saas-for-snelstart"></a>Hogyan Azure-szolgáltatások engedélyezése SaaS SnelStart
Azure használatával SnelStart ki tud szolgálni az ügyfelek és a könyvelők többet zökkenőmentesen hello felhőben. Például az ügyfelek és a könyvelők információkat oszthat meg az Azure-on tárolt SnelStart tartozó ügyfél API közvetlen elérésével. Kuip állapota "újrafelhasználható szolgáltatásokról elérhető tooour ügyfélkapcsolati web apps, és ezek biztosítanak közös infrastruktúrát és funkciók tooallow toobusiness felügyelete az ügyfelek és az ügyfelek által használt toothird szoftver. Például a termék-konfiguráció képességek biztosítása, tűzfal-szabályok kezelése és a hosszú futású folyamatokat, például biztonsági másolatok kezelése. "

> Célunk tooprovide 100 %-os automation üzleti-felügyeleti szolgáltatások adhatunk ügyfeleink." 
> 
> – Carlo Kuip, informatikai felelős mérnök
> 
> 

Ezenkívül SnelStart webszolgáltatások engedélyezése az ügyfelek és a könyvelők tooeasily access adatok az Azure SQL Database rugalmas készletek. A Szolgáltatottszoftver modell, adatbázis a rugalmasság és az Azure Resource Manager alapján kialakulhat SnelStart biztosít minden Azure-telepítés kiegészítő méretezhetőség szolgáltatásokkal. hello megvalósítási teljesen automatizált kezelési kódtárakat C# használatával.

![SnelStart architektúra](./media/sql-database-implementation-snelstart/figure1.png)

1. ábra. 2016. június SnelStart rendelkezik, több mint 11 000 adatbázisok és több mint 50 rugalmas készletek

## <a name="simplicity-from-hello-cloud"></a>Egyszerűség hello felhőből
Helyezze át az Azure felhőalapú megoldás tooan, SnelStart óta képes toosupport gyors felhasználói növekedési kínálja a új szolgáltatások és szolgáltatások. TooBeen, szerint "az Azure-ral, azt biztosíthat közelében folyamatos frissítések adhatunk ügyfeleink, a műveleti személyzet kiterjesztése nélkül. És azt minden hello nagyszerű Azure szolgáltatásokat – például a méretezhetőséget és katasztrófa-helyreállítás – kötegelve. "

> "Ha a rendszer ténylegesen keresztül Redmondban található irodájába... Hívás érkezett egy fejlesztő vissza a hello Hollandia, a megadott problémáról hívnia nekem Azt is képes tooget Microsoft toodeliver az éles környezetben toosolve 48 órán belül változása a probléma. "
> 
> – Henry lett, a szoftver felelős mérnök
> 
> 

SnelStart is hello szoros együttműködésben azok már hello Azure SQL-adatbázis a Microsoft szakértőivel közösen kifejlesztett figyelemreméltónak tartja. Kuip állapotok, mint "beszélgetéseket van a szolgáltatások és hogyan toouse technológia, amely valami mindkét oldalon értékeljük."
hello azonnali a SnelStart célja meg ügyfelének alap növekvő tookeep. Mivel állapotai "Hello műszaki és erőforrás-korlátozások le kellett, mint egy független, nélkül nincs nincs korlát toohow, amennyiben azt a milyen mértékben növelhető."

## <a name="more-information"></a>További információ
* toolearn Azure rugalmas készletek, kapcsolatos további információkért lásd: [rugalmas készletek](sql-database-elastic-pool.md).
* További információ a webes és feldolgozói szerepkörök toolearn lásd [feldolgozói szerepkörök](../fundamentals-introduction-to-azure.md#compute).    
* További információ az Azure SQL Data Warehouse toolearn lásd [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)
* toolearn SnelStart, kapcsolatos további információkért lásd: [SnelStart](http://www.snelstart.nl).

