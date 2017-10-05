---
title: "Bevezetés az Azure hálózati figyelőt a következő ugrás |} Microsoft Docs"
description: "Ezen a lapon a hálózati figyelőt áttekintést nyújt következő ugrás funkció"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: febf7bca-e0b7-41d5-838f-a5a40ebc5aac
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 5dd65d2418cae206965a13013dd990b916ad0733
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="introduction-to-next-hop-in-azure-network-watcher"></a><span data-ttu-id="98686-103">Bevezetés az Azure hálózati figyelőt a következő ugrás</span><span class="sxs-lookup"><span data-stu-id="98686-103">Introduction to next hop in Azure Network Watcher</span></span>

<span data-ttu-id="98686-104">VM forgalmat elküldi a hatékony társított hálózati útvonalak alapján célhelyét.</span><span class="sxs-lookup"><span data-stu-id="98686-104">Traffic from a VM is sent to a destination based on the effective routes associated with a NIC.</span></span> <span data-ttu-id="98686-105">Következő ugrás lekérése a következő ugrás típusa és a csomagok IP-cím egy adott virtuális gép és a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="98686-105">Next hop gets the next hop type and IP address of a packet from a specific virtual machine and NIC.</span></span> <span data-ttu-id="98686-106">Ez segít meghatározni, ha a csomag van irányítja a cél, vagy éppen fekete forgalom furatos van.</span><span class="sxs-lookup"><span data-stu-id="98686-106">This helps to determine if the packet is being directed to the destination or is the traffic being black holed.</span></span> <span data-ttu-id="98686-107">Az útvonalak a felhasználó, ahol a forgalom irányítja a rendszer egy helyi helyre, vagy a virtuális készülék, egy nem megfelelő konfigurációs problémák vezethet.</span><span class="sxs-lookup"><span data-stu-id="98686-107">An improper configuration of routes by the user, where a traffic is directed to an on-premises location or a virtual appliance, can lead to connectivity issues.</span></span> <span data-ttu-id="98686-108">Következő ugrás is a következő ugrás hozzárendelt útválasztási táblázatot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="98686-108">Next hop also returns the route table associated with the next hop.</span></span> <span data-ttu-id="98686-109">Lekérdezésekor a következő ugrás, ha egy felhasználó által megadott útvonalat az útvonal van meghatározva, az adott útvonal eredmény.</span><span class="sxs-lookup"><span data-stu-id="98686-109">When querying a next hop if the route is defined as a user-defined route, that route will be returned.</span></span> <span data-ttu-id="98686-110">Egyéb esetben a következő ugrás "Rendszerútvonal" ad vissza.</span><span class="sxs-lookup"><span data-stu-id="98686-110">Otherwise Next hop returns "System Route".</span></span>

![a következő ugrás – áttekintés][1]

<span data-ttu-id="98686-112">A következő ugrás Típuslista, amely a következő ugrás lekérdezésekor adhatók vissza a következő:</span><span class="sxs-lookup"><span data-stu-id="98686-112">The following is a list of the next hop types that can be returned when querying Next hop.</span></span>

* <span data-ttu-id="98686-113">Internet</span><span class="sxs-lookup"><span data-stu-id="98686-113">Internet</span></span>
* <span data-ttu-id="98686-114">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="98686-114">VirtualAppliance</span></span>
* <span data-ttu-id="98686-115">Pedig</span><span class="sxs-lookup"><span data-stu-id="98686-115">VirtualNetworkGateway</span></span>
* <span data-ttu-id="98686-116">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="98686-116">VnetLocal</span></span>
* <span data-ttu-id="98686-117">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="98686-117">HyperNetGateway</span></span>
* <span data-ttu-id="98686-118">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="98686-118">VnetPeering</span></span>
* <span data-ttu-id="98686-119">None</span><span class="sxs-lookup"><span data-stu-id="98686-119">None</span></span>

### <a name="next-steps"></a><span data-ttu-id="98686-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98686-120">Next steps</span></span>

<span data-ttu-id="98686-121">Következő Ugrás használata a problémák keresése a hálózati kapcsolat hibája ellátogatva [ellenőrizze a következő ugrás a virtuális gép](network-watcher-check-next-hop-portal.md)</span><span class="sxs-lookup"><span data-stu-id="98686-121">Learn how to use next hop to find issues with network connectivity by visiting [Check the next hop on a VM](network-watcher-check-next-hop-portal.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













