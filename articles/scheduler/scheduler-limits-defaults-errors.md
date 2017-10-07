---
title: "aaaScheduler korlátozza, és alapértelmezés szerint használt érték"
description: "A Feladatütemező korlátozásai és alapértelmezett értéke"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 88f4a3e9-6dbd-4943-8543-f0649d423061
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 6fe0600d3ce3249d5aab1b877369b175316b5437
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-limits-and-defaults"></a>A Feladatütemező korlátozásai és alapértelmezett értéke
## <a name="scheduler-quotas-limits-defaults-and-throttles"></a>A Feladatütemező kvóták, korlátok, alapértelmezett és szabályozások
[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="hello-x-ms-request-id-header"></a>az x-ms-request-id hello fejléc
Felé irányuló hello a Feladatütemező szolgáltatás összes kérelmet adja vissza egy nevű válaszfejléc**az x-ms-request-id**. Ezt a fejlécet, amely egyedileg azonosítja a hello kérelem nem átlátszó értéket tartalmaz.

Ha a kérés következetesen futtatása sikertelen, és rendelkezik ellenőrizte, hogy hello kérelem megfelelően formázott van, az érték tooreport hello hiba tooMicrosoft használhatja. A jelentés tartalmazza a hello értékét az x-ms-request-id hello megközelítőleges időpont, amikor adott hello kérés érkezett, hello hello előfizetés, a feladatgyűjtemény és/vagy a feladat azonosítóját, és hello típusú, amely megkísérelte kérelem hello műveletet.

## <a name="see-also"></a>Lásd még:
 [A Scheduler ismertetése](scheduler-intro.md)

 [Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája](scheduler-concepts-terms.md)

 [Az ütemező hello Azure-portálon az első lépéseiben](scheduler-get-started-portal.md)

 [Csomagok és számlázás az Azure Schedulerben](scheduler-plans-billing.md)

 [Az Azure Scheduler REST API-jának leírása](https://msdn.microsoft.com/library/mt629143)

 [Az Azure Scheduler PowerShell-parancsmagjainak leírása](scheduler-powershell-reference.md)

 [Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Kimenő hitelesítés az Azure Schedulerben](scheduler-outbound-authentication.md)

