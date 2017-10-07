---
title: "az SQL Data Warehouse-adatok aaaLoad terabájt |} Microsoft Docs"
description: "Bemutatja, hogyan 1 TB adatot tölthetők be az Azure SQL Data Warehouse Azure Data Factory a 15 perc"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: a6c133c0-ced2-463c-86f0-a07b00c9e37f
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jingwang
ms.openlocfilehash: e63f60461b082b0e3979004cb631dbf4a6710de3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a>1 TB-os betöltése az Azure SQL Data Warehouse Data Factory a 15 perc
[Az SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) egy olyan felhőalapú, horizontálisan felskálázható adatbázis képes a nagy mennyiségű relációs és nem relációs adatok.  Nagymértékben párhuzamos feldolgozási (MPP) architektúrára épülő SQL Data Warehouse optimalizált vállalati adatok adatraktár munkaterhelések.  Felhőalapú hello rugalmasságot tooscale tárolóval a rugalmasság és a számítási egymástól függetlenül kínál.

Ismerkedés az Azure SQL Data Warehouse mostantól egyszerűbb, mint minden eddiginél használatával **Azure Data Factory**.  Az Azure Data Factory egy teljes körűen felügyelt felhőalapú integrációs szolgáltatás, amely lehet használt toopopulate egy SQL Data Warehouse a meglévő rendszerről, és akkor értékes időt az SQL Data Warehouse értékelése és felépítése az elemzés közben mentése hello adatokkal megoldások. Az alábbiakban hello legfontosabb előnyei adatok betöltése az Azure SQL Data Warehouse Azure Data Factory használatával:

* **Egyszerű tooset be**: 5. lépés intuitív varázsló nem parancsfájl-szükséges.
* **Gazdag adattároló támogatási**: a helyszíni és felhőalapú adattároló széles skáláját beépített támogatása.
* **Biztonságos és megfelelő**: HTTPS- vagy ExpressRoute keresztül továbbított adatok, és a globális szolgáltatás meglétének biztosítja az adatok sosem hagyja el hello földrajzi határ
* **A PolyBase használatával unparalleled teljesítmény** – a Polybase használatával hello leghatékonyabb módon toomove adatokat az Azure SQL Data Warehouse. Blob szolgáltatás átmeneti hello használ, nagy terhelés sebességű adattárolókhoz mellett az Azure Blob Storage tárolóban, amely támogatja a Polybase hello alapértelmezés szerint az összes típusú érhet el.

Ez a cikk bemutatja, hogyan toouse Data Factory másolása varázsló tooload 1 TB méretű adatok az Azure Blob Storage az Azure SQL Data Warehouse a 15 percig, több mint 1,2 GB/s átviteli sebesség.

Ez a cikk részletesen adatok áthelyezése az Azure SQL Data Warehouse hello másolása varázsló használatával.

> [!NOTE]
>  Általános képességeivel kapcsolatos információk a Data Factory az adatok áthelyezése az Azure SQL Data Warehouse, lásd: [adatok tooand áthelyezése az Azure SQL Data Warehouse Azure Data Factory használatával](data-factory-azure-sql-data-warehouse-connector.md) cikk.
>
> Is hozhat létre Azure-portált használja, a Visual Studio PowerShell, adatcsatornákat stb. Lásd: [oktatóanyag: adatok másolása az Azure Blob tooAzure SQL-adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a részletes utasításokat hello másolási tevékenység az Azure Data Factory gyors útmutatást.  
>
>

## <a name="prerequisites"></a>Előfeltételek
* Az Azure Blob Storage: ehhez a kísérlethez Azure Blob Storage (GRS) használ a TPC-H tesztelési adatkészlet tárolásához.  Ha nem rendelkezik Azure-tárfiók, további [hogyan toocreate tárfiók](../storage/common/storage-create-storage-account.md#create-a-storage-account).
* [TPC-H](http://www.tpc.org/tpch/) adatok: fogjuk toouse TPC-H, tesztelési dataset hello.  hogy szüksége van-e toouse toodo `dbgen` a TPC-H eszközkészlet segítségével hello dataset készítése.  Vagy letöltheti a forráskódot `dbgen` a [TPC eszközök](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) , és saját magára vagy letöltési összeállított hello bináris fájljának lefordítani [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).  Futtatási dbgen.exe hello következőre toogenerate 1 TB-os egybesimított fájl a parancsok `lineitem` terjedésének tábla 10 a fájlok között:

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * …
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    Másolás hello most fájlok tooAzure Blob jönnek létre.  Tekintse meg a túl[adatok tooand áthelyezését egy helyszíni fájlrendszer Azure Data Factory](data-factory-onprem-file-system-connector.md) arról, hogyan toodo, amely ADF-másolással.    
* Az SQL Data Warehouse: ehhez a kísérlethez adatokat tölt az Azure SQL Data Warehouse 6000 dwu-k számát létrehozva

    Tekintse meg a túl[hozzon létre egy Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) részletes információkra van szüksége hogyan toocreate egy SQL Data Warehouse-adatbázis.  tooget hello legjobb lehetséges terhelés teljesítmény elérése érdekében az SQL Data Warehouse Polybase használatával választjuk Adattárházegységek (dwu-k) engedélyezett az hello beállítás, amely 6000 dwu-k maximális számát.

  > [!NOTE]
  > Az Azure Blob betöltésekor hello Adatbetöltési teljesítmény közvetlenül arányos toohello beállítja az SQL Data Warehouse hello dwu-k száma:
  >
  > 1 TB-os betöltése 1000 a DWU az SQL Data Warehouse veszi a 87 perc (~ 200 MB/s átviteli) betöltése 1 TB 2000 DWU az SQL Data Warehouse vesz 46 perc (~ 380 MB/s átviteli) betöltése 1 TB az 6000 DWU SQL Data Warehouse perc 14 (~1.2 GB/s átviteli)
  >
  >

    6000 dwu-k, az SQL Data Warehouse toocreate csúszkát hello teljesítmény összes hello módon toohello jobbra:

    ![Teljesítmény csúszka](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    A meglévő adatbázis, amely nincs beállítva 6000 dwu-k legfeljebb az Azure-portál használatával.  Keresse meg a toohello adatbázis Azure-portálon, és nincs egy **méretezési** hello gombjára **áttekintése** panel hello kép a következő látható:

    ![Skála gomb](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    Kattintson a hello **méretezési** gomb tooopen hello következő panelen, hello csúszkát toohello maximális értéket, és kattintson **mentése** gombra.

    ![Párbeszédpanel](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    Ehhez a kísérlethez adatokat tölt be az Azure SQL Data Warehouse használatával `xlargerc` erőforrásosztály.

    tooachieve legjobb lehetséges átviteli sebességet másolása kell tartozó túl az SQL Data Warehouse felhasználó segítségével toobe`xlargerc` erőforrásosztály.  Megtudhatja, hogyan toodo, amelyek a következő [módosíthatja a felhasználói erőforrás osztály példa](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).  
* Cél táblaséma létrehozása az Azure SQL Data Warehouse-adatbázis, hello DDL-utasítás a következő futtatásával:

    ```SQL  
    CREATE TABLE [dbo].[lineitem]
    (
        [L_ORDERKEY] [bigint] NOT NULL,
        [L_PARTKEY] [bigint] NOT NULL,
        [L_SUPPKEY] [bigint] NOT NULL,
        [L_LINENUMBER] [int] NOT NULL,
        [L_QUANTITY] [decimal](15, 2) NULL,
        [L_EXTENDEDPRICE] [decimal](15, 2) NULL,
        [L_DISCOUNT] [decimal](15, 2) NULL,
        [L_TAX] [decimal](15, 2) NULL,
        [L_RETURNFLAG] [char](1) NULL,
        [L_LINESTATUS] [char](1) NULL,
        [L_SHIPDATE] [date] NULL,
        [L_COMMITDATE] [date] NULL,
        [L_RECEIPTDATE] [date] NULL,
        [L_SHIPINSTRUCT] [char](25) NULL,
        [L_SHIPMODE] [char](10) NULL,
        [L_COMMENT] [varchar](44) NULL
    )
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    ```
Hello előzetesen szükséges lépések leírását befejeződött, a felhőmodellből most készen áll a tooconfigure hello másolási tevékenység hello másolása varázsló használatával.

## <a name="launch-copy-wizard"></a>A Másolás varázsló indítása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **+ új** hello bal felső sarokban, kattintson **Eszközintelligencia + analitika**, és kattintson a **adat-előállító**.
3. A hello **új adat-előállító** panel:

   1. Adja meg **LoadIntoSQLDWDataFactory** a hello **neve**.
       az Azure data factory hello hello nevének globálisan egyedi kell lennie. Ha hello hibaüzenetet kapja: **nem érhető el adat-előállító "LoadIntoSQLDWDataFactory"**, hello adat-előállítóban (például yournameLoadIntoSQLDWDataFactory) hello nevének módosítása, és próbálja meg újra létrehozni. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.  
   2. Jelölje ki az Azure-**előfizetést**.
   3. Az erőforráscsoport hajtsa végre a lépéseket követve hello egyikét:
      1. Válassza ki **meglévő** tooselect egy meglévő erőforráscsoportot.
      2. Válassza ki **hozzon létre új** tooenter erőforráscsoport nevét.
   4. Válassza ki a **hely** hello adat-előállító esetében.
   5. Válassza ki **PIN-kód toodashboard** hello hello panel alsó részén jelölőnégyzetet.  
   6. Kattintson a **Create** (Létrehozás) gombra.
4. Hello létrehozásának befejezése után megjelenik a hello **adat-előállító** panelen látható hello kép a következő módon:

   ![Data factory kezdőlap](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. A hello adat-előállító kezdőlapján kattintson a hello **adatok másolása** toolaunch csempe **másolása varázsló**.

   > [!NOTE]
   > Ha azt látja, hogy hello webböngésző akadt-e a "Engedélyező...", tiltsa le vagy törölje a jelet **harmadik féltől származó cookie-k blokkolását, és a helyadatok** beállítása (vagy) engedélyezve legyen, és hozzon létre egy kivételt **login.microsoftonline.com**, és ezután próbálja meg újból elindítani a hello varázsló.
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a>1. lépés: Adatok betöltése az ütemezés konfigurálása
hello első lépése az tooconfigure hello adatai ütemezés betöltésekor.  

A hello **tulajdonságok** lap:

1. Adja meg **CopyFromBlobToAzureSqlDataWarehouse** a **feladat neve**
2. Válassza ki **futtassa egyszer most** lehetőséget.   
3. Kattintson a **Tovább** gombra.  

    ![Másolása varázsló – Tulajdonságok lap](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a>2. lépés: Állítson be forrás
A szakasz tartalmazza, akkor hello lépéseket tooconfigure hello forrás: Azure Blob tartalmazó hello 1 TB méretű TPC-H sorelemet fájlokat.

1. Jelölje be hello **Azure Blob Storage** hello adatok tárolására, és kattintson a **következő**.

    ![Másolása varázsló – forrás kiválasztása lap](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. Hello Azure Blob storage-fiók hello csatlakozási adatait, majd kattintson **következő**.

    ![Másolása varázsló – adatforrás kapcsolati adatait](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. Válassza ki a hello **mappa** hello TPC-H sort tartalmazó fájlok elemet, és kattintson a **következő**.

    ![Másolása varázsló – a bemeneti mappa kiválasztása](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. Gomb megnyomásakor **következő**, hello fájl formátuma beállításokat a rendszer automatikusan észleli.  Ellenőrizze, hogy az adott oszlop határolójel toomake ' |} "helyett hello alapértelmezett vesszővel", ".  Kattintson a **következő** után meg kell előzetesen hello adatok.

    ![Másolása varázsló – fájl formázási beállítások](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a>3. lépés: A célkiszolgáló konfigurálása
Ez a szakasz bemutatja, hogyan tooconfigure hello cél: `lineitem` hello Azure SQL Data Warehouse-adatbázis táblája.

1. Válasszon **Azure SQL Data Warehouse** hello célként tárolja, és kattintson a **következő**.

    ![Másolása varázsló – a célkiszolgáló kijelölése adattár](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. Töltse ki hello Azure SQL Data Warehouse-kapcsolódási információt.  Ellenőrizze, hogy a megadott hello szerepkör hello felhasználói `xlargerc` (lásd: hello **Előfeltételek** szakaszban részletes információkra van szüksége), és kattintson **tovább**.

    ![Másolása varázsló – cél Kapcsolatinformáció](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. Hello céltábla válasszon, majd kattintson a **következő**.

    ![Másolása varázsló – leképezés lapja](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. A séma leképezési lapon hagyja "Alkalmazza oszlopleképezés" beállítás nincs bejelölve, és kattintson a **következő**.

## <a name="step-4-performance-settings"></a>4. lépés: Teljesítményadat-beállításai

**A polybase lehetővé** alapértelmezés szerint be van jelölve.  Kattintson a **Tovább** gombra.

![Másolása varázsló – séma leképezési lap](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a>5. lépés: Telepítheti, és figyelheti a terhelést eredmények
1. Kattintson a **Befejezés** gomb toodeploy.

    ![Másolása varázsló – összegző lapja](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. Hello telepítés befejezése után kattintson `Click here toomonitor copy pipeline` toomonitor hello másolási folyamat futtatásához. Jelölje be hello másolási folyamat hello létrehozott **tevékenység Windows** listája.

    ![Másolása varázsló – összegző lapja](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    Megtekintheti a részletek futtatni hello hello másolása **tevékenység ablak Explorer** hello jobb oldali panelen, beleértve a hello adatmennyiség forrásból olvashatók és írhatók a cél, időtartama és hello átlagos átviteli sebességgel hello futtassa a.

    A következő képernyőfelvétel, 1 TB-os másolása az Azure Blob Storage-ból az SQL Data Warehouse hello látható tartott 14 perc, hatékonyan a 1.22 GB/s átviteli sebesség eléréséhez!

    ![Másolása varázsló – sikeres párbeszédpanel](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a>Ajánlott eljárások
Az alábbiakban néhány gyakorlati tanácsok az Azure SQL Data Warehouse-adatbázis futtatásához:

* Használjon nagyobb erőforrásosztály FÜRTÖZÖTT OSZLOPCENTRIKUS INDEX vezérlőbe való betöltés.
* Hatékonyabb táblákra érdemes lehet kivonatoló terjesztési round robin terjesztési alapértelmezett helyett válassza oszlop szerint.
* Betöltési sebesség gyorsabb érdemes halommemória az átmeneti adatok.
* Statisztikák létrehozása az Azure SQL Data Warehouse betöltése után.

Lásd: [ajánlott eljárások az Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) részleteiről.

## <a name="next-steps"></a>Következő lépések
* [Data Factory másolása varázsló](data-factory-copy-wizard.md) – Ez a cikk ismerteti a hello másolása varázsló.
* [Másolja a tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) -Ez a cikk hello hivatkozás TELJESÍTMÉNYMÉRÉSEK és hangolási útmutató tartalmazza.
