---
title: "IPv6 - Azure CLI aaaCreate egy internetre irányuló terheléselosztót |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate az Internet felé néző rendelkező terheléselosztó IPv6 Azure Resource Manager használatával hello Azure parancssori felület"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
keywords: "IPv6-alapú, azure load balancer, kettős verem, nyilvános IP-cím, natív ipv6, mobil, iot"
ms.assetid: a1957c9c-9c1d-423e-9d5c-d71449bc1f37
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 7ff75ac90d74a74e3d0c27672b36fbd955a086a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-hello-azure-cli"></a><span data-ttu-id="5cb9e-104">Hozzon létre az Internet felé néző terheléselosztón az IPv6 az Azure Resource Manager hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="5cb9e-104">Create an Internet facing load balancer with IPv6 in Azure Resource Manager using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5cb9e-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5cb9e-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="5cb9e-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5cb9e-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="5cb9e-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="5cb9e-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="5cb9e-108">Az Azure Load Balancer 4. szintű (TCP, UDP) terheléselosztónak minősül.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="5cb9e-109">hello terheléselosztó bejövő forgalmat a felhőszolgáltatások kifogástalan szolgáltatáspéldány vagy a load balancer csoportban lévő virtuális gépek között elosztásával magas rendelkezésre állást biztosít.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-109">hello load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="5cb9e-110">Az Azure Load Balancer a szolgáltatásokat több portra vagy több IP-címre, illetve portokra és IP-címekre egyaránt továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="5cb9e-111">Központi telepítési példa</span><span class="sxs-lookup"><span data-stu-id="5cb9e-111">Example deployment scenario</span></span>

<span data-ttu-id="5cb9e-112">hello következő diagram azt ábrázolja, hello terheléselosztási megoldás üzembe helyezéséhez sablonnal hello példa a cikkben.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-112">hello following diagram illustrates hello load balancing solution being deployed using hello example template described in this article.</span></span>

![Terheléselosztói forgatókönyv](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

<span data-ttu-id="5cb9e-114">Ebben a forgatókönyvben a következő Azure-erőforrások hello hozza létre:</span><span class="sxs-lookup"><span data-stu-id="5cb9e-114">In this scenario you will create hello following Azure resources:</span></span>

* <span data-ttu-id="5cb9e-115">két virtuális gépek (VM)</span><span class="sxs-lookup"><span data-stu-id="5cb9e-115">two virtual machines (VMs)</span></span>
* <span data-ttu-id="5cb9e-116">az egyes virtuális gépekhez rendelt IPv4 és IPv6-címmel rendelkező virtuális hálózati illesztő</span><span class="sxs-lookup"><span data-stu-id="5cb9e-116">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>
* <span data-ttu-id="5cb9e-117">egy internetre irányuló terheléselosztót egy IPv4-és IPv6 nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="5cb9e-117">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="5cb9e-118">egy rendelkezésre állási csoport toothat hello két virtuális gépeket tartalmaz</span><span class="sxs-lookup"><span data-stu-id="5cb9e-118">an Availability Set toothat contains hello two VMs</span></span>
* <span data-ttu-id="5cb9e-119">két terheléselosztási szabályok toomap hello nyilvános virtuális IP-címek toohello titkos végpontok betöltése</span><span class="sxs-lookup"><span data-stu-id="5cb9e-119">two load balancing rules toomap hello public VIPs toohello private endpoints</span></span>

## <a name="deploying-hello-solution-using-hello-azure-cli"></a><span data-ttu-id="5cb9e-120">Hello Azure parancssori felület használatával hello megoldás telepítése</span><span class="sxs-lookup"><span data-stu-id="5cb9e-120">Deploying hello solution using hello Azure CLI</span></span>

<span data-ttu-id="5cb9e-121">hello lépések bemutatják, hogyan toocreate egy internetre irányuló terheléselosztót a CLI Azure Resource Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-121">hello following steps show how toocreate an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="5cb9e-122">Az Azure Resource Manager az egyes erőforrások jön létre, és egyenként konfigurálni, majd tegye együtt toocreate erőforrás.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-122">With Azure Resource Manager, each resource is created and configured individually, and then put together toocreate a resource.</span></span>

<span data-ttu-id="5cb9e-123">a terheléselosztó toodeploy, létrehozása és beállítása a következő objektumok hello:</span><span class="sxs-lookup"><span data-stu-id="5cb9e-123">toodeploy a load balancer, you create and configure hello following objects:</span></span>

* <span data-ttu-id="5cb9e-124">Előtér-IP-konfiguráció – a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="5cb9e-125">Háttér-címkészlet - hello virtuális gépek tooreceive hálózati forgalmat a hello terheléselosztó hálózati adapterek (NIC) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-125">Back-end address pool - contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="5cb9e-126">Terheléselosztási szabályok - hello load balancer tooport hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-126">Load balancing rules - contains rules mapping a public port on hello load balancer tooport in hello back-end address pool.</span></span>
* <span data-ttu-id="5cb9e-127">Bejövő NAT-szabályok – hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-127">Inbound NAT rules - contains rules mapping a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="5cb9e-128">Mintavétel - állapotfigyelő használ mintavételi készleten toocheck rendelkezésre állása hello háttér címkészletet virtuálisgép-példánya tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-128">Probes - contains health probes used toocheck availability of virtual machines instances in hello back-end address pool.</span></span>

<span data-ttu-id="5cb9e-129">A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-your-cli-environment-toouse-azure-resource-manager"></a><span data-ttu-id="5cb9e-130">A parancssori felület környezet toouse Azure Resource Manager beállítása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-130">Set up your CLI environment toouse Azure Resource Manager</span></span>

<span data-ttu-id="5cb9e-131">Ehhez a példához jelenleg fut hello CLI-eszközeit egy PowerShell-parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-131">For this example, we are running hello CLI tools in a PowerShell command window.</span></span> <span data-ttu-id="5cb9e-132">Hello Azure PowerShell-parancsmagok nem használjuk, de a PowerShell parancsfájl-kezelési képességek tooimprove olvashatóság és újbóli használjuk.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-132">We are not using hello Azure PowerShell cmdlets but we use PowerShell's scripting capabilities tooimprove readability and reuse.</span></span>

1. <span data-ttu-id="5cb9e-133">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-133">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="5cb9e-134">Futtassa a hello **azure config mód** tooswitch tooResource Manager üzemmód.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-134">Run hello **azure config mode** command tooswitch tooResource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="5cb9e-135">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="5cb9e-135">Expected output:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="5cb9e-136">Jelentkezzen be tooAzure, és az előfizetések listáját.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-136">Sign in tooAzure and get a list of subscriptions.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="5cb9e-137">Adja meg, amikor a rendszer kéri az Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-137">Enter your Azure credentials when prompted.</span></span>

    ```azurecli
    azure account list
    ```

    <span data-ttu-id="5cb9e-138">Válassza ki a kívánt hello előfizetés toouse.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-138">Pick hello subscription you want toouse.</span></span> <span data-ttu-id="5cb9e-139">Jegyezze fel a hello előfizetési azonosító hello következő lépéshez.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-139">Make note of hello subscription Id for hello next step.</span></span>

4. <span data-ttu-id="5cb9e-140">Állítsa be a PowerShell változók hello parancssori felület parancsait való használatra.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-140">Set up PowerShell variables for use with hello CLI commands.</span></span>

    ```powershell
    $subscriptionid = "########-####-####-####-############"  # enter subscription id
    $location = "southcentralus"
    $rgName = "pscontosorg1southctrlus09152016"
    $vnetName = "contosoIPv4Vnet"
    $vnetPrefix = "10.0.0.0/16"
    $subnet1Name = "clicontosoIPv4Subnet1"
    $subnet1Prefix = "10.0.0.0/24"
    $subnet2Name = "clicontosoIPv4Subnet2"
    $subnet2Prefix = "10.0.1.0/24"
    $dnsLabel = "contoso09152016"
    $lbName = "myIPv4IPv6Lb"
    ```

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a><span data-ttu-id="5cb9e-141">Hozzon létre egy erőforráscsoportot, egy adott terheléselosztóhoz, egy virtuális hálózatot és alhálózatok</span><span class="sxs-lookup"><span data-stu-id="5cb9e-141">Create a resource group, a load balancer, a virtual network, and subnets</span></span>

1. <span data-ttu-id="5cb9e-142">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="5cb9e-142">Create a resource group</span></span>

    ```azurecli
    azure group create $rgName $location
    ```

2. <span data-ttu-id="5cb9e-143">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-143">Create a load balancer</span></span>

    ```azurecli
    $lb = azure network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. <span data-ttu-id="5cb9e-144">Hozzon létre egy virtuális hálózatot (VNet).</span><span class="sxs-lookup"><span data-stu-id="5cb9e-144">Create a virtual network (VNet).</span></span>

    ```azurecli
    $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

    <span data-ttu-id="5cb9e-145">Ez a virtuális hálózat létrehozása két alhálózatból állnak.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-145">Create two subnets in this VNet.</span></span>

    ```azurecli
    $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-hello-front-end-pool"></a><span data-ttu-id="5cb9e-146">Nyilvános IP-címek hello előtér-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-146">Create public IP addresses for hello front-end pool</span></span>

1. <span data-ttu-id="5cb9e-147">Hello PowerShell változók beállítása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-147">Set up hello PowerShell variables</span></span>

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. <span data-ttu-id="5cb9e-148">Hozzon létre egy nyilvános IP-hello előtér-IP címkészletet.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-148">Create a public IP address hello front-end IP pool.</span></span>

    ```azurecli
    $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="5cb9e-149">hello terheléselosztó hello tartománycímke hello nyilvános IP-használja, mint a teljes Tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-149">hello load balancer uses hello domain label of hello public IP as its FQDN.</span></span> <span data-ttu-id="5cb9e-150">A klasszikus üzembe helyezési, hello felhőszolgáltatás neve használja, mint a terheléselosztó FQDN hello módosítást.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-150">This a change from classic deployment, which uses hello cloud service name as hello load balancer FQDN.</span></span>
    > <span data-ttu-id="5cb9e-151">Ebben a példában hello FQDN-je *contoso09152016.southcentralus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-151">In this example, hello FQDN is *contoso09152016.southcentralus.cloudapp.azure.com*.</span></span>

## <a name="create-front-end-and-back-end-pools"></a><span data-ttu-id="5cb9e-152">Hozzon létre az előtér- és készletek</span><span class="sxs-lookup"><span data-stu-id="5cb9e-152">Create front-end and back-end pools</span></span>

<span data-ttu-id="5cb9e-153">Ebben a példában a hello előtér-IP-készlet, amely megkapja a bejövő hálózati forgalom hello hello terheléselosztón és hello háttér IP-készlet ahol hello előtér-készlet küldi hello hálózati forgalmat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-153">This example creates hello front-end IP pool that receives hello incoming network traffic on hello load balancer and hello back-end IP pool where hello front-end pool sends hello load balanced network traffic.</span></span>

1. <span data-ttu-id="5cb9e-154">Hello PowerShell változók beállítása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-154">Set up hello PowerShell variables</span></span>

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. <span data-ttu-id="5cb9e-155">Hozzon létre egy társítása hello nyilvános IP-cím létrehozott hello előző lépésben és hello terheléselosztó előtér-IP-címkészletet.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-155">Create a front-end IP pool associating hello public IP created in hello previous step and hello load balancer.</span></span>

    ```azurecli
    $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-hello-probe-nat-rules-and-lb-rules"></a><span data-ttu-id="5cb9e-156">Hello mintavételi, a NAT-szabályok és a Terheléselosztó szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-156">Create hello probe, NAT rules, and LB rules</span></span>

<span data-ttu-id="5cb9e-157">Ebben a példában a következő elemek hello hoz létre:</span><span class="sxs-lookup"><span data-stu-id="5cb9e-157">This example creates hello following items:</span></span>

* <span data-ttu-id="5cb9e-158">a mintavétel szabály toocheck a kapcsolat tooTCP 80-as port</span><span class="sxs-lookup"><span data-stu-id="5cb9e-158">a probe rule toocheck for connectivity tooTCP port 80</span></span>
* <span data-ttu-id="5cb9e-159">a NAT-szabályok tootranslate minden bejövő forgalom a 3389-es port tooport 3389-es RDP<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="5cb9e-159">a NAT rule tootranslate all incoming traffic on port 3389 tooport 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="5cb9e-160">a NAT-szabályok tootranslate minden bejövő forgalom a 3389-es port 3391 tooport RDP<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="5cb9e-160">a NAT rule tootranslate all incoming traffic on port 3391 tooport 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="5cb9e-161">a load balancer szabály toobalance minden bejövő forgalmat a 80-as port tooport 80 hello a címek hello háttér-készletben.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-161">a load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool.</span></span>

<span data-ttu-id="5cb9e-162"><sup>1</sup> NAT-szabályok adott virtuálisgép-példányt társított tooa hello terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-162"><sup>1</sup> NAT rules are associated tooa specific virtual machine instance behind hello load balancer.</span></span> <span data-ttu-id="5cb9e-163">hello hálózati 3389-es portot a bejövő adatforgalom toohello adott virtuális gép és a társított hello NAT-szabály portja.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-163">hello network traffic arriving on port 3389 is sent toohello specific virtual machine and port associated with hello NAT rule.</span></span> <span data-ttu-id="5cb9e-164">A NAT-szabályhoz meg kell adnia egy protokollt (UDP vagy TCP).</span><span class="sxs-lookup"><span data-stu-id="5cb9e-164">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="5cb9e-165">Mindkét protokollt nem lehet toohello hozzárendelve ugyanehhez a porthoz.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-165">Both protocols can't be assigned toohello same port.</span></span>

1. <span data-ttu-id="5cb9e-166">Hello PowerShell változók beállítása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-166">Set up hello PowerShell variables</span></span>

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. <span data-ttu-id="5cb9e-167">Hello mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-167">Create hello probe</span></span>

    <span data-ttu-id="5cb9e-168">hello alábbi példakód létrehozza a TCP mintavételi modul, amely ellenőrzi a kapcsolat tooback közötti TCP 80-as port 15 másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-168">hello following example creates a TCP probe that checks for connectivity tooback-end TCP port 80 every 15 seconds.</span></span> <span data-ttu-id="5cb9e-169">Azt jelöli hello háttér-erőforrás nem érhető el két egymást követő hibák után.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-169">It will mark hello back-end resource unavailable after two consecutive failures.</span></span>

    ```azurecli
    $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. <span data-ttu-id="5cb9e-170">Bejövő NAT-szabályok, amelyek lehetővé teszik az RDP-kapcsolatok toohello háttér-erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-170">Create inbound NAT rules that allow RDP connections toohello back-end resources</span></span>

    ```azurecli
    $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. <span data-ttu-id="5cb9e-171">Hozzon létre egy terhelés-kiegyenlítő szabályokat, amelyek forgalom toodifferent háttér-portok attól függően, hogy melyik előtér kapott hello kérelem elküldése</span><span class="sxs-lookup"><span data-stu-id="5cb9e-171">Create a load balancer rules that send traffic toodifferent back-end ports depending on which front end received hello request</span></span>

    ```azurecli
    $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. <span data-ttu-id="5cb9e-172">Ellenőrizze a beállításokat</span><span class="sxs-lookup"><span data-stu-id="5cb9e-172">Check your settings</span></span>

    ```azurecli
    azure network lb show --resource-group $rgName --name $lbName
    ```

    <span data-ttu-id="5cb9e-173">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="5cb9e-173">Expected output:</span></span>

        info:    Executing command network lb show
        info:    Looking up hello load balancer "myIPv4IPv6Lb"
        data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/pscontosorg1southctrlus09152016/providers/Microsoft.Network/loadBalancers/myIPv4IPv6Lb
        data:    Name                            : myIPv4IPv6Lb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : southcentralus
        data:    Provisioning state              : Succeeded
        data:
        data:    Frontend IP configurations:
        data:    Name             Provisioning state  Private IP allocation  Private IP   Subnet  Public IP
        data:    ---------------  ------------------  ---------------------  -----------  ------  ---------
        data:    FrontendVipIPv4  Succeeded           Dynamic                                     myIPv4Vip
        data:    FrontendVipIPv6  Succeeded           Dynamic                                     myIPv6Vip
        data:
        data:    Probes:
        data:    Name                 Provisioning state  Protocol  Port  Path  Interval  Count
        data:    -------------------  ------------------  --------  ----  ----  --------  -----
        data:    ProbeForIPv4AndIPv6  Succeeded           Tcp       80          15        2
        data:
        data:    Backend Address Pools:
        data:    Name             Provisioning state
        data:    ---------------  ------------------
        data:    BackendPoolIPv4  Succeeded
        data:    BackendPoolIPv6  Succeeded
        data:
        data:    Load Balancing Rules:
        data:    Name                  Provisioning state  Load distribution  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    --------------------  ------------------  -----------------  --------  -------------  ------------  ------------------  -----------------------
        data:    LBRuleForIPv4-Port80  Succeeded           Default            Tcp       80             80            false               4
        data:    LBRuleForIPv6-Port80  Succeeded           Default            Tcp       80             8080          false               4
        data:
        data:    Inbound NAT Rules:
        data:    Name                 Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes
        data:    -------------------  ------------------  --------  -------------  ------------  ------------------  -----------------------
        data:    NatRule-For-Rdp-VM1  Succeeded           Tcp       3389           3389          false               4
        data:    NatRule-For-Rdp-VM2  Succeeded           Tcp       3391           3389          false               4
        info:    network lb show

## <a name="create-nics"></a><span data-ttu-id="5cb9e-174">Hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-174">Create NICs</span></span>

<span data-ttu-id="5cb9e-175">Hálózati adaptert létrehozni, és rendelje hozzá őket tooNAT szabályok, load balancer szabályok és mintavételek menüpontban.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-175">Create NICs and associate them tooNAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="5cb9e-176">Hello PowerShell változók beállítása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-176">Set up hello PowerShell variables</span></span>

    ```powershell
    $nic1Name = "myIPv4IPv6Nic1"
    $nic2Name = "myIPv4IPv6Nic2"
    $subnet1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet1Name"
    $subnet2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgName/providers/Microsoft.Network/VirtualNetworks/$vnetName/subnets/$subnet2Name"
    $backendAddressPoolV4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV4Name"
    $backendAddressPoolV6Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/backendAddressPools/$backendAddressPoolV6Name"
    $natRule1V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule1V4Name"
    $natRule2V4Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/loadbalancers/$lbName/inboundNatRules/$natRule2V4Name"
    ```

2. <span data-ttu-id="5cb9e-177">Hozzon létre egy minden háttér hálózati Adapterhez, és adja hozzá az IPv6 konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-177">Create a NIC for each back-end and add an IPv6 configuration.</span></span>

    ```azurecli
    $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-hello-back-end-vm-resources-and-attach-each-nic"></a><span data-ttu-id="5cb9e-178">Hello háttér-Virtuálisgép-erőforrások létrehozása és csatolása az egyes hálózati adapterek</span><span class="sxs-lookup"><span data-stu-id="5cb9e-178">Create hello back-end VM resources and attach each NIC</span></span>

<span data-ttu-id="5cb9e-179">virtuális gépek toocreate, rendelkeznie kell egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-179">toocreate VMs, you must have a storage account.</span></span> <span data-ttu-id="5cb9e-180">A terheléselosztás hello virtuális gépek rendelkezésre állási csoport tagjai toobe kell.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-180">For load balancing, hello VMs need toobe members of an availability set.</span></span> <span data-ttu-id="5cb9e-181">Virtuális gépek létrehozásával kapcsolatos további információkért lásd: [hozzon létre egy Azure virtuális gép PowerShell használatával](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="5cb9e-181">For more information about creating VMs, see [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="5cb9e-182">Hello PowerShell változók beállítása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-182">Set up hello PowerShell variables</span></span>

    ```powershell
    $storageAccountName = "ps08092016v6sa0"
    $availabilitySetName = "myIPv4IPv6AvailabilitySet"
    $vm1Name = "myIPv4IPv6VM1"
    $vm2Name = "myIPv4IPv6VM2"
    $nic1Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic1Name"
    $nic2Id = "/subscriptions/$subscriptionid/resourceGroups/$rgname/providers/Microsoft.Network/networkInterfaces/$nic2Name"
    $disk1Name = "WindowsVMosDisk1"
    $disk2Name = "WindowsVMosDisk2"
    $osDisk1Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk1Name.vhd"
    $osDisk2Uri = "https://$storageAccountName.blob.core.windows.net/vhds/$disk2Name.vhd"
    $imageurn "MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest"
    $vmUserName = "vmUser"
    $mySecurePassword = "PlainTextPassword*1"
    ```

    > [!WARNING]
    > <span data-ttu-id="5cb9e-183">A példa hello felhasználónév és jelszó nyílt szövegként hello virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-183">This example uses hello username and password for hello VMs in clear text.</span></span> <span data-ttu-id="5cb9e-184">Megfelelő gondot kell fordítani, amikor használatával-felhasználó hitelesítő adatait a hello törölje.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-184">Appropriate care must be taken when using credentials in hello clear.</span></span> <span data-ttu-id="5cb9e-185">Egy biztonságosabb módszer a PowerShell hitelesítő adatok kezelésére, lásd: hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-185">For a more secure method of handling credentials in PowerShell, see hello [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

2. <span data-ttu-id="5cb9e-186">Hello tárolási fiók és a rendelkezésre állási készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-186">Create hello storage account and availability set</span></span>

    <span data-ttu-id="5cb9e-187">Használhat meglévő tárfiókot hello virtuális gépek létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-187">You may use an existing storage account when you create hello VMs.</span></span> <span data-ttu-id="5cb9e-188">a következő parancs hello hoz létre egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-188">hello following command creates a new storage account.</span></span>

    ```azurecli
    $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

    <span data-ttu-id="5cb9e-189">Ezután hozzon létre hello rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="5cb9e-189">Next, create hello availability set.</span></span>

    ```azurecli
    $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location
    ```

3. <span data-ttu-id="5cb9e-190">Hello virtuális gépek létrehozására hello társított hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="5cb9e-190">Create hello virtual machines with hello associated NICs</span></span>

    ```azurecli
    $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a><span data-ttu-id="5cb9e-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5cb9e-191">Next steps</span></span>

[<span data-ttu-id="5cb9e-192">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="5cb9e-192">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="5cb9e-193">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-193">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="5cb9e-194">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5cb9e-194">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
