---
title: "hálózati biztonsági csoport – Azure CLI 1.0 aaaManage |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage hálózati biztonsági csoportok használatával hello Azure parancssori felület (CLI) 1.0-s."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/21/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9a429f947abbcb5fa6adb40c84504f68efd5e20e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-network-security-groups-using-hello-azure-cli-10"></a><span data-ttu-id="2ff70-103">Hello Azure CLI 1.0 használata a hálózati biztonsági csoportok kezelése</span><span class="sxs-lookup"><span data-stu-id="2ff70-103">Manage network security groups using hello Azure CLI 1.0</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="2ff70-104">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="2ff70-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="2ff70-105">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="2ff70-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="2ff70-106">[Az Azure CLI 1.0](#View-existing-NSGs) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="2ff70-106">[Azure CLI 1.0](#View-existing-NSGs) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="2ff70-107">[Az Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk)</span><span class="sxs-lookup"><span data-stu-id="2ff70-107">[Azure CLI 2.0](virtual-network-manage-nsg-arm-cli.md) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="2ff70-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="2ff70-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="2ff70-109">Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="2ff70-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello classic deployment model.</span></span>
> 

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="retrieve-information"></a><span data-ttu-id="2ff70-110">Információk beolvasása</span><span class="sxs-lookup"><span data-stu-id="2ff70-110">Retrieve Information</span></span>
<span data-ttu-id="2ff70-111">Megtekintheti a meglévő NSG-ket, szabályok lekérdezni egy meglévő NSG-t, és megtudhatja, milyen erőforrásokat egy NSG társítva.</span><span class="sxs-lookup"><span data-stu-id="2ff70-111">You can view your existing NSGs, retrieve rules for an existing NSG, and find out what resources an NSG is associated to.</span></span>

### <a name="view-existing-nsgs"></a><span data-ttu-id="2ff70-112">Meglévő NSG-k megtekintése</span><span class="sxs-lookup"><span data-stu-id="2ff70-112">View existing NSGs</span></span>
<span data-ttu-id="2ff70-113">tooview hello listája az NSG-k egy adott erőforráscsoportban, futtassa a hello `azure network nsg list` parancsot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="2ff70-113">tooview hello list of NSGs in a specific resource group, run hello `azure network nsg list` command as shown below.</span></span>

```azurecli
azure network nsg list --resource-group RG-NSG
```

<span data-ttu-id="2ff70-114">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2ff70-114">Expected output:</span></span>

    info:    Executing command network nsg list
    + Getting hello network security groups
    data:    Name          Location
    data:    ------------  --------
    data:    NSG-BackEnd   westus
    data:    NSG-FrontEnd  westus
    info:    network nsg list command OK

### <a name="list-all-rules-for-an-nsg"></a><span data-ttu-id="2ff70-115">A szabályok egy NSG listázása</span><span class="sxs-lookup"><span data-stu-id="2ff70-115">List all rules for an NSG</span></span>
<span data-ttu-id="2ff70-116">az NSG nevű tooview hello szabályainak **NSG-előtér**- ben futtassa hello `azure network nsg show` parancsot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="2ff70-116">tooview hello rules of an NSG named **NSG-FrontEnd**, run hello `azure network nsg show` command as shown below.</span></span> 

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd
```

<span data-ttu-id="2ff70-117">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2ff70-117">Expected output:</span></span>

    info:    Executing command network nsg show
    + Looking up hello network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Name                            : NSG-FrontEnd
    data:    Type                            : Microsoft.Network/networkSecurityGroups
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    Tags                            : displayName=NSG - Front End
    data:    Security group rules:
    data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
    data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
    data:    rdp-rule                       Internet           *            *               3389              Tcp       Inbound    Allow   100
    data:    web-rule                       Internet           *            *               80                Tcp       Inbound    Allow   101
    data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000
    data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001
    data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500
    data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000
    data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001
    data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500
    info:    network nsg show command OK

> [!NOTE]
> <span data-ttu-id="2ff70-118">Is `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` toolist hello szabályok hello **NSG-előtérbeli** NSG.</span><span class="sxs-lookup"><span data-stu-id="2ff70-118">You can also use `azure network nsg rule list --resource-group RG-NSG --nsg-name NSG-FrontEnd` toolist hello rules from hello **NSG-FrontEnd** NSG.</span></span>
>

### <a name="view-nsg-associations"></a><span data-ttu-id="2ff70-119">NSG-társítások megtekintése</span><span class="sxs-lookup"><span data-stu-id="2ff70-119">View NSG associations</span></span>

<span data-ttu-id="2ff70-120">tooview milyen erőforrásokat hello **NSG-előtérbeli** NSG társítása a, futtatási hello `azure network nsg show` parancsot a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="2ff70-120">tooview what resources hello **NSG-FrontEnd** NSG is associate with, run hello `azure network nsg show` command as shown below.</span></span> <span data-ttu-id="2ff70-121">Figyelje meg, hogy hello egyetlen különbség az hello hello használata **--json** paraméter.</span><span class="sxs-lookup"><span data-stu-id="2ff70-121">Notice that hello only difference is hello use of hello **--json** parameter.</span></span>

```azurecli
azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json
```

<span data-ttu-id="2ff70-122">Keresse meg hello **hálózati illesztők** és **alhálózatok** tulajdonságok alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="2ff70-122">Look for hello **networkInterfaces** and **subnets** properties as shown below:</span></span>

    "networkInterfaces": [],
    ...
    "subnets": [
        {
            "id": "/subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd"
        }
    ],
    ...

<span data-ttu-id="2ff70-123">Hello a fenti példában az NSG nincs hello tooany hálózati adapterek (NIC) társított, és a kapcsolódó tooa alhálózati nevű **előtér**.</span><span class="sxs-lookup"><span data-stu-id="2ff70-123">In hello example above, hello NSG is not associated tooany network interfaces (NICs), and it is associated tooa subnet named **FrontEnd**.</span></span>

## <a name="manage-rules"></a><span data-ttu-id="2ff70-124">Szabályok kezelése</span><span class="sxs-lookup"><span data-stu-id="2ff70-124">Manage rules</span></span>
<span data-ttu-id="2ff70-125">Adja hozzá a meglévő NSG szabályok tooan, szerkesztheti a meglévő szabályokat, és törölje a szabályokat.</span><span class="sxs-lookup"><span data-stu-id="2ff70-125">You can add rules tooan existing NSG, edit existing rules, and remove rules.</span></span>

### <a name="add-a-rule"></a><span data-ttu-id="2ff70-126">Szabály hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2ff70-126">Add a rule</span></span>
<span data-ttu-id="2ff70-127">egy szabály, amely lehetővé teszi tooadd **bejövő** forgalom tooport **443-as** bármely gépen toohello a **NSG-előtér** NSG-t, írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="2ff70-127">tooadd a rule allowing **inbound** traffic tooport **443** from any machine toohello **NSG-FrontEnd** NSG, enter hello following command:</span></span>

```azurecli
azure network nsg rule create --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --description "Allow access tooport 443 for HTTPS" \
    --protocol Tcp \
    --source-address-prefix * \
    --source-port-range * \
    --destination-address-prefix * \
    --destination-port-range 443 \
    --access Allow \
    --priority 102 \
    --direction Inbound
```

<span data-ttu-id="2ff70-128">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2ff70-128">Expected output:</span></span>

    info:    Executing command network nsg rule create
    + Looking up hello network security rule "allow-https"
    + Creating a network security rule "allow-https"
    + Looking up hello network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access tooport 443 for HTTPS
    data:    Source IP                       : *
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule create command OK

### <a name="change-a-rule"></a><span data-ttu-id="2ff70-129">Szabály módosítása</span><span class="sxs-lookup"><span data-stu-id="2ff70-129">Change a rule</span></span>
<span data-ttu-id="2ff70-130">toochange hello szabály tooallow a fenti létrehozott hello érkező bejövő adatforgalmat **Internet** csak, futtatási hello következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2ff70-130">toochange hello rule created above tooallow inbound traffic from hello **Internet** only, run hello following command:</span></span>

```azurecli
azure network nsg rule set --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --source-address-prefix Internet
```

<span data-ttu-id="2ff70-131">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2ff70-131">Expected output:</span></span>

    info:    Executing command network nsg rule set
    + Looking up hello network security group "NSG-FrontEnd"
    + Setting a network security rule "allow-https"
    + Looking up hello network security group "NSG-FrontEnd"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/allow-https
    data:    Name                            : allow-https
    data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
    data:    Provisioning state              : Succeeded
    data:    Description                     : Allow access tooport 443 for HTTPS
    data:    Source IP                       : Internet
    data:    Source Port                     : *
    data:    Destination IP                  : *
    data:    Destination Port                : 443
    data:    Protocol                        : Tcp
    data:    Direction                       : Inbound
    data:    Access                          : Allow
    data:    Priority                        : 102
    info:    network nsg rule set command OK

### <a name="delete-a-rule"></a><span data-ttu-id="2ff70-132">Szabály törlése</span><span class="sxs-lookup"><span data-stu-id="2ff70-132">Delete a rule</span></span>
<span data-ttu-id="2ff70-133">a fenti létrehozott toodelete hello szabály futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="2ff70-133">toodelete hello rule created above, run hello following command:</span></span>

```azurecli
azure network nsg rule delete --resource-group RG-NSG \
    --nsg-name NSG-FrontEnd \
    --name allow-https \
    --quiet
```

> [!NOTE]
> <span data-ttu-id="2ff70-134">Hello `--quiet` paraméter biztosítja, nem kell tooconfirm hello törlését.</span><span class="sxs-lookup"><span data-stu-id="2ff70-134">hello `--quiet` parameter ensures you don't need tooconfirm hello deletion.</span></span>
>

<span data-ttu-id="2ff70-135">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2ff70-135">Expected output:</span></span>

    info:    Executing command network nsg rule delete
    + Looking up hello network security group "NSG-FrontEnd"
    + Deleting network security rule "allow-https"
    info:    network nsg rule delete command OK

## <a name="manage-associations"></a><span data-ttu-id="2ff70-136">Társítások kezelése</span><span class="sxs-lookup"><span data-stu-id="2ff70-136">Manage associations</span></span>
<span data-ttu-id="2ff70-137">Az NSG toosubnets és a hálózati adapterek is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="2ff70-137">You can associate an NSG toosubnets and NICs.</span></span> <span data-ttu-id="2ff70-138">Is is leválasztja az összes erőforrásból, amelyekhez társítva vannak a NSG.</span><span class="sxs-lookup"><span data-stu-id="2ff70-138">You can also dissociate an NSG from any resource it's associated to.</span></span>

### <a name="associate-an-nsg-tooa-nic"></a><span data-ttu-id="2ff70-139">Társítson egy NSG tooa hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="2ff70-139">Associate an NSG tooa NIC</span></span>
<span data-ttu-id="2ff70-140">tooassociate hello **NSG-előtérbeli** NSG toohello **TestNICWeb1** hálózati adapter, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="2ff70-140">tooassociate hello **NSG-FrontEnd** NSG toohello **TestNICWeb1** NIC, run hello following command:</span></span>

```azurecli
azure network nic set --resource-group RG-NSG \
    --name TestNICWeb1 \
    --network-security-group-name NSG-FrontEnd
```

<span data-ttu-id="2ff70-141">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2ff70-141">Expected output:</span></span>

    info:    Executing command network nic set
    + Looking up hello network interface "TestNICWeb1"
    + Looking up hello network security group "NSG-FrontEnd"
    + Updating network interface "TestNICWeb1"
    + Looking up hello network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Network security group          : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
    data:    Virtual machine                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-nic"></a><span data-ttu-id="2ff70-142">A társítást egy NSG-t a hálózati Adapterhez</span><span class="sxs-lookup"><span data-stu-id="2ff70-142">Dissociate an NSG from a NIC</span></span>

<span data-ttu-id="2ff70-143">toodissociate hello **NSG-előtérbeli** hello az NSG **TestNICWeb1** hálózati adapter, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="2ff70-143">toodissociate hello **NSG-FrontEnd** NSG from hello **TestNICWeb1** NIC, run hello following command:</span></span>

```azurecli
azure network nic set --resource-group RG-NSG --name TestNICWeb1 --network-security-group-id ""
```

> [!NOTE]
> <span data-ttu-id="2ff70-144">Értesítés hello "" hello (üres) értéket `network-security-group-id` paraméter.</span><span class="sxs-lookup"><span data-stu-id="2ff70-144">Notice hello "" (empty) value for hello `network-security-group-id` parameter.</span></span> <span data-ttu-id="2ff70-145">Ez hogyan távolítsa el az egy társítás tooan NSG.</span><span class="sxs-lookup"><span data-stu-id="2ff70-145">That is how you remove an association tooan NSG.</span></span> <span data-ttu-id="2ff70-146">Nem azonos a hello hello `network-security-group-name` paraméter.</span><span class="sxs-lookup"><span data-stu-id="2ff70-146">You can't do hello same with hello `network-security-group-name` parameter.</span></span>
> 

<span data-ttu-id="2ff70-147">A várt eredmény:</span><span class="sxs-lookup"><span data-stu-id="2ff70-147">Expected result:</span></span>

    info:    Executing command network nic set
    + Looking up hello network interface "TestNICWeb1"
    + Updating network interface "TestNICWeb1"
    + Looking up hello network interface "TestNICWeb1"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1
    data:    Name                            : TestNICWeb1
    data:    Type                            : Microsoft.Network/networkInterfaces
    data:    Location                        : westus
    data:    Provisioning state              : Succeeded
    data:    MAC address                     : 00-0D-3A-30-A1-F8
    data:    Enable IP forwarding            : false
    data:    Tags                            : displayName=NetworkInterfaces - Web
    data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Compute/virtualMachines/Web1
    data:    IP configurations:
    data:      Name                          : ipconfig1
    data:      Provisioning state            : Succeeded
    data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/publicIPAddresses/TestPIPWeb1
    data:      Private IP address            : 192.168.1.5
    data:      Private IP Allocation Method  : Dynamic
    data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:
    info:    network nic set command OK

### <a name="dissociate-an-nsg-from-a-subnet"></a><span data-ttu-id="2ff70-148">Az NSG alhálózatból származó leválasztani</span><span class="sxs-lookup"><span data-stu-id="2ff70-148">Dissociate an NSG from a subnet</span></span>
<span data-ttu-id="2ff70-149">toodissociate hello **NSG-előtérbeli** hello az NSG **előtér** alhálózati, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="2ff70-149">toodissociate hello **NSG-FrontEnd** NSG from hello **FrontEnd** subnet, run hello following command:</span></span>

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-id ""
```

<span data-ttu-id="2ff70-150">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2ff70-150">Expected output:</span></span>

    info:    Executing command network vnet subnet set
    + Looking up hello subnet "FrontEnd"
    + Setting subnet "FrontEnd"
    + Looking up hello subnet "FrontEnd"
    data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
    data:    Type                            : Microsoft.Network/virtualNetworks/subnets
    data:    ProvisioningState               : Succeeded
    data:    Name                            : FrontEnd
    data:    Address prefix                  : 192.168.1.0/24
    data:    IP configurations:
    data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
    data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
    data:
    info:    network vnet subnet set command OK

### <a name="associate-an-nsg-tooa-subnet"></a><span data-ttu-id="2ff70-151">Társítsa az NSG-tooa alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="2ff70-151">Associate an NSG tooa subnet</span></span>
<span data-ttu-id="2ff70-152">tooassociate hello **NSG-előtérbeli** NSG toohello **FronEnd** alhálózati ismét, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="2ff70-152">tooassociate hello **NSG-FrontEnd** NSG toohello **FronEnd** subnet again, run hello following command:</span></span>

```azurecli
azure network vnet subnet set --resource-group RG-NSG \
    --vnet-name TestVNet \
    --name FrontEnd \
    --network-security-group-name NSG-FronEnd
```

> [!NOTE]
> <span data-ttu-id="2ff70-153">hello a fenti parancs csak akkor működik, mert hello **NSG-előtérbeli** NSG hello szerepel ugyanabban az erőforráscsoportban hello virtuális hálózatként **TestVNet**.</span><span class="sxs-lookup"><span data-stu-id="2ff70-153">hello command above only works because hello **NSG-FrontEnd** NSG is in hello same resource group as hello virtual network **TestVNet**.</span></span> <span data-ttu-id="2ff70-154">Ha hello NSG-t egy másik erőforráscsoportban található, meg kell-e toouse hello `--network-security-group-id` paraméter Ehelyett, és adjon meg teljes azonosító hello hello NSG.</span><span class="sxs-lookup"><span data-stu-id="2ff70-154">If hello NSG is in a different resource group, you need toouse hello `--network-security-group-id` parameter instead, and provide hello full id for hello NSG.</span></span> <span data-ttu-id="2ff70-155">Hello azonosító futtassa le `azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json` és hello keres **azonosító** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="2ff70-155">You can retrieve hello id by running `azure network nsg show --resource-group RG-NSG --name NSG-FrontEnd --json` and looking for hello **id** property.</span></span> 
> 

<span data-ttu-id="2ff70-156">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2ff70-156">Expected output:</span></span>

        info:    Executing command network vnet subnet set
        + Looking up hello subnet "FrontEnd"
        + Looking up hello network security group "NSG-FrontEnd"
        + Setting subnet "FrontEnd"
        + Looking up hello subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ipConfigurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/RG-NSG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ipConfigurations/ipconfig1
        data:
        info:    network vnet subnet set command OK

## <a name="delete-an-nsg"></a><span data-ttu-id="2ff70-157">Az NSG törlése</span><span class="sxs-lookup"><span data-stu-id="2ff70-157">Delete an NSG</span></span>
<span data-ttu-id="2ff70-158">Ha nem kapcsolódnak hozzá erőforrás tooany csak törlése egy NSG.</span><span class="sxs-lookup"><span data-stu-id="2ff70-158">You can only delete an NSG if it's not associated tooany resource.</span></span> <span data-ttu-id="2ff70-159">az NSG toodelete kövesse hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2ff70-159">toodelete an NSG, follow hello steps below.</span></span>

1. <span data-ttu-id="2ff70-160">toocheck hello erőforrásokhoz rendelt tooan NSG, futtassa a hello `azure network nsg show` látható módon [nézet NSG-ket társítások](#View-NSGs-associations).</span><span class="sxs-lookup"><span data-stu-id="2ff70-160">toocheck hello resources associated tooan NSG, run hello `azure network nsg show` as shown in [View NSGs associations](#View-NSGs-associations).</span></span>
2. <span data-ttu-id="2ff70-161">Ha hello NSG társított tooany hálózati adapterek, futtassa a hello `azure network nic set` látható módon [leválasztani a hálózati Adapterhez egy NSG](#Dissociate-an-NSG-from-a-NIC) az egyes hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="2ff70-161">If hello NSG is associated tooany NICs, run hello `azure network nic set` as shown in [Dissociate an NSG from a NIC](#Dissociate-an-NSG-from-a-NIC) for each NIC.</span></span> 
3. <span data-ttu-id="2ff70-162">Ha hello NSG társított tooany alhálózati, futtassa a hello `azure network vnet subnet set` látható módon [leválasztani az NSG alhálózatból származó](#Dissociate-an-NSG-from-a-subnet) az egyes alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="2ff70-162">If hello NSG is associated tooany subnet, run hello `azure network vnet subnet set` as shown in [Dissociate an NSG from a subnet](#Dissociate-an-NSG-from-a-subnet) for each subnet.</span></span>
4. <span data-ttu-id="2ff70-163">toodelete hello NSG, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="2ff70-163">toodelete hello NSG, run hello following command:</span></span>

    ```azurecli
    azure network nsg delete --resource-group RG-NSG --name NSG-FrontEnd --quiet
    ```

    <span data-ttu-id="2ff70-164">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="2ff70-164">Expected output:</span></span>

        info:    Executing command network nsg delete
        + Looking up hello network security group "NSG-FrontEnd"
        + Deleting network security group "NSG-FrontEnd"
        info:    network nsg delete command OK

## <a name="next-steps"></a><span data-ttu-id="2ff70-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2ff70-165">Next steps</span></span>
* <span data-ttu-id="2ff70-166">[Naplózás engedélyezése](virtual-network-nsg-manage-log.md) az NSG-ket.</span><span class="sxs-lookup"><span data-stu-id="2ff70-166">[Enable logging](virtual-network-nsg-manage-log.md) for NSGs.</span></span>

