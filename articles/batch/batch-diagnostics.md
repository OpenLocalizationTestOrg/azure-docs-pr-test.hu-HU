---
title: "diagnosztikai naplózás kötegelt események - Azure aaaEnable |} Microsoft Docs"
description: "Jegyezze fel, és elemzi az Azure Batch-fiók erőforrásokhoz, mint a készletek és a feladatok diagnosztikai naplózási eseményeket."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: e14e611d-12cd-4671-91dc-bc506dc853e5
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9d03303a3e857e9303f40cc6de5c32b5a51d8f8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a>Alkalmazásnapló-események diagnosztikai kipróbálási és kötegelt megoldások monitorozása

Sok Azure-szolgáltatások, a Batch szolgáltatás hello alkalmazásnapló-események az egyes erőforrások során hello hello erőforrás élettartama bocsát ki. Azure Batch diagnosztikai naplók toorecord eseményeinek erőforrásokhoz, mint a készletek és a feladatok engedélyezése, és ezután használja az hello naplók diagnosztikai értékelési és figyelését. Események például a készlet létrehozása, a készlet törlése, a feladat elindítása, feladat elkészült és mások kötegelt diagnosztikai naplók szerepelnek.

> [!NOTE]
> Ez a cikk ismerteti a naplózási események kötegelt fiók erőforrások magukat, nem feladat, és feladatot, kimeneti adatokat. A feladatok és a feladatok hello kimeneti adatait tárolja a részletekért lásd: [Azure Batch megőrizni a feladat- és kimeneti](batch-task-output.md).
> 
> 

## <a name="prerequisites"></a>Előfeltételek
* [Azure Batch-fiók](batch-account-create-portal.md)
* [Azure Storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account)
  
  toopersist kötegelt diagnosztikai naplók, létre kell hoznia egy Azure Storage-fiók adott Azure hello naplók fogja tárolni. Ehhez a tárfiókhoz megadott amikor Ön [diagnosztikai naplózás engedélyezése](#enable-diagnostic-logging) a Batch-fiókhoz. hello naplógyűjtést engedélyezésekor megadott tárfiók nem van hello ugyanaz, mint egy csatolt tárolási fiók hivatkozott tooin hello [alkalmazáscsomagok](batch-application-packages.md) és [tevékenység kimeneti adatmegőrzési](batch-task-output.md) cikkeket.
  
  > [!WARNING]
  > Ön **felszámított** hello adatok az Azure Storage-fiókban tárolt. Ez magában foglalja a cikkben szereplő hello diagnosztikai naplókat. Ezt tartsa szem előtt tervezésekor a [adatmegőrzési jelentkezzen](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).
  > 
  > 

## <a name="enable-diagnostic-logging"></a>Diagnosztikai naplózás engedélyezése
Diagnosztikai naplózás nincs engedélyezve alapértelmezés szerint a Batch-fiókhoz. Diagnosztikai naplózás minden Batch-fiók kívánt toomonitor explicit módon engedélyeznie kell:

[Hogyan diagnosztikai naplók tooenable gyűjteménye](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs)

Azt javasoljuk, hogy olvassa el a teljes hello [áttekintés az Azure diagnosztikai naplók](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) hello által támogatott cikk toogain megismerhesse, nem csak hogyan tooenable naplózási, de hello jelentkezzen kategóriák különböző Azure-szolgáltatásokhoz. Azure Batch például támogatja egy napló kategóriában: **szolgáltatásnaplók**.

## <a name="service-logs"></a>Service naplóit
Azure Batch szolgáltatás naplók tartalmaz, például egy készletet vagy feladat kötegelt erőforrás hello élettartama során hello Azure Batch szolgáltatás által kibocsátott eseményeket. Minden esemény kötegelt által kibocsátott tárolva van megadva hello tárfiók JSON formátumban. Például ez az egy minta hello törzsét **készlet esemény létrehozása**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Minden esemény törzsében található egy .JSON kiterjesztésű fájlt a hello megadott Azure Storage-fiók. Ha azt szeretné, hogy közvetlenül a hello naplók tooaccess, Kezdésként tooreview hello [séma diagnosztikai naplók hello tárfiókban lévő](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account).

## <a name="service-log-events"></a>Szolgáltatás bejelentkezési események
hello Batch szolgáltatás jelenleg a következő szolgáltatás bejelentkezési események hello bocsát ki. Ez a lista nem lehet teljes, mivel további események is hozzá vannak adva ez a cikk utolsó frissítése óta.

| **Szolgáltatás bejelentkezési események** |
| --- |
| [Alkalmazáskészlet létrehozása][pool_create] |
| [Készlet törlése indítása][pool_delete_start] |
| [Teljes készlet törlése][pool_delete_complete] |
| [Készlet átméretezési indítása][pool_resize_start] |
| [Teljes készlet átméretezése][pool_resize_complete] |
| [A feladat indítása][task_start] |
| [A feladat befejezése][task_complete] |
| [A feladat sikertelen][task_fail] |

## <a name="next-steps"></a>Következő lépések
Továbbá toostoring diagnosztikai alkalmazásnapló-események az Azure Storage-fiók, az adatfolyam formájában Batch szolgáltatás bejelentkezési események tooan [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md), és küldje el túl[Azure Naplóelemzés](../log-analytics/log-analytics-overview.md).

* [Az adatfolyam Azure diagnosztikai naplók tooEvent hubok](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  Adatfolyam-kötegelt diagnosztikai események toohello kiválóan méretezhető adatbefogadási szolgáltatás, az Event Hubs. Az Event Hubs fogadására képes több millió esemény / másodperc, amely akkor átalakíthatja és tárolhatja bármilyen valós idejű elemzési szolgáltató használatával.
* [Log Analytics használata az Azure diagnosztikai naplók elemzése](../log-analytics/log-analytics-azure-storage.md)
  
  Elküldeni a diagnosztikai naplók tooLog elemzés, ahol a hello Operations Management Suite (OMS) portál elemezheti őket, vagy a Power bi-ban vagy az Excel elemzés céljából exportálhatja őket.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
