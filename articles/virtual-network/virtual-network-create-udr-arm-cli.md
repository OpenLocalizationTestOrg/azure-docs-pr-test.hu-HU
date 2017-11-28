---
title: "aaaControl útválasztási és a virtuális készülékek használata Azure CLI 2.0 hello |} Microsoft Docs"
description: "Ismerje meg, hogyan toocontrol útválasztási és a virtuális készülékek használata hello Azure CLI 2.0."
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
ms.openlocfilehash: 79b908848932a4365dab1b7497b6a0dbf44bbaf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-user-defined-routes-udr-using-hello-azure-cli-20"></a><span data-ttu-id="0eb8c-103">Hozzon létre felhasználói útvonalakat (UDR) hello Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="0eb8c-103">Create User-Defined Routes (UDR) using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="0eb8c-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="0eb8c-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="0eb8c-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="0eb8c-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="0eb8c-106">Sablon</span><span class="sxs-lookup"><span data-stu-id="0eb8c-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="0eb8c-107">PowerShell (Classic deployment)</span><span class="sxs-lookup"><span data-stu-id="0eb8c-107">PowerShell (Classic deployment)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="0eb8c-108">CLI (Classic deployment)</span><span class="sxs-lookup"><span data-stu-id="0eb8c-108">CLI (Classic deployment)</span></span>](virtual-network-create-udr-classic-cli.md)

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="0eb8c-109">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="0eb8c-109">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="0eb8c-110">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-110">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="0eb8c-111">[Az Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="0eb8c-111">[Azure CLI 1.0](virtual-network-create-udr-arm-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="0eb8c-112">[Az Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk)</span><span class="sxs-lookup"><span data-stu-id="0eb8c-112">[Azure CLI 2.0](#Create-the-UDR-for-the-front-end-subnet) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="0eb8c-113">minta hello Azure CLI-t az alábbi parancsok már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-113">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="0eb8c-114">Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre hello tesztkörnyezetben üzembe helyezésével [sablon](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), kattintson a **tooAzure telepítése**, cserélje le a hello alapértelmezett paraméterértékek Ha szükséges, és kövesse az utasításokat hello a hello portálon.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-114">If you want toorun hello commands as they are displayed in this document, first build hello test environment by deploying [this template](http://github.com/telmosampaio/azure-templates/tree/master/IaaS-NSG-UDR-Before), click **Deploy tooAzure**, replace hello default parameter values if necessary, and follow hello instructions in hello portal.</span></span>


## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="0eb8c-115">Hello UDR hello előtér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0eb8c-115">Create hello UDR for hello front-end subnet</span></span>
<span data-ttu-id="0eb8c-116">toocreate hello útválasztási táblázatot és hello előtérben lévő alhálózat alhálózat alapján hello forgatókönyv fenti, a szükséges útvonal kövesse a hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-116">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="0eb8c-117">Hozzon létre egy útválasztási táblázatot hello előtér-alhálózat hello [az hálózati útvonal-táblázat létrehozása](/cli/azure/network/route-table#create) parancs:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-117">Create a route table for hello front-end subnet with hello [az network route-table create](/cli/azure/network/route-table#create) command:</span></span>

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --location centralus \
    --name UDR-FrontEnd
    ```

    <span data-ttu-id="0eb8c-118">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-118">Output:</span></span>

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

2. <span data-ttu-id="0eb8c-119">Hozzon létre egy olyan útvonalat, amelyet az összes adatforgalmat toohello háttér-alhálózat (192.168.2.0/24) toohello küld **FW1** használó virtuális gépek (192.168.0.4) hello [az hálózati útvonaltábla útvonal létrehozása](/cli/azure/network/route-table/route#create) parancs:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-119">Create a route that sends all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4) using hello [az network route-table route create](/cli/azure/network/route-table/route#create) command:</span></span>

    ```azurecli 
    az network route-table route create \
    --resource-group testrg \
    --name RouteToBackEnd \
    --route-table-name UDR-FrontEnd \
    --address-prefix 192.168.2.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

    <span data-ttu-id="0eb8c-120">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-120">Output:</span></span>

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
    <span data-ttu-id="0eb8c-121">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-121">Parameters:</span></span>

    * <span data-ttu-id="0eb8c-122">**--útvonal-részes-táblanév**.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-122">**--route-table-name**.</span></span> <span data-ttu-id="0eb8c-123">Amennyiben adható hozzá hello útvonal hello útvonaltábla neve.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-123">Name of hello route table where hello route will be added.</span></span> <span data-ttu-id="0eb8c-124">A mi esetünkben *UDR-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-124">For our scenario, *UDR-FrontEnd*.</span></span>
    * <span data-ttu-id="0eb8c-125">**--address-prefix**.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-125">**--address-prefix**.</span></span> <span data-ttu-id="0eb8c-126">Ha a csomagok irányuló forgalomnál hello alhálózat címelőtagot.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-126">Address prefix for hello subnet where packets are destined to.</span></span> <span data-ttu-id="0eb8c-127">A mi esetünkben *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-127">For our scenario, *192.168.2.0/24*.</span></span>
    * <span data-ttu-id="0eb8c-128">**--következő ugrás-típusú**.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-128">**--next-hop-type**.</span></span> <span data-ttu-id="0eb8c-129">Típusú objektum forgalom kapnak.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-129">Type of object traffic will be sent to.</span></span> <span data-ttu-id="0eb8c-130">A lehetséges értékek: *VirtualAppliance*, *pedig*, *VNETLocal*, *Internet*, vagy *nincs*.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-130">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
    * <span data-ttu-id="0eb8c-131">**--next-hop-ip-cím**.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-131">**--next-hop-ip-address**.</span></span> <span data-ttu-id="0eb8c-132">Következő ugrás IP-címet.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-132">IP address for next hop.</span></span> <span data-ttu-id="0eb8c-133">A mi esetünkben *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-133">For our scenario, *192.168.0.4*.</span></span>

3. <span data-ttu-id="0eb8c-134">Futtassa a hello [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update) hello az előbb létrehozott parancs tooassociate hello útvonaltábla **előtér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-134">Run hello [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) command tooassociate hello route table created above with hello **FrontEnd** subnet:</span></span>

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name FrontEnd \
    --route-table UDR-FrontEnd
    ```

    <span data-ttu-id="0eb8c-135">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-135">Output:</span></span>

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

    <span data-ttu-id="0eb8c-136">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-136">Parameters:</span></span>
    
    * <span data-ttu-id="0eb8c-137">**--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-137">**--vnet-name**.</span></span> <span data-ttu-id="0eb8c-138">Hello ahol hello alhálózat VNet neve.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-138">Name of hello VNet where hello subnet is located.</span></span> <span data-ttu-id="0eb8c-139">A mi esetünkben *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-139">For our scenario, *TestVNet*.</span></span>

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="0eb8c-140">Hello UDR hello háttér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="0eb8c-140">Create hello UDR for hello back-end subnet</span></span>

<span data-ttu-id="0eb8c-141">toocreate hello útvonaltábla és útvonal-hello háttér-alhálózat a fenti lépések teljes hello hello forgatókönyv alapján szükséges:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-141">toocreate hello route table and route needed for hello back-end subnet based on hello scenario above, complete hello following steps:</span></span>

1. <span data-ttu-id="0eb8c-142">Futtassa a következő parancs toocreate hello hello háttér-alhálózat egy útválasztási táblázatot:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-142">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```azurecli
    az network route-table create \
    --resource-group testrg \
    --name UDR-BackEnd \
    --location centralus
    ```

2. <span data-ttu-id="0eb8c-143">Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello előtér-alhálózat (192.168.1.0/24) toohello **FW1** virtuális gép (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="0eb8c-143">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    az network route-table route create \
    --resource-group testrg \
    --name RouteToFrontEnd \
    --route-table-name UDR-BackEnd \
    --address-prefix 192.168.1.0/24 \
    --next-hop-type VirtualAppliance \
    --next-hop-ip-address 192.168.0.4
    ```

3. <span data-ttu-id="0eb8c-144">Futtatási hello parancs tooassociate hello útvonaltáblában hello a következő **háttér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-144">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```azurecli
    az network vnet subnet update \
    --resource-group testrg \
    --vnet-name testvnet \
    --name BackEnd \
    --route-table UDR-BackEnd
    ```

## <a name="enable-ip-forwarding-on-fw1"></a><span data-ttu-id="0eb8c-145">IP-továbbítás a FW1 engedélyezése</span><span class="sxs-lookup"><span data-stu-id="0eb8c-145">Enable IP forwarding on FW1</span></span>

<span data-ttu-id="0eb8c-146">a hálózati adapter által használt hello tooenable IP-továbbítás **FW1**, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-146">tooenable IP forwarding in hello NIC used by **FW1**, complete hello following steps:</span></span>

1. <span data-ttu-id="0eb8c-147">Hello futtatása [az hálózati nic megjelenítése](/cli/azure/network/nic#show) parancsot egy JMESPATH szűrő toodisplay hello aktuális **enable-ip-továbbítás** értékének **engedélyezése IP-továbbítás**.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-147">Run hello [az network nic show](/cli/azure/network/nic#show) command with a JMESPATH filter toodisplay hello current **enable-ip-forwarding** value for **Enable IP forwarding**.</span></span> <span data-ttu-id="0eb8c-148">Kell lennie állítva, akkor túl*hamis*.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-148">It should be set too*false*.</span></span>

    ```azurecli
    az network nic show \
    --resource-group testrg \
    --nname nicfw1 \
    --query 'enableIpForwarding' -o tsv
    ```

    <span data-ttu-id="0eb8c-149">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-149">Output:</span></span>

        false

2. <span data-ttu-id="0eb8c-150">Futtassa a következő parancs tooenable IP-továbbítás hello:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-150">Run hello following command tooenable IP forwarding:</span></span>

    ```azurecli
    az network nic update \
    --resource-group testrg \
    --name nicfw1 \
    --ip-forwarding true
    ```

    <span data-ttu-id="0eb8c-151">Vizsgálja meg a hello kimeneti adatfolyamként továbbított toohello konzol, vagy az adott hello csak ellenőrzése hosszadalmas **enableIpForwarding** érték:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-151">You can examine hello output streamed toohello console, or just retest for hello specific **enableIpForwarding** value:</span></span>

    ```azurecli
    az network nic show -g testrg -n nicfw1 --query 'enableIpForwarding' -o tsv
    ```

    <span data-ttu-id="0eb8c-152">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-152">Output:</span></span>

        true

    <span data-ttu-id="0eb8c-153">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="0eb8c-153">Parameters:</span></span>

    <span data-ttu-id="0eb8c-154">**--ip-továbbítás**: *igaz* vagy *hamis*.</span><span class="sxs-lookup"><span data-stu-id="0eb8c-154">**--ip-forwarding**: *true* or *false*.</span></span>

