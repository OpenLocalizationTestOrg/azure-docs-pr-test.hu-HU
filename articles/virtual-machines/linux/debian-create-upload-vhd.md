---
title: "Az Azure-ban Debian Linux virtuális merevlemez előkészítése |} Microsoft Docs"
description: "Útmutató a Debian 7 & központi telepítési 8 VHD-fájlok létrehozása az Azure-ban."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: a6de7a7c-cc70-44e7-aed0-2ae6884d401a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 63970d162c12984d6476bf0b9fc4ab70160eccdb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-a-debian-vhd-for-azure"></a><span data-ttu-id="177ab-103">Debian-alapú VHD előkészítése Azure-beli használatra</span><span class="sxs-lookup"><span data-stu-id="177ab-103">Prepare a Debian VHD for Azure</span></span>
## <a name="prerequisites"></a><span data-ttu-id="177ab-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="177ab-104">Prerequisites</span></span>
<span data-ttu-id="177ab-105">Ez a szakasz azt feltételezi, hogy már telepítette a Debian Linux operációs rendszer ISO-fájlból le: a [Debian webhely](https://www.debian.org/distrib/) egy virtuális merevlemezre.</span><span class="sxs-lookup"><span data-stu-id="177ab-105">This section assumes that you have already installed a Debian Linux operating system from an .iso file downloaded from the [Debian website](https://www.debian.org/distrib/) to a virtual hard disk.</span></span> <span data-ttu-id="177ab-106">Több különféle eszköz található .vhd fájlok; létrehozása Hyper-V csak egy példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="177ab-106">Multiple tools exist to create .vhd files; Hyper-V is only one example.</span></span> <span data-ttu-id="177ab-107">A Hyper-v-vel utasításokért lásd: [a Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="177ab-107">For instructions using Hyper-V, see [Install the Hyper-V Role and Configure a Virtual Machine](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

## <a name="installation-notes"></a><span data-ttu-id="177ab-108">A telepítéssel kapcsolatos megjegyzések</span><span class="sxs-lookup"><span data-stu-id="177ab-108">Installation notes</span></span>
* <span data-ttu-id="177ab-109">Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.</span><span class="sxs-lookup"><span data-stu-id="177ab-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="177ab-110">Az újabb VHDX formátum nem támogatott az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="177ab-110">The newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="177ab-111">A lemez VHD formátumú Hyper-V kezelőjével konvertálhatja vagy a **convert-vhd** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="177ab-111">You can convert the disk to VHD format using Hyper-V Manager or the **convert-vhd** cmdlet.</span></span>
* <span data-ttu-id="177ab-112">A Linux rendszer telepítésekor LVM (gyakran sok telepítés alapértelmezett), hanem szabványos partíciók használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="177ab-112">When installing the Linux system it is recommended that you use standard partitions rather than LVM (often the default for many installations).</span></span> <span data-ttu-id="177ab-113">LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációsrendszer-lemez legalább egyszer meg kell hibaelhárítási egy másik virtuális géphez csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="177ab-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another VM for troubleshooting.</span></span> <span data-ttu-id="177ab-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ha előnyben részesített adatlemezek használható.</span><span class="sxs-lookup"><span data-stu-id="177ab-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="177ab-115">Ne konfiguráljon egy swap partíciót az operációsrendszer-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="177ab-115">Do not configure a swap partition on the OS disk.</span></span> <span data-ttu-id="177ab-116">Az Azure Linux ügynök beállítható úgy, hogy az ideiglenes erőforrás lemezen a lapozófájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="177ab-116">The Azure Linux agent can be configured to create a swap file on the temporary resource disk.</span></span> <span data-ttu-id="177ab-117">További információk a megtalálhatók az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="177ab-117">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="177ab-118">Összes, a virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.</span><span class="sxs-lookup"><span data-stu-id="177ab-118">All of the VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-azure-manage-to-create-debian-vhds"></a><span data-ttu-id="177ab-119">Debian VHD-k létrehozásához használja a Azure-kezelése</span><span class="sxs-lookup"><span data-stu-id="177ab-119">Use Azure-Manage to create Debian VHDs</span></span>
<span data-ttu-id="177ab-120">Eszközök is elérhetők generálásához. az Azure-ba, Debian virtuális merevlemezeket, mint a [azure-kezelése](https://github.com/credativ/azure-manage) parancsfájl [credativ](http://www.credativ.com/).</span><span class="sxs-lookup"><span data-stu-id="177ab-120">There are tools available for generating Debian VHDs for Azure, such as the [azure-manage](https://github.com/credativ/azure-manage) scripts from [credativ](http://www.credativ.com/).</span></span> <span data-ttu-id="177ab-121">Ez az az ajánlott módszer, és teljesen új lemezkép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="177ab-121">This is the recommended approach versus creating an image from scratch.</span></span> <span data-ttu-id="177ab-122">Például létrehozásához futtassa az alábbi parancsokat letöltése Debian 8 virtuális merevlemez azure-kezelése (és függőségeinek) és a azure_build_image parancsprogrammal:</span><span class="sxs-lookup"><span data-stu-id="177ab-122">For example, to create a Debian 8 VHD run the following commands to download azure-manage (and dependencies) and run the azure_build_image script:</span></span>

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a><span data-ttu-id="177ab-123">Manuálisan a Debian virtuális merevlemez előkészítése</span><span class="sxs-lookup"><span data-stu-id="177ab-123">Manually prepare a Debian VHD</span></span>
1. <span data-ttu-id="177ab-124">A Hyper-V kezelőjében válassza ki a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="177ab-124">In Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="177ab-125">Kattintson a **Connect** nyissa meg a konzol ablakot a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="177ab-125">Click **Connect** to open a console window for the virtual machine.</span></span>
3. <span data-ttu-id="177ab-126">A vonal megjegyzésbe **deb cdrom** a `/etc/apt/source.list` Ha úgy konfigurálja a virtuális gép ISO-fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="177ab-126">Comment out the line for **deb cdrom** in `/etc/apt/source.list` if you set up the VM against an ISO file.</span></span>
4. <span data-ttu-id="177ab-127">Szerkessze a `/etc/default/grub` fájlt, és módosítsa a **GRUB_CMDLINE_LINUX** paraméter a következő kiegészítő rendszermag paramétereket tartalmazza az Azure.</span><span class="sxs-lookup"><span data-stu-id="177ab-127">Edit the `/etc/default/grub` file and modify the **GRUB_CMDLINE_LINUX** parameter as follows to include additional kernel parameters for Azure.</span></span>
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. <span data-ttu-id="177ab-128">Építse újra a lárvajárat, majd futtassa:</span><span class="sxs-lookup"><span data-stu-id="177ab-128">Rebuild the grub and run:</span></span>
   
        # sudo update-grub
6. <span data-ttu-id="177ab-129">Vegye fel a Debian tartozó Azure adattárak /etc/apt/sources.list Debian 7 vagy 8:</span><span class="sxs-lookup"><span data-stu-id="177ab-129">Add Debian's Azure repositories to /etc/apt/sources.list for either Debian 7 or 8:</span></span>
   
    <span data-ttu-id="177ab-130">**Debian 7.x "Wheezy"**</span><span class="sxs-lookup"><span data-stu-id="177ab-130">**Debian 7.x "Wheezy"**</span></span>
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    <span data-ttu-id="177ab-131">**Debian 8.x "Jessie"**</span><span class="sxs-lookup"><span data-stu-id="177ab-131">**Debian 8.x "Jessie"**</span></span>

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. <span data-ttu-id="177ab-132">Az Azure Linux ügynök telepítése:</span><span class="sxs-lookup"><span data-stu-id="177ab-132">Install the Azure Linux Agent:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. <span data-ttu-id="177ab-133">Debian 7 a wheezy backports tárházból 3.16-alapú kernel futtatásához szükség.</span><span class="sxs-lookup"><span data-stu-id="177ab-133">For Debian 7, it is required to run the 3.16-based kernel from the wheezy-backports repository.</span></span> <span data-ttu-id="177ab-134">Először létre kell hoznia a következő tartalommal /etc/apt/preferences.d/linux.pref nevű fájlba:</span><span class="sxs-lookup"><span data-stu-id="177ab-134">First create a file called /etc/apt/preferences.d/linux.pref with the following contents:</span></span>
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    <span data-ttu-id="177ab-135">Ezután futtassa "sudo apt get telepítése linux-lemezkép-amd64" az új kernel telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="177ab-135">Then run "sudo apt-get install linux-image-amd64" to install the new kernel.</span></span>
3. <span data-ttu-id="177ab-136">A virtuális gép kiosztásának megszüntetése és előkészíti az Azure-on történő üzembe helyezéséhez, és futtassa:</span><span class="sxs-lookup"><span data-stu-id="177ab-136">Deprovision the virtual machine and prepare it for provisioning on Azure and run:</span></span>
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. <span data-ttu-id="177ab-137">Kattintson a **művelet** -> leállítási le a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="177ab-137">Click **Action** -> Shut Down in Hyper-V Manager.</span></span> <span data-ttu-id="177ab-138">A Linux virtuális merevlemez az Azure-bA feltölteni kívánt készen áll.</span><span class="sxs-lookup"><span data-stu-id="177ab-138">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="177ab-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="177ab-139">Next steps</span></span>
<span data-ttu-id="177ab-140">Most már készen áll a Debian virtuális merevlemez segítségével új virtuális gépek létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="177ab-140">You're now ready to use your Debian virtual hard disk to create new virtual machines in Azure.</span></span> <span data-ttu-id="177ab-141">Ha az első alkalommal, hogy van-e a .vhd fájl feltöltése az Azure, tekintse meg a 2. és 3 [létrehozása és feltöltése, a Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="177ab-141">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

