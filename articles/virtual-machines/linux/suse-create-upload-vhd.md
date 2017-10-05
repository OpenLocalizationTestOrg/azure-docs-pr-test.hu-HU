---
title: "Létrehozása és feltöltése az Azure-ban SUSE Linux virtuális merevlemez"
description: "Ismerje meg, létrehozása és feltöltése az Azure virtuális merevlemez (VHD), amely tartalmazza a SUSE Linux operációs rendszert."
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
ms.openlocfilehash: c829f5d9a90b4260c6f41b2d9e511a0c6cb48f18
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="prepare-a-sles-or-opensuse-virtual-machine-for-azure"></a><span data-ttu-id="5a630-103">SLES- vagy openSUSE-alapú virtuális gép előkészítése Azure-beli használatra</span><span class="sxs-lookup"><span data-stu-id="5a630-103">Prepare a SLES or openSUSE virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="5a630-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5a630-104">Prerequisites</span></span>
<span data-ttu-id="5a630-105">Ez a cikk feltételezi, hogy már telepítve van egy SUSE vagy openSUSE Linux operációs rendszer egy virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="5a630-105">This article assumes that you have already installed a SUSE or openSUSE Linux operating system to a virtual hard disk.</span></span> <span data-ttu-id="5a630-106">Több különféle eszköz található .vhd fájlok, például egy például a Hyper-V virtualizálási megoldás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5a630-106">Multiple tools exist to create .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="5a630-107">Útmutatásért lásd: [a Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="5a630-107">For instructions, see [Install the Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="sles--opensuse-installation-notes"></a><span data-ttu-id="5a630-108">SLES / openSUSE telepítési megjegyzések</span><span class="sxs-lookup"><span data-stu-id="5a630-108">SLES / openSUSE installation notes</span></span>
* <span data-ttu-id="5a630-109">Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.</span><span class="sxs-lookup"><span data-stu-id="5a630-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="5a630-110">A VHDX formátum nem támogatott az Azure csak **rögzített VHD**.</span><span class="sxs-lookup"><span data-stu-id="5a630-110">The VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="5a630-111">Átválthat a lemez VHD formátumú Hyper-V kezelője vagy a convert-vhd-parancsmag segítségével.</span><span class="sxs-lookup"><span data-stu-id="5a630-111">You can convert the disk to VHD format using Hyper-V Manager or the convert-vhd cmdlet.</span></span>
* <span data-ttu-id="5a630-112">A Linux rendszer telepítésekor LVM (gyakran sok telepítés alapértelmezett), hanem szabványos partíciók használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="5a630-112">When installing the Linux system it is recommended that you use standard partitions rather than LVM (often the default for many installations).</span></span> <span data-ttu-id="5a630-113">LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációsrendszer-lemez legalább egyszer meg kell hibaelhárítási egy másik virtuális géphez csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="5a630-113">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs to be attached to another VM for troubleshooting.</span></span> <span data-ttu-id="5a630-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ha előnyben részesített adatlemezek használható.</span><span class="sxs-lookup"><span data-stu-id="5a630-114">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="5a630-115">Ne konfiguráljon egy swap partíciót az operációsrendszer-lemezképet.</span><span class="sxs-lookup"><span data-stu-id="5a630-115">Do not configure a swap partition on the OS disk.</span></span> <span data-ttu-id="5a630-116">A Linux-ügynök beállítható úgy, hogy az ideiglenes erőforrás lemezen a lapozófájl létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5a630-116">The Linux agent can be configured to create a swap file on the temporary resource disk.</span></span>  <span data-ttu-id="5a630-117">További információk a megtalálhatók az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5a630-117">More information about this can be found in the steps below.</span></span>
* <span data-ttu-id="5a630-118">Összes, a virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.</span><span class="sxs-lookup"><span data-stu-id="5a630-118">All of the VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="use-suse-studio"></a><span data-ttu-id="5a630-119">SUSE studióval</span><span class="sxs-lookup"><span data-stu-id="5a630-119">Use SUSE Studio</span></span>
<span data-ttu-id="5a630-120">[SUSE Studio](http://www.susestudio.com) könnyen létrehozása és a SLES és openSUSE lemezképek kezelése az Azure és a Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="5a630-120">[SUSE Studio](http://www.susestudio.com) can easily create and manage your SLES and openSUSE images for Azure and Hyper-V.</span></span> <span data-ttu-id="5a630-121">Ez az az ajánlott módszer a saját SLES és openSUSE lemezképek testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="5a630-121">This is the recommended approach for customizing your own SLES and openSUSE images.</span></span>

<span data-ttu-id="5a630-122">A saját virtuális merevlemez létrehozása helyett, SUSE is közzéteszi (Bring Your saját előfizetés) saját lemezképek a következő SLES [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).</span><span class="sxs-lookup"><span data-stu-id="5a630-122">As an alternative to building your own VHD, SUSE also publishes BYOS (Bring Your Own Subscription) images for SLES at [VMDepot](https://vmdepot.msopentech.com/User/Show?user=1007).</span></span>

## <a name="prepare-suse-linux-enterprise-server-11-sp4"></a><span data-ttu-id="5a630-123">SUSE Linux Enterprise Server 11 SP4 előkészítése</span><span class="sxs-lookup"><span data-stu-id="5a630-123">Prepare SUSE Linux Enterprise Server 11 SP4</span></span>
1. <span data-ttu-id="5a630-124">A középső ablaktáblán a Hyper-V kezelőjében válassza ki a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="5a630-124">In the center pane of Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="5a630-125">Kattintson a **Connect** a virtuális gép ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="5a630-125">Click **Connect** to open the window for the virtual machine.</span></span>
3. <span data-ttu-id="5a630-126">Regisztrálja a SUSE Linux Enterprise rendszer engedélyezze a frissítések letöltése és telepítése a csomagokat.</span><span class="sxs-lookup"><span data-stu-id="5a630-126">Register your SUSE Linux Enterprise system to allow it to download updates and install packages.</span></span>
4. <span data-ttu-id="5a630-127">Frissítés a rendszer, amely a legújabb javítások:</span><span class="sxs-lookup"><span data-stu-id="5a630-127">Update the system with the latest patches:</span></span>
   
        # sudo zypper update
5. <span data-ttu-id="5a630-128">Az Azure Linux ügynök telepítése a SLES tárházból:</span><span class="sxs-lookup"><span data-stu-id="5a630-128">Install the Azure Linux Agent from the SLES repository:</span></span>
   
        # sudo zypper install WALinuxAgent
6. <span data-ttu-id="5a630-129">Ha waagent van-e állítva az "on" beadása chkconfig, és ha nem, engedélyezze az automatikus indítás:</span><span class="sxs-lookup"><span data-stu-id="5a630-129">Check if waagent is set to "on" in chkconfig, and if not, enable it for autostart:</span></span>
   
        # sudo chkconfig waagent on
7. <span data-ttu-id="5a630-130">Ellenőrizze, hogy a waagent-szolgáltatás fut, és ha nem, indítsa el:</span><span class="sxs-lookup"><span data-stu-id="5a630-130">Check if waagent service is running, and if not, start it:</span></span> 
   
        # sudo service waagent start
8. <span data-ttu-id="5a630-131">Módosítsa a kernel rendszerindító sor lárvajárat konfigurációs kiegészítő rendszermag paraméterek az Azure-bA felvenni.</span><span class="sxs-lookup"><span data-stu-id="5a630-131">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="5a630-132">Ehhez a Megnyitás "/ boot/grub/menu.lst" egy szövegszerkesztőben, és győződjön meg arról, hogy az alapértelmezett kernel az alábbi paramétereket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5a630-132">To do this open "/boot/grub/menu.lst" in a text editor and ensure that the default kernel includes the following parameters:</span></span>
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
   
    <span data-ttu-id="5a630-133">Ezzel biztosíthatja, hogy minden konzol küldi el az első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="5a630-133">This will ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span>
9. <span data-ttu-id="5a630-134">Győződjön meg arról, hogy /boot/grub/menu.lst és /etc/fstab is hivatkoznak a lemez az UUID (által-uuid) helyett a Lemezazonosítót (-azonosító szerint).</span><span class="sxs-lookup"><span data-stu-id="5a630-134">Confirm that /boot/grub/menu.lst and /etc/fstab both reference the disk using its UUID (by-uuid) instead of the disk ID (by-id).</span></span> 
   
    <span data-ttu-id="5a630-135">Lemezek UUID azonosítója beolvasása</span><span class="sxs-lookup"><span data-stu-id="5a630-135">Get disk UUID</span></span>
   
        # ls /dev/disk/by-uuid/
   
    <span data-ttu-id="5a630-136">Ha /dev/disk/by-id / /boot/grub/menu.lst mind a/etc/fstab használ, frissítse a megfelelő által-uuid-értékkel rendelkező</span><span class="sxs-lookup"><span data-stu-id="5a630-136">If /dev/disk/by-id/ is used, update both /boot/grub/menu.lst and /etc/fstab with the proper by-uuid value</span></span>
   
    <span data-ttu-id="5a630-137">Változás előtt</span><span class="sxs-lookup"><span data-stu-id="5a630-137">Before change</span></span>
   
        root=/dev/disk/by-id/SCSI-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx-part1
   
    <span data-ttu-id="5a630-138">Módosítás után</span><span class="sxs-lookup"><span data-stu-id="5a630-138">After change</span></span>
   
        root=/dev/disk/by-uuid/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
10. <span data-ttu-id="5a630-139">Az Ethernet-adaptert statikus szabályainak elkerülése érdekében udev szabályok módosítása.</span><span class="sxs-lookup"><span data-stu-id="5a630-139">Modify udev rules to avoid generating static rules for the Ethernet interface(s).</span></span> <span data-ttu-id="5a630-140">Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v virtuális gép klónozása</span><span class="sxs-lookup"><span data-stu-id="5a630-140">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
    
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
11. <span data-ttu-id="5a630-141">A fájl szerkesztése ajánlott "/ etc/sysconfig/hálózati/dhcp", és módosítsa a `DHCLIENT_SET_HOSTNAME` a következő paramétert:</span><span class="sxs-lookup"><span data-stu-id="5a630-141">It is recommended to edit the file "/etc/sysconfig/network/dhcp" and change the `DHCLIENT_SET_HOSTNAME` parameter to the following:</span></span>
    
     <span data-ttu-id="5a630-142">DHCLIENT_SET_HOSTNAME = "nem"</span><span class="sxs-lookup"><span data-stu-id="5a630-142">DHCLIENT_SET_HOSTNAME="no"</span></span>
12. <span data-ttu-id="5a630-143">A "/ etc/sudoers" megjegyzéssé, vagy ha vannak ilyenek, távolítsa el a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="5a630-143">In "/etc/sudoers", comment out or remove the following lines if they exist:</span></span>
    
     <span data-ttu-id="5a630-144">Alapértelmezés szerint targetpw # kérje meg a jelszót, azaz legfelső szintű összes ALL=(ALL) minden célként megadott felhasználó # figyelmeztetés!</span><span class="sxs-lookup"><span data-stu-id="5a630-144">Defaults targetpw   # ask for the password of the target user i.e. root ALL    ALL=(ALL) ALL   # WARNING!</span></span> <span data-ttu-id="5a630-145">Csak ezzel együtt a 'Alapértelmezett targetpw'!</span><span class="sxs-lookup"><span data-stu-id="5a630-145">Only use this together with 'Defaults targetpw'!</span></span>
13. <span data-ttu-id="5a630-146">Győződjön meg arról, hogy az SSH-kiszolgálót telepítse és konfigurálja a rendszerindítás elindításához.</span><span class="sxs-lookup"><span data-stu-id="5a630-146">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="5a630-147">Ez általában az alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="5a630-147">This is usually the default.</span></span>
14. <span data-ttu-id="5a630-148">Ne hozzon létre az operációsrendszer-lemezképet a lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="5a630-148">Do not create swap space on the OS disk.</span></span>
    
    <span data-ttu-id="5a630-149">Az Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata a helyi erőforrás lemezt a virtuális Géphez csatolt Azure kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="5a630-149">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="5a630-150">Vegye figyelembe, hogy a helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy szerepelnek, ha a virtuális gép van platformelőfizetés.</span><span class="sxs-lookup"><span data-stu-id="5a630-150">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="5a630-151">Az Azure Linux ügynök telepítése után (lásd az előző lépésben), annak megfelelően módosítsa a /etc/waagent.conf a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="5a630-151">After installing the Azure Linux Agent (see previous step), modify the following parameters in /etc/waagent.conf appropriately:</span></span>
    
     <span data-ttu-id="5a630-152">ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Megjegyzés: állítsa függetlenül esetleg szükség lenne rá kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5a630-152">ResourceDisk.Format=y  ResourceDisk.Filesystem=ext4  ResourceDisk.MountPoint=/mnt/resource  ResourceDisk.EnableSwap=y  ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.</span></span>
15. <span data-ttu-id="5a630-153">A virtuális gép kiosztásának megszüntetése, és előkészíti az Azure-on történő üzembe helyezéséhez a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="5a630-153">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
    
    # <a name="sudo-waagent--force--deprovision"></a><span data-ttu-id="5a630-154">sudo waagent-force - deprovision</span><span class="sxs-lookup"><span data-stu-id="5a630-154">sudo waagent -force -deprovision</span></span>
    # <a name="export-histsize0"></a><span data-ttu-id="5a630-155">exportálja a HISTSIZE = 0</span><span class="sxs-lookup"><span data-stu-id="5a630-155">export HISTSIZE=0</span></span>
    # <a name="logout"></a><span data-ttu-id="5a630-156">Kijelentkezés</span><span class="sxs-lookup"><span data-stu-id="5a630-156">logout</span></span>
16. <span data-ttu-id="5a630-157">Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="5a630-157">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="5a630-158">A Linux virtuális merevlemez az Azure-bA feltölteni kívánt készen áll.</span><span class="sxs-lookup"><span data-stu-id="5a630-158">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

- - -
## <a name="prepare-opensuse-131"></a><span data-ttu-id="5a630-159">OpenSUSE 13.1 + előkészítése</span><span class="sxs-lookup"><span data-stu-id="5a630-159">Prepare openSUSE 13.1+</span></span>
1. <span data-ttu-id="5a630-160">A középső ablaktáblán a Hyper-V kezelőjében válassza ki a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="5a630-160">In the center pane of Hyper-V Manager, select the virtual machine.</span></span>
2. <span data-ttu-id="5a630-161">Kattintson a **Connect** a virtuális gép ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="5a630-161">Click **Connect** to open the window for the virtual machine.</span></span>
3. <span data-ttu-id="5a630-162">Futtassa a parancsot a rendszerhéj "`zypper lr`".</span><span class="sxs-lookup"><span data-stu-id="5a630-162">On the shell, run the command '`zypper lr`'.</span></span> <span data-ttu-id="5a630-163">Ha ez a parancs visszaadja kimenet az alábbihoz hasonló, akkor a tárolóhelyekkel konfigurált elvárt – módosítás nélkül szükség (vegye figyelembe, hogy a verziószáma változhat):</span><span class="sxs-lookup"><span data-stu-id="5a630-163">If this command returns output similar to the following, then the repositories are configured as expected--no adjustments are necessary (note that version numbers may vary):</span></span>
   
        # | Alias                 | Name                  | Enabled | Refresh
        --+-----------------------+-----------------------+---------+--------
        1 | Cloud:Tools_13.1      | Cloud:Tools_13.1      | Yes     | Yes
        2 | openSUSE_13.1_OSS     | openSUSE_13.1_OSS     | Yes     | Yes
        3 | openSUSE_13.1_Updates | openSUSE_13.1_Updates | Yes     | Yes
   
    <span data-ttu-id="5a630-164">Ha a parancs visszaadja a "Nincs definiálva... adattárak" használja az alábbi parancsokat a repók hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="5a630-164">If the command returns "No repositories defined..." then use the following commands to add these repos:</span></span>
   
        # sudo zypper ar -f http://download.opensuse.org/repositories/Cloud:Tools/openSUSE_13.1 Cloud:Tools_13.1
        # sudo zypper ar -f http://download.opensuse.org/distribution/13.1/repo/oss openSUSE_13.1_OSS
        # sudo zypper ar -f http://download.opensuse.org/update/13.1 openSUSE_13.1_Updates
   
    <span data-ttu-id="5a630-165">Ezután ellenőrizheti a tárolóhelyekkel lettek hozzáadva a parancs futtatásával "`zypper lr`" újra.</span><span class="sxs-lookup"><span data-stu-id="5a630-165">You can then verify the repositories have been added by running the command '`zypper lr`' again.</span></span> <span data-ttu-id="5a630-166">Abban az esetben, ha egy, a megfelelő frissítési adattárak nincs engedélyezve, engedélyezze a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="5a630-166">In case one of the relevant update repositories is not enabled, enable it with following command:</span></span>
   
        # sudo zypper mr -e [NUMBER OF REPOSITORY]
4. <span data-ttu-id="5a630-167">Frissítés a legújabb elérhető verziójára kernel:</span><span class="sxs-lookup"><span data-stu-id="5a630-167">Update the kernel to the latest available version:</span></span>
   
        # sudo zypper up kernel-default
   
    <span data-ttu-id="5a630-168">Vagy a rendszer frissítése a legújabb javítások:</span><span class="sxs-lookup"><span data-stu-id="5a630-168">Or to update the system with all the latest patches:</span></span>
   
        # sudo zypper update
5. <span data-ttu-id="5a630-169">Az Azure Linux ügynök telepítése.</span><span class="sxs-lookup"><span data-stu-id="5a630-169">Install the Azure Linux Agent.</span></span>
   
   # <a name="sudo-zypper-install-walinuxagent"></a><span data-ttu-id="5a630-170">sudo zypper telepítés WALinuxAgent</span><span class="sxs-lookup"><span data-stu-id="5a630-170">sudo zypper install WALinuxAgent</span></span>
6. <span data-ttu-id="5a630-171">Módosítsa a kernel rendszerindító sor lárvajárat konfigurációs kiegészítő rendszermag paraméterek az Azure-bA felvenni.</span><span class="sxs-lookup"><span data-stu-id="5a630-171">Modify the kernel boot line in your grub configuration to include additional kernel parameters for Azure.</span></span> <span data-ttu-id="5a630-172">Ehhez nyissa meg a "/ boot/grub/menu.lst" egy szövegszerkesztőben, és győződjön meg arról, hogy az alapértelmezett kernel az alábbi paramétereket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5a630-172">To do this, open "/boot/grub/menu.lst" in a text editor and ensure that the default kernel includes the following parameters:</span></span>
   
     <span data-ttu-id="5a630-173">konzol ttyS0 earlyprintk = ttyS0 rootdelay = = 300</span><span class="sxs-lookup"><span data-stu-id="5a630-173">console=ttyS0 earlyprintk=ttyS0 rootdelay=300</span></span>
   
   <span data-ttu-id="5a630-174">Ezzel biztosíthatja, hogy minden konzol küldi el az első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="5a630-174">This will ensure all console messages are sent to the first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="5a630-175">A következő paraméterek ezenkívül eltávolítása a kernel rendszerindító sor, ha vannak ilyenek:</span><span class="sxs-lookup"><span data-stu-id="5a630-175">In addition, remove the following parameters from the kernel boot line if they exist:</span></span>
   
     <span data-ttu-id="5a630-176">libata.atapi_enabled=0 tartalék = 0x1f0, 0x8</span><span class="sxs-lookup"><span data-stu-id="5a630-176">libata.atapi_enabled=0 reserve=0x1f0,0x8</span></span>
7. <span data-ttu-id="5a630-177">A fájl szerkesztése ajánlott "/ etc/sysconfig/hálózati/dhcp", és módosítsa a `DHCLIENT_SET_HOSTNAME` a következő paramétert:</span><span class="sxs-lookup"><span data-stu-id="5a630-177">It is recommended to edit the file "/etc/sysconfig/network/dhcp" and change the `DHCLIENT_SET_HOSTNAME` parameter to the following:</span></span>
   
     <span data-ttu-id="5a630-178">DHCLIENT_SET_HOSTNAME = "nem"</span><span class="sxs-lookup"><span data-stu-id="5a630-178">DHCLIENT_SET_HOSTNAME="no"</span></span>
8. <span data-ttu-id="5a630-179">**Fontos:** "/ etc/sudoers", a megjegyzéssé, vagy ha vannak ilyenek, távolítsa el a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="5a630-179">**Important:** In "/etc/sudoers", comment out or remove the following lines if they exist:</span></span>
   
     <span data-ttu-id="5a630-180">Alapértelmezés szerint targetpw # kérje meg a jelszót, azaz legfelső szintű összes ALL=(ALL) minden célként megadott felhasználó # figyelmeztetés!</span><span class="sxs-lookup"><span data-stu-id="5a630-180">Defaults targetpw   # ask for the password of the target user i.e. root ALL    ALL=(ALL) ALL   # WARNING!</span></span> <span data-ttu-id="5a630-181">Csak ezzel együtt a 'Alapértelmezett targetpw'!</span><span class="sxs-lookup"><span data-stu-id="5a630-181">Only use this together with 'Defaults targetpw'!</span></span>
9. <span data-ttu-id="5a630-182">Győződjön meg arról, hogy az SSH-kiszolgálót telepítse és konfigurálja a rendszerindítás elindításához.</span><span class="sxs-lookup"><span data-stu-id="5a630-182">Ensure that the SSH server is installed and configured to start at boot time.</span></span>  <span data-ttu-id="5a630-183">Ez általában az alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="5a630-183">This is usually the default.</span></span>
10. <span data-ttu-id="5a630-184">Ne hozzon létre az operációsrendszer-lemezképet a lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="5a630-184">Do not create swap space on the OS disk.</span></span>
    
    <span data-ttu-id="5a630-185">Az Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata a helyi erőforrás lemezt a virtuális Géphez csatolt Azure kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="5a630-185">The Azure Linux Agent can automatically configure swap space using the local resource disk that is attached to the VM after provisioning on Azure.</span></span> <span data-ttu-id="5a630-186">Vegye figyelembe, hogy a helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy szerepelnek, ha a virtuális gép van platformelőfizetés.</span><span class="sxs-lookup"><span data-stu-id="5a630-186">Note that the local resource disk is a *temporary* disk, and might be emptied when the VM is deprovisioned.</span></span> <span data-ttu-id="5a630-187">Az Azure Linux ügynök telepítése után (lásd az előző lépésben), annak megfelelően módosítsa a /etc/waagent.conf a következő paraméterekkel:</span><span class="sxs-lookup"><span data-stu-id="5a630-187">After installing the Azure Linux Agent (see previous step), modify the following parameters in /etc/waagent.conf appropriately:</span></span>
    
     <span data-ttu-id="5a630-188">ResourceDisk.Format=y ResourceDisk.Filesystem=ext4 ResourceDisk.MountPoint=/mnt/resource ResourceDisk.EnableSwap=y ResourceDisk.SwapSizeMB=&#2048;# Megjegyzés: állítsa függetlenül esetleg szükség lenne rá kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5a630-188">ResourceDisk.Format=y  ResourceDisk.Filesystem=ext4  ResourceDisk.MountPoint=/mnt/resource  ResourceDisk.EnableSwap=y  ResourceDisk.SwapSizeMB=2048    ## NOTE: set this to whatever you need it to be.</span></span>
11. <span data-ttu-id="5a630-189">A virtuális gép kiosztásának megszüntetése, és előkészíti az Azure-on történő üzembe helyezéséhez a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="5a630-189">Run the following commands to deprovision the virtual machine and prepare it for provisioning on Azure:</span></span>
    
    # <a name="sudo-waagent--force--deprovision"></a><span data-ttu-id="5a630-190">sudo waagent-force - deprovision</span><span class="sxs-lookup"><span data-stu-id="5a630-190">sudo waagent -force -deprovision</span></span>
    # <a name="export-histsize0"></a><span data-ttu-id="5a630-191">exportálja a HISTSIZE = 0</span><span class="sxs-lookup"><span data-stu-id="5a630-191">export HISTSIZE=0</span></span>
    # <a name="logout"></a><span data-ttu-id="5a630-192">Kijelentkezés</span><span class="sxs-lookup"><span data-stu-id="5a630-192">logout</span></span>
12. <span data-ttu-id="5a630-193">Győződjön meg arról, az Azure Linux ügynök a indításakor fut:</span><span class="sxs-lookup"><span data-stu-id="5a630-193">Ensure the Azure Linux Agent runs at startup:</span></span>
    
        # sudo systemctl enable waagent.service
13. <span data-ttu-id="5a630-194">Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="5a630-194">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="5a630-195">A Linux virtuális merevlemez az Azure-bA feltölteni kívánt készen áll.</span><span class="sxs-lookup"><span data-stu-id="5a630-195">Your Linux VHD is now ready to be uploaded to Azure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a630-196">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a630-196">Next steps</span></span>
<span data-ttu-id="5a630-197">Most már készen áll a SUSE Linux virtuális merevlemez segítségével új virtuális gépek létrehozása az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="5a630-197">You're now ready to use your SUSE Linux virtual hard disk to create new virtual machines in Azure.</span></span> <span data-ttu-id="5a630-198">Ha az első alkalommal, hogy van-e a .vhd fájl feltöltése az Azure, tekintse meg a 2. és 3 [létrehozása és feltöltése, a Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5a630-198">If this is the first time that you're uploading the .vhd file to Azure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains the Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

