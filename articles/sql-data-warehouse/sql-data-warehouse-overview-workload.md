---
title: "Azure SQL Data Warehouse-műveletekkel kapcsolatos aaaLearn |} Microsoft Docs"
description: "Az SQL Data Warehouse rugalmassága lehetővé teszi, hogy növelje, csökkentse vagy szüneteltesse a számítási teljesítményt az adattárházegységek (DWU-k) csúszkájával. Ez a cikk ismerteti a hello adatraktárak mérőszámait és azok tooDWUs. "
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: cadffa9c-589d-4db7-888a-1f202a753bc5
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 8be5ff6b14ab907e2b0a7eb55e0e2f4139aca8b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-warehouse-workload"></a>Adatraktár-számítási feladat
Egy adatraktár-számítási feladat, amely egy adatraktár transpire hello műveletek tooall hivatkozik. hello adatraktár-számítási feladat magában foglalja az adatok betöltését hello adatraktár, elemzése és hello adatraktárban a jelentéskészítés, hello data-adatraktár kezelése és adatainak exportálása hello adatraktár hello teljes folyamata. hello és ezeket az összetevőket mélysége gyakran arányos az adatraktár hello hello érettségi szintjével.

## <a name="new-toodata-warehousing"></a>Új toodata warehousing?
Adatraktár egy betöltött adatok gyűjteménye, vagy további adatok adatforrásokat és használt tooperform például jelentéskészítésre és adatelemzésre üzletiintelligencia-feladatok.

Az adatraktárak vannak lekérdezések jellemzik, amelyek nagyobb mennyiségű sort és nagy adattartományokat vizsgálja, és előfordulhat, hogy eredményeket viszonylag nagy hello célokra, elemzéshez és jelentéskészítéshez. Az adatraktárakra emellett a viszonylag nagy mennyiségű adatbetöltés és a kis mennyiségű tranzakciószintű beszúrás/frissítés/törlés a jellemző.

* Adatraktár leghatékonyabb, amikor hello adatai úgy, hogy optimalizálja a tooscan nagy mennyiségű sort vagy nagyobb adattartományokat lekérdezéseket. Ez a vizsgálati típus működik optimálisan, ha hello adatok tárolásának és a keresés oszlop, ahelyett, hogy sort.

> [!NOTE]
> hello memórián belüli oszlopcentrikus indexet, oszlopos tárolást alkalmazó keresztül biztosítja a too10x nagyobb szintű tömörítésre képes és 100 x lekérdezési teljesítménynövekedést érhet el hagyományos bináris fák jelentéskészítési és elemzési lekérdezések. Azt tekinti oszlopcentrikus indexek hello szabványos tárolásánál és vizsgálatánál nagy adatokat az adatraktárban.
> 
> 

* Az adatraktáraknak követelményei különböznek az online tranzakció-feldolgozásra (OLTP) optimalizált rendszerekétől. hello OLTP rendszerek számos beszúrási, frissítési és törlési műveletek. Ezek a műveletek hello tábla sorainak toospecific keresni. Tábla átvizsgálása akkor a legeredményesebb, ha hello adatokat soronként-módon tárolják. hello adatok rendezését és gyors keresést oszd és uralkodj megközelítés egy bináris fának, avagy fa keresési hívása.

## <a name="data-loading"></a>Az adatok betöltése
Az adatok betöltése hello adatraktározás számítási feladatáról nagy része. A vállalkozások általában rendelkeznek egy forgalmas OLTP rendszerrel, amely nyomon követi a módosításokat, ha ügyfelektől üzleti tranzakciók hello nap folyamán. Rendszeres időközönként gyakran éjszaka karbantartási időszak alatt, hello tranzakciók kerülnek, vagy toohello adatraktár másolt. Ha hello adatok hello adatraktárban, elemzők elvégezhetik az elemzést, és üzleti döntéseket hello adatokon.

* Betöltés hello folyamat hagyományosan ETL a kinyerési, átalakítási és betöltési nevezik. Adatok általában kell alakítani, hogy konzisztens hello adatraktár többi adatával toobe. Korábban a vállalkozások dedikált ETL kiszolgálók tooperform hello átalakítások használt. Most az ilyen nagymértékben párhuzamos feldolgozási adatok betöltése az SQL Data Warehouse először, és beállíthatja, majd a hello átalakításokat hajthattak végre. Ez a folyamat elnevezése kinyerési, betöltés és átalakítás (ELT), és egyre inkább ez válik új szabvánnyá az adatraktározás számítási feladatáról hello.

> [!NOTE]
> Az SQL Server 2016 használatával már valós időben is elemezheti az OLTP-táblákat. Ez nem helyettesíti a data warehouse toostore a hello és az adatok elemzése, de egy módon tooperform elemzés, valós idejű biztosít.
> 
> 

### <a name="reporting-and-analysis-queries"></a>Jelentés- és elemzéslekérdezések
A jelentés- és elemzési lekérdezéseket a kicsi, közepes és nagy kategóriákba sorolják, esetenként a feltételek száma, de legtöbbször az időtartam alapján. A legtöbb adatraktárban vegyes számítási feladatok futnak, amelyet gyorsan és lassan futó lekérdezések alkotnak. Minden esetben fontos toodetermine a vegyes és toodetermine annak gyakoriságát (óránként, naponta, hónap végén, negyedév végén, és így tovább). Fontos, hogy a vegyes lekérdezésekből munkaterhelés, egyidejű, az érdeklődési tooproper kapacitástervezési az adatok alapján kialakulhat hello toounderstand.

* Kapacitástervezés a vegyes lekérdezésekből munkaterhelés összetett feladat lehet, különösen akkor, ha egy hosszú átfutási idő tooadd kapacitás toohello adatraktár van szüksége. SQL Data Warehouse szolgáltatásban a kapacitástervezés hello sürgősség, mert akkor növelhető vagy csökkenthető a számítási kapacitás, tetszőleges időpontban, és a tárolási és számítási kapacitás vannak egymástól függetlenül lehet méretezni.

### <a name="data-management"></a>Adatkezelés
Adatkezelés fontos, különösen ha tudja nincs elég szabad lemezterület a jövőben közelében hello futtassa. Az adatraktárak általában értelmezhető tartományokra, egy tábla partíciókként tárolt megoszthatjuk az hello adatokat. Az összes SQL Server szolgáltatáson alapuló termékek lehetővé teszik, hogy helyezze át a partíciók mindkét hello tábla. Ez a partícióváltás lehetővé teszi, akkor helyezheti át, régebbi adatok tooless költséges, és láthatóan tartja hello legfrissebb adatok legyenek elérhetők online tárhelyen.

* Az oszlopcentrikus indexek támogatják a particionált táblákat. Az oszlopcentrikus indexek esetében a particionált táblákat adatkezelésre és archiválásra használják. A soronként tárolt tábláknál a partícióknak a lekérdezési teljesítményben van nagyobb szerepük.  
* A PolyBase fontos szerepet játszik az adatkezelésben. A PolyBase használatával, rendelkezik hello beállítás tooarchive régebbi adatok tooHadoop vagy az Azure blob Storage tárolóban.  Ez sok lehetőséget biztosít, mivel a hello adatok továbbra is működik.  Hosszabb tooretrieve adatokat Hadoop vehet igénybe, de hello kompromisszummal jár, lekérési időt, előfordulhat, hogy járó hello tárolási költségű.

### <a name="exporting-data"></a>Az adatok exportálása
Egyirányú toomake érhető el adat a jelentések és elemzések hello data warehouse tooservers a jelentések és elemzések futtatására dedikált toosend adatokat jelzi. Ezeket a kiszolgálókat hívják adatpiacnak. Például képes előre feldolgozzák a jelentésadatokat, és exportálása hello az adatraktár toomany kiszolgálók hello világon toomake az ügyfelek és az elemzők széles körben elérhető.

* A jelentések létrehozásához minden éjszaka fel lehet tölteni egy pillanatképet hello napi adatokról írásvédett jelentéskészítő kiszolgálókra. Ez nagyobb sávszélességet biztosít az ügyfelek miközben csökkenti hello hello adatraktár számítási erőforrásigényét. Biztonsági szempontjából adatpiacok lehetővé teszik tooreduce hello száma, akik rendelkeznek hozzáféréssel toohello data warehouse-bA.
* Az elemzés vagy létrehozhatja egy elemzési adatkockát hello adatraktár és hello adatraktár, elemzés futtatásához vagy előzetesen feldolgozni az adatokat és exportálni kell toohello elemzési kiszolgálóra további elemzés.

## <a name="next-steps"></a>Következő lépések
Most, hogy jobban megismerte az SQL Data Warehouse, megtudhatja, hogyan tooquickly [SQL Data Warehouse létrehozása] [ create a SQL Data Warehouse] és [mintaadatokat tölthet be][load sample data].

<!--Image references-->

<!--Article references-->
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md

<!--MSDN references-->

<!--Other web references-->
