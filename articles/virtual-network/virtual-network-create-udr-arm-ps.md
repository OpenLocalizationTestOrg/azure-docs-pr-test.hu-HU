---
title: "az Azure - PowerShell útválasztási és a virtuális készülékek aaaControl |} Microsoft Docs"
description: "Megtudhatja, hogyan toocontrol útválasztási és a virtuális készülékek PowerShell használatával."
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
ms.openlocfilehash: b7b8717529eb2cd8b1d28b8ab9c6e21159d14882
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-powershell"></a><span data-ttu-id="f7f7c-103">Hozzon létre felhasználói útvonalakat (UDR) PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="f7f7c-103">Create User-Defined Routes (UDR) using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="f7f7c-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f7f7c-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="f7f7c-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f7f7c-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="f7f7c-106">Sablon</span><span class="sxs-lookup"><span data-stu-id="f7f7c-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="f7f7c-107">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="f7f7c-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="f7f7c-108">Parancssori felület (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="f7f7c-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

> [!IMPORTANT]
> <span data-ttu-id="f7f7c-109">Azure-erőforrások használata előtt-e, hogy Azure jelenleg két üzembe helyezési modellel rendelkezik fontos toounderstand: Azure Resource Manager és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-109">Before you work with Azure resources, it's important toounderstand that Azure currently has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="f7f7c-110">Bizonyosodjon meg arról, hogy megfelelő ismeretekkel rendelkezik az [üzembe helyezési modellekről és eszközökről](../azure-resource-manager/resource-manager-deployment-model.md), mielőtt elkezdene dolgozni az Azure-erőforrásokkal.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-110">Make sure you understand [deployment models and tools](../azure-resource-manager/resource-manager-deployment-model.md) before you work with any Azure resource.</span></span> <span data-ttu-id="f7f7c-111">Ez a cikk hello tetején hello fülekre kattintva megtekintheti a különféle eszközök dokumentációit hello.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-111">You can view hello documentation for different tools by clicking hello tabs at hello top of this article.</span></span>
>

<span data-ttu-id="f7f7c-112">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-112">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="f7f7c-113">Emellett [udr-EK létrehozása hello klasszikus üzembe helyezési modellel](virtual-network-create-udr-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="f7f7c-113">You can also [create UDRs in hello classic deployment model](virtual-network-create-udr-classic-ps.md).</span></span>

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="f7f7c-114">hello minta az alábbi parancsok várt már létrehozott egy egyszerű környezetben PowerShell fenti hello forgatókönyv alapján.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-114">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="f7f7c-115">Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre hello tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek Ha szükséges, és kövesse az utasításokat hello a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-115">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="f7f7c-116">Hello UDR hello előtér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f7f7c-116">Create hello UDR for hello front-end subnet</span></span>
<span data-ttu-id="f7f7c-117">toocreate hello útválasztási táblázatot és hello előtér-alhálózatot a szükséges útvonal alapján a fenti lépések teljes hello hello forgatókönyv:</span><span class="sxs-lookup"><span data-stu-id="f7f7c-117">toocreate hello route table and route needed for hello front-end subnet based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="f7f7c-118">Hozzon létre egy használt útvonal toosend összes adatforgalmat toohello háttér-alhálózat (192.168.2.0/24) irányított toobe toohello **FW1** virtuális készülék (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="f7f7c-118">Create a route used toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toobe routed toohello **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToBackEnd `
    -AddressPrefix 192.168.2.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="f7f7c-119">Hozzon létre egy útválasztási táblázatot nevű **UDR-előtérbeli** a hello **westus** hello útvonal tartalmazó régióban.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-119">Create a route table named **UDR-FrontEnd** in hello **westus** region that contains hello route.</span></span>

    ```powershell
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-FrontEnd -Route $route
    ```

3. <span data-ttu-id="f7f7c-120">Hozzon létre egy változót, ahol hello alhálózati az VNet hello tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-120">Create a variable that contains hello VNet where hello subnet is.</span></span> <span data-ttu-id="f7f7c-121">A mi esetünkben hello VNet neve **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-121">In our scenario, hello VNet is named **TestVNet**.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

4. <span data-ttu-id="f7f7c-122">A fenti toohello létrehozott társítása hello útvonaltábla **előtér** alhálózat.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-122">Associate hello route table created above toohello **FrontEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
    -AddressPrefix 192.168.1.0/24 -RouteTable $routeTable
    ```

    > [!WARNING]
    > <span data-ttu-id="f7f7c-123">hello kimeneti hello parancs a fenti csak létezik PowerShell futtató hello számítógépen hello virtuális hálózati konfiguráció objektum hello tartalmának mutatja.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-123">hello output for hello command above shows hello content for hello virtual network configuration object, which only exists on hello computer where you are running PowerShell.</span></span> <span data-ttu-id="f7f7c-124">Toorun hello kell **Set-AzureVirtualNetwork** parancsmag toosave ezen beállítások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-124">You need toorun hello **Set-AzureVirtualNetwork** cmdlet toosave these settings tooAzure.</span></span>
    > 

5. <span data-ttu-id="f7f7c-125">Mentse a hello új alhálózat beállítása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-125">Save hello new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="f7f7c-126">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f7f7c-126">Expected output:</span></span>
   
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

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="f7f7c-127">Hello UDR hello háttér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="f7f7c-127">Create hello UDR for hello back-end subnet</span></span>

<span data-ttu-id="f7f7c-128">toocreate hello útválasztási táblázatot és hello háttér-alhálózat a fenti hello forgatókönyv alapján szükséges útvonal kövesse a hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-128">toocreate hello route table and route needed for hello back-end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="f7f7c-129">Hozzon létre egy használt útvonal toosend összes adatforgalmat toohello előtér-alhálózat (192.168.1.0/24) irányított toobe toohello **FW1** virtuális készülék (192.168.0.4).</span><span class="sxs-lookup"><span data-stu-id="f7f7c-129">Create a route used toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toobe routed toohello **FW1** virtual appliance (192.168.0.4).</span></span>

    ```powershell
    $route = New-AzureRmRouteConfig -Name RouteToFrontEnd `
    -AddressPrefix 192.168.1.0/24 -NextHopType VirtualAppliance `
    -NextHopIpAddress 192.168.0.4
    ```

2. <span data-ttu-id="f7f7c-130">Hozzon létre egy útválasztási táblázatot nevű **UDR-háttérrendszer** a hello **uswest** hello útvonal tartalmazó régió a fenti létrehozott.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-130">Create a route table named **UDR-BackEnd** in hello **uswest** region that contains hello route created above.</span></span>

    ```
    $routeTable = New-AzureRmRouteTable -ResourceGroupName TestRG -Location westus `
    -Name UDR-BackEnd -Route $route
    ```

3. <span data-ttu-id="f7f7c-131">A fenti toohello létrehozott társítása hello útvonaltábla **háttér** alhálózat.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-131">Associate hello route table created above toohello **BackEnd** subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd `
    -AddressPrefix 192.168.2.0/24 -RouteTable $routeTable
    ```

4. <span data-ttu-id="f7f7c-132">Mentse a hello új alhálózat beállítása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-132">Save hello new subnet configuration in Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="f7f7c-133">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f7f7c-133">Expected output:</span></span>
   
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

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="f7f7c-134">IP-továbbítás a FW1 engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f7f7c-134">Enable IP forwarding on FW1</span></span>
<span data-ttu-id="f7f7c-135">a hálózati adapter által használt hello tooenable IP-továbbítás **FW1**, kövesse az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-135">tooenable IP forwarding in hello NIC used by **FW1**, follow hello steps below.</span></span>

1. <span data-ttu-id="f7f7c-136">Hozzon létre egy változó, amely hello FW1 által használt hálózati hello beállításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-136">Create a variable that contains hello settings for hello NIC used by FW1.</span></span> <span data-ttu-id="f7f7c-137">A mi esetünkben hello hálózati adapter neve **NICFW1**.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-137">In our scenario, hello NIC is named **NICFW1**.</span></span>

    ```powershell
    $nicfw1 = Get-AzureRmNetworkInterface -ResourceGroupName TestRG -Name NICFW1
    ```

2. <span data-ttu-id="f7f7c-138">IP-továbbítás engedélyezése, és mentse a hello hálózati kártya beállításait.</span><span class="sxs-lookup"><span data-stu-id="f7f7c-138">Enable IP forwarding, and save hello NIC settings.</span></span>

    ```powershell
    $nicfw1.EnableIPForwarding = 1
    Set-AzureRmNetworkInterface -NetworkInterface $nicfw1
    ```
   
    <span data-ttu-id="f7f7c-139">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="f7f7c-139">Expected output:</span></span>
   
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

