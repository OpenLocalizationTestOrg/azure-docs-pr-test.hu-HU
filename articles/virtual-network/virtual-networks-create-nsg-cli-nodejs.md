---
title: "hálózati biztonsági csoport – Azure CLI 1.0 aaaCreate |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és hálózati biztonsági csoportok hello Azure CLI 1.0 használatával telepítheti."
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
ms.openlocfilehash: eeb7feedab959d92659e03c5c46d93fdfc08faea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-network-security-groups-using-hello-azure-cli-10"></a><span data-ttu-id="6c4c8-103">Hálózati biztonsági csoportok használatával hello Azure CLI 1.0 létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c4c8-103">Create network security groups using hello Azure CLI 1.0</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="6c4c8-104">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="6c4c8-104">CLI versions toocomplete hello task</span></span> 

<span data-ttu-id="6c4c8-105">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-105">You can complete hello task using one of hello following CLI versions:</span></span> 

- <span data-ttu-id="6c4c8-106">[Az Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="6c4c8-106">[Azure CLI 1.0](#how-to-create-the-nsg-for-the-front-end-subnet) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="6c4c8-107">[Az Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="6c4c8-107">[Azure CLI 2.0](virtual-networks-create-nsg-arm-cli.md) - our next-generation CLI for hello resource management deployment model</span></span> 

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="6c4c8-108">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="6c4c8-109">Emellett [NSG-k létrehozása hello klasszikus üzembe helyezési modellel](virtual-networks-create-nsg-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="6c4c8-109">You can also [create NSGs in hello classic deployment model](virtual-networks-create-nsg-classic-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="6c4c8-110">minta hello Azure CLI-t az alábbi parancsok már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-110">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> 

## <a name="how-toocreate-hello-nsg-for-hello-front-end-subnet"></a><span data-ttu-id="6c4c8-111">Hogyan toocreate hello hello előtér alhálózat NSG</span><span class="sxs-lookup"><span data-stu-id="6c4c8-111">How toocreate hello NSG for hello front end subnet</span></span>
<span data-ttu-id="6c4c8-112">az NSG nevű toocreate nevű *NSG-előtérbeli* kövesse a fenti hello forgatókönyv alapján, hello alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-112">toocreate an NSG named named *NSG-FrontEnd* based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="6c4c8-113">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-113">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="6c4c8-114">Futtassa a hello **azure config mód** tooswitch tooResource Manager üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-114">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>
   
        azure config mode arm
   
    <span data-ttu-id="6c4c8-115">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-115">Expected output:</span></span>
   
        info:    New mode is arm
3. <span data-ttu-id="6c4c8-116">Futtassa a hello **azure hálózati nsg létrehozása** parancs toocreate egy NSG.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-116">Run hello **azure network nsg create** command toocreate an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-FrontEnd
   
    <span data-ttu-id="6c4c8-117">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-117">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="6c4c8-118">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-118">Parameters:</span></span>
   
   * <span data-ttu-id="6c4c8-119">**-g (vagy --resource-group)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-119">**-g (or --resource-group)**.</span></span> <span data-ttu-id="6c4c8-120">Ahol létrejön az hello NSG hello erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-120">Name of hello resource group where hello NSG will be created.</span></span> <span data-ttu-id="6c4c8-121">A mi esetünkben *TestRG*.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-121">For our scenario, *TestRG*.</span></span>
   * <span data-ttu-id="6c4c8-122">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-122">**-l (or --location)**.</span></span> <span data-ttu-id="6c4c8-123">Azure-régió, ahol hello új NSG létrejön.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-123">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="6c4c8-124">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-124">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="6c4c8-125">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-125">**-n (or --name)**.</span></span> <span data-ttu-id="6c4c8-126">Hello nevét új NSG.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-126">Name for hello new NSG.</span></span> <span data-ttu-id="6c4c8-127">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-127">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="6c4c8-128">Futtassa a hello **azure hálózati nsg-szabály létrehozása** parancs toocreate egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 3389-es (RDP) a hello Internet.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-128">Run hello **azure network nsg rule create** command toocreate a rule that allows access tooport 3389 (RDP) from hello Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="6c4c8-129">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-129">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        warn:    Using default direction: Inbound
        info:    Looking up hello network security rule "rdp-rule"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="6c4c8-130">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-130">Parameters:</span></span>
   
   * <span data-ttu-id="6c4c8-131">**-a (vagy--nsg-név)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-131">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="6c4c8-132">Mely hello szabály létrehozza hello NSG neve.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-132">Name of hello NSG in which hello rule will be created.</span></span> <span data-ttu-id="6c4c8-133">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-133">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="6c4c8-134">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-134">**-n (or --name)**.</span></span> <span data-ttu-id="6c4c8-135">Hello új szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-135">Name for hello new rule.</span></span> <span data-ttu-id="6c4c8-136">A mi esetünkben *rdp-szabály*.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-136">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="6c4c8-137">**-c (vagy--hozzáférés)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-137">**-c (or --access)**.</span></span> <span data-ttu-id="6c4c8-138">Hozzáférési szint hello szabály (Megtagadás vagy engedélyezés).</span><span class="sxs-lookup"><span data-stu-id="6c4c8-138">Access level for hello rule (Deny or Allow).</span></span>
   * <span data-ttu-id="6c4c8-139">**-p (vagy--protokoll)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-139">**-p (or --protocol)**.</span></span> <span data-ttu-id="6c4c8-140">Protocol (Tcp, Udp vagy *) hello szabályhoz.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-140">Protocol (Tcp, Udp, or *) for hello rule.</span></span>
   * <span data-ttu-id="6c4c8-141">**-r (vagy--iránya)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-141">**-r (or --direction)**.</span></span> <span data-ttu-id="6c4c8-142">Irányát (bejövő vagy kimenő) kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-142">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="6c4c8-143">**-y (vagy--prioritás)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-143">**-y (or --priority)**.</span></span> <span data-ttu-id="6c4c8-144">Hello szabály prioritását.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-144">Priority for hello rule.</span></span>
   * <span data-ttu-id="6c4c8-145">**-f (vagy--forráscímelőtag)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-145">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="6c4c8-146">Forráscímelőtag CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-146">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="6c4c8-147">**-o (vagy--Forrásporttartomány)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-147">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="6c4c8-148">Forrásport, vagy porttartomány.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-148">Source port, or port range.</span></span>
   * <span data-ttu-id="6c4c8-149">**-e (vagy--cél címelőtag)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-149">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="6c4c8-150">Célcím-előtag CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-150">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="6c4c8-151">**-u (vagy--Célporttartomány)**.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-151">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="6c4c8-152">Célport vagy porttartomány.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-152">Destination port, or port range.</span></span>    
5. <span data-ttu-id="6c4c8-153">Futtassa a hello **azure hálózati nsg-szabály létrehozása** parancs toocreate egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 80-as (HTTP) az hello Internet.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-153">Run hello **azure network nsg rule create** command toocreate a rule that allows access tooport 80 (HTTP) from hello Internet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="6c4c8-154">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-154">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
6. <span data-ttu-id="6c4c8-155">Futtassa a hello **azure-hálózat virtuális hálózat alhálózati set** parancs toolink hello NSG toohello előtérben lévő alhálózat alhálózat.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-155">Run hello **azure network vnet subnet set** command toolink hello NSG toohello front end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n FrontEnd -o NSG-FrontEnd
   
    <span data-ttu-id="6c4c8-156">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-156">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Setting subnet "FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
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

## <a name="how-toocreate-hello-nsg-for-hello-back-end-subnet"></a><span data-ttu-id="6c4c8-157">Hogyan toocreate hello NSG hello vissza a záró alhálózat</span><span class="sxs-lookup"><span data-stu-id="6c4c8-157">How toocreate hello NSG for hello back end subnet</span></span>
<span data-ttu-id="6c4c8-158">az NSG nevű toocreate nevű *NSG-háttérrendszer* kövesse a fenti hello forgatókönyv alapján, hello alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-158">toocreate an NSG named named *NSG-BackEnd* based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="6c4c8-159">Futtassa a hello **azure hálózati nsg létrehozása** parancs toocreate egy NSG.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-159">Run hello **azure network nsg create** command toocreate an NSG.</span></span>
   
        azure network nsg create -g TestRG -l westus -n NSG-BackEnd
   
    <span data-ttu-id="6c4c8-160">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-160">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up hello network security group "NSG-BackEnd"
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
2. <span data-ttu-id="6c4c8-161">Futtassa a hello **azure hálózati nsg-szabály létrehozása** parancs toocreate egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 1433-as port (SQL) hello előtér alhálózatból.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-161">Run hello **azure network nsg rule create** command toocreate a rule that allows access tooport 1433 (SQL) from hello front end subnet.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="6c4c8-162">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-162">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security rule "sql-rule"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
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
3. <span data-ttu-id="6c4c8-163">Futtassa a hello **azure hálózati nsg-szabály létrehozása** parancs toocreate egy szabályt, amely megtagadja a hozzáférést toohello Internet a.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-163">Run hello **azure network nsg rule create** command toocreate a rule that denies access toohello Internet from.</span></span>
   
        azure network nsg rule create -g TestRG -a NSG-BackEnd -n web-rule -c Deny -p * -r Outbound -y 200 -f * -o * -e Internet -u *
   
    <span data-ttu-id="6c4c8-164">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-164">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security rule "web-rule"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
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
4. <span data-ttu-id="6c4c8-165">Futtassa a hello **azure-hálózat virtuális hálózat alhálózati set** toolink hello NSG toohello vissza end alhálózati parancs.</span><span class="sxs-lookup"><span data-stu-id="6c4c8-165">Run hello **azure network vnet subnet set** command toolink hello NSG toohello back end subnet.</span></span>
   
        azure network vnet subnet set -g TestRG -e TestVNet -n BackEnd -o NSG-BackEnd
   
    <span data-ttu-id="6c4c8-166">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6c4c8-166">Expected output:</span></span>
   
        info:    Executing command network vnet subnet set
        info:    Looking up hello subnet "BackEnd"
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Setting subnet "BackEnd"
        info:    Looking up hello subnet "BackEnd"
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

