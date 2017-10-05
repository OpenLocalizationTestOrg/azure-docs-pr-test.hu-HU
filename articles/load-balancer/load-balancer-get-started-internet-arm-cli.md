---
title: "Internetkapcsolattal rendelkező terheléselosztó létrehozása – Azure CLI | Microsoft Docs"
description: "Ismerje meg, hogyan hozható létre internetkapcsolattal rendelkező terheléselosztó a Resource Managerben az Azure parancssori felület használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: a8bcdd88-f94c-4537-8143-c710eaa86818
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 3b1780033cbc8aa3e108a213a4d2bfd0332fd7d7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-internet-load-balancer-using-the-azure-cli"></a><span data-ttu-id="d2f5f-103">Internetes terheléselosztó létrehozása az Azure parancssori felületével</span><span class="sxs-lookup"><span data-stu-id="d2f5f-103">Creating an internet load balancer using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="d2f5f-104">Portál</span><span class="sxs-lookup"><span data-stu-id="d2f5f-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="d2f5f-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d2f5f-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="d2f5f-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d2f5f-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="d2f5f-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="d2f5f-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="d2f5f-108">Ez a cikk a Resource Manager-alapú üzemi modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-108">This article covers the Resource Manager deployment model.</span></span> <span data-ttu-id="d2f5f-109">Emellett [azt is megismerheti, hogyan hozható létre internetkapcsolattal rendelkező terheléselosztó a klasszikus üzemelő példány használatával](load-balancer-get-started-internet-classic-portal.md)</span><span class="sxs-lookup"><span data-stu-id="d2f5f-109">You can also [Learn how to create an Internet facing load balancer using classic deployment](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-the-solution-using-the-azure-cli"></a><span data-ttu-id="d2f5f-110">A megoldás üzembe helyezése az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="d2f5f-110">Deploying the solution using the Azure CLI</span></span>

<span data-ttu-id="d2f5f-111">A következő lépések bemutatják, hogyan hozható létre internetkapcsolattal rendelkező terheléselosztó az Azure Resource Manager parancssori felületének használatával.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-111">The following steps show how to create an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="d2f5f-112">Az Azure Resource Manager lehetővé teszi, hogy az egyes erőforrások konfigurálása egyenként történjen, majd az összerakásukkal jöjjön létre egy erőforrás.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-112">With Azure Resource Manager each resource is created and configured individually, then put together to create a resource.</span></span>

<span data-ttu-id="d2f5f-113">A terheléselosztó üzembe helyezéséhez a következő objektumokat kell létrehozni és konfigurálni:</span><span class="sxs-lookup"><span data-stu-id="d2f5f-113">You must create and configure the following objects to deploy a load balancer:</span></span>

* <span data-ttu-id="d2f5f-114">Előtér-IP-konfiguráció – a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-114">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="d2f5f-115">Háttércímkészlet – hálózati adaptereket (NIC) tartalmaz, amelyek segítségével a virtuális gépek fogadhatják a terheléselosztóról érkező hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-115">Back-end address pool - contains network interfaces (NICs) for the virtual machines to receive network traffic from the load balancer.</span></span>
* <span data-ttu-id="d2f5f-116">Terheléselosztási szabályok – olyan szabályokat tartalmaz, amelyek a terheléselosztó nyilvános portjait rendelik hozzá háttércímkészlet portjaihoz.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-116">Load balancing rules - contains rules mapping a public port on the load balancer to port in the back-end address pool.</span></span>
* <span data-ttu-id="d2f5f-117">Bejövő NAT-szabályok – olyan szabályokat tartalmaz, amelyek a terheléselosztó nyilvános portjait rendelik hozzá egy adott virtuális gép portjához a háttércímkészletben.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-117">Inbound NAT rules - contains rules mapping a public port on the load balancer to a port for a specific virtual machine in the back-end address pool.</span></span>
* <span data-ttu-id="d2f5f-118">Mintavételezők – állapotfigyelő mintavételezőket tartalmaz, amelyek a virtuálisgép-példányok rendelkezésre állását ellenőrzik a háttércímkészletben.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-118">Probes - contains health probes used to check availability of virtual machines instances in the back-end address pool.</span></span>

<span data-ttu-id="d2f5f-119">A további információkat [Az Azure Resource Manager támogatása a terheléselosztó számára](load-balancer-arm.md) című rész tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-119">For more information see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-cli-to-use-resource-manager"></a><span data-ttu-id="d2f5f-120">A parancssori felület beállítása a Resource Manager használatához</span><span class="sxs-lookup"><span data-stu-id="d2f5f-120">Set up CLI to use Resource Manager</span></span>

1. <span data-ttu-id="d2f5f-121">Ha még sosem használta az Azure CLI-t, akkor tekintse meg [Install and Configure the Azure CLI](../cli-install-nodejs.md) (Az Azure CLI telepítése és konfigurálása) részt, és kövesse az utasításokat addig a pontig, ahol ki kell választania az Azure-fiókot és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-121">If you have never used Azure CLI, see [Install and Configure the Azure CLI](../cli-install-nodejs.md) and follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="d2f5f-122">Az **azure config mode** parancs futtatásával váltson az Erőforrás-kezelő módra, a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-122">Run the **azure config mode** command to switch to Resource Manager mode, as shown below.</span></span>

    ```azurecli
        azure config mode arm
    ```

    <span data-ttu-id="d2f5f-123">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="d2f5f-123">Expected output:</span></span>

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-the-front-end-ip-pool"></a><span data-ttu-id="d2f5f-124">Virtuális hálózat és nyilvános IP-cím létrehozása az előtér-IP-címkészlethez</span><span class="sxs-lookup"><span data-stu-id="d2f5f-124">Create a virtual network and a public IP address for the front-end IP pool</span></span>

1. <span data-ttu-id="d2f5f-125">Hozzon létre egy *NRPVnet* nevű virtuális hálózatot (VNet) az USA keleti régiója helyen az *NRPRG* nevű erőforráscsoporttal.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-125">Create a virtual network (VNet) named *NRPVnet* in the East US location using a resource group named *NRPRG*.</span></span>

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    <span data-ttu-id="d2f5f-126">Hozzon létre egy 10.0.0.0/24 CIDR-blokkot tartalmazó *NRPVnetSubnet* nevű alhálózatot az *NRPVnet* virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-126">Create a subnet named *NRPVnetSubnet* with a CIDR block of 10.0.0.0/24 in *NRPVnet*.</span></span>

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. <span data-ttu-id="d2f5f-127">Hozzon létre egy előtér-IP-címkészlet által használandó nyilvános IP-címet *NRPPublicIP* névvel és *loadbalancernrp.eastus.cloudapp.azure.com* DNS-névvel.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-127">Create a public IP address named *NRPPublicIP* to be used by a front-end IP pool with DNS name *loadbalancernrp.eastus.cloudapp.azure.com*.</span></span> <span data-ttu-id="d2f5f-128">Az alábbi parancs a statikus felosztástípust és 4 perces üresjárati időkorlátot használ.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-128">The command below uses the static allocation type and idle timeout of 4 minutes.</span></span>

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="d2f5f-129">A terheléselosztó a nyilvános IP-cím tartománycímkéjét fogja használni FQDN-ként.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-129">The load balancer will use the domain label of the public IP as its FQDN.</span></span> <span data-ttu-id="d2f5f-130">Ez áttérést jelent a klasszikus üzemelő példányról, amely a felhőszolgáltatást használja a terheléselosztó teljes tartományneveként (FQDN).</span><span class="sxs-lookup"><span data-stu-id="d2f5f-130">This a change from classic deployment, which uses the cloud service as the load balancer Fully Qualified Domain Name (FQDN).</span></span>
   > <span data-ttu-id="d2f5f-131">Ebben a példában a *loadbalancernrp.eastus.cloudapp.azure.com* az FQDN.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-131">In this example, the FQDN is *loadbalancernrp.eastus.cloudapp.azure.com*.</span></span>

## <a name="create-a-load-balancer"></a><span data-ttu-id="d2f5f-132">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2f5f-132">Create a load balancer</span></span>

<span data-ttu-id="d2f5f-133">A következő parancs létrehoz egy *NRPlb* nevű terheléselosztót az *NRPRG* erőforráscsoportban, az *USA keleti régiója* Azure-helyen.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-133">The following command creates a load balancer named *NRPlb* in the *NRPRG* resource group in the *East US* Azure location.</span></span>

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a><span data-ttu-id="d2f5f-134">Előtér-IP-címkészlet és háttércímkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2f5f-134">Create a front-end IP pool and a backend address pool</span></span>
<span data-ttu-id="d2f5f-135">Ez a példa bemutatja, hogyan kell létrehozni azt az előtér-IP-címkészletet, amely a bejövő hálózati forgalmat fogadja a terheléselosztón, illetve azt a háttér-IP-címkészletet, ahová az előtérkészlet küldi az elosztott terhelésű hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-135">This example demonstrates how to create the front-end IP pool that receives the incoming network traffic on the load balancer and the backend IP pool where the front-end pool sends the load balanced network traffic.</span></span>

1. <span data-ttu-id="d2f5f-136">Hozzon létre egy előtér-IP-címkészletet az előző lépésben létrehozott nyilvános IP-címet társítva a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-136">Create a front-end IP pool associating the public IP created in the previous step and the load balancer.</span></span>

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. <span data-ttu-id="d2f5f-137">Állítson be egy háttér-címkészletet az előtér-IP-címkészletből bejövő forgalom fogadásához.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-137">Set up a back-end address pool used to receive incoming traffic from the front-end IP pool.</span></span>

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a><span data-ttu-id="d2f5f-138">LB-szabályok, NAT-szabályok és mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2f5f-138">Create LB rules, NAT rules, and probe</span></span>

<span data-ttu-id="d2f5f-139">Ez a példa a következő elemeket hozza létre.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-139">This example creates the following items.</span></span>

* <span data-ttu-id="d2f5f-140">egy NAT-szabályt, amely a 21-es porton bejövő összes forgalmat lefordítja a 22<sup>1</sup>-es portra</span><span class="sxs-lookup"><span data-stu-id="d2f5f-140">a NAT rule to translate all incoming traffic on port 21 to port 22<sup>1</sup></span></span>
* <span data-ttu-id="d2f5f-141">egy NAT-szabályt, amely a 23-as porton bejövő összes forgalmat lefordítja a 22-es portra</span><span class="sxs-lookup"><span data-stu-id="d2f5f-141">a NAT rule to translate all incoming traffic on port 23 to port 22</span></span>
* <span data-ttu-id="d2f5f-142">egy terheléselosztó-szabályt, amely elosztja a 80-as porton bejövő összes forgalmat a háttér-címkészletben szereplő címekhez tartozó 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-142">a load balancer rule to balance all incoming traffic on port 80 to port 80 on the addresses in the back-end pool.</span></span>
* <span data-ttu-id="d2f5f-143">egy mintavételi szabályt, amely az állapotot ellenőrzi a *HealthProbe.aspx* nevű oldalon.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-143">a probe rule to check the health status on a page named *HealthProbe.aspx*.</span></span>

<span data-ttu-id="d2f5f-144"><sup>1</sup> A NAT-szabályok a terheléselosztó mögött található adott virtuálisgép-példányhoz vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-144"><sup>1</sup> NAT rules are associated to a specific virtual machine instance behind the load balancer.</span></span> <span data-ttu-id="d2f5f-145">A 21-es portra érkező hálózati forgalmat a rendszer elküldi a 22-es porton keresztül egy adott virtuális gépre, amely ehhez a NAT-szabályhoz van társítva.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-145">The network traffic arriving on port 21 is sent to a specific virtual machine on port 22 associated with this NAT rule.</span></span> <span data-ttu-id="d2f5f-146">A NAT-szabályhoz meg kell adnia egy protokollt (UDP vagy TCP).</span><span class="sxs-lookup"><span data-stu-id="d2f5f-146">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="d2f5f-147">Mindkét protokollt nem lehet hozzárendelni ugyanahhoz a porthoz.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-147">Both protocols can't be assigned to the same port.</span></span>

1. <span data-ttu-id="d2f5f-148">Hozza létre a NAT-szabályokat.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-148">Create the NAT rules.</span></span>

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. <span data-ttu-id="d2f5f-149">Hozzon létre egy terheléselosztó-szabályt.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-149">Create a load balancer rule.</span></span>

    ```azurecli
        azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. <span data-ttu-id="d2f5f-150">Hozzon létre egy állapotmintát.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-150">Create a health probe.</span></span>

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. <span data-ttu-id="d2f5f-151">Ellenőrizze a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-151">Check your settings.</span></span>

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    <span data-ttu-id="d2f5f-152">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="d2f5f-152">Expected output:</span></span>

        info:    Executing command network lb show
        + Looking up the load balancer "nrplb"
        + Looking up the public ip "NRPPublicIP"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb
        data:    Name                            : nrplb
        data:    Type                            : Microsoft.Network/loadBalancers
        data:    Location                        : eastus
        data:    Provisioning State              : Succeeded
        data:    Frontend IP configurations:
        data:      Name                          : NRPfrontendpool
        data:      Provisioning state            : Succeeded
        data:      Public IP address id          : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/publicIPAddresses/NRPPublicIP
        data:      Public IP allocation method   : Static
        data:      Public IP address             : 40.114.13.145
        data:
        data:    Backend address pools:
        data:      Name                          : NRPbackendpool
        data:      Provisioning state            : Succeeded
        data:
        data:    Load balancing rules:
        data:      Name                          : HTTP
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 80
        data:      Backend port                  : 80
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:      Backend address pool          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:
        data:    Inbound NAT rules:
        data:      Name                          : ssh1
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 21
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:      Name                          : ssh2
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Tcp
        data:      Frontend port                 : 23
        data:      Backend port                  : 22
        data:      Enable floating IP            : false
        data:      Idle timeout in minutes       : 4
        data:      Frontend IP configuration     : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/frontendIPConfigurations/NRPfrontendpool
        data:
        data:    Probes:
        data:      Name                          : healthprobe
        data:      Provisioning state            : Succeeded
        data:      Protocol                      : Http
        data:      Port                          : 80
        data:      Interval in seconds           : 15
        data:      Number of probes              : 4
        data:
        info:    network lb show command OK

## <a name="create-nics"></a><span data-ttu-id="d2f5f-153">Hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="d2f5f-153">Create NICs</span></span>

<span data-ttu-id="d2f5f-154">Hálózati adaptereket kell létrehoznia (vagy a meglévőket is módosíthatja), és hozzá kell rendelnie őket a NAT-szabályokhoz, a terheléselosztási szabályokhoz és a mintavételezőkhöz.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-154">You need to create NICs (or modify existing ones) and associate them to NAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="d2f5f-155">Hozzon létre egy hálózati adaptert *lb-nic1-be* néven, majd társítsa az *rdp1* NAT-szabállyal és az *NRPbackendpool* háttércímkészlettel.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-155">Create a NIC named *lb-nic1-be*, and associate it with the *rdp1* NAT rule, and the *NRPbackendpool* back-end address pool.</span></span>

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
    ```

    <span data-ttu-id="d2f5f-156">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="d2f5f-156">Expected output:</span></span>

        info:    Executing command network nic create
        + Looking up the network interface "lb-nic1-be"
        + Looking up the subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up the network interface "lb-nic1-be"
        data:    Id                              : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/networkInterfaces/lb-nic1-be
        data:    Name                            : lb-nic1-be
        data:    Type                            : Microsoft.Network/networkInterfaces
        data:    Location                        : eastus
        data:    Provisioning state              : Succeeded
        data:    Enable IP forwarding            : false
        data:    IP configurations:
        data:      Name                          : NIC-config
        data:      Provisioning state            : Succeeded
        data:      Private IP address            : 10.0.0.4
        data:      Private IP Allocation Method  : Dynamic
        data:      Subnet                        : /subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet
        data:      Load balancer backend address pools
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool
        data:      Load balancer inbound NAT rules:
        data:        Id                          : /subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1
        data:
        info:    network nic create command OK

2. <span data-ttu-id="d2f5f-157">Hozzon létre egy hálózati adaptert *lb-nic2-be* néven, majd társítsa az *rdp2* NAT-szabállyal és az *NRPbackendpool* háttércímkészlettel.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-157">Create a NIC named *lb-nic2-be*, and associate it with the *rdp2* NAT rule, and the *NRPbackendpool* back-end address pool.</span></span>

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. <span data-ttu-id="d2f5f-158">Hozzon létre egy virtuális gépet (VM) *web1* néven, és társítsa az *lb-nic1-be* nevű hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-158">Create a virtual machine (VM) named *web1*, and associate it with the NIC named *lb-nic1-be*.</span></span> <span data-ttu-id="d2f5f-159">Az alábbi parancs futtatása előtt létrejött egy *web1nrp* nevű tárfiók.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-159">A storage account called *web1nrp* was created before running the command below.</span></span>

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="d2f5f-160">A terheléselosztó virtuális gépeinek ugyanabban a rendelkezésre állási készletben kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-160">VMs in a load balancer need to be in the same availability set.</span></span> <span data-ttu-id="d2f5f-161">Használja az `azure availset create` parancsot a rendelkezésre állási készlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-161">Use `azure availset create` to create an availability set.</span></span>

    <span data-ttu-id="d2f5f-162">A kimenet az alábbihoz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="d2f5f-162">The output should be similar to the following:</span></span>

        info:    Executing command vm create
        + Looking up the VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using the VM Size "Standard_A1"
        info:    The [OS, Data] Disk or image configuration requires storage account
        + Looking up the storage account web1nrp
        + Looking up the availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up the NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in the NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > <span data-ttu-id="d2f5f-163">A következő tájékoztató üzenetnek kell megjelennie: **Ez egy nyilvános IP-cím nélkül konfigurált hálózati adapter**, mivel a terheléselosztóhoz létrehozott hálózati adapter a terheléselosztó nyilvános IP-címével csatlakozik az internethez.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-163">The informational message **This is a NIC without publicIP configured** is expected since the NIC created for the load balancer connecting to Internet using the load balancer public IP address.</span></span>

    <span data-ttu-id="d2f5f-164">Mivel az *lb-nic1-be* hálózati adapter az *rdp1* NAT-szabályhoz van társítva, az RDP-vel a terheléselosztó 3441-es portján keresztül csatlakozhat a *web1* virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-164">Since the *lb-nic1-be* NIC is associated with the *rdp1* NAT rule, you can connect to *web1* using RDP through port 3441 on the load balancer.</span></span>

4. <span data-ttu-id="d2f5f-165">Hozzon létre egy virtuális gépet (VM) *web2* néven, és társítsa az *lb-nic2-be* nevű hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-165">Create a virtual machine (VM) named *web2*, and associate it with the NIC named *lb-nic2-be*.</span></span> <span data-ttu-id="d2f5f-166">Az alábbi parancs futtatása előtt létrejött egy *web1nrp* nevű tárfiók.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-166">A storage account called *web1nrp* was created before running the command below.</span></span>

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="d2f5f-167">Meglévő terheléselosztó frissítése</span><span class="sxs-lookup"><span data-stu-id="d2f5f-167">Update an existing load balancer</span></span>
<span data-ttu-id="d2f5f-168">Hozzáadhat egy meglévő terheléselosztóra hivatkozó szabályokat.</span><span class="sxs-lookup"><span data-stu-id="d2f5f-168">You can add rules referencing an existing load balancer.</span></span> <span data-ttu-id="d2f5f-169">A következő példában egy új terheléselosztó-szabályt adunk hozzá egy **NRPlb** nevű meglévő terheléselosztóhoz</span><span class="sxs-lookup"><span data-stu-id="d2f5f-169">In the next example, a new load balancer rule is added to an existing load balancer **NRPlb**</span></span>

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a><span data-ttu-id="d2f5f-170">Terheléselosztó törlése</span><span class="sxs-lookup"><span data-stu-id="d2f5f-170">Delete a load balancer</span></span>
<span data-ttu-id="d2f5f-171">Terheléselosztó eltávolításához használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d2f5f-171">Use the following command to remove a load balancer:</span></span>

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a><span data-ttu-id="d2f5f-172">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2f5f-172">Next steps</span></span>
[<span data-ttu-id="d2f5f-173">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="d2f5f-173">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="d2f5f-174">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d2f5f-174">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="d2f5f-175">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d2f5f-175">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
