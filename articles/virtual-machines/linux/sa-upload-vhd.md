---
title: "Azure CLI 2.0 egyéni Linux lemezzel aaaUpload |} Microsoft Docs"
description: "Hozzon létre, és töltse fel a virtuális merevlemez (VHD) tooAzure hello Resource Manager üzembe helyezési modellben és hello Azure CLI 2.0 használatával"
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
ms.openlocfilehash: cf8556ab4dfe6c4e5eff4e99fe1ddc44393f774c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a><span data-ttu-id="3686c-103">Töltse fel, és a Linux virtuális gép létrehozása az Azure CLI 2.0 hello egyéni lemezről</span><span class="sxs-lookup"><span data-stu-id="3686c-103">Upload and create a Linux VM from custom disk with hello Azure CLI 2.0</span></span>
<span data-ttu-id="3686c-104">Ez a cikk bemutatja, hogyan tooupload egy virtuális merevlemez (VHD) tooan az Azure storage rendelkező fiók hello Azure CLI 2.0, és a Linux virtuális gépek létrehozása a egyéni lemezt.</span><span class="sxs-lookup"><span data-stu-id="3686c-104">This article shows you how tooupload a virtual hard disk (VHD) tooan Azure storage account with hello Azure CLI 2.0 and create Linux VMs from this custom disk.</span></span> <span data-ttu-id="3686c-105">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3686c-105">You can also perform these steps with hello [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="3686c-106">Ez a funkció lehetővé teszi tooinstall, és egy Linux distro tooyour követelmények konfigurálása, és ezután használja az adott VHD tooquickly létrehozása az Azure virtuális gépek (VM).</span><span class="sxs-lookup"><span data-stu-id="3686c-106">This functionality allows you tooinstall and configure a Linux distro tooyour requirements and then use that VHD tooquickly create Azure virtual machines (VMs).</span></span>

<span data-ttu-id="3686c-107">Ez a témakör az hello végső virtuális merevlemezek, de is végrehajthatja a lépések használatával tárfiókok használ [által kezelt lemezeken](upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="3686c-107">This topic uses storage accounts for hello final VHDs, but you can also do these steps using [managed disks](upload-vhd.md).</span></span> 

## <a name="quick-commands"></a><span data-ttu-id="3686c-108">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="3686c-108">Quick commands</span></span>
<span data-ttu-id="3686c-109">Ha tooquickly kell hello feladatnak, a következő szakasz részletek hello hello kiinduló parancsok tooupload egy virtuális merevlemez tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3686c-109">If you need tooquickly accomplish hello task, hello following section details hello base commands tooupload a VHD tooAzure.</span></span> <span data-ttu-id="3686c-110">Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#requirements).</span><span class="sxs-lookup"><span data-stu-id="3686c-110">More detailed information and context for each step can be found hello rest of hello document, [starting here](#requirements).</span></span>

<span data-ttu-id="3686c-111">Győződjön meg arról, hogy rendelkezik hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="3686c-111">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="3686c-112">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="3686c-112">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="3686c-113">Példa paraméter nevekre `myResourceGroup`, `mystorageaccount`, és `mydisks`.</span><span class="sxs-lookup"><span data-stu-id="3686c-113">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `mydisks`.</span></span>

<span data-ttu-id="3686c-114">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="3686c-114">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="3686c-115">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `WestUs` helye:</span><span class="sxs-lookup"><span data-stu-id="3686c-115">hello following example creates a resource group named `myResourceGroup` in hello `WestUs` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

<span data-ttu-id="3686c-116">Hozzon létre egy tárolási fiók toohold a virtuális lemezek, amelyek [az storage-fiók létrehozása](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="3686c-116">Create a storage account toohold your virtual disks with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="3686c-117">hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="3686c-117">hello following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

<span data-ttu-id="3686c-118">A tárfiók hello elérési kulcsainak listázása [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="3686c-118">List hello access keys for your storage account with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="3686c-119">Jegyezze fel a `key1`:</span><span class="sxs-lookup"><span data-stu-id="3686c-119">Make a note of `key1`:</span></span>

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

<span data-ttu-id="3686c-120">Hozzon létre egy tárolót a tárfiókon belül hello biztonságitár-kulcs használatával kapott [az tároló létrehozása](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="3686c-120">Create a container within your storage account using hello storage key you obtained with [az storage container create](/cli/azure/storage/container#create).</span></span> <span data-ttu-id="3686c-121">hello alábbi példa létrehoz egy nevű tárolót `mydisks` hello tárolási értékének használatával `key1`:</span><span class="sxs-lookup"><span data-stu-id="3686c-121">hello following example creates a container named `mydisks` using hello storage key value from `key1`:</span></span>

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

<span data-ttu-id="3686c-122">Végül, töltse fel a létrehozott VHD-toohello tároló [az tárolási blob feltöltése](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="3686c-122">Finally, upload your VHD toohello container you created with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="3686c-123">Adja meg a hello helyi elérési út tooyour virtuális merevlemez alapján `/path/to/disk/mydisk.vhd`:</span><span class="sxs-lookup"><span data-stu-id="3686c-123">Specify hello local path tooyour VHD under `/path/to/disk/mydisk.vhd`:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

<span data-ttu-id="3686c-124">Adja meg a hello URI tooyour lemez (`--image`) rendelkező [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="3686c-124">Specify hello URI tooyour disk (`--image`) with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="3686c-125">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` hello korábban feltöltött virtuális lemez segítségével:</span><span class="sxs-lookup"><span data-stu-id="3686c-125">hello following example creates a VM named `myVM` using hello virtual disk previously uploaded:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

<span data-ttu-id="3686c-126">hello céltárfiókot hello ugyanaz, mint ahol a virtuális lemez feltöltött toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3686c-126">hello destination storage account has toobe hello same as where you uploaded your virtual disk to.</span></span> <span data-ttu-id="3686c-127">Továbbá toospecify kell, vagy a vonatkozó választ, minden hello hello szükséges további paraméterek **az virtuális gép létrehozása** parancs például a virtuális hálózat, a nyilvános IP-cím, a felhasználónevet és az SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="3686c-127">You also need toospecify, or answer prompts for, all hello additional parameters required by hello **az vm create** command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="3686c-128">További tudnivalók hello [érhető el erőforrás-kezelő parancssori paraméterek](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="3686c-128">You can read more about hello [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

## <a name="requirements"></a><span data-ttu-id="3686c-129">Követelmények</span><span class="sxs-lookup"><span data-stu-id="3686c-129">Requirements</span></span>
<span data-ttu-id="3686c-130">toocomplete hello lépéseket követve van szüksége:</span><span class="sxs-lookup"><span data-stu-id="3686c-130">toocomplete hello following steps, you need:</span></span>

* <span data-ttu-id="3686c-131">**Linux operációs rendszer van telepítve, a .vhd-fájllá** -telepíteni egy [Azure által támogatott Linux-disztribúció](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (vagy lásd: [nem támogatott disztribúciókkal kapcsolatos információi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa hello VHD lévő virtuális lemezek formátumban.</span><span class="sxs-lookup"><span data-stu-id="3686c-131">**Linux operating system installed in a .vhd file** - Install an [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="3686c-132">Több különféle eszköz toocreate érhetők el a virtuális gép és a virtuális merevlemez:</span><span class="sxs-lookup"><span data-stu-id="3686c-132">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="3686c-133">Telepítse és konfigurálja [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve toouse a lemezkép formátumú virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="3686c-133">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="3686c-134">Ha szükséges, akkor [lemezkép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) használatával `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="3686c-134">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="3686c-135">Is használhatja a Hyper-V [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) vagy [Windows Server 2012 vagy 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="3686c-135">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="3686c-136">hello újabb VHDX formátum nem támogatott az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="3686c-136">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="3686c-137">Amikor létrehoz egy virtuális Gépet, adja meg a virtuális merevlemez hello formátumban.</span><span class="sxs-lookup"><span data-stu-id="3686c-137">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="3686c-138">Ha szükséges, VHDX-lemezek tooVHD használatával konvertálhatja [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="3686c-138">If needed, you can convert VHDX disks tooVHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="3686c-139">További Azure nem támogatja dinamikus virtuális merevlemezek, feltöltése, ezért meg kell tooconvert ilyen lemezek toostatic VHD-k feltöltés előtt.</span><span class="sxs-lookup"><span data-stu-id="3686c-139">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="3686c-140">Használhatja például a [NYISSA meg az Azure virtuális merevlemez segédprogramok](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert a dinamikus lemezek a hello folyamat tooAzure feltöltése során.</span><span class="sxs-lookup"><span data-stu-id="3686c-140">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>
> 
> 

* <span data-ttu-id="3686c-141">Az egyéni lemez alapján létrehozott virtuális gépek kell lennie, hello azonos maga hello lemezként storage-fiók</span><span class="sxs-lookup"><span data-stu-id="3686c-141">VMs created from your custom disk must reside in hello same storage account as hello disk itself</span></span>
  * <span data-ttu-id="3686c-142">Hozzon létre egy tárolási fiók és a tároló toohold, az egyéni lemez és a létrehozott virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="3686c-142">Create a storage account and container toohold both your custom disk and created VMs</span></span>
  * <span data-ttu-id="3686c-143">Miután létrehozta a virtuális gépek, nyugodtan törölheti a lemezen</span><span class="sxs-lookup"><span data-stu-id="3686c-143">After you have created all your VMs, you can safely delete your disk</span></span>

<span data-ttu-id="3686c-144">Győződjön meg arról, hogy rendelkezik hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="3686c-144">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="3686c-145">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="3686c-145">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="3686c-146">Példa paraméter nevekre `myResourceGroup`, `mystorageaccount`, és `mydisks`.</span><span class="sxs-lookup"><span data-stu-id="3686c-146">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `mydisks`.</span></span>

<span data-ttu-id="3686c-147"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="3686c-147"><a id="prepimage"> </a></span></span>

## <a name="prepare-hello-disk-toobe-uploaded"></a><span data-ttu-id="3686c-148">Készítse elő a hello lemez toobe feltöltve.</span><span class="sxs-lookup"><span data-stu-id="3686c-148">Prepare hello disk toobe uploaded</span></span>
<span data-ttu-id="3686c-149">Azure támogatja a különböző Linux terjesztésekről (lásd: [támogatott Disztribúciókkal](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="3686c-149">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="3686c-150">a következő cikkek hello végigvezetik hogyan tooprepare hello Azure által támogatott különböző Linux-disztribúció:</span><span class="sxs-lookup"><span data-stu-id="3686c-150">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure:</span></span>

* <span data-ttu-id="3686c-151">**[CentOS-alapú Disztribúciók](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="3686c-151">**[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="3686c-152">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="3686c-152">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="3686c-153">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="3686c-153">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="3686c-154">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="3686c-154">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="3686c-155">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="3686c-155">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="3686c-156">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="3686c-156">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="3686c-157">**[Egyéb - nem támogatott Disztribúciókkal](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="3686c-157">**[Other - Non-Endorsed Distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

<span data-ttu-id="3686c-158">Lásd még: hello  **[Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes)**  kapcsolatos további általános tippek Linux lemezképek előkészítése az Azure.</span><span class="sxs-lookup"><span data-stu-id="3686c-158">Also see hello **[Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="3686c-159">Hello [Azure platformon SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) Linux operációs rendszert futtató, csak ha egyik hello által támogatott disztribúciók használt hello konfigurációs adatait a támogatott verziók tooVMs érvényes [az Azure által támogatott Linux Azokat a terjesztéseket](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3686c-159">hello [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies tooVMs running Linux only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="create-a-resource-group"></a><span data-ttu-id="3686c-160">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="3686c-160">Create a resource group</span></span>
<span data-ttu-id="3686c-161">Erőforráscsoportok logikailag összefogni összes hello Azure-erőforrások toosupport a virtuális gépek, például a hello virtuális hálózati és tárolási.</span><span class="sxs-lookup"><span data-stu-id="3686c-161">Resource groups logically bring together all hello Azure resources toosupport your virtual machines, such as hello virtual networking and storage.</span></span> <span data-ttu-id="3686c-162">További információ erőforráscsoportok, lásd: [erőforráscsoportokat áttekintése](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3686c-162">For more information resource groups, see [resource groups overview](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="3686c-163">Feltöltése az egyéni lemez és virtuális gépek létrehozását, mielőtt először toocreate nevű erőforráscsoport [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="3686c-163">Before uploading your custom disk and creating VMs, you first need toocreate a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="3686c-164">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westus` helye:</span><span class="sxs-lookup"><span data-stu-id="3686c-164">hello following example creates a resource group named `myResourceGroup` in hello `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a><span data-ttu-id="3686c-165">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="3686c-165">Create a storage account</span></span>

<span data-ttu-id="3686c-166">Hozzon létre egy tárfiókot, az egyéni lemez és virtuális gépek [az storage-fiók létrehozása](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="3686c-166">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> <span data-ttu-id="3686c-167">A virtuális gépeket hoz létre az egyéni lemez kell toobe a nem felügyelt lemezzel hello ugyanazt a tárfiókot, a lemeznek.</span><span class="sxs-lookup"><span data-stu-id="3686c-167">Any VMs with unmanaged disks that you create from your custom disk need toobe in hello same storage account as that disk.</span></span> 

<span data-ttu-id="3686c-168">hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount` hello erőforráscsoportban korábban hozott létre:</span><span class="sxs-lookup"><span data-stu-id="3686c-168">hello following example creates a storage account named `mystorageaccount` in hello resource group previously created:</span></span>

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a><span data-ttu-id="3686c-169">Tárfiókkulcsok listázása</span><span class="sxs-lookup"><span data-stu-id="3686c-169">List storage account keys</span></span>
<span data-ttu-id="3686c-170">Az Azure létrehoz két 512 bites kulcsot minden tárfiók.</span><span class="sxs-lookup"><span data-stu-id="3686c-170">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="3686c-171">A hozzáférési kulcsot használnak az toohello tárfiókot, például az írási műveletek toocarry hitelesítésekor.</span><span class="sxs-lookup"><span data-stu-id="3686c-171">These access keys are used when authenticating toohello storage account, such as toocarry out write operations.</span></span> <span data-ttu-id="3686c-172">Tudjon meg többet az [kezelése hozzáférési toostorage Itt](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="3686c-172">Read more about [managing access toostorage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="3686c-173">Megtekintheti a hello hívóbetűk [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="3686c-173">You view hello access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="3686c-174">Hello elérési kulcsainak megtekintése létrehozott tárfiók hello:</span><span class="sxs-lookup"><span data-stu-id="3686c-174">View hello access keys for hello storage account you created:</span></span>

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

<span data-ttu-id="3686c-175">hello kimeneti hasonlít:</span><span class="sxs-lookup"><span data-stu-id="3686c-175">hello output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="3686c-176">Jegyezze fel a `key1` még szüksége lesz rájuk toointeract a storage-fiók hello következő lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="3686c-176">Make a note of `key1` as you will use it toointeract with your storage account in hello next steps.</span></span>

## <a name="create-a-storage-container"></a><span data-ttu-id="3686c-177">A tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="3686c-177">Create a storage container</span></span>
<span data-ttu-id="3686c-178">A hello azonos módon, hogy hozzon létre különböző könyvtárak toologically rendezheti a helyi fájlrendszer, a lemezek létrehozni a tárolási fiók tooorganize tárolókra.</span><span class="sxs-lookup"><span data-stu-id="3686c-178">In hello same way that you create different directories toologically organize your local file system, you create containers within a storage account tooorganize your disks.</span></span> <span data-ttu-id="3686c-179">A storage-fiók korlátlan számú tárolót tárolhat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="3686c-179">A storage account can contain any number of containers.</span></span> <span data-ttu-id="3686c-180">Hozzon létre egy tárolót a [az tároló létrehozása](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="3686c-180">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="3686c-181">hello alábbi példa létrehoz egy nevű tárolót `mydisks`:</span><span class="sxs-lookup"><span data-stu-id="3686c-181">hello following example creates a container named `mydisks`:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a><span data-ttu-id="3686c-182">Virtuális merevlemez feltöltéséhez</span><span class="sxs-lookup"><span data-stu-id="3686c-182">Upload VHD</span></span>
<span data-ttu-id="3686c-183">Töltsön fel az egyéni lemezzel [az tárolási blob feltöltése](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="3686c-183">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="3686c-184">Töltse fel, és tárolja az egyéni lemezt oldalblobként.</span><span class="sxs-lookup"><span data-stu-id="3686c-184">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="3686c-185">Adja meg a hozzáférési kulcsot a hello tároló hello előző lépésben létrehozott, és ezután hello az elérési út toohello egyéni lemezt a helyi számítógépen:</span><span class="sxs-lookup"><span data-stu-id="3686c-185">Specify your access key, hello container you created in hello previous step, and then hello path toohello custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-hello-vm"></a><span data-ttu-id="3686c-186">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="3686c-186">Create hello VM</span></span>
<span data-ttu-id="3686c-187">nem felügyelt lemezzel rendelkező virtuális gép toocreate meg hello URI tooyour lemezt (`--image`) rendelkező [az virtuális gép létrehozása](/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="3686c-187">toocreate a VM with unmanaged disks, specify hello URI tooyour disk (`--image`) with [az vm create](/cli/azure/vm#create).</span></span> <span data-ttu-id="3686c-188">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` hello korábban feltöltött virtuális lemez segítségével:</span><span class="sxs-lookup"><span data-stu-id="3686c-188">hello following example creates a VM named `myVM` using hello virtual disk previously uploaded:</span></span>

<span data-ttu-id="3686c-189">Megadja a hello `--image` paraméterrel [az virtuális gép létrehozása](/cli/azure/vm#create) toopoint tooyour egyéni lemez.</span><span class="sxs-lookup"><span data-stu-id="3686c-189">You specify hello `--image` parameter with [az vm create](/cli/azure/vm#create) toopoint tooyour custom disk.</span></span> <span data-ttu-id="3686c-190">Győződjön meg arról, hogy `--storage-account` egyező hello tárfiók a egyéni lemezen tárolja.</span><span class="sxs-lookup"><span data-stu-id="3686c-190">Ensure that `--storage-account` matches hello storage account where your custom disk is stored.</span></span> <span data-ttu-id="3686c-191">Nincs toouse hello ugyanabban a tárolóban, egyéni lemez toostore hello a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="3686c-191">You do not have toouse hello same container as hello custom disk toostore your VMs.</span></span> <span data-ttu-id="3686c-192">Győződjön meg arról, hogy toocreate további tárolókkal hello azonos módon hello korábbi lépéseket az egyéni lemez feltöltés előtt.</span><span class="sxs-lookup"><span data-stu-id="3686c-192">Make sure toocreate any additional containers in hello same way as hello earlier steps before uploading your custom disk.</span></span>

<span data-ttu-id="3686c-193">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` a egyéni lemezről:</span><span class="sxs-lookup"><span data-stu-id="3686c-193">hello following example creates a VM named `myVM` from your custom disk:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

<span data-ttu-id="3686c-194">Ön továbbra is szükséges toospecify, vagy a választ kér, összes hello hello szükséges további paraméterek **az virtuális gép létrehozása** parancsot például felhasználónév és SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="3686c-194">You still need toospecify, or answer prompts for, all hello additional parameters required by hello **az vm create** command such as username and SSH keys.</span></span>


## <a name="resource-manager-template"></a><span data-ttu-id="3686c-195">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="3686c-195">Resource Manager template</span></span>
<span data-ttu-id="3686c-196">Az Azure Resource Manager-sablonok a JavaScript Object Notation (JSON) fájlok hello környezet toobuild kívánja.</span><span class="sxs-lookup"><span data-stu-id="3686c-196">Azure Resource Manager templates are JavaScript Object Notation (JSON) files that define hello environment you wish toobuild.</span></span> <span data-ttu-id="3686c-197">hello sablonok az erőforrás-szolgáltatók toodifferent például számítási és hálózati részletezik.</span><span class="sxs-lookup"><span data-stu-id="3686c-197">hello templates are broken down in toodifferent resource providers such as compute or network.</span></span> <span data-ttu-id="3686c-198">Meglévő sablonok használhatja, vagy a saját írni.</span><span class="sxs-lookup"><span data-stu-id="3686c-198">You can use existing templates or write your own.</span></span> <span data-ttu-id="3686c-199">Tudjon meg többet az [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3686c-199">Read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="3686c-200">Hello belül `Microsoft.Compute/virtualMachines` a sablon-szolgáltatót, hogy egy `storageProfile` hello konfigurációs adatait a virtuális gép tartalmazó csomópontot.</span><span class="sxs-lookup"><span data-stu-id="3686c-200">Within hello `Microsoft.Compute/virtualMachines` provider of your template, you have a `storageProfile` node that contains hello configuration details for your VM.</span></span> <span data-ttu-id="3686c-201">hello két fő paraméterek tooedit hello `image` és `vhd` URI-azonosítók, amelyek tooyour egyéni és az új virtuális gép virtuális lemezzel.</span><span class="sxs-lookup"><span data-stu-id="3686c-201">hello two main parameters tooedit are hello `image` and `vhd` URIs that point tooyour custom disk and your new VM's virtual disk.</span></span> <span data-ttu-id="3686c-202">hello következő hello JSON látható az egyéni lemezt használ:</span><span class="sxs-lookup"><span data-stu-id="3686c-202">hello following shows an example of hello JSON for using a custom disk:</span></span>

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

<span data-ttu-id="3686c-203">Használhat [a meglévő sablon toocreate egy virtuális Gépet egy egyéni lemezképből](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) vagy foglalkozó témakört [a saját Azure Resource Manager-sablonok készítése](../../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3686c-203">You can use [this existing template toocreate a VM from a custom image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) or read about [creating your own Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 

<span data-ttu-id="3686c-204">Ha egy sablon konfigurálva van, a [az csoport központi telepítésének létrehozása](/cli/azure/group/deployment#create) toocreate a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="3686c-204">Once you have a template configured, use [az group deployment create](/cli/azure/group/deployment#create) toocreate your VMs.</span></span> <span data-ttu-id="3686c-205">Adja meg a JSON-sablon URI hello hello `--template-uri` paraméter:</span><span class="sxs-lookup"><span data-stu-id="3686c-205">Specify hello URI of your JSON template with hello `--template-uri` parameter:</span></span>

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

<span data-ttu-id="3686c-206">Ha egy számítógépen helyben tárolt JSON-fájl, használhatja a hello `--template-file` paraméter helyette:</span><span class="sxs-lookup"><span data-stu-id="3686c-206">If you have a JSON file stored locally on your computer, you can use hello `--template-file` parameter instead:</span></span>

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a><span data-ttu-id="3686c-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3686c-207">Next steps</span></span>
<span data-ttu-id="3686c-208">Miután előkészített és feltöltése az egyéni virtuális lemez, akkor további információ [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3686c-208">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="3686c-209">Akkor is érdemes lehet túl[hozzá adatlemezt](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour új virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="3686c-209">You may also want too[add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour new VMs.</span></span> <span data-ttu-id="3686c-210">Ha a virtuális gépeken futó, hogy kell-e tooaccess alkalmazásai, ne feledje túl[nyisson meg portokat és a végpontok](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3686c-210">If you have applications running on your VMs that you need tooaccess, be sure too[open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

