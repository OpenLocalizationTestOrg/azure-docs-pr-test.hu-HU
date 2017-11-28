---
title: aaaPrepare egy Debian Linux VHD az Azure-ban |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate Debian 7 & 8 VHD-fájlokat az Azure-telepítés."
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
ms.openlocfilehash: e67d126de3db146357a6509aedb5f478576768f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-debian-vhd-for-azure"></a><span data-ttu-id="8f536-103">Debian-alapú VHD előkészítése Azure-beli használatra</span><span class="sxs-lookup"><span data-stu-id="8f536-103">Prepare a Debian VHD for Azure</span></span>
## <a name="prerequisites"></a><span data-ttu-id="8f536-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8f536-104">Prerequisites</span></span>
<span data-ttu-id="8f536-105">Ez a szakasz azt feltételezi, hogy már telepítette a Debian Linux operációs rendszer hello letöltésének .iso fájlból [Debian webhely](https://www.debian.org/distrib/) tooa virtuális merevlemezt.</span><span class="sxs-lookup"><span data-stu-id="8f536-105">This section assumes that you have already installed a Debian Linux operating system from an .iso file downloaded from hello [Debian website](https://www.debian.org/distrib/) tooa virtual hard disk.</span></span> <span data-ttu-id="8f536-106">Több különféle eszköz létezik toocreate .vhd fájlok; Hyper-V csak egy példaként szolgál.</span><span class="sxs-lookup"><span data-stu-id="8f536-106">Multiple tools exist toocreate .vhd files; Hyper-V is only one example.</span></span> <span data-ttu-id="8f536-107">A Hyper-v-vel utasításokért lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](https://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="8f536-107">For instructions using Hyper-V, see [Install hello Hyper-V Role and Configure a Virtual Machine](https://technet.microsoft.com/library/hh846766.aspx).</span></span>

## <a name="installation-notes"></a><span data-ttu-id="8f536-108">A telepítéssel kapcsolatos megjegyzések</span><span class="sxs-lookup"><span data-stu-id="8f536-108">Installation notes</span></span>
* <span data-ttu-id="8f536-109">Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.</span><span class="sxs-lookup"><span data-stu-id="8f536-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="8f536-110">hello újabb VHDX formátum nem támogatott az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8f536-110">hello newer VHDX format is not supported in Azure.</span></span> <span data-ttu-id="8f536-111">Hello tooVHD lemezformátum Hyper-V kezelőjével vagy a hello konvertálhatja **convert-vhd** parancsmag.</span><span class="sxs-lookup"><span data-stu-id="8f536-111">You can convert hello disk tooVHD format using Hyper-V Manager or hello **convert-vhd** cmdlet.</span></span>
* <span data-ttu-id="8f536-112">Hello Linux rendszer telepítésekor LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="8f536-112">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="8f536-113">LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációs rendszer lemezén legalább egyszer kell csatolt toobe tooanother VM hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="8f536-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="8f536-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ha előnyben részesített adatlemezek használható.</span><span class="sxs-lookup"><span data-stu-id="8f536-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="8f536-115">Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel.</span><span class="sxs-lookup"><span data-stu-id="8f536-115">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="8f536-116">hello Azure Linux ügynök konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.</span><span class="sxs-lookup"><span data-stu-id="8f536-116">hello Azure Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span> <span data-ttu-id="8f536-117">További információk a hello lépéseket találhatók.</span><span class="sxs-lookup"><span data-stu-id="8f536-117">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="8f536-118">Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.</span><span class="sxs-lookup"><span data-stu-id="8f536-118">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-azure-manage-toocreate-debian-vhds"></a><span data-ttu-id="8f536-119">Azure-kezelése toocreate Debian VHD-k használata</span><span class="sxs-lookup"><span data-stu-id="8f536-119">Use Azure-Manage toocreate Debian VHDs</span></span>
<span data-ttu-id="8f536-120">Eszközök is elérhetők az Azure-ba, Debian virtuális merevlemezek létrehozásának például hello [azure-kezelése](https://github.com/credativ/azure-manage) parancsfájl [credativ](http://www.credativ.com/).</span><span class="sxs-lookup"><span data-stu-id="8f536-120">There are tools available for generating Debian VHDs for Azure, such as hello [azure-manage](https://github.com/credativ/azure-manage) scripts from [credativ](http://www.credativ.com/).</span></span> <span data-ttu-id="8f536-121">Ez az ajánlott megközelítést alkalmazva, és teljesen új lemezkép létrehozása hello.</span><span class="sxs-lookup"><span data-stu-id="8f536-121">This is hello recommended approach versus creating an image from scratch.</span></span> <span data-ttu-id="8f536-122">Debian 8 virtuális merevlemez toocreate például futtassa a következő parancsok toodownload azure-kezelése hello (és függőségeinek), és hello azure_build_image parancsfájl futtatása:</span><span class="sxs-lookup"><span data-stu-id="8f536-122">For example, toocreate a Debian 8 VHD run hello following commands toodownload azure-manage (and dependencies) and run hello azure_build_image script:</span></span>

    # sudo apt-get update
    # sudo apt-get install git qemu-utils mbr kpartx debootstrap

    # sudo apt-get install python3-pip python3-dateutil python3-cryptography
    # sudo pip3 install azure-storage azure-servicemanagement-legacy azure-common pytest pyyaml
    # git clone https://github.com/credativ/azure-manage.git
    # cd azure-manage
    # sudo pip3 install .

    # sudo azure_build_image --option release=jessie --option image_size_gb=30 --option image_prefix=debian-jessie-azure section


## <a name="manually-prepare-a-debian-vhd"></a><span data-ttu-id="8f536-123">Manuálisan a Debian virtuális merevlemez előkészítése</span><span class="sxs-lookup"><span data-stu-id="8f536-123">Manually prepare a Debian VHD</span></span>
1. <span data-ttu-id="8f536-124">A Hyper-V kezelőjében válassza ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="8f536-124">In Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="8f536-125">Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="8f536-125">Click **Connect** tooopen a console window for hello virtual machine.</span></span>
3. <span data-ttu-id="8f536-126">Hello sort megjegyzésbe **deb cdrom** a `/etc/apt/source.list` Ha hello virtuális gép ISO-fájl alapján.</span><span class="sxs-lookup"><span data-stu-id="8f536-126">Comment out hello line for **deb cdrom** in `/etc/apt/source.list` if you set up hello VM against an ISO file.</span></span>
4. <span data-ttu-id="8f536-127">Hello szerkesztése `/etc/default/grub` fájlt, és módosítsa a hello **GRUB_CMDLINE_LINUX** paraméter használata a következőképpen történik a tooinclude kiegészítő rendszermag paraméterek az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="8f536-127">Edit hello `/etc/default/grub` file and modify hello **GRUB_CMDLINE_LINUX** parameter as follows tooinclude additional kernel parameters for Azure.</span></span>
   
        GRUB_CMDLINE_LINUX="console=tty0 console=ttyS0,115200 earlyprintk=ttyS0,115200 rootdelay=30"
5. <span data-ttu-id="8f536-128">Hello lárvajárat újraépítése, majd futtassa:</span><span class="sxs-lookup"><span data-stu-id="8f536-128">Rebuild hello grub and run:</span></span>
   
        # sudo update-grub
6. <span data-ttu-id="8f536-129">Adja hozzá a Debian tartozó Azure adattárak too/etc/apt/sources.list Debian 7 vagy 8:</span><span class="sxs-lookup"><span data-stu-id="8f536-129">Add Debian's Azure repositories too/etc/apt/sources.list for either Debian 7 or 8:</span></span>
   
    <span data-ttu-id="8f536-130">**Debian 7.x "Wheezy"**</span><span class="sxs-lookup"><span data-stu-id="8f536-130">**Debian 7.x "Wheezy"**</span></span>
   
        deb http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb-src http://debian-archive.trafficmanager.net/debian wheezy-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure wheezy main
        deb-src http://debian-archive.trafficmanager.net/debian-azure wheezy main

    <span data-ttu-id="8f536-131">**Debian 8.x "Jessie"**</span><span class="sxs-lookup"><span data-stu-id="8f536-131">**Debian 8.x "Jessie"**</span></span>

        deb http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb-src http://debian-archive.trafficmanager.net/debian jessie-backports main
        deb http://debian-archive.trafficmanager.net/debian-azure jessie main
        deb-src http://debian-archive.trafficmanager.net/debian-azure jessie main


1. <span data-ttu-id="8f536-132">Hello Azure Linux ügynök telepítése:</span><span class="sxs-lookup"><span data-stu-id="8f536-132">Install hello Azure Linux Agent:</span></span>
   
        # sudo apt-get update
        # sudo apt-get install waagent
2. <span data-ttu-id="8f536-133">Debian 7 esetén szükséges toorun hello 3.16-alapú kernel adattárból hello wheezy backports.</span><span class="sxs-lookup"><span data-stu-id="8f536-133">For Debian 7, it is required toorun hello 3.16-based kernel from hello wheezy-backports repository.</span></span> <span data-ttu-id="8f536-134">Először létre kell hoznia egy /etc/apt/preferences.d/linux.pref meghívásra hello tartalma a következő fájl:</span><span class="sxs-lookup"><span data-stu-id="8f536-134">First create a file called /etc/apt/preferences.d/linux.pref with hello following contents:</span></span>
   
        Package: linux-image-amd64 initramfs-tools
        Pin: release n=wheezy-backports
        Pin-Priority: 500
   
    <span data-ttu-id="8f536-135">Ezután futtassa a "sudo apt-get telepítése linux-lemezkép-amd64" tooinstall hello új kernel.</span><span class="sxs-lookup"><span data-stu-id="8f536-135">Then run "sudo apt-get install linux-image-amd64" tooinstall hello new kernel.</span></span>
3. <span data-ttu-id="8f536-136">Hello virtuális gép kiosztásának megszüntetése és előkészíti az Azure-on történő üzembe helyezéséhez, és futtassa:</span><span class="sxs-lookup"><span data-stu-id="8f536-136">Deprovision hello virtual machine and prepare it for provisioning on Azure and run:</span></span>
   
        # sudo waagent –force -deprovision
        # export HISTSIZE=0
        # logout
4. <span data-ttu-id="8f536-137">Kattintson a **művelet** -> leállítási le a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="8f536-137">Click **Action** -> Shut Down in Hyper-V Manager.</span></span> <span data-ttu-id="8f536-138">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="8f536-138">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8f536-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8f536-139">Next steps</span></span>
<span data-ttu-id="8f536-140">Ön éppen most már készen áll a toouse a Debian virtuális merevlemez toocreate új virtuális gépeket az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8f536-140">You're now ready toouse your Debian virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="8f536-141">Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8f536-141">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

