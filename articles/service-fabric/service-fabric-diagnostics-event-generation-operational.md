---
title: "Service Fabric működési csatorna aaaAzure |} Microsoft Docs"
description: "Hello működési csatorna az Azure Service Fabric-fürtök a létrehozott naplók átfogó listáját."
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
ms.date: 07/19/2017
ms.author: dekapur
ms.openlocfilehash: 358782420ed62b202d6a89fe0f200b5ef0384c9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="operational-channel"></a>Működési csatorna 

hello működési csatorna naplók végzi el a Service Fabric a csomópontokon és a fürt magas szintű műveletekből áll. Ha engedélyezve van a "diagnosztika" hello Azure diagnosztikai ügynök telepítve van a fürt, és alapértelmezés szerint a fürt konfigurált tooread naplók hello működési csatornán. További információk hello konfigurálása [Azure diagnosztikai ügynök](service-fabric-diagnostics-event-aggregation-wad.md) toomodify hello diagnosztika konfigurálása a fürt toopick fel további naplóit vagy teljesítményszámlálóit. 

## <a name="operational-channel-logs"></a>Működési csatorna naplók 

A naplók hello működési csatorna a Service Fabric által biztosított átfogó listája itt található. 

| Eseményazonosító | Név | Forrás (feladat) | Szint |
| --- | --- | --- | --- |
| 25620 | NodeOpening | FabricNode | Tájékoztató |
| 25621 | NodeOpenedSuccess | FabricNode | Tájékoztató |
| 25622 | NodeOpenedFailed | FabricNode | Tájékoztató |
| 25623 | NodeClosing | FabricNode | Tájékoztató |
| 25624 | NodeClosed | FabricNode | Tájékoztató |
| 25625 | NodeAborting | FabricNode | Tájékoztató |
| 25626 | NodeAborted | FabricNode | Tájékoztató |
| 29627 | ClusterUpgradeStart | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 29628 | ClusterUpgradeComplete | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 29629 | ClusterUpgradeRollback | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 29630 | ClusterUpgradeRollbackComplete | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 29631 | ClusterUpgradeDomainComplete | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 23074 | ContainerActivated | Üzemeltetési | Tájékoztató |
| 23075 | ContainerDeactivated | Üzemeltetési | Tájékoztató |
| 29620 | ApplicationCreated | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 29621 | ApplicationUpgradeStart | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 29622 | ApplicationUpgradeComplete | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 29623 | ApplicationUpgradeRollback | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 29624 | ApplicationUpgradeRollbackComplete | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 29625 | ApplicationDeleted | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 29626 | ApplicationUpgradeDomainComplete | TANÚSÍTVÁNYKEZELŐ | Tájékoztató |
| 18566 | ServiceCreated | FM | Tájékoztató |
| 18567 | ServiceDeleted | FM | Tájékoztató |

## <a name="next-steps"></a>Következő lépések

* További általános információ [hello platform szintjén az eseménygenerálás](service-fabric-diagnostics-event-generation-infra.md) a Service Fabric
* Módosítja a [Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) konfigurációs toocollect több naplózza.
* [Az Application Insights beállítása](service-fabric-diagnostics-event-analysis-appinsights.md) toosee a működési csatorna naplói
