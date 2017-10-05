---
title: "Az NSG-k létrehozása a klasszikus mód az Azure parancssori felülettel |} Microsoft Docs"
description: "Megtudhatja, hogyan hozhat létre és telepíthet az NSG-k klasszikus módban, az Azure parancssori felület használatával"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 115a1937a4c88ba2b986a40c84b1b759ed5e03b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a><span data-ttu-id="95630-103">Az NSG-k (klasszikus) létrehozása az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="95630-103">How to create NSGs (classic) in the Azure CLI</span></span>
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="95630-104">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="95630-104">This article covers the classic deployment model.</span></span> <span data-ttu-id="95630-105">Emellett [NSG-k létrehozása a Resource Manager üzembe helyezési modellel](virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="95630-105">You can also [create NSGs in the Resource Manager deployment model](virtual-networks-create-nsg-arm-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="95630-106">Az alábbi minta Azure parancssori felület parancsait már a fenti forgatókönyv alapján létre egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="95630-106">The sample Azure CLI commands below expect a simple environment already created based on the scenario above.</span></span> <span data-ttu-id="95630-107">Ha szeretné a parancsokat a jelen dokumentum megjelenített, először által a tesztkörnyezet felépítéséhez [létre virtuális hálózatok](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="95630-107">If you want to run the commands as they are displayed in this document, first build the test environment by [creating a VNet](virtual-networks-create-vnet-classic-cli.md).</span></span>

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a><span data-ttu-id="95630-108">Az NSG az előtér-alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="95630-108">How to create the NSG for the front end subnet</span></span>
<span data-ttu-id="95630-109">Hozzon létre egy NSG nevű nevű **NSG-előtérbeli** a fenti forgatókönyv alapján, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="95630-109">To create an NSG named named **NSG-FrontEnd** based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="95630-110">Ha még sosem használta az Azure CLI-t, akkor tekintse meg [Install and Configure the Azure CLI](../cli-install-nodejs.md) (Az Azure CLI telepítése és konfigurálása) részt, és kövesse az utasításokat addig a pontig, ahol ki kell választania az Azure-fiókot és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="95630-110">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="95630-111">Futtassa a  **`azure config mode`**  parancs futtatásával váltson klasszikus módban, a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="95630-111">Run the **`azure config mode`** command to switch to classic mode, as shown below.</span></span>
   
        azure config mode asm
   
    <span data-ttu-id="95630-112">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="95630-112">Expected output:</span></span>
   
        info:    New mode is asm
3. <span data-ttu-id="95630-113">Futtassa a  **`azure network nsg create`**  parancs futtatásával hozzon létre egy NSG.</span><span class="sxs-lookup"><span data-stu-id="95630-113">Run the **`azure network nsg create`** command to create an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    <span data-ttu-id="95630-114">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="95630-114">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    <span data-ttu-id="95630-115">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="95630-115">Parameters:</span></span>
   
   * <span data-ttu-id="95630-116">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="95630-116">**-l (or --location)**.</span></span> <span data-ttu-id="95630-117">Azure-régió, ahol létrejön az új NSG.</span><span class="sxs-lookup"><span data-stu-id="95630-117">Azure region where the new NSG will be created.</span></span> <span data-ttu-id="95630-118">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="95630-118">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="95630-119">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="95630-119">**-n (or --name)**.</span></span> <span data-ttu-id="95630-120">Az új NSG neve.</span><span class="sxs-lookup"><span data-stu-id="95630-120">Name for the new NSG.</span></span> <span data-ttu-id="95630-121">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="95630-121">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="95630-122">Futtassa a  **`azure network nsg rule create`**  parancs futtatásával hozzon létre egy szabályt, amely engedélyezi a hozzáférést a 3389-es (RDP) az internetről.</span><span class="sxs-lookup"><span data-stu-id="95630-122">Run the **`azure network nsg rule create`** command to create a rule that allows access to port 3389 (RDP) from the Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="95630-123">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="95630-123">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    <span data-ttu-id="95630-124">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="95630-124">Parameters:</span></span>
   
   * <span data-ttu-id="95630-125">**-a (vagy--nsg-név)**.</span><span class="sxs-lookup"><span data-stu-id="95630-125">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="95630-126">Az ebben a szabály jön létre NSG neve.</span><span class="sxs-lookup"><span data-stu-id="95630-126">Name of the NSG in which the rule will be created.</span></span> <span data-ttu-id="95630-127">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="95630-127">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="95630-128">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="95630-128">**-n (or --name)**.</span></span> <span data-ttu-id="95630-129">Az új szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="95630-129">Name for the new rule.</span></span> <span data-ttu-id="95630-130">A mi esetünkben *rdp-szabály*.</span><span class="sxs-lookup"><span data-stu-id="95630-130">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="95630-131">**-c (vagy--művelet)**.</span><span class="sxs-lookup"><span data-stu-id="95630-131">**-c (or --action)**.</span></span> <span data-ttu-id="95630-132">Hozzáférési szint a szabály (Megtagadás vagy engedélyezés).</span><span class="sxs-lookup"><span data-stu-id="95630-132">Access level for the rule (Deny or Allow).</span></span>
   * <span data-ttu-id="95630-133">**-p (vagy--protokoll)**.</span><span class="sxs-lookup"><span data-stu-id="95630-133">**-p (or --protocol)**.</span></span> <span data-ttu-id="95630-134">Protocol (Tcp, Udp vagy *) a szabályhoz.</span><span class="sxs-lookup"><span data-stu-id="95630-134">Protocol (Tcp, Udp, or *) for the rule.</span></span>
   * <span data-ttu-id="95630-135">**-r (vagy--típus)**.</span><span class="sxs-lookup"><span data-stu-id="95630-135">**-r (or --type)**.</span></span> <span data-ttu-id="95630-136">Irányát (bejövő vagy kimenő) kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="95630-136">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="95630-137">**-y (vagy--prioritás)**.</span><span class="sxs-lookup"><span data-stu-id="95630-137">**-y (or --priority)**.</span></span> <span data-ttu-id="95630-138">A szabály prioritását.</span><span class="sxs-lookup"><span data-stu-id="95630-138">Priority for the rule.</span></span>
   * <span data-ttu-id="95630-139">**-f (vagy--forráscímelőtag)**.</span><span class="sxs-lookup"><span data-stu-id="95630-139">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="95630-140">Forráscímelőtag CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="95630-140">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="95630-141">**-o (vagy--Forrásporttartomány)**.</span><span class="sxs-lookup"><span data-stu-id="95630-141">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="95630-142">Forrásport, vagy porttartomány.</span><span class="sxs-lookup"><span data-stu-id="95630-142">Source port, or port range.</span></span>
   * <span data-ttu-id="95630-143">**-e (vagy--cél címelőtag)**.</span><span class="sxs-lookup"><span data-stu-id="95630-143">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="95630-144">Célcím-előtag CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="95630-144">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="95630-145">**-u (vagy--Célporttartomány)**.</span><span class="sxs-lookup"><span data-stu-id="95630-145">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="95630-146">Célport vagy porttartomány.</span><span class="sxs-lookup"><span data-stu-id="95630-146">Destination port, or port range.</span></span>
5. <span data-ttu-id="95630-147">Futtassa a  **`azure network nsg rule create`**  parancs futtatásával hozzon létre egy szabályt, amely engedélyezi a hozzáférést a port a 80-as (HTTP) az internetről.</span><span class="sxs-lookup"><span data-stu-id="95630-147">Run the **`azure network nsg rule create`** command to create a rule that allows access to port 80 (HTTP) from the Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="95630-148">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="95630-148">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. <span data-ttu-id="95630-149">Futtassa a  **`azure network nsg subnet add`**  parancs az NSG csatolása az előtér-alhálózat.</span><span class="sxs-lookup"><span data-stu-id="95630-149">Run the **`azure network nsg subnet add`** command to link the NSG to the front end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    <span data-ttu-id="95630-150">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="95630-150">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a><span data-ttu-id="95630-151">Az NSG a háttérbeli alhálózat létrehozása</span><span class="sxs-lookup"><span data-stu-id="95630-151">How to create the NSG for the back end subnet</span></span>
<span data-ttu-id="95630-152">Hozzon létre egy NSG nevű nevű *NSG-háttérrendszer* a fenti forgatókönyv alapján, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="95630-152">To create an NSG named named *NSG-BackEnd* based on the scenario above, follow the steps below.</span></span>

1. <span data-ttu-id="95630-153">Futtassa a  **`azure network nsg create`**  parancs futtatásával hozzon létre egy NSG.</span><span class="sxs-lookup"><span data-stu-id="95630-153">Run the **`azure network nsg create`** command to create an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    <span data-ttu-id="95630-154">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="95630-154">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    <span data-ttu-id="95630-155">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="95630-155">Parameters:</span></span>
   
   * <span data-ttu-id="95630-156">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="95630-156">**-l (or --location)**.</span></span> <span data-ttu-id="95630-157">Azure-régió, ahol létrejön az új NSG.</span><span class="sxs-lookup"><span data-stu-id="95630-157">Azure region where the new NSG will be created.</span></span> <span data-ttu-id="95630-158">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="95630-158">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="95630-159">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="95630-159">**-n (or --name)**.</span></span> <span data-ttu-id="95630-160">Az új NSG neve.</span><span class="sxs-lookup"><span data-stu-id="95630-160">Name for the new NSG.</span></span> <span data-ttu-id="95630-161">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="95630-161">For our scenario, *NSG-FrontEnd*.</span></span>
2. <span data-ttu-id="95630-162">Futtassa a  **`azure network nsg rule create`**  parancs futtatásával hozzon létre egy szabályt, amely engedélyezi a hozzáférést a port a 1433-as port (SQL) az előtér-alhálózatból.</span><span class="sxs-lookup"><span data-stu-id="95630-162">Run the **`azure network nsg rule create`** command to create a rule that allows access to port 1433 (SQL) from the front end subnet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="95630-163">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="95630-163">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. <span data-ttu-id="95630-164">Futtassa a  **`azure network nsg rule create`**  parancs futtatásával hozzon létre egy szabályt, amely megtagadja a hozzáférést az internethez.</span><span class="sxs-lookup"><span data-stu-id="95630-164">Run the **`azure network nsg rule create`** command to create a rule that denies access to the Internet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    <span data-ttu-id="95630-165">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="95630-165">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. <span data-ttu-id="95630-166">Futtassa a  **`azure network nsg subnet add`**  csatolja az NSG a háttérbeli alhálózati parancsot.</span><span class="sxs-lookup"><span data-stu-id="95630-166">Run the **`azure network nsg subnet add`** command to link the NSG to the back end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    <span data-ttu-id="95630-167">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="95630-167">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

