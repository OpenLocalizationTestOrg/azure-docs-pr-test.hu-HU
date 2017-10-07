---
title: "aaaOnline biztonsági mentése és visszaállítása az Azure Cosmos DB |} Microsoft Docs"
description: "Megtudhatja, hogyan tooperform automatikus biztonsági mentése és visszaállítása egy Azure Cosmos DB adatbázisban."
keywords: "biztonsági mentés és visszaállítás, az online biztonsági mentés"
services: cosmos-db
documentationcenter: 
author: RahulPrasad16
manager: jhubbard
editor: monicar
ms.assetid: 98eade4a-7ef4-4667-b167-6603ecd80b79
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 08/11/2017
ms.author: raprasa
ms.openlocfilehash: a0b464c95681dfc7b5462b02bf04c2c43d6bc16f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>Automatikus online biztonsági mentés és helyreállítás Azure Cosmos DB
Azure Cosmos-adatbázis rendszeres időközönként automatikusan az adatok biztonsági másolatainak vesz igénybe. hello automatikus biztonsági mentésekhez készít a nem befolyásolja a hello teljesítmény vagy a rendelkezésre álló a Helyadatbázis-műveletekhez. A biztonsági mentések külön-külön tárolják egy másik tárhelyre, és azokat a biztonsági mentések globálisan replikálva vannak a rugalmasságot regionális vészhelyzetek ellen. Ha véletlenül törli a a Comos DB tárolót, és később a adat-helyreállítás vagy egy vész-helyreállítási megoldást igényelnek szánt forgatókönyvek hello automatikus biztonsági mentésekhez.  

Ez a cikk egy rövid összefoglaló hello adatok redundancia és rendelkezésre állás érdekében Cosmos DB kezdődik, és majd a biztonsági mentések ismerteti. 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Magas rendelkezésre állás az Cosmos DB - egy összefoglalása
Cosmos DB tervezett toobe [globálisan elosztott](distribute-data-globally.md) – lehetővé teszi tooscale átviteli együtt feladatátvevő és transzparens többhelyű API-alapú házirend több Azure-régiók között. Egy adatbázis rendszer ajánlat mint [rendelkezésre állás 99,99 % SLA-k](https://azure.microsoft.com/support/legal/sla/cosmos-db), minden hello írás az Adatbázisba az Cosmos véglegesítésére toolocal a lemezek helyi adatközpontban lévő replikák másodlagosak toohello ügyfél igazolása előtt. Ne feledje, hogy hello Cosmos-adatbázis magas rendelkezésre állású támaszkodik a helyi tároló nem függ semmilyen külső tárolási technológiákat. Továbbá ha az adatbázis-fiók egynél több Azure-régió tartozik, az írási műveletek replikálódnak más régiókból is. tooscale alacsony késleltetésű, az átviteli sebesség és a hozzáférési adatok is különböző régiókban-adatbázis fiókjához tartozó tetszés szerinti olvasás. Minden olvasási régióban (replikált) hello adatok tartósan állandó között a replikakészlethez.  

A következő diagram hello módon van-e egyetlen Cosmos DB tárolót [vízszintesen particionált](partition-data.md). A következő diagram hello kör megjelölt "Partíció", és mindegyik partíció keresztül egy magas rendelkezésre állású legyen. Ez a helyi terjesztési hello (hello X tengely jelölik) egyetlen Azure régión belül. Továbbá minden egyes partícióra (a megfelelő replikakészlethez) van majd globálisan elosztott-(például régiókban az ábra hello három – USA keleti régiója, USA nyugati régiója és közép-Indiában) adatbázis fiókjához tartozó különféle régiókban. hello "partíció set" globálisan elosztott az entitás minden régióban (hello Y tengely jelölik) az adatok több példányt tartalmazó. Rendelhet prioritást-adatbázis fiókjához tartozó toohello régiók és Cosmos DB fog transzparens feladatátvétel toohello következő terület katasztrófa esetén. Feladatátvételi tootest hello végpont alkalmazás rendelkezésre állásának a szimulálhatja manuálisan is.  

hello alábbi kép ábrázolja a Cosmos DB redundancia magas fokú hello.

![A Cosmos DB redundancia magas fokú](./media/online-backup-and-restore/redundancy.png)

![A Cosmos DB redundancia magas fokú](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>Teljes, automatikus, online biztonsági mentések
Sajnos a törölt a tároló, vagy az adatbázis! A Cosmos DB nem csak az adatokat, de hello biztonsági másolatokat is magas redundáns és rugalmas tooregional vészhelyzetek esetére. Automatikus biztonsági mentéseket jelenleg körülbelül négy óránként készít, és a legújabb 2 biztonsági másolatai mindig. Ha véletlenül hello adatok eldobása, vagy sérült, adjon [kérje az Azure támogatási](https://azure.microsoft.com/support/options/) 8 órán belül. 

hello biztonsági mentést készít a nem befolyásolja a hello teljesítmény vagy a rendelkezésre álló a Helyadatbázis-műveletekhez. A kiépített RUs fel vagy hello teljesítményét és nélkül hello az adatbázis elérhetőségét érintő cosmos DB hello biztonsági mentés hello háttérben vesz igénybe. 

A Cosmos DB belül tárolt adatokat, eltérően hello automatikus biztonsági másolatai Azure Blob Storage szolgáltatás. tooguarantee hello alacsony késleltetésű/hatékony, a biztonsági mentési pillanatképet hello feltöltése feltöltött tooan példányát az Azure Blob storage hello hello aktuális írási terület Cosmos-adatbázis adatbázis-fiókja és ugyanabban a régióban. A regionális katasztrófa szembeni hibatűrést minden pillanatképet a biztonsági mentési adatok az Azure Blob Storage újra replikálja a rendszer georedundáns tárolás (GRS) tooanother régió keresztül. hello következő ábrán az látható, hogy hello teljes Cosmos DB tárolóhoz (az összes három elsődleges partíció USA nyugati régiója, az ebben a példában) készül biztonsági másolat egy távoli Azure Blob Storage-fiókban az USA nyugati régiója és Georedundáns replikálja tooEast VELÜNK. 

hello alábbi kép ábrázolja Georedundáns Azure Storage összes Cosmos DB entitásának rendszeres teljes biztonsági mentést.

![Az összes Cosmos DB entitások Georedundáns Azure Storage rendszeres teljes biztonsági mentés](./media/online-backup-and-restore/automatic-backup.png)

## <a name="backup-retention-period"></a>Biztonsági mentés megőrzési időtartam
A fent leírtaknak megfelelően Azure Cosmos DB időt vesz igénybe az adatok pillanatfelvételének négy óránként, és 30 napig őrzi meg hello utolsó két pillanatképek minden partíció. A megfelelőségi szabályzat / pillanatképek kiürítésekor 90 nap után.

Ha toomaintain saját pillanatképeket, használhatja a hello Azure Cosmos DB hello exportálási tooJSON beállítás [adatáttelepítési eszközét](import-data.md#export-to-json-file) tooschedule további biztonsági másolatoknak. 

## <a name="restoring-a-database-from-an-online-backup"></a>Adatbázis visszaállítása az online biztonsági mentés
Ha véletlenül törli az adatbázis vagy a gyűjtemény, akkor [fájlt egy támogatási jegy](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) vagy [Azure az ügyfélszolgálat](https://azure.microsoft.com/support/options/) hello utolsó automatikus biztonsági mentés toorestore hello adatait. Ha szüksége toorestore az adatbázis adatsérülés következett be miatt, tekintse meg [adatsérülés kezelése](#handling-data-corruption) szükség szerint további tootake tooprevent hello sérült adatok lépések áthatoló hello biztonsági másolatból. A biztonsági másolat visszaállítása toobe adott pillanatképet Cosmos DB szükséges hello adatok volt elérhető a hello hello biztonsági mentési ciklust, hogy a pillanatkép idejére.

## <a name="handling-data-corruption"></a>Adatsérülés kezelése
Azure Cosmos DB hello minden partíció utolsó két biztonsági mentést hello rendszer megőrzi. Ez a modell jól működik, ha óta hello utolsó verziók egyike állítható vissza véletlenül törli a tároló (gyűjtemény dokumentumok, a graph, a táblázat), vagy az adatbázis. Azonban a hello eset, amikor a felhasználók vezethetnek adatsérülés következett be, amikor Azure Cosmos DB előfordulhat, hogy ne tudjanak a hello program sérült adatokat, és előfordulhat, hogy a sérülés hello előfordulhat, hogy rendelkezik követhessék hello biztonsági mentéseket. Amint sérülést észlel, törölnie kell sérült hello tároló (gyűjtemény vagy graph vagy tábla), hogy a biztonsági mentések védi a felülírás sérült adatokkal. Hello utolsó biztonsági mentés régi négy óra lehet, mert hello felhasználói is alkalmaz [adatcsatorna módosítása](change-feed.md) toocapture és a tároló hello utolsó négy óra érdemes adatok hello tároló törlése előtt.

## <a name="next-steps"></a>Következő lépések

tooreplicate az adatbázis több adatközpontokban, lásd: [terjesztése az adatok globálisan Cosmos DB](distribute-data-globally.md). 

toofile Forduljon Azure támogatási [hello Azure-portálon az igénylést](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

