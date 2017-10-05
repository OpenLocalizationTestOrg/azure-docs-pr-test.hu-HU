---
title: "Linux virtuális gép másolása az Azure CLI 2.0 használatával |} Microsoft Docs"
description: "Útmutató egy példányát az Azure Linux virtuális gép létrehozása Azure CLI 2.0 és a felügyelt lemezek használatával."
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 03/10/2017
ms.author: cynthn
ms.openlocfilehash: 7983061a933370803669480296d7625106e1360c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a><span data-ttu-id="cd579-103">Hozzon létre egy Linux virtuális gép egy másolatát Azure CLI 2.0 és felügyelt lemezek</span><span class="sxs-lookup"><span data-stu-id="cd579-103">Create a copy of a Linux VM by using Azure CLI 2.0 and Managed Disks</span></span>


<span data-ttu-id="cd579-104">Ez a cikk bemutatja, hogyan hozzon létre egy másolatot az Azure CLI 2.0 és az Azure Resource Manager telepítési modell segítségével Linux operációs rendszert futtató Azure virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="cd579-104">This article shows you how to create a copy of your Azure virtual machine (VM) running Linux using the Azure CLI 2.0 and the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="cd579-105">Az [Azure CLI 1.0-s](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cd579-105">You can also perform these steps with the [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="cd579-106">Emellett [töltse fel, és hozzon létre egy virtuális gép olyan virtuális merevlemezről](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cd579-106">You can also [upload and create a VM from a VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cd579-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cd579-107">Prerequisites</span></span>


-   <span data-ttu-id="cd579-108">Telepítés [Azure CLI 2.0](/cli/azure/install-az-cli2)</span><span class="sxs-lookup"><span data-stu-id="cd579-108">Install [Azure CLI 2.0](/cli/azure/install-az-cli2)</span></span>

-   <span data-ttu-id="cd579-109">Jelentkezzen be Azure-fiókkal rendelkező [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="cd579-109">Sign in to an Azure account with [az login](/cli/azure/#login).</span></span>

-   <span data-ttu-id="cd579-110">Egy Azure virtuális Gépen, a másolás forrásaként használandó rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="cd579-110">Have an Azure VM to use as the source for your copy.</span></span>

## <a name="step-1-stop-the-source-vm"></a><span data-ttu-id="cd579-111">1. lépés: A forrás virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="cd579-111">Step 1: Stop the source VM</span></span>


<span data-ttu-id="cd579-112">A forrás virtuális gép felszabadítása használatával [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="cd579-112">Deallocate the source VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span>
<span data-ttu-id="cd579-113">Az alábbi példa felszabadítja a nevű virtuális gép **myVM** erőforráscsoportban **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="cd579-113">The following example deallocates the VM named **myVM** in the resource group **myResourceGroup**:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-the-source-vm"></a><span data-ttu-id="cd579-114">2. lépés: A forrás virtuális gép másolása</span><span class="sxs-lookup"><span data-stu-id="cd579-114">Step 2: Copy the source VM</span></span>


<span data-ttu-id="cd579-115">A virtuális gépek másolásához az alapul szolgáló virtuális merevlemez másolatának létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cd579-115">To copy a VM, you create a copy of the underlying virtual hard disk.</span></span> <span data-ttu-id="cd579-116">A folyamat egy speciális virtuális Merevlemezt, amely felügyelt lemezként, mint a forrás virtuális gép konfigurációs és a beállításokat tartalmazó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cd579-116">This process creates a specialized VHD as a Managed Disk that contains the same configuration and settings as the source VM.</span></span>

<span data-ttu-id="cd579-117">További információ az Azure Managed Disksről: [Azure Managed Disks – áttekintés](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cd579-117">For more information about Azure Managed Disks, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span> 

1.  <span data-ttu-id="cd579-118">A lemez minden virtuális gép és az operációs rendszer neve lista [az vm lista](/cli/azure/vm#list).</span><span class="sxs-lookup"><span data-stu-id="cd579-118">List each VM and the name of its OS disk with [az vm list](/cli/azure/vm#list).</span></span> <span data-ttu-id="cd579-119">Az alábbi példa felsorolja az összes virtuális gép nevű erőforráscsoportban **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="cd579-119">The following example lists all VMs in the resource group named **myResourceGroup**:</span></span>
    
    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    <span data-ttu-id="cd579-120">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="cd579-120">The output is similar to the following example:</span></span>

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  <span data-ttu-id="cd579-121">Másolás a lemez hozzon létre egy új lemez használatával kezelhetők [az lemez létrehozása](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="cd579-121">Copy the disk by creating a new managed disk using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="cd579-122">Az alábbi példakód létrehozza nevű lemez **myCopiedDisk** nevű felügyelt lemezről **myDisk**:</span><span class="sxs-lookup"><span data-stu-id="cd579-122">The following example creates a disk named **myCopiedDisk** from the managed disk named **myDisk**:</span></span>

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ``` 

1.  <span data-ttu-id="cd579-123">Ellenőrizheti a felügyelt lemezek most az erőforráscsoportban [az Lemezlista](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="cd579-123">Verify the managed disks now in your resource group by using [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="cd579-124">Az alábbi példa felsorolja a felügyelt lemezek nevű erőforráscsoportban **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="cd579-124">The following example lists the managed disks in the resource group named **myResourceGroup**:</span></span>

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

1.  <span data-ttu-id="cd579-125">Folytassa a ["3. lépés: virtuális hálózat beállítása"](#step-3-set-up-a-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="cd579-125">Skip to ["Step 3: Set up a virtual network"](#step-3-set-up-a-virtual-network).</span></span>


## <a name="step-3-set-up-a-virtual-network"></a><span data-ttu-id="cd579-126">3. lépés: Virtuális hálózat beállítása</span><span class="sxs-lookup"><span data-stu-id="cd579-126">Step 3: Set up a virtual network</span></span>


<span data-ttu-id="cd579-127">A következő lépések hozzon létre egy új virtuális hálózat, az alhálózat, a nyilvános IP-cím és a virtuális hálózati kártya (NIC).</span><span class="sxs-lookup"><span data-stu-id="cd579-127">The following optional steps create a new virtual network, subnet, public IP address, and virtual network interface card (NIC).</span></span>

<span data-ttu-id="cd579-128">A virtuális gép másolása hibaelhárítási célokra, vagy további központi telepítéseket, előfordulhat, hogy nem használni kívánt virtuális gép egy meglévő virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="cd579-128">If you are copying a VM for troubleshooting purposes or additional deployments, you might not want to use a VM in an existing virtual network.</span></span>

<span data-ttu-id="cd579-129">Ha szeretne létrehozni egy virtuális hálózati infrastruktúra a másolt virtuális gépek, hajtsa végre a következő néhány lépést.</span><span class="sxs-lookup"><span data-stu-id="cd579-129">If you want to create a virtual network infrastructure for your copied VMs, follow the next few steps.</span></span> <span data-ttu-id="cd579-130">Ha nem szeretné létrehozni a virtuális hálózatot, folytassa a [4. lépés: virtuális gép létrehozása](#step-4-create-a-vm).</span><span class="sxs-lookup"><span data-stu-id="cd579-130">If you don't want to create a virtual network, skip to [Step 4: Create a VM](#step-4-create-a-vm).</span></span>

1.  <span data-ttu-id="cd579-131">A virtuális hálózat létrehozása a [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="cd579-131">Create the virtual network by using [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="cd579-132">Az alábbi példa létrehoz egy virtuális hálózatot nevű **myVnet** és nevű alhálózat **mySubnet**:</span><span class="sxs-lookup"><span data-stu-id="cd579-132">The following example creates a virtual network named **myVnet** and a subnet named **mySubnet**:</span></span>

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

1.  <span data-ttu-id="cd579-133">Hozzon létre egy nyilvános IP-cím használatával [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="cd579-133">Create a public IP by using [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="cd579-134">Az alábbi példa létrehoz egy nyilvános IP-cím nevű **myPublicIP** a DNS-nevét **mypublicdns**.</span><span class="sxs-lookup"><span data-stu-id="cd579-134">The following example creates a public IP named **myPublicIP** with the DNS name of **mypublicdns**.</span></span> <span data-ttu-id="cd579-135">(A DNS-név kell egyedinek lennie, így adjon meg egy egyedi nevet.)</span><span class="sxs-lookup"><span data-stu-id="cd579-135">(The DNS name must be unique, so provide a unique name.)</span></span>

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

1.  <span data-ttu-id="cd579-136">Hozzon létre a hálózati adapter által használt [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="cd579-136">Create the NIC using [az network nic create](/cli/azure/network/nic#create).</span></span>
    <span data-ttu-id="cd579-137">Az alábbi példa létrehoz egy hálózati adapter nevű **myNic** , amely csatolva van a **mySubnet** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="cd579-137">The following example creates a NIC named **myNic** that's attached to the **mySubnet** subnet:</span></span>

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westus --name myNic \
        --vnet-name myVnet --subnet mySubnet --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a><span data-ttu-id="cd579-138">4. lépés: Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd579-138">Step 4: Create a VM</span></span>

<span data-ttu-id="cd579-139">Mostantól létrehozhat egy virtuális gép használatával [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="cd579-139">You can now create a VM by using [az vm create](/cli/azure/vm#create).</span></span>

<span data-ttu-id="cd579-140">Adja meg a másolt felügyelt lemezt kell használni az operációs rendszer lemezeként (--csatolása operációsrendszer-lemez), az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="cd579-140">Specify the copied managed disk to use as the OS disk (--attach-os-disk), as follows:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a><span data-ttu-id="cd579-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd579-141">Next steps</span></span>

<span data-ttu-id="cd579-142">Azure CLI használata az új virtuális gép kezelése című témakörben talál [Azure parancssori felület parancsait az Azure Resource Manager](../azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="cd579-142">To learn how to use Azure CLI to manage your new VM, see [Azure CLI commands for the Azure Resource Manager](../azure-cli-arm-commands.md).</span></span>
