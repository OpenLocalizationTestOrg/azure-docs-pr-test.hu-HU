---
title: "aaaCreate és az Oracle Linux virtuális merevlemez feltöltéséhez |} Microsoft Docs"
description: "Toocreate ismerje meg, és töltse fel az Azure virtuális merevlemez (VHD), amely tartalmazza az Oracle Linux operációs rendszer."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: dd96f771-26eb-4391-9a89-8c8b6d691822
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: szark
ms.openlocfilehash: be9cf284d7f5e7122a106506a343e53e9f1ac56e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-an-oracle-linux-virtual-machine-for-azure"></a><span data-ttu-id="7a70d-103">Oracle Linux-alapú virtuális gép előkészítése Azure-beli használatra</span><span class="sxs-lookup"><span data-stu-id="7a70d-103">Prepare an Oracle Linux virtual machine for Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="7a70d-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7a70d-104">Prerequisites</span></span>
<span data-ttu-id="7a70d-105">Ez a cikk feltételezi, hogy az Oracle Linux operációs rendszer tooa virtuális merevlemez már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="7a70d-105">This article assumes that you have already installed an Oracle Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="7a70d-106">Több különféle eszköz toocreate .vhd fájlok, például egy hálózatvirtualizálási megoldás például a Hyper-V létezik.</span><span class="sxs-lookup"><span data-stu-id="7a70d-106">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="7a70d-107">Útmutatásért lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="7a70d-107">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

### <a name="oracle-linux-installation-notes"></a><span data-ttu-id="7a70d-108">Oracle Linux telepítési megjegyzések</span><span class="sxs-lookup"><span data-stu-id="7a70d-108">Oracle Linux installation notes</span></span>
* <span data-ttu-id="7a70d-109">Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.</span><span class="sxs-lookup"><span data-stu-id="7a70d-109">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="7a70d-110">Red Hat kompatibilis kernel Oracle és azok UEK3 (szoros vállalati Kernel) is támogatott a Hyper-V és az Azure.</span><span class="sxs-lookup"><span data-stu-id="7a70d-110">Oracle's Red Hat compatible kernel and their UEK3 (Unbreakable Enterprise Kernel) are both supported on Hyper-V and Azure.</span></span> <span data-ttu-id="7a70d-111">A legjobb eredmények elérése érdekében Felhívjuk meg arról, hogy tooupdate toohello legújabb kernel az Oracle Linux virtuális merevlemez előkészítése során.</span><span class="sxs-lookup"><span data-stu-id="7a70d-111">For best results, please be sure tooupdate toohello latest kernel while preparing your Oracle Linux VHD.</span></span>
* <span data-ttu-id="7a70d-112">Oracle UEK2 nem támogatott a Hyper-V és az Azure, nem tartalmazza a szükséges hello illesztőprogramokat.</span><span class="sxs-lookup"><span data-stu-id="7a70d-112">Oracle's UEK2 is not supported on Hyper-V and Azure as it does not include hello required drivers.</span></span>
* <span data-ttu-id="7a70d-113">hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**.</span><span class="sxs-lookup"><span data-stu-id="7a70d-113">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="7a70d-114">Alakítsa át a Hyper-V kezelőjével hello tooVHD formátummal, vagy convert-vhd parancsmag hello.</span><span class="sxs-lookup"><span data-stu-id="7a70d-114">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span>
* <span data-ttu-id="7a70d-115">Hello Linux rendszer telepítésekor LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="7a70d-115">When installing hello Linux system it is recommended that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="7a70d-116">LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációs rendszer lemezén legalább egyszer kell csatolt toobe tooanother VM hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="7a70d-116">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother VM for troubleshooting.</span></span> <span data-ttu-id="7a70d-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) Ha előnyben részesített adatlemezek használható.</span><span class="sxs-lookup"><span data-stu-id="7a70d-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks if preferred.</span></span>
* <span data-ttu-id="7a70d-118">NUMA nagyobb Virtuálisgép-méretek Linux kernel verziójánál régebbi 2.6.37 tooa hibája miatt nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="7a70d-118">NUMA is not supported for larger VM sizes due tooa bug in Linux kernel versions below 2.6.37.</span></span> <span data-ttu-id="7a70d-119">Ez elsősorban az azokat a terjesztéseket használatával hello fölérendelt piros hatások ki Hat 2.6.32 kernel.</span><span class="sxs-lookup"><span data-stu-id="7a70d-119">This issue primarily impacts distributions using hello upstream Red Hat 2.6.32 kernel.</span></span> <span data-ttu-id="7a70d-120">Manuális telepítés hello Azure Linux-ügynök (waagent) automatikusan letiltja a NUMA hello Linux kernel hello LÁRVAJÁRAT konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="7a70d-120">Manual installation of hello Azure Linux agent (waagent) will automatically disable NUMA in hello GRUB configuration for hello Linux kernel.</span></span> <span data-ttu-id="7a70d-121">További információk a hello lépéseket találhatók.</span><span class="sxs-lookup"><span data-stu-id="7a70d-121">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="7a70d-122">Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel.</span><span class="sxs-lookup"><span data-stu-id="7a70d-122">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="7a70d-123">Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.</span><span class="sxs-lookup"><span data-stu-id="7a70d-123">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="7a70d-124">További információk a hello lépéseket találhatók.</span><span class="sxs-lookup"><span data-stu-id="7a70d-124">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="7a70d-125">Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.</span><span class="sxs-lookup"><span data-stu-id="7a70d-125">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>
* <span data-ttu-id="7a70d-126">Győződjön meg arról, hogy hello `Addons` tárház engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="7a70d-126">Make sure that hello `Addons` repository is enabled.</span></span> <span data-ttu-id="7a70d-127">Hello fájl szerkesztésével `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) vagy `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux), és módosítsa a hello sor `enabled=0` túl`enabled=1` alatt **[ol6_addons]** vagy **[ol7_addons]** ezen fájl.</span><span class="sxs-lookup"><span data-stu-id="7a70d-127">Edit hello file `/etc/yum.repo.d/public-yum-ol6.repo`(Oracle Linux 6) or `/etc/yum.repo.d/public-yum-ol7.repo`(Oracle Linux ), and change hello line `enabled=0` too`enabled=1` under **[ol6_addons]** or **[ol7_addons]** in this file.</span></span>

## <a name="oracle-linux-64"></a><span data-ttu-id="7a70d-128">Oracle Linux 6.4 +</span><span class="sxs-lookup"><span data-stu-id="7a70d-128">Oracle Linux 6.4+</span></span>
<span data-ttu-id="7a70d-129">Hello operációs rendszer hello virtuális gép toorun az Azure-ban az adott konfigurációs lépések elvégzése után.</span><span class="sxs-lookup"><span data-stu-id="7a70d-129">You must complete specific configuration steps in hello operating system for hello virtual machine toorun in Azure.</span></span>

1. <span data-ttu-id="7a70d-130">Hyper-V kezelője hello középső ablaktáblában jelölje ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="7a70d-130">In hello center pane of Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="7a70d-131">Kattintson a **Connect** tooopen hello ablak hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="7a70d-131">Click **Connect** tooopen hello window for hello virtual machine.</span></span>
3. <span data-ttu-id="7a70d-132">Távolítsa el a NetworkManager hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7a70d-132">Uninstall NetworkManager by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager
   
    <span data-ttu-id="7a70d-133">**Megjegyzés:** hello csomag nincs telepítve, ha ez a parancs a hibaüzenettel meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="7a70d-133">**Note:** If hello package is not already installed, this command will fail with an error message.</span></span> <span data-ttu-id="7a70d-134">Ez várható.</span><span class="sxs-lookup"><span data-stu-id="7a70d-134">This is expected.</span></span>
4. <span data-ttu-id="7a70d-135">Hozzon létre egy fájlt **hálózati** a hello `/etc/sysconfig/` hello a következő szöveget tartalmazó könyvtár:</span><span class="sxs-lookup"><span data-stu-id="7a70d-135">Create a file named **network** in hello `/etc/sysconfig/` directory that contains hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
5. <span data-ttu-id="7a70d-136">Hozzon létre egy fájlt **ifcfg-eth0** a hello `/etc/sysconfig/network-scripts/` hello a következő szöveget tartalmazó könyvtár:</span><span class="sxs-lookup"><span data-stu-id="7a70d-136">Create a file named **ifcfg-eth0** in hello `/etc/sysconfig/network-scripts/` directory that contains hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
6. <span data-ttu-id="7a70d-137">Módosítsa a udev szabályok tooavoid hello Ethernet adaptert statikus szabályainak létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7a70d-137">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="7a70d-138">Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v virtuális gép klónozása</span><span class="sxs-lookup"><span data-stu-id="7a70d-138">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules
7. <span data-ttu-id="7a70d-139">Győződjön meg arról, hello hálózati szolgáltatás indul rendszerindítás hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7a70d-139">Ensure hello network service will start at boot time by running hello following command:</span></span>
   
        # chkconfig network on
8. <span data-ttu-id="7a70d-140">Telepítse a python-pyasn1 hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7a70d-140">Install python-pyasn1 by running hello following command:</span></span>
   
        # sudo yum install python-pyasn1
9. <span data-ttu-id="7a70d-141">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="7a70d-141">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="7a70d-142">a nyitott toodo "/ boot/grub/menu.lst" egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="7a70d-142">toodo this open "/boot/grub/menu.lst" in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
   
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300 numa=off
   
   <span data-ttu-id="7a70d-143">Ez biztosítja az összes konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="7a70d-143">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="7a70d-144">Ezzel a lépéssel letiltja NUMA Oracle Red Hat kompatibilis kernel tooa hiba miatt.</span><span class="sxs-lookup"><span data-stu-id="7a70d-144">This will disable NUMA due tooa bug in Oracle's Red Hat compatible kernel.</span></span>
   
   <span data-ttu-id="7a70d-145">Továbbá a fenti toohello, ajánlott túl*eltávolítása* hello a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="7a70d-145">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
   <span data-ttu-id="7a70d-146">Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="7a70d-146">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>
   
   <span data-ttu-id="7a70d-147">Hello `crashkernel` beállítás lehet, hogy szükség esetén konfigurálva balra, de vegye figyelembe, hogy ez a paraméter csökkenti a hello VM legalább 128 MB, rendelkezésre álló memória mennyisége hello esetleg problémás hello kisebb Virtuálisgép-méretek a.</span><span class="sxs-lookup"><span data-stu-id="7a70d-147">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>
10. <span data-ttu-id="7a70d-148">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7a70d-148">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="7a70d-149">Ez általában akkor hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="7a70d-149">This is usually hello default.</span></span>
11. <span data-ttu-id="7a70d-150">Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával.</span><span class="sxs-lookup"><span data-stu-id="7a70d-150">Install hello Azure Linux Agent by running hello following command.</span></span> <span data-ttu-id="7a70d-151">hello legfrissebb verzió: 2.0.15-ös.</span><span class="sxs-lookup"><span data-stu-id="7a70d-151">hello latest version is 2.0.15.</span></span>
    
        # sudo yum install WALinuxAgent
    
    <span data-ttu-id="7a70d-152">Ne feledje, hogy telepíteni hello WALinuxAgent csomag eltávolítja hello NetworkManager NetworkManager-gnome csomagok Ha nem már eltávolította azokat lásd a 2. lépés.</span><span class="sxs-lookup"><span data-stu-id="7a70d-152">Note that installing hello WALinuxAgent package will remove hello NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 2.</span></span>
12. <span data-ttu-id="7a70d-153">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="7a70d-153">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="7a70d-154">hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="7a70d-154">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="7a70d-155">Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="7a70d-155">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="7a70d-156">Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:</span><span class="sxs-lookup"><span data-stu-id="7a70d-156">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
13. <span data-ttu-id="7a70d-157">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="7a70d-157">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
14. <span data-ttu-id="7a70d-158">Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="7a70d-158">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="7a70d-159">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="7a70d-159">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

- - -
## <a name="oracle-linux-70"></a><span data-ttu-id="7a70d-160">Oracle Linux 7.0 +</span><span class="sxs-lookup"><span data-stu-id="7a70d-160">Oracle Linux 7.0+</span></span>
<span data-ttu-id="7a70d-161">**Oracle Linux 7 változásai**</span><span class="sxs-lookup"><span data-stu-id="7a70d-161">**Changes in Oracle Linux 7**</span></span>

<span data-ttu-id="7a70d-162">Az Oracle Linux 7 virtuális gép előkészítése az Azure-nagyon hasonló tooOracle Linux 6, azonban nincsenek érdemes megjegyezni számos fontos különbség:</span><span class="sxs-lookup"><span data-stu-id="7a70d-162">Preparing an Oracle Linux 7 virtual machine for Azure is very similar tooOracle Linux 6, however there are several important differences worth noting:</span></span>

* <span data-ttu-id="7a70d-163">Red Hat kompatibilis kernel hello és Oracle UEK3 is támogatottak az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7a70d-163">Both hello Red Hat compatible kernel and Oracle's UEK3 are supported in Azure.</span></span>  <span data-ttu-id="7a70d-164">hello UEK3 kernel ajánlott.</span><span class="sxs-lookup"><span data-stu-id="7a70d-164">hello UEK3 kernel is recommended.</span></span>
* <span data-ttu-id="7a70d-165">hello NetworkManager csomag már nem ütközik az hello Azure Linux ügynök.</span><span class="sxs-lookup"><span data-stu-id="7a70d-165">hello NetworkManager package no longer conflicts with hello Azure Linux agent.</span></span> <span data-ttu-id="7a70d-166">Alapértelmezés szerint ez a csomag telepítve van, és azt javasoljuk, hogy nem törli.</span><span class="sxs-lookup"><span data-stu-id="7a70d-166">This package is installed by default and we recommend that it is not removed.</span></span>
* <span data-ttu-id="7a70d-167">GRUB2 most használatos az alapértelmezett rendszertöltő hello így hello eljárás kernel paraméterek szerkesztésre megváltozott (lásd alább).</span><span class="sxs-lookup"><span data-stu-id="7a70d-167">GRUB2 is now used as hello default bootloader, so hello procedure for editing kernel parameters has changed (see below).</span></span>
* <span data-ttu-id="7a70d-168">XFS már hello alapértelmezett fájlrendszert.</span><span class="sxs-lookup"><span data-stu-id="7a70d-168">XFS is now hello default file system.</span></span> <span data-ttu-id="7a70d-169">hello ext4 fájlrendszer továbbra is használható, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="7a70d-169">hello ext4 file system can still be used if desired.</span></span>

<span data-ttu-id="7a70d-170">**Konfigurációs lépések**</span><span class="sxs-lookup"><span data-stu-id="7a70d-170">**Configuration steps**</span></span>

1. <span data-ttu-id="7a70d-171">A Hyper-V kezelőjében válassza ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="7a70d-171">In Hyper-V Manager, select hello virtual machine.</span></span>
2. <span data-ttu-id="7a70d-172">Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="7a70d-172">Click **Connect** tooopen a console window for hello virtual machine.</span></span>
3. <span data-ttu-id="7a70d-173">Hozzon létre egy fájlt **hálózati** a hello `/etc/sysconfig/` hello a következő szöveget tartalmazó könyvtár:</span><span class="sxs-lookup"><span data-stu-id="7a70d-173">Create a file named **network** in hello `/etc/sysconfig/` directory that contains hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain
4. <span data-ttu-id="7a70d-174">Hozzon létre egy fájlt **ifcfg-eth0** a hello `/etc/sysconfig/network-scripts/` hello a következő szöveget tartalmazó könyvtár:</span><span class="sxs-lookup"><span data-stu-id="7a70d-174">Create a file named **ifcfg-eth0** in hello `/etc/sysconfig/network-scripts/` directory that contains hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
5. <span data-ttu-id="7a70d-175">Módosítsa a udev szabályok tooavoid hello Ethernet adaptert statikus szabályainak létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7a70d-175">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="7a70d-176">Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v virtuális gép klónozása</span><span class="sxs-lookup"><span data-stu-id="7a70d-176">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
6. <span data-ttu-id="7a70d-177">Győződjön meg arról, hello hálózati szolgáltatás indul rendszerindítás hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7a70d-177">Ensure hello network service will start at boot time by running hello following command:</span></span>
   
        # sudo chkconfig network on
7. <span data-ttu-id="7a70d-178">Hello python-pyasn1 csomag telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7a70d-178">Install hello python-pyasn1 package by running hello following command:</span></span>
   
        # sudo yum install python-pyasn1
8. <span data-ttu-id="7a70d-179">Futtassa a következő parancs tooclear hello aktuális yum metaadatok hello, és telepítse a frissítéseket:</span><span class="sxs-lookup"><span data-stu-id="7a70d-179">Run hello following command tooclear hello current yum metadata and install any updates:</span></span>
   
        # sudo yum clean all
        # sudo yum -y update
9. <span data-ttu-id="7a70d-180">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="7a70d-180">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="7a70d-181">a nyitott "/ etc/alapértelmezett/lárvajárat" toodo az a szöveg-szerkesztő és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter, például:</span><span class="sxs-lookup"><span data-stu-id="7a70d-181">toodo this open "/etc/default/grub" in a text editor and edit hello `GRUB_CMDLINE_LINUX` parameter, for example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="7a70d-182">Ez biztosítja az összes konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="7a70d-182">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="7a70d-183">Azt is kikapcsolja hello új OEL 7 elnevezési konvenciói a hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="7a70d-183">It also turns off hello new OEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="7a70d-184">Továbbá a fenti toohello, ajánlott túl*eltávolítása* hello a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="7a70d-184">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
   
       rhgb quiet crashkernel=auto
   
   <span data-ttu-id="7a70d-185">Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="7a70d-185">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>
   
   <span data-ttu-id="7a70d-186">Hello `crashkernel` beállítás lehet, hogy szükség esetén konfigurálva balra, de vegye figyelembe, hogy ez a paraméter csökkenti a hello VM legalább 128 MB, rendelkezésre álló memória mennyisége hello esetleg problémás hello kisebb Virtuálisgép-méretek a.</span><span class="sxs-lookup"><span data-stu-id="7a70d-186">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>
10. <span data-ttu-id="7a70d-187">Miután a szerkesztési "/ etc/alapértelmezett/lárvajárat" / fent, futtassa a hello toorebuild hello lárvajárat konfigurálása a következő:</span><span class="sxs-lookup"><span data-stu-id="7a70d-187">Once you are done editing "/etc/default/grub" per above, run hello following command toorebuild hello grub configuration:</span></span>
    
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg
11. <span data-ttu-id="7a70d-188">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7a70d-188">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="7a70d-189">Ez általában akkor hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="7a70d-189">This is usually hello default.</span></span>
12. <span data-ttu-id="7a70d-190">Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="7a70d-190">Install hello Azure Linux Agent by running hello following command:</span></span>
    
        # sudo yum install WALinuxAgent
        # sudo systemctl enable waagent
13. <span data-ttu-id="7a70d-191">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="7a70d-191">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="7a70d-192">hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="7a70d-192">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="7a70d-193">Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="7a70d-193">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="7a70d-194">Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben hello), módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:</span><span class="sxs-lookup"><span data-stu-id="7a70d-194">After installing hello Azure Linux Agent (see hello previous step), modify hello following parameters in /etc/waagent.conf appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.
14. <span data-ttu-id="7a70d-195">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="7a70d-195">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout
15. <span data-ttu-id="7a70d-196">Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="7a70d-196">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="7a70d-197">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="7a70d-197">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7a70d-198">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7a70d-198">Next steps</span></span>
<span data-ttu-id="7a70d-199">Ön éppen most már készen áll a toouse az Oracle Linux .vhd toocreate új virtuális gépek Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="7a70d-199">You're now ready toouse your Oracle Linux .vhd toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="7a70d-200">Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7a70d-200">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

