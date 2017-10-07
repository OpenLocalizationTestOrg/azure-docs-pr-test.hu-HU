---
title: "aaaAdd hibatűrést az Azure Data Factory másolási tevékenység nem kompatibilis sorok átugrásával |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd hibatűrés az Azure Data Factory másolási tevékenység során példány nem kompatibilis sorok átugrásával"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jingwang
ms.openlocfilehash: e7cf6117655910844b292d340674d8d631450a81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a><span data-ttu-id="d9042-103">A hibatűrés hozzáadása a másolási tevékenység nem kompatibilis sorok kihagyása</span><span class="sxs-lookup"><span data-stu-id="d9042-103">Add fault tolerance in Copy Activity by skipping incompatible rows</span></span>

<span data-ttu-id="d9042-104">Az Azure Data Factory [másolási tevékenység](data-factory-data-movement-activities.md) kínálja kétféleképpen toohandle nem kompatibilis sorokat forrás és a fogadó adattárolók között adatok másolásakor:</span><span class="sxs-lookup"><span data-stu-id="d9042-104">Azure Data Factory [Copy Activity](data-factory-data-movement-activities.md) offers you two ways toohandle incompatible rows when copying data between source and sink data stores:</span></span>

- <span data-ttu-id="d9042-105">Megszakítja, és sikertelen hello másolási tevékenység nem kompatibilis adat esetén történt (alapértelmezés).</span><span class="sxs-lookup"><span data-stu-id="d9042-105">You can abort and fail hello copy activity when incompatible data is encountered (default behavior).</span></span>
- <span data-ttu-id="d9042-106">Folytatás toocopy hello adatok hozzáadásával a hibatűrést, és nem kompatibilis adat sorok kihagyása.</span><span class="sxs-lookup"><span data-stu-id="d9042-106">You can continue toocopy all of hello data by adding fault tolerance and skipping incompatible data rows.</span></span> <span data-ttu-id="d9042-107">Ezenkívül bejelentkezhet hello nem kompatibilis sorok Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="d9042-107">In addition, you can log hello incompatible rows in Azure Blob storage.</span></span> <span data-ttu-id="d9042-108">Majd keresse meg a hello napló toolearn hello hello hiba okát, javítsa ki a hello adatok hello az adatforrás és ismételje meg a hello másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="d9042-108">You can then examine hello log toolearn hello cause for hello failure, fix hello data on hello data source, and retry hello copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="d9042-109">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="d9042-109">Supported scenarios</span></span>
<span data-ttu-id="d9042-110">Másolási tevékenység észlelésekor, kihagyása és naplózási adatok nem kompatibilis három forgatókönyveket teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="d9042-110">Copy Activity supports three scenarios for detecting, skipping, and logging incompatible data:</span></span>

- <span data-ttu-id="d9042-111">**Kompatibilitási hello forrás adattípus és hello fogadó natív típusa között**</span><span class="sxs-lookup"><span data-stu-id="d9042-111">**Incompatibility between hello source data type and hello sink native type**</span></span>

    <span data-ttu-id="d9042-112">Például: adatok másolása CSV-fájlból a Blob storage tooa SQL-adatbázis egy séma-definícióval, amely tartalmazza a három **INT** típusú oszlop.</span><span class="sxs-lookup"><span data-stu-id="d9042-112">For example: Copy data from a CSV file in Blob storage tooa SQL database with a schema definition that contains three **INT** type columns.</span></span> <span data-ttu-id="d9042-113">például numerikus adatokat tartalmazó CSV-fájl sorok hello `123,456,789` sikeresen másolta toohello fogadó tároló.</span><span class="sxs-lookup"><span data-stu-id="d9042-113">hello CSV file rows that contain numeric data, such as `123,456,789` are copied successfully toohello sink store.</span></span> <span data-ttu-id="d9042-114">Például a nem numerikus értékeket tartalmazó sorok azonban hello `123,456,abc` észlelhetők a nem kompatibilis, és kimarad.</span><span class="sxs-lookup"><span data-stu-id="d9042-114">However, hello rows that contain non-numeric values, such as `123,456,abc` are detected as incompatible and are skipped.</span></span>

- <span data-ttu-id="d9042-115">**Hello hello forrás- és fogadó hello között oszlopok száma nem egyezik**</span><span class="sxs-lookup"><span data-stu-id="d9042-115">**Mismatch in hello number of columns between hello source and hello sink**</span></span>

    <span data-ttu-id="d9042-116">Például: adatok másolása CSV-fájlból a Blob storage tooa SQL adatbázis-egy séma-definícióval, amely hat oszlopokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="d9042-116">For example: Copy data from a CSV file in Blob storage tooa SQL database with a schema definition that contains six columns.</span></span> <span data-ttu-id="d9042-117">hello hat oszlopokat tartalmazó sorokat tartalmazó CSV-fájl sikeresen másolta toohello fogadó tároló.</span><span class="sxs-lookup"><span data-stu-id="d9042-117">hello CSV file rows that contain six columns are copied successfully toohello sink store.</span></span> <span data-ttu-id="d9042-118">hello CSV fájlt tartalmazó sorok több vagy kevesebb, mint hat oszlopok észlelhetők a nem kompatibilis, és kimarad.</span><span class="sxs-lookup"><span data-stu-id="d9042-118">hello CSV file rows that contain more or fewer than six columns are detected as incompatible and are skipped.</span></span>

- <span data-ttu-id="d9042-119">**Elsődleges kulcs megsértése tooa relációs adatbázis írásakor**</span><span class="sxs-lookup"><span data-stu-id="d9042-119">**Primary key violation when writing tooa relational database**</span></span>

    <span data-ttu-id="d9042-120">Például: adatok másolása az SQL server tooa SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="d9042-120">For example: Copy data from a SQL server tooa SQL database.</span></span> <span data-ttu-id="d9042-121">Hello fogadó SQL adatbázis meg van adva egy elsődleges kulcs, de nincs ilyen elsődleges kulcs hello forrás SQL server van definiálva.</span><span class="sxs-lookup"><span data-stu-id="d9042-121">A primary key is defined in hello sink SQL database, but no such primary key is defined in hello source SQL server.</span></span> <span data-ttu-id="d9042-122">Duplikált hello azon sorait, amelyek hello forrás szerepel másolt toohello fogadó nem lehet.</span><span class="sxs-lookup"><span data-stu-id="d9042-122">hello duplicated rows that exist in hello source cannot be copied toohello sink.</span></span> <span data-ttu-id="d9042-123">Másolási tevékenység csak hello adatok első sora hello forrás hello fogadó másolja.</span><span class="sxs-lookup"><span data-stu-id="d9042-123">Copy Activity copies only hello first row of hello source data into hello sink.</span></span> <span data-ttu-id="d9042-124">hello hello ismétlődő elsődleges kulcs értéke a következő forrás sorokat észlelhetők a nem kompatibilis és kimarad.</span><span class="sxs-lookup"><span data-stu-id="d9042-124">hello subsequent source rows that contain hello duplicated primary key value are detected as incompatible and are skipped.</span></span>

## <a name="configuration"></a><span data-ttu-id="d9042-125">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d9042-125">Configuration</span></span>
<span data-ttu-id="d9042-126">hello alábbi példa tartalmazza a JSON definition tooconfigure hello másolási tevékenység nem kompatibilis sorok kihagyása:</span><span class="sxs-lookup"><span data-stu-id="d9042-126">hello following example provides a JSON definition tooconfigure skipping hello incompatible rows in Copy Activity:</span></span>

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },         
    "enableSkipIncompatibleRow": true,           
    "redirectIncompatibleRowSettings": {
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| <span data-ttu-id="d9042-127">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="d9042-127">Property</span></span> | <span data-ttu-id="d9042-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="d9042-128">Description</span></span> | <span data-ttu-id="d9042-129">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="d9042-129">Allowed values</span></span> | <span data-ttu-id="d9042-130">Szükséges</span><span class="sxs-lookup"><span data-stu-id="d9042-130">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="d9042-131">**enableSkipIncompatibleRow**</span><span class="sxs-lookup"><span data-stu-id="d9042-131">**enableSkipIncompatibleRow**</span></span> | <span data-ttu-id="d9042-132">Vagy nem engedélyezheti a nem kompatibilis sorok kihagyása másolása során.</span><span class="sxs-lookup"><span data-stu-id="d9042-132">Enable skipping incompatible rows during copy or not.</span></span> | <span data-ttu-id="d9042-133">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="d9042-133">True</span></span><br/><span data-ttu-id="d9042-134">Hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="d9042-134">False (default)</span></span> | <span data-ttu-id="d9042-135">Nem</span><span class="sxs-lookup"><span data-stu-id="d9042-135">No</span></span> |
| <span data-ttu-id="d9042-136">**redirectIncompatibleRowSettings**</span><span class="sxs-lookup"><span data-stu-id="d9042-136">**redirectIncompatibleRowSettings**</span></span> | <span data-ttu-id="d9042-137">Egy csoportját, amely tulajdonságok megadott, ha azt szeretné, hogy toolog hello nem kompatibilis sorok.</span><span class="sxs-lookup"><span data-stu-id="d9042-137">A group of properties that can be specified when you want toolog hello incompatible rows.</span></span> | &nbsp; | <span data-ttu-id="d9042-138">Nem</span><span class="sxs-lookup"><span data-stu-id="d9042-138">No</span></span> |
| <span data-ttu-id="d9042-139">**linkedServiceName**</span><span class="sxs-lookup"><span data-stu-id="d9042-139">**linkedServiceName**</span></span> | <span data-ttu-id="d9042-140">hello társított szolgáltatásnak Azure Storage toostore hello napló kihagyva hello sorokat tartalmaz-e.</span><span class="sxs-lookup"><span data-stu-id="d9042-140">hello linked service of Azure Storage toostore hello log that contains hello skipped rows.</span></span> | <span data-ttu-id="d9042-141">hello nevét egy [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) vagy [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) társított szolgáltatás, amely a toohello tárolási példányt, amelyet az toouse toostore hello naplófájl.</span><span class="sxs-lookup"><span data-stu-id="d9042-141">hello name of an [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) or [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) linked service, which refers toohello storage instance that you want toouse toostore hello log file.</span></span> | <span data-ttu-id="d9042-142">Nem</span><span class="sxs-lookup"><span data-stu-id="d9042-142">No</span></span> |
| <span data-ttu-id="d9042-143">**elérési út**</span><span class="sxs-lookup"><span data-stu-id="d9042-143">**path**</span></span> | <span data-ttu-id="d9042-144">hello hello tartalmazó hello naplófájl elérési útja kihagyja a sort.</span><span class="sxs-lookup"><span data-stu-id="d9042-144">hello path of hello log file that contains hello skipped rows.</span></span> | <span data-ttu-id="d9042-145">Adja meg a hello Blob. tárolási elérési útja, amelyet az toouse toolog hello inkompatibilis adatokat.</span><span class="sxs-lookup"><span data-stu-id="d9042-145">Specify hello Blob storage path that you want toouse toolog hello incompatible data.</span></span> <span data-ttu-id="d9042-146">Ha nem ad meg egy elérési utat, hello szolgáltatás létrehoz egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="d9042-146">If you do not provide a path, hello service creates a container for you.</span></span> | <span data-ttu-id="d9042-147">Nem</span><span class="sxs-lookup"><span data-stu-id="d9042-147">No</span></span> |

## <a name="monitoring"></a><span data-ttu-id="d9042-148">Figyelés</span><span class="sxs-lookup"><span data-stu-id="d9042-148">Monitoring</span></span>
<span data-ttu-id="d9042-149">Futtatás hello másolási tevékenység befejezése után hello számát kihagyott sorok hello figyelés szakaszban tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="d9042-149">After hello copy activity run completes, you can see hello number of skipped rows in hello monitoring section:</span></span>

![A figyelő nem kompatibilis sorok kihagyva](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

<span data-ttu-id="d9042-151">Ha toolog hello nem kompatibilis sorok konfigurálásához hello naplófájl ezen az elérési úton található: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` hello naplófájlban is hello azon sorait, amelyek ki lettek hagyva és hello hello kompatibilitási okozza.</span><span class="sxs-lookup"><span data-stu-id="d9042-151">If you configure toolog hello incompatible rows, you can find hello log file at this path: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` In hello log file, you can see hello rows that were skipped and hello root cause of hello incompatibility.</span></span>

<span data-ttu-id="d9042-152">Hello fájl hello eredeti adatok és a megfelelő hello hiba jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="d9042-152">Both hello original data and hello corresponding error are logged in hello file.</span></span> <span data-ttu-id="d9042-153">Hello naplótartalmak fájlt például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="d9042-153">An example of hello log file content is as follows:</span></span>
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' tootype 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. hello duplicate key value is (data4).
```

## <a name="next-steps"></a><span data-ttu-id="d9042-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d9042-154">Next steps</span></span>
<span data-ttu-id="d9042-155">További információ az Azure Data Factory másolási tevékenység során toolearn lásd [adatok áthelyezése a másolási tevékenység segítségével](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="d9042-155">toolearn more about Azure Data Factory Copy Activity, see [Move data by using Copy Activity](data-factory-data-movement-activities.md).</span></span>
