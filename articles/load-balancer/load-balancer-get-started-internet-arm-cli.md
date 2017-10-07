---
title: "az internetre irányuló aaaCreate terheléselosztó - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy Internet felé néző terheléselosztót a Resource Manager használatával hello Azure parancssori felület"
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
ms.openlocfilehash: cadb5edb3b4a4e2f0813109d027eaafdc7ef7303
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-internet-load-balancer-using-hello-azure-cli"></a><span data-ttu-id="5de4c-103">Az internet terheléselosztói hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="5de4c-103">Creating an internet load balancer using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5de4c-104">Portál</span><span class="sxs-lookup"><span data-stu-id="5de4c-104">Portal</span></span>](../load-balancer/load-balancer-get-started-internet-portal.md)
> * [<span data-ttu-id="5de4c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5de4c-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-internet-arm-ps.md)
> * [<span data-ttu-id="5de4c-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5de4c-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-internet-arm-cli.md)
> * [<span data-ttu-id="5de4c-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="5de4c-107">Template</span></span>](../load-balancer/load-balancer-get-started-internet-arm-template.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

<span data-ttu-id="5de4c-108">Ez a cikk ismerteti a hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="5de4c-108">This article covers hello Resource Manager deployment model.</span></span> <span data-ttu-id="5de4c-109">Emellett [megtudhatja, hogyan toocreate egy internetre irányuló terheléselosztót használja a klasszikus üzembe helyezési](load-balancer-get-started-internet-classic-portal.md)</span><span class="sxs-lookup"><span data-stu-id="5de4c-109">You can also [Learn how toocreate an Internet facing load balancer using classic deployment](load-balancer-get-started-internet-classic-portal.md)</span></span>

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="deploying-hello-solution-using-hello-azure-cli"></a><span data-ttu-id="5de4c-110">Hello Azure parancssori felület használatával hello megoldás telepítése</span><span class="sxs-lookup"><span data-stu-id="5de4c-110">Deploying hello solution using hello Azure CLI</span></span>

<span data-ttu-id="5de4c-111">hello lépések bemutatják, hogyan toocreate egy internetre irányuló terheléselosztót a CLI Azure Resource Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="5de4c-111">hello following steps show how toocreate an Internet facing load balancer using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="5de4c-112">Az Azure Resource Manager az egyes erőforrások hozzák létre, és egyenként konfigurálni, majd hozzáfoghat toocreate erőforrás.</span><span class="sxs-lookup"><span data-stu-id="5de4c-112">With Azure Resource Manager each resource is created and configured individually, then put together toocreate a resource.</span></span>

<span data-ttu-id="5de4c-113">Kell létrehozni, és a következő objektumok toodeploy terheléselosztó hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="5de4c-113">You must create and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="5de4c-114">Előtér-IP-konfiguráció – a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz.</span><span class="sxs-lookup"><span data-stu-id="5de4c-114">Front-end IP configuration - contains public IP addresses for incoming network traffic.</span></span>
* <span data-ttu-id="5de4c-115">Háttér-címkészlet - hello virtuális gépek tooreceive hálózati forgalmat a hello terheléselosztó hálózati adapterek (NIC) tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5de4c-115">Back-end address pool - contains network interfaces (NICs) for hello virtual machines tooreceive network traffic from hello load balancer.</span></span>
* <span data-ttu-id="5de4c-116">Terheléselosztási szabályok - hello load balancer tooport hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5de4c-116">Load balancing rules - contains rules mapping a public port on hello load balancer tooport in hello back-end address pool.</span></span>
* <span data-ttu-id="5de4c-117">Bejövő NAT-szabályok – hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port leképezési szabályokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5de4c-117">Inbound NAT rules - contains rules mapping a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool.</span></span>
* <span data-ttu-id="5de4c-118">Mintavétel - állapotfigyelő használ mintavételi készleten toocheck rendelkezésre állása hello háttér címkészletet virtuálisgép-példánya tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5de4c-118">Probes - contains health probes used toocheck availability of virtual machines instances in hello back-end address pool.</span></span>

<span data-ttu-id="5de4c-119">A további információkat [Az Azure Resource Manager támogatása a terheléselosztó számára](load-balancer-arm.md) című rész tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5de4c-119">For more information see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-cli-toouse-resource-manager"></a><span data-ttu-id="5de4c-120">Parancssori felület toouse erőforrás-kezelő beállítása</span><span class="sxs-lookup"><span data-stu-id="5de4c-120">Set up CLI toouse Resource Manager</span></span>

1. <span data-ttu-id="5de4c-121">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="5de4c-121">If you have never used Azure CLI, see [Install and Configure hello Azure CLI](../cli-install-nodejs.md) and follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="5de4c-122">Futtassa a hello **azure config mód** tooswitch tooResource Manager üzemmód, alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="5de4c-122">Run hello **azure config mode** command tooswitch tooResource Manager mode, as shown below.</span></span>

    ```azurecli
        azure config mode arm
    ```

    <span data-ttu-id="5de4c-123">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="5de4c-123">Expected output:</span></span>

        info:    New mode is arm

## <a name="create-a-virtual-network-and-a-public-ip-address-for-hello-front-end-ip-pool"></a><span data-ttu-id="5de4c-124">Hozzon létre egy virtuális hálózatot és egy nyilvános IP-cím hello előtér-IP-címtartományhoz.</span><span class="sxs-lookup"><span data-stu-id="5de4c-124">Create a virtual network and a public IP address for hello front-end IP pool</span></span>

1. <span data-ttu-id="5de4c-125">Hozzon létre egy virtuális hálózatot (VNet) nevű *NRPVnet* hello USA keleti régiója helyen erőforráscsoport használatával nevű *NRPRG*.</span><span class="sxs-lookup"><span data-stu-id="5de4c-125">Create a virtual network (VNet) named *NRPVnet* in hello East US location using a resource group named *NRPRG*.</span></span>

    ```azurecli
        azure network vnet create NRPRG NRPVnet eastUS -a 10.0.0.0/16
    ```

    <span data-ttu-id="5de4c-126">Hozzon létre egy 10.0.0.0/24 CIDR-blokkot tartalmazó *NRPVnetSubnet* nevű alhálózatot az *NRPVnet* virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="5de4c-126">Create a subnet named *NRPVnetSubnet* with a CIDR block of 10.0.0.0/24 in *NRPVnet*.</span></span>

    ```azurecli
        azure network vnet subnet create NRPRG NRPVnet NRPVnetSubnet -a 10.0.0.0/24
    ```

2. <span data-ttu-id="5de4c-127">Hozzon létre egy nyilvános IP-cím nevű *NRPPublicIP* DNS-nevet egy előtér-IP-készlet által használt toobe *loadbalancernrp.eastus.cloudapp.azure.com*. hello parancs az alábbi hello statikus foglalási típust használja, és 4 perc üresjárati időkorlátját.</span><span class="sxs-lookup"><span data-stu-id="5de4c-127">Create a public IP address named *NRPPublicIP* toobe used by a front-end IP pool with DNS name *loadbalancernrp.eastus.cloudapp.azure.com*. hello command below uses hello static allocation type and idle timeout of 4 minutes.</span></span>

    ```azurecli
        azure network public-ip create -g NRPRG -n NRPPublicIP -l eastus -d loadbalancernrp -a static -i 4
    ```

   > [!IMPORTANT]
   > <span data-ttu-id="5de4c-128">hello terheléselosztó azonos teljes Tartománynevű hello tartomány címkéjének hello nyilvános IP-címet fog használni.</span><span class="sxs-lookup"><span data-stu-id="5de4c-128">hello load balancer will use hello domain label of hello public IP as its FQDN.</span></span> <span data-ttu-id="5de4c-129">A klasszikus üzembe helyezési, hello felhőalapú szolgáltatás, a terheléselosztó teljesen minősített tartománynév (FQDN) hello használó módosítást.</span><span class="sxs-lookup"><span data-stu-id="5de4c-129">This a change from classic deployment, which uses hello cloud service as hello load balancer Fully Qualified Domain Name (FQDN).</span></span>
   > <span data-ttu-id="5de4c-130">Ebben a példában hello FQDN-je *loadbalancernrp.eastus.cloudapp.azure.com*.</span><span class="sxs-lookup"><span data-stu-id="5de4c-130">In this example, hello FQDN is *loadbalancernrp.eastus.cloudapp.azure.com*.</span></span>

## <a name="create-a-load-balancer"></a><span data-ttu-id="5de4c-131">Terheléselosztó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5de4c-131">Create a load balancer</span></span>

<span data-ttu-id="5de4c-132">hello következő parancs létrehoz egy terhelés-kiegyenlítő nevű *NRPlb* a hello *NRPRG* hello erőforráscsoportja *USA keleti régiója* Azure-beli hely.</span><span class="sxs-lookup"><span data-stu-id="5de4c-132">hello following command creates a load balancer named *NRPlb* in hello *NRPRG* resource group in hello *East US* Azure location.</span></span>

    ```azurecli
    azure network lb create NRPRG NRPlb eastus
    ```

## <a name="create-a-front-end-ip-pool-and-a-backend-address-pool"></a><span data-ttu-id="5de4c-133">Előtér-IP-címkészlet és háttércímkészlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="5de4c-133">Create a front-end IP pool and a backend address pool</span></span>
<span data-ttu-id="5de4c-134">Ez a példa bemutatja, hogyan toocreate hello előtér-IP-készlet, amely megkapja a bejövő hálózati forgalmat hello hello terheléselosztó és hello háttérbeli IP-készlet, ahol hello előtér-készlet küldi hello hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="5de4c-134">This example demonstrates how toocreate hello front-end IP pool that receives hello incoming network traffic on hello load balancer and hello backend IP pool where hello front-end pool sends hello load balanced network traffic.</span></span>

1. <span data-ttu-id="5de4c-135">Hozzon létre egy társítása hello nyilvános IP-cím létrehozott hello előző lépésben és hello terheléselosztó előtér-IP-címkészletet.</span><span class="sxs-lookup"><span data-stu-id="5de4c-135">Create a front-end IP pool associating hello public IP created in hello previous step and hello load balancer.</span></span>

    ```azurecli
        azure network lb frontend-ip create nrpRG NRPlb NRPfrontendpool -i nrppublicip
    ```

2. <span data-ttu-id="5de4c-136">Állítsa be egy háttér címkészletet tooreceive bejövő forgalom használt hello előtér-IP-készletből.</span><span class="sxs-lookup"><span data-stu-id="5de4c-136">Set up a back-end address pool used tooreceive incoming traffic from hello front-end IP pool.</span></span>

    ```azurecli
        azure network lb address-pool create NRPRG NRPlb NRPbackendpool
    ```

## <a name="create-lb-rules-nat-rules-and-probe"></a><span data-ttu-id="5de4c-137">LB-szabályok, NAT-szabályok és mintavétel létrehozása</span><span class="sxs-lookup"><span data-stu-id="5de4c-137">Create LB rules, NAT rules, and probe</span></span>

<span data-ttu-id="5de4c-138">Ebben a példában a következő elemek hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5de4c-138">This example creates hello following items.</span></span>

* <span data-ttu-id="5de4c-139">a NAT-szabályok tootranslate minden bejövő forgalom a 22-es port 21 tooport<sup>1</sup></span><span class="sxs-lookup"><span data-stu-id="5de4c-139">a NAT rule tootranslate all incoming traffic on port 21 tooport 22<sup>1</sup></span></span>
* <span data-ttu-id="5de4c-140">a NAT-szabályok tootranslate minden bejövő forgalom a 22-es port 23 tooport</span><span class="sxs-lookup"><span data-stu-id="5de4c-140">a NAT rule tootranslate all incoming traffic on port 23 tooport 22</span></span>
* <span data-ttu-id="5de4c-141">a load balancer szabály toobalance minden bejövő forgalmat a 80-as port tooport 80 hello a címek hello háttér-készletben.</span><span class="sxs-lookup"><span data-stu-id="5de4c-141">a load balancer rule toobalance all incoming traffic on port 80 tooport 80 on hello addresses in hello back-end pool.</span></span>
* <span data-ttu-id="5de4c-142">a mintavétel szabály toocheck hello állapot nevű oldalon *HealthProbe.aspx*.</span><span class="sxs-lookup"><span data-stu-id="5de4c-142">a probe rule toocheck hello health status on a page named *HealthProbe.aspx*.</span></span>

<span data-ttu-id="5de4c-143"><sup>1</sup> NAT-szabályok adott virtuálisgép-példányt társított tooa hello terheléselosztó mögött.</span><span class="sxs-lookup"><span data-stu-id="5de4c-143"><sup>1</sup> NAT rules are associated tooa specific virtual machine instance behind hello load balancer.</span></span> <span data-ttu-id="5de4c-144">hello hálózati 21 portot a bejövő adatforgalom tooa adott virtuális gép a NAT-szabály társított 22-es porton.</span><span class="sxs-lookup"><span data-stu-id="5de4c-144">hello network traffic arriving on port 21 is sent tooa specific virtual machine on port 22 associated with this NAT rule.</span></span> <span data-ttu-id="5de4c-145">A NAT-szabályhoz meg kell adnia egy protokollt (UDP vagy TCP).</span><span class="sxs-lookup"><span data-stu-id="5de4c-145">You must specify a protocol (UDP or TCP) for a NAT rule.</span></span> <span data-ttu-id="5de4c-146">Mindkét protokollt nem lehet toohello hozzárendelve ugyanehhez a porthoz.</span><span class="sxs-lookup"><span data-stu-id="5de4c-146">Both protocols can't be assigned toohello same port.</span></span>

1. <span data-ttu-id="5de4c-147">Hello NAT-szabályok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5de4c-147">Create hello NAT rules.</span></span>

    ```azurecli
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh1 --protocol tcp --frontend-port 21 --backend-port 22
        azure network lb inbound-nat-rule create --resource-group nrprg --lb-name nrplb --name ssh2 --protocol tcp --frontend-port 23 --backend-port 22
    ```

2. <span data-ttu-id="5de4c-148">Hozzon létre egy terheléselosztó-szabályt.</span><span class="sxs-lookup"><span data-stu-id="5de4c-148">Create a load balancer rule.</span></span>

    ```azurecli
        azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule --protocol tcp --frontend-port 80 --backend-port 80 --frontend-ip-name NRPfrontendpool --backend-address-pool-name NRPbackendpool
    ```

3. <span data-ttu-id="5de4c-149">Hozzon létre egy állapotmintát.</span><span class="sxs-lookup"><span data-stu-id="5de4c-149">Create a health probe.</span></span>

    ```azurecli
        azure network lb probe create --resource-group nrprg --lb-name nrplb --name healthprobe --protocol "http" --port 80 --path healthprobe.aspx --interval 15 --count 4
    ```

4. <span data-ttu-id="5de4c-150">Ellenőrizze a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="5de4c-150">Check your settings.</span></span>

    ```azurecli
        azure network lb show nrprg nrplb
    ```

    <span data-ttu-id="5de4c-151">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="5de4c-151">Expected output:</span></span>

        info:    Executing command network lb show
        + Looking up hello load balancer "nrplb"
        + Looking up hello public ip "NRPPublicIP"
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

## <a name="create-nics"></a><span data-ttu-id="5de4c-152">Hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="5de4c-152">Create NICs</span></span>

<span data-ttu-id="5de4c-153">Toocreate hálózati adapterrel kell (vagy módosíthatja a meglévőket), és rendelje hozzá őket tooNAT szabályok, load balancer szabályok és mintavételek menüpontban.</span><span class="sxs-lookup"><span data-stu-id="5de4c-153">You need toocreate NICs (or modify existing ones) and associate them tooNAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="5de4c-154">Hozzon létre egy hálózati adapter nevű *lb nic1 kell*, és társítsa azt az hello *rdp1* NAT szabály és hello *NRPbackendpool* háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="5de4c-154">Create a NIC named *lb-nic1-be*, and associate it with hello *rdp1* NAT rule, and hello *NRPbackendpool* back-end address pool.</span></span>

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" eastus
    ```

    <span data-ttu-id="5de4c-155">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="5de4c-155">Expected output:</span></span>

        info:    Executing command network nic create
        + Looking up hello network interface "lb-nic1-be"
        + Looking up hello subnet "nrpvnetsubnet"
        + Creating network interface "lb-nic1-be"
        + Looking up hello network interface "lb-nic1-be"
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

2. <span data-ttu-id="5de4c-156">Hozzon létre egy hálózati adapter nevű *lb nic2 kell*, és társítsa azt az hello *rdp2* NAT szabály és hello *NRPbackendpool* háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="5de4c-156">Create a NIC named *lb-nic2-be*, and associate it with hello *rdp2* NAT rule, and hello *NRPbackendpool* back-end address pool.</span></span>

    ```azurecli
        azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/NRPbackendpool" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" eastus
    ```

3. <span data-ttu-id="5de4c-157">Hozzon létre egy virtuális gépet (VM) nevű *web1*, és rendelje hozzá azt a hálózati adapter nevű hello *lb nic1 kell*.</span><span class="sxs-lookup"><span data-stu-id="5de4c-157">Create a virtual machine (VM) named *web1*, and associate it with hello NIC named *lb-nic1-be*.</span></span> <span data-ttu-id="5de4c-158">A tárfiók neve *web1nrp* alábbi hello parancs futtatása előtt lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="5de4c-158">A storage account called *web1nrp* was created before running hello command below.</span></span>

    ```azurecli
        azure vm create --resource-group nrprg --name web1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="5de4c-159">A betöltési terheléselosztó kell toobe a virtuális gépek hello azonos rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="5de4c-159">VMs in a load balancer need toobe in hello same availability set.</span></span> <span data-ttu-id="5de4c-160">Használjon `azure availset create` toocreate rendelkezésre állási készlet.</span><span class="sxs-lookup"><span data-stu-id="5de4c-160">Use `azure availset create` toocreate an availability set.</span></span>

    <span data-ttu-id="5de4c-161">hello kimeneti hasonló toohello következő legyen:</span><span class="sxs-lookup"><span data-stu-id="5de4c-161">hello output should be similar toohello following:</span></span>

        info:    Executing command vm create
        + Looking up hello VM "web1"
        Enter username: azureuser
        Enter password for azureuser: *********
        Confirm password: *********
        info:    Using hello VM Size "Standard_A1"
        info:    hello [OS, Data] Disk or image configuration requires storage account
        + Looking up hello storage account web1nrp
        + Looking up hello availability set "nrp-avset"
        info:    Found an Availability set "nrp-avset"
        + Looking up hello NIC "lb-nic1-be"
        info:    Found an existing NIC "lb-nic1-be"
        info:    Found an IP configuration with virtual network subnet id "/subscriptions/####################################/resourceGroups/NRPRG/providers/Microsoft.Network/virtualNetworks/NRPVnet/subnets/NRPVnetSubnet" in hello NIC "lb-nic1-be"
        info:    This is a NIC without publicIP configured
        + Creating VM "web1"
        info:    vm create command OK

    > [!NOTE]
    > <span data-ttu-id="5de4c-162">hello tájékoztató üzenet **Ez az egy hálózati adapter nélkül konfigurált publicIP** várt hello hálózati adapter létrehozása óta hello terheléselosztóhoz csatlakozó tooInternet hello load balancer nyilvános IP-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="5de4c-162">hello informational message **This is a NIC without publicIP configured** is expected since hello NIC created for hello load balancer connecting tooInternet using hello load balancer public IP address.</span></span>

    <span data-ttu-id="5de4c-163">Hello óta *lb nic1 kell* hálózati adapter kapcsolódik a hello *rdp1* NAT-szabály túl kapcsolódhatnak*web1* RDP Funkciót használnak a hello terheléselosztón 3441 porton keresztül.</span><span class="sxs-lookup"><span data-stu-id="5de4c-163">Since hello *lb-nic1-be* NIC is associated with hello *rdp1* NAT rule, you can connect too*web1* using RDP through port 3441 on hello load balancer.</span></span>

4. <span data-ttu-id="5de4c-164">Hozzon létre egy virtuális gépet (VM) nevű *web2*, és rendelje hozzá azt a hálózati adapter nevű hello *lb nic2 kell*.</span><span class="sxs-lookup"><span data-stu-id="5de4c-164">Create a virtual machine (VM) named *web2*, and associate it with hello NIC named *lb-nic2-be*.</span></span> <span data-ttu-id="5de4c-165">A tárfiók neve *web1nrp* alábbi hello parancs futtatása előtt lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="5de4c-165">A storage account called *web1nrp* was created before running hello command below.</span></span>

    ```azurecli
        azure vm create --resource-group nrprg --name web2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="update-an-existing-load-balancer"></a><span data-ttu-id="5de4c-166">Meglévő terheléselosztó frissítése</span><span class="sxs-lookup"><span data-stu-id="5de4c-166">Update an existing load balancer</span></span>
<span data-ttu-id="5de4c-167">Hozzáadhat egy meglévő terheléselosztóra hivatkozó szabályokat.</span><span class="sxs-lookup"><span data-stu-id="5de4c-167">You can add rules referencing an existing load balancer.</span></span> <span data-ttu-id="5de4c-168">Hello a következő példában, egy új terheléselosztási szabály kerül a meglévő terheléselosztó tooan **NRPlb**</span><span class="sxs-lookup"><span data-stu-id="5de4c-168">In hello next example, a new load balancer rule is added tooan existing load balancer **NRPlb**</span></span>

```azurecli
azure network lb rule create --resource-group nrprg --lb-name nrplb --name lbrule2 --protocol tcp --frontend-port 8080 --backend-port 8051 --frontend-ip-name frontendnrppool --backend-address-pool-name NRPbackendpool
```

## <a name="delete-a-load-balancer"></a><span data-ttu-id="5de4c-169">Terheléselosztó törlése</span><span class="sxs-lookup"><span data-stu-id="5de4c-169">Delete a load balancer</span></span>
<span data-ttu-id="5de4c-170">A következő parancs tooremove terheléselosztó hello használata:</span><span class="sxs-lookup"><span data-stu-id="5de4c-170">Use hello following command tooremove a load balancer:</span></span>

```azurecli
azure network lb delete --resource-group nrprg --name nrplb
```

## <a name="next-steps"></a><span data-ttu-id="5de4c-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5de4c-171">Next steps</span></span>
[<span data-ttu-id="5de4c-172">Bevezetés a belső terheléselosztók konfigurálásába</span><span class="sxs-lookup"><span data-stu-id="5de4c-172">Get started configuring an internal load balancer</span></span>](load-balancer-get-started-ilb-arm-cli.md)

[<span data-ttu-id="5de4c-173">A terheléselosztó elosztási módjának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5de4c-173">Configure a load balancer distribution mode</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="5de4c-174">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5de4c-174">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)
