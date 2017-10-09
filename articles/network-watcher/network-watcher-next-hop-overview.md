---
title: "aaaIntroduction toonext Ugrás az Azure hálózati figyelőt |} Microsoft Docs"
description: "Ezen a lapon következő ugrás képességet biztosít hello hálózati figyelőt áttekintése"
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
ms.openlocfilehash: 916af736d0d52abadeafed746f0f8a0173b11033
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toonext-hop-in-azure-network-watcher"></a><span data-ttu-id="20962-103">Bevezetés toonext Ugrás az Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="20962-103">Introduction toonext hop in Azure Network Watcher</span></span>

<span data-ttu-id="20962-104">A virtuális gépről érkező forgalom mennyiségének tooa cél hello hatékony útvonalak társított hálózati alapján</span><span class="sxs-lookup"><span data-stu-id="20962-104">Traffic from a VM is sent tooa destination based on hello effective routes associated with a NIC.</span></span> <span data-ttu-id="20962-105">Következő ugrás lekérése hello következő ugrás típusa és a csomagok IP-cím egy adott virtuális gép és a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="20962-105">Next hop gets hello next hop type and IP address of a packet from a specific virtual machine and NIC.</span></span> <span data-ttu-id="20962-106">Ez segít toodetermine, ha hello csomagot irányított toohello cél vagy rendszer éppen fekete hello forgalom furatos.</span><span class="sxs-lookup"><span data-stu-id="20962-106">This helps toodetermine if hello packet is being directed toohello destination or is hello traffic being black holed.</span></span> <span data-ttu-id="20962-107">Útvonalak hello felhasználó, ahol a forgalom irányított tooan helyszíni helyre vagy egy virtuális készülékre, egy nem megfelelő konfigurációja tooconnectivity problémákkal is járhat.</span><span class="sxs-lookup"><span data-stu-id="20962-107">An improper configuration of routes by hello user, where a traffic is directed tooan on-premises location or a virtual appliance, can lead tooconnectivity issues.</span></span> <span data-ttu-id="20962-108">Következő ugrás is hello következő ugrás társított hello útválasztási táblázatot ad vissza.</span><span class="sxs-lookup"><span data-stu-id="20962-108">Next hop also returns hello route table associated with hello next hop.</span></span> <span data-ttu-id="20962-109">Lekérdezésekor a következő ugrás, ha egy felhasználó által megadott útvonal hello útvonal van meghatározva, az adott útvonal eredmény.</span><span class="sxs-lookup"><span data-stu-id="20962-109">When querying a next hop if hello route is defined as a user-defined route, that route will be returned.</span></span> <span data-ttu-id="20962-110">Egyéb esetben a következő ugrás "Rendszerútvonal" ad vissza.</span><span class="sxs-lookup"><span data-stu-id="20962-110">Otherwise Next hop returns "System Route".</span></span>

![a következő ugrás – áttekintés][1]

<span data-ttu-id="20962-112">hello az alábbiakban olvashat egy listát hello következő ugrás típusa, amely a következő ugrás lekérdezésekor adhatók vissza.</span><span class="sxs-lookup"><span data-stu-id="20962-112">hello following is a list of hello next hop types that can be returned when querying Next hop.</span></span>

* <span data-ttu-id="20962-113">Internet</span><span class="sxs-lookup"><span data-stu-id="20962-113">Internet</span></span>
* <span data-ttu-id="20962-114">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="20962-114">VirtualAppliance</span></span>
* <span data-ttu-id="20962-115">Pedig</span><span class="sxs-lookup"><span data-stu-id="20962-115">VirtualNetworkGateway</span></span>
* <span data-ttu-id="20962-116">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="20962-116">VnetLocal</span></span>
* <span data-ttu-id="20962-117">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="20962-117">HyperNetGateway</span></span>
* <span data-ttu-id="20962-118">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="20962-118">VnetPeering</span></span>
* <span data-ttu-id="20962-119">None</span><span class="sxs-lookup"><span data-stu-id="20962-119">None</span></span>

### <a name="next-steps"></a><span data-ttu-id="20962-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="20962-120">Next steps</span></span>

<span data-ttu-id="20962-121">Ismerje meg, hogyan toouse következő ugrás toofind állít ki a hálózati kapcsolat hibája ellátogatva [ellenőrzés hello következő ugrás a virtuális gép](network-watcher-check-next-hop-portal.md)</span><span class="sxs-lookup"><span data-stu-id="20962-121">Learn how toouse next hop toofind issues with network connectivity by visiting [Check hello next hop on a VM](network-watcher-check-next-hop-portal.md)</span></span>

<!--Image references-->
[1]: ./media/network-watcher-next-hop-overview/figure1.png













