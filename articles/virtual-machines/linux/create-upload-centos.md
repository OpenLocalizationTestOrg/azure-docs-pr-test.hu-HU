---
title: "aaaCreate és feltöltése az Azure-ban CentOS alapú Linux virtuális"
description: "Toocreate megtudhatja, és töltse fel az Azure virtuális merevlemez (VHD), amely tartalmazza a CentOS-alapú Linux operációs rendszert."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 0e518e92-e981-43f4-b12c-9cba1064c4bb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 78f36be1d36e3d44cb836c912d143f2c9a72aab5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-centos-based-virtual-machine-for-azure"></a><span data-ttu-id="a02ff-103">CentOS-alapú virtuális gép előkészítése Azure-beli használatra</span><span class="sxs-lookup"><span data-stu-id="a02ff-103">Prepare a CentOS-based virtual machine for Azure</span></span>
* [<span data-ttu-id="a02ff-104">Az Azure-bA a CentOS 6.x virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="a02ff-104">Prepare a CentOS 6.x virtual machine for Azure</span></span>](#centos-6x)
* [<span data-ttu-id="a02ff-105">Az Azure-bA a CentOS 7.0 + virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="a02ff-105">Prepare a CentOS 7.0+ virtual machine for Azure</span></span>](#centos-70)

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="prerequisites"></a><span data-ttu-id="a02ff-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a02ff-106">Prerequisites</span></span>
<span data-ttu-id="a02ff-107">Ez a cikk feltételezi, hogy a CentOS már telepítve van (vagy hasonló származtatott) Linux operációs rendszer tooa virtuális merevlemez.</span><span class="sxs-lookup"><span data-stu-id="a02ff-107">This article assumes that you have already installed a CentOS (or similar derivative) Linux operating system tooa virtual hard disk.</span></span> <span data-ttu-id="a02ff-108">Több különféle eszköz toocreate .vhd fájlok, például egy hálózatvirtualizálási megoldás például a Hyper-V létezik.</span><span class="sxs-lookup"><span data-stu-id="a02ff-108">Multiple tools exist toocreate .vhd files, for example a virtualization solution such as Hyper-V.</span></span> <span data-ttu-id="a02ff-109">Útmutatásért lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="a02ff-109">For instructions, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="a02ff-110">**Telepítési jegyzetek centOS**</span><span class="sxs-lookup"><span data-stu-id="a02ff-110">**CentOS installation notes**</span></span>

* <span data-ttu-id="a02ff-111">Is lásd: [általános Linux telepítési jegyzetek](create-upload-generic.md#general-linux-installation-notes) kapcsolatos további információkat az Azure Linux előkészítése.</span><span class="sxs-lookup"><span data-stu-id="a02ff-111">Please see also [General Linux Installation Notes](create-upload-generic.md#general-linux-installation-notes) for more tips on preparing Linux for Azure.</span></span>
* <span data-ttu-id="a02ff-112">hello VHDX formátum nem támogatott az Azure csak **rögzített VHD**.</span><span class="sxs-lookup"><span data-stu-id="a02ff-112">hello VHDX format is not supported in Azure, only **fixed VHD**.</span></span>  <span data-ttu-id="a02ff-113">Alakítsa át a Hyper-V kezelőjével hello tooVHD formátummal, vagy convert-vhd parancsmag hello.</span><span class="sxs-lookup"><span data-stu-id="a02ff-113">You can convert hello disk tooVHD format using Hyper-V Manager or hello convert-vhd cmdlet.</span></span> <span data-ttu-id="a02ff-114">VirtualBox rendszer használata esetén ez azt jelenti, hogy kiválasztásával **mérete rögzített** , ellenezte toohello alapértelmezett dinamikusan lefoglalt hello lemez létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a02ff-114">If you are using VirtualBox this means selecting **Fixed size** as opposed toohello default dynamically allocated when creating hello disk.</span></span>
* <span data-ttu-id="a02ff-115">Ha telepíti a hello Linux rendszer *ajánlott* LVM (gyakran hello alapértelmezett sok telepítés), hanem szabványos partíciók használja.</span><span class="sxs-lookup"><span data-stu-id="a02ff-115">When installing hello Linux system it is *recommended* that you use standard partitions rather than LVM (often hello default for many installations).</span></span> <span data-ttu-id="a02ff-116">LVM neve ütközik a klónozott virtuális gépek, így elkerülhető, különösen akkor, ha egy operációsrendszer-lemez valaha is szüksége van a csatolt toobe tooanother hibaelhárítási azonos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a02ff-116">This will avoid LVM name conflicts with cloned VMs, particularly if an OS disk ever needs toobe attached tooanother identical VM for troubleshooting.</span></span> <span data-ttu-id="a02ff-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) adatlemezek használható.</span><span class="sxs-lookup"><span data-stu-id="a02ff-117">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks.</span></span>
* <span data-ttu-id="a02ff-118">Kernel támogatása UDF fájlrendszerek csatlakoztatására szükség.</span><span class="sxs-lookup"><span data-stu-id="a02ff-118">Kernel support for mounting UDF file systems is required.</span></span> <span data-ttu-id="a02ff-119">Az Azure hello első rendszerindításkor létesítési konfiguráció lett átadva a toohello Linux virtuális gép UDF formátumú adathordozó csatolt toohello Vendég keresztül.</span><span class="sxs-lookup"><span data-stu-id="a02ff-119">At first boot on Azure hello provisioning configuration is passed toohello Linux VM via UDF-formatted media that is attached toohello guest.</span></span> <span data-ttu-id="a02ff-120">hello Azure Linux ügynök legyen képes toomount hello UDF fájl rendszer tooread konfigurációját és hello virtuális gép kiépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a02ff-120">hello Azure Linux agent must be able toomount hello UDF file system tooread its configuration and provision hello VM.</span></span>
* <span data-ttu-id="a02ff-121">Linux kernel verziójánál régebbi 2.6.37 nem támogatott a Hyper-V nagyobb Virtuálisgép-méretek a.</span><span class="sxs-lookup"><span data-stu-id="a02ff-121">Linux kernel versions below 2.6.37 do not support NUMA on Hyper-V with larger VM sizes.</span></span> <span data-ttu-id="a02ff-122">A probléma főként hatások régebbi azokat a terjesztéseket hello használata előtt Red Hat 2.6.32 kernel és javítását a RHEL 6.6 (kernel-2.6.32-504).</span><span class="sxs-lookup"><span data-stu-id="a02ff-122">This issue primarily impacts older distributions using hello upstream Red Hat 2.6.32 kernel, and was fixed in RHEL 6.6 (kernel-2.6.32-504).</span></span> <span data-ttu-id="a02ff-123">Rendszerekre egyéni kernelek régebbi, mint 2.6.37 vagy régebbi RHEL alapú kernelek 2.6.32-504 kell beállítani, mint a rendszerindítási paraméter hello `numa=off` a hello kernel grub.conf a parancssori.</span><span class="sxs-lookup"><span data-stu-id="a02ff-123">Systems running custom kernels older than 2.6.37, or RHEL-based kernels older than 2.6.32-504 must set hello boot parameter `numa=off` on hello kernel command-line in grub.conf.</span></span> <span data-ttu-id="a02ff-124">További információ: a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="a02ff-124">For more information see Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>
* <span data-ttu-id="a02ff-125">Ne konfiguráljon egy swap partíció hello operációsrendszer-lemezzel.</span><span class="sxs-lookup"><span data-stu-id="a02ff-125">Do not configure a swap partition on hello OS disk.</span></span> <span data-ttu-id="a02ff-126">Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.</span><span class="sxs-lookup"><span data-stu-id="a02ff-126">hello Linux agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="a02ff-127">További információk a hello lépéseket találhatók.</span><span class="sxs-lookup"><span data-stu-id="a02ff-127">More information about this can be found in hello steps below.</span></span>
* <span data-ttu-id="a02ff-128">Az összes hello virtuális merevlemezeket kell rendelkeznie, amely többszörösei 1 MB méretű.</span><span class="sxs-lookup"><span data-stu-id="a02ff-128">All of hello VHDs must have sizes that are multiples of 1 MB.</span></span>

## <a name="centos-6x"></a><span data-ttu-id="a02ff-129">CentOS 6.x</span><span class="sxs-lookup"><span data-stu-id="a02ff-129">CentOS 6.x</span></span>

1. <span data-ttu-id="a02ff-130">A Hyper-V kezelőjében válassza ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="a02ff-130">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="a02ff-131">Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="a02ff-131">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="a02ff-132">A CentOS 6 NetworkManager zavarhatja hello Azure Linux ügynök.</span><span class="sxs-lookup"><span data-stu-id="a02ff-132">In CentOS 6, NetworkManager can interfere with hello Azure Linux agent.</span></span> <span data-ttu-id="a02ff-133">Ez a csomag eltávolítása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a02ff-133">Uninstall this package by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

4. <span data-ttu-id="a02ff-134">Hozzon létre vagy szerkeszthet hello fájlt `/etc/sysconfig/network` , és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="a02ff-134">Create or edit hello file `/etc/sysconfig/network` and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="a02ff-135">Hozzon létre vagy szerkeszthet hello fájlt `/etc/sysconfig/network-scripts/ifcfg-eth0` , és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="a02ff-135">Create or edit hello file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="a02ff-136">Módosítsa a udev szabályok tooavoid hello Ethernet adaptert statikus szabályainak létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a02ff-136">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="a02ff-137">Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v virtuális gép klónozása</span><span class="sxs-lookup"><span data-stu-id="a02ff-137">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="a02ff-138">Győződjön meg arról, hello hálózati szolgáltatás indul rendszerindítás hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="a02ff-138">Ensure hello network service will start at boot time by running hello following command:</span></span>
   
        # sudo chkconfig network on

8. <span data-ttu-id="a02ff-139">Ha szeretné toouse hello OpenLogic tükör hello Azure adatközpontjaiban belül futó, akkor cserélje le hello `/etc/yum.repos.d/CentOS-Base.repo` hello tárházak találhatók a következő fájl.</span><span class="sxs-lookup"><span data-stu-id="a02ff-139">If you would like toouse hello OpenLogic mirrors that are hosted within hello Azure datacenters, then replace hello `/etc/yum.repos.d/CentOS-Base.repo` file with hello following repositories.</span></span>  <span data-ttu-id="a02ff-140">A művelet továbbá felvesz hello **[openlogic]** tárház, például a hello Azure Linux ügynök további csomagokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="a02ff-140">This will also add hello **[openlogic]** repository that includes additional packages such as hello Azure Linux agent:</span></span>

        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6
        
        #contrib - packages by Centos Users
        [contrib]
        name=CentOS-$releasever - Contrib
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=contrib&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/contrib/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

    >[!Note]
    <span data-ttu-id="a02ff-141">hello Ez az útmutató többi fogja feltételezni, hogy azok be legalább hello `[openlogic]` tárház, amely használt tooinstall hello Azure Linux ügynök az alábbi lesz.</span><span class="sxs-lookup"><span data-stu-id="a02ff-141">hello rest of this guide will assume you are using at least hello `[openlogic]` repo, which will be used tooinstall hello Azure Linux agent below.</span></span>


9. <span data-ttu-id="a02ff-142">Adja hozzá a következő sor too/etc/yum.conf hello:</span><span class="sxs-lookup"><span data-stu-id="a02ff-142">Add hello following line too/etc/yum.conf:</span></span>
    
        http_caching=packages

10. <span data-ttu-id="a02ff-143">Futtassa a következő parancs tooclear hello aktuális yum metaadatok és a frissítési hello rendszer hello legújabb csomagokkal hello:</span><span class="sxs-lookup"><span data-stu-id="a02ff-143">Run hello following command tooclear hello current yum metadata and update hello system with hello latest packages:</span></span>
    
        # yum clean all

    <span data-ttu-id="a02ff-144">Kivéve, ha a régebbi verziójú CentOS egy kép létrehozásakor tooupdate összes hello csomagok toohello legújabb ajánlott:</span><span class="sxs-lookup"><span data-stu-id="a02ff-144">Unless you are creating an image for an older version of CentOS, it is recommended tooupdate all hello packages toohello latest:</span></span>

        # sudo yum -y update

    <span data-ttu-id="a02ff-145">A parancs futtatása után a rendszer újraindítása lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="a02ff-145">A reboot may be required after running this command.</span></span>

11. <span data-ttu-id="a02ff-146">(Választható) Telepítse a Linux integrációs szolgáltatások (LIS) hello hello illesztőprogramjait.</span><span class="sxs-lookup"><span data-stu-id="a02ff-146">(Optional) Install hello drivers for hello Linux Integration Services (LIS).</span></span>
   
    >[!IMPORTANT]
    <span data-ttu-id="a02ff-147">hello lépés **szükséges** a CentOS 6.3 és korábbi, és kötelező megadni a későbbi kibocsátásokban megtörténik.</span><span class="sxs-lookup"><span data-stu-id="a02ff-147">hello step is **required** for CentOS 6.3 and earlier, and optional for later releases.</span></span>

        # sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
        # sudo yum install microsoft-hyper-v

    <span data-ttu-id="a02ff-148">Azt is megteheti, kövesse a hello manuális telepítési utasításokat a hello [LIS letöltési oldalát](https://go.microsoft.com/fwlink/?linkid=403033) tooinstall hello RPM, a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="a02ff-148">Alternatively, you can follow hello manual installation instructions on hello [LIS download page](https://go.microsoft.com/fwlink/?linkid=403033) tooinstall hello RPM onto your VM.</span></span>
 
12. <span data-ttu-id="a02ff-149">Hello Azure Linux ügynök és a függőségek telepítése:</span><span class="sxs-lookup"><span data-stu-id="a02ff-149">Install hello Azure Linux Agent and dependencies:</span></span>
    
        # sudo yum install python-pyasn1 WALinuxAgent
    
    <span data-ttu-id="a02ff-150">hello WALinuxAgent csomag eltávolítja hello NetworkManager és NetworkManager-gnome csomagok, ha nem már eltávolította azokat a 3. lépésben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="a02ff-150">hello WALinuxAgent package will remove hello NetworkManager and NetworkManager-gnome packages if they were not already removed as described in step 3.</span></span>


13. <span data-ttu-id="a02ff-151">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="a02ff-151">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="a02ff-152">toodo a, nyissa meg `/boot/grub/menu.lst` egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="a02ff-152">toodo this, open `/boot/grub/menu.lst` in a text editor and ensure that hello default kernel includes hello following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="a02ff-153">Ez biztosítja az összes konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="a02ff-153">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="a02ff-154">Továbbá a fenti toohello, ajánlott túl*eltávolítása* hello a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="a02ff-154">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="a02ff-155">Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="a02ff-155">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>  <span data-ttu-id="a02ff-156">Hello `crashkernel` beállítás lehet, hogy szükség esetén konfigurálva balra, de vegye figyelembe, hogy ez a paraméter csökkenti a hello VM legalább 128 MB, rendelkezésre álló memória mennyisége hello esetleg problémás hello kisebb Virtuálisgép-méretek a.</span><span class="sxs-lookup"><span data-stu-id="a02ff-156">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>

    >[!Important]
    <span data-ttu-id="a02ff-157">CentOS 6.5-ös vagy korábbi is be kell állítani hello kernel paraméter `numa=off`.</span><span class="sxs-lookup"><span data-stu-id="a02ff-157">CentOS 6.5 and earlier must also set hello kernel parameter `numa=off`.</span></span> <span data-ttu-id="a02ff-158">Tekintse meg a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="a02ff-158">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

14. <span data-ttu-id="a02ff-159">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="a02ff-159">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span>  <span data-ttu-id="a02ff-160">Ez általában akkor hello alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="a02ff-160">This is usually hello default.</span></span>

15. <span data-ttu-id="a02ff-161">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="a02ff-161">Do not create swap space on hello OS disk.</span></span>
    
    <span data-ttu-id="a02ff-162">hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="a02ff-162">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="a02ff-163">Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="a02ff-163">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="a02ff-164">Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa a paramétereket a következő hello `/etc/waagent.conf` megfelelően:</span><span class="sxs-lookup"><span data-stu-id="a02ff-164">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>
    
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. <span data-ttu-id="a02ff-165">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="a02ff-165">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
    
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

17. <span data-ttu-id="a02ff-166">Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="a02ff-166">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="a02ff-167">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="a02ff-167">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>


- - -
## <a name="centos-70"></a><span data-ttu-id="a02ff-168">CentOS 7.0 +</span><span class="sxs-lookup"><span data-stu-id="a02ff-168">CentOS 7.0+</span></span>
<span data-ttu-id="a02ff-169">**Változások a CentOS 7 (és hasonló származékai)**</span><span class="sxs-lookup"><span data-stu-id="a02ff-169">**Changes in CentOS 7 (and similar derivatives)**</span></span>

<span data-ttu-id="a02ff-170">A CentOS 7 virtuális gép előkészítése az Azure-nagyon hasonló tooCentOS 6, azonban nincsenek érdemes megjegyezni számos fontos különbség:</span><span class="sxs-lookup"><span data-stu-id="a02ff-170">Preparing a CentOS 7 virtual machine for Azure is very similar tooCentOS 6, however there are several important differences worth noting:</span></span>

* <span data-ttu-id="a02ff-171">hello NetworkManager csomag már nem ütközik az hello Azure Linux ügynök.</span><span class="sxs-lookup"><span data-stu-id="a02ff-171">hello NetworkManager package no longer conflicts with hello Azure Linux agent.</span></span> <span data-ttu-id="a02ff-172">Alapértelmezés szerint ez a csomag telepítve van, és azt javasoljuk, hogy nem törli.</span><span class="sxs-lookup"><span data-stu-id="a02ff-172">This package is installed by default and we recommend that it is not removed.</span></span>
* <span data-ttu-id="a02ff-173">GRUB2 most használatos az alapértelmezett rendszertöltő hello így hello eljárás kernel paraméterek szerkesztésre megváltozott (lásd alább).</span><span class="sxs-lookup"><span data-stu-id="a02ff-173">GRUB2 is now used as hello default bootloader, so hello procedure for editing kernel parameters has changed (see below).</span></span>
* <span data-ttu-id="a02ff-174">XFS már hello alapértelmezett fájlrendszert.</span><span class="sxs-lookup"><span data-stu-id="a02ff-174">XFS is now hello default file system.</span></span> <span data-ttu-id="a02ff-175">hello ext4 fájlrendszer továbbra is használható, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="a02ff-175">hello ext4 file system can still be used if desired.</span></span>

<span data-ttu-id="a02ff-176">**Konfigurációs lépések**</span><span class="sxs-lookup"><span data-stu-id="a02ff-176">**Configuration Steps**</span></span>

1. <span data-ttu-id="a02ff-177">A Hyper-V kezelőjében válassza ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="a02ff-177">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="a02ff-178">Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="a02ff-178">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="a02ff-179">Hozzon létre vagy szerkeszthet hello fájlt `/etc/sysconfig/network` , és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="a02ff-179">Create or edit hello file `/etc/sysconfig/network` and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. <span data-ttu-id="a02ff-180">Hozzon létre vagy szerkeszthet hello fájlt `/etc/sysconfig/network-scripts/ifcfg-eth0` , és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="a02ff-180">Create or edit hello file `/etc/sysconfig/network-scripts/ifcfg-eth0` and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. <span data-ttu-id="a02ff-181">Módosítsa a udev szabályok tooavoid hello Ethernet adaptert statikus szabályainak létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="a02ff-181">Modify udev rules tooavoid generating static rules for hello Ethernet interface(s).</span></span> <span data-ttu-id="a02ff-182">Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v virtuális gép klónozása</span><span class="sxs-lookup"><span data-stu-id="a02ff-182">These rules can cause problems when cloning a virtual machine in Microsoft Azure or Hyper-V:</span></span>
   
        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

6. <span data-ttu-id="a02ff-183">Ha szeretné toouse hello OpenLogic tükör hello Azure adatközpontjaiban belül futó, akkor cserélje le hello `/etc/yum.repos.d/CentOS-Base.repo` hello tárházak találhatók a következő fájl.</span><span class="sxs-lookup"><span data-stu-id="a02ff-183">If you would like toouse hello OpenLogic mirrors that are hosted within hello Azure datacenters, then replace hello `/etc/yum.repos.d/CentOS-Base.repo` file with hello following repositories.</span></span>  <span data-ttu-id="a02ff-184">A művelet továbbá felvesz hello **[openlogic]** tárház, amely tartalmazza az Azure Linux ügynök hello csomagok:</span><span class="sxs-lookup"><span data-stu-id="a02ff-184">This will also add hello **[openlogic]** repository that includes packages for hello Azure Linux agent:</span></span>
   
        [openlogic]
        name=CentOS-$releasever - openlogic packages for $basearch
        baseurl=http://olcentgbl.trafficmanager.net/openlogic/$releasever/openlogic/$basearch/
        enabled=1
        gpgcheck=0
        
        [base]
        name=CentOS-$releasever - Base
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/os/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #released updates
        [updates]
        name=CentOS-$releasever - Updates
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/updates/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that may be useful
        [extras]
        name=CentOS-$releasever - Extras
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=extras&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/extras/$basearch/
        gpgcheck=1
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
        
        #additional packages that extend functionality of existing packages
        [centosplus]
        name=CentOS-$releasever - Plus
        #mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=centosplus&infra=$infra
        baseurl=http://olcentgbl.trafficmanager.net/centos/$releasever/centosplus/$basearch/
        gpgcheck=1
        enabled=0
        gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

    >[!Note]
    <span data-ttu-id="a02ff-185">hello Ez az útmutató többi fogja feltételezni, hogy azok be legalább hello `[openlogic]` tárház, amely használt tooinstall hello Azure Linux ügynök az alábbi lesz.</span><span class="sxs-lookup"><span data-stu-id="a02ff-185">hello rest of this guide will assume you are using at least hello `[openlogic]` repo, which will be used tooinstall hello Azure Linux agent below.</span></span>

7. <span data-ttu-id="a02ff-186">Futtassa a következő parancs tooclear hello aktuális yum metaadatok hello, és telepítse a frissítéseket:</span><span class="sxs-lookup"><span data-stu-id="a02ff-186">Run hello following command tooclear hello current yum metadata and install any updates:</span></span>
   
        # sudo yum clean all

    <span data-ttu-id="a02ff-187">Kivéve, ha a régebbi verziójú CentOS egy kép létrehozásakor tooupdate összes hello csomagok toohello legújabb ajánlott:</span><span class="sxs-lookup"><span data-stu-id="a02ff-187">Unless you are creating an image for an older version of CentOS, it is recommended tooupdate all hello packages toohello latest:</span></span>

        # sudo yum -y update

    <span data-ttu-id="a02ff-188">Lehet, hogy ez a parancs futtatása után a szükséges újraindítás.</span><span class="sxs-lookup"><span data-stu-id="a02ff-188">A reboot maybe required after running this command.</span></span>

8. <span data-ttu-id="a02ff-189">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="a02ff-189">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="a02ff-190">toodo a, nyissa meg `/etc/default/grub` az a szöveg-szerkesztő és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter, például:</span><span class="sxs-lookup"><span data-stu-id="a02ff-190">toodo this, open `/etc/default/grub` in a text editor and edit hello `GRUB_CMDLINE_LINUX` parameter, for example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="a02ff-191">Ez biztosítja az összes konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="a02ff-191">This will also ensure all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="a02ff-192">Azt is kikapcsolja hello új CentOS 7 elnevezési konvenciói a hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="a02ff-192">It also turns off hello new CentOS 7 naming conventions for NICs.</span></span> <span data-ttu-id="a02ff-193">Továbbá a fenti toohello, ajánlott túl*eltávolítása* hello a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="a02ff-193">In addition toohello above, it is recommended too*remove* hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="a02ff-194">Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="a02ff-194">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="a02ff-195">Hello `crashkernel` beállítás lehet, hogy szükség esetén konfigurálva balra, de vegye figyelembe, hogy ez a paraméter csökkenti a hello VM legalább 128 MB, rendelkezésre álló memória mennyisége hello esetleg problémás hello kisebb Virtuálisgép-méretek a.</span><span class="sxs-lookup"><span data-stu-id="a02ff-195">hello `crashkernel` option may be left configured if desired, but note that this parameter will reduce hello amount of available memory in hello VM by 128MB or more, which may be problematic on hello smaller VM sizes.</span></span>

9. <span data-ttu-id="a02ff-196">Miután a szerkesztési `/etc/default/grub` / fent, futtassa a hello toorebuild hello lárvajárat konfigurálása a következő:</span><span class="sxs-lookup"><span data-stu-id="a02ff-196">Once you are done editing `/etc/default/grub` per above, run hello following command toorebuild hello grub configuration:</span></span>
   
        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

10. <span data-ttu-id="a02ff-197">Ha a hello lemezkép **VMWare, VirtualBox vagy KVM:** ellenőrizze, hogy a Hyper-V hello illesztőprogramok hello initramfs szerepelnek:</span><span class="sxs-lookup"><span data-stu-id="a02ff-197">If building hello image from **VMWare, VirtualBox or KVM:** Ensure hello Hyper-V drivers are included in hello initramfs:</span></span>
   
   <span data-ttu-id="a02ff-198">Szerkesztés `/etc/dracut.conf`, adja hozzá a tartalomhoz:</span><span class="sxs-lookup"><span data-stu-id="a02ff-198">Edit `/etc/dracut.conf`, add content:</span></span>
   
        add_drivers+=”hv_vmbus hv_netvsc hv_storvsc”
   
   <span data-ttu-id="a02ff-199">Építse újra hello initramfs:</span><span class="sxs-lookup"><span data-stu-id="a02ff-199">Rebuild hello initramfs:</span></span>
   
        # sudo dracut –f -v

11. <span data-ttu-id="a02ff-200">Hello Azure Linux ügynök és a függőségek telepítése:</span><span class="sxs-lookup"><span data-stu-id="a02ff-200">Install hello Azure Linux Agent and dependencies:</span></span>

        # sudo yum install python-pyasn1 WALinuxAgent
        # sudo systemctl enable waagent

12. <span data-ttu-id="a02ff-201">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="a02ff-201">Do not create swap space on hello OS disk.</span></span>
   
   <span data-ttu-id="a02ff-202">hello Azure Linux ügynök automatikusan konfigurálhatók a lapozóterület használata hello helyi erőforrás lemezhez csatolt toohello VM Azure kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="a02ff-202">hello Azure Linux Agent can automatically configure swap space using hello local resource disk that is attached toohello VM after provisioning on Azure.</span></span> <span data-ttu-id="a02ff-203">Vegye figyelembe, hogy hello helyi erőforrás lemez egy *ideiglenes* lemezre, és előfordulhat, hogy üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="a02ff-203">Note that hello local resource disk is a *temporary* disk, and might be emptied when hello VM is deprovisioned.</span></span> <span data-ttu-id="a02ff-204">Miután telepítette a hello Azure Linux ügynök (lásd az előző lépésben), módosítsa a paramétereket a következő hello `/etc/waagent.conf` megfelelően:</span><span class="sxs-lookup"><span data-stu-id="a02ff-204">After installing hello Azure Linux Agent (see previous step), modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>
   
        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. <span data-ttu-id="a02ff-205">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="a02ff-205">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

14. <span data-ttu-id="a02ff-206">Kattintson a **művelet le -> leállítási** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="a02ff-206">Click **Action -> Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="a02ff-207">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="a02ff-207">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a02ff-208">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a02ff-208">Next steps</span></span>
<span data-ttu-id="a02ff-209">Ön éppen most már készen áll a toouse a CentOS Linux virtuális merevlemez toocreate új virtuális gépeket az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a02ff-209">You're now ready toouse your CentOS Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="a02ff-210">Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a02ff-210">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

