---
title: "Belső terheléselosztó létrehozása – Azure CLI | Microsoft Docs"
description: "Ismerje meg, hogyan hozható létre belső terheléselosztó az Azure parancssori felület használatával a Resource Managerben"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
tags: azure-resource-manager
ms.assetid: c7a24e92-b4da-43c0-90f2-841c1b7ce489
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 128b91c685b5f7e494a69ca5b04165a0ee7cbb78
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-internal-load-balancer-by-using-the-azure-cli"></a><span data-ttu-id="6522c-103">Belső terheléselosztó létrehozása az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="6522c-103">Create an internal load balancer by using the Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="6522c-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="6522c-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="6522c-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6522c-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="6522c-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="6522c-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="6522c-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="6522c-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="6522c-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="6522c-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="6522c-109">Ez a cikk a Resource Manager-alapú üzemi modell használatát ismerteti, amelyet a Microsoft a legtöbb új telepítéshez a [klasszikus üzemi modell](load-balancer-get-started-ilb-classic-cli.md) helyett javasol.</span><span class="sxs-lookup"><span data-stu-id="6522c-109">This article covers using the Resource Manager deployment model, which Microsoft recommends for most new deployments instead of the [classic deployment model](load-balancer-get-started-ilb-classic-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-the-solution-by-using-the-azure-cli"></a><span data-ttu-id="6522c-110">A megoldás üzembe helyezése az Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="6522c-110">Deploy the solution by using the Azure CLI</span></span>

<span data-ttu-id="6522c-111">A következő lépések bemutatják, hogyan lehet internetes elérésű terheléselosztót létrehozni az Azure Resource Manager és a parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="6522c-111">The following steps show how to create an Internet-facing load balancer by using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="6522c-112">Az Azure Resource Manager lehetővé teszi, hogy az egyes erőforrások konfigurálása egyenként történjen, majd az összerakásukkal jöjjön létre egy erőforrás.</span><span class="sxs-lookup"><span data-stu-id="6522c-112">With Azure Resource Manager, each resource is created and configured individually, and then put together to create a resource.</span></span>

<span data-ttu-id="6522c-113">A terheléselosztó üzembe helyezéséhez a következő objektumokat kell létrehozni és konfigurálni:</span><span class="sxs-lookup"><span data-stu-id="6522c-113">You need to create and configure the following objects to deploy a load balancer:</span></span>

* <span data-ttu-id="6522c-114">**Előtérbeli IP-konfiguráció**: a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz</span><span class="sxs-lookup"><span data-stu-id="6522c-114">**Front-end IP configuration**: contains public IP addresses for incoming network traffic</span></span>
* <span data-ttu-id="6522c-115">**Háttércímkészlet**: hálózati adaptereket (NIC) tartalmaz, amelyek lehetővé teszik a virtuális gépek számára a terheléselosztóról érkező hálózati forgalom fogadását</span><span class="sxs-lookup"><span data-stu-id="6522c-115">**Back-end address pool**: contains network interfaces (NICs) that enable the virtual machines to receive network traffic from the load balancer</span></span>
* <span data-ttu-id="6522c-116">**Terheléselosztási szabályok**: olyan szabályokat tartalmaz, amelyek a terheléselosztó nyilvános portját rendelik hozzá a háttér címkészletben levő porthoz</span><span class="sxs-lookup"><span data-stu-id="6522c-116">**Load-balancing rules**: contains rules that map a public port on the load balancer to port in the back-end address pool</span></span>
* <span data-ttu-id="6522c-117">**Bejövő NAT-szabályok**: olyan szabályokat tartalmaz, amelyek a terheléselosztó nyilvános portjait rendelik hozzá egy adott virtuális gép portjához a háttércímkészletben</span><span class="sxs-lookup"><span data-stu-id="6522c-117">**Inbound NAT rules**: contains rules that map a public port on the load balancer to a port for a specific virtual machine in the back-end address pool</span></span>
* <span data-ttu-id="6522c-118">**Mintavételezők**: állapotfigyelő mintavételezőket tartalmaz, amelyek a virtuálisgép-példányok rendelkezésre állását ellenőrzik a háttércímkészletben</span><span class="sxs-lookup"><span data-stu-id="6522c-118">**Probes**: contains health probes that are used to check the availability of virtual machines instances in the back-end address pool</span></span>

<span data-ttu-id="6522c-119">A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="6522c-119">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-cli-to-use-resource-manager"></a><span data-ttu-id="6522c-120">A parancssori felület beállítása a Resource Manager használatához</span><span class="sxs-lookup"><span data-stu-id="6522c-120">Set up CLI to use Resource Manager</span></span>

1. <span data-ttu-id="6522c-121">Ha még soha nem használta az Azure parancssori felületét, lásd: [Install and configure the Azure CLI](../cli-install-nodejs.md) (Azure parancssori felület (CLI) telepítése és konfigurálása).</span><span class="sxs-lookup"><span data-stu-id="6522c-121">If you have never used Azure CLI, see [Install and configure the Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="6522c-122">Kövesse az utasításokat az Azure-fiók és -előfizetés kiválasztásáig.</span><span class="sxs-lookup"><span data-stu-id="6522c-122">Follow the instructions up to the point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="6522c-123">Az **azure config mode** parancs futtatásával váltson Resource Manager módra, a következők szerint:</span><span class="sxs-lookup"><span data-stu-id="6522c-123">Run the **azure config mode** command to switch to Resource Manager mode, as follows:</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="6522c-124">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6522c-124">Expected output:</span></span>

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a><span data-ttu-id="6522c-125">Belső terheléselosztó létrehozásának lépései</span><span class="sxs-lookup"><span data-stu-id="6522c-125">Create an internal load balancer step by step</span></span>

1. <span data-ttu-id="6522c-126">Jelentkezzen be az Azure-ba.</span><span class="sxs-lookup"><span data-stu-id="6522c-126">Sign in to Azure.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="6522c-127">Amikor a rendszer erre kéri, adja meg Azure rendszerbeli hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="6522c-127">When prompted, enter your Azure credentials.</span></span>

2. <span data-ttu-id="6522c-128">Módosítsa a parancssori eszközöket az Azure Resource Manager módra.</span><span class="sxs-lookup"><span data-stu-id="6522c-128">Change the command tools to Azure Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a><span data-ttu-id="6522c-129">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="6522c-129">Create a resource group</span></span>

<span data-ttu-id="6522c-130">Az Azure Resource Managerben minden erőforrás egy erőforráscsoporthoz van társítva.</span><span class="sxs-lookup"><span data-stu-id="6522c-130">All resources in Azure Resource Manager are associated with a resource group.</span></span> <span data-ttu-id="6522c-131">Ha még nem tette meg, hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="6522c-131">If you haven't done so yet, create a resource group.</span></span>

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a><span data-ttu-id="6522c-132">Hozzon létre egy belső terheléselosztó készletet</span><span class="sxs-lookup"><span data-stu-id="6522c-132">Create an internal load balancer set</span></span>

1. <span data-ttu-id="6522c-133">Hozzon létre egy belső terheléselosztót</span><span class="sxs-lookup"><span data-stu-id="6522c-133">Create an internal load balancer</span></span>

    <span data-ttu-id="6522c-134">A következő forgatókönyv esetében az nrprg nevű erőforráscsoportot hozzuk létre az Egyesült Államok keleti régiójában.</span><span class="sxs-lookup"><span data-stu-id="6522c-134">In the following scenario, a resource group named nrprg is created in East US region.</span></span>

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > <span data-ttu-id="6522c-135">A belső terheléselosztók összes erőforrásának (például virtuális hálózatok és virtuális alhálózatok) ugyanabban az erőforráscsoportban és ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6522c-135">All resources for an internal load balancers, such as virtual networks and virtual network subnets, must be in the same resource group and in the same region.</span></span>

2. <span data-ttu-id="6522c-136">Hozzon létre egy előtérbeli IP-címet a belső terheléselosztó számára.</span><span class="sxs-lookup"><span data-stu-id="6522c-136">Create a front-end IP address for the internal load balancer.</span></span>

    <span data-ttu-id="6522c-137">Az IP-címnek kötelező a virtuális hálózat alhálózati tartományában lennie.</span><span class="sxs-lookup"><span data-stu-id="6522c-137">The IP address that you use must be within the subnet range of your virtual network.</span></span>

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. <span data-ttu-id="6522c-138">Hozza létre a háttércímkészletet.</span><span class="sxs-lookup"><span data-stu-id="6522c-138">Create the back-end address pool.</span></span>

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    <span data-ttu-id="6522c-139">Miután megtörtént az előtérbeli IP-cím, valamint a háttércímkészlet definiálása, létre lehet hozni a terheléselosztási szabályokat, a bejövő NAT-szabályokat, valamint a testre szabott állapotfigyelő mintavételezőket.</span><span class="sxs-lookup"><span data-stu-id="6522c-139">After you define a front-end IP address and a back-end address pool, you can create load balancer rules, inbound NAT rules, and customized health probes.</span></span>

4. <span data-ttu-id="6522c-140">Hozzon létre egy terheléselosztási szabályt a belső terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="6522c-140">Create a load balancer rule for the internal load balancer.</span></span>

    <span data-ttu-id="6522c-141">Az előző lépések végrehajtásakor a parancs olyan terheléselosztási szabályt hoz létre, amelyik az 1433-as portot figyeli az előtérkészletben, és a háttér címkészletre szintén az 1433-as port használatával küldi az elosztott terhelésű hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="6522c-141">When you follow the previous steps, the command creates a load-balancer rule for listening to port 1433 in the front-end pool and sending load-balanced network traffic to the back-end address pool, also using port 1433.</span></span>

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. <span data-ttu-id="6522c-142">Hozza létre a bejövő NAT-szabályokat.</span><span class="sxs-lookup"><span data-stu-id="6522c-142">Create inbound NAT rules.</span></span>

    <span data-ttu-id="6522c-143">A bejövő NAT-szabályok olyan végpontok létrehozásához használhatók a terheléselosztóban, amelyek egy adott virtuálisgép-példányhoz tartoznak.</span><span class="sxs-lookup"><span data-stu-id="6522c-143">Inbound NAT rules are used to create endpoints in a load balancer that go to a specific virtual machine instance.</span></span> <span data-ttu-id="6522c-144">Az előző lépések két NAT-szabályt hoztak létre a távoli asztalhoz.</span><span class="sxs-lookup"><span data-stu-id="6522c-144">The previous steps created two NAT rules  for remote desktop.</span></span>

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. <span data-ttu-id="6522c-145">Hozzon létre állapotfigyelő mintavételezőket a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="6522c-145">Create health probes for the load balancer.</span></span>

    <span data-ttu-id="6522c-146">Az állapotfigyelő mintavételező az összes virtuálisgép-példányt ellenőrzi, hogy biztosan képesek legyenek hálózati forgalom küldésére.</span><span class="sxs-lookup"><span data-stu-id="6522c-146">A health probe checks all virtual machine instances to make sure they can send network traffic.</span></span> <span data-ttu-id="6522c-147">A mintavételező tesztjén elbukó virtuálisgép-példányokat a rendszer eltávolítja a terheléselosztóból, és így is maradnak, amíg ismét online állapotúak nem lesznek, és a mintavételező tesztje azt nem jelzi, hogy megfelelő az állapotuk.</span><span class="sxs-lookup"><span data-stu-id="6522c-147">The virtual machine instance with failed probe checks is removed from the load balancer until it goes back online and a probe check determines that it's healthy.</span></span>

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > <span data-ttu-id="6522c-148">A Microsoft Azure platform egy statikus, nyilvánosan irányítható IPv4-címet használ számos különböző felügyeleti forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="6522c-148">The Microsoft Azure platform uses a static, publicly routable IPv4 address for a variety of administrative scenarios.</span></span> <span data-ttu-id="6522c-149">Az IP-cím a 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="6522c-149">The IP address is 168.63.129.16.</span></span> <span data-ttu-id="6522c-150">Ezt az IP-címet nem blokkolhatja egy tűzfal sem, mert ez nem várt viselkedést okozhat.</span><span class="sxs-lookup"><span data-stu-id="6522c-150">This IP address should not be blocked by any firewalls, because this can cause unexpected behavior.</span></span>
    > <span data-ttu-id="6522c-151">Az Azure belső terheléselosztás esetében ezt az IP-címet használják a terheléselosztó figyelési mintavételezői az elosztott terhelésű készlet virtuális gépeinek állapotmeghatározásához.</span><span class="sxs-lookup"><span data-stu-id="6522c-151">With respect to Azure internal load balancing, this IP address is used by monitoring probes from the load balancer to determine the health state for virtual machines in a load-balanced set.</span></span> <span data-ttu-id="6522c-152">Ha egy belső elosztott terhelésű készletben hálózati biztonsági csoportot használnak az Azure virtuális gépekre irányuló forgalom korlátozásához, vagy egy virtuális alhálózaton alkalmazzák, győződjön meg arról, hogy a 168.63.129.16 címről érkező adatforgalom engedélyezéséhez felvettek egy hálózati biztonsági szabályt.</span><span class="sxs-lookup"><span data-stu-id="6522c-152">If a network security group is used to restrict traffic to Azure virtual machines in an internally load-balanced set or is applied to a virtual network subnet, ensure that a network security rule is added to allow traffic from 168.63.129.16.</span></span>

## <a name="create-nics"></a><span data-ttu-id="6522c-153">Hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="6522c-153">Create NICs</span></span>

<span data-ttu-id="6522c-154">Hálózati adaptereket kell létrehoznia (vagy a meglévőket is módosíthatja), és hozzá kell rendelnie őket a NAT-szabályokhoz, a terheléselosztási szabályokhoz és a mintavételezőkhöz.</span><span class="sxs-lookup"><span data-stu-id="6522c-154">You need to create NICs (or modify existing ones) and associate them to NAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="6522c-155">Hozzon létre egy hálózati adaptert *lb-nic1-be* néven, majd társítsa az *rdp1* NAT-szabályhoz és a *beilb* háttércímkészlethez.</span><span class="sxs-lookup"><span data-stu-id="6522c-155">Create an NIC named *lb-nic1-be*, and then associate it with the *rdp1* NAT rule and the *beilb* back-end address pool.</span></span>

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
    ```

    <span data-ttu-id="6522c-156">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="6522c-156">Expected output:</span></span>

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

2. <span data-ttu-id="6522c-157">Hozzon létre egy hálózati adaptert *lb-nic2-be* néven, majd társítsa az *rdp2* NAT-szabályhoz és a *beilb* háttércímkészlethez.</span><span class="sxs-lookup"><span data-stu-id="6522c-157">Create an NIC named *lb-nic2-be*, and then associate it with the *rdp2* NAT rule and the *beilb* back-end address pool.</span></span>

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. <span data-ttu-id="6522c-158">Hozzon létre egy virtuális gépet *DB1* néven, és társítsa az *lb-nic1-be* nevű hálózati adapterrel.</span><span class="sxs-lookup"><span data-stu-id="6522c-158">Create a virtual machine named *DB1*, and then associate it with the NIC named *lb-nic1-be*.</span></span> <span data-ttu-id="6522c-159">A következő parancs futtatása előtt létrejön a *web1nrp* nevű tárfiók:</span><span class="sxs-lookup"><span data-stu-id="6522c-159">A storage account called *web1nrp* is created before the following command runs:</span></span>

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > <span data-ttu-id="6522c-160">A terheléselosztó virtuális gépeinek ugyanabban a rendelkezésre állási készletben kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="6522c-160">VMs in a load balancer need to be in the same availability set.</span></span> <span data-ttu-id="6522c-161">Használja az `azure availset create` parancsot a rendelkezésre állási készlet létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="6522c-161">Use `azure availset create` to create an availability set.</span></span>

4. <span data-ttu-id="6522c-162">Hozzon létre egy virtuális gépet (VM) *DB2* néven, és társítsa az *lb-nic2-be* nevű hálózati adapterhez.</span><span class="sxs-lookup"><span data-stu-id="6522c-162">Create a virtual machine (VM) named *DB2*, and then associate it with the NIC named *lb-nic2-be*.</span></span> <span data-ttu-id="6522c-163">A következő parancs futtatása előtt létrejön a *web1nrp* nevű tárfiók.</span><span class="sxs-lookup"><span data-stu-id="6522c-163">A storage account called *web1nrp* was created before running the following command.</span></span>

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a><span data-ttu-id="6522c-164">Terheléselosztó törlése</span><span class="sxs-lookup"><span data-stu-id="6522c-164">Delete a load balancer</span></span>

<span data-ttu-id="6522c-165">A terheléselosztó eltávolításához használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6522c-165">To remove a load balancer, use the following command:</span></span>

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a><span data-ttu-id="6522c-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6522c-166">Next steps</span></span>

[<span data-ttu-id="6522c-167">A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával</span><span class="sxs-lookup"><span data-stu-id="6522c-167">Configure a load balancer distribution mode by using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="6522c-168">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6522c-168">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

