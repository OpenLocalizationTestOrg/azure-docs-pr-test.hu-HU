---
title: "aaaMove Data Factory használatával Amazon egyszerű tárolási szolgáltatás adatait |} Microsoft Docs"
description: "Megtudhatja, hogyan toomove Azure Data Factory használatával Amazon egyszerű tárolási szolgáltatás (S3) adatait."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 8a8cd2845fd1de74413bd0372f3aabfb4817549b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a><span data-ttu-id="b0129-103">Adatok áthelyezése tárolószolgáltatásból Amazon egyszerű Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="b0129-103">Move data from Amazon Simple Storage Service by using Azure Data Factory</span></span>
<span data-ttu-id="b0129-104">Ez a cikk azt ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatok tárolószolgáltatásból Amazon egyszerű (S3).</span><span class="sxs-lookup"><span data-stu-id="b0129-104">This article explains how toouse hello copy activity in Azure Data Factory toomove data from Amazon Simple Storage Service (S3).</span></span> <span data-ttu-id="b0129-105">-Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="b0129-105">It builds on hello [Data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with hello copy activity.</span></span>

<span data-ttu-id="b0129-106">Amazon S3 támogatott tooany fogadó adattár adatainak másolhatja.</span><span class="sxs-lookup"><span data-stu-id="b0129-106">You can copy data from Amazon S3 tooany supported sink data store.</span></span> <span data-ttu-id="b0129-107">Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla.</span><span class="sxs-lookup"><span data-stu-id="b0129-107">For a list of data stores supported as sinks by hello copy activity, see hello [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) table.</span></span> <span data-ttu-id="b0129-108">Adat-előállító jelenleg támogatja a mozgóátlag adatok kizárólag az Amazon S3 tooother adattárolókhoz, de tooAmazon S3 adatok nem áthelyezését más adatokat tárolja.</span><span class="sxs-lookup"><span data-stu-id="b0129-108">Data Factory currently supports only moving data from Amazon S3 tooother data stores, but not moving data from other data stores tooAmazon S3.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="b0129-109">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="b0129-109">Required permissions</span></span>
<span data-ttu-id="b0129-110">toocopy adatait Amazon S3, ellenőrizze, hogy rendelkezik a következő engedélyek hello:</span><span class="sxs-lookup"><span data-stu-id="b0129-110">toocopy data from Amazon S3, make sure you have been granted hello following permissions:</span></span>

* <span data-ttu-id="b0129-111">`s3:GetObject`és `s3:GetObjectVersion` Amazon S3 objektum műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="b0129-111">`s3:GetObject` and `s3:GetObjectVersion` for Amazon S3 Object Operations.</span></span>
* <span data-ttu-id="b0129-112">`s3:ListBucket`Amazon S3 gyűjtő műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="b0129-112">`s3:ListBucket` for Amazon S3 Bucket Operations.</span></span> <span data-ttu-id="b0129-113">Hello Data Factory másolása varázsló használata `s3:ListAllMyBuckets` is szükség.</span><span class="sxs-lookup"><span data-stu-id="b0129-113">If you are using hello Data Factory Copy Wizard, `s3:ListAllMyBuckets` is also required.</span></span>

<span data-ttu-id="b0129-114">További információk az Amazon S3 engedélyek hello teljes listája: [megadása engedélyeket egy házirendben](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).</span><span class="sxs-lookup"><span data-stu-id="b0129-114">For details about hello full list of Amazon S3 permissions, see [Specifying Permissions in a Policy](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).</span></span>

## <a name="getting-started"></a><span data-ttu-id="b0129-115">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="b0129-115">Getting started</span></span>
<span data-ttu-id="b0129-116">A másolási tevékenység, amely Amazon S3 forrásból származó adatokat a különböző eszközök vagy API-k használatával helyezi át a feldolgozási sor hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="b0129-116">You can create a pipeline with a copy activity that moves data from an Amazon S3 source by using different tools or APIs.</span></span>

<span data-ttu-id="b0129-117">hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**.</span><span class="sxs-lookup"><span data-stu-id="b0129-117">hello easiest way toocreate a pipeline is toouse hello **Copy Wizard**.</span></span> <span data-ttu-id="b0129-118">Gyors bemutatóért lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="b0129-118">For a quick walkthrough, see [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md).</span></span>

<span data-ttu-id="b0129-119">Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**.</span><span class="sxs-lookup"><span data-stu-id="b0129-119">You can also use hello following tools toocreate a pipeline: **Azure portal**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager template**, **.NET API**, and **REST API**.</span></span> <span data-ttu-id="b0129-120">Részletes útmutatás toocreate a másolási tevékenység során a folyamat, lásd: hello [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="b0129-120">For step-by-step instructions toocreate a pipeline with a copy activity, see hello [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>

<span data-ttu-id="b0129-121">Akár eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:</span><span class="sxs-lookup"><span data-stu-id="b0129-121">Whether you use tools or APIs, you perform hello following steps toocreate a pipeline that moves data from a source data store tooa sink data store:</span></span>

1. <span data-ttu-id="b0129-122">Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b0129-122">Create **linked services** toolink input and output data stores tooyour data factory.</span></span>
2. <span data-ttu-id="b0129-123">Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.</span><span class="sxs-lookup"><span data-stu-id="b0129-123">Create **datasets** toorepresent input and output data for hello copy operation.</span></span>
3. <span data-ttu-id="b0129-124">Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.</span><span class="sxs-lookup"><span data-stu-id="b0129-124">Create a **pipeline** with a copy activity that takes a dataset as an input and a dataset as an output.</span></span>

<span data-ttu-id="b0129-125">Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="b0129-125">When you use hello wizard, JSON definitions for these Data Factory entities (linked services, datasets, and hello pipeline) are automatically created for you.</span></span> <span data-ttu-id="b0129-126">Eszközök vagy API-k (kivéve a .NET API-t) használ, akkor határozzák meg a Data Factory entitások hello JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="b0129-126">When you use tools or APIs (except .NET API), you define these Data Factory entities by using hello JSON format.</span></span> <span data-ttu-id="b0129-127">Az adat-előállító entitások, amelyek az Amazon S3 adattárolóból használt toocopy adatok JSON-definíciók minta, lásd: hello [JSON-példa: adatok másolása az Amazon S3 tooAzure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) című szakaszát.</span><span class="sxs-lookup"><span data-stu-id="b0129-127">For a sample with JSON definitions for Data Factory entities that are used toocopy data from an Amazon S3 data store, see hello [JSON example: Copy data from Amazon S3 tooAzure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) section of this article.</span></span>

> [!NOTE]
> <span data-ttu-id="b0129-128">A másolási tevékenység során támogatott fájl- és tömörítési formátumok kapcsolatos részletekért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span><span class="sxs-lookup"><span data-stu-id="b0129-128">For details about supported file and compression formats for a copy activity, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md).</span></span>

<span data-ttu-id="b0129-129">a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAmazon S3 részleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="b0129-129">hello following sections provide details about JSON properties that are used toodefine Data Factory entities specific tooAmazon S3.</span></span>

## <a name="linked-service-properties"></a><span data-ttu-id="b0129-130">A kapcsolódószolgáltatás-tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="b0129-130">Linked service properties</span></span>
<span data-ttu-id="b0129-131">A társított szolgáltatás adatokat tároló tooa adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b0129-131">A linked service links a data store tooa data factory.</span></span> <span data-ttu-id="b0129-132">Típusú társított szolgáltatás létrehozása **AwsAccessKey** toolink az Amazon S3 adatok tárolásához tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b0129-132">You create a linked service of type **AwsAccessKey** toolink your Amazon S3 data store tooyour data factory.</span></span> <span data-ttu-id="b0129-133">a következő táblázat hello leírását adott JSON-elemek tooAmazon S3 (AwsAccessKey) társított szolgáltatást biztosít.</span><span class="sxs-lookup"><span data-stu-id="b0129-133">hello following table provides description for JSON elements specific tooAmazon S3 (AwsAccessKey) linked service.</span></span>

| <span data-ttu-id="b0129-134">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b0129-134">Property</span></span> | <span data-ttu-id="b0129-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="b0129-135">Description</span></span> | <span data-ttu-id="b0129-136">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b0129-136">Allowed values</span></span> | <span data-ttu-id="b0129-137">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b0129-137">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b0129-138">accessKeyID</span><span class="sxs-lookup"><span data-stu-id="b0129-138">accessKeyID</span></span> |<span data-ttu-id="b0129-139">Hello titkos hívóbetű azonosítója.</span><span class="sxs-lookup"><span data-stu-id="b0129-139">ID of hello secret access key.</span></span> |<span data-ttu-id="b0129-140">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b0129-140">string</span></span> |<span data-ttu-id="b0129-141">Igen</span><span class="sxs-lookup"><span data-stu-id="b0129-141">Yes</span></span> |
| <span data-ttu-id="b0129-142">secretAccessKey</span><span class="sxs-lookup"><span data-stu-id="b0129-142">secretAccessKey</span></span> |<span data-ttu-id="b0129-143">hello titkos hívóbetű magát.</span><span class="sxs-lookup"><span data-stu-id="b0129-143">hello secret access key itself.</span></span> |<span data-ttu-id="b0129-144">Titkosított titkos karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b0129-144">Encrypted secret string</span></span> |<span data-ttu-id="b0129-145">Igen</span><span class="sxs-lookup"><span data-stu-id="b0129-145">Yes</span></span> |

<span data-ttu-id="b0129-146">Például:</span><span class="sxs-lookup"><span data-stu-id="b0129-146">Here is an example:</span></span>

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

## <a name="dataset-properties"></a><span data-ttu-id="b0129-147">Adatkészlet tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b0129-147">Dataset properties</span></span>
<span data-ttu-id="b0129-148">a dataset toorepresent toospecify bemeneti adatai az Azure Blob storage hello típus tulajdonságbeállító hello adatkészlet túl**AmazonS3**.</span><span class="sxs-lookup"><span data-stu-id="b0129-148">toospecify a dataset toorepresent input data in Azure Blob storage, set hello type property of hello dataset too**AmazonS3**.</span></span> <span data-ttu-id="b0129-149">Set hello **linkedServiceName** toohello adatkészletnevet hello az Amazon S3 hello tulajdonságának társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b0129-149">Set hello **linkedServiceName** property of hello dataset toohello name of hello Amazon S3 linked service.</span></span> <span data-ttu-id="b0129-150">Szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: [adatkészletek létrehozása](data-factory-create-datasets.md).</span><span class="sxs-lookup"><span data-stu-id="b0129-150">For a full list of sections and properties available for defining datasets, see [Creating datasets](data-factory-create-datasets.md).</span></span> 

<span data-ttu-id="b0129-151">Például struktúra, a rendelkezésre állás és a házirend hasonlítanak minden adatkészlet esetében (például az SQL-adatbázis, az Azure blob és az Azure tábla).</span><span class="sxs-lookup"><span data-stu-id="b0129-151">Sections such as structure, availability, and policy are similar for all dataset types (such as SQL database, Azure blob, and Azure table).</span></span> <span data-ttu-id="b0129-152">Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti.</span><span class="sxs-lookup"><span data-stu-id="b0129-152">hello **typeProperties** section is different for each type of dataset, and provides information about hello location of hello data in hello data store.</span></span> <span data-ttu-id="b0129-153">Hello **typeProperties** szakasz egy adatkészlet típusú **AmazonS3** következő tulajdonságai hello (amely tartalmazza a hello Amazon S3 dataset) rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="b0129-153">hello **typeProperties** section for a dataset of type **AmazonS3** (which includes hello Amazon S3 dataset) has hello following properties:</span></span>

| <span data-ttu-id="b0129-154">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b0129-154">Property</span></span> | <span data-ttu-id="b0129-155">Leírás</span><span class="sxs-lookup"><span data-stu-id="b0129-155">Description</span></span> | <span data-ttu-id="b0129-156">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b0129-156">Allowed values</span></span> | <span data-ttu-id="b0129-157">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b0129-157">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b0129-158">bucketName</span><span class="sxs-lookup"><span data-stu-id="b0129-158">bucketName</span></span> |<span data-ttu-id="b0129-159">hello S3 gyűjtő neve.</span><span class="sxs-lookup"><span data-stu-id="b0129-159">hello S3 bucket name.</span></span> |<span data-ttu-id="b0129-160">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b0129-160">String</span></span> |<span data-ttu-id="b0129-161">Igen</span><span class="sxs-lookup"><span data-stu-id="b0129-161">Yes</span></span> |
| <span data-ttu-id="b0129-162">kulcs</span><span class="sxs-lookup"><span data-stu-id="b0129-162">key</span></span> |<span data-ttu-id="b0129-163">hello S3 objektum kulcsának.</span><span class="sxs-lookup"><span data-stu-id="b0129-163">hello S3 object key.</span></span> |<span data-ttu-id="b0129-164">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b0129-164">String</span></span> |<span data-ttu-id="b0129-165">Nem</span><span class="sxs-lookup"><span data-stu-id="b0129-165">No</span></span> |
| <span data-ttu-id="b0129-166">előtag</span><span class="sxs-lookup"><span data-stu-id="b0129-166">prefix</span></span> |<span data-ttu-id="b0129-167">Hello S3 Objektumkulcs előtagját.</span><span class="sxs-lookup"><span data-stu-id="b0129-167">Prefix for hello S3 object key.</span></span> <span data-ttu-id="b0129-168">Kiválasztott objektumok, amelynek kulcsait a előtaggal kezdődik.</span><span class="sxs-lookup"><span data-stu-id="b0129-168">Objects whose keys start with this prefix are selected.</span></span> <span data-ttu-id="b0129-169">Érvényes, csak ha kulcsa üres.</span><span class="sxs-lookup"><span data-stu-id="b0129-169">Applies only when key is empty.</span></span> |<span data-ttu-id="b0129-170">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b0129-170">String</span></span> |<span data-ttu-id="b0129-171">Nem</span><span class="sxs-lookup"><span data-stu-id="b0129-171">No</span></span> |
| <span data-ttu-id="b0129-172">Verzió</span><span class="sxs-lookup"><span data-stu-id="b0129-172">version</span></span> |<span data-ttu-id="b0129-173">hello S3 objektum, ha engedélyezve van a S3 versioning hello verziója.</span><span class="sxs-lookup"><span data-stu-id="b0129-173">hello version of hello S3 object, if S3 versioning is enabled.</span></span> |<span data-ttu-id="b0129-174">Karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b0129-174">String</span></span> |<span data-ttu-id="b0129-175">Nem</span><span class="sxs-lookup"><span data-stu-id="b0129-175">No</span></span> |
| <span data-ttu-id="b0129-176">Formátumban</span><span class="sxs-lookup"><span data-stu-id="b0129-176">format</span></span> | <span data-ttu-id="b0129-177">a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**.</span><span class="sxs-lookup"><span data-stu-id="b0129-177">hello following format types are supported: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat**, **ParquetFormat**.</span></span> <span data-ttu-id="b0129-178">Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b0129-178">Set hello **type** property under format tooone of these values.</span></span> <span data-ttu-id="b0129-179">További információkért lásd: hello [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [JSON formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátumban ](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok.</span><span class="sxs-lookup"><span data-stu-id="b0129-179">For more information, see hello [Text format](data-factory-supported-file-and-compression-formats.md#text-format), [JSON format](data-factory-supported-file-and-compression-formats.md#json-format), [Avro format](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc format](data-factory-supported-file-and-compression-formats.md#orc-format), and [Parquet format](data-factory-supported-file-and-compression-formats.md#parquet-format) sections.</span></span> <br><br> <span data-ttu-id="b0129-180">Ha azt szeretné, hogy toocopy fájlokat-fájlalapú tárolók (bináris másolás), skip hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet definíciók között.</span><span class="sxs-lookup"><span data-stu-id="b0129-180">If you want toocopy files as-is between file-based stores (binary copy), skip hello format section in both input and output dataset definitions.</span></span> |<span data-ttu-id="b0129-181">Nem</span><span class="sxs-lookup"><span data-stu-id="b0129-181">No</span></span> | |
| <span data-ttu-id="b0129-182">Tömörítés</span><span class="sxs-lookup"><span data-stu-id="b0129-182">compression</span></span> | <span data-ttu-id="b0129-183">Adja meg a hello típusát és hello adatok tömörítése szintjét.</span><span class="sxs-lookup"><span data-stu-id="b0129-183">Specify hello type and level of compression for hello data.</span></span> <span data-ttu-id="b0129-184">hello támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**.</span><span class="sxs-lookup"><span data-stu-id="b0129-184">hello supported types are: **GZip**, **Deflate**, **BZip2**, and **ZipDeflate**.</span></span> <span data-ttu-id="b0129-185">hello támogatott szintek a következők: **Optimal** és **leggyorsabb**.</span><span class="sxs-lookup"><span data-stu-id="b0129-185">hello supported levels are: **Optimal** and **Fastest**.</span></span> <span data-ttu-id="b0129-186">További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span><span class="sxs-lookup"><span data-stu-id="b0129-186">For more information, see [File and compression formats in Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support).</span></span> |<span data-ttu-id="b0129-187">Nem</span><span class="sxs-lookup"><span data-stu-id="b0129-187">No</span></span> | |


> [!NOTE]
> <span data-ttu-id="b0129-188">**bucketName + kulcs** hello S3 objektum, amelyen gyűjtő S3 objektumok hello legfelső szintű tárolója, és kulcs hello teljes elérési útja toohello S3 objektum hello helyet határoz meg.</span><span class="sxs-lookup"><span data-stu-id="b0129-188">**bucketName + key** specifies hello location of hello S3 object, where bucket is hello root container for S3 objects, and key is hello full path toohello S3 object.</span></span>

### <a name="sample-dataset-with-prefix"></a><span data-ttu-id="b0129-189">Minta-adatkészleteken előtaggal</span><span class="sxs-lookup"><span data-stu-id="b0129-189">Sample dataset with prefix</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "testbucket",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
### <a name="sample-dataset-with-version"></a><span data-ttu-id="b0129-190">Minta-adatkészleteken (verziójával)</span><span class="sxs-lookup"><span data-stu-id="b0129-190">Sample dataset (with version)</span></span>

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "testbucket",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="dynamic-paths-for-s3"></a><span data-ttu-id="b0129-191">S3 dinamikus útvonalak</span><span class="sxs-lookup"><span data-stu-id="b0129-191">Dynamic paths for S3</span></span>
<span data-ttu-id="b0129-192">hello előző példa értékeket használ a rögzített hello **kulcs** és **bucketName** hello Amazon S3 adatkészlet tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="b0129-192">hello preceding sample uses fixed values for hello **key** and **bucketName** properties in hello Amazon S3 dataset.</span></span>

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

<span data-ttu-id="b0129-193">Adat-előállító kiszámításához ezeket a tulajdonságokat, dinamikusan futásidőben, például a SliceStart rendszerváltozók használatával lehet.</span><span class="sxs-lookup"><span data-stu-id="b0129-193">You can have Data Factory calculate these properties dynamically at runtime, by using system variables such as SliceStart.</span></span>

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

<span data-ttu-id="b0129-194">Mindent hello azonos hello **előtag** az Amazon S3 adatkészlet tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="b0129-194">You can do hello same for hello **prefix** property of an Amazon S3 dataset.</span></span> <span data-ttu-id="b0129-195">Támogatott funkciók és változók listájáért lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md).</span><span class="sxs-lookup"><span data-stu-id="b0129-195">For a list of supported functions and variables, see [Data Factory functions and system variables](data-factory-functions-variables.md).</span></span>

## <a name="copy-activity-properties"></a><span data-ttu-id="b0129-196">Másolási tevékenység tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b0129-196">Copy activity properties</span></span>
<span data-ttu-id="b0129-197">Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: [folyamatok létrehozása](data-factory-create-pipelines.md).</span><span class="sxs-lookup"><span data-stu-id="b0129-197">For a full list of sections and properties available for defining activities, see [Creating pipelines](data-factory-create-pipelines.md).</span></span> <span data-ttu-id="b0129-198">Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b0129-198">Properties such as name, description, input and output tables, and policies are available for all types of activities.</span></span> <span data-ttu-id="b0129-199">Tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek.</span><span class="sxs-lookup"><span data-stu-id="b0129-199">Properties available in hello **typeProperties** section of hello activity vary with each activity type.</span></span> <span data-ttu-id="b0129-200">Hello másolási tevékenységhez tulajdonságok hello típusú források és mosdók függenek.</span><span class="sxs-lookup"><span data-stu-id="b0129-200">For hello copy activity, properties vary depending on hello types of sources and sinks.</span></span> <span data-ttu-id="b0129-201">Ha egy forrást a hello másolási tevékenység típusa nem **FileSystemSource** (amely tartalmazza az Amazon S3), a következő tulajdonság hello érhető el **typeProperties** szakasz:</span><span class="sxs-lookup"><span data-stu-id="b0129-201">When a source in hello copy activity is of type **FileSystemSource** (which includes Amazon S3), hello following property is available in **typeProperties** section:</span></span>

| <span data-ttu-id="b0129-202">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="b0129-202">Property</span></span> | <span data-ttu-id="b0129-203">Leírás</span><span class="sxs-lookup"><span data-stu-id="b0129-203">Description</span></span> | <span data-ttu-id="b0129-204">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="b0129-204">Allowed values</span></span> | <span data-ttu-id="b0129-205">Szükséges</span><span class="sxs-lookup"><span data-stu-id="b0129-205">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="b0129-206">Rekurzív</span><span class="sxs-lookup"><span data-stu-id="b0129-206">recursive</span></span> |<span data-ttu-id="b0129-207">Meghatározza, hogy toorecursively lista S3 objektumokat hello könyvtára alatt tárolja.</span><span class="sxs-lookup"><span data-stu-id="b0129-207">Specifies whether toorecursively list S3 objects under hello directory.</span></span> |<span data-ttu-id="b0129-208">Igaz/hamis</span><span class="sxs-lookup"><span data-stu-id="b0129-208">true/false</span></span> |<span data-ttu-id="b0129-209">Nem</span><span class="sxs-lookup"><span data-stu-id="b0129-209">No</span></span> |

## <a name="json-example-copy-data-from-amazon-s3-tooazure-blob-storage"></a><span data-ttu-id="b0129-210">JSON-példa: adatok másolása az Amazon S3 tooAzure Blob-tároló</span><span class="sxs-lookup"><span data-stu-id="b0129-210">JSON example: Copy data from Amazon S3 tooAzure Blob storage</span></span>
<span data-ttu-id="b0129-211">Ez a példa bemutatja, hogyan Amazon S3 tooan Azure Blob Storage tárolóban toocopy adatait.</span><span class="sxs-lookup"><span data-stu-id="b0129-211">This sample shows how toocopy data from Amazon S3 tooan Azure Blob storage.</span></span> <span data-ttu-id="b0129-212">Azonban adatok átmásolhatók közvetlenül túl[bármely támogatott hello nyelő](data-factory-data-movement-activities.md#supported-data-stores-and-formats) hello másolási tevékenység során a Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="b0129-212">However, data can be copied directly too[any of hello sinks that are supported](data-factory-data-movement-activities.md#supported-data-stores-and-formats) by using hello copy activity in Data Factory.</span></span>

<span data-ttu-id="b0129-213">hello minta biztosít a következő adat-előállító entitások hello JSON jelentésdefiníciókat.</span><span class="sxs-lookup"><span data-stu-id="b0129-213">hello sample provides JSON definitions for hello following Data Factory entities.</span></span> <span data-ttu-id="b0129-214">A definíciók toocreate egy folyamat toocopy adatok Amazon S3 tooBlob tárolásból hello segítségével is használhatók [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="b0129-214">You can use these definitions toocreate a pipeline toocopy data from Amazon S3 tooBlob storage, by using hello [Azure portal](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), or [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).</span></span>   

* <span data-ttu-id="b0129-215">A társított szolgáltatás típusa [AwsAccessKey](#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b0129-215">A linked service of type [AwsAccessKey](#linked-service-properties).</span></span>
* <span data-ttu-id="b0129-216">A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span><span class="sxs-lookup"><span data-stu-id="b0129-216">A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).</span></span>
* <span data-ttu-id="b0129-217">Bemeneti [dataset](data-factory-create-datasets.md) típusú [AmazonS3](#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b0129-217">An input [dataset](data-factory-create-datasets.md) of type [AmazonS3](#dataset-properties).</span></span>
* <span data-ttu-id="b0129-218">Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span><span class="sxs-lookup"><span data-stu-id="b0129-218">An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).</span></span>
* <span data-ttu-id="b0129-219">A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span><span class="sxs-lookup"><span data-stu-id="b0129-219">A [pipeline](data-factory-create-pipelines.md) with copy activity that uses [FileSystemSource](#copy-activity-properties) and [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).</span></span>

<span data-ttu-id="b0129-220">hello minta másol adatokat az Azure blob Amazon S3 tooan óránként.</span><span class="sxs-lookup"><span data-stu-id="b0129-220">hello sample copies data from Amazon S3 tooan Azure blob every hour.</span></span> <span data-ttu-id="b0129-221">Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.</span><span class="sxs-lookup"><span data-stu-id="b0129-221">hello JSON properties used in these samples are described in sections following hello samples.</span></span>

### <a name="amazon-s3-linked-service"></a><span data-ttu-id="b0129-222">Amazon S3 társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b0129-222">Amazon S3 linked service</span></span>

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a><span data-ttu-id="b0129-223">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b0129-223">Azure Storage linked service</span></span>

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

### <a name="amazon-s3-input-dataset"></a><span data-ttu-id="b0129-224">Amazon S3 bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b0129-224">Amazon S3 input dataset</span></span>

<span data-ttu-id="b0129-225">Beállítás **"external": true** hello Data Factory szolgáltatásnak arról tájékoztatja, hogy hello dataset külső toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b0129-225">Setting **"external": true** informs hello Data Factory service that hello dataset is external toohello data factory.</span></span> <span data-ttu-id="b0129-226">Ez a tulajdonság tootrue be egy bemeneti adatkészlet nem hello feldolgozási soros tevékenység által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="b0129-226">Set this property tootrue on an input dataset that is not produced by an activity in hello pipeline.</span></span>

```json
    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }
```


### <a name="azure-blob-output-dataset"></a><span data-ttu-id="b0129-227">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b0129-227">Azure Blob output dataset</span></span>

<span data-ttu-id="b0129-228">Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1).</span><span class="sxs-lookup"><span data-stu-id="b0129-228">Data is written tooa new blob every hour (frequency: hour, interval: 1).</span></span> <span data-ttu-id="b0129-229">hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján.</span><span class="sxs-lookup"><span data-stu-id="b0129-229">hello folder path for hello blob is dynamically evaluated based on hello start time of hello slice that is being processed.</span></span> <span data-ttu-id="b0129-230">hello mappa elérési útját használja hello év, hónap, nap és óra részei hello kezdési ideje.</span><span class="sxs-lookup"><span data-stu-id="b0129-230">hello folder path uses hello year, month, day, and hours parts of hello start time.</span></span>

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
            "partitionedBy": [
                {
                    "name": "Year",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "yyyy"
                    }
                },
                {
                    "name": "Month",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "MM"
                    }
                },
                {
                    "name": "Day",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "dd"
                    }
                },
                {
                    "name": "Hour",
                    "value": {
                        "type": "DateTime",
                        "date": "SliceStart",
                        "format": "HH"
                    }
                }
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a><span data-ttu-id="b0129-231">Másolási tevékenység az Amazon S3 és egy blob fogadó egy folyamaton belül</span><span class="sxs-lookup"><span data-stu-id="b0129-231">Copy activity in a pipeline with an Amazon S3 source and a blob sink</span></span>

<span data-ttu-id="b0129-232">hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek, és ütemezett toorun óránként.</span><span class="sxs-lookup"><span data-stu-id="b0129-232">hello pipeline contains a copy activity that is configured toouse hello input and output datasets, and is scheduled toorun every hour.</span></span> <span data-ttu-id="b0129-233">Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**FileSystemSource**, és **fogadó** típusuk értéke túl**BlobSink**.</span><span class="sxs-lookup"><span data-stu-id="b0129-233">In hello pipeline JSON definition, hello **source** type is set too**FileSystemSource**, and **sink** type is set too**BlobSink**.</span></span>

```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource",
                        "recursive": true
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonS3InputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonS3ToBlob"
            }
        ],
        "start": "2014-08-08T18:00:00Z",
        "end": "2014-08-08T19:00:00Z"
    }
}
```
> [!NOTE]
> <span data-ttu-id="b0129-234">a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat lásd [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).</span><span class="sxs-lookup"><span data-stu-id="b0129-234">toomap columns from a source dataset toocolumns from a sink dataset, see [Mapping dataset columns in Azure Data Factory](data-factory-map-columns.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="b0129-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b0129-235">Next steps</span></span>
<span data-ttu-id="b0129-236">Tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="b0129-236">See hello following articles:</span></span>

* <span data-ttu-id="b0129-237">toolearn kulccsal kapcsolatos tényezők adatátvitelt jelölik a (másolási tevékenység) az adat-előállítót, és különböző módokon toooptimize hatás teljesítmény, lásd: hello [másolása tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md).</span><span class="sxs-lookup"><span data-stu-id="b0129-237">toolearn about key factors that impact performance of data movement (copy activity) in Data Factory, and various ways toooptimize it, see hello [Copy activity performance and tuning guide](data-factory-copy-activity-performance.md).</span></span>

* <span data-ttu-id="b0129-238">A másolási tevékenység során a folyamat létrehozása részletes ismertetését lásd: hello [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="b0129-238">For step-by-step instructions for creating a pipeline with a copy activity, see hello [Copy activity tutorial](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
