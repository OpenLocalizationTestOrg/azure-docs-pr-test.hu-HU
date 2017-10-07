---
title: "aaaUpload vagy Azure CLI 2.0 egyéni Linux virtuális gép másolása |} Microsoft Docs"
description: "Töltse fel vagy másolja a testreszabott virtuális gépek hello Resource Manager üzembe helyezési modellben és hello Azure CLI 2.0"
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
ms.date: 07/06/2017
ms.author: cynthn
ms.openlocfilehash: 79af897120a6ba7f4a427ba6c7d0c31b7b870bd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a><span data-ttu-id="d41ea-103">Linux virtuális gép létrehozása az Azure CLI 2.0 hello egyéni lemezről</span><span class="sxs-lookup"><span data-stu-id="d41ea-103">Create a Linux VM from custom disk with hello Azure CLI 2.0</span></span>

<!-- rename toocreate-vm-specialized -->

<span data-ttu-id="d41ea-104">A cikkből megtudhatja, hogyan tooupload testreszabott virtuális merevlemez (VHD), vagy másolja a meglévő VHD az Azure-ban, és hozzon létre új Linux virtuális gépek (VM) hello egyéni lemezről.</span><span class="sxs-lookup"><span data-stu-id="d41ea-104">This article shows you how tooupload a customized virtual hard disk (VHD) or copy a an existing VHD in Azure and create new Linux virtual machines (VMs) from hello custom disk.</span></span> <span data-ttu-id="d41ea-105">Telepítheti és konfigurálhatja a Linux distro tooyour követelményeknek és majd használni az adott VHD tooquickly hozzon létre egy új Azure virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="d41ea-105">You can install and configure a Linux distro tooyour requirements and then use that VHD tooquickly create a new Azure virtual machine.</span></span>

<span data-ttu-id="d41ea-106">Ha azt szeretné toocreate több virtuális gép a testreszabott lemezről, a virtuális gép vagy a virtuális merevlemez egy lemezképet kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="d41ea-106">If you want toocreate multiple VMs from your customized disk, you should create an image from your VM or VHD.</span></span> <span data-ttu-id="d41ea-107">További információkért lásd: [hozzon létre egy Azure virtuális gép hello parancssori felület használatával egyéni lemezkép](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="d41ea-107">For more information, see [Create a custom image of an Azure VM using hello CLI](tutorial-custom-images.md).</span></span>

<span data-ttu-id="d41ea-108">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="d41ea-108">You have two options:</span></span>
* [<span data-ttu-id="d41ea-109">VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="d41ea-109">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="d41ea-110">Egy meglévő Azure virtuális gép másolása</span><span class="sxs-lookup"><span data-stu-id="d41ea-110">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a><span data-ttu-id="d41ea-111">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="d41ea-111">Quick commands</span></span>

<span data-ttu-id="d41ea-112">Segítségével új virtuális gép létrehozásakor [az virtuális gép létrehozása](/cli/azure/vm#create) testreszabott vagy speciális lemezről, **csatolása** hello lemez (--csatolása operációsrendszer-lemez) egy egyéni vagy Piactéri lemezkép megadása helyett (--kép).</span><span class="sxs-lookup"><span data-stu-id="d41ea-112">When creating a new VM using [az vm create](/cli/azure/vm#create) from a customized or specialized disk you **attach** hello disk (--attach-os-disk) instead of specifying a custom or marketplace image (--image).</span></span> <span data-ttu-id="d41ea-113">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* hello felügyelt nevű lemez *myManagedDisk* a testreszabott virtuális merevlemezből létrehozni:</span><span class="sxs-lookup"><span data-stu-id="d41ea-113">hello following example creates a VM named *myVM* using hello managed disk named *myManagedDisk* created from your customized VHD:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a><span data-ttu-id="d41ea-114">Követelmények</span><span class="sxs-lookup"><span data-stu-id="d41ea-114">Requirements</span></span>
<span data-ttu-id="d41ea-115">toocomplete hello lépéseket követve van szüksége:</span><span class="sxs-lookup"><span data-stu-id="d41ea-115">toocomplete hello following steps, you need:</span></span>

* <span data-ttu-id="d41ea-116">Linux virtuális gépek, amelyek az Azure-ban történő használatra készült.</span><span class="sxs-lookup"><span data-stu-id="d41ea-116">A Linux virtual machine that has been prepared for use in Azure.</span></span> <span data-ttu-id="d41ea-117">Hello [Prepare hello VM](#prepare-the-vm) című szakaszát ismerteti, hogyan toofind distro adott telepítéséről információt hello Azure Linux ügynök (waagent) hello VM toowork megfelelően az Azure-ban és az Ön toobe képes tooconnect szükséges tooit SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="d41ea-117">hello [Prepare hello VM](#prepare-the-vm) section of this article covers how toofind distro specific information on installing hello Azure Linux Agent (waagent) which is required for hello VM toowork properly in Azure and for you toobe able tooconnect tooit using SSH.</span></span>
* <span data-ttu-id="d41ea-118">a meglévő VHD-fájl hello [Azure által támogatott Linux-disztribúció](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (vagy lásd: [nem támogatott disztribúciókkal kapcsolatos információi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuális lemez hello VHD-formátumban.</span><span class="sxs-lookup"><span data-stu-id="d41ea-118">hello VHD file from an existing [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="d41ea-119">Több különféle eszköz toocreate érhetők el a virtuális gép és a virtuális merevlemez:</span><span class="sxs-lookup"><span data-stu-id="d41ea-119">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="d41ea-120">Telepítse és konfigurálja [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve toouse a lemezkép formátumú virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="d41ea-120">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="d41ea-121">Ha szükséges, akkor [lemezkép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) használatával **qemu-img átalakítása**.</span><span class="sxs-lookup"><span data-stu-id="d41ea-121">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using **qemu-img convert**.</span></span>
  * <span data-ttu-id="d41ea-122">Is használhatja a Hyper-V [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) vagy [Windows Server 2012 vagy 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="d41ea-122">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="d41ea-123">hello újabb VHDX formátum nem támogatott az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d41ea-123">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="d41ea-124">Amikor létrehoz egy virtuális Gépet, adja meg a virtuális merevlemez hello formátumban.</span><span class="sxs-lookup"><span data-stu-id="d41ea-124">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="d41ea-125">Ha szükséges, VHDX-lemezek tooVHD használatával konvertálhatja [qemu-img átalakítása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy hello [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="d41ea-125">If needed, you can convert VHDX disks tooVHD using [qemu-img convert](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="d41ea-126">További Azure nem támogatja dinamikus virtuális merevlemezek, feltöltése, ezért meg kell tooconvert ilyen lemezek toostatic VHD-k feltöltés előtt.</span><span class="sxs-lookup"><span data-stu-id="d41ea-126">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="d41ea-127">Használhatja például a [NYISSA meg az Azure virtuális merevlemez segédprogramok](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert a dinamikus lemezek a hello folyamat tooAzure feltöltése során.</span><span class="sxs-lookup"><span data-stu-id="d41ea-127">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>
> 
> 


* <span data-ttu-id="d41ea-128">Győződjön meg arról, hogy rendelkezik hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="d41ea-128">Make sure that you have hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="d41ea-129">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="d41ea-129">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="d41ea-130">Példa paraméter nevekre *myResourceGroup*, *mystorageaccount*, és *mydisks*.</span><span class="sxs-lookup"><span data-stu-id="d41ea-130">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *mydisks*.</span></span>

<span data-ttu-id="d41ea-131"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="d41ea-131"><a id="prepimage"> </a></span></span>

## <a name="prepare-hello-vm"></a><span data-ttu-id="d41ea-132">Hello virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="d41ea-132">Prepare hello VM</span></span>

<span data-ttu-id="d41ea-133">Azure támogatja a különböző Linux terjesztésekről (lásd: [támogatott Disztribúciókkal](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="d41ea-133">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="d41ea-134">a következő cikkek hello végigvezetik hogyan tooprepare hello Azure által támogatott különböző Linux-disztribúció:</span><span class="sxs-lookup"><span data-stu-id="d41ea-134">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure:</span></span>

* [<span data-ttu-id="d41ea-135">CentOS-alapú Disztribúciók</span><span class="sxs-lookup"><span data-stu-id="d41ea-135">CentOS-based Distributions</span></span>](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="d41ea-136">Debian Linux</span><span class="sxs-lookup"><span data-stu-id="d41ea-136">Debian Linux</span></span>](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="d41ea-137">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="d41ea-137">Oracle Linux</span></span>](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="d41ea-138">Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="d41ea-138">Red Hat Enterprise Linux</span></span>](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="d41ea-139">SLES & openSUSE</span><span class="sxs-lookup"><span data-stu-id="d41ea-139">SLES & openSUSE</span></span>](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="d41ea-140">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="d41ea-140">Ubuntu</span></span>](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="d41ea-141">Egyéb - nem támogatott Disztribúciókkal</span><span class="sxs-lookup"><span data-stu-id="d41ea-141">Other - Non-Endorsed Distributions</span></span>](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

<span data-ttu-id="d41ea-142">Lásd még: hello [Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további általános tippek Linux lemezképek előkészítése az Azure.</span><span class="sxs-lookup"><span data-stu-id="d41ea-142">Also see hello [Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="d41ea-143">Hello [Azure platformon SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) Linux operációs rendszert futtató, csak ha egyik hello által támogatott disztribúciók használt hello konfigurációs adatait a támogatott verziók tooVMs érvényes [az Azure által támogatott Linux Azokat a terjesztéseket](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d41ea-143">hello [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies tooVMs running Linux only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="option-1-upload-a-vhd"></a><span data-ttu-id="d41ea-144">1. lehetőség: A virtuális merevlemez feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="d41ea-144">Option 1: Upload a VHD</span></span>

<span data-ttu-id="d41ea-145">Testre szabott virtuális Merevlemezt, amely a helyi számítógépen futó vagy egy másik felhőből exportált feltölthet.</span><span class="sxs-lookup"><span data-stu-id="d41ea-145">You can upload a customized VHD that you have running on a local machine or that you exported from another cloud.</span></span> <span data-ttu-id="d41ea-146">toouse hello VHD toocreate egy új Azure virtuális Gépet, kell tooupload hello VHD tooa tárolási fiókját, és hozhat létre egy felügyelt lemezt hello VHD-t.</span><span class="sxs-lookup"><span data-stu-id="d41ea-146">toouse hello VHD toocreate a new Azure VM, you need tooupload hello VHD tooa storage account and create a managed disk from hello VHD.</span></span> 

### <a name="create-a-resource-group"></a><span data-ttu-id="d41ea-147">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="d41ea-147">Create a resource group</span></span>

<span data-ttu-id="d41ea-148">Feltöltése az egyéni lemez és virtuális gépek létrehozását, mielőtt először toocreate nevű erőforráscsoport [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="d41ea-148">Before uploading your custom disk and creating VMs, you first need toocreate a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="d41ea-149">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helye: [Azure felügyelt lemezekhez – áttekintés](../windows/managed-disks-overview.md)</span><span class="sxs-lookup"><span data-stu-id="d41ea-149">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location: [Azure Managed Disks overview](../windows/managed-disks-overview.md)</span></span>
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a><span data-ttu-id="d41ea-150">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="d41ea-150">Create a storage account</span></span>

<span data-ttu-id="d41ea-151">Hozzon létre egy tárfiókot, az egyéni lemez és virtuális gépek [az storage-fiók létrehozása](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="d41ea-151">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> 

<span data-ttu-id="d41ea-152">hello alábbi példa létrehoz egy nevű tárfiók *mystorageaccount* hello erőforráscsoportban korábban hozott létre:</span><span class="sxs-lookup"><span data-stu-id="d41ea-152">hello following example creates a storage account named *mystorageaccount* in hello resource group previously created:</span></span>

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a><span data-ttu-id="d41ea-153">Tárfiókkulcsok listázása</span><span class="sxs-lookup"><span data-stu-id="d41ea-153">List storage account keys</span></span>
<span data-ttu-id="d41ea-154">Az Azure létrehoz két 512 bites kulcsot minden tárfiók.</span><span class="sxs-lookup"><span data-stu-id="d41ea-154">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="d41ea-155">A hozzáférési kulcsot használnak az toohello tárfiókot, például az írási műveletek elvégzése hitelesítésekor.</span><span class="sxs-lookup"><span data-stu-id="d41ea-155">These access keys are used when authenticating toohello storage account, like carrying out write operations.</span></span> <span data-ttu-id="d41ea-156">Tudjon meg többet az [kezelése hozzáférési toostorage Itt](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="d41ea-156">Read more about [managing access toostorage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="d41ea-157">Megtekintheti a hello hívóbetűk [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="d41ea-157">You view hello access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="d41ea-158">Hello elérési kulcsainak megtekintése létrehozott tárfiók hello:</span><span class="sxs-lookup"><span data-stu-id="d41ea-158">View hello access keys for hello storage account you created:</span></span>

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

<span data-ttu-id="d41ea-159">hello kimeneti hasonlít:</span><span class="sxs-lookup"><span data-stu-id="d41ea-159">hello output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="d41ea-160">Jegyezze fel a **key1** még szüksége lesz rájuk toointeract a storage-fiók hello következő lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="d41ea-160">Make a note of **key1** as you will use it toointeract with your storage account in hello next steps.</span></span>

### <a name="create-a-storage-container"></a><span data-ttu-id="d41ea-161">A tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="d41ea-161">Create a storage container</span></span>
<span data-ttu-id="d41ea-162">A hello azonos módon, hogy hozzon létre különböző könyvtárak toologically rendezheti a helyi fájlrendszer, a lemezek létrehozni a tárolási fiók tooorganize tárolókra.</span><span class="sxs-lookup"><span data-stu-id="d41ea-162">In hello same way that you create different directories toologically organize your local file system, you create containers within a storage account tooorganize your disks.</span></span> <span data-ttu-id="d41ea-163">A storage-fiók korlátlan számú tárolót tárolhat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="d41ea-163">A storage account can contain any number of containers.</span></span> <span data-ttu-id="d41ea-164">Hozzon létre egy tárolót a [az tároló létrehozása](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="d41ea-164">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="d41ea-165">hello alábbi példa létrehoz egy nevű tárolót *mydisks*:</span><span class="sxs-lookup"><span data-stu-id="d41ea-165">hello following example creates a container named *mydisks*:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-hello-vhd"></a><span data-ttu-id="d41ea-166">Hello VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="d41ea-166">Upload hello VHD</span></span>
<span data-ttu-id="d41ea-167">Töltsön fel az egyéni lemezzel [az tárolási blob feltöltése](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="d41ea-167">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="d41ea-168">Töltse fel, és tárolja az egyéni lemezt oldalblobként.</span><span class="sxs-lookup"><span data-stu-id="d41ea-168">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="d41ea-169">Adja meg a hozzáférési kulcsot a hello tároló hello előző lépésben létrehozott, és ezután hello az elérési út toohello egyéni lemezt a helyi számítógépen:</span><span class="sxs-lookup"><span data-stu-id="d41ea-169">Specify your access key, hello container you created in hello previous step, and then hello path toohello custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
<span data-ttu-id="d41ea-170">VHD feltöltése hello eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="d41ea-170">Uploading hello VHD may take a while.</span></span>

### <a name="create-a-managed-disk"></a><span data-ttu-id="d41ea-171">Kezelt lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="d41ea-171">Create a managed disk</span></span>


<span data-ttu-id="d41ea-172">Hozzon létre egy felügyelt lemezes hello virtuális merevlemez használatával [az lemez létrehozása](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="d41ea-172">Create a managed disk from hello VHD using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="d41ea-173">hello alábbi példa létrehoz egy nevű felügyelt lemezes *myManagedDisk* hello VHD-t a tárfiók és tároló nevű tooyour feltöltött:</span><span class="sxs-lookup"><span data-stu-id="d41ea-173">hello following example creates a managed disk named *myManagedDisk* from hello VHD you uploaded tooyour named storage account and container:</span></span>

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a><span data-ttu-id="d41ea-174">2. lehetőség: Egy meglévő virtuális gép másolása</span><span class="sxs-lookup"><span data-stu-id="d41ea-174">Option 2: Copy an existing VM</span></span>

<span data-ttu-id="d41ea-175">Is létrehozhat a Azure, majd az operációs rendszer hello lemezmásolás testreszabott VM hello és tooa új virtuális gép toocreate csatlakoztatása egy másik példányt.</span><span class="sxs-lookup"><span data-stu-id="d41ea-175">You can also create hello customized VM in Azure and then copy hello OS disk and attach it tooa new VM toocreate another copy.</span></span> <span data-ttu-id="d41ea-176">Ez ideális tesztelési, de ha egy meglévő Azure virtuális gép toouse hello modell több új virtuális gépek, valóban készítsen egy **kép** helyette.</span><span class="sxs-lookup"><span data-stu-id="d41ea-176">This is fine for testing, but if you want toouse an existing Azure VM as hello model for multiple new VMs, you really should create an **image** instead.</span></span> <span data-ttu-id="d41ea-177">További információ a lemezkép létrehozása egy meglévő Azure virtuális gépről: [létrehozhat egyéni rendszerképeket egy Azure virtuális gép hello parancssori felület használatával](tutorial-custom-images.md)</span><span class="sxs-lookup"><span data-stu-id="d41ea-177">For more information about creating an image from an existing Azure VM, see [Create a custom image of an Azure VM using hello CLI](tutorial-custom-images.md)</span></span>

### <a name="create-a-snapshot"></a><span data-ttu-id="d41ea-178">Pillanatkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d41ea-178">Create a snapshot</span></span>

<span data-ttu-id="d41ea-179">Ez a példa pillanatképet hoz létre a virtuális gépek nevű *myVM* erőforráscsoportban *myResourceGroup* nevű pillanatképet készít, és *osDiskSnapshot*.</span><span class="sxs-lookup"><span data-stu-id="d41ea-179">This example creates a snapshot of a VM named *myVM* in resource group *myResourceGroup* and creates a snapshot named *osDiskSnapshot*.</span></span>

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-hello-managed-disk"></a><span data-ttu-id="d41ea-180">Hello kezelt lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="d41ea-180">Create hello managed disk</span></span>

<span data-ttu-id="d41ea-181">Hozzon létre egy új felügyelt lemezes hello pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="d41ea-181">Create a new managed disk from hello snapshot.</span></span>

<span data-ttu-id="d41ea-182">Hello pillanatkép hello azonosító beszerzése.</span><span class="sxs-lookup"><span data-stu-id="d41ea-182">Get hello ID of hello snapshot.</span></span> <span data-ttu-id="d41ea-183">Ebben a példában hello pillanatkép nevű *osDiskSnapshot* és hello *myResourceGroup* erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d41ea-183">In this example, hello snapshot is named *osDiskSnapshot* and it is in hello *myResourceGroup* resource group.</span></span>

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

<span data-ttu-id="d41ea-184">Hello kezelt lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d41ea-184">Create hello managed disk.</span></span> <span data-ttu-id="d41ea-185">Ebben a példában létrehozunk egy felügyelt lemezes nevű *myManagedDisk* a pillanatképből, amely 128 GB-nál standard szintű tárolót.</span><span class="sxs-lookup"><span data-stu-id="d41ea-185">In this example, we will create a managed disk named *myManagedDisk* from our snapshot, that is 128GB in size in standard storage.</span></span>

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-hello-vm"></a><span data-ttu-id="d41ea-186">Hello virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="d41ea-186">Create hello VM</span></span>

<span data-ttu-id="d41ea-187">Hozza létre a virtuális Gépet a [az virtuális gép létrehozása](/cli/azure/vm#create) , és csatlakoztassa (--csatolása operációsrendszer-lemez) hello felügyelt hello az operációs rendszer lemezeként lemez.</span><span class="sxs-lookup"><span data-stu-id="d41ea-187">Now, create your VM with [az vm create](/cli/azure/vm#create) and attach (--attach-os-disk) hello managed disk as hello OS disk.</span></span> <span data-ttu-id="d41ea-188">hello alábbi példakód létrehozza a virtuális gépek nevű *myNewVM* hello felügyelt a feltöltött virtuális merevlemez alapján létrehozott lemez:</span><span class="sxs-lookup"><span data-stu-id="d41ea-188">hello following example creates a VM named *myNewVM* using hello managed disk created from your uploaded VHD:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

<span data-ttu-id="d41ea-189">A virtuális gép hello képes tooSSH kell hello forrás virtuális gép hello hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="d41ea-189">You should be able tooSSH into hello VM using hello credentials from hello source VM.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d41ea-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d41ea-190">Next steps</span></span>
<span data-ttu-id="d41ea-191">Miután előkészített és feltöltése az egyéni virtuális lemez, akkor további információ [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d41ea-191">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="d41ea-192">Akkor is érdemes lehet túl[hozzá adatlemezt](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour új virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="d41ea-192">You may also want too[add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour new VMs.</span></span> <span data-ttu-id="d41ea-193">Ha a virtuális gépeken futó, hogy kell-e tooaccess alkalmazásai, ne feledje túl[nyisson meg portokat és a végpontok](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d41ea-193">If you have applications running on your VMs that you need tooaccess, be sure too[open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

