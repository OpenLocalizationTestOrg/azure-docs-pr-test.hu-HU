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
# <a name="load-1-tb-into-azure-sql-data-warehouse-under-15-minutes-with-data-factory"></a><span data-ttu-id="891c3-103">1 TB-os betöltése az Azure SQL Data Warehouse Data Factory a 15 perc</span><span class="sxs-lookup"><span data-stu-id="891c3-103">Load 1 TB into Azure SQL Data Warehouse under 15 minutes with Data Factory</span></span>
<span data-ttu-id="891c3-104">[Az SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) egy olyan felhőalapú, horizontálisan felskálázható adatbázis képes a nagy mennyiségű relációs és nem relációs adatok.</span><span class="sxs-lookup"><span data-stu-id="891c3-104">[Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) is a cloud-based, scale-out database capable of processing massive volumes of data, both relational and non-relational.</span></span>  <span data-ttu-id="891c3-105">Nagymértékben párhuzamos feldolgozási (MPP) architektúrára épülő SQL Data Warehouse optimalizált vállalati adatok adatraktár munkaterhelések.</span><span class="sxs-lookup"><span data-stu-id="891c3-105">Built on massively parallel processing (MPP) architecture, SQL Data Warehouse is optimized for enterprise data warehouse workloads.</span></span>  <span data-ttu-id="891c3-106">Felhőalapú hello rugalmasságot tooscale tárolóval a rugalmasság és a számítási egymástól függetlenül kínál.</span><span class="sxs-lookup"><span data-stu-id="891c3-106">It offers cloud elasticity with hello flexibility tooscale storage and compute independently.</span></span>

<span data-ttu-id="891c3-107">Ismerkedés az Azure SQL Data Warehouse mostantól egyszerűbb, mint minden eddiginél használatával **Azure Data Factory**.</span><span class="sxs-lookup"><span data-stu-id="891c3-107">Getting started with Azure SQL Data Warehouse is now easier than ever using **Azure Data Factory**.</span></span>  <span data-ttu-id="891c3-108">Az Azure Data Factory egy teljes körűen felügyelt felhőalapú integrációs szolgáltatás, amely lehet használt toopopulate egy SQL Data Warehouse a meglévő rendszerről, és akkor értékes időt az SQL Data Warehouse értékelése és felépítése az elemzés közben mentése hello adatokkal megoldások.</span><span class="sxs-lookup"><span data-stu-id="891c3-108">Azure Data Factory is a fully managed cloud-based data integration service, which can be used toopopulate a SQL Data Warehouse with hello data from your existing system, and saving you valuable time while evaluating SQL Data Warehouse and building your analytics solutions.</span></span> <span data-ttu-id="891c3-109">Az alábbiakban hello legfontosabb előnyei adatok betöltése az Azure SQL Data Warehouse Azure Data Factory használatával:</span><span class="sxs-lookup"><span data-stu-id="891c3-109">Here are hello key benefits of loading data into Azure SQL Data Warehouse using Azure Data Factory:</span></span>

* <span data-ttu-id="891c3-110">**Egyszerű tooset be**: 5. lépés intuitív varázsló nem parancsfájl-szükséges.</span><span class="sxs-lookup"><span data-stu-id="891c3-110">**Easy tooset up**: 5-step intuitive wizard with no scripting required.</span></span>
* <span data-ttu-id="891c3-111">**Gazdag adattároló támogatási**: a helyszíni és felhőalapú adattároló széles skáláját beépített támogatása.</span><span class="sxs-lookup"><span data-stu-id="891c3-111">**Rich data store support**: built-in support for a rich set of on-premises and cloud-based data stores.</span></span>
* <span data-ttu-id="891c3-112">**Biztonságos és megfelelő**: HTTPS- vagy ExpressRoute keresztül továbbított adatok, és a globális szolgáltatás meglétének biztosítja az adatok sosem hagyja el hello földrajzi határ</span><span class="sxs-lookup"><span data-stu-id="891c3-112">**Secure and compliant**: data is transferred over HTTPS or ExpressRoute, and global service presence ensures your data never leaves hello geographical boundary</span></span>
* <span data-ttu-id="891c3-113">**A PolyBase használatával unparalleled teljesítmény** – a Polybase használatával hello leghatékonyabb módon toomove adatokat az Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="891c3-113">**Unparalleled performance by using PolyBase** – Using Polybase is hello most efficient way toomove data into Azure SQL Data Warehouse.</span></span> <span data-ttu-id="891c3-114">Blob szolgáltatás átmeneti hello használ, nagy terhelés sebességű adattárolókhoz mellett az Azure Blob Storage tárolóban, amely támogatja a Polybase hello alapértelmezés szerint az összes típusú érhet el.</span><span class="sxs-lookup"><span data-stu-id="891c3-114">Using hello staging blob feature, you can achieve high load speeds from all types of data stores besides Azure Blob storage, which hello Polybase supports by default.</span></span>

<span data-ttu-id="891c3-115">Ez a cikk bemutatja, hogyan toouse Data Factory másolása varázsló tooload 1 TB méretű adatok az Azure Blob Storage az Azure SQL Data Warehouse a 15 percig, több mint 1,2 GB/s átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="891c3-115">This article shows you how toouse Data Factory Copy Wizard tooload 1-TB data from Azure Blob Storage into Azure SQL Data Warehouse in under 15 minutes, at over 1.2 GBps throughput.</span></span>

<span data-ttu-id="891c3-116">Ez a cikk részletesen adatok áthelyezése az Azure SQL Data Warehouse hello másolása varázsló használatával.</span><span class="sxs-lookup"><span data-stu-id="891c3-116">This article provides step-by-step instructions for moving data into Azure SQL Data Warehouse by using hello Copy Wizard.</span></span>

> [!NOTE]
>  <span data-ttu-id="891c3-117">Általános képességeivel kapcsolatos információk a Data Factory az adatok áthelyezése az Azure SQL Data Warehouse, lásd: [adatok tooand áthelyezése az Azure SQL Data Warehouse Azure Data Factory használatával](data-factory-azure-sql-data-warehouse-connector.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="891c3-117">For general information about capabilities of Data Factory in moving data to/from Azure SQL Data Warehouse, see [Move data tooand from Azure SQL Data Warehouse using Azure Data Factory](data-factory-azure-sql-data-warehouse-connector.md) article.</span></span>
>
> <span data-ttu-id="891c3-118">Is hozhat létre Azure-portált használja, a Visual Studio PowerShell, adatcsatornákat stb. Lásd: [oktatóanyag: adatok másolása az Azure Blob tooAzure SQL-adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) a részletes utasításokat hello másolási tevékenység az Azure Data Factory gyors útmutatást.</span><span class="sxs-lookup"><span data-stu-id="891c3-118">You can also build pipelines using Azure portal, Visual Studio, PowerShell, etc. See [Tutorial: Copy data from Azure Blob tooAzure SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for a quick walkthrough with step-by-step instructions for using hello Copy Activity in Azure Data Factory.</span></span>  
>
>

## <a name="prerequisites"></a><span data-ttu-id="891c3-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="891c3-119">Prerequisites</span></span>
* <span data-ttu-id="891c3-120">Az Azure Blob Storage: ehhez a kísérlethez Azure Blob Storage (GRS) használ a TPC-H tesztelési adatkészlet tárolásához.</span><span class="sxs-lookup"><span data-stu-id="891c3-120">Azure Blob Storage: this experiment uses Azure Blob Storage (GRS) for storing TPC-H testing dataset.</span></span>  <span data-ttu-id="891c3-121">Ha nem rendelkezik Azure-tárfiók, további [hogyan toocreate tárfiók](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span><span class="sxs-lookup"><span data-stu-id="891c3-121">If you do not have an Azure storage account, learn [how toocreate a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account).</span></span>
* <span data-ttu-id="891c3-122">[TPC-H](http://www.tpc.org/tpch/) adatok: fogjuk toouse TPC-H, tesztelési dataset hello.</span><span class="sxs-lookup"><span data-stu-id="891c3-122">[TPC-H](http://www.tpc.org/tpch/) data: we are going toouse TPC-H as hello testing dataset.</span></span>  <span data-ttu-id="891c3-123">hogy szüksége van-e toouse toodo `dbgen` a TPC-H eszközkészlet segítségével hello dataset készítése.</span><span class="sxs-lookup"><span data-stu-id="891c3-123">toodo that, you need toouse `dbgen` from TPC-H toolkit, which helps you generate hello dataset.</span></span>  <span data-ttu-id="891c3-124">Vagy letöltheti a forráskódot `dbgen` a [TPC eszközök](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) , és saját magára vagy letöltési összeállított hello bináris fájljának lefordítani [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).</span><span class="sxs-lookup"><span data-stu-id="891c3-124">You can either download source code for `dbgen` from [TPC Tools](http://www.tpc.org/tpc_documents_current_versions/current_specifications.asp) and compile it yourself, or download hello compiled binary from [GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/TPCHTools).</span></span>  <span data-ttu-id="891c3-125">Futtatási dbgen.exe hello következőre toogenerate 1 TB-os egybesimított fájl a parancsok `lineitem` terjedésének tábla 10 a fájlok között:</span><span class="sxs-lookup"><span data-stu-id="891c3-125">Run dbgen.exe with hello following commands toogenerate 1 TB flat file for `lineitem` table spread across 10 files:</span></span>

  * `Dbgen -s 1000 -S **1** -C 10 -T L -v`
  * `Dbgen -s 1000 -S **2** -C 10 -T L -v`
  * <span data-ttu-id="891c3-126">…</span><span class="sxs-lookup"><span data-stu-id="891c3-126">…</span></span>
  * `Dbgen -s 1000 -S **10** -C 10 -T L -v`

    <span data-ttu-id="891c3-127">Másolás hello most fájlok tooAzure Blob jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="891c3-127">Now copy hello generated files tooAzure Blob.</span></span>  <span data-ttu-id="891c3-128">Tekintse meg a túl[adatok tooand áthelyezését egy helyszíni fájlrendszer Azure Data Factory](data-factory-onprem-file-system-connector.md) arról, hogyan toodo, amely ADF-másolással.</span><span class="sxs-lookup"><span data-stu-id="891c3-128">Refer too[Move data tooand from an on-premises file system by using Azure Data Factory](data-factory-onprem-file-system-connector.md) for how toodo that using ADF Copy.</span></span>    
* <span data-ttu-id="891c3-129">Az SQL Data Warehouse: ehhez a kísérlethez adatokat tölt az Azure SQL Data Warehouse 6000 dwu-k számát létrehozva</span><span class="sxs-lookup"><span data-stu-id="891c3-129">Azure SQL Data Warehouse: this experiment loads data into Azure SQL Data Warehouse created with 6,000 DWUs</span></span>

    <span data-ttu-id="891c3-130">Tekintse meg a túl[hozzon létre egy Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) részletes információkra van szüksége hogyan toocreate egy SQL Data Warehouse-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="891c3-130">Refer too[Create an Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-provision.md) for detailed instructions on how toocreate a SQL Data Warehouse database.</span></span>  <span data-ttu-id="891c3-131">tooget hello legjobb lehetséges terhelés teljesítmény elérése érdekében az SQL Data Warehouse Polybase használatával választjuk Adattárházegységek (dwu-k) engedélyezett az hello beállítás, amely 6000 dwu-k maximális számát.</span><span class="sxs-lookup"><span data-stu-id="891c3-131">tooget hello best possible load performance into SQL Data Warehouse using Polybase, we choose maximum number of Data Warehouse Units (DWUs) allowed in hello Performance setting, which is 6,000 DWUs.</span></span>

  > [!NOTE]
  > <span data-ttu-id="891c3-132">Az Azure Blob betöltésekor hello Adatbetöltési teljesítmény közvetlenül arányos toohello beállítja az SQL Data Warehouse hello dwu-k száma:</span><span class="sxs-lookup"><span data-stu-id="891c3-132">When loading from Azure Blob, hello data loading performance is directly proportional toohello number of DWUs you configure on hello SQL Data Warehouse:</span></span>
  >
  > <span data-ttu-id="891c3-133">1 TB-os betöltése 1000 a DWU az SQL Data Warehouse veszi a 87 perc (~ 200 MB/s átviteli) betöltése 1 TB 2000 DWU az SQL Data Warehouse vesz 46 perc (~ 380 MB/s átviteli) betöltése 1 TB az 6000 DWU SQL Data Warehouse perc 14 (~1.2 GB/s átviteli)</span><span class="sxs-lookup"><span data-stu-id="891c3-133">Loading 1 TB into 1,000 DWU SQL Data Warehouse takes 87 minutes (~200 MBps throughput) Loading 1 TB into 2,000 DWU SQL Data Warehouse takes 46 minutes (~380 MBps throughput) Loading 1 TB into 6,000 DWU SQL Data Warehouse takes 14 minutes (~1.2 GBps throughput)</span></span>
  >
  >

    <span data-ttu-id="891c3-134">6000 dwu-k, az SQL Data Warehouse toocreate csúszkát hello teljesítmény összes hello módon toohello jobbra:</span><span class="sxs-lookup"><span data-stu-id="891c3-134">toocreate a SQL Data Warehouse with 6,000 DWUs, move hello Performance slider all hello way toohello right:</span></span>

    ![Teljesítmény csúszka](media/data-factory-load-sql-data-warehouse/performance-slider.png)

    <span data-ttu-id="891c3-136">A meglévő adatbázis, amely nincs beállítva 6000 dwu-k legfeljebb az Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="891c3-136">For an existing database that is not configured with 6,000 DWUs, you can scale it up using Azure portal.</span></span>  <span data-ttu-id="891c3-137">Keresse meg a toohello adatbázis Azure-portálon, és nincs egy **méretezési** hello gombjára **áttekintése** panel hello kép a következő látható:</span><span class="sxs-lookup"><span data-stu-id="891c3-137">Navigate toohello database in Azure portal, and there is a **Scale** button in hello **Overview** panel shown in hello following image:</span></span>

    ![Skála gomb](media/data-factory-load-sql-data-warehouse/scale-button.png)    

    <span data-ttu-id="891c3-139">Kattintson a hello **méretezési** gomb tooopen hello következő panelen, hello csúszkát toohello maximális értéket, és kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="891c3-139">Click hello **Scale** button tooopen hello following panel, move hello slider toohello maximum value, and click **Save** button.</span></span>

    ![Párbeszédpanel](media/data-factory-load-sql-data-warehouse/scale-dialog.png)

    <span data-ttu-id="891c3-141">Ehhez a kísérlethez adatokat tölt be az Azure SQL Data Warehouse használatával `xlargerc` erőforrásosztály.</span><span class="sxs-lookup"><span data-stu-id="891c3-141">This experiment loads data into Azure SQL Data Warehouse using `xlargerc` resource class.</span></span>

    <span data-ttu-id="891c3-142">tooachieve legjobb lehetséges átviteli sebességet másolása kell tartozó túl az SQL Data Warehouse felhasználó segítségével toobe`xlargerc` erőforrásosztály.</span><span class="sxs-lookup"><span data-stu-id="891c3-142">tooachieve best possible throughput, copy needs toobe performed using a SQL Data Warehouse user belonging too`xlargerc` resource class.</span></span>  <span data-ttu-id="891c3-143">Megtudhatja, hogyan toodo, amelyek a következő [módosíthatja a felhasználói erőforrás osztály példa](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span><span class="sxs-lookup"><span data-stu-id="891c3-143">Learn how toodo that by following [Change a user resource class example](../sql-data-warehouse/sql-data-warehouse-develop-concurrency.md#changing-user-resource-class-example).</span></span>  
* <span data-ttu-id="891c3-144">Cél táblaséma létrehozása az Azure SQL Data Warehouse-adatbázis, hello DDL-utasítás a következő futtatásával:</span><span class="sxs-lookup"><span data-stu-id="891c3-144">Create destination table schema in Azure SQL Data Warehouse database, by running hello following DDL statement:</span></span>

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
<span data-ttu-id="891c3-145">Hello előzetesen szükséges lépések leírását befejeződött, a felhőmodellből most készen áll a tooconfigure hello másolási tevékenység hello másolása varázsló használatával.</span><span class="sxs-lookup"><span data-stu-id="891c3-145">With hello prerequisite steps completed, we are now ready tooconfigure hello copy activity using hello Copy Wizard.</span></span>

## <a name="launch-copy-wizard"></a><span data-ttu-id="891c3-146">A Másolás varázsló indítása</span><span class="sxs-lookup"><span data-stu-id="891c3-146">Launch Copy Wizard</span></span>
1. <span data-ttu-id="891c3-147">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="891c3-147">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="891c3-148">Kattintson a **+ új** hello bal felső sarokban, kattintson **Eszközintelligencia + analitika**, és kattintson a **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="891c3-148">Click **+ NEW** from hello top-left corner, click **Intelligence + analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="891c3-149">A hello **új adat-előállító** panel:</span><span class="sxs-lookup"><span data-stu-id="891c3-149">In hello **New data factory** blade:</span></span>

   1. <span data-ttu-id="891c3-150">Adja meg **LoadIntoSQLDWDataFactory** a hello **neve**.</span><span class="sxs-lookup"><span data-stu-id="891c3-150">Enter **LoadIntoSQLDWDataFactory** for hello **name**.</span></span>
       <span data-ttu-id="891c3-151">az Azure data factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="891c3-151">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="891c3-152">Ha hello hibaüzenetet kapja: **nem érhető el adat-előállító "LoadIntoSQLDWDataFactory"**, hello adat-előállítóban (például yournameLoadIntoSQLDWDataFactory) hello nevének módosítása, és próbálja meg újra létrehozni.</span><span class="sxs-lookup"><span data-stu-id="891c3-152">If you receive hello error: **Data factory name “LoadIntoSQLDWDataFactory” is not available**, change hello name of hello data factory (for example, yournameLoadIntoSQLDWDataFactory) and try creating again.</span></span> <span data-ttu-id="891c3-153">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="891c3-153">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>  
   2. <span data-ttu-id="891c3-154">Jelölje ki az Azure-**előfizetést**.</span><span class="sxs-lookup"><span data-stu-id="891c3-154">Select your Azure **subscription**.</span></span>
   3. <span data-ttu-id="891c3-155">Az erőforráscsoport hajtsa végre a lépéseket követve hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="891c3-155">For Resource Group, do one of hello following steps:</span></span>
      1. <span data-ttu-id="891c3-156">Válassza ki **meglévő** tooselect egy meglévő erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="891c3-156">Select **Use existing** tooselect an existing resource group.</span></span>
      2. <span data-ttu-id="891c3-157">Válassza ki **hozzon létre új** tooenter erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="891c3-157">Select **Create new** tooenter a name for a resource group.</span></span>
   4. <span data-ttu-id="891c3-158">Válassza ki a **hely** hello adat-előállító esetében.</span><span class="sxs-lookup"><span data-stu-id="891c3-158">Select a **location** for hello data factory.</span></span>
   5. <span data-ttu-id="891c3-159">Válassza ki **PIN-kód toodashboard** hello hello panel alsó részén jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="891c3-159">Select **Pin toodashboard** check box at hello bottom of hello blade.</span></span>  
   6. <span data-ttu-id="891c3-160">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="891c3-160">Click **Create**.</span></span>
4. <span data-ttu-id="891c3-161">Hello létrehozásának befejezése után megjelenik a hello **adat-előállító** panelen látható hello kép a következő módon:</span><span class="sxs-lookup"><span data-stu-id="891c3-161">After hello creation is complete, you see hello **Data Factory** blade as shown in hello following image:</span></span>

   ![Data factory kezdőlap](media/data-factory-load-sql-data-warehouse/data-factory-home-page-copy-data.png)
5. <span data-ttu-id="891c3-163">A hello adat-előállító kezdőlapján kattintson a hello **adatok másolása** toolaunch csempe **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="891c3-163">On hello Data Factory home page, click hello **Copy data** tile toolaunch **Copy Wizard**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="891c3-164">Ha azt látja, hogy hello webböngésző akadt-e a "Engedélyező...", tiltsa le vagy törölje a jelet **harmadik féltől származó cookie-k blokkolását, és a helyadatok** beállítása (vagy) engedélyezve legyen, és hozzon létre egy kivételt **login.microsoftonline.com**, és ezután próbálja meg újból elindítani a hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="891c3-164">If you see that hello web browser is stuck at "Authorizing...", disable/uncheck **Block third party cookies and site data** setting (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching hello wizard again.</span></span>
   >
   >

## <a name="step-1-configure-data-loading-schedule"></a><span data-ttu-id="891c3-165">1. lépés: Adatok betöltése az ütemezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="891c3-165">Step 1: Configure data loading schedule</span></span>
<span data-ttu-id="891c3-166">hello első lépése az tooconfigure hello adatai ütemezés betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="891c3-166">hello first step is tooconfigure hello data loading schedule.</span></span>  

<span data-ttu-id="891c3-167">A hello **tulajdonságok** lap:</span><span class="sxs-lookup"><span data-stu-id="891c3-167">In hello **Properties** page:</span></span>

1. <span data-ttu-id="891c3-168">Adja meg **CopyFromBlobToAzureSqlDataWarehouse** a **feladat neve**</span><span class="sxs-lookup"><span data-stu-id="891c3-168">Enter **CopyFromBlobToAzureSqlDataWarehouse** for **Task name**</span></span>
2. <span data-ttu-id="891c3-169">Válassza ki **futtassa egyszer most** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="891c3-169">Select **Run once now** option.</span></span>   
3. <span data-ttu-id="891c3-170">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="891c3-170">Click **Next**.</span></span>  

    ![Másolása varázsló – Tulajdonságok lap](media/data-factory-load-sql-data-warehouse/copy-wizard-properties-page.png)

## <a name="step-2-configure-source"></a><span data-ttu-id="891c3-172">2. lépés: Állítson be forrás</span><span class="sxs-lookup"><span data-stu-id="891c3-172">Step 2: Configure source</span></span>
<span data-ttu-id="891c3-173">A szakasz tartalmazza, akkor hello lépéseket tooconfigure hello forrás: Azure Blob tartalmazó hello 1 TB méretű TPC-H sorelemet fájlokat.</span><span class="sxs-lookup"><span data-stu-id="891c3-173">This section shows you hello steps tooconfigure hello source: Azure Blob containing hello 1-TB TPC-H line item files.</span></span>

1. <span data-ttu-id="891c3-174">Jelölje be hello **Azure Blob Storage** hello adatok tárolására, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="891c3-174">Select hello **Azure Blob Storage** as hello data store and click **Next**.</span></span>

    ![Másolása varázsló – forrás kiválasztása lap](media/data-factory-load-sql-data-warehouse/select-source-connection.png)

2. <span data-ttu-id="891c3-176">Hello Azure Blob storage-fiók hello csatlakozási adatait, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="891c3-176">Fill in hello connection information for hello Azure Blob storage account, and click **Next**.</span></span>

    ![Másolása varázsló – adatforrás kapcsolati adatait](media/data-factory-load-sql-data-warehouse/source-connection-info.png)

3. <span data-ttu-id="891c3-178">Válassza ki a hello **mappa** hello TPC-H sort tartalmazó fájlok elemet, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="891c3-178">Choose hello **folder** containing hello TPC-H line item files and click **Next**.</span></span>

    ![Másolása varázsló – a bemeneti mappa kiválasztása](media/data-factory-load-sql-data-warehouse/select-input-folder.png)

4. <span data-ttu-id="891c3-180">Gomb megnyomásakor **következő**, hello fájl formátuma beállításokat a rendszer automatikusan észleli.</span><span class="sxs-lookup"><span data-stu-id="891c3-180">Upon clicking **Next**, hello file format settings are detected automatically.</span></span>  <span data-ttu-id="891c3-181">Ellenőrizze, hogy az adott oszlop határolójel toomake ' |} "helyett hello alapértelmezett vesszővel", ".</span><span class="sxs-lookup"><span data-stu-id="891c3-181">Check toomake sure that column delimiter is ‘|’ instead of hello default comma ‘,’.</span></span>  <span data-ttu-id="891c3-182">Kattintson a **következő** után meg kell előzetesen hello adatok.</span><span class="sxs-lookup"><span data-stu-id="891c3-182">Click **Next** after you have previewed hello data.</span></span>

    ![Másolása varázsló – fájl formázási beállítások](media/data-factory-load-sql-data-warehouse/file-format-settings.png)

## <a name="step-3-configure-destination"></a><span data-ttu-id="891c3-184">3. lépés: A célkiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="891c3-184">Step 3: Configure destination</span></span>
<span data-ttu-id="891c3-185">Ez a szakasz bemutatja, hogyan tooconfigure hello cél: `lineitem` hello Azure SQL Data Warehouse-adatbázis táblája.</span><span class="sxs-lookup"><span data-stu-id="891c3-185">This section shows you how tooconfigure hello destination: `lineitem` table in hello Azure SQL Data Warehouse database.</span></span>

1. <span data-ttu-id="891c3-186">Válasszon **Azure SQL Data Warehouse** hello célként tárolja, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="891c3-186">Choose **Azure SQL Data Warehouse** as hello destination store and click **Next**.</span></span>

    ![Másolása varázsló – a célkiszolgáló kijelölése adattár](media/data-factory-load-sql-data-warehouse/select-destination-data-store.png)

2. <span data-ttu-id="891c3-188">Töltse ki hello Azure SQL Data Warehouse-kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="891c3-188">Fill in hello connection information for Azure SQL Data Warehouse.</span></span>  <span data-ttu-id="891c3-189">Ellenőrizze, hogy a megadott hello szerepkör hello felhasználói `xlargerc` (lásd: hello **Előfeltételek** szakaszban részletes információkra van szüksége), és kattintson **tovább**.</span><span class="sxs-lookup"><span data-stu-id="891c3-189">Make sure you specify hello user that is a member of hello role `xlargerc` (see hello **prerequisites** section for detailed instructions), and click **Next**.</span></span>

    ![Másolása varázsló – cél Kapcsolatinformáció](media/data-factory-load-sql-data-warehouse/destination-connection-info.png)

3. <span data-ttu-id="891c3-191">Hello céltábla válasszon, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="891c3-191">Choose hello destination table and click **Next**.</span></span>

    ![Másolása varázsló – leképezés lapja](media/data-factory-load-sql-data-warehouse/table-mapping-page.png)

4. <span data-ttu-id="891c3-193">A séma leképezési lapon hagyja "Alkalmazza oszlopleképezés" beállítás nincs bejelölve, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="891c3-193">In Schema mapping page, leave "Apply column mapping" option unchecked and click **Next**.</span></span>

## <a name="step-4-performance-settings"></a><span data-ttu-id="891c3-194">4. lépés: Teljesítményadat-beállításai</span><span class="sxs-lookup"><span data-stu-id="891c3-194">Step 4: Performance settings</span></span>

<span data-ttu-id="891c3-195">**A polybase lehetővé** alapértelmezés szerint be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="891c3-195">**Allow polybase** is checked by default.</span></span>  <span data-ttu-id="891c3-196">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="891c3-196">Click **Next**.</span></span>

![Másolása varázsló – séma leképezési lap](media/data-factory-load-sql-data-warehouse/performance-settings-page.png)

## <a name="step-5-deploy-and-monitor-load-results"></a><span data-ttu-id="891c3-198">5. lépés: Telepítheti, és figyelheti a terhelést eredmények</span><span class="sxs-lookup"><span data-stu-id="891c3-198">Step 5: Deploy and monitor load results</span></span>
1. <span data-ttu-id="891c3-199">Kattintson a **Befejezés** gomb toodeploy.</span><span class="sxs-lookup"><span data-stu-id="891c3-199">Click **Finish** button toodeploy.</span></span>

    ![Másolása varázsló – összegző lapja](media/data-factory-load-sql-data-warehouse/summary-page.png)

2. <span data-ttu-id="891c3-201">Hello telepítés befejezése után kattintson `Click here toomonitor copy pipeline` toomonitor hello másolási folyamat futtatásához.</span><span class="sxs-lookup"><span data-stu-id="891c3-201">After hello deployment is complete, click `Click here toomonitor copy pipeline` toomonitor hello copy run progress.</span></span> <span data-ttu-id="891c3-202">Jelölje be hello másolási folyamat hello létrehozott **tevékenység Windows** listája.</span><span class="sxs-lookup"><span data-stu-id="891c3-202">Select hello copy pipeline you created in hello **Activity Windows** list.</span></span>

    ![Másolása varázsló – összegző lapja](media/data-factory-load-sql-data-warehouse/select-pipeline-monitor-manage-app.png)

    <span data-ttu-id="891c3-204">Megtekintheti a részletek futtatni hello hello másolása **tevékenység ablak Explorer** hello jobb oldali panelen, beleértve a hello adatmennyiség forrásból olvashatók és írhatók a cél, időtartama és hello átlagos átviteli sebességgel hello futtassa a.</span><span class="sxs-lookup"><span data-stu-id="891c3-204">You can view hello copy run details in hello **Activity Window Explorer** in hello right panel, including hello data volume read from source and written into destination, duration, and hello average throughput for hello run.</span></span>

    <span data-ttu-id="891c3-205">A következő képernyőfelvétel, 1 TB-os másolása az Azure Blob Storage-ból az SQL Data Warehouse hello látható tartott 14 perc, hatékonyan a 1.22 GB/s átviteli sebesség eléréséhez!</span><span class="sxs-lookup"><span data-stu-id="891c3-205">As you can see from hello following screen shot, copying 1 TB from Azure Blob Storage into SQL Data Warehouse took 14 minutes, effectively achieving 1.22 GBps throughput!</span></span>

    ![Másolása varázsló – sikeres párbeszédpanel](media/data-factory-load-sql-data-warehouse/succeeded-info.png)

## <a name="best-practices"></a><span data-ttu-id="891c3-207">Ajánlott eljárások</span><span class="sxs-lookup"><span data-stu-id="891c3-207">Best practices</span></span>
<span data-ttu-id="891c3-208">Az alábbiakban néhány gyakorlati tanácsok az Azure SQL Data Warehouse-adatbázis futtatásához:</span><span class="sxs-lookup"><span data-stu-id="891c3-208">Here are a few best practices for running your Azure SQL Data Warehouse database:</span></span>

* <span data-ttu-id="891c3-209">Használjon nagyobb erőforrásosztály FÜRTÖZÖTT OSZLOPCENTRIKUS INDEX vezérlőbe való betöltés.</span><span class="sxs-lookup"><span data-stu-id="891c3-209">Use a larger resource class when loading into a CLUSTERED COLUMNSTORE INDEX.</span></span>
* <span data-ttu-id="891c3-210">Hatékonyabb táblákra érdemes lehet kivonatoló terjesztési round robin terjesztési alapértelmezett helyett válassza oszlop szerint.</span><span class="sxs-lookup"><span data-stu-id="891c3-210">For more efficient joins, consider using hash distribution by a select column instead of default round robin distribution.</span></span>
* <span data-ttu-id="891c3-211">Betöltési sebesség gyorsabb érdemes halommemória az átmeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="891c3-211">For faster load speeds, consider using heap for transient data.</span></span>
* <span data-ttu-id="891c3-212">Statisztikák létrehozása az Azure SQL Data Warehouse betöltése után.</span><span class="sxs-lookup"><span data-stu-id="891c3-212">Create statistics after you finish loading Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="891c3-213">Lásd: [ajánlott eljárások az Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="891c3-213">See [Best practices for Azure SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-best-practices.md) for details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="891c3-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="891c3-214">Next steps</span></span>
* <span data-ttu-id="891c3-215">[Data Factory másolása varázsló](data-factory-copy-wizard.md) – Ez a cikk ismerteti a hello másolása varázsló.</span><span class="sxs-lookup"><span data-stu-id="891c3-215">[Data Factory Copy Wizard](data-factory-copy-wizard.md) - This article provides details about hello Copy Wizard.</span></span>
* <span data-ttu-id="891c3-216">[Másolja a tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) -Ez a cikk hello hivatkozás TELJESÍTMÉNYMÉRÉSEK és hangolási útmutató tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="891c3-216">[Copy Activity performance and tuning guide](data-factory-copy-activity-performance.md) - This article contains hello reference performance measurements and tuning guide.</span></span>
