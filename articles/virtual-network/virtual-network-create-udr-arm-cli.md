---
title: "Útválasztási és a virtuális készülékek használata az Azure CLI 2.0 szabályozása |} Microsoft Docs"
description: "Megtudhatja, hogyan szabályozhatja az Azure CLI 2.0 használatával útválasztási és a virtuális készülékek."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5452a0b8-21a6-4699-8d6a-e2d8faf32c25
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2017
ms.author: jdial
ms.openlocfilehash: e5d9519998346619093f443b740c8904283f76e8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="create-user-defined-routes-udr-using-the-azure-cli-20"></a><span data-ttu-id="c49b6-103">Hozzon létre felhasználói útvonalakat (UDR) az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="c49b6-103">Create User-Defined Routes (UDR) using the Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c49b6-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c49b6-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="c49b6-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c49b6-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="c49b6-106">Sablon</span><span class="sxs-lookup"><span data-stu-id="c49b6-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="c49b6-107">PowerShell (Classic deployment)</span><span class="sxs-lookup"><span data-stu-id="c49b6-107">PowerShell (Classic deployment)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="c49b6-108">CLI (Classic deployment)</span><span class="sxs-lookup"><span data-stu-id="c49b6-108">CLI (Classic deployment)</span></span>](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="c49b6-109">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="c49b6-109">CLI versions to complete the task</span></span> 

<span data-ttu-id="c49b6-110">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="c49b6-110">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="c49b6-111">[Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – parancssori felületünk a klasszikus és a Resource Management üzemi modellekhez</span><span class="sxs-lookup"><span data-stu-id="c49b6-111">[Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – our CLI for the classic and resource management deployment models</span></span> 
- <span data-ttu-id="c49b6-112">[Az Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) -erőforrás felügyeleti telepítési modell (Ez a cikk) a következő generációs parancssori felület</span><span class="sxs-lookup"><span data-stu-id="c49b6-112">[Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) - our next generation CLI for the resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="c49b6-113">Az alábbi minta Azure parancssori felület parancsait már a fenti forgatókönyv alapján létre egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="c49b6-113">The sample Azure CLI commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="c49b6-114">Ha szeretné a parancsokat a jelen dokumentum megjelenített, először összeállítása a tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), kattintson a **az Azure telepítéséhez**, cserélje le az alapértelmezett paraméterértékek, ha szükséges, és kövesse az utasításokat a portálon.</span><span class="sxs-lookup"><span data-stu-id="c49b6-114">If you want to run the commands as they are displayed in this document, first build the test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy to Azure**, replace the default parameter values if necessary, and follow the instructions in the portal.</span></span>


## <a name="create-the-udr-for-the-front-end-subnet"></a><span data-ttu-id="c49b6-115">Az előtér-alhálózat UDR létrehozása</span><span class="sxs-lookup"><span data-stu-id="c49b6-115">Create the UDR for the front-end subnet</span></span>
<span data-ttu-id="c49b6-116">Az útválasztási táblázatot és az előtér-alhálózat, a fenti forgatókönyv alapján szükséges útvonal létrehozásához kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c49b6-116">To create the route table and route needed for the front end subnet based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="c49b6-117">Hozzon létre egy útválasztási táblázatot előtér-alhálózat a [az hálózati útvonal-táblázat létrehozása](/cli/azure/network/route-table#create) parancs:</span><span class="sxs-lookup"><span data-stu-id="c49b6-117">Create a route table for the front-end subnet with the [az network route-table create](/cli/azure/network/route-table#create) command:</span></span>

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    <span data-ttu-id="c49b6-118">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="c49b6-118">Output:</span></span>

    ```json
    {
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
    "location": "centralus",
    "name": "UDR-FrontEnd",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "routes": [],
    "subnets": null,
    "tags": null,
    "type": "Microsoft.Network/routeTables"
    }
    ```

2. <span data-ttu-id="c49b6-119">Egy olyan útvonalat, amelyet az összes adatforgalmat küld a háttér-alhálózathoz (192.168.2.0/24) hozhat létre a **FW1** (192.168.0.4) virtuális gép használata a [az hálózati útvonaltábla útvonal létrehozása](/cli/azure/network/route-table/route#create) parancs:</span><span class="sxs-lookup"><span data-stu-id="c49b6-119">Create a route that sends all traffic destined to the back-end subnet (192.168.2.0/24) to the **FW1** VM (192.168.0.4) using the [az network route-table route create](/cli/azure/network/route-table/route#create) command:</span></span>

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    <span data-ttu-id="c49b6-120">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="c49b6-120">Output:</span></span>

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd/routes/RouteToBackEnd",
    "name": "RouteToBackEnd",
    "nextHopIpAddress": "192.168.0.4",
    "nextHopType": "VirtualAppliance",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg"
    }
    ```
    <span data-ttu-id="c49b6-121">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="c49b6-121">Parameters:</span></span>

    * <span data-ttu-id="c49b6-122">**--útvonal-részes-táblanév**.</span><span class="sxs-lookup"><span data-stu-id="c49b6-122">**--route-table-name**.</span></span> <span data-ttu-id="c49b6-123">Az útvonaltábla, ahova a rendszer hozzáadja az útvonal neve.</span><span class="sxs-lookup"><span data-stu-id="c49b6-123">Name of the route table where the route will be added.</span></span> <span data-ttu-id="c49b6-124">A mi esetünkben *UDR-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="c49b6-124">For our scenario, *UDR-FrontEnd*.</span></span>
    * <span data-ttu-id="c49b6-125">**--address-prefix**.</span><span class="sxs-lookup"><span data-stu-id="c49b6-125">**--address-prefix**.</span></span> <span data-ttu-id="c49b6-126">Az alhálózat, ahol a csomagok irányuló forgalomnál címelőtagot.</span><span class="sxs-lookup"><span data-stu-id="c49b6-126">Address prefix for the subnet where packets are destined to.</span></span> <span data-ttu-id="c49b6-127">A mi esetünkben *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="c49b6-127">For our scenario, *192.168.2.0/24*.</span></span>
    * <span data-ttu-id="c49b6-128">**--következő ugrás-típusú**.</span><span class="sxs-lookup"><span data-stu-id="c49b6-128">**--next-hop-type**.</span></span> <span data-ttu-id="c49b6-129">Típusú objektum forgalom kapnak.</span><span class="sxs-lookup"><span data-stu-id="c49b6-129">Type of object traffic will be sent to.</span></span> <span data-ttu-id="c49b6-130">A lehetséges értékek: *VirtualAppliance*, *pedig*, *VNETLocal*, *Internet*, vagy *nincs*.</span><span class="sxs-lookup"><span data-stu-id="c49b6-130">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
    * <span data-ttu-id="c49b6-131">**--next-hop-ip-cím**.</span><span class="sxs-lookup"><span data-stu-id="c49b6-131">**--next-hop-ip-address**.</span></span> <span data-ttu-id="c49b6-132">Következő ugrás IP-címet.</span><span class="sxs-lookup"><span data-stu-id="c49b6-132">IP address for next hop.</span></span> <span data-ttu-id="c49b6-133">A mi esetünkben *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="c49b6-133">For our scenario, *192.168.0.4*.</span></span>

3. <span data-ttu-id="c49b6-134">Futtassa a [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update) parancs az előbb létrehozott útvonaltábla hozzárendelni a **előtér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="c49b6-134">Run the [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) command to associate the route table created above with the **FrontEnd** subnet:</span></span>

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    <span data-ttu-id="c49b6-135">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="c49b6-135">Output:</span></span>

    ```json
    {
    "addressPrefix": "192.168.1.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/testvnet/subnets/FrontEnd",
    "ipConfigurations": null,
    "name": "FrontEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": {
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/routeTables/UDR-FrontEnd",
        "location": null,
        "name": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "routes": null,
        "subnets": null,
        "tags": null,
        "type": null
        }
    }
    ```

    <span data-ttu-id="c49b6-136">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="c49b6-136">Parameters:</span></span>
    
    * <span data-ttu-id="c49b6-137">**--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="c49b6-137">**--vnet-name**.</span></span> <span data-ttu-id="c49b6-138">A VNet neve, ahol az alhálózatban.</span><span class="sxs-lookup"><span data-stu-id="c49b6-138">Name of the VNet where the subnet is located.</span></span> <span data-ttu-id="c49b6-139">A mi esetünkben *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="c49b6-139">For our scenario, *TestVNet*.</span></span>

## <a name="create-the-udr-for-the-back-end-subnet"></a><span data-ttu-id="c49b6-140">A háttér-alhálózat UDR létrehozása</span><span class="sxs-lookup"><span data-stu-id="c49b6-140">Create the UDR for the back-end subnet</span></span>

<span data-ttu-id="c49b6-141">Az útvonaltábla és a háttér-alhálózat, a fenti forgatókönyv alapján szükséges útvonal létrehozásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c49b6-141">To create the route table and route needed for the back-end subnet based on the scenario above, complete the following steps:</span></span>

1. <span data-ttu-id="c49b6-142">A következő parancsot egy útválasztási táblázatot a háttér-alhálózat létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="c49b6-142">Run the following command to create a route table for the back-end subnet:</span></span>

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. <span data-ttu-id="c49b6-143">A következő parancsot egy útvonal létrehozása az útválasztási táblázatban az előtér-alhálózat (192.168.1.0/24) irányuló összes forgalom küldése a a **FW1** virtuális gép (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="c49b6-143">Run the following command to create a route in the route table to send all traffic destined to the front-end subnet (192.168.1.0/24) to the **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. <span data-ttu-id="c49b6-144">A következő parancsot az útvonaltáblában a társítja a **háttér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="c49b6-144">Run the following command to associate the route table with the **BackEnd** subnet:</span></span>

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="c49b6-145">IP-továbbítás a FW1 engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c49b6-145">Enable IP forwarding on FW1</span></span>

<span data-ttu-id="c49b6-146">A hálózati adapter által használt IP-továbbítás engedélyezése **FW1**, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c49b6-146">To enable IP forwarding in the NIC used by **FW1**, complete the following steps:</span></span>

1. <span data-ttu-id="c49b6-147">Futtassa a [az hálózati nic megjelenítése](/cli/azure/network/nic#show) JMESPATH szűrő megjelenítése az aktuális parancsot **enable-ip-továbbítás** értékének **engedélyezése IP-továbbítás**.</span><span class="sxs-lookup"><span data-stu-id="c49b6-147">Run the [az network nic show](/cli/azure/network/nic#show) command with a JMESPATH filter to display the current **enable-ip-forwarding** value for **Enable IP forwarding**.</span></span> <span data-ttu-id="c49b6-148">Beállításaként pedig *hamis*.</span><span class="sxs-lookup"><span data-stu-id="c49b6-148">It should be set to *false*.</span></span>

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    <span data-ttu-id="c49b6-149">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="c49b6-149">Output:</span></span>

        false

2. <span data-ttu-id="c49b6-150">A következő paranccsal IP-továbbítás engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="c49b6-150">Run the following command to enable IP forwarding:</span></span>

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    <span data-ttu-id="c49b6-151">Vizsgálja meg a folyamatos átviteli a konzol kimeneti, vagy az adott csak ellenőrzése hosszadalmas **enableIpForwarding** érték:</span><span class="sxs-lookup"><span data-stu-id="c49b6-151">You can examine the output streamed to the console, or just retest for the specific **enableIpForwarding** value:</span></span>

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    <span data-ttu-id="c49b6-152">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="c49b6-152">Output:</span></span>

        true

    <span data-ttu-id="c49b6-153">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="c49b6-153">Parameters:</span></span>

    <span data-ttu-id="c49b6-154">**--ip-továbbítás**: *igaz* vagy *hamis*.</span><span class="sxs-lookup"><span data-stu-id="c49b6-154">**--ip-forwarding**: *true* or *false*.</span></span>

