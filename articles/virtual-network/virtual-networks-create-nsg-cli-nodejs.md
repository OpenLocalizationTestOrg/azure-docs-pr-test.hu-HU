---
title: "Hozzon létre a hálózati biztonsági csoport – Azure CLI 1.0 |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre és telepíthet a hálózati biztonsági csoportok használata az Azure CLI 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/17/2017
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca8c182651e3c9f2f1f3a85b94361755d8e638d4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-network-security-groups-using-the-azure-cli-10"></a><span data-ttu-id="81611-103">Hálózati biztonsági csoportok használata az Azure CLI 1.0 létrehozása</span><span class="sxs-lookup"><span data-stu-id="81611-103">Create network security groups using the Azure CLI 1.0</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="81611-104">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="81611-104">CLI versions to complete the task</span></span> 

<span data-ttu-id="81611-105">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="81611-105">You can complete the task using one of the following CLI versions:</span></span> 

- <span data-ttu-id="81611-106">[Az Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – a parancssori felületen a klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="81611-106">[Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="81611-107">[Az Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) -erőforrás felügyeleti telepítési modell a következő generációs parancssori felület</span><span class="sxs-lookup"><span data-stu-id="81611-107">[Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) - our next-generation CLI for the resource management deployment model</span></span> 

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="81611-108">Ez a cikk a Resource Manager-alapú üzemi modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="81611-108">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="81611-109">Emellett [NSG-k létrehozása a klasszikus üzembe helyezési modellel](virtual-networks-create-nsg-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="81611-109">You can also [create NSGs in the classic deployment model](virtual-networks-create-nsg-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="81611-110">Az alábbi minta Azure parancssori felület parancsait már a fenti forgatókönyv alapján létre egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="81611-110">The sample Azure CLI commands below expect a simple environment already created based on the scenario above.</span></span> 

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a><span data-ttu-id="81611-111">Az NSG az előtér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="81611-111">How to create the NSG for the front end subnet</span></span>
<span data-ttu-id="81611-112">Hozzon létre egy NSG nevű nevű *NSG-előtérbeli* a fenti forgatókönyv alapján, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="81611-112">To create an NSG named named *NSG-FrontEnd* based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="81611-113">Ha még sosem használta az Azure CLI-t, akkor tekintse meg [Install and Configure the Azure CLI](../cli-install-nodejs.md) (Az Azure CLI telepítése és konfigurálása) részt, és kövesse az utasításokat addig a pontig, ahol ki kell választania az Azure-fiókot és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="81611-113">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="81611-114">Az **azure config mode** parancs futtatásával váltson az Erőforrás-kezelő módra, a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="81611-114">Run the **azure config mode** command to switch to Resource Manager mode, as shown below.</span></span>
   
        azure config mode arm
   
    <span data-ttu-id="81611-115">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81611-115">Expected output:</span></span>
   
        info:    New mode is arm
3. <span data-ttu-id="81611-116">Futtassa a **létrehozása azure-hálózat nsg** parancs futtatásával hozzon létre egy NSG.</span><span class="sxs-lookup"><span data-stu-id="81611-116">Run the **azure network nsg create** command to create an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-FrontEnd
   
    <span data-ttu-id="81611-117">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81611-117">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd
        data:    Name                            : NSG-FrontEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK
   
    <span data-ttu-id="81611-118">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="81611-118">Parameters:</span></span>
   
   * <span data-ttu-id="81611-119">**-g (vagy --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="81611-119">**-g (or --resource-group)**.</span></span> <span data-ttu-id="81611-120">Az erőforráscsoport, ahol létrejön az NSG neve.</span><span class="sxs-lookup"><span data-stu-id="81611-120">Name of the resource group where the NSG will be created.</span></span> <span data-ttu-id="81611-121">A mi esetünkben *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="81611-121">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="81611-122">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="81611-122">**-l (or --location)**.</span></span> <span data-ttu-id="81611-123">Azure-régió, ahol létrejön az új NSG.</span><span class="sxs-lookup"><span data-stu-id="81611-123">Azure region where the new NSG will be created.</span></span> <span data-ttu-id="81611-124">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="81611-124">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="81611-125">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="81611-125">**-n (or --name)**.</span></span> <span data-ttu-id="81611-126">Az új NSG neve.</span><span class="sxs-lookup"><span data-stu-id="81611-126">Name for the new NSG.</span></span> <span data-ttu-id="81611-127">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="81611-127">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="81611-128">Futtassa a **azure hálózati nsg-szabály létrehozása** parancs futtatásával hozzon létre egy szabályt, amely engedélyezi a hozzáférést a 3389-es (RDP) az internetről.</span><span class="sxs-lookup"><span data-stu-id="81611-128">Run the **azure network nsg rule create** command to create a rule that allows access to port 3389 (RDP) from the Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="81611-129">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81611-129">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        warn:    Using default direction: Inbound
        info:    Looking up the network security rule "rdp-rule"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkSecurityGroups/NSG-FrontEnd/securityRules/rdp
        -rule
        data:    Name                            : rdp-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 3389
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    <span data-ttu-id="81611-130">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="81611-130">Parameters:</span></span>
   
   * <span data-ttu-id="81611-131">**-a (vagy--nsg-név)**.</span><span class="sxs-lookup"><span data-stu-id="81611-131">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="81611-132">Az ebben a szabály jön létre NSG neve.</span><span class="sxs-lookup"><span data-stu-id="81611-132">Name of the NSG in which the rule will be created.</span></span> <span data-ttu-id="81611-133">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="81611-133">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="81611-134">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="81611-134">**-n (or --name)**.</span></span> <span data-ttu-id="81611-135">Az új szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="81611-135">Name for the new rule.</span></span> <span data-ttu-id="81611-136">A mi esetünkben *rdp-szabály*.</span><span class="sxs-lookup"><span data-stu-id="81611-136">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="81611-137">**-c (vagy--hozzáférés)**.</span><span class="sxs-lookup"><span data-stu-id="81611-137">**-c (or --access)**.</span></span> <span data-ttu-id="81611-138">Hozzáférési szint a szabály (Megtagadás vagy engedélyezés).</span><span class="sxs-lookup"><span data-stu-id="81611-138">Access level for the rule (Deny or Allow).</span></span>
   * <span data-ttu-id="81611-139">**-p (vagy--protokoll)**.</span><span class="sxs-lookup"><span data-stu-id="81611-139">**-p (or --protocol)**.</span></span> <span data-ttu-id="81611-140">Protocol (Tcp, Udp vagy *) a szabályhoz.</span><span class="sxs-lookup"><span data-stu-id="81611-140">Protocol (Tcp, Udp, or *) for the rule.</span></span>
   * <span data-ttu-id="81611-141">**-r (vagy--iránya)**.</span><span class="sxs-lookup"><span data-stu-id="81611-141">**-r (or --direction)**.</span></span> <span data-ttu-id="81611-142">Irányát (bejövő vagy kimenő) kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="81611-142">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="81611-143">**-y (vagy--prioritás)**.</span><span class="sxs-lookup"><span data-stu-id="81611-143">**-y (or --priority)**.</span></span> <span data-ttu-id="81611-144">A szabály prioritását.</span><span class="sxs-lookup"><span data-stu-id="81611-144">Priority for the rule.</span></span>
   * <span data-ttu-id="81611-145">**-f (vagy--forráscímelőtag)**.</span><span class="sxs-lookup"><span data-stu-id="81611-145">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="81611-146">Forráscímelőtag CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="81611-146">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="81611-147">**-o (vagy--Forrásporttartomány)**.</span><span class="sxs-lookup"><span data-stu-id="81611-147">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="81611-148">Forrásport, vagy porttartomány.</span><span class="sxs-lookup"><span data-stu-id="81611-148">Source port, or port range.</span></span>
   * <span data-ttu-id="81611-149">**-e (vagy--cél címelőtag)**.</span><span class="sxs-lookup"><span data-stu-id="81611-149">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="81611-150">Célcím-előtag CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="81611-150">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="81611-151">**-u (vagy--Célporttartomány)**.</span><span class="sxs-lookup"><span data-stu-id="81611-151">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="81611-152">Célport vagy porttartomány.</span><span class="sxs-lookup"><span data-stu-id="81611-152">Destination port, or port range.</span></span>    
5. <span data-ttu-id="81611-153">Futtassa a **azure hálózati nsg-szabály létrehozása** parancs futtatásával hozzon létre egy szabályt, amely engedélyezi a hozzáférést a port a 80-as (HTTP) az internetről.</span><span class="sxs-lookup"><span data-stu-id="81611-153">Run the **azure network nsg rule create** command to create a rule that allows access to port 80 (HTTP) from the Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="81611-154">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="81611-154">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-FrontEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : Internet
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 80
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. <span data-ttu-id="81611-155">Futtassa a **azure-hálózat virtuális hálózat alhálózati set** parancs az NSG csatolása az előtér-alhálózat.</span><span class="sxs-lookup"><span data-stu-id="81611-155">Run the **azure network vnet subnet set** command to link the NSG to the front end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -o NSG-FrontEnd
   
    <span data-ttu-id="81611-156">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81611-156">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/FrontEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : FrontEnd
        data:    Address prefix                  : 192.168.1.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb2/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICWeb1/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a><span data-ttu-id="81611-157">Az NSG a háttérbeli alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="81611-157">How to create the NSG for the back end subnet</span></span>
<span data-ttu-id="81611-158">Hozzon létre egy NSG nevű nevű *NSG-háttérrendszer* a fenti forgatókönyv alapján, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="81611-158">To create an NSG named named *NSG-BackEnd* based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="81611-159">Futtassa a **létrehozása azure-hálózat nsg** parancs futtatásával hozzon létre egy NSG.</span><span class="sxs-lookup"><span data-stu-id="81611-159">Run the **azure network nsg create** command to create an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-BackEnd
   
    <span data-ttu-id="81611-160">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81611-160">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd
        data:    Name                            : NSG-BackEnd
        data:    Type                            : Microsoft.Network/networkSecurityGroups
        data:    Location                        : westus
        data:    Provisioning state              : Succeeded
        data:    Security group rules:
        data:    Name                           Source IP          Source Port  Destination IP  Destination Port  Protocol  Direction  Access  Priority
        data:    -----------------------------  -----------------  -----------  --------------  ----------------  --------  ---------  ------  --------
        data:    AllowVnetInBound               VirtualNetwork     *            VirtualNetwork  *                 *         Inbound    Allow   65000   
        data:    AllowAzureLoadBalancerInBound  AzureLoadBalancer  *            *               *                 *         Inbound    Allow   65001   
        data:    DenyAllInBound                 *                  *            *               *                 *         Inbound    Deny    65500   
        data:    AllowVnetOutBound              VirtualNetwork     *            VirtualNetwork  *                 *         Outbound   Allow   65000   
        data:    AllowInternetOutBound          *                  *            Internet        *                 *         Outbound   Allow   65001   
        data:    DenyAllOutBound                *                  *            *               *                 *         Outbound   Deny    65500   
        info:    network nsg create command OK
2. <span data-ttu-id="81611-161">Futtassa a **azure hálózati nsg-szabály létrehozása** parancs futtatásával hozzon létre egy szabályt, amely engedélyezi a hozzáférést a port a 1433-as port (SQL) az előtér-alhálózatból.</span><span class="sxs-lookup"><span data-stu-id="81611-161">Run the **azure network nsg rule create** command to create a rule that allows access to port 1433 (SQL) from the front end subnet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="81611-162">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81611-162">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "sql-rule"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/sql-rule
        data:    Name                            : sql-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination IP                  : *
        data:    Destination Port                : 1433
        data:    Protocol                        : Tcp
        data:    Direction                       : Inbound
        data:    Access                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. <span data-ttu-id="81611-163">Futtassa a **azure hálózati nsg-szabály létrehozása** parancs futtatásával hozzon létre egy szabályt, amely megtagadja az internethez való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="81611-163">Run the **azure network nsg rule create** command to create a rule that denies access to the Internet from.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n web-rule -c Deny -p * -r Outbound -y 200 -f * -o * -e Internet -u *
   
    <span data-ttu-id="81611-164">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="81611-164">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        networkSecurityGroups/NSG-BackEnd/securityRules/web-rule
        data:    Name                            : web-rule
        data:    Type                            : Microsoft.Network/networkSecurityGroups/securityRules
        data:    Provisioning state              : Succeeded
        data:    Source IP                       : *
        data:    Source Port                     : *
        data:    Destination IP                  : Internet
        data:    Destination Port                : *
        data:    Protocol                        : *
        data:    Direction                       : Outbound
        data:    Access                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. <span data-ttu-id="81611-165">Futtassa a **azure-hálózat virtuális hálózat alhálózati set** csatolja az NSG a háttérbeli alhálózati parancsot.</span><span class="sxs-lookup"><span data-stu-id="81611-165">Run the **azure network vnet subnet set** command to link the NSG to the back end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -o NSG-BackEnd
   
    <span data-ttu-id="81611-166">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="81611-166">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up the subnet "BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Setting subnet "BackEnd"
        info:    Looking up the subnet "BackEnd"
        data:    Id                              : /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/
        virtualNetworks/TestVNet/subnets/BackEnd
        data:    Type                            : Microsoft.Network/virtualNetworks/subnets
        data:    ProvisioningState               : Succeeded
        data:    Name                            : BackEnd
        data:    Address prefix                  : 192.168.2.0/24
        data:    Network security group          : [object Object]
        data:    IP configurations:
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL1/ip
        Configurations/ipconfig1
        data:      /subscriptions/628dad04-b5d1-4f10-b3a4-dc61d88cf97c/resourceGroups/TestRG/providers/Microsoft.Network/networkInterfaces/TestNICSQL2/ip
        Configurations/ipconfig1
        data:    
        info:    network vnet subnet set command OK

