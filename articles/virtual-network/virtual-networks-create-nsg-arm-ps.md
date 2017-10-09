---
title: "hálózati biztonsági csoport – Azure PowerShell aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és hálózati biztonsági csoportok PowerShell használatával telepítheti."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9cef62b8-d889-4d16-b4d0-58639539a418
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/23/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1c8db773febb163d9cb010d23f2913b5ebe0fa94
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-powershell"></a><span data-ttu-id="af5d5-103">Hozza létre a hálózati biztonsági csoportokat PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="af5d5-103">Create network security groups using PowerShell</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

<span data-ttu-id="af5d5-104">Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="af5d5-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="af5d5-105">A Microsoft azt javasolja, hello Resource Manager üzembe helyezési modellben erőforrásoknak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="af5d5-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="af5d5-106">hello arról toolearn hello hello két modellek közötti különbséget olvasási [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="af5d5-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span> <span data-ttu-id="af5d5-107">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="af5d5-107">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="af5d5-108">Emellett [NSG-k létrehozása hello klasszikus üzembe helyezési modellel](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="af5d5-108">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="af5d5-109">hello minta az alábbi parancsok várt már létrehozott egy egyszerű környezetben PowerShell fenti hello forgatókönyv alapján.</span><span class="sxs-lookup"><span data-stu-id="af5d5-109">hello sample PowerShell commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="af5d5-110">Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre hello tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek Ha szükséges, és kövesse az utasításokat hello a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="af5d5-110">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>

## <a name="how-toocreate-hello-nsg-for-hello-front-end-subnet"></a><span data-ttu-id="af5d5-111">Hogyan toocreate hello hello előtér alhálózat NSG</span><span class="sxs-lookup"><span data-stu-id="af5d5-111">How toocreate hello NSG for hello front end subnet</span></span>
<span data-ttu-id="af5d5-112">az NSG nevű toocreate *NSG-előtérbeli* hello forgatókönyv alapján, végezze el a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="af5d5-112">toocreate an NSG named *NSG-FrontEnd* based on hello scenario, complete hello following steps:</span></span>

1. <span data-ttu-id="af5d5-113">Ha még sosem használta az Azure PowerShell, lásd: [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) és az útmutatás hello összes hello módon toohello toosign befejezése az Azure, és jelölje ki az előfizetését.</span><span class="sxs-lookup"><span data-stu-id="af5d5-113">If you have never used Azure PowerShell, see [How tooInstall and Configure Azure PowerShell](/powershell/azure/overview) and follow hello instructions all hello way toohello end toosign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="af5d5-114">Engedélyezi a hozzáférést a hello Internet tooport 3389-es biztonsági szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="af5d5-114">Create a security rule allowing access from hello Internet tooport 3389.</span></span>

    ```powershell
    $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
    ```

3. <span data-ttu-id="af5d5-115">Engedélyezi a hozzáférést a hello Internet tooport 80 biztonsági szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="af5d5-115">Create a security rule allowing access from hello Internet tooport 80.</span></span>

    ```powershell
    $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule -Description "Allow HTTP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
    -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 80
    ```

4. <span data-ttu-id="af5d5-116">Adjon hozzá új NSG nevű tooa a fenti létrehozott hello szabályokat **NSG-előtérbeli**.</span><span class="sxs-lookup"><span data-stu-id="af5d5-116">Add hello rules created above tooa new NSG named **NSG-FrontEnd**.</span></span>

    ```powershell
    $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG -Location westus `
    -Name "NSG-FrontEnd" -SecurityRules $rule1,$rule2
    ```

5. <span data-ttu-id="af5d5-117">Ellenőrizze a következő hello hello NSG létrehozott hello szabályok:</span><span class="sxs-lookup"><span data-stu-id="af5d5-117">Check hello rules created in hello NSG by typing hello following:</span></span>

    ```powershell
    $nsg
    ```
   
    <span data-ttu-id="af5d5-118">Kimeneti ábrázoló csak hello biztonsági szabályokat:</span><span class="sxs-lookup"><span data-stu-id="af5d5-118">Output showing just hello security rules:</span></span>
   
        SecurityRules        : [
                                 {
                                   "Name": "rdp-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
                                   "Description": "Allow RDP",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "3389",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 100,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 },
                                 {
                                   "Name": "web-rule",
                                   "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                   "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
                                   "Description": "Allow HTTP",
                                   "Protocol": "Tcp",
                                   "SourcePortRange": "*",
                                   "DestinationPortRange": "80",
                                   "SourceAddressPrefix": "Internet",
                                   "DestinationAddressPrefix": "*",
                                   "Access": "Allow",
                                   "Priority": 101,
                                   "Direction": "Inbound",
                                   "ProvisioningState": "Succeeded"
                                 }
                               ]
6. <span data-ttu-id="af5d5-119">A fenti toohello létrehozott NSG társítása hello *előtér* alhálózat.</span><span class="sxs-lookup"><span data-stu-id="af5d5-119">Associate hello NSG created above toohello *FrontEnd* subnet.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
    -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="af5d5-120">Kimeneti ábrázoló csak hello *előtér* alhálózati beállítások, értesítés hello értéke hello **hálózati biztonsági csoporthoz tartozik** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="af5d5-120">Output showing only hello *FrontEnd* subnet settings, notice hello value for hello **NetworkSecurityGroup** property:</span></span>
   
                    Subnets           : [
                                          {
                                            "Name": "FrontEnd",
                                            "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                                            "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                            "AddressPrefix": "192.168.1.0/24",
                                            "IpConfigurations": [
                                              {
                                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1"
                                              },
                                              {
                                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1"
                                              }
                                            ],
                                            "NetworkSecurityGroup": {
                                              "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                            },
                                            "RouteTable": null,
                                            "ProvisioningState": "Succeeded"
                                          }
   
   > [!WARNING]
   > <span data-ttu-id="af5d5-121">hello kimeneti hello parancs a fenti csak létezik PowerShell futtató hello számítógépen hello virtuális hálózati konfiguráció objektum hello tartalmának mutatja.</span><span class="sxs-lookup"><span data-stu-id="af5d5-121">hello output for hello command above shows hello content for hello virtual network configuration object, which only exists on hello computer where you are running PowerShell.</span></span> <span data-ttu-id="af5d5-122">Toorun hello kell `Set-AzureRmVirtualNetwork` parancsmag toosave ezen beállítások tooAzure.</span><span class="sxs-lookup"><span data-stu-id="af5d5-122">You need toorun hello `Set-AzureRmVirtualNetwork` cmdlet toosave these settings tooAzure.</span></span>
   > 
   > 
7. <span data-ttu-id="af5d5-123">Új virtuális hálózat beállításai tooAzure hello mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="af5d5-123">Save hello new VNet settings tooAzure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="af5d5-124">Kimeneti csak hello NSG részét megjelenítő:</span><span class="sxs-lookup"><span data-stu-id="af5d5-124">Output showing just hello NSG portion:</span></span>
   
        "NetworkSecurityGroup": {
          "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
        }

## <a name="how-toocreate-hello-nsg-for-hello-back-end-subnet"></a><span data-ttu-id="af5d5-125">Hogyan toocreate hello NSG hello háttér-alhálózat</span><span class="sxs-lookup"><span data-stu-id="af5d5-125">How toocreate hello NSG for hello back-end subnet</span></span>
<span data-ttu-id="af5d5-126">az NSG nevű toocreate *NSG-háttérrendszer* a fenti hello forgatókönyv alapján, végezze el a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="af5d5-126">toocreate an NSG named *NSG-BackEnd* based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="af5d5-127">Engedélyezi a hozzáférést a hello előtér alhálózati tooport alapértelmezett 1433-as (SQL Server által használt) biztonsági szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="af5d5-127">Create a security rule allowing access from hello front-end subnet tooport 1433 (default port used by SQL Server).</span></span>

    ```powershell
    $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name frontend-rule `
    -Description "Allow FE subnet" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
    -SourceAddressPrefix 192.168.1.0/24 -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 1433
    ```

2. <span data-ttu-id="af5d5-128">Blokkolja a hozzáférést toohello internetes biztonsági szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="af5d5-128">Create a security rule blocking access toohello Internet.</span></span>

    ```powershell
    $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule `
    -Description "Block Internet" `
    -Access Deny -Protocol * -Direction Outbound -Priority 200 `
    -SourceAddressPrefix * -SourcePortRange * `
    -DestinationAddressPrefix Internet -DestinationPortRange *
    ```

3. <span data-ttu-id="af5d5-129">Adjon hozzá új NSG nevű tooa a fenti létrehozott hello szabályokat **NSG-háttérrendszer**.</span><span class="sxs-lookup"><span data-stu-id="af5d5-129">Add hello rules created above tooa new NSG named **NSG-BackEnd**.</span></span>

    ```powershell
    $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG `
    -Location westus -Name "NSG-BackEnd" `
    -SecurityRules $rule1,$rule2
    ```

4. <span data-ttu-id="af5d5-130">A fenti toohello létrehozott NSG társítása hello *háttér* alhálózat.</span><span class="sxs-lookup"><span data-stu-id="af5d5-130">Associate hello NSG created above toohello *BackEnd* subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd ` 
    -AddressPrefix 192.168.2.0/24 -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="af5d5-131">Kimeneti ábrázoló csak hello *háttér* alhálózati beállítások, értesítés hello értéke hello **hálózati biztonsági csoporthoz tartozik** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="af5d5-131">Output showing only hello *BackEnd* subnet settings, notice hello value for hello **NetworkSecurityGroup** property:</span></span>
   
        Subnets           : [
                      {
                        "Name": "BackEnd",
                        "Etag": "W/\"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx\"",
                        "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                        "AddressPrefix": "192.168.2.0/24",
                        "IpConfigurations": [...],
                        "NetworkSecurityGroup": {
                          "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd"
                        },
                        "RouteTable": null,
                        "ProvisioningState": "Succeeded"
                      }
5. <span data-ttu-id="af5d5-132">Új virtuális hálózat beállításai tooAzure hello mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="af5d5-132">Save hello new VNet settings tooAzure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

## <a name="how-tooremove-an-nsg"></a><span data-ttu-id="af5d5-133">Hogyan tooremove az NSG</span><span class="sxs-lookup"><span data-stu-id="af5d5-133">How tooremove an NSG</span></span>
<span data-ttu-id="af5d5-134">egy meglévő NSG toodelete nevű *NSG-előtérbeli* ebben az esetben kövesse az alábbi hello. lépés:</span><span class="sxs-lookup"><span data-stu-id="af5d5-134">toodelete an existing NSG, called *NSG-Frontend* in this case, follow hello step below:</span></span>

<span data-ttu-id="af5d5-135">Futtassa a hello **Remove-AzureRmNetworkSecurityGroup** alább látható és lehet, hogy tooinclude hello erőforrás csoport hello NSG van.</span><span class="sxs-lookup"><span data-stu-id="af5d5-135">Run hello **Remove-AzureRmNetworkSecurityGroup** shown below and be sure tooinclude hello resource group hello NSG is in.</span></span>

```powershell
Remove-AzureRmNetworkSecurityGroup -Name "NSG-FrontEnd" -ResourceGroupName "TestRG"
```

