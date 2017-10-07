---
title: "aaaCreate és feltöltése az Ubuntu Linux VHD az Azure-ban"
description: "Toocreate ismerje meg, és töltse fel az Azure virtuális merevlemez (VHD), amely tartalmazza az Ubuntu Linux operációs rendszer."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 3e097959-84fc-4f6a-8cc8-35e087fd1542
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: cc546a487f769b32432a7e80ddcd0f6af44e201f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a><span data-ttu-id="f8e0c-103">Ubuntus virtuális gép előkészítése Azure-beli használatra</span><span class="sxs-lookup"><span data-stu-id="f8e0c-103">Prepare an Ubuntu virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="official-ubuntu-cloud-images"></a><span data-ttu-id="f8e0c-104">Hivatalos Ubuntu felhő lemezképek</span><span class="sxs-lookup"><span data-stu-id="f8e0c-104">Official Ubuntu cloud images</span></span>
<span data-ttu-id="f8e0c-105">Ubuntu most teszi közzé a következő hivatalos Azure VHD [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/).</span><span class="sxs-lookup"><span data-stu-id="f8e0c-105">Ubuntu now publishes official Azure VHDs for download at [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/).</span></span> <span data-ttu-id="f8e0c-106">Ha saját speciális Ubuntu kép toobuild kell az Azure-ba, helyett eljárással hello manuális alá az alábbi ismert ajánlott toostart működik a VHD-k és igény szerint testre szabhatja.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-106">If you need toobuild your own specialized Ubuntu image for Azure, rather than use hello manual procedure below it is recommended toostart with these known working VHDs and customize as needed.</span></span> <span data-ttu-id="f8e0c-107">hello Újdonságok kép mindig hello helyek a következő helyen találhatók:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-107">hello latest image releases can always be found at hello following locations:</span></span>

* <span data-ttu-id="f8e0c-108">Ubuntu 12.04/pontos: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="f8e0c-108">Ubuntu 12.04/Precise: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)</span></span>
* <span data-ttu-id="f8e0c-109">Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="f8e0c-109">Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)</span></span>
* <span data-ttu-id="f8e0c-110">Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)</span><span class="sxs-lookup"><span data-stu-id="f8e0c-110">Ubuntu 16.04/Xenial: [ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip](http://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vhd.zip)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8e0c-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f8e0c-111">Prerequisites</span></span>
<span data-ttu-id="f8e0c-112">Ez a cikk feltételezi, hogy az Ubuntu Linux operációs rendszer tooa virtuális merevlemez már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-112">This article assumes that you have already installed an Ubuntu Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="f8e0c-113">Több különféle eszköz toocreate .vhd fájlok, például egy hálózatvirtualizálási megoldás például a Hyper-V létezik.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-113">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="f8e0c-114">Útmutatásért lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="f8e0c-114">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="f8e0c-115">**Ubuntu telepítési megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="f8e0c-115">**Ubuntu installation notes**</span></span>

* <span data-ttu-id="f8e0c-116">Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-116">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="f8e0c-117">hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-117">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="f8e0c-118">Alakítsa át a Hyper-V kezelőjével hello tooVHD formátummal, vagy convert-vhd parancsmag hello.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-118">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span>
* <span data-ttu-id="f8e0c-119">Hello Linux rendszer telepítésekor LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-119">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="f8e0c-120">LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációs rendszer lemezén legalább egyszer kell csatolt toobe tooanother VM hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-120">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="f8e0c-121">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ha előnyben részesített adatlemezek használható.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-121">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="f8e0c-122">Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-122">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="f8e0c-123">Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-123">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="f8e0c-124">További információk a hello lépéseket találhatók.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-124">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="f8e0c-125">Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-125">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="manual-steps"></a><span data-ttu-id="f8e0c-126">Manuális lépések</span><span class="sxs-lookup"><span data-stu-id="f8e0c-126">Manual steps</span></span>
> [!NOTE]
> <span data-ttu-id="f8e0c-127">Toocreate megkísérlése előtt a saját egyéni Ubuntu rendszerképet az Azure-ba, fontolja meg hello segítségével előzetesen elkészített és tesztelése a képek [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) helyette.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-127">Before attempting toocreate your own custom Ubuntu image for Azure, please consider using hello pre-built and tested images from [http://cloud-images.ubuntu.com/](http://cloud-images.ubuntu.com/) instead.</span></span>
> 
> 

1. <span data-ttu-id="f8e0c-128">Hyper-V kezelője hello középső ablaktáblában jelölje ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-128">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="f8e0c-129">Kattintson a **Connect** tooopen hello ablak hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-129">Click **Connect** tooopen hello window for hello virtual machine.</span></span>

3. <span data-ttu-id="f8e0c-130">Cserélje le a hello hello kép toouse Ubuntu Azure repók az aktuális tárházak találhatók.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-130">Replace hello current repositories in hello image toouse Ubuntu's Azure repos.</span></span> <span data-ttu-id="f8e0c-131">hello lépések kissé hello Ubuntu verziójától függően eltérőek.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-131">hello steps vary slightly depending on hello Ubuntu version.</span></span>
   
    <span data-ttu-id="f8e0c-132">Szerkesztése előtt `/etc/apt/sources.list`, az ajánlott toomake biztonsági:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-132">Before editing `/etc/apt/sources.list`, it is recommended toomake a backup:</span></span>
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    <span data-ttu-id="f8e0c-133">Ubuntu 12.04:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-133">Ubuntu 12.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    <span data-ttu-id="f8e0c-134">Ubuntu 14.04:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-134">Ubuntu 14.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    <span data-ttu-id="f8e0c-135">Ubuntu 16.04:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-135">Ubuntu 16.04:</span></span>
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. <span data-ttu-id="f8e0c-136">hello Ubuntu Azure lemezképek Mostantól követi hello *hardver engedélyezése* (HWE) kernel.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-136">hello Ubuntu Azure images are now following hello *hardware enablement* (HWE) kernel.</span></span> <span data-ttu-id="f8e0c-137">Frissítse a hello operációs rendszer toohello legújabb kernel hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-137">Update hello operating system toohello latest kernel by running hello following commands:</span></span>

    <span data-ttu-id="f8e0c-138">Ubuntu 12.04:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-138">Ubuntu 12.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    <span data-ttu-id="f8e0c-139">Ubuntu 14.04:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-139">Ubuntu 14.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    <span data-ttu-id="f8e0c-140">Ubuntu 16.04:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-140">Ubuntu 16.04:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot

    <span data-ttu-id="f8e0c-141">**Lásd még:**</span><span class="sxs-lookup"><span data-stu-id="f8e0c-141">**See also:**</span></span>
    - [<span data-ttu-id="f8e0c-142">https://wiki.ubuntu.com/kernel/LTSEnablementStack</span><span class="sxs-lookup"><span data-stu-id="f8e0c-142">https://wiki.ubuntu.com/Kernel/LTSEnablementStack</span></span>](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [<span data-ttu-id="f8e0c-143">https://wiki.ubuntu.com/kernel/RollingLTSEnablementStack</span><span class="sxs-lookup"><span data-stu-id="f8e0c-143">https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack</span></span>](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. <span data-ttu-id="f8e0c-144">Hello kernel rendszerindító sor lárvajárat tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-144">Modify hello kernel boot line for Grub tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="f8e0c-145">a nyitott toodo `/etc/default/grub` egy szövegszerkesztőben található nevű hello változó `GRUB_CMDLINE_LINUX_DEFAULT` (vagy felveheti Ön is szükség esetén), és módosítsa a következő paraméterek tooinclude hello:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-145">toodo this open `/etc/default/grub` in a text editor, find hello variable called `GRUB_CMDLINE_LINUX_DEFAULT` (or add it if needed) and edit it tooinclude hello following parameters:</span></span>
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    <span data-ttu-id="f8e0c-146">Mentse és zárja be ezt a fájlt, és futtassa `sudo update-grub`.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-146">Save and close this file, and then run `sudo update-grub`.</span></span> <span data-ttu-id="f8e0c-147">Ezzel biztosíthatja, hogy minden konzol üzenetküldés toohello első soros port, amely segít a problémák hibakeresési Azure technikai támogatási szolgálathoz.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-147">This will ensure all console messages are sent toohello first serial port, which can assist Azure technical support with debugging issues.</span></span>

6. <span data-ttu-id="f8e0c-148">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-148">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="f8e0c-149">Ez általában akkor hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-149">This is usually hello default.</span></span>

7. <span data-ttu-id="f8e0c-150">Hello Azure Linux ügynök telepítése:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-150">Install hello Azure Linux Agent:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

    >[!Note]
    <span data-ttu-id="f8e0c-151">Hello `walinuxagent` csomag távolíthatja el a hello `NetworkManager` és `NetworkManager-gnome` a csomagokat, ha telepítve van.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-151">hello `walinuxagent` package may remove hello `NetworkManager` and `NetworkManager-gnome` packages, if they are installed.</span></span>

8. <span data-ttu-id="f8e0c-152">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-152">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

9. <span data-ttu-id="f8e0c-153">Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-153">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="f8e0c-154">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-154">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8e0c-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f8e0c-155">Next steps</span></span>
<span data-ttu-id="f8e0c-156">Ön éppen most már készen áll a toouse az Ubuntu Linux virtuális merevlemez toocreate új virtuális gépek Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="f8e0c-156">You're now ready toouse your Ubuntu Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="f8e0c-157">Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8e0c-157">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

## <a name="references"></a><span data-ttu-id="f8e0c-158">Referencia</span><span class="sxs-lookup"><span data-stu-id="f8e0c-158">References</span></span>
<span data-ttu-id="f8e0c-159">Ubuntu hardver engedélyezése (HWE) kernel:</span><span class="sxs-lookup"><span data-stu-id="f8e0c-159">Ubuntu hardware enablement (HWE) kernel:</span></span>

* [<span data-ttu-id="f8e0c-160">http://blog.utlemming.org/2015/01/ubuntu-1404-Azure-Images-Now-Tracking.HTML</span><span class="sxs-lookup"><span data-stu-id="f8e0c-160">http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html</span></span>](http://blog.utlemming.org/2015/01/ubuntu-1404-azure-images-now-tracking.html)
* [<span data-ttu-id="f8e0c-161">http://blog.utlemming.org/2015/02/1204-Azure-cloud-Images-Now-Using-hwe.HTML</span><span class="sxs-lookup"><span data-stu-id="f8e0c-161">http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html</span></span>](http://blog.utlemming.org/2015/02/1204-azure-cloud-images-now-using-hwe.html)

