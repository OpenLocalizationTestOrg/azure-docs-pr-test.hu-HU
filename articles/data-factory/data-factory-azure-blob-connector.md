---
title: az Azure Blob Storage aaaCopy adatok |} Microsoft Docs
description: "Ismerje meg, hogyan toocopy blob az Azure Data Factoryben az adatok. A minta: hogyan toocopy adatok tooand az Azure Blob Storage és az Azure SQL Database."
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
ms.openlocfilehash: 8428c64e8e8b1084b3f2f680c4e1819559e4ffa3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooor-from-azure-blob-storage-using-azure-data-factory"></a><span data-ttu-id="c704c-105">Adatok tooor másolása az Azure Blob Storage Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="c704c-105">Copy data tooor from Azure Blob Storage using Azure Data Factory</span></span>
<span data-ttu-id="c704c-106">Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toocopy adatok tooand Azure Blob Storage-ból a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c704c-106">This article explains how toouse hello Copy Activity in Azure Data Factory toocopy data tooand from Azure Blob Storage.</span></span> <span data-ttu-id="c704c-107">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="c704c-107">It builds on hello [Data Movement Activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

## <a name="overview"></a><span data-ttu-id="c704c-108">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c704c-108">Overview</span></span>
<span data-ttu-id="c704c-109">Bármely támogatott forráshierarchiából adatokat tooAzure Blob Storage tárolóban tárolja, és az Azure Blob Storage támogatott tooany fogadó adatok tárolásához adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="c704c-109">You can copy data from any supported source data store tooAzure Blob Storage or from Azure Blob Storage tooany supported sink data store.</span></span> <span data-ttu-id="c704c-110">hello következő tábla adattárolókhoz támogatott adatforrások listáját tartalmazza vagy hello másolási tevékenység által fogadók esetében.</span><span class="sxs-lookup"><span data-stu-id="c704c-110">hello following table provides a list of data stores supported as sources or sinks by hello copy activity.</span></span> <span data-ttu-id="c704c-111">Adatok áthelyezése például **a** egy SQL Server-adatbázist vagy egy Azure SQL-adatbázis **való** egy Azure blobtárolóba.</span><span class="sxs-lookup"><span data-stu-id="c704c-111">For example, you can move data **from** a SQL Server database or an Azure SQL database **to** an Azure blob storage.</span></span> <span data-ttu-id="c704c-112">És adatainak másolhatja **a** Azure blob storage **való** Azure SQL Data Warehouse vagy egy Azure Cosmos DB gyűjteményt.</span><span class="sxs-lookup"><span data-stu-id="c704c-112">And, you can copy data **from** Azure blob storage **to** an Azure SQL Data Warehouse or an Azure Cosmos DB collection.</span></span> 

## <a name="supported-scenarios"></a><span data-ttu-id="c704c-113">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="c704c-113">Supported scenarios</span></span>
<span data-ttu-id="c704c-114">Adatokat másolhat **az Azure Blob Storage** toohello a következő adatokat tárolja:</span><span class="sxs-lookup"><span data-stu-id="c704c-114">You can copy data **from Azure Blob Storage** toohello following data stores:</span></span>

[!INCLUDE [data-factory-supported-sink](../../includes/data-factory-supported-sinks.md)]

<span data-ttu-id="c704c-115">Adatok másolása a következő adatokat tárolja hello **tooAzure Blob Storage**:</span><span class="sxs-lookup"><span data-stu-id="c704c-115">You can copy data from hello following data stores **tooAzure Blob Storage**:</span></span>

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]
 
> [!IMPORTANT]
> <span data-ttu-id="c704c-116">Másolási tevékenység támogatja az adatok másolását / tooboth általános célú Azure Storage accounts és közbeni/Cool Blob storage.</span><span class="sxs-lookup"><span data-stu-id="c704c-116">Copy Activity supports copying data from/tooboth general-purpose Azure Storage accounts and Hot/Cool Blob storage.</span></span> <span data-ttu-id="c704c-117">hello tevékenység támogatja **blokk, hozzáfűzése, vagy olvasása lapblobokat**, azonban a **tooonly blokkblobokat írása**.</span><span class="sxs-lookup"><span data-stu-id="c704c-117">hello activity supports **reading from block, append, or page blobs**, but supports **writing tooonly block blobs**.</span></span> <span data-ttu-id="c704c-118">Prémium szintű Storage nem támogatott, a fogadó, mert a lapblobokat ezt támogatja.</span><span class="sxs-lookup"><span data-stu-id="c704c-118">Azure Premium Storage is not supported as a sink because it is backed by page blobs.</span></span>
> 
> <span data-ttu-id="c704c-119">Másolási tevékenység adatot nem töröl hello forrásból származó adatok sikeresen van hello toohello cél másolását követően.</span><span class="sxs-lookup"><span data-stu-id="c704c-119">Copy Activity does not delete data from hello source after hello data is successfully copied toohello destination.</span></span> <span data-ttu-id="c704c-120">Ha sikeres másolatot után kell toodelete forrásadatok, hozzon létre egy [egyéni tevékenység](data-factory-use-custom-activities.md) toodelete adatok hello és hello tevékenység hello folyamat használja.</span><span class="sxs-lookup"><span data-stu-id="c704c-120">If you need toodelete source data after a successful copy, create a [custom activity](data-factory-use-custom-activities.md) toodelete hello data and use hello activity in hello pipeline.</span></span> <span data-ttu-id="c704c-121">Egy vonatkozó példáért lásd: hello [Delete blob vagy mappa mintát a Githubon](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).</span><span class="sxs-lookup"><span data-stu-id="c704c-121">For an example, see hello [Delete blob or folder sample on GitHub](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/DeleteBlobFileFolderCustomActivity).</span></span> 

## <a name="get-started"></a><span data-ttu-id="c704c-122">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="c704c-122">Get started</span></span>
<span data-ttu-id="c704c-123">A másolási tevékenység, amely helyezi át az adatokat az Azure Blob Storage vagy a különböző eszközök/API-k használatával létrehozhat egy folyamatot.</span><span class="sxs-lookup"><span data-stu-id="c704c-123">You can create a pipeline with a copy activity that moves data to/from an Azure Blob Storage by using different tools/APIs.</span></span>

<span data-ttu-id="c704c-124">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="c704c-124">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="c704c-125">Ennek a cikknek a [forgatókönyv](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) létrehozásának adatcsatorna toocopy az az Azure Blob-tároló helye tooanother Azure Blob-tároló helye.</span><span class="sxs-lookup"><span data-stu-id="c704c-125">This article has a [walkthrough](#walkthrough-use-copy-wizard-to-copy-data-tofrom-blob-storage) for creating a pipeline toocopy data from an Azure Blob Storage location  tooanother Azure Blob Storage location.</span></span> <span data-ttu-id="c704c-126">Egy folyamat toocopy adatok létrehozása az Azure Blob Storage tooAzure SQL Database oktatóanyag, lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="c704c-126">For a tutorial on creating a pipeline toocopy data from an Azure Blob Storage tooAzure SQL Database, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="c704c-127">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="c704c-127">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="c704c-128">Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.</span><span class="sxs-lookup"><span data-stu-id="c704c-128">See [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) for step-by-step instructions toocreate a pipeline with a copy activity.</span></span>

<span data-ttu-id="c704c-129">Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="c704c-129">Whether you use hello tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="c704c-130">Hozzon létre egy **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="c704c-130">Create a **data factory**.</span></span> <span data-ttu-id="c704c-131">Egy adat-előállító tartalmazhat egy vagy több folyamatok.</span><span class="sxs-lookup"><span data-stu-id="c704c-131">A data factory may contain one or more pipelines.</span></span> 
2. <span data-ttu-id="c704c-132">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="c704c-132">Create **linked services** toolink input and output data stores tooyour data factory.</span></span> <span data-ttu-id="c704c-133">Adatok másolása az Azure blob storage tooan Azure SQL-adatbázis, akkor hozzon létre például két összekapcsolt szolgáltatások toolink az Azure storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="c704c-133">For example, if you are copying data from an Azure blob storage tooan Azure SQL database, you create two linked services toolink your Azure storage account and Azure SQL database tooyour data factory.</span></span> <span data-ttu-id="c704c-134">Csatolt szolgáltatás tulajdonságait, amelyek adott tooAzure Blob-tároló, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c704c-134">For linked service properties that are specific tooAzure Blob Storage, see [linked service properties](#linked-service-properties) section.</span></span> 
2. <span data-ttu-id="c704c-135">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="c704c-135">Create **datasets** toorepresent input and output data for hello copy operation.</span></span> <span data-ttu-id="c704c-136">Hello utolsó lépésében említett hello például létrehoz egy adatkészlet toospecify hello blobtárolót és hello bemeneti adatokat tartalmazó mappát.</span><span class="sxs-lookup"><span data-stu-id="c704c-136">In hello example mentioned in hello last step, you create a dataset toospecify hello blob container and folder that contains hello input data.</span></span> <span data-ttu-id="c704c-137">És egy másik dataset toospecify hello SQL táblázat hello blob-tároló átmásolva hello adatokat tartalmazó hello Azure SQL-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c704c-137">And, you create another dataset toospecify hello SQL table in hello Azure SQL database that holds hello data copied from hello blob storage.</span></span> <span data-ttu-id="c704c-138">Tekintse meg, amelyek adott tooAzure Blob-tároló adatkészlet tulajdonságai, [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c704c-138">For dataset properties that are specific tooAzure Blob Storage, see [dataset properties](#dataset-properties) section.</span></span>
3. <span data-ttu-id="c704c-139">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="c704c-139">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span> <span data-ttu-id="c704c-140">A korábban említett hello példában BlobSource forrás-és SqlSink akár használhatja a fogadó hello másolási tevékenységhez.</span><span class="sxs-lookup"><span data-stu-id="c704c-140">In hello example mentioned earlier, you use BlobSource as a source and SqlSink as a sink for hello copy activity.</span></span> <span data-ttu-id="c704c-141">Hasonlóképpen a Blob Storage Azure SQL Database tooAzure másolása, használható SqlSource és BlobSink hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c704c-141">Similarly, if you are copying from Azure SQL Database tooAzure Blob Storage, you use SqlSource and BlobSink in hello copy activity.</span></span> <span data-ttu-id="c704c-142">A másolási tevékenység tulajdonságait, amelyek adott tooAzure Blob-tároló, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c704c-142">For copy activity properties that are specific tooAzure Blob Storage, see [copy activity properties](#copy-activity-properties) section.</span></span> <span data-ttu-id="c704c-143">További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="c704c-143">For details on how toouse a data store as a source or a sink, click hello link in hello previous section for your data store.</span></span>  

<span data-ttu-id="c704c-144">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="c704c-144">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="c704c-145">Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="c704c-145">When you use tools/APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span>  <span data-ttu-id="c704c-146">Az adat-előállító entitások, amelyek az Azure Blob-tároló felhasznált toocopy adatok JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-blob-storage  ) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="c704c-146">For samples with JSON definitions for Data Factory entities that are used toocopy data to/from an Azure Blob Storage, see [JSON examples](#json-examples-for-copying-data-to-and-from-blob-storage  ) section of this article.</span></span>

<span data-ttu-id="c704c-147">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAzure Blob Storage részleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="c704c-147">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAzure Blob Storage.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="c704c-148">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="c704c-148">Linked service properties</span></span>
<span data-ttu-id="c704c-149">Két különböző összekapcsolt szolgáltatások toolink egy Azure Storage tooan az Azure data factory használatával.</span><span class="sxs-lookup"><span data-stu-id="c704c-149">There are two types of linked services you can use toolink an Azure Storage tooan Azure data factory.</span></span> <span data-ttu-id="c704c-150">Ezek: **AzureStorage** társított szolgáltatás és **AzureStorageSas** társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c704c-150">They are: **AzureStorage** linked service and **AzureStorageSas** linked service.</span></span> <span data-ttu-id="c704c-151">hello Azure tárolás társított szolgáltatásának biztosít hello data Factory összetevőnek a globális hozzáférési toohello Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c704c-151">hello Azure Storage linked service provides hello data factory with global access toohello Azure Storage.</span></span> <span data-ttu-id="c704c-152">Mivel hello Azure Storage SAS (közös hozzáférésű Jogosultságkód) kapcsolódó szolgáltatás biztosítja azt az Azure Storage korlátozott/időhöz kötött hozzáférés toohello hello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="c704c-152">Whereas, hello Azure Storage SAS (Shared Access Signature) linked service provides hello data factory with restricted/time-bound access toohello Azure Storage.</span></span> <span data-ttu-id="c704c-153">Nincsenek más különbségek a következő két összekapcsolt szolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="c704c-153">There are no other differences between these two linked services.</span></span> <span data-ttu-id="c704c-154">Válassza ki az igényeinek megfelelő kapcsolódó hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c704c-154">Choose hello linked service that suits your needs.</span></span> <span data-ttu-id="c704c-155">hello következő szakaszokban további részleteket a következő két összekapcsolt szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="c704c-155">hello following sections provide more details on these two linked services.</span></span>

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a><span data-ttu-id="c704c-156">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="c704c-156">Dataset properties</span></span>
<span data-ttu-id="c704c-157">a dataset toorepresent toospecify hello adatkészlet hello type tulajdonsága bemeneti vagy kimeneti adatokat az Azure Blob Storage tárolóban, beállítása: **AzureBlob**.</span><span class="sxs-lookup"><span data-stu-id="c704c-157">toospecify a dataset toorepresent input or output data in an Azure Blob Storage, you set hello type property of hello dataset to: **AzureBlob**.</span></span> <span data-ttu-id="c704c-158">Set hello **linkedServiceName** hello dataset toohello nevének hello Azure Storage vagy az Azure Storage SAS tulajdonság társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c704c-158">Set hello **linkedServiceName** property of hello dataset toohello name of hello Azure Storage or Azure Storage SAS linked service.</span></span>  <span data-ttu-id="c704c-159">hello dataset hello típus tulajdonságainak megadása hello **blob tároló** és hello **mappa** hello blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c704c-159">hello type properties of hello dataset specify hello **blob container** and hello **folder** in hello blob storage.</span></span>

<span data-ttu-id="c704c-160">JSON-szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c704c-160">For a full list of JSON sections & properties available for defining datasets, see hello [Creating datasets](data-factory-create-datasets.md) article.</span></span> <span data-ttu-id="c704c-161">Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).</span><span class="sxs-lookup"><span data-stu-id="c704c-161">Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).</span></span>

<span data-ttu-id="c704c-162">Adat-előállító támogatja alapú CLS-kompatibilis .NET típusú értékek biztosítani, például az Azure blob séma olvasható az adatforrásokhoz tartozó "structure" írja be az adatokat a következő hello: Int16, Int32, Int64, egyetlen, Double, Decimal, Byte [], Bool, String, Guid, Datetime, Datetimeoffset, Timespan.</span><span class="sxs-lookup"><span data-stu-id="c704c-162">Data factory supports hello following CLS-compliant .NET based type values for providing type information in “structure” for schema-on-read data sources like Azure blob: Int16, Int32, Int64, Single, Double, Decimal, Byte[], Bool, String, Guid, Datetime, Datetimeoffset, Timespan.</span></span> <span data-ttu-id="c704c-163">Adat-előállító automatikusan típuskonverziók hajt végre, a forrásadatok az adattároló tooa fogadó adattár áthelyezésekor.</span><span class="sxs-lookup"><span data-stu-id="c704c-163">Data Factory automatically performs type conversions when moving data from a source data store tooa sink data store.</span></span>

<span data-ttu-id="c704c-164">Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú, és információkat hello hely formátumra stb, hello adatok hello-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c704c-164">hello **typeProperties** section is different for each type of dataset and provides information about hello location, format etc., of hello data in hello data store.</span></span> <span data-ttu-id="c704c-165">hello typeProperties szakasz típusú adatkészlet **AzureBlob** dataset adatkészletben hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="c704c-165">hello typeProperties section for dataset of type **AzureBlob** dataset has hello following properties:</span></span>

| <span data-ttu-id="c704c-166">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c704c-166">Property</span></span> | <span data-ttu-id="c704c-167">Leírás</span><span class="sxs-lookup"><span data-stu-id="c704c-167">Description</span></span> | <span data-ttu-id="c704c-168">Szükséges</span><span class="sxs-lookup"><span data-stu-id="c704c-168">Required</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c704c-169">folderPath</span><span class="sxs-lookup"><span data-stu-id="c704c-169">folderPath</span></span> |<span data-ttu-id="c704c-170">Elérési út toohello tároló és mappa hello blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c704c-170">Path toohello container and folder in hello blob storage.</span></span> <span data-ttu-id="c704c-171">Példa: myblobcontainer\myblobfolder\\</span><span class="sxs-lookup"><span data-stu-id="c704c-171">Example: myblobcontainer\myblobfolder\\</span></span> |<span data-ttu-id="c704c-172">Igen</span><span class="sxs-lookup"><span data-stu-id="c704c-172">Yes</span></span> |
| <span data-ttu-id="c704c-173">fileName</span><span class="sxs-lookup"><span data-stu-id="c704c-173">fileName</span></span> |<span data-ttu-id="c704c-174">Hello blob neve.</span><span class="sxs-lookup"><span data-stu-id="c704c-174">Name of hello blob.</span></span> <span data-ttu-id="c704c-175">Fájlnév nem, kötelező, és a kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="c704c-175">fileName is optional and case-sensitive.</span></span><br/><br/><span data-ttu-id="c704c-176">Ha meg kell adnia egy fájlnevet, hello (például a Másolás) tevékenység works hello adott Blob.</span><span class="sxs-lookup"><span data-stu-id="c704c-176">If you specify a filename, hello activity (including Copy) works on hello specific Blob.</span></span><br/><br/><span data-ttu-id="c704c-177">Ha nincs megadva fájlnév, másolása összes BLOB bemeneti adatkészlet hello folderPath tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="c704c-177">When fileName is not specified, Copy includes all Blobs in hello folderPath for input dataset.</span></span><br/><br/><span data-ttu-id="c704c-178">Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet és **preserveHierarchy** nincs megadva a tevékenység fogadó hello létrehozott fájl hello név lesz a következő formátumban hello: adatok.<Guid>. txt (például:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span><span class="sxs-lookup"><span data-stu-id="c704c-178">When **fileName** is not specified for an output dataset and **preserveHierarchy** is not specified in activity sink, hello name of hello generated file would be in hello following this format: Data.<Guid>.txt (for example: : Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt</span></span> |<span data-ttu-id="c704c-179">Nem</span><span class="sxs-lookup"><span data-stu-id="c704c-179">No</span></span> |
| <span data-ttu-id="c704c-180">partitionedBy</span><span class="sxs-lookup"><span data-stu-id="c704c-180">partitionedBy</span></span> |<span data-ttu-id="c704c-181">partitionedBy egy nem kötelező tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c704c-181">partitionedBy is an optional property.</span></span> <span data-ttu-id="c704c-182">Használhatja a dinamikus folderPath toospecify és a fájlnév idő adatsorozat adatok.</span><span class="sxs-lookup"><span data-stu-id="c704c-182">You can use it toospecify a dynamic folderPath and filename for time series data.</span></span> <span data-ttu-id="c704c-183">Például folderPath is paraméteres adatok óránkénti.</span><span class="sxs-lookup"><span data-stu-id="c704c-183">For example, folderPath can be parameterized for every hour of data.</span></span> <span data-ttu-id="c704c-184">Lásd: hello [partitionedBy tulajdonság szakaszában](#using-partitionedBy-property) részletek és a példákat.</span><span class="sxs-lookup"><span data-stu-id="c704c-184">See hello [Using partitionedBy property section](#using-partitionedBy-property) for details and examples.</span></span> |<span data-ttu-id="c704c-185">Nem</span><span class="sxs-lookup"><span data-stu-id="c704c-185">No</span></span> |
| <span data-ttu-id="c704c-186">Formátumban</span><span class="sxs-lookup"><span data-stu-id="c704c-186">format</span></span> | <span data-ttu-id="c704c-187">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="c704c-187">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="c704c-188">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c704c-188">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="c704c-189">További információkért lásd: [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [Json formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátum](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="c704c-189">For more information, see [Text Format](data-factory-supported-file-and-compression-formats.md#text-format), [Json Format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro Format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc Format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet Format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="c704c-190">Ha azt szeretné, túl**másolja a fájlokat-van** közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet-definíciókban.</span><span class="sxs-lookup"><span data-stu-id="c704c-190">If you want too**copy files as-is** between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="c704c-191">Nem</span><span class="sxs-lookup"><span data-stu-id="c704c-191">No</span></span> |
| <span data-ttu-id="c704c-192">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="c704c-192">compression</span></span> | <span data-ttu-id="c704c-193">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="c704c-193">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="c704c-194">Támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="c704c-194">Supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="c704c-195">Támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="c704c-195">Supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="c704c-196">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="c704c-196">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="c704c-197">Nem</span><span class="sxs-lookup"><span data-stu-id="c704c-197">No</span></span> |

### <a name="using-partitionedby-property"></a><span data-ttu-id="c704c-198">PartitionedBy tulajdonság használatával</span><span class="sxs-lookup"><span data-stu-id="c704c-198">Using partitionedBy property</span></span>
<span data-ttu-id="c704c-199">Hello előző szakaszban említett, megadhatja a dinamikus folderPath és a sorozat időadatok fájlnevét hello **partitionedBy** tulajdonság, [adat-előállító funkciók és hello rendszerváltozók](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="c704c-199">As mentioned in hello previous section, you can specify a dynamic folderPath and filename for time series data with hello **partitionedBy** property, [Data Factory functions, and hello system variables](data-factory-functions-variables.md).</span></span>

<span data-ttu-id="c704c-200">Idő adatsorozat adatkészleteket, az ütemezés és a szeletek további információkért lásd: [létrehozása adatkészletek](data-factory-create-datasets.md) és [ütemezés & végrehajtási](data-factory-scheduling-and-execution.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="c704c-200">For more information on time series datasets, scheduling, and slices, see [Creating Datasets](data-factory-create-datasets.md) and [Scheduling & Execution](data-factory-scheduling-and-execution.md) articles.</span></span>

#### <a name="sample-1"></a><span data-ttu-id="c704c-201">1. példa</span><span class="sxs-lookup"><span data-stu-id="c704c-201">Sample 1</span></span>

```json
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

<span data-ttu-id="c704c-202">Ebben a példában {szelet} hello adat-előállító rendszer változó SliceStart hello formátumban (YYYYMMDDHH) megadott érték helyére.</span><span class="sxs-lookup"><span data-stu-id="c704c-202">In this example, {Slice} is replaced with hello value of Data Factory system variable SliceStart in hello format (YYYYMMDDHH) specified.</span></span> <span data-ttu-id="c704c-203">hello SliceStart toostart idő hello szelet hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="c704c-203">hello SliceStart refers toostart time of hello slice.</span></span> <span data-ttu-id="c704c-204">hello folderPath nem azonos az egyes szeletek.</span><span class="sxs-lookup"><span data-stu-id="c704c-204">hello folderPath is different for each slice.</span></span> <span data-ttu-id="c704c-205">Például: wikidatagateway/wikisampledataout/2014100103 vagy wikidatagateway/wikisampledataout/2014100104</span><span class="sxs-lookup"><span data-stu-id="c704c-205">For example: wikidatagateway/wikisampledataout/2014100103 or wikidatagateway/wikisampledataout/2014100104</span></span>

#### <a name="sample-2"></a><span data-ttu-id="c704c-206">2. példa</span><span class="sxs-lookup"><span data-stu-id="c704c-206">Sample 2</span></span>

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

<span data-ttu-id="c704c-207">Ebben a példában év, hónap, nap és SliceStart idején ki kell olvasni a külön változókat, amelyek folderPath és a fájlnév tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c704c-207">In this example, year, month, day, and time of SliceStart are extracted into separate variables that are used by folderPath and fileName properties.</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="c704c-208">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="c704c-208">Copy activity properties</span></span>
<span data-ttu-id="c704c-209">Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c704c-209">For a full list of sections & properties available for defining activities, see hello [Creating Pipelines](data-factory-create-pipelines.md) article.</span></span> <span data-ttu-id="c704c-210">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti adatkészletek és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c704c-210">Properties such as name, description, input and output datasets, and policies are available for all types of activities.</span></span> <span data-ttu-id="c704c-211">Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="c704c-211">Whereas, properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="c704c-212">A másolási tevékenység során két érték források és mosdók hello típusától függően.</span><span class="sxs-lookup"><span data-stu-id="c704c-212">For Copy activity, they vary depending on hello types of sources and sinks.</span></span> <span data-ttu-id="c704c-213">Egy Azure Blob Storage-ból adatokat helyez át, ha beállítása hello forrás típusa másolási tevékenység hello túl**BlobSource**.</span><span class="sxs-lookup"><span data-stu-id="c704c-213">If you are moving data from an Azure Blob Storage, you set hello source type in hello copy activity too**BlobSource**.</span></span> <span data-ttu-id="c704c-214">Ehhez hasonlóan adatok tooan Azure Blob Storage helyez át, ha beállítása hello a fogadó típusa másolási tevékenység hello túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="c704c-214">Similarly, if you are moving data tooan Azure Blob Storage, you set hello sink type in hello copy activity too**BlobSink**.</span></span> <span data-ttu-id="c704c-215">Ez a témakör BlobSource és BlobSink által támogatott tulajdonságokról.</span><span class="sxs-lookup"><span data-stu-id="c704c-215">This section provides a list of properties supported by BlobSource and BlobSink.</span></span>

<span data-ttu-id="c704c-216">**BlobSource** támogatja a következő tulajdonságai a hello hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="c704c-216">**BlobSource** supports hello following properties in hello **typeProperties** section:</span></span>

| <span data-ttu-id="c704c-217">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c704c-217">Property</span></span> | <span data-ttu-id="c704c-218">Leírás</span><span class="sxs-lookup"><span data-stu-id="c704c-218">Description</span></span> | <span data-ttu-id="c704c-219">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="c704c-219">Allowed values</span></span> | <span data-ttu-id="c704c-220">Szükséges</span><span class="sxs-lookup"><span data-stu-id="c704c-220">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c704c-221">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="c704c-221">recursive</span></span> |<span data-ttu-id="c704c-222">Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello.</span><span class="sxs-lookup"><span data-stu-id="c704c-222">Indicates whether hello data is read recursively from hello sub folders or only from hello specified folder.</span></span> |<span data-ttu-id="c704c-223">TRUE hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="c704c-223">True (default value), False</span></span> |<span data-ttu-id="c704c-224">Nem</span><span class="sxs-lookup"><span data-stu-id="c704c-224">No</span></span> |

<span data-ttu-id="c704c-225">**BlobSink** támogatja a következő tulajdonságai hello **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="c704c-225">**BlobSink** supports hello following properties **typeProperties** section:</span></span>

| <span data-ttu-id="c704c-226">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c704c-226">Property</span></span> | <span data-ttu-id="c704c-227">Leírás</span><span class="sxs-lookup"><span data-stu-id="c704c-227">Description</span></span> | <span data-ttu-id="c704c-228">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="c704c-228">Allowed values</span></span> | <span data-ttu-id="c704c-229">Szükséges</span><span class="sxs-lookup"><span data-stu-id="c704c-229">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="c704c-230">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="c704c-230">copyBehavior</span></span> |<span data-ttu-id="c704c-231">Hello másolási viselkedését határozza meg, ha hello adatforrás BlobSource vagy a fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="c704c-231">Defines hello copy behavior when hello source is BlobSource or FileSystem.</span></span> |<span data-ttu-id="c704c-232"><b>PreserveHierarchy</b>: megtartja hello fájl hierarchia hello célmappában.</span><span class="sxs-lookup"><span data-stu-id="c704c-232"><b>PreserveHierarchy</b>: preserves hello file hierarchy in hello target folder.</span></span> <span data-ttu-id="c704c-233">hello relatív fájl toosource forrásmappa elérési út azonos toohello fájl tootarget célmappa relatív elérési útját.</span><span class="sxs-lookup"><span data-stu-id="c704c-233">hello relative path of source file toosource folder is identical toohello relative path of target file tootarget folder.</span></span><br/><br/><span data-ttu-id="c704c-234"><b>FlattenHierarchy</b>: összes fájl hello forrásmappából a hello első szintű tároló mappa.</span><span class="sxs-lookup"><span data-stu-id="c704c-234"><b>FlattenHierarchy</b>: all files from hello source folder are in hello first level of target folder.</span></span> <span data-ttu-id="c704c-235">hello fájljaira automatikusan létrehozott nevet adni.</span><span class="sxs-lookup"><span data-stu-id="c704c-235">hello target files have auto generated name.</span></span> <br/><br/><span data-ttu-id="c704c-236"><b>Mergefiles típusú</b>: összes fájl forrásfájlból hello mappa tooone egyesíti.</span><span class="sxs-lookup"><span data-stu-id="c704c-236"><b>MergeFiles</b>: merges all files from hello source folder tooone file.</span></span> <span data-ttu-id="c704c-237">Ha hello fájl/Blob neve meg van adva, egyesített hello neve legyen hello megadott név; Ellenkező esetben lenne automatikusan létrehozott fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="c704c-237">If hello File/Blob Name is specified, hello merged file name would be hello specified name; otherwise, would be auto-generated file name.</span></span> |<span data-ttu-id="c704c-238">Nem</span><span class="sxs-lookup"><span data-stu-id="c704c-238">No</span></span> |

<span data-ttu-id="c704c-239">**BlobSource** is támogatja a visszamenőleges kompatibilitás érdekében a két tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c704c-239">**BlobSource** also supports these two properties for backward compatibility.</span></span>

* <span data-ttu-id="c704c-240">**treatEmptyAsNull**: Megadja, hogy tootreat null vagy üres karakterlánc null értékként.</span><span class="sxs-lookup"><span data-stu-id="c704c-240">**treatEmptyAsNull**: Specifies whether tootreat null or empty string as null value.</span></span>
* <span data-ttu-id="c704c-241">**skipHeaderLineCount** -határozza meg, hogy hány sort kell figyelmen kívül hagyja.</span><span class="sxs-lookup"><span data-stu-id="c704c-241">**skipHeaderLineCount** - Specifies how many lines need be skipped.</span></span> <span data-ttu-id="c704c-242">Azt is alkalmazható csak amikor bemeneti adatkészlet által használt szöveges.</span><span class="sxs-lookup"><span data-stu-id="c704c-242">It is applicable only when input dataset is using TextFormat.</span></span>

<span data-ttu-id="c704c-243">Hasonlóképpen **BlobSink** tulajdonság a visszamenőleges kompatibilitás érdekében a következő hello támogatja.</span><span class="sxs-lookup"><span data-stu-id="c704c-243">Similarly, **BlobSink** supports hello following property for backward compatibility.</span></span>

* <span data-ttu-id="c704c-244">**blobWriterAddHeader**: meghatározza, hogy tooadd tooan írásakor oszlopdefiníciók fejléc kimeneti adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="c704c-244">**blobWriterAddHeader**: Specifies whether tooadd a header of column definitions while writing tooan output dataset.</span></span>

<span data-ttu-id="c704c-245">Következő tulajdonságai, amelyek megvalósítják az adatkészletek most támogatási hello hello ugyanezt a funkcionalitást: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.</span><span class="sxs-lookup"><span data-stu-id="c704c-245">Datasets now support hello following properties that implement hello same functionality: **treatEmptyAsNull**, **skipLineCount**, **firstRowAsHeader**.</span></span>

<span data-ttu-id="c704c-246">hello következő táblázat nyújt útmutatást hello új adatkészlet tulajdonságai helyett a blob-forrás/fogadó tulajdonságok használatával.</span><span class="sxs-lookup"><span data-stu-id="c704c-246">hello following table provides guidance on using hello new dataset properties in place of these blob source/sink properties.</span></span>

| <span data-ttu-id="c704c-247">Másolja az Activity tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c704c-247">Copy Activity property</span></span> | <span data-ttu-id="c704c-248">A DataSet tulajdonság</span><span class="sxs-lookup"><span data-stu-id="c704c-248">Dataset property</span></span> |
|:--- |:--- |
| <span data-ttu-id="c704c-249">a BlobSource skipHeaderLineCount</span><span class="sxs-lookup"><span data-stu-id="c704c-249">skipHeaderLineCount on BlobSource</span></span> |<span data-ttu-id="c704c-250">skipLineCount és firstRowAsHeader.</span><span class="sxs-lookup"><span data-stu-id="c704c-250">skipLineCount and firstRowAsHeader.</span></span> <span data-ttu-id="c704c-251">Sorok először kimarad, és majd hello első sorának fejlécként olvasható.</span><span class="sxs-lookup"><span data-stu-id="c704c-251">Lines are skipped first and then hello first row is read as a header.</span></span> |
| <span data-ttu-id="c704c-252">a BlobSource treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="c704c-252">treatEmptyAsNull on BlobSource</span></span> |<span data-ttu-id="c704c-253">a bemeneti adatkészlet treatEmptyAsNull</span><span class="sxs-lookup"><span data-stu-id="c704c-253">treatEmptyAsNull on input dataset</span></span> |
| <span data-ttu-id="c704c-254">a BlobSink blobWriterAddHeader</span><span class="sxs-lookup"><span data-stu-id="c704c-254">blobWriterAddHeader on BlobSink</span></span> |<span data-ttu-id="c704c-255">a kimeneti adatkészlet firstRowAsHeader</span><span class="sxs-lookup"><span data-stu-id="c704c-255">firstRowAsHeader on output dataset</span></span> |

<span data-ttu-id="c704c-256">Lásd: [megadása szöveges](data-factory-supported-file-and-compression-formats.md#text-format) szakasz ezeket a tulajdonságokat részletes tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="c704c-256">See [Specifying TextFormat](data-factory-supported-file-and-compression-formats.md#text-format) section for detailed information on these properties.</span></span>    

### <a name="recursive-and-copybehavior-examples"></a><span data-ttu-id="c704c-257">rekurzív és copyBehavior példák</span><span class="sxs-lookup"><span data-stu-id="c704c-257">recursive and copyBehavior examples</span></span>
<span data-ttu-id="c704c-258">Ez a szakasz ismerteti a hello viselkedésről hello másolási műveletek kombinációk rekurzív és copybehavior tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="c704c-258">This section describes hello resulting behavior of hello Copy operation for different combinations of recursive and copyBehavior values.</span></span>

| <span data-ttu-id="c704c-259">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="c704c-259">recursive</span></span> | <span data-ttu-id="c704c-260">copyBehavior</span><span class="sxs-lookup"><span data-stu-id="c704c-260">copyBehavior</span></span> | <span data-ttu-id="c704c-261">Viselkedésről</span><span class="sxs-lookup"><span data-stu-id="c704c-261">Resulting behavior</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c704c-262">Igaz</span><span class="sxs-lookup"><span data-stu-id="c704c-262">true</span></span> |<span data-ttu-id="c704c-263">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="c704c-263">preserveHierarchy</span></span> |<span data-ttu-id="c704c-264">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c704c-264">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="c704c-265">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-265">Folder1</span></span><br/><span data-ttu-id="c704c-266">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-266">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c704c-267">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c704c-267">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c704c-268">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c704c-268">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c704c-269">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="c704c-269">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c704c-270">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c704c-270">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c704c-271">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c704c-271">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c704c-272">hello tároló mappa mappa1 azonos szerkezeti hello forrásaként hello jön létre</span><span class="sxs-lookup"><span data-stu-id="c704c-272">hello target folder Folder1 is created with hello same structure as hello source</span></span><br/><br/><span data-ttu-id="c704c-273">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-273">Folder1</span></span><br/><span data-ttu-id="c704c-274">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-274">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c704c-275">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c704c-275">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c704c-276">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c704c-276">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c704c-277">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="c704c-277">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c704c-278">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c704c-278">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c704c-279">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span><span class="sxs-lookup"><span data-stu-id="c704c-279">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5.</span></span> |
| <span data-ttu-id="c704c-280">Igaz</span><span class="sxs-lookup"><span data-stu-id="c704c-280">true</span></span> |<span data-ttu-id="c704c-281">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="c704c-281">flattenHierarchy</span></span> |<span data-ttu-id="c704c-282">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c704c-282">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="c704c-283">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-283">Folder1</span></span><br/><span data-ttu-id="c704c-284">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-284">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c704c-285">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c704c-285">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c704c-286">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c704c-286">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c704c-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="c704c-287">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c704c-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c704c-288">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c704c-289">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c704c-289">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c704c-290">a következő struktúra hello hello cél mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="c704c-290">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="c704c-291">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-291">Folder1</span></span><br/><span data-ttu-id="c704c-292">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-292">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="c704c-293">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="c704c-293">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><span data-ttu-id="c704c-294">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet fájl3</span><span class="sxs-lookup"><span data-stu-id="c704c-294">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File3</span></span><br/><span data-ttu-id="c704c-295">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File4</span><span class="sxs-lookup"><span data-stu-id="c704c-295">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File4</span></span><br/><span data-ttu-id="c704c-296">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File5</span><span class="sxs-lookup"><span data-stu-id="c704c-296">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File5</span></span> |
| <span data-ttu-id="c704c-297">Igaz</span><span class="sxs-lookup"><span data-stu-id="c704c-297">true</span></span> |<span data-ttu-id="c704c-298">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="c704c-298">mergeFiles</span></span> |<span data-ttu-id="c704c-299">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c704c-299">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="c704c-300">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-300">Folder1</span></span><br/><span data-ttu-id="c704c-301">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-301">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c704c-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c704c-302">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c704c-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c704c-303">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c704c-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="c704c-304">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c704c-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c704c-305">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c704c-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c704c-306">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c704c-307">a következő struktúra hello hello cél mappa1 jön létre:</span><span class="sxs-lookup"><span data-stu-id="c704c-307">hello target Folder1 is created with hello following structure:</span></span> <br/><br/><span data-ttu-id="c704c-308">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-308">Folder1</span></span><br/><span data-ttu-id="c704c-309">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + fájl3 + File4 + 5 fájl tartalmát egy fájl automatikusan létrehozott fájlnévvel egyesülnek</span><span class="sxs-lookup"><span data-stu-id="c704c-309">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + File3 + File4 + File 5 contents are merged into one file with auto-generated file name</span></span> |
| <span data-ttu-id="c704c-310">hamis</span><span class="sxs-lookup"><span data-stu-id="c704c-310">false</span></span> |<span data-ttu-id="c704c-311">preserveHierarchy</span><span class="sxs-lookup"><span data-stu-id="c704c-311">preserveHierarchy</span></span> |<span data-ttu-id="c704c-312">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c704c-312">For a source folder Folder1 with hello following structure:</span></span> <br/><br/><span data-ttu-id="c704c-313">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-313">Folder1</span></span><br/><span data-ttu-id="c704c-314">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-314">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c704c-315">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c704c-315">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c704c-316">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c704c-316">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c704c-317">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="c704c-317">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c704c-318">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c704c-318">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c704c-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c704c-319">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c704c-320">a következő struktúra hello hello tároló mappa mappa1 jön létre</span><span class="sxs-lookup"><span data-stu-id="c704c-320">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="c704c-321">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-321">Folder1</span></span><br/><span data-ttu-id="c704c-322">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-322">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c704c-323">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c704c-323">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><br/><br/><span data-ttu-id="c704c-324">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="c704c-324">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="c704c-325">hamis</span><span class="sxs-lookup"><span data-stu-id="c704c-325">false</span></span> |<span data-ttu-id="c704c-326">flattenHierarchy</span><span class="sxs-lookup"><span data-stu-id="c704c-326">flattenHierarchy</span></span> |<span data-ttu-id="c704c-327">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c704c-327">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="c704c-328">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-328">Folder1</span></span><br/><span data-ttu-id="c704c-329">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-329">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c704c-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c704c-330">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c704c-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c704c-331">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c704c-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="c704c-332">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c704c-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c704c-333">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c704c-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c704c-334">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c704c-335">a következő struktúra hello hello tároló mappa mappa1 jön létre</span><span class="sxs-lookup"><span data-stu-id="c704c-335">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="c704c-336">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-336">Folder1</span></span><br/><span data-ttu-id="c704c-337">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-337">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File1</span></span><br/><span data-ttu-id="c704c-338">&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2</span><span class="sxs-lookup"><span data-stu-id="c704c-338">&nbsp;&nbsp;&nbsp;&nbsp;auto-generated name for File2</span></span><br/><br/><br/><span data-ttu-id="c704c-339">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="c704c-339">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |
| <span data-ttu-id="c704c-340">hamis</span><span class="sxs-lookup"><span data-stu-id="c704c-340">false</span></span> |<span data-ttu-id="c704c-341">mergefiles típusú</span><span class="sxs-lookup"><span data-stu-id="c704c-341">mergeFiles</span></span> |<span data-ttu-id="c704c-342">A forrásmappa mappa1 a struktúra a következő hello:</span><span class="sxs-lookup"><span data-stu-id="c704c-342">For a source folder Folder1 with hello following structure:</span></span><br/><br/><span data-ttu-id="c704c-343">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-343">Folder1</span></span><br/><span data-ttu-id="c704c-344">&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-344">&nbsp;&nbsp;&nbsp;&nbsp;File1</span></span><br/><span data-ttu-id="c704c-345">&nbsp;&nbsp;&nbsp;&nbsp;File2</span><span class="sxs-lookup"><span data-stu-id="c704c-345">&nbsp;&nbsp;&nbsp;&nbsp;File2</span></span><br/><span data-ttu-id="c704c-346">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span><span class="sxs-lookup"><span data-stu-id="c704c-346">&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1</span></span><br/><span data-ttu-id="c704c-347">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3</span><span class="sxs-lookup"><span data-stu-id="c704c-347">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File3</span></span><br/><span data-ttu-id="c704c-348">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span><span class="sxs-lookup"><span data-stu-id="c704c-348">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4</span></span><br/><span data-ttu-id="c704c-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span><span class="sxs-lookup"><span data-stu-id="c704c-349">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5</span></span><br/><br/><span data-ttu-id="c704c-350">a következő struktúra hello hello tároló mappa mappa1 jön létre</span><span class="sxs-lookup"><span data-stu-id="c704c-350">hello target folder Folder1 is created with hello following structure</span></span><br/><br/><span data-ttu-id="c704c-351">Mappa1</span><span class="sxs-lookup"><span data-stu-id="c704c-351">Folder1</span></span><br/><span data-ttu-id="c704c-352">&nbsp;&nbsp;&nbsp;&nbsp;Egy fájl automatikusan létrehozott fájlnévvel egyesített file1 + File2 tartalma.</span><span class="sxs-lookup"><span data-stu-id="c704c-352">&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 contents are merged into one file with auto-generated file name.</span></span> <span data-ttu-id="c704c-353">automatikusan létrehozott nevet a file1 kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="c704c-353">auto-generated name for File1</span></span><br/><br/><span data-ttu-id="c704c-354">Fájl3, File4 és File5 Subfolder1 nem átveszik.</span><span class="sxs-lookup"><span data-stu-id="c704c-354">Subfolder1 with File3, File4, and File5 are not picked up.</span></span> |

## <a name="walkthrough-use-copy-wizard-toocopy-data-tofrom-blob-storage"></a><span data-ttu-id="c704c-355">Forgatókönyv: Másolása varázsló használata toocopy adatok Blob Storage onnan</span><span class="sxs-lookup"><span data-stu-id="c704c-355">Walkthrough: Use Copy Wizard toocopy data to/from Blob Storage</span></span>
<span data-ttu-id="c704c-356">Nézzük hogyan tooquickly adatok másolása az Azure és a blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="c704c-356">Let's look at how tooquickly copy data to/from an Azure blob storage.</span></span> <span data-ttu-id="c704c-357">Ebben a bemutatóban forrás- és célkiszolgálón adatokat tárolja, típus: Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c704c-357">In this walkthrough, both source and destination data stores of type: Azure Blob Storage.</span></span> <span data-ttu-id="c704c-358">hello ebben a bemutatóban csővezeték másol adatokat hello egy mappáját tooanother blob tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="c704c-358">hello pipeline in this walkthrough copies data from a folder tooanother folder in hello same blob container.</span></span> <span data-ttu-id="c704c-359">Ez a forgatókönyv nem szándékosan egyszerű tooshow beállítások vagy a Blob Storage használata a forrás vagy a fogadó tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="c704c-359">This walkthrough is intentionally simple tooshow you settings or properties when using Blob Storage as a source or sink.</span></span> 

### <a name="prerequisites"></a><span data-ttu-id="c704c-360">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c704c-360">Prerequisites</span></span>
1. <span data-ttu-id="c704c-361">Hozzon létre egy általános célú **Azure Storage-fiók** Ha Ön nem rendelkezik ilyennel.</span><span class="sxs-lookup"><span data-stu-id="c704c-361">Create a general-purpose **Azure Storage Account** if you don't have one already.</span></span> <span data-ttu-id="c704c-362">Hello a blob storage használata egyaránt **forrás** és **cél** adattároló ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="c704c-362">You use hello blob storage as both **source** and **destination** data store in this walkthrough.</span></span> <span data-ttu-id="c704c-363">Ha egy Azure storage-fiók nem rendelkezik, tekintse meg a hello [hozzon létre egy tárfiókot](../storage/common/storage-create-storage-account.md#create-a-storage-account) lépéseket toocreate egyet a cikkben találhat.</span><span class="sxs-lookup"><span data-stu-id="c704c-363">if you don't have an Azure storage account, see hello [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) article for steps toocreate one.</span></span>
2. <span data-ttu-id="c704c-364">Hozzon létre egy blob-tároló nevű **adfblobconnector** hello tárfiókban.</span><span class="sxs-lookup"><span data-stu-id="c704c-364">Create a blob container named **adfblobconnector** in hello storage account.</span></span> 
4. <span data-ttu-id="c704c-365">Hozza létre a **bemeneti** a hello **adfblobconnector** tároló.</span><span class="sxs-lookup"><span data-stu-id="c704c-365">Create a folder named **input** in hello **adfblobconnector** container.</span></span>
5. <span data-ttu-id="c704c-366">Hozzon létre egy fájlt **emp.txt** hello követően a tartalmat, és töltse fel toohello **bemeneti** mappa eszközökkel, mint [Azure Tártallózó](https://azurestorageexplorer.codeplex.com/)</span><span class="sxs-lookup"><span data-stu-id="c704c-366">Create a file named **emp.txt** with hello following content and upload it toohello **input** folder by using tools such as [Azure Storage Explorer](https://azurestorageexplorer.codeplex.com/)</span></span>
    ```json
    John, Doe
    Jane, Doe
    ```
### <a name="create-hello-data-factory"></a><span data-ttu-id="c704c-367">Hello adat-előállító létrehozása</span><span class="sxs-lookup"><span data-stu-id="c704c-367">Create hello data factory</span></span>
1. <span data-ttu-id="c704c-368">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c704c-368">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c704c-369">Kattintson a **+ új** hello bal felső sarokban, kattintson **Eszközintelligencia + analitika**, és kattintson a **adat-előállító**.</span><span class="sxs-lookup"><span data-stu-id="c704c-369">Click **+ NEW** from hello top-left corner, click **Intelligence + analytics**, and click **Data Factory**.</span></span>
3. <span data-ttu-id="c704c-370">A hello **új adat-előállító** panel:</span><span class="sxs-lookup"><span data-stu-id="c704c-370">In hello **New data factory** blade:</span></span>   
    1. <span data-ttu-id="c704c-371">Adja meg **ADFBlobConnectorDF** a hello **neve**.</span><span class="sxs-lookup"><span data-stu-id="c704c-371">Enter **ADFBlobConnectorDF** for hello **name**.</span></span> <span data-ttu-id="c704c-372">az Azure data factory hello hello nevének globálisan egyedi kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c704c-372">hello name of hello Azure data factory must be globally unique.</span></span> <span data-ttu-id="c704c-373">Ha hello hibaüzenetet kapja: `*Data factory name “ADFBlobConnectorDF” is not available`, hello adat-előállítóban (például yournameADFBlobConnectorDF) hello nevének módosítása, és próbálja meg újra létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c704c-373">If you receive hello error: `*Data factory name “ADFBlobConnectorDF” is not available`, change hello name of hello data factory (for example, yournameADFBlobConnectorDF) and try creating again.</span></span> <span data-ttu-id="c704c-374">A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.</span><span class="sxs-lookup"><span data-stu-id="c704c-374">See [Data Factory - Naming Rules](data-factory-naming-rules.md) topic for naming rules for Data Factory artifacts.</span></span>
    2. <span data-ttu-id="c704c-375">Jelölje ki az Azure-**előfizetést**.</span><span class="sxs-lookup"><span data-stu-id="c704c-375">Select your Azure **subscription**.</span></span>
    3. <span data-ttu-id="c704c-376">Az erőforráscsoport, válassza ki a **használata meglévő** tooselect egy meglévő erőforrás csoport (vagy) válassza **hozzon létre új** tooenter erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="c704c-376">For Resource Group, select **Use existing** tooselect an existing resource group (or) select **Create new** tooenter a name for a resource group.</span></span>
    4. <span data-ttu-id="c704c-377">Válassza ki a **hely** hello adat-előállító esetében.</span><span class="sxs-lookup"><span data-stu-id="c704c-377">Select a **location** for hello data factory.</span></span>
    5. <span data-ttu-id="c704c-378">Válassza ki **PIN-kód toodashboard** hello hello panel alsó részén jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c704c-378">Select **Pin toodashboard** check box at hello bottom of hello blade.</span></span>
    6. <span data-ttu-id="c704c-379">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c704c-379">Click **Create**.</span></span>
3. <span data-ttu-id="c704c-380">Hello létrehozásának befejezése után megjelenik a hello **adat-előállító** panelen látható hello kép a következő módon: ![Data factory kezdőlap](./media/data-factory-azure-blob-connector/data-factory-home-page.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-380">After hello creation is complete, you see hello **Data Factory** blade as shown in hello following image: ![Data factory home page](./media/data-factory-azure-blob-connector/data-factory-home-page.png)</span></span>

### <a name="copy-wizard"></a><span data-ttu-id="c704c-381">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="c704c-381">Copy Wizard</span></span>
1. <span data-ttu-id="c704c-382">A hello adat-előállító kezdőlapján kattintson a hello **[előzetes verzió] adatok másolása** toolaunch csempe **másolása varázsló** egy külön lapján.</span><span class="sxs-lookup"><span data-stu-id="c704c-382">On hello Data Factory home page, click hello **Copy data [PREVIEW]** tile toolaunch **Copy Data Wizard** in a separate tab.</span></span>    
    
    > [!NOTE]
    >    <span data-ttu-id="c704c-383">Ha azt látja, hogy hello webböngésző akadt-e a "Engedélyező...", tiltsa le vagy törölje a jelet **külső cookie-k blokkolását, és a helyadatok** beállítása (vagy) engedélyezve legyen, és hozzon létre egy kivételt **login.microsoftonline.com**, és ezután próbálja meg újból elindítani a hello varázsló.</span><span class="sxs-lookup"><span data-stu-id="c704c-383">If you see that hello web browser is stuck at "Authorizing...", disable/uncheck **Block third-party cookies and site data** setting (or) keep it enabled and create an exception for **login.microsoftonline.com** and then try launching hello wizard again.</span></span>
2. <span data-ttu-id="c704c-384">A hello **tulajdonságok** lap:</span><span class="sxs-lookup"><span data-stu-id="c704c-384">In hello **Properties** page:</span></span>
    1. <span data-ttu-id="c704c-385">Adja meg **CopyPipeline** a **feladatnév**.</span><span class="sxs-lookup"><span data-stu-id="c704c-385">Enter **CopyPipeline** for **Task name**.</span></span> <span data-ttu-id="c704c-386">hello feladatnév: hello hello kimenetátirányítási mechanizmusával a data factory neve.</span><span class="sxs-lookup"><span data-stu-id="c704c-386">hello task name is hello name of hello pipeline in your data factory.</span></span>
    2. <span data-ttu-id="c704c-387">Adjon meg egy **leírás** hello feladathoz (választható).</span><span class="sxs-lookup"><span data-stu-id="c704c-387">Enter a **description** for hello task (optional).</span></span>
    3. <span data-ttu-id="c704c-388">A **feladat ütemben történik vagy ütemezett feladat**, tartsa hello **futtatása rendszeres ütemezés szerint** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c704c-388">For **Task cadence or Task schedule**, keep hello **Run regularly on schedule** option.</span></span> <span data-ttu-id="c704c-389">Ha ezt a feladatot csak egyszer ahelyett, hogy futhat ismétlődően egy ütemezés szerint toorun, jelölje be **futtassa egyszer most**.</span><span class="sxs-lookup"><span data-stu-id="c704c-389">If you want toorun this task only once instead of run repeatedly on a schedule, select **Run once now**.</span></span> <span data-ttu-id="c704c-390">Választ ki, ha **futtassa egyszer most** beállítást, a [egyszeri folyamat](data-factory-create-pipelines.md#onetime-pipeline) jön létre.</span><span class="sxs-lookup"><span data-stu-id="c704c-390">If you select, **Run once now** option, a [one-time pipeline](data-factory-create-pipelines.md#onetime-pipeline) is created.</span></span> 
    4. <span data-ttu-id="c704c-391">Megtarthatja hello **ismétlődő mintát**.</span><span class="sxs-lookup"><span data-stu-id="c704c-391">Keep hello settings for **Recurring pattern**.</span></span> <span data-ttu-id="c704c-392">Ez a feladat futtatása naponta hello közötti kezdési és befejezési időpontokat, adja meg a következő lépésben hello.</span><span class="sxs-lookup"><span data-stu-id="c704c-392">This task runs daily between hello start and end times you specify in hello next step.</span></span>
    5. <span data-ttu-id="c704c-393">Változás hello **kezdő dátum/idő** túl**04/21/2017**.</span><span class="sxs-lookup"><span data-stu-id="c704c-393">Change hello **Start date time** too**04/21/2017**.</span></span> 
    6. <span data-ttu-id="c704c-394">Változás hello **záró dátum és idő** túl**04/25/2017**.</span><span class="sxs-lookup"><span data-stu-id="c704c-394">Change hello **End date time** too**04/25/2017**.</span></span> <span data-ttu-id="c704c-395">Érdemes lehet tootype hello dátum keresse meg azt hello naptár helyett.</span><span class="sxs-lookup"><span data-stu-id="c704c-395">You may want tootype hello date instead of browsing through hello calendar.</span></span>     
    8. <span data-ttu-id="c704c-396">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c704c-396">Click **Next**.</span></span>
      <span data-ttu-id="c704c-397">![Másolja az eszköz - Tulajdonságok lap](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-397">![Copy Tool - Properties page](./media/data-factory-azure-blob-connector/copy-tool-properties-page.png)</span></span> 
3. <span data-ttu-id="c704c-398">A hello **forrás adattár** kattintson **Azure Blob Storage** csempére.</span><span class="sxs-lookup"><span data-stu-id="c704c-398">On hello **Source data store** page, click **Azure Blob Storage** tile.</span></span> <span data-ttu-id="c704c-399">Az oldal toospecify hello forrás adattároló hello másolási feladathoz kell használnia.</span><span class="sxs-lookup"><span data-stu-id="c704c-399">You use this page toospecify hello source data store for hello copy task.</span></span> <span data-ttu-id="c704c-400">Használhatja egy meglévő adattár társított szolgáltatását, vagy megadhat egy új adattárat.</span><span class="sxs-lookup"><span data-stu-id="c704c-400">You can use an existing data store linked service (or) specify a new data store.</span></span> <span data-ttu-id="c704c-401">egy meglévő toouse társított szolgáltatás, a kiválasztott **a meglévő összekapcsolt szolgáltatások** , és válassza ki a megfelelő társított szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="c704c-401">toouse an existing linked service, you would select **FROM EXISTING LINKED SERVICES** and select hello right linked service.</span></span> 
    <span data-ttu-id="c704c-402">![Másolja az eszköz - forrás egy adattárolási lap](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-402">![Copy Tool - Source data store page](./media/data-factory-azure-blob-connector/copy-tool-source-data-store-page.png)</span></span>
4. <span data-ttu-id="c704c-403">A hello **hello Azure Blob storage-fiók megadása** lap:</span><span class="sxs-lookup"><span data-stu-id="c704c-403">On hello **Specify hello Azure Blob storage account** page:</span></span>
   1. <span data-ttu-id="c704c-404">Tartsa hello automatikusan létrehozott nevet **kapcsolatnév**.</span><span class="sxs-lookup"><span data-stu-id="c704c-404">Keep hello auto-generated name for **Connection name**.</span></span> <span data-ttu-id="c704c-405">hello kapcsolat neve a következő típusú csatolt hello szolgáltatást hello neve: Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="c704c-405">hello connection name is hello name of hello linked service of type: Azure Storage.</span></span> 
   2. <span data-ttu-id="c704c-406">Győződjön meg arról, hogy az **Account selection method** (Fiókválasztási módszer) mezőben a **From Azure subscriptions** (Azure-előfizetésekből) lehetőség van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="c704c-406">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="c704c-407">Válassza ki az Azure-előfizetéssel, vagy hagyja **válassza ki az összes** a **Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="c704c-407">Select your Azure subscription or keep **Select all** for **Azure subscription**.</span></span>   
   4. <span data-ttu-id="c704c-408">Válasszon egy **Azure storage-fiók** hello a hello kiválasztott előfizetésben elérhető fiókok az Azure storage listája.</span><span class="sxs-lookup"><span data-stu-id="c704c-408">Select an **Azure storage account** from hello list of Azure storage accounts available in hello selected subscription.</span></span> <span data-ttu-id="c704c-409">Másik lehetőségként tooenter tárolási fiók beállításait manuálisan kiválasztásával **adja meg manuálisan** hello beállítása **kijelöléséről fiók**.</span><span class="sxs-lookup"><span data-stu-id="c704c-409">You can also choose tooenter storage account settings manually by selecting **Enter manually** option for hello **Account selection method**.</span></span>
   5. <span data-ttu-id="c704c-410">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c704c-410">Click **Next**.</span></span> 
      <span data-ttu-id="c704c-411">![Másolja az eszköz – hello Azure Blob storage-fiók megadása](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-411">![Copy Tool - Specify hello Azure Blob storage account](./media/data-factory-azure-blob-connector/copy-tool-specify-azure-blob-storage-account.png)</span></span>
5. <span data-ttu-id="c704c-412">A **válasszon hello bemeneti fájl vagy mappa** lap:</span><span class="sxs-lookup"><span data-stu-id="c704c-412">On **Choose hello input file or folder** page:</span></span>
   1. <span data-ttu-id="c704c-413">Kattintson duplán a **adfblobcontainer**.</span><span class="sxs-lookup"><span data-stu-id="c704c-413">Double-click **adfblobcontainer**.</span></span>
   2. <span data-ttu-id="c704c-414">Válassza ki **bemeneti**, és kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="c704c-414">Select **input**, and click **Choose**.</span></span> <span data-ttu-id="c704c-415">Ebben a bemutatóban hello bemeneti mappa kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="c704c-415">In this walkthrough, you select hello input folder.</span></span> <span data-ttu-id="c704c-416">Kiválaszthatja hello emp.txt fájl hello mappában helyette.</span><span class="sxs-lookup"><span data-stu-id="c704c-416">You could also select hello emp.txt file in hello folder instead.</span></span> 
      <span data-ttu-id="c704c-417">![Másolja az eszköz - a hello bemeneti fájl vagy mappa](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-417">![Copy Tool - Choose hello input file or folder](./media/data-factory-azure-blob-connector/copy-tool-choose-input-file-or-folder.png)</span></span>
6. <span data-ttu-id="c704c-418">A hello **válasszon hello bemeneti fájl vagy mappa** lap:</span><span class="sxs-lookup"><span data-stu-id="c704c-418">On hello **Choose hello input file or folder** page:</span></span>
    1. <span data-ttu-id="c704c-419">Győződjön meg arról, hogy hello **fájl vagy mappa** értéke túl**adfblobconnector/bemeneti**.</span><span class="sxs-lookup"><span data-stu-id="c704c-419">Confirm that hello **file or folder** is set too**adfblobconnector/input**.</span></span> <span data-ttu-id="c704c-420">Ha hello fájlok runbookok, 2017/04/01, 2017/04/02, és így tovább, írja be például adfblobconnector/bemeneti / {year} / {month} / {day} fájlra vagy mappára.</span><span class="sxs-lookup"><span data-stu-id="c704c-420">If hello files are in sub folders, for example, 2017/04/01, 2017/04/02, and so on, enter adfblobconnector/input/{year}/{month}/{day} for file or folder.</span></span> <span data-ttu-id="c704c-421">TAB billentyű megnyomásával kívül hello szövegmezőben, három legördülő listákból tooselect formátumok láthatja az év (éééé), a hónap (MM), és a nap (nn).</span><span class="sxs-lookup"><span data-stu-id="c704c-421">When you press TAB out of hello text box, you see three drop-down lists tooselect formats for year (yyyy), month (MM), and day (dd).</span></span> 
    2. <span data-ttu-id="c704c-422">Ne adja meg az **fájl rekurzív módon másolja**.</span><span class="sxs-lookup"><span data-stu-id="c704c-422">Do not set **Copy file recursively**.</span></span> <span data-ttu-id="c704c-423">Válassza ezt a beállítást toorecursively bejárás fájlok toobe másolt toohello cél mappákban.</span><span class="sxs-lookup"><span data-stu-id="c704c-423">Select this option toorecursively traverse through folders for files toobe copied toohello destination.</span></span> 
    3. <span data-ttu-id="c704c-424">Nem hello **bináris másolási** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c704c-424">Do not hello **binary copy** option.</span></span> <span data-ttu-id="c704c-425">Válassza ezt a beállítást tooperform forrás fájl toohello cél bináris másolatát.</span><span class="sxs-lookup"><span data-stu-id="c704c-425">Select this option tooperform a binary copy of source file toohello destination.</span></span> <span data-ttu-id="c704c-426">Nem jelölt ki ez a forgatókönyv, hogy a további beállítások a következő oldalain hello láthatja.</span><span class="sxs-lookup"><span data-stu-id="c704c-426">Do not select for this walkthrough so that you can see more options in hello next pages.</span></span> 
    4. <span data-ttu-id="c704c-427">Győződjön meg arról, hogy hello **tömörítési típus** értéke túl**nincs**.</span><span class="sxs-lookup"><span data-stu-id="c704c-427">Confirm that hello **Compression type** is set too**None**.</span></span> <span data-ttu-id="c704c-428">Válassza az ehhez a beállításhoz tartozó értéket, ha a forrásfájlok tömörített hello támogatott formátumok egyikében.</span><span class="sxs-lookup"><span data-stu-id="c704c-428">Select a value for this option if your source files are compressed in one of hello supported formats.</span></span> 
    5. <span data-ttu-id="c704c-429">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c704c-429">Click **Next**.</span></span>
    <span data-ttu-id="c704c-430">![Másolja az eszköz - a hello bemeneti fájl vagy mappa](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-430">![Copy Tool - Choose hello input file or folder](./media/data-factory-azure-blob-connector/chose-input-file-folder.png)</span></span> 
7. <span data-ttu-id="c704c-431">A hello **fájl formázási beállítások** lapon látni hello határolójelek és hello sémát, amelyhez hello varázsló automatikusan észleli hello-fájl elemzése.</span><span class="sxs-lookup"><span data-stu-id="c704c-431">On hello **File format settings** page, you see hello delimiters and hello schema that is auto-detected by hello wizard by parsing hello file.</span></span> 
    1. <span data-ttu-id="c704c-432">Erősítse meg az alábbi beállítások hello: egy.</span><span class="sxs-lookup"><span data-stu-id="c704c-432">Confirm hello following options: a.</span></span> <span data-ttu-id="c704c-433">Hello **fájlformátum** értéke túl**szövegformátum**.</span><span class="sxs-lookup"><span data-stu-id="c704c-433">hello **file format** is set too**Text format**.</span></span> <span data-ttu-id="c704c-434">Minden hello támogatott formátumok hello legördülő listában tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="c704c-434">You can see all hello supported formats in hello drop-down list.</span></span> <span data-ttu-id="c704c-435">Például: JSON, Avro, ORC, Parquet.</span><span class="sxs-lookup"><span data-stu-id="c704c-435">For example: JSON, Avro, ORC, Parquet.</span></span>
        <span data-ttu-id="c704c-436">b.</span><span class="sxs-lookup"><span data-stu-id="c704c-436">b.</span></span> <span data-ttu-id="c704c-437">Hello **oszlop elválasztó** értéke túl`Comma (,)`.</span><span class="sxs-lookup"><span data-stu-id="c704c-437">hello **column delimiter** is set too`Comma (,)`.</span></span> <span data-ttu-id="c704c-438">Látható hello más hello legördülő listából válassza ki a Data Factory által támogatott oszlop elválasztó karaktert.</span><span class="sxs-lookup"><span data-stu-id="c704c-438">You can see hello other column delimiters supported by Data Factory in hello drop-down list.</span></span> <span data-ttu-id="c704c-439">Egy egyéni elválasztó karakter is megadható.</span><span class="sxs-lookup"><span data-stu-id="c704c-439">You can also specify a custom delimiter.</span></span>
        <span data-ttu-id="c704c-440">c.</span><span class="sxs-lookup"><span data-stu-id="c704c-440">c.</span></span> <span data-ttu-id="c704c-441">Hello **sor elválasztó** értéke túl`Carriage Return + Line feed (\r\n)`.</span><span class="sxs-lookup"><span data-stu-id="c704c-441">hello **row delimiter** is set too`Carriage Return + Line feed (\r\n)`.</span></span> <span data-ttu-id="c704c-442">Látható hello más hello legördülő listából válassza ki a Data Factory által támogatott sor elválasztó karaktert.</span><span class="sxs-lookup"><span data-stu-id="c704c-442">You can see hello other row delimiters supported by Data Factory in hello drop-down list.</span></span> <span data-ttu-id="c704c-443">Egy egyéni elválasztó karakter is megadható.</span><span class="sxs-lookup"><span data-stu-id="c704c-443">You can also specify a custom delimiter.</span></span>
        <span data-ttu-id="c704c-444">d.</span><span class="sxs-lookup"><span data-stu-id="c704c-444">d.</span></span> <span data-ttu-id="c704c-445">Hello **sorszám kihagyása** értéke túl**0**.</span><span class="sxs-lookup"><span data-stu-id="c704c-445">hello **skip line count** is set too**0**.</span></span> <span data-ttu-id="c704c-446">Ha azt szeretné, hogy néhány sorok toobe hello felső hello fájl kihagyva, adja meg itt a hello számát.</span><span class="sxs-lookup"><span data-stu-id="c704c-446">If you want a few lines toobe skipped at hello top of hello file, enter hello number here.</span></span>
        <span data-ttu-id="c704c-447">e.</span><span class="sxs-lookup"><span data-stu-id="c704c-447">e.</span></span>  <span data-ttu-id="c704c-448">Hello **adatok első sora oszlopneveket tartalmaz** nincs beállítva.</span><span class="sxs-lookup"><span data-stu-id="c704c-448">hello **first data row contains column names** is not set.</span></span> <span data-ttu-id="c704c-449">Ha hello forrásfájlok hello első sorában oszlopneveket tartalmaz, akkor válassza ezt a lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c704c-449">If hello source files contain column names in hello first row, select this option.</span></span>
        <span data-ttu-id="c704c-450">f.</span><span class="sxs-lookup"><span data-stu-id="c704c-450">f.</span></span> <span data-ttu-id="c704c-451">Hello **üres oszlopérték tekinti null** beállítás.</span><span class="sxs-lookup"><span data-stu-id="c704c-451">hello **treat empty column value as null** option is set.</span></span>
    2. <span data-ttu-id="c704c-452">Bontsa ki a **speciális beállítások** toosee speciális beállítás érhető el.</span><span class="sxs-lookup"><span data-stu-id="c704c-452">Expand **Advanced settings** toosee advanced option available.</span></span>
    3. <span data-ttu-id="c704c-453">Hello hello lap alsó részén, lásd: hello **előzetes** adatok hello emp.txt fájlból.</span><span class="sxs-lookup"><span data-stu-id="c704c-453">At hello bottom of hello page, see hello **preview** of data from hello emp.txt file.</span></span>
    4. <span data-ttu-id="c704c-454">Kattintson a **SÉMA** hello forrásfájlban hello adatok megtekintésével következtetni hello másolása varázsló hello alsó toosee hello séma lapot.</span><span class="sxs-lookup"><span data-stu-id="c704c-454">Click **SCHEMA** tab at hello bottom toosee hello schema that hello copy wizard inferred by looking at hello data in hello source file.</span></span>
    5. <span data-ttu-id="c704c-455">Kattintson a **következő** után tekintse át a hello elválasztó karaktert, és tekintse meg adatok.</span><span class="sxs-lookup"><span data-stu-id="c704c-455">Click **Next** after you review hello delimiters and preview data.</span></span>
    <span data-ttu-id="c704c-456">![Eszköz - fájl formátuma beállítások másolása](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-456">![Copy Tool - File format settings](./media/data-factory-azure-blob-connector/copy-tool-file-format-settings.png)</span></span>  
8. <span data-ttu-id="c704c-457">A hello **cél adattároló lap**, jelölje be **Azure Blob Storage**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="c704c-457">On hello **Destination data store page**, select **Azure Blob Storage**, and click **Next**.</span></span> <span data-ttu-id="c704c-458">Hello Azure Blob Storage mindkét hello forrás és cél adatokat tárolja a forgatókönyv használ.</span><span class="sxs-lookup"><span data-stu-id="c704c-458">You are using hello Azure Blob Storage as both hello source and destination data stores in this walkthrough.</span></span>    
    <span data-ttu-id="c704c-459">![Eszköz - célkiszolgáló kijelölése adattár másolása](media/data-factory-azure-blob-connector/select-destination-data-store.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-459">![Copy Tool - select destination data store](media/data-factory-azure-blob-connector/select-destination-data-store.png)</span></span>
9. <span data-ttu-id="c704c-460">A **hello Azure Blob storage-fiók megadása** lap:</span><span class="sxs-lookup"><span data-stu-id="c704c-460">On **Specify hello Azure Blob storage account** page:</span></span>
   1. <span data-ttu-id="c704c-461">Adja meg **AzureStorageLinkedService** a hello **kapcsolatnév** mező.</span><span class="sxs-lookup"><span data-stu-id="c704c-461">Enter **AzureStorageLinkedService** for hello **Connection name** field.</span></span>
   2. <span data-ttu-id="c704c-462">Győződjön meg arról, hogy az **Account selection method** (Fiókválasztási módszer) mezőben a **From Azure subscriptions** (Azure-előfizetésekből) lehetőség van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="c704c-462">Confirm that **From Azure subscriptions** option is selected for **Account selection method**.</span></span>
   3. <span data-ttu-id="c704c-463">Jelölje ki az Azure-**előfizetést**.</span><span class="sxs-lookup"><span data-stu-id="c704c-463">Select your Azure **subscription**.</span></span>  
   4. <span data-ttu-id="c704c-464">Válassza ki az Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="c704c-464">Select your Azure storage account.</span></span> 
   5. <span data-ttu-id="c704c-465">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c704c-465">Click **Next**.</span></span>     
10. <span data-ttu-id="c704c-466">A hello **válasszon hello kimeneti fájl vagy mappa** lap:</span><span class="sxs-lookup"><span data-stu-id="c704c-466">On hello **Choose hello output file or folder** page:</span></span> 
    6. <span data-ttu-id="c704c-467">Adja meg **mappa elérési útja** , **adfblobconnector/kimeneti / {year} / {month} / {day}**.</span><span class="sxs-lookup"><span data-stu-id="c704c-467">specify **Folder path** as **adfblobconnector/output/{year}/{month}/{day}**.</span></span> <span data-ttu-id="c704c-468">Adja meg **lapon**.</span><span class="sxs-lookup"><span data-stu-id="c704c-468">Enter **TAB**.</span></span>
    7. <span data-ttu-id="c704c-469">A hello **év**, jelölje be **éééé**.</span><span class="sxs-lookup"><span data-stu-id="c704c-469">For hello **year**, select **yyyy**.</span></span>
    8. <span data-ttu-id="c704c-470">A hello **hónap**, győződjön meg arról, hogy túl van-e állítva**MM**.</span><span class="sxs-lookup"><span data-stu-id="c704c-470">For hello **month**, confirm that it is set too**MM**.</span></span>
    9. <span data-ttu-id="c704c-471">A hello **nap**, győződjön meg arról, hogy túl van-e állítva**nn**.</span><span class="sxs-lookup"><span data-stu-id="c704c-471">For hello **day**, confirm that it is set too**dd**.</span></span>
    10. <span data-ttu-id="c704c-472">Győződjön meg arról, hogy hello **tömörítési típus** értéke túl**nincs**.</span><span class="sxs-lookup"><span data-stu-id="c704c-472">Confirm that hello **compression type** is set too**None**.</span></span>
    11. <span data-ttu-id="c704c-473">Győződjön meg arról, hogy hello **viselkedés másolása** értéke túl**fájlok egyesítése**.</span><span class="sxs-lookup"><span data-stu-id="c704c-473">Confirm that hello **copy behavior** is set too**Merge files**.</span></span> <span data-ttu-id="c704c-474">Ha hello kimeneti hello ugyanazzal a névvel már létezik a fájl, hello új tartalma hozzáadott toohello ugyanazon fájl hello végén.</span><span class="sxs-lookup"><span data-stu-id="c704c-474">If hello output file with hello same name already exists, hello new content is added toohello same file at hello end.</span></span>
    12. <span data-ttu-id="c704c-475">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c704c-475">Click **Next**.</span></span>
    <span data-ttu-id="c704c-476">![Másolja az eszköz - a kimeneti fájl vagy mappa](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-476">![Copy Tool - Choose output file or folder](media/data-factory-azure-blob-connector/choose-the-output-file-or-folder.png)</span></span>
11. <span data-ttu-id="c704c-477">A hello **fájl formázási beállítások** lapon tekintse át a hello-beállítások, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="c704c-477">On hello **File format settings** page, review hello settings, and click **Next**.</span></span> <span data-ttu-id="c704c-478">Hello Itt a további beállítások egyike tooadd egy fejléc toohello kimeneti fájl.</span><span class="sxs-lookup"><span data-stu-id="c704c-478">One of hello additional options here is tooadd a header toohello output file.</span></span> <span data-ttu-id="c704c-479">Ha ezt a beállítást választja, a fejlécsor hozzá rendelkező hello oszlopainak hello séma hello forrás.</span><span class="sxs-lookup"><span data-stu-id="c704c-479">If you select that option, a header row is added with names of hello columns from hello schema of hello source.</span></span> <span data-ttu-id="c704c-480">Átnevezheti hello alapértelmezett oszlopnevek hello séma hello forrás megtekintésekor.</span><span class="sxs-lookup"><span data-stu-id="c704c-480">You can rename hello default column names when viewing hello schema for hello source.</span></span> <span data-ttu-id="c704c-481">Megváltoztathatja például a hello első oszlop tooFirst nevét és a második oszlopban tooLast nevét.</span><span class="sxs-lookup"><span data-stu-id="c704c-481">For example, you could change hello first column tooFirst Name and second column tooLast Name.</span></span> <span data-ttu-id="c704c-482">Ezt követően hello kimeneti fájl jön létre az ezekkel a nevekkel fejléc oszlopnevekként.</span><span class="sxs-lookup"><span data-stu-id="c704c-482">Then, hello output file is generated with a header with these names as column names.</span></span> 
    <span data-ttu-id="c704c-483">![Eszköz - cél formátum beállításainak fájl másolása](media/data-factory-azure-blob-connector/file-format-destination.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-483">![Copy Tool - File format settings for destination](media/data-factory-azure-blob-connector/file-format-destination.png)</span></span>
12. <span data-ttu-id="c704c-484">A hello **Teljesítménybeállítások** lapján ellenőrizze, hogy **egységek cloud** és **másolatok párhuzamos** túl van beállítva**automatikus**, és kattintson a Tovább gombra.</span><span class="sxs-lookup"><span data-stu-id="c704c-484">On hello **Performance settings** page, confirm that **cloud units** and **parallel copies** are set too**Auto**, and click Next.</span></span> <span data-ttu-id="c704c-485">Ezek a beállítások kapcsolatos részletekért lásd: [másolása tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md#parallel-copy).</span><span class="sxs-lookup"><span data-stu-id="c704c-485">For details about these settings, see [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md#parallel-copy).</span></span>
    <span data-ttu-id="c704c-486">![Eszköz - teljesítmény beállítások másolása](media/data-factory-azure-blob-connector/copy-performance-settings.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-486">![Copy Tool - Performance settings](media/data-factory-azure-blob-connector/copy-performance-settings.png)</span></span> 
14. <span data-ttu-id="c704c-487">A hello **összegzés** lapon tekintse át az összes beállítást (a feladat tulajdonságai, a beállításokat a forrás és cél és a beállítások), és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="c704c-487">On hello **Summary** page, review all settings (task properties, settings for source and destination, and copy settings), and click **Next**.</span></span>
    <span data-ttu-id="c704c-488">![Másolja az eszköz - összegző lapja](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-488">![Copy Tool - Summary page](media/data-factory-azure-blob-connector/copy-tool-summary-page.png)</span></span>
15. <span data-ttu-id="c704c-489">Tekintse át a hello **összegzés** lapon, majd kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="c704c-489">Review information in hello **Summary** page, and click **Finish**.</span></span> <span data-ttu-id="c704c-490">hello varázsló létrehoz két társított szolgáltatások, két adatkészletet (bemeneti és kimeneti) és egy folyamat (amelyen Ön indítani hello másolása varázsló) az adat-előállító hello.</span><span class="sxs-lookup"><span data-stu-id="c704c-490">hello wizard creates two linked services, two datasets (input and output), and one pipeline in hello data factory (from where you launched hello Copy Wizard).</span></span>
    <span data-ttu-id="c704c-491">![Másolja az eszköz - központi telepítés lap](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-491">![Copy Tool - Deployment page](media/data-factory-azure-blob-connector/copy-tool-deployment-page.png)</span></span>

### <a name="monitor-hello-pipeline-copy-task"></a><span data-ttu-id="c704c-492">A figyelő hello csővezeték (feladatok)</span><span class="sxs-lookup"><span data-stu-id="c704c-492">Monitor hello pipeline (copy task)</span></span>

1. <span data-ttu-id="c704c-493">Hello hivatkozásra `Click here toomonitor copy pipeline` a hello **telepítési** lap.</span><span class="sxs-lookup"><span data-stu-id="c704c-493">Click hello link `Click here toomonitor copy pipeline` on hello **Deployment** page.</span></span> 
2. <span data-ttu-id="c704c-494">Megtekintheti az hello **felügyeletéhez és kezeléséhez az alkalmazás** egy külön lapján.  ![Megfigyelés és kezelés alkalmazás](media/data-factory-azure-blob-connector/monitor-manage-app.png)</span><span class="sxs-lookup"><span data-stu-id="c704c-494">You should see hello **Monitor and Manage application** in a separate tab.  ![Monitor and Manage App](media/data-factory-azure-blob-connector/monitor-manage-app.png)</span></span>
3. <span data-ttu-id="c704c-495">Változás hello **start** idő hello felső túl`04/19/2017` és **end** idő túl`04/27/2017`, és kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="c704c-495">Change hello **start** time at hello top too`04/19/2017` and **end** time too`04/27/2017`, and then click **Apply**.</span></span> 
4. <span data-ttu-id="c704c-496">Megtekintheti az öt tevékenység a windows hello **tevékenység WINDOWS** listája.</span><span class="sxs-lookup"><span data-stu-id="c704c-496">You should see five activity windows in hello **ACTIVITY WINDOWS** list.</span></span> <span data-ttu-id="c704c-497">Hello **WindowStart** alkalommal közé tartozik a csővezeték indítási toopipeline befejezési idejének minden nap.</span><span class="sxs-lookup"><span data-stu-id="c704c-497">hello **WindowStart** times should cover all days from pipeline start toopipeline end times.</span></span> 
5. <span data-ttu-id="c704c-498">Kattintson a **frissítése** hello gombra **tevékenység WINDOWS** lista néhány alkalommal amíg minden hello tevékenység windows hello állapotának tooReady van beállítva.</span><span class="sxs-lookup"><span data-stu-id="c704c-498">Click **Refresh** button for hello **ACTIVITY WINDOWS** list a few times until you see hello status of all hello activity windows is set tooReady.</span></span> 
6. <span data-ttu-id="c704c-499">Most győződjön meg arról, hogy a kimeneti fájlok hello adfblobconnector tároló hello kimeneti mappában legyenek létrehozva.</span><span class="sxs-lookup"><span data-stu-id="c704c-499">Now, verify that hello output files are generated in hello output folder of adfblobconnector container.</span></span> <span data-ttu-id="c704c-500">A következő mappaszerkezet hello kimeneti mappában hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="c704c-500">You should see hello following folder structure in hello output folder:</span></span> 
    ```
    2017/04/21
    2017/04/22
    2017/04/23
    2017/04/24
    2017/04/25    
    ```
<span data-ttu-id="c704c-501">Figyelése és kezelése az adat-előállítók kapcsolatos részletes információkért lásd: [figyelése és kezelése a Data Factory-folyamathoz](data-factory-monitor-manage-app.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="c704c-501">For detailed information about monitoring and managing data factories, see [Monitor and manage Data Factory pipeline](data-factory-monitor-manage-app.md) article.</span></span> 
 
### <a name="data-factory-entities"></a><span data-ttu-id="c704c-502">Data Factory entitások</span><span class="sxs-lookup"><span data-stu-id="c704c-502">Data Factory entities</span></span>
<span data-ttu-id="c704c-503">Most váltson vissza toohello hello adat-előállító Kezdőlap lap.</span><span class="sxs-lookup"><span data-stu-id="c704c-503">Now, switch back toohello tab with hello Data Factory home page.</span></span> <span data-ttu-id="c704c-504">Figyelje meg, hogy van két összekapcsolt szolgáltatások, két adatkészletet, és a data factory egyik adatcsatornáinak most.</span><span class="sxs-lookup"><span data-stu-id="c704c-504">Notice that there are two linked services, two datasets, and one pipeline in your data factory now.</span></span> 

![Data Factory kezdőlapján entitások](media/data-factory-azure-blob-connector/data-factory-home-page-with-numbers.png)

<span data-ttu-id="c704c-506">Kattintson a **Szerző és központi telepítése** toolaunch Data Factory Editor.</span><span class="sxs-lookup"><span data-stu-id="c704c-506">Click **Author and deploy** toolaunch Data Factory Editor.</span></span> 

![A Data Factory szerkesztője](media/data-factory-azure-blob-connector/data-factory-editor.png)

<span data-ttu-id="c704c-508">A következő adat-előállító bejegyzései szerepelnek a data factory hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="c704c-508">You should see hello following Data Factory entities in your data factory:</span></span> 

 - <span data-ttu-id="c704c-509">Két társított szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="c704c-509">Two linked services.</span></span> <span data-ttu-id="c704c-510">Egy a hello forrás és cél hello egy másikra hello.</span><span class="sxs-lookup"><span data-stu-id="c704c-510">One for hello source and hello other one for hello destination.</span></span> <span data-ttu-id="c704c-511">Mindkét kapcsolódó hello szolgáltatás toohello tekintse meg a forgatókönyv azonos Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="c704c-511">Both hello linked services refer toohello same Azure Storage account in this walkthrough.</span></span> 
 - <span data-ttu-id="c704c-512">Két adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="c704c-512">Two datasets.</span></span> <span data-ttu-id="c704c-513">Egy bemeneti adatkészlet és egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="c704c-513">An input dataset and an output dataset.</span></span> <span data-ttu-id="c704c-514">Ez a forgatókönyv mindkettő használja a hello azonos blob-tároló, de toodifferent mappák (bemeneti és kimeneti) hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="c704c-514">In this walkthrough, both use hello same blob container but refer toodifferent folders (input and output).</span></span>
 - <span data-ttu-id="c704c-515">Egy folyamat.</span><span class="sxs-lookup"><span data-stu-id="c704c-515">A pipeline.</span></span> <span data-ttu-id="c704c-516">hello feldolgozási sor tartalmazza a másolási tevékenység, amely egy blob-forrás- és egy blob fogadó toocopy adatokat az Azure blob hely tooanother Azure blob helyére használja.</span><span class="sxs-lookup"><span data-stu-id="c704c-516">hello pipeline contains a copy activity that uses a blob source and a blob sink toocopy data from an Azure blob location tooanother Azure blob location.</span></span> 

<span data-ttu-id="c704c-517">hello következő szakaszok további információval szolgálnak ezeket az entitásokat.</span><span class="sxs-lookup"><span data-stu-id="c704c-517">hello following sections provide more information about these entities.</span></span> 

#### <a name="linked-services"></a><span data-ttu-id="c704c-518">Társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="c704c-518">Linked services</span></span>
<span data-ttu-id="c704c-519">Két összekapcsolt szolgáltatások kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="c704c-519">You should see two linked services.</span></span> <span data-ttu-id="c704c-520">Egy a hello forrás és cél hello egy másikra hello.</span><span class="sxs-lookup"><span data-stu-id="c704c-520">One for hello source and hello other one for hello destination.</span></span> <span data-ttu-id="c704c-521">Mindkét definíciók tekintse meg a forgatókönyv hello hello nevek kivételével azonos.</span><span class="sxs-lookup"><span data-stu-id="c704c-521">In this walkthrough, both definitions look hello same except for hello names.</span></span> <span data-ttu-id="c704c-522">Hello **típus** hello a társított szolgáltatás túl van beállítva**AzureStorage**.</span><span class="sxs-lookup"><span data-stu-id="c704c-522">hello **type** of hello linked service is set too**AzureStorage**.</span></span> <span data-ttu-id="c704c-523">Hello társított szolgáltatás definíciójának legfontosabb tulajdonsága hello **connectionString**, amely adat-előállító tooconnect tooyour futásidőben Azure Storage-fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="c704c-523">Most important property of hello linked service definition is hello **connectionString**, which is used by Data Factory tooconnect tooyour Azure Storage account at runtime.</span></span> <span data-ttu-id="c704c-524">Hagyja figyelmen kívül hello hubName tulajdonság hello definícióban.</span><span class="sxs-lookup"><span data-stu-id="c704c-524">Ignore hello hubName property in hello definition.</span></span> 

##### <a name="source-blob-storage-linked-service"></a><span data-ttu-id="c704c-525">Forrás blob storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c704c-525">Source blob storage linked service</span></span>
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

##### <a name="destination-blob-storage-linked-service"></a><span data-ttu-id="c704c-526">Cél blob storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="c704c-526">Destination blob storage linked service</span></span>

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

<span data-ttu-id="c704c-527">Az Azure tárolás társított szolgáltatásának kapcsolatos további információkért lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c704c-527">For more information about Azure Storage linked service, see [Linked service properties](#linked-service-properties) section.</span></span> 

#### <a name="datasets"></a><span data-ttu-id="c704c-528">Adathalmazok</span><span class="sxs-lookup"><span data-stu-id="c704c-528">Datasets</span></span>
<span data-ttu-id="c704c-529">Két adatkészletet van: egy bemeneti adatkészlet és egy kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="c704c-529">There are two datasets: an input dataset and an output dataset.</span></span> <span data-ttu-id="c704c-530">hello típusú hello dataset értéke túl**AzureBlob** is.</span><span class="sxs-lookup"><span data-stu-id="c704c-530">hello type of hello dataset is set too**AzureBlob** for both.</span></span> 

<span data-ttu-id="c704c-531">hello bemeneti adatkészlet mutat toohello **bemeneti** mappában található hello **adfblobconnector** blob tároló.</span><span class="sxs-lookup"><span data-stu-id="c704c-531">hello input dataset points toohello **input** folder of hello **adfblobconnector** blob container.</span></span> <span data-ttu-id="c704c-532">Hello **külső** tulajdonsága túl**igaz** ehhez az adatkészlethez, hello adatok nem hozzák hello csővezeték hello másolási tevékenységhez, amely ehhez az adatkészlethez bemenetként.</span><span class="sxs-lookup"><span data-stu-id="c704c-532">hello **external** property is set too**true** for this dataset as hello data is not produced by hello pipeline with hello copy activity that takes this dataset as an input.</span></span> 

<span data-ttu-id="c704c-533">kimeneti adatkészlet pontok toohello hello **kimeneti** mappában található hello blob tárolóhoz.</span><span class="sxs-lookup"><span data-stu-id="c704c-533">hello output dataset points toohello **output** folder of hello same blob container.</span></span> <span data-ttu-id="c704c-534">hello kimeneti adatkészletet is használ hello év, hónap és hello napján **SliceStart** rendszer változó toodynamically kiértékelése hello hello kimeneti fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="c704c-534">hello output dataset also uses hello year, month, and day of hello **SliceStart** system variable toodynamically evaluate hello path for hello output file.</span></span> <span data-ttu-id="c704c-535">Függvények és a Data Factory által támogatott rendszerváltozók listáját lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="c704c-535">For a list of functions and system variables supported by Data Factory, see [Data Factory functions and system variables](data-factory-functions-variables.md).</span></span> <span data-ttu-id="c704c-536">Hello **külső** tulajdonsága túl**hamis** (alapértelmezett érték), mert ez az adatkészlet hozzák hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="c704c-536">hello **external** property is set too**false** (default value) because this dataset is produced by hello pipeline.</span></span> 

<span data-ttu-id="c704c-537">Azure Blob-adathalmazra által támogatott tulajdonságokról kapcsolatos további információkért lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c704c-537">For more information about properties supported by Azure Blob dataset, see [Dataset properties](#dataset-properties) section.</span></span>

##### <a name="input-dataset"></a><span data-ttu-id="c704c-538">Bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="c704c-538">Input dataset</span></span>

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

##### <a name="output-dataset"></a><span data-ttu-id="c704c-539">Kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="c704c-539">Output dataset</span></span>

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

#### <a name="pipeline"></a><span data-ttu-id="c704c-540">Folyamat</span><span class="sxs-lookup"><span data-stu-id="c704c-540">Pipeline</span></span>
<span data-ttu-id="c704c-541">hello folyamat csak egy tevékenység rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c704c-541">hello pipeline has just one activity.</span></span> <span data-ttu-id="c704c-542">Hello **típus** hello tevékenység van állítva túl**másolási**.</span><span class="sxs-lookup"><span data-stu-id="c704c-542">hello **type** of hello activity is set too**Copy**.</span></span>  <span data-ttu-id="c704c-543">A típus tulajdonságai hello hello tevékenység szakaszok is vannak két, egy a forrás- és a fogadó egy másikra hello.</span><span class="sxs-lookup"><span data-stu-id="c704c-543">In hello type properties for hello activity, there are two sections, one for source and hello other one for sink.</span></span> <span data-ttu-id="c704c-544">hello adatforrás típusának értéke túl**BlobSource** , hello tevékenység van az adatok másolása egy blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c704c-544">hello source type is set too**BlobSource** as hello activity is copying data from a blob storage.</span></span> <span data-ttu-id="c704c-545">hello a fogadó típusa értéke túl**BlobSink** hello tevékenységként adattárolás tooa blob másolása.</span><span class="sxs-lookup"><span data-stu-id="c704c-545">hello sink type is set too**BlobSink** as hello activity copying data tooa blob storage.</span></span> <span data-ttu-id="c704c-546">hello másolási tevékenység InputDataset-z4y hello bemeneti és OutputDataset-z4y hello kimenetként vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c704c-546">hello copy activity takes InputDataset-z4y as hello input and OutputDataset-z4y as hello output.</span></span> 

<span data-ttu-id="c704c-547">BlobSource és BlobSink által támogatott tulajdonságokról kapcsolatos további információkért lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz.</span><span class="sxs-lookup"><span data-stu-id="c704c-547">For more information about properties supported by BlobSource and BlobSink, see [Copy activity properties](#copy-activity-properties) section.</span></span> 

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

## <a name="json-examples-for-copying-data-tooand-from-blob-storage"></a><span data-ttu-id="c704c-548">Az adatok tooand másolását a Blob Storage JSON példák</span><span class="sxs-lookup"><span data-stu-id="c704c-548">JSON examples for copying data tooand from Blob Storage</span></span>  
<span data-ttu-id="c704c-549">hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="c704c-549">hello following examples provide sample JSON definitions that you can use toocreate a pipeline by using [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md) or [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) or [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span> <span data-ttu-id="c704c-550">Azok hogyan toocopy adatok tooand az Azure Blob Storage és az Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="c704c-550">They show how toocopy data tooand from Azure Blob Storage and Azure SQL Database.</span></span> <span data-ttu-id="c704c-551">Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.</span><span class="sxs-lookup"><span data-stu-id="c704c-551">However, data can be copied **directly** from any of sources tooany of hello sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores-and-formats) using hello Copy Activity in Azure Data Factory.</span></span>

### <a name="json-example-copy-data-from-blob-storage-toosql-database"></a><span data-ttu-id="c704c-552">JSON-NÁ. példa: Adatok másolása a Blob Storage tooSQL adatbázis</span><span class="sxs-lookup"><span data-stu-id="c704c-552">JSON Example: Copy data from Blob Storage tooSQL Database</span></span>
<span data-ttu-id="c704c-553">a következő példa azt mutatja be hello:</span><span class="sxs-lookup"><span data-stu-id="c704c-553">hello following sample shows:</span></span>

1. <span data-ttu-id="c704c-554">A társított szolgáltatás típusa [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c704c-554">A linked service of type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="c704c-555">A társított szolgáltatás típusa [AzureStorage](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c704c-555">A linked service of type [AzureStorage](#linked-service-properties).</span></span>
3. <span data-ttu-id="c704c-556">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c704c-556">An input [dataset](data-factory-create-datasets.md) of type [AzureBlob](#dataset-properties).</span></span>
4. <span data-ttu-id="c704c-557">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c704c-557">An output [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
5. <span data-ttu-id="c704c-558">A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [BlobSource](#copy-activity-properties) és [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="c704c-558">A [pipeline](data-factory-create-pipelines.md) with a Copy activity that uses [BlobSource](#copy-activity-properties) and [SqlSink](data-factory-azure-sql-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="c704c-559">hello minta idősorozat adatainak másolása az Azure blob tooan Azure SQL-táblából óránként.</span><span class="sxs-lookup"><span data-stu-id="c704c-559">hello sample copies time-series data from an Azure blob tooan Azure SQL table hourly.</span></span> <span data-ttu-id="c704c-560">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="c704c-560">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="c704c-561">**Az Azure SQL társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="c704c-561">**Azure SQL linked service:**</span></span>

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
<span data-ttu-id="c704c-562">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="c704c-562">**Azure Storage linked service:**</span></span>

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
<span data-ttu-id="c704c-563">Az Azure Data Factory két típusú Azure Storage társított szolgáltatásokat támogat: **AzureStorage** és **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="c704c-563">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="c704c-564">Az első hello, hello kapcsolati karakterlánc, amely tartalmazza az hello fiókkulcs ad meg, és hello újabb egy, a közös hozzáférésű Jogosultságkód (SAS) Uri hello meg.</span><span class="sxs-lookup"><span data-stu-id="c704c-564">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="c704c-565">Lásd: [összekapcsolt szolgáltatások](#linked-service-properties) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="c704c-565">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="c704c-566">**Az Azure Blob bemeneti adatkészletet:**</span><span class="sxs-lookup"><span data-stu-id="c704c-566">**Azure Blob input dataset:**</span></span>

<span data-ttu-id="c704c-567">Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="c704c-567">Data is picked up from a new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="c704c-568">hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="c704c-568">hello folder path and file name for hello blob are dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="c704c-569">hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello.</span><span class="sxs-lookup"><span data-stu-id="c704c-569">hello folder path uses year, month, and day part of hello start time and file name uses hello hour part of hello start time.</span></span> <span data-ttu-id="c704c-570">"external": "true" beállítás arról értesíti az hello tábla külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák adat-Előállítóban.</span><span class="sxs-lookup"><span data-stu-id="c704c-570">“external”: “true” setting informs Data Factory that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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
<span data-ttu-id="c704c-571">**Az Azure SQL kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="c704c-571">**Azure SQL output dataset:**</span></span>

<span data-ttu-id="c704c-572">hello minta Azure SQL adatbázis "MyTable" nevű tooa adattábla másolja.</span><span class="sxs-lookup"><span data-stu-id="c704c-572">hello sample copies data tooa table named “MyTable” in an Azure SQL database.</span></span> <span data-ttu-id="c704c-573">Hello tábla létrehozása az Azure SQL adatbázis azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt.</span><span class="sxs-lookup"><span data-stu-id="c704c-573">Create hello table in your Azure SQL database with hello same number of columns as you expect hello Blob CSV file toocontain.</span></span> <span data-ttu-id="c704c-574">Új sorok hozzáadásakor toohello tábla óránként.</span><span class="sxs-lookup"><span data-stu-id="c704c-574">New rows are added toohello table every hour.</span></span>

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
<span data-ttu-id="c704c-575">**A másolási tevékenység során a Blob-forrás és fogadó SQL-feldolgozási folyamat:**</span><span class="sxs-lookup"><span data-stu-id="c704c-575">**A copy activity in a pipeline with Blob source and SQL sink:**</span></span>

<span data-ttu-id="c704c-576">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="c704c-576">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="c704c-577">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**SqlSink**.</span><span class="sxs-lookup"><span data-stu-id="c704c-577">In hello pipeline JSON definition, hello **source** type is set too**BlobSource** and **sink** type is set too**SqlSink**.</span></span>

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
### <a name="json-example-copy-data-from-azure-sql-tooazure-blob"></a><span data-ttu-id="c704c-578">JSON-NÁ. példa: Adatok másolása az Azure SQL tooAzure Blob</span><span class="sxs-lookup"><span data-stu-id="c704c-578">JSON Example: Copy data from Azure SQL tooAzure Blob</span></span>
<span data-ttu-id="c704c-579">a következő példa azt mutatja be hello:</span><span class="sxs-lookup"><span data-stu-id="c704c-579">hello following sample shows:</span></span>

1. <span data-ttu-id="c704c-580">A társított szolgáltatás típusa [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c704c-580">A linked service of type [AzureSqlDatabase](data-factory-azure-sql-connector.md#linked-service-properties).</span></span>
2. <span data-ttu-id="c704c-581">A társított szolgáltatás típusa [AzureStorage](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="c704c-581">A linked service of type [AzureStorage](#linked-service-properties).</span></span>
3. <span data-ttu-id="c704c-582">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c704c-582">An input [dataset](data-factory-create-datasets.md) of type [AzureSqlTable](data-factory-azure-sql-connector.md#dataset-properties).</span></span>
4. <span data-ttu-id="c704c-583">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="c704c-583">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](#dataset-properties).</span></span>
5. <span data-ttu-id="c704c-584">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) és [BlobSink](#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="c704c-584">A [pipeline](data-factory-create-pipelines.md) with Copy activity that uses [SqlSource](data-factory-azure-sql-connector.md#copy-activity-properties) and [BlobSink](#copy-activity-properties).</span></span>

<span data-ttu-id="c704c-585">hello minta másol idősorozat adatokat az Azure SQL-tábla tooan Azure blob óránként.</span><span class="sxs-lookup"><span data-stu-id="c704c-585">hello sample copies time-series data from an Azure SQL table tooan Azure blob hourly.</span></span> <span data-ttu-id="c704c-586">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="c704c-586">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

<span data-ttu-id="c704c-587">**Az Azure SQL társított szolgáltatásnak:**</span><span class="sxs-lookup"><span data-stu-id="c704c-587">**Azure SQL linked service:**</span></span>

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
<span data-ttu-id="c704c-588">**Az Azure tárolás társított szolgáltatásának:**</span><span class="sxs-lookup"><span data-stu-id="c704c-588">**Azure Storage linked service:**</span></span>

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
<span data-ttu-id="c704c-589">Az Azure Data Factory két típusú Azure Storage társított szolgáltatásokat támogat: **AzureStorage** és **AzureStorageSas**.</span><span class="sxs-lookup"><span data-stu-id="c704c-589">Azure Data Factory supports two types of Azure Storage linked services: **AzureStorage** and **AzureStorageSas**.</span></span> <span data-ttu-id="c704c-590">Az első hello, hello kapcsolati karakterlánc, amely tartalmazza az hello fiókkulcs ad meg, és hello újabb egy, a közös hozzáférésű Jogosultságkód (SAS) Uri hello meg.</span><span class="sxs-lookup"><span data-stu-id="c704c-590">For hello first one, you specify hello connection string that includes hello account key and for hello later one, you specify hello Shared Access Signature (SAS) Uri.</span></span> <span data-ttu-id="c704c-591">Lásd: [összekapcsolt szolgáltatások](#linked-service-properties) című szakaszban talál információt.</span><span class="sxs-lookup"><span data-stu-id="c704c-591">See [Linked Services](#linked-service-properties) section for details.</span></span>  

<span data-ttu-id="c704c-592">**Az Azure SQL bemeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="c704c-592">**Azure SQL input dataset:**</span></span>

<span data-ttu-id="c704c-593">hello minta azt feltételezi, hogy létrehozott egy tábla "MyTable" Azure SQL, egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="c704c-593">hello sample assumes you have created a table “MyTable” in Azure SQL and it contains a column called “timestampcolumn” for time series data.</span></span>

<span data-ttu-id="c704c-594">"External" beállítása: "true" tájékoztatja Data Factory szolgáltatásnak, hogy hello tábla külső toohello adat-előállító, ezért nem hozzák hello adat-előállítóban tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="c704c-594">Setting “external”: ”true” informs Data Factory service that hello table is external toohello data factory and is not produced by an activity in hello data factory.</span></span>

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

<span data-ttu-id="c704c-595">**Az Azure Blob kimeneti adatkészlet:**</span><span class="sxs-lookup"><span data-stu-id="c704c-595">**Azure Blob output dataset:**</span></span>

<span data-ttu-id="c704c-596">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="c704c-596">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="c704c-597">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="c704c-597">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="c704c-598">hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.</span><span class="sxs-lookup"><span data-stu-id="c704c-598">hello folder path uses year, month, day, and hours parts of hello start time.</span></span>

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

<span data-ttu-id="c704c-599">**A másolási tevékenység során az SQL-forrás és fogadó Blob egy folyamaton belül:**</span><span class="sxs-lookup"><span data-stu-id="c704c-599">**A copy activity in a pipeline with SQL source and Blob sink:**</span></span>

<span data-ttu-id="c704c-600">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="c704c-600">hello pipeline contains a Copy Activity that is configured toouse hello input and output datasets and is scheduled toorun every hour.</span></span> <span data-ttu-id="c704c-601">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlSource** és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="c704c-601">In hello pipeline JSON definition, hello **source** type is set too**SqlSource** and **sink** type is set too**BlobSink**.</span></span> <span data-ttu-id="c704c-602">hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.</span><span class="sxs-lookup"><span data-stu-id="c704c-602">hello SQL query specified for hello **SqlReaderQuery** property selects hello data in hello past hour toocopy.</span></span>

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
> <span data-ttu-id="c704c-603">Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="c704c-603">toomap columns from source dataset toocolumns from sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>

## <a name="performance-and-tuning"></a><span data-ttu-id="c704c-604">Teljesítmény- és hangolása</span><span class="sxs-lookup"><span data-stu-id="c704c-604">Performance and Tuning</span></span>
<span data-ttu-id="c704c-605">Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.</span><span class="sxs-lookup"><span data-stu-id="c704c-605">See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) toolearn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways toooptimize it.</span></span>
