---
title: "Belső terheléselosztó létrehozása az Azure Cloud Serviceshez | Microsoft Docs"
description: "Ismerje meg, hogyan hozható létre belső terheléselosztó a PowerShell használatával a klasszikus üzembehelyezési modellben"
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
ms.openlocfilehash: 8dbc951416d577fa7f534c2eab1605c6bee61fce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-for-cloud-services"></a><span data-ttu-id="a37be-103">Bevezetés a belső terheléselosztó (klasszikus) felhőszolgáltatásokhoz történő létrehozásába</span><span class="sxs-lookup"><span data-stu-id="a37be-103">Get started creating an internal load balancer (classic) for cloud services</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a37be-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a37be-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="a37be-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a37be-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="a37be-106">Felhőszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="a37be-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

> [!IMPORTANT]
> <span data-ttu-id="a37be-107">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a37be-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="a37be-108">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="a37be-108">This article covers using the classic deployment model.</span></span> <span data-ttu-id="a37be-109">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="a37be-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="a37be-110">Ismerje meg, [hogyan hajthatja végre ezeket a lépéseket a Resource Manager-modell használatával](load-balancer-get-started-ilb-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="a37be-110">Learn how to [perform these steps using the Resource Manager model](load-balancer-get-started-ilb-arm-ps.md).</span></span>

## <a name="configure-internal-load-balancer-for-cloud-services"></a><span data-ttu-id="a37be-111">Belső terheléselosztó konfigurálása a felhőszolgáltatásokhoz</span><span class="sxs-lookup"><span data-stu-id="a37be-111">Configure internal load balancer for cloud services</span></span>

<span data-ttu-id="a37be-112">A belső terheléselosztó használata virtuális gépek és felhőszolgáltatások esetén egyaránt támogatott.</span><span class="sxs-lookup"><span data-stu-id="a37be-112">Internal load balancer is supported for both virtual machines and cloud services.</span></span> <span data-ttu-id="a37be-113">Egy regionális virtuális hálózaton kívül eső felhőszolgáltatásban létrehozott belső terheléselosztói végpont csak az adott felhőszolgáltatásban érhető el.</span><span class="sxs-lookup"><span data-stu-id="a37be-113">An internal load balancer endpoint created in a cloud service that is outside a regional virtual network will be accessible only within the cloud service.</span></span>

<span data-ttu-id="a37be-114">A belső terheléselosztó konfigurációját be kell állítania az alábbi mintában látható módon, amikor az első telepítést létrehozza a felhőszolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="a37be-114">The internal load balancer configuration has to be set during the creation of the first deployment in the cloud service, as shown in the sample below.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a37be-115">Az alábbi lépések futtatásának előfeltétele, hogy a felhőtelepítéshez már létre legyen hozva egy virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="a37be-115">A prerequisite to run the steps below is to have a virtual network already created for the cloud deployment.</span></span> <span data-ttu-id="a37be-116">A belső terheléselosztás létrehozásához szüksége lesz a virtuális hálózat és az alhálózat nevére.</span><span class="sxs-lookup"><span data-stu-id="a37be-116">You will need the virtual network name and subnet name to create the Internal Load Balancing.</span></span>

### <a name="step-1"></a><span data-ttu-id="a37be-117">1. lépés</span><span class="sxs-lookup"><span data-stu-id="a37be-117">Step 1</span></span>

<span data-ttu-id="a37be-118">Nyissa meg a felhőtelepítéshez szükséges szolgáltatáskonfigurációs fájlt (.cscfg) a Visual Studióban, és a hálózat konfigurálásához adja hozzá a következő szakaszt az utolsó „`</Role>`” elem alatt, hogy létrehozhassa a belső terheléselosztást.</span><span class="sxs-lookup"><span data-stu-id="a37be-118">Open the service configuration file (.cscfg) for your cloud deployment in Visual Studio and add the following section to create the Internal Load Balancing under the last "`</Role>`" item for the network configuration.</span></span>

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="name of the load balancer">
        <FrontendIPConfiguration type="private" subnet="subnet-name" staticVirtualNetworkIPAddress="static-IP-address"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

<span data-ttu-id="a37be-119">Adja meg a hálózat konfigurációs fájljához szükséges értékeket, hogy lássa, hogyan fog kinézni.</span><span class="sxs-lookup"><span data-stu-id="a37be-119">Let's add the values for the network configuration file to show how it will look.</span></span> <span data-ttu-id="a37be-120">A példában feltételeztük, létrehozott egy „test_vnet” nevű virtuális hálózatot, amely egy test_subnet nevű 10.0.0.0/24 alhálózatot tartalmaz, és a statikus IP-címe 10.0.0.4.</span><span class="sxs-lookup"><span data-stu-id="a37be-120">In the example, assume you created a VNet called "test_vnet" with a subnet 10.0.0.0/24 called test_subnet and a static IP 10.0.0.4.</span></span> <span data-ttu-id="a37be-121">A terheléselosztó elnevezése testLB lesz.</span><span class="sxs-lookup"><span data-stu-id="a37be-121">The load balancer will be named testLB.</span></span>

```xml
<NetworkConfiguration>
    <LoadBalancers>
    <LoadBalancer name="testLB">
        <FrontendIPConfiguration type="private" subnet="test_subnet" staticVirtualNetworkIPAddress="10.0.0.4"/>
    </LoadBalancer>
    </LoadBalancers>
</NetworkConfiguration>
```

<span data-ttu-id="a37be-122">A terheléselosztó sémájával kapcsolatos további információkért lásd: [Add load balancer](https://msdn.microsoft.com/library/azure/dn722411.aspx) (Terheléselosztó hozzáadása).</span><span class="sxs-lookup"><span data-stu-id="a37be-122">For more information about the load balancer schema, see [Add load balancer](https://msdn.microsoft.com/library/azure/dn722411.aspx).</span></span>

### <a name="step-2"></a><span data-ttu-id="a37be-123">2. lépés</span><span class="sxs-lookup"><span data-stu-id="a37be-123">Step 2</span></span>

<span data-ttu-id="a37be-124">Ha végpontokat szeretne hozzáadni a belső terheléselosztáshoz, módosítsa a szolgáltatásdefiníciós fájlt (.csdef).</span><span class="sxs-lookup"><span data-stu-id="a37be-124">Change the service definition (.csdef) file to add endpoints to the Internal Load Balancing.</span></span> <span data-ttu-id="a37be-125">Szerepkörpéldány létrehozásakor a szolgáltatásdefiníciós fájl hozzáadja a szerepkörpéldányokat a belső terheléselosztáshoz.</span><span class="sxs-lookup"><span data-stu-id="a37be-125">The moment a role instance is created, the service definition file will add the role instances to the Internal Load Balancing.</span></span>

```xml
<WorkerRole name="worker-role-name" vmsize="worker-role-size" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="input-endpoint-name" protocol="[http|https|tcp|udp]" localPort="local-port-number" port="port-number" certificate="certificate-name" loadBalancerProbe="load-balancer-probe-name" loadBalancer="load-balancer-name" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="a37be-126">A fenti példa értékeit felhasználva adja hozzá az értékeket a szolgáltatásdefiníciós fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="a37be-126">Following the same values from the example above, let's add the values to the service definition file.</span></span>

```xml
<WorkerRole name="WorkerRole1" vmsize="A7" enableNativeCodeExecution="[true|false]">
    <Endpoints>
    <InputEndpoint name="endpoint1" protocol="http" localPort="80" port="80" loadBalancer="testLB" />
    </Endpoints>
</WorkerRole>
```

<span data-ttu-id="a37be-127">A hálózati forgalom terhelésének elosztása a testLB terheléselosztóval végezhető el, ehhez a 80-as portot kell használni a bejövő kérelmek fogadásához, valamint a feldolgozói szerepkörpéldányokra való küldést szintén a 80-as porton keresztül kell végezni.</span><span class="sxs-lookup"><span data-stu-id="a37be-127">The network traffic will be load balanced using the testLB load balancer using port 80 for incoming requests, sending to worker role instances also on port 80.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a37be-128">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a37be-128">Next steps</span></span>

[<span data-ttu-id="a37be-129">A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával</span><span class="sxs-lookup"><span data-stu-id="a37be-129">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="a37be-130">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a37be-130">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

