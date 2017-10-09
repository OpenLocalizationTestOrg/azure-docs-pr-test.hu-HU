---
title: "hálózati biztonsági csoport – Azure CLI 2.0 aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és hálózati biztonsági csoportok használatával hello Azure CLI 2.0 telepítése."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9ea82c09-f4a6-4268-88bc-fc439db40c48
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 30b1d60676331bf5e2bbbb046c747477be9d3338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-hello-azure-cli-20"></a><span data-ttu-id="29161-103">Hálózati biztonsági csoportok használatával hello Azure CLI 2.0 létrehozása</span><span class="sxs-lookup"><span data-stu-id="29161-103">Create network security groups using hello Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="29161-104">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="29161-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="29161-105">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="29161-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="29161-106">[Az Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – hello klasszikus és resource management üzembe helyezési modellek számára a parancssori felület</span><span class="sxs-lookup"><span data-stu-id="29161-106">[Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – our CLI for hello classic and resource management deployment models</span></span> 
- <span data-ttu-id="29161-107">[Az Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell (Ez a cikk)</span><span class="sxs-lookup"><span data-stu-id="29161-107">[Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) - our next generation CLI for hello resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="29161-108">hello minta Azure CLI 2.0 parancsokat következő várt már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben.</span><span class="sxs-lookup"><span data-stu-id="29161-108">hello sample Azure CLI 2.0 commands following expect a simple environment already created based on hello scenario preceding.</span></span> 

## <a name="create-hello-nsg-for-hello-frontend-subnet"></a><span data-ttu-id="29161-109">Hozzon létre a hello NSG hello `FrontEnd` alhálózati</span><span class="sxs-lookup"><span data-stu-id="29161-109">Create hello NSG for hello `FrontEnd` subnet</span></span>

<span data-ttu-id="29161-110">az NSG nevű toocreate *NSG-előtér* alapján az előző hello forgatókönyv, hajtsa végre a hello lépéseket következő.</span><span class="sxs-lookup"><span data-stu-id="29161-110">toocreate an NSG named *NSG-FrontEnd* based on hello scenario preceding, follow hello steps following.</span></span>

1. <span data-ttu-id="29161-111">Ha még nem még telepít, és hello konfigurálása legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="29161-111">If you haven't yet, install and configure hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span> 

2. <span data-ttu-id="29161-112">Hozzon létre egy NSG-t használó hello [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="29161-112">Create an NSG using hello [az network nsg create](/cli/azure/network/nsg#create) command.</span></span> 

    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-FrontEnd \
    --location centralus 
    ```

    <span data-ttu-id="29161-113">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="29161-113">Parameters:</span></span>
   
   * <span data-ttu-id="29161-114">`--resource-group`: Hello NSG létrehozási helyének hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="29161-114">`--resource-group`: Name of hello resource group where hello NSG is created.</span></span> <span data-ttu-id="29161-115">A mi esetünkben *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="29161-115">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="29161-116">`--location`: Az azure-régió, ahol hello új NSG jön létre.</span><span class="sxs-lookup"><span data-stu-id="29161-116">`--location`: Azure region where hello new NSG is created.</span></span> <span data-ttu-id="29161-117">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="29161-117">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="29161-118">`--name`: Hello nevét új NSG.</span><span class="sxs-lookup"><span data-stu-id="29161-118">`--name`: Name for hello new NSG.</span></span> <span data-ttu-id="29161-119">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="29161-119">For our scenario, *NSG-FrontEnd*.</span></span>

    <span data-ttu-id="29161-120">hello várt kimeneti meglehetősen bit, többek között az összes hello alapértelmezett szabályok listáját.</span><span class="sxs-lookup"><span data-stu-id="29161-120">hello expected output is quite a bit of information including a list of all hello default rules.</span></span> <span data-ttu-id="29161-121">hello alábbi példa bemutatja hello alapértelmezett szabályok JMESPATH lekérdezés szűrő használata hello `table` kimeneti formátum:</span><span class="sxs-lookup"><span data-stu-id="29161-121">hello following example shows hello default rules using a JMESPATH query filter with hello `table` output format:</span></span>

    ```azurecli
    az network nsg show \
    -g testrg \
    -n nsg-frontend \
    --query 'defaultSecurityRules[].{Access:access,Desc:description,DestPortRange:destinationPortRange,Direction:direction,Priority:priority}' \
    -o table
    ```
   
   <span data-ttu-id="29161-122">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="29161-122">Output:</span></span>

        Access    Desc                                                    DestPortRange    Direction      Priority
        
        Allow     Allow inbound traffic from all VMs in VNET              *                Inbound           65000
        Allow     Allow inbound traffic from azure load balancer          *                Inbound           65001
        Deny      Deny all inbound traffic                                *                Inbound           65500
        Allow     Allow outbound traffic from all VMs tooall VMs in VNET  *                Outbound          65000
        Allow     Allow outbound traffic from all VMs tooInternet         *                Outbound          65001
        Deny      Deny all outbound traffic                               *                Outbound          65500



3. <span data-ttu-id="29161-123">Hozzon létre egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 3389-es (RDP) a hello Internet a hello [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="29161-123">Create a rule that allows access tooport 3389 (RDP) from hello Internet with hello [az network nsg rule create](/cli/azure/network/nsg/rule#create) command.</span></span>

    > [!NOTE]
    > <span data-ttu-id="29161-124">Attól függően, hogy hello felületet használ, szükség lehet a toomodify hello `*` karakter hello argumentumok nem tooexpand hello argumentum végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="29161-124">Depending on hello shell you are using, you might need toomodify hello `*` character in hello arguments following so as not tooexpand hello argument before execution.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name rdp-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 3389
    ```
   
    <span data-ttu-id="29161-125">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="29161-125">Expected output:</span></span>
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "3389",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp-rule",
        "name": "rdp-rule",
        "priority": 100,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

    <span data-ttu-id="29161-126">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="29161-126">Parameters:</span></span>

    * <span data-ttu-id="29161-127">`--resource-group testrg`: hello erőforrás csoport toouse.</span><span class="sxs-lookup"><span data-stu-id="29161-127">`--resource-group testrg`: hello resource group toouse.</span></span> <span data-ttu-id="29161-128">Vegye figyelembe, hogy a rendszer azonban nem.</span><span class="sxs-lookup"><span data-stu-id="29161-128">Note that it is case-insensitive.</span></span>
    * <span data-ttu-id="29161-129">`--nsg-name NSG-FrontEnd`: A mely hello-szabály jön létre hello NSG neve.</span><span class="sxs-lookup"><span data-stu-id="29161-129">`--nsg-name NSG-FrontEnd`: Name of hello NSG in which hello rule is created.</span></span>
    * <span data-ttu-id="29161-130">`--name rdp-rule`: Hello új szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="29161-130">`--name rdp-rule`: Name for hello new rule.</span></span>
    * <span data-ttu-id="29161-131">`--access Allow`: A hozzáférési szint hello szabály (Megtagadás vagy engedélyezés).</span><span class="sxs-lookup"><span data-stu-id="29161-131">`--access Allow`: Access level for hello rule (Deny or Allow).</span></span>
    * <span data-ttu-id="29161-132">`--protocol Tcp`: A protokoll (Tcp, Udp vagy *).</span><span class="sxs-lookup"><span data-stu-id="29161-132">`--protocol Tcp`: Protocol (Tcp, Udp, or *).</span></span>
    * <span data-ttu-id="29161-133">`--direction Inbound`: Irányát (bejövő vagy kimenő) hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="29161-133">`--direction Inbound`: Direction of hello connection (Inbound or Outbound).</span></span>
    * <span data-ttu-id="29161-134">`--priority 100`: Hello szabály prioritását.</span><span class="sxs-lookup"><span data-stu-id="29161-134">`--priority 100`: Priority for hello rule.</span></span>
    * <span data-ttu-id="29161-135">`--source-address-prefix Internet`: Forrás címelőtagot CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="29161-135">`--source-address-prefix Internet`: Source address prefix in CIDR or using default tags.</span></span>
    * <span data-ttu-id="29161-136">`--source-port-range "*"`: A portot vagy porttartományt forrás.</span><span class="sxs-lookup"><span data-stu-id="29161-136">`--source-port-range "*"`: Source port or port range.</span></span> <span data-ttu-id="29161-137">Hello kapcsolatot megnyitó port.</span><span class="sxs-lookup"><span data-stu-id="29161-137">Port that opened hello connection.</span></span>
    * <span data-ttu-id="29161-138">`--destination-address-prefix "*"`: Cél címelőtagot CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="29161-138">`--destination-address-prefix "*"`: Destination address prefix in CIDR or using default tags.</span></span>
    * <span data-ttu-id="29161-139">`--destination-port-range 3389`: A célport vagy porttartomány.</span><span class="sxs-lookup"><span data-stu-id="29161-139">`--destination-port-range 3389`: Destination port or port range.</span></span> <span data-ttu-id="29161-140">Az port, amely megkapja a hello kapcsolódási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="29161-140">Port that receives hello connection request.</span></span>



4. <span data-ttu-id="29161-141">Hozzon létre egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 80-as (HTTP) az hello Internet **az hálózati nsg-szabály létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="29161-141">Create a rule that allows access tooport 80 (HTTP) from hello Internet **az network nsg rule create** command.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-FrontEnd \
    --name web-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 200 \
    --source-address-prefix Internet \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 80
    ```
   
    <span data-ttu-id="29161-142">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="29161-142">Expected putput:</span></span>
   
    ```json
    {
        "access": "Allow",
        "description": null,
        "destinationAddressPrefix": "*",
        "destinationPortRange": "80",
        "direction": "Inbound",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule",
        "name": "web-rule",
        "priority": 200,
        "protocol": "Tcp",
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "sourceAddressPrefix": "Internet",
        "sourcePortRange": "*"
    }
    ```

5. <span data-ttu-id="29161-143">Kötési hello NSG toohello **előtér** hello alhálózat [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update) parancsot.</span><span class="sxs-lookup"><span data-stu-id="29161-143">Bind hello NSG toohello **FrontEnd** subnet with hello [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) command.</span></span>
        
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name FrontEnd \
    --resource-group testrg \
    --network-security-group NSG-FrontEnd
    ```
   
    <span data-ttu-id="29161-144">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="29161-144">Expected output:</span></span>
   
    ```json
    {
        "addressPrefix": "192.168.1.0/24",
        "etag": "W/\"<guid>\"",
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/FrontEnd",
        "ipConfigurations": [
            {
            "etag": null,
            "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNIC/ipConfigurations/ipconfig1",
            "name": null,
            "privateIpAddress": null,
            "privateIpAllocationMethod": null,
            "provisioningState": null,
            "publicIpAddress": null,
            "resourceGroup": "TestRG",
            "subnet": null
            }
        ],
        "name": "FrontEnd",
        "networkSecurityGroup": {
            "defaultSecurityRules": null,
            "etag": null,
            "id": "/subscriptions/<guid>f/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd",
            "location": null,
            "name": null,
            "networkInterfaces": null,
            "provisioningState": null,
            "resourceGroup": "testrg",
            "resourceGuid": null,
            "securityRules": null,
            "subnets": null,
            "tags": null,
            "type": null
        },
        "provisioningState": "Succeeded",
        "resourceGroup": "testrg",
        "resourceNavigationLinks": null,
        "routeTable": null
    }
    ```

## <a name="create-hello-nsg-for-hello-backend-subnet"></a><span data-ttu-id="29161-145">Hozzon létre a hello NSG hello `BackEnd` alhálózati</span><span class="sxs-lookup"><span data-stu-id="29161-145">Create hello NSG for hello `BackEnd` subnet</span></span>
<span data-ttu-id="29161-146">az NSG nevű toocreate *NSG-háttérrendszer* alapján az előző hello forgatókönyv, hajtsa végre a hello lépéseket következő.</span><span class="sxs-lookup"><span data-stu-id="29161-146">toocreate an NSG named *NSG-BackEnd* based on hello scenario preceding, follow hello steps following.</span></span>

1. <span data-ttu-id="29161-147">Hozzon létre hello `NSG-BackEnd` az NSG **az hálózati nsg létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="29161-147">Create hello `NSG-BackEnd` NSG with **az network nsg create**.</span></span>
   
    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-BackEnd \
    --location centralus
    ```
   
    <span data-ttu-id="29161-148">Ahogy az előző, 2. lépés hello várható kimenete túl nagy, beleértve az alapértelmezett szabályokat.</span><span class="sxs-lookup"><span data-stu-id="29161-148">As in step 2, preceding, hello expected output is quite large, including default rules.</span></span>
   
2. <span data-ttu-id="29161-149">Hozzon létre egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 1433-as port (SQL) a hello `FrontEnd` hello alhálózat **az hálózati nsg-szabály létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="29161-149">Create a rule that allows access tooport 1433 (SQL) from hello `FrontEnd` subnet with hello **az network nsg rule create** command.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name sql-rule \
    --access Allow \
    --protocol Tcp \
    --direction Inbound \
    --priority 100 \
    --source-address-prefix 192.168.1.0/24 \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range 1433
    ```
   
    <span data-ttu-id="29161-150">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="29161-150">Expected output:</span></span>

    ```json  
    {
    "access": "Allow",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "1433",
    "direction": "Inbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule",
    "name": "sql-rule",
    "priority": 100,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "192.168.1.0/24",
    "sourcePortRange": "*"
    }
    ```

3. <span data-ttu-id="29161-151">Hozzon létre egy szabályt, amely megtagadja a hozzáférést toohello Internet használatával hello **az hálózati nsg-szabály létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="29161-151">Create a rule that denies access toohello Internet using hello **az network nsg rule create** command.</span></span>
   
    ```azurecli
    az network nsg rule create \
    --resource-group testrg \
    --nsg-name NSG-BackEnd \
    --name web-rule \
    --access Deny \
    --protocol Tcp  \
    --direction Outbound  \
    --priority 200 \
    --source-address-prefix "*" \
    --source-port-range "*" \
    --destination-address-prefix "*" \
    --destination-port-range "*"
    ```
   
    <span data-ttu-id="29161-152">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="29161-152">Expected putput:</span></span>
   
    ```json
    {
    "access": "Deny",
    "description": null,
    "destinationAddressPrefix": "*",
    "destinationPortRange": "*",
    "direction": "Outbound",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd/securityRules/web-rule",
    "name": "web-rule",
    "priority": 200,
    "protocol": "Tcp",
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "sourceAddressPrefix": "*",
    "sourcePortRange": "*"
    }
    ```

4. <span data-ttu-id="29161-153">Kötési hello NSG toohello `BackEnd` hello alhálózatot **az hálózati vnet alhálózati set** parancsot.</span><span class="sxs-lookup"><span data-stu-id="29161-153">Bind hello NSG toohello `BackEnd` subnet using hello **az network vnet subnet set** command.</span></span>
   
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name BackEnd \
    --resource-group testrg \
    --network-security-group NSG-BackEnd
    ```
   
    <span data-ttu-id="29161-154">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="29161-154">Expected output:</span></span>
   
    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid>\"",
    "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/virtualNetworks/TestVNET/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": {
        "defaultSecurityRules": null,
        "etag": null,
        "id": "/subscriptions/<guid>/resourceGroups/testrg/providers/Microsoft.Network/networkSecurityGroups/NSG-BackEnd",
        "location": null,
        "name": null,
        "networkInterfaces": null,
        "provisioningState": null,
        "resourceGroup": "testrg",
        "resourceGuid": null,
        "securityRules": null,
        "subnets": null,
        "tags": null,
        "type": null
    },
    "provisioningState": "Succeeded",
    "resourceGroup": "testrg",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```
