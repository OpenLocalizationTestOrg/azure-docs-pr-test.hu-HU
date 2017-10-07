---
title: "hálózati biztonsági csoport – Azure PowerShell aaaManage |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage hálózati biztonsági csoportok PowerShell használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3706ce6c-d9ae-46cb-a048-f0a4e84dc5cc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 930fe5e0827896ad67b24d84e41a5d3f898ba838
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-powershell"></a>PowerShell-lel hálózati biztonsági csoportok kezelése

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md). Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a>Információk beolvasása
Megtekintheti a meglévő NSG-ket, szabályok lekérdezni egy meglévő NSG-t, és megtudhatja, milyen erőforrásokat egy NSG társítva.

### <a name="view-existing-nsgs"></a>Meglévő NSG-k megtekintése
tooview minden meglévő NSG-ket egy előfizetésben futtatása hello `Get-AzureRmNetworkSecurityGroup` parancsmag.

A várt eredmény:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : WEB1
    ResourceGroupName    : RG101
    Location             : eastus2
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG101/providers/M
                           icrosoft.Network/networkSecurityGroups/WEB1
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]


tooview hello listája az NSG-k egy adott erőforráscsoportban, futtassa a hello `Get-AzureRmNetworkSecurityGroup` parancsmag.

Várt kimenet:

    Name                 : NSG-BackEnd
    ResourceGroupName    : RG-NSG
    Location             : westus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-BackEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 :                            
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

    Name                 : NSG-FrontEnd
    ResourceGroupName    : RG-NSG
    Location             : eastus
    Id                   : /subscriptions/[Subscription Id]/resourceGroups/NRP-RG/providers/
                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    Etag                 : W/"[Id]"
    ResourceGuid         : [Id]
    ProvisioningState    : Succeeded
    Tags                 : 
    SecurityRules        : [...]
    DefaultSecurityRules : [...]
    NetworkInterfaces    : [...]
    Subnets              : [...]

### <a name="list-all-rules-for-an-nsg"></a>A szabályok egy NSG listázása
az NSG nevű tooview hello szabályainak **NSG-előtérbeli**, adja meg a következő parancs hello:

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules
```

Várt kimenet:

    Name                     : rdp-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow RDP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 100
    Direction                : Inbound

    Name                     : web-rule
    Id                       : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/                           Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
    Etag                     : W/"[Id]"
    ProvisioningState        : Succeeded
    Description              : Allow HTTP
    Protocol                 : Tcp
    SourcePortRange          : *
    DestinationPortRange     : 80
    SourceAddressPrefix      : Internet
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 101
    Direction                : Inbound

> [!NOTE]
> Is `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` toolist hello alapértelmezett szabályok hello **NSG-előtérbeli** NSG.
> 

### <a name="view-nsgs-associations"></a>Az NSG-társításának megtekintéséhez
tooview milyen erőforrásokat hello **NSG-előtérbeli** NSG futtatási hello, rendeljen hozzá a következő parancsot:

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
```

Keresse meg hello **hálózati illesztők** és **alhálózatok** tulajdonságok alább látható módon:

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

Hello előző példában hello NSG nincs társított tooany hálózati adapterek (NIC); nevű társított tooa alhálózat **előtér**.

## <a name="manage-rules"></a>Szabályok kezelése
Adja hozzá a meglévő NSG szabályok tooan, szerkesztheti a meglévő szabályokat, és törölje a szabályokat.

### <a name="add-a-rule"></a>Szabály hozzáadása
egy szabály, amely lehetővé teszi tooadd **bejövő** forgalom tooport **443-as** bármely gépen toohello a **NSG-előtér** NSG-t, a következő lépéseket teljes hello:

1. Futtassa a következő parancs tooretrieve hello NSG meglévő hello és tárolható egy változóban:

    ```powershell   
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Futtassa a következő parancs tooadd hello egy szabály toohello NSG:

    ```powershell
    Add-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. toosave hello módosítások toohello NSG, futtassa a következő parancs hello:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
    Várt kimenet ábrázoló csak hello vonatkozó biztonsági szabályokat:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "*",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="change-a-rule"></a>Szabály módosítása
a fenti tooallow létrehozott toochange hello szabály hello érkező bejövő adatforgalmat **Internet** csak, kövesse az alábbi hello lépéseket.

1. Futtassa a következő parancs tooretrieve hello NSG meglévő hello és tárolható egy változóban:

    ```powershell 
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Futtassa a következő beállításokkal hello új szabály parancs hello:

    ```powershell
    Set-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg `
    -Name https-rule `
    -Description "Allow HTTPS" `
    -Access Allow `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 102 `
    -SourceAddressPrefix Internet `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 443
    ```

3. toosave hello módosítások toohello NSG, futtassa a következő parancs hello:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    Várt kimenet ábrázoló csak hello vonatkozó biztonsági szabályokat:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 },
                                 {
                                   "Name": "https-rule",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/https-rule",
                                   "Description": "Allow HTTPS",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "443",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 102,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]

### <a name="delete-a-rule"></a>Szabály törlése
1. Futtassa a következő parancs tooretrieve hello NSG meglévő hello és tárolható egy változóban:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Futtassa a parancsot tooremove hello szabály követően – hello NSG hello:

    ```powershell
    Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name https-rule
    ```

3. Hello végrehajtott módosítások toohello NSG, mentés hello a következő parancs futtatásával:

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    Várt kimenet megjelenítése csak hello vonatkozó biztonsági szabályokat, értesítés hello **https-szabály** nem jelenik meg:
   
        Name                 : NSG-FrontEnd
        ...
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   ...
                                 },
                                 {
                                   "Name": "web-rule",
                                   ...
                                 }
                               ]

## <a name="manage-associations"></a>Társítások kezelése
Az NSG toosubnets és a hálózati adapterek is hozzárendelhető. Is is leválasztja az összes erőforrásból, amelyekhez társítva vannak a NSG.

### <a name="associate-an-nsg-tooa-nic"></a>Társítson egy NSG tooa hálózati adapter
tooassociate hello **NSG-előtérbeli** NSG toohello **TestNICWeb1** a hálózati adapter teljes hello a következő lépéseket:

1. Futtassa a következő parancs tooretrieve hello NSG meglévő hello és tárolható egy változóban:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. Futtassa a következő parancs tooretrieve hello meglévő hálózati hello és tárolható egy változóban:

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

3. Set hello **hálózati biztonsági csoporthoz tartozik** hello tulajdonságának **NIC** hello változó toohello értékének **NSG** változó hello a következő parancs beírásával:

    ```powershell
    $nic.NetworkSecurityGroup = $nsg
    ```

4. toosave hello módosítások toohello hálózati adapter, futtassa a következő parancs hello:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    Várt kimenet ábrázoló csak hello **hálózati biztonsági csoporthoz tartozik** tulajdonság:
   
        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a>A társítást egy NSG-t a hálózati Adapterhez
toodissociate hello **NSG-előtérbeli** hello az NSG **TestNICWeb1** a hálózati adapter teljes hello a következő lépéseket:

1. Futtassa a következő parancs tooretrieve hello meglévő hálózati hello és tárolható egy változóban:

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

2. Set hello **hálózati biztonsági csoporthoz tartozik** hello tulajdonságának **NIC** változó túl**$null** hello a következő parancs futtatásával:

    ```powershell
    $nic.NetworkSecurityGroup = $null
    ```

3. toosave hello módosítások toohello hálózati adapter, futtassa a következő parancs hello:

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    Várt kimenet ábrázoló csak hello **hálózati biztonsági csoporthoz tartozik** tulajdonság:
   
        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a>Az NSG alhálózatból származó leválasztani
toodissociate hello **NSG-előtérbeli** hello az NSG **előtér** alhálózat, a teljes hello a következő lépéseket:

1. Futtassa a következő parancs tooretrieve hello meglévő VNet hello és tárolható egy változóban:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. Futtatási hello parancs tooretrieve hello következő **előtér** alhálózati és tárolható egy változóban:

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. Set hello **hálózati biztonsági csoporthoz tartozik** hello tulajdonságának **alhálózati** változó túl**$null** hello a következő parancs beírásával:

    ```powershell
    $subnet.NetworkSecurityGroup = $null
    ```

4. toosave hello módosítások toohello alhálózati, futtassa a következő parancs hello:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    Várt kimenet hello csak hello tulajdonságait megjelenítő **előtér** alhálózat. A tulajdonság nem létezik figyelmeztetés **hálózati biztonsági csoporthoz tartozik**:
   
            ...
            Subnets           : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                      },
                                      {
                                        "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                      }
                                    ],
                                    "ProvisioningState": "Succeeded"
                                  },
                                    ...
                                ]

### <a name="associate-an-nsg-tooa-subnet"></a>Társítsa az NSG-tooa alhálózatot.
tooassociate hello **NSG-előtérbeli** NSG toohello **FronEnd** alhálózati újra, a teljes hello a következő lépéseket:

1. Futtassa a következő parancs tooretrieve hello meglévő VNet hello és tárolható egy változóban:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. Futtatási hello parancs tooretrieve hello következő **előtér** alhálózati és tárolható egy változóban:

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. Futtassa a következő parancs tooretrieve hello NSG meglévő hello és tárolható egy változóban:

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

4. Set hello **hálózati biztonsági csoporthoz tartozik** hello tulajdonságának **alhálózati** változó túl**$null** hello a következő parancs futtatásával:

    ```powershell
    $subnet.NetworkSecurityGroup = $nsg
    ```

5. toosave hello módosítások toohello alhálózati, futtassa a következő parancs hello:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    Várt kimenet ábrázoló csak hello **hálózati biztonsági csoporthoz tartozik** hello tulajdonságának **előtér** alhálózati:
   
        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a>Az NSG törlése
Ha nem kapcsolódnak hozzá erőforrás tooany csak törlése egy NSG. az NSG toodelete kövesse hello lépéseket.

1. toocheck hello erőforrásokhoz rendelt tooan NSG, futtassa a hello `azure network nsg show` látható módon [nézet NSG-ket társítások](#View-NSGs-associations).
2. Ha hello NSG társított tooany hálózati adapterek, futtassa a hello `azure network nic set` látható módon [leválasztani a hálózati Adapterhez egy NSG](#Dissociate-an-NSG-from-a-NIC) az egyes hálózati adapterhez. 
3. Ha hello NSG társított tooany alhálózati, futtassa a hello `azure network vnet subnet set` látható módon [leválasztani az NSG alhálózatból származó](#Dissociate-an-NSG-from-a-subnet) az egyes alhálózatokon.
4. toodelete hello NSG, futtassa a következő parancs hello:

    ```powershell
    Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force
    ```
   
   > [!NOTE]
   > Hello `-Force` paraméter biztosítja, nem kell tooconfirm hello törlését.
   > 

## <a name="next-steps"></a>Következő lépések
* [Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.

