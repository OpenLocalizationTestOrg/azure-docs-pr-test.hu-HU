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
# <a name="create-a-virtual-network-using-powershell"></a><span data-ttu-id="81221-103">Hozzon létre egy virtuális hálózatot PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="81221-103">Create a virtual network using PowerShell</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="81221-104">Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="81221-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="81221-105">A Microsoft azt javasolja, hello Resource Manager üzembe helyezési modellben erőforrásoknak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="81221-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="81221-106">hello arról toolearn hello hello két modellek közötti különbséget olvasási [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="81221-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>
 
<span data-ttu-id="81221-107">Ez a cikk azt ismerteti, hogyan toocreate egy Vnetet hello erőforrás-kezelő központi modellhez tartozó PowerShell-lel.</span><span class="sxs-lookup"><span data-stu-id="81221-107">This article explains how toocreate a VNet through hello Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="81221-108">Hozzon létre egy Vnetet Resource Manageren keresztül más eszközök használatával vagy hello klasszikus telepítési modell használatával VNet létrehozása a következő lista hello egy másik lehetőség kiválasztásával is:</span><span class="sxs-lookup"><span data-stu-id="81221-108">You can also create a VNet through Resource Manager using other tools or create a VNet through hello classic deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="81221-109">Portál</span><span class="sxs-lookup"><span data-stu-id="81221-109">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
> * [<span data-ttu-id="81221-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="81221-110">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
> * [<span data-ttu-id="81221-111">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="81221-111">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
> * [<span data-ttu-id="81221-112">Sablon</span><span class="sxs-lookup"><span data-stu-id="81221-112">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
> * [<span data-ttu-id="81221-113">Portál (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="81221-113">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
> * [<span data-ttu-id="81221-114">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="81221-114">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [<span data-ttu-id="81221-115">Parancssori felület (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="81221-115">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a><span data-ttu-id="81221-116">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="81221-116">Create a virtual network</span></span>

<span data-ttu-id="81221-117">virtuális hálózat PowerShell, a következő teljes hello lépések toocreate:</span><span class="sxs-lookup"><span data-stu-id="81221-117">toocreate a virtual network using PowerShell, complete hello following steps:</span></span>

1. <span data-ttu-id="81221-118">Telepítse és konfigurálja az Azure PowerShell hello hello utasításait követve [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="81221-118">Install and configure Azure PowerShell, by following hello steps in hello [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>

2. <span data-ttu-id="81221-119">Szükség esetén hozzon létre egy új erőforráscsoportot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="81221-119">If necessary, create a new resource group, as shown below.</span></span> <span data-ttu-id="81221-120">Ebben a forgatókönyvben nevű erőforráscsoport létrehozása *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="81221-120">For this scenario, create a resource group named *TestRG*.</span></span> <span data-ttu-id="81221-121">További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md) (Az Azure Resource Manager áttekintése).</span><span class="sxs-lookup"><span data-stu-id="81221-121">For more information about resource groups, visit [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    ```powershell   
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    <span data-ttu-id="81221-122">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81221-122">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG    
3. <span data-ttu-id="81221-123">Hozzon létre egy új virtuális hálózat nevű *TestVNet*:</span><span class="sxs-lookup"><span data-stu-id="81221-123">Create a new VNet named *TestVNet*:</span></span>

    ```powershell
    New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location centralus
    ```

    <span data-ttu-id="81221-124">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81221-124">Expected output:</span></span>

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
4. <span data-ttu-id="81221-125">Hello virtuális hálózat objektumot tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="81221-125">Store hello virtual network object in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

   > [!TIP]
   > <span data-ttu-id="81221-126">3. és 4 lépést kombinálhatja futtatásával `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.</span><span class="sxs-lookup"><span data-stu-id="81221-126">You can combine steps 3 and 4 by running `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.</span></span>
   > 

5. <span data-ttu-id="81221-127">Alhálózati toohello új virtuális hálózat változó hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="81221-127">Add a subnet toohello new VNet variable:</span></span>

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
    ```

    <span data-ttu-id="81221-128">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81221-128">Expected output:</span></span>
   
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

6. <span data-ttu-id="81221-129">A fenti 5. ismétlődő lépés az egyes alhálózatokon toocreate keresi.</span><span class="sxs-lookup"><span data-stu-id="81221-129">Repeat step 5 above for each subnet you want toocreate.</span></span> <span data-ttu-id="81221-130">hello következő parancs létrehoz hello *háttér* alhálózati hello forgatókönyvhöz:</span><span class="sxs-lookup"><span data-stu-id="81221-130">hello following command creates hello *BackEnd* subnet for hello scenario:</span></span>

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
    ```

7. <span data-ttu-id="81221-131">Ugyan létrehoz alhálózatokat, azok jelenleg csak szerepel hello helyi változó használt tooretrieve hello virtuális hálózatot hoz létre a fenti 4.</span><span class="sxs-lookup"><span data-stu-id="81221-131">Although you create subnets, they currently only exist in hello local variable used tooretrieve hello VNet you create in step 4 above.</span></span> <span data-ttu-id="81221-132">toosave hello módosítások tooAzure, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="81221-132">toosave hello changes tooAzure, run hello following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```
   
    <span data-ttu-id="81221-133">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81221-133">Expected output:</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="81221-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81221-134">Next steps</span></span>

<span data-ttu-id="81221-135">Megtudhatja, hogyan tooconnect:</span><span class="sxs-lookup"><span data-stu-id="81221-135">Learn how tooconnect:</span></span>

- <span data-ttu-id="81221-136">A virtuális gép (VM) tooa virtuális hálózati hello olvasásával [Windows virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-ps-create.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="81221-136">A virtual machine (VM) tooa virtual network by reading hello [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md) article.</span></span> <span data-ttu-id="81221-137">Egy VNet és alhálózat létrehozása hello cikkek hello lépésekben, helyett választhatja egy meglévő VNet és alhálózat tooconnect egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="81221-137">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="81221-138">virtuális hálózat tooother virtuális hálózatok hello hello olvasásával [csatlakozás Vnetek](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="81221-138">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) article.</span></span>
- <span data-ttu-id="81221-139">hello virtuális hálózati tooan a helyi hálózati helyek virtuális magánhálózati (VPN) vagy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="81221-139">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="81221-140">Megtudhatja, hogyan hello olvasásával [VNet tooan a helyi hálózat a telephelyek közötti VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) és [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](../expressroute/expressroute-howto-linkvnet-arm.md) cikkeket.</span><span class="sxs-lookup"><span data-stu-id="81221-140">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) articles.</span></span>
