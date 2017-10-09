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
# <a name="get-started-creating-an-internal-load-balancer-classic-using-hello-azure-cli"></a>Elsajátíthatja a belső terheléselosztót (klasszikus) hello Azure parancssori felület használatával

> [!div class="op_single_selector"]
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-classic-cli.md)
> * [Felhőszolgáltatások](../load-balancer/load-balancer-get-started-ilb-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!IMPORTANT]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](load-balancer-get-started-ilb-arm-cli.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="toocreate-an-internal-load-balancer-set-for-virtual-machines"></a>toocreate belső terheléselosztót beállítani a virtuális gépek

belső terheléselosztót beállítani, és szeretne küldeni a forgalom tooit kiszolgálók hello toocreate, el kell végeznie a következő hello:

1. Belső terheléselosztás hello végpont egy elosztott terhelésű készlet hello kiszolgálóin bejövő forgalom toobe terhelés leendő példányt létrehozni.
2. Hello bejövő forgalmat fog kapni toohello virtuális gépek megfelelő végpont-hozzáadáshoz.
3. Hello forgalom toobe az elosztott terhelésű toosend a forgalom toohello virtuális IP-cím (VIP) cím hello belső terheléselosztás példány küldi hello kiszolgálók konfigurálása.

## <a name="step-by-step-creating-an-internal-load-balancer-using-cli"></a>Belső terheléselosztó parancssori felület használatával történő létrehozásának lépései

Ez az útmutató bemutatja, hogyan toocreate belső terheléselosztót alapján a fenti forgatókönyvben hello.

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello **azure config mód** tooswitch tooclassic üzemmód, alább látható módon.

    ```azurecli
    azure config mode asm
    ```

    Várt kimenet:

        info:    New mode is asm

## <a name="create-endpoint-and-load-balancer-set"></a>Végpont és terheléselosztó készlet létrehozása

hello forgatókönyv azt feltételezi, hogy a virtuális gépek "D1" és "DB2" hello "mytestcloud" nevű felhőszolgáltatás. Mindkét virtuális gép a „testvnet” nevű virtuális hálózatot használja, a „alhalozat-1” nevű alhálózattal.

Ez az útmutató létrehoz egy belső terheléselosztó készletet: az 1433-as portot használja nyilvános portként, és az 1433-as portot használja helyi portként.

Ez egy webfarmos közös hello háttér használata egy belső terheléselosztó tooguarantee hello adatbázis-kiszolgálók nem érhetők el közvetlenül a egy nyilvános IP-címet használja az SQL virtuális gépek esetében.

### <a name="step-1"></a>1. lépés

Belső terheléselosztó készlet létrehozása a következő használatával: `azure network service internal-load-balancer add`.

```azurecli
azure service internal-load-balancer add --serviceName mytestcloud --internalLBName ilbset --subnet-name subnet-1 --static-virtualnetwork-ipaddress 192.168.2.7
```

További információ: `azure service internal-load-balancer --help`.

Hello belső terheléselosztó tulajdonságai hello paranccsal ellenőrizheti `azure service internal-load-balancer list` *felhőszolgáltatás neve*.

Hello kimeneti példát a következő:

    azure service internal-load-balancer list my-testcloud
    info:    Executing command service internal-load-balancer list
    + Getting cloud service deployment
    data:    Name    Type     SubnetName  StaticVirtualNetworkIPAddress
    data:    ------  -------  ----------  -----------------------------
    data:    ilbset  Private  subnet-1    192.168.2.7
    info:    service internal-load-balancer list command OK


### <a name="step-2"></a>2. lépés

Hello belső elosztott terhelésű készlet a hello első végpont hozzáadásakor konfigurálja. Hello végpont, a virtuális gép és a mintavételi portot toohello belső elosztott terhelésű készlet ebben a lépésben fog társítani.

```azurecli
azure vm endpoint create db1 1433 --local-port 1433 --protocol tcp --probe-port 1433 --probe-protocol tcp --probe-interval 300 --probe-timeout 600 --internal-load-balancer-name ilbset
```

### <a name="step-3"></a>3. lépés

Ellenőrizze a hello terhelés terheléselosztó használt konfiguráció `azure vm show` *virtuális gép neve*

```azurecli
azure vm show DB1
```

hello kimenet lesz:

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

## <a name="create-a-remote-desktop-endpoint-for-a-virtual-machine"></a>Hozzon létre egy távoli asztali végpontot a virtuális géphez

Nyilvános port tooa helyi port, egy adott virtuális gép használata a távoli asztali végpont tooforward hálózati forgalom létrehozhat `azure vm endpoint create`.

```azurecli
azure vm endpoint create web1 54580 -k 3389
```

## <a name="remove-virtual-machine-from-load-balancer"></a>Virtuális gép eltávolítása a terheléselosztóból

A virtuális gépek eltávolíthatja a kapcsolódó hello végpont törlése által beállított belső terheléselosztót. Hello végpont eltávolítást követően hello virtuális gép toohello elosztott terhelésű készlet már nem tartozik.

A fenti hello példáját eltávolíthat létrehozott virtuális gép "D1" hello végpont belső terheléselosztó "ilbset" hello paranccsal `azure vm endpoint delete`.

```azurecli
azure vm endpoint delete DB1 tcp-1433-1433
```

További információ: `azure vm endpoint --help`.

## <a name="next-steps"></a>Következő lépések

[A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)
