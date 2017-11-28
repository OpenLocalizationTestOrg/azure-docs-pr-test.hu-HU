---
title: "Azure felhőszolgáltatások aaaCreate egy internetre irányuló terheléselosztót |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy internetre terheléselosztó klasszikus üzembe helyezési modellben a cloud services csomag"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 0bb16f96-56a6-429f-88f5-0de2d0136756
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: d93cf76d417cbfc744cf07ba48c43a63cc14df69
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a><span data-ttu-id="372b1-103">Hozzon létre egy internetkapcsolattal rendelkező terheléselosztót felhőszolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="372b1-103">Get started creating an Internet facing load balancer for cloud services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="372b1-104">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="372b1-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="372b1-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="372b1-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="372b1-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="372b1-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="372b1-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="372b1-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="372b1-108">Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Azure Resource Manager és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="372b1-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="372b1-109">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="372b1-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="372b1-110">Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello.</span><span class="sxs-lookup"><span data-stu-id="372b1-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="372b1-111">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="372b1-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="372b1-112">Emellett [megtudhatja, hogyan toocreate egy internetre terheléselosztó Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="372b1-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

<span data-ttu-id="372b1-113">Cloud services csomag terheléselosztót automatikusan konfigurálva, és testre szabható hello modell használatával.</span><span class="sxs-lookup"><span data-stu-id="372b1-113">Cloud services are automatically configured with a load balancer and can be customized via hello service model.</span></span>

## <a name="create-a-load-balancer-using-hello-service-definition-file"></a><span data-ttu-id="372b1-114">Hozzon létre egy terhelés-kiegyenlítő hello szolgáltatásdefiníciós fájl használatával</span><span class="sxs-lookup"><span data-stu-id="372b1-114">Create a load balancer using hello service definition file</span></span>

<span data-ttu-id="372b1-115">Hello Azure SDK-t a .NET 2.5 tooupdate kihasználhatják a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="372b1-115">You can leverage hello Azure SDK for .NET 2.5 tooupdate your cloud service.</span></span> <span data-ttu-id="372b1-116">A felhőszolgáltatások végpontbeállításokat végzett hello [webszolgáltatás](https://msdn.microsoft.com/library/azure/gg557553.aspx) .csdef fájl.</span><span class="sxs-lookup"><span data-stu-id="372b1-116">Endpoint settings for cloud services are made in hello [service definition](https://msdn.microsoft.com/library/azure/gg557553.aspx) .csdef file.</span></span>

<span data-ttu-id="372b1-117">a következő példa azt mutatja meg, hogyan van konfigurálva a felhő üzembe helyezése servicedefinition.csdef fájl hello:</span><span class="sxs-lookup"><span data-stu-id="372b1-117">hello following example shows how a servicedefinition.csdef file for a cloud deployment is configured:</span></span>

<span data-ttu-id="372b1-118">Hello részlet hello .csdef fájl egy felhő üzembe helyezése által generált ellenőrzése és a hello beállított külső végpont toouse portok HTTP 10000 10001 és 10002 porton tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="372b1-118">Checking hello snippet for hello .csdef file generated by a cloud deployment, you can see hello external endpoint configured toouse ports HTTP on port 10000, 10001, and 10002.</span></span>

```xml
<ServiceDefinition name=“Tenant“>
    <WorkerRole name=“FERole” vmsize=“Small“>
<Endpoints>
    <InputEndpoint name=“FE_External_Http” protocol=“http” port=“10000“ />
    <InputEndpoint name=“FE_External_Tcp“  protocol=“tcp“  port=“10001“ />
    <InputEndpoint name=“FE_External_Udp“  protocol=“udp“  port=“10002“ />

    <InputEndpointname=“HTTP_Probe” protocol=“http” port=“80” loadBalancerProbe=“MyProbe“ />

    <InstanceInputEndpoint name=“InstanceEP” protocol=“tcp” localPort=“80“>
        <AllocatePublicPortFrom>
            <FixedPortRange min=“10110” max=“10120“  />
        </AllocatePublicPortFrom>
    </InstanceInputEndpoint>
    <InternalEndpoint name=“FE_InternalEP_Tcp” protocol=“tcp“ />
</Endpoints>
    </WorkerRole>
</ServiceDefinition>
```

## <a name="check-load-balancer-health-status-for-cloud-services"></a><span data-ttu-id="372b1-119">Felhőszolgáltatások állapotinformációinak ellenőrzése a terheléselosztóban</span><span class="sxs-lookup"><span data-stu-id="372b1-119">Check load balancer health status for cloud services</span></span>

<span data-ttu-id="372b1-120">hello az alábbiakban látható egy példa egy állapotmintáihoz:</span><span class="sxs-lookup"><span data-stu-id="372b1-120">hello following is an example of a health probe:</span></span>

```xml
<LoadBalancerProbes>
    <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
</LoadBalancerProbes>
```

<span data-ttu-id="372b1-121">hello terheléselosztó egyesíti hello hello végpont és hello adatokat hello mintavételi toocreate az egy URL-cím formájában hello `http://{DIP of VM}:80/Probe.aspx` , amely lehet használt tooquery hello hello szolgáltatásának állapotát.</span><span class="sxs-lookup"><span data-stu-id="372b1-121">hello load balancer combines hello information of hello endpoint and hello information of hello probe toocreate a URL in hello form of `http://{DIP of VM}:80/Probe.aspx` that can be used tooquery hello health of hello service.</span></span>

<span data-ttu-id="372b1-122">hello szolgáltatás észleli a hello rendszeres mintavételt azonos IP-címet.</span><span class="sxs-lookup"><span data-stu-id="372b1-122">hello service detects periodic probes from hello same IP address.</span></span> <span data-ttu-id="372b1-123">Ez a hello állapotfigyelő mintavételi kérelem érkező hello állomás hello csomópont hello virtuális gépet futtató.</span><span class="sxs-lookup"><span data-stu-id="372b1-123">This is hello health probe request coming from hello host of hello node where hello virtual machine is running.</span></span> <span data-ttu-id="372b1-124">hello szolgáltatást a 200-as HTTP-állapotkód: hello load balancer tooassume, hogy hello szolgáltatás állapota megfelelő a(z) toorespond rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="372b1-124">hello service has toorespond with a HTTP 200 status code for hello load balancer tooassume that hello service is healthy.</span></span> <span data-ttu-id="372b1-125">Egyéb HTTP-állapot (például 503-as) code, közvetlen vesz hello Elforgatás virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="372b1-125">Any other HTTP status code (for example 503) directly takes hello virtual machine out of rotation.</span></span>

<span data-ttu-id="372b1-126">hello mintavételi definition hello mintavételi gyakoriság hello is vezérli.</span><span class="sxs-lookup"><span data-stu-id="372b1-126">hello probe definition also controls hello frequency of hello probe.</span></span> <span data-ttu-id="372b1-127">A fenti esetben hello terheléselosztó van probing hello végpont minden 5 másodperc.</span><span class="sxs-lookup"><span data-stu-id="372b1-127">In our case above, hello load balancer is probing hello endpoint every 5 secs.</span></span> <span data-ttu-id="372b1-128">Ha pozitív válasz érkezett 10 másodperc (két mintavételi időközönként), hello mintavételi feltételezett le, és az hello virtuális gép van Elforgatás kívül.</span><span class="sxs-lookup"><span data-stu-id="372b1-128">If no positive answer is received for 10 secs (two probe intervals), hello probe is assumed down, and hello virtual machine is taken out of rotation.</span></span> <span data-ttu-id="372b1-129">Hasonlóképpen ha hello szolgáltatás Elforgatás kívül esik, és pozitív válasz érkezett, hello szolgáltatást van kerüljenek vissza toorotation azonnal.</span><span class="sxs-lookup"><span data-stu-id="372b1-129">Similarly, if hello service is out of rotation and a positive answer is received, hello service is put back toorotation right away.</span></span> <span data-ttu-id="372b1-130">Hello szolgáltatást van ingadozó közötti megfelelő és nem kifogástalan, ha hello terheléselosztó toodelay hello újratelepítését hello szolgáltatás hátsó toorotation eldöntheti, amíg megfelelő mintavételt számú lett.</span><span class="sxs-lookup"><span data-stu-id="372b1-130">If hello service is fluctuating between healthy and unhealthy, hello load balancer can decide toodelay hello re-introduction of hello service back toorotation until it has been healthy for a number of probes.</span></span>

<span data-ttu-id="372b1-131">Nézze meg hello szolgáltatás definíciós sémában hello [állapotmintáihoz](https://msdn.microsoft.com/library/azure/jj151530.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="372b1-131">Check hello service definition schema for hello [health probe](https://msdn.microsoft.com/library/azure/jj151530.aspx) for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="372b1-132">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="372b1-132">Next steps</span></span>

[<span data-ttu-id="372b1-133">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="372b1-133">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="372b1-134">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="372b1-134">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="372b1-135">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="372b1-135">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

