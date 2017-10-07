---
title: "aaaUsing hello Azure Import/Export szolgáltatás REST API |} Microsoft Docs"
description: "Ismerje meg, ahol toofind erőforrások használatának hello Azure Import/Export szolgáltatás REST API-t, beleértve a mindkét hogyan-tooand referenciaanyag."
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
ms.openlocfilehash: a01c170b1bc9c2b2ce9086d39de78a39fafb2c8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-importexport-service-rest-api"></a><span data-ttu-id="5ae35-103">Hello Azure Import/Export szolgáltatás REST API használatával</span><span class="sxs-lookup"><span data-stu-id="5ae35-103">Using hello Azure Import/Export service REST API</span></span>

<span data-ttu-id="5ae35-104">hello Microsoft Azure Import/Export szolgáltatás közzétesz egy REST API tooenable programozott vezérlő az importálási/exportálási feladatok.</span><span class="sxs-lookup"><span data-stu-id="5ae35-104">hello Microsoft Azure Import/Export service exposes a REST API tooenable programmatic control of import/export jobs.</span></span> <span data-ttu-id="5ae35-105">Használhatja a hello REST API tooperform összes hello importálási/exportálási műveleteket hajthat végre a hello [Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="5ae35-105">You can use hello REST API tooperform all of hello import/export operations that you can perform with hello [Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="5ae35-106">Emellett használhatja hello REST API tooperform bizonyos részletes műveletek, például lekérdezése hello százalékos megvalósításának egy feladatot, amely még nem állnak rendelkezésre a hello klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="5ae35-106">Additionally, you can use hello REST API tooperform certain granular operations, such as querying hello percentage completion of a job, which are not currently available in hello classic portal.</span></span>

<span data-ttu-id="5ae35-107">Lásd: [hello Microsoft Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használatával](storage-import-export-service.md) hello Import/Export szolgáltatás és mutatja be, hogyan toouse klasszikus portál toocreate hello és kezelése az importálási áttekintését és exportálni a feladatokat.</span><span class="sxs-lookup"><span data-stu-id="5ae35-107">See [Using hello Microsoft Azure Import/Export service tooTransfer Data tooBlob Storage](storage-import-export-service.md) for an overview of hello Import/Export service and a tutorial that demonstrates how toouse hello classic portal toocreate and manage import and export jobs.</span></span>

## <a name="service-endpoints"></a><span data-ttu-id="5ae35-108">Szolgáltatás-végpontok</span><span class="sxs-lookup"><span data-stu-id="5ae35-108">Service endpoints</span></span>

<span data-ttu-id="5ae35-109">hello Azure Import/Export szolgáltatás egy erőforrás-szolgáltató az Azure Resource Manager és a REST API-k, a következő importálási/exportálási feladatok kezelésére a HTTPS-végpont hello biztosít:</span><span class="sxs-lookup"><span data-stu-id="5ae35-109">hello Azure Import/Export service is a resource provider for Azure Resource Manager and provides a set of REST APIs at hello following HTTPS endpoint for managing import/export jobs:</span></span>

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a><span data-ttu-id="5ae35-110">Versioning</span><span class="sxs-lookup"><span data-stu-id="5ae35-110">Versioning</span></span>

<span data-ttu-id="5ae35-111">Kérelmek toohello Import/Export szolgáltatás kell megadnia a hello `api-version` paraméter, és állítsa be az értékét túl`2016-11-01`.</span><span class="sxs-lookup"><span data-stu-id="5ae35-111">Requests toohello Import/Export service must specify hello `api-version` parameter and set its value too`2016-11-01`.</span></span>

## <a name="importexport-service-operations"></a><span data-ttu-id="5ae35-112">Importálási/exportálási szolgáltatási műveletek</span><span class="sxs-lookup"><span data-stu-id="5ae35-112">Import/Export service operations</span></span>

[<span data-ttu-id="5ae35-113">Importálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ae35-113">Creating an import job</span></span>](storage-import-export-creating-an-import-job.md)

[<span data-ttu-id="5ae35-114">Exportálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ae35-114">Creating an export job</span></span>](storage-import-export-creating-an-export-job.md)

[<span data-ttu-id="5ae35-115">Feladat állapotinformációinak lekérése</span><span class="sxs-lookup"><span data-stu-id="5ae35-115">Retrieving state information for a job</span></span>](storage-import-export-retrieving-state-info-for-a-job.md)

[<span data-ttu-id="5ae35-116">Feladatok számbavétele</span><span class="sxs-lookup"><span data-stu-id="5ae35-116">Enumerating jobs</span></span>](storage-import-export-enumerating-jobs.md)

[<span data-ttu-id="5ae35-117">Feladatok megszakítása és törlése</span><span class="sxs-lookup"><span data-stu-id="5ae35-117">Cancelling and deleting jobs</span></span>](storage-import-export-cancelling-and-deleting-jobs.md)

[<span data-ttu-id="5ae35-118">Meghajtó jegyzékfájlokat biztonsági mentése</span><span class="sxs-lookup"><span data-stu-id="5ae35-118">Backing Up drive manifests</span></span>](storage-import-export-backing-up-drive-manifests.md)

[<span data-ttu-id="5ae35-119">Import/Export-feladatok diagnosztizálása és hibajavítása</span><span class="sxs-lookup"><span data-stu-id="5ae35-119">Diagnostics and error recovery for Import/Export jobs</span></span>](storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a><span data-ttu-id="5ae35-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5ae35-120">Next steps</span></span>

* [<span data-ttu-id="5ae35-121">Tárolási importálási/exportálási REST</span><span class="sxs-lookup"><span data-stu-id="5ae35-121">Storage Import/Export REST</span></span>](/rest/api/storageimportexport)
