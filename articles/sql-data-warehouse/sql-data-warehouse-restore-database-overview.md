---
title: "az Azure adatraktár - a helyi és georedundáns aaaRestore |} Microsoft Docs"
description: "Áttekintés a hello adatbázis visszaállítási lehetőségek az Azure SQL Data Warehouse adatbázis helyreállítása."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: 3e01c65c-6708-4fd7-82f5-4e1b5f61d304
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: a96b898372b29d420e1416ca93a172ff8af47fc7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-restore"></a>Az SQL Data Warehouse visszaállítása
> [!div class="op_single_selector"]
> * [– Áttekintés][Overview]
> * [Portál][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

Az SQL Data Warehouse a helyi és földrajzi visszaállítások részeként a data warehouse vész helyreállítási lehetőségeket nyújt. Használja a data warehouse biztonsági mentések toorestore a data warehouse tooa visszaállítási pont hello elsődleges régióban, vagy georedundáns biztonsági mentések toorestore tooa különböző földrajzi régiót kell használnia. Ez a cikk ismerteti a hello mintaadatokról egy adatraktár a visszaállításakor.

## <a name="what-is-a-data-warehouse-restore"></a>Mi az a data warehouse visszaállítás?
A data warehouse visszaállítás egy új adatraktárat egy meglévő biztonsági másolatából létrehozott vagy törölt adatraktár. hello visszaállított adatraktár újra létrehozza a biztonsági másolat adatraktár hello adott időpontban. Mivel az SQL Data Warehouse egy elosztott rendszert, a data warehouse visszaállítás számos biztonsági mentés fájljaiból, az Azure-blobokat tárolt jön létre. 

Adatbázis visszaállítása nem minden üzleti folytonossági és vészhelyreállítási helyreállítási stratégia nagyon fontos részét képezik, mert az adatok véletlen sérülés vagy törlése után újból létrehozza.

További információkért lásd:

* [Az SQL Data Warehouse biztonsági mentések](sql-data-warehouse-backups.md)
* [Üzleti folytonosság – áttekintés](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Data warehouse visszaállítási pontok
Prémium szintű Azure Storage használatának előnye, mint az SQL Data Warehouse Azure Storage-Blobba pillanatképek toobackup hello elsődleges adatraktár használja. Minden egyes pillanatkép van egy olyan visszaállítási hello időt jelölő hello pillanatkép elindult. adatraktár toorestore, visszaállítási pont választja, és adja ki a restore parancsot.  

Az SQL Data Warehouse mindig hello biztonsági mentési tooa új adatraktárat állítja vissza. Akkor tartsa hello visszaállított adatraktár, és az aktuális hello, vagy törölje az egyik legyen. Ha szeretné tooreplace hello aktuális data warehouse hello vissza data warehouse-ba, hogy át lehessen nevezni.

Ha egy törölt vagy szüneteltetett adatraktár toorestore van szüksége, akkor [támogatási jegy létrehozása](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore hello last available restore point.

Yes, for hello next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps hello data warehouse and its snapshots for seven days just in case you need hello data. After seven days, you won't be able toorestore tooany of hello restore points. -->

## <a name="geo-redundant-restore"></a>Georedundáns helyreállítás
Az adatraktár tooany adatterület támogató Azure SQL Data Warehouse a kiválasztott teljesítmény szinten állíthatja vissza. Vegye figyelembe, hogy 9000 és 18000 DWU nem támogatottak minden régióban hello előzetes.

> [!NOTE]
> a georedundáns tooperform visszaállítási kell nem visszavonta igényét ezt a szolgáltatást.
> 
> 

## <a name="restore-timeline"></a>Az idősor visszaállítása
Visszaállíthatja egy adatbázis tooany elérhető visszaállítási pont belül hello elmúlt hét napban. A pillanatképek tooeight négy óránként start és hét napja. Ha pillanatkép régebbi, mint hét nap, jár le, és a helyreállítási pont már nem érhető el.

## <a name="restore-costs"></a>Állítsa vissza a költségek
hello tárolási kell fizetni hello visszaállított adatraktár hello prémium szintű Azure Storage díj számlázása történik. 

Ha egy visszaállított adatraktár felfüggesztéséhez van szó, a tárolási hello prémium szintű Azure Storage díj. hello felfüggesztéséhez előnye, nem kell fizetnie hello DWU számítási erőforrások.

További információ az SQL Data Warehouse díjszabása: [SQL Data Warehouse díjszabása](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>A visszaállítási használ
hello elsődleges funkciója a data warehouse visszaállítási toorecover adatok véletlen adatvesztés vagy adatsérülés utánra esik.

Használhatja a data warehouse visszaállítási tooretain biztonsági több mint hét nap. Miután visszaállította hello biztonsági mentés, hello adatraktár online rendelkezik, és akár szüneteltetheti is, hogy határozatlan ideig toosave számítási költségek. hello szüneteltetett adatbázis terhel tárolási hello prémium szintű Azure Storage díj. 

## <a name="related-topics"></a>Kapcsolódó témakörök
### <a name="scenarios"></a>Forgatókönyvek
* Üzleti folytonosság áttekintéséért lásd: [üzleti folytonosság – áttekintés](../sql-database/sql-database-business-continuity.md)

<!-- ### Tasks -->

adatraktár tooperform helyreállítása, visszaállítás segítségével:

* Az Azure portál, lásd: [hello Azure-portál használatával adatraktár visszaállítása](sql-data-warehouse-restore-database-portal.md)
* PowerShell-parancsmagok [visszaállítani az PowerShell-parancsmagok használatával adatraktár](sql-data-warehouse-restore-database-powershell.md)
* REST-API-k, lásd: [hello REST API-k használatával adatraktár visszaállítása](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
