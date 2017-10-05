---
title: "A hibatűrés hozzáadása az Azure Data Factory másolási tevékenység nem kompatibilis sorok kihagyása |} Microsoft Docs"
description: "Megtudhatja, hogyan hibatűrést hozzáadása az Azure Data Factory másolási tevékenység nem kompatibilis sorok kihagyása másolása során"
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
ms.openlocfilehash: e2a108752259d5da3b401666c6bdbaad13b7ea90
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a><span data-ttu-id="10136-103">A hibatűrés hozzáadása a másolási tevékenység nem kompatibilis sorok kihagyása</span><span class="sxs-lookup"><span data-stu-id="10136-103">Add fault tolerance in Copy Activity by skipping incompatible rows</span></span>

<span data-ttu-id="10136-104">Az Azure Data Factory [másolási tevékenység](data-factory-data-movement-activities.md) kétféleképpen Ön szeretné kezelni a forrás és a fogadó adattároló közötti másolás nem kompatibilis sorok:</span><span class="sxs-lookup"><span data-stu-id="10136-104">Azure Data Factory [Copy Activity](data-factory-data-movement-activities.md) offers you two ways to handle incompatible rows when copying data between source and sink data stores:</span></span>

- <span data-ttu-id="10136-105">Megszakítja, és sikertelen lesz a másolási tevékenység nem kompatibilis adat esetén történt (alapértelmezés).</span><span class="sxs-lookup"><span data-stu-id="10136-105">You can abort and fail the copy activity when incompatible data is encountered (default behavior).</span></span>
- <span data-ttu-id="10136-106">Továbbra is másolhatja az összes adat hozzáadásával a hibatűrést, és nem kompatibilis adat sorok kihagyása.</span><span class="sxs-lookup"><span data-stu-id="10136-106">You can continue to copy all of the data by adding fault tolerance and skipping incompatible data rows.</span></span> <span data-ttu-id="10136-107">Ezenkívül bejelentkezhet a nem kompatibilis sorok Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="10136-107">In addition, you can log the incompatible rows in Azure Blob storage.</span></span> <span data-ttu-id="10136-108">Ismerje meg a hiba okát, javítsa ki az adatokat az adatforrás, és ismételje meg a másolási tevékenység napló majd ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="10136-108">You can then examine the log to learn the cause for the failure, fix the data on the data source, and retry the copy activity.</span></span>

## <a name="supported-scenarios"></a><span data-ttu-id="10136-109">Támogatott helyzetek</span><span class="sxs-lookup"><span data-stu-id="10136-109">Supported scenarios</span></span>
<span data-ttu-id="10136-110">Másolási tevékenység észlelésekor, kihagyása és naplózási adatok nem kompatibilis három forgatókönyveket teszi lehetővé:</span><span class="sxs-lookup"><span data-stu-id="10136-110">Copy Activity supports three scenarios for detecting, skipping, and logging incompatible data:</span></span>

- <span data-ttu-id="10136-111">**A forrás adattípus és a fogadó natív típusa között inkompatibilitás**</span><span class="sxs-lookup"><span data-stu-id="10136-111">**Incompatibility between the source data type and the sink native type**</span></span>

    <span data-ttu-id="10136-112">Például: adatok másolása CSV-fájl Blob Storage egy séma-definícióval, amely tartalmazza a három SQL-adatbázis **INT** típusú oszlop.</span><span class="sxs-lookup"><span data-stu-id="10136-112">For example: Copy data from a CSV file in Blob storage to a SQL database with a schema definition that contains three **INT** type columns.</span></span> <span data-ttu-id="10136-113">Például numerikus adatokat tartalmazó CSV-fájl sorok `123,456,789` sikeresen a fogadó tárolóba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="10136-113">The CSV file rows that contain numeric data, such as `123,456,789` are copied successfully to the sink store.</span></span> <span data-ttu-id="10136-114">Azonban a sorokat tartalmazó nem numerikus értékeket, például `123,456,abc` észlelhetők a nem kompatibilis, és kimarad.</span><span class="sxs-lookup"><span data-stu-id="10136-114">However, the rows that contain non-numeric values, such as `123,456,abc` are detected as incompatible and are skipped.</span></span>

- <span data-ttu-id="10136-115">**A forrás- és a fogadó között oszlopok száma nem egyezik**</span><span class="sxs-lookup"><span data-stu-id="10136-115">**Mismatch in the number of columns between the source and the sink**</span></span>

    <span data-ttu-id="10136-116">Például: adatok másolása az a Blob-tároló CSV-fájl az SQL-adatbázis egy séma-definícióval, amely hat oszlopokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="10136-116">For example: Copy data from a CSV file in Blob storage to a SQL database with a schema definition that contains six columns.</span></span> <span data-ttu-id="10136-117">A fogadó tároló hat oszlopokat tartalmazó CSV-fájl sorok sikeresen lesz másolva.</span><span class="sxs-lookup"><span data-stu-id="10136-117">The CSV file rows that contain six columns are copied successfully to the sink store.</span></span> <span data-ttu-id="10136-118">A több vagy kevesebb, mint hat oszlopot tartalmazó CSV-fájl sorok észlelhetők a nem kompatibilis, és kimarad.</span><span class="sxs-lookup"><span data-stu-id="10136-118">The CSV file rows that contain more or fewer than six columns are detected as incompatible and are skipped.</span></span>

- <span data-ttu-id="10136-119">**Elsődleges kulcs megsértése egy relációs adatbázis írásakor**</span><span class="sxs-lookup"><span data-stu-id="10136-119">**Primary key violation when writing to a relational database**</span></span>

    <span data-ttu-id="10136-120">Például: adatok másolása az SQL server az SQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="10136-120">For example: Copy data from a SQL server to a SQL database.</span></span> <span data-ttu-id="10136-121">A fogadó SQL-adatbázis elsődleges kulcs van definiálva, de nincs ilyen elsődleges kulcs van definiálva a forrás SQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="10136-121">A primary key is defined in the sink SQL database, but no such primary key is defined in the source SQL server.</span></span> <span data-ttu-id="10136-122">A duplikált sorokat, amely szerepel a forrás nem lehet másolni a fogadó.</span><span class="sxs-lookup"><span data-stu-id="10136-122">The duplicated rows that exist in the source cannot be copied to the sink.</span></span> <span data-ttu-id="10136-123">Másolási tevékenység során az adatok csak az első sor a fogadó másolja.</span><span class="sxs-lookup"><span data-stu-id="10136-123">Copy Activity copies only the first row of the source data into the sink.</span></span> <span data-ttu-id="10136-124">Ismétlődő elsődleges kulcs értéke a következő adatforrás a sorokat a rendszer észleli a rendszer nem kompatibilis, és kimarad a.</span><span class="sxs-lookup"><span data-stu-id="10136-124">The subsequent source rows that contain the duplicated primary key value are detected as incompatible and are skipped.</span></span>

## <a name="configuration"></a><span data-ttu-id="10136-125">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="10136-125">Configuration</span></span>
<span data-ttu-id="10136-126">A következő példa egy JSON-definícióból, a másolási tevékenység nem kompatibilis sorok kihagyása konfigurálása biztosítja:</span><span class="sxs-lookup"><span data-stu-id="10136-126">The following example provides a JSON definition to configure skipping the incompatible rows in Copy Activity:</span></span>

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

| <span data-ttu-id="10136-127">Tulajdonság</span><span class="sxs-lookup"><span data-stu-id="10136-127">Property</span></span> | <span data-ttu-id="10136-128">Leírás</span><span class="sxs-lookup"><span data-stu-id="10136-128">Description</span></span> | <span data-ttu-id="10136-129">Megengedett értékek</span><span class="sxs-lookup"><span data-stu-id="10136-129">Allowed values</span></span> | <span data-ttu-id="10136-130">Szükséges</span><span class="sxs-lookup"><span data-stu-id="10136-130">Required</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="10136-131">**enableSkipIncompatibleRow**</span><span class="sxs-lookup"><span data-stu-id="10136-131">**enableSkipIncompatibleRow**</span></span> | <span data-ttu-id="10136-132">Vagy nem engedélyezheti a nem kompatibilis sorok kihagyása másolása során.</span><span class="sxs-lookup"><span data-stu-id="10136-132">Enable skipping incompatible rows during copy or not.</span></span> | <span data-ttu-id="10136-133">True (Igaz)</span><span class="sxs-lookup"><span data-stu-id="10136-133">True</span></span><br/><span data-ttu-id="10136-134">Hamis (alapértelmezés)</span><span class="sxs-lookup"><span data-stu-id="10136-134">False (default)</span></span> | <span data-ttu-id="10136-135">Nem</span><span class="sxs-lookup"><span data-stu-id="10136-135">No</span></span> |
| <span data-ttu-id="10136-136">**redirectIncompatibleRowSettings**</span><span class="sxs-lookup"><span data-stu-id="10136-136">**redirectIncompatibleRowSettings**</span></span> | <span data-ttu-id="10136-137">Egy csoport, amely tulajdonságok meg, ha azt szeretné, a nem kompatibilis sorok bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="10136-137">A group of properties that can be specified when you want to log the incompatible rows.</span></span> | &nbsp; | <span data-ttu-id="10136-138">Nem</span><span class="sxs-lookup"><span data-stu-id="10136-138">No</span></span> |
| <span data-ttu-id="10136-139">**linkedServiceName**</span><span class="sxs-lookup"><span data-stu-id="10136-139">**linkedServiceName**</span></span> | <span data-ttu-id="10136-140">A napló, a rendszer kihagyta sorokat tartalmazó tárolásához Azure Storage társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="10136-140">The linked service of Azure Storage to store the log that contains the skipped rows.</span></span> | <span data-ttu-id="10136-141">A neve egy [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) vagy [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) társított szolgáltatás, amely a tárolási példányon, amely a naplófájl tárolási használni kívánt vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="10136-141">The name of an [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) or [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) linked service, which refers to the storage instance that you want to use to store the log file.</span></span> | <span data-ttu-id="10136-142">Nem</span><span class="sxs-lookup"><span data-stu-id="10136-142">No</span></span> |
| <span data-ttu-id="10136-143">**elérési út**</span><span class="sxs-lookup"><span data-stu-id="10136-143">**path**</span></span> | <span data-ttu-id="10136-144">A rendszer kihagyta sorokat tartalmaz-e a naplófájl elérési útja</span><span class="sxs-lookup"><span data-stu-id="10136-144">The path of the log file that contains the skipped rows.</span></span> | <span data-ttu-id="10136-145">Adja meg a Blob storage elérési utat, amely a nem kompatibilis adatokat naplózhatnak használni kívánt.</span><span class="sxs-lookup"><span data-stu-id="10136-145">Specify the Blob storage path that you want to use to log the incompatible data.</span></span> <span data-ttu-id="10136-146">Ha nem ad meg egy elérési utat, a szolgáltatás létrehoz egy tárolót.</span><span class="sxs-lookup"><span data-stu-id="10136-146">If you do not provide a path, the service creates a container for you.</span></span> | <span data-ttu-id="10136-147">Nem</span><span class="sxs-lookup"><span data-stu-id="10136-147">No</span></span> |

## <a name="monitoring"></a><span data-ttu-id="10136-148">Figyelés</span><span class="sxs-lookup"><span data-stu-id="10136-148">Monitoring</span></span>
<span data-ttu-id="10136-149">A másolási tevékenység során futtatása után megtekintheti a figyelési szakaszban kihagyott sorok száma:</span><span class="sxs-lookup"><span data-stu-id="10136-149">After the copy activity run completes, you can see the number of skipped rows in the monitoring section:</span></span>

![A figyelő nem kompatibilis sorok kihagyva](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

<span data-ttu-id="10136-151">Ha a nem kompatibilis sorok bejelentkezni konfigurálja, a naplófájl ezen az elérési úton található: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` , amelyek ki lettek hagyva a sorok és a kompatibilitási okának láthatja a naplófájlban.</span><span class="sxs-lookup"><span data-stu-id="10136-151">If you configure to log the incompatible rows, you can find the log file at this path: `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` In the log file, you can see the rows that were skipped and the root cause of the incompatibility.</span></span>

<span data-ttu-id="10136-152">A fájl az eredeti adatok és a hiba jelentkezett be.</span><span class="sxs-lookup"><span data-stu-id="10136-152">Both the original data and the corresponding error are logged in the file.</span></span> <span data-ttu-id="10136-153">A napló fájl tartalma például a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="10136-153">An example of the log file content is as follows:</span></span>
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a><span data-ttu-id="10136-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10136-154">Next steps</span></span>
<span data-ttu-id="10136-155">Azure Data Factory másolási tevékenység kapcsolatos további információkért lásd: [adatok áthelyezése a másolási tevékenység segítségével](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="10136-155">To learn more about Azure Data Factory Copy Activity, see [Move data by using Copy Activity](data-factory-data-movement-activities.md).</span></span>