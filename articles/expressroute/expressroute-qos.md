---
title: "aaaQoS ExpressRoute követelményei |} Microsoft Docs"
description: "Ez az oldal ExpressRoute-kapcsolatcsoportok QoS-konfigurálásának és -kezelésének részletes követelményeit ismerteti."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: db1c1447-0283-4a09-907b-ae481adc40c7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: cherylmc
ms.openlocfilehash: 46cc81bd38ff50dd9e7a1bfdd0faa457ff7b2fa1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="expressroute-qos-requirements"></a><span data-ttu-id="cc76f-103">Az ExpressRoute QoS-követelményei</span><span class="sxs-lookup"><span data-stu-id="cc76f-103">ExpressRoute QoS requirements</span></span>
<span data-ttu-id="cc76f-104">A Skype Vállalati verzió különböző számítási feladatokat tartalmaz, amelyek különböző QoS-kezelést igényelnek.</span><span class="sxs-lookup"><span data-stu-id="cc76f-104">Skype for Business has various workloads that require differentiated QoS treatment.</span></span> <span data-ttu-id="cc76f-105">Ha tooconsume szolgáltatások ExpressRoute keresztül, meg kell felelnie toohello követelmények az alábbiakban.</span><span class="sxs-lookup"><span data-stu-id="cc76f-105">If you plan tooconsume voice services through ExpressRoute, you should adhere toohello requirements described below.</span></span>

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> <span data-ttu-id="cc76f-106">A QoS követelményeit toohello Microsoft társviszony-létesítés csak.</span><span class="sxs-lookup"><span data-stu-id="cc76f-106">QoS requirements apply toohello Microsoft peering only.</span></span> <span data-ttu-id="cc76f-107">a hálózati forgalom az Azure nyilvános társviszony és Azure magánhálózati társviszony-létesítés kapott hello DSCP értékkel alaphelyzetbe állítása too0 lesz.</span><span class="sxs-lookup"><span data-stu-id="cc76f-107">hello DSCP values in your network traffic received on Azure public peering and Azure private peering will be reset too0.</span></span> 
> 
> 

<span data-ttu-id="cc76f-108">hello következő táblázat felsorolja a DSCP-jelölések Skype vállalati használja.</span><span class="sxs-lookup"><span data-stu-id="cc76f-108">hello following table provides a list of DSCP markings used by Skype for Business.</span></span> <span data-ttu-id="cc76f-109">Tekintse meg a túl[QoS kezelése a Skype vállalati verzió](https://technet.microsoft.com/library/gg405409.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="cc76f-109">Refer too[Managing QoS for Skype for Business](https://technet.microsoft.com/library/gg405409.aspx) for more information.</span></span>

| <span data-ttu-id="cc76f-110">**Forgalomosztály**</span><span class="sxs-lookup"><span data-stu-id="cc76f-110">**Traffic Class**</span></span> | <span data-ttu-id="cc76f-111">**Kezelés (DSCP-jelölés)**</span><span class="sxs-lookup"><span data-stu-id="cc76f-111">**Treatment (DSCP Marking)**</span></span> | <span data-ttu-id="cc76f-112">**Skype Vállalati verzió számítási feladata**</span><span class="sxs-lookup"><span data-stu-id="cc76f-112">**Skype for Business Workloads**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cc76f-113">**Hang**</span><span class="sxs-lookup"><span data-stu-id="cc76f-113">**Voice**</span></span> |<span data-ttu-id="cc76f-114">EF (46)</span><span class="sxs-lookup"><span data-stu-id="cc76f-114">EF (46)</span></span> |<span data-ttu-id="cc76f-115">Skype- / Lync-hang</span><span class="sxs-lookup"><span data-stu-id="cc76f-115">Skype / Lync voice</span></span> |
| <span data-ttu-id="cc76f-116">**Interaktív**</span><span class="sxs-lookup"><span data-stu-id="cc76f-116">**Interactive**</span></span> |<span data-ttu-id="cc76f-117">AF41 (34)</span><span class="sxs-lookup"><span data-stu-id="cc76f-117">AF41 (34)</span></span> |<span data-ttu-id="cc76f-118">Videó, VBSS</span><span class="sxs-lookup"><span data-stu-id="cc76f-118">Video, VBSS</span></span> |
| <span data-ttu-id="cc76f-119">AF21 (18)</span><span class="sxs-lookup"><span data-stu-id="cc76f-119">AF21 (18)</span></span> |<span data-ttu-id="cc76f-120">Alkalmazásmegosztás</span><span class="sxs-lookup"><span data-stu-id="cc76f-120">App sharing</span></span> | |
| <span data-ttu-id="cc76f-121">**Alapértelmezett**</span><span class="sxs-lookup"><span data-stu-id="cc76f-121">**Default**</span></span> |<span data-ttu-id="cc76f-122">AF11 (10)</span><span class="sxs-lookup"><span data-stu-id="cc76f-122">AF11 (10)</span></span> |<span data-ttu-id="cc76f-123">Fájlátvitel</span><span class="sxs-lookup"><span data-stu-id="cc76f-123">File transfer</span></span> |
| <span data-ttu-id="cc76f-124">CS0 (0)</span><span class="sxs-lookup"><span data-stu-id="cc76f-124">CS0 (0)</span></span> |<span data-ttu-id="cc76f-125">Bármi más</span><span class="sxs-lookup"><span data-stu-id="cc76f-125">Anything else</span></span> | |

* <span data-ttu-id="cc76f-126">Hello munkaterhelések besorolásához kell, és jelölje meg hello jobb DSCP értékkel.</span><span class="sxs-lookup"><span data-stu-id="cc76f-126">You should classify hello workloads and mark hello right DSCP values.</span></span> <span data-ttu-id="cc76f-127">Követve hello [Itt](https://technet.microsoft.com/library/gg405409.aspx) hogyan tooset DSCP megjelölés a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="cc76f-127">Follow hello guidance provided [here](https://technet.microsoft.com/library/gg405409.aspx) on how tooset DSCP markings in your network.</span></span>
* <span data-ttu-id="cc76f-128">Több QoS várakozási sort kell konfigurálnia és támogatnia a hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="cc76f-128">You should configure and support multiple QoS queues within your network.</span></span> <span data-ttu-id="cc76f-129">Hangalámondás egy önálló osztály legyen, és RFC 3246 megadott hello EF-kezelés.</span><span class="sxs-lookup"><span data-stu-id="cc76f-129">Voice must be a standalone class and receive hello EF treatment specified in RFC 3246.</span></span> 
* <span data-ttu-id="cc76f-130">Dönthet úgy is hello queuing mechanizmus, torlódás szabályzat és sávszélesség-lefoglalást egyes forgalmi osztálynak.</span><span class="sxs-lookup"><span data-stu-id="cc76f-130">You can decide hello queuing mechanism, congestion detection policy, and bandwidth allocation per traffic class.</span></span> <span data-ttu-id="cc76f-131">De hello DSCP-jelölés a Skype vállalati munkaterhelések esetén meg kell őrizni.</span><span class="sxs-lookup"><span data-stu-id="cc76f-131">But, hello DSCP marking for Skype for Business workloads must be preserved.</span></span> <span data-ttu-id="cc76f-132">Ha használ, fent nem említett DSCP-jelölés például AF31 (26), meg kell írniuk a DSCP érték too0 hello csomagok tooMicrosoft elküldése előtt.</span><span class="sxs-lookup"><span data-stu-id="cc76f-132">If you are using DSCP markings not listed above, e.g. AF31 (26), you must rewrite this DSCP value too0 before sending hello packet tooMicrosoft.</span></span> <span data-ttu-id="cc76f-133">A Microsoft csak akkor csomagok DSCP érték hello táblázat felett megjelenő hello jelölésű küldi el.</span><span class="sxs-lookup"><span data-stu-id="cc76f-133">Microsoft only sends packets marked with hello DSCP value shown in hello above table.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="cc76f-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cc76f-134">Next steps</span></span>
* <span data-ttu-id="cc76f-135">Tekintse meg a toohello követelményei [útválasztás](expressroute-routing.md) és [NAT](expressroute-nat.md).</span><span class="sxs-lookup"><span data-stu-id="cc76f-135">Refer toohello requirements for [Routing](expressroute-routing.md) and [NAT](expressroute-nat.md).</span></span>
* <span data-ttu-id="cc76f-136">Tekintse meg a következő hello hivatkozásait tooconfigure az ExpressRoute-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="cc76f-136">See hello following links tooconfigure your ExpressRoute connection.</span></span>
  
  * [<span data-ttu-id="cc76f-137">ExpressRoute-kapcsolatcsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="cc76f-137">Create an ExpressRoute circuit</span></span>](expressroute-howto-circuit-classic.md)
  * [<span data-ttu-id="cc76f-138">Útválasztás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cc76f-138">Configure routing</span></span>](expressroute-howto-routing-classic.md)
  * [<span data-ttu-id="cc76f-139">Hivatkozásra egy VNet tooan ExpressRoute-kapcsolatcsoportot</span><span class="sxs-lookup"><span data-stu-id="cc76f-139">Link a VNet tooan ExpressRoute circuit</span></span>](expressroute-howto-linkvnet-classic.md)

