---
title: "a virtuális hálózati - Azure CLI 2.0 aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan egy virtuális hálózat használatával toocreate hello Azure CLI 2.0."
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e79b7fe780fc81f4866f810d830824e43a5a43b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-using-hello-azure-cli-20"></a><span data-ttu-id="3dff4-103">Hello Azure CLI 2.0 virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="3dff4-103">Create a virtual network using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

<span data-ttu-id="3dff4-104">Az Azure két üzemi modellel rendelkezik, az Azure Resource Managerrel és a klasszikussal.</span><span class="sxs-lookup"><span data-stu-id="3dff4-104">Azure has two deployment models: Azure Resource Manager and classic.</span></span> <span data-ttu-id="3dff4-105">A Microsoft azt javasolja, hello Resource Manager üzembe helyezési modellben erőforrásoknak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3dff4-105">Microsoft recommends creating resources through hello Resource Manager deployment model.</span></span> <span data-ttu-id="3dff4-106">hello arról toolearn hello hello két modellek közötti különbséget olvasási [megértéséhez Azure üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="3dff4-106">toolearn more about hello differences between hello two models, read hello [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) article.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="3dff4-107">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="3dff4-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="3dff4-108">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="3dff4-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="3dff4-109">[Az Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="3dff4-109">[Azure CLI 1.0](virtual-networks-create-vnet-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span>
- <span data-ttu-id="3dff4-110">[Az Azure CLI 2.0](#create-a-virtual-network) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk) "</span><span class="sxs-lookup"><span data-stu-id="3dff4-110">[Azure CLI 2.0](#create-a-virtual-network) - our next generation CLI for hello resource management deployment model (this article)\`</span></span>
 
    <span data-ttu-id="3dff4-111">Hozzon létre egy Vnetet Resource Manageren keresztül más eszközök használatával vagy hello klasszikus telepítési modell használatával VNet létrehozása a következő lista hello egy másik lehetőség kiválasztásával is:</span><span class="sxs-lookup"><span data-stu-id="3dff4-111">You can also create a VNet through Resource Manager using other tools or create a VNet through hello classic deployment model by selecting a different option from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="3dff4-112">Portál</span><span class="sxs-lookup"><span data-stu-id="3dff4-112">Portal</span></span>](virtual-networks-create-vnet-arm-pportal.md)
> * [<span data-ttu-id="3dff4-113">PowerShell</span><span class="sxs-lookup"><span data-stu-id="3dff4-113">PowerShell</span></span>](virtual-networks-create-vnet-arm-ps.md)
> * [<span data-ttu-id="3dff4-114">Parancssori felület</span><span class="sxs-lookup"><span data-stu-id="3dff4-114">CLI</span></span>](virtual-networks-create-vnet-arm-cli.md)
> * [<span data-ttu-id="3dff4-115">Sablon</span><span class="sxs-lookup"><span data-stu-id="3dff4-115">Template</span></span>](virtual-networks-create-vnet-arm-template-click.md)
> * [<span data-ttu-id="3dff4-116">Portál (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="3dff4-116">Portal (Classic)</span></span>](virtual-networks-create-vnet-classic-pportal.md)
> * [<span data-ttu-id="3dff4-117">PowerShell (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="3dff4-117">PowerShell (Classic)</span></span>](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [<span data-ttu-id="3dff4-118">Parancssori felület (klasszikus)</span><span class="sxs-lookup"><span data-stu-id="3dff4-118">CLI (Classic)</span></span>](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]


## <a name="create-a-virtual-network"></a><span data-ttu-id="3dff4-119">Virtuális hálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="3dff4-119">Create a virtual network</span></span>

<span data-ttu-id="3dff4-120">a virtuális hálózat használatával toocreate hello Azure CLI 2.0-s teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="3dff4-120">toocreate a virtual network using hello Azure CLI 2.0, complete hello following steps:</span></span>

1. <span data-ttu-id="3dff4-121">Telepítse és konfigurálja a hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="3dff4-121">Install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

2. <span data-ttu-id="3dff4-122">Hozzon létre egy erőforráscsoportot a vnet hello segítségével [az csoport létrehozása](/cli/azure/group#create) hello parancsot `--name` és `--location` argumentumai:</span><span class="sxs-lookup"><span data-stu-id="3dff4-122">Create a resource group for your VNet using hello [az group create](/cli/azure/group#create) command with hello `--name` and `--location` arguments:</span></span>

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. <span data-ttu-id="3dff4-123">Egy VNet és alhálózat létrehozása:</span><span class="sxs-lookup"><span data-stu-id="3dff4-123">Create a VNet and a subnet:</span></span>

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    <span data-ttu-id="3dff4-124">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="3dff4-124">Expected output:</span></span>
    
    ```json
    {
        "newVNet": {
            "addressSpace": {
            "addressPrefixes": [
            "192.168.0.0/16"
            ]
            },
            "dhcpOptions": {
            "dnsServers": []
            },
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>",
            "subnets": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                "name": "FrontEnd",
                "properties": {
                "addressPrefix": "192.168.1.0/24",
                "provisioningState": "Succeeded"
                },
                "resourceGroup": "TestRG"
            }
            ]
            }
    }
    ```

    <span data-ttu-id="3dff4-125">Használt paraméterek:</span><span class="sxs-lookup"><span data-stu-id="3dff4-125">Parameters used:</span></span>

    - <span data-ttu-id="3dff4-126">`--name TestVNet`: Hello VNet toobe létrehozott neve.</span><span class="sxs-lookup"><span data-stu-id="3dff4-126">`--name TestVNet`: Name of hello VNet toobe created.</span></span>
    - <span data-ttu-id="3dff4-127">`--resource-group TestRG`: # hello az erőforráscsoport neve, amely a hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3dff4-127">`--resource-group TestRG`: # hello resource group name that controls hello resource.</span></span> 
    - <span data-ttu-id="3dff4-128">`--location centralus`: hello mely toodeploy a helyen.</span><span class="sxs-lookup"><span data-stu-id="3dff4-128">`--location centralus`: hello location into which toodeploy.</span></span>
    - <span data-ttu-id="3dff4-129">`--address-prefix 192.168.0.0/16`: hello címelőtag, és letiltja.</span><span class="sxs-lookup"><span data-stu-id="3dff4-129">`--address-prefix 192.168.0.0/16`: hello address prefix and block.</span></span>  
    - <span data-ttu-id="3dff4-130">`--subnet-name FrontEnd`: hello alhálózati hello nevét.</span><span class="sxs-lookup"><span data-stu-id="3dff4-130">`--subnet-name FrontEnd`: hello name of hello subnet.</span></span>
    - <span data-ttu-id="3dff4-131">`--subnet-prefix 192.168.1.0/24`: hello címelőtag, és letiltja.</span><span class="sxs-lookup"><span data-stu-id="3dff4-131">`--subnet-prefix 192.168.1.0/24`: hello address prefix and block.</span></span>

    <span data-ttu-id="3dff4-132">toolist hello alapvető információkat toouse hello a következő parancsot, hello VNet segítségével lekérheti egy [lekérdezésszűrő](/cli/azure/query-az-cli2):</span><span class="sxs-lookup"><span data-stu-id="3dff4-132">toolist hello basic information toouse in hello next command, you can query hello VNet using a [query filter](/cli/azure/query-az-cli2):</span></span>

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    <span data-ttu-id="3dff4-133">A következő kimeneti hello állít elő:</span><span class="sxs-lookup"><span data-stu-id="3dff4-133">Which produces hello following output:</span></span>

        Where      Name      Group

        centralus  TestVNet  TestRG

4. <span data-ttu-id="3dff4-134">Hozzon létre egy alhálózatot:</span><span class="sxs-lookup"><span data-stu-id="3dff4-134">Create a subnet:</span></span>

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    <span data-ttu-id="3dff4-135">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="3dff4-135">Expected output:</span></span>

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid> \"",
    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "TestRG",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

    <span data-ttu-id="3dff4-136">Használt paraméterek:</span><span class="sxs-lookup"><span data-stu-id="3dff4-136">Parameters used:</span></span>

    - <span data-ttu-id="3dff4-137">`--address-prefix 192.168.2.0/24`: Alhálózat CIDR-blokkja.</span><span class="sxs-lookup"><span data-stu-id="3dff4-137">`--address-prefix 192.168.2.0/24`: Subnet CIDR block.</span></span>
    - <span data-ttu-id="3dff4-138">`--name BackEnd`: Hello új alhálózat neve.</span><span class="sxs-lookup"><span data-stu-id="3dff4-138">`--name BackEnd`: Name of hello new subnet.</span></span>
    - <span data-ttu-id="3dff4-139">`--resource-group TestRG`: hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="3dff4-139">`--resource-group TestRG`: hello resource group.</span></span>
    - <span data-ttu-id="3dff4-140">`--vnet-name TestVNet`: a tulajdonos VNet hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="3dff4-140">`--vnet-name TestVNet`: hello name of hello owning VNet.</span></span>

5. <span data-ttu-id="3dff4-141">Lekérdezés hello tulajdonságainak hello új virtuális hálózat:</span><span class="sxs-lookup"><span data-stu-id="3dff4-141">Query hello properties of hello new VNet:</span></span>

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    <span data-ttu-id="3dff4-142">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="3dff4-142">Expected output:</span></span>

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. <span data-ttu-id="3dff4-143">Lekérdezés hello tulajdonságok hello alhálózatok:</span><span class="sxs-lookup"><span data-stu-id="3dff4-143">Query hello properties of hello subnets:</span></span>

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    <span data-ttu-id="3dff4-144">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="3dff4-144">Expected output:</span></span>

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a><span data-ttu-id="3dff4-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3dff4-145">Next steps</span></span>

<span data-ttu-id="3dff4-146">Megtudhatja, hogyan tooconnect:</span><span class="sxs-lookup"><span data-stu-id="3dff4-146">Learn how tooconnect:</span></span>

- <span data-ttu-id="3dff4-147">A virtuális gép (VM) tooa virtuális hálózati hello olvasásával [hozzon létre egy Linux virtuális Gépet](../virtual-machines/linux/quick-create-cli.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="3dff4-147">A virtual machine (VM) tooa virtual network by reading hello [Create a Linux VM](../virtual-machines/linux/quick-create-cli.md) article.</span></span> <span data-ttu-id="3dff4-148">Egy VNet és alhálózat létrehozása hello cikkek hello lépésekben, helyett választhatja egy meglévő VNet és alhálózat tooconnect egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="3dff4-148">Instead of creating a VNet and subnet in hello steps of hello articles, you can select an existing VNet and subnet tooconnect a VM to.</span></span>
- <span data-ttu-id="3dff4-149">virtuális hálózat tooother virtuális hálózatok hello hello olvasásával [csatlakozás Vnetek](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="3dff4-149">hello virtual network tooother virtual networks by reading hello [Connect VNets](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) article.</span></span>
- <span data-ttu-id="3dff4-150">hello virtuális hálózati tooan a helyi hálózati helyek virtuális magánhálózati (VPN) vagy ExpressRoute-kapcsolatcsoportot.</span><span class="sxs-lookup"><span data-stu-id="3dff4-150">hello virtual network tooan on-premises network using a site-to-site virtual private network (VPN) or ExpressRoute circuit.</span></span> <span data-ttu-id="3dff4-151">Megtudhatja, hogyan hello olvasásával [VNet tooan a helyi hálózat a telephelyek közötti VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) és [csatolni a virtuális hálózat tooan ExpressRoute-kapcsolatcsoportot](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="3dff4-151">Learn how by reading hello [Connect a VNet tooan on-premises network using a site-to-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) and [Link a VNet tooan ExpressRoute circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).</span></span>
