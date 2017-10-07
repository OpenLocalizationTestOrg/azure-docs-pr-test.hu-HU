---
title: "az SQL Data Warehouse - aaaAzure első lépések útmutató |} Microsoft Docs"
description: "Ez az oktatóanyag útmutatást ad meg hogyan tooprovision és az adatok betöltése az Azure SQL Data Warehouse. Megismerheti a méretezés, szüneteltetése és hangolása hello alapjairól is."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: barbkess
ms.assetid: 52DFC191-E094-4B04-893F-B64D5828A900
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: quickstart
ms.date: 01/26/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: edd2a21b0fe49ca8e9792c7c512310339a822c55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-sql-data-warehouse"></a>Bevezetés az SQL Data Warehouse használatába

Ez az oktatóanyag bemutatja, hogyan tooprovision és az adatok betöltése az Azure SQL Data Warehouse. Megismerheti a méretezés, szüneteltetése és hangolása hello alapjairól is. Amikor végzett, lesz, készen áll a tooquery lenniük, és megismerkedhet az adatraktár.

**Becsült idő toocomplete:** egy végpont oktatóanyag, amely körülbelül 30 percet toocomplete után hello Előfeltételek megfelel példakód azt. 

## <a name="prerequisites"></a>Előfeltételek

hello oktatóanyag feltételezi, hogy az SQL Data Warehouse alapvető fogalmak megismerése. A bevezetésért lásd: [Mi az az SQL Data Warehouse?](sql-data-warehouse-overview-what-is.md) 

### <a name="sign-up-for-microsoft-azure"></a>Regisztráció a Microsoft Azure-ban
Ha még nem rendelkezik egy Microsoft Azure-fiók, meg kell feliratkozott egy toouse toosign ezt a szolgáltatást. Ha már rendelkezik fiókkal, ezt a lépést kihagyhatja. 

1. Keresse meg a fiók lapok toohello [https://azure.microsoft.com/account/](https://azure.microsoft.com/account/)
2. Hozzon létre egy ingyenes Azure-fiókot, vagy vásároljon egy fiókot.
3. Hello utasítások

### <a name="install-appropriate-sql-client-drivers-and-tools"></a>A megfelelő SQL-ügyfélillesztők és -ügyféleszközök telepítése

A legtöbb SQL-ügyféleszközöket JDBC, ODBC vagy az ADO.NET használatával képes kapcsolódni tooSQL Data warehouse-bA. Lejáró toohello nagy mennyiségű, amely támogatja az SQL Data Warehouse T-SQL funkciókat néhány ügyfélalkalmazások nincsenek teljesen kompatibilis, az SQL Data Warehouse szolgáltatással.

Ha Windows operációs rendszert használ, javasoljuk a [Visual Studio] vagy az [SQL Server Management Studio] használatát.

[!INCLUDE [Create a new logical server](../../includes/sql-data-warehouse-create-logical-server.md)] 

[!INCLUDE [SQL Database create server](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="create-a-sql-data-warehouse"></a>SQL Data Warehouse létrehozása

Az SQL Data Warehouse egy nagymértékben párhuzamos feldolgozáshoz kialakított speciális típusú adatbázis. hello adatbázis több csomópont között van elosztva, és feldolgozza a párhuzamos lekérdezések. Az SQL Data Warehouse van az összes hello csomópontjának hello tevékenységek vezénylő vezérlő csomópont. hello csomópontok magukat az adatokat SQL-adatbázis toomanage használni.  

> [!NOTE]
> Egy SQL Data Warehouse létrehozása egy új számlázható szolgáltatás létrejöttét eredményezheti.  További információ: [SQL Data Warehouse díjszabása](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).
>

### <a name="create-a-data-warehouse"></a>Adattárház létrehozása

1. Jelentkezzen be a hello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **New** > **Databases** > **SQL Data Warehouse** (Új > Adatbázisok > SQL Data Warehouse) elemre.

    ![NewBlade](../../includes/media/sql-data-warehouse-create-dw/blade-click-new.png)![SelectDW](../../includes/media/sql-data-warehouse-create-dw/blade-select-dw.png)

3. Adja meg az üzembe helyezés részleteit

    **Database name** (Adatbázis neve): Tetszés szerint bármit megadhat. Ha több adatraktárak, célszerű a nevek a következők részletekről, mint a hello régió, például a környezet *mydw-westus-1-teszt*.

    **Subscription** (Előfizetés): Az Ön Azure-előfizetése

    **Erőforráscsoport**: Hozzon létre egy erőforráscsoportot, vagy használjon egy meglévőt.
    > [!NOTE]
    > Az erőforráscsoportok hasznosak az erőforrások adminisztrációja, például a hozzáférés-vezérlés körének meghatározása és a sablonalapú üzembe helyezés során. Tudjon meg többet az Azure erőforráscsoportjairól és az ajánlott eljárásokról [itt](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups).

    **Forrás**: Üres adatbázis

    **Kiszolgáló**: Select hello server létrehozott [Előfeltételek].

    **Rendezés**: hello alapértelmezett rendezést SQL_Latin1_General_CP1_CI_AS hagyja.

    **Válassza ki a teljesítmény**: hello szabványos 400DWU kezdődően ajánlott.

4. Válasszon **PIN-kód toodashboard** ![PIN-kód tooDashboard](./media/sql-data-warehouse-get-started-tutorial/pin-to-dashboard.png)

5. Elhelyezkedik vissza, és várja meg a data warehouse toodeploy! A folyamat tootake normális néhány percig. hello portál értesíti, ha az adatraktár készen toouse. 

## <a name="connect-toosql-data-warehouse"></a>Csatlakozás az adatraktár tooSQL

Ez az oktatóanyag az SQL Server Management Studio (SSMS) tooconnect toohello adatraktár használja. A támogatott összekötők keresztül kapcsolódhatnak a Data Warehouse tooSQL: ADO.NET, JDBC, ODBC és a PHP. Ne feledje, hogy a Microsoft által nem támogatott eszközök esetében a funkcionalitás korlátozott lehet.


### <a name="get-connection-information"></a>Kapcsolatadatok lekérése

tooconnect tooyour adatraktár kell hello logikai SQL-kiszolgálón keresztül létrehozott tooconnect [Előfeltételek].

1. Válassza ki az adatraktár hello irányítópult vagy keresse meg azt a erőforrásokban.

    ![SQL Data Warehouse irányítópult](./media/sql-data-warehouse-get-started-tutorial/sql-dw-dashboard.png)

2. Található hello hello logikai SQL server teljes nevét.

    ![Kiszolgálónév kiválasztása](./media/sql-data-warehouse-get-started-tutorial/select-server.png)

3. Nyissa meg a szolgáltatáshoz az SSMS, object explorer tooconnect toothis kiszolgáló létrehozott hello server rendszergazdai hitelesítő adataival [Előfeltételek]

    ![Csatlakozás SSMS segítségével](./media/sql-data-warehouse-get-started-tutorial/ssms-connect.png)

Ha minden megfelelően megfelelően, kell csatlakoztatott tooyour logikai SQL-kiszolgáló. Mivel Ön bejelentkezett, a kiszolgáló rendszergazdája hello, hello kiszolgáló, többek között a master adatbázis hello által üzemeltetett tooany adatbázis is elérheti. 

Csak egy kiszolgáló-rendszergazdai fiókot, és minden olyan felhasználó, a legtöbb jogosultságával rendelkezik hello. Legyen óvatos nem tooallow a szervezet tooknow hello rendszergazdai jelszó túl sokan. 

Emellett rendelkezhet egy Azure Active Directory-rendszergazdai fiókkal is. A Microsoft hello részleteit itt nem ad meg. Ha azt szeretné, hogy Azure Active Directory-hitelesítés használatával kapcsolatos további toolearn, [az Azure AD-alapú hitelesítés](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication).

Ezután a további bejelentkezések és felhasználók létrehozásával ismerkedünk meg.


## <a name="create-a-database-user"></a>Adatbázis-felhasználó létrehozása

Ebben a lépésben létrehoz egy felhasználói fiók tooaccess az adatraktár. Azt is bemutatja, hogyan toogive adott felhasználó hello képességét toorun lekérdezi a nagy mennyiségű memória és CPU-erőforrást.

### <a name="notes-about-resource-classes-for-allocating-resources-tooqueries"></a>Erőforrás-osztályok a erőforrások tooqueries lefoglalásával kapcsolatos megjegyzések

- tookeep biztonságos, az adatok az éles adatbázist ne használjon hello server admin toorun lekérdezések. Hello minden olyan felhasználó, a legtöbb jogosultságával rendelkezik, és használja azt tooperform műveleteket a felhasználói adatok helyezi az adatok veszélyben. Is célja, hogy hello kiszolgálói rendszergazda tooperform kezelési műveletek, mert azt fut műveletek csak kis lefoglalása a memória és CPU-erőforrást. 

- Az SQL Data Warehouse előre meghatározott adatbázis-szerepkörök, erőforrás-osztályok, a memória, Processzor-erőforrások és feldolgozási üzembe helyezési ponti toousers különböző mennyiségű tooallocate nevű használja. Minden felhasználó tooa kis, közepes, nagy méretű vagy extra nagy erőforrásosztály is tartozhatnak. hello felhasználó erőforrásosztály meghatározza, hogy hello erőforrások hello felhasználó rendelkezik-e toorun lekérdezések és műveletek betölteni.

- Optimális adattömörítést hello felhasználói kell nagy tooload vagy extra nagy erőforrás-hozzárendelések. További információkat az erőforrásosztályokról [itt](./sql-data-warehouse-develop-concurrency.md#resource-classes) talál.

### <a name="create-an-account-that-can-control-a-database"></a>Az adatbázisok vezérlésére alkalmas fiók létrehozása

Mivel a kiszolgáló rendszergazdája hello naplózza, hogy engedélyeket toocreate bejelentkezéseket és a felhasználók.

1. Az SSMS vagy más lekérdezésügyfél használatával nyisson egy új lekérdezést a **masteren**.

    ![Új lekérdezés a Master adatbázison](./media/sql-data-warehouse-get-started-tutorial/query-on-server.png)

    ![Új lekérdezés a Master1 adatbázison](./media/sql-data-warehouse-get-started-tutorial/query-on-master.png)

2. Hello lekérdezési ablakban futtassa a T-SQL-parancs toocreate MedRCLogin nevű bejelentkezési azonosítót és LoadingUser nevű felhasználót. Ehhez a bejelentkezéshez toohello logikai SQL server is elérheti.

    ```sql
    CREATE LOGIN MedRCLogin WITH PASSWORD = 'a123reallySTRONGpassword!';
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

3. Most lekérdezése hello *SQL Data Warehouse-adatbázis*, hozzon létre egy adatbázis-felhasználó alapú tooaccess létrehozott bejelentkezési hello és műveleteket hajtson végre a hello adatbázis.

    ```sql
    CREATE USER LoadingUser FOR LOGIN MedRCLogin;
    ```

4. Adjon hello adatbázis felhasználói vezérlő engedélyek toohello adatbázisnak NYT nevezik. 

    ```sql
    GRANT CONTROL ON DATABASE::[NYT] tooLoadingUser;
    ```
    > [!NOTE]
    > Az adatbázisnév kötőjeleket rendelkezik, ha kell, hogy toowrap azt szögletes zárójelbe! 
    >

### <a name="give-hello-user-medium-resource-allocations"></a>Adjon hello felhasználói közepes erőforrás-hozzárendelések

1. Futtassa a T-SQL-parancs toomake nevezik mediumrc hello közepes erőforrás osztály tagja egy informatikai. 

    ```sql
    EXEC sp_addrolemember 'mediumrc', 'LoadingUser';
    ```
    > [!NOTE]
    > Kattintson a [Itt](sql-data-warehouse-develop-concurrency.md#resource-classes) feldolgozási és erőforrás-osztályok kapcsolatos további toolearn! 
    >

2. Csatlakoztassa a toohello logikai kiszolgálót hello új hitelesítő adatokkal

    ![Bejelentkezés az új bejelentkezési adatokkal](./media/sql-data-warehouse-get-started-tutorial/new-login.png)


## <a name="load-data-from-azure-blob-storage"></a>Adatok betöltése az Azure Blob Storage-ből

Most már áll készen tooload adatok az a data warehouse-bA. Ez a lépés bemutatja, hogyan tooload New York Város taxi cab adatait egy nyilvános Azure storage blob-e. 

- Közös úgy tooload adatokat az SQL Data Warehouse toofirst hello adatok tooAzure blob-tároló áthelyezéséhez, és ezután töltse be az adatraktár. toomake azt könnyebb toounderstand hogyan tooload, tudunk Győr taxi cab adatok már található egy nyilvános Azure storage-blobba. 

- Későbbi használatra toolearn hogyan tooget az adatok tooAzure blob-tároló vagy tooload azt közvetlenül a forráskiszolgálón az SQL Data Warehouse, lásd: hello [betöltést áttekintő](sql-data-warehouse-overview-load.md).


### <a name="define-external-data"></a>Külső adatok meghatározása

1. Hozzon létre egy főkulcsot. Csak egyszer adatbázisonként főkulcs toocreate kell. 

    ```sql
    CREATE MASTER KEY;
    ```

2. Adja meg a hello Azure blob hello taxi cab-adatokat tartalmazó hello helyét.  

    ```sql
    CREATE EXTERNAL DATA SOURCE NYTPublic
    WITH
    (
        TYPE = Hadoop,
        LOCATION = 'wasbs://2013@nytpublic.blob.core.windows.net/'
    );
    ```

3. Adja meg a hello külső fájlformátum

    Hello ```CREATE EXTERNAL FILE FORMAT``` parancs használt toospecify hello külső adatokat tartalmazó fájlok formátumban. Ezek egy vagy több karakter, az úgynevezett elválasztó karakterek használatával vannak elválasztva. Bemutatási célokra hello taxi cab-fájl tárolja tömörítetlen adatokhoz és gzip tömörített adatként történjen.

    T-SQL parancsot futtatva toodefine két különböző formátumokban: tömörítetlenül és tömörített.

    ```sql
    CREATE EXTERNAL FILE FORMAT uncompressedcsv
    WITH (
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( 
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '',
            DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        )
    );

    CREATE EXTERNAL FILE FORMAT compressedcsv
    WITH ( 
        FORMAT_TYPE = DELIMITEDTEXT,
        FORMAT_OPTIONS ( FIELD_TERMINATOR = '|',
            STRING_DELIMITER = '',
        DATE_FORMAT = '',
            USE_TYPE_DEFAULT = False
        ),
        DATA_COMPRESSION = 'org.apache.hadoop.io.compress.GzipCodec'
    );
    ```

4.  Hozzon létre egy sémát a külső fájlformátum számára. 

    ```sql
    CREATE SCHEMA ext;
    ```
5. Hello külső táblák létrehozása. Ezek a táblák az Azure Blob Storage-ben tárolt adatokra hivatkoznak. Futtassa a következő T-SQL-parancsok toocreate hello több külső táblák, hogy minden pont toohello Azure blob meghatározott korábban a külső adatforrás.

```sql
    CREATE EXTERNAL TABLE [ext].[Date] 
    (
        [DateID] int NOT NULL,
        [Date] datetime NULL,
        [DateBKey] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DaySuffix] varchar(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeek] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInMonth] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfWeekInYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfQuarter] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DayOfYear] varchar(3) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfMonth] varchar(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [WeekOfYear] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Month] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthOfQuarter] varchar(2) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Quarter] char(1) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [QuarterName] varchar(9) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Year] char(4) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [YearName] char(7) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MonthYear] char(10) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [MMYYYY] char(6) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FirstDayOfMonth] date NULL,
        [LastDayOfMonth] date NULL,
        [FirstDayOfQuarter] date NULL,
        [LastDayOfQuarter] date NULL,
        [FirstDayOfYear] date NULL,
        [LastDayOfYear] date NULL,
        [IsHolidayUSA] bit NULL,
        [IsWeekday] bit NULL,
        [HolidayUSA] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Date',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    );
    
    CREATE EXTERNAL TABLE [ext].[Geography]
    (
        [GeographyID] int NOT NULL,
        [ZipCodeBKey] varchar(10) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [County] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [City] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [State] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [Country] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [ZipCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Geography',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0 
    );
        
    
    CREATE EXTERNAL TABLE [ext].[HackneyLicense]
    (
        [HackneyLicenseID] int NOT NULL,
        [HackneyLicenseBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HackneyLicenseCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'HackneyLicense',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    
    CREATE EXTERNAL TABLE [ext].[Medallion]
    (
        [MedallionID] int NOT NULL,
        [MedallionBKey] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [MedallionCode] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL
    )
    WITH
    (
        LOCATION = 'Medallion',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
        
    CREATE EXTERNAL TABLE [ext].[Time]
    (
        [TimeID] int NOT NULL,
        [TimeBKey] varchar(8) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [HourNumber] tinyint NOT NULL,
        [MinuteNumber] tinyint NOT NULL,
        [SecondNumber] tinyint NOT NULL,
        [TimeInSecond] int NOT NULL,
        [HourlyBucket] varchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL,
        [DayTimeBucketGroupKey] int NOT NULL,
        [DayTimeBucket] varchar(100) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL
    )
    WITH
    (
        LOCATION = 'Time',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    
    CREATE EXTERNAL TABLE [ext].[Trip]
    (
        [DateID] int NOT NULL,
        [MedallionID] int NOT NULL,
        [HackneyLicenseID] int NOT NULL,
        [PickupTimeID] int NOT NULL,
        [DropoffTimeID] int NOT NULL,
        [PickupGeographyID] int NULL,
        [DropoffGeographyID] int NULL,
        [PickupLatitude] float NULL,
        [PickupLongitude] float NULL,
        [PickupLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [DropoffLatitude] float NULL,
        [DropoffLongitude] float NULL,
        [DropoffLatLong] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [PassengerCount] int NULL,
        [TripDurationSeconds] int NULL,
        [TripDistanceMiles] float NULL,
        [PaymentType] varchar(50) COLLATE SQL_Latin1_General_CP1_CI_AS NULL,
        [FareAmount] money NULL,
        [SurchargeAmount] money NULL,
        [TaxAmount] money NULL,
        [TipAmount] money NULL,
        [TollsAmount] money NULL,
        [TotalAmount] money NULL
    )
    WITH
    (
        LOCATION = 'Trip2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = compressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
    
    CREATE EXTERNAL TABLE [ext].[Weather]
    (
        [DateID] int NOT NULL,
        [GeographyID] int NOT NULL,
        [PrecipitationInches] float NOT NULL,
        [AvgTemperatureFahrenheit] float NOT NULL
    )
    WITH
    (
        LOCATION = 'Weather2013',
        DATA_SOURCE = NYTPublic,
        FILE_FORMAT = uncompressedcsv,
        REJECT_TYPE = value,
        REJECT_VALUE = 0
    )
    ;
```

### <a name="import-hello-data-from-azure-blob-storage"></a>Hello adatok importálása az Azure blob storage.

Az SQL Data Warehouse támogat egy CREATE TABLE AS SELECT (CTAS) nevű kulcsutasítást. A jelen nyilatkozat táblázatot hoz létre új hello eredmények select utasítás alapján. hello új táblának azonos oszlopok és adattípusok hello hello hello eredményeit válasszon ki az utasítást.  Ez az az Azure blob storage az SQL Data Warehouse egy elegáns módon tooimport adatokat.

1. Futtassa a parancsfájlt tooimport az adatok.

    ```sql
    CREATE TABLE [dbo].[Date]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Date]
    OPTION (LABEL = 'CTAS : Load [dbo].[Date]')
    ;
    
    CREATE TABLE [dbo].[Geography]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS
    SELECT * FROM [ext].[Geography]
    OPTION (LABEL = 'CTAS : Load [dbo].[Geography]')
    ;
    
    CREATE TABLE [dbo].[HackneyLicense]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[HackneyLicense]
    OPTION (LABEL = 'CTAS : Load [dbo].[HackneyLicense]')
    ;
    
    CREATE TABLE [dbo].[Medallion]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Medallion]
    OPTION (LABEL = 'CTAS : Load [dbo].[Medallion]')
    ;
    
    CREATE TABLE [dbo].[Time]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Time]
    OPTION (LABEL = 'CTAS : Load [dbo].[Time]')
    ;
    
    CREATE TABLE [dbo].[Weather]
    WITH
    ( 
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Weather]
    OPTION (LABEL = 'CTAS : Load [dbo].[Weather]')
    ;
    
    CREATE TABLE [dbo].[Trip]
    WITH
    (
        DISTRIBUTION = ROUND_ROBIN,
        CLUSTERED COLUMNSTORE INDEX
    )
    AS SELECT * FROM [ext].[Trip]
    OPTION (LABEL = 'CTAS : Load [dbo].[Trip]')
    ;
    ```

2. A betöltés közben megtekintheti az adatokat.

   Több GB-nyi adatot tölt be és tömörít nagy teljesítményű fürtözött oszlopcentrikus indexekbe. Futtassa a következő lekérdezés hello használó hello terhelés a dinamikus felügyeleti nézetek (dinamikus felügyeleti nézetek) tooshow hello állapota. Hello lekérdezés indítás után adása a kávé és egy Rögbi SQL Data Warehouse azonban néhány gyakori emelő.
    
    ```sql
    SELECT
        r.command,
        s.request_id,
        r.status,
        count(distinct input_name) as nbr_files,
        sum(s.bytes_processed)/1024/1024/1024 as gb_processed
    FROM 
        sys.dm_pdw_exec_requests r
        INNER JOIN sys.dm_pdw_dms_external_work s
        ON r.request_id = s.request_id
    WHERE
        r.[label] = 'CTAS : Load [dbo].[Date]' OR
        r.[label] = 'CTAS : Load [dbo].[Geography]' OR
        r.[label] = 'CTAS : Load [dbo].[HackneyLicense]' OR
        r.[label] = 'CTAS : Load [dbo].[Medallion]' OR
        r.[label] = 'CTAS : Load [dbo].[Time]' OR
        r.[label] = 'CTAS : Load [dbo].[Weather]' OR
        r.[label] = 'CTAS : Load [dbo].[Trip]'
    GROUP BY
        r.command,
        s.request_id,
        r.status
    ORDER BY
        nbr_files desc, 
        gb_processed desc;
    ```

3. Tekintse meg az összes rendszerlekérdezést.

    ```sql
    SELECT * FROM sys.dm_pdw_exec_requests;
    ```

4. Láthatja, ahogy adatai szépen betöltődnek az Azure SQL Data Warehouse-ba.

    ![Betöltött adatok megjelenítése](./media/sql-data-warehouse-get-started-tutorial/see-data-loaded.png)


## <a name="improve-query-performance"></a>Jobb lekérdezési teljesítmény

Számos módon tooimprove lekérdezési teljesítményt, és tooachieve hello nagy sebességű teljesítményéről, amely az SQL Data warehouse tooprovide tervezték.  

### <a name="see-hello-effect-of-scaling-on-query-performance"></a>Tekintse meg a lekérdezési teljesítmény méretezésének hello hatása 

Egyirányú tooimprove lekérdezési teljesítmény tooscale erőforrások hello DWU szolgáltatási szint az adatraktár módosításával. Minden szolgáltatási szintnek nagyobb a költsége, de bármikor visszaválthat vagy szüneteltetheti az erőforrásokat. 

Ebben a lépésben összehasonlítja a teljesítményt két különböző DWU-beállításnál.

Első, vertikálisan skálázzunk hello méretezési DWU, azt is képet kapjon a egy számítási csomópont lehet végre önállóan too100 le.

1. Nyissa meg toohello portálon, és válassza az SQL Data Warehouse.

2. Válassza ki a skála hello SQL Data Warehouse panelre. 

    ![DW méretezése a portálról](./media/sql-data-warehouse-get-started-tutorial/scale-dw.png)

3. Eszközterület-too100 DWU hello teljesítmény csökkentheti, és kattintson a mentés.

    ![Méretezés és mentés](./media/sql-data-warehouse-get-started-tutorial/scale-and-save.png)

4. Várjon, amíg a méretezési művelet toofinish.

    > [!NOTE]
    > Lekérdezések hello méretezési módosítása közben nem futtatható. A méretezés az épp futó lekérdezéseket **megszakítja**. Újraindításukra hello művelet befejezésekor.
    >
    
5. Hajtsa végre a vizsgálati művelet hello út adatokon, felső millió bejegyzések hello összes hello oszlop kiválasztása. Ha a számítógép különösen toomove gyorsan, érzi, hogy szabad tooselect kevesebb sort. Jegyezze fel a hello időt toorun ezt a műveletet.

    ```sql
    SELECT TOP(1000000) * FROM dbo.[Trip]
    ```
6. Az adatraktár méretezhető biztonsági too400 DWU. Ne feledje, hogy minden 100 DWU egy másik számítási csomópont tooyour Azure SQL Data Warehouse hozzáadásával.

7. Futtassa újra a hello lekérdezés! Jelentős eltérést kell tapasztalnia. 

    > [!NOTE]
    > Hello lekérdezés nagy mennyiségű adatot ad vissza, mert a hello számítógépen, amelyen SSMS hello sávszélesség rendelkezésre állását a teljesítménybeli szűk keresztmetszetek lehet. Emiatt lehetséges, hogy semmilyen teljesítménybeli javulást nem fog tapasztalni.

> [!NOTE]
> Ennek az az oka, hogy az SQL Data Warehouse nagymértékben párhuzamos feldolgozást használ. Ellenőrzési és analitikai funkciók végrehajtása több millió sort lekérdezések hello igaz hatványra emelésének Azure SQL Data Warehouse tapasztalhat.
>

### <a name="see-hello-effect-of-statistics-on-query-performance"></a>Statisztika hello hatásának tekintse meg a lekérdezési teljesítmény

1. Hogy illesztések hello hello út táblával dátumtáblázat-lekérdezés futtatható

    ```sql
    SELECT TOP (1000000) 
        dt.[DayOfWeek],
        tr.[MedallionID],
        tr.[HackneyLicenseID],
        tr.[PickupTimeID],
        tr.[DropoffTimeID],
        tr.[PickupGeographyID],
        tr.[DropoffGeographyID],
        tr.[PickupLatitude],
        tr.[PickupLongitude],
        tr.[PickupLatLong],
        tr.[DropoffLatitude],
        tr.[DropoffLongitude],
        tr.[DropoffLatLong],
        tr.[PassengerCount],
        tr.[TripDurationSeconds],
        tr.[TripDistanceMiles],
        tr.[PaymentType],
        tr.[FareAmount],
        tr.[SurchargeAmount],
        tr.[TaxAmount],
        tr.[TipAmount],
        tr.[TollsAmount],
        tr.[TotalAmount]
    FROM [dbo].[Trip] as tr
        JOIN dbo.[Date] as dt
        ON  tr.DateID = dt.DateID
    ```

    Ez a lekérdezés eltart egy ideig, mert az SQL Data Warehouse tooshuffle adatok előtt hello illesztési műveleteket hajthat végre. Illesztések ne legyen tooshuffle adatforrás, ha azok hello tervezett toojoin adatok ugyanúgy terjesztése történik. Ez egy mélyebb téma. 

2. A statisztika sokat számít. 
3. Futtassa a utasítás toocreate statisztika hello illesztési oszlop.

    ```sql
    CREATE STATISTICS [dbo.Date DateID stats] ON dbo.Date (DateID);
    CREATE STATISTICS [dbo.Trip DateID stats] ON dbo.Trip (DateID);
    ```

    > [!NOTE]
    > Az SQL DW nem kezeli automatikusan a statisztikákat Ön helyett. A statisztikák fontosak a lekérdezések teljesítménye szempontjából, ezért határozottan javasoljuk, hogy hozzon létre statisztikákat, és frissítse azokat.
    > 
    > **Ettől kezdve hello legtöbb juttatás azzal, hogy a statisztika oszlopokon érintett illesztésekben, oszlopok használt hello a GROUP BY záradék és az oszlopok találhatók.**
    >

3. Újra az Előfeltételek hello lekérdezés futtatása, és tekintse meg az összes teljesítmény különbséget. A lekérdezések teljesítményét hello különbségek nem lesz, mint a vertikális felskálázásával drasztikus, egy számlázhasson kell észlel. 

## <a name="next-steps"></a>Következő lépések

Most már készen áll a tooquery, és részletesen. Tekintse meg gyakorlati tanácsainkat és tippjeinket.

Ha végzett feltárása hello nap, győződjön meg arról, hogy toopause példány! Éles, máris elkezdheti felfedezni hatalmas megtakarítások felfüggesztése és a méretezésről toomeet által az üzleti igényeknek megfelelően.

![Szünet](./media/sql-data-warehouse-get-started-tutorial/pause.png)

## <a name="useful-readings"></a>Hasznos olvasmányok

[Egyidejűség és a számítási feladatok kezelése][]

[Ajánlott eljárások az Azure SQL Data Warehouse-hoz][]

[Lekérdezések figyelése][]

[A 10 leghasznosabb ajánlott eljárás nagyméretű relációs adattárházak létrehozásához][]

[Áttelepítési adatok tooAzure SQL Data Warehouse][]

[Egyidejűség és a számítási feladatok kezelése]: sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example
[Ajánlott eljárások az Azure SQL Data Warehouse-hoz]: sql-data-warehouse-best-practices.md#hash-distribute-large-tables
[Lekérdezések figyelése]: sql-data-warehouse-manage-monitor.md
[A 10 leghasznosabb ajánlott eljárás nagyméretű relációs adattárházak létrehozásához]: https://blogs.msdn.microsoft.com/sqlcat/2013/09/16/top-10-best-practices-for-building-a-large-scale-relational-data-warehouse/
[Áttelepítési adatok tooAzure SQL Data Warehouse]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/



[!INCLUDE [Additional Resources](../../includes/sql-data-warehouse-article-footer.md)]

<!-- Internal Links -->
[Előfeltételek]: sql-data-warehouse-get-started-tutorial.md#prerequisites

<!--Other Web references-->
[Visual Studio]: https://www.visualstudio.com/
[SQL Server Management Studio]: https://msdn.microsoft.com/en-us/library/mt238290.aspx
