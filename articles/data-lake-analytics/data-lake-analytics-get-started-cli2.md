---
title: "Az Azure Data Lake Analytics használatának első lépései az Azure CLI 2.0 használatával | Microsoft Docs"
description: "Ebből a cikkből megtudhatja, hogyan használhatja az Azure CLI 2.0-s verzióját egy Data Lake Analytics-fiók létrehozásához, egy Data Lake Analytics-feladat létrehozásához U-SQL használatával, valamint a feladat elküldéséhez. "
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
ms.openlocfilehash: fe2b84aac718ff5eddd4d73b5dc2120362952c1e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-data-lake-analytics-using-azure-cli-20"></a><span data-ttu-id="ced02-103">Az Azure Data Lake Analytics használatának első lépései az Azure parancssori felületének 2.0-s verziójával</span><span class="sxs-lookup"><span data-stu-id="ced02-103">Get started with Azure Data Lake Analytics using Azure CLI 2.0</span></span>
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

<span data-ttu-id="ced02-104">Az oktatóanyaggal egy olyan feladatot fog elkészíteni, amely beolvas egy tabulátorral elválasztott értékeket tartalmazó fájlt (TSV), majd konvertálja azt egy vesszővel elválasztott értékeket tartalmazó fájllá (CSV).</span><span class="sxs-lookup"><span data-stu-id="ced02-104">In this tutorial, you develop a job that reads a tab separated values (TSV) file and converts it into a comma-separated values (CSV) file.</span></span> <span data-ttu-id="ced02-105">Ha ugyanezt az oktatóanyagot más támogatott eszközök használatával szeretné elvégezni, használja a legördülő listákat a szakasz tetején.</span><span class="sxs-lookup"><span data-stu-id="ced02-105">To go through the same tutorial using other supported tools, use the dropdown list on the top of this section.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ced02-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ced02-106">Prerequisites</span></span>
<span data-ttu-id="ced02-107">Az oktatóanyag elkezdéséhez az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="ced02-107">Before you begin this tutorial, you must have the following items:</span></span>

* <span data-ttu-id="ced02-108">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="ced02-108">**An Azure subscription**.</span></span> <span data-ttu-id="ced02-109">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ced02-109">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ced02-110">**Azure CLI 2.0**.</span><span class="sxs-lookup"><span data-stu-id="ced02-110">**Azure CLI 2.0**.</span></span> <span data-ttu-id="ced02-111">Lásd: [Install and configure Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) (Az Azure parancssori felület telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="ced02-111">See [Install and configure Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="log-in-to-azure"></a><span data-ttu-id="ced02-112">Jelentkezzen be az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="ced02-112">Log in to Azure</span></span>

<span data-ttu-id="ced02-113">Jelentkezzen be az Azure-előfizetésébe:</span><span class="sxs-lookup"><span data-stu-id="ced02-113">To log in to your Azure subscription:</span></span>

```
azurecli
az login
```

<span data-ttu-id="ced02-114">Fel kell keresnie egy URL-címet, és ott meg kell adnia egy hitelesítő kódot.</span><span class="sxs-lookup"><span data-stu-id="ced02-114">You are requested to browse to a URL, and enter an authentication code.</span></span>  <span data-ttu-id="ced02-115">Ezután kövesse az útmutatásokat a hitelesítő adatai megadásához.</span><span class="sxs-lookup"><span data-stu-id="ced02-115">And then follow the instructions to enter your credentials.</span></span>

<span data-ttu-id="ced02-116">Miután bejelentkezett, a login paranccsal listázhatja az előfizetéseit.</span><span class="sxs-lookup"><span data-stu-id="ced02-116">Once you have logged in, the login command lists your subscriptions.</span></span>

<span data-ttu-id="ced02-117">Egy adott előfizetés használata:</span><span class="sxs-lookup"><span data-stu-id="ced02-117">To use a specific subscription:</span></span>

```
az account set --subscription <subscription id>
```

## <a name="create-data-lake-analytics-account"></a><span data-ttu-id="ced02-118">Data Lake Analytics-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="ced02-118">Create Data Lake Analytics account</span></span>
<span data-ttu-id="ced02-119">A feladatok futtatásához rendelkeznie kell egy Data Lake Analytics-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="ced02-119">You need a Data Lake Analytics account before you can run any jobs.</span></span> <span data-ttu-id="ced02-120">A Data Lake Analytics-fiók létrehozásához az alábbiakat kell megadnia:</span><span class="sxs-lookup"><span data-stu-id="ced02-120">To create a Data Lake Analytics account, you must specify the following items:</span></span>

* <span data-ttu-id="ced02-121">**Azure-erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="ced02-121">**Azure Resource Group**.</span></span> <span data-ttu-id="ced02-122">A Data Lake Analytics-fiókot egy Azure-erőforráscsoportban kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="ced02-122">A Data Lake Analytics account must be created within an Azure Resource group.</span></span> <span data-ttu-id="ced02-123">Az [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) lehetővé teszi, hogy az alkalmazásában lévő erőforrásokat csoportként használja.</span><span class="sxs-lookup"><span data-stu-id="ced02-123">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) enables you to work with the resources in your application as a group.</span></span> <span data-ttu-id="ced02-124">Az alkalmazás összes erőforrását egyetlen, koordinált műveletben telepítheti, frissítheti vagy törölheti.</span><span class="sxs-lookup"><span data-stu-id="ced02-124">You can deploy, update, or delete all of the resources for your application in a single, coordinated operation.</span></span>  

<span data-ttu-id="ced02-125">Az előfizetés alá tartozó meglévő erőforráscsoportok megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="ced02-125">To list the existing resource groups under your subscription:</span></span>

```
az group list
```

<span data-ttu-id="ced02-126">Új erőforráscsoport létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ced02-126">To create a new resource group:</span></span>

```
az group create --name "<Resource Group Name>" --location "<Azure Location>"
```

* <span data-ttu-id="ced02-127">**A Data Lake Analytics-fiók neve**.</span><span class="sxs-lookup"><span data-stu-id="ced02-127">**Data Lake Analytics account name**.</span></span> <span data-ttu-id="ced02-128">Minden Data Lake Analytics-fiók rendelkezik egy névvel.</span><span class="sxs-lookup"><span data-stu-id="ced02-128">Each Data Lake Analytics account has a name.</span></span>
* <span data-ttu-id="ced02-129">**Hely**.</span><span class="sxs-lookup"><span data-stu-id="ced02-129">**Location**.</span></span> <span data-ttu-id="ced02-130">Használja az egyik, a Data Lake Analytics szolgáltatást támogató Azure-adatközpontot.</span><span class="sxs-lookup"><span data-stu-id="ced02-130">Use one of the Azure data centers that supports Data Lake Analytics.</span></span>
* <span data-ttu-id="ced02-131">**Alapértelmezett Data Lake Store-fiók:** minden Data Lake Analytics-fiókhoz tartozik egy alapértelmezett Data Lake Store-fiók.</span><span class="sxs-lookup"><span data-stu-id="ced02-131">**Default Data Lake Store account**: Each Data Lake Analytics account has a default Data Lake Store account.</span></span>

<span data-ttu-id="ced02-132">A meglévő Data Lake Store-fiók megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="ced02-132">To list the existing Data Lake Store account:</span></span>

```
az dls account list
```

<span data-ttu-id="ced02-133">Új Data Lake Store-fiók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="ced02-133">To create a new Data Lake Store account:</span></span>

```azurecli
az dls account create --account "<Data Lake Store Account Name>" --resource-group "<Resource Group Name>"
```

<span data-ttu-id="ced02-134">Egy Data Lake Analytics-fiók létrehozásához használja a következő szintaxist:</span><span class="sxs-lookup"><span data-stu-id="ced02-134">Use the following syntax to create a Data Lake Analytics account:</span></span>

```
az dla account create --account "<Data Lake Analytics Account Name>" --resource-group "<Resource Group Name>" --location "<Azure location>" --default-data-lake-store "<Default Data Lake Store Account Name>"
```

<span data-ttu-id="ced02-135">A fiók létrehozása után az alábbi parancsok használatával listázhatja a fiókokat és jelentheti meg azok részleteit:</span><span class="sxs-lookup"><span data-stu-id="ced02-135">After creating an account, you can use the following commands to list the accounts and show account details:</span></span>

```
az dla account list
az dla account show --account "<Data Lake Analytics Account Name>"            
```

## <a name="upload-data-to-data-lake-store"></a><span data-ttu-id="ced02-136">Adatok feltöltése a Data Lake Store-ba</span><span class="sxs-lookup"><span data-stu-id="ced02-136">Upload data to Data Lake Store</span></span>
<span data-ttu-id="ced02-137">Az alábbi oktatóanyagban keresési naplókat fog feldolgozni.</span><span class="sxs-lookup"><span data-stu-id="ced02-137">In this tutorial, you process some search logs.</span></span>  <span data-ttu-id="ced02-138">A keresési napló tárolható Data Lake-adattárban vagy Azure Blob Storage-ban.</span><span class="sxs-lookup"><span data-stu-id="ced02-138">The search log can be stored in either Data Lake store or Azure Blob storage.</span></span>

<span data-ttu-id="ced02-139">Az Azure Portal felhasználói felületet biztosít bizonyos mintaadatfájlok alapértelmezett Data Lake Store-fiókba való másolásához. Ilyen adatfájl a keresési napló is.</span><span class="sxs-lookup"><span data-stu-id="ced02-139">The Azure portal provides a user interface for copying some sample data files to the default Data Lake Store account, which include a search log file.</span></span> <span data-ttu-id="ced02-140">Az adatok alapértelmezett Data Lake Store-fiókba való feltöltéséhez lásd a [Forrásadatok előkészítése](data-lake-analytics-get-started-portal.md) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="ced02-140">See [Prepare source data](data-lake-analytics-get-started-portal.md) to upload the data to the default Data Lake Store account.</span></span>

<span data-ttu-id="ced02-141">A fájloknak a parancssori felület 2.0-s verziójával történő feltöltéséhez használja az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="ced02-141">To upload files using CLI 2.0, use the following commands:</span></span>

```
az dls fs upload --account "<Data Lake Store Account Name>" --source-path "<Source File Path>" --destination-path "<Destination File Path>"
az dls fs list --account "<Data Lake Store Account Name>" --path "<Path>"
```

<span data-ttu-id="ced02-142">A Data Lake Analytics az Azure Blob Storage-hoz is rendelkezik hozzáféréssel.</span><span class="sxs-lookup"><span data-stu-id="ced02-142">Data Lake Analytics can also access Azure Blob storage.</span></span>  <span data-ttu-id="ced02-143">A fájlok az Azure Blob Storage-ba történő feltöltéséhez lásd: [Using the Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md) (Az Azure parancssori felület és az Azure Storage használata).</span><span class="sxs-lookup"><span data-stu-id="ced02-143">For uploading data to Azure Blob storage, see [Using the Azure CLI with Azure Storage](../storage/common/storage-azure-cli.md).</span></span>

## <a name="submit-data-lake-analytics-jobs"></a><span data-ttu-id="ced02-144">Data Lake Analytics-feladatok küldése</span><span class="sxs-lookup"><span data-stu-id="ced02-144">Submit Data Lake Analytics jobs</span></span>
<span data-ttu-id="ced02-145">A Data Lake Analytics-feladatok nyelve a U-SQL.</span><span class="sxs-lookup"><span data-stu-id="ced02-145">The Data Lake Analytics jobs are written in the U-SQL language.</span></span> <span data-ttu-id="ced02-146">A U-SQL-ről további tudnivalók [A U-SQL nyelv – első lépések](data-lake-analytics-u-sql-get-started.md) című cikkben és [a U-SQL nyelvvel foglalkozó segédanyagban](http://go.microsoft.com/fwlink/?LinkId=691348) találhatók.</span><span class="sxs-lookup"><span data-stu-id="ced02-146">To learn more about U-SQL, see [Get started with U-SQL language](data-lake-analytics-u-sql-get-started.md) and [U-SQL language eence](http://go.microsoft.com/fwlink/?LinkId=691348).</span></span>

<span data-ttu-id="ced02-147">**Data Lake Analytics-feladatparancsfájl létrehozása**</span><span class="sxs-lookup"><span data-stu-id="ced02-147">**To create a Data Lake Analytics job script**</span></span>

<span data-ttu-id="ced02-148">Hozzon létre egy szövegfájlt az alábbi U-SQL-parancsfájl alapján, és mentse a szövegfájlt a munkaállomáson:</span><span class="sxs-lookup"><span data-stu-id="ced02-148">Create a text file with following U-SQL script, and save the text file to your workstation:</span></span>

```
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

<span data-ttu-id="ced02-149">Ez a U-SQL-parancsfájl beolvassa a forrásadatfájlt az **Extractors.Tsv()** segítségével, majd létrehoz egy csv-fájlt az **Outputters.Csv()** használatával.</span><span class="sxs-lookup"><span data-stu-id="ced02-149">This U-SQL script reads the source data file using **Extractors.Tsv()**, and then creates a csv file using **Outputters.Csv()**.</span></span>

<span data-ttu-id="ced02-150">Ne módosítsa az elérési utat, kivéve, ha átmásolja a forrásfájlt egy másik helyre.</span><span class="sxs-lookup"><span data-stu-id="ced02-150">Don't modify the two paths unless you copy the source file into a different location.</span></span>  <span data-ttu-id="ced02-151">A Data Lake Analytics létrehozza a kimeneti mappát, ha az még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="ced02-151">Data Lake Analytics creates the output folder if it doesn't exist.</span></span>

<span data-ttu-id="ced02-152">Egyszerűbb relatív útvonalakat használni az alapértelmezett Data Lake Store-fiókokban tárolt fájlokhoz.</span><span class="sxs-lookup"><span data-stu-id="ced02-152">It is simpler to use relative paths for files stored in default Data Lake Store accounts.</span></span> <span data-ttu-id="ced02-153">De használhat abszolút elérési utakat is.</span><span class="sxs-lookup"><span data-stu-id="ced02-153">You can also use absolute paths.</span></span>  <span data-ttu-id="ced02-154">Példa:</span><span class="sxs-lookup"><span data-stu-id="ced02-154">For example:</span></span>

```
adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv
```

<span data-ttu-id="ced02-155">A társított tárfiókokban lévő fájlok eléréséhez abszolút elérési utakat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="ced02-155">You must use absolute paths to access files in linked Storage accounts.</span></span>  <span data-ttu-id="ced02-156">A társított Azure Storage-fiókban tárolt fájlok szintaxisa:</span><span class="sxs-lookup"><span data-stu-id="ced02-156">The syntax for files stored in linked Azure Storage account is:</span></span>

```
wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv
```

> [!NOTE]
> <span data-ttu-id="ced02-157">A nyilvános blobokat tartalmazó Azure Blob-tárolók nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="ced02-157">Azure Blob container with public blobs are not supported.</span></span>      
> <span data-ttu-id="ced02-158">A nyilvános tárolókat tartalmazó Azure Blob-tárolók nem támogatottak.</span><span class="sxs-lookup"><span data-stu-id="ced02-158">Azure Blob container with public containers are not supported.</span></span>      
>

<span data-ttu-id="ced02-159">**Feladatok elküldése**</span><span class="sxs-lookup"><span data-stu-id="ced02-159">**To submit jobs**</span></span>

<span data-ttu-id="ced02-160">Feladatok elküldéséhez használja a következő szintaxist.</span><span class="sxs-lookup"><span data-stu-id="ced02-160">Use the following syntax to submit a job.</span></span>

```
az dla job submit --account "<Data Lake Analytics Account Name>" --job-name "<Job Name>" --script "<Script Path and Name>"
```

<span data-ttu-id="ced02-161">Példa:</span><span class="sxs-lookup"><span data-stu-id="ced02-161">For example:</span></span>

```
az dla job submit --account "myadlaaccount" --job-name "myadlajob" --script @"C:\DLA\myscript.txt"
```

<span data-ttu-id="ced02-162">**Feladatok listázása és részleteik megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="ced02-162">**To list jobs and show job details**</span></span>

```
azurecli
az dla job list --account "<Data Lake Analytics Account Name>"
az dla job show --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

<span data-ttu-id="ced02-163">**Feladatok megszakítása**</span><span class="sxs-lookup"><span data-stu-id="ced02-163">**To cancel jobs**</span></span>

```
az dla job cancel --account "<Data Lake Analytics Account Name>" --job-identity "<Job Id>"
```

## <a name="retrieve-job-results"></a><span data-ttu-id="ced02-164">Feladatok eredményeinek lekérése</span><span class="sxs-lookup"><span data-stu-id="ced02-164">Retrieve job results</span></span>

<span data-ttu-id="ced02-165">A feladat befejezése után az alábbi parancsok segítségével listázhatja ki a kimeneti fájlokat, és töltheti le a fájlokat:</span><span class="sxs-lookup"><span data-stu-id="ced02-165">After a job is completed, you can use the following commands to list the output files, and download the files:</span></span>

```
az dls fs list --account "<Data Lake Store Account Name>" --source-path "/Output" --destination-path "<Destintion>"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv"
az dls fs preview --account "<Data Lake Store Account Name>" --path "/Output/SearchLog-from-Data-Lake.csv" --length 128 --offset 0
az dls fs downlod --account "<Data Lake Store Account Name>" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "<Destination Path and File Name>"
```

<span data-ttu-id="ced02-166">Példa:</span><span class="sxs-lookup"><span data-stu-id="ced02-166">For example:</span></span>

```
az dls fs downlod --account "myadlsaccount" --source-path "/Output/SearchLog-from-Data-Lake.csv" --destintion-path "C:\DLA\myfile.csv"
```

## <a name="pipelines-and-recurrences"></a><span data-ttu-id="ced02-167">Folyamatok és ismétlődések</span><span class="sxs-lookup"><span data-stu-id="ced02-167">Pipelines and recurrences</span></span>

<span data-ttu-id="ced02-168">**Folyamatok és ismétlődések adatainak lekérése**</span><span class="sxs-lookup"><span data-stu-id="ced02-168">**Get information about pipelines and recurrences**</span></span>

<span data-ttu-id="ced02-169">A korábban elküldött feladatok folyamatadatait az `az dla job pipeline` paranccsal tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="ced02-169">Use the `az dla job pipeline` commands to see the pipeline information previously submitted jobs.</span></span>

```
az dla job pipeline list --account "<Data Lake Analytics Account Name>"

az dla job pipeline show --account "<Data Lake Analytics Account Name>" --pipeline-identity "<Pipeline ID>"
```

<span data-ttu-id="ced02-170">A korábban elküldött feladatok ismétlődési adatait az `az dla job recurrence` paranccsal tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="ced02-170">Use the `az dla job recurrence` commands to see the recurrence information for previously submitted jobs.</span></span>

```
az dla job recurrence list --account "<Data Lake Analytics Account Name>"

az dla job recurrence show --account "<Data Lake Analytics Account Name>" --recurrence-identity "<Recurrence ID>"
```

## <a name="next-steps"></a><span data-ttu-id="ced02-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ced02-171">Next steps</span></span>

* <span data-ttu-id="ced02-172">A Data Lake Analytics parancssori felületének 2.0-s verziójával foglalkozó referenciadokumentum megtekintéséhez lásd: [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).</span><span class="sxs-lookup"><span data-stu-id="ced02-172">To see the Data Lake Analytics CLI 2.0 reference document, see [Data Lake Analytics](https://docs.microsoft.com/cli/azure/dla).</span></span>
* <span data-ttu-id="ced02-173">A Data Lake Store parancssori felületének 2.0-s verziójával foglalkozó referenciadokumentum megtekintéséhez lásd: [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).</span><span class="sxs-lookup"><span data-stu-id="ced02-173">To see the Data Lake Store CLI 2.0 reference document, see [Data Lake Store](https://docs.microsoft.com/cli/azure/dls).</span></span>
* <span data-ttu-id="ced02-174">Egy összetettebb lekérdezés megtekintéséhez lásd: [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md) (Webhelyek naplóinak elemzése az Azure Data Lake Analytics használatával).</span><span class="sxs-lookup"><span data-stu-id="ced02-174">To see a more complex query, see [Analyze Website logs using Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).</span></span>
