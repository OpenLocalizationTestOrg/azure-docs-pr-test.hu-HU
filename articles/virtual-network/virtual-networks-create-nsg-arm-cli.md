---
title: "hálózati biztonsági csoport – Azure CLI 2.0 aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és hálózati biztonsági csoportok használatával hello Azure CLI 2.0 telepítése."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9ea82c09-f4a6-4268-88bc-fc439db40c48
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30b1d60676331bf5e2bbbb046c747477be9d3338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-hello-azure-cli-20"></a>Hálózati biztonsági csoportok használatával hello Azure CLI 2.0 létrehozása

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat 

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre: 

- [Az Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület 
- [Az Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk)

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

hello minta Azure CLI 2.0 parancsokat következő várt már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben. 

## <a name="create-hello-nsg-for-hello-frontend-subnet"></a>Hozzon létre a hello NSG hello `FrontEnd` alhálózati

az NSG nevű toocreate *NSG-előtér* alapján az előző hello forgatókönyv, hajtsa végre a hello lépéseket következő.

1. Ha még nem még telepít, és hello konfigurálása legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login). 

2. Hozzon létre egy NSG-t használó hello [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) parancsot. 

    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-FrontEnd \
    --location centralus 
    ```

    Paraméterek:
   
   * `--resource-group`: Hello NSG létrehozási helyének hello erőforráscsoport nevét. A mi esetünkben *TestRG*.
   * `--location`: Az azure-régió, ahol hello új NSG jön létre. A mi esetünkben *westus*.
   * `--name`: Hello nevét új NSG. A mi esetünkben *NSG-előtérbeli*.

    hello várt kimeneti meglehetősen bit, többek között az összes hello alapértelmezett szabályok listáját. hello alábbi példa bemutatja hello alapértelmezett szabályok JMESPATH lekérdezés szűrő használata hello `table` kimeneti formátum:

    ```azurecli
    az network nsg show \
    -g testrg \
    -n nsg-frontend \
    --query 'defaultSecurityRules[].{Access:access,Desc:description,DestPortRange:destinationPortRange,Direction:direction,Priority:priority}' \
    -o table
    ```
   
   Kimenet:

        Access    Desc                                                    DestPortRange    Direction      Priority
        
        Allow     Allow inbound traffic from all VMs in VNET              *                Inbound           65000
        Allow     Allow inbound traffic from azure load balancer          *                Inbound           65001
        Deny      Deny all inbound traffic                                *                Inbound           65500
        Allow     Allow outbound traffic from all VMs tooall VMs in VNET  *                Outbound          65000
        Allow     Allow outbound traffic from all VMs tooInternet         *                Outbound          65001
        Deny      Deny all outbound traffic                               *                Outbound          65500



3. Hozzon létre egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 3389-es (RDP) a hello Internet a hello [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) parancsot.

    > [!NOTE]
    > Attól függően, hogy hello felületet használ, szükség lehet a toomodify hello `*` karakter hello argumentumok nem tooexpand hello argumentum végrehajtása előtt.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name rdp-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 3389
    ```
   
    Várt kimenet:
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "3389",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
        "name": "rdp-rule",
        "priority": 100,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

    Paraméterek:

    * `--resource-group testrg`: hello erőforrás csoport toouse. Vegye figyelembe, hogy a rendszer azonban nem.
    * `--nsg-name NSG-FrontEnd`: A mely hello-szabály jön létre hello NSG neve.
    * `--name rdp-rule`: Hello új szabály nevét.
    * `--access Allow`: A hozzáférési szint hello szabály (Megtagadás vagy engedélyezés).
    * `--protocol Tcp`: A protokoll (Tcp, Udp vagy *).
    * `--direction Inbound`: Irányát (bejövő vagy kimenő) hello kapcsolat.
    * `--priority 100`: Hello szabály prioritását.
    * `--source-address-prefix Internet`: Forrás címelőtagot CIDR vagy az alapértelmezett címkéket használ.
    * `--source-port-range "*"`: A portot vagy porttartományt forrás. Hello kapcsolatot megnyitó port.
    * `--destination-address-prefix "*"`: Cél címelőtagot CIDR vagy az alapértelmezett címkéket használ.
    * `--destination-port-range 3389`: A célport vagy porttartomány. Az port, amely megkapja a hello kapcsolódási kérelmet.



4. Hozzon létre egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 80-as (HTTP) az hello Internet **az hálózati nsg-szabály létrehozása** parancsot.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name web-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 200 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 80
    ```
   
    Várt putput:
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "80",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
        "name": "web-rule",
        "priority": 200,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

5. Kötési hello NSG toohello **előtér** hello alhálózat [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update) parancsot.
        
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name FrontEnd \
    --resource-group testrg \
    --network-security-group NSG-FrontEnd
    ```
   
    Várt kimenet:
   
    ```json
    {
        "addressPrefix": "192.168.1.0/24",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
        "ipConfigurations": [
            {
            "etag": null,
            "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
            "name": null,
            "privateIpAddress": null,
            "privateIpAllocationMethod": null,
            "provisioningState": null,
            "publicIpAddress": null,
            "resourceGroup": "TestRG",
            "subnet": null
            }
        ],
        "name": "FrontEnd",
        "networkSecurityGroup": {
            "defaultSecurityRules": null,
            "etag": null,
            "id": "/subscriptions/<guid>f/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
            "location": null,
            "name": null,
            "networkInterfaces": null,
            "provisioningState": null,
            "resourceGroup": "testrg",
            "resourceGuid": null,
            "securityRules": null,
            "subnets": null,
            "tags": null,
            "type": null
        },
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "resourceNavigationLinks": null,
        "routeTable": null
    }
    ```

## <a name="create-hello-nsg-for-hello-backend-subnet"></a>Hozzon létre a hello NSG hello `BackEnd` alhálózati
az NSG nevű toocreate *NSG-háttérrendszer* alapján az előző hello forgatókönyv, hajtsa végre a hello lépéseket következő.

1. Hozzon létre hello `NSG-BackEnd` az NSG **az hálózati nsg létrehozása**.
   
    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-BackEnd \
    --location centralus
    ```
   
    Ahogy az előző, 2. lépés hello várható kimenete túl nagy, beleértve az alapértelmezett szabályokat.
   
2. Hozzon létre egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 1433-as port (SQL) a hello `FrontEnd` hello alhálózat **az hálózati nsg-szabály létrehozása** parancsot.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name sql-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix 192.168.1.0/24 \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 1433
    ```
   
    Várt kimenet:

    ```json  
    {
    "access": "Allow",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "1433",
    "direction": "Inbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule",
    "name": "sql-rule",
    "priority": 100,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "192.168.1.0/24",
    "sourcePortRange": "*"
    }
    ```

3. Hozzon létre egy szabályt, amely megtagadja a hozzáférést toohello Internet használatával hello **az hálózati nsg-szabály létrehozása** parancsot.
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name web-rule \
    --access Deny \
    --protocol Tcp  \
    --direction Outbound  \
    --priority 200 \
    --source-address-prefix "*" \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range "*"
    ```
   
    Várt putput:
   
    ```json
    {
    "access": "Deny",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "*",
    "direction": "Outbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/web-rule",
    "name": "web-rule",
    "priority": 200,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "*",
    "sourcePortRange": "*"
    }
    ```

4. Kötési hello NSG toohello `BackEnd` hello alhálózatot **az hálózati vnet alhálózati set** parancsot.
   
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name BackEnd \
    --resource-group testrg \
    --network-security-group NSG-BackEnd
    ```
   
    Várt kimenet:
   
    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": {
        "defaultSecurityRules": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "location": null,
        "name": null,
        "networkInterfaces": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "resourceGuid": null,
        "securityRules": null,
        "subnets": null,
        "tags": null,
        "type": null
    },
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```
