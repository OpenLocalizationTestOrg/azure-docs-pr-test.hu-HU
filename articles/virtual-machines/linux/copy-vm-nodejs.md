---
title: "a Linux virtuális Gépet az Azure CLI 1.0 hello másolatát aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy-egy példányát az Azure Linux virtuális gép hello hello Resource Manager üzembe helyezési modellel Azure CLI 1.0"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 997a2c8109e7083ececd76fd1013e9ed4d3e6afd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-hello-azure-cli-10"></a><span data-ttu-id="3676b-103">Az Azure CLI 1.0 hello Azure-on futó Linux virtuális gépek másolatának létrehozása</span><span class="sxs-lookup"><span data-stu-id="3676b-103">Create a copy of a Linux virtual machine running on Azure with hello Azure CLI 1.0</span></span>
<span data-ttu-id="3676b-104">Ez a cikk bemutatja, hogyan toocreate egy példányát az Azure virtuális gép (VM) futó Linux használt hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="3676b-104">This article shows you how toocreate a copy of your Azure virtual machine (VM) running Linux using hello Resource Manager deployment model.</span></span> <span data-ttu-id="3676b-105">Először másolja át a hello operációs rendszer és az adatok lemezek tooa új tárolót, majd beállítása hello hálózati erőforrások és hello új virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3676b-105">First you copy over hello operating system and data disks tooa new container, then set up hello network resources and create hello new virtual machine.</span></span>

<span data-ttu-id="3676b-106">Emellett [töltse fel, és hozzon létre egy virtuális Gépet az egyéni lemezképet](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3676b-106">You can also [upload and create a VM from custom disk image](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="3676b-107">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="3676b-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="3676b-108">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="3676b-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="3676b-109">Az Azure CLI 1.0 – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="3676b-109">Azure CLI 1.0 – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="3676b-110">[Az Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="3676b-110">[Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="3676b-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="3676b-111">Before you begin</span></span>
<span data-ttu-id="3676b-112">Győződjön meg arról, hogy teljesülnek-e hello hello lépések megkezdése előtt a következő előfeltételek:</span><span class="sxs-lookup"><span data-stu-id="3676b-112">Ensure that you meet hello following prerequisites before you start hello steps:</span></span>

* <span data-ttu-id="3676b-113">Hello rendelkezik [Azure CLI](../../cli-install-nodejs.md) letöltötte és telepítette a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3676b-113">You have hello [Azure CLI](../../cli-install-nodejs.md) downloaded and installed on your machine.</span></span> 
* <span data-ttu-id="3676b-114">A meglévő Azure Linux virtuális Gépre vonatkozó kell néhány információt is:</span><span class="sxs-lookup"><span data-stu-id="3676b-114">You also need some information about your existing Azure Linux VM:</span></span>

| <span data-ttu-id="3676b-115">Forrás Virtuálisgép-adatok</span><span class="sxs-lookup"><span data-stu-id="3676b-115">Source VM information</span></span> | <span data-ttu-id="3676b-116">Ha tooget azt</span><span class="sxs-lookup"><span data-stu-id="3676b-116">Where tooget it</span></span> |
| --- | --- |
| <span data-ttu-id="3676b-117">a virtuális gép neve</span><span class="sxs-lookup"><span data-stu-id="3676b-117">VM name</span></span> |`azure vm list` |
| <span data-ttu-id="3676b-118">Erőforráscsoport neve</span><span class="sxs-lookup"><span data-stu-id="3676b-118">Resource Group name</span></span> |`azure vm list` |
| <span data-ttu-id="3676b-119">Hely</span><span class="sxs-lookup"><span data-stu-id="3676b-119">Location</span></span> |`azure vm list` |
| <span data-ttu-id="3676b-120">A tárfiók neve</span><span class="sxs-lookup"><span data-stu-id="3676b-120">Storage Account name</span></span> |`azure storage account list -g <resourceGroup>` |
| <span data-ttu-id="3676b-121">Tárolónév</span><span class="sxs-lookup"><span data-stu-id="3676b-121">Container name</span></span> |`azure storage container list -a <sourcestorageaccountname>` |
| <span data-ttu-id="3676b-122">Forrásfájl virtuális gép virtuális merevlemez neve</span><span class="sxs-lookup"><span data-stu-id="3676b-122">Source VM VHD file name</span></span> |`azure storage blob list --container <containerName>` |

* <span data-ttu-id="3676b-123">Az új virtuális gép kapcsolatos toomake néhány lesz szüksége:   </span><span class="sxs-lookup"><span data-stu-id="3676b-123">You will need toomake some choices about your new VM:    </span></span><br> <span data-ttu-id="3676b-124">-Tároló neve   </span><span class="sxs-lookup"><span data-stu-id="3676b-124">-Container name    </span></span><br> <span data-ttu-id="3676b-125">Virtuálisgép - nevet   </span><span class="sxs-lookup"><span data-stu-id="3676b-125">-VM name    </span></span><br> <span data-ttu-id="3676b-126">Virtuálisgép - méret   </span><span class="sxs-lookup"><span data-stu-id="3676b-126">-VM size    </span></span><br> <span data-ttu-id="3676b-127">-vNet neve   </span><span class="sxs-lookup"><span data-stu-id="3676b-127">-vNet name    </span></span><br> <span data-ttu-id="3676b-128">-Alhálózat neve   </span><span class="sxs-lookup"><span data-stu-id="3676b-128">-SubNet name    </span></span><br> <span data-ttu-id="3676b-129">-IP név   </span><span class="sxs-lookup"><span data-stu-id="3676b-129">-IP Name    </span></span><br> <span data-ttu-id="3676b-130">-Hálózati név</span><span class="sxs-lookup"><span data-stu-id="3676b-130">-NIC name</span></span>

## <a name="login-and-set-your-subscription"></a><span data-ttu-id="3676b-131">Bejelentkezés és az előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="3676b-131">Login and set your subscription</span></span>
1. <span data-ttu-id="3676b-132">Bejelentkezési toohello CLI-t.</span><span class="sxs-lookup"><span data-stu-id="3676b-132">Login toohello CLI.</span></span>

    ```azurecli
    azure login
    ```
2. <span data-ttu-id="3676b-133">Győződjön meg arról, hogy az erőforrás-kezelő módban van.</span><span class="sxs-lookup"><span data-stu-id="3676b-133">Make sure you are in Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```
3. <span data-ttu-id="3676b-134">Állítsa be a megfelelő előfizetés hello.</span><span class="sxs-lookup"><span data-stu-id="3676b-134">Set hello correct subscription.</span></span> <span data-ttu-id="3676b-135">Használhatja az "azure-fiók list" toosee összes előfizetését.</span><span class="sxs-lookup"><span data-stu-id="3676b-135">You can use 'azure account list' toosee all of your subscriptions.</span></span>

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-hello-vm"></a><span data-ttu-id="3676b-136">Hello VM leállítása</span><span class="sxs-lookup"><span data-stu-id="3676b-136">Stop hello VM</span></span>
<span data-ttu-id="3676b-137">Állítsa le és hello forrás virtuális gép felszabadítása.</span><span class="sxs-lookup"><span data-stu-id="3676b-137">Stop and deallocate hello source VM.</span></span> <span data-ttu-id="3676b-138">Az előfizetés és az erőforráscsoport-nevek a "list azure virtuális gép" tooget összes hello virtuális gépek listáját is használhatja.</span><span class="sxs-lookup"><span data-stu-id="3676b-138">You can use 'azure vm list' tooget a list of all of hello VMs in your subscription and their resource group names.</span></span>

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-hello-vhd"></a><span data-ttu-id="3676b-139">Hello virtuális merevlemez másolása</span><span class="sxs-lookup"><span data-stu-id="3676b-139">Copy hello VHD</span></span>
<span data-ttu-id="3676b-140">Virtuális merevlemez hello átmásolhatja hello forrás toohello célhelyet hello használata `azure storage blob copy start`.</span><span class="sxs-lookup"><span data-stu-id="3676b-140">You can copy hello VHD from hello source storage toohello destination using hello `azure storage blob copy start`.</span></span> <span data-ttu-id="3676b-141">Ebben a példában a toocopy hello VHD toohello fogjuk ugyanazt a tárfiókot, de egy másik tárolóban.</span><span class="sxs-lookup"><span data-stu-id="3676b-141">In this example, we are going toocopy hello VHD toohello same storage account, but a different container.</span></span>

<span data-ttu-id="3676b-142">toocopy hello VHD tooanother tárolóhoz hello ugyanazt a tárfiókot, írja be:</span><span class="sxs-lookup"><span data-stu-id="3676b-142">toocopy hello VHD tooanother container in hello same storage account, type:</span></span>

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-hello-virtual-network-for-your-new-vm"></a><span data-ttu-id="3676b-143">Az új virtuális gép virtuális hálózati hello beállítása</span><span class="sxs-lookup"><span data-stu-id="3676b-143">Set up hello virtual network for your new VM</span></span>
<span data-ttu-id="3676b-144">A virtuális hálózat és a hálózati adapter beállítása az új virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="3676b-144">Set up a virtual network and NIC for your new VM.</span></span> 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-hello-new-vm"></a><span data-ttu-id="3676b-145">Hozzon létre új virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="3676b-145">Create hello new VM</span></span>
<span data-ttu-id="3676b-146">Mostantól létrehozhat egy virtuális Gépet a feltöltött virtuális lemez [resource manager-sablon használatával](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) vagy keresztül hello hello URI tooyour megadásával CLI lemezre másolt beírásával:</span><span class="sxs-lookup"><span data-stu-id="3676b-146">You can now create a VM from your uploaded virtual disk [using a resource manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) or through hello CLI by specifying hello URI tooyour copied disk by typing:</span></span>

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a><span data-ttu-id="3676b-147">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3676b-147">Next steps</span></span>
<span data-ttu-id="3676b-148">toolearn hogyan toouse Azure CLI toomanage a új virtuális gépet, lásd: [hello Azure Resource Manager az Azure parancssori felület parancsait](../azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="3676b-148">toolearn how toouse Azure CLI toomanage your new virtual machine, see [Azure CLI commands for hello Azure Resource Manager](../azure-cli-arm-commands.md).</span></span>

