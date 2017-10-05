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
# <a name="introduction-to-topology-in-azure-network-watcher"></a><span data-ttu-id="e78c1-103">Bevezetés az Azure hálózati figyelőt-topológiája</span><span class="sxs-lookup"><span data-stu-id="e78c1-103">Introduction to topology in Azure Network Watcher</span></span>

<span data-ttu-id="e78c1-104">Topológia egy grafikonon hálózati erőforrások része virtuális hálózatnak adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e78c1-104">Topology returns a graph of network resources in a virtual network.</span></span> <span data-ttu-id="e78c1-105">Az ábra a végpontok közötti hálózati kapcsolatot képviselő erőforrások összekapcsolása ábrázolja.</span><span class="sxs-lookup"><span data-stu-id="e78c1-105">The graph depicts the interconnection between the resources to represent the end to end network connectivity.</span></span>

![topológia – áttekintés][1]

<span data-ttu-id="e78c1-107">A portál topológia adja vissza az erőforrás-objektumból egy egy virtuális hálózati alapján.</span><span class="sxs-lookup"><span data-stu-id="e78c1-107">In the portal, Topology returns the resource objects on a per virtual network basis.</span></span> <span data-ttu-id="e78c1-108">A kapcsolatok ezek ábrázolva hálózati figyelőt régió kívüli erőforrások erőforrások közötti vonalak, még akkor is, ha az erőforrás a csoport nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e78c1-108">The relationships are depicted by lines between the resources Resources outside of the Network Watcher region, even if in the resource group will not be displayed.</span></span> <span data-ttu-id="e78c1-109">Az erőforrások, az eredmény abban a portál nézet a hálózati összetevők, amelyek grafikon részhalmazát jelentik.</span><span class="sxs-lookup"><span data-stu-id="e78c1-109">The resources returned in the portal view are a subset of the networking components that are graphed.</span></span> <span data-ttu-id="e78c1-110">A hálózati erőforrások is használhatja a teljes listájának megjelenítéséhez [PowerShell](network-watcher-topology-powershell.md) vagy [REST](network-watcher-topology-rest.md)</span><span class="sxs-lookup"><span data-stu-id="e78c1-110">To see the full list of networking resources you can use [PowerShell](network-watcher-topology-powershell.md) or [REST](network-watcher-topology-rest.md)</span></span>

> [!NOTE]
> <span data-ttu-id="e78c1-111">Hálózati figyelőt egy példányát minden egyes régió topológia futtatni kívánt kell megadni.</span><span class="sxs-lookup"><span data-stu-id="e78c1-111">An instance of Network Watcher is required in each region that you want to run Topology on.</span></span>

<span data-ttu-id="e78c1-112">Erőforrások közöttük visszaadott a kapcsolatot, két kapcsolatok alapján van modellezve.</span><span class="sxs-lookup"><span data-stu-id="e78c1-112">As resources are returned the connection between them are modeled under two relationships.</span></span>

- <span data-ttu-id="e78c1-113">**A befoglaltsági** -példa: virtuális hálózat tartalmaz egy hálózati Adaptert tartalmazó alhálózat</span><span class="sxs-lookup"><span data-stu-id="e78c1-113">**Containment** - Example: Virtual Network contains a Subnet which contains a NIC</span></span>
- <span data-ttu-id="e78c1-114">**Kapcsolódó** -példa: A hálózati adapter kapcsolódik a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="e78c1-114">**Associated** - Example: A NIC is associated with a VM</span></span>

### <a name="next-steps"></a><span data-ttu-id="e78c1-115">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e78c1-115">Next steps</span></span>

<span data-ttu-id="e78c1-116">Megtudhatja, hogyan lehet lekérni a topológia e nézetében látogasson el a PowerShell használatával [hálózati figyelőt topológia a PowerShell használatával](network-watcher-topology-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="e78c1-116">Learn how to use PowerShell to retrieve the Topology view by visiting [Network Watcher topology with PowerShell](network-watcher-topology-powershell.md)</span></span>

<!--Image references-->

[1]: ./media/network-watcher-topology-overview/topology.png
