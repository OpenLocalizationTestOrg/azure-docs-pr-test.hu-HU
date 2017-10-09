---
title: "a Linux virtuális gépek Azure CLI 2.0 használatával aaaCopy |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate egy példányát az Azure Linux virtuális gép Azure CLI 2.0 és a felügyelt lemezek használatával."
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
ms.openlocfilehash: ee34a4259dd0c1e7bf49312fe3fe3ba809bf69e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-vm-by-using-azure-cli-20-and-managed-disks"></a><span data-ttu-id="d12cd-103">Hozzon létre egy Linux virtuális gép egy másolatát Azure CLI 2.0 és felügyelt lemezek</span><span class="sxs-lookup"><span data-stu-id="d12cd-103">Create a copy of a Linux VM by using Azure CLI 2.0 and Managed Disks</span></span>


<span data-ttu-id="d12cd-104">Ez a cikk bemutatja, hogyan toocreate egy példányát az Azure virtuális gép (VM) Linux használó futtatása hello Azure CLI 2.0 és hello Azure Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="d12cd-104">This article shows you how toocreate a copy of your Azure virtual machine (VM) running Linux using hello Azure CLI 2.0 and hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="d12cd-105">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d12cd-105">You can also perform these steps with hello [Azure CLI 1.0](copy-vm-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="d12cd-106">Emellett [töltse fel, és hozzon létre egy virtuális gép olyan virtuális merevlemezről](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d12cd-106">You can also [upload and create a VM from a VHD](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d12cd-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d12cd-107">Prerequisites</span></span>


-   <span data-ttu-id="d12cd-108">Telepítés [Azure CLI 2.0](/cli/azure/install-az-cli2)</span><span class="sxs-lookup"><span data-stu-id="d12cd-108">Install [Azure CLI 2.0](/cli/azure/install-az-cli2)</span></span>

-   <span data-ttu-id="d12cd-109">Az Azure-fiók tooan bejelentkezés [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="d12cd-109">Sign in tooan Azure account with [az login](/cli/azure/#login).</span></span>

-   <span data-ttu-id="d12cd-110">Rendelkezik egy Azure virtuális gép toouse hello a másolási forrásaként.</span><span class="sxs-lookup"><span data-stu-id="d12cd-110">Have an Azure VM toouse as hello source for your copy.</span></span>

## <a name="step-1-stop-hello-source-vm"></a><span data-ttu-id="d12cd-111">1. lépés: Hello forrás virtuális gép leállítása</span><span class="sxs-lookup"><span data-stu-id="d12cd-111">Step 1: Stop hello source VM</span></span>


<span data-ttu-id="d12cd-112">Hello forrás virtuális gép felszabadítása használatával [az virtuális gép felszabadítása](/cli/azure/vm#deallocate).</span><span class="sxs-lookup"><span data-stu-id="d12cd-112">Deallocate hello source VM by using [az vm deallocate](/cli/azure/vm#deallocate).</span></span>
<span data-ttu-id="d12cd-113">hello alábbi példa felszabadítja a hello nevű virtuális gép **myVM** hello erőforráscsoportban **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="d12cd-113">hello following example deallocates hello VM named **myVM** in hello resource group **myResourceGroup**:</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

## <a name="step-2-copy-hello-source-vm"></a><span data-ttu-id="d12cd-114">2. lépés: Hello forrás virtuális gép másolása</span><span class="sxs-lookup"><span data-stu-id="d12cd-114">Step 2: Copy hello source VM</span></span>


<span data-ttu-id="d12cd-115">toocopy egy virtuális Gépet, akkor hello alapul szolgáló virtuális merevlemez másolatának létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d12cd-115">toocopy a VM, you create a copy of hello underlying virtual hard disk.</span></span> <span data-ttu-id="d12cd-116">A folyamat hello tartalmazó felügyelt lemezként speciális virtuális Merevlemezt hoz létre a forrás virtuális gép azonos konfigurációs és hello beállításokat.</span><span class="sxs-lookup"><span data-stu-id="d12cd-116">This process creates a specialized VHD as a Managed Disk that contains hello same configuration and settings as hello source VM.</span></span>

<span data-ttu-id="d12cd-117">További információ az Azure Managed Disksről: [Azure Managed Disks – áttekintés](../windows/managed-disks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d12cd-117">For more information about Azure Managed Disks, see [Azure Managed Disks overview](../windows/managed-disks-overview.md).</span></span> 

1.  <span data-ttu-id="d12cd-118">Minden virtuális gép és hello nevét az operációsrendszer-lemez, a listában [az vm lista](/cli/azure/vm#list).</span><span class="sxs-lookup"><span data-stu-id="d12cd-118">List each VM and hello name of its OS disk with [az vm list](/cli/azure/vm#list).</span></span> <span data-ttu-id="d12cd-119">hello alábbi példa felsorolja az erőforráscsoport nevű virtuális gépeinek **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="d12cd-119">hello following example lists all VMs in the resource group named **myResourceGroup**:</span></span>
    
    ```azurecli
    az vm list -g myTestRG --query '[].{Name:name,DiskName:storageProfile.osDisk.name}' --output table
    ```

    <span data-ttu-id="d12cd-120">hello hasonló toohello a következő példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="d12cd-120">hello output is similar toohello following example:</span></span>

    ```azurecli
    Name    DiskName
    ------  --------
    myVM    myDisk
    ```

1.  <span data-ttu-id="d12cd-121">Hozzon létre egy új hello lemezmásolás lemez használatával kezelhetők [az lemez létrehozása](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="d12cd-121">Copy hello disk by creating a new managed disk using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="d12cd-122">hello alábbi példa létrehoz egy nevű lemez **myCopiedDisk** hello a felügyelt nevű lemez **myDisk**:</span><span class="sxs-lookup"><span data-stu-id="d12cd-122">hello following example creates a disk named **myCopiedDisk** from hello managed disk named **myDisk**:</span></span>

    ```azurecli
    az disk create --resource-group myResourceGroup --name myCopiedDisk --source myDisk
    ``` 

1.  <span data-ttu-id="d12cd-123">Felügyelt hello lemezek most az erőforráscsoportban ellenőrizheti [az Lemezlista](/cli/azure/disk#list).</span><span class="sxs-lookup"><span data-stu-id="d12cd-123">Verify hello managed disks now in your resource group by using [az disk list](/cli/azure/disk#list).</span></span> <span data-ttu-id="d12cd-124">hello alábbi példa felsorolja felügyelt hello lemezek nevű hello erőforráscsoportban **myResourceGroup**:</span><span class="sxs-lookup"><span data-stu-id="d12cd-124">hello following example lists hello managed disks in hello resource group named **myResourceGroup**:</span></span>

    ```azurecli
    az disk list --resource-group myResourceGroup --output table
    ```

1.  <span data-ttu-id="d12cd-125">Hagyja ki túl["3. lépés: virtuális hálózat beállítása"](#step-3-set-up-a-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="d12cd-125">Skip too["Step 3: Set up a virtual network"](#step-3-set-up-a-virtual-network).</span></span>


## <a name="step-3-set-up-a-virtual-network"></a><span data-ttu-id="d12cd-126">3. lépés: Virtuális hálózat beállítása</span><span class="sxs-lookup"><span data-stu-id="d12cd-126">Step 3: Set up a virtual network</span></span>


<span data-ttu-id="d12cd-127">hello következő lépések hozzon létre egy új virtuális hálózat, az alhálózat, a nyilvános IP-cím és a virtuális hálózati kártya (NIC).</span><span class="sxs-lookup"><span data-stu-id="d12cd-127">hello following optional steps create a new virtual network, subnet, public IP address, and virtual network interface card (NIC).</span></span>

<span data-ttu-id="d12cd-128">A virtuális gép másolása hibaelhárítási célokra, vagy további központi telepítéseket, nem biztos toouse egy virtuális gép egy meglévő virtuális hálózatban.</span><span class="sxs-lookup"><span data-stu-id="d12cd-128">If you are copying a VM for troubleshooting purposes or additional deployments, you might not want toouse a VM in an existing virtual network.</span></span>

<span data-ttu-id="d12cd-129">Ha a másolt virtuális gépek toocreate egy virtuális hálózati infrastruktúra, kövesse tovább hello néhány lépést.</span><span class="sxs-lookup"><span data-stu-id="d12cd-129">If you want toocreate a virtual network infrastructure for your copied VMs, follow hello next few steps.</span></span> <span data-ttu-id="d12cd-130">Ha nem szeretné, toocreate egy virtuális hálózathoz, hagyja ki túl[4. lépés: virtuális gép létrehozása](#step-4-create-a-vm).</span><span class="sxs-lookup"><span data-stu-id="d12cd-130">If you don't want toocreate a virtual network, skip too[Step 4: Create a VM](#step-4-create-a-vm).</span></span>

1.  <span data-ttu-id="d12cd-131">Hello virtuális hálózat létrehozása a [az hálózati vnet létrehozása](/cli/azure/network/vnet#create).</span><span class="sxs-lookup"><span data-stu-id="d12cd-131">Create hello virtual network by using [az network vnet create](/cli/azure/network/vnet#create).</span></span> <span data-ttu-id="d12cd-132">hello alábbi példa létrehoz egy virtuális hálózatot nevű **myVnet** és nevű alhálózat **mySubnet**:</span><span class="sxs-lookup"><span data-stu-id="d12cd-132">hello following example creates a virtual network named **myVnet** and a subnet named **mySubnet**:</span></span>

    ```azurecli
    az network vnet create --resource-group myResourceGroup --location westus --name myVnet \
        --address-prefix 192.168.0.0/16 --subnet-name mySubnet --subnet-prefix 192.168.1.0/24
    ```

1.  <span data-ttu-id="d12cd-133">Hozzon létre egy nyilvános IP-cím használatával [létrehozása az hálózati nyilvános ip-](/cli/azure/network/public-ip#create).</span><span class="sxs-lookup"><span data-stu-id="d12cd-133">Create a public IP by using [az network public-ip create](/cli/azure/network/public-ip#create).</span></span> <span data-ttu-id="d12cd-134">hello alábbi példa létrehoz egy nyilvános IP-cím nevű **myPublicIP** hello DNS-névvel, **mypublicdns**.</span><span class="sxs-lookup"><span data-stu-id="d12cd-134">hello following example creates a public IP named **myPublicIP** with hello DNS name of **mypublicdns**.</span></span> <span data-ttu-id="d12cd-135">(hello DNS-név kell egyedinek lennie, így adjon meg egy egyedi nevet.)</span><span class="sxs-lookup"><span data-stu-id="d12cd-135">(hello DNS name must be unique, so provide a unique name.)</span></span>

    ```azurecli
    az network public-ip create --resource-group myResourceGroup --location westus \
        --name myPublicIP --dns-name mypublicdns --allocation-method static --idle-timeout 4
    ```

1.  <span data-ttu-id="d12cd-136">Hozzon létre hello hálózati adapter által használt [az hálózat összevont hálózati létrehozása](/cli/azure/network/nic#create).</span><span class="sxs-lookup"><span data-stu-id="d12cd-136">Create hello NIC using [az network nic create](/cli/azure/network/nic#create).</span></span>
    <span data-ttu-id="d12cd-137">hello alábbi példa létrehoz egy hálózati adapter nevű **myNic** meg csatolt toothe **mySubnet** alhálózati:</span><span class="sxs-lookup"><span data-stu-id="d12cd-137">hello following example creates a NIC named **myNic** that's attached toothe **mySubnet** subnet:</span></span>

    ```azurecli
    az network nic create --resource-group myResourceGroup --location westus --name myNic \
        --vnet-name myVnet --subnet mySubnet --public-ip-address myPublicIP
    ```

## <a name="step-4-create-a-vm"></a><span data-ttu-id="d12cd-138">4. lépés: Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d12cd-138">Step 4: Create a VM</span></span>

<span data-ttu-id="d12cd-139">Mostantól létrehozhat egy virtuális gép használatával [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="d12cd-139">You can now create a VM by using [az vm create](/cli/azure/vm#create).</span></span>

<span data-ttu-id="d12cd-140">Adja meg a hello másolt felügyelt lemezes toouse hello az operációs rendszer lemezeként (--csatolása operációsrendszer-lemez), az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d12cd-140">Specify hello copied managed disk toouse as hello OS disk (--attach-os-disk), as follows:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --name myCopiedVM \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --nics myNic --size Standard_DS1_v2 --os-type Linux \
    --attach-os-disk myCopiedDisk
```

## <a name="next-steps"></a><span data-ttu-id="d12cd-141">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d12cd-141">Next steps</span></span>

<span data-ttu-id="d12cd-142">toolearn hogyan toouse Azure CLI toomanage az új virtuális gép, lásd: [hello Azure Resource Manager az Azure parancssori felület parancsait](../azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="d12cd-142">toolearn how toouse Azure CLI toomanage your new VM, see [Azure CLI commands for hello Azure Resource Manager](../azure-cli-arm-commands.md).</span></span>
