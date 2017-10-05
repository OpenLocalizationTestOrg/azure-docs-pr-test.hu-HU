---
title: "Online biztonsági mentés és helyreállítás Azure Cosmos DB |} Microsoft Docs"
description: "Ismerje meg az automatikus biztonsági mentés végrehajtásához, és egy Azure Cosmos DB adatbázis visszaállítása."
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
ms.openlocfilehash: 130f0eb259621737d6dbdb151e363915fb334ce1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="automatic-online-backup-and-restore-with-azure-cosmos-db"></a>Automatikus online biztonsági mentés és helyreállítás Azure Cosmos DB
Azure Cosmos-adatbázis rendszeres időközönként automatikusan az adatok biztonsági másolatainak vesz igénybe. Automatikus biztonsági mentés készül anélkül, hogy befolyásolná a teljesítmény vagy a rendelkezésre álló a Helyadatbázis-műveletekhez. A biztonsági mentések külön-külön tárolják egy másik tárhelyre, és azokat a biztonsági mentések globálisan replikálva vannak a rugalmasságot regionális vészhelyzetek ellen. Ha véletlenül törli a a Comos DB tárolót, és később a adat-helyreállítás vagy egy vész-helyreállítási megoldást igényelnek szánt forgatókönyvek az automatikus biztonsági mentésekhez.  

Ez a cikk egy rövid összefoglalása a adatok redundancia és rendelkezésre állás érdekében Cosmos DB kezdődik, és majd ismerteti a biztonsági másolatok. 

## <a name="high-availability-with-cosmos-db---a-recap"></a>Magas rendelkezésre állás az Cosmos DB - egy összefoglalása
Cosmos DB kialakításánál fogva [globálisan elosztott](distribute-data-globally.md) – lehetővé teszi a méretezhető átviteli együtt feladatátvevő és transzparens többhelyű API-alapú házirend több Azure-régiók között. Egy adatbázis rendszer ajánlat mint [rendelkezésre állás 99,99 % SLA-k](https://azure.microsoft.com/support/legal/sla/cosmos-db), Cosmos DB az írási műveletek tartósan elkötelezettek vagyunk a helyi lemezek helyi adatközpontban lévő replikák másodlagosak által az ügyfélnek igazolása előtt. Vegye figyelembe, hogy a magas rendelkezésre állású Cosmos-adatbázis helyi tároló alapul, és nem függ semmilyen külső tárolási technológiákat. Továbbá ha az adatbázis-fiók egynél több Azure-régió tartozik, az írási műveletek replikálódnak más régiókból is. Az átviteli sebesség és a hozzáférési adatok méretezhető, alacsony késleltetésű, akkor is a különböző régiókban-adatbázis fiókjához tartozó tetszés szerinti olvasás. Minden olvasási régióban a (replikált) adatok tartósan állandó között a replikakészlethez.  

Az alábbi ábrán szemléltetett, van-e egyetlen Cosmos DB tárolót [vízszintesen particionált](partition-data.md). Az alábbi ábra egy kör megjelölt "Partíció", és mindegyik partíció keresztül egy magas rendelkezésre állású legyen. Ez az a helyi terjesztési (az X tengely jelölik) egyetlen Azure régión belül. Továbbá minden egyes partícióra (a megfelelő replikakészlethez) van majd globálisan elosztott-(például régiókban az ábra a három – USA keleti régiója, USA nyugati régiója és közép-Indiában) adatbázis fiókjához tartozó különféle régiókban. A "partíció set" van globálisan elosztott entitás, minden régióban (Y tengely jelölik) az adatok többszörös lemásolását, amely. A régiók-adatbázis fiókjához tartozó prioritás rendelhető és Cosmos DB transzparens módon fog megfelelni a feladatátvétel katasztrófa esetén a következő terület. Manuálisan is szimulálhatja feladatátvételi tesztelése az alkalmazás-végpontok közötti rendelkezésre állását.  

Az alábbi ábrán a Cosmos DB redundanciát a magas fokú.

![A Cosmos DB redundancia magas fokú](./media/online-backup-and-restore/redundancy.png)

![A Cosmos DB redundancia magas fokú](./media/online-backup-and-restore/global-distribution.png)

## <a name="full-automatic-online-backups"></a>Teljes, automatikus, online biztonsági mentések
Sajnos a törölt a tároló, vagy az adatbázis! A Cosmos DB nem csak az adatokat, de az adatok biztonsági mentését is magas redundáns és rugalmas regionális katasztrófák. Automatikus biztonsági mentéseket jelenleg körülbelül négy óránként készít, és a legújabb 2 biztonsági másolatai mindig. Ha az adatok akár véletlenül eldobása, vagy sérült, adjon [kérje az Azure támogatási](https://azure.microsoft.com/support/options/) 8 órán belül. 

A biztonsági másolatokat készít a nem befolyásolja a teljesítmény vagy a rendelkezésre álló a Helyadatbázis-műveletekhez. A kiépített RUs fel, vagy a teljesítményét és az adatbázis elérhetőségét érintő nélkül cosmos adatbázis a biztonsági mentés a háttérben vesz igénybe. 

Ellentétben a Cosmos DB belül tárolt adatokat az automatikus biztonsági másolatai Azure Blob Storage szolgáltatásban. A kis késleltetésű/hatékony feltöltés biztosításához a biztonsági mentési pillanatképet tölt fel az Azure Blob Storage tárolóban ugyanabban a régióban, mint a jelenlegi írási terület a Cosmos DB adatbázis fiók egy példányát. A regionális katasztrófa szembeni hibatűrést minden pillanatképet a biztonsági mentési adatok az Azure Blob Storage újra replikálódik keresztül georedundáns tárolás (GRS) egy másik régióban. Az alábbi ábrán látható, hogy a teljes Cosmos DB tárolóhoz (az összes három elsődleges partíció USA nyugati régiója, az ebben a példában) készül biztonsági másolat egy távoli Azure Blob Storage-fiókban az USA nyugati régiója, és ezután Georedundáns USA keleti régiója replikálva. 

Az alábbi ábrán a Georedundáns Azure Storage összes Cosmos DB entitásának rendszeres teljes biztonsági mentést.

![Az összes Cosmos DB entitások Georedundáns Azure Storage rendszeres teljes biztonsági mentés](./media/online-backup-and-restore/automatic-backup.png)

## <a name="backup-retention-period"></a>Biztonsági mentés megőrzési időtartam
A fent leírtaknak megfelelően Azure Cosmos DB időt vesz igénybe az adatok pillanatfelvételének négy óránként, és 30 napig őrzi meg minden partíció utolsó két pillanatképeket. A megfelelőségi szabályzat / pillanatképek kiürítésekor 90 nap után.

Ha meg szeretné tartani a saját pillanatképeket, használhatja az Exportálás JSON beállítást az Azure Cosmos DB [adatáttelepítési eszközét](import-data.md#export-to-json-file) további biztonsági mentés ütemezése. 

## <a name="restoring-a-database-from-an-online-backup"></a>Adatbázis visszaállítása az online biztonsági mentés
Ha véletlenül törli az adatbázis vagy a gyűjtemény, akkor [fájlt egy támogatási jegy](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) vagy [Azure az ügyfélszolgálat](https://azure.microsoft.com/support/options/) az adatok biztonsági másolatból történő visszaállítását a legutóbbi automatikus. Ha szeretné visszaállítani az adatbázist, mert adatsérülés következett be, lásd: [adatsérülés kezelése](#handling-data-corruption) megakadályozhatja, hogy a sérült adatok a biztonsági mentés túljutó további lépések végrehajtására van lehetősége. A visszaállítandó biztonsági másolat egy adott pillanatképet Cosmos DB megköveteli, hogy az adatok volt elérhető a biztonsági mentési ciklus, hogy a pillanatkép idejére.

## <a name="handling-data-corruption"></a>Adatsérülés kezelése
Azure Cosmos-adatbázis minden partíció utolsó két biztonsági mentését a rendszer megőrzi. Ez a modell jól működik, ha óta az utolsó verziókra visszaállítható véletlenül törli a tároló (gyűjtemény dokumentumok, a graph, a táblázat), vagy az adatbázis. Azonban abban az esetben, amikor a felhasználók vezethetnek adatsérülés következett be, amikor Azure Cosmos DB előfordulhat, hogy ne tudjanak a program sérült adatokat a, és lehetséges, hogy a sérülés előfordulhat, hogy rendelkezik követhessék a biztonsági mentéseket. Amint sérülést észlel, törölnie kell a sérült tároló (gyűjtemény vagy graph vagy tábla), hogy a biztonsági mentések védi a felülírás sérült adatokkal. Az utolsó biztonsági mentés régi négy óra lehet, mert a felhasználó is alkalmaz [adatcsatorna módosítása](change-feed.md) rögzítése és tárolása az utolsó négy óra érdemes az adatok a tároló törlése előtt.

## <a name="next-steps"></a>Következő lépések

Az adatbázis több adatközpontokban replikációt, tekintse meg [terjesztése az adatok globálisan Cosmos DB](distribute-data-globally.md). 

A fájl forduljon Azure támogatási szolgálatához [Azure-portálról igénylést](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

