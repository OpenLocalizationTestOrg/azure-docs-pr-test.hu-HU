---
title: "aaaAzure SQL adatbázis Azure esettanulmány - Umbraco |} Microsoft Docs"
description: "Hogyan Umbraco használ SQL-adatbázis tooquickly biztosítása és a skála szolgáltatások bérlők akár több ezer hello felhőben megismerése"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 5243d31e-3241-4cb0-9470-ad488ff28572
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 93e39e509831a5ff90f129d9537ece0b0dafef0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="umbraco-uses-azure-sql-database-tooquickly-provision-and-scale-services-for-thousands-of-tenants-in-hello-cloud"></a>Umbraco használja az Azure SQL Database tooquickly biztosítása és a skála szolgáltatások bérlők akár több ezer hello felhőben
![Umbraco embléma](./media/sql-database-implementation-umbraco/umbracologo.png)

Umbraco egy olyan népszerű nyílt forráskódú tartalom-felügyeleti rendszer (CMS) is semmit kis kampány vagy ismertetőben helyekről toocomplex alkalmazásokat futtató Fortune 500 vállalatok és a globális media webhelyek számára. 

> "Tudunk igen nagy Közösség fejlesztőkkel több mint 100 000 fórumainkat, és több mint 350,000 helyeket, amelyek élő, Umbraco futtató hello rendszert használó fejlesztők számára."
> 
> – Péter sem Christensen, vezető, Umbraco
> 
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-Umbraco/player]
> 
> 

toosimplify felhasználói telepítés, Umbraco hozzáadott Umbraco,--szolgáltatás (UaaS): egy szoftver,--szolgáltatás (SaaS) ajánlat, amely szükségtelenné teszi hello helyszíni telepítésekkel beépített skálázás, és kezelési terhet eltávolítja engedélyezésével a fejlesztők toofocus megoldás helyett innováció felügyeleti termék. Umbraco megtalálható összes ezek előnyeit hello rugalmas platform,--szolgáltatás (PaaS) modell a Microsoft Azure által kínált képes tooprovide.

UaaS lehetővé teszi, hogy a SaaS ügyfél toouse Umbraco CMS képességeket, amelyek korábban már azok a reach kívül. Ezen ügyfelek ki van építve a CMS munkakörnyezet, amely tartalmazza az éles adatbázis. Az ügyfelek tootwo további adatbázisok fejlesztési és átmeneti környezet, attól függően, hogy a rájuk vonatkozó követelményeket is hozzáadhat. Ha egy új környezetre van szükség, egy automatikus folyamat rendeli hozzá, hogy a felhasználói adatbázis automatikusan. hello új adatbázist (másodpercben), készen áll, mert hello adatbázis van már előre kiépítve által Umbraco az elérhető adatbázisok (lásd az 1. ábra) Azure rugalmas készletből.

![Umbraco létesítési életciklusa](./media/sql-database-implementation-umbraco/figure1.png)

1. ábra. Üzembe helyezési életciklusa Umbraco szolgáltatásként (UaaS)

## <a name="azure-elastic-pools-and-automation-simplify-deployments"></a>Az Azure rugalmas készletek és automatizálás egyszerűsítése érdekében a központi telepítések
Az Azure SQL Database és más Azure-szolgáltatásokkal Umbraco ügyfelek önálló építhető ki a környezetben, és Umbraco könnyen figyelheti és kezelheti az adatbázisokat egy egyszerűen elsajátítható munkafolyamat részeként:

1. Üzembe helyezés
   
   Umbraco 200 elérhető előtti ponton aktívvá vált-adatbázisok rugalmas készletek kapacitású tart fenn. Egy új ügyfél előfizet UaaS, Umbraco biztosít, ha egy új CMS környezettel közel valós idejű hello ügyfél hozzárendelésével adatbázis hello rendelkezésre állási készletből.
   
   Amikor egy rendelkezésre állási készlet eléri az küszöböt, létrejön egy új rugalmas készletet, és új adatbázisok előtti ponton aktívvá vált toobe hozzárendelt toocustomers, igény szerint.
   
   Megvalósítási teljesen automatizált C# kezelési kódtárakat, és az Azure Service Bus-üzenetsorok használatával.
2. Használata
   
   Az ügyfelek egyikével toothree környezetekben (a termelési, átmeneti, és/vagy fejlesztői), minden egyes saját adatbázissal. Felhasználói adatbázisokat rugalmas készletek, amely lehetővé teszi a hatékony a skálázás anélkül, hogy tooover-provision Umbraco tooprovide szerepelnek.
   
   ![Umbraco projekt áttekintése](./media/sql-database-implementation-umbraco/figure2.png)
   
   ![Umbraco projekt részletei](./media/sql-database-implementation-umbraco/figure3.png)
   
   2. ábra Projekt áttekintése és a részleteket megjelenítő Umbraco,--szolgáltatás (UaaS) ügyfél-webhely
   
   Az Azure SQL Database fogyaszt adatbázis-tranzakciós egységek (dtu-k) toorepresent hello relatív valós adatbázis-tranzakció szükséges. A UaaS ügyfelek adatbázisok általában körülbelül 10 dtu-k működnek, de mindegyik rendelkezik-e hello rugalmasság tooscale az igény szerinti. Ez azt jelenti, hogy UaaS is biztosítja, hogy az ügyfelek mindig szükséges erőforrásokat, akár csúcsidőben. Például a legutóbbi vasárnap sport események esetén egy UaaS ügyfél észlelt adatbázis csúcsait mentése too100 Dtu hello játék hello időtartamára. Az Azure rugalmas készletek tette lehetővé a Umbraco toosupport, hogy a nagy érdeklődés teljesítmény csökkenése nélkül.
3. Figyelés
   
   Umbraco figyelők adatbázis-tevékenység irányítópultok belül hello Azure-portálon, és egyéni e-mailes riasztásokhoz.
4. Vészhelyreállítás
   
   Azure két vészhelyreállítás (DR) lehetőségeket is kínál: aktív georeplikáció és georedundáns helyreállítás. hello vész-Helyreállítási lehetőség, amely egy vállalati ki kell jelölni függ a [üzleti folytonosságot biztosító célok](sql-database-business-continuity.md).
   
   aktív georeplikáció biztosít hello leggyorsabb hello esemény állásidő válasz. Aktív georeplikációt használ, be a különböző régiókban található kiszolgálók a toofour olvasható másodlagos adatbázis hozhat létre, és majd kezdeményezheti feladatátvételi tooany több hello másodlagos adatbázist, a hibák hello eseményben.
   
   Umbraco a georeplikáció nem igényel, de azt kihasználni Azure georedundáns helyreállítás toohelp ellenőrizze a nem tervezett kimaradás hello esemény minimális állásidővel. georedundáns helyreállítás adatbázis a biztonsági másolatok georedundáns Azure storage támaszkodik. Felhasználók toorestore egy biztonsági másolatból, amely lehetővé teszi, ha kimaradás hello elsődleges régióban.
5. Deaktiválás kiépítése
   
   A projekt környezetekben a törölt a rendszer eltávolítja a társított adatbázisokban (fejlesztési, előkészítési vagy élő), Azure Service Bus várólista törlése során. Ez a folyamat automatikus visszaállítások hello nem használt adatbázisok tooUmbraco rugalmas adatbázis-rendelkezésre állási készletbe, így maximális kihasználtsági megőrzésével jövőbeli kialakítási elérhetővé válnak.

## <a name="elastic-pools-allow-uaas-tooscale-with-ease"></a>Rugalmas készletek UaaS tooscale alkalmazásaiba engedélyezése.
Kihasználásával Azure rugalmas készletek, Umbraco is optimalizálhatja a teljesítmény az ügyfelek számára anélkül, hogy tooover - vagy hiány kiépítéséhez. Umbraco jelenleg majdnem 3000 olyan adatbázisok 19 rugalmas készletek között, akkor a hello képességét tooeasily méretezése szükséges tooaccommodate, a meglévő 325,000 ügyfeleknek vagy készen toodeploy egy CMS hello felhőben új felhasználói.

Valójában szerint tooMorten Christensen, műszaki vezethet, Umbraco, "UaaS most tapasztalható növekedése körülbelül 30 naponta az új ügyfelek. Ügyfeleink nagyon hello kényelmi célokat szolgál, hogy az képes tooprovision új projektek másodpercben, azonnal frissítések tootheir élő helyeket a fejlesztési környezet "egykattintásos központi telepítéssel" közzététele, és ugyanúgy, mint gyorsan módosításokat, ha találnak hibák . "

Ha az ügyfél nem igényel többé egy második, illetve harmadik környezetben, egyszerűen távolíthat el ezeket a környezetben. Amely szabaddá válnak más ügyfelek számára hello Umbraco rugalmas adatbázis-rendelkezésre állási készlet részeként használható erőforrásokat.

![Umbraco üzembe helyezési architektúrája](./media/sql-database-implementation-umbraco/figure4.png)

3. ábra. A Microsoft Azure UaaS üzembe helyezési architektúrája

## <a name="hello-path-from-datacenter-toocloud"></a>datacenter toocloud hello elérési útja
Ha hello Umbraco fejlesztők kezdetben hello döntési toomove tooa Szolgáltatottszoftver-modell, azok tudta, hogy egy költséghatékony és skálázható módon toobuild hello szolgáltatást ki kell.

> "a rugalmas készletek az SaaS ajánlott, mert azt felfelé és lefelé igény szerint tárcsázza kapacitás tökéletes. Kiépítés egyszerű, és a telepítés részeként, azt is megtarthatják az kihasználtsági legfeljebb."
> 
> – Péter sem Christensen, vezető, Umbraco
> 
> 

"Toospend meg akartunk az idő megoldás, a felhasználók problémák, nem kezelheti az infrastruktúrát. Toomake meg akartunk megkönnyítik a felhasználók tooget hello legtöbb érték "Niels Hartvig, Umbraco a alapító szerint. "Kezdeti minősül ragozott formáival hello kiszolgálókat üzemeltető, de a kapacitástervezés lett volna a nightmare." Helyezi Umbraco nem alkalmaz minden adatbázis-rendszergazdák UaaS használatának legfontosabb értékajánlatához aláhúzásjeleket tartalmazhatnak, amelyek.

Egy fontos célja a hello Umbraco fejlesztők tooprovide UaaS ügyfelek tooprovision környezetek úgy lett, gyorsan és a kapacitás korlátozások nélkül. De biztosít egy dedikált, üzemeltetett szolgáltatás Umbraco adatközpontokban lenne szükséges rengeteg többletkapacitással toohandle felszakadásáig a feldolgozása. Amely akkor kell azt jelentette, hogy jelentős számítási infrastruktúrát használ, rendszeresen túl hozzáadása.

Emellett a hello Umbraco fejlesztői csapat olyan megoldás, amely lehetővé tenné, hogy azokat a meglévő kódot a lehető annyi tooreuse kívánta. Mikkel Madsen állapota Umbraco fejlesztőjeként, "volt elégedett, és a hello Microsoft fejlesztői eszközök, amelyeket már ismeri a, például a Microsoft SQL Server, Microsoft Azure SQL-adatbázis, az ASP.net és az Internet Information Services (IIS). Az infrastruktúra-szolgáltatási vagy egy PaaS erőforrásokba történő beruházások előtt a felhő megoldás, arról, hogy az akkor támogatja a Microsoft-eszközöket és a platformok, hogy így toomake bekövetkezett nagymértékű változások tooour kódbázis toomake meg akartunk. "

minden toomeet a feltételnek, Umbraco keresést végrehajtani a következő képesítéssel hello felhő partnert:

* Elegendő kapacitása és a megbízhatóság
* A Microsoft fejlesztői eszközök, így a mérnökök nem lenne Umbraco kényszerített toocompletely támogatása reinvent saját fejlesztői környezetükben
* Az összes hello földrajzi piacok, amelyben UaaS sávszélességen (vállalatok számára szükséges tooensure, hogy azok is hozzáférjenek az adataikhoz gyorsan és, hogy az adatok egy helyre, amely megfelel a regionális szabályozási követelmények tárolja) meglétének

## <a name="why-umbraco-chose-azure-for-uaas"></a>Miért Umbraco Azure számára választott UaaS
Szerint tooMorten Christensen "után annak eldöntéséhez, hogy a beállítások, jelenleg kijelölt Azure azt minden el a feltételeket, kezelhetőségi és a méretezhetőség toofamiliarity és a költséghatékonyság telepítve. Beállítjuk hello környezetek Azure virtuális gépeken, és minden környezet rendelkezik a saját Azure SQL Database-példányt, a rugalmas készletek összes hello osztályt. Adatbázisok közötti fejlesztési, átmeneti és élő környezetben elválasztva is felhasználóknak kínált nagy teljesítmény elkülönítési egyező tooscale – hatalmas win. "

Péter sem szűnik meg, "előtt manuálisan le kellett tooprovision kiszolgálók webes adatbázisokhoz. Most jelenleg nem áll toothink információt. Minden automatizált – a toocleanup kiosztásakor. "

Péter sem az Azure által biztosított képességek skálázás hello is problémamentesen működjön. "a rugalmas készletek az SaaS ajánlott, mert azt felfelé és lefelé igény szerint tárcsázza kapacitás tökéletes. Kiépítés egyszerű, és a telepítés részeként, azt is megtarthatják az kihasználtsági legfeljebb." Péter sem állapotok, a "hello egyszerű rugalmas készletek hello biztosítani kell a szolgáltatási réteg-alapú dtu-k, valamint a biztosítanak számunkra hello power tooprovision új erőforráskészletek igény szerint. Nemrég egy nagyobb ügyfeleink csúcsos too100 dtu-i, az élő környezetben. Azure használatával, a rugalmas készletek megadott hello felhasználói adatbázisok esetén valós időben szükség rájuk toopredict DTU követelmények nélkül hello erőforrásokat. Egyszerűen csak put, a felhasználók beolvasása hello válaszidő arra számítanak, és azt is megfelelnek a teljesítmény szolgáltatásiszint-szerződések."

Mikkel Madsen összegzi azt: "azt korábban elfogadott hello hatékony Azure algoritmus, amely egy közös SaaS forgatókönyv (Bevezetés az új ügyfelek léptékű valós idejű) tooour alkalmazásminta (adatbázisok, mindkét fejlesztési előzetes kiépítése és működés közbeni) hello fölött (az Azure Service Bus-üzenetsorok használatával együtt az Azure SQL Database) alapul szolgáló technológiát."

## <a name="with-azure-uaas-is-exceeding-customer-expectations"></a>Az Azure-UaaS meghaladja ügyfél verziójával kapcsolatos elvárások
Hálózatiadapter-Azure-bA a felhő partnerrel, Umbraco óta képes tooprovide UaaS ügyfelek optimalizált-Tartalomkezelés teljesítménnyel, egy önálló üzemeltetett megoldás a szükséges hello IT-erőforrás beruházás nélkül. Péter sem szerint, mert "azt kedvelt hello fejlesztők kényelme és méretezhetőséget, hogy Azure biztosítanak számunkra, és az ügyfelek thrilled hello szolgáltatások és a megbízhatóság. A teljes lett egy nagyszerű win nekünk!"

## <a name="more-information"></a>További információ
* toolearn Azure rugalmas készletek, kapcsolatos további információkért lásd: [rugalmas készletek](sql-database-elastic-pool.md).
* További információ az Azure Service Bus toolearn lásd [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* További információ a webes és feldolgozói szerepkörök toolearn lásd [feldolgozói szerepkörök](../fundamentals-introduction-to-azure.md#compute).    
* További információ a virtuális hálózat toolearn lásd [virtuális hálózat](https://azure.microsoft.com/documentation/services/virtual-network/).    
* További információ a biztonsági mentés és helyreállítás, toolearn lásd [az üzletmenet folytonossága](sql-database-business-continuity.md).    
* toolearn ppols, figyelés bővebben lásd: [figyelése erőforráskészleteket](sql-database-elastic-pool-manage-portal.md).    
* toolearn szolgáltatásként, Umbraco kapcsolatos további információkért lásd: [Umbraco](https://umbraco.com/cloud).

