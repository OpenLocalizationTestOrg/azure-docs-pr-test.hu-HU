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
# <a name="using-hello-azure-importexport-service-rest-api"></a>Hello Azure Import/Export szolgáltatás REST API használatával

hello Microsoft Azure Import/Export szolgáltatás közzétesz egy REST API tooenable programozott vezérlő az importálási/exportálási feladatok. Használhatja a hello REST API tooperform összes hello importálási/exportálási műveleteket hajthat végre a hello [Azure-portálon](https://portal.azure.com/). Emellett használhatja hello REST API tooperform bizonyos részletes műveletek, például lekérdezése hello százalékos megvalósításának egy feladatot, amely még nem állnak rendelkezésre a hello klasszikus portálon.

Lásd: [hello Microsoft Azure Import/Export szolgáltatás tooTransfer adatok tooBlob Storage használatával](storage-import-export-service.md) hello Import/Export szolgáltatás és mutatja be, hogyan toouse klasszikus portál toocreate hello és kezelése az importálási áttekintését és exportálni a feladatokat.

## <a name="service-endpoints"></a>Szolgáltatás-végpontok

hello Azure Import/Export szolgáltatás egy erőforrás-szolgáltató az Azure Resource Manager és a REST API-k, a következő importálási/exportálási feladatok kezelésére a HTTPS-végpont hello biztosít:

```
https://management.azure.com/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.ImportExport/jobs/<job-name>
```

## <a name="versioning"></a>Versioning

Kérelmek toohello Import/Export szolgáltatás kell megadnia a hello `api-version` paraméter, és állítsa be az értékét túl`2016-11-01`.

## <a name="importexport-service-operations"></a>Importálási/exportálási szolgáltatási műveletek

[Importálási feladat létrehozása](storage-import-export-creating-an-import-job.md)

[Exportálási feladat létrehozása](storage-import-export-creating-an-export-job.md)

[Feladat állapotinformációinak lekérése](storage-import-export-retrieving-state-info-for-a-job.md)

[Feladatok számbavétele](storage-import-export-enumerating-jobs.md)

[Feladatok megszakítása és törlése](storage-import-export-cancelling-and-deleting-jobs.md)

[Meghajtó jegyzékfájlokat biztonsági mentése](storage-import-export-backing-up-drive-manifests.md)

[Import/Export-feladatok diagnosztizálása és hibajavítása](storage-import-export-diagnostics-and-error-recovery.md)

## <a name="next-steps"></a>Következő lépések

* [Tárolási importálási/exportálási REST](/rest/api/storageimportexport)
