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
# <a name="introduction-tootopology-in-azure-network-watcher"></a><span data-ttu-id="2cfc6-103">Az Azure hálózati figyelőt bemutatása tootopology</span><span class="sxs-lookup"><span data-stu-id="2cfc6-103">Introduction tootopology in Azure Network Watcher</span></span>

<span data-ttu-id="2cfc6-104">Topológia egy grafikonon hálózati erőforrások része virtuális hálózatnak adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2cfc6-104">Topology returns a graph of network resources in a virtual network.</span></span> <span data-ttu-id="2cfc6-105">hello ábra hello összekapcsolása hello erőforrások toorepresent hello end tooend hálózati kapcsolatot ábrázolja.</span><span class="sxs-lookup"><span data-stu-id="2cfc6-105">hello graph depicts hello interconnection between hello resources toorepresent hello end tooend network connectivity.</span></span>

![topológia – áttekintés][1]

<span data-ttu-id="2cfc6-107">Hello portálon topológia hello erőforrás-objektumból lévő beolvasása a virtuális hálózati alap /.</span><span class="sxs-lookup"><span data-stu-id="2cfc6-107">In hello portal, Topology returns hello resource objects on a per virtual network basis.</span></span> <span data-ttu-id="2cfc6-108">hello kapcsolatok ezek ábrázolva hello erőforrások erőforrások kívül hello hálózati figyelőt régió, közötti vonalak, még akkor is, ha a hello erőforrás csoport nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="2cfc6-108">hello relationships are depicted by lines between hello resources Resources outside of hello Network Watcher region, even if in hello resource group will not be displayed.</span></span> <span data-ttu-id="2cfc6-109">hello portál nézetben visszaadott hello erőforrások hello hálózati összetevők, amelyek grafikon részhalmazát jelentik.</span><span class="sxs-lookup"><span data-stu-id="2cfc6-109">hello resources returned in hello portal view are a subset of hello networking components that are graphed.</span></span> <span data-ttu-id="2cfc6-110">toosee hello teljes listáját a hálózati erőforrások is használhat [PowerShell](network-watcher-topology-powershell.md) vagy [REST](network-watcher-topology-rest.md)</span><span class="sxs-lookup"><span data-stu-id="2cfc6-110">toosee hello full list of networking resources you can use [PowerShell](network-watcher-topology-powershell.md) or [REST](network-watcher-topology-rest.md)</span></span>

> [!NOTE]
> <span data-ttu-id="2cfc6-111">Hálózati figyelőt egy példányát minden egyes régió kívánt toorun topológia kell megadni.</span><span class="sxs-lookup"><span data-stu-id="2cfc6-111">An instance of Network Watcher is required in each region that you want toorun Topology on.</span></span>

<span data-ttu-id="2cfc6-112">Erőforrások hello kapcsolat visszaadott közöttük, a két van modellezve.</span><span class="sxs-lookup"><span data-stu-id="2cfc6-112">As resources are returned hello connection between them are modeled under two relationships.</span></span>

- <span data-ttu-id="2cfc6-113">**A befoglaltsági** -példa: virtuális hálózat tartalmaz egy hálózati Adaptert tartalmazó alhálózat</span><span class="sxs-lookup"><span data-stu-id="2cfc6-113">**Containment** - Example: Virtual Network contains a Subnet which contains a NIC</span></span>
- <span data-ttu-id="2cfc6-114">**Kapcsolódó** -példa: A hálózati adapter kapcsolódik a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="2cfc6-114">**Associated** - Example: A NIC is associated with a VM</span></span>

### <a name="next-steps"></a><span data-ttu-id="2cfc6-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2cfc6-115">Next steps</span></span>

<span data-ttu-id="2cfc6-116">Ismerje meg, hogyan toouse PowerShell tooretrieve hello topológia megtekintéséhez látogasson el [hálózati figyelőt topológia a PowerShell használatával](network-watcher-topology-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="2cfc6-116">Learn how toouse PowerShell tooretrieve hello Topology view by visiting [Network Watcher topology with PowerShell](network-watcher-topology-powershell.md)</span></span>

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
