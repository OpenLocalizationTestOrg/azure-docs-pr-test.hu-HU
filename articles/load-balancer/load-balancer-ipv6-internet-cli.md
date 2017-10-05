---
title: "Hozzon létre egy internetre irányuló terheléselosztót IPv6 - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ismerje meg az Internet felé néző IPv6 az Azure Resource Manager az Azure parancssori felülettel rendelkező terheléselosztó létrehozása"
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
ms.openlocfilehash: efb4771800c42df544c3cc37d1d164045fdcaf3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internet-facing-load-balancer-with-ipv6-in-azure-resource-manager-using-the-azure-cli"></a><span data-ttu-id="b5e91-104">Internet felé néző IPv6 az Azure Resource Manager az Azure parancssori felülettel rendelkező terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5e91-104">Create an Internet facing load balancer with IPv6 in Azure Resource Manager using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="b5e91-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b5e91-105">PowerShell</span></span>](load-balancer-ipv6-internet-ps.md)
> * [<span data-ttu-id="b5e91-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b5e91-106">Azure CLI</span></span>](load-balancer-ipv6-internet-cli.md)
> * [<span data-ttu-id="b5e91-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="b5e91-107">Template</span></span>](load-balancer-ipv6-internet-template.md)

<span data-ttu-id="b5e91-108">Az Azure Load Balancer 4. szintű (TCP, UDP) terheléselosztónak minősül.</span><span class="sxs-lookup"><span data-stu-id="b5e91-108">An Azure load balancer is a Layer-4 (TCP, UDP) load balancer.</span></span> <span data-ttu-id="b5e91-109">A terheléselosztó a felhőszolgáltatások vagy virtuális gépek kifogástalan állapotú szolgáltatási példányai között osztja meg a bejövő forgalmat egy terheléselosztói készletben, és ezáltal biztosítja a magas rendelkezésre állást.</span><span class="sxs-lookup"><span data-stu-id="b5e91-109">The load balancer provides high availability by distributing incoming traffic among healthy service instances in cloud services or virtual machines in a load balancer set.</span></span> <span data-ttu-id="b5e91-110">Az Azure Load Balancer a szolgáltatásokat több portra vagy több IP-címre, illetve portokra és IP-címekre egyaránt továbbíthatja.</span><span class="sxs-lookup"><span data-stu-id="b5e91-110">Azure Load Balancer can also present those services on multiple ports, multiple IP addresses, or both.</span></span>

## <a name="example-deployment-scenario"></a><span data-ttu-id="b5e91-111">Központi telepítési példa</span><span class="sxs-lookup"><span data-stu-id="b5e91-111">Example deployment scenario</span></span>

<span data-ttu-id="b5e91-112">A következő ábra szemlélteti a terheléselosztási megoldás üzembe helyezéséhez a jelen cikkben ismertetett példa sablonja segítségével.</span><span class="sxs-lookup"><span data-stu-id="b5e91-112">The following diagram illustrates the load balancing solution being deployed using the example template described in this article.</span></span>

![Terheléselosztói forgatókönyv](./media/load-balancer-ipv6-internet-cli/lb-ipv6-scenario-cli.png)

<span data-ttu-id="b5e91-114">Ebben a forgatókönyvben a következő Azure-erőforrások hoz létre:</span><span class="sxs-lookup"><span data-stu-id="b5e91-114">In this scenario you will create the following Azure resources:</span></span>

* <span data-ttu-id="b5e91-115">két virtuális gépek (VM)</span><span class="sxs-lookup"><span data-stu-id="b5e91-115">two virtual machines (VMs)</span></span>
* <span data-ttu-id="b5e91-116">az egyes virtuális gépekhez rendelt IPv4 és IPv6-címmel rendelkező virtuális hálózati illesztő</span><span class="sxs-lookup"><span data-stu-id="b5e91-116">a virtual network interface for each VM with both IPv4 and IPv6 addresses assigned</span></span>
* <span data-ttu-id="b5e91-117">egy internetre irányuló terheléselosztót egy IPv4-és IPv6 nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="b5e91-117">an Internet-facing Load Balancer with an IPv4 and an IPv6 Public IP address</span></span>
* <span data-ttu-id="b5e91-118">a két virtuális gépek rendelkezésre állási csoport számára, amely tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b5e91-118">an Availability Set to that contains the two VMs</span></span>
* <span data-ttu-id="b5e91-119">két terheléselosztási szabályok a nyilvános virtuális IP-címek hozzárendelését a saját végpontokhoz való betöltése</span><span class="sxs-lookup"><span data-stu-id="b5e91-119">two load balancing rules to map the public VIPs to the private endpoints</span></span>

## <a name="deploying-the-solution-using-the-azure-cli"></a><span data-ttu-id="b5e91-120">A megoldás üzembe helyezése az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="b5e91-120">Deploying the solution using the Azure CLI</span></span>

<span data-ttu-id="b5e91-121">A következő lépések bemutatják, hogyan hozható létre internetkapcsolattal rendelkező terheléselosztó az Azure Resource Manager parancssori felületének használatával.</span><span class="sxs-lookup"><span data-stu-id="b5e91-121">The following steps show how to create an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="b5e91-122">Az Azure Resource Manager lehetővé teszi, hogy az egyes erőforrások konfigurálása egyenként történjen, majd az összerakásukkal jöjjön létre egy erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b5e91-122">With Azure Resource Manager, each resource is created and configured individually, and then put together to create a resource.</span></span>

<span data-ttu-id="b5e91-123">Terheléselosztó telepítéséhez hozzon létre, és adja meg a következő objektumok:</span><span class="sxs-lookup"><span data-stu-id="b5e91-123">To deploy a load balancer, you create and configure the following objects:</span></span>

* <span data-ttu-id="b5e91-124">Előtér-IP-konfiguráció – a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz.</span><span class="sxs-lookup"><span data-stu-id="b5e91-124">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="b5e91-125">Háttércímkészlet – hálózati adaptereket (NIC) tartalmaz, amelyek segítségével a virtuális gépek fogadhatják a terheléselosztóról érkező hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="b5e91-125">Back-end address pool - contains network interfaces (NICs) for the virtual machines to receive network traffic from the load balancer.</span></span>
* <span data-ttu-id="b5e91-126">Terheléselosztási szabályok – olyan szabályokat tartalmaz, amelyek a terheléselosztó nyilvános portjait rendelik hozzá háttércímkészlet portjaihoz.</span><span class="sxs-lookup"><span data-stu-id="b5e91-126">Load balancing rules - contains rules mapping a public port on the load balancer to port in the back-end address pool.</span></span>
* <span data-ttu-id="b5e91-127">Bejövő NAT-szabályok – olyan szabályokat tartalmaz, amelyek a terheléselosztó nyilvános portjait rendelik hozzá egy adott virtuális gép portjához a háttércímkészletben.</span><span class="sxs-lookup"><span data-stu-id="b5e91-127">Inbound NAT rules - contains rules mapping a public port on the load balancer to a port for a specific virtual machine in the back-end address pool.</span></span>
* <span data-ttu-id="b5e91-128">Mintavételezők – állapotfigyelő mintavételezőket tartalmaz, amelyek a virtuálisgép-példányok rendelkezésre állását ellenőrzik a háttércímkészletben.</span><span class="sxs-lookup"><span data-stu-id="b5e91-128">Probes - contains health probes used to check availability of virtual machines instances in the back-end address pool.</span></span>

<span data-ttu-id="b5e91-129">A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b5e91-129">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-your-cli-environment-to-use-azure-resource-manager"></a><span data-ttu-id="b5e91-130">Állítsa be a parancssori felület környezetet az Azure Resource Manager használatára</span><span class="sxs-lookup"><span data-stu-id="b5e91-130">Set up your CLI environment to use Azure Resource Manager</span></span>

<span data-ttu-id="b5e91-131">Ehhez a példához jelenleg fut a CLI-eszközeit egy PowerShell-parancsablakot.</span><span class="sxs-lookup"><span data-stu-id="b5e91-131">For this example, we are running the CLI tools in a PowerShell command window.</span></span> <span data-ttu-id="b5e91-132">Az Azure PowerShell-parancsmagok nem használjuk, de a PowerShell parancsfájl-kezelési képességek olvashatóság és újbóli javítására használjuk.</span><span class="sxs-lookup"><span data-stu-id="b5e91-132">We are not using the Azure PowerShell cmdlets but we use PowerShell's scripting capabilities to improve readability and reuse.</span></span>

1. <span data-ttu-id="b5e91-133">Ha még sosem használta az Azure CLI-t, akkor tekintse meg [Install and Configure the Azure CLI](../cli-install-nodejs.md) (Az Azure CLI telepítése és konfigurálása) részt, és kövesse az utasításokat addig a pontig, ahol ki kell választania az Azure-fiókot és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="b5e91-133">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="b5e91-134">Futtassa a **azure config mód** parancs futtatásával váltson Resource Manager módra.</span><span class="sxs-lookup"><span data-stu-id="b5e91-134">Run the **azure config mode** command to switch to Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="b5e91-135">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="b5e91-135">Expected output:</span></span>

        info:    New mode is arm

3. <span data-ttu-id="b5e91-136">Jelentkezzen be az Azure-ba, és az előfizetések listáját.</span><span class="sxs-lookup"><span data-stu-id="b5e91-136">Sign in to Azure and get a list of subscriptions.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="b5e91-137">Adja meg, amikor a rendszer kéri az Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="b5e91-137">Enter your Azure credentials when prompted.</span></span>

    ```azurecli
    azure account list
    ```

    <span data-ttu-id="b5e91-138">Válassza ki a használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="b5e91-138">Pick the subscription you want to use.</span></span> <span data-ttu-id="b5e91-139">Jegyezze fel az előfizetés-azonosítója a következő lépéshez.</span><span class="sxs-lookup"><span data-stu-id="b5e91-139">Make note of the subscription Id for the next step.</span></span>

4. <span data-ttu-id="b5e91-140">A parancssori felület parancsait való használatra PowerShell változók beállítása.</span><span class="sxs-lookup"><span data-stu-id="b5e91-140">Set up PowerShell variables for use with the CLI commands.</span></span>

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

## <a name="create-a-resource-group-a-load-balancer-a-virtual-network-and-subnets"></a><span data-ttu-id="b5e91-141">Hozzon létre egy erőforráscsoportot, egy adott terheléselosztóhoz, egy virtuális hálózatot és alhálózatok</span><span class="sxs-lookup"><span data-stu-id="b5e91-141">Create a resource group, a load balancer, a virtual network, and subnets</span></span>

1. <span data-ttu-id="b5e91-142">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b5e91-142">Create a resource group</span></span>

    ```azurecli
    azure group create $rgName $location
    ```

2. <span data-ttu-id="b5e91-143">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5e91-143">Create a load balancer</span></span>

    ```azurecli
    $lb = azure network lb create --resource-group $rgname --location $location --name $lbName
    ```

3. <span data-ttu-id="b5e91-144">Hozzon létre egy virtuális hálózatot (VNet).</span><span class="sxs-lookup"><span data-stu-id="b5e91-144">Create a virtual network (VNet).</span></span>

    ```azurecli
    $vnet = azure network vnet create  --resource-group $rgname --name $vnetName --location $location --address-prefixes $vnetPrefix
    ```

    <span data-ttu-id="b5e91-145">Ez a virtuális hálózat létrehozása két alhálózatból állnak.</span><span class="sxs-lookup"><span data-stu-id="b5e91-145">Create two subnets in this VNet.</span></span>

    ```azurecli
    $subnet1 = azure network vnet subnet create --resource-group $rgname --name $subnet1Name --address-prefix $subnet1Prefix --vnet-name $vnetName
    $subnet2 = azure network vnet subnet create --resource-group $rgname --name $subnet2Name --address-prefix $subnet2Prefix --vnet-name $vnetName
    ```

## <a name="create-public-ip-addresses-for-the-front-end-pool"></a><span data-ttu-id="b5e91-146">Nyilvános IP-címeket az előtér-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5e91-146">Create public IP addresses for the front-end pool</span></span>

1. <span data-ttu-id="b5e91-147">A PowerShell változók beállítása</span><span class="sxs-lookup"><span data-stu-id="b5e91-147">Set up the PowerShell variables</span></span>

    ```powershell
    $publicIpv4Name = "myIPv4Vip"
    $publicIpv6Name = "myIPv6Vip"
    ```

2. <span data-ttu-id="b5e91-148">Hozzon létre egy nyilvános IP-címet az előtér-IP-címkészletet.</span><span class="sxs-lookup"><span data-stu-id="b5e91-148">Create a public IP address the front-end IP pool.</span></span>

    ```azurecli
    $publicipV4 = azure network public-ip create --resource-group $rgname --name $publicIpv4Name --location $location --ip-version IPv4 --allocation-method Dynamic --domain-name-label $dnsLabel
    $publicipV6 = azure network public-ip create --resource-group $rgname --name $publicIpv6Name --location $location --ip-version IPv6 --allocation-method Dynamic --domain-name-label $dnsLabel
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="b5e91-149">A terheléselosztó a nyilvános IP-cím tartománycímkéjét használja FQDN-ként.</span><span class="sxs-lookup"><span data-stu-id="b5e91-149">The load balancer uses the domain label of the public IP as its FQDN.</span></span> <span data-ttu-id="b5e91-150">Ez a változás a klasszikus üzembe helyezési, amely használja a felhőalapú szolgáltatás nevezze el a terheléselosztó FQDN.</span><span class="sxs-lookup"><span data-stu-id="b5e91-150">This a change from classic deployment, which uses the cloud service name as the load balancer FQDN.</span></span>
    > <span data-ttu-id="b5e91-151">Ebben a példában az FQDN-je *contoso09152016.southcentralus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="b5e91-151">In this example, the FQDN is *contoso09152016.southcentralus.cloudapp.azure.com*.</span></span>

## <a name="create-front-end-and-back-end-pools"></a><span data-ttu-id="b5e91-152">Hozzon létre az előtér- és készletek</span><span class="sxs-lookup"><span data-stu-id="b5e91-152">Create front-end and back-end pools</span></span>

<span data-ttu-id="b5e91-153">Ebben a példában az előtér-IP-címkészlet, amely megkapja a bejövő hálózati forgalmat a terheléselosztó és a háttér IP-készlet, amennyiben az előtér-készlet elküldi a hálózati forgalmat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b5e91-153">This example creates the front-end IP pool that receives the incoming network traffic on the load balancer and the back-end IP pool where the front-end pool sends the load balanced network traffic.</span></span>

1. <span data-ttu-id="b5e91-154">A PowerShell változók beállítása</span><span class="sxs-lookup"><span data-stu-id="b5e91-154">Set up the PowerShell variables</span></span>

    ```powershell
    $frontendV4Name = "FrontendVipIPv4"
    $frontendV6Name = "FrontendVipIPv6"
    $backendAddressPoolV4Name = "BackendPoolIPv4"
    $backendAddressPoolV6Name = "BackendPoolIPv6"
    ```

2. <span data-ttu-id="b5e91-155">Hozzon létre egy előtér-IP-címkészletet az előző lépésben létrehozott nyilvános IP-címet társítva a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="b5e91-155">Create a front-end IP pool associating the public IP created in the previous step and the load balancer.</span></span>

    ```azurecli
    $frontendV4 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV4Name --public-ip-name $publicIpv4Name --lb-name $lbName
    $frontendV6 = azure network lb frontend-ip create --resource-group $rgname --name $frontendV6Name --public-ip-name $publicIpv6Name --lb-name $lbName
    $backendAddressPoolV4 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV4Name --lb-name $lbName
    $backendAddressPoolV6 = azure network lb address-pool create --resource-group $rgname --name $backendAddressPoolV6Name --lb-name $lbName
    ```

## <a name="create-the-probe-nat-rules-and-lb-rules"></a><span data-ttu-id="b5e91-156">A mintavételi, a NAT-szabályok és a Terheléselosztó szabályok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5e91-156">Create the probe, NAT rules, and LB rules</span></span>

<span data-ttu-id="b5e91-157">Ez a példa a következő elemeket hozza létre:</span><span class="sxs-lookup"><span data-stu-id="b5e91-157">This example creates the following items:</span></span>

* <span data-ttu-id="b5e91-158">Ellenőrizze a kapcsolatot a 80-as TCP-port mintavételi szabály</span><span class="sxs-lookup"><span data-stu-id="b5e91-158">a probe rule to check for connectivity to TCP port 80</span></span>
* <span data-ttu-id="b5e91-159">a NAT-szabály RDP a 3389-es porton keresztül 3389-es port minden bejövő forgalom lefordítani<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="b5e91-159">a NAT rule to translate all incoming traffic on port 3389 to port 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="b5e91-160">a NAT-szabály RDP a 3389-es port 3391 porton minden bejövő forgalom lefordítani<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="b5e91-160">a NAT rule to translate all incoming traffic on port 3391 to port 3389 for RDP<sup>1</sup></span></span>
* <span data-ttu-id="b5e91-161">egy terheléselosztó-szabályt, amely elosztja a 80-as porton bejövő összes forgalmat a háttér-címkészletben szereplő címekhez tartozó 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="b5e91-161">a load balancer rule to balance all incoming traffic on port 80 to port 80 on the addresses in the back-end pool.</span></span>

<span data-ttu-id="b5e91-162"><sup>1</sup> A NAT-szabályok a terheléselosztó mögött található adott virtuálisgép-példányhoz vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="b5e91-162"><sup>1</sup> NAT rules are associated to a specific virtual machine instance behind the load balancer.</span></span> <span data-ttu-id="b5e91-163">A 3389-es portot a bejövő hálózati forgalom érkezik, az adott virtuális gép és a NAT-szabályhoz tartozó port.</span><span class="sxs-lookup"><span data-stu-id="b5e91-163">The network traffic arriving on port 3389 is sent to the specific virtual machine and port associated with the NAT rule.</span></span> <span data-ttu-id="b5e91-164">A NAT-szabályhoz meg kell adnia egy protokollt (UDP vagy TCP).</span><span class="sxs-lookup"><span data-stu-id="b5e91-164">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="b5e91-165">Mindkét protokollt nem lehet hozzárendelni ugyanahhoz a porthoz.</span><span class="sxs-lookup"><span data-stu-id="b5e91-165">Both protocols can't be assigned to the same port.</span></span>

1. <span data-ttu-id="b5e91-166">A PowerShell változók beállítása</span><span class="sxs-lookup"><span data-stu-id="b5e91-166">Set up the PowerShell variables</span></span>

    ```powershell
    $probeV4V6Name = "ProbeForIPv4AndIPv6"
    $natRule1V4Name = "NatRule-For-Rdp-VM1"
    $natRule2V4Name = "NatRule-For-Rdp-VM2"
    $lbRule1V4Name = "LBRuleForIPv4-Port80"
    $lbRule1V6Name = "LBRuleForIPv6-Port80"
    ```

2. <span data-ttu-id="b5e91-167">A mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5e91-167">Create the probe</span></span>

    <span data-ttu-id="b5e91-168">Az alábbi példa létrehoz egy TCP mintavételi modul, amely ellenőrzi a kapcsolatot a háttér-80-as TCP-port 15 másodpercenként.</span><span class="sxs-lookup"><span data-stu-id="b5e91-168">The following example creates a TCP probe that checks for connectivity to back-end TCP port 80 every 15 seconds.</span></span> <span data-ttu-id="b5e91-169">Az befejezettként jelöli meg a háttér-erőforrás nem érhető el két egymást követő hibák után.</span><span class="sxs-lookup"><span data-stu-id="b5e91-169">It will mark the back-end resource unavailable after two consecutive failures.</span></span>

    ```azurecli
    $probeV4V6 = azure network lb probe create --resource-group $rgname --name $probeV4V6Name --protocol tcp --port 80 --interval 15 --count 2 --lb-name $lbName
    ```

3. <span data-ttu-id="b5e91-170">Bejövő NAT-szabályok, amelyek lehetővé teszik a háttér-erőforrások RDP-kapcsolatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5e91-170">Create inbound NAT rules that allow RDP connections to the back-end resources</span></span>

    ```azurecli
    $inboundNatRuleRdp1 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule1V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3389 --backend-port 3389 --lb-name $lbName
    $inboundNatRuleRdp2 = azure network lb inbound-nat-rule create --resource-group $rgname --name $natRule2V4Name --frontend-ip-name $frontendV4Name --protocol Tcp --frontend-port 3391 --backend-port 3389 --lb-name $lbName
    ```

4. <span data-ttu-id="b5e91-171">Hozzon létre egy terhelés-kiegyenlítő szabályokat, amelyek forgalmat küldeni attól függően, melyik előtér a kérelmet kapott a másik háttér-portok</span><span class="sxs-lookup"><span data-stu-id="b5e91-171">Create a load balancer rules that send traffic to different back-end ports depending on which front end received the request</span></span>

    ```azurecli
    $lbruleIPv4 = azure network lb rule create --resource-group $rgname --name $lbRule1V4Name --frontend-ip-name $frontendV4Name --backend-address-pool-name $backendAddressPoolV4Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 80 --lb-name $lbName
    $lbruleIPv6 = azure network lb rule create --resource-group $rgname --name $lbRule1V6Name --frontend-ip-name $frontendV6Name --backend-address-pool-name $backendAddressPoolV6Name --probe-name $probeV4V6Name --protocol Tcp --frontend-port 80 --backend-port 8080 --lb-name $lbName
    ```

5. <span data-ttu-id="b5e91-172">Ellenőrizze a beállításokat</span><span class="sxs-lookup"><span data-stu-id="b5e91-172">Check your settings</span></span>

    ```azurecli
    azure network lb show --resource-group $rgName --name $lbName
    ```

    <span data-ttu-id="b5e91-173">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="b5e91-173">Expected output:</span></span>

        info:    Executing command network lb show
        info:    Looking up the load balancer "myIPv4IPv6Lb"
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

## <a name="create-nics"></a><span data-ttu-id="b5e91-174">Hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5e91-174">Create NICs</span></span>

<span data-ttu-id="b5e91-175">Hálózati adaptert létrehozni, és rendelje hozzá őket NAT szabályok, load balancer szabályok és mintavételek menüpontban.</span><span class="sxs-lookup"><span data-stu-id="b5e91-175">Create NICs and associate them to NAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="b5e91-176">A PowerShell változók beállítása</span><span class="sxs-lookup"><span data-stu-id="b5e91-176">Set up the PowerShell variables</span></span>

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

2. <span data-ttu-id="b5e91-177">Hozzon létre egy minden háttér hálózati Adapterhez, és adja hozzá az IPv6 konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b5e91-177">Create a NIC for each back-end and add an IPv6 configuration.</span></span>

    ```azurecli
    $nic1 = azure network nic create --name $nic1Name --resource-group $rgname --location $location --private-ip-version "IPv4" --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule1V4Id
    $nic1IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic1Name

    $nic2 = azure network nic create --name $nic2Name --resource-group $rgname --location $location --subnet-id $subnet1Id --lb-address-pool-ids $backendAddressPoolV4Id --lb-inbound-nat-rule-ids $natRule2V4Id
    $nic2IPv6 = azure network nic ip-config create --resource-group $rgname --name "IPv6IPConfig" --private-ip-version "IPv6" --lb-address-pool-ids $backendAddressPoolV6Id --nic-name $nic2Name
    ```

## <a name="create-the-back-end-vm-resources-and-attach-each-nic"></a><span data-ttu-id="b5e91-178">A háttér-Virtuálisgép-erőforrások létrehozása és csatolása az egyes hálózati adapterek</span><span class="sxs-lookup"><span data-stu-id="b5e91-178">Create the back-end VM resources and attach each NIC</span></span>

<span data-ttu-id="b5e91-179">Virtuális gépek létrehozására, rendelkeznie kell egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="b5e91-179">To create VMs, you must have a storage account.</span></span> <span data-ttu-id="b5e91-180">A terheléselosztást, a virtuális gépek kell lennie a rendelkezésre állási csoportok tagjai.</span><span class="sxs-lookup"><span data-stu-id="b5e91-180">For load balancing, the VMs need to be members of an availability set.</span></span> <span data-ttu-id="b5e91-181">Virtuális gépek létrehozásával kapcsolatos további információkért lásd: [hozzon létre egy Azure virtuális gép PowerShell használatával](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="b5e91-181">For more information about creating VMs, see [Create an Azure VM using PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md?toc=%2fazure%2fload-balancer%2ftoc.json)</span></span>

1. <span data-ttu-id="b5e91-182">A PowerShell változók beállítása</span><span class="sxs-lookup"><span data-stu-id="b5e91-182">Set up the PowerShell variables</span></span>

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
    > <span data-ttu-id="b5e91-183">Ez a példa a felhasználónevet és jelszót egyszerű szövegként a virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="b5e91-183">This example uses the username and password for the VMs in clear text.</span></span> <span data-ttu-id="b5e91-184">Megfelelő gondot kell fordítani, ha használja a hitelesítő adatok szövegként.</span><span class="sxs-lookup"><span data-stu-id="b5e91-184">Appropriate care must be taken when using credentials in the clear.</span></span> <span data-ttu-id="b5e91-185">Egy biztonságosabb módszer a PowerShell hitelesítő adatok kezelésére, tekintse meg a [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="b5e91-185">For a more secure method of handling credentials in PowerShell, see the [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

2. <span data-ttu-id="b5e91-186">A tárolási fiók és a rendelkezésre állási készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5e91-186">Create the storage account and availability set</span></span>

    <span data-ttu-id="b5e91-187">A virtuális gépek létrehozásakor, használhat meglévő tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="b5e91-187">You may use an existing storage account when you create the VMs.</span></span> <span data-ttu-id="b5e91-188">A következő parancs létrehoz egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="b5e91-188">The following command creates a new storage account.</span></span>

    ```azurecli
    $storageAcc = azure storage account create $storageAccountName --resource-group $rgName --location $location --sku-name "LRS" --kind "Storage"
    ```

    <span data-ttu-id="b5e91-189">Ezután hozzon létre a rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="b5e91-189">Next, create the availability set.</span></span>

    ```azurecli
    $availabilitySet = azure availset create --name $availabilitySetName --resource-group $rgName --location $location
    ```

3. <span data-ttu-id="b5e91-190">A társított hálózati adapterrel rendelkező virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5e91-190">Create the virtual machines with the associated NICs</span></span>

    ```azurecli
    $vm1 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm1Name --nic-id $nic1Id --os-disk-vhd $osDisk1Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension

    $vm2 = azure vm create --resource-group $rgname --location $location --availset-name $availabilitySetName --name $vm2Name --nic-id $nic2Id --os-disk-vhd $osDisk2Uri --os-type "Windows" --admin-username $vmUserName --admin-password $mySecurePassword --vm-size "Standard_A1" --image-urn $imageurn --storage-account-name $storageAccountName --disable-bginfo-extension
    ```

## <a name="next-steps"></a><span data-ttu-id="b5e91-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b5e91-191">Next steps</span></span>

[<span data-ttu-id="b5e91-192">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="b5e91-192">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="b5e91-193">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5e91-193">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="b5e91-194">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5e91-194">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
