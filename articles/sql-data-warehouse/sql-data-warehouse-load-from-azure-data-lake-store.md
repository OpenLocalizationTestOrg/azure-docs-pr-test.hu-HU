---
title: "aaaLoad - Azure Data Lake Store tooSQL adatraktár |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse PolyBase külső táblák az Azure SQL Data Warehouse az Azure Data Lake Store tooload adatait."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 01/25/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: 50ef23b3eba5f58bc9974095f84140dc5c11fa4b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-azure-data-lake-store-into-sql-data-warehouse"></a>Adatok betöltése az Azure Data Lake Store az SQL Data Warehouse
Ez a dokumentum lehetővé teszi az összes szükséges lépést tooload a saját adatait az Azure Data Lake Store-(ADLS-) az SQL Data Warehouse PolyBase használatával.
Amíg hello adatok tárolva ADLS hello külső táblák segítségével képes toorun ad hoc lekérdezéseket, ajánlott eljárásként javasoljuk, hogy hello adatok importálása az SQL Data Warehouse hello.
Becsült idő: 10 percig, feltéve, hogy hello Előfeltételek toocomplete kell.
Ebből az oktatóanyagból megtudhatja, hogyan:

1. Külső adatbázis objektumok tooload létrehozása az Azure Data Lake Store.
2. Csatlakozzon az Azure Data Lake Store könyvtárának tooan.
3. Adatok betöltése az Azure SQL Data warehouse-bA.

## <a name="before-you-begin"></a>Előkészületek
toorun ebben az oktatóanyagban szüksége:

* Az Azure Active Directory-alkalmazás toouse szolgáltatások hitelesítéshez. toocreate, hajtsa végre a [Active directory-hitelesítés](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)

>[!NOTE] 
> Hello ügyfél-azonosító, kulcs és az Active Directory-alkalmazás tooconnect tooyour Azure Data Lake az SQL Data Warehouse OAuth2.0 Token Endpoint értéke van szüksége. Hogyan tooget ezek az értékek szerepelnek hello hivatkozásra a fenti részleteit.
>Megjegyzés: az Azure Active Directory-alkalmazás regisztráció "Alkalmazásazonosítót" hello használata hello ügyfél-azonosítót.

* SQL Server Management Studio vagy SQL Server Data Tools összetevővel, toodownload SSMS és lásd: Csatlakozás [lekérdezés SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)

* Egy Azure SQL Data Warehouse egy toocreate kövesse: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision

* Egy Azure Data Lake Store, vagy a engedélyezhető a titkosítás nélkül. toocreate egy kövesse: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal




## <a name="configure-hello-data-source"></a>Hello adatforrás konfigurálása
A polybase T-SQL külső objektumok toodefine hello helyét és hello külső adatokra vonatkozó attribútumok. külső objektumok hello th külsőleg tárolja az SQL Data Warehouse- és referenciainformációkat hello adatok tárolódnak.


###  <a name="create-a-credential"></a>Hitelesítő adatok létrehozása
tooaccess az Azure Data Lake tárolására, szüksége lesz egy Adatbázisfőkulcs tooencrypt toocreate hello következő lépésben használt hitelesítő adatok titkos.
Ezután hozzon létre egy adatbázishoz kötődő hitelesítő adat, amely tárolja a hello szolgáltatás egyszerű hitelesítő adatok beállítása az aad-ben. Azok is, akik használta a PolyBase tooconnect tooWindows Azure Storage Blobs, vegye figyelembe, hogy hello hitelesítő adat szintaxis eltér.
tooconnect tooAzure Data Lake Store, kell **első** hozzon létre egy Azure Active Directory-alkalmazást, a hozzáférési kulcs létrehozása, és a hello alkalmazás hozzáférés toohello Azure Data Lake resource biztosítani. Instrucitons tooperform ezeket a lépéseket találhatók [Itt](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required tooencrypt hello credential secret in hello next step.
-- For more information on Master Key: https://msdn.microsoft.com/en-us/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Pass hello client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://msdn.microsoft.com/en-us/library/mt270260.aspx

CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- It should look something like this:
CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```


### <a name="create-hello-external-data-source"></a>Hello külső adatforrás létrehozása
Ezzel [külső ADATFORRÁS létrehozása] [ CREATE EXTERNAL DATA SOURCE] toostore hello helyét, valamint hello adatok hello típusú adatok parancsot.
Hello ADL URI hello Azure-portálon és www.portal.azure.com találja meg.

```sql
-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs tooaccess data in Azure Data Lake Store.
-- LOCATION: Provide Azure Data Lake accountname and URI
-- CREDENTIAL: Provide hello credential created in hello previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<AzureDataLake account_name>.azuredatalakestore.net',
    CREDENTIAL = ADLCredential
);
```



## <a name="configure-data-format"></a>Az adatformátum konfigurálása
ADLS tooimport hello adatait toospecify hello külső fájlformátumot kell. Ez a parancs rendelkezik formátummal kapcsolatos beállítások toodescribe adatait.
Alább példája egy gyakran használt fájlformátum cső tagolt szövegfájl.
Tekintse át a T-SQL dokumentációját teljes listáját [külső FÁJLFORMÁTUM létrehozása][CREATE EXTERNAL FILE FORMAT]

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks hello end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies hello field terminator for data of type string in hello text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store all Missing values as NULL

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-hello-external-tables"></a>Hello külső táblák létrehozása
Most, hogy a megadott hello forrás- és a fájl formátuma, készen áll a toocreate hello külső táblák áll. Külső táblák, hogyan működnek együtt a külső adatforráshoz. A polybase rekurzív directory átjárás tooread hello hely paraméterben megadott hello könyvtár alkönyvtáraiban található összes fájl. Is hello a következő példa bemutatja, hogyan toocreate hello objektum. Toocustomize hello utasítás toowork hello adatokkal ADLS van szüksége.

```sql
-- D: Create an External Table
-- LOCATION: Folder under hello ADLS root folder.
-- DATA_SOURCE: Specifies which Data Source Object toouse.
-- FILE_FORMAT: Specifies which File Format Object toouse
-- REJECT_TYPE: Specifies how you want toodeal with rejected rows. Either Value or percentage of hello total
-- REJECT_VALUE: Sets hello Reject value based on hello reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStore
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>A külső tábla kapcsolatos szempontok
A külső tábla létrehozása egyszerű, de van néhány apró tárgyalt toobe igénylő.

A polybase-zel adatok betöltése erős típusmegadású. Ez azt jelenti, hogy minden egyes sorára alatt okozhatnak hello adatok hello tábla sémadefiníciója meg kell felelniük.
Ha egy adott sor nem egyezik meg a hello sémadefiníciót, hello sor hello terhelés fogja elutasítani.

hello REJECT_TYPE és REJECT_VALUE beállítások lehetővé teszik toodefine hány sort vagy hello adatok hány százaléka hello végső tábla jelen kell lennie.
Betöltéskor hello utasítsa el az érték elérésekor, hello betöltése sikertelen. Elutasított sorok hello leggyakoribb oka, hogy séma definíciója nem egyeznek.
Például egy olyan oszlop helytelenül int hello sémája nincs megadva, ha hello adatok hello fájlban egy karakterlánc, minden sor tooload meghiúsul.

hello helye hello legfelső directory tooread adatait kívánt határozza meg.
Ebben az esetben volt /DimProduct/ PolyBase alkönyvtárába hello alkönyvtárak belül minden hello adatok importálásától.

## <a name="load-hello-data"></a>Hello adatok betöltése
Azure Data Lake Store tooload adatait használja hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] utasítást. A CTAS betöltése használ hello erős típusmegadású hozott létre a külső tábla.

CTAS új táblát hoz létre, és feltölti a select utasítás hello eredményekkel. CTAS meghatározása hello új tábla toohave azonos oszlopok és adattípusok hello hello hello eredményeit válasszon ki az utasítást. Ha minden hello oszlop egy külső tábla, hello új tábla hello oszlopok és típusok adatok replikáját hello külső tábla.

Ebben a példában létrehozzuk elosztott kivonattáblát a külső tábla DimProduct_external DimProduct hívása.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a>Oszlopcentrikus tömörítés optimalizálása
Alapértelmezés szerint az SQL Data Warehouse tárolja hello tábla fürtözött oszlopcentrikus index. A betöltés befejezése után néhány hello adatsorok előfordulhat, hogy nem lehet tömörített hello oszlopcentrikus.  Miért Ez akkor fordulhat elő, ennek több van. több, lásd: toolearn [oszlopcentrikus Indexek kezelése][manage columnstore indexes].

toooptimize lekérdezési teljesítmény és a betöltés után oszlopcentrikus tömörítési építse újra hello tábla tooforce hello oszlopcentrikus index toocompress hello összes sort.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

Az oszlopcentrikus indexek fenntartása További információkért lásd: hello [oszlopcentrikus Indexek kezelése] [ manage columnstore indexes] cikk.

## <a name="optimize-statistics"></a>Statisztika optimalizálása
A betöltés után azonnal is ajánlott toocreate egyoszlopos statisztikát. Nincsenek néhány statisztikai lehetőségeit. Például ha egyoszlopos statisztikát hoz létre minden egyes oszlophoz eltarthat egy hosszú ideig toorebuild összes hello statisztikáját. Ha ismeri az egyes oszlopok nem fog az lekérdezés predikátumokban toobe, kihagyhatja a statisztikák létrehozása az ilyen oszlopokat.

Ha úgy dönt, hogy minden tábla minden egyes oszlophoz a toocreate egyoszlopos statisztikát, használhatja a hello tárolt eljárás kódminta `prc_sqldw_create_stats` a hello [statisztika] [ statistics] cikk.

a következő példa hello statisztikák létrehozása az jó kiindulási pont. Egyoszlopos statisztikát hoz hello dimenzió tábla összes oszlopa, és minden hello ténytáblák csatlakozó oszlopában. Bármikor hozzáadhat egy vagy több oszlop statisztika tooother tény táblaoszlopok később.


## <a name="achievement-unlocked"></a>Zárolása feloldva elérésének!
Az Azure SQL Data Warehouse sikeresen betöltött adatok. Remek munka!

##<a name="next-steps"></a>Következő lépések
Adatok betöltése az hello első lépés toodeveloping az SQL Data Warehouse data warehouse megoldást. Tekintse meg a fejlesztői erőforrások a [táblák](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) és [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).


<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[manage columnstore indexes]: sql-data-warehouse-tables-index.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load hello full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
