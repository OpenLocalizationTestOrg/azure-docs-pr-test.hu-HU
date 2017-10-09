---
title: "aaaInternet irányuló load balancer áttekintése |} Microsoft Docs"
description: "Az internetre irányuló terheléselosztót és a szolgáltatások áttekintése. A terheléselosztó működéséről az Azure virtuális gépek és felhőszolgáltatások használatával."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: 529b37aa-a45c-41d1-8877-fee8cc1fa375
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3514f945d69ec576ed256cdd01069491e3e43936
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="internet-facing-load-balancer-overview"></a><span data-ttu-id="ad2b8-104">Az Internet felé néző terhelés terheléselosztó áttekintése</span><span class="sxs-lookup"><span data-stu-id="ad2b8-104">Internet facing load balancer overview</span></span>

<span data-ttu-id="ad2b8-105">Az Azure terheléselosztó továbbítja a hello nyilvános IP-cím és port száma bejövő forgalom toohello privát IP-cím és port számát hello virtuális gép, és ez fordítva is igaz hello válasz forgalom hello virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-105">Azure load balancer maps hello public IP address and port number of incoming traffic toohello private IP address and port number of hello virtual machine and vice versa for hello response traffic from hello virtual machine.</span></span> <span data-ttu-id="ad2b8-106">Terheléselosztási szabályok lehetővé teszik több virtuális gépek vagy szolgáltatások közötti forgalom toodistribute adott típusú.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-106">Load balancing rules allow you toodistribute specific types of traffic between multiple virtual machines or services.</span></span> <span data-ttu-id="ad2b8-107">Például a webes kérelem forgalom terhelését hello elosztva is több webkiszolgálók vagy webes szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-107">For example, you can spread hello load of web request traffic across multiple web servers or web roles.</span></span>

<span data-ttu-id="ad2b8-108">Egy felhőalapú szolgáltatás, amely tartalmazza a webes szerepkörök vagy feldolgozói szerepkörök példányai meghatározhatja egy nyilvános végpontot hello szolgáltatás definíciós (.csdef) fájl.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-108">For a cloud service that contains instances of web roles or worker roles, you can define a public endpoint in hello service definition (.csdef) file.</span></span>

<span data-ttu-id="ad2b8-109">Hello *servicedefinition.csdef* hello végpont-konfiguráció tartalmazza, ha több szerepkör példányát egy webes vagy feldolgozói szerepkör üzembe helyezése, hello terheléselosztó lesz állítva.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-109">hello *servicedefinition.csdef* file contains hello endpoint configuration and when you have multiple role instances for a web or worker role deployment, hello load balancer will be setup for it.</span></span> <span data-ttu-id="ad2b8-110">hello módon tooadd példányok tooyour felhő üzembe helyezése hello példányok száma a hello szolgáltatás konfigurációs fájljában (.csfg) változik.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-110">hello way tooadd instances tooyour cloud deployment is changing hello instance count on hello service configuration file (.csfg).</span></span>

<span data-ttu-id="ad2b8-111">hello alábbi ábrán egy elosztott terhelésű végpont a webes forgalomban hello nyilvános és titkos TCP-port a 80-as három virtuális gépek által közösen használt.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-111">hello following figure shows a load-balanced endpoint for web traffic that is shared among three virtual machines for hello public and private TCP port of 80.</span></span> <span data-ttu-id="ad2b8-112">A három virtuális gépek vannak egy elosztott terhelésű készlet.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-112">These three virtual machines are in a load-balanced set.</span></span>

![Példa nyilvános terheléselosztóra](./media/load-balancer-internet-overview/IC727496.png)

<span data-ttu-id="ad2b8-114">1. ábra – elosztott terhelésű végpont a webes forgalom</span><span class="sxs-lookup"><span data-stu-id="ad2b8-114">Figure 1 - Load-balanced endpoint for web traffic</span></span>

<span data-ttu-id="ad2b8-115">Internetes ügyfelek toohello nyilvános IP-cím hello felhőszolgáltatás küldött weblap kérelmek 80-as porton, hello Azure terheléselosztó hello kérelmek hello három virtuális gépek hello elosztott terhelésű készlet között osztja el.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-115">When Internet clients send web page requests toohello public IP address of hello cloud service on TCP port 80, hello Azure Load Balancer distributes hello requests between hello three virtual machines in hello load-balanced set.</span></span> <span data-ttu-id="ad2b8-116">A load balancer algoritmusok kapcsolatos további információkért lásd: hello [load balancer áttekintése lapon](load-balancer-overview.md#load-balancer-features).</span><span class="sxs-lookup"><span data-stu-id="ad2b8-116">For more information about load balancer algorithms, see hello [load balancer overview page](load-balancer-overview.md#load-balancer-features).</span></span>

<span data-ttu-id="ad2b8-117">Alapértelmezés szerint az Azure Load Balancer osztja el a hálózati forgalom több virtuálisgép-példánya között.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-117">By default, Azure Load Balancer distributes network traffic equally among multiple virtual machine instances.</span></span> <span data-ttu-id="ad2b8-118">Beállíthatja úgy is munkamenet-kapcsolatot, további információkért lásd: [terheléselosztó terheléselosztási mód](load-balancer-distribution-mode.md).</span><span class="sxs-lookup"><span data-stu-id="ad2b8-118">You can also configure session affinity, For more information, see [load balancer distribution mode](load-balancer-distribution-mode.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ad2b8-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ad2b8-119">Next steps</span></span>

<span data-ttu-id="ad2b8-120">További tudnivalók [belső terheléselosztó](load-balancer-internal-overview.md) toobetter megértse, mely terheléselosztót a felhő üzembe helyezése jobban megfelelnek.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-120">Learn about [Internal load balancer](load-balancer-internal-overview.md) toobetter understand which load balancer is a better fit for your cloud deployment.</span></span>

<span data-ttu-id="ad2b8-121">Is [Internet felé néző terheléselosztó létrehozásához](load-balancer-get-started-internet-arm-ps.md) és konfigurálása, hogy milyen típusú [mód](load-balancer-distribution-mode.md) egy meghatározott típusú terheléselosztó hálózati forgalom viselkedését.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-121">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for an specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="ad2b8-122">Ha az alkalmazásnak tookeep kapcsolatok életben kiszolgálók egy terheléselosztó mögött, akkor is jobban átláthatja kapcsolatos [üresjárati TCP időtúllépési beállítások terheléselosztó](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="ad2b8-122">If your application needs tookeep connections alive for servers behind a load balancer, you can understand more about [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="ad2b8-123">Az Azure Load Balancer használatakor segítségével toolearn kapcsolat üresjárati működése.</span><span class="sxs-lookup"><span data-stu-id="ad2b8-123">It will help toolearn about idle connection behavior when you are using Azure Load Balancer.</span></span>
