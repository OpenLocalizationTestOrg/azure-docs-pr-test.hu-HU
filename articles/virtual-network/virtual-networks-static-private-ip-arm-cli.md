---
title: "virtuális gépek - Azure CLI 2.0 aaaConfigure magánhálózati IP-címek |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure magánhálózati IP-címek használata virtuális gépek hello Azure parancssori felület (CLI) 2.0-s."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 40b03a1a-ea00-454c-b716-7574cea49ac0
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0e278e6ac63c0cda061cf70ab0edfaff5491c03b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-hello-azure-cli-20"></a>Hello Azure CLI 2.0 használatával virtuális gépek magánhálózati IP-címek konfigurálása

[!INCLUDE [virtual-networks-static-private-ip-selectors-arm-include](../../includes/virtual-networks-static-private-ip-selectors-arm-include.md)]


## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat 

Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre: 

- [Az Azure CLI 1.0](virtual-networks-static-private-ip-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület 
- [Az Azure CLI 2.0](#specify-a-static-private-ip-address-when-creating-a-vm) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk)

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben. Emellett [statikus magánhálózati IP-cím hello klasszikus üzembe helyezési modellel kezelése](virtual-networks-static-private-ip-classic-cli.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

> [!NOTE]
> minta hello Azure CLI 2.0 az alábbi parancsok már létrehozott egy egyszerű környezetben várható. Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre leírt tesztkörnyezet hello [hozhat létre egy vnetet](virtual-networks-create-vnet-arm-cli.md).

## <a name="specify-a-static-private-ip-address-when-creating-a-vm"></a>Adjon meg egy statikus magánhálózati IP-cím, amikor a virtuális gép létrehozása

toocreate nevű virtuális gép *DNS01* a hello *előtér* egy vnet nevű alhálózat *TestVNet* statikus magánhálózati IP-címe a *192.168.1.101*, kövesse az alábbi hello lépéseket:

1. Ha még nem még telepít, és hello konfigurálása legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login). 

2. Hozzon létre egy nyilvános IP-cím hello VM hello [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create) parancsot. hello kimenet után látható hello lista hello paramétereket ismerteti.

    > [!NOTE]
    > Azt szeretné, vagy a környezettől függően toouse különböző érték is az argumentumok ezen és a későbbi lépésekben kell.
   
    ```azurecli
    az network public-ip create \
    --name TestPIP \
    --resource-group TestRG \
    --location centralus \
    --allocation-method Static
    ```

    Várt kimenet:
   
   ```json
   {
        "publicIp": {
            "idleTimeoutInMinutes": 4,
            "ipAddress": "52.176.43.167",
            "provisioningState": "Succeeded",
            "publicIPAllocationMethod": "Static",
            "resourceGuid": "79e8baa3-33ce-466a-846c-37af3c721ce1"
        }
    }
    ```

   * `--resource-group`: A toocreate hello nyilvános IP-hello erőforráscsoport nevét.
   * `--name`: Hello nyilvános IP-cím neve.
   * `--location`: Mely toocreate hello nyilvános IP-cím azure-régiót.

3. Hello futtatása [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create) parancs toocreate statikus magánhálózati IP-címe a hálózati Adapterhez. hello kimenet után látható hello lista hello paramétereket ismerteti. 
   
    ```azurecli
    az network nic create \
    --resource-group TestRG \
    --name TestNIC \
    --location centralus \
    --subnet FrontEnd \
    --private-ip-address 192.168.1.101 \
    --vnet-name TestVNet
    ```

    Várt kimenet:
   
    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.101",
                "privateIPAllocationMethod": "Static",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>"
        }
    }
    ```
    
    Paraméterek:

    * `--private-ip-address`: Saját IP-cím statikus hello hálózati adaptert.
    * `--vnet-name`: Name hello vnet mely toocreate a hello hálózati adaptert.
    * `--subnet`: Mely toocreate hello hálózati hello alhálózatának nevét

4. Futtassa a hello [azure virtuális gép létrehozása](/cli/azure/vm/nic#create) parancs toocreate hello hello nyilvános IP-cím és a hálózati adapter használó virtuális gépek a fenti létrehozott. hello kimenet után látható hello lista hello paramétereket ismerteti.
   
    ```azurecli
    az vm create \
    --resource-group TestRG \
    --name DNS01 \
    --location centralus \
    --image Debian \
    --admin-username adminuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics TestNIC
    ```

    Várt kimenet:
   
    ```json
    {
        "fqdns": "",
        "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/DNS01",
        "location": "centralus",
        "macAddress": "00-0D-3A-92-C1-66",
        "powerState": "VM running",
        "privateIpAddress": "192.168.1.101",
        "publicIpAddress": "",
        "resourceGroup": "TestRG"
    }
    ```
   
   Alapszintű hello más paramétereket [az virtuális gép létrehozása](/cli/azure/vm#create) paraméterek.

   * `--nics`: Hello NIC toowhich hello virtuális gép neve van csatolva.
   

## <a name="retrieve-static-private-ip-address-information-for-a-vm"></a>Statikus magánhálózati IP-címadatok a virtuális gép beolvasása

tooview hello statikus magánhálózati IP-cím létrehozott, futtassa a következő parancs az Azure parancssori felület hello, és tekintse meg az értékeket hello *privát IP-foglalási-metódus* és *magánhálózati IP-cím*:

```azurecli
az vm show -g TestRG -n DNS01 --show-details --query 'privateIps'
```

Várt kimenet:

```json
"192.168.1.101"
```

toodisplay kifejezetten hello ezt a virtuális Gépet, a hálózati adapter lekérdezés hello hello NIC az adott IP-információit:

```azurecli
az network nic show \
-g testrg \
-n testnic \
--query 'ipConfigurations[0].{PrivateAddress:privateIpAddress,IPVer:privateIpAddressVersion,IpAllocMethod:p
rivateIpAllocationMethod,PublicAddress:publicIpAddress}'
```

hello eredménye a következőhöz hasonlóan:

```json
{
    "IPVer": "IPv4",
    "IpAllocMethod": "Static",
    "PrivateAddress": "192.168.1.101",
    "PublicAddress": null
}
```

## <a name="remove-a-static-private-ip-address-from-a-vm"></a>A statikus magánhálózati IP-cím eltávolítása a virtuális gépek

A resource manager üzembe helyezések hozzá statikus magánhálózati IP-cím nem távolítható el az Azure parancssori felület a hálózati Adapterhez. A következők szükségesek:
- Hozzon létre egy új hálózati Adaptert, amely egy dinamikus IP-cím használja
- A hálózati adapter hello be VM hello hello az újonnan létrehozott hálózati adaptert. 

a virtuális gép használt a fenti hello parancsok hello NIC toochange hello kövesse hello lépéseket.

1. Futtassa a hello **azure-hálózat hálózati adapter létrehozása** toocreate parancsot egy új hálózati Adaptert dinamikus IP-lefoglalást használatával új IP-címmel. Vegye figyelembe, hogy a megadott IP-cím, mert hello elosztási módszert **dinamikus**.

    ```azurecli
    az network nic create     \
    --resource-group TestRG     \
    --name TestNIC2     \
    --location centralus     \
    --subnet FrontEnd    \
    --vnet-name TestVNet
    ```        
   
    Várt kimenet:

    ```json
    {
        "newNIC": {
            "dnsSettings": {
            "appliedDnsServers": [],
            "dnsServers": []
            },
            "enableIPForwarding": false,
            "ipConfigurations": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC2/ipConfigurations/ipconfig1",
                "name": "ipconfig1",
                "properties": {
                "primary": true,
                "privateIPAddress": "192.168.1.4",
                "privateIPAllocationMethod": "Dynamic",
                "provisioningState": "Succeeded",
                "subnet": {
                    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                    "resourceGroup": "TestRG"
                }
                },
                "resourceGroup": "TestRG"
            }
            ],
            "provisioningState": "Succeeded",
            "resourceGuid": "0808a61c-476f-4d08-98ee-0fa83671b010"
        }
    }
    ```

2. Futtassa a hello **azure virtuálisgép-készlet** parancs toochange hello hello virtuális gép által használt hálózati adapter.
   
    ```azurecli
    azure vm set -g TestRG -n DNS01 -N TestNIC2
    ```

    Várt kimenet:
   
    ```json
    [
        {
            "id": "/subscriptions/0e220bf6-5caa-4e9f-8383-51f16b6c109f/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC3",
            "primary": true,
            "resourceGroup": "TestRG"
        }
    ]
    ```

    > [!NOTE]
    > Ha hello virtuális gép elég nagy toohave egynél több hálózati adapter, futtassa a hello **azure-hálózat hálózati törlése** parancs toodelete hello régi hálózati adaptert.
   
## <a name="next-steps"></a>Következő lépések
* További tudnivalók [foglalt nyilvános IP-cím](virtual-networks-reserved-public-ip.md) címek.
* További tudnivalók [példányszintű nyilvános IP (ILPIP)](virtual-networks-instance-level-public-ip.md) címek.
* Tekintse át a hello [fenntartott IP REST API-k](https://msdn.microsoft.com/library/azure/dn722420.aspx).

