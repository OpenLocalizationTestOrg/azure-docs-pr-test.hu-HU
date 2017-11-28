---
title: "A több hálózati adapterrel rendelkező Azure Linux virtuális gép létrehozása |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy Linux virtuális gép több hálózati adapter nem csatlakoztatható az Azure CLI 2.0 vagy az erőforrás-kezelő sablonok használatával."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 5d2d04d0-fc62-45fa-88b1-61808a2bc691
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 8a2931e462079c101c91497d459d7d3126234244
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a><span data-ttu-id="cf181-103">Hogyan Linux virtuális gép létrehozása az Azure-ban a több hálózati kártyák</span><span class="sxs-lookup"><span data-stu-id="cf181-103">How to create a Linux virtual machine in Azure with multiple network interface cards</span></span>
<span data-ttu-id="cf181-104">Létrehozhat egy virtuális gép (VM), amelyen több virtuális hálózati adapterek (NIC) nem csatlakoztatható az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="cf181-104">You can create a virtual machine (VM) in Azure that has multiple virtual network interfaces (NICs) attached to it.</span></span> <span data-ttu-id="cf181-105">Egy gyakori forgatókönyv, hogy az előtér- és kapcsolat, vagy a hálózaton, figyelési vagy biztonsági mentési megoldásra dedikált különböző alhálózatokon.</span><span class="sxs-lookup"><span data-stu-id="cf181-105">A common scenario is to have different subnets for front-end and back-end connectivity, or a network dedicated to a monitoring or backup solution.</span></span> <span data-ttu-id="cf181-106">Ez a cikk részletesen több hálózati adapter nem csatlakoztatható a virtuális gép létrehozása és hozzáadása vagy eltávolítása a hálózati adapter egy meglévő virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="cf181-106">This article details how to create a VM with multiple NICs attached to it and how to add or remove NICs from an existing VM.</span></span> <span data-ttu-id="cf181-107">Részletes információ, beleértve a saját Bash parancsfájlok belül több hálózati adapter létrehozása tudjon meg többet az [több hálózati Adapterrel virtuális gépek telepítése](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="cf181-107">For detailed information, including how to create multiple NICs within your own Bash scripts, read more about [deploying multi-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span></span> <span data-ttu-id="cf181-108">Különböző [Virtuálisgép-méretek](sizes.md) több hálózati adapter támogatja, így méretezés ennek megfelelően a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="cf181-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

<span data-ttu-id="cf181-109">Ez a cikk részletezi az Azure CLI 2.0 több hálózati adapterrel rendelkező virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cf181-109">This article details how to create a VM with multiple NICs with the Azure CLI 2.0.</span></span> <span data-ttu-id="cf181-110">Az [Azure CLI 1.0-s](multiple-nics-nodejs.md) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cf181-110">You can also perform these steps with the [Azure CLI 1.0](multiple-nics-nodejs.md).</span></span>


## <a name="create-supporting-resources"></a><span data-ttu-id="cf181-111">Kapcsolódó erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf181-111">Create supporting resources</span></span>
<span data-ttu-id="cf181-112">Telepítse a legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) és való bejelentkezéshez az Azure fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="cf181-112">Install the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="cf181-113">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="cf181-113">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="cf181-114">Példa paraméter nevekre *myResourceGroup*, *mystorageaccount*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="cf181-114">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *myVM*.</span></span>

<span data-ttu-id="cf181-115">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="cf181-115">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="cf181-116">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a a *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="cf181-116">The following example creates a resource group named *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="cf181-117">A virtuális hálózat létrehozása [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="cf181-117">Create the virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="cf181-118">Az alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* és nevű alhálózat *mySubnetFrontEnd*:</span><span class="sxs-lookup"><span data-stu-id="cf181-118">The following example creates a virtual network named *myVnet* and subnet named *mySubnetFrontEnd*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="cf181-119">Hozzon létre egy alhálózatot a háttér-forgalom [az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet#create).</span><span class="sxs-lookup"><span data-stu-id="cf181-119">Create a subnet for the back-end traffic with [az network vnet subnet create](/cli/azure/network/vnet/subnet#create).</span></span> <span data-ttu-id="cf181-120">Az alábbi példakód létrehozza nevű alhálózat *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="cf181-120">The following example creates a subnet named *mySubnetBackEnd*:</span></span>

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

<span data-ttu-id="cf181-121">Hozzon létre egy hálózati biztonsági csoport [az hálózati nsg létrehozása](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="cf181-121">Create a network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="cf181-122">Az alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="cf181-122">The following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a><span data-ttu-id="cf181-123">Létrehozhat és konfigurálhat több hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="cf181-123">Create and configure multiple NICs</span></span>
<span data-ttu-id="cf181-124">Hozzon létre két hálózati adapterrel [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="cf181-124">Create two NICs with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="cf181-125">Az alábbi példa létrehoz két hálózati adapterrel, nevű *myNic1* és *myNic2*, a hálózati biztonsági csoport csatlakoztatva egy NIC kártyával, minden egyes alhálózathoz csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="cf181-125">The following example creates two NICs, named *myNic1* and *myNic2*, connected the network security group, with one NIC connecting to each subnet:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic1 \
    --vnet-name myVnet \
    --subnet mySubnetFrontEnd \
    --network-security-group myNetworkSecurityGroup
az network nic create \
    --resource-group myResourceGroup \
    --name myNic2 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a><span data-ttu-id="cf181-126">Hozzon létre egy virtuális Gépet, és csatlakoztassa a hálózati adapterek</span><span class="sxs-lookup"><span data-stu-id="cf181-126">Create a VM and attach the NICs</span></span>
<span data-ttu-id="cf181-127">A virtuális gép létrehozásakor adja meg a hálózati adapter segítségével létrehozott `--nics`.</span><span class="sxs-lookup"><span data-stu-id="cf181-127">When you create the VM, specify the NICs you created with `--nics`.</span></span> <span data-ttu-id="cf181-128">Is kell vigyázni, ha a Virtuálisgép-méretet választja.</span><span class="sxs-lookup"><span data-stu-id="cf181-128">You also need to take care when you select the VM size.</span></span> <span data-ttu-id="cf181-129">Nincsenek korlátai, adhat hozzá egy virtuális hálózati adapterrel teljes száma.</span><span class="sxs-lookup"><span data-stu-id="cf181-129">There are limits for the total number of NICs that you can add to a VM.</span></span> <span data-ttu-id="cf181-130">Tudjon meg többet az [Linux Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="cf181-130">Read more about [Linux VM sizes](sizes.md).</span></span> 

<span data-ttu-id="cf181-131">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="cf181-131">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="cf181-132">Az alábbi példakód létrehozza a virtuális gépek nevű *myVM*:</span><span class="sxs-lookup"><span data-stu-id="cf181-132">The following example creates a VM named *myVM*:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --size Standard_DS3_v2 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --nics myNic1 myNic2
```

## <a name="add-a-nic-to-a-vm"></a><span data-ttu-id="cf181-133">Adja hozzá egy hálózati Adaptert egy virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="cf181-133">Add a NIC to a VM</span></span>
<span data-ttu-id="cf181-134">Az előző lépésekben létrehozott egy virtuális gép több hálózati adapterrel rendelkező.</span><span class="sxs-lookup"><span data-stu-id="cf181-134">The previous steps created a VM with multiple NICs.</span></span> <span data-ttu-id="cf181-135">Hálózati adapter egy meglévő virtuális gépre az Azure CLI 2.0 is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="cf181-135">You can also add NICs to an existing VM with the Azure CLI 2.0.</span></span> 

<span data-ttu-id="cf181-136">Hozzon létre egy másik hálózati Adaptert a [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="cf181-136">Create another NIC with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="cf181-137">Az alábbi példa létrehoz egy hálózati adapter nevű *myNic3* a háttér-alhálózat és a hálózati biztonsági csoport az előző lépésekben létrehozott csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="cf181-137">The following example creates a NIC named *myNic3* connected to the back-end subnet and network security group created in the previous steps:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="cf181-138">A hálózati adapter hozzáadása egy meglévő virtuális Gépre, először a virtuális Géphez a felszabadítani [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="cf181-138">To add a NIC to an existing VM, first deallocate the VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="cf181-139">Az alábbi példa felszabadítja a nevű virtuális gép *myVM*:</span><span class="sxs-lookup"><span data-stu-id="cf181-139">The following example deallocates the VM named *myVM*:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="cf181-140">Adja hozzá a hálózati adapter [az vm hálózati adapter hozzáadása](/cli/azure/vm/nic#add).</span><span class="sxs-lookup"><span data-stu-id="cf181-140">Add the NIC with [az vm nic add](/cli/azure/vm/nic#add).</span></span> <span data-ttu-id="cf181-141">A következő példakóddal felveheti *myNic3* való *myVM*:</span><span class="sxs-lookup"><span data-stu-id="cf181-141">The following example adds *myNic3* to *myVM*:</span></span>

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

<span data-ttu-id="cf181-142">Indítsa el a virtuális Géphez a [az vm indítása](/cli/azure/vm#start):</span><span class="sxs-lookup"><span data-stu-id="cf181-142">Start the VM with [az vm start](/cli/azure/vm#start):</span></span>

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a><span data-ttu-id="cf181-143">A virtuális gép egy hálózati adapter eltávolítása</span><span class="sxs-lookup"><span data-stu-id="cf181-143">Remove a NIC from a VM</span></span>
<span data-ttu-id="cf181-144">Meglévő virtuális hálózati Adapterhez eltávolításához először a virtuális Géphez a felszabadítani [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="cf181-144">To remove a NIC from an existing VM, first deallocate the VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="cf181-145">Az alábbi példa felszabadítja a nevű virtuális gép *myVM*:</span><span class="sxs-lookup"><span data-stu-id="cf181-145">The following example deallocates the VM named *myVM*:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="cf181-146">Távolítsa el a hálózati adapter [az vm hálózati adapter eltávolítása](/cli/azure/vm/nic#remove).</span><span class="sxs-lookup"><span data-stu-id="cf181-146">Remove the NIC with [az vm nic remove](/cli/azure/vm/nic#remove).</span></span> <span data-ttu-id="cf181-147">A következő példában eltávolítjuk *myNic3* a *myVM*:</span><span class="sxs-lookup"><span data-stu-id="cf181-147">The following example removes *myNic3* from *myVM*:</span></span>

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

<span data-ttu-id="cf181-148">Indítsa el a virtuális Géphez a [az vm indítása](/cli/azure/vm#start):</span><span class="sxs-lookup"><span data-stu-id="cf181-148">Start the VM with [az vm start](/cli/azure/vm#start):</span></span>

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a><span data-ttu-id="cf181-149">Resource Manager-sablonok segítségével több hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="cf181-149">Create multiple NICs using Resource Manager templates</span></span>
<span data-ttu-id="cf181-150">Az Azure Resource Manager-sablonok deklaratív JSON-fájlok segítségével határozza meg a környezetben.</span><span class="sxs-lookup"><span data-stu-id="cf181-150">Azure Resource Manager templates use declarative JSON files to define your environment.</span></span> <span data-ttu-id="cf181-151">Ha egy [áttekintése Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cf181-151">You can read an [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="cf181-152">Resource Manager-sablonok segítségével hozzon létre egy erőforrás több példánya központi telepítést végez, például több hálózati adapter létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="cf181-152">Resource Manager templates provide a way to create multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="cf181-153">Használhat *másolási* létrehozásához példányok száma:</span><span class="sxs-lookup"><span data-stu-id="cf181-153">You use *copy* to specify the number of instances to create:</span></span>

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="cf181-154">Tudjon meg többet az [használatával több példány létrehozásával *másolási*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="cf181-154">Read more about [creating multiple instances using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="cf181-155">Használhatja a `copyIndex()` majd hozzáfűzendő erőforrás nevét, amely lehetővé teszi, hogy hozzon létre több `myNic1`, `myNic2`stb. A következő hozzáfűzése a indexértéket példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="cf181-155">You can also use a `copyIndex()` to then append a number to a resource name, which allows you to create `myNic1`, `myNic2`, etc. The following shows an example of appending the index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="cf181-156">Átfogó példát olvasható [létrehozása a Resource Manager-sablonok segítségével több hálózati adapter](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="cf181-156">You can read a complete example of [creating multiple NICs using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf181-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf181-157">Next steps</span></span>
<span data-ttu-id="cf181-158">Felülvizsgálati [Linux Virtuálisgép-méretek](sizes.md) több hálózati adapterrel rendelkező virtuális gép létrehozása közben.</span><span class="sxs-lookup"><span data-stu-id="cf181-158">Review [Linux VM sizes](sizes.md) when trying to creating a VM with multiple NICs.</span></span> <span data-ttu-id="cf181-159">Nagy figyelmet fordítani az egyes Virtuálisgép-méretet támogatja a hálózati adapterek maximális száma.</span><span class="sxs-lookup"><span data-stu-id="cf181-159">Pay attention to the maximum number of NICs each VM size supports.</span></span> 