---
title: "Az Azure Import/Export szolgáltatás REST API használatával |} Microsoft Docs"
description: "Ismerje meg, hol találhatók erőforrások az Azure Import/Export szolgáltatás REST API-t, beleértve az útmutató és a referenciaanyagot használatával."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 233f80e9-2e7f-48e0-9639-5c7785e7d743
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: b780385ad0af34bcb15639683d1aa5d689b38b50
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-azure-importexport-service-rest-api"></a><span data-ttu-id="0bcfa-103">Az Azure Import/Export szolgáltatás REST API-jának használata</span><span class="sxs-lookup"><span data-stu-id="0bcfa-103">Using the Azure Import/Export service REST API</span></span>

<span data-ttu-id="0bcfa-104">A Microsoft Azure Import/Export szolgáltatás közzétesz egy REST API, hogy az importálási/exportálási feladatok programozott hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="0bcfa-104">The Microsoft Azure Import/Export service exposes a REST API to enable programmatic control of import/export jobs.</span></span> <span data-ttu-id="0bcfa-105">A REST API segítségével elvégzik az importálási/exportálási műveleteket hajthat végre a a [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="0bcfa-105">You can use the REST API to perform all of the import/export operations that you can perform with the [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="0bcfa-106">Emellett a REST API használatával bizonyos a részletes műveletek, például egy feladatot, amely már nem érhető el a klasszikus Azure portálon százalékos megvalósításának lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="0bcfa-106">Additionally, you can use the REST API to perform certain granular operations, such as querying the percentage completion of a job, which is not currently available in the Azure classic portal.</span></span>

<span data-ttu-id="0bcfa-107">Lásd: [adatok átviteléhez a Blob Storage a Microsoft Azure Import/Export szolgáltatás használata](../storage-import-export-service.md) az Import/Export szolgáltatás és mutatja be a klasszikus portál segítségével történő létrehozása és kezelése az importálás és exportálni a feladatokat áttekintését.</span><span class="sxs-lookup"><span data-stu-id="0bcfa-107">See [Using the Microsoft Azure Import/Export service to Transfer Data to Blob Storage](../storage-import-export-service.md) for an overview of the Import/Export service and a tutorial that demonstrates how to use the classic portal to create and manage import and export jobs.</span></span>

## <a name="service-endpoints"></a><span data-ttu-id="0bcfa-108">Szolgáltatás-végpontok</span><span class="sxs-lookup"><span data-stu-id="0bcfa-108">Service endpoints</span></span>

<span data-ttu-id="0bcfa-109">Az Azure Import/Export szolgáltatás egy erőforrás-szolgáltató az Azure Resource Manager és importálási/exportálási feladatok kezelése a REST API-készlet a következő HTTPS-végpont biztosít:</span><span class="sxs-lookup"><span data-stu-id="0bcfa-109">The Azure Import/Export service is a resource provider for Azure Resource Manager and provides a set of REST APIs at the following HTTPS endpoint for managing import/export jobs:</span></span>

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a><span data-ttu-id="0bcfa-110">Versioning</span><span class="sxs-lookup"><span data-stu-id="0bcfa-110">Versioning</span></span>

<span data-ttu-id="0bcfa-111">Az Import/Export szolgáltatás kéréseknek meg kell határozniuk a `api-version` paraméter, és állítsa be az értékét `2016-11-01`.</span><span class="sxs-lookup"><span data-stu-id="0bcfa-111">Requests to the Import/Export service must specify the `api-version` parameter and set its value to `2016-11-01`.</span></span>

## <a name="importexport-service-operations"></a><span data-ttu-id="0bcfa-112">Importálási/exportálási szolgáltatási műveletek</span><span class="sxs-lookup"><span data-stu-id="0bcfa-112">Import/Export service operations</span></span>

[<span data-ttu-id="0bcfa-113">Importálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0bcfa-113">Creating an import job</span></span>](../storage-import-export-creating-an-import-job.md)

[<span data-ttu-id="0bcfa-114">Exportálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0bcfa-114">Creating an export job</span></span>](../storage-import-export-creating-an-export-job.md)

[<span data-ttu-id="0bcfa-115">Feladat állapotinformációinak lekérése</span><span class="sxs-lookup"><span data-stu-id="0bcfa-115">Retrieving state information for a job</span></span>](storage-import-export-retrieving-state-info-for-a-job.md)

[<span data-ttu-id="0bcfa-116">Feladatok számbavétele</span><span class="sxs-lookup"><span data-stu-id="0bcfa-116">Enumerating jobs</span></span>](../storage-import-export-enumerating-jobs.md)

[<span data-ttu-id="0bcfa-117">Feladatok megszakítása és törlése</span><span class="sxs-lookup"><span data-stu-id="0bcfa-117">Cancelling and deleting jobs</span></span>](storage-import-export-cancelling-and-deleting-jobs.md)

[<span data-ttu-id="0bcfa-118">Meghajtó jegyzékfájlokat biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="0bcfa-118">Backing Up drive manifests</span></span>](../storage-import-export-backing-up-drive-manifests.md)

[<span data-ttu-id="0bcfa-119">Import/Export-feladatok diagnosztizálása és hibajavítása</span><span class="sxs-lookup"><span data-stu-id="0bcfa-119">Diagnostics and error recovery for Import/Export jobs</span></span>](../storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a><span data-ttu-id="0bcfa-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0bcfa-120">Next steps</span></span>

* [<span data-ttu-id="0bcfa-121">Tárolási importálási/exportálási REST</span><span class="sxs-lookup"><span data-stu-id="0bcfa-121">Storage Import/Export REST</span></span>](/rest/api/storageimportexport)
