---
title: "az Azure CLI 1.0 egyéni Linux kép aaaUpload |} Microsoft Docs"
description: "Hozzon létre, és töltse fel a virtuális merevlemez (VHD) tooAzure hello Resource Manager üzembe helyezési modellben és hello Azure CLI 1.0 használatával egyéni Linux képének."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: iainfou
ms.openlocfilehash: 0bbd232debee1e632233d1c010e388dbc1548bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-hello-azure-cli-10"></a><span data-ttu-id="37891-103">Töltse fel, és a Linux virtuális gép létrehozása az egyéni lemezképet hello Azure CLI 1.0 használatával</span><span class="sxs-lookup"><span data-stu-id="37891-103">Upload and create a Linux VM from custom disk image by using hello Azure CLI 1.0</span></span>
<span data-ttu-id="37891-104">Ez a cikk bemutatja, hogyan egy virtuális merevlemez (VHD) tooAzure használatával tooupload hello Resource Manager üzembe helyezési modellben és a Linux virtuális gépek létrehozása az egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="37891-104">This article shows you how tooupload a virtual hard disk (VHD) tooAzure using hello Resource Manager deployment model and create Linux VMs from this custom image.</span></span> <span data-ttu-id="37891-105">Ez a funkció lehetővé teszi tooinstall, és egy Linux distro tooyour követelmények konfigurálása, és ezután használja az adott VHD tooquickly létrehozása az Azure virtuális gépek (VM).</span><span class="sxs-lookup"><span data-stu-id="37891-105">This functionality allows you tooinstall and configure a Linux distro tooyour requirements and then use that VHD tooquickly create Azure virtual machines (VMs).</span></span>


## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="37891-106">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="37891-106">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="37891-107">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="37891-107">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="37891-108">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="37891-108">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="37891-109">[Az Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="37891-109">[Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="37891-110">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="37891-110">Quick commands</span></span>
<span data-ttu-id="37891-111">Ha tooquickly kell hello feladatnak, a következő szakasz részletek hello hello kiinduló parancsok tooupload egy virtuális gép tooAzure.</span><span class="sxs-lookup"><span data-stu-id="37891-111">If you need tooquickly accomplish hello task, hello following section details hello base commands tooupload a VM tooAzure.</span></span> <span data-ttu-id="37891-112">Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#requirements).</span><span class="sxs-lookup"><span data-stu-id="37891-112">More detailed information and context for each step can be found hello rest of hello document, [starting here](#requirements).</span></span>

<span data-ttu-id="37891-113">Győződjön meg arról, hogy rendelkezik [hello Azure CLI 1.0](../../cli-install-nodejs.md) jelentkezett be, és a Resource Manager módot használ:</span><span class="sxs-lookup"><span data-stu-id="37891-113">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="37891-114">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="37891-114">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="37891-115">Példa paraméter nevekre `myResourceGroup`, `mystorageaccount`, és `myimages`.</span><span class="sxs-lookup"><span data-stu-id="37891-115">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `myimages`.</span></span>

<span data-ttu-id="37891-116">Először hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="37891-116">First, create a resource group.</span></span> <span data-ttu-id="37891-117">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `WestUs` helye:</span><span class="sxs-lookup"><span data-stu-id="37891-117">hello following example creates a resource group named `myResourceGroup` in hello `WestUs` location:</span></span>

```azurecli
azure group create myResourceGroup --location "WestUS"
```

<span data-ttu-id="37891-118">A tárolási fiók toohold a virtuális lemezeket hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="37891-118">Create a storage account toohold your virtual disks.</span></span> <span data-ttu-id="37891-119">hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount`:</span><span class="sxs-lookup"><span data-stu-id="37891-119">hello following example creates a storage account named `mystorageaccount`:</span></span>

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

<span data-ttu-id="37891-120">A tárfiók hello elérési kulcsainak listázása.</span><span class="sxs-lookup"><span data-stu-id="37891-120">List hello access keys for your storage account.</span></span> <span data-ttu-id="37891-121">Jegyezze fel a `key1`:</span><span class="sxs-lookup"><span data-stu-id="37891-121">Make a note of `key1`:</span></span>

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

<span data-ttu-id="37891-122">Hozzon létre egy tárolót a beszerzett hello biztonságitár-kulcs használatával tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="37891-122">Create a container within your storage account using hello storage key you obtained.</span></span> <span data-ttu-id="37891-123">hello alábbi példa létrehoz egy nevű tárolót `myimages` hello tárolási értékének használatával `key1`:</span><span class="sxs-lookup"><span data-stu-id="37891-123">hello following example creates a container named `myimages` using hello storage key value from `key1`:</span></span>

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

<span data-ttu-id="37891-124">Végül töltse fel a létrehozott VHD-toohello tároló.</span><span class="sxs-lookup"><span data-stu-id="37891-124">Finally, upload your VHD toohello container you created.</span></span> <span data-ttu-id="37891-125">Adja meg a hello helyi elérési út tooyour virtuális merevlemez alapján `/path/to/disk/mydisk.vhd`:</span><span class="sxs-lookup"><span data-stu-id="37891-125">Specify hello local path tooyour VHD under `/path/to/disk/mydisk.vhd`:</span></span>

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

<span data-ttu-id="37891-126">Mostantól létrehozhat egy virtuális Gépet a feltöltött virtuális lemez [Resource Manager-sablon használatával](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd).</span><span class="sxs-lookup"><span data-stu-id="37891-126">You can now create a VM from your uploaded virtual disk [using a Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd).</span></span> <span data-ttu-id="37891-127">Használhatja a parancssori felület hello hello URI tooyour lemez megadásával (`--image-urn`).</span><span class="sxs-lookup"><span data-stu-id="37891-127">You can also use hello CLI by specifying hello URI tooyour disk (`--image-urn`).</span></span> <span data-ttu-id="37891-128">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` hello korábban feltöltött virtuális lemez segítségével:</span><span class="sxs-lookup"><span data-stu-id="37891-128">hello following example creates a VM named `myVM` using hello virtual disk previously uploaded:</span></span>

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

<span data-ttu-id="37891-129">hello céltárfiókot hello ugyanaz, mint ahol a virtuális lemez feltöltött toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="37891-129">hello destination storage account has toobe hello same as where you uploaded your virtual disk to.</span></span> <span data-ttu-id="37891-130">Továbbá toospecify kell, vagy a vonatkozó választ, minden hello hello szükséges további paraméterek `azure vm create` parancs például a virtuális hálózat, a nyilvános IP-cím, a felhasználónevet és az SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="37891-130">You also need toospecify, or answer prompts for, all hello additional parameters required by hello `azure vm create` command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="37891-131">További tudnivalók hello [érhető el erőforrás-kezelő parancssori paraméterek](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="37891-131">You can read more about hello [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

## <a name="requirements"></a><span data-ttu-id="37891-132">Követelmények</span><span class="sxs-lookup"><span data-stu-id="37891-132">Requirements</span></span>
<span data-ttu-id="37891-133">toocomplete hello lépéseket követve van szüksége:</span><span class="sxs-lookup"><span data-stu-id="37891-133">toocomplete hello following steps, you need:</span></span>

* <span data-ttu-id="37891-134">**Linux operációs rendszer van telepítve, a .vhd-fájllá** -telepíteni egy [Azure által támogatott Linux-disztribúció](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (vagy lásd: [nem támogatott disztribúciókkal kapcsolatos információi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa hello VHD lévő virtuális lemezek formátumban.</span><span class="sxs-lookup"><span data-stu-id="37891-134">**Linux operating system installed in a .vhd file** - Install an [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="37891-135">Több különféle eszköz toocreate érhetők el a virtuális gép és a virtuális merevlemez:</span><span class="sxs-lookup"><span data-stu-id="37891-135">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="37891-136">Telepítse és konfigurálja [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve toouse a lemezkép formátumú virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="37891-136">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="37891-137">Ha szükséges, akkor [lemezkép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) használatával `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="37891-137">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="37891-138">Is használhatja a Hyper-V [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) vagy [Windows Server 2012 vagy 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="37891-138">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="37891-139">hello újabb VHDX formátum nem támogatott az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="37891-139">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="37891-140">Amikor létrehoz egy virtuális Gépet, adja meg a virtuális merevlemez hello formátumban.</span><span class="sxs-lookup"><span data-stu-id="37891-140">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="37891-141">Ha szükséges, VHDX-lemezek tooVHD használatával konvertálhatja [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="37891-141">If needed, you can convert VHDX disks tooVHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="37891-142">További Azure nem támogatja dinamikus virtuális merevlemezek, feltöltése, ezért meg kell tooconvert ilyen lemezek toostatic VHD-k feltöltés előtt.</span><span class="sxs-lookup"><span data-stu-id="37891-142">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="37891-143">Használhatja például a [NYISSA meg az Azure virtuális merevlemez segédprogramok](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert a dinamikus lemezek a hello folyamat tooAzure feltöltése során.</span><span class="sxs-lookup"><span data-stu-id="37891-143">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>
> 
> 

* <span data-ttu-id="37891-144">Az egyéni lemezkép alapján létrehozott virtuális gépek kell lennie, hello azonos tárfiók maga hello képként</span><span class="sxs-lookup"><span data-stu-id="37891-144">VMs created from your custom image must reside in hello same storage account as hello image itself</span></span>
  * <span data-ttu-id="37891-145">Hozzon létre egy tárolási fiók és a tároló toohold, az egyéni lemezképet és a létrehozott virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="37891-145">Create a storage account and container toohold both your custom image and created VMs</span></span>
  * <span data-ttu-id="37891-146">Miután létrehozta a virtuális gépek, nyugodtan törölheti a kép</span><span class="sxs-lookup"><span data-stu-id="37891-146">After you have created all your VMs, you can safely delete your image</span></span>

<span data-ttu-id="37891-147">Győződjön meg arról, hogy rendelkezik [hello Azure CLI 1.0](../../cli-install-nodejs.md) jelentkezett be, és a Resource Manager módot használ:</span><span class="sxs-lookup"><span data-stu-id="37891-147">Make sure that you have [hello Azure CLI 1.0](../../cli-install-nodejs.md) logged in and using Resource Manager mode:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="37891-148">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="37891-148">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="37891-149">Példa paraméter nevekre `myResourceGroup`, `mystorageaccount`, és `myimages`.</span><span class="sxs-lookup"><span data-stu-id="37891-149">Example parameter names included `myResourceGroup`, `mystorageaccount`, and `myimages`.</span></span>

<span data-ttu-id="37891-150"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="37891-150"><a id="prepimage"> </a></span></span>

## <a name="prepare-hello-image-toobe-uploaded"></a><span data-ttu-id="37891-151">Készítse elő a hello kép toobe feltöltve.</span><span class="sxs-lookup"><span data-stu-id="37891-151">Prepare hello image toobe uploaded</span></span>
<span data-ttu-id="37891-152">Azure támogatja a különböző Linux terjesztésekről (lásd: [támogatott Disztribúciókkal](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="37891-152">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="37891-153">a következő cikkek hello végigvezetik hogyan tooprepare hello Azure által támogatott különböző Linux-disztribúció:</span><span class="sxs-lookup"><span data-stu-id="37891-153">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure:</span></span>

* <span data-ttu-id="37891-154">**[CentOS-alapú Disztribúciók](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="37891-154">**[CentOS-based Distributions](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="37891-155">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="37891-155">**[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="37891-156">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="37891-156">**[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="37891-157">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="37891-157">**[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="37891-158">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="37891-158">**[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="37891-159">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="37891-159">**[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="37891-160">**[Egyéb - nem támogatott Disztribúciókkal](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="37891-160">**[Other - Non-Endorsed Distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

<span data-ttu-id="37891-161">Lásd még: hello  **[Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes)**  kapcsolatos további általános tippek Linux lemezképek előkészítése az Azure.</span><span class="sxs-lookup"><span data-stu-id="37891-161">Also see hello **[Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="37891-162">Hello [Azure platformon SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) Linux operációs rendszert futtató, csak ha egyik hello által támogatott disztribúciók használt hello konfigurációs adatait a támogatott verziók tooVMs érvényes [az Azure által támogatott Linux Azokat a terjesztéseket](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="37891-162">hello [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies tooVMs running Linux only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>


## <a name="create-a-resource-group"></a><span data-ttu-id="37891-163">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="37891-163">Create a resource group</span></span>
<span data-ttu-id="37891-164">Erőforráscsoportok logikailag összefogni összes hello Azure-erőforrások toosupport a virtuális gépek, például a hello virtuális hálózati és tárolási.</span><span class="sxs-lookup"><span data-stu-id="37891-164">Resource groups logically bring together all hello Azure resources toosupport your virtual machines, such as hello virtual networking and storage.</span></span> <span data-ttu-id="37891-165">Tudjon meg többet az [Azure erőforráscsoportokat Itt](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="37891-165">Read more about [Azure resource groups here](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="37891-166">Feltöltése az egyéni lemezképet, és a virtuális gépek létrehozását, mielőtt először toocreate egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="37891-166">Before uploading your custom disk image and creating VMs, you first need toocreate a resource group.</span></span> 

<span data-ttu-id="37891-167">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `WestUS` helye:</span><span class="sxs-lookup"><span data-stu-id="37891-167">hello following example creates a resource group named `myResourceGroup` in hello `WestUS` location:</span></span>

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a><span data-ttu-id="37891-168">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="37891-168">Create a storage account</span></span>
<span data-ttu-id="37891-169">Virtuális gépek lapblobokat egy tárfiókon belül tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="37891-169">VMs are stored as page blobs within a storage account.</span></span> <span data-ttu-id="37891-170">Tudjon meg többet az [itt az Azure blob storage](../../storage/common/storage-introduction.md#blob-storage).</span><span class="sxs-lookup"><span data-stu-id="37891-170">Read more about [Azure blob storage here](../../storage/common/storage-introduction.md#blob-storage).</span></span> <span data-ttu-id="37891-171">Az egyéni lemezképet, és a virtuális gépek storage-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="37891-171">You create a storage account for your custom disk image and VMs.</span></span> <span data-ttu-id="37891-172">A virtuális gépeket hoz létre az egyéni lemez lemezképet kell toobe a hello ugyanazt a tárfiókot, mint a lemezkép.</span><span class="sxs-lookup"><span data-stu-id="37891-172">Any VMs that you create from your custom disk image need toobe in hello same storage account as that image.</span></span>

<span data-ttu-id="37891-173">hello alábbi példa létrehoz egy nevű tárfiók `mystorageaccount` hello erőforráscsoportban korábban hozott létre:</span><span class="sxs-lookup"><span data-stu-id="37891-173">hello following example creates a storage account named `mystorageaccount` in hello resource group previously created:</span></span>

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a><span data-ttu-id="37891-174">Tárfiókkulcsok listázása</span><span class="sxs-lookup"><span data-stu-id="37891-174">List storage account keys</span></span>
<span data-ttu-id="37891-175">Az Azure létrehoz két 512 bites kulcsot minden tárfiók.</span><span class="sxs-lookup"><span data-stu-id="37891-175">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="37891-176">A hozzáférési kulcsot használnak az toohello tárfiókot, például az írási műveletek toocarry hitelesítésekor.</span><span class="sxs-lookup"><span data-stu-id="37891-176">These access keys are used when authenticating toohello storage account, such as toocarry out write operations.</span></span> <span data-ttu-id="37891-177">Tudjon meg többet az [kezelése hozzáférési toostorage Itt](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="37891-177">Read more about [managing access toostorage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="37891-178">Hello a hívóbetűk megtekintéséhez `azure storage account keys list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="37891-178">You can view access keys with hello `azure storage account keys list` command.</span></span>

<span data-ttu-id="37891-179">Hello elérési kulcsainak megtekintése létrehozott tárfiók hello:</span><span class="sxs-lookup"><span data-stu-id="37891-179">View hello access keys for hello storage account you created:</span></span>

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

<span data-ttu-id="37891-180">hello kimeneti hasonlít:</span><span class="sxs-lookup"><span data-stu-id="37891-180">hello output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
<span data-ttu-id="37891-181">Jegyezze fel a `key1` még szüksége lesz rájuk toointeract a storage-fiók hello következő lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="37891-181">Make a note of `key1` as you will use it toointeract with your storage account in hello next steps.</span></span>

## <a name="create-a-storage-container"></a><span data-ttu-id="37891-182">A tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="37891-182">Create a storage container</span></span>
<span data-ttu-id="37891-183">A hello azonos módon, hogy hozzon létre különböző könyvtárak toologically rendezheti a helyi fájlrendszer, a virtuális lemezek és a képek hozzon létre egy tárolási fiók tooorganize tárolókra.</span><span class="sxs-lookup"><span data-stu-id="37891-183">In hello same way that you create different directories toologically organize your local file system, you create containers within a storage account tooorganize your virtual disks and images.</span></span> <span data-ttu-id="37891-184">A storage-fiók korlátlan számú tárolót tárolhat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="37891-184">A storage account can contain any number of containers.</span></span> 

<span data-ttu-id="37891-185">hello alábbi példa létrehoz egy nevű tárolót `myimages`, hello előző lépésben kapott hello hozzáférési kulcs megadása (`key1`):</span><span class="sxs-lookup"><span data-stu-id="37891-185">hello following example creates a container named `myimages`, specifying hello access key obtained in hello previous step (`key1`):</span></span>

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a><span data-ttu-id="37891-186">Virtuális merevlemez feltöltéséhez</span><span class="sxs-lookup"><span data-stu-id="37891-186">Upload VHD</span></span>
<span data-ttu-id="37891-187">Most ténylegesen feltöltheti az egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="37891-187">Now you can actually upload your custom disk image.</span></span> <span data-ttu-id="37891-188">A virtuális gép által használt összes virtuális lemezek, töltse fel, és tárolja az egyéni lemezképet oldalblobként.</span><span class="sxs-lookup"><span data-stu-id="37891-188">As with all virtual disks used by VMs, you upload and store your custom disk image as a page blob.</span></span>

<span data-ttu-id="37891-189">Adja meg a hozzáférési kulcsot a hello tároló hello előző lépésben létrehozott, és ezután hello az elérési út toohello egyéni lemezképet a helyi számítógépen:</span><span class="sxs-lookup"><span data-stu-id="37891-189">Specify your access key, hello container you created in hello previous step, and then hello path toohello custom disk image on your local computer:</span></span>

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a><span data-ttu-id="37891-190">Egyéni lemezképet a virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="37891-190">Create VM from custom image</span></span>
<span data-ttu-id="37891-191">Amikor virtuális gépeket hoz létre az egyéni lemezképet, adjon meg hello URI toohello lemezképet.</span><span class="sxs-lookup"><span data-stu-id="37891-191">When you create VMs from your custom disk image, specify hello URI toohello disk image.</span></span> <span data-ttu-id="37891-192">Gondoskodjon arról, hogy hello rendeltetési tárolási fiók megfelel az egyéni lemezképet tároló.</span><span class="sxs-lookup"><span data-stu-id="37891-192">Ensure that hello destination storage account matches where your custom disk image is stored.</span></span> <span data-ttu-id="37891-193">A virtuális gép hello Azure CLI vagy erőforrás-kezelő JSON-sablon használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="37891-193">You can create your VM using hello Azure CLI or Resource Manager JSON template.</span></span>

### <a name="create-a-vm-using-hello-azure-cli"></a><span data-ttu-id="37891-194">Hozzon létre egy virtuális Gépet hello Azure parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="37891-194">Create a VM using hello Azure CLI</span></span>
<span data-ttu-id="37891-195">Megadja a hello `--image-urn` hello paraméterrel `azure vm create` parancs toopoint tooyour egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="37891-195">You specify hello `--image-urn` parameter with hello `azure vm create` command toopoint tooyour custom disk image.</span></span> <span data-ttu-id="37891-196">Győződjön meg arról, hogy `--storage-account-name` egyezések hello tárfiók az egyéni lemezképet tárolja.</span><span class="sxs-lookup"><span data-stu-id="37891-196">Ensure that `--storage-account-name` matches hello storage account where your custom disk image is stored.</span></span> <span data-ttu-id="37891-197">Nincs toouse hello ugyanabban a tárolóban, egyéni lemez kép toostore hello a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="37891-197">You do not have toouse hello same container as hello custom disk image toostore your VMs.</span></span> <span data-ttu-id="37891-198">Győződjön meg arról, hogy toocreate további tárolókkal hello azonos módon hello korábbi lépéseket az egyéni lemezképek feltöltés előtt.</span><span class="sxs-lookup"><span data-stu-id="37891-198">Make sure toocreate any additional containers in hello same way as hello earlier steps before uploading your custom disk images.</span></span>

<span data-ttu-id="37891-199">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` származó az egyéni lemezképet:</span><span class="sxs-lookup"><span data-stu-id="37891-199">hello following example creates a VM named `myVM` from your custom disk image:</span></span>

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

<span data-ttu-id="37891-200">Ön továbbra is szükséges toospecify, vagy a választ kér, összes hello hello szükséges további paraméterek `azure vm create` parancs például a virtuális hálózat, a nyilvános IP-cím, a felhasználónevet és az SSH-kulcsok.</span><span class="sxs-lookup"><span data-stu-id="37891-200">You still need toospecify, or answer prompts for, all hello additional parameters required by hello `azure vm create` command such as virtual network, public IP address, username, and SSH keys.</span></span> <span data-ttu-id="37891-201">További információk a hello [érhető el erőforrás-kezelő parancssori paraméterek](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span><span class="sxs-lookup"><span data-stu-id="37891-201">Read more about hello [available CLI Resource Manager parameters](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).</span></span>

### <a name="create-a-vm-using-a-json-template"></a><span data-ttu-id="37891-202">A virtuális gép egy JSON-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="37891-202">Create a VM using a JSON template</span></span>
<span data-ttu-id="37891-203">Az Azure Resource Manager-sablonok a JavaScript Object Notation (JSON) fájlok hello környezet toobuild kívánja.</span><span class="sxs-lookup"><span data-stu-id="37891-203">Azure Resource Manager templates are JavaScript Object Notation (JSON) files that define hello environment you wish toobuild.</span></span> <span data-ttu-id="37891-204">hello sablonok az erőforrás-szolgáltatók toodifferent például számítási és hálózati részletezik.</span><span class="sxs-lookup"><span data-stu-id="37891-204">hello templates are broken down in toodifferent resource providers such as compute or network.</span></span> <span data-ttu-id="37891-205">Meglévő sablonok használhatja, vagy a saját írni.</span><span class="sxs-lookup"><span data-stu-id="37891-205">You can use existing templates or write your own.</span></span> <span data-ttu-id="37891-206">Tudjon meg többet az [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="37891-206">Read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="37891-207">Hello belül `Microsoft.Compute/virtualMachines` a sablon-szolgáltatót, hogy egy `storageProfile` hello konfigurációs adatait a virtuális gép tartalmazó csomópontot.</span><span class="sxs-lookup"><span data-stu-id="37891-207">Within hello `Microsoft.Compute/virtualMachines` provider of your template, you have a `storageProfile` node that contains hello configuration details for your VM.</span></span> <span data-ttu-id="37891-208">hello két fő paraméterek tooedit hello `image` és `vhd` URI-azonosítók, amelyek az új virtuális gép virtuális lemez és tooyour egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="37891-208">hello two main parameters tooedit are hello `image` and `vhd` URIs that point tooyour custom disk image and your new VM's virtual disk.</span></span> <span data-ttu-id="37891-209">hello következő használatához az egyéni lemezképet hello JSON példáját mutatja be:</span><span class="sxs-lookup"><span data-stu-id="37891-209">hello following shows an example of hello JSON for using a custom disk image:</span></span>

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

<span data-ttu-id="37891-210">Használhat [a meglévő sablon toocreate egy virtuális Gépet egy egyéni lemezképből](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) vagy foglalkozó témakört [a saját Azure Resource Manager-sablonok készítése](../../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="37891-210">You can use [this existing template toocreate a VM from a custom image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) or read about [creating your own Azure Resource Manager templates](../../azure-resource-manager/resource-group-authoring-templates.md).</span></span> 

<span data-ttu-id="37891-211">Ha már konfigurált egy sablon, hoz létre a virtuális gépek hello `azure group deployment create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="37891-211">Once you have a template configured, you create your VMs using hello `azure group deployment create` command.</span></span> <span data-ttu-id="37891-212">Adja meg a JSON-sablon URI hello hello `--template-uri` paraméter:</span><span class="sxs-lookup"><span data-stu-id="37891-212">Specify hello URI of your JSON template with hello `--template-uri` parameter:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

<span data-ttu-id="37891-213">Ha egy számítógépen helyben tárolt JSON-fájl, használhatja a hello `--template-file` paraméter helyette:</span><span class="sxs-lookup"><span data-stu-id="37891-213">If you have a JSON file stored locally on your computer, you can use hello `--template-file` parameter instead:</span></span>

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a><span data-ttu-id="37891-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="37891-214">Next steps</span></span>
<span data-ttu-id="37891-215">Miután előkészített és feltöltése az egyéni virtuális lemez, akkor további információ [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="37891-215">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="37891-216">Akkor is érdemes lehet túl[hozzá adatlemezt](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour új virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="37891-216">You may also want too[add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour new VMs.</span></span> <span data-ttu-id="37891-217">Ha a virtuális gépeken futó, hogy kell-e tooaccess alkalmazásai, ne feledje túl[nyisson meg portokat és a végpontok](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="37891-217">If you have applications running on your VMs that you need tooaccess, be sure too[open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

