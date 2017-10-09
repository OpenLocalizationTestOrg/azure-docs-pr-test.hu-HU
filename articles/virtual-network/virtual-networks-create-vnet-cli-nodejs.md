---
title: "a virtuális hálózat használatával aaaCreate hello Azure CLI 1.0 |} Microsoft Docs"
description: "Ismerje meg, hogyan egy virtuális hálózat használatával toocreate hello Azure CLI 1.0 |} Erőforrás-kezelő."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.openlocfilehash: f48a8b23a5994164b71c9b51ee8a6810d17f9392
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-hello-azure-cli"></a>Hozzon létre egy virtuális hálózatot hello Azure parancssori felület használatával

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal. A Microsoft azt javasolja, hello Resource Manager üzembe helyezési modellben erőforrásoknak létrehozása. hello arról toolearn hello hello két modellek közötti különbséget olvasási [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md) cikk.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell
- [Az Azure CLI 1.0](#create-a-virtual-network) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Virtuális hálózat létrehozása

a virtuális hálózat használatával toocreate hello Azure CLI, teljes hello a következő lépéseket:

1. Telepíteni és konfigurálni az Azure parancssori felület következő hello által lépések hello hello [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) cikk.

2. Egy VNet és alhálózat létrehozása:

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    Várt kimenet:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    Használt paraméterek:

   * **--vnet**. Hello VNet toobe létrehozott neve. A mi esetünkben *TestVNet*
   * **-e (vagy--címtartomány)**. Virtuális hálózat címterület. A mi esetünkben *192.168.0.0*
   * **-i (vagy - cidr)**. Hálózati maszk CIDR formátumban. A mi esetünkben *16*.
   * **-n (vagy--alhálózatneve**). Hello első alhálózat neve. A mi esetünkben *FrontEnd*.
   * **-p (vagy--ip-alhálózat-start)**. Kezdő IP-cím alhálózati vagy alhálózati címtartományt. A mi esetünkben *192.168.1.0*.
   * **-r (vagy--alhálózat-cidr)**. Hálózati maszk alhálózat CIDR formátumban. A mi esetünkben *24*.
   * **-l (vagy --location)**. Azure-régió, ahol a hello VNet jön létre. A mi esetünkben *USA középső RÉGIÓJA*.

3. Hozzon létre egy alhálózatot:

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    Várt kimenet:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    Használt paraméterek:

   * **-t (vagy--vnet-name**. Hello ahol hello alhálózati létrejön a VNet neve. A mi esetünkben *TestVNet*.
   * **-n (vagy --name)**. Hello új alhálózat neve. A mi esetünkben *háttér*.
   * **-a (vagy --address-prefix)**. Az alhálózat CIDR-blokkja. Négy esetünkben *192.168.2.0/24*.
   
4. tooview hello tulajdonságainak hello új virtuális hálózat:

    ```azurecli
    azure network vnet show
    ```
   
    Várt kimenet:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan tooconnect:

- A virtuális gép (VM) tooa virtuális hálózati hello olvasásával [hozzon létre egy Linux virtuális Gépet](../virtual-machines/linux/quick-create-cli.md) cikk. Egy VNet és alhálózat létrehozása hello cikkek hello lépésekben, helyett választhatja egy meglévő VNet és alhálózat tooconnect egy virtuális Gépet.
- virtuális hálózat tooother virtuális hálózatok hello hello olvasásával [csatlakozás Vnetek](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) cikk.
- hello virtuális hálózati tooan a helyi hálózati helyek virtuális magánhálózati (VPN) vagy ExpressRoute-kapcsolatcsoportot. Megtudhatja, hogyan hello olvasásával [VNet tooan a helyi hálózat a telephelyek közötti VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) és [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).
