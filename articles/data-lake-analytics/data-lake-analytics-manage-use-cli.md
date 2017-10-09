---
title: "Azure Data Lake Analytics Azure parancssori felület használatának aaaManage |} Microsoft Docs"
description: "Megtudhatja, hogyan toomanage Data Lake Analytics-fiókok, adatforrások, feladatok és a felhasználók Azure parancssori felület használatával"
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 4e5a3a0a-6d7f-43ed-aeb5-c3b3979a1e0a
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: 0af1f89080739b39f6980989b7694734cc135715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-data-lake-analytics-using-azure-command-line-interface-cli"></a><span data-ttu-id="46285-103">Azure parancssori felület (CLI) használatával Azure Data Lake Analytics kezelése</span><span class="sxs-lookup"><span data-stu-id="46285-103">Manage Azure Data Lake Analytics using Azure Command-line Interface (CLI)</span></span>
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

<span data-ttu-id="46285-104">Ismerje meg, hogyan toomanage Azure Data Lake Analytics-fiókok, adatforrások, felhasználók és használó feladatok hello Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="46285-104">Learn how toomanage Azure Data Lake Analytics accounts, data sources, users, and jobs using hello Azure CLI.</span></span> <span data-ttu-id="46285-105">más eszközök használatával toosee felügyeleti témakörök kattintson hello lapválasztóra.</span><span class="sxs-lookup"><span data-stu-id="46285-105">toosee management topics using other tools, click hello tab select above.</span></span>


<span data-ttu-id="46285-106">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="46285-106">**Prerequisites**</span></span>

<span data-ttu-id="46285-107">Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="46285-107">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="46285-108">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="46285-108">**An Azure subscription**.</span></span> <span data-ttu-id="46285-109">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="46285-109">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="46285-110">**Azure parancssori felület (CLI)**.</span><span class="sxs-lookup"><span data-stu-id="46285-110">**Azure CLI**.</span></span> <span data-ttu-id="46285-111">Lásd: [Install and configure Azure CLI](../cli-install-nodejs.md) (Az Azure parancssori felület telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="46285-111">See [Install and configure Azure CLI](../cli-install-nodejs.md).</span></span>
  * <span data-ttu-id="46285-112">Töltse le és telepítse a hello **kiadás előtti** [Azure CLI-eszközeit](https://github.com/MicrosoftBigData/AzureDataLake/releases) a rendezés toocomplete ebben a bemutatóban.</span><span class="sxs-lookup"><span data-stu-id="46285-112">Download and install hello **pre-release** [Azure CLI tools](https://github.com/MicrosoftBigData/AzureDataLake/releases) in order toocomplete this demo.</span></span>
* <span data-ttu-id="46285-113">**Hitelesítési**használatával hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="46285-113">**Authentication**, using hello following command:</span></span>
  
        azure login
    <span data-ttu-id="46285-114">Munkahelyi vagy iskolai fiókkal hitelesítésről további információkért lásd: [hello Azure CLI Azure-előfizetés tooan kapcsolódó](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="46285-114">For more information on authenticating using a work or school account, see [Connect tooan Azure subscription from hello Azure CLI](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="46285-115">**Kapcsoló toohello Azure Resource Manager módra**használatával hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="46285-115">**Switch toohello Azure Resource Manager mode**, using hello following command:</span></span>
  
        azure config mode arm

<span data-ttu-id="46285-116">**toolist hello Data Lake Store és a Data Lake Analytics parancsokat:**</span><span class="sxs-lookup"><span data-stu-id="46285-116">**toolist hello Data Lake Store and Data Lake Analytics commands:**</span></span>

    azure datalake store
    azure datalake analytics

<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-accounts"></a><span data-ttu-id="46285-117">Fiókok kezelése</span><span class="sxs-lookup"><span data-stu-id="46285-117">Manage accounts</span></span>
<span data-ttu-id="46285-118">Minden Data Lake Analytics-feladatok futtatásához rendelkeznie kell egy Data Lake Analytics-fiók.</span><span class="sxs-lookup"><span data-stu-id="46285-118">Before running any Data Lake Analytics jobs, you must have a Data Lake Analytics account.</span></span> <span data-ttu-id="46285-119">Azure HDInsight eltérően nem kell fizetnie az Analytics-fiók, amikor nem fut egy feladat.</span><span class="sxs-lookup"><span data-stu-id="46285-119">Unlike Azure HDInsight, you don't pay for an Analytics account when it is not running a job.</span></span>  <span data-ttu-id="46285-120">Csak kell fizetnie, ha egy feladat fut. hello ideje.</span><span class="sxs-lookup"><span data-stu-id="46285-120">You only pay for hello time when it is running a job.</span></span>  <span data-ttu-id="46285-121">További információkért lásd: [Azure Data Lake Analytics áttekintése](data-lake-analytics-overview.md).</span><span class="sxs-lookup"><span data-stu-id="46285-121">For more information, see [Azure Data Lake Analytics Overview](data-lake-analytics-overview.md).</span></span>  

### <a name="create-accounts"></a><span data-ttu-id="46285-122">Fiókok létrehozása</span><span class="sxs-lookup"><span data-stu-id="46285-122">Create accounts</span></span>
      azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"


### <a name="update-accounts"></a><span data-ttu-id="46285-123">Fiókok frissítése</span><span class="sxs-lookup"><span data-stu-id="46285-123">Update accounts</span></span>
<span data-ttu-id="46285-124">hello következő parancs frissíti egy meglévő Data Lake Analytics-fiók hello tulajdonságairól</span><span class="sxs-lookup"><span data-stu-id="46285-124">hello following command updates hello properties of an existing Data Lake Analytics Account</span></span>

    azure datalake analytics account set "<Data Lake Analytics Account Name>"


### <a name="list-accounts"></a><span data-ttu-id="46285-125">Fiókok listázása</span><span class="sxs-lookup"><span data-stu-id="46285-125">List accounts</span></span>
<span data-ttu-id="46285-126">Lista Data Lake Analytics-fiókok</span><span class="sxs-lookup"><span data-stu-id="46285-126">List Data Lake Analytics accounts</span></span> 

    azure datalake analytics account list

<span data-ttu-id="46285-127">Egy adott erőforráscsoportban lista Data Lake Analytics-fiókok</span><span class="sxs-lookup"><span data-stu-id="46285-127">List Data Lake Analytics accounts within a specific resource group</span></span>

    azure datalake analytics account list -g "<Azure Resource Group Name>"

<span data-ttu-id="46285-128">Egy adott Data Lake Analytics-fiók részletek</span><span class="sxs-lookup"><span data-stu-id="46285-128">Get details of a specific Data Lake Analytics account</span></span>

    azure datalake analytics account show -g "<Azure Resource Group Name>" -n "<Data Lake Analytics Account Name>"

### <a name="delete-data-lake-analytics-accounts"></a><span data-ttu-id="46285-129">Törölje a Data Lake Analytics-fiókok</span><span class="sxs-lookup"><span data-stu-id="46285-129">Delete Data Lake Analytics accounts</span></span>
      azure datalake analytics account delete "<Data Lake Analytics Account Name>"


<!-- ################################ -->
<!-- ################################ -->
## <a name="manage-account-data-sources"></a><span data-ttu-id="46285-130">Fiók adatforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="46285-130">Manage account data sources</span></span>
<span data-ttu-id="46285-131">A Data Lake Analytics jelenleg a következő adatforrások hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="46285-131">Data Lake Analytics currently supports hello following data sources:</span></span>

* [<span data-ttu-id="46285-132">Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="46285-132">Azure Data Lake Store</span></span>](../data-lake-store/data-lake-store-overview.md)
* [<span data-ttu-id="46285-133">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="46285-133">Azure Storage</span></span>](../storage/common/storage-introduction.md)

<span data-ttu-id="46285-134">Az Analytics-fiók létrehozásakor ki kell jelölnie egy Azure Data Lake tárolási fiók toobe hello alapértelmezett tárfiók.</span><span class="sxs-lookup"><span data-stu-id="46285-134">When you create an Analytics account, you must designate an Azure Data Lake Storage account toobe hello default storage account.</span></span> <span data-ttu-id="46285-135">hello alapértelmezett ADL tárfiókot használja toostore feladat metaadatok és a feladat naplók.</span><span class="sxs-lookup"><span data-stu-id="46285-135">hello default ADL storage account is used toostore job metadata and job audit logs.</span></span> <span data-ttu-id="46285-136">Az Analytics-fiók létrehozása után hozzáadhat további Data Lake-tárfiókokat és/vagy az Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="46285-136">After you have created an Analytics account, you can add additional Data Lake Storage accounts and/or Azure Storage account.</span></span> 

### <a name="find-hello-default-adl-storage-account"></a><span data-ttu-id="46285-137">Hello alapértelmezett ADL tárfiók keresése</span><span class="sxs-lookup"><span data-stu-id="46285-137">Find hello default ADL storage account</span></span>
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

<span data-ttu-id="46285-138">hello érték megtalálható-e tulajdonságok: datalakeStoreAccount:name.</span><span class="sxs-lookup"><span data-stu-id="46285-138">hello value is listed under properties:datalakeStoreAccount:name.</span></span>

### <a name="add-additional-azure-blob-storage-accounts"></a><span data-ttu-id="46285-139">További Azure Blob storage-fiókok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="46285-139">Add additional Azure Blob storage accounts</span></span>
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -b "<Azure Blob Storage Account Short Name>" -k "<Azure Storage Account Key>"

> [!NOTE]
> <span data-ttu-id="46285-140">Csak a Blob storage rövid nevek használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="46285-140">Only Blob storage short names are supported.</span></span>  <span data-ttu-id="46285-141">Ne használjon teljes Tartománynevet, például "myblob.blob.core.windows.net".</span><span class="sxs-lookup"><span data-stu-id="46285-141">Don't use FQDN, for example "myblob.blob.core.windows.net".</span></span>
> 
> 

### <a name="add-additional-data-lake-store-accounts"></a><span data-ttu-id="46285-142">További Data Lake Store-fiók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="46285-142">Add additional Data Lake Store accounts</span></span>
      azure datalake analytics account datasource add -n "<Data Lake Analytics Account Name>" -l "<Data Lake Store Account Name>" [-d]

<span data-ttu-id="46285-143">[-d] egy választható kapcsoló tooindicate van-e hozzáadni kívánt Data Lake hello hello alapértelmezett Data Lake-fiók.</span><span class="sxs-lookup"><span data-stu-id="46285-143">[-d] is an optional switch tooindicate whether hello Data Lake being added is hello default Data Lake account.</span></span> 

### <a name="update-existing-data-source"></a><span data-ttu-id="46285-144">Meglévő adatforrás frissítése.</span><span class="sxs-lookup"><span data-stu-id="46285-144">Update existing data source</span></span>
<span data-ttu-id="46285-145">egy meglévő Data Lake Store fiók toobe hello alapértelmezett tooset:</span><span class="sxs-lookup"><span data-stu-id="46285-145">tooset an existing Data Lake Store account toobe hello default:</span></span>

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -l "<Azure Data Lake Store Account Name>" -d

<span data-ttu-id="46285-146">a Blob storage meglévő fiókkulcs tooupdate:</span><span class="sxs-lookup"><span data-stu-id="46285-146">tooupdate an existing Blob storage account key:</span></span>

      azure datalake analytics account datasource set -n "<Data Lake Analytics Account Name>" -b "<Blob Storage Account Name>" -k "<New Blob Storage Account Key>"

### <a name="list-data-sources"></a><span data-ttu-id="46285-147">Az adatforrások listája:</span><span class="sxs-lookup"><span data-stu-id="46285-147">List data sources:</span></span>
    azure datalake analytics account show "<Data Lake Analytics Account Name>"

![A Data Lake Analytics lista adatforrás](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-data-source.png)

### <a name="delete-data-sources"></a><span data-ttu-id="46285-149">Adatforrások törlése:</span><span class="sxs-lookup"><span data-stu-id="46285-149">Delete data sources:</span></span>
<span data-ttu-id="46285-150">Data Lake Store-fiók toodelete:</span><span class="sxs-lookup"><span data-stu-id="46285-150">toodelete a Data Lake Store account:</span></span>

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Azure Data Lake Store Account Name>"

<span data-ttu-id="46285-151">a Blob storage-fiók toodelete:</span><span class="sxs-lookup"><span data-stu-id="46285-151">toodelete a Blob storage account:</span></span>

      azure datalake analytics account datasource delete "<Data Lake Analytics Account Name>" "<Blob Storage Account Name>"

## <a name="manage-jobs"></a><span data-ttu-id="46285-152">Feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="46285-152">Manage jobs</span></span>
<span data-ttu-id="46285-153">Data Lake Analytics-fiók egy feladat létrehozása előtt kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="46285-153">You must have a Data Lake Analytics account before you can create a job.</span></span>  <span data-ttu-id="46285-154">További információkért lásd: [kezelése Data Lake Analytics-fiókok](#manage-accounts).</span><span class="sxs-lookup"><span data-stu-id="46285-154">For more information, see [Manage Data Lake Analytics accounts](#manage-accounts).</span></span>

### <a name="list-jobs"></a><span data-ttu-id="46285-155">Lista feladatok</span><span class="sxs-lookup"><span data-stu-id="46285-155">List jobs</span></span>
      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"

![A Data Lake Analytics lista adatforrás](./media/data-lake-analytics-manage-use-cli/data-lake-analytics-list-jobs.png)

### <a name="get-job-details"></a><span data-ttu-id="46285-157">Lekérhet feladatadatokat</span><span class="sxs-lookup"><span data-stu-id="46285-157">Get job details</span></span>
      azure datalake analytics job show -n "<Data Lake Analytics Account Name>" -j "<Job ID>"

### <a name="submit-jobs"></a><span data-ttu-id="46285-158">Feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="46285-158">Submit jobs</span></span>
> [!NOTE]
> <span data-ttu-id="46285-159">hello alapértelmezett prioritás feladat 1000, és a feladat párhuzamossági fokát hello alapértelmezett értéke 1.</span><span class="sxs-lookup"><span data-stu-id="46285-159">hello default priority of a job is 1000, and hello default degree of parallelism for a job is 1.</span></span>
> 
> 

    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

### <a name="cancel-jobs"></a><span data-ttu-id="46285-160">Feladatok megszakítása</span><span class="sxs-lookup"><span data-stu-id="46285-160">Cancel jobs</span></span>
<span data-ttu-id="46285-161">Hello lista parancs toofind hello feladatazonosító használja, és aztán toocancel hello feladat.</span><span class="sxs-lookup"><span data-stu-id="46285-161">Use hello list command toofind hello job id, and then use cancel toocancel hello job.</span></span>

      azure datalake analytics job list -n "<Data Lake Analytics Account Name>"
      azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job ID>"

## <a name="manage-catalog"></a><span data-ttu-id="46285-162">Katalógus kezelése</span><span class="sxs-lookup"><span data-stu-id="46285-162">Manage catalog</span></span>
<span data-ttu-id="46285-163">hello U-SQL catalog használt toostructure adatok és a kód, így azok megoszthatók U-SQL-parancsfájlok.</span><span class="sxs-lookup"><span data-stu-id="46285-163">hello U-SQL catalog is used toostructure data and code so they can be shared by U-SQL scripts.</span></span> <span data-ttu-id="46285-164">hello katalógus lehetővé teszi, hogy a lehető legjobb teljesítmény hello Azure Data Lake-adatokkal szolgálja.</span><span class="sxs-lookup"><span data-stu-id="46285-164">hello catalog enables hello highest performance possible with data in Azure Data Lake.</span></span> <span data-ttu-id="46285-165">További információk: [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md) (U-SQL-katalógus használata).</span><span class="sxs-lookup"><span data-stu-id="46285-165">For more information, see [Use U-SQL catalog](data-lake-analytics-use-u-sql-catalog.md).</span></span>

### <a name="list-catalog-items"></a><span data-ttu-id="46285-166">A szolgáltatáskatalógusban található elemek listája</span><span class="sxs-lookup"><span data-stu-id="46285-166">List catalog items</span></span>
    #List databases
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t database

    #List tables
    azure datalake analytics catalog list -n "<Data Lake Analytics Account Name>" -t table

<span data-ttu-id="46285-167">hello típusok tartalmazza az adatbázis, a séma, a szerelvény, a külső adatforrás, a tábla, a táblázat értékű függvény és a tábla statisztikai adatainak.</span><span class="sxs-lookup"><span data-stu-id="46285-167">hello types include database, schema, assembly, external data source, table, table valued function or table statistics.</span></span>

<!-- ################################ -->
<!-- ################################ -->
## <a name="use-arm-groups"></a><span data-ttu-id="46285-168">ARM-csoportok használata</span><span class="sxs-lookup"><span data-stu-id="46285-168">Use ARM groups</span></span>
<span data-ttu-id="46285-169">Az alkalmazások általában számos összetevőből állnak, például webalkalmazásból, adatbázisból, adatbázis-kiszolgálóból, tárolóból és külső szolgáltatásokból.</span><span class="sxs-lookup"><span data-stu-id="46285-169">Applications are typically made up of many components, for example a web app, database, database server, storage, and 3rd party services.</span></span> <span data-ttu-id="46285-170">Az Azure Resource Manager (ARM) lehetővé teszi az alkalmazás egy csoportot, hivatkozott tooas Azure-erőforráscsoport hello erőforrásokat toowork.</span><span class="sxs-lookup"><span data-stu-id="46285-170">Azure Resource Manager (ARM) enables you toowork with hello resources in your application as a group, referred tooas an Azure Resource Group.</span></span> <span data-ttu-id="46285-171">Központi telepítése, frissítése, figyelheti vagy törölje az összes hello erőforrások egyetlen, koordinált műveletben az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="46285-171">You can deploy, update, monitor or delete all of hello resources for your application in a single, coordinated operation.</span></span> <span data-ttu-id="46285-172">A telepítéshez egy sablont használ, amely különböző, például tesztelési, átmeneti és üzemi környezetekben is képes működni.</span><span class="sxs-lookup"><span data-stu-id="46285-172">You use a template for deployment and that template can work for different environments such as testing, staging and production.</span></span> <span data-ttu-id="46285-173">Tisztázhatja a szervezete teljes csoport hello hello összegzett költségeinek megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="46285-173">You can clarify billing for your organization by viewing hello rolled-up costs for hello entire group.</span></span> <span data-ttu-id="46285-174">További információk: [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md) (Az Azure Resource Manager áttekintése).</span><span class="sxs-lookup"><span data-stu-id="46285-174">For more information, see [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span> 

<span data-ttu-id="46285-175">A Data Lake Analytics szolgáltatás a következő összetevők hello az alábbiakból állhat:</span><span class="sxs-lookup"><span data-stu-id="46285-175">A Data Lake Analytics service can include hello following components:</span></span>

* <span data-ttu-id="46285-176">Azure Data Lake Analytics-fiók</span><span class="sxs-lookup"><span data-stu-id="46285-176">Azure Data Lake Analytics account</span></span>
* <span data-ttu-id="46285-177">Szükséges alapértelmezett Azure Data Lake-tárfiókra</span><span class="sxs-lookup"><span data-stu-id="46285-177">Required default Azure Data Lake Storage account</span></span>
* <span data-ttu-id="46285-178">További Azure Data Lake-tárfiókokat</span><span class="sxs-lookup"><span data-stu-id="46285-178">Additional Azure Data Lake Storage accounts</span></span>
* <span data-ttu-id="46285-179">További Azure Storage-fiókok</span><span class="sxs-lookup"><span data-stu-id="46285-179">Additional Azure Storage accounts</span></span>

<span data-ttu-id="46285-180">Létrehozhat egy ARM-csoport toomake alatt mindezen összetevők azokat könnyebben toomanage.</span><span class="sxs-lookup"><span data-stu-id="46285-180">You can create all these components under one ARM group toomake them easier toomanage.</span></span>

![Azure Data Lake Analytics-fiók és tárolás](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

<span data-ttu-id="46285-182">Data Lake Analytics-fiók és a függő tárfiókok hello kell helyezni hello azonos Azure-adatközpont.</span><span class="sxs-lookup"><span data-stu-id="46285-182">A Data Lake Analytics account and hello dependent storage accounts must be placed in hello same Azure data center.</span></span>
<span data-ttu-id="46285-183">azonban hello ARM csoport elhelyezhető egy másik adatközpont.</span><span class="sxs-lookup"><span data-stu-id="46285-183">hello ARM group however can be located in a different data center.</span></span>  

## <a name="see-also"></a><span data-ttu-id="46285-184">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="46285-184">See also</span></span>
* [<span data-ttu-id="46285-185">A Microsoft Azure Data Lake Analytics áttekintése</span><span class="sxs-lookup"><span data-stu-id="46285-185">Overview of Microsoft Azure Data Lake Analytics</span></span>](data-lake-analytics-overview.md)
* [<span data-ttu-id="46285-186">A Data Lake Analytics használatának első lépései az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="46285-186">Get started with Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="46285-187">Az Azure Data Lake Analytics kezelése az Azure Portal használatával</span><span class="sxs-lookup"><span data-stu-id="46285-187">Manage Azure Data Lake Analytics using Azure Portal</span></span>](data-lake-analytics-manage-use-portal.md)
* [<span data-ttu-id="46285-188">Az Azure Data Lake Analytics-feladatok figyelése és hibaelhárítása az Azure Portallal</span><span class="sxs-lookup"><span data-stu-id="46285-188">Monitor and troubleshoot Azure Data Lake Analytics jobs using Azure Portal</span></span>](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

