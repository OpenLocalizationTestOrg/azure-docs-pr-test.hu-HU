---
title: "Bevezetés az Azure hálózati figyelőt topológiája |} Microsoft Docs"
description: "Ezen a lapon a hálózati figyelőt topológia képességek áttekintése"
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
ms.openlocfilehash: 42443f614b76b8180ac163b9889163021adbf048
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-topology-in-azure-network-watcher"></a>Bevezetés az Azure hálózati figyelőt-topológiája

Topológia egy grafikonon hálózati erőforrások része virtuális hálózatnak adja vissza. Az ábra a végpontok közötti hálózati kapcsolatot képviselő erőforrások összekapcsolása ábrázolja.

![topológia – áttekintés][1]

A portál topológia adja vissza az erőforrás-objektumból egy egy virtuális hálózati alapján. A kapcsolatok ezek ábrázolva hálózati figyelőt régió kívüli erőforrások erőforrások közötti vonalak, még akkor is, ha az erőforrás a csoport nem jelenik meg. Az erőforrások, az eredmény abban a portál nézet a hálózati összetevők, amelyek grafikon részhalmazát jelentik. A hálózati erőforrások is használhatja a teljes listájának megjelenítéséhez [PowerShell](network-watcher-topology-powershell.md) vagy [REST](network-watcher-topology-rest.md)

> [!NOTE]
> Hálózati figyelőt egy példányát minden egyes régió topológia futtatni kívánt kell megadni.

Erőforrások közöttük visszaadott a kapcsolatot, két kapcsolatok alapján van modellezve.

- **A befoglaltsági** -példa: virtuális hálózat tartalmaz egy hálózati Adaptert tartalmazó alhálózat
- **Kapcsolódó** -példa: A hálózati adapter kapcsolódik a virtuális gépek

### <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan lehet lekérni a topológia e nézetében látogasson el a PowerShell használatával [hálózati figyelőt topológia a PowerShell használatával](network-watcher-topology-powershell.md)

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
