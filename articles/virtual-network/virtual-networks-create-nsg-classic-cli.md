---
title: "aaaHow toocreate NSG-ket a klasszikus mód használatával hello Azure parancssori Felülettel |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és NSG-k telepítése a klasszikus módban, hello Azure parancssori felület használatával"
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
ms.openlocfilehash: eb78861e10a0dd950bb2c3783ee957d1cce55016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-nsgs-classic-in-hello-azure-cli"></a><span data-ttu-id="66629-103">Hogyan toocreate NSG-k (klasszikus) hello Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="66629-103">How toocreate NSGs (classic) in hello Azure CLI</span></span>
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="66629-104">Ez a cikk ismerteti a hello klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="66629-104">This article covers hello classic deployment model.</span></span> <span data-ttu-id="66629-105">Emellett [NSG-k létrehozása hello Resource Manager üzembe helyezési modellel](virtual-networks-create-nsg-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="66629-105">You can also [create NSGs in hello Resource Manager deployment model](virtual-networks-create-nsg-arm-cli.md).</span></span>

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

<span data-ttu-id="66629-106">minta hello Azure CLI-t az alábbi parancsok már a fenti hello forgatókönyv alapján létre egy egyszerű környezetben várható.</span><span class="sxs-lookup"><span data-stu-id="66629-106">hello sample Azure CLI commands below expect a simple environment already created based on hello scenario above.</span></span> <span data-ttu-id="66629-107">Ha toorun hello parancsok ebben a dokumentumban megjelenített, először létre tesztkörnyezetben hello által [létre virtuális hálózatok](virtual-networks-create-vnet-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="66629-107">If you want toorun hello commands as they are displayed in this document, first build hello test environment by [creating a VNet](virtual-networks-create-vnet-classic-cli.md).</span></span>

## <a name="how-toocreate-hello-nsg-for-hello-front-end-subnet"></a><span data-ttu-id="66629-108">Hogyan toocreate hello hello előtér alhálózat NSG</span><span class="sxs-lookup"><span data-stu-id="66629-108">How toocreate hello NSG for hello front end subnet</span></span>
<span data-ttu-id="66629-109">az NSG nevű toocreate nevű **NSG-előtérbeli** kövesse a fenti hello forgatókönyv alapján, hello alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="66629-109">toocreate an NSG named named **NSG-FrontEnd** based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="66629-110">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="66629-110">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="66629-111">Futtassa a hello  **`azure config mode`**  tooswitch tooclassic üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="66629-111">Run hello **`azure config mode`** command tooswitch tooclassic mode, as shown below.</span></span>
   
        azure config mode asm
   
    <span data-ttu-id="66629-112">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="66629-112">Expected output:</span></span>
   
        info:    New mode is asm
3. <span data-ttu-id="66629-113">Futtassa a hello  **`azure network nsg create`**  parancs toocreate egy NSG.</span><span class="sxs-lookup"><span data-stu-id="66629-113">Run hello **`azure network nsg create`** command toocreate an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    <span data-ttu-id="66629-114">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="66629-114">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="66629-115">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="66629-115">Parameters:</span></span>
   
   * <span data-ttu-id="66629-116">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="66629-116">**-l (or --location)**.</span></span> <span data-ttu-id="66629-117">Azure-régió, ahol hello új NSG létrejön.</span><span class="sxs-lookup"><span data-stu-id="66629-117">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="66629-118">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="66629-118">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="66629-119">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="66629-119">**-n (or --name)**.</span></span> <span data-ttu-id="66629-120">Hello nevét új NSG.</span><span class="sxs-lookup"><span data-stu-id="66629-120">Name for hello new NSG.</span></span> <span data-ttu-id="66629-121">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="66629-121">For our scenario, *NSG-FrontEnd*.</span></span>
4. <span data-ttu-id="66629-122">Futtassa a hello  **`azure network nsg rule create`**  parancs toocreate egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 3389-es (RDP) a hello Internet.</span><span class="sxs-lookup"><span data-stu-id="66629-122">Run hello **`azure network nsg rule create`** command toocreate a rule that allows access tooport 3389 (RDP) from hello Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    <span data-ttu-id="66629-123">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="66629-123">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
   
    <span data-ttu-id="66629-124">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="66629-124">Parameters:</span></span>
   
   * <span data-ttu-id="66629-125">**-a (vagy--nsg-név)**.</span><span class="sxs-lookup"><span data-stu-id="66629-125">**-a (or --nsg-name)**.</span></span> <span data-ttu-id="66629-126">Mely hello szabály létrehozza hello NSG neve.</span><span class="sxs-lookup"><span data-stu-id="66629-126">Name of hello NSG in which hello rule will be created.</span></span> <span data-ttu-id="66629-127">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="66629-127">For our scenario, *NSG-FrontEnd*.</span></span>
   * <span data-ttu-id="66629-128">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="66629-128">**-n (or --name)**.</span></span> <span data-ttu-id="66629-129">Hello új szabály nevét.</span><span class="sxs-lookup"><span data-stu-id="66629-129">Name for hello new rule.</span></span> <span data-ttu-id="66629-130">A mi esetünkben *rdp-szabály*.</span><span class="sxs-lookup"><span data-stu-id="66629-130">For our scenario, *rdp-rule*.</span></span>
   * <span data-ttu-id="66629-131">**-c (vagy--művelet)**.</span><span class="sxs-lookup"><span data-stu-id="66629-131">**-c (or --action)**.</span></span> <span data-ttu-id="66629-132">Hozzáférési szint hello szabály (Megtagadás vagy engedélyezés).</span><span class="sxs-lookup"><span data-stu-id="66629-132">Access level for hello rule (Deny or Allow).</span></span>
   * <span data-ttu-id="66629-133">**-p (vagy--protokoll)**.</span><span class="sxs-lookup"><span data-stu-id="66629-133">**-p (or --protocol)**.</span></span> <span data-ttu-id="66629-134">Protocol (Tcp, Udp vagy *) hello szabályhoz.</span><span class="sxs-lookup"><span data-stu-id="66629-134">Protocol (Tcp, Udp, or *) for hello rule.</span></span>
   * <span data-ttu-id="66629-135">**-r (vagy--típus)**.</span><span class="sxs-lookup"><span data-stu-id="66629-135">**-r (or --type)**.</span></span> <span data-ttu-id="66629-136">Irányát (bejövő vagy kimenő) kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="66629-136">Direction of connection (Inbound or Outbound).</span></span>
   * <span data-ttu-id="66629-137">**-y (vagy--prioritás)**.</span><span class="sxs-lookup"><span data-stu-id="66629-137">**-y (or --priority)**.</span></span> <span data-ttu-id="66629-138">Hello szabály prioritását.</span><span class="sxs-lookup"><span data-stu-id="66629-138">Priority for hello rule.</span></span>
   * <span data-ttu-id="66629-139">**-f (vagy--forráscímelőtag)**.</span><span class="sxs-lookup"><span data-stu-id="66629-139">**-f (or --source-address-prefix)**.</span></span> <span data-ttu-id="66629-140">Forráscímelőtag CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="66629-140">Source address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="66629-141">**-o (vagy--Forrásporttartomány)**.</span><span class="sxs-lookup"><span data-stu-id="66629-141">**-o (or --source-port-range)**.</span></span> <span data-ttu-id="66629-142">Forrásport, vagy porttartomány.</span><span class="sxs-lookup"><span data-stu-id="66629-142">Source port, or port range.</span></span>
   * <span data-ttu-id="66629-143">**-e (vagy--cél címelőtag)**.</span><span class="sxs-lookup"><span data-stu-id="66629-143">**-e (or --destination-address-prefix)**.</span></span> <span data-ttu-id="66629-144">Célcím-előtag CIDR vagy az alapértelmezett címkéket használ.</span><span class="sxs-lookup"><span data-stu-id="66629-144">Destination address prefix in CIDR or using default tags.</span></span>
   * <span data-ttu-id="66629-145">**-u (vagy--Célporttartomány)**.</span><span class="sxs-lookup"><span data-stu-id="66629-145">**-u (or --destination-port-range)**.</span></span> <span data-ttu-id="66629-146">Célport vagy porttartomány.</span><span class="sxs-lookup"><span data-stu-id="66629-146">Destination port, or port range.</span></span>
5. <span data-ttu-id="66629-147">Futtassa a hello  **`azure network nsg rule create`**  parancs toocreate egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 80-as (HTTP) az hello Internet.</span><span class="sxs-lookup"><span data-stu-id="66629-147">Run hello **`azure network nsg rule create`** command toocreate a rule that allows access tooport 80 (HTTP) from hello Internet.</span></span>
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    <span data-ttu-id="66629-148">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="66629-148">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-FrontEnd"
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
6. <span data-ttu-id="66629-149">Futtassa a hello  **`azure network nsg subnet add`**  parancs toolink hello NSG toohello előtérben lévő alhálózat alhálózat.</span><span class="sxs-lookup"><span data-stu-id="66629-149">Run hello **`azure network nsg subnet add`** command toolink hello NSG toohello front end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    <span data-ttu-id="66629-150">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="66629-150">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up hello network security group "NSG-FrontEnd"
        info:    Looking up hello subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-toocreate-hello-nsg-for-hello-back-end-subnet"></a><span data-ttu-id="66629-151">Hogyan toocreate hello NSG hello vissza a záró alhálózat</span><span class="sxs-lookup"><span data-stu-id="66629-151">How toocreate hello NSG for hello back end subnet</span></span>
<span data-ttu-id="66629-152">az NSG nevű toocreate nevű *NSG-háttérrendszer* kövesse a fenti hello forgatókönyv alapján, hello alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="66629-152">toocreate an NSG named named *NSG-BackEnd* based on hello scenario above, follow hello steps below.</span></span>

1. <span data-ttu-id="66629-153">Futtassa a hello  **`azure network nsg create`**  parancs toocreate egy NSG.</span><span class="sxs-lookup"><span data-stu-id="66629-153">Run hello **`azure network nsg create`** command toocreate an NSG.</span></span>
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    <span data-ttu-id="66629-154">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="66629-154">Expected output:</span></span>
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up hello network security group "NSG-BackEnd"
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
   
    <span data-ttu-id="66629-155">Paraméterek:</span><span class="sxs-lookup"><span data-stu-id="66629-155">Parameters:</span></span>
   
   * <span data-ttu-id="66629-156">**-l (vagy --location)**.</span><span class="sxs-lookup"><span data-stu-id="66629-156">**-l (or --location)**.</span></span> <span data-ttu-id="66629-157">Azure-régió, ahol hello új NSG létrejön.</span><span class="sxs-lookup"><span data-stu-id="66629-157">Azure region where hello new NSG will be created.</span></span> <span data-ttu-id="66629-158">A mi esetünkben *westus*.</span><span class="sxs-lookup"><span data-stu-id="66629-158">For our scenario, *westus*.</span></span>
   * <span data-ttu-id="66629-159">**-n (vagy --name)**.</span><span class="sxs-lookup"><span data-stu-id="66629-159">**-n (or --name)**.</span></span> <span data-ttu-id="66629-160">Hello nevét új NSG.</span><span class="sxs-lookup"><span data-stu-id="66629-160">Name for hello new NSG.</span></span> <span data-ttu-id="66629-161">A mi esetünkben *NSG-előtérbeli*.</span><span class="sxs-lookup"><span data-stu-id="66629-161">For our scenario, *NSG-FrontEnd*.</span></span>
2. <span data-ttu-id="66629-162">Futtassa a hello  **`azure network nsg rule create`**  parancs toocreate egy szabályt, amely lehetővé teszi, hogy a hozzáférés tooport 1433-as port (SQL) hello előtér alhálózatból.</span><span class="sxs-lookup"><span data-stu-id="66629-162">Run hello **`azure network nsg rule create`** command toocreate a rule that allows access tooport 1433 (SQL) from hello front end subnet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    <span data-ttu-id="66629-163">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="66629-163">Expected output:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
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
3. <span data-ttu-id="66629-164">Futtassa a hello  **`azure network nsg rule create`**  parancs toocreate egy szabályt, amely megtagadja a hozzáférést toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="66629-164">Run hello **`azure network nsg rule create`** command toocreate a rule that denies access toohello Internet.</span></span>
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    <span data-ttu-id="66629-165">Várt putput:</span><span class="sxs-lookup"><span data-stu-id="66629-165">Expected putput:</span></span>
   
        info:    Executing command network nsg rule create
        info:    Looking up hello network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up hello network security group "NSG-BackEnd"
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
4. <span data-ttu-id="66629-166">Futtassa a hello  **`azure network nsg subnet add`**  toolink hello NSG toohello vissza end alhálózati parancsot.</span><span class="sxs-lookup"><span data-stu-id="66629-166">Run hello **`azure network nsg subnet add`** command toolink hello NSG toohello back end subnet.</span></span>
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    <span data-ttu-id="66629-167">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="66629-167">Expected output:</span></span>
   
        info:    Executing command network nsg subnet add
        info:    Looking up hello network security group "NSG-BackEndX"
        info:    Looking up hello subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

