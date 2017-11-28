---
title: "Szabályozhatja az Azure Virtual Network - CLI - klasszikus Útválasztás |} Microsoft Docs"
description: "Megtudhatja, hogyan szabályozhatja az Azure parancssori felület használatával a klasszikus üzembe helyezési modellel Vnetek Útválasztás"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-service-management
ms.assetid: ca2b4638-8777-4d30-b972-eb790a7c804f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: 8fcb98723e7e872c932908e3456dc8680deb0901
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-the-azure-cli"></a><span data-ttu-id="abd85-103">Szabályozhatja az Útválasztás és használni a virtuális készülékeket (klasszikus) az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="abd85-103">Control routing and use virtual appliances (classic) using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="abd85-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="abd85-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="abd85-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="abd85-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="abd85-106">Sablon</span><span class="sxs-lookup"><span data-stu-id="abd85-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="abd85-107">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="abd85-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="abd85-108">Parancssori felület (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="abd85-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="abd85-109">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="abd85-109">This article covers the classic deployment model.</span></span> <span data-ttu-id="abd85-110">Emellett [szabályozhatja az Útválasztás és a virtuális készülékek használata a Resource Manager üzembe helyezési modellel](virtual-network-create-udr-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="abd85-110">You can also [control routing and use virtual appliances in the Resource Manager deployment model](virtual-network-create-udr-arm-cli.md).</span></span>

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="abd85-111">Az alábbi minta Azure parancssori felület parancsait már a fenti forgatókönyv alapján létre egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="abd85-111">The sample Azure CLI commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="abd85-112">Ha szeretné a parancsokat a jelen dokumentum megjelenített, hozza létre a környezet megjelenő [hozzon létre egy virtuális hálózat (klasszikus) az Azure parancssori felület használatával](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="abd85-112">If you want to run the commands as they are displayed in this document, create the environment shown in [create a VNet (classic) using the Azure CLI](virtual-networks-create-vnet-classic-cli.md).</span></span>

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-the-udr-for-the-front-end-subnet"></a><span data-ttu-id="abd85-113">Az előtér-alhálózat UDR létrehozása</span><span class="sxs-lookup"><span data-stu-id="abd85-113">Create the UDR for the front end subnet</span></span>
<span data-ttu-id="abd85-114">Az útválasztási táblázatot és az előtér-alhálózat, a fenti forgatókönyv alapján szükséges útvonal létrehozásához kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="abd85-114">To create the route table and route needed for the front end subnet based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="abd85-115">Futtassa a következő parancsot a klasszikus mód:</span><span class="sxs-lookup"><span data-stu-id="abd85-115">Run the following command to switch to classic mode:</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="abd85-116">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="abd85-116">Output:</span></span>

        info:    New mode is asm

2. <span data-ttu-id="abd85-117">Futtassa a következő parancsot az előtér-alhálózat egy útválasztási táblázatot létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="abd85-117">Run the following command to create a route table for the front-end subnet:</span></span>

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    <span data-ttu-id="abd85-118">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="abd85-118">Output:</span></span>
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    <span data-ttu-id="abd85-119">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="abd85-119">Parameters:</span></span>
   
   * <span data-ttu-id="abd85-120">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="abd85-120">**-l (or --location)**.</span></span> <span data-ttu-id="abd85-121">Azure-régió, ahol létrejön az új NSG.</span><span class="sxs-lookup"><span data-stu-id="abd85-121">Azure region where the new NSG will be created.</span></span> <span data-ttu-id="abd85-122">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="abd85-122">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="abd85-123">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="abd85-123">**-n (or --name)**.</span></span> <span data-ttu-id="abd85-124">Az új NSG neve.</span><span class="sxs-lookup"><span data-stu-id="abd85-124">Name for the new NSG.</span></span> <span data-ttu-id="abd85-125">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="abd85-125">For our scenario, *NSG-FrontEnd*.</span></span>
3. <span data-ttu-id="abd85-126">A következő parancsot egy útvonal létrehozása az útvonaltáblában a háttér-alhálózat (192.168.2.0/24) irányuló összes forgalom küldése a a **FW1** virtuális gép (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="abd85-126">Run the following command to create a route in the route table to send all traffic destined to the back-end subnet (192.168.2.0/24) to the **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    <span data-ttu-id="abd85-127">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="abd85-127">Output:</span></span>
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    <span data-ttu-id="abd85-128">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="abd85-128">Parameters:</span></span>
   
   * <span data-ttu-id="abd85-129">**-r (vagy--útvonal-részes-táblanév)**.</span><span class="sxs-lookup"><span data-stu-id="abd85-129">**-r (or --route-table-name)**.</span></span> <span data-ttu-id="abd85-130">Az útvonaltábla, ahova a rendszer hozzáadja az útvonal neve.</span><span class="sxs-lookup"><span data-stu-id="abd85-130">Name of the route table where the route will be added.</span></span> <span data-ttu-id="abd85-131">A mi esetünkben *UDR-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="abd85-131">For our scenario, *UDR-FrontEnd*.</span></span>
   * <span data-ttu-id="abd85-132">**-a (vagy --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="abd85-132">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="abd85-133">Az alhálózat, ahol a csomagok irányuló forgalomnál címelőtagot.</span><span class="sxs-lookup"><span data-stu-id="abd85-133">Address prefix for the subnet where packets are destined to.</span></span> <span data-ttu-id="abd85-134">A mi esetünkben *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="abd85-134">For our scenario, *192.168.2.0/24*.</span></span>
   * <span data-ttu-id="abd85-135">**-t (vagy--tovább-hop-type)**.</span><span class="sxs-lookup"><span data-stu-id="abd85-135">**-t (or --next-hop-type)**.</span></span> <span data-ttu-id="abd85-136">Típusú objektum forgalom kapnak.</span><span class="sxs-lookup"><span data-stu-id="abd85-136">Type of object traffic will be sent to.</span></span> <span data-ttu-id="abd85-137">A lehetséges értékek: *VirtualAppliance*, *pedig*, *VNETLocal*, *Internet*, vagy *nincs*.</span><span class="sxs-lookup"><span data-stu-id="abd85-137">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
   * <span data-ttu-id="abd85-138">**-p (vagy--next-hop-ip-cím**).</span><span class="sxs-lookup"><span data-stu-id="abd85-138">**-p (or --next-hop-ip-address**).</span></span> <span data-ttu-id="abd85-139">Következő ugrás IP-címet.</span><span class="sxs-lookup"><span data-stu-id="abd85-139">IP address for next hop.</span></span> <span data-ttu-id="abd85-140">A mi esetünkben *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="abd85-140">For our scenario, *192.168.0.4*.</span></span>
4. <span data-ttu-id="abd85-141">Rendelje hozzá a létrehozott útválasztási táblázatot a következő parancsot a **előtér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="abd85-141">Run the following command to associate the route table created with the **FrontEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    <span data-ttu-id="abd85-142">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="abd85-142">Output:</span></span>
   
        info:    Executing command network vnet subnet route-table add
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK    
   
    <span data-ttu-id="abd85-143">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="abd85-143">Parameters:</span></span>
   
   * <span data-ttu-id="abd85-144">**-t (vagy--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="abd85-144">**-t (or --vnet-name)**.</span></span> <span data-ttu-id="abd85-145">A VNet neve, ahol az alhálózatban.</span><span class="sxs-lookup"><span data-stu-id="abd85-145">Name of the VNet where the subnet is located.</span></span> <span data-ttu-id="abd85-146">A mi esetünkben *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="abd85-146">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="abd85-147">**-n (vagy--alhálózatneve**.</span><span class="sxs-lookup"><span data-stu-id="abd85-147">**-n (or --subnet-name**.</span></span> <span data-ttu-id="abd85-148">Az útvonaltábla nem kerülnek be az alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="abd85-148">Name of the subnet the route table will be added to.</span></span> <span data-ttu-id="abd85-149">A mi esetünkben *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="abd85-149">For our scenario, *FrontEnd*.</span></span>

## <a name="create-the-udr-for-the-back-end-subnet"></a><span data-ttu-id="abd85-150">A háttér-alhálózat UDR létrehozása</span><span class="sxs-lookup"><span data-stu-id="abd85-150">Create the UDR for the back-end subnet</span></span>
<span data-ttu-id="abd85-151">Az útvonaltábla és a háttér-alhálózat alapján a forgatókönyvhöz szükséges útvonal létrehozásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="abd85-151">To create the route table and route needed for the back-end subnet based on the scenario, complete the following steps:</span></span>

1. <span data-ttu-id="abd85-152">A következő parancsot egy útválasztási táblázatot a háttér-alhálózat létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="abd85-152">Run the following command to create a route table for the back-end subnet:</span></span>

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. <span data-ttu-id="abd85-153">A következő parancsot egy útvonal létrehozása az útválasztási táblázatban az előtér-alhálózat (192.168.1.0/24) irányuló összes forgalom küldése a a **FW1** virtuális gép (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="abd85-153">Run the following command to create a route in the route table to send all traffic destined to the front-end subnet (192.168.1.0/24) to the **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. <span data-ttu-id="abd85-154">A következő parancsot az útvonaltáblában a társítja a **háttér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="abd85-154">Run the following command to associate the route table with the **BackEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

