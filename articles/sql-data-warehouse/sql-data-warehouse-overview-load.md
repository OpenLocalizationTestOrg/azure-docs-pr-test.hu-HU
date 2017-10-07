---
title: az Azure SQL Data Warehouse aaaLoad adatok |} Microsoft Docs
description: "Ismerje meg az adatok betöltése az SQL Data Warehouse hello gyakori forgatókönyvei. Ezek közé tartozik a PolyBase, az Azure blob Storage tárolóban, egybesimított fájlokba és lemez szállítási használatával. Külső eszközöket használhatja."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 2253bf46-cf72-4de7-85ce-f267494d55fa
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: d1a5063f484e9bd95f854e27a4baed512148aad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-azure-sql-data-warehouse"></a>Adatok betöltése az Azure SQL Data Warehouse
Hello forgatókönyv beállítások és adatok betöltése az SQL Data Warehouse javaslatok összegzését.

hello terhelést hello nagyon nehéz részét adatok betöltése hello adatok általában előkészítését végzi. Azure egyszerűbbé teszi betöltése használatával az Azure blob storage közös adattárként hello szolgáltatások számos, és közötti kommunikáció és az adatok mozgása tooorchestrate hello Azure Data Factory használatához Azure-szolgáltatásokhoz. Ezeket a folyamatokat integrált PolyBase technológia, amely nagymértékben párhuzamos feldolgozási (MPP) tooload adatokat az Azure blob storage párhuzamosan az SQL Data Warehouse használ. 

Oktatóprogramot kínál, amelyek mintaadatbázisokat betölteni, lásd: [mintaadatbázisokat betölteni][Load sample databases].

## <a name="load-from-azure-blob-storage"></a>Az Azure blob storage betöltése
hello leggyorsabb módon tooimport adatokat az SQL Data Warehouse PolyBase tooload adatokat az Azure blob storage toouse. A PolyBase által használt SQL Data Warehouse a nagymértékben párhuzamos adatfeldolgozás (MPP) tervezési tooload az Azure blob storage párhuzamosan. a PolyBase toouse, használhatja T-SQL parancsokkal vagy egy Azure Data Factory-folyamathoz.

### <a name="1-use-polybase-and-t-sql"></a>1. A PolyBase és a T-SQL
A betöltési folyamat összegzése:

1. Helyezze át a adatok tooAzure blob-tároló vagy az Azure Data Lake Store, és a szöveges fájlt tárolja.
2. Külső objektumok konfigurálása az SQL Data Warehouse toodefine hello helyét és hello adatok formátuma
3. Egy új táblába adatbázis egy T-SQL parancsot tooload hello adatok párhuzamosan futnak.

<!-- 5. Schedule and run a loading job. --> 

Az oktatóanyagok esetén lásd: [adatok betöltése az Azure blob storage tooSQL Data warehouse-ba (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].

### <a name="2-use-azure-data-factory"></a>2. Az Azure Data Factory használata
Egy egyszerűbb módon toouse PolyBase létrehozhat egy Azure Data Factory-folyamathoz, amely az SQL Data Warehouse PolyBase tooload adatait az Azure blob storage használja. Gyors tooconfigure Ez azért, mert nem kell toodefine hello T-SQL-objektumokat. Ha tooquery hello külső adatforráshoz kell importálni a nélkül, használja a T-SQL. 

A betöltési folyamat összegzése:

1. Helyezze át az adatokat tooAzure blob-tároló, és szöveges fájlt tárolja. Az Azure Data Factory jelenleg nem támogatja a polybase-zel ADLS-kapcsolat).
2. Hozzon létre egy Azure Data Factory adatcsatorna tooingest hello adatokat. Hello PolyBase lehetőséggel.
4. Ütemezési és futtatási hello folyamat.

Az oktatóanyagok esetén lásd: [adatok betöltése az Azure blob storage tooSQL Data warehouse-ba (Azure Data Factory)][Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)].

## <a name="load-from-sql-server"></a>Betöltése az SQL Server
is használhat az Integration Services (SSIS), az adatraktár SQL Server tooSQL tooload adatait át egybesimított fájlokba, vagy küldje el a lemezek tooMicrosoft. Olvassa el a toosee különböző folyamatok és hivatkozások tootutorials betöltése hello összegzését.

tooplan teljes adatok áttelepítését a SQL Server tooSQL adatraktár, lásd: hello [áttelepítése – áttekintés][Migration overview]. 

### <a name="use-integration-services-ssis"></a>Használja az Integration Services (SSIS)
Ha már használja az SQL Server Integration Services (SSIS) csomag tooload, frissítheti a csomagok toouse SQL Server hello forrás-és az SQL Data Warehouse hello célként. Ez a gyors és egyszerű toodo, és akkor hasznos, ha nem kívánt toomigrate a betöltés már hello felhőben toouse adatok feldolgozása. hello kompromisszumot hello terhelés lassabb, mint a PolyBase használatával, mert a SSIS nem végez hello terhelés párhuzamos lesz.

A betöltési folyamat összegzése:

1. Vizsgálja felül az integrációs szolgáltatások csomag toopoint toohello SQL Server-példányt hello forrás és hello cél hello SQL Data Warehouse-adatbázis.
2. Telepítse át a séma tooSQL Data Warehouse még nem létezik.
3. A csomagok hello megfeleltetés módosítása csak hello adattípusok használatát az SQL Data Warehouse által támogatott.
4. Ütemezés, és futtassa hello csomagot.

Az oktatóanyagok esetén lásd: [adatok betöltése az SQL Server tooAzure SQL Data Warehouse (SSIS)][Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)].

### <a name="use-azcopy-recommended-for--10-tb-data"></a>Használja az AZCopy (< 10 TB-adatok esetén ajánlott)
Ha az adatok mérete < 10 TB, akkor is hello adatok exportálása az SQL Server tooflat fájlok, másolása hello fájlok tooAzure a blob storage és majd használja az SQL Data Warehouse PolyBase tooload hello adatok

A betöltési folyamat összegzése:

1. SQL Server tooflat fájlok hello bcp parancssori segédprogram tooexport adatokat használja.
2. Hello AZCopy parancssori segédprogram toocopy adatokat a egybesimított fájlokba tooAzure blob storage használata.
3. Az SQL Data Warehouse PolyBase tooload használata

Az oktatóanyagok esetén lásd: [adatok betöltése az Azure blob storage tooSQL Data warehouse-ba (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].

### <a name="use-bcp"></a>Használja a bcp
Ha kis mennyiségű adatokat a bcp tooload közvetlenül az Azure SQL Data Warehouse is használhatja.

A betöltési folyamat összegzése:

1. SQL Server tooflat fájlok hello bcp parancssori segédprogram tooexport adatokat használja.
2. A használandó bcp tooload adatok simán fájlok közvetlenül tooSQL Data warehouse-bA.

Az oktatóanyagok esetén lásd: [adatok betöltése az SQL Server tooAzure SQL Data warehouse-ba (bcp)][Load data from SQL Server tooAzure SQL Data Warehouse (bcp)].

### <a name="use-importexport-recommended-for--10-tb-data"></a>Használja az Import/Export (> 10 TB-adatok esetén ajánlott)
Ha az adatok mérete > 10 TB-os és toomove azt tooAzure, azt javasoljuk, hogy használja a szolgáltatás szállítási lemez [Import/Export][Import/Export]. 

A betöltési folyamat összegzése

1. SQL Server tooflat fájlokat továbbítható lemezeken hello bcp parancssori segédprogram tooexport adatokat használja.
2. Hello lemezek tooMicrosoft szolgáltatástól.
3. A Microsoft hello adatokat tölt az SQL Data Warehouse

## <a name="load-from-hdinsight"></a>A HDInsight-ból betöltése
Az SQL Data Warehouse polybase HDInsight az adatok betöltését támogatja. hello folyamat van hello ugyanaz, mint az adatok betöltése az Azure Blob Storage - PolyBase tooconnect tooHDInsight tooload adatokkal. 

### <a name="1-use-polybase-and-t-sql"></a>1. A PolyBase és a T-SQL
A betöltési folyamat összegzése:

1. Helyezze át az adatokat tooHDInsight, és tárolja a szövegfájlok, ORC vagy Parquet formátumban.
2. Külső objektumok adja meg az SQL Data Warehouse toodefine hello helyét és hello adatok formátuma.
3. Egy új táblába adatbázis egy T-SQL parancsot tooload hello adatok párhuzamosan futnak.

Az oktatóanyagok esetén lásd: [adatok betöltése az Azure blob storage tooSQL Data warehouse-ba (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].

## <a name="recommendations"></a>Javaslatok
Partnereink számos rendelkezik megoldások betöltésekor. További, toofind listájának megtekintéséhez a [megoldási partnerek][solution partners]. 

Ha az adatokat a nem relációs forrásból származik, és azt az SQL Data adatraktár, akkor tooload kell tootransform sorok és oszlopok ahhoz, hogy töltse be azt. hello átalakított adatokat nem szükséges egy adatbázisban tárolt toobe, szövegfájlok tárolható.

Statisztika létrehozása az újonnan betöltött adatokról. Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését.  A sorrend tooget hello legjobb teljesítmény elérése érdekében a lekérdezéseket a fontos toocreate statisztika után hello táblák összes oszlopához az első betöltése vagy hello adatok minden lényeges módosítását fordul elő.  További információkért lásd: [statisztika][Statistics].

## <a name="next-steps"></a>Következő lépések
További fejlesztési tippek, lásd: hello [fejlesztői áttekintés][development overview].

<!--Image references-->

<!--Article references-->
[Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)]: ./sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md
[Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)]: ./sql-data-warehouse-load-from-sql-server-with-integration-services.md
[Load data from SQL Server tooAzure SQL Data Warehouse (bcp)]: ./sql-data-warehouse-load-from-sql-server-with-bcp.md
[Load data from SQL Server tooAzure SQL Data Warehouse (AZCopy)]: ./sql-data-warehouse-load-from-sql-server-with-azcopy.md

[Load sample databases]: ./sql-data-warehouse-load-sample-databases.md
[Migration overview]: ./sql-data-warehouse-overview-migrate.md
[solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[development overview]: ./sql-data-warehouse-overview-develop.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->

<!--Other Web references-->
[Import/Export]: https://azure.microsoft.com/documentation/articles/storage-import-export-service/
