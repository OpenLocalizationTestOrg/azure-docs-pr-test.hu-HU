---
title: "(vízszintes particionálás) kiterjesztett felhő az adatbázisok közötti aaaReport |} Microsoft Docs"
description: "Hogyan toouse kereszt-adatbázis adatbázis-lekérdezések"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: c81ef5e3-41e9-4fd2-8631-868f2e168147
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: mlandzic
ms.openlocfilehash: e34f398f8d408cffd91a70fc2cfbda73daec3550
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a>Jelentés közötti kiterjesztett felhő (előzetes verzió)
A több Azure SQL-adatbázisok egyetlen kapcsolódási pont használatával jelentéseket hozhat létre egy [rugalmas lekérdezési](sql-database-elastic-query-overview.md). hello adatbázisok vízszintesen kell particionálni (más néven "szilánkos").

Ha egy meglévő adatbázist, olvassa el [áttelepítése a meglévő adatbázis tooscaled kibővített adatbázis](sql-database-elastic-convert-to-use-elastic-tools.md).

toounderstand hello SQL objektumok szükséges tooquery című [vízszintesen particionált az adatbázisok közötti lekérdezés](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="prerequisites"></a>Előfeltételek
Töltse le és futtassa a hello [Ismerkedés a rugalmas adatbázis eszközök minta](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-hello-sample-app"></a>A shard térkép létrehozásához-kezelőt hello mintaalkalmazás
Itt hoz létre a shard térképre manager együtt több szilánkok hello szilánkok be az adatok beszúrását követ. Ha véletlenül tooalready szilánkok telepítő szilánkos adatokkal rendelkezik rajtuk, hello lépéseket kihagyhatja, és helyezze át a következő szakaszban toohello.

1. Hozza létre, és futtassa a hello **Ismerkedés a rugalmas adatbáziseszközöket** mintaalkalmazást. Amíg hello szakasz 7. lépés hello lépésekkel [hello minta alkalmazás letöltése és futtatása](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). A 7. lépés hello végén jelenik meg a következő parancssor hello:

    ![parancssor][1]
2. Hello parancsablakot, írja be az "1", és nyomja le az ENTER **Enter**. Hello shard térkép manager hoz létre, és két szilánkok toohello kiszolgálót. Ezután írja be a "3", és nyomja le az ENTER **Enter**; hello művelet megismétlése négy alkalommal. Ez a szilánkok minta adatsorok szúrja be.
3. Hello [Azure-portálon](https://portal.azure.com) jelenítsen meg három új adatbázist a kiszolgálón:

   ![A Visual Studio-jóváhagyás][2]

   Ezen a ponton a kereszt-adatbázis-lekérdezések hello Elastic Database ügyféloldali kódtáraknál keresztül támogatottak. Például hello parancsablakban használja a 4. lehetőséget. hello egy lekérdezés eredményeként előálló több shard a rendszer mindig egy **UNION ALL** összes szilánkok hello eredményeinek.

   Hello a következő szakaszban létrehozhatunk egy minta adatbázis végponttal, amely támogatja a gazdagabb lekérdezése hello adatok szilánkok között.

## <a name="create-an-elastic-query-database"></a>A lekérdezés rugalmas adatbázis létrehozása
1. Nyissa meg hello [Azure-portálon](https://portal.azure.com) , és jelentkezzen be.
2. Hozzon létre egy új Azure SQL database hello ugyanarra a kiszolgálóra a shard beállításai. Név hello adatbázis "ElasticDBQuery."

    ![Azure-portál és a tarifacsomag][3]

    > [!NOTE]
    > használhat egy meglévő adatbázist. Ha így tesz, nem lehet, hogy szeretné-e tooexecute hello szilánkok egyikét a lekérdezések. Ez az adatbázis hello metaadatok objektumok egy rugalmas adatbázis-lekérdezés létrehozásához használható.
    >

## <a name="create-database-objects"></a>Adatbázis-objektumok létrehozása
### <a name="database-scoped-master-key-and-credentials"></a>Adatbázis-hatókörű főkulcs és a hitelesítő adatok
Ezek a kulcsok használt tooconnect toohello shard térkép manager és a hello szilánkok:

1. Nyissa meg az SQL Server Management Studio vagy Visual Studio SQL Server Data Tools összetevővel.
2. Csatlakozás tooElasticDBQuery adatbázis, és hajtsa végre a következő T-SQL parancsokkal hello:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    "felhasználónév" és a "password" kell kell hello ugyanaz, mint a 6. lépésében használt bejelentkezési adatok [hello minta alkalmazás letöltése és futtatása](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) a [Ismerkedés a rugalmas adatbáziseszközöket](sql-database-elastic-scale-get-started.md).

### <a name="external-data-sources"></a>Külső adatforrások
toocreate egy külső adatforrásból parancs következő hello ElasticDBQuery adatbázison hello hajtható végre:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 "CustomerIDShardMap" esetén hello hello shard leképezés, ha létrehozott hello shard térkép és shard térkép hello rugalmas adatbázis eszközök minta-kezelőt. Azonban ha az egyéni telepítő ezt a mintát használja, majd kell hello shard leképezésnév választja, az alkalmazás.

### <a name="external-tables"></a>Külső táblák
Hozzon létre egy külső táblát, amely megfelel a hello ügyfelek tábla hello szilánkok a parancs következő ElasticDBQuery adatbázison hello végrehajtásával:

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Egy minta rugalmas adatbázis T-SQL-lekérdezés végrehajtása
A külső adatforrást és a külső táblák meghatározása után a külső táblákon végrehajtott most már használhatja a teljes T-SQL.

Hello ElasticDBQuery adatbázison hajtsa végre a lekérdezést:

    select count(CustomerId) from [dbo].[Customers]

Megfigyelheti, hogy hello lekérdezés az összes hello szilánkok összesíti az eredményeket, és lehetőséget ad a következő kimeneti hello:

![Kimeneti részletei][4]

## <a name="import-elastic-database-query-results-tooexcel"></a>A rugalmas adatbázis-lekérdezés eredménye tooExcel importálása
 A lekérdezés tooan Excel fájl eredményei hello importálhatja.

1. Indítsa el az Excel 2013.
2. Keresse meg a toohello **adatok** menüszalagján.
3. Kattintson a **egyéb forrásokból származó** kattintson **az SQL Server**.

   ![Excel importálása más forrásokból][5]
4. A hello **Adatkapcsolat varázsló** írja be a hello kiszolgálónév és hitelesítő adatait. Ezután kattintson a **Next** (Tovább) gombra.
5. Hello párbeszédpanelen **hello adatokat tartalmazó adatbázis válassza hello**, jelölje be hello **ElasticDBQuery** adatbázis.
6. Jelölje be hello **ügyfelek** hello listanézetben táblázatban, majd kattintson **következő**. Kattintson a **Befejezés**.
7. A hello **és adatokat importálhat** űrlap **válassza ki, hogy tooview ezeket az adatokat a munkafüzet**, jelölje be **tábla** kattintson **OK**.

Az összes sorát hello **ügyfelek** a különböző szilánkok tárolt tábla feltöltéséhez hello Excel-táblában.

Most már használhatja az Excel hatékony képi megjelenítés funkciók. A kiszolgáló nevét, az adatbázisnév hello kapcsolati karakterlánc használatával, és a hitelesítő adatok tooconnect a BI és az integráció eszközök toohello rugalmas adatbázis lekérdezése. Győződjön meg arról, hogy az SQL Server támogatja-e az eszköz adatforrásként. Olvassa el a toohello rugalmas lekérdezési adatbázis és a külső táblák csakúgy, mint bármely más SQL Server-adatbázis és, hogy Ön kapcsolódnának toowith az eszköz az SQL Server-táblákra.

### <a name="cost"></a>Költségek
Nem kell külön fizetni hello rugalmas adatbázis-lekérdezés szolgáltatás használatára vonatkozó van.

Díjszabási információkért lásd: [SQL adatbázis díjszabás](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Következő lépések

* Rugalmas lekérdezési áttekintését lásd: [rugalmas lekérdezési áttekintése](sql-database-elastic-query-overview.md).
* Függőleges particionálási oktatóanyagért lásd a [első lépések (a vertikális particionálás) közötti adatbázis-lekérdezés](sql-database-elastic-query-getting-started-vertical.md).
* A szintaxis és a minta lekérdezések függőleges particionált adatok, lásd: [adatok lekérdezése függőleges particionálva)](sql-database-elastic-query-vertical-partitioning.md)
* A szintaxis és a minta lekérdezések vízszintesen particionált adatok, lásd: [adatok vízszintesen lekérdezése particionálva)](sql-database-elastic-query-horizontal-partitioning.md)
* Lásd: [sp\_hajtható végre \_távoli](https://msdn.microsoft.com/library/mt703714) tárolt eljárás, amely végrehajtja a Transact-SQL-utasítás egy egyetlen távoli Azure SQL Database vagy az adatbázisok egy vízszintes particionálási sémát a szilánkok szolgál.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
