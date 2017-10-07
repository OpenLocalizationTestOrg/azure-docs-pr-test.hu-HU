---
title: "a virtuális hálózat – az Azure PowerShell aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy virtuális hálózathoz, PowerShell használatával."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a31f4f12-54ee-4339-b968-1a8097ca77d3
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/03/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d6e395a77f71de9f94b6304b05450e46b47544f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-powershell"></a>Hozzon létre egy virtuális hálózatot PowerShell használatával

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal. A Microsoft azt javasolja, hello Resource Manager üzembe helyezési modellben erőforrásoknak létrehozása. hello arról toolearn hello hello két modellek közötti különbséget olvasási [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md) cikk.
 
Ez a cikk azt ismerteti, hogyan toocreate egy Vnetet hello erőforrás-kezelő központi modellhez tartozó PowerShell-lel. Hozzon létre egy Vnetet Resource Manageren keresztül más eszközök használatával vagy hello klasszikus telepítési modell használatával VNet létrehozása a következő lista hello egy másik lehetőség kiválasztásával is:

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

virtuális hálózat PowerShell, a következő teljes hello lépések toocreate:

1. Telepítse és konfigurálja az Azure PowerShell hello hello utasításait követve [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) cikk.

2. Szükség esetén hozzon létre egy új erőforráscsoportot a lent látható módon. Ebben a forgatókönyvben nevű erőforráscsoport létrehozása *TestRG*. További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md) (Az Azure Resource Manager áttekintése).

    ```powershell   
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    Várt kimenet:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG    
3. Hozzon létre egy új virtuális hálózat nevű *TestVNet*:

    ```powershell
    New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location centralus
    ```

    Várt kimenet:

        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                   : W/"[Id]"
        ProvisioningState          : Succeeded
        Tags                       : 
        AddressSpace               : {
                                   "AddressPrefixes": [
                                     "192.168.0.0/16"
                                   ]
                                  }
        DhcpOptions                : {}
        Subnets                    : []
        VirtualNetworkPeerings     : []
4. Hello virtuális hálózat objektumot tárolható egy változóban:

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

   > [!TIP]
   > 3. és 4 lépést kombinálhatja futtatásával `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.
   > 

5. Alhálózati toohello új virtuális hálózat változó hozzáadása:

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
    ```

    Várt kimenet:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {}
        Subnets             : [
                                  {
                                    "Name": "FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24"
                                  }
                                ]
        VirtualNetworkPeerings     : []

6. A fenti 5. ismétlődő lépés az egyes alhálózatokon toocreate keresi. hello következő parancs létrehoz hello *háttér* alhálózati hello forgatókönyvhöz:

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
    ```

7. Ugyan létrehoz alhálózatokat, azok jelenleg csak szerepel hello helyi változó használt tooretrieve hello virtuális hálózatot hoz létre a fenti 4. toosave hello módosítások tooAzure, futtassa a következő parancs hello:

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```
   
    Várt kimenet:
   
        Name                  : TestVNet
        ResourceGroupName     : TestRG
        Location              : centralus
        Id                    : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag                  : W/"[Id]"
        ProvisioningState     : Succeeded
        Tags                  :
        AddressSpace          : {
                                  "AddressPrefixes": [
                                    "192.168.0.0/16"
                                  ]
                                }
        DhcpOptions           : {
                                  "DnsServers": []
                                }
        Subnets               : [
                                  {
                                    "Name": "FrontEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                    "AddressPrefix": "192.168.1.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  },
                                  {
                                    "Name": "BackEnd",
                                    "Etag": "W/\"[Id]\"",
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                    "AddressPrefix": "192.168.2.0/24",
                                    "IpConfigurations": [],
                                    "ProvisioningState": "Succeeded"
                                  }
                                ]
        VirtualNetworkPeerings : []

## <a name="next-steps"></a>Következő lépések

Megtudhatja, hogyan tooconnect:

- A virtuális gép (VM) tooa virtuális hálózati hello olvasásával [Windows virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-ps-create.md) cikk. Egy VNet és alhálózat létrehozása hello cikkek hello lépésekben, helyett választhatja egy meglévő VNet és alhálózat tooconnect egy virtuális Gépet.
- virtuális hálózat tooother virtuális hálózatok hello hello olvasásával [csatlakozás Vnetek](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) cikk.
- hello virtuális hálózati tooan a helyi hálózati helyek virtuális magánhálózati (VPN) vagy ExpressRoute-kapcsolatcsoportot. Megtudhatja, hogyan hello olvasásával [VNet tooan a helyi hálózat a telephelyek közötti VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) és [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](../expressroute/expressroute-howto-linkvnet-arm.md) cikkeket.
