---
title: "Szabályozhatja az Azure - PowerShell útválasztási és a virtuális készülékek |} Microsoft Docs"
description: "Megtudhatja, hogyan ellenőrzésére Útválasztás és a virtuális készülékek PowerShell használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 9582fdaa-249c-4c98-9618-8c30d496940f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.openlocfilehash: 3ab24f193c74449ae7414b4ea0675c0aae0211f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-user-defined-routes-udr-using-powershell"></a><span data-ttu-id="f01d2-103">Hozzon létre felhasználói útvonalakat (UDR) PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="f01d2-103">Create User-Defined Routes (UDR) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f01d2-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f01d2-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="f01d2-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f01d2-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="f01d2-106">Sablon</span><span class="sxs-lookup"><span data-stu-id="f01d2-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="f01d2-107">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="f01d2-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="f01d2-108">Parancssori felület (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="f01d2-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="f01d2-109">Az Azure-erőforrásokkal való munka megkezdése előtt fontos megérteni, hogy az Azure jelenleg két üzembe helyezési modellel rendelkezik, a Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="f01d2-109">Before you work with Azure resources, it's important to understand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="f01d2-110">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-resource-manager/resource-manager-deployment-model.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="f01d2-110">Make sure you understand [deployment models and tools](../azure-resource-manager/resource-manager-deployment-model.md) before you work with any Azure resource.</span></span> <span data-ttu-id="f01d2-111">A különféle eszközök dokumentációit a cikk tetején található fülekre kattintva tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="f01d2-111">You can view the documentation for different tools by clicking the tabs at the top of this article.</span></span>
>

<span data-ttu-id="f01d2-112">Ez a cikk a Resource Manager-alapú üzemi modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="f01d2-112">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="f01d2-113">Emellett [udr-EK létrehozása a klasszikus üzembe helyezési modellel](virtual-network-create-udr-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="f01d2-113">You can also [create UDRs in the classic deployment model](virtual-network-create-udr-classic-ps.md).</span></span>

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="f01d2-114">A minta az alábbi parancsok várt már létrehozott egy egyszerű környezetben PowerShell a fenti forgatókönyv alapján.</span><span class="sxs-lookup"><span data-stu-id="f01d2-114">The sample PowerShell commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="f01d2-115">Ha szeretné a parancsokat a jelen dokumentum megjelenített, először összeállítása a tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), kattintson a **az Azure telepítéséhez**, cserélje le az alapértelmezett paraméterértékek, ha szükséges, és kövesse az utasításokat a portálon.</span><span class="sxs-lookup"><span data-stu-id="f01d2-115">If you want to run the commands as they are displayed in this document, first build the test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a><span data-ttu-id="f01d2-116">Az előtér-alhálózat UDR létrehozása</span><span class="sxs-lookup"><span data-stu-id="f01d2-116">Create the UDR for the front-end subnet</span></span>
<span data-ttu-id="f01d2-117">Az útválasztási táblázatot és az előtér-alhálózat, a fenti forgatókönyv alapján szükséges útvonal létrehozásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="f01d2-117">To create the route table and route needed for the front-end subnet based on the scenario above, complete the following steps:</span></span>

1. <span data-ttu-id="f01d2-118">A háttér-alhálózat (192.168.2.0/24) irányítása irányuló teljes forgalomra küldéséhez használt útvonal létrehozása a **FW1** virtuális készülék (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="f01d2-118">Create a route used to send all traffic destined to the back-end subnet (192.168.2.0/24) to be routed to the **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToBackEnd `
    -AddressPrefix 192.168.2.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="f01d2-119">Hozzon létre egy útválasztási táblázatot nevű **UDR-előtérbeli** a a **westus** régiót, amelyben az útvonalat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f01d2-119">Create a route table named **UDR-FrontEnd** in the **westus** region that contains the route.</span></span>

    ```powershell
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-FrontEnd -Route $route
    ```

3. <span data-ttu-id="f01d2-120">Hozzon létre egy változó, amely tartalmazza a VNet, ahol az alhálózatban van.</span><span class="sxs-lookup"><span data-stu-id="f01d2-120">Create a variable that contains the VNet where the subnet is.</span></span> <span data-ttu-id="f01d2-121">A mi esetünkben a VNet neve **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="f01d2-121">In our scenario, the VNet is named **TestVNet**.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

4. <span data-ttu-id="f01d2-122">A fentiekben létrehozott útvonaltábla társítsa a **előtér** alhálózat.</span><span class="sxs-lookup"><span data-stu-id="f01d2-122">Associate the route table created above to the **FrontEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
    -AddressPrefix 192.168.1.0/24 -RouteTable $routeTable
    ```

    > [!WARNING]
    > <span data-ttu-id="f01d2-123">A fenti parancs kimenetét a virtuális hálózati konfiguráció objektumban, amelyet csak megtalálható a számítógépen, ahol a PowerShell futtatja tartalmának mutatja.</span><span class="sxs-lookup"><span data-stu-id="f01d2-123">The output for the command above shows the content for the virtual network configuration object, which only exists on the computer where you are running PowerShell.</span></span> <span data-ttu-id="f01d2-124">Futtatnia kell a **Set-AzureVirtualNetwork** parancsmagot, hogy ezek a beállítások mentése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="f01d2-124">You need to run the **Set-AzureVirtualNetwork** cmdlet to save these settings to Azure.</span></span>
    > 

5. <span data-ttu-id="f01d2-125">Mentse az új alhálózat-konfigurációt az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f01d2-125">Save the new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="f01d2-126">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f01d2-126">Expected output:</span></span>
   
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
   
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                                ...,
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-FrontEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              },
                                ...
                            ]    

## <a name="create-the-udr-for-the-back-end-subnet"></a><span data-ttu-id="f01d2-127">A háttér-alhálózat UDR létrehozása</span><span class="sxs-lookup"><span data-stu-id="f01d2-127">Create the UDR for the back-end subnet</span></span>

<span data-ttu-id="f01d2-128">Az útvonaltábla és a háttér-alhálózat, a fenti forgatókönyv alapján szükséges útvonal létrehozásához kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f01d2-128">To create the route table and route needed for the back-end subnet based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="f01d2-129">Hozzon létre az előtér-alhálózat (192.168.1.0/24) irányítása irányuló teljes forgalomra küldéséhez használt útvonalat a **FW1** virtuális készülék (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="f01d2-129">Create a route used to send all traffic destined to the front-end subnet (192.168.1.0/24) to be routed to the **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="f01d2-130">Hozzon létre egy útválasztási táblázatot nevű **UDR-háttérrendszer** a a **uswest** régió, amely tartalmazza az előbb létrehozott útvonal.</span><span class="sxs-lookup"><span data-stu-id="f01d2-130">Create a route table named **UDR-BackEnd** in the **uswest** region that contains the route created above.</span></span>

    ```
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-BackEnd -Route $route
    ```

3. <span data-ttu-id="f01d2-131">A fentiekben létrehozott útvonaltábla társítsa a **háttér** alhálózat.</span><span class="sxs-lookup"><span data-stu-id="f01d2-131">Associate the route table created above to the **BackEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd `
    -AddressPrefix 192.168.2.0/24 -RouteTable $routeTable
    ```

4. <span data-ttu-id="f01d2-132">Mentse az új alhálózat-konfigurációt az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f01d2-132">Save the new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="f01d2-133">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f01d2-133">Expected output:</span></span>
   
        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : westus
        Id                : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              : 
                            Name         Value
                            ===========  =====
                            displayName  VNet 
   
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              ...,
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL2/ipConfigurations/ipconfig1"
                                  },
                                  {
                                    "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICSQL1/ipConfigurations/ipconfig1"
                                  }
                                ],
                                "NetworkSecurityGroup": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BacEnd"
                                },
                                "RouteTable": {
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/routeTables/UDR-BackEnd"
                                },
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="f01d2-134">IP-továbbítás a FW1 engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f01d2-134">Enable IP forwarding on FW1</span></span>
<span data-ttu-id="f01d2-135">A hálózati adapter által használt IP-továbbítás engedélyezése **FW1**, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f01d2-135">To enable IP forwarding in the NIC used by **FW1**, follow the steps below.</span></span>

1. <span data-ttu-id="f01d2-136">Hozzon létre egy változó, amely tartalmazza a FW1 által használt hálózati beállításait.</span><span class="sxs-lookup"><span data-stu-id="f01d2-136">Create a variable that contains the settings for the NIC used by FW1.</span></span> <span data-ttu-id="f01d2-137">A mi esetünkben a hálózati adapter neve **NICFW1**.</span><span class="sxs-lookup"><span data-stu-id="f01d2-137">In our scenario, the NIC is named **NICFW1**.</span></span>

    ```powershell
    $nicfw1 = Get-AzureRmNetworkInterface -ResourceGroupName TestRG -Name NICFW1
    ```

2. <span data-ttu-id="f01d2-138">IP-továbbítás engedélyezése, és a hálózati beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="f01d2-138">Enable IP forwarding, and save the NIC settings.</span></span>

    ```powershell
    $nicfw1.EnableIPForwarding = 1
    Set-AzureRmNetworkInterface -NetworkInterface $nicfw1
    ```
   
    <span data-ttu-id="f01d2-139">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f01d2-139">Expected output:</span></span>
   
        Name                 : NICFW1
        ResourceGroupName    : TestRG
        Location             : westus
        Id                   : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1
        Etag                 : W/"[Id]"
        ProvisioningState    : Succeeded
        Tags                 : 
                               Name         Value                  
                               ===========  =======================
                               displayName  NetworkInterfaces - DMZ
   
        VirtualMachine       : {
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/virtualMachines/FW1"
                               }
        IpConfigurations     : [
                                 {
                                   "Name": "ipconfig1",
                                   "Etag": "W/\"[Id]\"",
                                   "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICFW1/ipConfigurations/ipconfig1",
                                   "PrivateIpAddress": "192.168.0.4",
                                   "PrivateIpAllocationMethod": "Static",
                                   "Subnet": {
                                     "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/DMZ"
                                   },
                                   "PublicIpAddress": {
                                     "Id": "/subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/publicIPAddresses/PIPFW1"
                                   },
                                   "LoadBalancerBackendAddressPools": [],
                                   "LoadBalancerInboundNatRules": [],
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
        DnsSettings          : {
                                 "DnsServers": [],
                                 "AppliedDnsServers": [],
                                 "InternalDnsNameLabel": null,
                                 "InternalFqdn": null
                               }
        EnableIPForwarding   : True
        NetworkSecurityGroup : null
        Primary              : True

