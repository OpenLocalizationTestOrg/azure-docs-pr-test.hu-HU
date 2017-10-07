---
title: "a virtuális hálózati - Azure CLI 2.0 aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan egy virtuális hálózat használatával toocreate hello Azure CLI 2.0."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e79b7fe780fc81f4866f810d830824e43a5a43b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-hello-azure-cli-20"></a>Hello Azure CLI 2.0 virtuális hálózat létrehozása

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal. A Microsoft azt javasolja, hello Resource Manager üzembe helyezési modellben erőforrásoknak létrehozása. hello arról toolearn hello hello két modellek közötti különbséget olvasási [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md) cikk.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület
- [Az Azure CLI 2.0](#create-a-virtual-network) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk) "
 
    Hozzon létre egy Vnetet Resource Manageren keresztül más eszközök használatával vagy hello klasszikus telepítési modell használatával VNet létrehozása a következő lista hello egy másik lehetőség kiválasztásával is:

> [!div class="op_single_selector"]
> * [Portál](virtual-networks-create-vnet-arm-pportal.md)
> * [PowerShell](virtual-networks-create-vnet-arm-ps.md)
> * [Parancssori felület](virtual-networks-create-vnet-arm-cli.md)
> * [Sablon](virtual-networks-create-vnet-arm-template-click.md)
> * [Portál (klasszikus)](virtual-networks-create-vnet-classic-pportal.md)
> * [PowerShell (klasszikus)](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [Parancssori felület (klasszikus)](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]


## <a name="create-a-virtual-network"></a>Virtuális hálózat létrehozása

a virtuális hálózat használatával toocreate hello Azure CLI 2.0-s teljes hello a következő lépéseket:

1. Telepítse és konfigurálja a hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).

2. Hozzon létre egy erőforráscsoportot a vnet hello segítségével [az csoport létrehozása](/cli/azure/group#create) hello parancsot `--name` és `--location` argumentumai:

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. Egy VNet és alhálózat létrehozása:

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    Várt kimenet:
    
    ```json
    {
        "newVNet": {
            "addressSpace": {
            "addressPrefixes": [
            "192.168.0.0/16"
            ]
            },
            "dhcpOptions": {
            "dnsServers": []
            },
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>",
            "subnets": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                "name": "FrontEnd",
                "properties": {
                "addressPrefix": "192.168.1.0/24",
                "provisioningState": "Succeeded"
                },
                "resourceGroup": "TestRG"
            }
            ]
            }
    }
    ```

    Használt paraméterek:

    - `--name TestVNet`: Hello VNet toobe létrehozott neve.
    - `--resource-group TestRG`: # hello az erőforráscsoport neve, amely a hello erőforrás. 
    - `--location centralus`: hello mely toodeploy a helyen.
    - `--address-prefix 192.168.0.0/16`: hello címelőtag, és letiltja.  
    - `--subnet-name FrontEnd`: hello alhálózati hello nevét.
    - `--subnet-prefix 192.168.1.0/24`: hello címelőtag, és letiltja.

    toolist hello alapvető információkat toouse hello a következő parancsot, hello VNet segítségével lekérheti egy [lekérdezésszűrő](/cli/azure/query-az-cli2):

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    A következő kimeneti hello állít elő:

        Where      Name      Group

        centralus  TestVNet  TestRG

4. Hozzon létre egy alhálózatot:

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    Várt kimenet:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid> \"",
    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "TestRG",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

    Használt paraméterek:

    - `--address-prefix 192.168.2.0/24`: Alhálózat CIDR-blokkja.
    - `--name BackEnd`: Hello új alhálózat neve.
    - `--resource-group TestRG`: hello erőforráscsoportot.
    - `--vnet-name TestVNet`: a tulajdonos VNet hello hello nevét.

5. Lekérdezés hello tulajdonságainak hello új virtuális hálózat:

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    Várt kimenet:

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. Lekérdezés hello tulajdonságok hello alhálózatok:

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    Várt kimenet:

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan tooconnect:

- A virtuális gép (VM) tooa virtuális hálózati hello olvasásával [hozzon létre egy Linux virtuális Gépet](../virtual-machines/linux/quick-create-cli.md) cikk. Egy VNet és alhálózat létrehozása hello cikkek hello lépésekben, helyett választhatja egy meglévő VNet és alhálózat tooconnect egy virtuális Gépet.
- virtuális hálózat tooother virtuális hálózatok hello hello olvasásával [csatlakozás Vnetek](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) cikk.
- hello virtuális hálózati tooan a helyi hálózati helyek virtuális magánhálózati (VPN) vagy ExpressRoute-kapcsolatcsoportot. Megtudhatja, hogyan hello olvasásával [VNet tooan a helyi hálózat a telephelyek közötti VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) és [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).
