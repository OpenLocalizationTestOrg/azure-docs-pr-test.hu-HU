---
title: "egy Azure-beli virtuális hálózatra - CLI - klasszikus útválasztás aaaControl |} Microsoft Docs"
description: "Ismerje meg, hogyan útválasztás használatával Vnetek toocontrol hello hello klasszikus üzembe helyezési modellel Azure parancssori felület"
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
ms.openlocfilehash: 07dde573f1a605bf280156c261d51e213ede0cdc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="control-routing-and-use-virtual-appliances-classic-using-hello-azure-cli"></a><span data-ttu-id="66cf0-103">Vezérlő az Útválasztás és a használata (klasszikus) virtuális készülékek használata Azure CLI hello</span><span class="sxs-lookup"><span data-stu-id="66cf0-103">Control routing and use virtual appliances (classic) using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="66cf0-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="66cf0-104">PowerShell</span></span>](virtual-network-create-udr-arm-ps.md)
> * [<span data-ttu-id="66cf0-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="66cf0-105">Azure CLI</span></span>](virtual-network-create-udr-arm-cli.md)
> * [<span data-ttu-id="66cf0-106">Sablon</span><span class="sxs-lookup"><span data-stu-id="66cf0-106">Template</span></span>](virtual-network-create-udr-arm-template.md)
> * [<span data-ttu-id="66cf0-107">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="66cf0-107">PowerShell (Classic)</span></span>](virtual-network-create-udr-classic-ps.md)
> * [<span data-ttu-id="66cf0-108">Parancssori felület (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="66cf0-108">CLI (Classic)</span></span>](virtual-network-create-udr-classic-cli.md)

[!INCLUDE [virtual-network-create-udr-intro-include.md](../../includes/virtual-network-create-udr-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="66cf0-109">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="66cf0-109">This article covers hello classic deployment model.</span></span> <span data-ttu-id="66cf0-110">Emellett [szabályozhatja az Útválasztás és használni a virtuális készülékeket hello Resource Manager üzembe helyezési modellel](virtual-network-create-udr-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="66cf0-110">You can also [control routing and use virtual appliances in hello Resource Manager deployment model](virtual-network-create-udr-arm-cli.md).</span></span>

[!INCLUDE [virtual-network-create-udr-scenario-include.md](../../includes/virtual-network-create-udr-scenario-include.md)]

<span data-ttu-id="66cf0-111">minta hello Azure CLI-t az alábbi parancsok már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="66cf0-111">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="66cf0-112">Ha toorun hello parancsok ebben a dokumentumban megjelenített, hozzon létre hello környezet látható [VNet létrehozása (klasszikus) használatával hello Azure CLI](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="66cf0-112">If you want toorun hello commands as they are displayed in this document, create hello environment shown in [create a VNet (classic) using hello Azure CLI](virtual-networks-create-vnet-classic-cli.md).</span></span>

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="create-hello-udr-for-hello-front-end-subnet"></a><span data-ttu-id="66cf0-113">Hello UDR hello előtér alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="66cf0-113">Create hello UDR for hello front end subnet</span></span>
<span data-ttu-id="66cf0-114">toocreate hello útválasztási táblázatot és hello előtérben lévő alhálózat alhálózat alapján hello forgatókönyv fenti, a szükséges útvonal kövesse a hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="66cf0-114">toocreate hello route table and route needed for hello front end subnet based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="66cf0-115">Futtassa a következő tooswitch tooclassic üzemmód hello:</span><span class="sxs-lookup"><span data-stu-id="66cf0-115">Run hello following command tooswitch tooclassic mode:</span></span>

    ```azurecli
    azure config mode asm
    ```

    <span data-ttu-id="66cf0-116">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="66cf0-116">Output:</span></span>

        info:    New mode is asm

2. <span data-ttu-id="66cf0-117">Futtassa a következő parancs toocreate hello hello előtér-alhálózat egy útválasztási táblázatot:</span><span class="sxs-lookup"><span data-stu-id="66cf0-117">Run hello following command toocreate a route table for hello front-end subnet:</span></span>

    ```azurecli
    azure network route-table create -n UDR-FrontEnd -l uswest
    ```
   
    <span data-ttu-id="66cf0-118">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="66cf0-118">Output:</span></span>
   
        info:    Executing command network route-table create
        info:    Creating route table "UDR-FrontEnd"
        info:    Getting route table "UDR-FrontEnd"
        data:    Name                            : UDR-FrontEnd
        data:    Location                        : West US
        info:    network route-table create command OK
   
    <span data-ttu-id="66cf0-119">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="66cf0-119">Parameters:</span></span>
   
   * <span data-ttu-id="66cf0-120">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="66cf0-120">**-l (or --location)**.</span></span> <span data-ttu-id="66cf0-121">Azure-régió, ahol hello új NSG létrejön.</span><span class="sxs-lookup"><span data-stu-id="66cf0-121">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="66cf0-122">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="66cf0-122">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="66cf0-123">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="66cf0-123">**-n (or --name)**.</span></span> <span data-ttu-id="66cf0-124">Hello nevét új NSG.</span><span class="sxs-lookup"><span data-stu-id="66cf0-124">Name for hello new NSG.</span></span> <span data-ttu-id="66cf0-125">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="66cf0-125">For our scenario, *NSG-FrontEnd*.</span></span>
3. <span data-ttu-id="66cf0-126">Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello háttér-alhálózat (192.168.2.0/24) toohello **FW1** virtuális gép (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="66cf0-126">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello back-end subnet (192.168.2.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route set -r UDR-FrontEnd -n RouteToBackEnd -a 192.168.2.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

    <span data-ttu-id="66cf0-127">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="66cf0-127">Output:</span></span>
   
        info:    Executing command network route-table route set
        info:    Getting route table "UDR-FrontEnd"
        info:    Setting route "RouteToBackEnd" in a route table "UDR-FrontEnd"
        info:    network route-table route set command OK
   
    <span data-ttu-id="66cf0-128">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="66cf0-128">Parameters:</span></span>
   
   * <span data-ttu-id="66cf0-129">**-r (vagy--útvonal-részes-táblanév)**.</span><span class="sxs-lookup"><span data-stu-id="66cf0-129">**-r (or --route-table-name)**.</span></span> <span data-ttu-id="66cf0-130">Amennyiben adható hozzá hello útvonal hello útvonaltábla neve.</span><span class="sxs-lookup"><span data-stu-id="66cf0-130">Name of hello route table where hello route will be added.</span></span> <span data-ttu-id="66cf0-131">A mi esetünkben *UDR-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="66cf0-131">For our scenario, *UDR-FrontEnd*.</span></span>
   * <span data-ttu-id="66cf0-132">**-a (vagy --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="66cf0-132">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="66cf0-133">Ha a csomagok irányuló forgalomnál hello alhálózat címelőtagot.</span><span class="sxs-lookup"><span data-stu-id="66cf0-133">Address prefix for hello subnet where packets are destined to.</span></span> <span data-ttu-id="66cf0-134">A mi esetünkben *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="66cf0-134">For our scenario, *192.168.2.0/24*.</span></span>
   * <span data-ttu-id="66cf0-135">**-t (vagy--tovább-hop-type)**.</span><span class="sxs-lookup"><span data-stu-id="66cf0-135">**-t (or --next-hop-type)**.</span></span> <span data-ttu-id="66cf0-136">Típusú objektum forgalom kapnak.</span><span class="sxs-lookup"><span data-stu-id="66cf0-136">Type of object traffic will be sent to.</span></span> <span data-ttu-id="66cf0-137">A lehetséges értékek: *VirtualAppliance*, *pedig*, *VNETLocal*, *Internet*, vagy *nincs*.</span><span class="sxs-lookup"><span data-stu-id="66cf0-137">Possible values are *VirtualAppliance*, *VirtualNetworkGateway*, *VNETLocal*, *Internet*, or *None*.</span></span>
   * <span data-ttu-id="66cf0-138">**-p (vagy--next-hop-ip-cím**).</span><span class="sxs-lookup"><span data-stu-id="66cf0-138">**-p (or --next-hop-ip-address**).</span></span> <span data-ttu-id="66cf0-139">Következő ugrás IP-címet.</span><span class="sxs-lookup"><span data-stu-id="66cf0-139">IP address for next hop.</span></span> <span data-ttu-id="66cf0-140">A mi esetünkben *192.168.0.4*.</span><span class="sxs-lookup"><span data-stu-id="66cf0-140">For our scenario, *192.168.0.4*.</span></span>
4. <span data-ttu-id="66cf0-141">Futtatási hello következő parancsot a tooassociate hello útvonaltábla hello létre **előtér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="66cf0-141">Run hello following command tooassociate hello route table created with hello **FrontEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n FrontEnd -r UDR-FrontEnd
    ```
   
    <span data-ttu-id="66cf0-142">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="66cf0-142">Output:</span></span>
   
        info:    Executing command network vnet subnet route-table add
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        info:    Associating route table "UDR-FrontEnd" and subnet "FrontEnd"
        info:    Looking up network gateway route tables in virtual network "TestVNet" subnet "FrontEnd"
        data:    Route table name                : UDR-FrontEnd
        data:      Location                      : West US
        data:      Routes:
        info:    network vnet subnet route-table add command OK    
   
    <span data-ttu-id="66cf0-143">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="66cf0-143">Parameters:</span></span>
   
   * <span data-ttu-id="66cf0-144">**-t (vagy--vnet-name)**.</span><span class="sxs-lookup"><span data-stu-id="66cf0-144">**-t (or --vnet-name)**.</span></span> <span data-ttu-id="66cf0-145">Hello ahol hello alhálózat VNet neve.</span><span class="sxs-lookup"><span data-stu-id="66cf0-145">Name of hello VNet where hello subnet is located.</span></span> <span data-ttu-id="66cf0-146">A mi esetünkben *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="66cf0-146">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="66cf0-147">**-n (vagy--alhálózatneve**.</span><span class="sxs-lookup"><span data-stu-id="66cf0-147">**-n (or --subnet-name**.</span></span> <span data-ttu-id="66cf0-148">Hozzáadandó hello alhálózati hello útvonaltábla neve.</span><span class="sxs-lookup"><span data-stu-id="66cf0-148">Name of hello subnet hello route table will be added to.</span></span> <span data-ttu-id="66cf0-149">A mi esetünkben *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="66cf0-149">For our scenario, *FrontEnd*.</span></span>

## <a name="create-hello-udr-for-hello-back-end-subnet"></a><span data-ttu-id="66cf0-150">Hello UDR hello háttér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="66cf0-150">Create hello UDR for hello back-end subnet</span></span>
<span data-ttu-id="66cf0-151">toocreate hello útválasztási táblázatot és hello háttér-alhálózat hello esetben teljes hello lépések alapján szükséges útvonal:</span><span class="sxs-lookup"><span data-stu-id="66cf0-151">toocreate hello route table and route needed for hello back-end subnet based on hello scenario, complete hello following steps:</span></span>

1. <span data-ttu-id="66cf0-152">Futtassa a következő parancs toocreate hello hello háttér-alhálózat egy útválasztási táblázatot:</span><span class="sxs-lookup"><span data-stu-id="66cf0-152">Run hello following command toocreate a route table for hello back-end subnet:</span></span>

    ```azurecli
    azure network route-table create -n UDR-BackEnd -l uswest
    ```

2. <span data-ttu-id="66cf0-153">Futtassa a következő parancs toocreate hello útvonal tábla toosend adott útvonal hello összes adatforgalmat toohello előtér-alhálózat (192.168.1.0/24) toohello **FW1** virtuális gép (192.168.0.4):</span><span class="sxs-lookup"><span data-stu-id="66cf0-153">Run hello following command toocreate a route in hello route table toosend all traffic destined toohello front-end subnet (192.168.1.0/24) toohello **FW1** VM (192.168.0.4):</span></span>

    ```azurecli
    azure network route-table route set -r UDR-BackEnd -n RouteToFrontEnd -a 192.168.1.0/24 -t VirtualAppliance -p 192.168.0.4
    ```

3. <span data-ttu-id="66cf0-154">Futtatási hello parancs tooassociate hello útvonaltáblában hello a következő **háttér** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="66cf0-154">Run hello following command tooassociate hello route table with hello **BackEnd** subnet:</span></span>

    ```azurecli
    azure network vnet subnet route-table add -t TestVNet -n BackEnd -r UDR-BackEnd
    ```

