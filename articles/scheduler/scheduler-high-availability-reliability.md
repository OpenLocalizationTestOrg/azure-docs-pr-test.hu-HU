---
title: "aaaScheduler magas rendelkezésre állása és megbízhatósága"
description: "A Feladatütemező magas rendelkezésre állású és a megbízhatóság"
services: scheduler
documentationcenter: .NET
author: derek1ee
manager: kevinlam1
editor: 
ms.assetid: 5ec78e60-a9b9-405a-91a8-f010f3872d50
ms.service: scheduler
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/16/2016
ms.author: deli
ms.openlocfilehash: 5c9efb333eb42b393adc5deea657ca99206d425e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scheduler-high-availability-and-reliability"></a>A Feladatütemező magas rendelkezésre állású és a megbízhatóság
## <a name="azure-scheduler-high-availability"></a>Azure Schedulerrel magas rendelkezésre állású
Alapszintű Azure platformon service Azure Scheduler magas rendelkezésre állású és georedundáns szolgáltatás központi telepítése és a földrajzi-területi feladat replikációs.

### <a name="geo-redundant-service-deployment"></a>Georedundáns szolgáltatás központi telepítése
Azure Schedulerrel hello szinte minden földrajzi régióban, amely az Azure-ban ma felhasználói felületén keresztül érhető el. hello listája, amely Azure Scheduler elérhető régiók [az itt felsorolt](https://azure.microsoft.com/regions/#services). Egy adott adatközpont üzemeltetett régióban megjelenítése nem érhető el, ha Azure Scheduler hello feladatátvételi képességének vannak úgy, hogy egy másik adatközpontból hello szolgáltatás áll rendelkezésre.

### <a name="geo-regional-job-replication"></a>Földrajzi-területi feladat replikáció
Nem csak az hello Azure Scheduler előtér-kérelmek, de a saját feladat elérhető egyben georeplikált. Ha kimaradás van egy régió tartozik, az Azure Scheduler átadja a feladatokat, és biztosítja a hello feladat fut egy másik adatközpont hello párosított földrajzi régióban.

Például ha létrehozott egy feladatot a déli középső Régiójában, Azure Scheduler automatikusan replikálja az északi középső Régiójában feladatot. Ha hiba történik a déli középső Régiójában, Azure Scheduler biztosítja a hello feladat fut északi középső Régiójában. 

![][1]

Ennek eredményeképpen Azure Scheduler biztosítja, hogy az adatok marad belül hello szélesebb körű azonos földrajzi régióban egy Azure hiba esetén. Ennek eredményeképpen kell nem másolatot készít a feladat csak tooadd magas rendelkezésre állás – Azure Scheduler automatikusan a feladatok a magas rendelkezésre állású képességeket biztosít.

## <a name="azure-scheduler-reliability"></a>Azure Schedulerrel megbízhatóság
Azure Schedulerrel biztosítja, hogy a saját magas rendelkezésre állású, és időt vesz igénybe egy másik módszert toouser által létrehozott feladatok. Például a feladat alkalmazhatja a HTTP-végponttal, amely nem érhető el. Azure Schedulerrel mindazonáltal megpróbál tooexecute a feladat sikeres legyen, mivel a alternatív beállítások toodeal hibával. Azure Schedulerrel hajtja végre ezt két módon:

### <a name="configurable-retry-policy-via-retrypolicy"></a>"RetryPolicy" via konfigurálható újra házirend
Azure Schedulerrel tooconfigure újrapróbálkozási házirendje lehetővé teszi. Alapértelmezés szerint a feladat sikertelen lesz, ha a Feladatütemező megpróbál hello feladat újra négy alkalommal, 30 másodperces időközönként. Újból konfigurálhatja az újrapróbálkozási házirend toobe szigorúbb (például tízszeresének, 30 másodperces időközönként) vagy lazább (például kétszer, naponta történik.)

Amikor ez segíthet az például létrehozhat egy feladatot, amely hetente egyszer fut, és elindítja a HTTP-végponttal. Ha a feladat futtatásakor néhány órán keresztül hello HTTP-végpont nem működik, kívánt toowait egy további hét hello feladat toorun újra óta sikertelen lesz, még akkor is hello alapértelmezett újrapróbálkozási házirendje. Ezekben az esetekben előfordulhat, hogy újrakonfigurálta hello szokásos újrapróbálkozási házirend tooretry három óránként (például) 30 másodpercenként helyett.

Hogyan tooconfigure újrapróbálkozási házirendje, tekintse meg a túl toolearn[retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Másik végpont Beállíthatóság "errorAction" keresztül
Az Azure Scheduler feladatot a cél-végponthoz hello marad nem érhető el, ha Azure Scheduler visszavált toohello alternatív hibakezelő végpont az újrapróbálkozási házirendje végrehajtása után. Ha egy másik hibakezelés végpontot úgy van beállítva, az Azure Scheduler meghívja. Egy másik végponthoz a saját feladatok hello tapasztalt hibák a magas rendelkezésre állású.

Tegyük fel, az alábbi, ábrán hello Azure Scheduler követi az újrapróbálkozási házirend toohit egy Győri webszolgáltatás-bővítmény. Után hello újrapróbálkozik a sikertelen lesz, ha nincs alternatív ellenőrzi. Majd előre kerül, és elindítja a kérést ugyanaz a hello alternatív toohello újrapróbálkozási házirendet.

![][2]

Vegye figyelembe, hogy ugyanazon újrapróbálkozási házirendje hello tooboth hello eredeti művelet és hello másik hibaműveletet vonatkozik. Az is lehetséges toohave hello alternatív hiba művelet művelettípus kell hello fő művelet művelet típusa. Például hello fő műveletétől. Előfordulhat, hogy egy HTTP-végpont meghívása kell, amíg hello hiba művelet lehet egy tároló várólista, a service bus-üzenetsorba vagy a service bus témakör művelet, amelyet hiba-naplózás.

Hogyan tooconfigure egy másik végpont, tekintse meg a túl toolearn[errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Lásd még:
 [A Scheduler ismertetése](scheduler-intro.md)

 [Az Azure Scheduler alapfogalmai, terminológiája és entitáshierarchiája](scheduler-concepts-terms.md)

 [Az ütemező hello Azure-portálon az első lépéseiben](scheduler-get-started-portal.md)

 [Csomagok és számlázás az Azure Schedulerben](scheduler-plans-billing.md)

 [Hogyan ütemezi a toobuild összetett és speciális ismétlődési és Azure-ütemező](scheduler-advanced-complexity.md)

 [Az Azure Scheduler REST API-jának leírása](https://msdn.microsoft.com/library/mt629143)

 [Az Azure Scheduler PowerShell-parancsmagjainak leírása](scheduler-powershell-reference.md)

 [Azure Scheduler – korlátozások, alapértékek és hibakódok](scheduler-limits-defaults-errors.md)

 [Kimenő hitelesítés az Azure Schedulerben](scheduler-outbound-authentication.md)

[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
