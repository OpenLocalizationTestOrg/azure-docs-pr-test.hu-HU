---
title: "egy Azure-beli virtuális hálózatra - CLI - klasszikus útválasztás aaaControl |} Microsoft Docs"
description: "Ismerje meg, hogyan útválasztás használatával Vnetek toocontrol hello hello klasszikus üzembe helyezési modellel Azure parancssori felület"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: ca2b4638-8777-4d30-b972-eb790a7c804f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 07dde573f1a605bf280156c261d51e213ede0cdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-hello-azure-cli"></a>Vezérlő az Útválasztás és a használata (klasszikus) virtuális készülékek használata Azure CLI hello

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Sablon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klasszikus)](virtual-network-create-udr-classic-ps.md)
> * [Parancssori felület (klasszikus)](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben. Emellett [szabályozhatja az Útválasztás és használni a virtuális készülékeket hello Resource Manager üzembe helyezési modellel](virtual-network-create-udr-arm-cli.md).

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

minta hello Azure CLI-t az alábbi parancsok már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben várható. Ha toorun hello parancsok ebben a dokumentumban megjelenített, hozzon létre hello környezet látható [VNet létrehozása (klasszikus) használatával hello Azure CLI](virtual-networks-create-vnet-classic-cli.md).

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a>Hello UDR hello előtér alhálózat létrehozása
toocreate hello útválasztási táblázatot és hello előtérben lévő alhálózat alhálózat alapján hello forgatókönyv fenti, a szükséges útvonal kövesse a hello lépéseket.

1. Futtassa a következő tooswitch tooclassic üzemmód hello:

    ```azurecli
    azure config mode asm
    ```

    Kimenet:

        info:    New mode is asm

2. Futtassa a következő parancs toocreate hello hello előtér-alhálózat egy útválasztási táblázatot:

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    Kimenet:
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    Paraméterek:
   
   * **-l (vagy --location)**. Azure-régió, ahol hello új NSG létrejön. A mi esetünkben *westus*.
   * **-n (vagy --name)**. Hello nevét új NSG. A mi esetünkben *NSG-előtérbeli*.
3. Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello háttér-alhálózat (192.168.2.0/24) toohello **FW1** virtuális gép (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    Kimenet:
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    Paraméterek:
   
   * **-r (vagy--útvonal-részes-táblanév)**. Amennyiben adható hozzá hello útvonal hello útvonaltábla neve. A mi esetünkben *UDR-előtérbeli*.
   * **-a (vagy --address-prefix)**. Ha a csomagok irányuló forgalomnál hello alhálózat címelőtagot. A mi esetünkben *192.168.2.0/24*.
   * **-t (vagy--tovább-hop-type)**. Típusú objektum forgalom kapnak. A lehetséges értékek: *VirtualAppliance*, *pedig*, *VNETLocal*, *Internet*, vagy *nincs*.
   * **-p (vagy--next-hop-ip-cím**). Következő ugrás IP-címet. A mi esetünkben *192.168.0.4*.
4. Futtatási hello következő parancsot a tooassociate hello útvonaltábla hello létre **előtér** alhálózati:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Kimenet:
   
        info:    Executing command network vnet subnet route-table add
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK    
   
    Paraméterek:
   
   * **-t (vagy--vnet-name)**. Hello ahol hello alhálózat VNet neve. A mi esetünkben *TestVNet*.
   * **-n (vagy--alhálózatneve**. Hozzáadandó hello alhálózati hello útvonaltábla neve. A mi esetünkben *FrontEnd*.

## <a name="create-hello-udr-for-hello-back-end-subnet"></a>Hello UDR hello háttér-alhálózat létrehozása
toocreate hello útválasztási táblázatot és hello háttér-alhálózat hello esetben teljes hello lépések alapján szükséges útvonal:

1. Futtassa a következő parancs toocreate hello hello háttér-alhálózat egy útválasztási táblázatot:

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello előtér-alhálózat (192.168.1.0/24) toohello **FW1** virtuális gép (192.168.0.4):

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. Futtatási hello parancs tooassociate hello útvonaltáblában hello a következő **háttér** alhálózati:

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

