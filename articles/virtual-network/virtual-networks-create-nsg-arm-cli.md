---
title: "Hozzon létre a hálózati biztonsági csoport – Azure CLI 2.0 |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre és telepíthet a hálózati biztonsági csoportok az Azure CLI 2.0 használatával."
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
ms.openlocfilehash: 8efb3ab66d07875b51f723fed5594bcb477ed025
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-network-security-groups-using-the-azure-cli-20"></a><span data-ttu-id="322fa-103">Hálózati biztonsági csoportok használata az Azure CLI 2.0 létrehozása</span><span class="sxs-lookup"><span data-stu-id="322fa-103">Create network security groups using the Azure CLI 2.0</span></span>

[!INCLUDE [virtual-networks-create-nsg-selectors-arm-include](../../includes/virtual-networks-create-nsg-selectors-arm-include.md)]

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="322fa-104">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="322fa-104">CLI versions to complete the task</span></span> 

<span data-ttu-id="322fa-105">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="322fa-105">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="322fa-106">[Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – parancssori felületünk a klasszikus és a Resource Management üzemi modellekhez</span><span class="sxs-lookup"><span data-stu-id="322fa-106">[Azure CLI 1.0](virtual-networks-create-nsg-cli-nodejs.md) – our CLI for the classic and resource management deployment models</span></span> 
- <span data-ttu-id="322fa-107">[Az Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) -erőforrás felügyeleti telepítési modell (Ez a cikk) a következő generációs parancssori felület</span><span class="sxs-lookup"><span data-stu-id="322fa-107">[Azure CLI 2.0](#Create-the-nsg-for-the-front-end-subnet) - our next generation CLI for the resource management deployment model (this article)</span></span>

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="322fa-108">A következő Példaparancsok Azure CLI 2.0 már a fenti forgatókönyv alapján létre egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="322fa-108">The sample Azure CLI 2.0 commands following expect a simple environment already created based on the scenario preceding.</span></span> 

## <a name="create-the-nsg-for-the-frontend-subnet"></a><span data-ttu-id="322fa-109">Az NSG létrehozása a `FrontEnd` alhálózati</span><span class="sxs-lookup"><span data-stu-id="322fa-109">Create the NSG for the `FrontEnd` subnet</span></span>

<span data-ttu-id="322fa-110">Az NSG nevű létrehozásához *NSG-előtérbeli* a fenti forgatókönyv alapján, kövesse a lépéseket következő.</span><span class="sxs-lookup"><span data-stu-id="322fa-110">To create an NSG named *NSG-FrontEnd* based on the scenario preceding, follow the steps following.</span></span>

1. <span data-ttu-id="322fa-111">Ha még nem még konfigurál, a legutóbbi [Azure CLI 2.0](/cli/azure/install-az-cli2) és való bejelentkezéshez az Azure fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="322fa-111">If you haven't yet, install and configure the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span> 

2. <span data-ttu-id="322fa-112">Hozzon létre egy NSG-t használ a [az hálózati nsg létrehozása](/cli/azure/network/nsg#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="322fa-112">Create an NSG using the [az network nsg create](/cli/azure/network/nsg#create) command.</span></span> 

    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-FrontEnd \
    --location centralus 
    ```

    <span data-ttu-id="322fa-113">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="322fa-113">Parameters:</span></span>
   
   * <span data-ttu-id="322fa-114">`--resource-group`: Az erőforráscsoport, ahol létrehozzák az NSG neve.</span><span class="sxs-lookup"><span data-stu-id="322fa-114">`--resource-group`: Name of the resource group where the NSG is created.</span></span> <span data-ttu-id="322fa-115">A mi esetünkben *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="322fa-115">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="322fa-116">`--location`: Az azure-régió, ahol létrejön az új NSG.</span><span class="sxs-lookup"><span data-stu-id="322fa-116">`--location`: Azure region where the new NSG is created.</span></span> <span data-ttu-id="322fa-117">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="322fa-117">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="322fa-118">`--name`: Az új NSG neve.</span><span class="sxs-lookup"><span data-stu-id="322fa-118">`--name`: Name for the new NSG.</span></span> <span data-ttu-id="322fa-119">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="322fa-119">For our scenario, *NSG-FrontEnd*.</span></span>

    <span data-ttu-id="322fa-120">A várható kimenete meglehetősen bit, többek között az alapértelmezett szabályok listáját.</span><span class="sxs-lookup"><span data-stu-id="322fa-120">The expected output is quite a bit of information including a list of all the default rules.</span></span> <span data-ttu-id="322fa-121">A következő példa bemutatja az alapértelmezett szabályok használatával egy JMESPATH lekérdezés szűrő a `table` kimeneti formátum:</span><span class="sxs-lookup"><span data-stu-id="322fa-121">The following example shows the default rules using a JMESPATH query filter with the `table` output format:</span></span>

    ```azurecli
    az network nsg show \
    -g testrg \
    -n nsg-frontend \
    --query 'defaultSecurityRules[].{Access:access,Desc:description,DestPortRange:destinationPortRange,Direction:direction,Priority:priority}' \
    -o table
    ```
   
   <span data-ttu-id="322fa-122">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="322fa-122">Output:</span></span>

        Access    Desc                                                    DestPortRange    Direction      Priority
        
        Allow     Allow inbound traffic from all VMs in VNET              *                Inbound           65000
        Allow     Allow inbound traffic from azure load balancer          *                Inbound           65001
        Deny      Deny all inbound traffic                                *                Inbound           65500
        Allow     Allow outbound traffic from all VMs to all VMs in VNET  *                Outbound          65000
        Allow     Allow outbound traffic from all VMs to Internet         *                Outbound          65001
        Deny      Deny all outbound traffic                               *                Outbound          65500



3. <span data-ttu-id="322fa-123">Hozzon létre egy szabályt, amely hozzáférést biztosít a 3389-es port (RDP) az internetről a [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="322fa-123">Create a rule that allows access to port 3389 (RDP) from the Internet with the [az network nsg rule create](/cli/azure/network/nsg/rule#create) command.</span></span>

    > [!NOTE]
    > <span data-ttu-id="322fa-124">Attól függően, hogy a rendszerhéj használ, előfordulhat, hogy módosítania a `*` az argumentumok, hogy ne bontsa ki a következő argumentum végrehajtása előtt a következő karakter.</span><span class="sxs-lookup"><span data-stu-id="322fa-124">Depending on the shell you are using, you might need to modify the `*` character in the arguments following so as not to expand the argument before execution.</span></span>
   
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
   
    <span data-ttu-id="322fa-125">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="322fa-125">Expected output:</span></span>
   
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

    <span data-ttu-id="322fa-126">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="322fa-126">Parameters:</span></span>

    * <span data-ttu-id="322fa-127">`--resource-group testrg`: Az erőforráscsoport használatára.</span><span class="sxs-lookup"><span data-stu-id="322fa-127">`--resource-group testrg`: The resource group to use.</span></span> <span data-ttu-id="322fa-128">Vegye figyelembe, hogy a rendszer azonban nem.</span><span class="sxs-lookup"><span data-stu-id="322fa-128">Note that it is case-insensitive.</span></span>
    * <span data-ttu-id="322fa-129">`--nsg-name NSG-FrontEnd`: A szabály létrehozása zajlik az NSG neve.</span><span class="sxs-lookup"><span data-stu-id="322fa-129">`--nsg-name NSG-FrontEnd`: Name of the NSG in which the rule is created.</span></span>
    * <span data-ttu-id="322fa-130">`--name rdp-rule`: Az új szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="322fa-130">`--name rdp-rule`: Name for the new rule.</span></span>
    * <span data-ttu-id="322fa-131">`--access Allow`: A hozzáférési szint a szabály (Megtagadás vagy engedélyezés).</span><span class="sxs-lookup"><span data-stu-id="322fa-131">`--access Allow`: Access level for the rule (Deny or Allow).</span></span>
    * <span data-ttu-id="322fa-132">`--protocol Tcp`: A protokoll (Tcp, Udp vagy *).</span><span class="sxs-lookup"><span data-stu-id="322fa-132">`--protocol Tcp`: Protocol (Tcp, Udp, or *).</span></span>
    * <span data-ttu-id="322fa-133">`--direction Inbound`: A (bejövő vagy kimenő) kapcsolat irányát.</span><span class="sxs-lookup"><span data-stu-id="322fa-133">`--direction Inbound`: Direction of the connection (Inbound or Outbound).</span></span>
    * <span data-ttu-id="322fa-134">`--priority 100`: A szabály prioritását.</span><span class="sxs-lookup"><span data-stu-id="322fa-134">`--priority 100`: Priority for the rule.</span></span>
    * <span data-ttu-id="322fa-135">`--source-address-prefix Internet`: Forrás címelőtagot CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="322fa-135">`--source-address-prefix Internet`: Source address prefix in CIDR or using default tags.</span></span>
    * <span data-ttu-id="322fa-136">`--source-port-range "*"`: A portot vagy porttartományt forrás.</span><span class="sxs-lookup"><span data-stu-id="322fa-136">`--source-port-range "*"`: Source port or port range.</span></span> <span data-ttu-id="322fa-137">Az portot, amelyet a kapcsolat megnyitása.</span><span class="sxs-lookup"><span data-stu-id="322fa-137">Port that opened the connection.</span></span>
    * <span data-ttu-id="322fa-138">`--destination-address-prefix "*"`: Cél címelőtagot CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="322fa-138">`--destination-address-prefix "*"`: Destination address prefix in CIDR or using default tags.</span></span>
    * <span data-ttu-id="322fa-139">`--destination-port-range 3389`: A célport vagy porttartomány.</span><span class="sxs-lookup"><span data-stu-id="322fa-139">`--destination-port-range 3389`: Destination port or port range.</span></span> <span data-ttu-id="322fa-140">Az port, amely megkapja a kapcsolódási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="322fa-140">Port that receives the connection request.</span></span>



4. <span data-ttu-id="322fa-141">Hozzon létre egy szabályt, amely engedélyezi a hozzáférést a port a 80-as (HTTP) az internetről **az hálózati nsg-szabály létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="322fa-141">Create a rule that allows access to port 80 (HTTP) from the Internet **az network nsg rule create** command.</span></span>
   
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
   
    <span data-ttu-id="322fa-142">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="322fa-142">Expected putput:</span></span>
   
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

5. <span data-ttu-id="322fa-143">Az NSG kötni a **előtér** alhálózat a [az hálózati vnet alhálózati frissítés](/cli/azure/network/vnet/subnet#update) parancs.</span><span class="sxs-lookup"><span data-stu-id="322fa-143">Bind the NSG to the **FrontEnd** subnet with the [az network vnet subnet update](/cli/azure/network/vnet/subnet#update) command.</span></span>
        
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name FrontEnd \
    --resource-group testrg \
    --network-security-group NSG-FrontEnd
    ```
   
    <span data-ttu-id="322fa-144">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="322fa-144">Expected output:</span></span>
   
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

## <a name="create-the-nsg-for-the-backend-subnet"></a><span data-ttu-id="322fa-145">Az NSG létrehozása a `BackEnd` alhálózati</span><span class="sxs-lookup"><span data-stu-id="322fa-145">Create the NSG for the `BackEnd` subnet</span></span>
<span data-ttu-id="322fa-146">Az NSG nevű létrehozásához *NSG-háttérrendszer* a fenti forgatókönyv alapján, kövesse a lépéseket következő.</span><span class="sxs-lookup"><span data-stu-id="322fa-146">To create an NSG named *NSG-BackEnd* based on the scenario preceding, follow the steps following.</span></span>

1. <span data-ttu-id="322fa-147">Hozzon létre a `NSG-BackEnd` az NSG **az hálózati nsg létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="322fa-147">Create the `NSG-BackEnd` NSG with **az network nsg create**.</span></span>
   
    ```azurecli
    az network nsg create \
    --resource-group testrg \
    --name NSG-BackEnd \
    --location centralus
    ```
   
    <span data-ttu-id="322fa-148">2. lépéshez, előző, mint a várt kimeneti elég nagy, beleértve az alapértelmezett szabályokat.</span><span class="sxs-lookup"><span data-stu-id="322fa-148">As in step 2, preceding, the expected output is quite large, including default rules.</span></span>
   
2. <span data-ttu-id="322fa-149">Hozzon létre egy szabályt, amely hozzáférést biztosít az 1433-as port (SQL) az a `FrontEnd` alhálózat a **az hálózati nsg-szabály létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="322fa-149">Create a rule that allows access to port 1433 (SQL) from the `FrontEnd` subnet with the **az network nsg rule create** command.</span></span>
   
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
   
    <span data-ttu-id="322fa-150">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="322fa-150">Expected output:</span></span>

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

3. <span data-ttu-id="322fa-151">Hozzon létre egy szabályt, amely megtagadja a hozzáférést az Internet használatát az **az hálózati nsg-szabály létrehozása** parancsot.</span><span class="sxs-lookup"><span data-stu-id="322fa-151">Create a rule that denies access to the Internet using the **az network nsg rule create** command.</span></span>
   
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
   
    <span data-ttu-id="322fa-152">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="322fa-152">Expected putput:</span></span>
   
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

4. <span data-ttu-id="322fa-153">Az NSG kötni a `BackEnd` alhálózati használata a **az hálózati vnet alhálózati set** parancs.</span><span class="sxs-lookup"><span data-stu-id="322fa-153">Bind the NSG to the `BackEnd` subnet using the **az network vnet subnet set** command.</span></span>
   
    ```azurecli
    az network vnet subnet update \
    --vnet-name TestVNET \
    --name BackEnd \
    --resource-group testrg \
    --network-security-group NSG-BackEnd
    ```
   
    <span data-ttu-id="322fa-154">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="322fa-154">Expected output:</span></span>
   
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
