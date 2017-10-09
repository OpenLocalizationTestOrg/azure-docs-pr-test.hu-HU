---
title: "a virtuális hálózat használatával aaaCreate hello Azure CLI 1.0 |} Microsoft Docs"
description: "Ismerje meg, hogyan egy virtuális hálózat használatával toocreate hello Azure CLI 1.0 |} Erőforrás-kezelő."
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.openlocfilehash: f48a8b23a5994164b71c9b51ee8a6810d17f9392
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-hello-azure-cli"></a><span data-ttu-id="36f08-103">Hozzon létre egy virtuális hálózatot hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="36f08-103">Create a virtual network using hello Azure CLI</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="36f08-104">Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="36f08-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="36f08-105">A Microsoft azt javasolja, hello Resource Manager üzembe helyezési modellben erőforrásoknak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="36f08-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="36f08-106">hello arról toolearn hello hello két modellek közötti különbséget olvasási [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="36f08-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="36f08-107">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="36f08-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="36f08-108">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="36f08-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="36f08-109">[Az Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="36f08-109">[Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md) - our next generation CLI for hello resource management deployment model</span></span>
- <span data-ttu-id="36f08-110">[Az Azure CLI 1.0](#create-a-virtual-network) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="36f08-110">[Azure CLI 1.0](#create-a-virtual-network) – our CLI for hello classic and resource management deployment models (this article)</span></span>

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a><span data-ttu-id="36f08-111">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="36f08-111">Create a virtual network</span></span>

<span data-ttu-id="36f08-112">a virtuális hálózat használatával toocreate hello Azure CLI, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="36f08-112">toocreate a virtual network using hello Azure CLI, complete hello following steps:</span></span>

1. <span data-ttu-id="36f08-113">Telepíteni és konfigurálni az Azure parancssori felület következő hello által lépések hello hello [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="36f08-113">Install and configure hello Azure CLI by following hello steps in hello [Install and Configure hello Azure CLI](../cli-install-nodejs.md) article.</span></span>

2. <span data-ttu-id="36f08-114">Egy VNet és alhálózat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="36f08-114">Create a VNet and a subnet:</span></span>

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    <span data-ttu-id="36f08-115">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="36f08-115">Expected output:</span></span>
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    <span data-ttu-id="36f08-116">Használt paraméterek:</span><span class="sxs-lookup"><span data-stu-id="36f08-116">Parameters used:</span></span>

   * <span data-ttu-id="36f08-117">**--vnet**.</span><span class="sxs-lookup"><span data-stu-id="36f08-117">**--vnet**.</span></span> <span data-ttu-id="36f08-118">Hello VNet toobe létrehozott neve.</span><span class="sxs-lookup"><span data-stu-id="36f08-118">Name of hello VNet toobe created.</span></span> <span data-ttu-id="36f08-119">A mi esetünkben *TestVNet*</span><span class="sxs-lookup"><span data-stu-id="36f08-119">For our scenario, *TestVNet*</span></span>
   * <span data-ttu-id="36f08-120">**-e (vagy--címtartomány)**.</span><span class="sxs-lookup"><span data-stu-id="36f08-120">**-e (or --address-space)**.</span></span> <span data-ttu-id="36f08-121">Virtuális hálózat címterület.</span><span class="sxs-lookup"><span data-stu-id="36f08-121">VNet address space.</span></span> <span data-ttu-id="36f08-122">A mi esetünkben *192.168.0.0*</span><span class="sxs-lookup"><span data-stu-id="36f08-122">For our scenario, *192.168.0.0*</span></span>
   * <span data-ttu-id="36f08-123">**-i (vagy - cidr)**.</span><span class="sxs-lookup"><span data-stu-id="36f08-123">**-i (or -cidr)**.</span></span> <span data-ttu-id="36f08-124">Hálózati maszk CIDR formátumban.</span><span class="sxs-lookup"><span data-stu-id="36f08-124">Network mask in CIDR format.</span></span> <span data-ttu-id="36f08-125">A mi esetünkben *16*.</span><span class="sxs-lookup"><span data-stu-id="36f08-125">For our scenario, *16*.</span></span>
   * <span data-ttu-id="36f08-126">**-n (vagy--alhálózatneve**).</span><span class="sxs-lookup"><span data-stu-id="36f08-126">**-n (or --subnet-name**).</span></span> <span data-ttu-id="36f08-127">Hello első alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="36f08-127">Name of hello first subnet.</span></span> <span data-ttu-id="36f08-128">A mi esetünkben *FrontEnd*.</span><span class="sxs-lookup"><span data-stu-id="36f08-128">For our scenario, *FrontEnd*.</span></span>
   * <span data-ttu-id="36f08-129">**-p (vagy--ip-alhálózat-start)**.</span><span class="sxs-lookup"><span data-stu-id="36f08-129">**-p (or --subnet-start-ip)**.</span></span> <span data-ttu-id="36f08-130">Kezdő IP-cím alhálózati vagy alhálózati címtartományt.</span><span class="sxs-lookup"><span data-stu-id="36f08-130">Starting IP address for subnet, or subnet address space.</span></span> <span data-ttu-id="36f08-131">A mi esetünkben *192.168.1.0*.</span><span class="sxs-lookup"><span data-stu-id="36f08-131">For our scenario, *192.168.1.0*.</span></span>
   * <span data-ttu-id="36f08-132">**-r (vagy--alhálózat-cidr)**.</span><span class="sxs-lookup"><span data-stu-id="36f08-132">**-r (or --subnet-cidr)**.</span></span> <span data-ttu-id="36f08-133">Hálózati maszk alhálózat CIDR formátumban.</span><span class="sxs-lookup"><span data-stu-id="36f08-133">Network mask in CIDR format for subnet.</span></span> <span data-ttu-id="36f08-134">A mi esetünkben *24*.</span><span class="sxs-lookup"><span data-stu-id="36f08-134">For our scenario, *24*.</span></span>
   * <span data-ttu-id="36f08-135">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="36f08-135">**-l (or --location)**.</span></span> <span data-ttu-id="36f08-136">Azure-régió, ahol a hello VNet jön létre.</span><span class="sxs-lookup"><span data-stu-id="36f08-136">Azure region where hello VNet is created.</span></span> <span data-ttu-id="36f08-137">A mi esetünkben *USA középső RÉGIÓJA*.</span><span class="sxs-lookup"><span data-stu-id="36f08-137">For our scenario, *Central US*.</span></span>

3. <span data-ttu-id="36f08-138">Hozzon létre egy alhálózatot:</span><span class="sxs-lookup"><span data-stu-id="36f08-138">Create a subnet:</span></span>

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    <span data-ttu-id="36f08-139">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="36f08-139">Expected output:</span></span>

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up hello subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    <span data-ttu-id="36f08-140">Használt paraméterek:</span><span class="sxs-lookup"><span data-stu-id="36f08-140">Parameters used:</span></span>

   * <span data-ttu-id="36f08-141">**-t (vagy--vnet-name**.</span><span class="sxs-lookup"><span data-stu-id="36f08-141">**-t (or --vnet-name**.</span></span> <span data-ttu-id="36f08-142">Hello ahol hello alhálózati létrejön a VNet neve.</span><span class="sxs-lookup"><span data-stu-id="36f08-142">Name of hello VNet where hello subnet will be created.</span></span> <span data-ttu-id="36f08-143">A mi esetünkben *TestVNet*.</span><span class="sxs-lookup"><span data-stu-id="36f08-143">For our scenario, *TestVNet*.</span></span>
   * <span data-ttu-id="36f08-144">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="36f08-144">**-n (or --name)**.</span></span> <span data-ttu-id="36f08-145">Hello új alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="36f08-145">Name of hello new subnet.</span></span> <span data-ttu-id="36f08-146">A mi esetünkben *háttér*.</span><span class="sxs-lookup"><span data-stu-id="36f08-146">For our scenario, *BackEnd*.</span></span>
   * <span data-ttu-id="36f08-147">**-a (vagy --address-prefix)**.</span><span class="sxs-lookup"><span data-stu-id="36f08-147">**-a (or --address-prefix)**.</span></span> <span data-ttu-id="36f08-148">Az alhálózat CIDR-blokkja.</span><span class="sxs-lookup"><span data-stu-id="36f08-148">Subnet CIDR block.</span></span> <span data-ttu-id="36f08-149">Négy esetünkben *192.168.2.0/24*.</span><span class="sxs-lookup"><span data-stu-id="36f08-149">Four our scenario, *192.168.2.0/24*.</span></span>
   
4. <span data-ttu-id="36f08-150">tooview hello tulajdonságainak hello új virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="36f08-150">tooview hello properties of hello new VNet:</span></span>

    ```azurecli
    azure network vnet show
    ```
   
    <span data-ttu-id="36f08-151">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="36f08-151">Expected output:</span></span>
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up hello virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

## <a name="next-steps"></a><span data-ttu-id="36f08-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="36f08-152">Next steps</span></span>

<span data-ttu-id="36f08-153">Megtudhatja, hogyan tooconnect:</span><span class="sxs-lookup"><span data-stu-id="36f08-153">Learn how tooconnect:</span></span>

- <span data-ttu-id="36f08-154">A virtuális gép (VM) tooa virtuális hálózati hello olvasásával [hozzon létre egy Linux virtuális Gépet](../virtual-machines/linux/quick-create-cli.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="36f08-154">A virtual machine (VM) tooa virtual network by reading hello [Create a Linux VM](../virtual-machines/linux/quick-create-cli.md) article.</span></span> <span data-ttu-id="36f08-155">Egy VNet és alhálózat létrehozása hello cikkek hello lépésekben, helyett választhatja egy meglévő VNet és alhálózat tooconnect egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="36f08-155">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="36f08-156">virtuális hálózat tooother virtuális hálózatok hello hello olvasásával [csatlakozás Vnetek](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="36f08-156">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) article.</span></span>
- <span data-ttu-id="36f08-157">hello virtuális hálózati tooan a helyi hálózati helyek virtuális magánhálózati (VPN) vagy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="36f08-157">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="36f08-158">Megtudhatja, hogyan hello olvasásával [VNet tooan a helyi hálózat a telephelyek közötti VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) és [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="36f08-158">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span></span>
