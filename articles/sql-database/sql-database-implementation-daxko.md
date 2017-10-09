---
title: "aaaAzure SQL adatbázis Azure esettanulmány - Daxko/CSI |} Microsoft Docs"
description: "Tudnivalók arról, hogyan Daxko/CSI használja-e az SQL-adatbázis tooaccelerate a fejlesztési ciklus és tooenhance az ügyfélszolgálat és teljesítmény"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 00c8a713-f20c-4d6b-b8b7-0c1b9ba5f05b
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 3e3d58a1d9c3c919fc0e4cdb2765f680719c19d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="daxkocsi-used-azure-tooaccelerate-its-development-cycle-and-tooenhance-its-customer-services-and-performance"></a>A fejlesztési ciklus és tooenhance Daxko/CSI használja az Azure tooaccelerate az ügyfélszolgálat és teljesítmény
![Daxko/CSI embléma](./media/sql-database-implementation-daxko/csidaxkologo25.png)

Daxko/CSI szoftver egy ellenőrző tapasztalt: alkalmasságra és újbóli központok az ügyfél alapja gyorsan, növekszik az toohello sikerességének Köszönjük, hogy az átfogó vállalati-szoftveres megoldás, de az IT-infrastruktúra kell, hogy növekvő hello tartása vásárlói bázisunk lett tesztelés hello vállalati informatikai személyzetet tart fenn. hello vállalati egyre növekvő műveletek terhelést jelenthet, különösen a növekvő adatbázisok kezelése által lett korlátozott. Adott műveletek terhelés rosszabb, fejlesztői erőforrások a új kezdeményezések, például az hello vállalati szoftver új mobilitási szolgáltatásai be lett kivágja.

TooDavid Molina szerint, igazgató a fejlesztés, Daxko/CSI Azure megadott CSI szoftver modellel hello platform,--szolgáltatás (PaaS), hogy szükséges-e toosimplify adatbázis felügyeleti, méretezhetőség javítása és szabadítható fel az erőforrások toofocus szoftver ops helyett. "Az azure SQL Database volt az USA nagyszerű lehetőséget. Nem rendelkezik egy SQL Server feladatátvevő fürt és az összes hello karbantartása tooworry más infrastruktúrához volt ideális az USA."

Mivel az áttelepítést tooAzure, CSI szoftver kell egy műveleti személyzet csak két toomanage a 600 felhasználói adatbázis. hello vállalat az Azure SQL Database rugalmas készletek toomove felhasználói adatbázisokat mérete alapján, és kell használja.

Molina továbbra is fennáll, "ügyfeleink nemez hello azonnal módosítása. Rugalmas készletek előtt alkalmanként rendelkeztek időtúllépések és más olyan problémák kapacitásnövelés időszakokban. Az Azure rugalmas készletek hogy igény szerint kapacitásnövelés, és hibák nélkül hello szoftver."

Ezenkívül tooimproving teljesítmény az ügyfelek, Azure rugalmas készletek felszabadulnak CSI szoftver erőforrások toofocus az új szolgáltatásokat és lehetőségeket, műveletek és a felügyeleti kezelésével helyett. Ezen informatikai erőforrások segített CSI szoftver javítása az ajánlat, SpectrumNG, toohelp vállalati szoftverek Fitnessklub tagok megszólítása, növelje személyzet hatékonyságát, és adjon a munkatársak és a tagok mobil hozzáférés az interaktív feladatok és a valós idejű.

Azure is hozzájárult CSI szoftver egyre gyorsabban jelennek meg, és javíthatja a hello fejlesztési és minőségi ág (QA) ciklus automatizálási lehetőségek engedélyezésével. A vállalat hello Azure végrehajtására build kezelők is becsomagolhatja a összetevők hello kattintással gomb. Mivel Molina ismerteti, a "hello kiadási ciklus részeként QA már képes toodeploy tooa tesztkörnyezetben, amely a termelési verem szorosan utánozza az Azure-ban. Telepíthetünk buildek azonnal tooour fejlesztői környezet toovet változik. Ez egy nagy win nekünk, mert nem rendelkeztünk tesztelése előtt, a paritásos."

## <a name="offloading-toohello-cloud"></a>Szervez toohello felhő
Toohello felhő, mielőtt CSI szoftver kellett sikeresen létrehozott egy helyi adatközpontban Houston a saját több-bérlős infrastruktúra. Hello vállalati kibontva, növekvő növő fáradságot megvásárlásáról kiépítés és karbantartása hello hardvere tapasztalt, és szoftver szükséges toosupport ügyfeleinek. Informatikai személyzeti toohandle műveletek egy másik szűk tooa lassulást új erőforrások kiépítése és terítésével új szolgáltatások toocustomers vezető vált.

CSI szoftver vizsgálni felhő beállításait, hogy a terhelést, így kiküszöböli, hogy sikerült összpontosítson, a kód helyett a műveleteket. hello vállalati észlelhető, hogy felső hello szolgáltatók számos csak infrastruktúra,--szolgáltatás (IaaS)-megoldásokat nyújtsanak a nagy informatikai munkatársak toomanage hello IaaS verem továbbra is szükséges. A hello végén CSI megállapította, hogy volt-e a hello Azure PaaS megoldás hello ajánlott az igényeinek megfelelően. Molina ismerteti, "Azure lekérdezi hello hardver- és szoftver hello útból, a szoftver ajánlatokkal közben csökkenhet az informatikai részleg terhelését összpontosíthatunk."

## <a name="making-hello-transition-tooazure"></a>Hello áttűnés tooAzure elvégzése
A PaaS megoldás kiválasztása az Azure, után CSI szoftver megkezdte a háttérrendszer infrastruktúra és az adatbázisok toohello felhő áttelepítése. Előzetes tooAzure SpectrumNG ügyfelek tooinstall egy ügyfélalkalmazást, amely egy Windows Communication Foundation (WCF) szolgáltatással hello háttérben futó közölt szükséges. TooMolina, szerint "bár egyes ügyfelek üzemelteti a saját adatközpontját tartalmát, azt a beépített hello termék toobe több-bérlős ki. A Microsoft üzemeltetett mindent Houston, az adatközpontban hello adattár SQL Server használatával.

"A termék ajánlat is megtalálhatók tag felé néző webes portálon (írt ASP.net), tervezett toobe fehér címkével toomatch hello ügyfél webszolgáltatás, és a SOAP API toosupport hello online lapok és az bármely harmadik fél integrációs lépett."

hello áttelepítési toohello felhő nem vette hosszú hello architektúra. TooMolina szerint, "hello többsége hello erőfeszítéssel foglalkozik módosítja, hogy azt a konfigurációs fájl adatainak, központosított kapcsolat-karakterlánc módosítását, olvassa el és automatizálás hello csomagolása, feltöltése és a megjelent hello módját."

toodevelop hello hozza létre az automatizálási, CSI szoftverfejlesztő használt Azure PowerShell és a REST API-k toocreate csomagokat, és feltöltheti ezeket tooa átmeneti környezet kiadás éjszakánként vonatkozó beállítást.
hello általános átmenet tooan Azure felhő alapú központi telepítési hiba gyorsan és problémamentesen lezajlott hello CSI szoftver IT-részleg. Molina ismerteti, "az összes, azt kellett a béta-környezet hello felhőben hello projekt a három toofour héten belül. Amely nem egy meglepő win nekünk."

Konfigurálás és tesztelés hello környezet, miután CSI szoftver megkezdte áttelepítése ügyfelek. A már használja a CSI szoftvert futtató ügyfelek hello áttűnés majdnem zökkenőmentes volt. Az ügyfelek áttelepítését egy helyi központi telepítéséből, hello áttelepítési toohello felhő további időt vett igénybe, de továbbra is elsősorban problémás szabad ügyfelek és a CSI szoftver.

Új ügyfelek számára, CSI szoftver tartozó informatikai munkatársak használja hello feldolgozási tooon-tábla következő őket tooAzure:

1. Az Azure PowerShell-parancsprogramok használt toospin hello ügyfél; az új adatbázis összes ügyfél el a támogatási réteg tooensure hello áttűnés elég kezdeti adatátviteli sebességét.
2. Ha lehetséges, akkor CSI szoftver hello Azure SQL varázsló toomove meglévő adatok tooan Azure SQL Database-példányt használ.
3. Végezetül, a Microsoft SQL Server Integration Services (SSIS) használt tooreconcile hello vagy tooperform ellentmondások bármely adatok karbantartása, mint a szükséges.

Ma körülbelül 99 százalékához kiindulási CSI szoftver ügyfelek tárolt Azure-ban (északi központi, Dél központi, kelet és Nyugat) négy regionális üzemeltetésében. Azzal, hogy az adatközpontok egyes ügyfelek földrajzi régióban, késés tartják tooa minimális.

## <a name="azure-elastic-pools-free-up-it-resources"></a>Szabadítson fel informatikai erőforrásra lenne szükség Azure rugalmas készletek
Az Azure számos szolgáltatást segítettek CSI szoftver shift infrastruktúrájának és műveleteinek célzott toobeing szolgáltatás és a fejlesztői arra irányul, nem. Lehet, hogy a hello legnagyobb előnye a rugalmas készletek lett.

CSI szoftver készül 550 adatbázisok jelenleg biztosít az ügyfelek. Rugalmas készletek előtt volt nehéz toomanage belül ennyi adatbázisok réteg struktúrában. OPS kezelők kellett tooassign teljesítmény rétegek ügyféllel, amely jelentős IT-erőforrás terhet szükséges hello kapacitásnövelés igényeinek megfelelően. A rugalmas készletek kezelők rendelhet bérlők egy premium vagy a szabványos készlet megfelelő, és majd helyezze át az ügyfelek mérete alapján és kell. Az ügyfelek nemez hello rugalmas készletek hello hatásait szinte azonnal; rugalmas készletek előtt ügyfelek kellett időtúllépések és más olyan problémák kapacitásnövelés kihasználtságú időszakok alatt, de rugalmas készletek ügyfelek fedezheti tevékenység felszakadásáig igény szerint, és toouse SpectrumNG hibák nélkül továbbra.

## <a name="azure-active-geo-replication-accelerates-reporting"></a>Az Azure aktív georeplikáció felgyorsítják reporting
Több CSI szoftver ügyfelek Azure aktív georeplikáció előnyeit is tart. Az aktív georeplikáció, toofour be másodlagos adatbázisok olvasható konfigurálható hello ugyanazon vagy másik adatközpont-régiókban. CSI szoftverfrissítési kétféle módon aktív georeplikáció használ: először hello másodlagos adatbázisok találhatók hello eset datacenter kimaradás vagy hello meggátoló tooconnect toohello elsődleges adatbázis; és a második, hello másodlagos adatbázisok olvasható és csak olvasható munkaterhelésekkel, például jelentéskészítéssel feladatok lehetnek használt toooffload. Néhány CSI szoftver a munkafolyamatok reporting juttatás tooaccelerate használják.

## <a name="csi-software-application-logic-and-architecture"></a>CSI szoftver úgy az alkalmazáslogikát és architektúra
SpectrumNG webes szerepkörök használja. Mert hello alkalmazás több-bérlős, a WCF-szolgáltatások használt toohandle hello kezdeti kapcsolatkérelem-ügyfél. Molina állapotok, mint a "hello kérelem azonosítja az egyes ügyfelek, majd révén a us létrehozása egy kapcsolati karakterláncot tootheir adatbázisok toodo kimenő függetlenül meg kell toodo."

Hello webes réteg a szolgáltatás, a CSI szoftver kihasználja az Azure automatikus skálázás, napon és időpontban alapján. Állnak rendelkezésre a automatikusan megnövekedett tooaccommodate magasabb használati munkaidőben, minden egyes regionális adatközpontok toohello időzónájának megfelelően. Erőforrások vannak is be tooscale hétvégeken, amikor az ügyfelek igényeinek megfelelően alacsonyabbak.

![Daxko/CSI architektúrája](./media/sql-database-implementation-daxko/figure1.png)

1. ábra. A felhőalapú szolgáltatások feldolgozói szerepkör Azure SQL Database és félig strukturált adatokból a table storage a strukturált adatok megrajzolja. SpectrumNG felhasználók használják, hogy a felhő-adatszolgáltatások összetevőjének Webes szerepkör.

## <a name="using-web-apps-and-a-web-plan-tier-for-mobile-apps"></a>Webalkalmazások és webes-terv réteget használó mobilalkalmazásokhoz
Azure SQL Database CSI szoftver tooenable erőforrásait felszabadulnak használatával új kezdeményezéseket, beleértve a teljes mobilplatformot alapján egy az Azure web apps üzemeltetett egyéni API. hello platform lehetővé teszi a tagok Fitnessklub és személyzet toouse mobileszközök toocheck ütemezéseket, osztályok foglalható le, és üzeneteket fogadni.

hello platform használja szolgáltatásorientált architektúra (SOA) tootake adott összetevőt – például egy POS (POS) vagy egy értékesítési rendszer – hello behúzás tooanother webes terven helyezze, és majd lépett fel a szolgáltatás toosupport, hogy az összetevő úgy, hogy minden más, a hello eredeti webes terv. Ez a lehetőség CSI szoftver rengeteg rugalmasságot biztosít, és segít költséges alacsonyan tartása.

## <a name="azure-lets-csi-software-developers-focus-on-apps-and-services"></a>Azure lehetővé teszi, hogy a szoftver CSI fejlesztők arra utalnak, alkalmazások és szolgáltatások
Az Azure SQL-adatbázis nem csak egy boon tooSpectrumNG felhasználók, akiknek a hello gyors és megbízható szolgáltatás, egyúttal egy nagy win CSI szoftver informatikai szakemberek és fejlesztők számára. Ops tooAzure hello felhőben történő kiszervezésével a CSI szoftver csökken a terhelés, az erőforrások és infrastruktúra, nagy mértékben az elérését gyorsítja fel a fejlesztési ciklus, és már nincs szüksége a bérlők számára toomicromanage adatbázisok toooptimize teljesítmény.

## <a name="more-information"></a>További információ
* toolearn Azure rugalmas készletek, kapcsolatos további információkért lásd: [rugalmas készletek](sql-database-elastic-pool.md).
* További információ az adatbázis-eszközök és a rugalmas méretezést toolearn lásd [skálázáshoz rugalmas adatbáziseszközöket és a rugalmas méretezést](sql-database-elastic-scale-get-started.md).
* További információ az SQL Server-adatbázis migrálása az toolearn lásd: lásd: [áttelepítése egy SQL Server adatbázis tooAzure](sql-database-cloud-migrate.md).
* További információ az aktív georeplikáció, toolearn lásd [aktív georeplikáció](sql-database-geo-replication-overview.md).
* További információ a webes és feldolgozói szerepkörök toolearn lásd [feldolgozói szerepkörök](../fundamentals-introduction-to-azure.md#compute).    
* További információ az Azure Service Bus toolearn lásd [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).
* toolearn automatikus méretezése kapcsolatos további információkért lásd: [felhőszolgáltatások skálázás](../cloud-services/cloud-services-how-to-scale.md).

