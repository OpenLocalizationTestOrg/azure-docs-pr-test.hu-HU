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
# <a name="load-data-from-azure-data-lake-store-into-sql-data-warehouse"></a><span data-ttu-id="58999-103">Adatok betöltése az Azure Data Lake Store az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="58999-103">Load data from Azure Data Lake Store into SQL Data Warehouse</span></span>
<span data-ttu-id="58999-104">Ez a dokumentum lehetővé teszi az összes szükséges lépést tooload a saját adatait az Azure Data Lake Store-(ADLS-) az SQL Data Warehouse PolyBase használatával.</span><span class="sxs-lookup"><span data-stu-id="58999-104">This document gives you all steps you  need tooload your own data from Azure Data Lake Store (ADLS) into SQL Data Warehouse using PolyBase.</span></span>
<span data-ttu-id="58999-105">Amíg hello adatok tárolva ADLS hello külső táblák segítségével képes toorun ad hoc lekérdezéseket, ajánlott eljárásként javasoljuk, hogy hello adatok importálása az SQL Data Warehouse hello.</span><span class="sxs-lookup"><span data-stu-id="58999-105">While you are able toorun adhoc queries over hello data stored in ADLS using hello External Tables, as a best practice we suggest importing hello data into hello SQL Data Warehouse.</span></span>
<span data-ttu-id="58999-106">Becsült idő: 10 percig, feltéve, hogy hello Előfeltételek toocomplete kell.</span><span class="sxs-lookup"><span data-stu-id="58999-106">Time Estimate: 10 minutes assuming you have hello prerequisites need toocomplete.</span></span>
<span data-ttu-id="58999-107">Ebből az oktatóanyagból megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="58999-107">In this tutorial you will learn how to:</span></span>

1. <span data-ttu-id="58999-108">Külső adatbázis objektumok tooload létrehozása az Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="58999-108">Create External Database objects tooload from Azure Data Lake Store.</span></span>
2. <span data-ttu-id="58999-109">Csatlakozzon az Azure Data Lake Store könyvtárának tooan.</span><span class="sxs-lookup"><span data-stu-id="58999-109">Connect tooan Azure Data Lake Store Directory.</span></span>
3. <span data-ttu-id="58999-110">Adatok betöltése az Azure SQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="58999-110">Load data into Azure SQL Data Warehouse.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="58999-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="58999-111">Before you begin</span></span>
<span data-ttu-id="58999-112">toorun ebben az oktatóanyagban szüksége:</span><span class="sxs-lookup"><span data-stu-id="58999-112">toorun this tutorial, you need:</span></span>

* <span data-ttu-id="58999-113">Az Azure Active Directory-alkalmazás toouse szolgáltatások hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="58999-113">Azure Active Directory Application toouse for Service-to-Service authentication.</span></span> <span data-ttu-id="58999-114">toocreate, hajtsa végre a [Active directory-hitelesítés](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)</span><span class="sxs-lookup"><span data-stu-id="58999-114">toocreate, follow [Active directory authentication](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)</span></span>

>[!NOTE] 
> <span data-ttu-id="58999-115">Hello ügyfél-azonosító, kulcs és az Active Directory-alkalmazás tooconnect tooyour Azure Data Lake az SQL Data Warehouse OAuth2.0 Token Endpoint értéke van szüksége.</span><span class="sxs-lookup"><span data-stu-id="58999-115">You need hello client ID, Key, and OAuth2.0 Token Endpoint Value of your Active Directory Application tooconnect tooyour Azure Data Lake from SQL Data Warehouse.</span></span> <span data-ttu-id="58999-116">Hogyan tooget ezek az értékek szerepelnek hello hivatkozásra a fenti részleteit.</span><span class="sxs-lookup"><span data-stu-id="58999-116">Details for how tooget these values are in hello link above.</span></span>
><span data-ttu-id="58999-117">Megjegyzés: az Azure Active Directory-alkalmazás regisztráció "Alkalmazásazonosítót" hello használata hello ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="58999-117">Note for Azure Active Directory App Registration use hello 'Application ID' as hello Client ID.</span></span>

* <span data-ttu-id="58999-118">SQL Server Management Studio vagy SQL Server Data Tools összetevővel, toodownload SSMS és lásd: Csatlakozás [lekérdezés SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)</span><span class="sxs-lookup"><span data-stu-id="58999-118">SQL Server Management Studio or SQL Server Data Tools, toodownload SSMS and connect see [Query SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)</span></span>

* <span data-ttu-id="58999-119">Egy Azure SQL Data Warehouse egy toocreate kövesse: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision</span><span class="sxs-lookup"><span data-stu-id="58999-119">An Azure SQL Data Warehouse, toocreate one follow: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision</span></span>

* <span data-ttu-id="58999-120">Egy Azure Data Lake Store, vagy a engedélyezhető a titkosítás nélkül.</span><span class="sxs-lookup"><span data-stu-id="58999-120">An Azure Data Lake Store, with or without encryption enabled.</span></span> <span data-ttu-id="58999-121">toocreate egy kövesse: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal</span><span class="sxs-lookup"><span data-stu-id="58999-121">toocreate one follow: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal</span></span>




## <a name="configure-hello-data-source"></a><span data-ttu-id="58999-122">Hello adatforrás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="58999-122">Configure hello data source</span></span>
<span data-ttu-id="58999-123">A polybase T-SQL külső objektumok toodefine hello helyét és hello külső adatokra vonatkozó attribútumok.</span><span class="sxs-lookup"><span data-stu-id="58999-123">PolyBase uses T-SQL external objects toodefine hello location and attributes of hello external data.</span></span> <span data-ttu-id="58999-124">külső objektumok hello th külsőleg tárolja az SQL Data Warehouse- és referenciainformációkat hello adatok tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="58999-124">hello external objects are stored in SQL Data Warehouse and reference hello data th is stored externally.</span></span>


###  <a name="create-a-credential"></a><span data-ttu-id="58999-125">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="58999-125">Create a credential</span></span>
<span data-ttu-id="58999-126">tooaccess az Azure Data Lake tárolására, szüksége lesz egy Adatbázisfőkulcs tooencrypt toocreate hello következő lépésben használt hitelesítő adatok titkos.</span><span class="sxs-lookup"><span data-stu-id="58999-126">tooaccess your Azure Data Lake Store, you will need toocreate a Database Master Key tooencrypt your credential secret used in hello next step.</span></span>
<span data-ttu-id="58999-127">Ezután hozzon létre egy adatbázishoz kötődő hitelesítő adat, amely tárolja a hello szolgáltatás egyszerű hitelesítő adatok beállítása az aad-ben.</span><span class="sxs-lookup"><span data-stu-id="58999-127">You then create a Database scoped credential, which stores hello service principal credentials set up in AAD.</span></span> <span data-ttu-id="58999-128">Azok is, akik használta a PolyBase tooconnect tooWindows Azure Storage Blobs, vegye figyelembe, hogy hello hitelesítő adat szintaxis eltér.</span><span class="sxs-lookup"><span data-stu-id="58999-128">For those of you who have used PolyBase tooconnect tooWindows Azure Storage Blobs, note that hello credential syntax is different.</span></span>
<span data-ttu-id="58999-129">tooconnect tooAzure Data Lake Store, kell **első** hozzon létre egy Azure Active Directory-alkalmazást, a hozzáférési kulcs létrehozása, és a hello alkalmazás hozzáférés toohello Azure Data Lake resource biztosítani.</span><span class="sxs-lookup"><span data-stu-id="58999-129">tooconnect tooAzure Data Lake Store, you must **first** create an Azure Active Directory Application, create an access key, and grant hello application access toohello Azure Data Lake resource.</span></span> <span data-ttu-id="58999-130">Instrucitons tooperform ezeket a lépéseket találhatók [Itt](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).</span><span class="sxs-lookup"><span data-stu-id="58999-130">Instrucitons tooperform these steps are located [here](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).</span></span>

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


### <a name="create-hello-external-data-source"></a><span data-ttu-id="58999-131">Hello külső adatforrás létrehozása</span><span class="sxs-lookup"><span data-stu-id="58999-131">Create hello external data source</span></span>
<span data-ttu-id="58999-132">Ezzel [külső ADATFORRÁS létrehozása] [ CREATE EXTERNAL DATA SOURCE] toostore hello helyét, valamint hello adatok hello típusú adatok parancsot.</span><span class="sxs-lookup"><span data-stu-id="58999-132">Use this [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE] command toostore hello location of hello data, and hello type of data.</span></span>
<span data-ttu-id="58999-133">Hello ADL URI hello Azure-portálon és www.portal.azure.com találja meg.</span><span class="sxs-lookup"><span data-stu-id="58999-133">You can find hello ADL URI in hello Azure portal and www.portal.azure.com.</span></span>

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



## <a name="configure-data-format"></a><span data-ttu-id="58999-134">Az adatformátum konfigurálása</span><span class="sxs-lookup"><span data-stu-id="58999-134">Configure data format</span></span>
<span data-ttu-id="58999-135">ADLS tooimport hello adatait toospecify hello külső fájlformátumot kell.</span><span class="sxs-lookup"><span data-stu-id="58999-135">tooimport hello data from ADLS, you need toospecify hello external file format.</span></span> <span data-ttu-id="58999-136">Ez a parancs rendelkezik formátummal kapcsolatos beállítások toodescribe adatait.</span><span class="sxs-lookup"><span data-stu-id="58999-136">This command has format-specific options toodescribe your data.</span></span>
<span data-ttu-id="58999-137">Alább példája egy gyakran használt fájlformátum cső tagolt szövegfájl.</span><span class="sxs-lookup"><span data-stu-id="58999-137">Below is an example of a commonly used file format that is a pipe-delimited text file.</span></span>
<span data-ttu-id="58999-138">Tekintse át a T-SQL dokumentációját teljes listáját [külső FÁJLFORMÁTUM létrehozása][CREATE EXTERNAL FILE FORMAT]</span><span class="sxs-lookup"><span data-stu-id="58999-138">Look at our T-SQL documentation for a complete list of [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT]</span></span>

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

## <a name="create-hello-external-tables"></a><span data-ttu-id="58999-139">Hello külső táblák létrehozása</span><span class="sxs-lookup"><span data-stu-id="58999-139">Create hello external tables</span></span>
<span data-ttu-id="58999-140">Most, hogy a megadott hello forrás- és a fájl formátuma, készen áll a toocreate hello külső táblák áll.</span><span class="sxs-lookup"><span data-stu-id="58999-140">Now that you have specified hello data source and file format, you are ready toocreate hello external tables.</span></span> <span data-ttu-id="58999-141">Külső táblák, hogyan működnek együtt a külső adatforráshoz.</span><span class="sxs-lookup"><span data-stu-id="58999-141">External tables are how you interact with external data.</span></span> <span data-ttu-id="58999-142">A polybase rekurzív directory átjárás tooread hello hely paraméterben megadott hello könyvtár alkönyvtáraiban található összes fájl.</span><span class="sxs-lookup"><span data-stu-id="58999-142">PolyBase uses recursive directory traversal tooread all files in all subdirectories of hello directory specified in hello location parameter.</span></span> <span data-ttu-id="58999-143">Is hello a következő példa bemutatja, hogyan toocreate hello objektum.</span><span class="sxs-lookup"><span data-stu-id="58999-143">Also, hello following example shows how toocreate hello object.</span></span> <span data-ttu-id="58999-144">Toocustomize hello utasítás toowork hello adatokkal ADLS van szüksége.</span><span class="sxs-lookup"><span data-stu-id="58999-144">You need toocustomize hello statement toowork with hello data you have in ADLS.</span></span>

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

## <a name="external-table-considerations"></a><span data-ttu-id="58999-145">A külső tábla kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="58999-145">External Table Considerations</span></span>
<span data-ttu-id="58999-146">A külső tábla létrehozása egyszerű, de van néhány apró tárgyalt toobe igénylő.</span><span class="sxs-lookup"><span data-stu-id="58999-146">Creating an external table is easy, but there are some nuances that need toobe discussed.</span></span>

<span data-ttu-id="58999-147">A polybase-zel adatok betöltése erős típusmegadású.</span><span class="sxs-lookup"><span data-stu-id="58999-147">Loading data with PolyBase is strongly typed.</span></span> <span data-ttu-id="58999-148">Ez azt jelenti, hogy minden egyes sorára alatt okozhatnak hello adatok hello tábla sémadefiníciója meg kell felelniük.</span><span class="sxs-lookup"><span data-stu-id="58999-148">This means that each row of hello data being ingested must satisfy hello table schema definition.</span></span>
<span data-ttu-id="58999-149">Ha egy adott sor nem egyezik meg a hello sémadefiníciót, hello sor hello terhelés fogja elutasítani.</span><span class="sxs-lookup"><span data-stu-id="58999-149">If a given row does not match hello schema definition, hello row is rejected from hello load.</span></span>

<span data-ttu-id="58999-150">hello REJECT_TYPE és REJECT_VALUE beállítások lehetővé teszik toodefine hány sort vagy hello adatok hány százaléka hello végső tábla jelen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="58999-150">hello REJECT_TYPE and REJECT_VALUE options allow you toodefine how many rows or what percentage of hello data must be present in hello final table.</span></span>
<span data-ttu-id="58999-151">Betöltéskor hello utasítsa el az érték elérésekor, hello betöltése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="58999-151">During load, if hello reject value is reached, hello load fails.</span></span> <span data-ttu-id="58999-152">Elutasított sorok hello leggyakoribb oka, hogy séma definíciója nem egyeznek.</span><span class="sxs-lookup"><span data-stu-id="58999-152">hello most common cause of rejected rows is a schema definition mismatch.</span></span>
<span data-ttu-id="58999-153">Például egy olyan oszlop helytelenül int hello sémája nincs megadva, ha hello adatok hello fájlban egy karakterlánc, minden sor tooload meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="58999-153">For example, if a column is incorrectly given hello schema of int when hello data in hello file is a string, every row will fail tooload.</span></span>

<span data-ttu-id="58999-154">hello helye hello legfelső directory tooread adatait kívánt határozza meg.</span><span class="sxs-lookup"><span data-stu-id="58999-154">hello Location specifies hello topmost directory that you want tooread data from.</span></span>
<span data-ttu-id="58999-155">Ebben az esetben volt /DimProduct/ PolyBase alkönyvtárába hello alkönyvtárak belül minden hello adatok importálásától.</span><span class="sxs-lookup"><span data-stu-id="58999-155">In this case, if there were subdirectories under /DimProduct/ PolyBase would import all hello data within hello subdirectories.</span></span>

## <a name="load-hello-data"></a><span data-ttu-id="58999-156">Hello adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="58999-156">Load hello data</span></span>
<span data-ttu-id="58999-157">Azure Data Lake Store tooload adatait használja hello [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] utasítást.</span><span class="sxs-lookup"><span data-stu-id="58999-157">tooload data from Azure Data Lake Store use hello [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] statement.</span></span> <span data-ttu-id="58999-158">A CTAS betöltése használ hello erős típusmegadású hozott létre a külső tábla.</span><span class="sxs-lookup"><span data-stu-id="58999-158">Loading with CTAS uses hello strongly typed external table you have created.</span></span>

<span data-ttu-id="58999-159">CTAS új táblát hoz létre, és feltölti a select utasítás hello eredményekkel.</span><span class="sxs-lookup"><span data-stu-id="58999-159">CTAS creates a new table and populates it with hello results of a select statement.</span></span> <span data-ttu-id="58999-160">CTAS meghatározása hello új tábla toohave azonos oszlopok és adattípusok hello hello hello eredményeit válasszon ki az utasítást.</span><span class="sxs-lookup"><span data-stu-id="58999-160">CTAS defines hello new table toohave hello same columns and data types as hello results of hello select statement.</span></span> <span data-ttu-id="58999-161">Ha minden hello oszlop egy külső tábla, hello új tábla hello oszlopok és típusok adatok replikáját hello külső tábla.</span><span class="sxs-lookup"><span data-stu-id="58999-161">If you select all hello columns from an external table, hello new table is a replica of hello columns and data types in hello external table.</span></span>

<span data-ttu-id="58999-162">Ebben a példában létrehozzuk elosztott kivonattáblát a külső tábla DimProduct_external DimProduct hívása.</span><span class="sxs-lookup"><span data-stu-id="58999-162">In this example, we are creating a hash distributed table called DimProduct from our External Table DimProduct_external.</span></span>

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a><span data-ttu-id="58999-163">Oszlopcentrikus tömörítés optimalizálása</span><span class="sxs-lookup"><span data-stu-id="58999-163">Optimize columnstore compression</span></span>
<span data-ttu-id="58999-164">Alapértelmezés szerint az SQL Data Warehouse tárolja hello tábla fürtözött oszlopcentrikus index.</span><span class="sxs-lookup"><span data-stu-id="58999-164">By default, SQL Data Warehouse stores hello table as a clustered columnstore index.</span></span> <span data-ttu-id="58999-165">A betöltés befejezése után néhány hello adatsorok előfordulhat, hogy nem lehet tömörített hello oszlopcentrikus.</span><span class="sxs-lookup"><span data-stu-id="58999-165">After a load completes, some of hello data rows might not be compressed into hello columnstore.</span></span>  <span data-ttu-id="58999-166">Miért Ez akkor fordulhat elő, ennek több van.</span><span class="sxs-lookup"><span data-stu-id="58999-166">There's a variety of reasons why this can happen.</span></span> <span data-ttu-id="58999-167">több, lásd: toolearn [oszlopcentrikus Indexek kezelése][manage columnstore indexes].</span><span class="sxs-lookup"><span data-stu-id="58999-167">toolearn more, see [manage columnstore indexes][manage columnstore indexes].</span></span>

<span data-ttu-id="58999-168">toooptimize lekérdezési teljesítmény és a betöltés után oszlopcentrikus tömörítési építse újra hello tábla tooforce hello oszlopcentrikus index toocompress hello összes sort.</span><span class="sxs-lookup"><span data-stu-id="58999-168">toooptimize query performance and columnstore compression after a load, rebuild hello table tooforce hello columnstore index toocompress all hello rows.</span></span>

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

<span data-ttu-id="58999-169">Az oszlopcentrikus indexek fenntartása További információkért lásd: hello [oszlopcentrikus Indexek kezelése] [ manage columnstore indexes] cikk.</span><span class="sxs-lookup"><span data-stu-id="58999-169">For more information on maintaining columnstore indexes, see hello [manage columnstore indexes][manage columnstore indexes] article.</span></span>

## <a name="optimize-statistics"></a><span data-ttu-id="58999-170">Statisztika optimalizálása</span><span class="sxs-lookup"><span data-stu-id="58999-170">Optimize statistics</span></span>
<span data-ttu-id="58999-171">A betöltés után azonnal is ajánlott toocreate egyoszlopos statisztikát.</span><span class="sxs-lookup"><span data-stu-id="58999-171">It is best toocreate single-column statistics immediately after a load.</span></span> <span data-ttu-id="58999-172">Nincsenek néhány statisztikai lehetőségeit.</span><span class="sxs-lookup"><span data-stu-id="58999-172">There are some choices for statistics.</span></span> <span data-ttu-id="58999-173">Például ha egyoszlopos statisztikát hoz létre minden egyes oszlophoz eltarthat egy hosszú ideig toorebuild összes hello statisztikáját.</span><span class="sxs-lookup"><span data-stu-id="58999-173">For example, if you create single-column statistics on every column it might take a long time toorebuild all hello statistics.</span></span> <span data-ttu-id="58999-174">Ha ismeri az egyes oszlopok nem fog az lekérdezés predikátumokban toobe, kihagyhatja a statisztikák létrehozása az ilyen oszlopokat.</span><span class="sxs-lookup"><span data-stu-id="58999-174">If you know certain columns are not going toobe in query predicates, you can skip creating statistics on those columns.</span></span>

<span data-ttu-id="58999-175">Ha úgy dönt, hogy minden tábla minden egyes oszlophoz a toocreate egyoszlopos statisztikát, használhatja a hello tárolt eljárás kódminta `prc_sqldw_create_stats` a hello [statisztika] [ statistics] cikk.</span><span class="sxs-lookup"><span data-stu-id="58999-175">If you decide toocreate single-column statistics on every column of every table, you can use hello stored procedure code sample `prc_sqldw_create_stats` in hello [statistics][statistics] article.</span></span>

<span data-ttu-id="58999-176">a következő példa hello statisztikák létrehozása az jó kiindulási pont.</span><span class="sxs-lookup"><span data-stu-id="58999-176">hello following example is a good starting point for creating statistics.</span></span> <span data-ttu-id="58999-177">Egyoszlopos statisztikát hoz hello dimenzió tábla összes oszlopa, és minden hello ténytáblák csatlakozó oszlopában.</span><span class="sxs-lookup"><span data-stu-id="58999-177">It creates single-column statistics on each column in hello dimension table, and on each joining column in hello fact tables.</span></span> <span data-ttu-id="58999-178">Bármikor hozzáadhat egy vagy több oszlop statisztika tooother tény táblaoszlopok később.</span><span class="sxs-lookup"><span data-stu-id="58999-178">You can always add single or multi-column statistics tooother fact table columns later on.</span></span>


## <a name="achievement-unlocked"></a><span data-ttu-id="58999-179">Zárolása feloldva elérésének!</span><span class="sxs-lookup"><span data-stu-id="58999-179">Achievement unlocked!</span></span>
<span data-ttu-id="58999-180">Az Azure SQL Data Warehouse sikeresen betöltött adatok.</span><span class="sxs-lookup"><span data-stu-id="58999-180">You have successfully loaded data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="58999-181">Remek munka!</span><span class="sxs-lookup"><span data-stu-id="58999-181">Great job!</span></span>

##<a name="next-steps"></a><span data-ttu-id="58999-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="58999-182">Next Steps</span></span>
<span data-ttu-id="58999-183">Adatok betöltése az hello első lépés toodeveloping az SQL Data Warehouse data warehouse megoldást.</span><span class="sxs-lookup"><span data-stu-id="58999-183">Loading data is hello first step toodeveloping a data warehouse solution using SQL Data Warehouse.</span></span> <span data-ttu-id="58999-184">Tekintse meg a fejlesztői erőforrások a [táblák](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) és [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).</span><span class="sxs-lookup"><span data-stu-id="58999-184">Check out our development resources on [Tables](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) and [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).</span></span>


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
