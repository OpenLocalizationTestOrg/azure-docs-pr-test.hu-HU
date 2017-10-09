---
title: "aaaControl útválasztási és a virtuális készülékek használata Azure CLI 1.0 hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toocontrol útválasztási és a virtuális készülékek használata hello Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/18/2017
ms.author: jdial
ms.openlocfilehash: 1c8a552d949521fa554880c00405e65fa47a8162
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-10"></a><span data-ttu-id="779f5-103">Hozzon létre felhasználói útvonalakat (UDR) hello Azure CLI 1.0 használatával</span><span class="sxs-lookup"><span data-stu-id="779f5-103">Create User-Defined Routes (UDR) using hello Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="779f5-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="779f5-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="779f5-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="779f5-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="779f5-106">Sablon</span><span class="sxs-lookup"><span data-stu-id="779f5-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="779f5-107">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="779f5-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="779f5-108">Parancssori felület (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="779f5-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

<span data-ttu-id="779f5-109">Hozzon létre egyéni Útválasztás és a virtuális készülékek hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="779f5-109">Create custom routing and virtual appliances using hello Azure CLI.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="779f5-110">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="779f5-110">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="779f5-111">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="779f5-111">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="779f5-112">[Az Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="779f5-112">[Azure CLI 1.0](#Create-the-UDR-for-the-front-end-subnet) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="779f5-113">[Az Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="779f5-113">[Azure CLI 2.0](virtual-network-create-udr-arm-cli.md) - our next generation CLI for hello resource management deployment model</span></span> 


[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="779f5-114">minta hello Azure CLI-t az alábbi parancsok már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="779f5-114">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="779f5-115">Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre hello tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek Ha szükséges, és kövesse az utasításokat hello a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="779f5-115">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>


## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="779f5-116">Hello UDR hello előtér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="779f5-116">Create hello UDR for hello front-end subnet</span></span>
<span data-ttu-id="779f5-117">toocreate hello útválasztási táblázatot és hello előtérben lévő alhálózat alhálózat alapján hello forgatókönyv fenti, a szükséges útvonal kövesse a hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="779f5-117">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="779f5-118">Futtassa a következő parancs toocreate hello hello előtér-alhálózat egy útválasztási táblázatot:</span><span class="sxs-lookup"><span data-stu-id="779f5-118">Run hello following command toocreate a route table for hello front-end subnet:</span></span>

    ```azurecli
    azure network route-table create -g TestRG -n UDR-FrontEnd -l uswest
    ```
   
    <span data-ttu-id="779f5-119">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="779f5-119">Output:</span></span>
   
        info:    Executing command network route-table create
        info:    Looking up route table "UDR-FrontEnd"
        info:    Creating route table "UDR-FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    Name                            : UDR-FrontEnd
        data:    Type                            : Microsoft.Network/routeTables
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        info:    network route-table create command OK
   
    <span data-ttu-id="779f5-120">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="779f5-120">Parameters:</span></span>
   
   * <span data-ttu-id="779f5-121">**-g (vagy --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="779f5-121">**-g (or --resource-group)**.</span></span> <span data-ttu-id="779f5-122">Ahol létrejön hello UDR hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="779f5-122">Name of hello resource group where hello UDR will be created.</span></span> <span data-ttu-id="779f5-123">A mi esetünkben *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="779f5-123">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="779f5-124">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="779f5-124">**-l (or --location)**.</span></span> <span data-ttu-id="779f5-125">Azure-régió, ahol hello új UDR létrejön.</span><span class="sxs-lookup"><span data-stu-id="779f5-125">Azure region where hello new UDR will be created.</span></span> <span data-ttu-id="779f5-126">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="779f5-126">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="779f5-127">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="779f5-127">**-n (or --name)**.</span></span> <span data-ttu-id="779f5-128">Hello nevét új UDR.</span><span class="sxs-lookup"><span data-stu-id="779f5-128">Name for hello new UDR.</span></span> <span data-ttu-id="779f5-129">A mi esetünkben *UDR-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="779f5-129">For our scenario, *UDR-FrontEnd*.</span></span>
2. <span data-ttu-id="779f5-130">Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello háttér-alhálózat (192.168.2.0/24) toohello **FW1** virtuális gép (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="779f5-130">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -y VirtualAppliance -p 192.168.0.4
    ```
   
    <span data-ttu-id="779f5-131">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="779f5-131">Output:</span></span>
   
        info:    Executing command network route-table route create
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        info:    Creating route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    Looking up route "RouteToBackEnd" in route table "UDR-FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd/routes/RouteToBackEnd
        data:    Name                            : RouteToBackEnd
        data:    Provisioning state              : Succeeded
        data:    Next hop type                   : VirtualAppliance
        data:    Next hop IP address             : 192.168.0.4
        data:    Address prefix                  : 192.168.2.0/24
        info:    network route-table route create command OK
   
    <span data-ttu-id="779f5-132">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="779f5-132">Parameters:</span></span>
   
   * <span data-ttu-id="779f5-133">**-r (vagy--útvonal-részes-táblanév)**.</span><span class="sxs-lookup"><span data-stu-id="779f5-133">**-r (or --route-table-name)**.</span></span> <span data-ttu-id="779f5-134">Amennyiben adható hozzá hello útvonal hello útvonaltábla neve.</span><span class="sxs-lookup"><span data-stu-id="779f5-134">Name of hello route table where hello route will be added.</span></span> <span data-ttu-id="779f5-135">A mi esetünkben *UDR-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="779f5-135">For our scenario, *UDR-FrontEnd*.</span></span>
   * <span data-ttu-id="779f5-136">**-a (vagy --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="779f5-136">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="779f5-137">Ha a csomagok irányuló forgalomnál hello alhálózat címelőtagot.</span><span class="sxs-lookup"><span data-stu-id="779f5-137">Address prefix for hello subnet where packets are destined to.</span></span> <span data-ttu-id="779f5-138">A mi esetünkben *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="779f5-138">For our scenario, *192.168.2.0/24*.</span></span>
   * <span data-ttu-id="779f5-139">**-y (vagy--tovább-hop-type)**.</span><span class="sxs-lookup"><span data-stu-id="779f5-139">**-y (or --next-hop-type)**.</span></span> <span data-ttu-id="779f5-140">Típusú objektum forgalom kapnak.</span><span class="sxs-lookup"><span data-stu-id="779f5-140">Type of object traffic will be sent to.</span></span> <span data-ttu-id="779f5-141">A lehetséges értékek: *VirtualAppliance*, *pedig*, *VNETLocal*, *Internet*, vagy *nincs*.</span><span class="sxs-lookup"><span data-stu-id="779f5-141">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
   * <span data-ttu-id="779f5-142">**-p (vagy--next-hop-ip-cím**).</span><span class="sxs-lookup"><span data-stu-id="779f5-142">**-p (or --next-hop-ip-address**).</span></span> <span data-ttu-id="779f5-143">Következő ugrás IP-címet.</span><span class="sxs-lookup"><span data-stu-id="779f5-143">IP address for next hop.</span></span> <span data-ttu-id="779f5-144">A mi esetünkben *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="779f5-144">For our scenario, *192.168.0.4*.</span></span>
3. <span data-ttu-id="779f5-145">Futtatási hello következő parancsot a tooassociate hello útvonaltábla hello az előbb létrehozott **előtér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="779f5-145">Run hello following command tooassociate hello route table created above with hello **FrontEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    <span data-ttu-id="779f5-146">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="779f5-146">Output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up route table "UDR-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    Route Table                     : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        routeTables/UDR-FrontEnd
        data:    IP configurations:
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB1/ipConf
        igurations/ipconfig1
        data:      /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/NICWEB2/ipConf
        igurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK
   
    <span data-ttu-id="779f5-147">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="779f5-147">Parameters:</span></span>
   
   * <span data-ttu-id="779f5-148">**-e (vagy--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="779f5-148">**-e (or --vnet-name)**.</span></span> <span data-ttu-id="779f5-149">Hello ahol hello alhálózat VNet neve.</span><span class="sxs-lookup"><span data-stu-id="779f5-149">Name of hello VNet where hello subnet is located.</span></span> <span data-ttu-id="779f5-150">A mi esetünkben *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="779f5-150">For our scenario, *TestVNet*.</span></span>

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="779f5-151">Hello UDR hello háttér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="779f5-151">Create hello UDR for hello back-end subnet</span></span>
<span data-ttu-id="779f5-152">toocreate hello útvonaltábla és útvonal-hello háttér-alhálózat a fenti lépések teljes hello hello forgatókönyv alapján szükséges:</span><span class="sxs-lookup"><span data-stu-id="779f5-152">toocreate hello route table and route needed for hello back-end subnet based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="779f5-153">Futtassa a következő parancs toocreate hello hello háttér-alhálózat egy útválasztási táblázatot:</span><span class="sxs-lookup"><span data-stu-id="779f5-153">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```azurecli
    azure network route-table create -g TestRG -n UDR-BackEnd -l westus
    ```

2. <span data-ttu-id="779f5-154">Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello előtér-alhálózat (192.168.1.0/24) toohello **FW1** virtuális gép (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="779f5-154">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route create -g TestRG -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -y VirtualAppliance -p 192.168.0.4
    ```

3. <span data-ttu-id="779f5-155">Futtatási hello parancs tooassociate hello útvonaltáblában hello a következő **háttér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="779f5-155">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -r UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="779f5-156">IP-továbbítás a FW1 engedélyezése</span><span class="sxs-lookup"><span data-stu-id="779f5-156">Enable IP forwarding on FW1</span></span>
<span data-ttu-id="779f5-157">a hálózati adapter által használt hello tooenable IP-továbbítás **FW1**, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="779f5-157">tooenable IP forwarding in hello NIC used by **FW1**, complete hello following steps:</span></span>

1. <span data-ttu-id="779f5-158">Futtassa a következő és hello érték a hello parancsot **engedélyezése IP-továbbítás**.</span><span class="sxs-lookup"><span data-stu-id="779f5-158">Run hello command that follows and notice hello value for **Enable IP forwarding**.</span></span> <span data-ttu-id="779f5-159">Kell lennie állítva, akkor túl*hamis*.</span><span class="sxs-lookup"><span data-stu-id="779f5-159">It should be set too*false*.</span></span>

    ```azurecli
    azure network nic show -g TestRG -n NICFW1
    ```

    <span data-ttu-id="779f5-160">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="779f5-160">Output:</span></span>
   
        info:    Executing command network nic show
        info:    Looking up hello network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : false
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic show command OK
2. <span data-ttu-id="779f5-161">Futtassa a következő parancs tooenable IP-továbbítás hello:</span><span class="sxs-lookup"><span data-stu-id="779f5-161">Run hello following command tooenable IP forwarding:</span></span>

    ```azurecli
    azure network nic set -g TestRG -n NICFW1 -f true
    ```
   
    <span data-ttu-id="779f5-162">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="779f5-162">Output:</span></span>
   
        info:    Executing command network nic set
        info:    Looking up hello network interface "NICFW1"
        info:    Updating network interface "NICFW1"
        info:    Looking up hello network interface "NICFW1"
        data:    Id                              : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        networkInterfaces/NICFW1
        data:    Name                            : NICFW1
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    MAC address                     : 00-0D-3A-30-95-B3
        data:    Enable IP forwarding            : true
        data:    Tags                            : displayName=NetworkInterfaces - DMZ
        data:    Virtual machine                 : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Compute/
        virtualMachines/FW1
        data:    IP configurations:
        data:      Name                          : ipconfig1
        data:      Provisioning state            : Succeeded
        data:      Public IP address             : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        publicIPAddresses/PIPFW1
        data:      Private IP address            : 192.168.0.4
        data:      Private IP Allocation Method  : Static
        data:      Subnet                        : /subscriptions/[Subscription Id]/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/DMZ
        data:    
        info:    network nic set command OK
   
    <span data-ttu-id="779f5-163">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="779f5-163">Parameters:</span></span>
   
   * <span data-ttu-id="779f5-164">**-f (vagy--enable-ip-továbbítás)**.</span><span class="sxs-lookup"><span data-stu-id="779f5-164">**-f (or --enable-ip-forwarding)**.</span></span> <span data-ttu-id="779f5-165">*Igaz* vagy *hamis*.</span><span class="sxs-lookup"><span data-stu-id="779f5-165">*true* or *false*.</span></span>

