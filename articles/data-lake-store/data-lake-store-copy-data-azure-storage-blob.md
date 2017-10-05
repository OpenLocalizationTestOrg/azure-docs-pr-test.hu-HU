---
title: "Adatok másolása az Azure Storage blobs szolgáltatásban a Data Lake Store |} Microsoft Docs"
description: "Adatok másolása az Azure Storage Blobs Data Lake Store AdlCopy eszközzel"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 68f44991432a76c2ef1c79ec6dffdea4c62bcb17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a><span data-ttu-id="9a6de-103">Adatok másolása az Azure Storage-blobokból a Data Lake Store-ba</span><span class="sxs-lookup"><span data-stu-id="9a6de-103">Copy data from Azure Storage Blobs to Data Lake Store</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9a6de-104">A DistCp használata</span><span class="sxs-lookup"><span data-stu-id="9a6de-104">Using DistCp</span></span>](data-lake-store-copy-data-wasb-distcp.md)
> * [<span data-ttu-id="9a6de-105">Az AdlCopy használata</span><span class="sxs-lookup"><span data-stu-id="9a6de-105">Using AdlCopy</span></span>](data-lake-store-copy-data-azure-storage-blob.md)
>
>

<span data-ttu-id="9a6de-106">Azure Data Lake Store biztosítja a parancssori eszköz [AdlCopy](http://aka.ms/downloadadlcopy), a következő forrásokból származó adatok másolása:</span><span class="sxs-lookup"><span data-stu-id="9a6de-106">Azure Data Lake Store provides a command line tool, [AdlCopy](http://aka.ms/downloadadlcopy), to copy data from the following sources:</span></span>

* <span data-ttu-id="9a6de-107">Tárolóblobokból az Azure Data Lake-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="9a6de-107">From Azure Storage Blobs into Data Lake Store.</span></span> <span data-ttu-id="9a6de-108">Adatok másolása az Data Lake Store az Azure Storage blobs AdlCopy nem használható.</span><span class="sxs-lookup"><span data-stu-id="9a6de-108">You cannot use AdlCopy to copy data from Data Lake Store to Azure Storage blobs.</span></span>
* <span data-ttu-id="9a6de-109">Között két Azure Data Lake Store-fiókot.</span><span class="sxs-lookup"><span data-stu-id="9a6de-109">Between two Azure Data Lake Store accounts.</span></span>

<span data-ttu-id="9a6de-110">Ezenkívül használhatja a AdlCopy eszköz két különböző módban:</span><span class="sxs-lookup"><span data-stu-id="9a6de-110">Also, you can use the AdlCopy tool in two different modes:</span></span>

* <span data-ttu-id="9a6de-111">**Önálló**, ahol az eszköz Data Lake Store erőforrást használ a feladat végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="9a6de-111">**Standalone**, where the tool uses Data Lake Store resources to perform the task.</span></span>
* <span data-ttu-id="9a6de-112">**A Data Lake Analytics-fiókkal**, ahol a másolási művelet végrehajtásához az egység a Data Lake Analytics-fiókjához társított használják.</span><span class="sxs-lookup"><span data-stu-id="9a6de-112">**Using a Data Lake Analytics account**, where the units assigned to your Data Lake Analytics account are used to perform the copy operation.</span></span> <span data-ttu-id="9a6de-113">Előfordulhat, hogy szeretné ezt a beállítást használja, ha szeretné a másolási feladatokat kiszámítható módon.</span><span class="sxs-lookup"><span data-stu-id="9a6de-113">You might want to use this option when you are looking to perform the copy tasks in a predictable manner.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9a6de-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9a6de-114">Prerequisites</span></span>
<span data-ttu-id="9a6de-115">A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="9a6de-115">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="9a6de-116">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="9a6de-116">**An Azure subscription**.</span></span> <span data-ttu-id="9a6de-117">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9a6de-117">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="9a6de-118">**Az Azure Storage Blobs** néhány adat-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="9a6de-118">**Azure Storage Blobs** container with some data.</span></span>
* <span data-ttu-id="9a6de-119">**Egy Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="9a6de-119">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="9a6de-120">Hogyan hozhat létre ilyet, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="9a6de-120">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="9a6de-121">**Az Azure Data Lake Analytics fiók (nem kötelező)** -lásd [Ismerkedés az Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) Data Lake Store-fiók létrehozásához útmutatást.</span><span class="sxs-lookup"><span data-stu-id="9a6de-121">**Azure Data Lake Analytics account (optional)** - See [Get started with Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) for instructions on how to create a Data Lake Store account.</span></span>
* <span data-ttu-id="9a6de-122">**AdlCopy eszköz**.</span><span class="sxs-lookup"><span data-stu-id="9a6de-122">**AdlCopy tool**.</span></span> <span data-ttu-id="9a6de-123">Telepítse a AdlCopy eszközt [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).</span><span class="sxs-lookup"><span data-stu-id="9a6de-123">Install the AdlCopy tool from [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).</span></span>

## <a name="syntax-of-the-adlcopy-tool"></a><span data-ttu-id="9a6de-124">A AdlCopy eszköz szintaxisa</span><span class="sxs-lookup"><span data-stu-id="9a6de-124">Syntax of the AdlCopy tool</span></span>
<span data-ttu-id="9a6de-125">Az alábbi szintaxissal a AdlCopy eszköz használata</span><span class="sxs-lookup"><span data-stu-id="9a6de-125">Use the following syntax to work with the AdlCopy tool</span></span>

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern

<span data-ttu-id="9a6de-126">A paraméterek a szintaxist a leírása a következő:</span><span class="sxs-lookup"><span data-stu-id="9a6de-126">The parameters in the syntax are described below:</span></span>

| <span data-ttu-id="9a6de-127">Beállítás</span><span class="sxs-lookup"><span data-stu-id="9a6de-127">Option</span></span> | <span data-ttu-id="9a6de-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="9a6de-128">Description</span></span> |
| --- | --- |
| <span data-ttu-id="9a6de-129">Forrás</span><span class="sxs-lookup"><span data-stu-id="9a6de-129">Source</span></span> |<span data-ttu-id="9a6de-130">Adja meg az adatok helye az Azure storage-blobba.</span><span class="sxs-lookup"><span data-stu-id="9a6de-130">Specifies the location of the source data in the Azure storage blob.</span></span> <span data-ttu-id="9a6de-131">A forrás lehet egy blob-tároló, egy blobba vagy egy másik Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="9a6de-131">The source can be a blob container, a blob, or another Data Lake Store account.</span></span> |
| <span data-ttu-id="9a6de-132">Cél</span><span class="sxs-lookup"><span data-stu-id="9a6de-132">Dest</span></span> |<span data-ttu-id="9a6de-133">Adja meg a Data Lake Store cél másolja.</span><span class="sxs-lookup"><span data-stu-id="9a6de-133">Specifies the Data Lake Store destination to copy to.</span></span> |
| <span data-ttu-id="9a6de-134">SourceKey</span><span class="sxs-lookup"><span data-stu-id="9a6de-134">SourceKey</span></span> |<span data-ttu-id="9a6de-135">Adja meg a hozzáférési kulcsot az Azure storage-blob adatforrásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="9a6de-135">Specifies the storage access key for the Azure storage blob source.</span></span> <span data-ttu-id="9a6de-136">Ez azért szükséges, csak ha a forrás egy blob-tároló vagy egy blobot.</span><span class="sxs-lookup"><span data-stu-id="9a6de-136">This is required only if the source is a blob container or a blob.</span></span> |
| <span data-ttu-id="9a6de-137">Fiók</span><span class="sxs-lookup"><span data-stu-id="9a6de-137">Account</span></span> |<span data-ttu-id="9a6de-138">**Választható**.</span><span class="sxs-lookup"><span data-stu-id="9a6de-138">**Optional**.</span></span> <span data-ttu-id="9a6de-139">Akkor használja, ha azt szeretné, a másolási feladat futtatása az Azure Data Lake Analytics-fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="9a6de-139">Use this if you want to use Azure Data Lake Analytics account to run the copy job.</span></span> <span data-ttu-id="9a6de-140">Ha használja a /Account lehetőséget a szintaxissal, de nem adja meg a Data Lake Analytics-fiók, AdlCopy egy alapértelmezett fiók használatával futtatni a feladatot.</span><span class="sxs-lookup"><span data-stu-id="9a6de-140">If you use the /Account option in the syntax but do not specify a Data Lake Analytics account, AdlCopy uses a default account to run the job.</span></span> <span data-ttu-id="9a6de-141">Is ha ezt a beállítást használja, hozzá kell adnia a forrás (Azure Storage-Blobba) és a cél (az Azure Data Lake Store) adatforrásaként a Data Lake Analytics-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="9a6de-141">Also, if you use this option, you must add the source (Azure Storage Blob) and destination (Azure Data Lake Store) as data sources for your Data Lake Analytics account.</span></span> |
| <span data-ttu-id="9a6de-142">egység</span><span class="sxs-lookup"><span data-stu-id="9a6de-142">Units</span></span> |<span data-ttu-id="9a6de-143">Itt adhatja meg, amely jelzi a másolási feladatot a Data Lake Analytics egységek száma.</span><span class="sxs-lookup"><span data-stu-id="9a6de-143">Specifies the number of Data Lake Analytics units that will be used for the copy job.</span></span> <span data-ttu-id="9a6de-144">Ez a beállítás nem kötelező, ha a **/fiók** beállítással adhatja meg a Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="9a6de-144">This option is mandatory if you use the **/Account** option to specify the Data Lake Analytics account.</span></span> |
| <span data-ttu-id="9a6de-145">Minta</span><span class="sxs-lookup"><span data-stu-id="9a6de-145">Pattern</span></span> |<span data-ttu-id="9a6de-146">Adja meg a reguláris kifejezéssel mintát, amely azt jelzi, mely blobokkal vagy a fájlok másolása.</span><span class="sxs-lookup"><span data-stu-id="9a6de-146">Specifies a regex pattern that indicates which blobs or files to copy.</span></span> <span data-ttu-id="9a6de-147">AdlCopy használja a megfelelő kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="9a6de-147">AdlCopy uses case-sensitive matching.</span></span> <span data-ttu-id="9a6de-148">Az alapértelmezett használható, ha nincs minta van megadva a mintája, másolja az összes elemet.</span><span class="sxs-lookup"><span data-stu-id="9a6de-148">The default pattern used when no pattern is specified is to copy all items.</span></span> <span data-ttu-id="9a6de-149">Több fájl minták megadása nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="9a6de-149">Specifying multiple file patterns is not supported.</span></span> |

## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a><span data-ttu-id="9a6de-150">Használják AdlCopy (önálló) adatok másolása az Azure Storage-blobba</span><span class="sxs-lookup"><span data-stu-id="9a6de-150">Use AdlCopy (as standalone) to copy data from an Azure Storage blob</span></span>
1. <span data-ttu-id="9a6de-151">Nyisson meg egy parancssort, és keresse meg azt a könyvtárat, AdlCopy futtató, általában `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="9a6de-151">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="9a6de-152">A következő parancsot egy adott blob átmásolja a forrás-tároló egy Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="9a6de-152">Run the following command to copy a specific blob from the source container to a Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    <span data-ttu-id="9a6de-153">Példa:</span><span class="sxs-lookup"><span data-stu-id="9a6de-153">For example:</span></span>

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE] <span data-ttu-id="9a6de-154">A fenti szintaxist határozza meg, hogy a fájl másolása egy mappába a Data Lake Store-fiókban.</span><span class="sxs-lookup"><span data-stu-id="9a6de-154">The syntax above specifies the file to be copied to a folder in the Data Lake Store account.</span></span> <span data-ttu-id="9a6de-155">AdlCopy eszköz egy mappát hoz létre, ha a megadott mappanév nem létezik.</span><span class="sxs-lookup"><span data-stu-id="9a6de-155">AdlCopy tool creates a folder if the specified folder name does not exist.</span></span>

    <span data-ttu-id="9a6de-156">A rendszer bekéri a hitelesítő adatok megadása az Azure-előfizetés alapján, amely rendelkezik a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="9a6de-156">You will be prompted to enter the credentials for the Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="9a6de-157">Egy a következőhöz hasonló kimenetet fog látni:</span><span class="sxs-lookup"><span data-stu-id="9a6de-157">You will see an output similar to the following:</span></span>

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. <span data-ttu-id="9a6de-158">Is átmásolhatja a blobokat több tároló a Data Lake Store-fiókba, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="9a6de-158">You can also copy all the blobs from one container to the Data Lake Store account using the following command:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    <span data-ttu-id="9a6de-159">Példa:</span><span class="sxs-lookup"><span data-stu-id="9a6de-159">For example:</span></span>

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a><span data-ttu-id="9a6de-160">A teljesítménnyel kapcsolatos megfontolások</span><span class="sxs-lookup"><span data-stu-id="9a6de-160">Performance considerations</span></span>

<span data-ttu-id="9a6de-161">Az Azure Blob Storage-fiók másolása, a lehetséges, hogy szabályozva, a blob storage oldalon másolása során.</span><span class="sxs-lookup"><span data-stu-id="9a6de-161">If you are copying from an Azure Blob Storage account, you may be throttled during copy on the blob storage side.</span></span> <span data-ttu-id="9a6de-162">Ez csökkenti a teljesítményt, a másolat.</span><span class="sxs-lookup"><span data-stu-id="9a6de-162">This will degrade the performance of your copy job.</span></span> <span data-ttu-id="9a6de-163">Az Azure Blob Storage korlátai által megszabott kapcsolatos további információkért lásd: Azure Storage-korlátok, [Azure-előfizetés és a szolgáltatásra vonatkozó korlátozások](../azure-subscription-service-limits.md).</span><span class="sxs-lookup"><span data-stu-id="9a6de-163">To learn more about the limits of Azure Blob Storage, see Azure Storage limits at [Azure subscription and service limits](../azure-subscription-service-limits.md).</span></span>

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a><span data-ttu-id="9a6de-164">Használják AdlCopy (önálló) adatok másolása másik Data Lake Store-fiók</span><span class="sxs-lookup"><span data-stu-id="9a6de-164">Use AdlCopy (as standalone) to copy data from another Data Lake Store account</span></span>
<span data-ttu-id="9a6de-165">AdlCopy használatával másolja az adatokat két Data Lake Store-fiókok között.</span><span class="sxs-lookup"><span data-stu-id="9a6de-165">You can also use AdlCopy to copy data between two Data Lake Store accounts.</span></span>

1. <span data-ttu-id="9a6de-166">Nyisson meg egy parancssort, és keresse meg azt a könyvtárat, AdlCopy futtató, általában `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="9a6de-166">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="9a6de-167">A következő parancsot egy adott fájlt átmásolni egy Data Lake Store-fiók egy másikra.</span><span class="sxs-lookup"><span data-stu-id="9a6de-167">Run the following command to copy a specific file from one Data Lake Store account to another.</span></span>

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    <span data-ttu-id="9a6de-168">Példa:</span><span class="sxs-lookup"><span data-stu-id="9a6de-168">For example:</span></span>

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > <span data-ttu-id="9a6de-169">A fenti szintaxist határozza meg, hogy a fájl másolása egy mappába a célként megadott Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="9a6de-169">The syntax above specifies the file to be copied to a folder in the destination Data Lake Store account.</span></span> <span data-ttu-id="9a6de-170">AdlCopy eszköz egy mappát hoz létre, ha a megadott mappanév nem létezik.</span><span class="sxs-lookup"><span data-stu-id="9a6de-170">AdlCopy tool creates a folder if the specified folder name does not exist.</span></span>
   >
   >

    <span data-ttu-id="9a6de-171">A rendszer bekéri a hitelesítő adatok megadása az Azure-előfizetés alapján, amely rendelkezik a Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="9a6de-171">You will be prompted to enter the credentials for the Azure subscription under which you have your Data Lake Store account.</span></span> <span data-ttu-id="9a6de-172">Egy a következőhöz hasonló kimenetet fog látni:</span><span class="sxs-lookup"><span data-stu-id="9a6de-172">You will see an output similar to the following:</span></span>

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. <span data-ttu-id="9a6de-173">A következő parancsot egy adott mappából a forráshelyen Data Lake Store-fiók összes fájlt másolja a célként megadott Data Lake Store-fiók mappába.</span><span class="sxs-lookup"><span data-stu-id="9a6de-173">The following command copies all files from a specific folder in the source Data Lake Store account to a folder in the destination Data Lake Store account.</span></span>

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a><span data-ttu-id="9a6de-174">A teljesítménnyel kapcsolatos megfontolások</span><span class="sxs-lookup"><span data-stu-id="9a6de-174">Performance considerations</span></span>

<span data-ttu-id="9a6de-175">AdlCopy önálló eszközként használatakor a példány futtatja megosztott, az Azure által felügyelt erőforrások.</span><span class="sxs-lookup"><span data-stu-id="9a6de-175">When using AdlCopy as a standalone tool, the copy is run on shared, Azure managed resources.</span></span> <span data-ttu-id="9a6de-176">A teljesítmény jelenhet meg ebben a környezetben a rendszerterhelés és a rendelkezésre álló erőforrások függ.</span><span class="sxs-lookup"><span data-stu-id="9a6de-176">The performance you may get in this environment depends on system load and available resources.</span></span> <span data-ttu-id="9a6de-177">Ebben a módban leginkább kis átvitelek eseti alapon szolgál.</span><span class="sxs-lookup"><span data-stu-id="9a6de-177">This mode is best used for small transfers on an ad hoc basis.</span></span> <span data-ttu-id="9a6de-178">Paraméterek nélkül kell AdlCopy használata önálló eszközként kell beállítani.</span><span class="sxs-lookup"><span data-stu-id="9a6de-178">No parameters need to be tuned when using AdlCopy as a standalone tool.</span></span>

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a><span data-ttu-id="9a6de-179">Használja AdlCopy (Data Lake Analytics-fiók) adatok másolása</span><span class="sxs-lookup"><span data-stu-id="9a6de-179">Use AdlCopy (with Data Lake Analytics account) to copy data</span></span>
<span data-ttu-id="9a6de-180">A Data Lake Analytics-fiók használatával a Data Lake Store-adatok másolása az Azure storage blobs AdlCopy feladat futtatása.</span><span class="sxs-lookup"><span data-stu-id="9a6de-180">You can also use your Data Lake Analytics account to run the AdlCopy job to copy data from Azure storage blobs to Data Lake Store.</span></span> <span data-ttu-id="9a6de-181">Általában használja ezt a beállítást, amikor az adatok áthelyezésének gigabájt és terabájt a tartományban, és azt szeretné, hogy jobb és kiszámítható teljesítmény átviteli sebesség.</span><span class="sxs-lookup"><span data-stu-id="9a6de-181">You would typically use this option when the data to be moved is in the range of gigabytes and terabytes, and you want better and predictable performance throughput.</span></span>

<span data-ttu-id="9a6de-182">A Data Lake Analytics-fiók használata AdlCopy másolása az Azure Storage-Blobból, a forrás (Azure Storage-Blobba) a Data Lake Analytics-fiók adatforrásként hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="9a6de-182">To use your Data Lake Analytics account with AdlCopy to copy from an Azure Storage Blob, the source (Azure Storage Blob) must be added as a data source for your Data Lake Analytics account.</span></span> <span data-ttu-id="9a6de-183">A Data Lake Analytics-fiókhoz való hozzáadását további adatforrások, lásd: [Data Lake Analytics kezelése az adatforrások fiók](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).</span><span class="sxs-lookup"><span data-stu-id="9a6de-183">For instructions on adding additional data sources to your Data Lake Analytics account, see [Manage Data Lake Analytics account data sources](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).</span></span>

> [!NOTE]
> <span data-ttu-id="9a6de-184">A Data Lake Analytics-fiókkal forrásaként másolása az Azure Data Lake Store-fiókból, nem kell a Data Lake Store-fiók társítása a Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="9a6de-184">If you are copying from an Azure Data Lake Store account as the source using a Data Lake Analytics account, you do not need to associate the Data Lake Store account with the Data Lake Analytics account.</span></span> <span data-ttu-id="9a6de-185">A követelmény a forrás tárolási társítja a Data Lake Analytics-fiók csak akkor, ha a forrás álljon az Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="9a6de-185">The requirement to associate the source store with the Data Lake Analytics account is only when the source is an Azure Storage account.</span></span>
>
>

<span data-ttu-id="9a6de-186">A következő parancsot egy Data Lake Store-fiók használatával a Data Lake Analytics-fiók egy Azure Storage-blob másolása:</span><span class="sxs-lookup"><span data-stu-id="9a6de-186">Run the following command to copy from an Azure Storage blob to a Data Lake Store account using Data Lake Analytics account:</span></span>

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

<span data-ttu-id="9a6de-187">Példa:</span><span class="sxs-lookup"><span data-stu-id="9a6de-187">For example:</span></span>

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

<span data-ttu-id="9a6de-188">Hasonlóképpen futtassa a következő parancsot egy Data Lake Store-fiók használatával a Data Lake Analytics-fiók egy Azure Storage-blob másolása:</span><span class="sxs-lookup"><span data-stu-id="9a6de-188">Similarly, run the following command to copy from an Azure Storage blob to a Data Lake Store account using Data Lake Analytics account:</span></span>

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a><span data-ttu-id="9a6de-189">A teljesítménnyel kapcsolatos megfontolások</span><span class="sxs-lookup"><span data-stu-id="9a6de-189">Performance considerations</span></span>

<span data-ttu-id="9a6de-190">Az adatok másolásának terabájt közé, amikor a AdlCopy a saját Azure Data Lake Analytics-fiók használatával hatékonyabb és sokkal kiszámíthatóbbá teljesítményt biztosít.</span><span class="sxs-lookup"><span data-stu-id="9a6de-190">When copying data in the range of terabytes, using AdlCopy with your own Azure Data Lake Analytics account provides better and more predictable performance.</span></span> <span data-ttu-id="9a6de-191">Amely kell kell hangolni paraméter Azure Data Lake Analytics egység a másolási feladat használandó számát.</span><span class="sxs-lookup"><span data-stu-id="9a6de-191">The parameter that should be tuned is the number of Azure Data Lake Analytics Units to use for the copy job.</span></span> <span data-ttu-id="9a6de-192">Egységek számának növelésével növeli a teljesítményt, a másolat.</span><span class="sxs-lookup"><span data-stu-id="9a6de-192">Increasing the number of units will increase the performance of your copy job.</span></span> <span data-ttu-id="9a6de-193">A másolandó fájlok maximális egy egységet használhat.</span><span class="sxs-lookup"><span data-stu-id="9a6de-193">Each file to be copied can use maximum one unit.</span></span> <span data-ttu-id="9a6de-194">Fájlok másolásának számánál több egységek megadása nem növeli a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="9a6de-194">Specifying more units than the number of files being copied will not increase performance.</span></span>

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a><span data-ttu-id="9a6de-195">Másolja az adatokat használ a mintamegfeleltetés AdlCopy segítségével</span><span class="sxs-lookup"><span data-stu-id="9a6de-195">Use AdlCopy to copy data using pattern matching</span></span>
<span data-ttu-id="9a6de-196">Ebben a szakaszban megismerheti, hogyan AdlCopy használatával másolja az adatokat egy forrásból (az alábbi példában használjuk Azure Storage-Blobba) használ a mintamegfeleltetés cél Data Lake Store-fiókra.</span><span class="sxs-lookup"><span data-stu-id="9a6de-196">In this section, you learn how to use AdlCopy to copy data from a source (in our example below we use Azure Storage Blob) to a destination Data Lake Store account using pattern matching.</span></span> <span data-ttu-id="9a6de-197">Az alábbi lépéseket használhatja például a forrás blob összes .csv kiterjesztésű fájlt átmásolni a cél.</span><span class="sxs-lookup"><span data-stu-id="9a6de-197">For example, you can use the steps below to copy all files with .csv extension from the source blob to the destination.</span></span>

1. <span data-ttu-id="9a6de-198">Nyisson meg egy parancssort, és keresse meg azt a könyvtárat, AdlCopy futtató, általában `%HOMEPATH%\Documents\adlcopy`.</span><span class="sxs-lookup"><span data-stu-id="9a6de-198">Open a command prompt and navigate to the directory where AdlCopy is installed, typically `%HOMEPATH%\Documents\adlcopy`.</span></span>
2. <span data-ttu-id="9a6de-199">A következő parancsot az összes *.csv kiterjesztésű fájlt átmásolni egy adott blob-forrás tárolójából. a Data Lake Store:</span><span class="sxs-lookup"><span data-stu-id="9a6de-199">Run the following command to copy all files with *.csv extension from a specific blob from the source container to a Data Lake Store:</span></span>

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    <span data-ttu-id="9a6de-200">Példa:</span><span class="sxs-lookup"><span data-stu-id="9a6de-200">For example:</span></span>

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a><span data-ttu-id="9a6de-201">Számlázás</span><span class="sxs-lookup"><span data-stu-id="9a6de-201">Billing</span></span>
* <span data-ttu-id="9a6de-202">Ha a AdlCopy eszközzel önálló számlázott kilépő költségek áthelyezésére adatokat, ha a forrás Azure Storage-fiók nem ugyanabban a régióban, mint a Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="9a6de-202">If you use the AdlCopy tool as standalone you will be billed for egress costs for moving data, if the source Azure Storage account is not in the same region as the Data Lake Store.</span></span>
* <span data-ttu-id="9a6de-203">Ha a AdlCopy eszköz használata a Data Lake Analytics fiók, szabványos [Data Lake Analytics díjszabás számlázási](https://azure.microsoft.com/pricing/details/data-lake-analytics/) alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="9a6de-203">If you use the AdlCopy tool with your Data Lake Analytics account, standard [Data Lake Analytics billing rates](https://azure.microsoft.com/pricing/details/data-lake-analytics/) will apply.</span></span>

## <a name="considerations-for-using-adlcopy"></a><span data-ttu-id="9a6de-204">AdlCopy használatának szempontjai</span><span class="sxs-lookup"><span data-stu-id="9a6de-204">Considerations for using AdlCopy</span></span>
* <span data-ttu-id="9a6de-205">(Verziójához 1.0.5), AdlCopy támogatja, amelyek együttesen több ezer fájlok és mappák forrásokból származó adatok másolását.</span><span class="sxs-lookup"><span data-stu-id="9a6de-205">AdlCopy (for version 1.0.5), supports copying data from sources that collectively have more than thousands of files and folders.</span></span> <span data-ttu-id="9a6de-206">Azonban, hogy a felmerülő problémák nagy dataset másolása után a fájlok és mappák terjesztése különböző alárendelt mappákba is inkább az elérési út a alárendelt mappákra forrásaként.</span><span class="sxs-lookup"><span data-stu-id="9a6de-206">However, if you encounter issues copying a large dataset, you can distribute the files/folders into different sub-folders and use the path to those sub-folders as the source instead.</span></span>

## <a name="performance-considerations-for-using-adlcopy"></a><span data-ttu-id="9a6de-207">AdlCopy a teljesítménnyel kapcsolatos szempontok</span><span class="sxs-lookup"><span data-stu-id="9a6de-207">Performance considerations for using AdlCopy</span></span>

<span data-ttu-id="9a6de-208">AdlCopy támogatja az adatok másolását a fájlok és mappák ezer tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="9a6de-208">AdlCopy supports copying data containing thousands of files and folders.</span></span> <span data-ttu-id="9a6de-209">Azonban ha problémák nagy dataset másolása, terjesztheti a fájlok és mappák kisebb alárendelt mappákba.</span><span class="sxs-lookup"><span data-stu-id="9a6de-209">However, if you encounter issues copying a large dataset, you can distribute the files/folders into smaller sub-folders.</span></span> <span data-ttu-id="9a6de-210">AdlCopy alkalmi példány lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="9a6de-210">AdlCopy was built for ad hoc copies.</span></span> <span data-ttu-id="9a6de-211">Ha az ismétlődő adatokat másolni kívánt, érdemes [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) , amely körül a másolási műveletek teljes felügyeletet biztosít.</span><span class="sxs-lookup"><span data-stu-id="9a6de-211">If you are trying to copy data on a recurring basis, you should consider using [Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md) that provides full management around the copy operations.</span></span>

## <a name="release-notes"></a><span data-ttu-id="9a6de-212">Kibocsátási megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9a6de-212">Release notes</span></span>
* <span data-ttu-id="9a6de-213">1.0.13 - másolása adatok a azonos Azure Data Lake Store-fiókba között több adlcopy parancs, akkor nem kell minden egyes futtatásához a hitelesítő adatok újbóli többé.</span><span class="sxs-lookup"><span data-stu-id="9a6de-213">1.0.13 - If you are copying data to the same Azure Data Lake Store account across multiple adlcopy commands, you do not need to reenter your credentials for each run anymore.</span></span> <span data-ttu-id="9a6de-214">Adlcopy között több futtatása most gyorsítótárazhatják az adatokat.</span><span class="sxs-lookup"><span data-stu-id="9a6de-214">Adlcopy will now cache that information across multiple runs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a6de-215">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9a6de-215">Next steps</span></span>
* [<span data-ttu-id="9a6de-216">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="9a6de-216">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="9a6de-217">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="9a6de-217">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="9a6de-218">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="9a6de-218">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
