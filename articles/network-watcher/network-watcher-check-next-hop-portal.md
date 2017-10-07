---
title: "következő ugrás aaaFind rendelkező Azure hálózati figyelő következő ugrás - Azure-portál |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan található milyen hello a következő ugrás típusa, és IP-cím használatával a következő ugrás használatával hello Azure-portálon"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b459dcf-4077-424e-a774-f7bfa34c5975
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b64a2a5275c15aa8bdb10601de4ae1504a9ab551
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="find-out-what-hello-next-hop-type-is-using-hello-next-hop-capability-in-azure-network-watcher-using-hello-portal"></a><span data-ttu-id="9b5ac-103">Megtudhatja, milyen hello következő ugrás típusa hello következő ugrás funkció használ az Azure hálózati figyelőt hello portál használatával</span><span class="sxs-lookup"><span data-stu-id="9b5ac-103">Find out what hello next hop type is using hello Next Hop capability in Azure Network Watcher using hello portal</span></span>

> [!div class="op_single_selector"]
> - [<span data-ttu-id="9b5ac-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9b5ac-104">Azure portal</span></span>](network-watcher-check-next-hop-portal.md)
> - [<span data-ttu-id="9b5ac-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9b5ac-105">PowerShell</span></span>](network-watcher-check-next-hop-powershell.md)
> - [<span data-ttu-id="9b5ac-106">CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9b5ac-106">CLI 1.0</span></span>](network-watcher-check-next-hop-cli-nodejs.md)
> - [<span data-ttu-id="9b5ac-107">CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9b5ac-107">CLI 2.0</span></span>](network-watcher-check-next-hop-cli.md)
> - [<span data-ttu-id="9b5ac-108">Az Azure REST API-n</span><span class="sxs-lookup"><span data-stu-id="9b5ac-108">Azure REST API</span></span>](network-watcher-check-next-hop-rest.md)

<span data-ttu-id="9b5ac-109">Következő Ugrás az egyik funkciója, amely lehetővé teszi, hello hálózati figyelőt le hello következő ugrás típusa és a megadott virtuális gépen alapuló IP-címet.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-109">Next hop is a feature of Network Watcher that provides hello ability get hello next hop type and IP address based on a specified virtual machine.</span></span> <span data-ttu-id="9b5ac-110">A szolgáltatás akkor hasznos, ha egy virtuális gép elhagyó forgalomra halad át egy átjáró, az interneten vagy a virtuális hálózatok tooget tooits cél meghatározására.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-110">This feature is useful in determining if traffic leaving a virtual machine traverses a gateway, internet, or virtual networks tooget tooits destination.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="9b5ac-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="9b5ac-111">Before you begin</span></span>

<span data-ttu-id="9b5ac-112">Ez a forgatókönyv azt feltételezi, hogy már követte hello lépéseit [hozzon létre egy hálózati figyelőt](network-watcher-create.md) toocreate egy hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-112">This scenario assumes you have already followed hello steps in [Create a Network Watcher](network-watcher-create.md) toocreate a Network Watcher.</span></span> <span data-ttu-id="9b5ac-113">hello is feltételezzük, hogy létezik-e egy érvényes virtuális géppel erőforrás csoport toobe használt.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-113">hello scenario also assumes that a Resource Group with a valid virtual machine exists toobe used.</span></span>

## <a name="scenario"></a><span data-ttu-id="9b5ac-114">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="9b5ac-114">Scenario</span></span>

<span data-ttu-id="9b5ac-115">a cikkben szereplő hello forgatókönyv használja a következő ugrás toofind hello következő ugrás típusa és IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-115">hello scenario covered in this article uses Next hop toofind out hello next hop type and IP address for a resource.</span></span> <span data-ttu-id="9b5ac-116">toolearn következő ugrásaként kapcsolatos további információkért látogasson el [következő ugrásaként áttekintése](network-watcher-next-hop-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9b5ac-116">toolearn more about Next Hop, visit [Next Hop Overview](network-watcher-next-hop-overview.md).</span></span>

<span data-ttu-id="9b5ac-117">Ebben a forgatókönyvben a tartalma:</span><span class="sxs-lookup"><span data-stu-id="9b5ac-117">In this scenario, you will:</span></span>

* <span data-ttu-id="9b5ac-118">Következő ugrás hello lekérése egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-118">Retrieve hello next hop from a virtual machine.</span></span>

## <a name="get-next-hop"></a><span data-ttu-id="9b5ac-119">Következő ugrás beolvasása</span><span class="sxs-lookup"><span data-stu-id="9b5ac-119">Get Next Hop</span></span>

### <a name="step-1"></a><span data-ttu-id="9b5ac-120">1. lépés</span><span class="sxs-lookup"><span data-stu-id="9b5ac-120">Step 1</span></span>

<span data-ttu-id="9b5ac-121">Keresse meg a hálózati figyelőt erőforrás tooyour hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-121">Navigate tooyour Network Watcher resource in hello Azure portal.</span></span>

### <a name="step-2"></a><span data-ttu-id="9b5ac-122">2. lépés</span><span class="sxs-lookup"><span data-stu-id="9b5ac-122">Step 2</span></span>

<span data-ttu-id="9b5ac-123">Kattintson a **a következő Ugrás** hello navigációs ablaktábla, válassza hello virtuálisgép- és hálózati adapter, töltse ki a forrás és cél IP hello, majd kattintson a hello **a következő Ugrás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-123">Click **Next hop** in hello navigation pane, select hello virtual machine and network interface, fill out hello source and destination IP, and click hello **Next hop** button.</span></span>

> [!NOTE]
> <span data-ttu-id="9b5ac-124">Következő ugrás megköveteli, hogy hello VM erőforrás toorun van lefoglalva.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-124">Next hop requires that hello VM resource is allocated toorun.</span></span>

![következő ugrás áttekintése beolvasása][1]

### <a name="step-3"></a><span data-ttu-id="9b5ac-126">3. lépés</span><span class="sxs-lookup"><span data-stu-id="9b5ac-126">Step 3</span></span>

<span data-ttu-id="9b5ac-127">Ha hello feladat befejeződött, hello eredményei.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-127">Once hello task is complete, hello results are provided.</span></span> <span data-ttu-id="9b5ac-128">hello IP-cím és az eszköz hello következő ugrás típusa, jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-128">hello IP address and type of device hello next hop is, is displayed.</span></span> <span data-ttu-id="9b5ac-129">hello következő táblázatban hello elérhető visszaadott értékek hello portálon.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-129">hello following table shows hello available returned values in hello portal.</span></span>

<span data-ttu-id="9b5ac-130">**Következő ugrás típusa**</span><span class="sxs-lookup"><span data-stu-id="9b5ac-130">**Next Hop Type**</span></span>

* <span data-ttu-id="9b5ac-131">Internet</span><span class="sxs-lookup"><span data-stu-id="9b5ac-131">Internet</span></span>
* <span data-ttu-id="9b5ac-132">VirtualAppliance</span><span class="sxs-lookup"><span data-stu-id="9b5ac-132">VirtualAppliance</span></span>
* <span data-ttu-id="9b5ac-133">Pedig</span><span class="sxs-lookup"><span data-stu-id="9b5ac-133">VirtualNetworkGateway</span></span>
* <span data-ttu-id="9b5ac-134">VnetLocal</span><span class="sxs-lookup"><span data-stu-id="9b5ac-134">VnetLocal</span></span>
* <span data-ttu-id="9b5ac-135">HyperNetGateway</span><span class="sxs-lookup"><span data-stu-id="9b5ac-135">HyperNetGateway</span></span>
* <span data-ttu-id="9b5ac-136">VnetPeering</span><span class="sxs-lookup"><span data-stu-id="9b5ac-136">VnetPeering</span></span>
* <span data-ttu-id="9b5ac-137">None</span><span class="sxs-lookup"><span data-stu-id="9b5ac-137">None</span></span>

<span data-ttu-id="9b5ac-138">Ha egy egyéni útvonal lett használt tooroute a forgalmat, hello felhasználó által megadott útvonal (UDR) jelenik meg, valamint hello eredményekkel.</span><span class="sxs-lookup"><span data-stu-id="9b5ac-138">If a custom route was used tooroute this traffic, hello User-defined route (UDR) is shown as well with hello results.</span></span>

![következő ugrás eredményt ad][2]

## <a name="next-steps"></a><span data-ttu-id="9b5ac-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b5ac-140">Next steps</span></span>

<span data-ttu-id="9b5ac-141">Megtudhatja, hogyan tooreview programozott módon látogasson el a hálózati biztonsági csoport beállításainak [NSG naplózás hálózati figyelőt](network-watcher-nsg-auditing-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="9b5ac-141">Learn how tooreview your network security group settings programmatically by visiting [NSG Auditing with Network Watcher](network-watcher-nsg-auditing-powershell.md)</span></span>

[1]: ./media/network-watcher-check-next-hop-portal/figure1.png
[2]: ./media/network-watcher-check-next-hop-portal/figure2.png














