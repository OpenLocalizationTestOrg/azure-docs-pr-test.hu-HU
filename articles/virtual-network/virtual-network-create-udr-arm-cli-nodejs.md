---
title: "aaaControl útválasztási és a virtuális készülékek használata Azure CLI 1.0 hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toocontrol útválasztási és a virtuális készülékek használata hello Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2017
ms.author: jdial
ms.openlocfilehash: 1c8a552d949521fa554880c00405e65fa47a8162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-10"></a>Hozzon létre felhasználói útvonalakat (UDR) hello Azure CLI 1.0 használatával

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Sablon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (klasszikus)](virtual-network-create-udr-classic-ps.md)
> * [Parancssori felület (klasszikus)](virtual-network-create-udr-classic-cli.md)

Hozzon létre egyéni Útválasztás és a virtuális készülékek hello Azure parancssori felület használatával.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat 

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre: 

- [Az Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell 


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

minta hello Azure CLI-t az alábbi parancsok már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben várható. Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre hello tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek Ha szükséges, és kövesse az utasításokat hello a hello portálon.


## <a name="create-hello-udr-for-hello-front-end-subnet"></a>Hello UDR hello előtér-alhálózat létrehozása
toocreate hello útválasztási táblázatot és hello előtérben lévő alhálózat alhálózat alapján hello forgatókönyv fenti, a szükséges útvonal kövesse a hello lépéseket.

1. Futtassa a következő parancs toocreate hello hello előtér-alhálózat egy útválasztási táblázatot:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    Kimenet:
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    Paraméterek:
   
   * **-g (vagy --resource-group)**. Ahol létrejön hello UDR hello erőforráscsoport nevét. A mi esetünkben *TestRG*.
   * **-l (vagy --location)**. Azure-régió, ahol hello új UDR létrejön. A mi esetünkben *westus*.
   * **-n (vagy --name)**. Hello nevét új UDR. A mi esetünkben *UDR-előtérbeli*.
2. Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello háttér-alhálózat (192.168.2.0/24) toohello **FW1** virtuális gép (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    Kimenet:
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    Paraméterek:
   
   * **-r (vagy--útvonal-részes-táblanév)**. Amennyiben adható hozzá hello útvonal hello útvonaltábla neve. A mi esetünkben *UDR-előtérbeli*.
   * **-a (vagy --address-prefix)**. Ha a csomagok irányuló forgalomnál hello alhálózat címelőtagot. A mi esetünkben *192.168.2.0/24*.
   * **-y (vagy--tovább-hop-type)**. Típusú objektum forgalom kapnak. A lehetséges értékek: *VirtualAppliance*, *pedig*, *VNETLocal*, *Internet*, vagy *nincs*.
   * **-p (vagy--next-hop-ip-cím**). Következő ugrás IP-címet. A mi esetünkben *192.168.0.4*.
3. Futtatási hello következő parancsot a tooassociate hello útvonaltábla hello az előbb létrehozott **előtér** alhálózati:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    Kimenet:
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    Paraméterek:
   
   * **-e (vagy--vnet-name)**. Hello ahol hello alhálózat VNet neve. A mi esetünkben *TestVNet*.

## <a name="create-hello-udr-for-hello-back-end-subnet"></a>Hello UDR hello háttér-alhálózat létrehozása
toocreate hello útvonaltábla és útvonal-hello háttér-alhálózat a fenti lépések teljes hello hello forgatókönyv alapján szükséges:

1. Futtassa a következő parancs toocreate hello hello háttér-alhálózat egy útválasztási táblázatot:

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello előtér-alhálózat (192.168.1.0/24) toohello **FW1** virtuális gép (192.168.0.4):

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. Futtatási hello parancs tooassociate hello útvonaltáblában hello a következő **háttér** alhálózati:

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>IP-továbbítás a FW1 engedélyezése
a hálózati adapter által használt hello tooenable IP-továbbítás **FW1**, teljes hello a következő lépéseket:

1. Futtassa a következő és hello érték a hello parancsot **engedélyezése IP-továbbítás**. Kell lennie állítva, akkor túl*hamis*.

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    Kimenet:
   
        info:    Executing command network nic show
        info:    Looking up hello network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. Futtassa a következő parancs tooenable IP-továbbítás hello:

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    Kimenet:
   
        info:    Executing command network nic set
        info:    Looking up hello network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up hello network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    Paraméterek:
   
   * **-f (vagy--enable-ip-továbbítás)**. *Igaz* vagy *hamis*.

