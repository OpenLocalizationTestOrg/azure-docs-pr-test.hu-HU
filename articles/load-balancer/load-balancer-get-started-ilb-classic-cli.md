---
title: "aaaCreate egy belső terheléselosztó - klasszikus Azure CLI |} Microsoft Docs"
description: "Ismerje meg, hogyan egy belső terheléselosztó használatával toocreate hello hello klasszikus üzembe helyezési modellel Azure parancssori felület"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: becbbbde-a118-4269-9444-d3153f00bf34
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: ef29dfda5f7a75a411bbabe8b688a31c6bf81113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internal-load-balancer-classic-using-hello-azure-cli"></a><span data-ttu-id="69227-103">Elsajátíthatja a belső terheléselosztót (klasszikus) hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="69227-103">Get started creating an internal load balancer (classic) using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="69227-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="69227-104">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [<span data-ttu-id="69227-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="69227-105">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [<span data-ttu-id="69227-106">Felhőszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="69227-106">Cloud services</span></span>](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="69227-107">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="69227-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="69227-108">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="69227-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="69227-109">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="69227-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="69227-110">Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](load-balancer-get-started-ilb-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="69227-110">Learn how too[perform these steps using hello Resource Manager model](load-balancer-get-started-ilb-arm-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="toocreate-an-internal-load-balancer-set-for-virtual-machines"></a><span data-ttu-id="69227-111">toocreate belső terheléselosztót beállítani a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="69227-111">toocreate an internal load balancer set for virtual machines</span></span>

<span data-ttu-id="69227-112">belső terheléselosztót beállítani, és szeretne küldeni a forgalom tooit kiszolgálók hello toocreate, el kell végeznie a következő hello:</span><span class="sxs-lookup"><span data-stu-id="69227-112">toocreate an internal load balancer set and hello servers that will send their traffic tooit, you must do hello following:</span></span>

1. <span data-ttu-id="69227-113">Belső terheléselosztás hello végpont egy elosztott terhelésű készlet hello kiszolgálóin bejövő forgalom toobe terhelés leendő példányt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="69227-113">Create an instance of Internal Load Balancing that will be hello endpoint of incoming traffic toobe load balanced across hello servers of a load-balanced set.</span></span>
2. <span data-ttu-id="69227-114">Hello bejövő forgalmat fog kapni toohello virtuális gépek megfelelő végpont-hozzáadáshoz.</span><span class="sxs-lookup"><span data-stu-id="69227-114">Add endpoints corresponding toohello virtual machines that will be receiving hello incoming traffic.</span></span>
3. <span data-ttu-id="69227-115">Hello forgalom toobe az elosztott terhelésű toosend a forgalom toohello virtuális IP-cím (VIP) cím hello belső terheléselosztás példány küldi hello kiszolgálók konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="69227-115">Configure hello servers that will be sending hello traffic toobe load balanced toosend their traffic toohello virtual IP (VIP) address of hello Internal Load Balancing instance.</span></span>

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a><span data-ttu-id="69227-116">Belső terheléselosztó parancssori felület használatával történő létrehozásának lépései</span><span class="sxs-lookup"><span data-stu-id="69227-116">Step by step creating an internal load balancer using CLI</span></span>

<span data-ttu-id="69227-117">Ez az útmutató bemutatja, hogyan toocreate belső terheléselosztót alapján a fenti forgatókönyvben hello.</span><span class="sxs-lookup"><span data-stu-id="69227-117">This guide shows how toocreate an internal load balancer based on hello scenario above.</span></span>

1. <span data-ttu-id="69227-118">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="69227-118">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="69227-119">Futtassa a hello **azure config mód** tooswitch tooclassic üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="69227-119">Run hello **azure config mode** command tooswitch tooclassic mode, as shown below.</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="69227-120">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="69227-120">Expected output:</span></span>

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a><span data-ttu-id="69227-121">Végpont és terheléselosztó készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="69227-121">Create endpoint and load balancer set</span></span>

<span data-ttu-id="69227-122">hello forgatókönyv azt feltételezi, hogy a virtuális gépek "D1" és "DB2" hello "mytestcloud" nevű felhőszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="69227-122">hello scenario assumes hello virtual machines "DB1" and "DB2" in a cloud service called "mytestcloud".</span></span> <span data-ttu-id="69227-123">Mindkét virtuális gép a „testvnet” nevű virtuális hálózatot használja, a „alhalozat-1” nevű alhálózattal.</span><span class="sxs-lookup"><span data-stu-id="69227-123">Both virtual machines are using a virtual network called my "testvnet" with subnet "subnet-1".</span></span>

<span data-ttu-id="69227-124">Ez az útmutató létrehoz egy belső terheléselosztó készletet: az 1433-as portot használja nyilvános portként, és az 1433-as portot használja helyi portként.</span><span class="sxs-lookup"><span data-stu-id="69227-124">This guide will create an internal load balancer set using port 1433 as private port and 1433 as local port.</span></span>

<span data-ttu-id="69227-125">Ez egy webfarmos közös hello háttér használata egy belső terheléselosztó tooguarantee hello adatbázis-kiszolgálók nem érhetők el közvetlenül a egy nyilvános IP-címet használja az SQL virtuális gépek esetében.</span><span class="sxs-lookup"><span data-stu-id="69227-125">This is a common scenario where you have SQL virtual machines on hello back end using an internal load balancer tooguarantee hello database servers won't be exposed directly using a public IP address.</span></span>

### <a name="step-1"></a><span data-ttu-id="69227-126">1. lépés</span><span class="sxs-lookup"><span data-stu-id="69227-126">Step 1</span></span>

<span data-ttu-id="69227-127">Belső terheléselosztó készlet létrehozása a következő használatával: `azure network service internal-load-balancer add`.</span><span class="sxs-lookup"><span data-stu-id="69227-127">Create an internal load balancer set using `azure network service internal-load-balancer add`.</span></span>

```azurecli
azure service internal-load-balancer add --serviceName mytestcloud --internalLBName ilbset --subnet-name subnet-1 --static-virtualnetwork-ipaddress 192.168.2.7
```

<span data-ttu-id="69227-128">További információ: `azure service internal-load-balancer --help`.</span><span class="sxs-lookup"><span data-stu-id="69227-128">Check out `azure service internal-load-balancer --help` for more information.</span></span>

<span data-ttu-id="69227-129">Hello belső terheléselosztó tulajdonságai hello paranccsal ellenőrizheti `azure service internal-load-balancer list` *felhőszolgáltatás neve*.</span><span class="sxs-lookup"><span data-stu-id="69227-129">You can check hello internal load balancer properties using hello command `azure service internal-load-balancer list` *cloud service name*.</span></span>

<span data-ttu-id="69227-130">Hello kimeneti példát a következő:</span><span class="sxs-lookup"><span data-stu-id="69227-130">Here follows an example of hello output:</span></span>

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


### <a name="step-2"></a><span data-ttu-id="69227-131">2. lépés</span><span class="sxs-lookup"><span data-stu-id="69227-131">Step 2</span></span>

<span data-ttu-id="69227-132">Hello belső elosztott terhelésű készlet a hello első végpont hozzáadásakor konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="69227-132">You configure hello internal load balancer set when you add hello first endpoint.</span></span> <span data-ttu-id="69227-133">Hello végpont, a virtuális gép és a mintavételi portot toohello belső elosztott terhelésű készlet ebben a lépésben fog társítani.</span><span class="sxs-lookup"><span data-stu-id="69227-133">You will associate hello endpoint, virtual machine and probe port toohello internal load balancer set in this step.</span></span>

```azurecli
azure vm endpoint create db1 1433 --local-port 1433 --protocol tcp --probe-port 1433 --probe-protocol tcp --probe-interval 300 --probe-timeout 600 --internal-load-balancer-name ilbset
```

### <a name="step-3"></a><span data-ttu-id="69227-134">3. lépés</span><span class="sxs-lookup"><span data-stu-id="69227-134">Step 3</span></span>

<span data-ttu-id="69227-135">Ellenőrizze a hello terhelés terheléselosztó használt konfiguráció `azure vm show` *virtuális gép neve*</span><span class="sxs-lookup"><span data-stu-id="69227-135">Verify hello load balancer configuration using `azure vm show` *virtual machine name*</span></span>

```azurecli
azure vm show DB1
```

<span data-ttu-id="69227-136">hello kimenet lesz:</span><span class="sxs-lookup"><span data-stu-id="69227-136">hello output will be:</span></span>

    azure vm show DB1
    info:    Executing command vm show
    + Getting virtual machines
    data:    DNSName "mytestcloud.cloudapp.net"
    data:    Location "East US 2"
    data:    VMName "DB1"
    data:    IPAddress "192.168.2.4"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "db1-DB1-0-201511120457370846"
    data:    OSDisk mediaLink "https://XXXX.blob.core.windows.net/vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-20151022-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "137.116.64.107"
    data:    VirtualIPAddresses 0 name "db1ContractContract"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    VirtualIPAddresses 1 address "192.168.2.7"
    data:    VirtualIPAddresses 1 name "ilbset"
    data:    Network Endpoints 0 localPort 5986
    data:    Network Endpoints 0 name "PowerShell"
    data:    Network Endpoints 0 port 5986
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 3389
    data:    Network Endpoints 1 name "Remote Desktop"
    data:    Network Endpoints 1 port 60173
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "137.116.64.107"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 1433
    data:    Network Endpoints 2 name "tcp-1433-1433"
    data:    Network Endpoints 2 port 1433
    data:    Network Endpoints 2 loadBalancerProbe port 1433
    data:    Network Endpoints 2 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 2 loadBalancerProbe intervalInSeconds 300
    data:    Network Endpoints 2 loadBalancerProbe timeoutInSeconds 600
    data:    Network Endpoints 2 protocol "tcp"
    data:    Network Endpoints 2 virtualIPAddress "192.168.2.7"
    data:    Network Endpoints 2 enableDirectServerReturn false
    data:    Network Endpoints 2 loadBalancerName "ilbset"
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a><span data-ttu-id="69227-137">Hozzon létre egy távoli asztali végpontot a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="69227-137">Create a remote desktop endpoint for a virtual machine</span></span>

<span data-ttu-id="69227-138">Nyilvános port tooa helyi port, egy adott virtuális gép használata a távoli asztali végpont tooforward hálózati forgalom létrehozhat `azure vm endpoint create`.</span><span class="sxs-lookup"><span data-stu-id="69227-138">You can create a remote desktop endpoint tooforward network traffic from a public port tooa local port for a specific virtual machine using `azure vm endpoint create`.</span></span>

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a><span data-ttu-id="69227-139">Virtuális gép eltávolítása a terheléselosztóból</span><span class="sxs-lookup"><span data-stu-id="69227-139">Remove virtual machine from load balancer</span></span>

<span data-ttu-id="69227-140">A virtuális gépek eltávolíthatja a kapcsolódó hello végpont törlése által beállított belső terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="69227-140">You can remove a virtual machine from an internal load balancer set by deleting hello associated endpoint.</span></span> <span data-ttu-id="69227-141">Hello végpont eltávolítást követően hello virtuális gép toohello elosztott terhelésű készlet már nem tartozik.</span><span class="sxs-lookup"><span data-stu-id="69227-141">Once hello endpoint is removed, hello virtual machine won't belong toohello load balancer set anymore.</span></span>

<span data-ttu-id="69227-142">A fenti hello példáját eltávolíthat létrehozott virtuális gép "D1" hello végpont belső terheléselosztó "ilbset" hello paranccsal `azure vm endpoint delete`.</span><span class="sxs-lookup"><span data-stu-id="69227-142">Using hello example above, you can remove hello endpoint created for virtual machine "DB1" from internal load balancer "ilbset" by using hello command `azure vm endpoint delete`.</span></span>

```azurecli
azure vm endpoint delete DB1 tcp-1433-1433
```

<span data-ttu-id="69227-143">További információ: `azure vm endpoint --help`.</span><span class="sxs-lookup"><span data-stu-id="69227-143">Check out `azure vm endpoint --help` for more information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="69227-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="69227-144">Next steps</span></span>

[<span data-ttu-id="69227-145">A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával</span><span class="sxs-lookup"><span data-stu-id="69227-145">Configure a load balancer distribution mode using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="69227-146">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="69227-146">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
