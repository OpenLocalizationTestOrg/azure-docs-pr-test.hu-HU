---
title: "Linux Azure Diagnostics használatával aaaCollect naplók |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan naplózza az Azure Diagnostics toocollect mentése tooset futtató fürtről a Service Fabric Linux az Azure-ban."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: a160d469-8b7d-4560-82dd-8500db34a44a
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/28/2017
ms.author: subramar
ms.openlocfilehash: f61172876e744ea3e361f9ae513254239d6ba27f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="collect-logs-by-using-azure-diagnostics"></a>Naplók gyűjtése az Azure Diagnostics használatával
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
> * [Linux](service-fabric-diagnostics-how-to-setup-lad.md)
> 
> 

Az Azure Service Fabric-fürtöt használ, esetén a jó ötlet toocollect hello jelentkezik minden hello csomópontról egy központi helyen. Hello naplók egy központi helyen teszi, hogy könnyen tooanalyze és elhárítása, hogy a szolgáltatások, az alkalmazás vagy a hello fürt vannak. Egyirányú tooupload és a gyűjtés naplók toouse hello Azure Diagnostics bővítményt, mely feltöltések tooAzure tárolóhely Azure Application Insights vagy az Azure Event Hubs naplózza. Is tároló- és az Event Hubs hello eseményeket olvasni, és helyezze el őket a termék például [Naplóelemzési](../log-analytics/log-analytics-service-fabric.md) vagy egy másik napló elemzési megoldás. [Az Azure Application Insights](https://azure.microsoft.com/services/application-insights/) egy átfogó naplózási keresési és analytics szolgáltatás beépített tartalmaz.

## <a name="log-sources-that-you-might-want-toocollect"></a>Napló adatforrások, hogy érdemes-e toocollect
* **A Service Fabric naplók**: hello platformról keresztül kibocsátott [LTTng](http://lttng.org) és fel kell tölteni a tooyour tárfiók. Naplók lehet működési eseményeit, vagy futásidejű az eseményeket, amelyek hello platform bocsát ki. Ezek a naplók tárolt hello helyen, hogy hello fürtjegyzékben határozza meg. (tooget hello tárfiókadatok, keresse meg a hello címke **AzureTableWinFabETWQueryable** , és keressen **StoreConnectionString**.)
* **Alkalmazásesemények**: a szolgáltatás kódból kibocsátott. Szöveges naplófájlok – például LTTng ír naplózási megoldások is használhatja. További információ a dokumentációban hello LTTng a nyomkövetés az alkalmazás.  

## <a name="deploy-hello-diagnostics-extension"></a>Hello diagnosztika bővítmény telepítése
hello első lépése a naplók gyűjtésére toodeploy hello diagnosztika bővítmény minden egyes hello Service Fabric-fürt hello virtuális gépen. hello diagnosztika bővítmény naplókat gyűjt mindegyik virtuális gépre, és feltölti megadott toohello tárfiók. hello lépéseket hogy használ-e hello Azure-portálon vagy az Azure Resource Manager függően változhat.

toodeploy hello diagnosztika bővítmény toohello virtuális gépek hello fürt fürt létrehozásának részeként beállítása **diagnosztika** túl**a**. Hello fürt létrehozása után nem módosítható a beállítás hello portál használatával.

Ezután konfigurálása Linux Azure Diagnostics (LAD) toocollect hello fájlokat, és elhelyezése a tárfiók. Ez a folyamat ("tölti fel a saját") hello cikkben 3. forgatókönyv szerint kifejtett [használatával LAD toomonitor és diagnosztizálhatja a Linux virtuális gépek](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json). A folyamat lekérdezi toohello hozzáférhet a következő követi. Hello nyomkövetések tooa vizualizálója az Ön által választott feltölthet.

Hello diagnosztika bővítmény Azure Resource Manager használatával is telepítheti. hello folyamata hasonló a Windows és Linux és a Windows-fürtök a dokumentált [hogyan toocollect naplózza az Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md).

Használhatja az Operations Management Suite a [Operations Management Suite Naplóelemzési Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Ez a konfiguráció befejezése után hello LAD ügynök figyelők hello megadott naplófájlokat. Ha egy új sor hozzáfűzött toohello fájl, létrehoz egy, az elküldött toohello tároló megadott syslog bejegyzést.

## <a name="next-steps"></a>Következő lépések
részletesebben toounderstand milyen eseményeket meg kell vizsgálni, amíg a problémákat, tekintse meg [LTTng dokumentáció](http://lttng.org/docs) és [használatával LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

