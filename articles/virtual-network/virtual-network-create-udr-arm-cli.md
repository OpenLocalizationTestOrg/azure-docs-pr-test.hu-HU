---
title: "aaaControl útválasztási és a virtuális készülékek használata Azure CLI 2.0 hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toocontrol útválasztási és a virtuális készülékek használata hello Azure CLI 2.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2017
ms.author: jdial
ms.openlocfilehash: 79b908848932a4365dab1b7497b6a0dbf44bbaf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-20"></a>Hozzon létre felhasználói útvonalakat (UDR) hello Azure CLI 2.0 használatával

> [!div class="op_single_selector"]
> * [PowerShell](virtual-network-create-udr-arm-ps.md)
> * [Azure CLI](virtual-network-create-udr-arm-cli.md)
> * [Sablon](virtual-network-create-udr-arm-template.md)
> * [PowerShell (Classic deployment)](virtual-network-create-udr-classic-ps.md)
> * [CLI (Classic deployment)](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat 

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre: 

- [Az Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület 
- [Az Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk)

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

minta hello Azure CLI-t az alábbi parancsok már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben várható. Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre hello tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek Ha szükséges, és kövesse az utasításokat hello a hello portálon.


## <a name="create-hello-udr-for-hello-front-end-subnet"></a>Hello UDR hello előtér-alhálózat létrehozása
toocreate hello útválasztási táblázatot és hello előtérben lévő alhálózat alhálózat alapján hello forgatókönyv fenti, a szükséges útvonal kövesse a hello lépéseket.

1. Hozzon létre egy útválasztási táblázatot hello előtér-alhálózat hello [az hálózati útvonal-táblázat létrehozása](/cli/azure/network/route-table#create) parancs:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    Kimenet:

    ```json
    {
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
    "location": "centralus",
    "name": "UDR-FrontEnd",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "routes": [],
    "subnets": null,
    "tags": null,
    "type": "Microsoft.Network/routeTables"
    }
    ```

2. Hozzon létre egy olyan útvonalat, amelyet az összes adatforgalmat toohello háttér-alhálózat (192.168.2.0/24) toohello küld **FW1** használó virtuális gépek (192.168.0.4) hello [az hálózati útvonaltábla útvonal létrehozása](/cli/azure/network/route-table/route#create) parancs:

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    Kimenet:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd/routes/RouteToBackEnd",
    "name": "RouteToBackEnd",
    "nextHopIpAddress": "192.168.0.4",
    "nextHopType": "VirtualAppliance",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg"
    }
    ```
    Paraméterek:

    * **--útvonal-részes-táblanév**. Amennyiben adható hozzá hello útvonal hello útvonaltábla neve. A mi esetünkben *UDR-előtérbeli*.
    * **--address-prefix**. Ha a csomagok irányuló forgalomnál hello alhálózat címelőtagot. A mi esetünkben *192.168.2.0/24*.
    * **--következő ugrás-típusú**. Típusú objektum forgalom kapnak. A lehetséges értékek: *VirtualAppliance*, *pedig*, *VNETLocal*, *Internet*, vagy *nincs*.
    * **--next-hop-ip-cím**. Következő ugrás IP-címet. A mi esetünkben *192.168.0.4*.

3. Futtassa a hello [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update) hello az előbb létrehozott parancs tooassociate hello útvonaltábla **előtér** alhálózati:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    Kimenet:

    ```json
    {
    "addressPrefix": "192.168.1.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/FrontEnd",
    "ipConfigurations": null,
    "name": "FrontEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": {
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
        "location": null,
        "name": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "routes": null,
        "subnets": null,
        "tags": null,
        "type": null
        }
    }
    ```

    Paraméterek:
    
    * **--vnet-name**. Hello ahol hello alhálózat VNet neve. A mi esetünkben *TestVNet*.

## <a name="create-hello-udr-for-hello-back-end-subnet"></a>Hello UDR hello háttér-alhálózat létrehozása

toocreate hello útvonaltábla és útvonal-hello háttér-alhálózat a fenti lépések teljes hello hello forgatókönyv alapján szükséges:

1. Futtassa a következő parancs toocreate hello hello háttér-alhálózat egy útválasztási táblázatot:

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello előtér-alhálózat (192.168.1.0/24) toohello **FW1** virtuális gép (192.168.0.4):

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. Futtatási hello parancs tooassociate hello útvonaltáblában hello a következő **háttér** alhálózati:

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a>IP-továbbítás a FW1 engedélyezése

a hálózati adapter által használt hello tooenable IP-továbbítás **FW1**, teljes hello a következő lépéseket:

1. Hello futtatása [az hálózati nic megjelenítése](/cli/azure/network/nic#show) parancsot egy JMESPATH szűrő toodisplay hello aktuális **enable-ip-továbbítás** értékének **engedélyezése IP-továbbítás**. Kell lennie állítva, akkor túl*hamis*.

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    Kimenet:

        false

2. Futtassa a következő parancs tooenable IP-továbbítás hello:

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    Vizsgálja meg a hello kimeneti adatfolyamként továbbított toohello konzol, vagy az adott hello csak ellenőrzése hosszadalmas **enableIpForwarding** érték:

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    Kimenet:

        true

    Paraméterek:

    **--ip-továbbítás**: *igaz* vagy *hamis*.

