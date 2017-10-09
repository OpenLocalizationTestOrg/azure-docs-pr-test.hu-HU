---
title: "az Azure SQL Data Warehouse – adat-előállító aaaLoad adatok |} Microsoft Docs"
description: "Ez az oktatóanyag adatokat tölt az Azure SQL Data Warehouse Azure Data Factory használatával, és egy SQL Server-adatbázist használja, mint hello adatforrás."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse;azure-data-factory
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: loading
ms.date: 02/08/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 471871d3ee00ab34cc84bb63fbd13a323d14c2b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-into-sql-data-warehouse-with-data-factory"></a>Adatok betöltése az SQL Data Warehouse Data Factory

Azure Data Factory tooload adatokat használhatja az Azure SQL Data Warehouse bármelyik hello [támogatott forrás adattárolókhoz](../data-factory/data-factory-data-movement-activities.md#supported-data-stores-and-formats). Például akkor lehet adatok betöltése az Azure SQL-adatbázis vagy Oracle-adatbázishoz az SQL data warehouse Data Factory használatával. Ez a cikk az oktatóanyag bemutatja, hogyan tooload adatait egy helyi SQL Server adatbázis-e az SQL data warehouse.

**Becsült idő**: Ez az oktatóanyag kapcsolatos toocomplete 10 – 15 percet vesz igénybe, hello előfeltételek teljesülése után.

## <a name="prerequisites"></a>Előfeltételek

- Kell egy **SQL Server-adatbázis** hello adatokat tartalmazó táblák toobe felülírt toohello SQL data warehouse-bA.  

- Az online kell **SQL Data Warehouse**. Ha még nem rendelkezik adatraktárral, megtudhatja, hogyan túl[hozzon létre egy Azure SQL Data Warehouse](sql-data-warehouse-get-started-provision.md).

- Kell egy **Azure Storage-fiók**. Ha még nem rendelkezik egy tárfiókot, megtudhatja, hogyan túl[hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md). A legjobb teljesítmény érdekében keresse meg a tárfiók hello és hello az adatraktár-hello az azonos Azure-régiót.

## <a name="configure-a-data-factory"></a>Egy adat-előállító konfigurálása
1. Jelentkezzen be toohello [Azure-portálon][].
2. Keresse meg az adatraktár és tooopen kattintson azt.
3. Hello fő paneljén kattintson **adatok betöltése** > **Azure Data Factory**.

    ![Adatok betöltése varázsló indítása](media/sql-data-warehouse-load-with-data-factory/launch-load-data-wizard.png)

4. Ha nem rendelkezik adat-előállítót az Azure-előfizetéssel, akkor tekintse meg a **új adat-előállító** hello böngésző külön lapon párbeszédpanel. Adja meg a hello a kért információkat, majd kattintson az **létrehozása**. Hello adat-előállító létrehozása után hello **új adat-előállító** párbeszédpanel bezárul, és meg hello **válasszon adat-előállító** párbeszédpanel megnyitásához.

    Ha már a hello Azure-előfizetés egy vagy több adat-előállítók, megjelenik a hello **válasszon adat-előállító** párbeszédpanel megnyitásához. Az ezen a párbeszédpanelen válassza ki valamelyik adat-előállítót, vagy kattintson a **hozzon létre új adat-előállító** toocreate egy újat.

    ![Adat-előállító konfigurálása](media/sql-data-warehouse-load-with-data-factory/configure-data-factory.png)

5. A hello **válasszon adat-előállító** párbeszédpanelen hello **adatok betöltése** beállítás alapértelmezés szerint. Kattintson a **következő** toostart a feladat betöltése adatok létrehozása.

## <a name="configure-hello-data-factory-properties"></a>Hello data factory tulajdonságainak konfigurálása
Most, hogy egy adat-előállító létrehozott hello következő lépésre tooconfigure hello Adatbetöltési ütemezés.

1. A **feladatnév**, adja meg **DWLoadData-fromSQLServer**.
2. Hello alapértelmezett **futtassa egyszer most** , majd **következő**.

    ![Betöltési ütemezését úgy állítsa be](media/sql-data-warehouse-load-with-data-factory/configure-load-schedule.png)

## <a name="configure-hello-source-data-store-and-gateway"></a>Hello forrás adattár és átjáró konfigurálása
Most pedig utasítani fogja a Data Factory hello a helyszíni SQL Server adatbázis-, amelyből el kívánja tooload adatokat.

1. Válasszon **SQL Server** hello támogatott forrásadatok katalógus tárolja, és kattintson a **következő**.

    ![Válassza ki az SQL Server-adatforrás](media/sql-data-warehouse-load-with-data-factory/choose-sql-server-source.png)

2. A **megadása hello a helyszíni SQL Server-adatbázis** párbeszédpanel jelenik meg. először hello **kapcsolatnév** mező rendszer automatikusan kitölti. hello második mező kér hello hello neve **átjáró**. Ha használ, amely már rendelkezik az átjáró valamelyik adat-előállítót, hello legördülő listában használni hello átjáró. Kattintson a hello **átjáró létrehozása** toocreate adatkezelési átjárót hivatkozásra.  

    > [!NOTE]
    > Ha hello forrásadatok tárolja a helyszínen vagy egy Azure IaaS virtuális gépen, az adatkezelési átjáró szükséges. Átjáró egy adat-előállító 1-1 kapcsolattal rendelkezik. Az adat-előállító nem használható, de több Adatbetöltési hello a feladatok használható azonos adat-előállítóban. Átjáró lehet használt tooconnect toomultiple adattárolókhoz Adatbetöltési feladatok futtatásakor.
    >
    > Hello átjáró kapcsolatos részletes információkért lásd: [az adatkezelési átjáró](../data-factory/data-factory-data-management-gateway.md) cikk.

3. A **átjáró létrehozása** párbeszédpanel jelenik meg. A nevét, adja meg a **GatewayForDWLoading**, és kattintson a **létrehozása**.

4. A **átjáró konfigurálása** párbeszédpanel jelenik meg. Kattintson a **indítsa el a számítógépen a gyorstelepítés** tooautomatically letöltése, telepítése, és az adatkezelési átjáró regisztrálása az aktuális számítógépen. hello folyamatban van egy előugró ablak látható. Ha hello gép toohello adattár nem tud csatlakozni, manuálisan is [töltse le és telepítse a hello átjáró](https://www.microsoft.com/download/details.aspx?id=39717) olyan gépen, amely képes kapcsolódni a toohello adatok tárolására, és hello kulcs tooregister használja.
    > [!NOTE]
    > a gyorstelepítés hello natív módon a Microsoft Edge és az Internet Explorer működik. Ha Google Chrome használ, először telepítse hello ClickOnce bővítmény Chrome webes store-ból.

    ![Expressz telepítés elindítása](media/sql-data-warehouse-load-with-data-factory/launch-express-setup.png)

5. Várjon, amíg hello átjáró telepítő toocomplete. Miután hello átjáró regisztrálása sikeres volt, és online állapotban, hello előugró ablak bezárása és hello új átjáró hello átjáró mezőben jelenik meg. Ezután a fill hello többi kötelező mezők az alábbiak szerint, majd kattintson a **következő**.
    - **Kiszolgálónév**: hello nevét a helyszíni SQL Server.
    - **Az adatbázisnév**: SQL Server-adatbázist.
    - **Hitelesítőadat-titkosítás**: "által webböngésző" hello alapértelmezett használja.
    - **Hitelesítés típusa**: Adja meg hello típusú hitelesítést használ.
    - **Felhasználónév** és **jelszó**: hello felhasználónév és jelszó megadása egy felhasználó, aki rendelkezik engedéllyel toocopy hello adatokat.

    ![Expressz telepítés elindítása](media/sql-data-warehouse-load-with-data-factory/configure-sql-server.png)

6. hello a következő lépésre toochoose hello táblák mely toocopy hello adatokból. Hello táblák kulcsszavak használatával végezhet. És hello adatok és a tábla séma hello alsó panelen megtekintheti. A kijelölés befejezése után kattintson **következő**.

    ![Select tables (Táblák kiválasztása)](media/sql-data-warehouse-load-with-data-factory/select-tables.png)

## <a name="configure-hello-destination-your-sql-data-warehouse"></a>Hello cél, az SQL Data Warehouse konfigurálása

Most pedig utasítani fogja a Data Factory hello cél információkról.

1. Az SQL Data Warehouse-kapcsolat adatainak automatikusan kitölti a rendszer. Adjon meg felhasználónevet hello hello jelszót. Kattintson **következő**.

    ![Cél konfigurálása](media/sql-data-warehouse-load-with-data-factory/configure-destination.png)

2. Egy intelligens táblaleképezés jelenik meg, amely leképezi a forrás toodestination táblák táblanevek alapján. Ha hello tábla nem létezik a célként megadott hello, alapértelmezés szerint ADF létrehoz egyet a hello ugyanazt a nevet (Ez vonatkozik a tooSQL Server vagy az Azure SQL Database forrásaként). Másik lehetőségként toomap tooan meglévő tábla. Tekintse át, és kattintson a **következő**.

    ![Táblázatok hozzárendelése](media/sql-data-warehouse-load-with-data-factory/table-mapping.png)

3. Tekintse át a hello séma-hozzárendeléséhez, és keressen hiba vagy figyelmeztető üzeneteket. Intelligens leképezési oszlopnév alapul. Egy nem támogatott típus hello forrás és cél oszlop közötti átalakítás esetén lásd: hello megfelelő tábla mellett hibaüzenetet. Ha úgy dönt, hogy a Data Factory automatikus toolet hello táblák létrehozása, megfelelő adattípus konvertálása toofix hello kompatibilitási forrás- és tárolók közötti szükség esetén fordulhat elő.

    ![Térkép séma](media/sql-data-warehouse-load-with-data-factory/schema-mapping.png)

4. Kattintson a **Tovább** gombra.

## <a name="configure-hello-performance-settings"></a>Hello Teljesítménybeállítások konfigurálása
Hello teljesítmény konfigurációkban átmeneti hello adatokat, az SQL Data Warehouse performantly használatával megelőzően használt Azure storage-fiók konfigurálása [PolyBase](sql-data-warehouse-best-practices.md#use-polybase-to-load-and-export-data-quickly). Hello másolása után a program automatikusan kiüríti hello köztes adatok.

Jelöljön ki egy meglévő Azure Storage-fiók, majd a **következő**.

![Átmeneti blob konfigurálása](media/sql-data-warehouse-load-with-data-factory/configure-staging-blob.png)

## <a name="review-summary-information-and-deploy-hello-pipeline"></a>Tekintse át az összefoglaló információkat és hello adatcsatornát

Olvassa el a hello konfigurációs és a **Befejezés** gomb toodeploy hello folyamat.

![Adat-előállító telepítése](media/sql-data-warehouse-load-with-data-factory/deploy-data-factory.png)

## <a name="monitor-data-loading-progress"></a>A figyelő adatok betöltése folyamatban

Hello központi telepítésének állapotáról és hello eredményez **telepítési** lap.

1. Miután hello üzembe helyezés hivatkozásra hello, amely szerint **ide toomonitor másolási folyamat** toomonitor adatok betöltése folyamatban van.

    ![Folyamat figyelése](media/sql-data-warehouse-load-with-data-factory/monitor-pipeline.png)

2. az újonnan létrehozott hello **DWLoadData-fromSQLServer** adatok betöltését csővezeték kiválasztva a bal oldali hello automatikus **erőforrás-kezelő**.

    ![Nézet folyamat](media/sql-data-warehouse-load-with-data-factory/view-pipeline.png)

3. Kattintson bele hello folyamatba hello középső panel toosee hello részletes minden táblához, amely leképezhető tooan tevékenység állapota.

    ![Tábla tevékenység megtekintése](media/sql-data-warehouse-load-with-data-factory/view-table-activity.png)

4. További kattintson be egy tevékenységet, és adataira hello hello jobb oldali panelen, mint például a adatméret vagy sorok, átviteli részletek betöltésekor.

    ![Tábla tevékenység részleteinek megtekintése](media/sql-data-warehouse-load-with-data-factory/view-table-activity-details.png)

5. toolaunch a figyelési nézet újabb, lépjen tooyour SQL Data Warehouse, kattintson a **adatok betöltése > Azure Data Factory**, válassza ki a gyári, és válassza a **meglévő betöltése feladatok figyelése**.

## <a name="next-steps"></a>Következő lépések

toomigrate az adatbázis tooSQL adatraktár, lásd: [áttelepítése – áttekintés](sql-data-warehouse-overview-migrate.md).

További információ az Azure Data Factory és az adatok mozgása platformképességei toolearn lásd: a következő cikkek hello:

- [Data Factory bemutatása tooAzure](../data-factory/data-factory-introduction.md)
- [Adatok áthelyezése a másolási tevékenység használatával](../data-factory/data-factory-data-movement-activities.md)
- [Adatok tooand áthelyezése az Azure SQL Data Warehouse Azure Data Factory használatával](../data-factory/data-factory-azure-sql-data-warehouse-connector.md)

tooexplore az adatokat az SQL Data Warehouse, lásd a következő cikkek hello:

- [Visual Studio és az SSDT tooSQL adatraktár csatlakozás](sql-data-warehouse-query-visual-studio.md)
- [Visual adataiba a Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md).

<!-- Azure references -->
[Azure-portálon]: https://portal.azure.com
