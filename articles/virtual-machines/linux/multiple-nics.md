---
title: "a több hálózati adapterrel rendelkező Azure Linux virtuális gép aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate több hálózati adapterrel rendelkező Linux virtuális gép csatlakoztatott tooit hello Azure CLI 2.0 vagy az erőforrás-kezelő sablonok használatával."
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
ms.openlocfilehash: 2723405914777a5dce4354d4f5d8413e357f58e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-in-azure-with-multiple-network-interface-cards"></a><span data-ttu-id="c5b91-103">Hogyan toocreate egy Linux virtuális gép az Azure-ban több hálózati kártyák</span><span class="sxs-lookup"><span data-stu-id="c5b91-103">How toocreate a Linux virtual machine in Azure with multiple network interface cards</span></span>
<span data-ttu-id="c5b91-104">Létrehozhat egy virtuális gép (VM) több virtuális hálózati adapterek (NIC) kapcsolódó tooit rendelkező Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c5b91-104">You can create a virtual machine (VM) in Azure that has multiple virtual network interfaces (NICs) attached tooit.</span></span> <span data-ttu-id="c5b91-105">Egy általános forgatókönyv toohave különböző alhálózatokon a előtér- és hálózati kapcsolatot, vagy egy hálózati dedikált tooa figyelési vagy biztonsági mentési megoldás.</span><span class="sxs-lookup"><span data-stu-id="c5b91-105">A common scenario is toohave different subnets for front-end and back-end connectivity, or a network dedicated tooa monitoring or backup solution.</span></span> <span data-ttu-id="c5b91-106">Ez a cikk részletesen hogyan toocreate több hálózati adapterrel rendelkező virtuális gép csatlakoztatott tooit, és hogyan tooadd, vagy távolítsa el a meglévő virtuális hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="c5b91-106">This article details how toocreate a VM with multiple NICs attached tooit and how tooadd or remove NICs from an existing VM.</span></span> <span data-ttu-id="c5b91-107">Részletes információkat, beleértve a hogyan toocreate saját belül több hálózati adapter Bash parancsfájlok, tudjon meg többet az [több hálózati Adapterrel virtuális gépek telepítése](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c5b91-107">For detailed information, including how toocreate multiple NICs within your own Bash scripts, read more about [deploying multi-NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-cli.md).</span></span> <span data-ttu-id="c5b91-108">Különböző [Virtuálisgép-méretek](sizes.md) több hálózati adapter támogatja, így méretezés ennek megfelelően a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="c5b91-108">Different [VM sizes](sizes.md) support a varying number of NICs, so size your VM accordingly.</span></span>

<span data-ttu-id="c5b91-109">Ez a cikk részletesen, hogyan toocreate több hálózati adapterrel rendelkező virtuális gépet a hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="c5b91-109">This article details how toocreate a VM with multiple NICs with hello Azure CLI 2.0.</span></span> <span data-ttu-id="c5b91-110">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](multiple-nics-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="c5b91-110">You can also perform these steps with hello [Azure CLI 1.0](multiple-nics-nodejs.md).</span></span>


## <a name="create-supporting-resources"></a><span data-ttu-id="c5b91-111">Kapcsolódó erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5b91-111">Create supporting resources</span></span>
<span data-ttu-id="c5b91-112">Legutóbbi telepítés hello [Azure CLI 2.0](/cli/azure/install-az-cli2) tooan Azure-fiók használatával jelentkezzen [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="c5b91-112">Install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="c5b91-113">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="c5b91-113">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="c5b91-114">Példa paraméter nevekre *myResourceGroup*, *mystorageaccount*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="c5b91-114">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *myVM*.</span></span>

<span data-ttu-id="c5b91-115">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="c5b91-115">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="c5b91-116">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="c5b91-116">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="c5b91-117">Hozzon létre virtuális hálózat hello [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="c5b91-117">Create hello virtual network with [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="c5b91-118">hello alábbi példa létrehoz egy virtuális hálózatot nevű *myVnet* és nevű alhálózat *mySubnetFrontEnd*:</span><span class="sxs-lookup"><span data-stu-id="c5b91-118">hello following example creates a virtual network named *myVnet* and subnet named *mySubnetFrontEnd*:</span></span>

```azurecli
az network vnet create \
    --resource-group myResourceGroup \
    --name myVnet \
    --address-prefix 192.168.0.0/16 \
    --subnet-name mySubnetFrontEnd \
    --subnet-prefix 192.168.1.0/24
```

<span data-ttu-id="c5b91-119">Hozzon létre egy alhálózatot hello háttér-forgalom [az alhálózaton virtuális hálózat létrehozása](/cli/azure/network/vnet/subnet#create).</span><span class="sxs-lookup"><span data-stu-id="c5b91-119">Create a subnet for hello back-end traffic with [az network vnet subnet create](/cli/azure/network/vnet/subnet#create).</span></span> <span data-ttu-id="c5b91-120">hello alábbi példa létrehoz egy nevű alhálózat *mySubnetBackEnd*:</span><span class="sxs-lookup"><span data-stu-id="c5b91-120">hello following example creates a subnet named *mySubnetBackEnd*:</span></span>

```azurecli
az network vnet subnet create \
    --resource-group myResourceGroup \
    --vnet-name myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

<span data-ttu-id="c5b91-121">Hozzon létre egy hálózati biztonsági csoport [az hálózati nsg létrehozása](/cli/azure/network/nsg#create).</span><span class="sxs-lookup"><span data-stu-id="c5b91-121">Create a network security group with [az network nsg create](/cli/azure/network/nsg#create).</span></span> <span data-ttu-id="c5b91-122">hello alábbi példakód létrehozza a hálózati biztonsági csoport nevű *myNetworkSecurityGroup*:</span><span class="sxs-lookup"><span data-stu-id="c5b91-122">hello following example creates a network security group named *myNetworkSecurityGroup*:</span></span>

```azurecli
az network nsg create \
    --resource-group myResourceGroup \
    --name myNetworkSecurityGroup
```

## <a name="create-and-configure-multiple-nics"></a><span data-ttu-id="c5b91-123">Létrehozhat és konfigurálhat több hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="c5b91-123">Create and configure multiple NICs</span></span>
<span data-ttu-id="c5b91-124">Hozzon létre két hálózati adapterrel [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="c5b91-124">Create two NICs with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="c5b91-125">hello alábbi példa létrehoz két hálózati adapterrel, nevű *myNic1* és *myNic2*, csatlakoztatott hello hálózati biztonsági csoport, egyetlen hálózati adapterrel tooeach csatlakozó:</span><span class="sxs-lookup"><span data-stu-id="c5b91-125">hello following example creates two NICs, named *myNic1* and *myNic2*, connected hello network security group, with one NIC connecting tooeach subnet:</span></span>

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

## <a name="create-a-vm-and-attach-hello-nics"></a><span data-ttu-id="c5b91-126">Hozzon létre egy virtuális Gépet, és hello hálózati adapter csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="c5b91-126">Create a VM and attach hello NICs</span></span>
<span data-ttu-id="c5b91-127">Virtuális gép, hello létrehozásakor adja meg a hello hálózati adapter segítségével létrehozott `--nics`.</span><span class="sxs-lookup"><span data-stu-id="c5b91-127">When you create hello VM, specify hello NICs you created with `--nics`.</span></span> <span data-ttu-id="c5b91-128">Szükség tootake gondot kiválasztásakor hello Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="c5b91-128">You also need tootake care when you select hello VM size.</span></span> <span data-ttu-id="c5b91-129">Nincsenek korlátai hello vehet fel tooa virtuális gép hálózati adapterek száma.</span><span class="sxs-lookup"><span data-stu-id="c5b91-129">There are limits for hello total number of NICs that you can add tooa VM.</span></span> <span data-ttu-id="c5b91-130">Tudjon meg többet az [Linux Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="c5b91-130">Read more about [Linux VM sizes](sizes.md).</span></span> 

<span data-ttu-id="c5b91-131">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="c5b91-131">Create a VM with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="c5b91-132">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM*:</span><span class="sxs-lookup"><span data-stu-id="c5b91-132">hello following example creates a VM named *myVM*:</span></span>

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

## <a name="add-a-nic-tooa-vm"></a><span data-ttu-id="c5b91-133">A virtuális gép hálózati tooa hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c5b91-133">Add a NIC tooa VM</span></span>
<span data-ttu-id="c5b91-134">hello előző lépésekben létrehozott egy virtuális gép több hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="c5b91-134">hello previous steps created a VM with multiple NICs.</span></span> <span data-ttu-id="c5b91-135">Meglévő virtuális gép hello Azure CLI 2.0 rendelkező hálózati adapterek tooan is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="c5b91-135">You can also add NICs tooan existing VM with hello Azure CLI 2.0.</span></span> 

<span data-ttu-id="c5b91-136">Hozzon létre egy másik hálózati Adaptert a [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="c5b91-136">Create another NIC with [az network nic create](/cli/azure/network/nic#create).</span></span> <span data-ttu-id="c5b91-137">hello alábbi példa létrehoz egy hálózati adapter nevű *myNic3* toohello háttér-alhálózat és a hálózati biztonsági csoport hello előző lépésekben létrehozott csatlakoztatva:</span><span class="sxs-lookup"><span data-stu-id="c5b91-137">hello following example creates a NIC named *myNic3* connected toohello back-end subnet and network security group created in hello previous steps:</span></span>

```azurecli
az network nic create \
    --resource-group myResourceGroup \
    --name myNic3 \
    --vnet-name myVnet \
    --subnet mySubnetBackEnd \
    --network-security-group myNetworkSecurityGroup
```

<span data-ttu-id="c5b91-138">egy hálózati adapter tooan tooadd meglévő virtuális gép, először felszabadítani hello tulajdonsággal rendelkező virtuális gépet [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="c5b91-138">tooadd a NIC tooan existing VM, first deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="c5b91-139">hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM*:</span><span class="sxs-lookup"><span data-stu-id="c5b91-139">hello following example deallocates hello VM named *myVM*:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="c5b91-140">Adja hozzá a hálózati adapter hello [az vm hálózati adapter hozzáadása](/cli/azure/vm/nic#add).</span><span class="sxs-lookup"><span data-stu-id="c5b91-140">Add hello NIC with [az vm nic add](/cli/azure/vm/nic#add).</span></span> <span data-ttu-id="c5b91-141">hello következő példakóddal felveheti a *myNic3* túl*myVM*:</span><span class="sxs-lookup"><span data-stu-id="c5b91-141">hello following example adds *myNic3* too*myVM*:</span></span>

```azurecli
az vm nic add \
    --resource-group myResourceGroup \
    --vm-name myVM \
    --nics myNic3
```

<span data-ttu-id="c5b91-142">Indítsa el a virtuális gép hello [az vm indítása](/cli/azure/vm#start):</span><span class="sxs-lookup"><span data-stu-id="c5b91-142">Start hello VM with [az vm start](/cli/azure/vm#start):</span></span>

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```

## <a name="remove-a-nic-from-a-vm"></a><span data-ttu-id="c5b91-143">A virtuális gép egy hálózati adapter eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c5b91-143">Remove a NIC from a VM</span></span>
<span data-ttu-id="c5b91-144">egy meglévő virtuális hálózati Adapterhez tooremove először felszabadítani hello VM rendelkező [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="c5b91-144">tooremove a NIC from an existing VM, first deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate).</span></span> <span data-ttu-id="c5b91-145">hello alábbi példa felszabadítja a hello nevű virtuális gép *myVM*:</span><span class="sxs-lookup"><span data-stu-id="c5b91-145">hello following example deallocates hello VM named *myVM*:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="c5b91-146">Távolítsa el a hálózati adapter hello [az vm hálózati adapter eltávolítása](/cli/azure/vm/nic#remove).</span><span class="sxs-lookup"><span data-stu-id="c5b91-146">Remove hello NIC with [az vm nic remove](/cli/azure/vm/nic#remove).</span></span> <span data-ttu-id="c5b91-147">hello következő példában eltávolítjuk *myNic3* a *myVM*:</span><span class="sxs-lookup"><span data-stu-id="c5b91-147">hello following example removes *myNic3* from *myVM*:</span></span>

```azurecli
az vm nic remove \
    --resource-group myResourceGroup \
    --vm-name myVM 
    --nics myNic3
```

<span data-ttu-id="c5b91-148">Indítsa el a virtuális gép hello [az vm indítása](/cli/azure/vm#start):</span><span class="sxs-lookup"><span data-stu-id="c5b91-148">Start hello VM with [az vm start](/cli/azure/vm#start):</span></span>

```azurecli
az vm start --resource-group myResourceGroup --name myVM
```


## <a name="create-multiple-nics-using-resource-manager-templates"></a><span data-ttu-id="c5b91-149">Resource Manager-sablonok segítségével több hálózati adapter létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5b91-149">Create multiple NICs using Resource Manager templates</span></span>
<span data-ttu-id="c5b91-150">Az Azure Resource Manager-sablonok használata deklaratív JSON-fájlok toodefine a környezetben.</span><span class="sxs-lookup"><span data-stu-id="c5b91-150">Azure Resource Manager templates use declarative JSON files toodefine your environment.</span></span> <span data-ttu-id="c5b91-151">Ha egy [áttekintése Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c5b91-151">You can read an [overview of Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="c5b91-152">Resource Manager-sablonok adjon meg egy módon toocreate erőforrás több példánya központi telepítést végez, például több hálózati adapter létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="c5b91-152">Resource Manager templates provide a way toocreate multiple instances of a resource during deployment, such as creating multiple NICs.</span></span> <span data-ttu-id="c5b91-153">Használhat *másolási* példányok toocreate toospecify hello száma:</span><span class="sxs-lookup"><span data-stu-id="c5b91-153">You use *copy* toospecify hello number of instances toocreate:</span></span>

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

<span data-ttu-id="c5b91-154">Tudjon meg többet az [használatával több példány létrehozásával *másolási*](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="c5b91-154">Read more about [creating multiple instances using *copy*](../../resource-group-create-multiple.md).</span></span> 

<span data-ttu-id="c5b91-155">Használhatja a `copyIndex()` toothen hozzáfűzése számú tooa erőforrás nevét, amely lehetővé teszi toocreate `myNic1`, `myNic2`, stb. hello következő hello indexértéket fűznek példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="c5b91-155">You can also use a `copyIndex()` toothen append a number tooa resource name, which allows you toocreate `myNic1`, `myNic2`, etc. hello following shows an example of appending hello index value:</span></span>

```json
"name": "[concat('myNic', copyIndex())]", 
```

<span data-ttu-id="c5b91-156">Átfogó példát olvasható [létrehozása a Resource Manager-sablonok segítségével több hálózati adapter](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="c5b91-156">You can read a complete example of [creating multiple NICs using Resource Manager templates](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5b91-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5b91-157">Next steps</span></span>
<span data-ttu-id="c5b91-158">Felülvizsgálati [Linux Virtuálisgép-méretek](sizes.md) toocreating több hálózati adapterrel rendelkező virtuális gép tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="c5b91-158">Review [Linux VM sizes](sizes.md) when trying toocreating a VM with multiple NICs.</span></span> <span data-ttu-id="c5b91-159">Nagy figyelmet toohello több hálózati adapter támogatja az egyes Virtuálisgép-méretet.</span><span class="sxs-lookup"><span data-stu-id="c5b91-159">Pay attention toohello maximum number of NICs each VM size supports.</span></span> 
