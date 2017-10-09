---
title: "a figyelő automatikus skálázás közös metrikák aaaAzure |} Microsoft Docs"
description: "Ismerje meg, melyik metrikák gyakran használják az automatikus skálázást a Felhőszolgáltatásokat, a virtuális gépek és a Web Apps."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 189b2a13-01c8-4aca-afd5-90711903ca59
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/6/2016
ms.author: ancav
ms.openlocfilehash: 372a40d72d7a6c22c5ff854b1460ec8a3b7ed1d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor-autoscaling-common-metrics"></a>Az Azure a figyelő automatikus skálázás közös metrikák
Azure figyelő automatikus skálázás lehetővé teszi futó példányok száma tooscale hello felfelé vagy lefelé, telemetriai adatokat (metrikák) alapján. Ez a dokumentum ismerteti, hogy érdemes-e toouse közös metrikákat. Hello Azure-portál a felhőalapú szolgáltatások és a kiszolgálófarmok hello metrikája hello erőforrás tooscale alapján választhatja ki. Azonban is választhat a metrika egy másik erőforráscsoportban tooscale által.

Az Azure a figyelő automatikus skálázás csak túl vonatkozik[virtuálisgép-méretezési csoportok](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Felhőszolgáltatások](https://azure.microsoft.com/services/cloud-services/), és [App Service - webalkalmazások](https://azure.microsoft.com/services/app-service/web/). Más Azure-szolgáltatások különböző méretezési módszer használatára.

## <a name="compute-metrics-for-resource-manager-based-vms"></a>Metrikák Resource Manager-alapú virtuális gépek számítási
Alapértelmezés szerint a Resource Manager-alapú virtuális gépek és virtuálisgép-méretezési csoportok létrehozása (gazdaszintű) alapvető metrikákat. Emellett egy Azure virtuális gép és VMSS diagnosztikai adatok gyűjtésének konfigurálásakor hello Azure diagnosztikai bővítmény is megfelelően kibocsát Vendég-operációsrendszer teljesítményszámlálók (gyakran nevezik "Vendég-operációsrendszer-metrikák").  Ezeket a mérési szabályok automatikus skálázás használható.

Használhatja a hello `Get MetricDefinitions` API/PoSH/CLI tooview hello metrikákat a VMSS erőforrás érhető el.

Ha Virtuálisgép-méretezési készlet használata és egy adott metrika felsorolt nem jelenik meg, akkor valószínűleg *le van tiltva* a diagnosztika bővítményben.

Ha egy adott metrika nem folyamatban van a mintában szereplő vagy átvitt: hello gyakorisággal, frissítheti hello diagnosztika konfigurációját.

Ha az előző mindenképpen értéke true, majd tekintse át [használja a Powershellt tooenable Azure Diagnostics Windows rendszerű virtuális gépként](../virtual-machines/windows/ps-extensions-diagnostics.md) PowerShell tooconfigure és a frissítés az Azure Virtuálisgép-diagnosztika bővítmény tooenable hello metrikát. Cikkben is egy minta diagnosztika konfigurációs fájlt.

### <a name="host-metrics-for-resource-manager-based-windows-and-linux-vms"></a>A Resource Manager-alapú Windows és Linux virtuális gépek gazdagép-metrikák
a következő gazdagép szintekhez tartozó metrikákat hello kibocsátott alapértelmezés szerint az Azure virtuális gép és VMSS a Windows és Linux-példányok. A metrikák írják le az Azure virtuális gép, de hello Azure Virtuálisgép-gazda nem hello Vendég virtuális Gépen telepített ügynök keresztül gyűjtött. Ezeket a mérési szabályok automatikus skálázás lehet használni.

- [A Resource Manager-alapú Windows és Linux virtuális gépek gazdagép-metrikák](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)
- [A Resource Manager-alapú Windows és Linux Virtuálisgép-méretezési készlet gazdagép metrikák](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)

### <a name="guest-os-metrics-resource-manager-based-windows-vms"></a>Resource Manager-alapú Windows virtuális gépek vendég operációs rendszer metrikák
Ha a virtuális gép létrehozása az Azure-ban, diagnosztika hello diagnosztika bővítményével engedélyezve van. hello diagnosztika bővítmény belül hello VM vett metrikák készlete bocsát ki. Ez azt jelenti, hogy ki alapértelmezés szerint nem kibocsátott metrikák automatikus skálázás is.

Hello metrikák listája hello a következő parancsot a PowerShell használatával is létrehozhat.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

A következő metrikák hello riasztást hozhat létre:

| Metrika neve | Unit (Egység) |
| --- | --- |
| \Processor(_Total)\% Processor Time |Százalék |
| \Processor(_Total)\% védett módú használatának aránya |Százalék |
| \Processor(_Total)\% felhasználói idő |Százalék |
| \Processor információkat (_Total) \Processor gyakorisága |Darabszám |
| \System\Processes |Darabszám |
| \Process (_Total) \Thread száma |Darabszám |
| \Process (_Total) \Handle száma |Darabszám |
| \Memory\% előjegyzett memória |Százalék |
| \Memory\Available Bytes |Bájtok |
| \Memory\Committed bájt |Bájtok |
| \Memory\Commit korlátot |Bájtok |
| Lapozható \Memory\Pool bájt |Bájtok |
| \Memory\Pool nem lapozható készlet mérete |Bájtok |
| \PhysicalDisk(_Total)\% lemez idő |Százalék |
| \PhysicalDisk(_Total)\% lemez olvasási idő |Százalék |
| \PhysicalDisk(_Total)\% lemezre írási ideje |Százalék |
| \PhysicalDisk (_Total) \Disk átvitel/mp |CountPerSecond |
| Lemezolvasások/mp (%) (_Total) \PhysicalDisk \Disk |CountPerSecond |
| \PhysicalDisk (_Total) \Disk/mp |CountPerSecond |
| \PhysicalDisk (_Total) \Disk bájtok/s |BytesPerSecond |
| Olvasott bájt/mp (%) (_Total) \PhysicalDisk \Disk |BytesPerSecond |
| \PhysicalDisk (_Total) \Disk írt bájt/mp |BytesPerSecond |
| \PhysicalDisk (_Total) \Avg. Lemezvárólista hossza |Darabszám |
| \PhysicalDisk (_Total) \Avg. Olvasási Lemezvárólista hossza |Darabszám |
| \PhysicalDisk (_Total) \Avg. Írási Lemezvárólista hossza |Darabszám |
| \LogicalDisk(_Total)\% szabad területe |Százalék |
| \LogicalDisk (_Total) \Free mérete (MB) |Darabszám |

### <a name="guest-os-metrics-linux-vms"></a>Linux virtuális gépek vendég operációs rendszer metrikák
Ha a virtuális gép létrehozása az Azure-ban, diagnosztika alapértelmezés szerint engedélyezve van diagnosztika bővítmény használatával.

Hello metrikák listája hello a következő parancsot a PowerShell használatával is létrehozhat.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

 A következő metrikák hello riasztást hozhat létre:

| Metrika neve | Unit (Egység) |
| --- | --- |
| \Memory\AvailableMemory |Bájtok |
| \Memory\PercentAvailableMemory |Százalék |
| \Memory\UsedMemory |Bájtok |
| \Memory\PercentUsedMemory |Százalék |
| \Memory\PercentUsedByCache |Százalék |
| \Memory\PagesPerSec |CountPerSecond |
| \Memory\PagesReadPerSec |CountPerSecond |
| \Memory\PagesWrittenPerSec |CountPerSecond |
| \Memory\AvailableSwap |Bájtok |
| \Memory\PercentAvailableSwap |Százalék |
| \Memory\UsedSwap |Bájtok |
| \Memory\PercentUsedSwap |Százalék |
| \Processor\PercentIdleTime |Százalék |
| \Processor\PercentUserTime |Százalék |
| \Processor\PercentNiceTime |Százalék |
| \Processor\PercentPrivilegedTime |Százalék |
| \Processor\PercentInterruptTime |Százalék |
| \Processor\PercentDPCTime |Százalék |
| \Processor\PercentProcessorTime |Százalék |
| \Processor\PercentIOWaitTime |Százalék |
| \PhysicalDisk\BytesPerSecond |BytesPerSecond |
| \PhysicalDisk\ReadBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\WriteBytesPerSecond |BytesPerSecond |
| \PhysicalDisk\TransfersPerSecond |CountPerSecond |
| \PhysicalDisk\ReadsPerSecond |CountPerSecond |
| \PhysicalDisk\WritesPerSecond |CountPerSecond |
| \PhysicalDisk\AverageReadTime |másodperc |
| \PhysicalDisk\AverageWriteTime |másodperc |
| \PhysicalDisk\AverageTransferTime |másodperc |
| \PhysicalDisk\AverageDiskQueueLength |Darabszám |
| \NetworkInterface\BytesTransmitted |Bájtok |
| \NetworkInterface\BytesReceived |Bájtok |
| \NetworkInterface\PacketsTransmitted |Darabszám |
| \NetworkInterface\PacketsReceived |Darabszám |
| \NetworkInterface\BytesTotal |Bájtok |
| \NetworkInterface\TotalRxErrors |Darabszám |
| \NetworkInterface\TotalTxErrors |Darabszám |
| \NetworkInterface\TotalCollisions |Darabszám |

## <a name="commonly-used-web-server-farm-metrics"></a>A gyakran használt (kiszolgálófarm) webes metrikák
Automatikus skálázás például hello Http-várólista hossza közös web server mérőszámok alapján is elvégezheti. Az metrika neve **HttpQueueLength**.  a következő szakasz listák elérhető server farm (webalkalmazások) mérőszámok hello.

### <a name="web-apps-metrics"></a>Webes alkalmazások metrikák
Hello webalkalmazások metrikák listája hello a következő parancsot a PowerShell használatával is létrehozhat.

```
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

Riasztás, vagy skálázhatja által metrikákat.

| Metrika neve | Unit (Egység) |
| --- | --- |
| CpuPercentage |Százalék |
| MemoryPercentage |Százalék |
| DiskQueueLength |Darabszám |
| HttpQueueLength |Darabszám |
| BytesReceived |Bájtok |
| BytesSent |Bájtok |

## <a name="commonly-used-storage-metrics"></a>Tárolási általánosan használt mérőszámait
Tároló várólista hossza, amely hello hello tárolási várólistában lévő üzenetek száma szerint méretezheti. Tároló várólista hossza különleges metrika és hello küszöbértéke hello példányonként üzenetek száma. Például ha két esetben és hello küszöbértéke too100, skálázás esetén hello hello várólistában lévő üzenetek teljes száma 200. Amely lehet egy példány 100 üzenetek, 120 és 80 vagy bármely más kombinációja, amely too200 vagy több ad hozzá.

Ezt a beállítást hello Azure portálra az hello konfigurálása **beállítások** panelen. A Virtuálisgép-méretezési készlet, frissítheti az automatikus skálázási beállítás hello hello Resource Manager sablon toouse *metricName* , *ApproximateMessageCount* , és adja át a hello azonosító hello tároló várólista,  *metricResourceUri*.

Például egy klasszikus Tárfiók hello az automatikus skálázási beállítás metricTrigger a következők:

```
"metricName": "ApproximateMessageCount",
 "metricNamespace": "",
 "metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ClassicStorage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
 ```

(Nem klasszikus) tárfiókot hello metricTrigger ilyen műveletekre:

```
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.Storage/storageAccounts/STORAGE_ACCOUNT_NAME/services/queue/queues/QUEUE_NAME"
```

## <a name="commonly-used-service-bus-metrics"></a>A Service Bus általánosan használt mérőszámait
A Service Bus-várólista hossza, amely hello hello Service Bus várólistában lévő üzenetek száma szerint méretezheti. A Service Bus-várólista hossza különleges metrika és hello küszöbértéke hello példányonként üzenetek száma. Például ha két esetben és hello küszöbértéke too100, skálázás esetén hello hello várólistában lévő üzenetek teljes száma 200. Amely lehet egy példány 100 üzenetek, 120 és 80 vagy bármely más kombinációja, amely too200 vagy több ad hozzá.

A Virtuálisgép-méretezési készlet, frissítheti az automatikus skálázási beállítás hello hello Resource Manager sablon toouse *metricName* , *ApproximateMessageCount* , és adja át a hello azonosító hello tároló várólista,  *metricResourceUri*.

```
"metricName": "MessageCount",
 "metricNamespace": "",
"metricResourceUri": "/subscriptions/SUBSCRIPTION_ID/resourceGroups/RES_GROUP_NAME/providers/Microsoft.ServiceBus/namespaces/SB_NAMESPACE/queues/QUEUE_NAME"
```

> [!NOTE]
> Service Bus hello erőforrás csoport fogalma nem létezik, de Azure Resource Manager régiónként alapértelmezett erőforrás csoportot hoz létre. hello erőforráscsoport általában hello "Default - Szolgáltatásbusz-[régió]" formátumban van. Például: "Alapértelmezett-Szolgáltatásbusz-EastUS", "Alapértelmezett-Szolgáltatásbusz-WestUS", "Alapértelmezett-Szolgáltatásbusz-AustraliaEast" stb.
>
>
