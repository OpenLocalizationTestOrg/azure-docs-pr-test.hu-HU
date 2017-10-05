---
title: "Töltse fel az Azure CLI 2.0 egyéni Linux lemezzel |} Microsoft Docs"
description: "Hozzon létre, és töltse fel a virtuális merevlemez (VHD) a Resource Manager üzembe helyezési modellel és az Azure CLI 2.0 használatával"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: 9159960af396e89f373da711e0cc46fdd996ab83
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a><span data-ttu-id="eac63-103">Töltse fel, és a Linux virtuális gép létrehozása az Azure CLI 2.0 egyéni lemezről</span><span class="sxs-lookup"><span data-stu-id="eac63-103">Upload and create a Linux VM from custom disk with the Azure CLI 2.0</span></span>
<span data-ttu-id="eac63-104">Ez a cikk bemutatja, hogyan egy virtuális merevlemez (VHD) feltöltése az Azure CLI 2.0 Azure storage-fiók és a Linux virtuális gépek létrehozása a egyéni lemezt.</span><span class="sxs-lookup"><span data-stu-id="eac63-104">This article shows you how to upload a virtual hard disk (VHD) to an Azure storage account with the Azure CLI 2.0 and create Linux VMs from this custom disk.</span></span> <span data-ttu-id="eac63-105">Az [Azure CLI 1.0-s](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="eac63-105">You can also perform these steps with the [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="eac63-106">Ez a funkció lehetővé teszi telepítése és konfigurálása a Linux distro az igényeinek megfelelően, valamint, hogy a virtuális merevlemez használatával gyorsan hozzon létre az Azure virtuális gépek (VM).</span><span class="sxs-lookup"><span data-stu-id="eac63-106">This functionality allows you to install and configure a Linux distro to your requirements and then use that VHD to quickly create Azure virtual machines (VMs).</span></span>

<span data-ttu-id="eac63-107">Ez a témakör a végső virtuális merevlemezeket használ a storage-fiókok, de ezeket a lépéseket használatával is végrehajthatja [által kezelt lemezeken](upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="eac63-107">This topic uses storage accounts for the final VHDs, but you can also do these steps using [managed disks](upload-vhd.md).</span></span> 

## <a name="quick-commands"></a><span data-ttu-id="eac63-108">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="eac63-108">Quick commands</span></span>
<span data-ttu-id="eac63-109">Ha szeretné gyorsan a feladatnak a, a következő szakasz részleteit a következő parancsokat a virtuális merevlemez feltöltéséhez az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="eac63-109">If you need to quickly accomplish the task, the following section details the base commands to upload a VHD to Azure.</span></span> <span data-ttu-id="eac63-110">Részletes információkat és a környezetben az egyes lépések a dokumentum többi részén található [itt indítása](#requirements).</span><span class="sxs-lookup"><span data-stu-id="eac63-110">More detailed information and context for each step can be found the rest of the document, [starting here](#requirements).</span></span>

<span data-ttu-id="eac63-111">Győződjön meg arról, hogy rendelkezik-e a legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett az Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="eac63-111">Make sure that you have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="eac63-112">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="eac63-112">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="eac63-113">Példa paraméter nevekre `myResourceGroup`, `mystorageaccount`, és `mydisks`.</span><span class="sxs-lookup"><span data-stu-id="eac63-113">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `mydisks`.</span></span>

<span data-ttu-id="eac63-114">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="eac63-114">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="eac63-115">Az alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a a `WestUs` helye:</span><span class="sxs-lookup"><span data-stu-id="eac63-115">The following example creates a resource group named `myResourceGroup` in the `WestUs` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="eac63-116">Hozzon létre egy tárfiókot, ahhoz, hogy a virtuális lemezek, amelyek [az storage-fiók létrehozása](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="eac63-116">Create a storage account to hold your virtual disks with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="eac63-117">Az alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="eac63-117">The following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

<span data-ttu-id="eac63-118">A tárfiók hozzáférési kulcsainak listázása [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="eac63-118">List the access keys for your storage account with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="eac63-119">Jegyezze fel a `key1`:</span><span class="sxs-lookup"><span data-stu-id="eac63-119">Make a note of `key1`:</span></span>

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

<span data-ttu-id="eac63-120">Hozzon létre egy tárolót a tárfiókon belül a biztonságitár-kulcs használatával kapott [az tároló létrehozása](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="eac63-120">Create a container within your storage account using the storage key you obtained with [az storage container create](/cli/azure/storage/container#create).</span></span> <span data-ttu-id="eac63-121">Az alábbi példa létrehoz egy nevű tárolót `mydisks` használ a tárolási értékének `key1`:</span><span class="sxs-lookup"><span data-stu-id="eac63-121">The following example creates a container named `mydisks` using the storage key value from `key1`:</span></span>

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

<span data-ttu-id="eac63-122">Végezetül a VHD-fájlt feltölti a létrehozott tároló [az tárolási blob feltöltése](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="eac63-122">Finally, upload your VHD to the container you created with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="eac63-123">Adja meg a helyi elérési útját a virtuális merevlemez alapján `/path/to/disk/mydisk.vhd`:</span><span class="sxs-lookup"><span data-stu-id="eac63-123">Specify the local path to your VHD under `/path/to/disk/mydisk.vhd`:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

<span data-ttu-id="eac63-124">Adja meg az URI-t a lemez (`--image`) rendelkező [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="eac63-124">Specify the URI to your disk (`--image`) with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="eac63-125">Az alábbi példakód létrehozza a virtuális gépek nevű `myVM` korábban feltöltött a virtuális lemez segítségével:</span><span class="sxs-lookup"><span data-stu-id="eac63-125">The following example creates a VM named `myVM` using the virtual disk previously uploaded:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

<span data-ttu-id="eac63-126">A cél tárfiókkal nem lehet ugyanaz, mint ahol a virtuális lemez feltöltött.</span><span class="sxs-lookup"><span data-stu-id="eac63-126">The destination storage account has to be the same as where you uploaded your virtual disk to.</span></span> <span data-ttu-id="eac63-127">Is meg kell adni, vagy válasz kér, által igényelt minden további paramétereket a **az virtuális gép létrehozása** parancs például a virtuális hálózat, a nyilvános IP-cím, a felhasználónevet és az SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="eac63-127">You also need to specify, or answer prompts for, all the additional parameters required by the **az vm create** command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="eac63-128">További információ a [érhető el erőforrás-kezelő parancssori paraméterek](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="eac63-128">You can read more about the [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

## <a name="requirements"></a><span data-ttu-id="eac63-129">Követelmények</span><span class="sxs-lookup"><span data-stu-id="eac63-129">Requirements</span></span>
<span data-ttu-id="eac63-130">A következő lépések elvégzéséhez szüksége:</span><span class="sxs-lookup"><span data-stu-id="eac63-130">To complete the following steps, you need:</span></span>

* <span data-ttu-id="eac63-131">**Linux operációs rendszer van telepítve, a .vhd-fájllá** -telepíteni egy [Azure által támogatott Linux-disztribúció](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (vagy lásd: [nem támogatott disztribúciókkal kapcsolatos információi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) a virtuális merevlemez virtuális lemezre formátumban.</span><span class="sxs-lookup"><span data-stu-id="eac63-131">**Linux operating system installed in a .vhd file** - Install an [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) to a virtual disk in the VHD format.</span></span> <span data-ttu-id="eac63-132">Több különféle eszköz létezik a virtuális gép és a virtuális merevlemez létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="eac63-132">Multiple tools exist to create a VM and VHD:</span></span>
  * <span data-ttu-id="eac63-133">Telepítse és konfigurálja [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve arra, hogy a virtuális merevlemez használata a képformátum.</span><span class="sxs-lookup"><span data-stu-id="eac63-133">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care to use VHD as your image format.</span></span> <span data-ttu-id="eac63-134">Ha szükséges, akkor [lemezkép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) használatával `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="eac63-134">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="eac63-135">Is használhatja a Hyper-V [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) vagy [Windows Server 2012 vagy 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="eac63-135">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="eac63-136">Az újabb VHDX formátum nem támogatott az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="eac63-136">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="eac63-137">Amikor létrehoz egy virtuális Gépet, adja meg a VHD formátumban.</span><span class="sxs-lookup"><span data-stu-id="eac63-137">When you create a VM, specify VHD as the format.</span></span> <span data-ttu-id="eac63-138">Szükség esetén, VHDX-lemezek konvertálása virtuális merevlemez használatával [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy a [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="eac63-138">If needed, you can convert VHDX disks to VHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or the [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="eac63-139">További Azure nem támogatja dinamikus virtuális merevlemezek, feltöltése, ezért ilyen lemezek konvertálása statikus virtuális merevlemezek feltöltés előtt meg kell.</span><span class="sxs-lookup"><span data-stu-id="eac63-139">Further, Azure does not support uploading dynamic VHDs, so you need to convert such disks to static VHDs before uploading.</span></span> <span data-ttu-id="eac63-140">Használhatja például a [NYISSA meg az Azure virtuális merevlemez segédprogramok](https://github.com/Microsoft/azure-vhd-utils-for-go) átalakítani a dinamikus lemezek Azure feltöltése során.</span><span class="sxs-lookup"><span data-stu-id="eac63-140">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) to convert dynamic disks during the process of uploading to Azure.</span></span>
> 
> 

* <span data-ttu-id="eac63-141">Az egyéni lemez alapján létrehozott virtuális gépek ugyanazt a tárfiókot, magán a lemezen kell lennie</span><span class="sxs-lookup"><span data-stu-id="eac63-141">VMs created from your custom disk must reside in the same storage account as the disk itself</span></span>
  * <span data-ttu-id="eac63-142">Hozzon létre egy tárfiók és tároló az egyéni lemez és a létrehozott virtuális gépek tárolására</span><span class="sxs-lookup"><span data-stu-id="eac63-142">Create a storage account and container to hold both your custom disk and created VMs</span></span>
  * <span data-ttu-id="eac63-143">Miután létrehozta a virtuális gépek, nyugodtan törölheti a lemezen</span><span class="sxs-lookup"><span data-stu-id="eac63-143">After you have created all your VMs, you can safely delete your disk</span></span>

<span data-ttu-id="eac63-144">Győződjön meg arról, hogy rendelkezik-e a legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett az Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="eac63-144">Make sure that you have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="eac63-145">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="eac63-145">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="eac63-146">Példa paraméter nevekre `myResourceGroup`, `mystorageaccount`, és `mydisks`.</span><span class="sxs-lookup"><span data-stu-id="eac63-146">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `mydisks`.</span></span>

<span data-ttu-id="eac63-147"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="eac63-147"><a id="prepimage"> </a></span></span>

## <a name="prepare-the-disk-to-be-uploaded"></a><span data-ttu-id="eac63-148">Készítse elő a feltölteni kívánt lemez</span><span class="sxs-lookup"><span data-stu-id="eac63-148">Prepare the disk to be uploaded</span></span>
<span data-ttu-id="eac63-149">Azure támogatja a különböző Linux terjesztésekről (lásd: [támogatott Disztribúciókkal](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="eac63-149">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="eac63-150">A következő cikkekben végigvezeti Önt a különböző Linux terjesztésekről Azure által támogatott előkészítése:</span><span class="sxs-lookup"><span data-stu-id="eac63-150">The following articles guide you through how to prepare the various Linux distributions that are supported on Azure:</span></span>

* <span data-ttu-id="eac63-151">**[CentOS-alapú Disztribúciók](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="eac63-151">**[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="eac63-152">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="eac63-152">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="eac63-153">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="eac63-153">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="eac63-154">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="eac63-154">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="eac63-155">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="eac63-155">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="eac63-156">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="eac63-156">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="eac63-157">**[Egyéb - nem támogatott Disztribúciókkal](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="eac63-157">**[Other - Non-Endorsed Distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

<span data-ttu-id="eac63-158">Lásd még: a  **[Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes)**  kapcsolatos további általános tippek Linux lemezképek előkészítése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="eac63-158">Also see the **[Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="eac63-159">A [Azure platformon SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) csak akkor, ha a konfigurációs részleteket a támogatott verziók használt egyik a hitelesített terjesztéseket Linuxot futtató virtuális gépek érvényes [az Azure által támogatott Linux Azokat a terjesztéseket](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="eac63-159">The [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies to VMs running Linux only when one of the endorsed distributions is used with the configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="create-a-resource-group"></a><span data-ttu-id="eac63-160">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="eac63-160">Create a resource group</span></span>
<span data-ttu-id="eac63-161">Erőforráscsoportok logikailag összegyűjteni az Azure erőforrások, például a virtuális hálózati és tárolási a virtuális gép támogatása érdekében.</span><span class="sxs-lookup"><span data-stu-id="eac63-161">Resource groups logically bring together all the Azure resources to support your virtual machines, such as the virtual networking and storage.</span></span> <span data-ttu-id="eac63-162">További információ erőforráscsoportok, lásd: [erőforráscsoportokat áttekintése](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eac63-162">For more information resource groups, see [resource groups overview](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="eac63-163">Feltöltése az egyéni lemez és virtuális gépek létrehozását, mielőtt először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="eac63-163">Before uploading your custom disk and creating VMs, you first need to create a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="eac63-164">Az alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a a `westus` helye:</span><span class="sxs-lookup"><span data-stu-id="eac63-164">The following example creates a resource group named `myResourceGroup` in the `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a><span data-ttu-id="eac63-165">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="eac63-165">Create a storage account</span></span>

<span data-ttu-id="eac63-166">Hozzon létre egy tárfiókot, az egyéni lemez és virtuális gépek [az storage-fiók létrehozása](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="eac63-166">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="eac63-167">A virtuális gépeket hoz létre az egyéni lemez kell lenniük, hogy a lemez, ugyanazt a tárfiókot a nem felügyelt lemezzel.</span><span class="sxs-lookup"><span data-stu-id="eac63-167">Any VMs with unmanaged disks that you create from your custom disk need to be in the same storage account as that disk.</span></span> 

<span data-ttu-id="eac63-168">Az alábbi példa létrehoz egy nevű tárfiók `mystorageaccount` a korábban létrehozott erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="eac63-168">The following example creates a storage account named `mystorageaccount` in the resource group previously created:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a><span data-ttu-id="eac63-169">Tárfiókkulcsok listázása</span><span class="sxs-lookup"><span data-stu-id="eac63-169">List storage account keys</span></span>
<span data-ttu-id="eac63-170">Az Azure létrehoz két 512 bites kulcsot minden tárfiók.</span><span class="sxs-lookup"><span data-stu-id="eac63-170">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="eac63-171">A tárelérési kulcsok írási műveletek végrehajtására többek között a tárolási fiók hitelesítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="eac63-171">These access keys are used when authenticating to the storage account, such as to carry out write operations.</span></span> <span data-ttu-id="eac63-172">Tudjon meg többet az [tárolási itt történő hozzáférés felügyeletéhez](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="eac63-172">Read more about [managing access to storage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="eac63-173">Megtekintheti a tárelérési kulcsokat a [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="eac63-173">You view the access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="eac63-174">A létrehozott tárfiók hozzáférési kulcsainak megtekintése:</span><span class="sxs-lookup"><span data-stu-id="eac63-174">View the access keys for the storage account you created:</span></span>

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

<span data-ttu-id="eac63-175">Az eredmény az alábbihoz hasonlóan fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="eac63-175">The output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="eac63-176">Jegyezze fel a `key1` , amellyel kommunikálni tud a storage-fiókot a következő lépéseket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="eac63-176">Make a note of `key1` as you will use it to interact with your storage account in the next steps.</span></span>

## <a name="create-a-storage-container"></a><span data-ttu-id="eac63-177">A tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="eac63-177">Create a storage container</span></span>
<span data-ttu-id="eac63-178">Az Ön által létrehozott különböző könyvtárak, hogy logikusan rendszerezhesse az a helyi fájlrendszer ugyanúgy hozzon létre egy tárfiókot, a lemezek rendszerezéséhez tárolókra.</span><span class="sxs-lookup"><span data-stu-id="eac63-178">In the same way that you create different directories to logically organize your local file system, you create containers within a storage account to organize your disks.</span></span> <span data-ttu-id="eac63-179">A storage-fiók korlátlan számú tárolót tárolhat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="eac63-179">A storage account can contain any number of containers.</span></span> <span data-ttu-id="eac63-180">Hozzon létre egy tárolót a [az tároló létrehozása](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="eac63-180">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="eac63-181">Az alábbi példa létrehoz egy nevű tárolót `mydisks`:</span><span class="sxs-lookup"><span data-stu-id="eac63-181">The following example creates a container named `mydisks`:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a><span data-ttu-id="eac63-182">Virtuális merevlemez feltöltéséhez</span><span class="sxs-lookup"><span data-stu-id="eac63-182">Upload VHD</span></span>
<span data-ttu-id="eac63-183">Töltsön fel az egyéni lemezzel [az tárolási blob feltöltése](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="eac63-183">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="eac63-184">Töltse fel, és tárolja az egyéni lemezt oldalblobként.</span><span class="sxs-lookup"><span data-stu-id="eac63-184">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="eac63-185">Adja meg a hozzáférési kulcsot a tároló létrehozott az előző lépést, és az egyéni lemez elérési útja a helyi számítógépen:</span><span class="sxs-lookup"><span data-stu-id="eac63-185">Specify your access key, the container you created in the previous step, and then the path to the custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-the-vm"></a><span data-ttu-id="eac63-186">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="eac63-186">Create the VM</span></span>
<span data-ttu-id="eac63-187">Nem felügyelt lemezzel rendelkező virtuális gép létrehozása, adja meg az URI-t a lemez (`--image`) rendelkező [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="eac63-187">To create a VM with unmanaged disks, specify the URI to your disk (`--image`) with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="eac63-188">Az alábbi példakód létrehozza a virtuális gépek nevű `myVM` korábban feltöltött a virtuális lemez segítségével:</span><span class="sxs-lookup"><span data-stu-id="eac63-188">The following example creates a VM named `myVM` using the virtual disk previously uploaded:</span></span>

<span data-ttu-id="eac63-189">Adja meg, hogy a `--image` paraméterrel [az virtuális gép létrehozása](/cli/azure/vm#create) úgy, hogy az egyéni lemez mutasson.</span><span class="sxs-lookup"><span data-stu-id="eac63-189">You specify the `--image` parameter with [az vm create](/cli/azure/vm#create) to point to your custom disk.</span></span> <span data-ttu-id="eac63-190">Győződjön meg arról, hogy `--storage-account` megegyezik a tárfiókot, az egyéni lemez tárolására.</span><span class="sxs-lookup"><span data-stu-id="eac63-190">Ensure that `--storage-account` matches the storage account where your custom disk is stored.</span></span> <span data-ttu-id="eac63-191">Nincs a virtuális gépek tárolására használni, az egyéni lemezként ugyanabban a tárolóban.</span><span class="sxs-lookup"><span data-stu-id="eac63-191">You do not have to use the same container as the custom disk to store your VMs.</span></span> <span data-ttu-id="eac63-192">Ügyeljen arra, hogy az egyéni lemez feltöltés előtt hozzon létre semmilyen további tárolókat ugyanúgy, mint a korábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="eac63-192">Make sure to create any additional containers in the same way as the earlier steps before uploading your custom disk.</span></span>

<span data-ttu-id="eac63-193">Az alábbi példakód létrehozza a virtuális gépek nevű `myVM` a egyéni lemezről:</span><span class="sxs-lookup"><span data-stu-id="eac63-193">The following example creates a VM named `myVM` from your custom disk:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

<span data-ttu-id="eac63-194">Meg kell adni, vagy válasz kér, által igényelt minden további paramétereket a **az virtuális gép létrehozása** parancsot például felhasználónév és SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="eac63-194">You still need to specify, or answer prompts for, all the additional parameters required by the **az vm create** command such as username and SSH keys.</span></span>


## <a name="resource-manager-template"></a><span data-ttu-id="eac63-195">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="eac63-195">Resource Manager template</span></span>
<span data-ttu-id="eac63-196">Az Azure Resource Manager-sablonok olyan JavaScript Object Notation (JSON), amelyeket a környezet létrehozása build kíván.</span><span class="sxs-lookup"><span data-stu-id="eac63-196">Azure Resource Manager templates are JavaScript Object Notation (JSON) files that define the environment you wish to build.</span></span> <span data-ttu-id="eac63-197">A sablonok megszakadnak a például számítási és hálózati különböző erőforrás-szolgáltatók.</span><span class="sxs-lookup"><span data-stu-id="eac63-197">The templates are broken down in to different resource providers such as compute or network.</span></span> <span data-ttu-id="eac63-198">Meglévő sablonok használhatja, vagy a saját írni.</span><span class="sxs-lookup"><span data-stu-id="eac63-198">You can use existing templates or write your own.</span></span> <span data-ttu-id="eac63-199">Tudjon meg többet az [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eac63-199">Read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="eac63-200">Belül a `Microsoft.Compute/virtualMachines` a sablon-szolgáltatót, hogy egy `storageProfile` csomópont, amely tartalmazza a konfigurációs részleteket a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="eac63-200">Within the `Microsoft.Compute/virtualMachines` provider of your template, you have a `storageProfile` node that contains the configuration details for your VM.</span></span> <span data-ttu-id="eac63-201">A két fő paraméterek szerkesztése a `image` és `vhd` URI-azonosítók, amelyek az egyéni és az új virtuális gép virtuális lemezzel.</span><span class="sxs-lookup"><span data-stu-id="eac63-201">The two main parameters to edit are the `image` and `vhd` URIs that point to your custom disk and your new VM's virtual disk.</span></span> <span data-ttu-id="eac63-202">A következő egyéni lemezt használ a JSON példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="eac63-202">The following shows an example of the JSON for using a custom disk:</span></span>

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

<span data-ttu-id="eac63-203">Használhat [a meglévő sablont, és hozzon létre egy virtuális Gépet egy egyéni lemezképből](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) vagy foglalkozó témakört [a saját Azure Resource Manager-sablonok készítése](../../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="eac63-203">You can use [this existing template to create a VM from a custom image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) or read about [creating your own Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 

<span data-ttu-id="eac63-204">Ha egy sablon konfigurálva van, a [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) a virtuális gépek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="eac63-204">Once you have a template configured, use [az group deployment create](/cli/azure/group/deployment#create) to create your VMs.</span></span> <span data-ttu-id="eac63-205">Adja meg a JSON sablonok az URI a `--template-uri` paraméter:</span><span class="sxs-lookup"><span data-stu-id="eac63-205">Specify the URI of your JSON template with the `--template-uri` parameter:</span></span>

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

<span data-ttu-id="eac63-206">Ha egy számítógépen helyben tárolt JSON-fájl, használhatja a `--template-file` paraméter helyette:</span><span class="sxs-lookup"><span data-stu-id="eac63-206">If you have a JSON file stored locally on your computer, you can use the `--template-file` parameter instead:</span></span>

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a><span data-ttu-id="eac63-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eac63-207">Next steps</span></span>
<span data-ttu-id="eac63-208">Miután előkészített és feltöltése az egyéni virtuális lemez, akkor további információ [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eac63-208">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="eac63-209">Is érdemes lehet [hozzá adatlemezt](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) az új virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="eac63-209">You may also want to [add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to your new VMs.</span></span> <span data-ttu-id="eac63-210">Ha fut a virtuális gépek elérését igénylő alkalmazások, ügyeljen arra, hogy [nyisson meg portokat és a végpontok](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="eac63-210">If you have applications running on your VMs that you need to access, be sure to [open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

