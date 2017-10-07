---
title: az Azure Diagnostics aaaOverview |} Microsoft Docs
description: "Az Azure diagnostics használjon hibakeresés, méri a teljesítményt, figyelés, a forgalom elemzése a felhőszolgáltatások, virtuális gépek és a service fabric"
services: multiple
documentationcenter: .net
author: rboucher
manager: 
editor: 
ms.assetid: baad40d8-c915-4f93-b486-8b160bf33463
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/18/2017
ms.author: robb
ms.openlocfilehash: 2a03a96a37091894d7ab16120c125116e4bf462a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-diagnostics"></a>Mi az Azure Diagnostics
Az Azure Diagnostics hello funkció, amely lehetővé teszi a központilag telepített alkalmazás vonatkozó diagnosztikai adatok gyűjtésére hello Azure belül. Számos különböző forrásokból származó hello diagnosztika bővítmény is használhatja. Azure Cloud Service webes és feldolgozói szerepkörök, Microsoft Windows és a Service Fabric rendszert futtató Azure virtuális gépek jelenleg támogatott vannak. Más Azure-szolgáltatásokkal rendelkezik saját külön diagnosztika.

## <a name="data-you-can-collect"></a>Begyűjtheti adatok
Az Azure Diagnostics össze tudják gyűjteni a következő típusú adatok hello:

| Adatforrás | Leírás |
| --- | --- |
| Teljesítményszámlálók |Operációs rendszer és az egyéni teljesítményszámlálói |
| Alkalmazás-naplók |Az alkalmazás nyomkövetési üzenetek |
| Windows-Eseménynapló |Toohello Windows eseménynaplózásának rendszer küldött információk |
| .NET eseményforrás |Kód írása hello .NET használatával események [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) osztály |
| IIS-napló |IIS-webhelyek kapcsolatos információk |
| Jegyzékfájl alapú ETW |A folyamat által generált Windows nyomkövetési eseményeinek |
| Összeomlási memóriaképek |Egy alkalmazás összeomlása hello folyamat hello eseményben hello állapotával kapcsolatos információkat |
| Egyéni hibanaplókat |Az alkalmazás vagy szolgáltatás által létrehozott naplók |
| Azure-infrastruktúra diagnosztikai naplók |Maga diagnosztikai információk |

hello Azure diagnostics bővítmény át az adatokat tooan Azure storage-fiók, vagy küldje el például tooservices [Application Insights](../application-insights/app-insights-cloudservices.md). Hibakeresés és hibaelhárítás, méri a teljesítményt, erőforrás-használat, a forgalom elemzése és a kapacitástervezés figyelés és naplózás hello adatokat is használhatja.

## <a name="versioning"></a>Versioning
Lásd: [Azure Diagnostics Rendszerverzió-előzmények](azure-diagnostics-versioning-history.md).

## <a name="next-steps"></a>Következő lépések
Válassza ki, melyik szolgáltatás próbált toocollect diagnosztika, és használja a következő lépések cikkek tooget hello. Hello általános Azure diagnostics hivatkozások használata az egyes feladatok esetében.

## <a name="web-apps"></a>Web Apps
Vegye figyelembe, hogy nem használó webalkalmazások Azure Diagnostics. Hello megfelelő adatokat: található [webalkalmazások](../app-service-web/web-sites-enable-diagnostic-log.md)

## <a name="cloud-services-using-azure-diagnostics"></a>Cloud Services Azure Diagnostics használatával
* Ha a Visual Studio használatával, lásd: [használja a Visual Studio tootrace a Cloud Services alkalmazás](../vs-azure-tools-debug-cloud-services-virtual-machines.md) tooget elindult. Egyéb esetben lásd:
* [Hogyan toomonitor felhőalapú szolgáltatások, Azure Diagnostics használatával](../cloud-services/cloud-services-how-to-monitor.md)
* [Cloud Services alkalmazásokban Azure Diagnostics beállítása](../cloud-services/cloud-services-dotnet-diagnostics.md)

Az összetettebb témákra lásd:

* [Az Application insights szolgáltatással használ az Azure Diagnostics Cloud Services csomag](../application-insights/app-insights-cloudservices.md)
* [Az Azure Diagnostics Felhőszolgáltatások alkalmazás nyomkövetési hello folyamata](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md)
* [PowerShell tooset diagnosztika másolatot használja a Cloud Services csomag](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="virtual-machines-using-azure-diagnostics"></a>Virtuális gépek Azure Diagnostics használatával
* Ha a Visual Studio használatával, lásd: [használja a Visual Studio tootrace Azure virtuális gépek](../vs-azure-tools-debug-cloud-services-virtual-machines.md) tooget elindult. Egyéb esetben lásd:
* [Állítsa be az Azure virtuális gép Azure Diagnostics](../virtual-machines-dotnet-diagnostics.md)

Az összetettebb témákra lásd:

* [Használjon PowerShell tooset diagnosztika mentése Azure virtuális gépeken](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Windows virtuális gép létrehozása a figyelési és -diagnosztika Azure Resource Manager-sablon](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="service-fabric-using-azure-diagnostics"></a>A Service Fabric Azure Diagnostics használatával
A kezdéshez [a Service Fabric-alkalmazás figyelése](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Más Service Fabric diagnosztika cikkeket a hello navigációs fában a bal oldali, miután toothis cikk hello érhetők el.

## <a name="general-azure-diagnostics-articles"></a>Általános Azure Diagnostics cikkek
* [Az Azure Diagnostics séma konfigurációs](https://msdn.microsoft.com/library/azure/mt634524.aspx) – megtudhatja, hogyan toochange hello séma fájl toocollect és diagnosztikai adatainak irányításához. Vegye figyelembe, hogy a Visual Studio toochange hello sémafájl is használhatja.
* [Az Azure Storage Azure diagnosztikai adatok tárolási módjára](../cloud-services/cloud-services-dotnet-diagnostics-storage.md) -ismerje hello táblák és ahol hello diagnosztikai adatok írása blobok hello nevét.
* Ismerje meg, túl[teljesítményszámlálók használata az Azure Diagnostics](../cloud-services/cloud-services-dotnet-diagnostics-performance-counters.md).
* Ismerje meg, túl[útvonal Azure diagnosztikai adatokat tooApplication Insights](azure-diagnostics-configure-application-insights.md)
* Ha problémája van diagnosztika indítása, vagy hogy a rendszer az adatok Azure Storage-táblákat, lásd: [Azure Diagnostics hibaelhárítása](azure-diagnostics-troubleshooting.md)
