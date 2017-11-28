---
title: "az Azure CLI 2.0 verziót használja Azure Data Lake Analytics használatába aaaGet |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure parancssori felület 2.0 toocreate Data Lake Analytics-fiók, hozzon létre egy Data Lake Analytics-feladatot U-SQL használatával, valamint hello feladat elküldéséhez. "
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: saveenr
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: jgao
ms.openlocfilehash: c4e91c0d3526e4932c2948c0a326d4cedc985791
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli-20"></a><span data-ttu-id="fada9-103">Az Azure Data Lake Analytics használatának első lépései az Azure parancssori felületének 2.0-s verziójával</span><span class="sxs-lookup"><span data-stu-id="fada9-103">Get started with Azure Data Lake Analytics using Azure CLI 2.0</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="fada9-104">Az oktatóanyaggal egy olyan feladatot fog elkészíteni, amely beolvas egy tabulátorral elválasztott értékeket tartalmazó fájlt (TSV), majd konvertálja azt egy vesszővel elválasztott értékeket tartalmazó fájllá (CSV).</span><span class="sxs-lookup"><span data-stu-id="fada9-104">In this tutorial, you develop a job that reads a tab separated values (TSV) file and converts it into a comma-separated values (CSV) file.</span></span> <span data-ttu-id="fada9-105">toogo keresztül hello ugyanezt az oktatóanyagot más használatával támogatott eszközöket, hello hello felül Ez a szakasz a legördülő lista használata.</span><span class="sxs-lookup"><span data-stu-id="fada9-105">toogo through hello same tutorial using other supported tools, use hello dropdown list on hello top of this section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fada9-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fada9-106">Prerequisites</span></span>
<span data-ttu-id="fada9-107">Ez az oktatóanyag elkezdéséhez a következő elemek hello kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="fada9-107">Before you begin this tutorial, you must have hello following items:</span></span>

* <span data-ttu-id="fada9-108">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="fada9-108">**An Azure subscription**.</span></span> <span data-ttu-id="fada9-109">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fada9-109">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="fada9-110">**Azure CLI 2.0**.</span><span class="sxs-lookup"><span data-stu-id="fada9-110">**Azure CLI 2.0**.</span></span> <span data-ttu-id="fada9-111">Lásd: [Install and configure Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (Az Azure parancssori felület telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="fada9-111">See [Install and configure Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="fada9-112">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="fada9-112">Log in tooAzure</span></span>

<span data-ttu-id="fada9-113">az Azure-előfizetés tooyour toolog:</span><span class="sxs-lookup"><span data-stu-id="fada9-113">toolog in tooyour Azure subscription:</span></span>

```
azurecli
az login
```

<span data-ttu-id="fada9-114">Kért toobrowse tooa URL-címet, és egy hitelesítési kód megadására.</span><span class="sxs-lookup"><span data-stu-id="fada9-114">You are requested toobrowse tooa URL, and enter an authentication code.</span></span>  <span data-ttu-id="fada9-115">És a hitelesítő adatok hello utasításokat tooenter kövesse.</span><span class="sxs-lookup"><span data-stu-id="fada9-115">And then follow hello instructions tooenter your credentials.</span></span>

<span data-ttu-id="fada9-116">Ha már bejelentkezett, hello bejelentkezési parancs felsorolja az előfizetések.</span><span class="sxs-lookup"><span data-stu-id="fada9-116">Once you have logged in, hello login command lists your subscriptions.</span></span>

<span data-ttu-id="fada9-117">az adott előfizetést toouse:</span><span class="sxs-lookup"><span data-stu-id="fada9-117">toouse a specific subscription:</span></span>

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a><span data-ttu-id="fada9-118">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="fada9-118">Create Data Lake Analytics account</span></span>
<span data-ttu-id="fada9-119">A feladatok futtatásához rendelkeznie kell egy Data Lake Analytics-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="fada9-119">You need a Data Lake Analytics account before you can run any jobs.</span></span> <span data-ttu-id="fada9-120">toocreate Data Lake Analytics-fiók, meg kell adnia a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="fada9-120">toocreate a Data Lake Analytics account, you must specify hello following items:</span></span>

* <span data-ttu-id="fada9-121">**Azure-erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="fada9-121">**Azure Resource Group**.</span></span> <span data-ttu-id="fada9-122">A Data Lake Analytics-fiókot egy Azure-erőforráscsoportban kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="fada9-122">A Data Lake Analytics account must be created within an Azure Resource group.</span></span> <span data-ttu-id="fada9-123">[Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) toowork hello erőforrásokkal egy csoportként az alkalmazás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="fada9-123">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) enables you toowork with hello resources in your application as a group.</span></span> <span data-ttu-id="fada9-124">Telepítéséhez, frissítéséhez, vagy törölje az összes hello erőforrások egyetlen, koordinált műveletben az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="fada9-124">You can deploy, update, or delete all of hello resources for your application in a single, coordinated operation.</span></span>  

<span data-ttu-id="fada9-125">toolist hello meglévő erőforráscsoportok az előfizetéshez tartozó:</span><span class="sxs-lookup"><span data-stu-id="fada9-125">toolist hello existing resource groups under your subscription:</span></span>

```
az group list
```

<span data-ttu-id="fada9-126">Új erőforráscsoport toocreate:</span><span class="sxs-lookup"><span data-stu-id="fada9-126">toocreate a new resource group:</span></span>

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* <span data-ttu-id="fada9-127">**A Data Lake Analytics-fiók neve**.</span><span class="sxs-lookup"><span data-stu-id="fada9-127">**Data Lake Analytics account name**.</span></span> <span data-ttu-id="fada9-128">Minden Data Lake Analytics-fiók rendelkezik egy névvel.</span><span class="sxs-lookup"><span data-stu-id="fada9-128">Each Data Lake Analytics account has a name.</span></span>
* <span data-ttu-id="fada9-129">**Hely**.</span><span class="sxs-lookup"><span data-stu-id="fada9-129">**Location**.</span></span> <span data-ttu-id="fada9-130">Hello Azure-adatközpont, amely támogatja a Data Lake Analytics egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="fada9-130">Use one of hello Azure data centers that supports Data Lake Analytics.</span></span>
* <span data-ttu-id="fada9-131">**Alapértelmezett Data Lake Store-fiók:** minden Data Lake Analytics-fiókhoz tartozik egy alapértelmezett Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="fada9-131">**Default Data Lake Store account**: Each Data Lake Analytics account has a default Data Lake Store account.</span></span>

<span data-ttu-id="fada9-132">toolist hello meglévő Data Lake Store-fiók:</span><span class="sxs-lookup"><span data-stu-id="fada9-132">toolist hello existing Data Lake Store account:</span></span>

```
az dls account list
```

<span data-ttu-id="fada9-133">új Data Lake Store-fiók toocreate:</span><span class="sxs-lookup"><span data-stu-id="fada9-133">toocreate a new Data Lake Store account:</span></span>

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

<span data-ttu-id="fada9-134">A következő szintaxist toocreate Data Lake Analytics-fiók hello használata:</span><span class="sxs-lookup"><span data-stu-id="fada9-134">Use hello following syntax toocreate a Data Lake Analytics account:</span></span>

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

<span data-ttu-id="fada9-135">Fiók létrehozása után a következő parancsok toolist hello fiókok hello használata, és a fiók adatainak megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="fada9-135">After creating an account, you can use hello following commands toolist hello accounts and show account details:</span></span>

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-toodata-lake-store"></a><span data-ttu-id="fada9-136">TooData Lake adattárban feltöltése</span><span class="sxs-lookup"><span data-stu-id="fada9-136">Upload data tooData Lake Store</span></span>
<span data-ttu-id="fada9-137">Az alábbi oktatóanyagban keresési naplókat fog feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="fada9-137">In this tutorial, you process some search logs.</span></span>  <span data-ttu-id="fada9-138">hello keresési napló tárolható Data Lake-adattárban vagy Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="fada9-138">hello search log can be stored in either Data Lake store or Azure Blob storage.</span></span>

<span data-ttu-id="fada9-139">hello Azure portál felhasználói felületet biztosít néhány minta az fájlok toohello alapértelmezett Data Lake Store fiókja, többek között a napló fájl másolása.</span><span class="sxs-lookup"><span data-stu-id="fada9-139">hello Azure portal provides a user interface for copying some sample data files toohello default Data Lake Store account, which include a search log file.</span></span> <span data-ttu-id="fada9-140">Lásd: [forrásadatok előkészítése](data-lake-analytics-get-started-portal.md) tooupload hello adatok toohello alapértelmezett Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="fada9-140">See [Prepare source data](data-lake-analytics-get-started-portal.md) tooupload hello data toohello default Data Lake Store account.</span></span>

<span data-ttu-id="fada9-141">tooupload fájlokat a CLI 2.0, a következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="fada9-141">tooupload files using CLI 2.0, use hello following commands:</span></span>

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

<span data-ttu-id="fada9-142">A Data Lake Analytics az Azure Blob Storage-hoz is rendelkezik hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="fada9-142">Data Lake Analytics can also access Azure Blob storage.</span></span>  <span data-ttu-id="fada9-143">Adattárolás tooAzure Blob feltöltése, lásd: [az Azure Storage Azure CLI használata hello](../storage/common/storage-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="fada9-143">For uploading data tooAzure Blob storage, see [Using hello Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md).</span></span>

## <a name="submit-data-lake-analytics-jobs"></a><span data-ttu-id="fada9-144">Data Lake Analytics-feladatok küldése</span><span class="sxs-lookup"><span data-stu-id="fada9-144">Submit Data Lake Analytics jobs</span></span>
<span data-ttu-id="fada9-145">Data Lake Analytics-feladatok hello hello U-SQL nyelv nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="fada9-145">hello Data Lake Analytics jobs are written in hello U-SQL language.</span></span> <span data-ttu-id="fada9-146">További információk a U-SQL, toolearn lásd [Ismerkedés a U-SQL nyelv](data-lake-analytics-u-sql-get-started.md) és [U-SQL nyelv eence](http://go.microsoft.com/fwlink/?LinkId=691348).</span><span class="sxs-lookup"><span data-stu-id="fada9-146">toolearn more about U-SQL, see [Get started with U-SQL language](data-lake-analytics-u-sql-get-started.md) and [U-SQL language eence](http://go.microsoft.com/fwlink/?LinkId=691348).</span></span>

<span data-ttu-id="fada9-147">**a Data Lake Analytics-feladatparancsfájl toocreate**</span><span class="sxs-lookup"><span data-stu-id="fada9-147">**toocreate a Data Lake Analytics job script**</span></span>

<span data-ttu-id="fada9-148">Hozzon létre egy szövegfájlt az alábbi U-SQL-parancsfájlt, és mentse a hello szöveges fájl tooyour munkaállomáson:</span><span class="sxs-lookup"><span data-stu-id="fada9-148">Create a text file with following U-SQL script, and save hello text file tooyour workstation:</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    too"/data.csv"
    USING Outputters.Csv();
```

<span data-ttu-id="fada9-149">A U-SQL-parancsfájl beolvassa hello forrásadatfájlt **Extractors.Tsv()**, majd létrehoz egy csv fájl használatával **Outputters.Csv()**.</span><span class="sxs-lookup"><span data-stu-id="fada9-149">This U-SQL script reads hello source data file using **Extractors.Tsv()**, and then creates a csv file using **Outputters.Csv()**.</span></span>

<span data-ttu-id="fada9-150">Hello két elérési utak nem módosítható, kivéve, ha hello forrásfájl átmásolja egy másik helyre.</span><span class="sxs-lookup"><span data-stu-id="fada9-150">Don't modify hello two paths unless you copy hello source file into a different location.</span></span>  <span data-ttu-id="fada9-151">A Data Lake Analytics hello kimeneti mappát hoz létre, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="fada9-151">Data Lake Analytics creates hello output folder if it doesn't exist.</span></span>

<span data-ttu-id="fada9-152">Egyszerűbb toouse relatív elérési utak alapértelmezett Data Lake Store-fiók tárolt fájlok is.</span><span class="sxs-lookup"><span data-stu-id="fada9-152">It is simpler toouse relative paths for files stored in default Data Lake Store accounts.</span></span> <span data-ttu-id="fada9-153">De használhat abszolút elérési utakat is.</span><span class="sxs-lookup"><span data-stu-id="fada9-153">You can also use absolute paths.</span></span>  <span data-ttu-id="fada9-154">Példa:</span><span class="sxs-lookup"><span data-stu-id="fada9-154">For example:</span></span>

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

<span data-ttu-id="fada9-155">A társított tárfiókokban tooaccess fájlok abszolút elérési utakat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="fada9-155">You must use absolute paths tooaccess files in linked Storage accounts.</span></span>  <span data-ttu-id="fada9-156">hello csatolt Azure Storage-fiókban tárolt fájlok szintaxisa a következő:</span><span class="sxs-lookup"><span data-stu-id="fada9-156">hello syntax for files stored in linked Azure Storage account is:</span></span>

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> <span data-ttu-id="fada9-157">A nyilvános blobokat tartalmazó Azure Blob-tárolók nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="fada9-157">Azure Blob container with public blobs are not supported.</span></span>      
> <span data-ttu-id="fada9-158">A nyilvános tárolókat tartalmazó Azure Blob-tárolók nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="fada9-158">Azure Blob container with public containers are not supported.</span></span>      
>

<span data-ttu-id="fada9-159">**toosubmit feladatok**</span><span class="sxs-lookup"><span data-stu-id="fada9-159">**toosubmit jobs**</span></span>

<span data-ttu-id="fada9-160">A következő szintaxist toosubmit egy feladat hello használata.</span><span class="sxs-lookup"><span data-stu-id="fada9-160">Use hello following syntax toosubmit a job.</span></span>

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

<span data-ttu-id="fada9-161">Példa:</span><span class="sxs-lookup"><span data-stu-id="fada9-161">For example:</span></span>

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

<span data-ttu-id="fada9-162">**toolist feladatok és a feladat részleteinek megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="fada9-162">**toolist jobs and show job details**</span></span>

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

<span data-ttu-id="fada9-163">**toocancel feladatok**</span><span class="sxs-lookup"><span data-stu-id="fada9-163">**toocancel jobs**</span></span>

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a><span data-ttu-id="fada9-164">Feladatok eredményeinek lekérése</span><span class="sxs-lookup"><span data-stu-id="fada9-164">Retrieve job results</span></span>

<span data-ttu-id="fada9-165">A feladat befejezése után használja a következő parancsok toolist hello kimeneti fájlok hello, és hello fájlok letöltése:</span><span class="sxs-lookup"><span data-stu-id="fada9-165">After a job is completed, you can use hello following commands toolist hello output files, and download hello files:</span></span>

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

<span data-ttu-id="fada9-166">Példa:</span><span class="sxs-lookup"><span data-stu-id="fada9-166">For example:</span></span>

```
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="pipelines-and-recurrences"></a><span data-ttu-id="fada9-167">Folyamatok és ismétlődések</span><span class="sxs-lookup"><span data-stu-id="fada9-167">Pipelines and recurrences</span></span>

<span data-ttu-id="fada9-168">**Folyamatok és ismétlődések adatainak lekérése**</span><span class="sxs-lookup"><span data-stu-id="fada9-168">**Get information about pipelines and recurrences**</span></span>

<span data-ttu-id="fada9-169">Használjon hello `az dla job pipeline` toosee hello csővezeték korábban információkat feladatok parancsok.</span><span class="sxs-lookup"><span data-stu-id="fada9-169">Use hello `az dla job pipeline` commands toosee hello pipeline information previously submitted jobs.</span></span>

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

<span data-ttu-id="fada9-170">Használjon hello `az dla job recurrence` toosee hello ismétlődési információkat az előzőleg elküldött feladatok parancsokat.</span><span class="sxs-lookup"><span data-stu-id="fada9-170">Use hello `az dla job recurrence` commands toosee hello recurrence information for previously submitted jobs.</span></span>

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="next-steps"></a><span data-ttu-id="fada9-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fada9-171">Next steps</span></span>

* <span data-ttu-id="fada9-172">Tekintse meg a Data Lake Analytics CLI 2.0 referenciadokumentum, toosee hello [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).</span><span class="sxs-lookup"><span data-stu-id="fada9-172">toosee hello Data Lake Analytics CLI 2.0 reference document, see [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).</span></span>
* <span data-ttu-id="fada9-173">Tekintse meg a Data Lake Store CLI 2.0 referenciadokumentum, toosee hello [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).</span><span class="sxs-lookup"><span data-stu-id="fada9-173">toosee hello Data Lake Store CLI 2.0 reference document, see [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).</span></span>
* <span data-ttu-id="fada9-174">egy összetettebb lekérdezés toosee lásd [elemzés webhely naplózza az Azure Data Lake Analytics használatával](data-lake-analytics-analyze-weblogs.md).</span><span class="sxs-lookup"><span data-stu-id="fada9-174">toosee a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
