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
# <a name="manage-network-security-groups-using-powershell"></a><span data-ttu-id="4b076-103">PowerShell-lel hálózati biztonsági csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="4b076-103">Manage network security groups using PowerShell</span></span>

[!INCLUDE [virtual-network-manage-arm-selectors-include.md](../../includes/virtual-network-manage-nsg-arm-selectors-include.md)]

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="4b076-104">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="4b076-104">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="4b076-105">Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="4b076-105">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="4b076-106">Információk beolvasása</span><span class="sxs-lookup"><span data-stu-id="4b076-106">Retrieve Information</span></span>
<span data-ttu-id="4b076-107">Megtekintheti a meglévő NSG-ket, szabályok lekérdezni egy meglévő NSG-t, és megtudhatja, milyen erőforrásokat egy NSG társítva.</span><span class="sxs-lookup"><span data-stu-id="4b076-107">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="4b076-108">Meglévő NSG-k megtekintése</span><span class="sxs-lookup"><span data-stu-id="4b076-108">View existing NSGs</span></span>
<span data-ttu-id="4b076-109">tooview minden meglévő NSG-ket egy előfizetésben futtatása hello `Get-AzureRmNetworkSecurityGroup` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="4b076-109">tooview all existing NSGs in a subscription, run hello `Get-AzureRmNetworkSecurityGroup` cmdlet.</span></span>

<span data-ttu-id="4b076-110">A várt eredmény:</span><span class="sxs-lookup"><span data-stu-id="4b076-110">Expected result:</span></span>

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


<span data-ttu-id="4b076-111">tooview hello listája az NSG-k egy adott erőforráscsoportban, futtassa a hello `Get-AzureRmNetworkSecurityGroup` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="4b076-111">tooview hello list of NSGs in a specific resource group, run hello `Get-AzureRmNetworkSecurityGroup` cmdlet.</span></span>

<span data-ttu-id="4b076-112">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="4b076-112">Expected output:</span></span>

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

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="4b076-113">A szabályok egy NSG listázása</span><span class="sxs-lookup"><span data-stu-id="4b076-113">List all rules for an NSG</span></span>
<span data-ttu-id="4b076-114">az NSG nevű tooview hello szabályainak **NSG-előtérbeli**, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-114">tooview hello rules of an NSG named **NSG-FrontEnd**, enter hello following command:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd | Select SecurityRules -ExpandProperty SecurityRules
```

<span data-ttu-id="4b076-115">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="4b076-115">Expected output:</span></span>

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
> <span data-ttu-id="4b076-116">Is `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` toolist hello alapértelmezett szabályok hello **NSG-előtérbeli** NSG.</span><span class="sxs-lookup"><span data-stu-id="4b076-116">You can also use `Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name "NSG-FrontEnd" | Select DefaultSecurityRules -ExpandProperty DefaultSecurityRules` toolist hello default rules from hello **NSG-FrontEnd** NSG.</span></span>
> 

### <a name="view-nsgs-associations"></a><span data-ttu-id="4b076-117">Az NSG-társításának megtekintéséhez</span><span class="sxs-lookup"><span data-stu-id="4b076-117">View NSGs associations</span></span>
<span data-ttu-id="4b076-118">tooview milyen erőforrásokat hello **NSG-előtérbeli** NSG futtatási hello, rendeljen hozzá a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="4b076-118">tooview what resources hello **NSG-FrontEnd** NSG is associate with, run hello following command:</span></span>

```powershell
Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
```

<span data-ttu-id="4b076-119">Keresse meg hello **hálózati illesztők** és **alhálózatok** tulajdonságok alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="4b076-119">Look for hello **NetworkInterfaces** and **Subnets** properties as shown below:</span></span>

    NetworkInterfaces    : []
    Subnets              : [
                             {
                               "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                               "IpConfigurations": []
                             }
                           ]

<span data-ttu-id="4b076-120">Hello előző példában hello NSG nincs társított tooany hálózati adapterek (NIC); nevű társított tooa alhálózat **előtér**.</span><span class="sxs-lookup"><span data-stu-id="4b076-120">In hello previous example, hello NSG is not associated tooany network interfaces (NICs); it is associated tooa subnet named **FrontEnd**.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="4b076-121">Szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="4b076-121">Manage rules</span></span>
<span data-ttu-id="4b076-122">Adja hozzá a meglévő NSG szabályok tooan, szerkesztheti a meglévő szabályokat, és törölje a szabályokat.</span><span class="sxs-lookup"><span data-stu-id="4b076-122">You can add rules tooan existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="4b076-123">Szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="4b076-123">Add a rule</span></span>
<span data-ttu-id="4b076-124">egy szabály, amely lehetővé teszi tooadd **bejövő** forgalom tooport **443-as** bármely gépen toohello a **NSG-előtér** NSG-t, a következő lépéseket teljes hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-124">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, complete hello following steps:</span></span>

1. <span data-ttu-id="4b076-125">Futtassa a következő parancs tooretrieve hello NSG meglévő hello és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-125">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell   
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="4b076-126">Futtassa a következő parancs tooadd hello egy szabály toohello NSG:</span><span class="sxs-lookup"><span data-stu-id="4b076-126">Run hello following command tooadd a rule toohello NSG:</span></span>

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

3. <span data-ttu-id="4b076-127">toosave hello módosítások toohello NSG, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-127">toosave hello changes made toohello NSG, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```
    <span data-ttu-id="4b076-128">Várt kimenet ábrázoló csak hello vonatkozó biztonsági szabályokat:</span><span class="sxs-lookup"><span data-stu-id="4b076-128">Expected output showing only hello security rules:</span></span>
   
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

### <a name="change-a-rule"></a><span data-ttu-id="4b076-129">Szabály módosítása</span><span class="sxs-lookup"><span data-stu-id="4b076-129">Change a rule</span></span>
<span data-ttu-id="4b076-130">a fenti tooallow létrehozott toochange hello szabály hello érkező bejövő adatforgalmat **Internet** csak, kövesse az alábbi hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4b076-130">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, follow hello steps below.</span></span>

1. <span data-ttu-id="4b076-131">Futtassa a következő parancs tooretrieve hello NSG meglévő hello és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-131">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell 
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="4b076-132">Futtassa a következő beállításokkal hello új szabály parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-132">Run hello following command with hello new rule settings:</span></span>

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

3. <span data-ttu-id="4b076-133">toosave hello módosítások toohello NSG, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-133">toosave hello changes made toohello NSG, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="4b076-134">Várt kimenet ábrázoló csak hello vonatkozó biztonsági szabályokat:</span><span class="sxs-lookup"><span data-stu-id="4b076-134">Expected output showing only hello security rules:</span></span>
   
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

### <a name="delete-a-rule"></a><span data-ttu-id="4b076-135">Szabály törlése</span><span class="sxs-lookup"><span data-stu-id="4b076-135">Delete a rule</span></span>
1. <span data-ttu-id="4b076-136">Futtassa a következő parancs tooretrieve hello NSG meglévő hello és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-136">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="4b076-137">Futtassa a parancsot tooremove hello szabály követően – hello NSG hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-137">Run hello following command tooremove hello rule from hello NSG:</span></span>

    ```powershell
    Remove-AzureRmNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name https-rule
    ```

3. <span data-ttu-id="4b076-138">Hello végrehajtott módosítások toohello NSG, mentés hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4b076-138">Save hello changes made toohello NSG, by running hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
    ```

    <span data-ttu-id="4b076-139">Várt kimenet megjelenítése csak hello vonatkozó biztonsági szabályokat, értesítés hello **https-szabály** nem jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="4b076-139">Expected output showing only hello security rules, notice hello **https-rule** is no longer listed:</span></span>
   
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

## <a name="manage-associations"></a><span data-ttu-id="4b076-140">Társítások kezelése</span><span class="sxs-lookup"><span data-stu-id="4b076-140">Manage associations</span></span>
<span data-ttu-id="4b076-141">Az NSG toosubnets és a hálózati adapterek is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="4b076-141">You can associate an NSG toosubnets and NICs.</span></span> <span data-ttu-id="4b076-142">Is is leválasztja az összes erőforrásból, amelyekhez társítva vannak a NSG.</span><span class="sxs-lookup"><span data-stu-id="4b076-142">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="4b076-143">Társítson egy NSG tooa hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="4b076-143">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="4b076-144">tooassociate hello **NSG-előtérbeli** NSG toohello **TestNICWeb1** a hálózati adapter teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4b076-144">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="4b076-145">Futtassa a következő parancs tooretrieve hello NSG meglévő hello és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-145">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

2. <span data-ttu-id="4b076-146">Futtassa a következő parancs tooretrieve hello meglévő hálózati hello és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-146">Run hello following command tooretrieve hello existing NIC and store it in a variable:</span></span>

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

3. <span data-ttu-id="4b076-147">Set hello **hálózati biztonsági csoporthoz tartozik** hello tulajdonságának **NIC** hello változó toohello értékének **NSG** változó hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="4b076-147">Set hello **NetworkSecurityGroup** property of hello **NIC** variable toohello value of hello **NSG** variable, by entering hello following command:</span></span>

    ```powershell
    $nic.NetworkSecurityGroup = $nsg
    ```

4. <span data-ttu-id="4b076-148">toosave hello módosítások toohello hálózati adapter, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-148">toosave hello changes made toohello NIC, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    <span data-ttu-id="4b076-149">Várt kimenet ábrázoló csak hello **hálózati biztonsági csoporthoz tartozik** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="4b076-149">Expected output showing only hello **NetworkSecurityGroup** property:</span></span>
   
        NetworkSecurityGroup : {
                                 "SecurityRules": [],
                                 "DefaultSecurityRules": [],
                                 "NetworkInterfaces": [],
                                 "Subnets": [],
                                 "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                               }

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="4b076-150">A társítást egy NSG-t a hálózati Adapterhez</span><span class="sxs-lookup"><span data-stu-id="4b076-150">Dissociate an NSG from a NIC</span></span>
<span data-ttu-id="4b076-151">toodissociate hello **NSG-előtérbeli** hello az NSG **TestNICWeb1** a hálózati adapter teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4b076-151">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, complete hello following steps:</span></span>

1. <span data-ttu-id="4b076-152">Futtassa a következő parancs tooretrieve hello meglévő hálózati hello és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-152">Run hello following command tooretrieve hello existing NIC and store it in a variable:</span></span>

    ```powershell
    $nic = Get-AzureRmNetworkInterface -ResourceGroupName RG-NSG -Name TestNICWeb1
    ```

2. <span data-ttu-id="4b076-153">Set hello **hálózati biztonsági csoporthoz tartozik** hello tulajdonságának **NIC** változó túl**$null** hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4b076-153">Set hello **NetworkSecurityGroup** property of hello **NIC** variable too**$null** by running hello following command:</span></span>

    ```powershell
    $nic.NetworkSecurityGroup = $null
    ```

3. <span data-ttu-id="4b076-154">toosave hello módosítások toohello hálózati adapter, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-154">toosave hello changes made toohello NIC, run hello following command:</span></span>

    ```powershell
    Set-AzureRmNetworkInterface -NetworkInterface $nic
    ```
   
    <span data-ttu-id="4b076-155">Várt kimenet ábrázoló csak hello **hálózati biztonsági csoporthoz tartozik** tulajdonság:</span><span class="sxs-lookup"><span data-stu-id="4b076-155">Expected output showing only hello **NetworkSecurityGroup** property:</span></span>
   
        NetworkSecurityGroup : null

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="4b076-156">Az NSG alhálózatból származó leválasztani</span><span class="sxs-lookup"><span data-stu-id="4b076-156">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="4b076-157">toodissociate hello **NSG-előtérbeli** hello az NSG **előtér** alhálózat, a teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4b076-157">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, complete hello following steps:</span></span>

1. <span data-ttu-id="4b076-158">Futtassa a következő parancs tooretrieve hello meglévő VNet hello és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-158">Run hello following command tooretrieve hello existing VNet and store it in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. <span data-ttu-id="4b076-159">Futtatási hello parancs tooretrieve hello következő **előtér** alhálózati és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-159">Run hello following command tooretrieve hello **FrontEnd** subnet and store it in a variable:</span></span>

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. <span data-ttu-id="4b076-160">Set hello **hálózati biztonsági csoporthoz tartozik** hello tulajdonságának **alhálózati** változó túl**$null** hello a következő parancs beírásával:</span><span class="sxs-lookup"><span data-stu-id="4b076-160">Set hello **NetworkSecurityGroup** property of hello **subnet** variable too**$null** by entering hello following command:</span></span>

    ```powershell
    $subnet.NetworkSecurityGroup = $null
    ```

4. <span data-ttu-id="4b076-161">toosave hello módosítások toohello alhálózati, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-161">toosave hello changes made toohello subnet, run hello following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="4b076-162">Várt kimenet hello csak hello tulajdonságait megjelenítő **előtér** alhálózat.</span><span class="sxs-lookup"><span data-stu-id="4b076-162">Expected output showing only hello properties of hello **FrontEnd** subnet.</span></span> <span data-ttu-id="4b076-163">A tulajdonság nem létezik figyelmeztetés **hálózati biztonsági csoporthoz tartozik**:</span><span class="sxs-lookup"><span data-stu-id="4b076-163">Notice there isn't a property for **NetworkSecurityGroup**:</span></span>
   
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

### <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="4b076-164">Társítsa az NSG-tooa alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="4b076-164">Associate an NSG tooa subnet</span></span>
<span data-ttu-id="4b076-165">tooassociate hello **NSG-előtérbeli** NSG toohello **FronEnd** alhálózati újra, a teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="4b076-165">tooassociate hello **NSG-FrontEnd** NSG toohello **FronEnd** subnet again, complete hello following steps:</span></span>

1. <span data-ttu-id="4b076-166">Futtassa a következő parancs tooretrieve hello meglévő VNet hello és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-166">Run hello following command tooretrieve hello existing VNet and store it in a variable:</span></span>

    ```powershell
    $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName RG-NSG -Name TestVNet
    ```

2. <span data-ttu-id="4b076-167">Futtatási hello parancs tooretrieve hello következő **előtér** alhálózati és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-167">Run hello following command tooretrieve hello **FrontEnd** subnet and store it in a variable:</span></span>

    ```powershell
    $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name FrontEnd
    ```
 
3. <span data-ttu-id="4b076-168">Futtassa a következő parancs tooretrieve hello NSG meglévő hello és tárolható egy változóban:</span><span class="sxs-lookup"><span data-stu-id="4b076-168">Run hello following command tooretrieve hello existing NSG and store it in a variable:</span></span>

    ```powershell
    $nsg = Get-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd
    ```

4. <span data-ttu-id="4b076-169">Set hello **hálózati biztonsági csoporthoz tartozik** hello tulajdonságának **alhálózati** változó túl**$null** hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="4b076-169">Set hello **NetworkSecurityGroup** property of hello **subnet** variable too**$null** by running hello following command:</span></span>

    ```powershell
    $subnet.NetworkSecurityGroup = $nsg
    ```

5. <span data-ttu-id="4b076-170">toosave hello módosítások toohello alhálózati, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-170">toosave hello changes made toohello subnet, run hello following command:</span></span>

    ```powershell
    Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
    ```

    <span data-ttu-id="4b076-171">Várt kimenet ábrázoló csak hello **hálózati biztonsági csoporthoz tartozik** hello tulajdonságának **előtér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="4b076-171">Expected output showing only hello **NetworkSecurityGroup** property of hello **FrontEnd** subnet:</span></span>
   
        ...
        "NetworkSecurityGroup": {
                                  "SecurityRules": [],
                                  "DefaultSecurityRules": [],
                                  "NetworkInterfaces": [],
                                  "Subnets": [],
                                  "Id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd"
                                }
        ...

## <a name="delete-an-nsg"></a><span data-ttu-id="4b076-172">Az NSG törlése</span><span class="sxs-lookup"><span data-stu-id="4b076-172">Delete an NSG</span></span>
<span data-ttu-id="4b076-173">Ha nem kapcsolódnak hozzá erőforrás tooany csak törlése egy NSG.</span><span class="sxs-lookup"><span data-stu-id="4b076-173">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="4b076-174">az NSG toodelete kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4b076-174">toodelete an NSG, follow hello steps below.</span></span>

1. <span data-ttu-id="4b076-175">toocheck hello erőforrásokhoz rendelt tooan NSG, futtassa a hello `azure network nsg show` látható módon [nézet NSG-ket társítások](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="4b076-175">toocheck hello resources associated tooan NSG, run hello `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="4b076-176">Ha hello NSG társított tooany hálózati adapterek, futtassa a hello `azure network nic set` látható módon [leválasztani a hálózati Adapterhez egy NSG](#Dissociate-an-NSG-from-a-NIC) az egyes hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="4b076-176">If hello NSG is associated tooany NICs, run hello `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="4b076-177">Ha hello NSG társított tooany alhálózati, futtassa a hello `azure network vnet subnet set` látható módon [leválasztani az NSG alhálózatból származó](#Dissociate-an-NSG-from-a-subnet) az egyes alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="4b076-177">If hello NSG is associated tooany subnet, run hello `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="4b076-178">toodelete hello NSG, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="4b076-178">toodelete hello NSG, run hello following command:</span></span>

    ```powershell
    Remove-AzureRmNetworkSecurityGroup -ResourceGroupName RG-NSG -Name NSG-FrontEnd -Force
    ```
   
   > [!NOTE]
   > <span data-ttu-id="4b076-179">Hello `-Force` paraméter biztosítja, nem kell tooconfirm hello törlését.</span><span class="sxs-lookup"><span data-stu-id="4b076-179">hello `-Force` parameter ensures you don't need tooconfirm hello deletion.</span></span>
   > 

## <a name="next-steps"></a><span data-ttu-id="4b076-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b076-180">Next steps</span></span>
* <span data-ttu-id="4b076-181">[Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="4b076-181">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

