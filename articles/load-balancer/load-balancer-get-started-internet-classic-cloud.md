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
# <a name="get-started-creating-an-internet-facing-load-balancer-for-cloud-services"></a>Hozzon létre egy internetkapcsolattal rendelkező terheléselosztót felhőszolgáltatásokhoz

> [!div class="op_single_selector"]
> * [klasszikus Azure portál](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Azure Resource Manager és klasszikus. Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal. Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello. Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben. Emellett [megtudhatja, hogyan toocreate egy internetre terheléselosztó Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).

Cloud services csomag terheléselosztót automatikusan konfigurálva, és testre szabható hello modell használatával.

## <a name="create-a-load-balancer-using-hello-service-definition-file"></a>Hozzon létre egy terhelés-kiegyenlítő hello szolgáltatásdefiníciós fájl használatával

Hello Azure SDK-t a .NET 2.5 tooupdate kihasználhatják a felhőalapú szolgáltatás. A felhőszolgáltatások végpontbeállításokat végzett hello [webszolgáltatás](https://msdn.microsoft.com/library/azure/gg557553.aspx) .csdef fájl.

a következő példa azt mutatja meg, hogyan van konfigurálva a felhő üzembe helyezése servicedefinition.csdef fájl hello:

Hello részlet hello .csdef fájl egy felhő üzembe helyezése által generált ellenőrzése és a hello beállított külső végpont toouse portok HTTP 10000 10001 és 10002 porton tekintheti meg.

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

## <a name="check-load-balancer-health-status-for-cloud-services"></a>Felhőszolgáltatások állapotinformációinak ellenőrzése a terheléselosztóban

hello az alábbiakban látható egy példa egy állapotmintáihoz:

```xml
<LoadBalancerProbes>
    <LoadBalancerProbe name=“MyProbe” protocol=“http” path=“Probe.aspx” intervalInSeconds=“5” timeoutInSeconds=“100“ />
</LoadBalancerProbes>
```

hello terheléselosztó egyesíti hello hello végpont és hello adatokat hello mintavételi toocreate az egy URL-cím formájában hello `http://{DIP of VM}:80/Probe.aspx` , amely lehet használt tooquery hello hello szolgáltatásának állapotát.

hello szolgáltatás észleli a hello rendszeres mintavételt azonos IP-címet. Ez a hello állapotfigyelő mintavételi kérelem érkező hello állomás hello csomópont hello virtuális gépet futtató. hello szolgáltatást a 200-as HTTP-állapotkód: hello load balancer tooassume, hogy hello szolgáltatás állapota megfelelő a(z) toorespond rendelkezik. Egyéb HTTP-állapot (például 503-as) code, közvetlen vesz hello Elforgatás virtuális gép.

hello mintavételi definition hello mintavételi gyakoriság hello is vezérli. A fenti esetben hello terheléselosztó van probing hello végpont minden 5 másodperc. Ha pozitív válasz érkezett 10 másodperc (két mintavételi időközönként), hello mintavételi feltételezett le, és az hello virtuális gép van Elforgatás kívül. Hasonlóképpen ha hello szolgáltatás Elforgatás kívül esik, és pozitív válasz érkezett, hello szolgáltatást van kerüljenek vissza toorotation azonnal. Hello szolgáltatást van ingadozó közötti megfelelő és nem kifogástalan, ha hello terheléselosztó toodelay hello újratelepítését hello szolgáltatás hátsó toorotation eldöntheti, amíg megfelelő mintavételt számú lett.

Nézze meg hello szolgáltatás definíciós sémában hello [állapotmintáihoz](https://msdn.microsoft.com/library/azure/jj151530.aspx) további információt.

## <a name="next-steps"></a>Következő lépések

[Bevezetés a belső terheléselosztók konfigurálásába](load-balancer-get-started-ilb-arm-ps.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)

