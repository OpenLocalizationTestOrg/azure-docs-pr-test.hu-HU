---
title: "az Azure SQL-adatbázisok aaaManage csoportok |} Microsoft Docs"
description: "Végezze el a létrehozását és kezelését egy rugalmas feladat."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: f858344d-085b-4022-935e-1b5fa20adbac
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: a73c4fb24c332fae0e917c18272724cccd56f29a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-scaled-out-azure-sql-databases-using-elastic-jobs-preview"></a>Létrehozásához és kezeléséhez méretezett kimenő Azure SQL Database-adatbázisok rugalmas feladat (előzetes verzió)


**Rugalmas adatbázis-feladatok** egyszerűbbé teheti az adatbázisok csoportok kezelését a következő felügyeleti műveletek, például a sémamódosítások, a hitelesítő adatok kezelése, a hivatkozás adatfrissítések, a teljesítményadat-gyűjtés vagy a bérlő (ügyfél) telemetriai adatok gyűjtése futtatásával. Rugalmas adatbázis-feladatok érhető el jelenleg hello Azure-portál és a PowerShell-parancsmagok segítségével. Azonban az Azure portál felületek csökkentett hello korlátozott között az összes adatbázis tooexecution egy [rugalmas készlet (előzetes verzió)](sql-database-elastic-pool.md). tooaccess további funkciók és a parancsfájlok között beleértve egyénileg definiált gyűjteményét, illetve a shard adatbázisok csoportja végrehajtásának beállítása (használatával létrehozott [Elastic Database ügyféloldali kódtárának](sql-database-elastic-scale-introduction.md)), lásd: [létrehozása és kezelése PowerShell-lel feladatok](sql-database-elastic-jobs-powershell.md). További információ a feladatok: [rugalmas adatbázis-feladatok áttekintése](sql-database-elastic-jobs-overview.md). 

## <a name="prerequisites"></a>Előfeltételek
* Azure-előfizetés. Ingyenes próbaverzió, lásd: [ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).
* Egy rugalmas készlet. Lásd: [kapcsolatos rugalmas készletek](sql-database-elastic-pool.md).
* Rugalmas feladat szolgáltatás-összetevők telepítését. Lásd: [hello rugalmas feladat szolgáltatás telepítése](sql-database-elastic-jobs-service-installation.md).

## <a name="creating-jobs"></a>Feladat létrehozása
1. Hello segítségével [Azure-portálon](https://portal.azure.com), a meglévő rugalmas feladat adatbáziskészlet, kattintson az **létrehozása feladat**.
2. Írja be hello felhasználónevét és jelszavát (létre telepítési feladatok) adatbázis-rendszergazda hello hello feladatok vezérlő adatbázis (metaadat-tároló-feladatok).
   
    ![Hello feladat nevét, írja be vagy illessze be a kódját, majd kattintson a Futtatás][1]
3. A hello **feladat létrehozása** panelen adja meg a hello feladat nevét.
4. Írja be a hello felhasználói nevet és jelszót tooconnect toohello céladatbázisokhoz parancsfájl végrehajtásának toosucceed szükséges engedélyekkel.
5. Illessze be, vagy írja be a hello T-SQL parancsfájl.
6. Kattintson a **mentése** majd **futtatása**.
   
    ![Feladatok létrehozása és futtatása][5]

## <a name="run-idempotent-jobs"></a>Az idempotent feladatok futtatása
Egy parancsfájl-adatbázisok való összevetéssel futtatásakor meg arról, hogy hello parancsfájl idempotent kell lennie. Ez azt jelenti, hogy hello parancsfájl kell tudni toorun többször, még akkor is, ha nem tudott előtt állapotban. Például, ha a parancsfájl futása sikertelen, hello feladat automatikusan megpróbálja replikációig (határértékek közé essen, mint hello újrapróbálkozási logika végül megszűnnek hello újrapróbálkozás). hello módon toodo ez toouse hello az "IF létezik-e" záradék, és törölje minden talált példányt egy új objektum létrehozása előtt. Példa itt:

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

Másik megoldásként használhatja az "IF nem létezik" záradék előtt egy új példányának létrehozásakor:

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO

    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

Ez a parancsfájl frissíti korábban létrehozott hello táblát.

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN

    ALTER TABLE TestTable

    ADD AdditionalInformation NVARCHAR(400);
    END
    GO

    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO


## <a name="checking-job-status"></a>Feladat állapotának ellenőrzése
Miután egy feladat elindult, akkor is ellenőrizhesse a telepítés előrehaladását.

1. Hello rugalmas készlet lapon kattintson **feladatok kezelése**.
   
    ![Kattintson a "Kezelése feladatok"][2]
2. Kattintson a hello (a) a feladat nevét. Hello **állapot** "Kész" vagy "Sikertelen". hello feladat részletei (b) a dátum és idő létrehozása és futtatása jelennek meg. hello (c) az alábbi listából hello hello parancsfájl minden adatbázison hello előrehaladását mutatja a hello készlet, jogosultságot ad a dátum és idő részletek.
   
    ![A befejezett feladatok ellenőrzése][3]

## <a name="checking-failed-jobs"></a>Feladatok ellenőrzése nem sikerült.
Ha egy feladat sikertelen lesz, a napló, a futtatása is található. Nem sikerült hello feladat toosee hello nevét a kattintson a Részletek gombra.

![Ellenőrizze a sikertelen feladat adatait][4]

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png


