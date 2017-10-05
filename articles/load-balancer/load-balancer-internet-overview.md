---
title: "Internetre irányuló load balancer áttekintése |} Microsoft Docs"
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
ms.openlocfilehash: c420b38fbe8054bc4b701f89ebc417677ca47a27
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="internet-facing-load-balancer-overview"></a><span data-ttu-id="f93f5-104">Az Internet felé néző terhelés terheléselosztó áttekintése</span><span class="sxs-lookup"><span data-stu-id="f93f5-104">Internet facing load balancer overview</span></span>

<span data-ttu-id="f93f5-105">Az Azure terheléselosztó van leképezve a nyilvános IP-cím és port száma bejövő forgalmat a magánhálózati IP-cím és port számát a virtuális gép, és ez fordítva is igaz a válasz forgalmat a virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="f93f5-105">Azure load balancer maps the public IP address and port number of incoming traffic to the private IP address and port number of the virtual machine and vice versa for the response traffic from the virtual machine.</span></span> <span data-ttu-id="f93f5-106">Terheléselosztási szabályok lehetővé teszik több virtuális gépek vagy szolgáltatások közötti forgalom adott típusú terjesztése.</span><span class="sxs-lookup"><span data-stu-id="f93f5-106">Load balancing rules allow you to distribute specific types of traffic between multiple virtual machines or services.</span></span> <span data-ttu-id="f93f5-107">Például a webes kérelem forgalom terhelését elosztva is több webkiszolgálók vagy webes szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="f93f5-107">For example, you can spread the load of web request traffic across multiple web servers or web roles.</span></span>

<span data-ttu-id="f93f5-108">Egy felhőalapú szolgáltatás, amely tartalmazza a webes szerepkörök vagy feldolgozói szerepkörök példányai egy nyilvános végpontot meghatározhatja a szolgáltatás definíciós (.csdef) fájlban.</span><span class="sxs-lookup"><span data-stu-id="f93f5-108">For a cloud service that contains instances of web roles or worker roles, you can define a public endpoint in the service definition (.csdef) file.</span></span>

<span data-ttu-id="f93f5-109">A *servicedefinition.csdef* fájl tartalmazza a végpont-konfiguráció, és ha több szerepkör példányát egy webes vagy feldolgozói szerepkör üzembe helyezése, a terheléselosztó lesz-e állítva.</span><span class="sxs-lookup"><span data-stu-id="f93f5-109">The *servicedefinition.csdef* file contains the endpoint configuration and when you have multiple role instances for a web or worker role deployment, the load balancer will be setup for it.</span></span> <span data-ttu-id="f93f5-110">Példányok hozzáadása a felhő üzembe helyezése úgy módosítja a példányok száma a szolgáltatás konfigurációs fájljában (.csfg).</span><span class="sxs-lookup"><span data-stu-id="f93f5-110">The way to add instances to your cloud deployment is changing the instance count on the service configuration file (.csfg).</span></span>

<span data-ttu-id="f93f5-111">Az alábbi ábrán egy elosztott terhelésű végpont a webes forgalomban, amely a nyilvános és titkos TCP port a 80-as három virtuális gép között meg van osztva.</span><span class="sxs-lookup"><span data-stu-id="f93f5-111">The following figure shows a load-balanced endpoint for web traffic that is shared among three virtual machines for the public and private TCP port of 80.</span></span> <span data-ttu-id="f93f5-112">A három virtuális gépek vannak egy elosztott terhelésű készlet.</span><span class="sxs-lookup"><span data-stu-id="f93f5-112">These three virtual machines are in a load-balanced set.</span></span>

![Példa nyilvános terheléselosztóra](./media/load-balancer-internet-overview/IC727496.png)

<span data-ttu-id="f93f5-114">1. ábra – elosztott terhelésű végpont a webes forgalom</span><span class="sxs-lookup"><span data-stu-id="f93f5-114">Figure 1 - Load-balanced endpoint for web traffic</span></span>

<span data-ttu-id="f93f5-115">Amikor internetes ügyfelek weblap kérést küld a nyilvános IP-cím 80-as TCP-porton a felhőalapú szolgáltatás, az Azure terheléselosztó a kérelmeket az elosztott terhelésű készlet a három virtuális gépek között osztja el.</span><span class="sxs-lookup"><span data-stu-id="f93f5-115">When Internet clients send web page requests to the public IP address of the cloud service on TCP port 80, the Azure Load Balancer distributes the requests between the three virtual machines in the load-balanced set.</span></span> <span data-ttu-id="f93f5-116">A load balancer algoritmusok kapcsolatos további információkért lásd: a [load balancer áttekintése lapon](load-balancer-overview.md#load-balancer-features).</span><span class="sxs-lookup"><span data-stu-id="f93f5-116">For more information about load balancer algorithms, see the [load balancer overview page](load-balancer-overview.md#load-balancer-features).</span></span>

<span data-ttu-id="f93f5-117">Alapértelmezés szerint az Azure Load Balancer osztja el a hálózati forgalom több virtuálisgép-példánya között.</span><span class="sxs-lookup"><span data-stu-id="f93f5-117">By default, Azure Load Balancer distributes network traffic equally among multiple virtual machine instances.</span></span> <span data-ttu-id="f93f5-118">Beállíthatja úgy is munkamenet-kapcsolatot, további információkért lásd: [terheléselosztó terheléselosztási mód](load-balancer-distribution-mode.md).</span><span class="sxs-lookup"><span data-stu-id="f93f5-118">You can also configure session affinity, For more information, see [load balancer distribution mode](load-balancer-distribution-mode.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="f93f5-119">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f93f5-119">Next steps</span></span>

<span data-ttu-id="f93f5-120">További tudnivalók [belső terheléselosztó](load-balancer-internal-overview.md) jobb megértése érdekében melyik terheléselosztó a felhő üzembe helyezése jobban megfelelnek.</span><span class="sxs-lookup"><span data-stu-id="f93f5-120">Learn about [Internal load balancer](load-balancer-internal-overview.md) to better understand which load balancer is a better fit for your cloud deployment.</span></span>

<span data-ttu-id="f93f5-121">Is [Internet felé néző terheléselosztó létrehozásához](load-balancer-get-started-internet-arm-ps.md) és konfigurálása, hogy milyen típusú [mód](load-balancer-distribution-mode.md) egy meghatározott típusú terheléselosztó hálózati forgalom viselkedését.</span><span class="sxs-lookup"><span data-stu-id="f93f5-121">You can also [get started creating an Internet facing load balancer](load-balancer-get-started-internet-arm-ps.md) and configure what type of [distribution mode](load-balancer-distribution-mode.md) for an specific load balancer network traffic behavior.</span></span>

<span data-ttu-id="f93f5-122">Ha az alkalmazásának fenn kell tartania a kapcsolatot a terheléselosztó mögött lévő kiszolgálókkal, többet megtudhat a [terheléselosztó üresjárati TCP-időkorlátjának beállításairól](load-balancer-tcp-idle-timeout.md).</span><span class="sxs-lookup"><span data-stu-id="f93f5-122">If your application needs to keep connections alive for servers behind a load balancer, you can understand more about [idle TCP timeout settings for a load balancer](load-balancer-tcp-idle-timeout.md).</span></span> <span data-ttu-id="f93f5-123">A segítségével jobban megismerheti az üresjárati kapcsolatok viselkedését az Azure Load Balancer használata során.</span><span class="sxs-lookup"><span data-stu-id="f93f5-123">It will help to learn about idle connection behavior when you are using Azure Load Balancer.</span></span>
