---
title: "a több-bérlős SaaS-alkalmazásokhoz és az Azure SQL Database aaaDesign minták |} Microsoft Docs"
description: "Ez a cikk ismerteti, amelyek hello követelmények és a közös adatok architektúra mintákat egy felhőalapú környezetben futó, több-bérlős adatbázis alkalmazások tooconsider kell, és ezeket a mintákat társított különböző mellékhatásokkal hello. Azt is bemutatja, hogyan Azure SQL Database rugalmas készletek és rugalmas eszközök segítségével nem-nem biztonságos módon követelménynek."
keywords: 
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 1dd20c6b-ddbb-40ef-ad34-609d398d008a
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: sqldb-design
ms.date: 02/01/2017
ms.author: srinia
ms.openlocfilehash: a4b58935b08cb78792e65a675d680de708b709fc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-patterns-for-multi-tenant-saas-applications-and-azure-sql-database"></a>A kialakítási minták a több-bérlős SaaS-alkalmazásokhoz és az Azure SQL adatbázis
Ebből a cikkből megtudhatja hello követelményeiről és a közös adatok architektúra minták a több-bérlős szoftverek, mint egy szolgáltatott szoftverként (SaaS) adatbázis-alkalmazások, amelyek egy felhőalapú környezetben futnak. Hello tényezők tooconsider kell, és annak különböző tervezési kompromisszumot hello is ismerteti. Rugalmas készletek és az Azure SQL Database rugalmas eszközök segíthetnek a igényeknek más célok veszélyeztetése nélkül.

A fejlesztők néha döntéseket, amely azokat a vállalat kiszolgálása modelleket hello adat szintet a több-bérlős alkalmazásokhoz a tervezésekor a hosszú távú érdekében dolgozhat. Kezdetben legalább egy fejlesztő előfordulhat, hogy alapvető változtatásokat hozott a könnyű fejlesztés és alacsonyabb felhőalapú szolgáltatás szolgáltató költségeket, mint a fontosabb, mint az alkalmazás bérlői elkülönítési vagy hello méretezhetőségét. Ez a választás vezethet toocustomer elégedettségének maillel és költséges állomásokon javítási később.

Egy több-bérlős alkalmazás üzemeltetett felhőalapú környezetek alkalmazás, és azonos beállítása szolgáltatások toohundreds vagy a bérlő nem megosztása, vagy másik adatok megtekintéséhez több ezer hello biztosít. Példa: egy SaaS-alkalmazás, amely a felhőben üzemeltetett környezetben a szolgáltatások tootenants biztosít.

## <a name="multi-tenant-applications"></a>Több-bérlős alkalmazásokhoz
A több-bérlős alkalmazásokhoz adatokhoz és a munkaterhelés lehet könnyen particionálni. Akkor is partícióazonosító adatok és a munkaterhelés, például mentén bérlői határokat, mert legtöbb kérelem egy bérlő hello határain belül történik. Tulajdonság rejlő hello adatok és hello munkaterhelés, és azt a cikkben említett hello alkalmazás minták előtérbe.

A fejlesztők Ez az alkalmazástípus használata a felhőalapú alkalmazások, beleértve a teljes skáláját hello:

* Partner adatbázis-alkalmazások éppen állapotra váltott toohello felhőalapú, SaaS-alkalmazásokhoz
* SaaS-alkalmazásokhoz készült szabad hello hello felhőben
* Közvetlen, ügyfélkapcsolati alkalmazások
* Alkalmazott számára is elérhető a vállalati alkalmazások

Hello felhő- és gyökérrel rendelkező, a partner adatbázis-alkalmazások általában több-bérlős alkalmazásokhoz készült SaaS-alkalmazásokhoz. A Szolgáltatottszoftver-alkalmazáshoz egy speciális szoftver alkalmazásra, amely a szolgáltatás tootheir bérlők biztosításához. Bérlők hello alkalmazásszolgáltatás elérheti és rendelkezik a társított hello alkalmazásokban tárolt adatok teljes tulajdonjogát. De tootake előnyeit, SaaS, a bérlők hello előnyei kell lemondást néhány szabályozhatják, hogy a saját adataikat. Hello SaaS service provider tookeep megbízhatóságát az adataikat, biztonságos és különítve a többi bérlő adatokat. A több-bérlős SaaS-alkalmazás ilyen többek között az MYOB SnelStart és a Salesforce.com. Mindkét alkalmazás bérlői határokat és támogatási hello alkalmazás tervezési mintáról olvashat arról lesz szó ebben a cikkben mentén lehet particionálni.

Alkalmazásokat, amelyek egy közvetlen szolgáltatás toocustomers vagy tooemployees (gyakran hivatkozott tooas felhasználók helyett bérlők) szervezeten belül olyan másik kategóriához hello több-bérlős alkalmazás pontszámot. Az ügyfelek toohello szolgáltatás előfizetés, és nem a saját tulajdonában szolgáltató hello hello adatokat gyűjt, és tárolja. Szolgáltatók rendelkezik kevésbé szigorú követelményeket tookeep túl kormányzati megbízott adatvédelmi előírásokat egymástól különítve az ügyfelek adatokat. Az ilyen ügyfélkapcsolati több-bérlős alkalmazás példák media tartalomszolgáltatók például Netflix, a Spotify és az Xbox LIVE. További példák könnyen particionálható alkalmazások ügyfélkapcsolati, Internet-skálázási alkalmazást, vagy az eszközök internetes hálózatát (IoT) alkalmazást, mely minden felhasználói vagy az eszköz lehet a partíció szolgál. Partícióhatárok elkülönítheti a felhasználók és eszközök számára.

Nem minden alkalmazások partíció könnyen mentén egyetlen tulajdonság például bérlői, felhasználói, felhasználó vagy eszköz. Egy összetett vállalati erőforrás-tervező (ERP) alkalmazás, például a termékek, rendeléseket és ügyfelek rendelkezik. Általában tartalmaz egy összetett séma több ezer magas egymáshoz kapcsolódó táblákat.

Nincs egyetlen partícióra stratégia tooall táblák alkalmazását és az alkalmazás munkaterhelés keresztül működnek. Ez a cikk foglalkozik a több-bérlős az alkalmazásokat, amelyek könnyen particionálható adatok és a munkaterhelések.

## <a name="multi-tenant-application-design-trade-offs"></a>Több-bérlős alkalmazás tervezési kompromisszumot
egy több-bérlős alkalmazásfejlesztő úgy dönt, általában hello kialakításban veszi figyelembe az alábbi tényezők hello alapul:

* **Bérlők elkülönítésének**. a fejlesztőnek hello tooensure, hogy nincs bérlői nemkívánatos hozzáférés tooother bérlők adatokat tartalmaz. hello elkülönítési követelmény tooother tulajdonságait, például a védelem biztosítása a zajos szomszédok, képes toorestore éppen egy bérlő adatok és a bérlői-specifikus testreszabásokat terjeszti ki.
* **A felhő erőforrásköltségeket**. Egy SaaS-alkalmazáshoz kell toobe költség-versenyképesség fenntartása érdekében. Egy több-bérlős alkalmazásfejlesztő előfordulhat, hogy az alacsonyabb költségek toooptimize válassza a felhőalapú erőforrások, például a tárolási hello használatával, és számítási költségeket.
* **DevOps könnyű**. Egy több-bérlős alkalmazásfejlesztőnek tooincorporate elkülönítési védelmi karbantartása, és az alkalmazás és az adatbázis-séma hello állapotának figyelése és bérlői problémák elhárításához. Alkalmazásfejlesztés és művelet összetettségét lefordítja közvetlenül tooincreased költség, és felkéri a bérlő számára.
* **Méretezhetőség**. hello képességét tooincrementally adja hozzá a további bérlők és kapacitást a bérlők számára igénylő imperatív tooa sikeres SaaS művelet.

Ezek a tényezők mindegyikén kompromisszumot képest tooanother. hello legkisebb költségű felhőalapú ajánlat esetleg nem nyújtanak hello legkényelmesebben fejlesztési felület. Fontos a fejlesztő toomake szerez tudomást döntések ezeket a beállításokat és azok kompromisszumot hello alkalmazás tervezési folyamat során.

A legnépszerűbb fejlesztési minta toopack van néhány adatbázisát. a több bérlő. Ez a módszer előnyei hello nincsenek alacsonyabb költségű, mert néhány adatbázisok és hello relatív egyszerűség az adatbázisok száma korlátozott kell fizetnie. De idővel SaaS több-bérlős alkalmazásfejlesztő fog fontos megjegyezni, hogy ez a beállítás a bérlők elkülönítését és a méretezhetőség jelentős downsides. Ha a bérlők elszigetelésére válik a további műveleteket az szükséges tooprotect bérlő adatai a megosztott tárolóban, az illetéktelen hozzáféréstől és zajos szomszédok. Ebből a törekvésből jelentősen növeli a előfordulhat, hogy a fejlesztéshez és elkülönítési karbantartási költségek. Ehhez hasonlóan hozzáadása a bérlők szükség, ha ebben a kialakítási mintában általában szakértői tooredistribute bérlői adatok szükséges adatbázisok tooproperly méretezési hello adatrétegbeli alkalmazás között.  

Gondoskodjon arról, hogy toobusinesses és a szervezetek SaaS-több-bérlős alkalmazásokhoz a alapvető gyakran a bérlők elszigetelésére feltétele. A fejlesztők által észlelt előnyöket egyszerűség és a költségeket a bérlők elkülönítését és a méretezhetőség tempted előfordulhat, hogy lehet. A kompromisszum bizonyítja összetett és költséges, hello szolgáltatás növekszik és a bérlői elkülönítési követelményei válnak fontos, és felügyelt hello alkalmazás rétegben. A több-bérlős alkalmazásokat, amelyek egy közvetlen, a felhasználók felé néző szolgáltatás toocustomers, azonban bérlők elszigetelésére lehet optimalizálja a felhő erőforrás költsége alacsonyabb prioritású.

## <a name="multi-tenant-data-models"></a>Több-bérlős adatmodelljeiről
Bérlői adatok elhelyezéséhez közös tervezési eljárásokat hajtsa végre a három különböző modellt, 1. ábrán látható.

![több-bérlős alkalmazás-adatmodellek](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-multi-tenant-data-models.png)

1. ábra: Több-bérlős adatmodellekben eljárások általános tervezési

* **Adatbázis-/-bérlő**. Mindegyik bérlő saját adatbázis van. Az összes bérlői vonatkozó adatokat zárt toohello bérlői adatbázis, és más bérlők és az adataik egymástól.
* **A megosztott adatbázis-szilánkos**. Több bérlő több adatbázisok közül az egyik megosztani. Például a kivonat, a tartomány vagy a lista particionálás jó particionálási stratégia segítségével egy meghatározott készletét bérlők tooeach adatbázis hozzá van rendelve. Az adatok terjesztési stratégia gyakran hivatkozott tooas horizontális.
* **Adatbázis-egyetlen megosztott**. Egyetlen néha nagy adatbázisban adatokat tartalmaz, az összes-bérlőkkel, amelyek egy bérlői azonosító oszlop használatát is.

> [!NOTE]
> Az alkalmazásfejlesztő előfordulhat, hogy válasszon tooplace különböző bérlők különböző adatbázis sémák, és hello séma neve toodisambiguate hello különböző bérlők használja. Nem ajánlott ennek a megközelítésnek mert dinamikus SQL hello használata általában szüksége van, és hatékonyan terv gyorsítótárazás nem lehet. Hello a cikk hátralévő része, az azt összpontosítani megosztott hello tábla megközelítés a kategória több-bérlős alkalmazás is.
> 
> 

## <a name="popular-multi-tenant-data-models"></a>Népszerű több-bérlős adatmodelljeiről
Több-bérlős modellekhez hello alkalmazás tervezési kompromisszumot már felismertük szempontjából fontos tooevaluate hello különböző. Ezek a tényezők segítségével jól jellemezhető hello három leggyakoribb több-bérlős adatmodellekben korábban leírt és az adatbázis-használat a 2. ábrán látható módon.

* **Elkülönítés**. bérlők elkülönítését hello mértékű lehet biztosítása mennyi bérlők elszigetelésére adatmodellt éri el.
* **A felhő erőforrásköltségeket**. bérlők közötti erőforrás-megosztás hello mennyisége felhő erőforrás költség is optimalizálhatja. Egy erőforrás hello számítási és tárolási költség adható meg.
* **DevOps költség**. hello a könnyű alkalmazásfejlesztést, a központi telepítés és a kezelhetőségét csökkenti az általános SaaS működési költséget.  

A 2. ábrán hello Y tengely a bérlők elszigetelésére hello szintjét mutatja. hello X tengely hello szintű erőforrás-megosztás jeleníti meg. hello szürke átlós hello középső irányát nyíl hello DevOps költségek mentesítse tooincrease vagy csökken.

![Több-bérlős népszerű alkalmazás-tervezési minták](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-popular-application-patterns.png)

2. ábra: Több-bérlős népszerű adatok modellek

hello jobb alsó részre osztott a 2. ábrán látható egy alkalmazás mintát, amely potenciálisan nagy, megosztott adatbázist használ, illetve hello megosztott tábla (vagy különálló séma) módszer. Az erőforrás-megosztás, mivel minden bérlők azonos adatbázis-erőforrások (Processzor, memória, bemeneti/kimeneti) egyetlen adatbázisban hello helyes legyen. De a bérlők elszigetelésére korlátozva. Szükség lehet tootake további lépéseket tooprotect bérlők egymástól hello alkalmazás rétegben. Ezeket a további lépéseket jelentősen növelheti a hello DevOps költségét fejlesztésének és kezelésének hello alkalmazás. Méretezhetőség hello-adatbázist futtató hardver hello hello mérete korlátozza.

hello bal alsó részre osztott a 2. ábrán látható szilánkos több bérlő több adatbázis közötti (általában a más hardverekre skálázási egységek). Az egyes adatbázisok bérlők, egy részét, amely megoldja hello méretezhetőség kapcsolatos azon aggodalmát, annak a más üzemelteti. Ha további kapacitás szükséges további bérlők, könnyen elhelyezheti hello bérlők toonew hardver méretezési egységek lefoglalt új adatbázisokat. Azonban erőforrások megosztása hello mennyisége csökken. Csak a bérlők helyezve hello azonos méretezési egységek osztozik az erőforrásokon. Ez a megközelítés kevés fokozása tootenant elkülönítést is biztosít, mert több bérlő továbbra is közös elhelyezésű nélkül egymás műveletekből automatikusan védett. Alkalmazás összetettsége továbbra is magas.

a 2. ábrán hello bal felső részre osztott hello harmadik módszer. Mindegyik bérlő adatok helyezi a saját adatbázisában. Ez a megközelítés jó bérlői-elkülönítési tulajdonságokkal rendelkezik, de korlátozza az erőforrás-megosztás, ha az egyes adatbázisok saját dedikált erőforrások. Ez a módszer akkor hasznos, ha minden bérlők előre jelezhető munkaterhelésekkel rendelkeznek. Ha a bérlői terhelések kevesebb előre jelezhető, hello szolgáltató erőforrások megosztása nem lehet optimalizálni. Által esetében gyakori, az SaaS-alkalmazásokhoz. hello szolgáltató vagy kell túlzott kiépíteni, toomeet iránti igények kielégítése érdekében vagy alacsonyabb erőforrásokat. Mindkét művelet vagy magasabb költségek, vagy alacsonyabb bérlői elégedettségének eredményez. Erőforrás-megosztás bérlők között nagyobb fokú kívánatos toomake hello megoldás költséghatékonyabb válik. Hello növekvő számú adatbázis is növeli a DevOps költség toodeploy és hello alkalmazás karbantartása. Annak ellenére, hogy ezeket a problémákat Ez a módszer el vannak különítve hello legjobb és legegyszerűbb a bérlők számára.

Ezek a tényezők is befolyásoló hello kialakítási minta egy ügyfél:

* **Bérlői adatok tulajdonjogát**. Egy alkalmazás bérlők tartsa meg a saját adatok tulajdonjogát előtérbe bérlőnként egy önálló adatbázis hello mintáját.
* **Méretezés**. Egy alkalmazás, amelynek célja a több ezer vagy a bérlők több millió több száz adatbázismegosztás módszerek, például a horizontális előtérbe. Elkülönítési követelményei továbbra is kihívást is jelent.
* **Érték és az üzleti modell**. Ha egy alkalmazás / bérlői bevétel Ha kis (kevesebb mint dollár), a kevésbé fontos válnak elkülönítési követelményei és a megosztott adatbázis teljesen logikus. Ha a bérlő bevétel néhány dollár vagy több, egy adatbázis-/-bérlős modell több valósítható meg. Akkor lehet, hogy fejlesztési a költségek csökkentése érdekében.

Az ideális több-bérlős modell megadott hello tervezési kompromisszumot alakítson ki a 2. ábrán látható, a bérlők közötti optimális erőforrás kell tooincorporate jó bérlői elkülönítési tulajdonságok. Ez a modell elfér hello kategória hello jobb felső részre osztott 2. ábra ismerteti.

## <a name="multi-tenancy-support-in-azure-sql-database"></a>Több-bérlős támogatás az Azure SQL-adatbázis
Az Azure SQL Database támogatja a 2. ábra leírt minden alkalmazás több-bérlős minták. A rugalmas készletek is támogatja. az alkalmazás mintát, amely egyesíti a helyes erőforrás-megosztás és hello elkülönítési előnyei hello adatbázis-/-bérlő közelítse (lásd a 3. ábra hello jobb felső részre osztott). Rugalmas adatbáziseszközöket és a képességek az SQL-adatbázis segítségével csökkentheti a hello költség toodevelop és sok más (a 3. ábra árnyékolt hello területen látható) alkalmazások működik. Ezek az eszközök segítségével létrehozása és kezelése hello több adatbázis minták bármelyikét használó alkalmazásokat.

![Az Azure SQL-adatbázis minták](./media/sql-database-design-patterns-multi-tenancy-saas-applications/sql-database-patterns-sqldb.png)

3. ábra: Több-bérlős alkalmazás mintákat az Azure SQL-adatbázis

## <a name="database-per-tenant-model-with-elastic-pools-and-tools"></a>Adatbázis-/-bérlős modell rugalmas készletek és eszközök
Az SQL Database rugalmas készletek kombinálhatja a bérlők elkülönítését a erőforrás-megosztás bérlői adatbázisok toobetter támogatási hello adatbázis / bérlői megközelítés között. SQL-adatbázis egy olyan adatok réteg megoldás a Szolgáltatottszoftver-szolgáltatók számára olyan több-bérlős alkalmazásokat hozhat létre. erőforrás-megosztás bérlők között hello terhétől átvált rétegből hello alkalmazás réteg toohello adatbázis szolgáltatás. rugalmas feladat, a Rugalmas lekérdezési, a rugalmas tranzakciók és a hello elastic database ügyféloldali kódtárának egyszerűsödött, kezelése és az adatbázisok közötti léptékű lekérdezése hello összetettsége.

| Alkalmazáskövetelmények | SQL-adatbázis képességek |
| --- | --- |
| Bérlők elkülönítését és erőforrás-megosztás |[Rugalmas készletek](sql-database-elastic-pool.md): az SQL Database-erőforrásokat foglal le, és különböző adatbázisok közötti hello erőforrások megosztása. Is, az egyes adatbázisok is felhasználhatja a legtöbb erőforrást hello készlet szükséges tooaccommodate kapacitás igény szerinti teljesítményt, a bérlői terhelések esedékes toochanges. a rugalmas készlet hello maga is méretezhető felfelé vagy lefelé igény szerint. Rugalmas készletek is adja meg a könnyű kezelhetőség és figyelés és hibaelhárítás hello készlet szinten. |
| Az adatbázisok közötti DevOps könnyű |[Rugalmas készletek](sql-database-elastic-pool.md): mivel korábban feljegyzett. |
| | [Rugalmas lekérdezési](sql-database-elastic-query-horizontal-partitioning.md): az adatbázisok közötti lekérdezés vagy a kereszt-bérlő elemzés. |
| | [Rugalmas feladat](sql-database-elastic-jobs-overview.md): csomag és megbízhatóan telepíteni az adatbázis-karbantartási műveletek vagy adatbázisséma toomultiple adatbázisok változik. |
| | [Rugalmas tranzakciók](sql-database-elastic-transactions-overview.md): megváltoznak tooseveral adatbázisok atomi és elkülönített módon. Rugalmas tranzakciók van szükség, amikor az alkalmazások több adatbázis-műveletek "mindent vagy semmit" garanciák van szükségük. |
| | [A rugalmas adatbázis ügyféloldali kódtár](sql-database-elastic-database-client-library.md): adatok terjesztéseket kezelése és a térkép bérlők toodatabases. |

## <a name="shared-models"></a>Megosztott modellek
Ismertetett módon, a legtöbb Szolgáltatottszoftver-szolgáltatók, a megosztott modell megközelítés bérlői elkülönítési kapcsolatos problémákkal rendelkező problémák és összetett szolgáltatásokkal, az alkalmazások fejlesztését és karbantartási nézve jelent. Előfordulhat azonban, a több-bérlős alkalmazásokat, amelyek a szolgáltatás közvetlenül tooconsumers, bérlői elkülönítési követelményei nem egy minimalizálja a költség, a magas prioritású. Egy vagy több adatbázis egy nagy sűrűségű tooreduce költségek képes toopack bérlők lehetnek. Egy egyetlen vagy több szilánkos adatbázis használ a megosztott adatbázis modellek az erőforrás-megosztás és a teljes költség további hatékonyság eredményezheti. Azure SQL Database is tartalmaz néhány funkció, amely segít a felhasználóknak a nagyobb biztonság és a felügyelet hello adatok réteg elkülönítési build.

| Alkalmazáskövetelmények | SQL-adatbázis képességek |
| --- | --- |
| Biztonsági elkülönítés funkciói |[Biztonsági szint](https://msdn.microsoft.com/library/dn765131.aspx) |
| [Adatbázis-séma](https://msdn.microsoft.com/library/dd207005.aspx) | |
| Az adatbázisok közötti DevOps könnyű |[Rugalmas lekérdezés](sql-database-elastic-query-horizontal-partitioning.md) |
| | [Rugalmas feladatok](sql-database-elastic-jobs-overview.md) |
| | [Rugalmas tranzakciók](sql-database-elastic-transactions-overview.md) |
| | [Elastic Database-ügyfélkódtár](sql-database-elastic-database-client-library.md) |
| | [Rugalmas adatbázis felosztása és egyesítése](sql-database-elastic-scale-overview-split-and-merge.md) |

## <a name="summary"></a>Összefoglalás
Bérlői elkülönítési követelményei fontosak az többsége több-bérlős SaaS-alkalmazásokhoz. hello ajánlott beállítás tooprovide elkülönítési leans fokozottan hello adatbázis / bérlői megközelítés felé. hello más két megközelítés szükséges összetett alkalmazás rétegek jelentősen növeli a költségeket és a kockázati gyakorlott fejlesztési csapattal tooprovide elkülönítését igénylő befektetések. Ha elkülönítési követelményei a számításból korai szakaszában hello szolgáltatások fejlesztéséhez, azokat alkalmazásával még több költséges lehet hello első két modellekben. hello fő hátrányai hello adatbázis / bérlői modellhez társított kapcsolódó tooincreased felhő erőforrás költségek miatt tooreduced megosztását, és fenntartására, és több adatbázis kezelésére is. SaaS-alkalmazások fejlesztői gyakran ezek kompromisszumot elkészítésekor kihívást jelent.

Bár kompromisszumot lehet, hogy a legtöbb adatbázis felhőszolgáltatók jelentős korlátokat, az Azure SQL Database kiküszöböli hello korlátok a rugalmas készlet és a rugalmas adatbázis-képességeket. SaaS-fejlesztők egy adatbázis-/-bérlős modell hello elkülönítési jellemzői egyesítheti és optimalizálása erőforrás megosztása és hello kezelhetőségi fejlesztések a fő sok adatbázisok rugalmas készletek és a kapcsolódó eszközök segítségével.

Több-bérlős alkalmazás szolgáltatók rendelkező, nem bérlői elkülönítési követelményei, és egy adatbázisban nagy sűrűségű bérlők számára is csomag Előfordulhat, hogy a megosztott adatok modellek eredményt a további hatékonyságának erőforrás-megosztás, és csökkentheti a teljes költség szempontjából. Az Azure SQL Database rugalmas adatbáziseszközöket, horizontális szalagtárak és biztonsági szolgáltatások segítségével a Szolgáltatottszoftver-szolgáltatók létrehozása és kezelése a több-bérlős alkalmazásokhoz.

## <a name="next-steps"></a>Következő lépések
[Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md) azt mutatja be, hello ügyféloldali kódtár minta alkalmazáshoz.

Hozzon létre egy [rugalmas készlet egyéni irányítópult szoftverszolgáltatások](https://github.com/Microsoft/sql-server-samples/tree/master/samples/manage/azure-sql-db-elastic-pools-custom-dashboard) rugalmas készletek adatbázis költséghatékony, méretezhető megoldást használó minta alkalmazáshoz.

Eszközeivel hello Azure SQL Database túl[telepítse át a meglévő adatbázisok tooscale kimenő](sql-database-elastic-convert-to-use-elastic-tools.md).

egy rugalmas készlet használatával toocreate hello Azure portál, lásd: [hozzon létre egy rugalmas készlet](sql-database-elastic-pool-manage-portal.md).  

Ismerje meg, hogyan túl[felügyeletéhez és kezeléséhez a rugalmas készletekben](sql-database-elastic-pool-manage-portal.md).

## <a name="additional-resources"></a>További források

* [Központi telepítése, és vizsgálja meg a több-bérlős alkalmazás az Azure SQL Database - Wingtip SaaS használ](sql-database-saas-tutorial.md)
* [Mi az Azure rugalmas készletek?](sql-database-elastic-pool.md)
* [Horizontális felskálázás az Azure SQL Database segítségével](sql-database-elastic-scale-introduction.md)
* [több-bérlős alkalmazásokhoz a rugalmas adatbáziseszközöket és sorszintű biztonsággal](sql-database-elastic-tools-multi-tenant-row-level-security.md)
* [A több-bérlős alkalmazásokban az Azure Active Directory és az OpenID Connect hitelesítési](../guidance/guidance-multitenant-identity-authenticate.md)
* [A Tailspin Surveys-alkalmazás](../guidance/guidance-multitenant-identity-tailspin.md)


## <a name="questions-and-feature-requests"></a>Kérdések és funkciókra vonatkozó kérések

A kérdésekhez megkeresés a hello [SQL-adatbázis fórum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted). Adja hozzá a szolgáltatás kérése hello [SQL adatbázis-visszajelzési fórumon](https://feedback.azure.com/forums/217321-sql-database/).

