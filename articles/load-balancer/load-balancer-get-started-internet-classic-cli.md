---
title: "Internetkapcsolattal rendelkező terheléselosztó létrehozása – klasszikus Azure CLI | Microsoft Docs"
description: "Ismerje meg, hogyan hozható létre internetkapcsolattal rendelkező terheléselosztó klasszikus üzembehelyezési modellben az Azure parancssori felület használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-service-management
ms.assetid: e433a824-4a8a-44d2-8765-a74f52d4e584
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: da3a908f17ff5c6d3923549a884ecc0a13cb8e9e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-the-azure-cli"></a><span data-ttu-id="1d33b-103">Bevezetés az internetkapcsolattal rendelkező terheléselosztó (klasszikus) Azure parancssori felület használatával történő létrehozásába</span><span class="sxs-lookup"><span data-stu-id="1d33b-103">Get started creating an Internet facing load balancer (classic) in the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="1d33b-104">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="1d33b-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="1d33b-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="1d33b-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="1d33b-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1d33b-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="1d33b-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="1d33b-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="1d33b-108">Az Azure-erőforrásokkal való munka megkezdése előtt fontos megérteni, hogy az Azure jelenleg két üzembe helyezési modellel rendelkezik, a Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="1d33b-108">Before you work with Azure resources, it's important to understand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="1d33b-109">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="1d33b-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="1d33b-110">A különféle eszközök dokumentációit a cikk tetején található fülekre kattintva tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="1d33b-110">You can view the documentation for different tools by clicking the tabs at the top of this article.</span></span> <span data-ttu-id="1d33b-111">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="1d33b-111">This article covers the classic deployment model.</span></span> <span data-ttu-id="1d33b-112">Emellett [azt is megismerheti, hogyan lehet internetkapcsolattal rendelkező terheléselosztót létrehozni az Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="1d33b-112">You can also [Learn how to create an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a><span data-ttu-id="1d33b-113">Internetkapcsolattal rendelkező terheléselosztó parancssori felület használatával történő létrehozásának lépései</span><span class="sxs-lookup"><span data-stu-id="1d33b-113">Step by step creating an Internet facing load balancer using CLI</span></span>

<span data-ttu-id="1d33b-114">Ez az útmutató bemutatja, hogyan hozhat létre internetkapcsolattal rendelkező terheléselosztót a fenti forgatókönyv alapján.</span><span class="sxs-lookup"><span data-stu-id="1d33b-114">This guide shows how to create an Internet load balancer based on the scenario above.</span></span>

1. <span data-ttu-id="1d33b-115">Ha még sosem használta az Azure CLI-t, akkor tekintse meg [Install and Configure the Azure CLI](../cli-install-nodejs.md) (Az Azure CLI telepítése és konfigurálása) részt, és kövesse az utasításokat addig a pontig, ahol ki kell választania az Azure-fiókot és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="1d33b-115">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="1d33b-116">Az **azure config mode** parancs futtatásával váltson a klasszikus módra, a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="1d33b-116">Run the **azure config mode** command to switch to classic mode, as shown below.</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="1d33b-117">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="1d33b-117">Expected output:</span></span>

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a><span data-ttu-id="1d33b-118">Végpont és terheléselosztó készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d33b-118">Create endpoint and load balancer set</span></span>

<span data-ttu-id="1d33b-119">A forgatókönyv feltételezi, hogy létrehozták a „web1” és „web2” virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="1d33b-119">The scenario assumes the virtual machines "web1" and "web2" were created.</span></span>
<span data-ttu-id="1d33b-120">Ez az útmutató létrehoz egy terheléselosztó készletet: a 80-as portot használja nyilvános portként, és a 80-as portot használja helyi portként.</span><span class="sxs-lookup"><span data-stu-id="1d33b-120">This guide will create a load balancer set using port 80 as public port and port 80 as local port.</span></span> <span data-ttu-id="1d33b-121">A mintavételi port szintén a 80-as porton van konfigurálva, és a terheléselosztó készlet „lbset” nevet kapta.</span><span class="sxs-lookup"><span data-stu-id="1d33b-121">A probe port is also configured on port 80 and named the load balancer set "lbset".</span></span>

### <a name="step-1"></a><span data-ttu-id="1d33b-122">1. lépés</span><span class="sxs-lookup"><span data-stu-id="1d33b-122">Step 1</span></span>

<span data-ttu-id="1d33b-123">Hozza létre az első végpontot és terheléselosztó készletet a következő használatával a „web1” virtuális géphez: `azure network vm endpoint create`.</span><span class="sxs-lookup"><span data-stu-id="1d33b-123">Create the first endpoint and load balancer set using `azure network vm endpoint create` for virtual machine "web1".</span></span>

```azurecli
azure vm endpoint create web1 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-2"></a><span data-ttu-id="1d33b-124">2. lépés</span><span class="sxs-lookup"><span data-stu-id="1d33b-124">Step 2</span></span>

<span data-ttu-id="1d33b-125">Adjon hozzá egy második virtuális gépet a terheléselosztó készlethez „web2” néven.</span><span class="sxs-lookup"><span data-stu-id="1d33b-125">Add a second virtual machine "web2" to the load balancer set.</span></span>

```azurecli
azure vm endpoint create web2 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-3"></a><span data-ttu-id="1d33b-126">3. lépés</span><span class="sxs-lookup"><span data-stu-id="1d33b-126">Step 3</span></span>

<span data-ttu-id="1d33b-127">Ellenőrizze a terheléselosztó konfigurációját a következő használatával: `azure vm show`.</span><span class="sxs-lookup"><span data-stu-id="1d33b-127">Verify the load balancer configuration using `azure vm show` .</span></span>

```azurecli
azure vm show web1
```

<span data-ttu-id="1d33b-128">A kimenet a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="1d33b-128">The output will be:</span></span>

    data:    DNSName "contoso.cloudapp.net"
    data:    Location "East US"
    data:    VMName "web1"
    data:    IPAddress "10.0.0.5"
    data:    InstanceStatus "ReadyRole"
    data:    InstanceSize "Standard_D1"
    data:    Image "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-2015
    6-en.us-127GB.vhd"
    data:    OSDisk hostCaching "ReadWrite"
    data:    OSDisk name "joaoma-1-web1-0-201509251804250879"
    data:    OSDisk mediaLink "https://XXXXXXXXXXXXXXX.blob.core.windows.
    /vhds/joaomatest-web1-2015-09-25.vhd"
    data:    OSDisk sourceImageName "a699494373c04fc0bc8f2bb1389d6106__Windows-Se
    r-2012-R2-20150916-en.us-127GB.vhd"
    data:    OSDisk operatingSystem "Windows"
    data:    OSDisk iOType "Standard"
    data:    ReservedIPName ""
    data:    VirtualIPAddresses 0 address "XXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 name "XXXXXXXXXXXXXXXXXXXX"
    data:    VirtualIPAddresses 0 isDnsProgrammed true
    data:    Network Endpoints 0 loadBalancedEndpointSetName "lbset"
    data:    Network Endpoints 0 localPort 80
    data:    Network Endpoints 0 name "tcp-80-80"
    data:    Network Endpoints 0 port 80
    data:    Network Endpoints 0 loadBalancerProbe port 80
    data:    Network Endpoints 0 loadBalancerProbe protocol "tcp"
    data:    Network Endpoints 0 loadBalancerProbe intervalInSeconds 15
    data:    Network Endpoints 0 loadBalancerProbe timeoutInSeconds 31
    data:    Network Endpoints 0 protocol "tcp"
    data:    Network Endpoints 0 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 0 enableDirectServerReturn false
    data:    Network Endpoints 1 localPort 5986
    data:    Network Endpoints 1 name "PowerShell"
    data:    Network Endpoints 1 port 5986
    data:    Network Endpoints 1 protocol "tcp"
    data:    Network Endpoints 1 virtualIPAddress "XXXXXXXXXXXX"
    data:    Network Endpoints 1 enableDirectServerReturn false
    data:    Network Endpoints 2 localPort 3389
    data:    Network Endpoints 2 name "Remote Desktop"
    data:    Network Endpoints 2 port 58081
    info:    vm show command OK

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a><span data-ttu-id="1d33b-129">Hozzon létre egy távoli asztali végpontot a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="1d33b-129">Create a remote desktop endpoint for a virtual machine</span></span>

<span data-ttu-id="1d33b-130">Létrehozhat egy távoli asztali végpontot a hálózati forgalom nyilvános portról helyi portra történő továbbításához egy adott virtuális géphez a `azure vm endpoint create` parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="1d33b-130">You can create a remote desktop endpoint to forward network traffic from a public port to a local port for a specific virtual machine using `azure vm endpoint create`.</span></span>

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a><span data-ttu-id="1d33b-131">Virtuális gép eltávolítása a terheléselosztóból</span><span class="sxs-lookup"><span data-stu-id="1d33b-131">Remove virtual machine from load balancer</span></span>

<span data-ttu-id="1d33b-132">Törölnie kell a terheléselosztó készlethez társított végpontot a virtuális gépből.</span><span class="sxs-lookup"><span data-stu-id="1d33b-132">You have to delete the endpoint associated to the load balancer set from the virtual machine.</span></span> <span data-ttu-id="1d33b-133">Miután megtörtént a végpont eltávolítása, a virtuális gép többé nem tartozik a terheléselosztó készlethez.</span><span class="sxs-lookup"><span data-stu-id="1d33b-133">Once the endpoint is removed, the virtual machine doesn't belong to the load balancer set anymore.</span></span>

<span data-ttu-id="1d33b-134">A fenti példa használatával eltávolíthatja a „web1” virtuális géphez létrehozott végpontot az „lbset” terheléselosztóból a következő parancs használatával: `azure vm endpoint delete`.</span><span class="sxs-lookup"><span data-stu-id="1d33b-134">Using the example above, you can remove the endpoint created for virtual machine "web1" from load balancer "lbset" using the command `azure vm endpoint delete`.</span></span>

```azurecli
azure vm endpoint delete web1 tcp-80-80
```

> [!NOTE]
> <span data-ttu-id="1d33b-135">A következő paranccsal további, a végpontok kezelésére szolgáló beállítást ismerhet meg: `azure vm endpoint --help`</span><span class="sxs-lookup"><span data-stu-id="1d33b-135">You can explore more options to manage endpoints using the command `azure vm endpoint --help`</span></span>

## <a name="next-steps"></a><span data-ttu-id="1d33b-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d33b-136">Next steps</span></span>

[<span data-ttu-id="1d33b-137">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="1d33b-137">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="1d33b-138">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1d33b-138">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="1d33b-139">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1d33b-139">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
