---
title: "a több hálózati adapterrel rendelkező Azure Linux virtuális gép aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate több hálózati adapterrel rendelkező Linux virtuális gép csatlakoztatott tooit hello Azure parancssori felületen vagy a Resource Manager sablonok használatával."
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
ms.openlocfilehash: 457dab734ceeeefd35cddaf1ebb9ea0a82f4e207
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-virtual-machine-with-multiple-nics-using-hello-azure-cli-10"></a><span data-ttu-id="e0475-103">Azure CLI 1.0 hello segítségével több hálózati adapterrel rendelkező Linux virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0475-103">Create a Linux virtual machine with multiple NICs using hello Azure CLI 1.0</span></span>
<span data-ttu-id="e0475-104">Létrehozhat egy virtuális gép (VM) több virtuális hálózati adapterek (NIC) kapcsolódó tooit rendelkező Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e0475-104">You can create a virtual machine (VM) in Azure that has multiple virtual network interfaces (NICs) attached tooit.</span></span> <span data-ttu-id="e0475-105">Egy általános forgatókönyv toohave különböző alhálózatokon a előtér- és hálózati kapcsolatot, vagy egy hálózati dedikált tooa figyelési vagy biztonsági mentési megoldás.</span><span class="sxs-lookup"><span data-stu-id="e0475-105">A common scenario is toohave different subnets for front-end and back-end connectivity, or a network dedicated tooa monitoring or backup solution.</span></span> <span data-ttu-id="e0475-106">Ez a cikk a virtuális gép több csatlakoztatott hálózati adapterrel tooit és gyors parancsok toocreate tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="e0475-106">This article provides quick commands toocreate a VM with multiple NICs attached tooit.</span></span> <span data-ttu-id="e0475-107">Részletes információkat, beleértve a hogyan toocreate saját belül több hálózati adapter Bash parancsfájlok, tudjon meg többet az [több hálózati Adapterrel virtuális gépek telepítése](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e0475-107">For detailed information, including how toocreate multiple NICs within your own Bash scripts, read more about [deploying multi-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span></span> <span data-ttu-id="e0475-108">Különböző [Virtuálisgép-méretek](sizes.md) több hálózati adapter támogatja, így méretezés ennek megfelelően a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="e0475-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

> [!WARNING]
> <span data-ttu-id="e0475-109">Ha a virtuális gép létrehozása – hello Azure CLI 1.0 a virtuális gép meglévő hálózati adapter tooan nem adhat hozzá kell rendelni több hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="e0475-109">You must attach multiple NICs when you create a VM - you cannot add NICs tooan existing VM with hello Azure CLI 1.0.</span></span> <span data-ttu-id="e0475-110">Is [adja hozzá a virtuális gép meglévő hello Azure CLI 2.0 a hálózati adapterek tooan](multiple-nics.md).</span><span class="sxs-lookup"><span data-stu-id="e0475-110">You can [add NICs tooan existing VM with hello Azure CLI 2.0](multiple-nics.md).</span></span> <span data-ttu-id="e0475-111">Emellett [hozzon létre egy virtuális Gépet hello eredeti virtuális lemez(ek) alapján](copy-vm.md) és több hálózati adaptert létrehozni, a központilag telepített virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="e0475-111">You can also [create a VM based on hello original virtual disk(s)](copy-vm.md) and create multiple NICs as you deploy hello VM.</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="e0475-112">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="e0475-112">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="e0475-113">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="e0475-113">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="e0475-114">[Az Azure CLI 1.0](#create-supporting-resources) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="e0475-114">[Azure CLI 1.0](#create-supporting-resources) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="e0475-115">[Az Azure CLI 2.0](multiple-nics.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="e0475-115">[Azure CLI 2.0](multiple-nics.md) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="create-supporting-resources"></a><span data-ttu-id="e0475-116">Kapcsolódó erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0475-116">Create supporting resources</span></span>
<span data-ttu-id="e0475-117">Győződjön meg arról, hogy rendelkezik-e hello [Azure CLI](../../cli-install-nodejs.md) jelentkezett be, és a Resource Manager módot használ:</span><span class="sxs-lookup"><span data-stu-id="e0475-117">Make sure that you have hello [Azure CLI](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="e0475-118">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="e0475-118">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="e0475-119">Példa paraméter nevekre *myResourceGroup*, *mystorageaccount*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="e0475-119">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *myVM*.</span></span>

<span data-ttu-id="e0475-120">Először hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="e0475-120">First, create a resource group.</span></span> <span data-ttu-id="e0475-121">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="e0475-121">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
azure group create myResourceGroup --location eastus
```

<span data-ttu-id="e0475-122">Hozzon létre egy tárolási fiók toohold a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e0475-122">Create a storage account toohold your VMs.</span></span> <span data-ttu-id="e0475-123">hello alábbi példa létrehoz egy nevű tárfiók *mystorageaccount*:</span><span class="sxs-lookup"><span data-stu-id="e0475-123">hello following example creates a storage account named *mystorageaccount*:</span></span>

```azurecli
azure storage account create mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --kind Storage \
    --sku-name PLRS
```

<span data-ttu-id="e0475-124">Hozzon létre egy virtuális hálózati tooconnect a virtuális gépek számára.</span><span class="sxs-lookup"><span data-stu-id="e0475-124">Create a virtual network tooconnect your VMs to.</span></span> <span data-ttu-id="e0475-125">hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* rendelkező egy címelőtagot *192.168.0.0/16*:</span><span class="sxs-lookup"><span data-stu-id="e0475-125">hello following example creates a virtual network named *myVnet* with an address prefix of *192.168.0.0/16*:</span></span>

```azurecli
azure network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefixes 192.168.0.0/16
```

<span data-ttu-id="e0475-126">Hozzon létre két virtuális hálózati alhálózat - egy előtér-forgalmat, egy, a háttér-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="e0475-126">Create two virtual network subnets - one for front-end traffic and one for back-end traffic.</span></span> <span data-ttu-id="e0475-127">hello alábbi példa létrehoz két alhálózat, nevű *mySubnetFrontEnd* és *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="e0475-127">hello following example creates two subnets, named *mySubnetFrontEnd* and *mySubnetBackEnd*:</span></span>

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

## <a name="create-and-configure-multiple-nics"></a><span data-ttu-id="e0475-128">Létrehozhat és konfigurálhat több hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="e0475-128">Create and configure multiple NICs</span></span>
<span data-ttu-id="e0475-129">További részleteket olvashat [hello Azure CLI segítségével több hálózati adapter telepítésével](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), beleértve parancsfájlokat ismétlése toocreate hello hálózati adapterek összes hello folyamatán.</span><span class="sxs-lookup"><span data-stu-id="e0475-129">You can read more details about [deploying multiple NICs using hello Azure CLI](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md), including scripting hello process of looping through toocreate all hello NICs.</span></span>

<span data-ttu-id="e0475-130">hello alábbi példa létrehoz két hálózati adapterrel, nevű *myNic1* és *myNic2*, egyetlen hálózati adapterrel tooeach csatlakozó:</span><span class="sxs-lookup"><span data-stu-id="e0475-130">hello following example creates two NICs, named *myNic1* and *myNic2*, with one NIC connecting tooeach subnet:</span></span>

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

<span data-ttu-id="e0475-131">Általában akkor is létrehozhat egy [hálózati biztonsági csoport](../../virtual-network/virtual-networks-nsg.md) vagy [terheléselosztó](../../load-balancer/load-balancer-overview.md) toohelp kezelése és a forgalom szétosztását a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="e0475-131">Typically you also create a [Network Security Group](../../virtual-network/virtual-networks-nsg.md) or [load balancer](../../load-balancer/load-balancer-overview.md) toohelp manage and distribute traffic across your VMs.</span></span> <span data-ttu-id="e0475-132">hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="e0475-132">hello following example creates a Network Security Group named *myNetworkSecurityGroup*:</span></span>

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

<span data-ttu-id="e0475-133">Kötést a hálózati adapterek toohello hálózati biztonsági csoport `azure network nic set`.</span><span class="sxs-lookup"><span data-stu-id="e0475-133">Bind your NICs toohello Network Security Group using `azure network nic set`.</span></span> <span data-ttu-id="e0475-134">hello alábbi példa köti *myNic1* és *myNic2* rendelkező *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="e0475-134">hello following example binds *myNic1* and *myNic2* with *myNetworkSecurityGroup*:</span></span>

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

## <a name="create-a-vm-and-attach-hello-nics"></a><span data-ttu-id="e0475-135">Hozzon létre egy virtuális Gépet, és hello hálózati adapter csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="e0475-135">Create a VM and attach hello NICs</span></span>
<span data-ttu-id="e0475-136">Hello virtuális gép létrehozásakor most több hálózati adaptert adjon meg.</span><span class="sxs-lookup"><span data-stu-id="e0475-136">When creating hello VM, you now specify multiple NICs.</span></span> <span data-ttu-id="e0475-137">Ahelyett, hogy használatával `--nic-name` tooprovide egyetlen hálózati adapter, akkor inkább `--nic-names` , és adja meg a hálózati adapterek vesszővel tagolt listája.</span><span class="sxs-lookup"><span data-stu-id="e0475-137">Rather using `--nic-name` tooprovide a single NIC, instead you use `--nic-names` and provide a comma-separated list of NICs.</span></span> <span data-ttu-id="e0475-138">Szükség tootake gondot kiválasztásakor hello Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="e0475-138">You also need tootake care when you select hello VM size.</span></span> <span data-ttu-id="e0475-139">Nincsenek korlátai hello vehet fel tooa virtuális gép hálózati adapterek száma.</span><span class="sxs-lookup"><span data-stu-id="e0475-139">There are limits for hello total number of NICs that you can add tooa VM.</span></span> <span data-ttu-id="e0475-140">Tudjon meg többet az [Linux Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="e0475-140">Read more about [Linux VM sizes](sizes.md).</span></span> <span data-ttu-id="e0475-141">hello következő példa bemutatja, hogyan toospecify több hálózati adapter és a virtuális gép mérete által támogatott segítségével több hálózati adapter (*Standard_DS2_v2*):</span><span class="sxs-lookup"><span data-stu-id="e0475-141">hello following example shows how toospecify multiple NICs and then a VM size that supports using multiple NICs (*Standard_DS2_v2*):</span></span>

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

## <a name="create-multiple-nics-using-resource-manager-templates"></a><span data-ttu-id="e0475-142">Resource Manager-sablonok segítségével több hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0475-142">Create multiple NICs using Resource Manager templates</span></span>
<span data-ttu-id="e0475-143">Az Azure Resource Manager-sablonok használata deklaratív JSON-fájlok toodefine a környezetben.</span><span class="sxs-lookup"><span data-stu-id="e0475-143">Azure Resource Manager templates use declarative JSON files toodefine your environment.</span></span> <span data-ttu-id="e0475-144">Ha egy [áttekintése Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e0475-144">You can read an [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="e0475-145">Resource Manager-sablonok adjon meg egy módon toocreate erőforrás több példánya központi telepítést végez, például több hálózati adapter létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="e0475-145">Resource Manager templates provide a way toocreate multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="e0475-146">Használhat *másolási* példányok toocreate toospecify hello száma:</span><span class="sxs-lookup"><span data-stu-id="e0475-146">You use *copy* toospecify hello number of instances toocreate:</span></span>

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="e0475-147">Tudjon meg többet az [használatával több példány létrehozásával *másolási*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="e0475-147">Read more about [creating multiple instances using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="e0475-148">Használhatja a `copyIndex()` toothen hozzáfűzése számú tooa erőforrás nevét, amely lehetővé teszi toocreate `myNic1`, `myNic2`, stb. hello következő hello indexértéket fűznek példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="e0475-148">You can also use a `copyIndex()` toothen append a number tooa resource name, which allows you toocreate `myNic1`, `myNic2`, etc. hello following shows an example of appending hello index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="e0475-149">Átfogó példát olvasható [létrehozása a Resource Manager-sablonok segítségével több hálózati adapter](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="e0475-149">You can read a complete example of [creating multiple NICs using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0475-150">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0475-150">Next steps</span></span>
<span data-ttu-id="e0475-151">Győződjön meg arról, hogy tooreview [Linux Virtuálisgép-méretek](sizes.md) toocreating több hálózati adapterrel rendelkező virtuális gép tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="e0475-151">Make sure tooreview [Linux VM sizes](sizes.md) when trying toocreating a VM with multiple NICs.</span></span> <span data-ttu-id="e0475-152">Nagy figyelmet toohello több hálózati adapter támogatja az egyes Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="e0475-152">Pay attention toohello maximum number of NICs each VM size supports.</span></span> 

<span data-ttu-id="e0475-153">Ne feledje, hogy nem adható hozzá a virtuális gép meglévő további hálózati adapterek tooan, hello virtuális gép telepítésekor létre kell hoznia minden hello hálózati adaptert.</span><span class="sxs-lookup"><span data-stu-id="e0475-153">Remember that you cannot add additional NICs tooan existing VM, you must create all hello NICs when you deploy hello VM.</span></span> <span data-ttu-id="e0475-154">Mi gondoskodunk a központi telepítések, hogy rendelkezik-e minden szükséges hello hálózati kapcsolat hello kezdettől toomake tervezése során.</span><span class="sxs-lookup"><span data-stu-id="e0475-154">Take care when planning your deployments toomake sure that you have all hello required network connectivity from hello outset.</span></span>

