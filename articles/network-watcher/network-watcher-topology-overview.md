---
title: "az Azure hálózati figyelőt aaaIntroduction tootopology |} Microsoft Docs"
description: "Ezen a lapon hello hálózati figyelőt topológia képességek áttekintése"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e753a435-38e0-482b-846b-121cb547555c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 7fa1c5518e4a25a5db999d898a9ee19fd0121db7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tootopology-in-azure-network-watcher"></a>Az Azure hálózati figyelőt bemutatása tootopology

Topológia egy grafikonon hálózati erőforrások része virtuális hálózatnak adja vissza. hello ábra hello összekapcsolása hello erőforrások toorepresent hello end tooend hálózati kapcsolatot ábrázolja.

![topológia – áttekintés][1]

Hello portálon topológia hello erőforrás-objektumból lévő beolvasása a virtuális hálózati alap /. hello kapcsolatok ezek ábrázolva hello erőforrások erőforrások kívül hello hálózati figyelőt régió, közötti vonalak, még akkor is, ha a hello erőforrás csoport nem jelenik meg. hello portál nézetben visszaadott hello erőforrások hello hálózati összetevők, amelyek grafikon részhalmazát jelentik. toosee hello teljes listáját a hálózati erőforrások is használhat [PowerShell](network-watcher-topology-powershell.md) vagy [REST](network-watcher-topology-rest.md)

> [!NOTE]
> Hálózati figyelőt egy példányát minden egyes régió kívánt toorun topológia kell megadni.

Erőforrások hello kapcsolat visszaadott közöttük, a két van modellezve.

- **A befoglaltsági** -példa: virtuális hálózat tartalmaz egy hálózati Adaptert tartalmazó alhálózat
- **Kapcsolódó** -példa: A hálózati adapter kapcsolódik a virtuális gépek

### <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan toouse PowerShell tooretrieve hello topológia megtekintéséhez látogasson el [hálózati figyelőt topológia a PowerShell használatával](network-watcher-topology-powershell.md)

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
