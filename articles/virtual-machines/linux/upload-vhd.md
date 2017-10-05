---
title: "Töltse fel, vagy az Azure CLI 2.0 egyéni Linux virtuális gép másolása |} Microsoft Docs"
description: "Töltse fel vagy másolja a Resource Manager üzembe helyezési modellel és az Azure CLI 2.0 testreszabott virtuális gép"
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
ms.openlocfilehash: 7c297725c26ea6c44403a10ecdcc3542f89f10b4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a><span data-ttu-id="5c2ef-103">Linux virtuális gép létrehozása az Azure CLI 2.0 egyéni lemezről</span><span class="sxs-lookup"><span data-stu-id="5c2ef-103">Create a Linux VM from custom disk with the Azure CLI 2.0</span></span>

<!-- rename to create-vm-specialized -->

<span data-ttu-id="5c2ef-104">A cikkből megtudhatja, hogyan tölthet fel egy testreszabott virtuális merevlemez (VHD) vagy a példány egy meglévő VHD az Azure-ban, és hozzon létre új Linux virtuális gépek (VM) a egyéni lemezről.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-104">This article shows you how to upload a customized virtual hard disk (VHD) or copy a an existing VHD in Azure and create new Linux virtual machines (VMs) from the custom disk.</span></span> <span data-ttu-id="5c2ef-105">Telepítése és konfigurálása a Linux distro az igényeinek megfelelően, valamint, hogy a virtuális merevlemez használatával gyorsan hozzon létre egy új Azure virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-105">You can install and configure a Linux distro to your requirements and then use that VHD to quickly create a new Azure virtual machine.</span></span>

<span data-ttu-id="5c2ef-106">Ha azt szeretné, a testre szabott lemezről több virtuális gép létrehozásához, egy lemezképet kell létrehoznia a virtuális gép vagy a virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-106">If you want to create multiple VMs from your customized disk, you should create an image from your VM or VHD.</span></span> <span data-ttu-id="5c2ef-107">További információkért lásd: [hozzon létre egy Azure virtuális gép a parancssori felület használatával egyéni lemezkép](tutorial-custom-images.md).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-107">For more information, see [Create a custom image of an Azure VM using the CLI](tutorial-custom-images.md).</span></span>

<span data-ttu-id="5c2ef-108">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-108">You have two options:</span></span>
* [<span data-ttu-id="5c2ef-109">VHD feltöltése</span><span class="sxs-lookup"><span data-stu-id="5c2ef-109">Upload a VHD</span></span>](#option-1-upload-a-specialized-vhd)
* [<span data-ttu-id="5c2ef-110">Egy meglévő Azure virtuális gép másolása</span><span class="sxs-lookup"><span data-stu-id="5c2ef-110">Copy an existing Azure VM</span></span>](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a><span data-ttu-id="5c2ef-111">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="5c2ef-111">Quick commands</span></span>

<span data-ttu-id="5c2ef-112">Segítségével új virtuális gép létrehozásakor [az virtuális gép létrehozása](/cli/azure/vm#create) testreszabott vagy speciális lemezről, **csatolása** a lemez (--csatolása operációsrendszer-lemez) egy egyéni vagy Piactéri lemezkép megadása helyett (--kép).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-112">When creating a new VM using [az vm create](/cli/azure/vm#create) from a customized or specialized disk you **attach** the disk (--attach-os-disk) instead of specifying a custom or marketplace image (--image).</span></span> <span data-ttu-id="5c2ef-113">Az alábbi példakód létrehozza a virtuális gépek nevű *myVM* nevű felügyelt lemezzel *myManagedDisk* a testreszabott virtuális merevlemezből létrehozni:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-113">The following example creates a VM named *myVM* using the managed disk named *myManagedDisk* created from your customized VHD:</span></span>

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a><span data-ttu-id="5c2ef-114">Követelmények</span><span class="sxs-lookup"><span data-stu-id="5c2ef-114">Requirements</span></span>
<span data-ttu-id="5c2ef-115">A következő lépések elvégzéséhez szüksége:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-115">To complete the following steps, you need:</span></span>

* <span data-ttu-id="5c2ef-116">Linux virtuális gépek, amelyek az Azure-ban történő használatra készült.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-116">A Linux virtual machine that has been prepared for use in Azure.</span></span> <span data-ttu-id="5c2ef-117">A [készítse elő a virtuális gép](#prepare-the-vm) szakasz Ez a cikk bemutatja, hogyan adhat distro konkrét információk a telepítése az Azure Linux ügynök (waagent), amely pedig szükséges, a virtuális gép működéséhez az Azure-ban, és meg kell kapcsolódnia kell az SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-117">The [Prepare the VM](#prepare-the-vm) section of this article covers how to find distro specific information on installing the Azure Linux Agent (waagent) which is required for the VM to work properly in Azure and for you to be able to connect to it using SSH.</span></span>
* <span data-ttu-id="5c2ef-118">A meglévő VHD-fájl [Azure által támogatott Linux-disztribúció](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (vagy lásd: [nem támogatott disztribúciókkal kapcsolatos információi](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) a VHD formátumú virtuális lemezre.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-118">The VHD file from an existing [Azure-endorsed Linux distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) to a virtual disk in the VHD format.</span></span> <span data-ttu-id="5c2ef-119">Több különféle eszköz létezik a virtuális gép és a virtuális merevlemez létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-119">Multiple tools exist to create a VM and VHD:</span></span>
  * <span data-ttu-id="5c2ef-120">Telepítse és konfigurálja [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve arra, hogy a virtuális merevlemez használata a képformátum.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-120">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care to use VHD as your image format.</span></span> <span data-ttu-id="5c2ef-121">Ha szükséges, akkor [lemezkép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) használatával **qemu-img átalakítása**.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-121">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using **qemu-img convert**.</span></span>
  * <span data-ttu-id="5c2ef-122">Is használhatja a Hyper-V [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) vagy [Windows Server 2012 vagy 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-122">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="5c2ef-123">Az újabb VHDX formátum nem támogatott az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-123">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="5c2ef-124">Amikor létrehoz egy virtuális Gépet, adja meg a VHD formátumban.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-124">When you create a VM, specify VHD as the format.</span></span> <span data-ttu-id="5c2ef-125">Szükség esetén, VHDX-lemezek konvertálása virtuális merevlemez használatával [qemu-img átalakítása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy a [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-125">If needed, you can convert VHDX disks to VHD using [qemu-img convert](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or the [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="5c2ef-126">További Azure nem támogatja dinamikus virtuális merevlemezek, feltöltése, ezért ilyen lemezek konvertálása statikus virtuális merevlemezek feltöltés előtt meg kell.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-126">Further, Azure does not support uploading dynamic VHDs, so you need to convert such disks to static VHDs before uploading.</span></span> <span data-ttu-id="5c2ef-127">Használhatja például a [NYISSA meg az Azure virtuális merevlemez segédprogramok](https://github.com/Microsoft/azure-vhd-utils-for-go) átalakítani a dinamikus lemezek Azure feltöltése során.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-127">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) to convert dynamic disks during the process of uploading to Azure.</span></span>
> 
> 


* <span data-ttu-id="5c2ef-128">Győződjön meg arról, hogy rendelkezik-e a legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett az Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-128">Make sure that you have the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="5c2ef-129">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-129">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="5c2ef-130">Példa paraméter nevekre *myResourceGroup*, *mystorageaccount*, és *mydisks*.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-130">Example parameter names included *myResourceGroup*, *mystorageaccount*, and *mydisks*.</span></span>

<span data-ttu-id="5c2ef-131"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="5c2ef-131"><a id="prepimage"> </a></span></span>

## <a name="prepare-the-vm"></a><span data-ttu-id="5c2ef-132">A virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="5c2ef-132">Prepare the VM</span></span>

<span data-ttu-id="5c2ef-133">Azure támogatja a különböző Linux terjesztésekről (lásd: [támogatott Disztribúciókkal](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-133">Azure supports various Linux distributions (see [Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="5c2ef-134">A következő cikkekben végigvezeti Önt a különböző Linux terjesztésekről Azure által támogatott előkészítése:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-134">The following articles guide you through how to prepare the various Linux distributions that are supported on Azure:</span></span>

* [<span data-ttu-id="5c2ef-135">CentOS-alapú Disztribúciók</span><span class="sxs-lookup"><span data-stu-id="5c2ef-135">CentOS-based Distributions</span></span>](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5c2ef-136">Debian Linux</span><span class="sxs-lookup"><span data-stu-id="5c2ef-136">Debian Linux</span></span>](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5c2ef-137">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="5c2ef-137">Oracle Linux</span></span>](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5c2ef-138">Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="5c2ef-138">Red Hat Enterprise Linux</span></span>](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5c2ef-139">SLES & openSUSE</span><span class="sxs-lookup"><span data-stu-id="5c2ef-139">SLES & openSUSE</span></span>](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5c2ef-140">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5c2ef-140">Ubuntu</span></span>](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="5c2ef-141">Egyéb - nem támogatott Disztribúciókkal</span><span class="sxs-lookup"><span data-stu-id="5c2ef-141">Other - Non-Endorsed Distributions</span></span>](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

<span data-ttu-id="5c2ef-142">Lásd még: a [Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további általános tippek Linux lemezképek előkészítése az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-142">Also see the [Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more general tips on preparing Linux images for Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="5c2ef-143">A [Azure platformon SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) csak akkor, ha a konfigurációs részleteket a támogatott verziók használt egyik a hitelesített terjesztéseket Linuxot futtató virtuális gépek érvényes [az Azure által támogatott Linux Azokat a terjesztéseket](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-143">The [Azure platform SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) applies to VMs running Linux only when one of the endorsed distributions is used with the configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
> 

## <a name="option-1-upload-a-vhd"></a><span data-ttu-id="5c2ef-144">1. lehetőség: A virtuális merevlemez feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-144">Option 1: Upload a VHD</span></span>

<span data-ttu-id="5c2ef-145">Testre szabott virtuális Merevlemezt, amely a helyi számítógépen futó vagy egy másik felhőből exportált feltölthet.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-145">You can upload a customized VHD that you have running on a local machine or that you exported from another cloud.</span></span> <span data-ttu-id="5c2ef-146">A virtuális merevlemez használatával hozzon létre egy új Azure virtuális Gépet, akkor kell a VHD-fájlt feltölti egy tárfiókot, és hozzon létre egy felügyelt lemezes virtuális merevlemezről.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-146">To use the VHD to create a new Azure VM, you need to upload the VHD to a storage account and create a managed disk from the VHD.</span></span> 

### <a name="create-a-resource-group"></a><span data-ttu-id="5c2ef-147">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="5c2ef-147">Create a resource group</span></span>

<span data-ttu-id="5c2ef-148">Feltöltése az egyéni lemez és virtuális gépek létrehozását, mielőtt először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-148">Before uploading your custom disk and creating VMs, you first need to create a resource group with [az group create](/cli/azure/group#create).</span></span>

<span data-ttu-id="5c2ef-149">Az alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a a *eastus* helye: [Azure felügyelt lemezekhez – áttekintés](../windows/managed-disks-overview.md)</span><span class="sxs-lookup"><span data-stu-id="5c2ef-149">The following example creates a resource group named *myResourceGroup* in the *eastus* location: [Azure Managed Disks overview](../windows/managed-disks-overview.md)</span></span>
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a><span data-ttu-id="5c2ef-150">Create a storage account</span><span class="sxs-lookup"><span data-stu-id="5c2ef-150">Create a storage account</span></span>

<span data-ttu-id="5c2ef-151">Hozzon létre egy tárfiókot, az egyéni lemez és virtuális gépek [az storage-fiók létrehozása](/cli/azure/storage/account#create).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-151">Create a storage account for your custom disk and VMs with [az storage account create](/cli/azure/storage/account#create).</span></span> 

<span data-ttu-id="5c2ef-152">Az alábbi példa létrehoz egy nevű tárfiók *mystorageaccount* a korábban létrehozott erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-152">The following example creates a storage account named *mystorageaccount* in the resource group previously created:</span></span>

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a><span data-ttu-id="5c2ef-153">Tárfiókkulcsok listázása</span><span class="sxs-lookup"><span data-stu-id="5c2ef-153">List storage account keys</span></span>
<span data-ttu-id="5c2ef-154">Az Azure létrehoz két 512 bites kulcsot minden tárfiók.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-154">Azure generates two 512-bit access keys for each storage account.</span></span> <span data-ttu-id="5c2ef-155">A hozzáférési kulcsot használnak az hitelesítésekor, a tárfiók, például az írási műveletek elvégzésére.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-155">These access keys are used when authenticating to the storage account, like carrying out write operations.</span></span> <span data-ttu-id="5c2ef-156">Tudjon meg többet az [tárolási itt történő hozzáférés felügyeletéhez](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-156">Read more about [managing access to storage here](../../storage/common/storage-create-storage-account.md#manage-your-storage-account).</span></span> <span data-ttu-id="5c2ef-157">Megtekintheti a tárelérési kulcsokat a [az tárolási fióklista kulcsok](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-157">You view the access keys with [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span>

<span data-ttu-id="5c2ef-158">A létrehozott tárfiók hozzáférési kulcsainak megtekintése:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-158">View the access keys for the storage account you created:</span></span>

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

<span data-ttu-id="5c2ef-159">Az eredmény az alábbihoz hasonlóan fog kinézni:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-159">The output is similar to:</span></span>

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
<span data-ttu-id="5c2ef-160">Jegyezze fel a **key1** , amellyel kommunikálni tud a storage-fiókot a következő lépéseket fogja használni.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-160">Make a note of **key1** as you will use it to interact with your storage account in the next steps.</span></span>

### <a name="create-a-storage-container"></a><span data-ttu-id="5c2ef-161">A tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c2ef-161">Create a storage container</span></span>
<span data-ttu-id="5c2ef-162">Az Ön által létrehozott különböző könyvtárak, hogy logikusan rendszerezhesse az a helyi fájlrendszer ugyanúgy hozzon létre egy tárfiókot, a lemezek rendszerezéséhez tárolókra.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-162">In the same way that you create different directories to logically organize your local file system, you create containers within a storage account to organize your disks.</span></span> <span data-ttu-id="5c2ef-163">A storage-fiók korlátlan számú tárolót tárolhat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-163">A storage account can contain any number of containers.</span></span> <span data-ttu-id="5c2ef-164">Hozzon létre egy tárolót a [az tároló létrehozása](/cli/azure/storage/container#create).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-164">Create a container with [az storage container create](/cli/azure/storage/container#create).</span></span>

<span data-ttu-id="5c2ef-165">Az alábbi példa létrehoz egy nevű tárolót *mydisks*:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-165">The following example creates a container named *mydisks*:</span></span>

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-the-vhd"></a><span data-ttu-id="5c2ef-166">A virtuális merevlemez feltöltéséhez</span><span class="sxs-lookup"><span data-stu-id="5c2ef-166">Upload the VHD</span></span>
<span data-ttu-id="5c2ef-167">Töltsön fel az egyéni lemezzel [az tárolási blob feltöltése](/cli/azure/storage/blob#upload).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-167">Now upload your custom disk with [az storage blob upload](/cli/azure/storage/blob#upload).</span></span> <span data-ttu-id="5c2ef-168">Töltse fel, és tárolja az egyéni lemezt oldalblobként.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-168">You upload and store your custom disk as a page blob.</span></span>

<span data-ttu-id="5c2ef-169">Adja meg a hozzáférési kulcsot a tároló létrehozott az előző lépést, és az egyéni lemez elérési útja a helyi számítógépen:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-169">Specify your access key, the container you created in the previous step, and then the path to the custom disk on your local computer:</span></span>

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
<span data-ttu-id="5c2ef-170">A virtuális merevlemez feltöltése eltarthat egy ideig.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-170">Uploading the VHD may take a while.</span></span>

### <a name="create-a-managed-disk"></a><span data-ttu-id="5c2ef-171">Kezelt lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c2ef-171">Create a managed disk</span></span>


<span data-ttu-id="5c2ef-172">Hozzon létre egy felügyelt lemezes a VHD-t a [az lemez létrehozása](/cli/azure/disk#create).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-172">Create a managed disk from the VHD using [az disk create](/cli/azure/disk#create).</span></span> <span data-ttu-id="5c2ef-173">Az alábbi példakód létrehozza nevű felügyelt lemezes *myManagedDisk* feltöltött a nevű tárfiók és tároló virtuális merevlemezről:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-173">The following example creates a managed disk named *myManagedDisk* from the VHD you uploaded to your named storage account and container:</span></span>

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a><span data-ttu-id="5c2ef-174">2. lehetőség: Egy meglévő virtuális gép másolása</span><span class="sxs-lookup"><span data-stu-id="5c2ef-174">Option 2: Copy an existing VM</span></span>

<span data-ttu-id="5c2ef-175">Is a testre szabott virtuális gép létrehozása az Azure-ban és majd másolja az operációsrendszer-lemezképet és mellékelje egy új virtuális Gépet egy másik példányt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-175">You can also create the customized VM in Azure and then copy the OS disk and attach it to a new VM to create another copy.</span></span> <span data-ttu-id="5c2ef-176">Ennek megfelelően működik tesztelési, de ha egy meglévő Azure virtuális gép használata a modell több új virtuális gépek, valóban készítsen egy **kép** helyette.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-176">This is fine for testing, but if you want to use an existing Azure VM as the model for multiple new VMs, you really should create an **image** instead.</span></span> <span data-ttu-id="5c2ef-177">További információ a lemezkép létrehozása egy meglévő Azure virtuális gépről: [hozzon létre egy egyéni rendszerképet az Azure virtuális gép a parancssori felület használatával](tutorial-custom-images.md)</span><span class="sxs-lookup"><span data-stu-id="5c2ef-177">For more information about creating an image from an existing Azure VM, see [Create a custom image of an Azure VM using the CLI](tutorial-custom-images.md)</span></span>

### <a name="create-a-snapshot"></a><span data-ttu-id="5c2ef-178">Pillanatkép létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c2ef-178">Create a snapshot</span></span>

<span data-ttu-id="5c2ef-179">Ez a példa pillanatképet hoz létre a virtuális gépek nevű *myVM* erőforráscsoportban *myResourceGroup* nevű pillanatképet készít, és *osDiskSnapshot*.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-179">This example creates a snapshot of a VM named *myVM* in resource group *myResourceGroup* and creates a snapshot named *osDiskSnapshot*.</span></span>

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-the-managed-disk"></a><span data-ttu-id="5c2ef-180">A kezelt lemez létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c2ef-180">Create the managed disk</span></span>

<span data-ttu-id="5c2ef-181">Hozzon létre egy új felügyelt lemezes a pillanatkép.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-181">Create a new managed disk from the snapshot.</span></span>

<span data-ttu-id="5c2ef-182">A pillanatkép azonosítója beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-182">Get the ID of the snapshot.</span></span> <span data-ttu-id="5c2ef-183">Ebben a példában a pillanatkép neve *osDiskSnapshot* és az a *myResourceGroup* erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-183">In this example, the snapshot is named *osDiskSnapshot* and it is in the *myResourceGroup* resource group.</span></span>

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

<span data-ttu-id="5c2ef-184">A kezelt lemez létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-184">Create the managed disk.</span></span> <span data-ttu-id="5c2ef-185">Ebben a példában létrehozunk egy felügyelt lemezes nevű *myManagedDisk* a pillanatképből, amely 128 GB-nál standard szintű tárolót.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-185">In this example, we will create a managed disk named *myManagedDisk* from our snapshot, that is 128GB in size in standard storage.</span></span>

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-the-vm"></a><span data-ttu-id="5c2ef-186">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="5c2ef-186">Create the VM</span></span>

<span data-ttu-id="5c2ef-187">Hozza létre a virtuális Gépet a [az virtuális gép létrehozása](/cli/azure/vm#create) , és csatlakoztassa (--csatolása operációsrendszer-lemez) a kezelt lemez az operációs rendszer lemezeként.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-187">Now, create your VM with [az vm create](/cli/azure/vm#create) and attach (--attach-os-disk) the managed disk as the OS disk.</span></span> <span data-ttu-id="5c2ef-188">Az alábbi példakód létrehozza a virtuális gépek nevű *myNewVM* a felügyelt lemezes, a feltöltött virtuális merevlemezből létrehozni:</span><span class="sxs-lookup"><span data-stu-id="5c2ef-188">The following example creates a VM named *myNewVM* using the managed disk created from your uploaded VHD:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

<span data-ttu-id="5c2ef-189">SSH-ból a virtuális Gépet, a hitelesítő adatok használatával a forrás virtuális gép képesnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-189">You should be able to SSH into the VM using the credentials from the source VM.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5c2ef-190">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c2ef-190">Next steps</span></span>
<span data-ttu-id="5c2ef-191">Miután előkészített és feltöltése az egyéni virtuális lemez, akkor további információ [erőforrás-kezelő és a sablonok használatával](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-191">After you have prepared and uploaded your custom virtual disk, you can read more about [using Resource Manager and templates](../../azure-resource-manager/resource-group-overview.md).</span></span> <span data-ttu-id="5c2ef-192">Is érdemes lehet [hozzá adatlemezt](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) az új virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="5c2ef-192">You may also want to [add a data disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) to your new VMs.</span></span> <span data-ttu-id="5c2ef-193">Ha fut a virtuális gépek elérését igénylő alkalmazások, ügyeljen arra, hogy [nyisson meg portokat és a végpontok](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5c2ef-193">If you have applications running on your VMs that you need to access, be sure to [open ports and endpoints](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

