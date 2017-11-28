---
title: "Adatok másolása az Azure Blob Storage |} Microsoft Docs"
description: "Ismerje meg, hogy a blob-adatok másolása az Azure Data Factory. A minta: adatok másolása az Azure Blob Storage és az Azure SQL Database."
keywords: "Blobadatok, az azure blob másolása"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: bec8160f-5e07-47e4-8ee1-ebb14cfb805d
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 2cf955b52010869a4e753c441e17bdd32fd2e63d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="copy-data-to-or-from-azure-blob-storage-using-azure-data-factory"></a><span data-ttu-id="8b86b-105">Másolja az adatokat, vagy az Azure Blob Storage Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="8b86b-105">Copy data to or from Azure Blob Storage using Azure Data Factory</span></span>
<span data-ttu-id="8b86b-106">Ez a cikk ismerteti, hogyan a másolási tevékenység során az Azure Data Factory és az Azure Blob Storage-adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="8b86b-106">This article explains how to use the Copy Activity in Azure Data Factory to copy data to and from Azure Blob Storage.</span></span> <span data-ttu-id="8b86b-107">Buildekről nyújtanak a [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amelynek során adatátvitel a másolási tevékenység az általános áttekintést.</span><span class="sxs-lookup"><span data-stu-id="8b86b-107">It builds on the [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with the copy activity.</span></span>

## <a name="overview"></a><span data-ttu-id="8b86b-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8b86b-108">Overview</span></span>
<span data-ttu-id="8b86b-109">Bármely támogatott forrás adattárolóból Azure Blob Storage vagy az Azure Blob Storage bármely támogatott fogadó adattárolóhoz adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="8b86b-109">You can copy data from any supported source data store to Azure Blob Storage or from Azure Blob Storage to any supported sink data store.</span></span> <span data-ttu-id="8b86b-110">A következő táblázat adattárolókhoz támogatott adatforrások listáját tartalmazza, vagy a másolási tevékenység által fogadók esetében.</span><span class="sxs-lookup"><span data-stu-id="8b86b-110">The following table provides a list of data stores supported as sources or sinks by the copy activity.</span></span> <span data-ttu-id="8b86b-111">Adatok áthelyezése például **a** egy SQL Server-adatbázist vagy egy Azure SQL-adatbázis **való** egy Azure blobtárolóba.</span><span class="sxs-lookup"><span data-stu-id="8b86b-111">For example, you can move data **from** a SQL Server database or an Azure SQL database **to** an Azure blob storage.</span></span> <span data-ttu-id="8b86b-112">És adatainak másolhatja **a** Azure blob storage **való** Azure SQL Data Warehouse vagy egy Azure Cosmos DB gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="8b86b-112">And, you can copy data **from** Azure blob storage **to** an Azure SQL Data Warehouse or an Azure Cosmos DB collection.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="8b86b-113">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="8b86b-113">Supported scenarios</span></span>
<span data-ttu-id="8b86b-114">Adatokat másolhat **az Azure Blob Storage** tárolja a következő adatokat:</span><span class="sxs-lookup"><span data-stu-id="8b86b-114">You can copy data **from Azure Blob Storage** to the following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="8b86b-115">Adatok másolása a következő adatokat tárolja **Azure Blob Storage**:</span><span class="sxs-lookup"><span data-stu-id="8b86b-115">You can copy data from the following data stores **to Azure Blob Storage**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]
 
> [!IMPORTANT]
> <span data-ttu-id="8b86b-116">Másolási tevékenység támogatja az adatok másolását a/az általános célú Azure Storage-fiókok és a gyakran használt adatok/ritkán Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="8b86b-116">Copy Activity supports copying data from/to both general-purpose Azure Storage accounts and Hot/Cool Blob storage.</span></span> <span data-ttu-id="8b86b-117">A tevékenység támogatja **blokk, hozzáfűzése, vagy olvasása lapblobokat**, azonban a **csak a blokkblobokat írása**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-117">The activity supports **reading from block, append, or page blobs**, but supports **writing to only block blobs**.</span></span> <span data-ttu-id="8b86b-118">Prémium szintű Storage nem támogatott, a fogadó, mert a lapblobokat ezt támogatja.</span><span class="sxs-lookup"><span data-stu-id="8b86b-118">Azure Premium Storage is not supported as a sink because it is backed by page blobs.</span></span>
> 
> <span data-ttu-id="8b86b-119">Másolási tevékenység adatot nem töröl a forrás után a rendszer sikeresen átmásolja az adatokat a célhelyre.</span><span class="sxs-lookup"><span data-stu-id="8b86b-119">Copy Activity does not delete data from the source after the data is successfully copied to the destination.</span></span> <span data-ttu-id="8b86b-120">Ha sikeres másolatot megszüntetését követően törölheti a forrásadatok van szüksége, hozzon létre egy [egyéni tevékenység](data-factory-use-custom-activities.md) törli az adatokat, és az adatcsatorna használja a tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="8b86b-120">If you need to delete source data after a successful copy, create a [custom activity](data-factory-use-custom-activities.md) to delete the data and use the activity in the pipeline.</span></span> <span data-ttu-id="8b86b-121">Egy vonatkozó példáért lásd: a [Delete blob vagy mappa mintát a Githubon](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).</span><span class="sxs-lookup"><span data-stu-id="8b86b-121">For an example, see the [Delete blob or folder sample on GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).</span></span> 

## <a name="get-started"></a><span data-ttu-id="8b86b-122">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="8b86b-122">Get started</span></span>
<span data-ttu-id="8b86b-123">A másolási tevékenység, amely helyezi át az adatokat az Azure Blob Storage vagy a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="8b86b-123">You can create a pipeline with a copy activity that moves data to/from an Azure Blob Storage by using different tools/APIs.</span></span>

<span data-ttu-id="8b86b-124">Hozzon létre egy folyamatot a legegyszerűbb módja használatára a **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-124">The easiest way to create a pipeline is to use the **Copy Wizard**.</span></span> <span data-ttu-id="8b86b-125">Ennek a cikknek a [forgatókönyv](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) adatok másolása az Azure Blob Storage-helyre máshová Azure Blob Storage folyamat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="8b86b-125">This article has a [walkthrough](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) for creating a pipeline to copy data from an Azure Blob Storage location  to another Azure Blob Storage location.</span></span> <span data-ttu-id="8b86b-126">Az adatok másolása az Azure Blob Storage Azure SQL Database adatcsatorna létrehozásával oktatóanyagok esetén lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="8b86b-126">For a tutorial on creating a pipeline to copy data from an Azure Blob Storage to Azure SQL Database, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="8b86b-127">Az alábbi eszközöket használhatja a folyamatokat létrehozni: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager sablon**, **.NET API**, és **REST API**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-127">You can also use the following tools to create a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="8b86b-128">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) hozzon létre egy folyamatot a másolási tevékenység részletes útmutatóját.</span><span class="sxs-lookup"><span data-stu-id="8b86b-128">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions to create a pipeline with a copy activity.</span></span>

<span data-ttu-id="8b86b-129">Akár az eszközök vagy API-k, hajtsa végre a következő lépésekkel hozza létre egy folyamatot, amely mozgatja az adatokat a forrás-tárolóban a fogadó tárolóban:</span><span class="sxs-lookup"><span data-stu-id="8b86b-129">Whether you use the tools or APIs, you perform the following steps to create a pipeline that moves data from a source data store to a sink data store:</span></span>

1. <span data-ttu-id="8b86b-130">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-130">Create a **data factory**.</span></span> <span data-ttu-id="8b86b-131">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="8b86b-131">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="8b86b-132">Hozzon létre **összekapcsolt szolgáltatások** bemeneti és kimeneti adatok csatolásához tárolja a a data factory.</span><span class="sxs-lookup"><span data-stu-id="8b86b-132">Create **linked services** to link input and output data stores to your data factory.</span></span> <span data-ttu-id="8b86b-133">Például ha a másolt adatok az az Azure blob storage Azure SQL-adatbázishoz, hoz létre az Azure storage-fiók és az Azure SQL adatbázis összekapcsolása a data factory két összekapcsolt szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="8b86b-133">For example, if you are copying data from an Azure blob storage to an Azure SQL database, you create two linked services to link your Azure storage account and Azure SQL database to your data factory.</span></span> <span data-ttu-id="8b86b-134">Azure Blob Storage jellemző csatolt szolgáltatás tulajdonságait, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8b86b-134">For linked service properties that are specific to Azure Blob Storage, see [linked service properties](#linked-service-properties) section.</span></span> 
2. <span data-ttu-id="8b86b-135">Hozzon létre **adatkészletek** a másolási művelet bemeneti és kimeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="8b86b-135">Create **datasets** to represent input and output data for the copy operation.</span></span> <span data-ttu-id="8b86b-136">A példában az előző lépésben említett hozzon létre egy adatkészlet adja meg a blob-tároló és a bemeneti adatokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="8b86b-136">In the example mentioned in the last step, you create a dataset to specify the blob container and folder that contains the input data.</span></span> <span data-ttu-id="8b86b-137">És hoz létre, ha meg szeretné adni az SQL-tábla az Azure SQL-adatbázis, amely tárolja az adatokat a blob-tároló átmásolja egy másik DataSet adatkészletben.</span><span class="sxs-lookup"><span data-stu-id="8b86b-137">And, you create another dataset to specify the SQL table in the Azure SQL database that holds the data copied from the blob storage.</span></span> <span data-ttu-id="8b86b-138">Adott Azure Blob-tároló adatkészlet tulajdonságai, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8b86b-138">For dataset properties that are specific to Azure Blob Storage, see [dataset properties](#dataset-properties) section.</span></span>
3. <span data-ttu-id="8b86b-139">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="8b86b-139">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="8b86b-140">A korábban említett példában BlobSource forrás-és SqlSink akár használhatja a fogadó a másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="8b86b-140">In the example mentioned earlier, you use BlobSource as a source and SqlSink as a sink for the copy activity.</span></span> <span data-ttu-id="8b86b-141">Ehhez hasonlóan az Azure SQL Database az Azure Blob Storage másolása, használható SqlSource és BlobSink a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8b86b-141">Similarly, if you are copying from Azure SQL Database to Azure Blob Storage, you use SqlSource and BlobSink in the copy activity.</span></span> <span data-ttu-id="8b86b-142">Tekintse meg a másolási tevékenység tulajdonságok adott Azure Blob Storage [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8b86b-142">For copy activity properties that are specific to Azure Blob Storage, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="8b86b-143">További részletek a tárolóban használatáról a forrás vagy a fogadó a hivatkozásra a adattároló az előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="8b86b-143">For details on how to use a data store as a source or a sink, click the link in the previous section for your data store.</span></span>  

<span data-ttu-id="8b86b-144">A varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és a feldolgozási sor) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="8b86b-144">When you use the wizard, JSON definitions for these Data Factory entities (linked services, datasets, and the pipeline) are automatically created for you.</span></span> <span data-ttu-id="8b86b-145">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások a JSON formátum használatával.</span><span class="sxs-lookup"><span data-stu-id="8b86b-145">When you use tools/APIs (except .NET API), you define these Data Factory entities by using the JSON format.</span></span>  <span data-ttu-id="8b86b-146">A mintában használt adatok másolása az Azure Blob Storage az adat-előállító entitások JSON-definíciók, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-blob-storage  ) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="8b86b-146">For samples with JSON definitions for Data Factory entities that are used to copy data to/from an Azure Blob Storage, see [JSON examples](#json-examples-for-copying-data-to-and-from-blob-storage  ) section of this article.</span></span>

<span data-ttu-id="8b86b-147">A következő szakaszok részletesen bemutatják, amely segítségével határozza meg a Data Factory entitások adott Azure Blob Storage JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="8b86b-147">The following sections provide details about JSON properties that are used to define Data Factory entities specific to Azure Blob Storage.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="8b86b-148">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="8b86b-148">Linked service properties</span></span>
<span data-ttu-id="8b86b-149">Az összekapcsolt szolgáltatások használatával egy Azure Storage összekapcsolása egy Azure data factory két típusa van.</span><span class="sxs-lookup"><span data-stu-id="8b86b-149">There are two types of linked services you can use to link an Azure Storage to an Azure data factory.</span></span> <span data-ttu-id="8b86b-150">Ezek: **AzureStorage** társított szolgáltatás és **AzureStorageSas** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8b86b-150">They are: **AzureStorage** linked service and **AzureStorageSas** linked service.</span></span> <span data-ttu-id="8b86b-151">Az Azure tárolás társított szolgáltatása az adat-előállítóban globális hozzáférést biztosít az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="8b86b-151">The Azure Storage linked service provides the data factory with global access to the Azure Storage.</span></span> <span data-ttu-id="8b86b-152">Mivel az Azure Storage SAS (közös hozzáférésű Jogosultságkód) kapcsolódó szolgáltatás korlátozott/időhöz kötött hozzáféréssel a data factory biztosítja az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="8b86b-152">Whereas, The Azure Storage SAS (Shared Access Signature) linked service provides the data factory with restricted/time-bound access to the Azure Storage.</span></span> <span data-ttu-id="8b86b-153">Nincsenek más különbségek a következő két összekapcsolt szolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="8b86b-153">There are no other differences between these two linked services.</span></span> <span data-ttu-id="8b86b-154">Válassza ki az igényeinek megfelelő társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8b86b-154">Choose the linked service that suits your needs.</span></span> <span data-ttu-id="8b86b-155">A következő szakaszokban további részleteket a következő két összekapcsolt szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="8b86b-155">The following sections provide more details on these two linked services.</span></span>

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a><span data-ttu-id="8b86b-156">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="8b86b-156">Dataset properties</span></span>
<span data-ttu-id="8b86b-157">Adja meg a bemeneti vagy kimeneti adatok az Azure Blob-tároló adatkészlet, állítsa be a type tulajdonságot az adathalmaz: **AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-157">To specify a dataset to represent input or output data in an Azure Blob Storage, you set the type property of the dataset to: **AzureBlob**.</span></span> <span data-ttu-id="8b86b-158">Állítsa be a **linkedServiceName** tulajdonság nevére, az Azure Storage vagy az Azure Storage SAS DataSet társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8b86b-158">Set the **linkedServiceName** property of the dataset to the name of the Azure Storage or Azure Storage SAS linked service.</span></span>  <span data-ttu-id="8b86b-159">A DataSet tulajdonságait adja meg a **blob tároló** és a **mappa** a blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="8b86b-159">The type properties of the dataset specify the **blob container** and the **folder** in the blob storage.</span></span>

<span data-ttu-id="8b86b-160">JSON-szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: a [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="8b86b-160">For a full list of JSON sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="8b86b-161">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="8b86b-161">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="8b86b-162">Adat-előállítót a következő alapú CLS-kompatibilis .NET típusú értékek támogatja a például az Azure blob séma olvasható az adatforrásokhoz tartozó "structure" írja be az adatokat biztosító: Int16, Int32, Int64, egyetlen, Double, Decimal, Byte [], Bool, String, Guid, Datetime, Datetimeoffset, Timespan.</span><span class="sxs-lookup"><span data-stu-id="8b86b-162">Data factory supports the following CLS-compliant .NET based type values for providing type information in “structure” for schema-on-read data sources like Azure blob: Int16, Int32, Int64, Single, Double, Decimal, Byte[], Bool, String, Guid, Datetime, Datetimeoffset, Timespan.</span></span> <span data-ttu-id="8b86b-163">Adat-előállító automatikusan típuskonverziók hajt végre, amikor adatokat a forrás-tárolóban a fogadó tárolóban.</span><span class="sxs-lookup"><span data-stu-id="8b86b-163">Data Factory automatically performs type conversions when moving data from a source data store to a sink data store.</span></span>

<span data-ttu-id="8b86b-164">A **typeProperties** szakasz eltérő adatkészlet egyes típusai és információkat nyújt a hely formátumra stb, az adatok az adattárban.</span><span class="sxs-lookup"><span data-stu-id="8b86b-164">The **typeProperties** section is different for each type of dataset and provides information about the location, format etc., of the data in the data store.</span></span> <span data-ttu-id="8b86b-165">A typeProperties szakasz típusú adatkészlet **AzureBlob** adatkészlet tulajdonságai a következők:</span><span class="sxs-lookup"><span data-stu-id="8b86b-165">The typeProperties section for dataset of type **AzureBlob** dataset has the following properties:</span></span>

| <span data-ttu-id="8b86b-166">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8b86b-166">Property</span></span> | <span data-ttu-id="8b86b-167">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b86b-167">Description</span></span> | <span data-ttu-id="8b86b-168">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b86b-168">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8b86b-169">folderPath</span><span class="sxs-lookup"><span data-stu-id="8b86b-169">folderPath</span></span> |<span data-ttu-id="8b86b-170">A tároló és a blob-tároló mappa elérési útja.</span><span class="sxs-lookup"><span data-stu-id="8b86b-170">Path to the container and folder in the blob storage.</span></span> <span data-ttu-id="8b86b-171">Példa: myblobcontainer\myblobfolder\\</span><span class="sxs-lookup"><span data-stu-id="8b86b-171">Example: myblobcontainer\myblobfolder\\</span></span> |<span data-ttu-id="8b86b-172">Igen</span><span class="sxs-lookup"><span data-stu-id="8b86b-172">Yes</span></span> |
| <span data-ttu-id="8b86b-173">fileName</span><span class="sxs-lookup"><span data-stu-id="8b86b-173">fileName</span></span> |<span data-ttu-id="8b86b-174">A blob neve.</span><span class="sxs-lookup"><span data-stu-id="8b86b-174">Name of the blob.</span></span> <span data-ttu-id="8b86b-175">Fájlnév nem, kötelező, és a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="8b86b-175">fileName is optional and case-sensitive.</span></span><br/><br/><span data-ttu-id="8b86b-176">Ha megad egy fájlnevet, a tevékenység (például a Másolás) működik a megadott Blob.</span><span class="sxs-lookup"><span data-stu-id="8b86b-176">If you specify a filename, the activity (including Copy) works on the specific Blob.</span></span><br/><br/><span data-ttu-id="8b86b-177">Ha nincs megadva fájlnév, másolása összes BLOB bemeneti adatkészlet folderPath tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8b86b-177">When fileName is not specified, Copy includes all Blobs in the folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="8b86b-178">Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet és **preserveHierarchy** nincs megadva a tevékenység fogadó, a létrehozott fájl nevét a következő lenne ebben a formátumban: adatok.<Guid>. txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="8b86b-178">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, the name of the generated file would be in the following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="8b86b-179">Nem</span><span class="sxs-lookup"><span data-stu-id="8b86b-179">No</span></span> |
| <span data-ttu-id="8b86b-180">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="8b86b-180">partitionedBy</span></span> |<span data-ttu-id="8b86b-181">partitionedBy egy nem kötelező tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8b86b-181">partitionedBy is an optional property.</span></span> <span data-ttu-id="8b86b-182">Használhatja a dinamikus folderPath és fájlnevét idő adatsorozat adatok megadása.</span><span class="sxs-lookup"><span data-stu-id="8b86b-182">You can use it to specify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="8b86b-183">Például folderPath is paraméteres adatok óránkénti.</span><span class="sxs-lookup"><span data-stu-id="8b86b-183">For example, folderPath can be parameterized for every hour of data.</span></span> <span data-ttu-id="8b86b-184">Tekintse meg a [partitionedBy tulajdonság szakaszában](#using-partitionedBy-property) részletek és a példákat.</span><span class="sxs-lookup"><span data-stu-id="8b86b-184">See the [Using partitionedBy property section](#using-partitionedBy-property) for details and examples.</span></span> |<span data-ttu-id="8b86b-185">Nem</span><span class="sxs-lookup"><span data-stu-id="8b86b-185">No</span></span> |
| <span data-ttu-id="8b86b-186">Formátumban</span><span class="sxs-lookup"><span data-stu-id="8b86b-186">format</span></span> | <span data-ttu-id="8b86b-187">A következő formátumban típusok támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-187">The following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="8b86b-188">Állítsa be a **típus** tulajdonság a formátuma a következő értékek egyikét.</span><span class="sxs-lookup"><span data-stu-id="8b86b-188">Set the **type** property under format to one of these values.</span></span> <span data-ttu-id="8b86b-189">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="8b86b-189">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="8b86b-190">Ha azt szeretné, hogy **másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a Formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="8b86b-190">If you want to **copy files as-is** between file-based stores (binary copy), skip the format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="8b86b-191">Nem</span><span class="sxs-lookup"><span data-stu-id="8b86b-191">No</span></span> |
| <span data-ttu-id="8b86b-192">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="8b86b-192">compression</span></span> | <span data-ttu-id="8b86b-193">Adja meg a típus és az adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="8b86b-193">Specify the type and level of compression for the data.</span></span> <span data-ttu-id="8b86b-194">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-194">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="8b86b-195">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-195">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="8b86b-196">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="8b86b-196">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="8b86b-197">Nem</span><span class="sxs-lookup"><span data-stu-id="8b86b-197">No</span></span> |

### <a name="using-partitionedby-property"></a><span data-ttu-id="8b86b-198">PartitionedBy tulajdonság használatával</span><span class="sxs-lookup"><span data-stu-id="8b86b-198">Using partitionedBy property</span></span>
<span data-ttu-id="8b86b-199">Az előző szakaszban említett, megadhat egy dinamikus folderPath és a fájlnév idő adatsorozat adatokhoz a **partitionedBy** tulajdonság, [adat-előállító funkciók és a rendszer változók](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="8b86b-199">As mentioned in the previous section, you can specify a dynamic folderPath and filename for time series data with the **partitionedBy** property, [Data Factory functions, and the system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="8b86b-200">Idő adatsorozat adatkészleteket, az ütemezés és a szeletek további információkért lásd: [létrehozása adatkészletek](data-factory-create-datasets.md) és [ütemezés & végrehajtási](data-factory-scheduling-and-execution.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="8b86b-200">For more information on time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md) and [Scheduling & Execution](data-factory-scheduling-and-execution.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="8b86b-201">1. példa</span><span class="sxs-lookup"><span data-stu-id="8b86b-201">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="8b86b-202">Ebben a példában {szelet} adat-előállító rendszer változó SliceStart (YYYYMMDDHH) formátumban megadott érték helyére.</span><span class="sxs-lookup"><span data-stu-id="8b86b-202">In this example, {Slice} is replaced with the value of Data Factory system variable SliceStart in the format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="8b86b-203">A szelet kezdete a SliceStart hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="8b86b-203">The SliceStart refers to start time of the slice.</span></span> <span data-ttu-id="8b86b-204">A folderPath nem azonos az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="8b86b-204">The folderPath is different for each slice.</span></span> <span data-ttu-id="8b86b-205">Például: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104</span><span class="sxs-lookup"><span data-stu-id="8b86b-205">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104</span></span>

#### <a name="sample-2"></a><span data-ttu-id="8b86b-206">2. példa</span><span class="sxs-lookup"><span data-stu-id="8b86b-206">Sample 2</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```

<span data-ttu-id="8b86b-207">Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a külön változókat, amelyek folderPath és a fájlnév tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8b86b-207">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="8b86b-208">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="8b86b-208">Copy activity properties</span></span>
<span data-ttu-id="8b86b-209">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: a [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="8b86b-209">For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="8b86b-210">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti adatkészletek és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="8b86b-210">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="8b86b-211">Mivel a tulajdonságok érhetők el a **typeProperties** szakasz a tevékenység tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="8b86b-211">Whereas, properties available in the **typeProperties** section of the activity vary with each activity type.</span></span> <span data-ttu-id="8b86b-212">A másolási tevékenység során két érték források és mosdók típusától függően.</span><span class="sxs-lookup"><span data-stu-id="8b86b-212">For Copy activity, they vary depending on the types of sources and sinks.</span></span> <span data-ttu-id="8b86b-213">Ha egy Azure Blob Storage-ból adatokat helyez át, a másolási tevékenység beállítása a forrástípus **BlobSource**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-213">If you are moving data from an Azure Blob Storage, you set the source type in the copy activity to **BlobSource**.</span></span> <span data-ttu-id="8b86b-214">Hasonlóképpen, ha adatokat az Azure Blob Storage, beállítása a fogadó típusa a másolási tevékenység **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-214">Similarly, if you are moving data to an Azure Blob Storage, you set the sink type in the copy activity to **BlobSink**.</span></span> <span data-ttu-id="8b86b-215">Ez a témakör BlobSource és BlobSink által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="8b86b-215">This section provides a list of properties supported by BlobSource and BlobSink.</span></span>

<span data-ttu-id="8b86b-216">**BlobSource** támogatja a következő tulajdonságok a **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="8b86b-216">**BlobSource** supports the following properties in the **typeProperties** section:</span></span>

| <span data-ttu-id="8b86b-217">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8b86b-217">Property</span></span> | <span data-ttu-id="8b86b-218">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b86b-218">Description</span></span> | <span data-ttu-id="8b86b-219">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="8b86b-219">Allowed values</span></span> | <span data-ttu-id="8b86b-220">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b86b-220">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8b86b-221">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="8b86b-221">recursive</span></span> |<span data-ttu-id="8b86b-222">Azt jelzi, hogy az adatok olvasható rekurzív módon az almappák vagy csak a megadott mappát.</span><span class="sxs-lookup"><span data-stu-id="8b86b-222">Indicates whether the data is read recursively from the sub folders or only from the specified folder.</span></span> |<span data-ttu-id="8b86b-223">TRUE hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="8b86b-223">True (default value), False</span></span> |<span data-ttu-id="8b86b-224">Nem</span><span class="sxs-lookup"><span data-stu-id="8b86b-224">No</span></span> |

<span data-ttu-id="8b86b-225">**BlobSink** támogatja a következő tulajdonságok **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="8b86b-225">**BlobSink** supports the following properties **typeProperties** section:</span></span>

| <span data-ttu-id="8b86b-226">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8b86b-226">Property</span></span> | <span data-ttu-id="8b86b-227">Leírás</span><span class="sxs-lookup"><span data-stu-id="8b86b-227">Description</span></span> | <span data-ttu-id="8b86b-228">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="8b86b-228">Allowed values</span></span> | <span data-ttu-id="8b86b-229">Szükséges</span><span class="sxs-lookup"><span data-stu-id="8b86b-229">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="8b86b-230">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="8b86b-230">copyBehavior</span></span> |<span data-ttu-id="8b86b-231">Másolás viselkedését határozza meg, ha az adatforrás BlobSource vagy a fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="8b86b-231">Defines the copy behavior when the source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="8b86b-232"><b>PreserveHierarchy</b>: őrzi meg a fájl hierarchia a célmappában.</span><span class="sxs-lookup"><span data-stu-id="8b86b-232"><b>PreserveHierarchy</b>: preserves the file hierarchy in the target folder.</span></span> <span data-ttu-id="8b86b-233">A következő forrásfájl forrásmappához relatív elérési a relatív elérési út a cél-fájlját és a célmappa megegyezik.</span><span class="sxs-lookup"><span data-stu-id="8b86b-233">The relative path of source file to source folder is identical to the relative path of target file to target folder.</span></span><br/><br/><span data-ttu-id="8b86b-234"><b>FlattenHierarchy</b>: a forrásmappából a fájlok a célmappában első szintjét is.</span><span class="sxs-lookup"><span data-stu-id="8b86b-234"><b>FlattenHierarchy</b>: all files from the source folder are in the first level of target folder.</span></span> <span data-ttu-id="8b86b-235">A fájlok céljaként automatikusan létrehozott nevet adni.</span><span class="sxs-lookup"><span data-stu-id="8b86b-235">The target files have auto generated name.</span></span> <br/><br/><span data-ttu-id="8b86b-236"><b>Mergefiles típusú</b>: egy fájl összes fájlt a forrásmappából egyesíti.</span><span class="sxs-lookup"><span data-stu-id="8b86b-236"><b>MergeFiles</b>: merges all files from the source folder to one file.</span></span> <span data-ttu-id="8b86b-237">Ha a fájl/Blob neve meg van adva, az egyesített neve legyen a megadott név; Ellenkező esetben lenne automatikusan létrehozott fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="8b86b-237">If the File/Blob Name is specified, the merged file name would be the specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="8b86b-238">Nem</span><span class="sxs-lookup"><span data-stu-id="8b86b-238">No</span></span> |

<span data-ttu-id="8b86b-239">**BlobSource** is támogatja a visszamenőleges kompatibilitás érdekében a két tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="8b86b-239">**BlobSource** also supports these two properties for backward compatibility.</span></span>

* <span data-ttu-id="8b86b-240">**treatEmptyAsNull**: Megadja, hogy null vagy üres karakterlánc null értékként kezelje-e.</span><span class="sxs-lookup"><span data-stu-id="8b86b-240">**treatEmptyAsNull**: Specifies whether to treat null or empty string as null value.</span></span>
* <span data-ttu-id="8b86b-241">**skipHeaderLineCount** -határozza meg, hogy hány sort kell figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="8b86b-241">**skipHeaderLineCount** - Specifies how many lines need be skipped.</span></span> <span data-ttu-id="8b86b-242">Azt is alkalmazható csak amikor bemeneti adatkészlet által használt szöveges.</span><span class="sxs-lookup"><span data-stu-id="8b86b-242">It is applicable only when input dataset is using TextFormat.</span></span>

<span data-ttu-id="8b86b-243">Hasonlóképpen **BlobSink** támogatja a következő tulajdonságot a visszamenőleges kompatibilitás érdekében.</span><span class="sxs-lookup"><span data-stu-id="8b86b-243">Similarly, **BlobSink** supports the following property for backward compatibility.</span></span>

* <span data-ttu-id="8b86b-244">**blobWriterAddHeader**: Megadja, hogy az oszlopdefiníciók fejlécet hozzá egy kimeneti adatkészlet írása közben.</span><span class="sxs-lookup"><span data-stu-id="8b86b-244">**blobWriterAddHeader**: Specifies whether to add a header of column definitions while writing to an output dataset.</span></span>

<span data-ttu-id="8b86b-245">Adatkészletek mostantól támogatja a következő tulajdonságok funkcióit megvalósító: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-245">Datasets now support the following properties that implement the same functionality: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.</span></span>

<span data-ttu-id="8b86b-246">A következő táblázat az új adatkészlet tulajdonságai helyett a blob-forrás/fogadó tulajdonságok használatával nyújt útmutatást.</span><span class="sxs-lookup"><span data-stu-id="8b86b-246">The following table provides guidance on using the new dataset properties in place of these blob source/sink properties.</span></span>

| <span data-ttu-id="8b86b-247">Másolja az Activity tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8b86b-247">Copy Activity property</span></span> | <span data-ttu-id="8b86b-248">A DataSet tulajdonság</span><span class="sxs-lookup"><span data-stu-id="8b86b-248">Dataset property</span></span> |
|:--- |:--- |
| <span data-ttu-id="8b86b-249">a BlobSource skipHeaderLineCount</span><span class="sxs-lookup"><span data-stu-id="8b86b-249">skipHeaderLineCount on BlobSource</span></span> |<span data-ttu-id="8b86b-250">skipLineCount és firstRowAsHeader.</span><span class="sxs-lookup"><span data-stu-id="8b86b-250">skipLineCount and firstRowAsHeader.</span></span> <span data-ttu-id="8b86b-251">Sorok először kimarad, és az első sor fejléc, olvassa el.</span><span class="sxs-lookup"><span data-stu-id="8b86b-251">Lines are skipped first and then the first row is read as a header.</span></span> |
| <span data-ttu-id="8b86b-252">a BlobSource treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="8b86b-252">treatEmptyAsNull on BlobSource</span></span> |<span data-ttu-id="8b86b-253">a bemeneti adatkészlet treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="8b86b-253">treatEmptyAsNull on input dataset</span></span> |
| <span data-ttu-id="8b86b-254">a BlobSink blobWriterAddHeader</span><span class="sxs-lookup"><span data-stu-id="8b86b-254">blobWriterAddHeader on BlobSink</span></span> |<span data-ttu-id="8b86b-255">a kimeneti adatkészlet firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="8b86b-255">firstRowAsHeader on output dataset</span></span> |

<span data-ttu-id="8b86b-256">Lásd: [megadása szöveges](data-factory-supported-file-and-compression-formats.md#text-format) szakasz ezeket a tulajdonságokat részletes tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="8b86b-256">See [Specifying TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) section for detailed information on these properties.</span></span>    

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="8b86b-257">rekurzív és copyBehavior példák</span><span class="sxs-lookup"><span data-stu-id="8b86b-257">recursive and copyBehavior examples</span></span>
<span data-ttu-id="8b86b-258">Ez a szakasz ismerteti az eredményül kapott viselkedéstől rekurzív és copyBehavior kombinációk a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="8b86b-258">This section describes the resulting behavior of the Copy operation for different combinations of recursive and copyBehavior values.</span></span>

| <span data-ttu-id="8b86b-259">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="8b86b-259">recursive</span></span> | <span data-ttu-id="8b86b-260">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="8b86b-260">copyBehavior</span></span> | <span data-ttu-id="8b86b-261">Viselkedésről</span><span class="sxs-lookup"><span data-stu-id="8b86b-261">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="8b86b-262">Igaz</span><span class="sxs-lookup"><span data-stu-id="8b86b-262">true</span></span> |<span data-ttu-id="8b86b-263">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="8b86b-263">preserveHierarchy</span></span> |<span data-ttu-id="8b86b-264">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="8b86b-264">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="8b86b-265">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-265">Folder1</span></span><br/><span data-ttu-id="8b86b-266">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-266">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="8b86b-267">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="8b86b-267">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="8b86b-268">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="8b86b-268">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="8b86b-269">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="8b86b-269">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="8b86b-270">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="8b86b-270">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="8b86b-271">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="8b86b-271">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="8b86b-272">a célmappában mappa1 forrásaként azonos struktúrájú jön létre.</span><span class="sxs-lookup"><span data-stu-id="8b86b-272">the target folder Folder1 is created with the same structure as the source</span></span><br/><br/><span data-ttu-id="8b86b-273">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-273">Folder1</span></span><br/><span data-ttu-id="8b86b-274">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-274">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="8b86b-275">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="8b86b-275">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="8b86b-276">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="8b86b-276">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="8b86b-277">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="8b86b-277">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="8b86b-278">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="8b86b-278">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="8b86b-279">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span><span class="sxs-lookup"><span data-stu-id="8b86b-279">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span></span> |
| <span data-ttu-id="8b86b-280">Igaz</span><span class="sxs-lookup"><span data-stu-id="8b86b-280">true</span></span> |<span data-ttu-id="8b86b-281">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="8b86b-281">flattenHierarchy</span></span> |<span data-ttu-id="8b86b-282">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="8b86b-282">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="8b86b-283">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-283">Folder1</span></span><br/><span data-ttu-id="8b86b-284">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-284">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="8b86b-285">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="8b86b-285">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="8b86b-286">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="8b86b-286">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="8b86b-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="8b86b-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="8b86b-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="8b86b-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="8b86b-289">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="8b86b-289">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="8b86b-290">a cél az alábbi szerkezettel mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="8b86b-290">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="8b86b-291">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-291">Folder1</span></span><br/><span data-ttu-id="8b86b-292">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-292">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="8b86b-293">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="8b86b-293">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="8b86b-294">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet fájl3</span><span class="sxs-lookup"><span data-stu-id="8b86b-294">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="8b86b-295">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File4</span><span class="sxs-lookup"><span data-stu-id="8b86b-295">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="8b86b-296">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File5</span><span class="sxs-lookup"><span data-stu-id="8b86b-296">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="8b86b-297">Igaz</span><span class="sxs-lookup"><span data-stu-id="8b86b-297">true</span></span> |<span data-ttu-id="8b86b-298">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="8b86b-298">mergeFiles</span></span> |<span data-ttu-id="8b86b-299">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="8b86b-299">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="8b86b-300">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-300">Folder1</span></span><br/><span data-ttu-id="8b86b-301">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="8b86b-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="8b86b-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="8b86b-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="8b86b-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="8b86b-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="8b86b-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="8b86b-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="8b86b-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="8b86b-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="8b86b-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="8b86b-307">a cél az alábbi szerkezettel mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="8b86b-307">the target Folder1 is created with the following structure:</span></span> <br/><br/><span data-ttu-id="8b86b-308">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-308">Folder1</span></span><br/><span data-ttu-id="8b86b-309">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + fájl3 + File4 + 5 fájl tartalmát egy fájl automatikusan létrehozott fájlnévvel egyesülnek</span><span class="sxs-lookup"><span data-stu-id="8b86b-309">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with auto-generated file name</span></span> |
| <span data-ttu-id="8b86b-310">hamis</span><span class="sxs-lookup"><span data-stu-id="8b86b-310">false</span></span> |<span data-ttu-id="8b86b-311">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="8b86b-311">preserveHierarchy</span></span> |<span data-ttu-id="8b86b-312">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="8b86b-312">For a source folder Folder1 with the following structure:</span></span> <br/><br/><span data-ttu-id="8b86b-313">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-313">Folder1</span></span><br/><span data-ttu-id="8b86b-314">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-314">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="8b86b-315">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="8b86b-315">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="8b86b-316">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="8b86b-316">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="8b86b-317">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="8b86b-317">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="8b86b-318">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="8b86b-318">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="8b86b-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="8b86b-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="8b86b-320">a célmappa mappa1 jön létre a következő struktúra</span><span class="sxs-lookup"><span data-stu-id="8b86b-320">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="8b86b-321">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-321">Folder1</span></span><br/><span data-ttu-id="8b86b-322">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-322">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="8b86b-323">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="8b86b-323">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><br/><span data-ttu-id="8b86b-324">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="8b86b-324">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="8b86b-325">hamis</span><span class="sxs-lookup"><span data-stu-id="8b86b-325">false</span></span> |<span data-ttu-id="8b86b-326">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="8b86b-326">flattenHierarchy</span></span> |<span data-ttu-id="8b86b-327">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="8b86b-327">For a source folder Folder1 with the following structure:</span></span><br/><br/><span data-ttu-id="8b86b-328">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-328">Folder1</span></span><br/><span data-ttu-id="8b86b-329">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="8b86b-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="8b86b-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="8b86b-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="8b86b-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="8b86b-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="8b86b-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="8b86b-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="8b86b-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="8b86b-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="8b86b-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="8b86b-335">a célmappa mappa1 jön létre a következő struktúra</span><span class="sxs-lookup"><span data-stu-id="8b86b-335">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="8b86b-336">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-336">Folder1</span></span><br/><span data-ttu-id="8b86b-337">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-337">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="8b86b-338">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="8b86b-338">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><br/><span data-ttu-id="8b86b-339">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="8b86b-339">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="8b86b-340">hamis</span><span class="sxs-lookup"><span data-stu-id="8b86b-340">false</span></span> |<span data-ttu-id="8b86b-341">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="8b86b-341">mergeFiles</span></span> |<span data-ttu-id="8b86b-342">A forrásmappa mappa1, az alábbi szerkezettel:</span><span class="sxs-lookup"><span data-stu-id="8b86b-342">For a source folder Folder1 with the following structure:</span></span><br/><br/><span data-ttu-id="8b86b-343">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-343">Folder1</span></span><br/><span data-ttu-id="8b86b-344">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-344">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="8b86b-345">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="8b86b-345">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="8b86b-346">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="8b86b-346">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="8b86b-347">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="8b86b-347">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="8b86b-348">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="8b86b-348">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="8b86b-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="8b86b-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="8b86b-350">a célmappa mappa1 jön létre a következő struktúra</span><span class="sxs-lookup"><span data-stu-id="8b86b-350">the target folder Folder1 is created with the following structure</span></span><br/><br/><span data-ttu-id="8b86b-351">Mappa1</span><span class="sxs-lookup"><span data-stu-id="8b86b-351">Folder1</span></span><br/><span data-ttu-id="8b86b-352">&nbsp;&nbsp;&nbsp;&nbsp;Egy fájl automatikusan létrehozott fájlnévvel egyesített file1 + File2 tartalma.</span><span class="sxs-lookup"><span data-stu-id="8b86b-352">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with auto-generated file name.</span></span> <span data-ttu-id="8b86b-353">automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="8b86b-353">auto-generated name for File1</span></span><br/><br/><span data-ttu-id="8b86b-354">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="8b86b-354">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |

## <a name="walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage"></a><span data-ttu-id="8b86b-355">Forgatókönyv: Másolása varázsló használata a Blob Storage az adatok másolása</span><span class="sxs-lookup"><span data-stu-id="8b86b-355">Walkthrough: Use Copy Wizard to copy data to/from Blob Storage</span></span>
<span data-ttu-id="8b86b-356">Vizsgáljuk meg gyorsan az adatok az Azure blob storage és a másolása.</span><span class="sxs-lookup"><span data-stu-id="8b86b-356">Let's look at how to quickly copy data to/from an Azure blob storage.</span></span> <span data-ttu-id="8b86b-357">Ebben a bemutatóban forrás- és célkiszolgálón adatokat tárolja, típus: Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="8b86b-357">In this walkthrough, both source and destination data stores of type: Azure Blob Storage.</span></span> <span data-ttu-id="8b86b-358">A kimenetátirányítási mechanizmusával Ez a forgatókönyv adatokat egy mappából ugyanabban a blob-tárolóban egy másik mappába másolja.</span><span class="sxs-lookup"><span data-stu-id="8b86b-358">The pipeline in this walkthrough copies data from a folder to another folder in the same blob container.</span></span> <span data-ttu-id="8b86b-359">Ez a forgatókönyv nem szándékosan egyszerű mutatjuk be, beállítások vagy tulajdonságok forrás vagy a fogadó Blob Storage használata esetén.</span><span class="sxs-lookup"><span data-stu-id="8b86b-359">This walkthrough is intentionally simple to show you settings or properties when using Blob Storage as a source or sink.</span></span> 

### <a name="prerequisites"></a><span data-ttu-id="8b86b-360">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8b86b-360">Prerequisites</span></span>
1. <span data-ttu-id="8b86b-361">Hozzon létre egy általános célú **Azure Storage-fiók** Ha Ön nem rendelkezik ilyennel.</span><span class="sxs-lookup"><span data-stu-id="8b86b-361">Create a general-purpose **Azure Storage Account** if you don't have one already.</span></span> <span data-ttu-id="8b86b-362">A blob storage használata egyaránt **forrás** és **cél** adattároló ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="8b86b-362">You use the blob storage as both **source** and **destination** data store in this walkthrough.</span></span> <span data-ttu-id="8b86b-363">Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) cikk lépéseit követve létrehozhat egyet.</span><span class="sxs-lookup"><span data-stu-id="8b86b-363">if you don't have an Azure storage account, see the [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps to create one.</span></span>
2. <span data-ttu-id="8b86b-364">Hozzon létre egy blob-tároló nevű **adfblobconnector** tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="8b86b-364">Create a blob container named **adfblobconnector** in the storage account.</span></span> 
4. <span data-ttu-id="8b86b-365">Hozza létre a **bemeneti** a a **adfblobconnector** tároló.</span><span class="sxs-lookup"><span data-stu-id="8b86b-365">Create a folder named **input** in the **adfblobconnector** container.</span></span>
5. <span data-ttu-id="8b86b-366">Hozzon létre egy fájlt **emp.txt** az a következő tartalmat, és töltse fel, hogy a **bemeneti** mappa eszközökkel, mint [Azure Tártallózó](https://azurestorageexplorer.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="8b86b-366">Create a file named **emp.txt** with the following content and upload it to the **input** folder by using tools such as [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/)</span></span>
    ```json
    John, Doe
    Jane, Doe
    ```
### <a name="create-the-data-factory"></a><span data-ttu-id="8b86b-367">A data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="8b86b-367">Create the data factory</span></span>
1. <span data-ttu-id="8b86b-368">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8b86b-368">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="8b86b-369">Kattintson **+ új** kattintson a bal felső sarkának **Eszközintelligencia + analitika**, és kattintson a **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-369">Click **+ NEW** from the top-left corner, click **Intelligence + analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="8b86b-370">A **New data factory** (Új data factory) panelen:</span><span class="sxs-lookup"><span data-stu-id="8b86b-370">In the **New data factory** blade:</span></span>   
    1. <span data-ttu-id="8b86b-371">Adja meg **ADFBlobConnectorDF** a a **neve**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-371">Enter **ADFBlobConnectorDF** for the **name**.</span></span> <span data-ttu-id="8b86b-372">Az Azure data factory nevének globálisan egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="8b86b-372">The name of the Azure data factory must be globally unique.</span></span> <span data-ttu-id="8b86b-373">Ha a hibaüzenetet kapja: `*Data factory name “ADFBlobConnectorDF” is not available`, változtassa meg az adat-előállítóban (például yournameADFBlobConnectorDF) nevét, és próbálja meg újra létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8b86b-373">If you receive the error: `*Data factory name “ADFBlobConnectorDF” is not available`, change the name of the data factory (for example, yournameADFBlobConnectorDF) and try creating again.</span></span> <span data-ttu-id="8b86b-374">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="8b86b-374">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
    2. <span data-ttu-id="8b86b-375">Jelölje ki az Azure-**előfizetést**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-375">Select your Azure **subscription**.</span></span>
    3. <span data-ttu-id="8b86b-376">Az erőforráscsoport, válassza ki a **használata meglévő** jelöljön ki egy meglévő erőforráscsoportot (vagy) az **hozzon létre új** egy erőforráscsoport nevének megadása.</span><span class="sxs-lookup"><span data-stu-id="8b86b-376">For Resource Group, select **Use existing** to select an existing resource group (or) select **Create new** to enter a name for a resource group.</span></span>
    4. <span data-ttu-id="8b86b-377">Válassza ki a Data Factory **helyét**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-377">Select a **location** for the data factory.</span></span>
    5. <span data-ttu-id="8b86b-378">A panel alján jelölje be a **Pin to dashboard** (Rögzítés az irányítópulton) jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="8b86b-378">Select **Pin to dashboard** check box at the bottom of the blade.</span></span>
    6. <span data-ttu-id="8b86b-379">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8b86b-379">Click **Create**.</span></span>
3. <span data-ttu-id="8b86b-380">A létrehozásának befejezése után megjelenik a **adat-előállító** panelen a következő ábrán látható módon: ![Data factory kezdőlap](./media/data-factory-azure-blob-connector/data-factory-home-page.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-380">After the creation is complete, you see the **Data Factory** blade as shown in the following image: ![Data factory home page](./media/data-factory-azure-blob-connector/data-factory-home-page.png)</span></span>

### <a name="copy-wizard"></a><span data-ttu-id="8b86b-381">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="8b86b-381">Copy Wizard</span></span>
1. <span data-ttu-id="8b86b-382">A Data Factory kezdőlapján kattintson a **[előzetes verzió] adatok másolása** elindíthatja a csempe **másolása varázsló** egy külön lapján.</span><span class="sxs-lookup"><span data-stu-id="8b86b-382">On the Data Factory home page, click the **Copy data [PREVIEW]** tile to launch **Copy Data Wizard** in a separate tab.</span></span>    
    
    > [!NOTE]
    >    <span data-ttu-id="8b86b-383">Ha azt látja, hogy a webböngésző akadt-e a "Engedélyező...", tiltsa le vagy törölje a jelet **külső cookie-k blokkolását, és a helyadatok** beállítása (vagy) engedélyezve legyen, és hozzon létre egy kivételt **login.microsoftonline.com**, és ezután próbálja meg újból elindítani a varázslót.</span><span class="sxs-lookup"><span data-stu-id="8b86b-383">If you see that the web browser is stuck at "Authorizing...", disable/uncheck **Block third-party cookies and site data** setting (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching the wizard again.</span></span>
2. <span data-ttu-id="8b86b-384">A **Properties** (Tulajdonságok) oldalon:</span><span class="sxs-lookup"><span data-stu-id="8b86b-384">In the **Properties** page:</span></span>
    1. <span data-ttu-id="8b86b-385">Adja meg **CopyPipeline** a **feladatnév**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-385">Enter **CopyPipeline** for **Task name**.</span></span> <span data-ttu-id="8b86b-386">A feladatnév: a kimenetátirányítási mechanizmusával a data factory nevét.</span><span class="sxs-lookup"><span data-stu-id="8b86b-386">The task name is the name of the pipeline in your data factory.</span></span>
    2. <span data-ttu-id="8b86b-387">Adjon meg egy **leírás** a feladathoz (választható).</span><span class="sxs-lookup"><span data-stu-id="8b86b-387">Enter a **description** for the task (optional).</span></span>
    3. <span data-ttu-id="8b86b-388">A **feladat ütemben történik vagy ütemezett feladat**, tartsa a **futtatása rendszeres ütemezés szerint** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="8b86b-388">For **Task cadence or Task schedule**, keep the **Run regularly on schedule** option.</span></span> <span data-ttu-id="8b86b-389">Ha szeretné végrehajtani a feladatot csak egyszer ütemezés ismételten a Futtatás helyett, jelölje be **futtassa egyszer most**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-389">If you want to run this task only once instead of run repeatedly on a schedule, select **Run once now**.</span></span> <span data-ttu-id="8b86b-390">Választ ki, ha **futtassa egyszer most** beállítást, a [egyszeri folyamat](data-factory-create-pipelines.md#onetime-pipeline) jön létre.</span><span class="sxs-lookup"><span data-stu-id="8b86b-390">If you select, **Run once now** option, a [one-time pipeline](data-factory-create-pipelines.md#onetime-pipeline) is created.</span></span> 
    4. <span data-ttu-id="8b86b-391">Megtarthatja a **ismétlődő mintát**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-391">Keep the settings for **Recurring pattern**.</span></span> <span data-ttu-id="8b86b-392">Ez a feladat futtatása naponta a következő lépésben megadhatja a kezdő és befejező időpontja között.</span><span class="sxs-lookup"><span data-stu-id="8b86b-392">This task runs daily between the start and end times you specify in the next step.</span></span>
    5. <span data-ttu-id="8b86b-393">Módosítsa a **kezdő dátum/idő** való **04/21/2017**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-393">Change the **Start date time** to **04/21/2017**.</span></span> 
    6. <span data-ttu-id="8b86b-394">Módosítsa a **záró dátum és idő** való **04/25/2017**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-394">Change the **End date time** to **04/25/2017**.</span></span> <span data-ttu-id="8b86b-395">Érdemes lehet írja be a dátum, keresse meg azt a naptár helyett.</span><span class="sxs-lookup"><span data-stu-id="8b86b-395">You may want to type the date instead of browsing through the calendar.</span></span>     
    8. <span data-ttu-id="8b86b-396">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b86b-396">Click **Next**.</span></span>
      <span data-ttu-id="8b86b-397">![Másolja az eszköz - Tulajdonságok lap](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-397">![Copy Tool - Properties page](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)</span></span> 
3. <span data-ttu-id="8b86b-398">A **Source data store** (Forrásadattár) oldalon kattintson az **Azure Blob Storage** csempére.</span><span class="sxs-lookup"><span data-stu-id="8b86b-398">On the **Source data store** page, click **Azure Blob Storage** tile.</span></span> <span data-ttu-id="8b86b-399">Az oldal használatával megadhatja a forrásadattárat a másolási feladathoz.</span><span class="sxs-lookup"><span data-stu-id="8b86b-399">You use this page to specify the source data store for the copy task.</span></span> <span data-ttu-id="8b86b-400">Használhatja egy meglévő adattár társított szolgáltatását, vagy megadhat egy új adattárat.</span><span class="sxs-lookup"><span data-stu-id="8b86b-400">You can use an existing data store linked service (or) specify a new data store.</span></span> <span data-ttu-id="8b86b-401">Egy meglévő társított szolgáltatást használja, a kiválasztott **a meglévő összekapcsolt szolgáltatások** válassza ki a megfelelő társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8b86b-401">To use an existing linked service, you would select **FROM EXISTING LINKED SERVICES** and select the right linked service.</span></span> 
    <span data-ttu-id="8b86b-402">![Másolja az eszköz - forrás egy adattárolási lap](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-402">![Copy Tool - Source data store page](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)</span></span>
4. <span data-ttu-id="8b86b-403">A **Specify the Azure Blob storage account** (Az Azure Blob Storage-fiók megadása) oldalon:</span><span class="sxs-lookup"><span data-stu-id="8b86b-403">On the **Specify the Azure Blob storage account** page:</span></span>
   1. <span data-ttu-id="8b86b-404">Az automatikusan létrehozott nevet **kapcsolatnév**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-404">Keep the auto-generated name for **Connection name**.</span></span> <span data-ttu-id="8b86b-405">A kapcsolat nevének megadása típusú társított szolgáltatás neve: Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="8b86b-405">The connection name is the name of the linked service of type: Azure Storage.</span></span> 
   2. <span data-ttu-id="8b86b-406">Győződjön meg arról, hogy az **Account selection method** (Fiókválasztási módszer) mezőben a **From Azure subscriptions** (Azure-előfizetésekből) lehetőség van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="8b86b-406">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="8b86b-407">Válassza ki az Azure-előfizetéssel, vagy hagyja **válassza ki az összes** a **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-407">Select your Azure subscription or keep **Select all** for **Azure subscription**.</span></span>   
   4. <span data-ttu-id="8b86b-408">A kiválasztott előfizetéshez elérhető Azure Storage-fiókok listájából válasszon ki egy **Azure Storage-fiókot**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-408">Select an **Azure storage account** from the list of Azure storage accounts available in the selected subscription.</span></span> <span data-ttu-id="8b86b-409">Másik lehetőségként tárolási fiók beállításainak manuális megadása kiválasztásával **manuálisan adja meg** választás, a **kijelöléséről fiók**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-409">You can also choose to enter storage account settings manually by selecting **Enter manually** option for the **Account selection method**.</span></span>
   5. <span data-ttu-id="8b86b-410">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b86b-410">Click **Next**.</span></span> 
      <span data-ttu-id="8b86b-411">![Eszköz másolni - adja meg az Azure Blob storage-fiók](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-411">![Copy Tool - Specify the Azure Blob storage account](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)</span></span>
5. <span data-ttu-id="8b86b-412">A **Choose the input file or folder** (A bemeneti fájl vagy mappa kiválasztása) oldalon:</span><span class="sxs-lookup"><span data-stu-id="8b86b-412">On **Choose the input file or folder** page:</span></span>
   1. <span data-ttu-id="8b86b-413">Kattintson duplán a **adfblobcontainer**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-413">Double-click **adfblobcontainer**.</span></span>
   2. <span data-ttu-id="8b86b-414">Válassza ki **bemeneti**, és kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-414">Select **input**, and click **Choose**.</span></span> <span data-ttu-id="8b86b-415">Ebben a bemutatóban válassza ki a bemeneti mappát.</span><span class="sxs-lookup"><span data-stu-id="8b86b-415">In this walkthrough, you select the input folder.</span></span> <span data-ttu-id="8b86b-416">Kiválaszthatja a emp.txt fájl a mappában helyette.</span><span class="sxs-lookup"><span data-stu-id="8b86b-416">You could also select the emp.txt file in the folder instead.</span></span> 
      <span data-ttu-id="8b86b-417">![Másolja az eszköz – a bemeneti fájl vagy mappa kiválasztása](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-417">![Copy Tool - Choose the input file or folder](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)</span></span>
6. <span data-ttu-id="8b86b-418">Az a **válassza ki azt a bemeneti fájl vagy mappa** lap:</span><span class="sxs-lookup"><span data-stu-id="8b86b-418">On the **Choose the input file or folder** page:</span></span>
    1. <span data-ttu-id="8b86b-419">Ellenőrizze, hogy a **fájl vagy mappa** értéke **adfblobconnector/bemeneti**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-419">Confirm that the **file or folder** is set to **adfblobconnector/input**.</span></span> <span data-ttu-id="8b86b-420">Ha a fájlok runbookok, 2017/04/01, 2017/04/02, és így tovább, írja be például adfblobconnector/bemeneti / {year} / {month} / {day} fájlra vagy mappára.</span><span class="sxs-lookup"><span data-stu-id="8b86b-420">If the files are in sub folders, for example, 2017/04/01, 2017/04/02, and so on, enter adfblobconnector/input/{year}/{month}/{day} for file or folder.</span></span> <span data-ttu-id="8b86b-421">A szövegmező kívül TAB billentyű megnyomásával, lásd: három legördülő listában formátumok (éééé) év, hónap (hh) és nap (nn).</span><span class="sxs-lookup"><span data-stu-id="8b86b-421">When you press TAB out of the text box, you see three drop-down lists to select formats for year (yyyy), month (MM), and day (dd).</span></span> 
    2. <span data-ttu-id="8b86b-422">Ne adja meg az **fájl rekurzív módon másolja**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-422">Do not set **Copy file recursively**.</span></span> <span data-ttu-id="8b86b-423">Válassza ki ezt a beállítást a célhelyre másolja a fájlokat a rekurzív módon bejárás mappák segítségével.</span><span class="sxs-lookup"><span data-stu-id="8b86b-423">Select this option to recursively traverse through folders for files to be copied to the destination.</span></span> 
    3. <span data-ttu-id="8b86b-424">Nem a **bináris másolási** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="8b86b-424">Do not the **binary copy** option.</span></span> <span data-ttu-id="8b86b-425">Ezt a beállítást a célként megadott forrásfájl bináris elkészítésére.</span><span class="sxs-lookup"><span data-stu-id="8b86b-425">Select this option to perform a binary copy of source file to the destination.</span></span> <span data-ttu-id="8b86b-426">Nem jelölt ki ez a forgatókönyv, hogy a további beállítások a következő lapokon megjelenik.</span><span class="sxs-lookup"><span data-stu-id="8b86b-426">Do not select for this walkthrough so that you can see more options in the next pages.</span></span> 
    4. <span data-ttu-id="8b86b-427">Ellenőrizze, hogy a **tömörítési típus** értéke **nincs**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-427">Confirm that the **Compression type** is set to **None**.</span></span> <span data-ttu-id="8b86b-428">Válassza az ehhez a beállításhoz tartozó értéket, ha a forrásfájlok tömörített a támogatott formátumok egyikében.</span><span class="sxs-lookup"><span data-stu-id="8b86b-428">Select a value for this option if your source files are compressed in one of the supported formats.</span></span> 
    5. <span data-ttu-id="8b86b-429">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b86b-429">Click **Next**.</span></span>
    <span data-ttu-id="8b86b-430">![Másolja az eszköz – a bemeneti fájl vagy mappa kiválasztása](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-430">![Copy Tool - Choose the input file or folder](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)</span></span> 
7. <span data-ttu-id="8b86b-431">A **File format settings** (Fájlformátum beállításai) oldalon a fájl elemzése során a varázsló által automatikusan észlelt elválasztó karakterek és séma láthatók.</span><span class="sxs-lookup"><span data-stu-id="8b86b-431">On the **File format settings** page, you see the delimiters and the schema that is auto-detected by the wizard by parsing the file.</span></span> 
    1. <span data-ttu-id="8b86b-432">Erősítse meg a következő beállításokat: egy.</span><span class="sxs-lookup"><span data-stu-id="8b86b-432">Confirm the following options: a.</span></span> <span data-ttu-id="8b86b-433">A **fájlformátum** értéke **szövegformátum**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-433">The **file format** is set to **Text format**.</span></span> <span data-ttu-id="8b86b-434">A támogatott formátumok a legördülő listában tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="8b86b-434">You can see all the supported formats in the drop-down list.</span></span> <span data-ttu-id="8b86b-435">Például: JSON, Avro, ORC, Parquet.</span><span class="sxs-lookup"><span data-stu-id="8b86b-435">For example: JSON, Avro, ORC, Parquet.</span></span>
        <span data-ttu-id="8b86b-436">b.</span><span class="sxs-lookup"><span data-stu-id="8b86b-436">b.</span></span> <span data-ttu-id="8b86b-437">A **oszlop elválasztó** értéke `Comma (,)`.</span><span class="sxs-lookup"><span data-stu-id="8b86b-437">The **column delimiter** is set to `Comma (,)`.</span></span> <span data-ttu-id="8b86b-438">A legördülő listából válassza ki a Data Factory által támogatott más oszlop határolójeleket tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="8b86b-438">You can see the other column delimiters supported by Data Factory in the drop-down list.</span></span> <span data-ttu-id="8b86b-439">Egy egyéni elválasztó karakter is megadható.</span><span class="sxs-lookup"><span data-stu-id="8b86b-439">You can also specify a custom delimiter.</span></span>
        <span data-ttu-id="8b86b-440">c.</span><span class="sxs-lookup"><span data-stu-id="8b86b-440">c.</span></span> <span data-ttu-id="8b86b-441">A **sor elválasztó** értéke `Carriage Return + Line feed (\r\n)`.</span><span class="sxs-lookup"><span data-stu-id="8b86b-441">The **row delimiter** is set to `Carriage Return + Line feed (\r\n)`.</span></span> <span data-ttu-id="8b86b-442">A legördülő listából válassza ki a Data Factory által támogatott más sor határolójeleket tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="8b86b-442">You can see the other row delimiters supported by Data Factory in the drop-down list.</span></span> <span data-ttu-id="8b86b-443">Egy egyéni elválasztó karakter is megadható.</span><span class="sxs-lookup"><span data-stu-id="8b86b-443">You can also specify a custom delimiter.</span></span>
        <span data-ttu-id="8b86b-444">d.</span><span class="sxs-lookup"><span data-stu-id="8b86b-444">d.</span></span> <span data-ttu-id="8b86b-445">A **sorszám kihagyása** értéke **0**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-445">The **skip line count** is set to **0**.</span></span> <span data-ttu-id="8b86b-446">Ha azt szeretné, hogy néhány sorok figyelmen kívül hagyja a fájl elején, adja meg itt.</span><span class="sxs-lookup"><span data-stu-id="8b86b-446">If you want a few lines to be skipped at the top of the file, enter the number here.</span></span>
        <span data-ttu-id="8b86b-447">e.</span><span class="sxs-lookup"><span data-stu-id="8b86b-447">e.</span></span>  <span data-ttu-id="8b86b-448">A **adatok első sora oszlopneveket tartalmaz** nincs beállítva.</span><span class="sxs-lookup"><span data-stu-id="8b86b-448">The **first data row contains column names** is not set.</span></span> <span data-ttu-id="8b86b-449">Ha a forrásfájlok oszlopnevek az első sort tartalmaz, akkor válassza ezt a lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="8b86b-449">If the source files contain column names in the first row, select this option.</span></span>
        <span data-ttu-id="8b86b-450">f.</span><span class="sxs-lookup"><span data-stu-id="8b86b-450">f.</span></span> <span data-ttu-id="8b86b-451">A **üres oszlopérték tekinti null** beállítás.</span><span class="sxs-lookup"><span data-stu-id="8b86b-451">The **treat empty column value as null** option is set.</span></span>
    2. <span data-ttu-id="8b86b-452">Bontsa ki a **speciális beállítások** elérhető speciális beállítás megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8b86b-452">Expand **Advanced settings** to see advanced option available.</span></span>
    3. <span data-ttu-id="8b86b-453">A lap alján, tekintse meg a **előzetes** adatok emp.txt fájlból.</span><span class="sxs-lookup"><span data-stu-id="8b86b-453">At the bottom of the page, see the **preview** of data from the emp.txt file.</span></span>
    4. <span data-ttu-id="8b86b-454">Kattintson a **SÉMA** fül megtekintéséhez a séma, amely a varázsló következtetni rá, ha megnézi a forrás-fájlt.</span><span class="sxs-lookup"><span data-stu-id="8b86b-454">Click **SCHEMA** tab at the bottom to see the schema that the copy wizard inferred by looking at the data in the source file.</span></span>
    5. <span data-ttu-id="8b86b-455">Az elválasztó karakterek megtekintése és adatok előzetes megtekintése után kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="8b86b-455">Click **Next** after you review the delimiters and preview data.</span></span>
    <span data-ttu-id="8b86b-456">![Eszköz - fájl formátuma beállítások másolása](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-456">![Copy Tool - File format settings](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)</span></span>  
8. <span data-ttu-id="8b86b-457">Az a **cél adattároló lap**, jelölje be **Azure Blob Storage**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-457">On the **Destination data store page**, select **Azure Blob Storage**, and click **Next**.</span></span> <span data-ttu-id="8b86b-458">Az Azure Blob Storage mind a forrás és cél adattárolókhoz a forgatókönyv használ.</span><span class="sxs-lookup"><span data-stu-id="8b86b-458">You are using the Azure Blob Storage as both the source and destination data stores in this walkthrough.</span></span>    
    <span data-ttu-id="8b86b-459">![Eszköz - célkiszolgáló kijelölése adattár másolása](media/data-factory-azure-blob-connector/select-destination-data-store.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-459">![Copy Tool - select destination data store](media/data-factory-azure-blob-connector/select-destination-data-store.png)</span></span>
9. <span data-ttu-id="8b86b-460">A **adja meg az Azure Blob storage-fiók** lap:</span><span class="sxs-lookup"><span data-stu-id="8b86b-460">On **Specify the Azure Blob storage account** page:</span></span>
   1. <span data-ttu-id="8b86b-461">Adja meg **AzureStorageLinkedService** a a **kapcsolatnév** mező.</span><span class="sxs-lookup"><span data-stu-id="8b86b-461">Enter **AzureStorageLinkedService** for the **Connection name** field.</span></span>
   2. <span data-ttu-id="8b86b-462">Győződjön meg arról, hogy az **Account selection method** (Fiókválasztási módszer) mezőben a **From Azure subscriptions** (Azure-előfizetésekből) lehetőség van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="8b86b-462">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="8b86b-463">Jelölje ki az Azure-**előfizetést**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-463">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="8b86b-464">Válassza ki az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="8b86b-464">Select your Azure storage account.</span></span> 
   5. <span data-ttu-id="8b86b-465">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b86b-465">Click **Next**.</span></span>     
10. <span data-ttu-id="8b86b-466">Az a **válassza ki azt a kimeneti fájl vagy mappa** lap:</span><span class="sxs-lookup"><span data-stu-id="8b86b-466">On the **Choose the output file or folder** page:</span></span> 
    6. <span data-ttu-id="8b86b-467">Adja meg **mappa elérési útja** , **adfblobconnector/kimeneti / {year} / {month} / {day}**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-467">specify **Folder path** as **adfblobconnector/output/{year}/{month}/{day}**.</span></span> <span data-ttu-id="8b86b-468">Adja meg **lapon**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-468">Enter **TAB**.</span></span>
    7. <span data-ttu-id="8b86b-469">Az a **év**, jelölje be **éééé**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-469">For the **year**, select **yyyy**.</span></span>
    8. <span data-ttu-id="8b86b-470">Az a **hónap**, győződjön meg arról, hogy van-e állítva **MM**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-470">For the **month**, confirm that it is set to **MM**.</span></span>
    9. <span data-ttu-id="8b86b-471">Az a **nap**, győződjön meg arról, hogy van-e állítva **nn**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-471">For the **day**, confirm that it is set to **dd**.</span></span>
    10. <span data-ttu-id="8b86b-472">Ellenőrizze, hogy a **tömörítési típus** értéke **nincs**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-472">Confirm that the **compression type** is set to **None**.</span></span>
    11. <span data-ttu-id="8b86b-473">Ellenőrizze, hogy a **viselkedés másolása** értéke **fájlok egyesítése**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-473">Confirm that the **copy behavior** is set to **Merge files**.</span></span> <span data-ttu-id="8b86b-474">Ha a kimeneti fájl ugyanazzal a névvel már létezik, az új tartalom kerüljön végén ugyanazt a fájlt.</span><span class="sxs-lookup"><span data-stu-id="8b86b-474">If the output file with the same name already exists, the new content is added to the same file at the end.</span></span>
    12. <span data-ttu-id="8b86b-475">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b86b-475">Click **Next**.</span></span>
    <span data-ttu-id="8b86b-476">![Másolja az eszköz - a kimeneti fájl vagy mappa](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-476">![Copy Tool - Choose output file or folder](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)</span></span>
11. <span data-ttu-id="8b86b-477">Az a **fájl formázási beállítások** lapon tekintse át a beállításokat, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-477">On the **File format settings** page, review the settings, and click **Next**.</span></span> <span data-ttu-id="8b86b-478">A további beállítások egyik egy fejléc hozzáadását a kimeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="8b86b-478">One of the additional options here is to add a header to the output file.</span></span> <span data-ttu-id="8b86b-479">Ha ezt a beállítást választja, a fejlécsor hozzá rendelkező oszlopok neveinek a séma, a forrás.</span><span class="sxs-lookup"><span data-stu-id="8b86b-479">If you select that option, a header row is added with names of the columns from the schema of the source.</span></span> <span data-ttu-id="8b86b-480">Az alapértelmezett oszlopnevek átnevezheti a séma a következő adatforrás megtekintésekor.</span><span class="sxs-lookup"><span data-stu-id="8b86b-480">You can rename the default column names when viewing the schema for the source.</span></span> <span data-ttu-id="8b86b-481">Például megváltoztathatja az első oszlop Utónév és Vezetéknév második oszlopnak.</span><span class="sxs-lookup"><span data-stu-id="8b86b-481">For example, you could change the first column to First Name and second column to Last Name.</span></span> <span data-ttu-id="8b86b-482">Ezt követően a kimeneti fájl jön létre ezekkel a nevekkel fejléccel oszlopnevekként.</span><span class="sxs-lookup"><span data-stu-id="8b86b-482">Then, the output file is generated with a header with these names as column names.</span></span> 
    <span data-ttu-id="8b86b-483">![Eszköz - cél formátum beállításainak fájl másolása](media/data-factory-azure-blob-connector/file-format-destination.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-483">![Copy Tool - File format settings for destination](media/data-factory-azure-blob-connector/file-format-destination.png)</span></span>
12. <span data-ttu-id="8b86b-484">A a **Teljesítménybeállítások** lapján ellenőrizze, hogy **egységek cloud** és **másolatok párhuzamos** vannak beállítva, hogy **automatikus**, és kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="8b86b-484">On the **Performance settings** page, confirm that **cloud units** and **parallel copies** are set to **Auto**, and click Next.</span></span> <span data-ttu-id="8b86b-485">Ezek a beállítások kapcsolatos részletekért lásd: [másolása tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md#parallel-copy).</span><span class="sxs-lookup"><span data-stu-id="8b86b-485">For details about these settings, see [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md#parallel-copy).</span></span>
    <span data-ttu-id="8b86b-486">![Eszköz - teljesítmény beállítások másolása](media/data-factory-azure-blob-connector/copy-performance-settings.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-486">![Copy Tool - Performance settings](media/data-factory-azure-blob-connector/copy-performance-settings.png)</span></span> 
14. <span data-ttu-id="8b86b-487">Az a **összegzés** lapon tekintse át az összes beállítást (a feladat tulajdonságai, a beállításokat a forrás és cél és a beállítások), és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-487">On the **Summary** page, review all settings (task properties, settings for source and destination, and copy settings), and click **Next**.</span></span>
    <span data-ttu-id="8b86b-488">![Másolja az eszköz - összegző lapja](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-488">![Copy Tool - Summary page](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)</span></span>
15. <span data-ttu-id="8b86b-489">Tekintse át a **Summary** (Összegzés) oldalon szereplő információkat, majd kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8b86b-489">Review information in the **Summary** page, and click **Finish**.</span></span> <span data-ttu-id="8b86b-490">A varázsló létrehoz két társított szolgáltatást, két adathalmazt (bemeneti és kimeneti), valamint egy folyamatot a data factoryban (ahonnét elindította a Másolás varázslót).</span><span class="sxs-lookup"><span data-stu-id="8b86b-490">The wizard creates two linked services, two datasets (input and output), and one pipeline in the data factory (from where you launched the Copy Wizard).</span></span>
    <span data-ttu-id="8b86b-491">![Másolja az eszköz - központi telepítés lap](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-491">![Copy Tool - Deployment page](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)</span></span>

### <a name="monitor-the-pipeline-copy-task"></a><span data-ttu-id="8b86b-492">A figyelő az adatcsatorna (feladatok)</span><span class="sxs-lookup"><span data-stu-id="8b86b-492">Monitor the pipeline (copy task)</span></span>

1. <span data-ttu-id="8b86b-493">Kattintson a hivatkozásra `Click here to monitor copy pipeline` a a **telepítési** lap.</span><span class="sxs-lookup"><span data-stu-id="8b86b-493">Click the link `Click here to monitor copy pipeline` on the **Deployment** page.</span></span> 
2. <span data-ttu-id="8b86b-494">Megjelenik a **felügyeletéhez és kezeléséhez az alkalmazás** egy külön lapján.  ![Megfigyelés és kezelés alkalmazás](media/data-factory-azure-blob-connector/monitor-manage-app.png)</span><span class="sxs-lookup"><span data-stu-id="8b86b-494">You should see the **Monitor and Manage application** in a separate tab.  ![Monitor and Manage App](media/data-factory-azure-blob-connector/monitor-manage-app.png)</span></span>
3. <span data-ttu-id="8b86b-495">Módosítás a **start** időt legfelül `04/19/2017` és **end** beosztásba `04/27/2017`, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-495">Change the **start** time at the top to `04/19/2017` and **end** time to `04/27/2017`, and then click **Apply**.</span></span> 
4. <span data-ttu-id="8b86b-496">Az öt tevékenység windows kell megjelennie a **tevékenység WINDOWS** listája.</span><span class="sxs-lookup"><span data-stu-id="8b86b-496">You should see five activity windows in the **ACTIVITY WINDOWS** list.</span></span> <span data-ttu-id="8b86b-497">A **WindowStart** alkalommal minden nap közé tartozik folyamat kezdetétől a csővezeték-befejezési időpontjával.</span><span class="sxs-lookup"><span data-stu-id="8b86b-497">The **WindowStart** times should cover all days from pipeline start to pipeline end times.</span></span> 
5. <span data-ttu-id="8b86b-498">Kattintson a **frissítése** gombra kattint, az a **tevékenység WINDOWS** lista néhány alkalommal amíg meg nem jelenik a tevékenység windows állapotának a Kész értékre van állítva.</span><span class="sxs-lookup"><span data-stu-id="8b86b-498">Click **Refresh** button for the **ACTIVITY WINDOWS** list a few times until you see the status of all the activity windows is set to Ready.</span></span> 
6. <span data-ttu-id="8b86b-499">Most győződjön meg arról, hogy a kimeneti fájlok adfblobconnector tároló kimeneti mappában vannak-e létre.</span><span class="sxs-lookup"><span data-stu-id="8b86b-499">Now, verify that the output files are generated in the output folder of adfblobconnector container.</span></span> <span data-ttu-id="8b86b-500">A következő gyökérmappa-szerkezetében kimeneti mappában kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="8b86b-500">You should see the following folder structure in the output folder:</span></span> 
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25    
    ```
<span data-ttu-id="8b86b-501">Figyelése és kezelése az adat-előállítók kapcsolatos részletes információkért lásd: [figyelése és kezelése a Data Factory-folyamathoz](data-factory-monitor-manage-app.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="8b86b-501">For detailed information about monitoring and managing data factories, see [Monitor and manage Data Factory pipeline](data-factory-monitor-manage-app.md) article.</span></span> 
 
### <a name="data-factory-entities"></a><span data-ttu-id="8b86b-502">Data Factory entitások</span><span class="sxs-lookup"><span data-stu-id="8b86b-502">Data Factory entities</span></span>
<span data-ttu-id="8b86b-503">Most lépjen vissza a lap a Data Factory kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="8b86b-503">Now, switch back to the tab with the Data Factory home page.</span></span> <span data-ttu-id="8b86b-504">Figyelje meg, hogy van két összekapcsolt szolgáltatások, két adatkészletet, és a data factory egyik adatcsatornáinak most.</span><span class="sxs-lookup"><span data-stu-id="8b86b-504">Notice that there are two linked services, two datasets, and one pipeline in your data factory now.</span></span> 

![Data Factory kezdőlapján entitások](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

<span data-ttu-id="8b86b-506">Kattintson a **Szerző és központi telepítése** elindíthatja a Data Factory Editor.</span><span class="sxs-lookup"><span data-stu-id="8b86b-506">Click **Author and deploy** to launch Data Factory Editor.</span></span> 

![A Data Factory szerkesztője](media/data-factory-azure-blob-connector/data-factory-editor.png)

<span data-ttu-id="8b86b-508">A következő adat-előállító entitások az adat-előállítóban kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="8b86b-508">You should see the following Data Factory entities in your data factory:</span></span> 

 - <span data-ttu-id="8b86b-509">Két társított szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="8b86b-509">Two linked services.</span></span> <span data-ttu-id="8b86b-510">Egyet a forrás- és a cél a másik egy.</span><span class="sxs-lookup"><span data-stu-id="8b86b-510">One for the source and the other one for the destination.</span></span> <span data-ttu-id="8b86b-511">Az összekapcsolt szolgáltatások tekintse meg az azonos Azure Storage-fiók ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="8b86b-511">Both the linked services refer to the same Azure Storage account in this walkthrough.</span></span> 
 - <span data-ttu-id="8b86b-512">Két adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="8b86b-512">Two datasets.</span></span> <span data-ttu-id="8b86b-513">Egy bemeneti adatkészlet és egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="8b86b-513">An input dataset and an output dataset.</span></span> <span data-ttu-id="8b86b-514">Ebben a bemutatóban mindkét blob tárolóhoz használni de különböző mappák (bemeneti és kimeneti) hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="8b86b-514">In this walkthrough, both use the same blob container but refer to different folders (input and output).</span></span>
 - <span data-ttu-id="8b86b-515">Egy folyamat.</span><span class="sxs-lookup"><span data-stu-id="8b86b-515">A pipeline.</span></span> <span data-ttu-id="8b86b-516">A folyamat, amely egy blob-forrás- és egy blob fogadó használja az adatok másolása az Azure blob helyére máshová az Azure blob másolási tevékenységet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8b86b-516">The pipeline contains a copy activity that uses a blob source and a blob sink to copy data from an Azure blob location to another Azure blob location.</span></span> 

<span data-ttu-id="8b86b-517">Az alábbi szakaszok nyújtanak további információt arról ezeket az entitásokat.</span><span class="sxs-lookup"><span data-stu-id="8b86b-517">The following sections provide more information about these entities.</span></span> 

#### <a name="linked-services"></a><span data-ttu-id="8b86b-518">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="8b86b-518">Linked services</span></span>
<span data-ttu-id="8b86b-519">Két összekapcsolt szolgáltatások kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="8b86b-519">You should see two linked services.</span></span> <span data-ttu-id="8b86b-520">Egyet a forrás- és a cél a másik egy.</span><span class="sxs-lookup"><span data-stu-id="8b86b-520">One for the source and the other one for the destination.</span></span> <span data-ttu-id="8b86b-521">Ebben a bemutatóban mindkét definíciók ugyanúgy néznek ki, kivéve a neveket.</span><span class="sxs-lookup"><span data-stu-id="8b86b-521">In this walkthrough, both definitions look the same except for the names.</span></span> <span data-ttu-id="8b86b-522">A **típus** társított szolgáltatás beállítása **AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-522">The **type** of the linked service is set to **AzureStorage**.</span></span> <span data-ttu-id="8b86b-523">A társított szolgáltatás definíciójának legfontosabb tulajdonsága nem a **connectionString**, amely használják adat-előállító csatlakozni az Azure Storage-fiók futásidőben.</span><span class="sxs-lookup"><span data-stu-id="8b86b-523">Most important property of the linked service definition is the **connectionString**, which is used by Data Factory to connect to your Azure Storage account at runtime.</span></span> <span data-ttu-id="8b86b-524">Figyelmen kívül hagyja a hubName tulajdonság a-definícióban.</span><span class="sxs-lookup"><span data-stu-id="8b86b-524">Ignore the hubName property in the definition.</span></span> 

##### <a name="source-blob-storage-linked-service"></a><span data-ttu-id="8b86b-525">Forrás blob storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8b86b-525">Source blob storage linked service</span></span>
```json
{
    "name": "Source-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

##### <a name="destination-blob-storage-linked-service"></a><span data-ttu-id="8b86b-526">Cél blob storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="8b86b-526">Destination blob storage linked service</span></span>

```json
{
    "name": "Destination-BlobStorage-z4y",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=mystorageaccount;AccountKey=**********"
        }
    }
}
```

<span data-ttu-id="8b86b-527">Az Azure tárolás társított szolgáltatásának kapcsolatos további információkért lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8b86b-527">For more information about Azure Storage linked service, see [Linked service properties](#linked-service-properties) section.</span></span> 

#### <a name="datasets"></a><span data-ttu-id="8b86b-528">Adathalmazok</span><span class="sxs-lookup"><span data-stu-id="8b86b-528">Datasets</span></span>
<span data-ttu-id="8b86b-529">Két adatkészletet van: egy bemeneti adatkészlet és egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="8b86b-529">There are two datasets: an input dataset and an output dataset.</span></span> <span data-ttu-id="8b86b-530">A DataSet adatkészlet típusúra **AzureBlob** is.</span><span class="sxs-lookup"><span data-stu-id="8b86b-530">The type of the dataset is set to **AzureBlob** for both.</span></span> 

<span data-ttu-id="8b86b-531">A bemeneti adatkészlet mutat a **bemeneti** mappában található a **adfblobconnector** blob tároló.</span><span class="sxs-lookup"><span data-stu-id="8b86b-531">The input dataset points to the **input** folder of the **adfblobconnector** blob container.</span></span> <span data-ttu-id="8b86b-532">A **külső** tulajdonsága **igaz** ehhez az adatkészlethez az adatok nem előállított, a másolási tevékenység során, amely ehhez az adatkészlethez bemeneti adatokként a folyamat.</span><span class="sxs-lookup"><span data-stu-id="8b86b-532">The **external** property is set to **true** for this dataset as the data is not produced by the pipeline with the copy activity that takes this dataset as an input.</span></span> 

<span data-ttu-id="8b86b-533">A kimeneti adatkészlet mutat a **kimeneti** a azonos blob-tároló mappa.</span><span class="sxs-lookup"><span data-stu-id="8b86b-533">The output dataset points to the **output** folder of the same blob container.</span></span> <span data-ttu-id="8b86b-534">A kimeneti adatkészletet is használ az évet, hónapot és napot a **SliceStart** rendszerváltozó dinamikusan kiértékelése a kimeneti fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="8b86b-534">The output dataset also uses the year, month, and day of the **SliceStart** system variable to dynamically evaluate the path for the output file.</span></span> <span data-ttu-id="8b86b-535">Függvények és a Data Factory által támogatott rendszerváltozók listáját lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="8b86b-535">For a list of functions and system variables supported by Data Factory, see [Data Factory functions and system variables](data-factory-functions-variables.md).</span></span> <span data-ttu-id="8b86b-536">A **külső** tulajdonsága **hamis** (alapértelmezett érték), mert ez az adatkészlet hozzák a feldolgozási sor.</span><span class="sxs-lookup"><span data-stu-id="8b86b-536">The **external** property is set to **false** (default value) because this dataset is produced by the pipeline.</span></span> 

<span data-ttu-id="8b86b-537">Azure Blob-adathalmazra által támogatott tulajdonságokról kapcsolatos további információkért lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8b86b-537">For more information about properties supported by Azure Blob dataset, see [Dataset properties](#dataset-properties) section.</span></span>

##### <a name="input-dataset"></a><span data-ttu-id="8b86b-538">Bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="8b86b-538">Input dataset</span></span>

```json
{
    "name": "InputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Source-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/input/",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```

##### <a name="output-dataset"></a><span data-ttu-id="8b86b-539">Kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="8b86b-539">Output dataset</span></span>

```json
{
    "name": "OutputDataset-z4y",
    "properties": {
        "structure": [
            { "name": "Prop_0", "type": "String" },
            { "name": "Prop_1", "type": "String" }
        ],
        "type": "AzureBlob",
        "linkedServiceName": "Destination-BlobStorage-z4y",
        "typeProperties": {
            "folderPath": "adfblobconnector/output/{year}/{month}/{day}",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            },
            "partitionedBy": [
                { "name": "year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
                { "name": "month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
                { "name": "day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }
            ]
        },
        "availability": {
            "frequency": "Day",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }
}
```

#### <a name="pipeline"></a><span data-ttu-id="8b86b-540">Folyamat</span><span class="sxs-lookup"><span data-stu-id="8b86b-540">Pipeline</span></span>
<span data-ttu-id="8b86b-541">A folyamat csak egy tevékenység rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8b86b-541">The pipeline has just one activity.</span></span> <span data-ttu-id="8b86b-542">A **típus** a tevékenység értéke **másolási**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-542">The **type** of the activity is set to **Copy**.</span></span>  <span data-ttu-id="8b86b-543">A tevékenység típusa tulajdonságai nincsenek két részből áll, egy forrás és a többi fogadó egy.</span><span class="sxs-lookup"><span data-stu-id="8b86b-543">In the type properties for the activity, there are two sections, one for source and the other one for sink.</span></span> <span data-ttu-id="8b86b-544">A forrás típusa **BlobSource** , a tevékenység egy blob-tároló az adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="8b86b-544">The source type is set to **BlobSource** as the activity is copying data from a blob storage.</span></span> <span data-ttu-id="8b86b-545">A fogadó típusa **BlobSink** adatok másolása egy blob Storage tevékenységként.</span><span class="sxs-lookup"><span data-stu-id="8b86b-545">The sink type is set to **BlobSink** as the activity copying data to a blob storage.</span></span> <span data-ttu-id="8b86b-546">A másolási tevékenység veszi InputDataset-z4y bemeneteként és OutputDataset-z4y kimeneteként.</span><span class="sxs-lookup"><span data-stu-id="8b86b-546">The copy activity takes InputDataset-z4y as the input and OutputDataset-z4y as the output.</span></span> 

<span data-ttu-id="8b86b-547">BlobSource és BlobSink által támogatott tulajdonságokról kapcsolatos további információkért lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="8b86b-547">For more information about properties supported by BlobSource and BlobSink, see [Copy activity properties](#copy-activity-properties) section.</span></span> 

```json
{
    "name": "CopyPipeline",
    "properties": {
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource",
                        "recursive": false
                    },
                    "sink": {
                        "type": "BlobSink",
                        "copyBehavior": "MergeFiles",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataset-z4y"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataset-z4y"
                    }
                ],
                "policy": {
                    "timeout": "1.00:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "style": "StartOfInterval",
                    "retry": 3,
                    "longRetry": 0,
                    "longRetryInterval": "00:00:00"
                },
                "scheduler": {
                    "frequency": "Day",
                    "interval": 1
                },
                "name": "Activity-0-Blob path_ adfblobconnector_input_->OutputDataset-z4y"
            }
        ],
        "start": "2017-04-21T22:34:00Z",
        "end": "2017-04-25T05:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
```

## <a name="json-examples-for-copying-data-to-and-from-blob-storage"></a><span data-ttu-id="8b86b-548">Adatok másolása, és a Blob Storage JSON példák</span><span class="sxs-lookup"><span data-stu-id="8b86b-548">JSON examples for copying data to and from Blob Storage</span></span>  
<span data-ttu-id="8b86b-549">Az alábbi példák megadják minta JSON-definíciókat tartalmazzon, segítségével hozzon létre egy folyamatot [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="8b86b-549">The following examples provide sample JSON definitions that you can use to create a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="8b86b-550">Adatok másolása az Azure Blob Storage és az Azure SQL Database mutatnak.</span><span class="sxs-lookup"><span data-stu-id="8b86b-550">They show how to copy data to and from Azure Blob Storage and Azure SQL Database.</span></span> <span data-ttu-id="8b86b-551">Azonban az adatok átmásolhatók **közvetlenül** a forrásokban, sem a megadott nyelő [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a másolási tevékenység során az Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="8b86b-551">However, data can be copied **directly** from any of sources to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using the Copy Activity in Azure Data Factory.</span></span>

### <a name="json-example-copy-data-from-blob-storage-to-sql-database"></a><span data-ttu-id="8b86b-552">JSON-NÁ. példa: Adatok másolása az Blob-tároló az SQL-adatbázis</span><span class="sxs-lookup"><span data-stu-id="8b86b-552">JSON Example: Copy data from Blob Storage to SQL Database</span></span>
<span data-ttu-id="8b86b-553">A következő példában:</span><span class="sxs-lookup"><span data-stu-id="8b86b-553">The following sample shows:</span></span>

1. <span data-ttu-id="8b86b-554">A társított szolgáltatás típusa [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8b86b-554">A linked service of type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="8b86b-555">A társított szolgáltatás típusa [AzureStorage](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8b86b-555">A linked service of type [AzureStorage](#linked-service-properties).</span></span>
3. <span data-ttu-id="8b86b-556">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8b86b-556">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](#dataset-properties).</span></span>
4. <span data-ttu-id="8b86b-557">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8b86b-557">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="8b86b-558">A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [BlobSource](#copy-activity-properties) és [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="8b86b-558">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [BlobSource](#copy-activity-properties) and [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="8b86b-559">A minta másolatok idősorozat adatokat az Azure blob-Azure SQL táblázat óránként.</span><span class="sxs-lookup"><span data-stu-id="8b86b-559">The sample copies time-series data from an Azure blob to an Azure SQL table hourly.</span></span> <span data-ttu-id="8b86b-560">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="8b86b-560">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="8b86b-561">**Az Azure SQL társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="8b86b-561">**Azure SQL linked service:**</span></span>

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="8b86b-562">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="8b86b-562">**Azure Storage linked service:**</span></span>

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="8b86b-563">Az Azure Data Factory két típusú Azure Storage társított szolgáltatásokat támogat: **AzureStorage** és **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-563">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="8b86b-564">Az első címtárra a kapcsolati karakterlánc, amely tartalmazza a fiókkulcs ad meg, és a későbbi egy, a közös hozzáférésű Jogosultságkód (SAS) Uri megadása.</span><span class="sxs-lookup"><span data-stu-id="8b86b-564">For the first one, you specify the connection string that includes the account key and for the later one, you specify the Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="8b86b-565">Lásd: [összekapcsolt szolgáltatások](#linked-service-properties) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="8b86b-565">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="8b86b-566">**Az Azure Blob bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="8b86b-566">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="8b86b-567">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="8b86b-567">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="8b86b-568">A mappa elérési útját és nevét a BLOB dinamikusan értékeli ki a kezdési időt a szelet által feldolgozott alapján.</span><span class="sxs-lookup"><span data-stu-id="8b86b-568">The folder path and file name for the blob are dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="8b86b-569">A mappa elérési útját használja év, hónap és nap részét kezdési idejét, valamint fájl nevét a kezdő időpontja óra részét.</span><span class="sxs-lookup"><span data-stu-id="8b86b-569">The folder path uses year, month, and day part of the start time and file name uses the hour part of the start time.</span></span> <span data-ttu-id="8b86b-570">"external": "true" beállítás arról értesíti az, hogy a tábla az adat-előállítóban külső, és egy tevékenység adat-előállító nem hozzák adat-Előállítóban.</span><span class="sxs-lookup"><span data-stu-id="8b86b-570">“external”: “true” setting informs Data Factory that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```
<span data-ttu-id="8b86b-571">**Az Azure SQL kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="8b86b-571">**Azure SQL output dataset:**</span></span>

<span data-ttu-id="8b86b-572">A mintaadatok másolatok táblához "MyTable" Azure SQL adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="8b86b-572">The sample copies data to a table named “MyTable” in an Azure SQL database.</span></span> <span data-ttu-id="8b86b-573">A tábla létrehozása az Azure SQL adatbázis azonos számú oszlopot tartalmaz a Blob CSV-fájl várt.</span><span class="sxs-lookup"><span data-stu-id="8b86b-573">Create the table in your Azure SQL database with the same number of columns as you expect the Blob CSV file to contain.</span></span> <span data-ttu-id="8b86b-574">Új sorok hozzáadásakor a tábla minden órában.</span><span class="sxs-lookup"><span data-stu-id="8b86b-574">New rows are added to the table every hour.</span></span>

```json
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
<span data-ttu-id="8b86b-575">**A másolási tevékenység során a Blob-forrás és fogadó SQL-feldolgozási folyamat:**</span><span class="sxs-lookup"><span data-stu-id="8b86b-575">**A copy activity in a pipeline with Blob source and SQL sink:**</span></span>

<span data-ttu-id="8b86b-576">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="8b86b-576">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="8b86b-577">Az adatcsatorna JSON-definícióból a **forrás** típusúra **BlobSource** és **fogadó** típusúra **SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-577">In the pipeline JSON definition, the **source** type is set to **BlobSource** and **sink** type is set to **SqlSink**.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "SqlSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
### <a name="json-example-copy-data-from-azure-sql-to-azure-blob"></a><span data-ttu-id="8b86b-578">JSON-NÁ. példa: Adatok másolása az Azure SQL Azure-blobba</span><span class="sxs-lookup"><span data-stu-id="8b86b-578">JSON Example: Copy data from Azure SQL to Azure Blob</span></span>
<span data-ttu-id="8b86b-579">A következő példában:</span><span class="sxs-lookup"><span data-stu-id="8b86b-579">The following sample shows:</span></span>

1. <span data-ttu-id="8b86b-580">A társított szolgáltatás típusa [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8b86b-580">A linked service of type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="8b86b-581">A társított szolgáltatás típusa [AzureStorage](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="8b86b-581">A linked service of type [AzureStorage](#linked-service-properties).</span></span>
3. <span data-ttu-id="8b86b-582">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8b86b-582">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="8b86b-583">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="8b86b-583">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](#dataset-properties).</span></span>
5. <span data-ttu-id="8b86b-584">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) és [BlobSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="8b86b-584">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) and [BlobSink](#copy-activity-properties).</span></span>

<span data-ttu-id="8b86b-585">A minta-idősoros adatok az Azure SQL tábla másolja az Azure blob óránként.</span><span class="sxs-lookup"><span data-stu-id="8b86b-585">The sample copies time-series data from an Azure SQL table to an Azure blob hourly.</span></span> <span data-ttu-id="8b86b-586">A mintákat a következő szakaszok ismertetik ezeket a mintákat használt JSON-tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="8b86b-586">The JSON properties used in these samples are described in sections following the samples.</span></span>

<span data-ttu-id="8b86b-587">**Az Azure SQL társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="8b86b-587">**Azure SQL linked service:**</span></span>

```json
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
<span data-ttu-id="8b86b-588">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="8b86b-588">**Azure Storage linked service:**</span></span>

```json
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
<span data-ttu-id="8b86b-589">Az Azure Data Factory két típusú Azure Storage társított szolgáltatásokat támogat: **AzureStorage** és **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-589">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="8b86b-590">Az első címtárra a kapcsolati karakterlánc, amely tartalmazza a fiókkulcs ad meg, és a későbbi egy, a közös hozzáférésű Jogosultságkód (SAS) Uri megadása.</span><span class="sxs-lookup"><span data-stu-id="8b86b-590">For the first one, you specify the connection string that includes the account key and for the later one, you specify the Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="8b86b-591">Lásd: [összekapcsolt szolgáltatások](#linked-service-properties) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="8b86b-591">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="8b86b-592">**Az Azure SQL bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="8b86b-592">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="8b86b-593">A minta azt feltételezi, hogy létrehozott egy tábla "MyTable" Azure SQL, egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8b86b-593">The sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="8b86b-594">"External" beállítása: "true" tájékoztatja Data Factory szolgáltatásnak, hogy a tábla külső data factoryval való, és nem hozzák adat-előállító tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8b86b-594">Setting “external”: ”true” informs Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.</span></span>

```json
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

<span data-ttu-id="8b86b-595">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="8b86b-595">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="8b86b-596">Adatot ír egy új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="8b86b-596">Data is written to a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="8b86b-597">A mappa elérési útját a BLOB a szelet által feldolgozott kezdési ideje alapján dinamikusan történik.</span><span class="sxs-lookup"><span data-stu-id="8b86b-597">The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed.</span></span> <span data-ttu-id="8b86b-598">A mappa elérési útját használja, év, hónap, nap és a kezdési idő órában részeit.</span><span class="sxs-lookup"><span data-stu-id="8b86b-598">The folder path uses year, month, day, and hours parts of the start time.</span></span>

```json
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": { "type": "DateTime",  "date": "SliceStart", "format": "yyyy" } },
        { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
        { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
        { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "HH" } }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

<span data-ttu-id="8b86b-599">**A másolási tevékenység során az SQL-forrás és fogadó Blob egy folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="8b86b-599">**A copy activity in a pipeline with SQL source and Blob sink:**</span></span>

<span data-ttu-id="8b86b-600">A feldolgozási sor tartalmazza a másolási tevékenység, amely a bemeneti és kimeneti adatkészletek használatára van konfigurálva, és óránkénti futásra nem ütemezték.</span><span class="sxs-lookup"><span data-stu-id="8b86b-600">The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour.</span></span> <span data-ttu-id="8b86b-601">Az adatcsatorna JSON-definícióból a **forrás** típusúra **SqlSource** és **fogadó** típusúra **BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="8b86b-601">In the pipeline JSON definition, the **source** type is set to **SqlSource** and **sink** type is set to **BlobSink**.</span></span> <span data-ttu-id="8b86b-602">A megadott SQL-lekérdezést a **SqlReaderQuery** tulajdonság kiválasztása az adatok másolása az elmúlt órában.</span><span class="sxs-lookup"><span data-stu-id="8b86b-602">The SQL query specified for the **SqlReaderQuery** property selects the data in the past hour to copy.</span></span>

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureSQLtoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureSQLInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "SqlSource",
                        "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                },
                   "scheduler": {
                      "frequency": "Hour",
                      "interval": 1
                },
                "policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "OldestFirst",
                      "retry": 0,
                      "timeout": "01:00:00"
                }
              }
         ]
    }
}
```

> [!NOTE]
> <span data-ttu-id="8b86b-603">Képezze le a fogadó adatkészletből oszlopok forrás adatkészletből oszlopokat, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="8b86b-603">To map columns from source dataset to columns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="8b86b-604">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="8b86b-604">Performance and Tuning</span></span>
<span data-ttu-id="8b86b-605">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) tájékozódhat az kulcsfontosságú szerepet játszik adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon optimalizálhatja azt, hogy hatás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="8b86b-605">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.</span></span>
