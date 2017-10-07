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
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-cli"></a>Internet felé néző terheléselosztó (klasszikus) Azure CLI hello létrehozásához

> [!div class="op_single_selector"]
> * [klasszikus Azure portál](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Azure Resource Manager és klasszikus. Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-classic-rm.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal. Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello. Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben. Emellett [megtudhatja, hogyan toocreate egy internetre terheléselosztó Azure Resource Manager használatával](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="step-by-step-creating-an-internet-facing-load-balancer-using-cli"></a>Internetkapcsolattal rendelkező terheléselosztó parancssori felület használatával történő létrehozásának lépései

Ez az útmutató bemutatja, hogyan toocreate egy Internet terheléselosztó alapján a fenti forgatókönyvben hello.

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello **azure config mód** tooswitch tooclassic üzemmód, alább látható módon.

    ```azurecli
    azure config mode asm
    ```

    Várt kimenet:

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a>Végpont és terheléselosztó készlet létrehozása

hello forgatókönyv azt feltételezi, hogy a hello virtuális gépek "web1" és "web2" lettek létrehozva.
Ez az útmutató létrehoz egy terheléselosztó készletet: a 80-as portot használja nyilvános portként, és a 80-as portot használja helyi portként. A mintavételi portot is konfigurálva van a 80-as porton, és a terheléselosztó elnevezett hello beállítása "lbset".

### <a name="step-1"></a>1. lépés

Hello első végpont létrehozásához és a terheléselosztó használatával be `azure network vm endpoint create` a "weben 1" virtuális gép.

```azurecli
azure vm endpoint create web1 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-2"></a>2. lépés

Adjon hozzá egy másik virtuális gép "web2" toohello terhelés terheléselosztó készletben.

```azurecli
azure vm endpoint create web2 80 --local-port 80 --protocol tcp --probe-port 80 --load-balanced-set-name lbset
```

## <a name="step-3"></a>3. lépés

Ellenőrizze a hello terhelés terheléselosztó konfigurációs használatával `azure vm show` .

```azurecli
azure vm show web1
```

hello kimenet lesz:

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

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Hozzon létre egy távoli asztali végpontot a virtuális géphez

Nyilvános port tooa helyi port, egy adott virtuális gép használata a távoli asztali végpont tooforward hálózati forgalom létrehozhat `azure vm endpoint create`.

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a>Virtuális gép eltávolítása a terheléselosztóból

Lehetősége van toodelete hello tartozó végpont toohello tartozó terheléselosztó készletének hello virtuális gépről. Hello végpont eltávolítást követően hello virtuális gép toohello elosztott terhelésű készlet már nem tartozik.

A fenti hello példáját eltávolíthatja a "weben 1" virtuális gép létrehozása hello végpont terheléselosztó "lbset" hello paranccsal `azure vm endpoint delete`.

```azurecli
azure vm endpoint delete web1 tcp-80-80
```

> [!NOTE]
> Ismerje meg a további beállítások toomanage végpontok hello paranccsal`azure vm endpoint --help`

## <a name="next-steps"></a>Következő lépések

[Bevezetés a belső terheléselosztók konfigurálásába](load-balancer-get-started-ilb-arm-ps.md)

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
