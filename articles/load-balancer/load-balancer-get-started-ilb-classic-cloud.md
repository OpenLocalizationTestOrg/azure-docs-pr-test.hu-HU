---
title: "Azure-szolgáltatásokhoz belső terheléselosztót aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy belső terheléselosztó hello klasszikus üzembe helyezési modellben a PowerShell használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: 57966056-0f46-4f95-a295-483ca1ad135d
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: fe7975bca7bec3248626b0ad0fad6823e278ade2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a><span data-ttu-id="05320-103">Bevezetés a belső terheléselosztó (klasszikus) felhőszolgáltatásokhoz történő létrehozásába</span><span class="sxs-lookup"><span data-stu-id="05320-103">Get started creating an internal load balancer (classic) for cloud services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="05320-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="05320-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="05320-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="05320-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="05320-106">Felhőszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="05320-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

> [!IMPORTANT]
> <span data-ttu-id="05320-107">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="05320-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="05320-108">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="05320-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="05320-109">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="05320-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="05320-110">Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](load-balancer-get-started-ilb-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="05320-110">Learn how too[perform these steps using hello Resource Manager model](load-balancer-get-started-ilb-arm-ps.md).</span></span>

## <a name="configure-internal-load-balancer-for-cloud-services"></a><span data-ttu-id="05320-111">Belső terheléselosztó konfigurálása a felhőszolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="05320-111">Configure internal load balancer for cloud services</span></span>

<span data-ttu-id="05320-112">A belső terheléselosztó használata virtuális gépek és felhőszolgáltatások esetén egyaránt támogatott.</span><span class="sxs-lookup"><span data-stu-id="05320-112">Internal load balancer is supported for both virtual machines and cloud services.</span></span> <span data-ttu-id="05320-113">Belső terheléselosztó a végpont egy felhőalapú szolgáltatás, amely a regionális virtuális hálózatokon kívül létrehozott lesz csak a felhőszolgáltatáshoz hello belülről érhetők el.</span><span class="sxs-lookup"><span data-stu-id="05320-113">An internal load balancer endpoint created in a cloud service that is outside a regional virtual network will be accessible only within hello cloud service.</span></span>

<span data-ttu-id="05320-114">belső terheléselosztó-konfiguráció hello beállítása hello létrehozása hello hello felhőalapú szolgáltatás, az első központi telepítése során, ahogy az alábbi minta hello toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="05320-114">hello internal load balancer configuration has toobe set during hello creation of hello first deployment in hello cloud service, as shown in hello sample below.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05320-115">Egy előfeltétel toorun hello lépéseket toohave hello felhő üzembe helyezése már létrehozott egy virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="05320-115">A prerequisite toorun hello steps below is toohave a virtual network already created for hello cloud deployment.</span></span> <span data-ttu-id="05320-116">Virtuális hálózat neve és az alhálózati név toocreate hello belső terheléselosztás hello kell.</span><span class="sxs-lookup"><span data-stu-id="05320-116">You will need hello virtual network name and subnet name toocreate hello Internal Load Balancing.</span></span>

### <a name="step-1"></a><span data-ttu-id="05320-117">1. lépés</span><span class="sxs-lookup"><span data-stu-id="05320-117">Step 1</span></span>

<span data-ttu-id="05320-118">Nyissa meg a felhő üzembe helyezése a Visual Studio hello szolgáltatás konfigurációs fájlját (.cscfg), és adja hozzá a következő szakasz toocreate hello belső terheléselosztás, a hello utolsó hello "`</Role>`" hello hálózati konfigurációs elemet.</span><span class="sxs-lookup"><span data-stu-id="05320-118">Open hello service configuration file (.cscfg) for your cloud deployment in Visual Studio and add hello following section toocreate hello Internal Load Balancing under hello last "`</Role>`" item for hello network configuration.</span></span>

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="name of hello load balancer">
        <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

<span data-ttu-id="05320-119">Adjunk hello hálózati konfigurációs fájl tooshow megjelenését hello értékeit.</span><span class="sxs-lookup"><span data-stu-id="05320-119">Let's add hello values for hello network configuration file tooshow how it will look.</span></span> <span data-ttu-id="05320-120">Hello példában feltételezzük létrehozott egy "test_vnet" meghívva egy alhálózat 10.0.0.0/24 test_subnet és egy statikus IP-cím 10.0.0.4 nevű Vnetet.</span><span class="sxs-lookup"><span data-stu-id="05320-120">In hello example, assume you created a VNet called "test_vnet" with a subnet 10.0.0.0/24 called test_subnet and a static IP 10.0.0.4.</span></span> <span data-ttu-id="05320-121">hello terheléselosztó testLB lesznek elnevezve.</span><span class="sxs-lookup"><span data-stu-id="05320-121">hello load balancer will be named testLB.</span></span>

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="testLB">
        <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

<span data-ttu-id="05320-122">Hello load balancer séma kapcsolatos további információkért lásd: [Hozzáadás terheléselosztó](https://msdn.microsoft.com/library/azure/dn722411.aspx).</span><span class="sxs-lookup"><span data-stu-id="05320-122">For more information about hello load balancer schema, see [Add load balancer](https://msdn.microsoft.com/library/azure/dn722411.aspx).</span></span>

### <a name="step-2"></a><span data-ttu-id="05320-123">2. lépés</span><span class="sxs-lookup"><span data-stu-id="05320-123">Step 2</span></span>

<span data-ttu-id="05320-124">Hello szolgáltatás definíciós (.csdef) fájl tooadd végpontok toohello belső terheléselosztás módosítása.</span><span class="sxs-lookup"><span data-stu-id="05320-124">Change hello service definition (.csdef) file tooadd endpoints toohello Internal Load Balancing.</span></span> <span data-ttu-id="05320-125">hello szolgáltatásdefiníciós fájl hello szerepkör példányok toohello belső terheléselosztás ad hozzá, hello néhány percet a szerepkör példánya jön létre.</span><span class="sxs-lookup"><span data-stu-id="05320-125">hello moment a role instance is created, hello service definition file will add hello role instances toohello Internal Load Balancing.</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="05320-126">Következő hello ugyanaz a fenti példa hello értékei adjuk hozzá hello értékek toohello szolgáltatásdefiníciós fájlban.</span><span class="sxs-lookup"><span data-stu-id="05320-126">Following hello same values from hello example above, let's add hello values toohello service definition file.</span></span>

```xml
<WorkerRole name="WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="05320-127">hello hálózati forgalom használ a 80-as portot a bejövő kérelmeket küldő tooworker szerepkörpéldányokat is 80-as porton hello testLB terheléselosztó segítségével elosztott terhelésű lesz.</span><span class="sxs-lookup"><span data-stu-id="05320-127">hello network traffic will be load balanced using hello testLB load balancer using port 80 for incoming requests, sending tooworker role instances also on port 80.</span></span>

## <a name="next-steps"></a><span data-ttu-id="05320-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05320-128">Next steps</span></span>

[<span data-ttu-id="05320-129">A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával</span><span class="sxs-lookup"><span data-stu-id="05320-129">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="05320-130">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="05320-130">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

