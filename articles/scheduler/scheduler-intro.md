---
title: aaaWhat az Azure Scheduler? | Microsoft Docs
description: "Azure Schedulerrel lehetővé teszi a toodeclaratively műveletek toorun hello felhőben írják le. A rendszer ezt követően a műveletek ütemezését és futtatását automatikusan végzi el."
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 52aa6ae1-4c3d-43fb-81b0-6792c84bcfae
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/18/2016
ms.author: deli
ms.openlocfilehash: 062e25ae473510264dc0038198c05e7ac1e86210
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-scheduler"></a>Mi az Azure Scheduler?
Azure Schedulerrel lehetővé teszi a toodeclaratively műveletek toorun hello felhőben írják le. A rendszer ezt követően a műveletek ütemezését és futtatását automatikusan végzi el.  A Feladatütemező ezt használatával hajtja végre [hello Azure-portálon](scheduler-get-started-portal.md), kód, [REST API](https://msdn.microsoft.com/library/mt629143.aspx), vagy az Azure PowerShell.

A Scheduler elvégzi az ütemezett munkák létrehozását, karbantartását és meghívását.  A Scheduler nem futtat számítási feladatokat vagy kódokat. A szolgáltatás csupán *meghívja* a máshol (az Azure-ban, helyszínen vagy másik szolgáltató által) futtatott kódokat. A meghívás a következőkön keresztül történhet: HTTP, HTTPS, tárolási sor, Service Bus-üzenetsor vagy Service Bus-témakör.

A Feladatütemező ütemezések [feladatok](scheduler-concepts-terms.md), tartja a feladat végrehajtásának eredménye előzményeit, hogy egy tekintse át, és deterministically és megbízhatóan ütemezések munkaterhelések toobe fut-e. Azure WebJobs (Azure App Service Web Apps szolgáltatása hello része) és az egyéb Azure ütemezési szolgáltatása használja a Feladatütemező hello háttérben. Hello [Feladatütemező REST API](https://msdn.microsoft.com/library/mt629143.aspx) segít az ilyen műveletek hello kommunikáció kezelése. Ezért a Scheduler támogatja a [komplex és speciális, ismétlődő ütemezések](scheduler-advanced-complexity.md) egyszerű létrehozását.

Nincsenek több szolgáltatásokat, amelyek alkalmasak a Feladatütemező toohello használatát. Példa:

* *Ismétlődő alkalmazásműveletek:* A Twitteren megjelenő adatok hírcsatornába foglalása.
* *Napi karbantartás*: A naplók naponta történő törlése, biztonsági másolatok készítése és egyéb karbantartási feladatok. Például egy rendszergazda dönthet hello adatbázis tooback 1:00 órakor minden nap hello következő kilenc hónapban.

A Feladatütemező toocreate lehetővé teszi, frissítése, törlése, megtekintése és feladatok kezelése és [gyűjtemények feladat](scheduler-concepts-terms.md) programozott módon, a parancsfájlok segítségével, és hello portálon.

## <a name="see-also"></a>Lásd még:
 [Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája](scheduler-concepts-terms.md)

 [Az ütemező hello Azure-portálon az első lépéseiben](scheduler-get-started-portal.md)

 [Csomagok és számlázás az Azure Schedulerben](scheduler-plans-billing.md)

 [Hogyan ütemezi a toobuild összetett és speciális ismétlődési és Azure-ütemező](scheduler-advanced-complexity.md)

 [Az Azure Scheduler REST API-jának leírása](https://msdn.microsoft.com/library/mt629143)

 [Az Azure Scheduler PowerShell-parancsmagjainak leírása](scheduler-powershell-reference.md)

 [Azure Scheduler – magas fokú rendelkezésre állás és megbízhatóság](scheduler-high-availability-reliability.md)

 [Azure Scheduler – korlátozások, alapértékek és hibakódok](scheduler-limits-defaults-errors.md)

 [Kimenő hitelesítés az Azure Schedulerben](scheduler-outbound-authentication.md)

