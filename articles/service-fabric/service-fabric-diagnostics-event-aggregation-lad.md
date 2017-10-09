---
title: "Service Fabric esemény összesítési rendelkező Linux Azure Diagnostics aaaAzure |} Microsoft Docs"
description: "További tudnivalók összesítésére és események gyűjtése LAD figyelési és diagnosztika Azure Service Fabric-fürt segítségével."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: aefa869219a0dd219e01e6574816fe3ce47fe472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-aggregation-and-collection-using-linux-azure-diagnostics"></a>Esemény összevonásának és a gyűjtemény Linux Azure Diagnostics használatával
> [!div class="op_single_selector"]
> * [Windows](service-fabric-diagnostics-event-aggregation-wad.md)
> * [Linux](service-fabric-diagnostics-event-aggregation-lad.md)
>
>

Az Azure Service Fabric-fürtöt használ, esetén a jó ötlet toocollect hello jelentkezik minden hello csomópontról egy központi helyen. Hello naplók rendelkezik egy központi helyen segítségével elemezheti és a fürt, vagy az adott fürtben található futó hello alkalmazások és szolgáltatások a problémák elhárításához.

Egyirányú tooupload és a gyűjtés naplók toouse hello Linux Azure Diagnostics (LAD) bővítmény a naplók tooAzure tárolási feltölti, és hello beállítás toosend naplók tooAzure Application Insights vagy az Event Hubs is rendelkezik. Is használja egy külső folyamat tooread hello eseményeket tárolóból, és helyezze el őket egy analysis platform termék, például a [OMS Naplóelemzési](../log-analytics/log-analytics-service-fabric.md) vagy egy másik napló elemzési megoldás.

## <a name="log-and-event-sources"></a>Naplófájl és esemény források

### <a name="service-fabric-platform-events"></a>A Service Fabric platform események
A Service Fabric bocsát ki néhány out-of-az-box naplók keresztül [LTTng](http://lttng.org), beleértve a működési események vagy futásidejű események. Ezek a naplók, a sablon meghatározza a fürt erőforrás-kezelő hello hello helyen tárolódnak. tooget vagy hello tárfiókadatok megadásához keresse meg a hello címke **AzureTableWinFabETWQueryable** , és keressen **StoreConnectionString**.

### <a name="application-events"></a>Alkalmazás-események
 Ha a szoftver tagolása leírt módon, hogy az alkalmazások és szolgáltatások kódból kibocsátott események. Szöveges naplófájlok – például LTTng ír naplózási megoldások is használhatja. További információ a dokumentációban hello LTTng a nyomkövetés az alkalmazás.

[Figyelése és diagnosztizálása szolgáltatásai a helyi számítógép fejlesztési telepítő](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md).

## <a name="deploy-hello-diagnostics-extension"></a>Hello diagnosztika bővítmény telepítése
hello első lépése a naplók gyűjtésére toodeploy hello diagnosztika bővítmény minden egyes hello Service Fabric-fürt hello virtuális gépen. hello diagnosztika bővítmény naplókat gyűjt mindegyik virtuális gépre, és feltölti megadott toohello tárfiók. hello lépéseket hogy használ-e hello Azure-portálon vagy az Azure Resource Manager függően változhat.

toodeploy hello diagnosztika bővítmény toohello virtuális gépek hello fürt fürt létrehozásának részeként beállítása **diagnosztika** túl**a**. Hello fürt létrehozása után nem módosítható a beállítás hello portál használatával.

Ezután konfigurálása Linux Azure Diagnostics (LAD) toocollect hello fájlokat, és elhelyezése a tárfiók. Ez a folyamat ("tölti fel a saját") hello cikkben 3. forgatókönyv szerint kifejtett [használatával LAD toomonitor és diagnosztizálhatja a Linux virtuális gépek](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json). A folyamat lekérdezi toohello hozzáférhet a következő követi. Hello nyomkövetések tooa vizualizálója az Ön által választott feltölthet.

Hello diagnosztika bővítmény Azure Resource Manager használatával is telepítheti. hello folyamata hasonló a Windows és Linux és a Windows-fürtök a dokumentált [hogyan toocollect naplózza az Azure Diagnostics](service-fabric-diagnostics-how-to-setup-wad.md).

Használhatja az Operations Management Suite a [Operations Management Suite Naplóelemzési Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Ez a konfiguráció befejezése után hello LAD ügynök figyelők hello megadott naplófájlokat. Ha egy új sor hozzáfűzött toohello fájl, létrehoz egy, az elküldött toohello tároló megadott syslog bejegyzést.

## <a name="next-steps"></a>Következő lépések

részletesebben toounderstand milyen eseményeket meg kell vizsgálni, amíg a problémákat, tekintse meg [LTTng dokumentáció](http://lttng.org/docs) és [használatával LAD](../virtual-machines/linux/classic/diagnostic-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).
