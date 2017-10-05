---
title: "A több hálózati adapterrel rendelkező Azure Linux virtuális gép létrehozása |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy Linux virtuális gép több hálózati adapter nem csatlakoztatható az Azure parancssori felületen vagy a Resource Manager sablonok használatával."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 814825cce61909167a1247a96c17a3ee9c5f2af4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-linux-virtual-machine-with-multiple-nics-using-the-azure-cli-10"></a><span data-ttu-id="7baa7-103">Az Azure CLI 1.0 használatával több hálózati adapterrel rendelkező Linux virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="7baa7-103">Create a Linux virtual machine with multiple NICs using the Azure CLI 1.0</span></span>
<span data-ttu-id="7baa7-104">Létrehozhat egy virtuális gép (VM), amelyen több virtuális hálózati adapterek (NIC) nem csatlakoztatható az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7baa7-104">You can create a virtual machine (VM) in Azure that has multiple virtual network interfaces (NICs) attached to it.</span></span> <span data-ttu-id="7baa7-105">Egy gyakori forgatókönyv, hogy az előtér- és kapcsolat, vagy a hálózaton, figyelési vagy biztonsági mentési megoldásra dedikált különböző alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="7baa7-105">A common scenario is to have different subnets for front-end and back-end connectivity, or a network dedicated to a monitoring or backup solution.</span></span> <span data-ttu-id="7baa7-106">A cikkben gyors parancsok futtatásával hozzon létre egy virtuális gép több hálózati adapter nem csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="7baa7-106">This article provides quick commands to create a VM with multiple NICs attached to it.</span></span> <span data-ttu-id="7baa7-107">Részletes információ, beleértve a saját Bash parancsfájlok belül több hálózati adapter létrehozása tudjon meg többet az [több hálózati Adapterrel virtuális gépek telepítése](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="7baa7-107">For detailed information, including how to create multiple NICs within your own Bash scripts, read more about [deploying multi-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span></span> <span data-ttu-id="7baa7-108">Különböző [Virtuálisgép-méretek](sizes.md) több hálózati adapter támogatja, így méretezés ennek megfelelően a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="7baa7-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

> [!WARNING]
> <span data-ttu-id="7baa7-109">Ha a virtuális gép létrehozása – az Azure CLI 1.0 a meglévő virtuális hálózati adapter nem adhat hozzá kell rendelni több hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="7baa7-109">You must attach multiple NICs when you create a VM - you cannot add NICs to an existing VM with the Azure CLI 1.0.</span></span> <span data-ttu-id="7baa7-110">Is [hálózati adapterek hozzáadása egy meglévő virtuális gépre az Azure CLI 2.0](multiple-nics.md).</span><span class="sxs-lookup"><span data-stu-id="7baa7-110">You can [add NICs to an existing VM with the Azure CLI 2.0](multiple-nics.md).</span></span> <span data-ttu-id="7baa7-111">Emellett [hozzon létre egy virtuális Gépet az eredeti virtuális lemez(ek) alapján](copy-vm.md) , és hozzon létre több hálózati adaptert a virtuális gép központi telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="7baa7-111">You can also [create a VM based on the original virtual disk(s)](copy-vm.md) and create multiple NICs as you deploy the VM.</span></span>


## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="7baa7-112">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="7baa7-112">CLI versions to complete the task</span></span>
<span data-ttu-id="7baa7-113">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="7baa7-113">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="7baa7-114">[Az Azure CLI 1.0](#create-supporting-resources) – a parancssori felületen a klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="7baa7-114">[Azure CLI 1.0](#create-supporting-resources) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="7baa7-115">[Azure CLI 2.0](multiple-nics.md) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="7baa7-115">[Azure CLI 2.0](multiple-nics.md) - our next generation CLI for the resource management deployment model</span></span>


## <a name="create-supporting-resources"></a><span data-ttu-id="7baa7-116">Kapcsolódó erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="7baa7-116">Create supporting resources</span></span>
<span data-ttu-id="7baa7-117">Győződjön meg arról, hogy rendelkezik a [Azure CLI](../../cli-install-nodejs.md) jelentkezett be, és a Resource Manager módot használ:</span><span class="sxs-lookup"><span data-stu-id="7baa7-117">Make sure that you have the [Azure CLI](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="7baa7-118">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="7baa7-118">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="7baa7-119">Példa paraméter nevekre *myResourceGroup*, *mystorageaccount*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="7baa7-119">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *myVM*.</span></span>

<span data-ttu-id="7baa7-120">Először hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="7baa7-120">First, create a resource group.</span></span> <span data-ttu-id="7baa7-121">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a a *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="7baa7-121">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
azure group create myResourceGroup --location eastus
```

<span data-ttu-id="7baa7-122">Hozzon létre egy tárfiókot, a virtuális gépek tárolására.</span><span class="sxs-lookup"><span data-stu-id="7baa7-122">Create a storage account to hold your VMs.</span></span> <span data-ttu-id="7baa7-123">Az alábbi példa létrehoz egy nevű tárfiók *mystorageaccount*:</span><span class="sxs-lookup"><span data-stu-id="7baa7-123">The following example creates a storage account named *mystorageaccount*:</span></span>

```azurecli
azure storage account create mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --kind Storage \
    --sku-name PLRS
```

<span data-ttu-id="7baa7-124">Csatlakozás a virtuális gép virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7baa7-124">Create a virtual network to connect your VMs to.</span></span> <span data-ttu-id="7baa7-125">Az alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* rendelkező egy címelőtagot *192.168.0.0/16*:</span><span class="sxs-lookup"><span data-stu-id="7baa7-125">The following example creates a virtual network named *myVnet* with an address prefix of *192.168.0.0/16*:</span></span>

```azurecli
azure network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="7baa7-126">Hozzon létre két virtuális hálózati alhálózat - egy előtér-forgalmat, egy, a háttér-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="7baa7-126">Create two virtual network subnets - one for front-end traffic and one for back-end traffic.</span></span> <span data-ttu-id="7baa7-127">Az alábbi példa létrehoz két alhálózat, nevű *mySubnetFrontEnd* és *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="7baa7-127">The following example creates two subnets, named *mySubnetFrontEnd* and *mySubnetBackEnd*:</span></span>

```azurecli
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetFrontEnd \
    --address-prefix 192.168.1.0/24
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

## <a name="create-and-configure-multiple-nics"></a><span data-ttu-id="7baa7-128">Létrehozhat és konfigurálhat több hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="7baa7-128">Create and configure multiple NICs</span></span>
<span data-ttu-id="7baa7-129">További részleteket olvashat [üzembe helyezése az Azure parancssori felület használatával több hálózati adapter](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), beleértve a hálózati adapterek létrehozása ismétlése folyamata parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="7baa7-129">You can read more details about [deploying multiple NICs using the Azure CLI](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), including scripting the process of looping through to create all the NICs.</span></span>

<span data-ttu-id="7baa7-130">Az alábbi példa létrehoz két hálózati adapterrel, nevű *myNic1* és *myNic2*, egyetlen hálózati adapterrel, minden egyes alhálózathoz csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="7baa7-130">The following example creates two NICs, named *myNic1* and *myNic2*, with one NIC connecting to each subnet:</span></span>

```azurecli
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic1 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetFrontEnd
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic2 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetBackEnd
```

<span data-ttu-id="7baa7-131">Általában akkor is létrehozhat egy [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) vagy [terheléselosztó](../../load-balancer/load-balancer-overview.md) segítségével kezelhetők és eloszthatók a forgalmat a virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="7baa7-131">Typically you also create a [Network Security Group](../../virtual-network/virtual-networks-nsg.md) or [load balancer](../../load-balancer/load-balancer-overview.md) to help manage and distribute traffic across your VMs.</span></span> <span data-ttu-id="7baa7-132">Az alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="7baa7-132">The following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="7baa7-133">A hálózati adapter a hálózati biztonsági csoport történő kötés `azure network nic set`.</span><span class="sxs-lookup"><span data-stu-id="7baa7-133">Bind your NICs to the Network Security Group using `azure network nic set`.</span></span> <span data-ttu-id="7baa7-134">Az alábbi példa köti *myNic1* és *myNic2* rendelkező *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="7baa7-134">The following example binds *myNic1* and *myNic2* with *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a><span data-ttu-id="7baa7-135">Hozzon létre egy virtuális Gépet, és csatlakoztassa a hálózati adapterek</span><span class="sxs-lookup"><span data-stu-id="7baa7-135">Create a VM and attach the NICs</span></span>
<span data-ttu-id="7baa7-136">A virtuális gép létrehozásakor most több hálózati adaptert adjon meg.</span><span class="sxs-lookup"><span data-stu-id="7baa7-136">When creating the VM, you now specify multiple NICs.</span></span> <span data-ttu-id="7baa7-137">Ahelyett, hogy használatával `--nic-name` egyetlen hálózati adapter biztosít, Ehelyett használhatja `--nic-names` , és adja meg a hálózati adapterek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="7baa7-137">Rather using `--nic-name` to provide a single NIC, instead you use `--nic-names` and provide a comma-separated list of NICs.</span></span> <span data-ttu-id="7baa7-138">Is kell vigyázni, ha a Virtuálisgép-méretet választja.</span><span class="sxs-lookup"><span data-stu-id="7baa7-138">You also need to take care when you select the VM size.</span></span> <span data-ttu-id="7baa7-139">Nincsenek korlátai, adhat hozzá egy virtuális hálózati adapterrel teljes száma.</span><span class="sxs-lookup"><span data-stu-id="7baa7-139">There are limits for the total number of NICs that you can add to a VM.</span></span> <span data-ttu-id="7baa7-140">Tudjon meg többet az [Linux Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="7baa7-140">Read more about [Linux VM sizes](sizes.md).</span></span> <span data-ttu-id="7baa7-141">A következő példa bemutatja, hogyan adhatja meg a több hálózati adapterrel és egy virtuális gép méretét, amely támogatja a több hálózati adapter (*Standard_DS2_v2*):</span><span class="sxs-lookup"><span data-stu-id="7baa7-141">The following example shows how to specify multiple NICs and then a VM size that supports using multiple NICs (*Standard_DS2_v2*):</span></span>

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username azureuser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

## <a name="create-multiple-nics-using-resource-manager-templates"></a><span data-ttu-id="7baa7-142">Resource Manager-sablonok segítségével több hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="7baa7-142">Create multiple NICs using Resource Manager templates</span></span>
<span data-ttu-id="7baa7-143">Az Azure Resource Manager-sablonok deklaratív JSON-fájlok segítségével határozza meg a környezetben.</span><span class="sxs-lookup"><span data-stu-id="7baa7-143">Azure Resource Manager templates use declarative JSON files to define your environment.</span></span> <span data-ttu-id="7baa7-144">Ha egy [áttekintése Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7baa7-144">You can read an [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="7baa7-145">Resource Manager-sablonok segítségével hozzon létre egy erőforrás több példánya központi telepítést végez, például több hálózati adapter létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="7baa7-145">Resource Manager templates provide a way to create multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="7baa7-146">Használhat *másolási* létrehozásához példányok száma:</span><span class="sxs-lookup"><span data-stu-id="7baa7-146">You use *copy* to specify the number of instances to create:</span></span>

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="7baa7-147">Tudjon meg többet az [használatával több példány létrehozásával *másolási*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="7baa7-147">Read more about [creating multiple instances using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="7baa7-148">Használhatja a `copyIndex()` majd hozzáfűzendő erőforrás nevét, amely lehetővé teszi, hogy hozzon létre több `myNic1`, `myNic2`stb. A következő hozzáfűzése a indexértéket példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="7baa7-148">You can also use a `copyIndex()` to then append a number to a resource name, which allows you to create `myNic1`, `myNic2`, etc. The following shows an example of appending the index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="7baa7-149">Átfogó példát olvasható [létrehozása a Resource Manager-sablonok segítségével több hálózati adapter](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="7baa7-149">You can read a complete example of [creating multiple NICs using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7baa7-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7baa7-150">Next steps</span></span>
<span data-ttu-id="7baa7-151">Mindenképpen tekintse át [Linux Virtuálisgép-méretek](sizes.md) több hálózati adapterrel rendelkező virtuális gép létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="7baa7-151">Make sure to review [Linux VM sizes](sizes.md) when trying to creating a VM with multiple NICs.</span></span> <span data-ttu-id="7baa7-152">Nagy figyelmet fordítani az egyes Virtuálisgép-méretet támogatja a hálózati adapterek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="7baa7-152">Pay attention to the maximum number of NICs each VM size supports.</span></span> 

<span data-ttu-id="7baa7-153">Ne feledje, hogy nem adható hozzá a további hálózati adapterek egy meglévő virtuális gépre, a virtuális gép telepítésekor létre kell hoznia a hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="7baa7-153">Remember that you cannot add additional NICs to an existing VM, you must create all the NICs when you deploy the VM.</span></span> <span data-ttu-id="7baa7-154">Győződjön meg arról, hogy rendelkezik-e a kezdettől szükséges hálózati kapcsolatot a központi telepítések tervezése során kezeli.</span><span class="sxs-lookup"><span data-stu-id="7baa7-154">Take care when planning your deployments to make sure that you have all the required network connectivity from the outset.</span></span>

