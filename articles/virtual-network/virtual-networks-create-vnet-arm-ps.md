---
title: "Hozzon létre virtuális hálózatot - Azure PowerShell |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre virtuális hálózatot PowerShell használatával."
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
ms.openlocfilehash: e7072ddf51570d46578111e2e392e3cbea53f2aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-using-powershell"></a><span data-ttu-id="2e089-103">Hozzon létre egy virtuális hálózatot PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2e089-103">Create a virtual network using PowerShell</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="2e089-104">Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="2e089-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="2e089-105">A Microsoft azt javasolja, hogy az erőforrások létrehozásához használja a Resource Manager-alapú üzemi modellt.</span><span class="sxs-lookup"><span data-stu-id="2e089-105">Microsoft recommends creating resources through the Resource Manager deployment model.</span></span> <span data-ttu-id="2e089-106">A két modell közti különbségekkel kapcsolatos további információkért olvassa el [Az Azure üzemi modelljeinek megismerése](../azure-resource-manager/resource-manager-deployment-model.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="2e089-106">To learn more about the differences between the two models, read the [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>
 
<span data-ttu-id="2e089-107">Ez a cikk azt ismerteti, hogyan hozhat létre egy Vnetet keresztül a Resource Manager üzembe helyezési modellel PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="2e089-107">This article explains how to create a VNet through the Resource Manager deployment model using PowerShell.</span></span> <span data-ttu-id="2e089-108">Resource Manager-alapú VNetet létrehozhat egyéb eszközökkel is, illetve létrehozhat VNetet a klasszikus üzemi modellben is, ha az alábbi listából egy másik lehetőséget választ:</span><span class="sxs-lookup"><span data-stu-id="2e089-108">You can also create a VNet through Resource Manager using other tools or create a VNet through the classic deployment model by selecting a different option from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="2e089-109">Portál</span><span class="sxs-lookup"><span data-stu-id="2e089-109">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
> * [<span data-ttu-id="2e089-110">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2e089-110">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
> * [<span data-ttu-id="2e089-111">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="2e089-111">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
> * [<span data-ttu-id="2e089-112">Sablon</span><span class="sxs-lookup"><span data-stu-id="2e089-112">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
> * [<span data-ttu-id="2e089-113">Portál (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="2e089-113">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
> * [<span data-ttu-id="2e089-114">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="2e089-114">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [<span data-ttu-id="2e089-115">Parancssori felület (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="2e089-115">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a><span data-ttu-id="2e089-116">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e089-116">Create a virtual network</span></span>

<span data-ttu-id="2e089-117">PowerShell-lel virtuális hálózat létrehozásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2e089-117">To create a virtual network using PowerShell, complete the following steps:</span></span>

1. <span data-ttu-id="2e089-118">Telepítse és konfigurálja az Azure PowerShell, a lépések a [telepítése és konfigurálása az Azure PowerShell](/powershell/azure/overview) cikk.</span><span class="sxs-lookup"><span data-stu-id="2e089-118">Install and configure Azure PowerShell, by following the steps in the [How to Install and Configure Azure PowerShell](/powershell/azure/overview) article.</span></span>

2. <span data-ttu-id="2e089-119">Szükség esetén hozzon létre egy új erőforráscsoportot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="2e089-119">If necessary, create a new resource group, as shown below.</span></span> <span data-ttu-id="2e089-120">Ebben a forgatókönyvben nevű erőforráscsoport létrehozása *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="2e089-120">For this scenario, create a resource group named *TestRG*.</span></span> <span data-ttu-id="2e089-121">További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md) (Az Azure Resource Manager áttekintése).</span><span class="sxs-lookup"><span data-stu-id="2e089-121">For more information about resource groups, visit [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>

    ```powershell   
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    <span data-ttu-id="2e089-122">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2e089-122">Expected output:</span></span>

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/[Subscription Id]/resourceGroups/TestRG    
3. <span data-ttu-id="2e089-123">Hozzon létre egy új virtuális hálózat nevű *TestVNet*:</span><span class="sxs-lookup"><span data-stu-id="2e089-123">Create a new VNet named *TestVNet*:</span></span>

    ```powershell
    New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet `
    -AddressPrefix 192.168.0.0/16 -Location centralus
    ```

    <span data-ttu-id="2e089-124">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2e089-124">Expected output:</span></span>

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
4. <span data-ttu-id="2e089-125">A virtuális hálózat objektumot tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="2e089-125">Store the virtual network object in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

   > [!TIP]
   > <span data-ttu-id="2e089-126">3. és 4 lépést kombinálhatja futtatásával `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.</span><span class="sxs-lookup"><span data-stu-id="2e089-126">You can combine steps 3 and 4 by running `$vnet = New-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet -AddressPrefix 192.168.0.0/16 -Location centralus`.</span></span>
   > 

5. <span data-ttu-id="2e089-127">Adjon hozzá egy alhálózatot az új VNet változóhoz:</span><span class="sxs-lookup"><span data-stu-id="2e089-127">Add a subnet to the new VNet variable:</span></span>

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name FrontEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.1.0/24
    ```

    <span data-ttu-id="2e089-128">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2e089-128">Expected output:</span></span>
   
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

6. <span data-ttu-id="2e089-129">A fenti 5. lépést ismételje meg minden létrehozni kívánt alhálózat esetében.</span><span class="sxs-lookup"><span data-stu-id="2e089-129">Repeat step 5 above for each subnet you want to create.</span></span> <span data-ttu-id="2e089-130">A következő parancs létrehozza a *háttér* alhálózati az esethez:</span><span class="sxs-lookup"><span data-stu-id="2e089-130">The following command creates the *BackEnd* subnet for the scenario:</span></span>

    ```powershell
    Add-AzureRmVirtualNetworkSubnetConfig -Name BackEnd `
    -VirtualNetwork $vnet -AddressPrefix 192.168.2.0/24
    ```

7. <span data-ttu-id="2e089-131">Ugyan létrehoz alhálózatokat, azok jelenleg csak a VNet lekéréséhez a 4. lépésben használt helyi változóban léteznek.</span><span class="sxs-lookup"><span data-stu-id="2e089-131">Although you create subnets, they currently only exist in the local variable used to retrieve the VNet you create in step 4 above.</span></span> <span data-ttu-id="2e089-132">Menti a módosításokat az Azure-ba, a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2e089-132">To save the changes to Azure, run the following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```
   
    <span data-ttu-id="2e089-133">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2e089-133">Expected output:</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="2e089-134">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e089-134">Next steps</span></span>

<span data-ttu-id="2e089-135">Ismerje meg, hogyan csatlakoztathat:</span><span class="sxs-lookup"><span data-stu-id="2e089-135">Learn how to connect:</span></span>

- <span data-ttu-id="2e089-136">A virtuális gép (VM) virtuális hálózathoz olvasásával a [Windows virtuális gép létrehozása](../virtual-machines/virtual-machines-windows-ps-create.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="2e089-136">A virtual machine (VM) to a virtual network by reading the [Create a Windows VM](../virtual-machines/virtual-machines-windows-ps-create.md) article.</span></span> <span data-ttu-id="2e089-137">Ha nem szeretne egy VNetet és alhálózatot létrehozni a cikkben ismertetett lépések szerint, használhat meglévő VNetet és alhálózatot, amelyekhez kapcsolódhat.</span><span class="sxs-lookup"><span data-stu-id="2e089-137">Instead of creating a VNet and subnet in the steps of the articles, you can select an existing VNet and subnet to connect a VM to.</span></span>
- <span data-ttu-id="2e089-138">virtuális hálózatot más virtuális hálózatokhoz – olvassa el a [virtuális hálózatok csatlakoztatását](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="2e089-138">The virtual network to other virtual networks by reading the [Connect VNets](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) article.</span></span>
- <span data-ttu-id="2e089-139">virtuális hálózatot helyszíni hálózathoz helyek közti virtuális magánhálózat (VPN) vagy ExpressRoute-kapcsolatcsoport használatával.</span><span class="sxs-lookup"><span data-stu-id="2e089-139">The virtual network to an on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="2e089-140">Az elsajátításához olvassa el a [Virtuális hálózat csatlakoztatása helyszíni hálózathoz helyek közti VPN használatával](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) és a [Virtuális hálózat csatlakoztatása ExpressRoute-kapcsolatcsoporthoz](../expressroute/expressroute-howto-linkvnet-arm.md) eljárásokat ismertető cikkeket.</span><span class="sxs-lookup"><span data-stu-id="2e089-140">Learn how by reading the [Connect a VNet to an on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet to an ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-arm.md) articles.</span></span>
