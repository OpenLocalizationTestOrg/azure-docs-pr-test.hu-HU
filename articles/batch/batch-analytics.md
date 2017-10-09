---
title: "Kötegelt elemzés aaaAzure |} Microsoft Docs"
description: "Az Azure Batch Analytics hivatkozását."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 462fbad1ac522b485ae18c1e8891b9d2cabd45e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="batch-analytics"></a>Kötegelt elemzés
Kötegelt elemzés hello témakörei hello események és riasztások érhető el a Batch szolgáltatás erőforrások referencia jellegű információt tartalmaz.

Lásd: [Azure Batch diagnosztikai naplózás](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) engedélyezése és kötegelt diagnosztikai naplók fel további információt.

## <a name="diagnostic-logs"></a>Diagnosztikai naplók

Azure Batch szolgáltatás hello diagnosztikai naplóeseményeket bizonyos kötegelt erőforrások hello élettartama során a következő hello bocsát ki.

**Szolgáltatás bejelentkezési események**
* [Alkalmazáskészlet létrehozása](batch-pool-create-event.md)
* [Készlet törlése indítása](batch-pool-delete-start-event.md)
* [Teljes készlet törlése](batch-pool-delete-complete-event.md)
* [Készlet átméretezési indítása](batch-pool-resize-start-event.md)
* [Teljes készlet átméretezése](batch-pool-resize-complete-event.md)
* [A feladat indítása](batch-task-start-event.md)
* [A feladat befejezése](batch-task-complete-event.md)
* [A feladat sikertelen](batch-task-fail-event.md)