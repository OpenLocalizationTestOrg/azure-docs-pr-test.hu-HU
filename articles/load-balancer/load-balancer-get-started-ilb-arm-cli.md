---
title: "aaaCreate egy belső terheléselosztó - Azure parancssori Felülettel |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate a belső terheléselosztók használatával hello Azure CLI az erőforrás-kezelőben"
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
ms.openlocfilehash: 3aea6fdb07600f0d661ec6b8ffc784b03380a127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-by-using-hello-azure-cli"></a><span data-ttu-id="a9413-103">Hozzon létre egy belső elosztott terhelésű hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="a9413-103">Create an internal load balancer by using hello Azure CLI</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="a9413-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a9413-104">Azure Portal</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [<span data-ttu-id="a9413-105">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a9413-105">PowerShell</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [<span data-ttu-id="a9413-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="a9413-106">Azure CLI</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [<span data-ttu-id="a9413-107">Sablon</span><span class="sxs-lookup"><span data-stu-id="a9413-107">Template</span></span>](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> <span data-ttu-id="a9413-108">Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="a9413-108">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="a9413-109">Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](load-balancer-get-started-ilb-classic-cli.md).</span><span class="sxs-lookup"><span data-stu-id="a9413-109">This article covers using hello Resource Manager deployment model, which Microsoft recommends for most new deployments instead of hello [classic deployment model](load-balancer-get-started-ilb-classic-cli.md).</span></span>

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="deploy-hello-solution-by-using-hello-azure-cli"></a><span data-ttu-id="a9413-110">Hello megoldás telepítése hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="a9413-110">Deploy hello solution by using hello Azure CLI</span></span>

<span data-ttu-id="a9413-111">hello lépések bemutatják, hogyan toocreate egy internetre irányuló terheléselosztó CLI Azure Resource Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="a9413-111">hello following steps show how toocreate an Internet-facing load balancer by using Azure Resource Manager with CLI.</span></span> <span data-ttu-id="a9413-112">Az Azure Resource Manager az egyes erőforrások jön létre, és egyenként konfigurálni, majd tegye együtt toocreate erőforrás.</span><span class="sxs-lookup"><span data-stu-id="a9413-112">With Azure Resource Manager, each resource is created and configured individually, and then put together toocreate a resource.</span></span>

<span data-ttu-id="a9413-113">Toocreate kell, és a következő objektumok toodeploy terheléselosztó hello konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="a9413-113">You need toocreate and configure hello following objects toodeploy a load balancer:</span></span>

* <span data-ttu-id="a9413-114">**Előtérbeli IP-konfiguráció**: a nyilvános IP-címeket tartalmazza a bejövő hálózati forgalomhoz</span><span class="sxs-lookup"><span data-stu-id="a9413-114">**Front-end IP configuration**: contains public IP addresses for incoming network traffic</span></span>
* <span data-ttu-id="a9413-115">**Háttér-címkészlet**: tartalmaz, amelyek lehetővé teszik a hello virtuális gépek tooreceive hálózati forgalmat a hello terheléselosztó hálózati adapterek (NIC)</span><span class="sxs-lookup"><span data-stu-id="a9413-115">**Back-end address pool**: contains network interfaces (NICs) that enable hello virtual machines tooreceive network traffic from hello load balancer</span></span>
* <span data-ttu-id="a9413-116">**Terheléselosztási szabályok**: hello load balancer tooport hello háttér-címkészletbeli nyilvános port hozzárendelését szabályokat tartalmaz</span><span class="sxs-lookup"><span data-stu-id="a9413-116">**Load-balancing rules**: contains rules that map a public port on hello load balancer tooport in hello back-end address pool</span></span>
* <span data-ttu-id="a9413-117">**Bejövő NAT-szabályok**: hello terhelés terheléselosztó tooa port egy adott virtuális gép hello háttér-címkészletbeli nyilvános port hozzárendelését szabályokat tartalmaz</span><span class="sxs-lookup"><span data-stu-id="a9413-117">**Inbound NAT rules**: contains rules that map a public port on hello load balancer tooa port for a specific virtual machine in hello back-end address pool</span></span>
* <span data-ttu-id="a9413-118">**Vizsgálat**: tartalmaz, amelyek a virtuális gépek példánya hello háttér címkészletet használt toocheck hello rendelkezésre állási állapotát mintavételt</span><span class="sxs-lookup"><span data-stu-id="a9413-118">**Probes**: contains health probes that are used toocheck hello availability of virtual machines instances in hello back-end address pool</span></span>

<span data-ttu-id="a9413-119">A további információkat az [Azure Resource Manager support for Load Balancer](load-balancer-arm.md) (Azure Resource Manager-támogatás a terheléselosztóhoz) című rész tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a9413-119">For more information, see [Azure Resource Manager support for Load Balancer](load-balancer-arm.md).</span></span>

## <a name="set-up-cli-toouse-resource-manager"></a><span data-ttu-id="a9413-120">Parancssori felület toouse erőforrás-kezelő beállítása</span><span class="sxs-lookup"><span data-stu-id="a9413-120">Set up CLI toouse Resource Manager</span></span>

1. <span data-ttu-id="a9413-121">Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="a9413-121">If you have never used Azure CLI, see [Install and configure hello Azure CLI](../cli-install-nodejs.md).</span></span> <span data-ttu-id="a9413-122">Hello követésével mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.</span><span class="sxs-lookup"><span data-stu-id="a9413-122">Follow hello instructions up toohello point where you select your Azure account and subscription.</span></span>
2. <span data-ttu-id="a9413-123">Futtassa a hello **azure config mód** tooswitch tooResource kezelő módra, az alábbiak szerint parancsot:</span><span class="sxs-lookup"><span data-stu-id="a9413-123">Run hello **azure config mode** command tooswitch tooResource Manager mode, as follows:</span></span>

    ```azurecli
    azure config mode arm
    ```

    <span data-ttu-id="a9413-124">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="a9413-124">Expected output:</span></span>

        info:    New mode is arm

## <a name="create-an-internal-load-balancer-step-by-step"></a><span data-ttu-id="a9413-125">Belső terheléselosztó létrehozásának lépései</span><span class="sxs-lookup"><span data-stu-id="a9413-125">Create an internal load balancer step by step</span></span>

1. <span data-ttu-id="a9413-126">Jelentkezzen be tooAzure.</span><span class="sxs-lookup"><span data-stu-id="a9413-126">Sign in tooAzure.</span></span>

    ```azurecli
    azure login
    ```

    <span data-ttu-id="a9413-127">Amikor a rendszer erre kéri, adja meg Azure rendszerbeli hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a9413-127">When prompted, enter your Azure credentials.</span></span>

2. <span data-ttu-id="a9413-128">Módosítsa a hello parancs eszközök tooAzure Resource Manager módra.</span><span class="sxs-lookup"><span data-stu-id="a9413-128">Change hello command tools tooAzure Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```

## <a name="create-a-resource-group"></a><span data-ttu-id="a9413-129">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="a9413-129">Create a resource group</span></span>

<span data-ttu-id="a9413-130">Az Azure Resource Managerben minden erőforrás egy erőforráscsoporthoz van társítva.</span><span class="sxs-lookup"><span data-stu-id="a9413-130">All resources in Azure Resource Manager are associated with a resource group.</span></span> <span data-ttu-id="a9413-131">Ha még nem tette meg, hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a9413-131">If you haven't done so yet, create a resource group.</span></span>

```azurecli
azure group create <resource group name> <location>
```

## <a name="create-an-internal-load-balancer-set"></a><span data-ttu-id="a9413-132">Hozzon létre egy belső terheléselosztó készletet</span><span class="sxs-lookup"><span data-stu-id="a9413-132">Create an internal load balancer set</span></span>

1. <span data-ttu-id="a9413-133">Hozzon létre egy belső terheléselosztót</span><span class="sxs-lookup"><span data-stu-id="a9413-133">Create an internal load balancer</span></span>

    <span data-ttu-id="a9413-134">USA keleti régiójában hello forgatókönyv a következő, a nrprg nevű erőforráscsoport jön létre.</span><span class="sxs-lookup"><span data-stu-id="a9413-134">In hello following scenario, a resource group named nrprg is created in East US region.</span></span>

    ```azurecli
    azure network lb create --name nrprg --location eastus
    ```

   > [!NOTE]
   > <span data-ttu-id="a9413-135">Belső terheléselosztók, például a virtuális hálózatok és virtuális hálózati alhálózat összes erőforrás kell hello ugyanabban az erőforráscsoportban, és a hello azonos régióban.</span><span class="sxs-lookup"><span data-stu-id="a9413-135">All resources for an internal load balancers, such as virtual networks and virtual network subnets, must be in hello same resource group and in hello same region.</span></span>

2. <span data-ttu-id="a9413-136">Hozzon létre egy hello belső terheléselosztó előtér-IP-címet.</span><span class="sxs-lookup"><span data-stu-id="a9413-136">Create a front-end IP address for hello internal load balancer.</span></span>

    <span data-ttu-id="a9413-137">hello IP-cím, amelyekkel a virtuális hálózat hello alhálózati tartományba kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a9413-137">hello IP address that you use must be within hello subnet range of your virtual network.</span></span>

    ```azurecli
    azure network lb frontend-ip create --resource-group nrprg --lb-name ilbset --name feilb --private-ip-address 10.0.0.7 --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet
    ```

3. <span data-ttu-id="a9413-138">Hello háttér-címkészlet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a9413-138">Create hello back-end address pool.</span></span>

    ```azurecli
    azure network lb address-pool create --resource-group nrprg --lb-name ilbset --name beilb
    ```

    <span data-ttu-id="a9413-139">Miután megtörtént az előtérbeli IP-cím, valamint a háttércímkészlet definiálása, létre lehet hozni a terheléselosztási szabályokat, a bejövő NAT-szabályokat, valamint a testre szabott állapotfigyelő mintavételezőket.</span><span class="sxs-lookup"><span data-stu-id="a9413-139">After you define a front-end IP address and a back-end address pool, you can create load balancer rules, inbound NAT rules, and customized health probes.</span></span>

4. <span data-ttu-id="a9413-140">Hozzon létre olyan terheléselosztó szabályhoz hello belső terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="a9413-140">Create a load balancer rule for hello internal load balancer.</span></span>

    <span data-ttu-id="a9413-141">Hello előző lépések végrehajtásával, ha hello parancs hoz létre egy terheléselosztó szabály, figyelő tooport 1433 a hello előtér-készlet és a küldő terhelésű hálózati forgalom toohello háttér címkészletet, továbbá használatával az 1433-as port.</span><span class="sxs-lookup"><span data-stu-id="a9413-141">When you follow hello previous steps, hello command creates a load-balancer rule for listening tooport 1433 in hello front-end pool and sending load-balanced network traffic toohello back-end address pool, also using port 1433.</span></span>

    ```azurecli
    azure network lb rule create --resource-group nrprg --lb-name ilbset --name ilbrule --protocol tcp --frontend-port 1433 --backend-port 1433 --frontend-ip-name feilb --backend-address-pool-name beilb
    ```

5. <span data-ttu-id="a9413-142">Hozza létre a bejövő NAT-szabályokat.</span><span class="sxs-lookup"><span data-stu-id="a9413-142">Create inbound NAT rules.</span></span>

    <span data-ttu-id="a9413-143">Bejövő NAT-szabályok olyan használt toocreate végpontok egy terheléselosztó, amely tooa adott virtuálisgép-példányt.</span><span class="sxs-lookup"><span data-stu-id="a9413-143">Inbound NAT rules are used toocreate endpoints in a load balancer that go tooa specific virtual machine instance.</span></span> <span data-ttu-id="a9413-144">hello előző lépést a távoli asztal két NAT-szabályok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a9413-144">hello previous steps created two NAT rules  for remote desktop.</span></span>

    ```azurecli
    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule1 --protocol TCP --frontend-port 5432 --backend-port 3389

    azure network lb inbound-nat-rule create --resource-group nrprg --lb-name ilbset --name NATrule2 --protocol TCP --frontend-port 5433 --backend-port 3389
    ```

6. <span data-ttu-id="a9413-145">Hozzon létre állapotfigyelő mintavételt hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="a9413-145">Create health probes for hello load balancer.</span></span>

    <span data-ttu-id="a9413-146">Egy állapotmintáihoz ellenőrzi az összes virtuális gép példányok toomake meg arról, hogy a hálózati forgalom elküldése lehetséges.</span><span class="sxs-lookup"><span data-stu-id="a9413-146">A health probe checks all virtual machine instances toomake sure they can send network traffic.</span></span> <span data-ttu-id="a9413-147">hello virtuálisgép-példány sikertelen mintavételi ellenőrzések hello terheléselosztó törlődik, amíg újra online állapotba kerül, és a mintavétel-ellenőrzés határozza meg, hogy a rendszer kifogástalan.</span><span class="sxs-lookup"><span data-stu-id="a9413-147">hello virtual machine instance with failed probe checks is removed from hello load balancer until it goes back online and a probe check determines that it's healthy.</span></span>

    ```azurecli
    azure network lb probe create --resource-group nrprg --lb-name ilbset --name ilbprobe --protocol tcp --interval 300 --count 4
    ```

    > [!NOTE]
    > <span data-ttu-id="a9413-148">hello Microsoft Azure platform különféle felügyeleti forgatókönyvekhez, amik egy nyilvánosan elérhető, statikus IPv4-címet használ.</span><span class="sxs-lookup"><span data-stu-id="a9413-148">hello Microsoft Azure platform uses a static, publicly routable IPv4 address for a variety of administrative scenarios.</span></span> <span data-ttu-id="a9413-149">hello IP-cím 168.63.129.16.</span><span class="sxs-lookup"><span data-stu-id="a9413-149">hello IP address is 168.63.129.16.</span></span> <span data-ttu-id="a9413-150">Ezt az IP-címet nem blokkolhatja egy tűzfal sem, mert ez nem várt viselkedést okozhat.</span><span class="sxs-lookup"><span data-stu-id="a9413-150">This IP address should not be blocked by any firewalls, because this can cause unexpected behavior.</span></span>
    > <span data-ttu-id="a9413-151">A tekintetben tooAzure belső terheléselosztás, az IP-címet használják mintavételt hello load balancer toodetermine hello állapotát a virtuális gépek egy elosztott terhelésű készlet figyelése.</span><span class="sxs-lookup"><span data-stu-id="a9413-151">With respect tooAzure internal load balancing, this IP address is used by monitoring probes from hello load balancer toodetermine hello health state for virtual machines in a load-balanced set.</span></span> <span data-ttu-id="a9413-152">Ha egy hálózati biztonsági csoportot használt toorestrict forgalom tooAzure virtuális gépek egy belső elosztott terhelésű készlet vagy alkalmazott tooa virtuális hálózati alhálózat, győződjön meg arról, hogy a hálózati biztonsági szabály legyen-e adva a 168.63.129.16 tooallow forgalom.</span><span class="sxs-lookup"><span data-stu-id="a9413-152">If a network security group is used toorestrict traffic tooAzure virtual machines in an internally load-balanced set or is applied tooa virtual network subnet, ensure that a network security rule is added tooallow traffic from 168.63.129.16.</span></span>

## <a name="create-nics"></a><span data-ttu-id="a9413-153">Hálózati adapterek létrehozása</span><span class="sxs-lookup"><span data-stu-id="a9413-153">Create NICs</span></span>

<span data-ttu-id="a9413-154">Toocreate hálózati adapterrel kell (vagy módosíthatja a meglévőket), és rendelje hozzá őket tooNAT szabályok, load balancer szabályok és mintavételek menüpontban.</span><span class="sxs-lookup"><span data-stu-id="a9413-154">You need toocreate NICs (or modify existing ones) and associate them tooNAT rules, load balancer rules, and probes.</span></span>

1. <span data-ttu-id="a9413-155">Hozzon létre egy olyan hálózati adapter nevű *lb nic1 kell*, és társíthatja hello *rdp1* NAT szabály és hello *beilb* háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="a9413-155">Create an NIC named *lb-nic1-be*, and then associate it with hello *rdp1* NAT rule and hello *beilb* back-end address pool.</span></span>

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic1-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp1" --location eastus
    ```

    <span data-ttu-id="a9413-156">Várt kimenet:</span><span class="sxs-lookup"><span data-stu-id="a9413-156">Expected output:</span></span>

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

2. <span data-ttu-id="a9413-157">Hozzon létre egy olyan hálózati adapter nevű *lb nic2 kell*, és társíthatja hello *rdp2* NAT szabály és hello *beilb* háttér címkészletet.</span><span class="sxs-lookup"><span data-stu-id="a9413-157">Create an NIC named *lb-nic2-be*, and then associate it with hello *rdp2* NAT rule and hello *beilb* back-end address pool.</span></span>

    ```azurecli
    azure network nic create --resource-group nrprg --name lb-nic2-be --subnet-name nrpvnetsubnet --subnet-vnet-name nrpvnet --lb-address-pool-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/backendAddressPools/beilb" --lb-inbound-nat-rule-ids "/subscriptions/####################################/resourceGroups/nrprg/providers/Microsoft.Network/loadBalancers/nrplb/inboundNatRules/rdp2" --location eastus
    ```

3. <span data-ttu-id="a9413-158">Nevű virtuális gép létrehozása *D1*, és társíthatja hello nevű hálózati *lb nic1 kell*.</span><span class="sxs-lookup"><span data-stu-id="a9413-158">Create a virtual machine named *DB1*, and then associate it with hello NIC named *lb-nic1-be*.</span></span> <span data-ttu-id="a9413-159">A tárfiók neve *web1nrp* hello a következő parancs futtatása előtt hozza létre:</span><span class="sxs-lookup"><span data-stu-id="a9413-159">A storage account called *web1nrp* is created before hello following command runs:</span></span>

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB1 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic1-be --availset-name nrp-avset --storage-account-name web1nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```
    > [!IMPORTANT]
    > <span data-ttu-id="a9413-160">A betöltési terheléselosztó kell toobe a virtuális gépek hello azonos rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="a9413-160">VMs in a load balancer need toobe in hello same availability set.</span></span> <span data-ttu-id="a9413-161">Használjon `azure availset create` toocreate rendelkezésre állási készlet.</span><span class="sxs-lookup"><span data-stu-id="a9413-161">Use `azure availset create` toocreate an availability set.</span></span>

4. <span data-ttu-id="a9413-162">Hozzon létre egy virtuális gépet (VM) nevű *DB2*, és társíthatja hello nevű hálózati *lb nic2 kell*.</span><span class="sxs-lookup"><span data-stu-id="a9413-162">Create a virtual machine (VM) named *DB2*, and then associate it with hello NIC named *lb-nic2-be*.</span></span> <span data-ttu-id="a9413-163">A tárfiók neve *web1nrp* hello a következő parancs futtatása előtt lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="a9413-163">A storage account called *web1nrp* was created before running hello following command.</span></span>

    ```azurecli
    azure vm create --resource--resource-grouproup nrprg --name DB2 --location eastus --vnet-name nrpvnet --vnet-subnet-name nrpvnetsubnet --nic-name lb-nic2-be --availset-name nrp-avset --storage-account-name web2nrp --os-type Windows --image-urn MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:4.0.20150825
    ```

## <a name="delete-a-load-balancer"></a><span data-ttu-id="a9413-164">Terheléselosztó törlése</span><span class="sxs-lookup"><span data-stu-id="a9413-164">Delete a load balancer</span></span>

<span data-ttu-id="a9413-165">egy terhelés-kiegyenlítő tooremove hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="a9413-165">tooremove a load balancer, use hello following command:</span></span>

```azurecli
azure network lb delete --resource-group nrprg --name ilbset
```

## <a name="next-steps"></a><span data-ttu-id="a9413-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a9413-166">Next steps</span></span>

[<span data-ttu-id="a9413-167">A terheléselosztó elosztási módjának konfigurálása forrás IP-affinitás használatával</span><span class="sxs-lookup"><span data-stu-id="a9413-167">Configure a load balancer distribution mode by using source IP affinity</span></span>](load-balancer-distribution-mode.md)

[<span data-ttu-id="a9413-168">A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a9413-168">Configure idle TCP timeout settings for your load balancer</span></span>](load-balancer-tcp-idle-timeout.md)

