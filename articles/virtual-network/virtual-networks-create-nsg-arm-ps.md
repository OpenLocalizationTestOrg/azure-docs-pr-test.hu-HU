---
title: "Hozzon létre a hálózati biztonsági csoport – Azure PowerShell |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre és telepíthet a PowerShell használatával a hálózati biztonsági csoportok."
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
ms.openlocfilehash: 26fe67b43d63c6685d8ae7644dd7df6931a4d2a5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-network-security-groups-using-powershell"></a><span data-ttu-id="60ec6-103">Hozza létre a hálózati biztonsági csoportokat PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="60ec6-103">Create network security groups using PowerShell</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

<span data-ttu-id="60ec6-104">Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="60ec6-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="60ec6-105">A Microsoft azt javasolja, hogy az erőforrások létrehozásához használja a Resource Manager-alapú üzemi modellt.</span><span class="sxs-lookup"><span data-stu-id="60ec6-105">Microsoft recommends creating resources through the Resource Manager deployment model.</span></span> <span data-ttu-id="60ec6-106">A két modell közti különbségekkel kapcsolatos további információkért olvassa el [Az Azure üzemi modelljeinek megismerése](../azure-resource-manager/resource-manager-deployment-model.md) című cikket.</span><span class="sxs-lookup"><span data-stu-id="60ec6-106">To learn more about the differences between the two models, read the [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span> <span data-ttu-id="60ec6-107">Ez a cikk a Resource Manager-alapú üzemi modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="60ec6-107">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="60ec6-108">Emellett [NSG-k létrehozása a klasszikus üzembe helyezési modellel](virtual-networks-create-nsg-classic-ps.md).</span><span class="sxs-lookup"><span data-stu-id="60ec6-108">You can also [create NSGs in the classic deployment model](virtual-networks-create-nsg-classic-ps.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="60ec6-109">A minta az alábbi parancsok várt már létrehozott egy egyszerű környezetben PowerShell a fenti forgatókönyv alapján.</span><span class="sxs-lookup"><span data-stu-id="60ec6-109">The sample PowerShell commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="60ec6-110">Ha szeretné a parancsokat a jelen dokumentum megjelenített, először összeállítása a tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), kattintson a **az Azure telepítéséhez**, cserélje le az alapértelmezett paraméterértékek, ha szükséges, és kövesse az utasításokat a portálon.</span><span class="sxs-lookup"><span data-stu-id="60ec6-110">If you want to run the commands as they are displayed in this document, first build the test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/201-IaaS-WebFrontEnd-SQLBackEnd), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a><span data-ttu-id="60ec6-111">Az NSG az előtér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="60ec6-111">How to create the NSG for the front end subnet</span></span>
<span data-ttu-id="60ec6-112">Az NSG nevű létrehozásához *NSG-előtérbeli* forgatókönyv alapján, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="60ec6-112">To create an NSG named *NSG-FrontEnd* based on the scenario, complete the following steps:</span></span>

1. <span data-ttu-id="60ec6-113">Ha még nem használta az Azure PowerShellt, tekintse meg [How to Install and Configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása) című részt, majd kövesse az utasításokat egészen az utolsó lépésig az Azure-ba való bejelentkezéshez és az előfizetése kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="60ec6-113">If you have never used Azure PowerShell, see [How to Install and Configure Azure PowerShell](/powershell/azure/overview) and follow the instructions all the way to the end to sign into Azure and select your subscription.</span></span>
2. <span data-ttu-id="60ec6-114">Hozzon létre egy biztonsági szabályt, amely engedélyezi webtartalmak elérését az internetről 3389-es port.</span><span class="sxs-lookup"><span data-stu-id="60ec6-114">Create a security rule allowing access from the Internet to port 3389.</span></span>

    ```powershell
    $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name rdp-rule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
    ```

3. <span data-ttu-id="60ec6-115">Hozzáférés engedélyezése az internetről a 80-as port biztonsági szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="60ec6-115">Create a security rule allowing access from the Internet to port 80.</span></span>

    ```powershell
    $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule -Description "Allow HTTP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
    -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * `
    -DestinationPortRange 80
    ```

4. <span data-ttu-id="60ec6-116">A fent egy NSG nevű létrehozott szabályok felvétele **NSG-előtérbeli**.</span><span class="sxs-lookup"><span data-stu-id="60ec6-116">Add the rules created above to a new NSG named **NSG-FrontEnd**.</span></span>

    ```powershell
    $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG -Location westus `
    -Name "NSG-FrontEnd" -SecurityRules $rule1,$rule2
    ```

5. <span data-ttu-id="60ec6-117">Ellenőrizze az NSG létrehozott, írja be a következő szabályokat:</span><span class="sxs-lookup"><span data-stu-id="60ec6-117">Check the rules created in the NSG by typing the following:</span></span>

    ```powershell
    $nsg
    ```
   
    <span data-ttu-id="60ec6-118">Csak a biztonsági szabályok megjelenítő kimenete:</span><span class="sxs-lookup"><span data-stu-id="60ec6-118">Output showing just the security rules:</span></span>
   
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
6. <span data-ttu-id="60ec6-119">A létrehozott fenti NSG társítása a *előtér* alhálózat.</span><span class="sxs-lookup"><span data-stu-id="60ec6-119">Associate the NSG created above to the *FrontEnd* subnet.</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd `
    -AddressPrefix 192.168.1.0/24 -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="60ec6-120">Kimenet megjelenítése csak a *előtér* alhálózat-beállítások, figyelje meg az értéket a **hálózati biztonsági csoporthoz tartozik** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="60ec6-120">Output showing only the *FrontEnd* subnet settings, notice the value for the **NetworkSecurityGroup** property:</span></span>
   
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
   > <span data-ttu-id="60ec6-121">A fenti parancs kimenetét a virtuális hálózati konfiguráció objektumban, amelyet csak megtalálható a számítógépen, ahol a PowerShell futtatja tartalmának mutatja.</span><span class="sxs-lookup"><span data-stu-id="60ec6-121">The output for the command above shows the content for the virtual network configuration object, which only exists on the computer where you are running PowerShell.</span></span> <span data-ttu-id="60ec6-122">Futtatnia kell a `Set-AzureRmVirtualNetwork` parancsmagot, hogy ezek a beállítások mentése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="60ec6-122">You need to run the `Set-AzureRmVirtualNetwork` cmdlet to save these settings to Azure.</span></span>
   > 
   > 
7. <span data-ttu-id="60ec6-123">Mentse az új VNet beállításait az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="60ec6-123">Save the new VNet settings to Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="60ec6-124">A kimeneti megjelenítő csak az NSG részében:</span><span class="sxs-lookup"><span data-stu-id="60ec6-124">Output showing just the NSG portion:</span></span>
   
        "NetworkSecurityGroup": {
          "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
        }

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a><span data-ttu-id="60ec6-125">Az NSG-t, a háttér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="60ec6-125">How to create the NSG for the back-end subnet</span></span>
<span data-ttu-id="60ec6-126">Az NSG nevű létrehozásához *NSG-háttérrendszer* a fenti forgatókönyv alapján, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="60ec6-126">To create an NSG named *NSG-BackEnd* based on the scenario above, complete the following steps:</span></span>

1. <span data-ttu-id="60ec6-127">Hozzáférés az előtér-alhálózatból az 1433-as port (SQL Server által használt alapértelmezett portot) biztonsági szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="60ec6-127">Create a security rule allowing access from the front-end subnet to port 1433 (default port used by SQL Server).</span></span>

    ```powershell
    $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name frontend-rule `
    -Description "Allow FE subnet" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 `
    -SourceAddressPrefix 192.168.1.0/24 -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 1433
    ```

2. <span data-ttu-id="60ec6-128">Hozzon létre egy biztonsági szabály blokkolja-e az internethez való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="60ec6-128">Create a security rule blocking access to the Internet.</span></span>

    ```powershell
    $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name web-rule `
    -Description "Block Internet" `
    -Access Deny -Protocol * -Direction Outbound -Priority 200 `
    -SourceAddressPrefix * -SourcePortRange * `
    -DestinationAddressPrefix Internet -DestinationPortRange *
    ```

3. <span data-ttu-id="60ec6-129">A fent egy NSG nevű létrehozott szabályok felvétele **NSG-háttérrendszer**.</span><span class="sxs-lookup"><span data-stu-id="60ec6-129">Add the rules created above to a new NSG named **NSG-BackEnd**.</span></span>

    ```powershell
    $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName TestRG `
    -Location westus -Name "NSG-BackEnd" `
    -SecurityRules $rule1,$rule2
    ```

4. <span data-ttu-id="60ec6-130">A létrehozott fenti NSG társítása a *háttér* alhálózat.</span><span class="sxs-lookup"><span data-stu-id="60ec6-130">Associate the NSG created above to the *BackEnd* subnet.</span></span>

    ```powershell
    Set-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name BackEnd ` 
    -AddressPrefix 192.168.2.0/24 -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="60ec6-131">Kimenet megjelenítése csak a *háttér* alhálózat-beállítások, figyelje meg az értéket a **hálózati biztonsági csoporthoz tartozik** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="60ec6-131">Output showing only the *BackEnd* subnet settings, notice the value for the **NetworkSecurityGroup** property:</span></span>
   
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
5. <span data-ttu-id="60ec6-132">Mentse az új VNet beállításait az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="60ec6-132">Save the new VNet settings to Azure.</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

## <a name="how-to-remove-an-nsg"></a><span data-ttu-id="60ec6-133">Az NSG eltávolítása</span><span class="sxs-lookup"><span data-stu-id="60ec6-133">How to remove an NSG</span></span>
<span data-ttu-id="60ec6-134">Egy meglévő NSG, úgynevezett törlése *NSG-előtérbeli* ebben az esetben hajtsa végre az alábbi:</span><span class="sxs-lookup"><span data-stu-id="60ec6-134">To delete an existing NSG, called *NSG-Frontend* in this case, follow the step below:</span></span>

<span data-ttu-id="60ec6-135">Futtassa a **Remove-AzureRmNetworkSecurityGroup** alább látható, és ügyeljen arra, hogy az erőforráscsoport, a rendszer az NSG-t tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="60ec6-135">Run the **Remove-AzureRmNetworkSecurityGroup** shown below and be sure to include the resource group the NSG is in.</span></span>

```powershell
Remove-AzureRmNetworkSecurityGroup -Name "NSG-FrontEnd" -ResourceGroupName "TestRG"
```

