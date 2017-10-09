---
title: "aaaCreate, és töltse fel a Linux virtuális merevlemez tooAzure |} Microsoft Docs"
description: "Létrehozása és feltöltése az Azure virtuális merevlemez (VHD), amely tartalmazza a hello Linux operációs rendszert hello klasszikus telepítési modell segítségével"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8058ff98-db03-4309-9bf4-69842bd64dd4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: iainfou
ms.openlocfilehash: 77b01316386c4a6eb68c129fa68d42f0a8996edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-hello-linux-operating-system"></a><span data-ttu-id="f1907-103">Létrehozásával, majd ismét feltölteni a virtuális merevlemez a Contains hello Linux operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="f1907-103">Creating and Uploading a Virtual Hard Disk that Contains hello Linux Operating System</span></span>
> [!IMPORTANT] 
> <span data-ttu-id="f1907-104">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="f1907-104">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="f1907-105">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="f1907-105">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="f1907-106">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="f1907-106">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="f1907-107">Emellett [feltöltése az Azure Resource Manager használatával egyéni lemezképet](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f1907-107">You can also [upload a custom disk image using Azure Resource Manager](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="f1907-108">Ez a cikk bemutatja, hogyan toocreate és a virtuális merevlemez (VHD), mint a saját használhassa feltöltése kép toocreate virtuális gépek Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f1907-108">This article shows you how toocreate and upload a virtual hard disk (VHD) so you can use it as your own image toocreate virtual machines in Azure.</span></span> <span data-ttu-id="f1907-109">Ismerje meg, hogyan tooprepare hello operációs rendszer, hogy használhassa az toocreate több virtuális gép alapján, hogy a lemezképet.</span><span class="sxs-lookup"><span data-stu-id="f1907-109">Learn how tooprepare hello operating system so you can use it toocreate multiple virtual machines based on that image.</span></span> 


## <a name="prerequisites"></a><span data-ttu-id="f1907-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f1907-110">Prerequisites</span></span>
<span data-ttu-id="f1907-111">Ez a cikk feltételezi, hogy rendelkezik-e a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f1907-111">This article assumes that you have hello following items:</span></span>

* <span data-ttu-id="f1907-112">**Linux operációs rendszer van telepítve, a .vhd-fájllá** -telepítette egy [Azure által támogatott Linux-disztribúció](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (vagy lásd: [nem támogatott disztribúciókkal kapcsolatos információi](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuális lemez hello VHD-formátumban.</span><span class="sxs-lookup"><span data-stu-id="f1907-112">**Linux operating system installed in a .vhd file** - You have installed an [Azure-endorsed Linux distribution](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (or see [information for non-endorsed distributions](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtual disk in hello VHD format.</span></span> <span data-ttu-id="f1907-113">Több különféle eszköz toocreate érhetők el a virtuális gép és a virtuális merevlemez:</span><span class="sxs-lookup"><span data-stu-id="f1907-113">Multiple tools exist toocreate a VM and VHD:</span></span>
  * <span data-ttu-id="f1907-114">Telepítse és konfigurálja [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) vagy [KVM](http://www.linux-kvm.org/page/RunningKVM), ügyelve toouse a lemezkép formátumú virtuális Merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="f1907-114">Install and configure [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) or [KVM](http://www.linux-kvm.org/page/RunningKVM), taking care toouse VHD as your image format.</span></span> <span data-ttu-id="f1907-115">Ha szükséges, akkor [lemezkép konvertálása](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) használatával `qemu-img convert`.</span><span class="sxs-lookup"><span data-stu-id="f1907-115">If needed, you can [convert an image](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) using `qemu-img convert`.</span></span>
  * <span data-ttu-id="f1907-116">Is használhatja a Hyper-V [Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) vagy [Windows Server 2012 vagy 2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="f1907-116">You can also use Hyper-V [on Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) or [on Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="f1907-117">hello újabb VHDX formátum nem támogatott az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f1907-117">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="f1907-118">Amikor létrehoz egy virtuális Gépet, adja meg a virtuális merevlemez hello formátumban.</span><span class="sxs-lookup"><span data-stu-id="f1907-118">When you create a VM, specify VHD as hello format.</span></span> <span data-ttu-id="f1907-119">Ha szükséges, VHDX-lemezek tooVHD használatával konvertálhatja [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) vagy hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="f1907-119">If needed, you can convert VHDX disks tooVHD using [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) or hello [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) PowerShell cmdlet.</span></span> <span data-ttu-id="f1907-120">További Azure nem támogatja dinamikus virtuális merevlemezek, feltöltése, ezért meg kell tooconvert ilyen lemezek toostatic VHD-k feltöltés előtt.</span><span class="sxs-lookup"><span data-stu-id="f1907-120">Further, Azure does not support uploading dynamic VHDs, so you need tooconvert such disks toostatic VHDs before uploading.</span></span> <span data-ttu-id="f1907-121">Használhatja például a [NYISSA meg az Azure virtuális merevlemez segédprogramok](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert a dinamikus lemezek a hello folyamat tooAzure feltöltése során.</span><span class="sxs-lookup"><span data-stu-id="f1907-121">You can use tools such as [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamic disks during hello process of uploading tooAzure.</span></span>

* <span data-ttu-id="f1907-122">**Azure parancssori felület** -telepítés hello legújabb [Azure parancssori felület](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooupload hello VHD-t.</span><span class="sxs-lookup"><span data-stu-id="f1907-122">**Azure Command-line Interface** - Install hello latest [Azure Command-Line Interface](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooupload hello VHD.</span></span>

<span data-ttu-id="f1907-123"><a id="prepimage"> </a></span><span class="sxs-lookup"><span data-stu-id="f1907-123"><a id="prepimage"> </a></span></span>

## <a name="step-1-prepare-hello-image-toobe-uploaded"></a><span data-ttu-id="f1907-124">1. lépés: Felkészülés hello kép toobe feltöltve.</span><span class="sxs-lookup"><span data-stu-id="f1907-124">Step 1: Prepare hello image toobe uploaded</span></span>
<span data-ttu-id="f1907-125">Azure támogatja a különböző Linux terjesztésekről (lásd: [támogatott Disztribúciókkal](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span><span class="sxs-lookup"><span data-stu-id="f1907-125">Azure supports various Linux distributions (see [Endorsed Distributions](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)).</span></span> <span data-ttu-id="f1907-126">a következő cikkek hello végigvezetik hogyan tooprepare hello Azure által támogatott különböző Linux-disztribúció.</span><span class="sxs-lookup"><span data-stu-id="f1907-126">hello following articles guide you through how tooprepare hello various Linux distributions that are supported on Azure.</span></span> <span data-ttu-id="f1907-127">Miután elvégezte a következő útmutatók hello hello lépéseit, lépjen vissza ide, ha már van egy VHD-fájlt, amely készen áll a tooupload tooAzure:</span><span class="sxs-lookup"><span data-stu-id="f1907-127">After you complete hello steps in hello following guides, come back here once you have a VHD file that is ready tooupload tooAzure:</span></span>

* <span data-ttu-id="f1907-128">**[CentOS-alapú Disztribúciók](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="f1907-128">**[CentOS-based Distributions](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="f1907-129">**[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="f1907-129">**[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="f1907-130">**[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="f1907-130">**[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="f1907-131">**[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="f1907-131">**[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="f1907-132">**[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="f1907-132">**[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="f1907-133">**[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="f1907-133">**[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>
* <span data-ttu-id="f1907-134">**[Egyéb - nem támogatott Disztribúciókkal](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span><span class="sxs-lookup"><span data-stu-id="f1907-134">**[Other - Non-Endorsed Distributions](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**</span></span>

> [!NOTE]
> <span data-ttu-id="f1907-135">hello Azure platformon SLA vonatkozik a Linux operációs rendszert futtató csak akkor, ha egy hello által támogatott disztribúciók használt hello konfigurációs adatait a támogatott verziók hello működő toovirtual gépek [Azure-Endorsed Terjesztéseket Linux ](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f1907-135">hello Azure platform SLA applies toovirtual machines running hello Linux OS only when one of hello endorsed distributions is used with hello configuration details as specified under 'Supported Versions' in [Linux on Azure-Endorsed Distributions](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="f1907-136">Az összes Linux terjesztésekről hello kép: Azure-katalógus a szükséges konfigurációval hello hitelesített terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="f1907-136">All Linux distributions in hello Azure image gallery are endorsed distributions with hello required configuration.</span></span>
> 
> 

<span data-ttu-id="f1907-137">Lásd még: hello  **[Linux telepítési jegyzetek](../create-upload-generic.md#general-linux-installation-notes)**  kapcsolatos további általános tippek Linux lemezképek előkészítése az Azure.</span><span class="sxs-lookup"><span data-stu-id="f1907-137">Also see hello **[Linux Installation Notes](../create-upload-generic.md#general-linux-installation-notes)** for more general tips on preparing Linux images for Azure.</span></span>

<span data-ttu-id="f1907-138"><a id="connect"> </a></span><span class="sxs-lookup"><span data-stu-id="f1907-138"><a id="connect"> </a></span></span>

## <a name="step-2-prepare-hello-connection-tooazure"></a><span data-ttu-id="f1907-139">2. lépés: Hello kapcsolat tooAzure előkészítése</span><span class="sxs-lookup"><span data-stu-id="f1907-139">Step 2: Prepare hello connection tooAzure</span></span>
<span data-ttu-id="f1907-140">Ellenőrizze, hogy hello Azure CLI hello klasszikus üzembe helyezési modellel használ (`azure config mode asm`), majd jelentkezzen be tooyour fiók:</span><span class="sxs-lookup"><span data-stu-id="f1907-140">Make sure you are using hello Azure CLI in hello classic deployment model (`azure config mode asm`), then log in tooyour account:</span></span>

```azurecli
azure login
```


<span data-ttu-id="f1907-141"><a id="upload"> </a></span><span class="sxs-lookup"><span data-stu-id="f1907-141"><a id="upload"> </a></span></span>

## <a name="step-3-upload-hello-image-tooazure"></a><span data-ttu-id="f1907-142">3. lépés: Hello kép tooAzure feltöltése</span><span class="sxs-lookup"><span data-stu-id="f1907-142">Step 3: Upload hello image tooAzure</span></span>
<span data-ttu-id="f1907-143">A tárolási fiók tooupload kell a virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="f1907-143">You need a storage account tooupload your VHD file to.</span></span> <span data-ttu-id="f1907-144">Vagy válassza ki meglévő tárfiók vagy [hozzon létre egy újat](../../../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="f1907-144">You can either pick an existing storage account or [create a new one](../../../storage/common/storage-create-storage-account.md).</span></span>

<span data-ttu-id="f1907-145">Hello Azure CLI tooupload hello kép használata hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="f1907-145">Use hello Azure CLI tooupload hello image by using hello following command:</span></span>

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

<span data-ttu-id="f1907-146">Az előző példában hello:</span><span class="sxs-lookup"><span data-stu-id="f1907-146">In hello previous example:</span></span>

* <span data-ttu-id="f1907-147">**BlobStorageURL** toouse tervezi hello tárfiók hello URL-címe</span><span class="sxs-lookup"><span data-stu-id="f1907-147">**BlobStorageURL** is hello URL for hello storage account you plan toouse</span></span>
* <span data-ttu-id="f1907-148">**YourImagesFolder** a blob storage tárolóban hello ahol van toostore a lemezképbe</span><span class="sxs-lookup"><span data-stu-id="f1907-148">**YourImagesFolder** is hello container within blob storage where you want toostore your images</span></span>
* <span data-ttu-id="f1907-149">**VHDName** hello címke sem, hogy megjelenik-portál tooidentify hello virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="f1907-149">**VHDName** is hello label that appears in portal tooidentify hello virtual hard disk.</span></span>
* <span data-ttu-id="f1907-150">**PathToVHDFile** hello teljes elérési útja és neve hello .vhd fájlt a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="f1907-150">**PathToVHDFile** is hello full path and name of hello .vhd file on your machine.</span></span>

<span data-ttu-id="f1907-151">a következő parancs hello egy teljes példát mutat be:</span><span class="sxs-lookup"><span data-stu-id="f1907-151">hello following command shows a complete example:</span></span>

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-hello-image"></a><span data-ttu-id="f1907-152">4. lépés: Virtuális gép létrehozása hello lemezképből</span><span class="sxs-lookup"><span data-stu-id="f1907-152">Step 4: Create a VM from hello image</span></span>
<span data-ttu-id="f1907-153">A virtuális gép használata `azure vm create` a hello ugyanúgy, ahogy a normál virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="f1907-153">You create a VM using `azure vm create` in hello same way as a regular VM.</span></span> <span data-ttu-id="f1907-154">Adja meg a lemezkép hello előző lépésben megadott hello névnek.</span><span class="sxs-lookup"><span data-stu-id="f1907-154">Specify hello name you gave your image in hello previous step.</span></span> <span data-ttu-id="f1907-155">A következő példa hello, használjuk hello **myImage** hello előző lépésben megadott lemezkép neve:</span><span class="sxs-lookup"><span data-stu-id="f1907-155">In hello following example, we use hello **myImage** image name given in hello previous step:</span></span>

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

<span data-ttu-id="f1907-156">toocreate a saját virtuális gépeket, adja meg a saját felhasználónév + a jelszó, a hely, a DNS-név és a kép neve.</span><span class="sxs-lookup"><span data-stu-id="f1907-156">toocreate your own VMs, provide your own username + password, location, DNS name, and image name.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1907-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1907-157">Next steps</span></span>
<span data-ttu-id="f1907-158">További információkért lásd: [hello Azure klasszikus telepítési modell Azure parancssori felület referenciája](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span><span class="sxs-lookup"><span data-stu-id="f1907-158">For more information, see [Azure CLI reference for hello Azure classic deployment model](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).</span></span>

[Step 1: Prepare hello image toobe uploaded]:#prepimage
[Step 2: Prepare hello connection tooAzure]:#connect
[Step 3: Upload hello image tooAzure]:#upload
