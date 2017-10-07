---
title: aaaMove adatok tooan Azure SQL Database az Azure Machine Learning |} Microsoft Docs
description: "SQL-tábla létrehozása és adatok tooSQL tábla betöltése"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 50f8b862-4d32-44b2-a1e2-4fbc8024acaa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: b33ef836f42c17a56794baf763281e9998b16bef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooan-azure-sql-database-for-azure-machine-learning"></a>MOVE data tooan Azure SQL Database az Azure gépi tanulás
Ez a témakör bemutatja a hello beállítások megköveteli az adatok strukturálatlan fájlból (CSV vagy TSV formátumban) vagy a helyszíni SQL Server tooan Azure SQL adatbázis tárolt adatokból. Ezek a feladatok áthelyezése adatok toohello felhő hello Team adatok tudományos folyamat részét képezik.

Ez a témakör ismerteti az adatok áthelyezése tooan hello-beállítások a helyszíni SQL Server a Machine Learning szolgáltatáshoz, a következő témakörben: [adatok tooSQL kiszolgáló áthelyezése egy Azure virtuális gépen](machine-learning-data-science-move-sql-server-virtual-machine.md).

hello következő **menü** hivatkozásokat tartalmaz, amelyek ismertetik, hogyan tooingest adatok cél környezetekben, ahol hello adatok is tárolhatók és feldolgozhatók, során hello Team adatok tudományos folyamat (TDSP) tootopics.

[!INCLUDE [cap-ingest-data-selector](../../includes/cap-ingest-data-selector.md)]

hello következő táblázat összefoglalja áthelyezése adatok tooan Azure SQL Database hello beállításait.

| <b>FORRÁS</b> | <b>CÉL: Az Azure SQL-adatbázis</b> |
| --- | --- |
| <b>Egybesimított fájl (CSV vagy formázott TSV)</b> |<a href="#bulk-insert-sql-query">A tömeges beszúrás SQL-lekérdezés |
| <b>A helyszíni SQL Server</b> |1. <a href="#export-flat-file">TooFlat fájl exportálása<br> 2. <a href="#insert-tables-bcp">SQL-adatbázis áttelepítése varázsló<br> 3. <a href="#db-migration">Adatbázis biztonsági mentése és visszaállítása<br> 4. <a href="#adf">Az Azure Data Factory |

## <a name="prereqs"></a>Előfeltételek
itt leírt hello eljárásnak szükség van:

* Egy **Azure-előfizetés**. Ha nem rendelkezik előfizetéssel, regisztrálhat egy [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).
* Egy **Azure storage-fiók**. Egy Azure storage-fiók ebben az oktatóanyagban hello adatok tárolásához használja. Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) cikk. Hello storage-fiók létrehozását követően használja a tooaccess hello tárolási tooobtain hello fiók szükséges. Lásd: [a tárelérési kulcsok kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Hozzáférés tooan **Azure SQL Database**. Ha be kell állítania egy Azure SQL Database [Ismerkedés a Microsoft Azure SQL Database](../sql-database/sql-database-get-started.md) bemutatja, hogy miként tooprovision Azure SQL adatbázis új példányát.
* Telepített és konfigurált **Azure PowerShell** helyileg. Útmutatásért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).

**Adatok**: hello áttelepítési folyamatok hello segítségével egy [NYC Taxi dataset](http://chriswhong.com/open-data/foil_nyc_taxi/). hello NYC Taxi dataset út adatok és vásárokon információkat tartalmaz az és elérhető az Azure blob storage: [NYC Taxi adatok](http://www.andresmh.com/nyctaxitrips/). A minta és az ezek a fájlok leírása szerepelnek [NYC Taxi Utazgatással adatkészlet leírása](machine-learning-data-science-process-sql-walkthrough.md#dataset).

Igazítja hello eljárások ismertetett ide tooa a saját adatok készletét, vagy hello lépésekkel hello NYC Taxi dataset használatával leírtak szerint. a helyszíni SQL Server adatbázisba NYC Taxi dataset tooupload hello eljárással hello leírt [tömeges adatok importálása az SQL Server-adatbázisba](machine-learning-data-science-process-sql-walkthrough.md#dbload). Ezeket az utasításokat egy SQL Server egy Azure virtuális gépen, de a helyszíni SQL Server toohello feltöltése hello eljárását hello azonos.

## <a name="file-to-azure-sql-database"></a>Adatok áthelyezése egy egybesimított fájl forrás tooan Azure SQL adatbázis
Lehet, hogy az adatokat a (fürt megosztott kötetei szolgáltatás vagy TSV formázott) egybesimított fájlokba áthelyezett tooan Azure SQL-adatbázis egy tömeges beszúrási SQL lekérdezést használva.

### <a name="bulk-insert-sql-query"></a>A tömeges beszúrás SQL-lekérdezés
hello hello eljárás használatával hello tömeges beszúrás SQL-lekérdezés lépésekre hasonló toothose szó hello szakaszokban az adatok áthelyezése egy egybesimított fájl forrás tooSQL kiszolgálót egy Azure virtuális gépen. További információkért lásd: [tömeges beszúrás SQL-lekérdezés](machine-learning-data-science-move-sql-server-virtual-machine.md#insert-tables-bulkquery).

## <a name="sql-on-prem-to-sazure-sql-database"></a>A helyszíni SQL Server tooan Azure SQL-adatbázis áthelyezése adatait
Hello forrásadatait egy helyi SQL Server, ha nincsenek hello adatok tooan Azure SQL adatbázis áthelyezése különböző lehetőségeit:

1. [TooFlat fájl exportálása](#export-flat-file)
2. [SQL-adatbázis áttelepítése varázsló](#insert-tables-bcp)
3. [Adatbázis biztonsági mentése és visszaállítása](#db-migration)
4. [Azure Data Factory](#adf)

hello az első három hello lépésekre nagyon hasonló toothose részei [adatok tooSQL kiszolgáló áthelyezése egy Azure virtuális gépen](machine-learning-data-science-move-sql-server-virtual-machine.md) , amelyek ugyanezeket az eljárásokat vonatkoznak. Hivatkozások toohello megfelelő szakaszait, hogy a témakörben található utasításokat követve hello szerepelnek.

### <a name="export-flat-file"></a>TooFlat fájl exportálása
hello a exportáló tooa egybesimított fájl lépésekre ismertetett hasonló toothose [tooFlat fájl exportálása](machine-learning-data-science-move-sql-server-virtual-machine.md#export-flat-file).

### <a name="insert-tables-bcp"></a>SQL-adatbázis áttelepítése varázsló
hello hello SQL-adatbázis áttelepítése varázsló használatához vezető lépések ismertetett hasonló toothose [SQL-adatbázis áttelepítése varázsló](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-migration).

### <a name="db-migration"></a>Adatbázis biztonsági mentése és visszaállítása
hello lépéseket az adatbázis biztonsági mentése és visszaállítása ismertetett hasonló toothose [adatbázis biztonsági mentése és visszaállítása](machine-learning-data-science-move-sql-server-virtual-machine.md#sql-backup).

### <a name="adf"></a>Az Azure Data Factory
hello áthelyezése adatok tooan Azure SQL adatbázis Azure Data Factory (ADF) eljárásról hello témakör [tárolt adatok mozgatása egy helyi SQL server tooSQL Azure szolgáltatásban az Azure Data Factory](machine-learning-data-science-move-sql-azure-adf.md). Ez a témakör bemutatja hogyan toomove adatait egy helyi SQL Server adatbázis tooan Azure SQL adatbázis Azure Blob Storage segítségével ADF használatával.

Érdemes lehet ADF áttelepítésekor adatok igényeinek toobe folyamatosan hibrid forgatókönyv esetében, aki hozzáfér a helyszíni és felhőalapú erőforrásaihoz, és a hello adatok tranzakcióalapú van, vagy meg kell toobe üzleti logika hozzáadott tooit ha áttelepíti. ADF hello ütemezés és a feladatok által kezelt adatok rendszeres időközönként hello mozgása egyszerű JSON-parancsfájlok használata a figyelését teszi lehetővé. ADF is rendelkeznek egyéb képességeit, például a összetett műveletek támogatása.
