---
title: "aaaCreate és feltöltése az Azure-ban SUSE Linux virtuális merevlemez"
description: "Toocreate ismerje meg, és töltse fel az Azure virtuális merevlemez (VHD) a SUSE Linux operációs rendszert tartalmazó."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 066d01a6-2a54-4718-bcd0-90fe7a5303a1
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: szark
ms.openlocfilehash: 9185c7e67279357f00db0f43e944e96c58f0dd60
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a><span data-ttu-id="6e3c6-103">SLES- vagy openSUSE-alapú virtuális gép előkészítése Azure-beli használatra</span><span class="sxs-lookup"><span data-stu-id="6e3c6-103">Prepare a SLES or openSUSE virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="6e3c6-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6e3c6-104">Prerequisites</span></span>
<span data-ttu-id="6e3c6-105">Ez a cikk feltételezi, hogy már telepítve van a SUSE vagy openSUSE Linux operációs rendszer tooa virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-105">This article assumes that you have already installed a SUSE or openSUSE Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="6e3c6-106">Több különféle eszköz toocreate .vhd fájlok, például egy hálózatvirtualizálási megoldás például a Hyper-V létezik.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-106">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="6e3c6-107">Útmutatásért lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="6e3c6-107">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="sles--opensuse-installation-notes"></a><span data-ttu-id="6e3c6-108">SLES / openSUSE telepítési megjegyzések</span><span class="sxs-lookup"><span data-stu-id="6e3c6-108">SLES / openSUSE installation notes</span></span>
* <span data-ttu-id="6e3c6-109">Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="6e3c6-110">hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-110">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="6e3c6-111">Alakítsa át a Hyper-V kezelőjével hello tooVHD formátummal, vagy convert-vhd parancsmag hello.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-111">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span>
* <span data-ttu-id="6e3c6-112">Hello Linux rendszer telepítésekor LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-112">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="6e3c6-113">LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációs rendszer lemezén legalább egyszer kell csatolt toobe tooanother VM hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="6e3c6-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ha előnyben részesített adatlemezek használható.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="6e3c6-115">Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-115">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="6e3c6-116">Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-116">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="6e3c6-117">További információk a hello lépéseket találhatók.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-117">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="6e3c6-118">Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-118">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-suse-studio"></a><span data-ttu-id="6e3c6-119">SUSE studióval</span><span class="sxs-lookup"><span data-stu-id="6e3c6-119">Use SUSE Studio</span></span>
<span data-ttu-id="6e3c6-120">[SUSE Studio](http://www.susestudio.com) könnyen létrehozása és a SLES és openSUSE lemezképek kezelése az Azure és a Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-120">[SUSE Studio](http://www.susestudio.com) can easily create and manage your SLES and openSUSE images for Azure and Hyper-V.</span></span> <span data-ttu-id="6e3c6-121">Ez az ajánlott megközelítést alkalmazva a saját SLES és openSUSE lemezképek testreszabása a hello.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-121">This is hello recommended approach for customizing your own SLES and openSUSE images.</span></span>

<span data-ttu-id="6e3c6-122">Egy alternatív toobuilding, a saját virtuális merevlemez, SUSE is közzéteszi (Bring Your saját előfizetés) saját lemezképek a következő SLES [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).</span><span class="sxs-lookup"><span data-stu-id="6e3c6-122">As an alternative toobuilding your own VHD, SUSE also publishes BYOS (Bring Your Own Subscription) images for SLES at [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).</span></span>

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a><span data-ttu-id="6e3c6-123">SUSE Linux Enterprise Server 11 SP4 előkészítése</span><span class="sxs-lookup"><span data-stu-id="6e3c6-123">Prepare SUSE Linux Enterprise Server 11 SP4</span></span>
1. <span data-ttu-id="6e3c6-124">Hyper-V kezelője hello középső ablaktáblában jelölje ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-124">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="6e3c6-125">Kattintson a **Connect** tooopen hello ablak hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-125">Click **Connect** tooopen hello window for hello virtual machine.</span></span>
3. <span data-ttu-id="6e3c6-126">A SUSE Linux Enterprise rendszer tooallow regisztrálja azt toodownload frissítések és a csomagok telepítése.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-126">Register your SUSE Linux Enterprise system tooallow it toodownload updates and install packages.</span></span>
4. <span data-ttu-id="6e3c6-127">Hello rendszer frissítése a legújabb javítások hello:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-127">Update hello system with hello latest patches:</span></span>
   
        # sudo zypper update
5. <span data-ttu-id="6e3c6-128">Hello Azure Linux ügynök telepítése SLES adattárból hello:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-128">Install hello Azure Linux Agent from hello SLES repository:</span></span>
   
        # sudo zypper install WALinuxAgent
6. <span data-ttu-id="6e3c6-129">Annak ellenőrzése, ha a waagent túl van-e beállítva "on" a chkconfig, és ha nem, engedélyezze az automatikus indítása:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-129">Check if waagent is set too"on" in chkconfig, and if not, enable it for autostart:</span></span>
   
        # sudo chkconfig waagent on
7. <span data-ttu-id="6e3c6-130">Ellenőrizze, hogy a waagent-szolgáltatás fut, és ha nem, indítsa el:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-130">Check if waagent service is running, and if not, start it:</span></span> 
   
        # sudo service waagent start
8. <span data-ttu-id="6e3c6-131">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-131">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="6e3c6-132">a nyitott toodo "/ boot/grub/menu.lst" egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-132">toodo this open "/boot/grub/menu.lst" in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    <span data-ttu-id="6e3c6-133">Ezzel biztosíthatja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-133">This will ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
9. <span data-ttu-id="6e3c6-134">Ellenőrizze, hogy /boot/grub/menu.lst és a/etc/fstab mindkét hivatkozás hello lemez az UUID (által-uuid) helyett hello Lemezazonosítót (-azonosító szerint).</span><span class="sxs-lookup"><span data-stu-id="6e3c6-134">Confirm that /boot/grub/menu.lst and /etc/fstab both reference hello disk using its UUID (by-uuid) instead of hello disk ID (by-id).</span></span> 
   
    <span data-ttu-id="6e3c6-135">Lemezek UUID azonosítója beolvasása</span><span class="sxs-lookup"><span data-stu-id="6e3c6-135">Get disk UUID</span></span>
   
        # ls /dev/disk/by-uuid/
   
    <span data-ttu-id="6e3c6-136">Ha /dev/disk/by-id / hello megfelelő által-uuid-értékkel rendelkező /boot/grub/menu.lst mind a/etc/fstab használt, frissítése</span><span class="sxs-lookup"><span data-stu-id="6e3c6-136">If /dev/disk/by-id/ is used, update both /boot/grub/menu.lst and /etc/fstab with hello proper by-uuid value</span></span>
   
    <span data-ttu-id="6e3c6-137">Változás előtt</span><span class="sxs-lookup"><span data-stu-id="6e3c6-137">Before change</span></span>
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    <span data-ttu-id="6e3c6-138">Módosítás után</span><span class="sxs-lookup"><span data-stu-id="6e3c6-138">After change</span></span>
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. <span data-ttu-id="6e3c6-139">Módosítsa a udev szabályok tooavoid hello Ethernet adaptert statikus szabályainak létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-139">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="6e3c6-140">Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v virtuális gép klónozása</span><span class="sxs-lookup"><span data-stu-id="6e3c6-140">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. <span data-ttu-id="6e3c6-141">Ajánlott tooedit hello fájl "/ etc/sysconfig/hálózati/dhcp", és módosítsa a hello `DHCLIENT_SET_HOSTNAME` paraméter toohello következő:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-141">It is recommended tooedit hello file "/etc/sysconfig/network/dhcp" and change hello `DHCLIENT_SET_HOSTNAME` parameter toohello following:</span></span>
    
     <span data-ttu-id="6e3c6-142">DHCLIENT_SET_HOSTNAME = "nem"</span><span class="sxs-lookup"><span data-stu-id="6e3c6-142">DHCLIENT_SET_HOSTNAME="no"</span></span>
12. <span data-ttu-id="6e3c6-143">A "/ etc/sudoers" megjegyzéssé, vagy távolítsa el az alábbi sorokat, ha vannak ilyenek hello:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-143">In "/etc/sudoers", comment out or remove hello following lines if they exist:</span></span>
    
     <span data-ttu-id="6e3c6-144">Alapértelmezés szerint targetpw # hello célként megadott felhasználó összes ALL=(ALL) összes azaz gyökér hello jelszó kérése # figyelmeztetés!</span><span class="sxs-lookup"><span data-stu-id="6e3c6-144">Defaults targetpw   # ask for hello password of hello target user i.e. root ALL    ALL=(ALL) ALL   # WARNING!</span></span> <span data-ttu-id="6e3c6-145">Csak ezzel együtt a 'Alapértelmezett targetpw'!</span><span class="sxs-lookup"><span data-stu-id="6e3c6-145">Only use this together with 'Defaults targetpw'!</span></span>
13. <span data-ttu-id="6e3c6-146">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-146">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="6e3c6-147">Ez általában akkor hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-147">This is usually hello default.</span></span>
14. <span data-ttu-id="6e3c6-148">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-148">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="6e3c6-149">hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-149">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="6e3c6-150">Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-150">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="6e3c6-151">Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-151">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
     <span data-ttu-id="6e3c6-152">ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Megjegyzés: a szükséges toobe toowhatever beállítása.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-152">ResourceDisk.Format=y  ResourceDisk.Filesystem=ext4  ResourceDisk.MountPoint=/mnt/resource  ResourceDisk.EnableSwap=y  ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.</span></span>
15. <span data-ttu-id="6e3c6-153">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-153">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
    # <a name="sudo-waagent--force--deprovision"></a><span data-ttu-id="6e3c6-154">sudo waagent-force - deprovision</span><span class="sxs-lookup"><span data-stu-id="6e3c6-154">sudo waagent -force -deprovision</span></span>
    # <a name="export-histsize0"></a><span data-ttu-id="6e3c6-155">exportálja a HISTSIZE = 0</span><span class="sxs-lookup"><span data-stu-id="6e3c6-155">export HISTSIZE=0</span></span>
    # <a name="logout"></a><span data-ttu-id="6e3c6-156">Kijelentkezés</span><span class="sxs-lookup"><span data-stu-id="6e3c6-156">logout</span></span>
16. <span data-ttu-id="6e3c6-157">Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-157">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="6e3c6-158">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-158">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

- - -
## <a name="prepare-opensuse-131"></a><span data-ttu-id="6e3c6-159">OpenSUSE 13.1 + előkészítése</span><span class="sxs-lookup"><span data-stu-id="6e3c6-159">Prepare openSUSE 13.1+</span></span>
1. <span data-ttu-id="6e3c6-160">Hyper-V kezelője hello középső ablaktáblában jelölje ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-160">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="6e3c6-161">Kattintson a **Connect** tooopen hello ablak hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-161">Click **Connect** tooopen hello window for hello virtual machine.</span></span>
3. <span data-ttu-id="6e3c6-162">Hello rendszerhéj parancsot hello "`zypper lr`".</span><span class="sxs-lookup"><span data-stu-id="6e3c6-162">On hello shell, run hello command '`zypper lr`'.</span></span> <span data-ttu-id="6e3c6-163">Ha ez a parancs visszaadja a kimeneti hasonló toohello követve, majd várt a módosítás nélkül szükségesek hello adattárak vannak konfigurálva (vegye figyelembe, hogy verziószáma változhat):</span><span class="sxs-lookup"><span data-stu-id="6e3c6-163">If this command returns output similar toohello following, then hello repositories are configured as expected--no adjustments are necessary (note that version numbers may vary):</span></span>
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    <span data-ttu-id="6e3c6-164">Ha hello parancs visszaadja a "Nincs definiálva... adattárak" használja a következő parancsok tooadd hello ezek repók:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-164">If hello command returns "No repositories defined..." then use hello following commands tooadd these repos:</span></span>
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    <span data-ttu-id="6e3c6-165">Ezután ellenőrizheti hello adattárak hello parancs futtatásával hozzáadott "`zypper lr`" újra.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-165">You can then verify hello repositories have been added by running hello command '`zypper lr`' again.</span></span> <span data-ttu-id="6e3c6-166">Abban az esetben, ha egy hello vonatkozó update adattárak nincs engedélyezve, engedélyezze a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-166">In case one of hello relevant update repositories is not enabled, enable it with following command:</span></span>
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. <span data-ttu-id="6e3c6-167">Hello kernel toohello elérhető legújabb verzióra frissíteni:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-167">Update hello kernel toohello latest available version:</span></span>
   
        # sudo zypper up kernel-default
   
    <span data-ttu-id="6e3c6-168">Vagy tooupdate hello rendszer hello legújabb javításokat:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-168">Or tooupdate hello system with all hello latest patches:</span></span>
   
        # sudo zypper update
5. <span data-ttu-id="6e3c6-169">Hello Azure Linux ügynök telepítése.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-169">Install hello Azure Linux Agent.</span></span>
   
   # <a name="sudo-zypper-install-walinuxagent"></a><span data-ttu-id="6e3c6-170">sudo zypper telepítés WALinuxAgent</span><span class="sxs-lookup"><span data-stu-id="6e3c6-170">sudo zypper install WALinuxAgent</span></span>
6. <span data-ttu-id="6e3c6-171">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-171">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="6e3c6-172">toodo a, nyissa meg "/ boot/grub/menu.lst" egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-172">toodo this, open "/boot/grub/menu.lst" in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
   
     <span data-ttu-id="6e3c6-173">konzol ttyS0 earlyprintk = ttyS0 rootdelay = = 300</span><span class="sxs-lookup"><span data-stu-id="6e3c6-173">console=ttyS0 earlyprintk=ttyS0 rootdelay=300</span></span>
   
   <span data-ttu-id="6e3c6-174">Ezzel biztosíthatja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-174">This will ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="6e3c6-175">Ezenkívül távolítsa el a következő paraméterek hello kernel rendszerindító sor, ha vannak ilyenek hello:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-175">In addition, remove hello following parameters from hello kernel boot line if they exist:</span></span>
   
     <span data-ttu-id="6e3c6-176">libata.atapi_enabled=0 tartalék = 0x1f0, 0x8</span><span class="sxs-lookup"><span data-stu-id="6e3c6-176">libata.atapi_enabled=0 reserve=0x1f0,0x8</span></span>
7. <span data-ttu-id="6e3c6-177">Ajánlott tooedit hello fájl "/ etc/sysconfig/hálózati/dhcp", és módosítsa a hello `DHCLIENT_SET_HOSTNAME` paraméter toohello következő:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-177">It is recommended tooedit hello file "/etc/sysconfig/network/dhcp" and change hello `DHCLIENT_SET_HOSTNAME` parameter toohello following:</span></span>
   
     <span data-ttu-id="6e3c6-178">DHCLIENT_SET_HOSTNAME = "nem"</span><span class="sxs-lookup"><span data-stu-id="6e3c6-178">DHCLIENT_SET_HOSTNAME="no"</span></span>
8. <span data-ttu-id="6e3c6-179">**Fontos:** "/ etc/sudoers", a megjegyzéssé, vagy távolítsa el az alábbi sorokat, ha vannak ilyenek hello:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-179">**Important:** In "/etc/sudoers", comment out or remove hello following lines if they exist:</span></span>
   
     <span data-ttu-id="6e3c6-180">Alapértelmezés szerint targetpw # hello célként megadott felhasználó összes ALL=(ALL) összes azaz gyökér hello jelszó kérése # figyelmeztetés!</span><span class="sxs-lookup"><span data-stu-id="6e3c6-180">Defaults targetpw   # ask for hello password of hello target user i.e. root ALL    ALL=(ALL) ALL   # WARNING!</span></span> <span data-ttu-id="6e3c6-181">Csak ezzel együtt a 'Alapértelmezett targetpw'!</span><span class="sxs-lookup"><span data-stu-id="6e3c6-181">Only use this together with 'Defaults targetpw'!</span></span>
9. <span data-ttu-id="6e3c6-182">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-182">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="6e3c6-183">Ez általában akkor hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-183">This is usually hello default.</span></span>
10. <span data-ttu-id="6e3c6-184">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-184">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="6e3c6-185">hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-185">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="6e3c6-186">Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-186">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="6e3c6-187">Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-187">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
     <span data-ttu-id="6e3c6-188">ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Megjegyzés: a szükséges toobe toowhatever beállítása.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-188">ResourceDisk.Format=y  ResourceDisk.Filesystem=ext4  ResourceDisk.MountPoint=/mnt/resource  ResourceDisk.EnableSwap=y  ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.</span></span>
11. <span data-ttu-id="6e3c6-189">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-189">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
    # <a name="sudo-waagent--force--deprovision"></a><span data-ttu-id="6e3c6-190">sudo waagent-force - deprovision</span><span class="sxs-lookup"><span data-stu-id="6e3c6-190">sudo waagent -force -deprovision</span></span>
    # <a name="export-histsize0"></a><span data-ttu-id="6e3c6-191">exportálja a HISTSIZE = 0</span><span class="sxs-lookup"><span data-stu-id="6e3c6-191">export HISTSIZE=0</span></span>
    # <a name="logout"></a><span data-ttu-id="6e3c6-192">Kijelentkezés</span><span class="sxs-lookup"><span data-stu-id="6e3c6-192">logout</span></span>
12. <span data-ttu-id="6e3c6-193">Ellenőrizze, hogy hello Azure Linux ügynök fut, a rendszer indításakor:</span><span class="sxs-lookup"><span data-stu-id="6e3c6-193">Ensure hello Azure Linux Agent runs at startup:</span></span>
    
        # sudo systemctl enable waagent.service
13. <span data-ttu-id="6e3c6-194">Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-194">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="6e3c6-195">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-195">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e3c6-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6e3c6-196">Next steps</span></span>
<span data-ttu-id="6e3c6-197">Ön éppen most már készen áll a toouse a SUSE Linux virtuális merevlemez toocreate új virtuális gépeket az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="6e3c6-197">You're now ready toouse your SUSE Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="6e3c6-198">Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6e3c6-198">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

