---
title: "az internetre irányuló aaaCreate terheléselosztó - klasszikus Azure CLI |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Internet felé néző terheléselosztót a klasszikus üzembe helyezési modell használatával hello Azure parancssori felület"
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
ms.openlocfilehash: e6070cbc574f74bca0cccb960ff192847d6511bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-cli"></a><span data-ttu-id="20f1d-103">Internet felé néző terheléselosztó (klasszikus) Azure CLI hello létrehozásához</span><span class="sxs-lookup"><span data-stu-id="20f1d-103">Get started creating an Internet facing load balancer (classic) in hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="20f1d-104">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="20f1d-104">Azure classic portal</span></span>](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [<span data-ttu-id="20f1d-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="20f1d-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [<span data-ttu-id="20f1d-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="20f1d-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [<span data-ttu-id="20f1d-107">Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="20f1d-107">Azure Cloud Services</span></span>](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="20f1d-108">Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Azure Resource Manager és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="20f1d-108">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="20f1d-109">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="20f1d-109">Make sure you understand [deployment models and tools](../azure-classic-rm.md) before you work with any Azure resource.</span></span> <span data-ttu-id="20f1d-110">Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello.</span><span class="sxs-lookup"><span data-stu-id="20f1d-110">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span> <span data-ttu-id="20f1d-111">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="20f1d-111">This article covers hello classic deployment model.</span></span> <span data-ttu-id="20f1d-112">Emellett [megtudhatja, hogyan toocreate egy internetre terheléselosztó Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="20f1d-112">You can also [Learn how toocreate an Internet facing load balancer using Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a><span data-ttu-id="20f1d-113">Internetkapcsolattal rendelkező terheléselosztó parancssori felület használatával történő létrehozásának lépései</span><span class="sxs-lookup"><span data-stu-id="20f1d-113">Step by step creating an Internet facing load balancer using CLI</span></span>

<span data-ttu-id="20f1d-114">Ez az útmutató bemutatja, hogyan toocreate egy Internet terheléselosztó alapján a fenti forgatókönyvben hello.</span><span class="sxs-lookup"><span data-stu-id="20f1d-114">This guide shows how toocreate an Internet load balancer based on hello scenario above.</span></span>

1. <span data-ttu-id="20f1d-115">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="20f1d-115">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="20f1d-116">Futtassa a hello **azure config mód** tooswitch tooclassic üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="20f1d-116">Run hello **azure config mode** command tooswitch tooclassic mode, as shown below.</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="20f1d-117">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="20f1d-117">Expected output:</span></span>

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a><span data-ttu-id="20f1d-118">Végpont és terheléselosztó készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="20f1d-118">Create endpoint and load balancer set</span></span>

<span data-ttu-id="20f1d-119">hello forgatókönyv azt feltételezi, hogy a hello virtuális gépek "web1" és "web2" lettek létrehozva.</span><span class="sxs-lookup"><span data-stu-id="20f1d-119">hello scenario assumes hello virtual machines "web1" and "web2" were created.</span></span>
<span data-ttu-id="20f1d-120">Ez az útmutató létrehoz egy terheléselosztó készletet: a 80-as portot használja nyilvános portként, és a 80-as portot használja helyi portként.</span><span class="sxs-lookup"><span data-stu-id="20f1d-120">This guide will create a load balancer set using port 80 as public port and port 80 as local port.</span></span> <span data-ttu-id="20f1d-121">A mintavételi portot is konfigurálva van a 80-as porton, és a terheléselosztó elnevezett hello beállítása "lbset".</span><span class="sxs-lookup"><span data-stu-id="20f1d-121">A probe port is also configured on port 80 and named hello load balancer set "lbset".</span></span>

### <a name="step-1"></a><span data-ttu-id="20f1d-122">1. lépés</span><span class="sxs-lookup"><span data-stu-id="20f1d-122">Step 1</span></span>

<span data-ttu-id="20f1d-123">Hello első végpont létrehozásához és a terheléselosztó használatával be `azure network vm endpoint create` a "weben 1" virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="20f1d-123">Create hello first endpoint and load balancer set using `azure network vm endpoint create` for virtual machine "web1".</span></span>

```azurecli
azure vm endpoint create web1 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-2"></a><span data-ttu-id="20f1d-124">2. lépés</span><span class="sxs-lookup"><span data-stu-id="20f1d-124">Step 2</span></span>

<span data-ttu-id="20f1d-125">Adjon hozzá egy másik virtuális gép "web2" toohello terhelés terheléselosztó készletben.</span><span class="sxs-lookup"><span data-stu-id="20f1d-125">Add a second virtual machine "web2" toohello load balancer set.</span></span>

```azurecli
azure vm endpoint create web2 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-3"></a><span data-ttu-id="20f1d-126">3. lépés</span><span class="sxs-lookup"><span data-stu-id="20f1d-126">Step 3</span></span>

<span data-ttu-id="20f1d-127">Ellenőrizze a hello terhelés terheléselosztó konfigurációs használatával `azure vm show` .</span><span class="sxs-lookup"><span data-stu-id="20f1d-127">Verify hello load balancer configuration using `azure vm show` .</span></span>

```azurecli
azure vm show web1
```

<span data-ttu-id="20f1d-128">hello kimenet lesz:</span><span class="sxs-lookup"><span data-stu-id="20f1d-128">hello output will be:</span></span>

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

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a><span data-ttu-id="20f1d-129">Hozzon létre egy távoli asztali végpontot a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="20f1d-129">Create a remote desktop endpoint for a virtual machine</span></span>

<span data-ttu-id="20f1d-130">Nyilvános port tooa helyi port, egy adott virtuális gép használata a távoli asztali végpont tooforward hálózati forgalom létrehozhat `azure vm endpoint create`.</span><span class="sxs-lookup"><span data-stu-id="20f1d-130">You can create a remote desktop endpoint tooforward network traffic from a public port tooa local port for a specific virtual machine using `azure vm endpoint create`.</span></span>

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a><span data-ttu-id="20f1d-131">Virtuális gép eltávolítása a terheléselosztóból</span><span class="sxs-lookup"><span data-stu-id="20f1d-131">Remove virtual machine from load balancer</span></span>

<span data-ttu-id="20f1d-132">Lehetősége van toodelete hello tartozó végpont toohello tartozó terheléselosztó készletének hello virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="20f1d-132">You have toodelete hello endpoint associated toohello load balancer set from hello virtual machine.</span></span> <span data-ttu-id="20f1d-133">Hello végpont eltávolítást követően hello virtuális gép toohello elosztott terhelésű készlet már nem tartozik.</span><span class="sxs-lookup"><span data-stu-id="20f1d-133">Once hello endpoint is removed, hello virtual machine doesn't belong toohello load balancer set anymore.</span></span>

<span data-ttu-id="20f1d-134">A fenti hello példáját eltávolíthatja a "weben 1" virtuális gép létrehozása hello végpont terheléselosztó "lbset" hello paranccsal `azure vm endpoint delete`.</span><span class="sxs-lookup"><span data-stu-id="20f1d-134">Using hello example above, you can remove hello endpoint created for virtual machine "web1" from load balancer "lbset" using hello command `azure vm endpoint delete`.</span></span>

```azurecli
azure vm endpoint delete web1 tcp-80-80
```

> [!NOTE]
> <span data-ttu-id="20f1d-135">Ismerje meg a további beállítások toomanage végpontok hello paranccsal`azure vm endpoint --help`</span><span class="sxs-lookup"><span data-stu-id="20f1d-135">You can explore more options toomanage endpoints using hello command `azure vm endpoint --help`</span></span>

## <a name="next-steps"></a><span data-ttu-id="20f1d-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="20f1d-136">Next steps</span></span>

[<span data-ttu-id="20f1d-137">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="20f1d-137">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-ps.md)

[<span data-ttu-id="20f1d-138">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="20f1d-138">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="20f1d-139">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="20f1d-139">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
