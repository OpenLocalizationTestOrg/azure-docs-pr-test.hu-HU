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
# <a name="load-data-into-azure-sql-data-warehouse"></a><span data-ttu-id="46b8c-105">Adatok betöltése az Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="46b8c-105">Load data into Azure SQL Data Warehouse</span></span>
<span data-ttu-id="46b8c-106">Hello forgatókönyv beállítások és adatok betöltése az SQL Data Warehouse javaslatok összegzését.</span><span class="sxs-lookup"><span data-stu-id="46b8c-106">A summary of hello scenario options and recommendations for loading data into SQL Data Warehouse.</span></span>

<span data-ttu-id="46b8c-107">hello terhelést hello nagyon nehéz részét adatok betöltése hello adatok általában előkészítését végzi.</span><span class="sxs-lookup"><span data-stu-id="46b8c-107">hello hardest part of loading data is usually preparing hello data for hello load.</span></span> <span data-ttu-id="46b8c-108">Azure egyszerűbbé teszi betöltése használatával az Azure blob storage közös adattárként hello szolgáltatások számos, és közötti kommunikáció és az adatok mozgása tooorchestrate hello Azure Data Factory használatához Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="46b8c-108">Azure simplifies loading by using Azure blob storage as a common data store for many of hello services, and using Azure Data Factory tooorchestrate communication and data movement between hello Azure services.</span></span> <span data-ttu-id="46b8c-109">Ezeket a folyamatokat integrált PolyBase technológia, amely nagymértékben párhuzamos feldolgozási (MPP) tooload adatokat az Azure blob storage párhuzamosan az SQL Data Warehouse használ.</span><span class="sxs-lookup"><span data-stu-id="46b8c-109">These processes are integrated with PolyBase technology which uses massively parallel processing (MPP) tooload data in parallel from Azure blob storage into SQL Data Warehouse.</span></span> 

<span data-ttu-id="46b8c-110">Oktatóprogramot kínál, amelyek mintaadatbázisokat betölteni, lásd: [mintaadatbázisokat betölteni][Load sample databases].</span><span class="sxs-lookup"><span data-stu-id="46b8c-110">For tutorials that load sample databases, see [Load sample databases][Load sample databases].</span></span>

## <a name="load-from-azure-blob-storage"></a><span data-ttu-id="46b8c-111">Az Azure blob storage betöltése</span><span class="sxs-lookup"><span data-stu-id="46b8c-111">Load from Azure blob storage</span></span>
<span data-ttu-id="46b8c-112">hello leggyorsabb módon tooimport adatokat az SQL Data Warehouse PolyBase tooload adatokat az Azure blob storage toouse.</span><span class="sxs-lookup"><span data-stu-id="46b8c-112">hello fastest way tooimport data into SQL Data Warehouse is toouse PolyBase tooload data from Azure blob storage.</span></span> <span data-ttu-id="46b8c-113">A PolyBase által használt SQL Data Warehouse a nagymértékben párhuzamos adatfeldolgozás (MPP) tervezési tooload az Azure blob storage párhuzamosan.</span><span class="sxs-lookup"><span data-stu-id="46b8c-113">PolyBase uses SQL Data Warehouse's massively parallel processing (MPP) design tooload data in parallel from Azure blob storage.</span></span> <span data-ttu-id="46b8c-114">a PolyBase toouse, használhatja T-SQL parancsokkal vagy egy Azure Data Factory-folyamathoz.</span><span class="sxs-lookup"><span data-stu-id="46b8c-114">toouse PolyBase, you can use T-SQL commands or an Azure Data Factory pipeline.</span></span>

### <a name="1-use-polybase-and-t-sql"></a><span data-ttu-id="46b8c-115">1. A PolyBase és a T-SQL</span><span class="sxs-lookup"><span data-stu-id="46b8c-115">1. Use PolyBase and T-SQL</span></span>
<span data-ttu-id="46b8c-116">A betöltési folyamat összegzése:</span><span class="sxs-lookup"><span data-stu-id="46b8c-116">Summary of loading process:</span></span>

1. <span data-ttu-id="46b8c-117">Helyezze át a adatok tooAzure blob-tároló vagy az Azure Data Lake Store, és a szöveges fájlt tárolja.</span><span class="sxs-lookup"><span data-stu-id="46b8c-117">Move your data tooAzure blob storage or Azure Data Lake Store and store it in text files.</span></span>
2. <span data-ttu-id="46b8c-118">Külső objektumok konfigurálása az SQL Data Warehouse toodefine hello helyét és hello adatok formátuma</span><span class="sxs-lookup"><span data-stu-id="46b8c-118">Configure external objects in SQL Data Warehouse toodefine hello location and format of hello data</span></span>
3. <span data-ttu-id="46b8c-119">Egy új táblába adatbázis egy T-SQL parancsot tooload hello adatok párhuzamosan futnak.</span><span class="sxs-lookup"><span data-stu-id="46b8c-119">Run a T-SQL command tooload hello data in parallel into a new database table.</span></span>

<!-- 5. Schedule and run a loading job. --> 

<span data-ttu-id="46b8c-120">Az oktatóanyagok esetén lásd: [adatok betöltése az Azure blob storage tooSQL Data warehouse-ba (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="46b8c-120">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span></span>

### <a name="2-use-azure-data-factory"></a><span data-ttu-id="46b8c-121">2. Az Azure Data Factory használata</span><span class="sxs-lookup"><span data-stu-id="46b8c-121">2. Use Azure Data Factory</span></span>
<span data-ttu-id="46b8c-122">Egy egyszerűbb módon toouse PolyBase létrehozhat egy Azure Data Factory-folyamathoz, amely az SQL Data Warehouse PolyBase tooload adatait az Azure blob storage használja.</span><span class="sxs-lookup"><span data-stu-id="46b8c-122">For a simpler way toouse PolyBase, you can create an Azure Data Factory pipeline that uses PolyBase tooload data from Azure blob storage into SQL Data Warehouse.</span></span> <span data-ttu-id="46b8c-123">Gyors tooconfigure Ez azért, mert nem kell toodefine hello T-SQL-objektumokat.</span><span class="sxs-lookup"><span data-stu-id="46b8c-123">This is fast tooconfigure since you don't need toodefine hello T-SQL objects.</span></span> <span data-ttu-id="46b8c-124">Ha tooquery hello külső adatforráshoz kell importálni a nélkül, használja a T-SQL.</span><span class="sxs-lookup"><span data-stu-id="46b8c-124">If you need tooquery hello external data without importing it, use T-SQL.</span></span> 

<span data-ttu-id="46b8c-125">A betöltési folyamat összegzése:</span><span class="sxs-lookup"><span data-stu-id="46b8c-125">Summary of loading process:</span></span>

1. <span data-ttu-id="46b8c-126">Helyezze át az adatokat tooAzure blob-tároló, és szöveges fájlt tárolja.</span><span class="sxs-lookup"><span data-stu-id="46b8c-126">Move your data tooAzure blob storage and store it in text files.</span></span> <span data-ttu-id="46b8c-127">Az Azure Data Factory jelenleg nem támogatja a polybase-zel ADLS-kapcsolat).</span><span class="sxs-lookup"><span data-stu-id="46b8c-127">Azure Data Factory does not currently support ADLS connectivity with PolyBase).</span></span>
2. <span data-ttu-id="46b8c-128">Hozzon létre egy Azure Data Factory adatcsatorna tooingest hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="46b8c-128">Create an Azure Data Factory pipeline tooingest hello data.</span></span> <span data-ttu-id="46b8c-129">Hello PolyBase lehetőséggel.</span><span class="sxs-lookup"><span data-stu-id="46b8c-129">Use hello PolyBase option.</span></span>
4. <span data-ttu-id="46b8c-130">Ütemezési és futtatási hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="46b8c-130">Schedule and run hello pipeline.</span></span>

<span data-ttu-id="46b8c-131">Az oktatóanyagok esetén lásd: [adatok betöltése az Azure blob storage tooSQL Data warehouse-ba (Azure Data Factory)][Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)].</span><span class="sxs-lookup"><span data-stu-id="46b8c-131">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)][Load data from Azure blob storage tooSQL Data Warehouse (Azure Data Factory)].</span></span>

## <a name="load-from-sql-server"></a><span data-ttu-id="46b8c-132">Betöltése az SQL Server</span><span class="sxs-lookup"><span data-stu-id="46b8c-132">Load from SQL Server</span></span>
<span data-ttu-id="46b8c-133">is használhat az Integration Services (SSIS), az adatraktár SQL Server tooSQL tooload adatait át egybesimított fájlokba, vagy küldje el a lemezek tooMicrosoft.</span><span class="sxs-lookup"><span data-stu-id="46b8c-133">tooload data from SQL Server tooSQL Data Warehouse you can use Integration Services (SSIS), transfer flat files, or ship disks tooMicrosoft.</span></span> <span data-ttu-id="46b8c-134">Olvassa el a toosee különböző folyamatok és hivatkozások tootutorials betöltése hello összegzését.</span><span class="sxs-lookup"><span data-stu-id="46b8c-134">Read on toosee a summary of hello different loading processes and links tootutorials.</span></span>

<span data-ttu-id="46b8c-135">tooplan teljes adatok áttelepítését a SQL Server tooSQL adatraktár, lásd: hello [áttelepítése – áttekintés][Migration overview].</span><span class="sxs-lookup"><span data-stu-id="46b8c-135">tooplan a full data migration from SQL Server tooSQL Data Warehouse, see hello [Migration overview][Migration overview].</span></span> 

### <a name="use-integration-services-ssis"></a><span data-ttu-id="46b8c-136">Használja az Integration Services (SSIS)</span><span class="sxs-lookup"><span data-stu-id="46b8c-136">Use Integration Services (SSIS)</span></span>
<span data-ttu-id="46b8c-137">Ha már használja az SQL Server Integration Services (SSIS) csomag tooload, frissítheti a csomagok toouse SQL Server hello forrás-és az SQL Data Warehouse hello célként.</span><span class="sxs-lookup"><span data-stu-id="46b8c-137">If you are already using Integration Services (SSIS) packages tooload into SQL Server, you can update your packages toouse SQL Server as hello source and SQL Data Warehouse as hello destination.</span></span> <span data-ttu-id="46b8c-138">Ez a gyors és egyszerű toodo, és akkor hasznos, ha nem kívánt toomigrate a betöltés már hello felhőben toouse adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="46b8c-138">This is quick and easy toodo, and is a good choice if you are not trying toomigrate your loading process toouse data already in hello cloud.</span></span> <span data-ttu-id="46b8c-139">hello kompromisszumot hello terhelés lassabb, mint a PolyBase használatával, mert a SSIS nem végez hello terhelés párhuzamos lesz.</span><span class="sxs-lookup"><span data-stu-id="46b8c-139">hello tradeoff is hello load will be slower than using PolyBase because this SSIS does not perform hello load in parallel.</span></span>

<span data-ttu-id="46b8c-140">A betöltési folyamat összegzése:</span><span class="sxs-lookup"><span data-stu-id="46b8c-140">Summary of loading process:</span></span>

1. <span data-ttu-id="46b8c-141">Vizsgálja felül az integrációs szolgáltatások csomag toopoint toohello SQL Server-példányt hello forrás és hello cél hello SQL Data Warehouse-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="46b8c-141">Revise your Integration Services package toopoint toohello SQL Server instance for hello source and hello SQL Data Warehouse database for hello destination.</span></span>
2. <span data-ttu-id="46b8c-142">Telepítse át a séma tooSQL Data Warehouse még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="46b8c-142">Migrate your schema tooSQL Data Warehouse, if it is not there already.</span></span>
3. <span data-ttu-id="46b8c-143">A csomagok hello megfeleltetés módosítása csak hello adattípusok használatát az SQL Data Warehouse által támogatott.</span><span class="sxs-lookup"><span data-stu-id="46b8c-143">Change hello mapping in your packages use only hello data types that are supported by SQL Data Warehouse.</span></span>
4. <span data-ttu-id="46b8c-144">Ütemezés, és futtassa hello csomagot.</span><span class="sxs-lookup"><span data-stu-id="46b8c-144">Schedule and run hello package.</span></span>

<span data-ttu-id="46b8c-145">Az oktatóanyagok esetén lásd: [adatok betöltése az SQL Server tooAzure SQL Data Warehouse (SSIS)][Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)].</span><span class="sxs-lookup"><span data-stu-id="46b8c-145">For a tutorial, see [Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)][Load data from SQL Server tooAzure SQL Data Warehouse (SSIS)].</span></span>

### <a name="use-azcopy-recommended-for--10-tb-data"></a><span data-ttu-id="46b8c-146">Használja az AZCopy (< 10 TB-adatok esetén ajánlott)</span><span class="sxs-lookup"><span data-stu-id="46b8c-146">Use AZCopy (recommended for < 10 TB data)</span></span>
<span data-ttu-id="46b8c-147">Ha az adatok mérete < 10 TB, akkor is hello adatok exportálása az SQL Server tooflat fájlok, másolása hello fájlok tooAzure a blob storage és majd használja az SQL Data Warehouse PolyBase tooload hello adatok</span><span class="sxs-lookup"><span data-stu-id="46b8c-147">If your data size is < 10 TB, you can export hello data from SQL Server tooflat files, copy hello files tooAzure blob storage, and then use PolyBase tooload hello data into SQL Data Warehouse</span></span>

<span data-ttu-id="46b8c-148">A betöltési folyamat összegzése:</span><span class="sxs-lookup"><span data-stu-id="46b8c-148">Summary of loading process:</span></span>

1. <span data-ttu-id="46b8c-149">SQL Server tooflat fájlok hello bcp parancssori segédprogram tooexport adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="46b8c-149">Use hello bcp command-line utility tooexport data from SQL Server tooflat files.</span></span>
2. <span data-ttu-id="46b8c-150">Hello AZCopy parancssori segédprogram toocopy adatokat a egybesimított fájlokba tooAzure blob storage használata.</span><span class="sxs-lookup"><span data-stu-id="46b8c-150">Use hello AZCopy command-line utility toocopy data from flat files tooAzure blob storage.</span></span>
3. <span data-ttu-id="46b8c-151">Az SQL Data Warehouse PolyBase tooload használata</span><span class="sxs-lookup"><span data-stu-id="46b8c-151">Use PolyBase tooload into SQL Data Warehouse.</span></span>

<span data-ttu-id="46b8c-152">Az oktatóanyagok esetén lásd: [adatok betöltése az Azure blob storage tooSQL Data warehouse-ba (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="46b8c-152">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span></span>

### <a name="use-bcp"></a><span data-ttu-id="46b8c-153">Használja a bcp</span><span class="sxs-lookup"><span data-stu-id="46b8c-153">Use bcp</span></span>
<span data-ttu-id="46b8c-154">Ha kis mennyiségű adatokat a bcp tooload közvetlenül az Azure SQL Data Warehouse is használhatja.</span><span class="sxs-lookup"><span data-stu-id="46b8c-154">If you have a small amount of data you can use bcp tooload directly into Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="46b8c-155">A betöltési folyamat összegzése:</span><span class="sxs-lookup"><span data-stu-id="46b8c-155">Summary of loading process:</span></span>

1. <span data-ttu-id="46b8c-156">SQL Server tooflat fájlok hello bcp parancssori segédprogram tooexport adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="46b8c-156">Use hello bcp command-line utility tooexport data from SQL Server tooflat files.</span></span>
2. <span data-ttu-id="46b8c-157">A használandó bcp tooload adatok simán fájlok közvetlenül tooSQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="46b8c-157">Use bcp tooload data from flat files directly tooSQL Data Warehouse.</span></span>

<span data-ttu-id="46b8c-158">Az oktatóanyagok esetén lásd: [adatok betöltése az SQL Server tooAzure SQL Data warehouse-ba (bcp)][Load data from SQL Server tooAzure SQL Data Warehouse (bcp)].</span><span class="sxs-lookup"><span data-stu-id="46b8c-158">For a tutorial, see [Load data from SQL Server tooAzure SQL Data Warehouse (bcp)][Load data from SQL Server tooAzure SQL Data Warehouse (bcp)].</span></span>

### <a name="use-importexport-recommended-for--10-tb-data"></a><span data-ttu-id="46b8c-159">Használja az Import/Export (> 10 TB-adatok esetén ajánlott)</span><span class="sxs-lookup"><span data-stu-id="46b8c-159">Use Import/Export (recommended for > 10 TB data)</span></span>
<span data-ttu-id="46b8c-160">Ha az adatok mérete > 10 TB-os és toomove azt tooAzure, azt javasoljuk, hogy használja a szolgáltatás szállítási lemez [Import/Export][Import/Export].</span><span class="sxs-lookup"><span data-stu-id="46b8c-160">If your data size is > 10 TB and you want toomove it tooAzure, we recommend that you use our disk shipping service [Import/Export][Import/Export].</span></span> 

<span data-ttu-id="46b8c-161">A betöltési folyamat összegzése</span><span class="sxs-lookup"><span data-stu-id="46b8c-161">Summary of loading process</span></span>

1. <span data-ttu-id="46b8c-162">SQL Server tooflat fájlokat továbbítható lemezeken hello bcp parancssori segédprogram tooexport adatokat használja.</span><span class="sxs-lookup"><span data-stu-id="46b8c-162">Use hello bcp command-line utility tooexport data from SQL Server tooflat files on transferrable disks.</span></span>
2. <span data-ttu-id="46b8c-163">Hello lemezek tooMicrosoft szolgáltatástól.</span><span class="sxs-lookup"><span data-stu-id="46b8c-163">Ship hello disks tooMicrosoft.</span></span>
3. <span data-ttu-id="46b8c-164">A Microsoft hello adatokat tölt az SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="46b8c-164">Microsoft loads hello data into SQL Data Warehouse</span></span>

## <a name="load-from-hdinsight"></a><span data-ttu-id="46b8c-165">A HDInsight-ból betöltése</span><span class="sxs-lookup"><span data-stu-id="46b8c-165">Load from HDInsight</span></span>
<span data-ttu-id="46b8c-166">Az SQL Data Warehouse polybase HDInsight az adatok betöltését támogatja.</span><span class="sxs-lookup"><span data-stu-id="46b8c-166">SQL Data Warehouse supports loading data from HDInsight via PolyBase.</span></span> <span data-ttu-id="46b8c-167">hello folyamat van hello ugyanaz, mint az adatok betöltése az Azure Blob Storage - PolyBase tooconnect tooHDInsight tooload adatokkal.</span><span class="sxs-lookup"><span data-stu-id="46b8c-167">hello process is hello same as loading data from Azure Blob Storage - using PolyBase tooconnect tooHDInsight tooload data.</span></span> 

### <a name="1-use-polybase-and-t-sql"></a><span data-ttu-id="46b8c-168">1. A PolyBase és a T-SQL</span><span class="sxs-lookup"><span data-stu-id="46b8c-168">1. Use PolyBase and T-SQL</span></span>
<span data-ttu-id="46b8c-169">A betöltési folyamat összegzése:</span><span class="sxs-lookup"><span data-stu-id="46b8c-169">Summary of loading process:</span></span>

1. <span data-ttu-id="46b8c-170">Helyezze át az adatokat tooHDInsight, és tárolja a szövegfájlok, ORC vagy Parquet formátumban.</span><span class="sxs-lookup"><span data-stu-id="46b8c-170">Move your data tooHDInsight and store it in text files, ORC or Parquet format.</span></span>
2. <span data-ttu-id="46b8c-171">Külső objektumok adja meg az SQL Data Warehouse toodefine hello helyét és hello adatok formátuma.</span><span class="sxs-lookup"><span data-stu-id="46b8c-171">Configure external objects in SQL Data Warehouse toodefine hello location and format of hello data.</span></span>
3. <span data-ttu-id="46b8c-172">Egy új táblába adatbázis egy T-SQL parancsot tooload hello adatok párhuzamosan futnak.</span><span class="sxs-lookup"><span data-stu-id="46b8c-172">Run a T-SQL command tooload hello data in parallel into a new database table.</span></span>

<span data-ttu-id="46b8c-173">Az oktatóanyagok esetén lásd: [adatok betöltése az Azure blob storage tooSQL Data warehouse-ba (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span><span class="sxs-lookup"><span data-stu-id="46b8c-173">For a tutorial, see [Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)][Load data from Azure blob storage tooSQL Data Warehouse (PolyBase)].</span></span>

## <a name="recommendations"></a><span data-ttu-id="46b8c-174">Javaslatok</span><span class="sxs-lookup"><span data-stu-id="46b8c-174">Recommendations</span></span>
<span data-ttu-id="46b8c-175">Partnereink számos rendelkezik megoldások betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="46b8c-175">Many of our partners have loading solutions.</span></span> <span data-ttu-id="46b8c-176">További, toofind listájának megtekintéséhez a [megoldási partnerek][solution partners].</span><span class="sxs-lookup"><span data-stu-id="46b8c-176">toofind out more, see a list of our [solution partners][solution partners].</span></span> 

<span data-ttu-id="46b8c-177">Ha az adatokat a nem relációs forrásból származik, és azt az SQL Data adatraktár, akkor tooload kell tootransform sorok és oszlopok ahhoz, hogy töltse be azt.</span><span class="sxs-lookup"><span data-stu-id="46b8c-177">If your data is coming from a non-relational source and you want tooload it into SQL Data Warehouse you will need tootransform it into rows and columns before you load it.</span></span> <span data-ttu-id="46b8c-178">hello átalakított adatokat nem szükséges egy adatbázisban tárolt toobe, szövegfájlok tárolható.</span><span class="sxs-lookup"><span data-stu-id="46b8c-178">hello transformed data doesn't need toobe stored in a database, it can be stored in text files.</span></span>

<span data-ttu-id="46b8c-179">Statisztika létrehozása az újonnan betöltött adatokról.</span><span class="sxs-lookup"><span data-stu-id="46b8c-179">Create statistics on newly loaded data.</span></span> <span data-ttu-id="46b8c-180">Az Azure SQL Data Warehouse még nem támogatja a statisztikák automatikus létrehozását és frissítését.</span><span class="sxs-lookup"><span data-stu-id="46b8c-180">Azure SQL Data Warehouse does not yet support auto create or auto update statistics.</span></span>  <span data-ttu-id="46b8c-181">A sorrend tooget hello legjobb teljesítmény elérése érdekében a lekérdezéseket a fontos toocreate statisztika után hello táblák összes oszlopához az első betöltése vagy hello adatok minden lényeges módosítását fordul elő.</span><span class="sxs-lookup"><span data-stu-id="46b8c-181">In order tooget hello best performance from your queries, it's important toocreate statistics on all columns of all tables after hello first load or any substantial changes occur in hello data.</span></span>  <span data-ttu-id="46b8c-182">További információkért lásd: [statisztika][Statistics].</span><span class="sxs-lookup"><span data-stu-id="46b8c-182">For details, see [Statistics][Statistics].</span></span>

## <a name="next-steps"></a><span data-ttu-id="46b8c-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46b8c-183">Next steps</span></span>
<span data-ttu-id="46b8c-184">További fejlesztési tippek, lásd: hello [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="46b8c-184">For more development tips, see hello [development overview][development overview].</span></span>

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
