---
title: "aaaCreate, és töltse fel a Red Hat Enterprise Linux virtuális merevlemez használata az Azure-ban |} Microsoft Docs"
description: "Toocreate ismerje meg, és töltse fel az Azure virtuális merevlemez (VHD) a Red Hat Linux operációs rendszert tartalmazó."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-resource-manager,azure-service-management
ms.assetid: 6c6b8f72-32d3-47fa-be94-6cb54537c69f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/28/2017
ms.author: szark
ms.openlocfilehash: bdd1bbfbee55b5cc61d69a09b06b6bd2c3de362b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-a-red-hat-based-virtual-machine-for-azure"></a><span data-ttu-id="70e88-103">Red Hat-alapú virtuális gép előkészítése Azure-beli használatra</span><span class="sxs-lookup"><span data-stu-id="70e88-103">Prepare a Red Hat-based virtual machine for Azure</span></span>
<span data-ttu-id="70e88-104">Ebből a cikkből megtudhatja, hogyan használható az Azure-ban Red Hat Enterprise Linux (RHEL) virtuális gép tooprepare.</span><span class="sxs-lookup"><span data-stu-id="70e88-104">In this article, you will learn how tooprepare a Red Hat Enterprise Linux (RHEL) virtual machine for use in Azure.</span></span> <span data-ttu-id="70e88-105">hello verziói RHEL ebben a cikkben ismertetett 6.7 + és 7.1 +.</span><span class="sxs-lookup"><span data-stu-id="70e88-105">hello versions of RHEL that are covered in this article are 6.7+ and 7.1+.</span></span> <span data-ttu-id="70e88-106">hello hipervizorok, ebben a cikkben ismertetett előkészítéséhez a Hyper-V, a kernel-alapú virtuális gép (KVM), és a VMware.</span><span class="sxs-lookup"><span data-stu-id="70e88-106">hello hypervisors for preparation that are covered in this article are Hyper-V, kernel-based virtual machine (KVM), and VMware.</span></span> <span data-ttu-id="70e88-107">Red Hat Felhőelérést programban való részvételre vonatkozó jogosultság követelményeivel kapcsolatos további információkért lásd: [Red Hat Felhőelérést webhely](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) és [az Azure-on futó RHEL](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).</span><span class="sxs-lookup"><span data-stu-id="70e88-107">For more information about eligibility requirements for participating in Red Hat's Cloud Access program, see [Red Hat's Cloud Access website](http://www.redhat.com/en/technologies/cloud-computing/cloud-access) and [Running RHEL on Azure](https://access.redhat.com/ecosystem/ccsp/microsoft-azure).</span></span>

## <a name="prepare-a-red-hat-based-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="70e88-108">A Hyper-V kezelőjéből a Red Hat-alapú virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="70e88-108">Prepare a Red Hat-based virtual machine from Hyper-V Manager</span></span>

### <a name="prerequisites"></a><span data-ttu-id="70e88-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="70e88-109">Prerequisites</span></span>
<span data-ttu-id="70e88-110">Ez a szakasz azt feltételezi, hogy ha már beszerezte az ISO-fájl hello Red Hat webhely és a telepített hello RHEL kép tooa virtuális merevlemez (VHD).</span><span class="sxs-lookup"><span data-stu-id="70e88-110">This section assumes that you have already obtained an ISO file from hello Red Hat website and installed hello RHEL image tooa virtual hard disk (VHD).</span></span> <span data-ttu-id="70e88-111">További részletes információt toouse Hyper-V kezelője tooinstall az operációs rendszer lemezképét, lásd: [hello Hyper-V szerepkör telepítése és konfigurálása a virtuális gépek](http://technet.microsoft.com/library/hh846766.aspx).</span><span class="sxs-lookup"><span data-stu-id="70e88-111">For more details about how toouse Hyper-V Manager tooinstall an operating system image, see [Install hello Hyper-V Role and Configure a Virtual Machine](http://technet.microsoft.com/library/hh846766.aspx).</span></span>

<span data-ttu-id="70e88-112">**RHEL telepítési megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="70e88-112">**RHEL installation notes**</span></span>

* <span data-ttu-id="70e88-113">Azure nem támogatja a hello VHDX formátumú.</span><span class="sxs-lookup"><span data-stu-id="70e88-113">Azure does not support hello VHDX format.</span></span> <span data-ttu-id="70e88-114">Az Azure által támogatott csak rögzített VHD-t.</span><span class="sxs-lookup"><span data-stu-id="70e88-114">Azure supports only fixed VHD.</span></span> <span data-ttu-id="70e88-115">Használhatja a Hyper-V kezelője tooVHD tooconvert hello lemezformátumot, vagy hello convert-vhd parancsmagot használhatja.</span><span class="sxs-lookup"><span data-stu-id="70e88-115">You can use Hyper-V Manager tooconvert hello disk tooVHD format, or you can use hello convert-vhd cmdlet.</span></span> <span data-ttu-id="70e88-116">Ha VirtualBox használja, jelölje be **mérete rögzített** mint toohello alapértelmezett számára dinamikusan kiosztható beállítás hello lemezek létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="70e88-116">If you use VirtualBox, select **Fixed size** as opposed toohello default dynamically allocated option when you create hello disk.</span></span>
* <span data-ttu-id="70e88-117">Azure csak az 1. generációs virtuális gépek támogatja.</span><span class="sxs-lookup"><span data-stu-id="70e88-117">Azure supports only generation 1 virtual machines.</span></span> <span data-ttu-id="70e88-118">1. generációs virtuális gépek VHDX toohello VHD formátumú és eltávolítása dinamikusan bővülő tooa rögzített méretű lemez válthat.</span><span class="sxs-lookup"><span data-stu-id="70e88-118">You can convert a generation 1 virtual machine from VHDX toohello VHD file format and from dynamically expanding tooa fixed-size disk.</span></span> <span data-ttu-id="70e88-119">Nem módosíthatja a virtuális gép generációját.</span><span class="sxs-lookup"><span data-stu-id="70e88-119">You can't change a virtual machine's generation.</span></span> <span data-ttu-id="70e88-120">További információkért lásd: [érdemes létrehozni 1 vagy 2. generációs virtuális gépek a Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).</span><span class="sxs-lookup"><span data-stu-id="70e88-120">For more information, see [Should I create a generation 1 or 2 virtual machine in Hyper-V?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).</span></span>
* <span data-ttu-id="70e88-121">hello maximális megengedett hello virtuális merevlemez mérete 1,023 GB.</span><span class="sxs-lookup"><span data-stu-id="70e88-121">hello maximum size that's allowed for hello VHD is 1,023 GB.</span></span>
* <span data-ttu-id="70e88-122">Hello Linux operációs rendszer telepítésekor azt javasoljuk, hogy Ön normál partíció helyett használjon logikai kötet Manager (LVM), ez utóbbi érték gyakran hello alapértelmezett sok telepítés.</span><span class="sxs-lookup"><span data-stu-id="70e88-122">When you install hello Linux operating system, we recommend that you use standard partitions rather than Logical Volume Manager (LVM), which is often hello default for many installations.</span></span> <span data-ttu-id="70e88-123">Ezzel a gyakorlattal elkerülheti LVM neve ütközik a klónozott virtuális gépek, különösen akkor, ha az operációs rendszer lemez tooanother azonos virtuális gép tooattach hibaelhárítási átállítására lenne szükség.</span><span class="sxs-lookup"><span data-stu-id="70e88-123">This practice will avoid LVM name conflicts with cloned virtual machines, particularly if you ever need tooattach an operating system disk tooanother identical virtual machine for troubleshooting.</span></span> <span data-ttu-id="70e88-124">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) adatlemezek használható.</span><span class="sxs-lookup"><span data-stu-id="70e88-124">[LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [RAID](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) may be used on data disks.</span></span>
* <span data-ttu-id="70e88-125">Univerzális lemez formátum (UDF) fájlrendszerek csatlakoztatására kernel támogatására szükség.</span><span class="sxs-lookup"><span data-stu-id="70e88-125">Kernel support for mounting Universal Disk Format (UDF) file systems is required.</span></span> <span data-ttu-id="70e88-126">Az Azure, hello UDF formátumú adathordozón, amely első rendszerindításkor csatolt toohello Vendég hello létesítési konfiguráció toohello Linux virtuális gép továbbítja.</span><span class="sxs-lookup"><span data-stu-id="70e88-126">At first boot on Azure, hello UDF-formatted media that is attached toohello guest passes hello provisioning configuration toohello Linux virtual machine.</span></span> <span data-ttu-id="70e88-127">hello Azure Linux ügynök kell tudni toomount hello UDF fájl rendszer tooread konfigurációját és rendszerű hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="70e88-127">hello Azure Linux Agent must be able toomount hello UDF file system tooread its configuration and provision hello virtual machine.</span></span>
* <span data-ttu-id="70e88-128">Hello Linux kernel 2.6.37 verziónál korábbi verziói nem támogatják nem egységes memóriaelérés (NUMA) a Hyper-V a nagyobb virtuális gépek méretét.</span><span class="sxs-lookup"><span data-stu-id="70e88-128">Versions of hello Linux kernel that are earlier than 2.6.37 do not support non-uniform memory access (NUMA) on Hyper-V with larger virtual machine sizes.</span></span> <span data-ttu-id="70e88-129">A probléma főként hatások régebbi azokat a terjesztéseket, amelyek hello előtt Red Hat 2.6.32 kernel és javítását a RHEL 6.6 (kernel-2.6.32-504).</span><span class="sxs-lookup"><span data-stu-id="70e88-129">This issue primarily impacts older distributions that use hello upstream Red Hat 2.6.32 kernel and was fixed in RHEL 6.6 (kernel-2.6.32-504).</span></span> <span data-ttu-id="70e88-130">Egyéni kernelek futtatják, amelyek régebbiek 2.6.37 vagy RHEL-alapú kernelek 2.6.32-504 régebbi értéke hello `numa=off` paraméter rendszerindító hello kernel parancssorában grub.conf.</span><span class="sxs-lookup"><span data-stu-id="70e88-130">Systems that run custom kernels that are older than 2.6.37 or RHEL-based kernels that are older than 2.6.32-504 must set hello `numa=off` boot parameter on hello kernel command line in grub.conf.</span></span> <span data-ttu-id="70e88-131">További információkért tekintse meg a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="70e88-131">For more information, see Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>
* <span data-ttu-id="70e88-132">Ne konfiguráljon egy swap partíció hello operációsrendszer-lemez.</span><span class="sxs-lookup"><span data-stu-id="70e88-132">Do not configure a swap partition on hello operating system disk.</span></span> <span data-ttu-id="70e88-133">Linux-ügynök hello konfigurált toocreate hello ideiglenes erőforrás lemezen a lapozófájl lehet.</span><span class="sxs-lookup"><span data-stu-id="70e88-133">hello Linux Agent can be configured toocreate a swap file on hello temporary resource disk.</span></span>  <span data-ttu-id="70e88-134">További információ található a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="70e88-134">More information about this can be found in hello following steps.</span></span>
* <span data-ttu-id="70e88-135">Minden virtuális merevlemez, amely többszörösei 1 MB méretű kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="70e88-135">All VHDs must have sizes that are multiples of 1 MB.</span></span>

### <a name="prepare-a-rhel-6-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="70e88-136">A Hyper-V kezelőjéből a RHEL 6 virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="70e88-136">Prepare a RHEL 6 virtual machine from Hyper-V Manager</span></span>

1. <span data-ttu-id="70e88-137">A Hyper-V kezelőjében válassza ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="70e88-137">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="70e88-138">Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="70e88-138">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="70e88-139">Az RHEL 6 NetworkManager zavarhatja hello Azure Linux ügynök.</span><span class="sxs-lookup"><span data-stu-id="70e88-139">In RHEL 6, NetworkManager can interfere with hello Azure Linux agent.</span></span> <span data-ttu-id="70e88-140">Ez a csomag eltávolítása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-140">Uninstall this package by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

4. <span data-ttu-id="70e88-141">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-141">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="70e88-142">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-142">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="70e88-143">Helyezze át vagy távolítsa el hello udev szabályok tooavoid hello Ethernet-adapter statikus szabályok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="70e88-143">Move (or remove) hello udev rules tooavoid generating static rules for hello Ethernet interface.</span></span> <span data-ttu-id="70e88-144">Ezek a szabályok problémákat okozhat, ha a Microsoft Azure- vagy Hyper-v egy virtuális gép klónozását</span><span class="sxs-lookup"><span data-stu-id="70e88-144">These rules cause problems when you clone a virtual machine in Microsoft Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules
        
        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="70e88-145">Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-145">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

8. <span data-ttu-id="70e88-146">A Red Hat előfizetés tooenable hello telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-146">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

9. <span data-ttu-id="70e88-147">hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve.</span><span class="sxs-lookup"><span data-stu-id="70e88-147">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="70e88-148">Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-148">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

10. <span data-ttu-id="70e88-149">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="70e88-149">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="70e88-150">toodo módosítást, nyissa meg `/boot/grub/menu.lst` egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-150">toodo this modification, open `/boot/grub/menu.lst` in a text editor, and ensure that hello default kernel includes hello following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="70e88-151">Ez biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="70e88-151">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="70e88-152">Ezenkívül azt javasoljuk, hogy távolítsa el a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-152">In addition, we recommended that you remove hello following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="70e88-153">Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="70e88-153">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span>  <span data-ttu-id="70e88-154">Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="70e88-154">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="70e88-155">Vegye figyelembe, hogy ez a paraméter csökkenti a hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB.</span><span class="sxs-lookup"><span data-stu-id="70e88-155">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more.</span></span> <span data-ttu-id="70e88-156">Ez a konfiguráció kisebb virtuálisgép-méretek a problémás lehet.</span><span class="sxs-lookup"><span data-stu-id="70e88-156">This configuration might be problematic on smaller virtual machine sizes.</span></span>

    >[!Important]
    <span data-ttu-id="70e88-157">RHEL 6.5-ös vagy korábbi is be kell állítani hello `numa=off` kernel paraméter.</span><span class="sxs-lookup"><span data-stu-id="70e88-157">RHEL 6.5 and earlier must also set hello `numa=off` kernel parameter.</span></span> <span data-ttu-id="70e88-158">Tekintse meg a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="70e88-158">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

11. <span data-ttu-id="70e88-159">Győződjön meg arról, hogy hello secure shell (SSH) kiszolgáló telepítve van, és rendszerindítás, amely általában hello alapértelmezett toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="70e88-159">Ensure that hello secure shell (SSH) server is installed and configured toostart at boot time, which is usually hello default.</span></span> <span data-ttu-id="70e88-160">Módosítsa a következő sor /etc/ssh/sshd_config tooinclude hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-160">Modify /etc/ssh/sshd_config tooinclude hello following line:</span></span>

        ClientAliveInterval 180

12. <span data-ttu-id="70e88-161">Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-161">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

    <span data-ttu-id="70e88-162">Telepítés hello WALinuxAgent csomag hello NetworkManager és NetworkManager-gnome csomagok eltávolítja, ha nem már eltávolította azokat a 3. lépésben.</span><span class="sxs-lookup"><span data-stu-id="70e88-162">Installing hello WALinuxAgent package removes hello NetworkManager and NetworkManager-gnome packages if they were not already removed in step 3.</span></span>

13. <span data-ttu-id="70e88-163">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="70e88-163">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="70e88-164">hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="70e88-164">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="70e88-165">Vegye figyelembe, hogy helyi erőforrás egy ideiglenes lemez és, hogy, előfordulhat, hogy ki kell üríteni amikor hello virtuális gép van platformelőfizetés hello.</span><span class="sxs-lookup"><span data-stu-id="70e88-165">Note that hello local resource disk is a temporary disk and that it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="70e88-166">Telepítése után hello Azure Linux ügynök hello előző lépésben, módosítsa megfelelően a következő paraméterek a /etc/waagent.conf hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-166">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in /etc/waagent.conf appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

14. <span data-ttu-id="70e88-167">Hello előfizetés regisztrációját (ha szükséges) hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-167">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # sudo subscription-manager unregister

15. <span data-ttu-id="70e88-168">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="70e88-168">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

16. <span data-ttu-id="70e88-169">Kattintson a **művelet** > **leállítása** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="70e88-169">Click **Action** > **Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="70e88-170">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="70e88-170">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>


### <a name="prepare-a-rhel-7-virtual-machine-from-hyper-v-manager"></a><span data-ttu-id="70e88-171">A Hyper-V kezelőjéből a RHEL 7 virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="70e88-171">Prepare a RHEL 7 virtual machine from Hyper-V Manager</span></span>

1. <span data-ttu-id="70e88-172">A Hyper-V kezelőjében válassza ki a hello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="70e88-172">In Hyper-V Manager, select hello virtual machine.</span></span>

2. <span data-ttu-id="70e88-173">Kattintson a **Connect** tooopen a konzolablakban hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="70e88-173">Click **Connect** tooopen a console window for hello virtual machine.</span></span>

3. <span data-ttu-id="70e88-174">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-174">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

4. <span data-ttu-id="70e88-175">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-175">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

5. <span data-ttu-id="70e88-176">Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-176">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

6. <span data-ttu-id="70e88-177">A Red Hat előfizetés tooenable hello telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-177">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. <span data-ttu-id="70e88-178">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="70e88-178">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="70e88-179">toodo módosítást, nyissa meg `/etc/default/grub` egy szövegszerkesztőben, és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter.</span><span class="sxs-lookup"><span data-stu-id="70e88-179">toodo this modification, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="70e88-180">Példa:</span><span class="sxs-lookup"><span data-stu-id="70e88-180">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="70e88-181">Ez biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="70e88-181">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="70e88-182">Ez a konfiguráció is kikapcsolja hello új RHEL 7 elnevezési konvenciói a hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="70e88-182">This configuration also turns off hello new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="70e88-183">Emellett javasoljuk, hogy távolítsa el a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-183">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="70e88-184">Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="70e88-184">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="70e88-185">Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="70e88-185">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="70e88-186">Vegye figyelembe, hogy ez a paraméter csökkenti hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB, amelyek kisebb virtuálisgép-méretek a problémás lehet.</span><span class="sxs-lookup"><span data-stu-id="70e88-186">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

8. <span data-ttu-id="70e88-187">Miután befejezte az szerkesztési `/etc/default/grub`- ben futtassa hello következő toorebuild hello lárvajárat konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="70e88-187">After you are done editing `/etc/default/grub`, run hello following command toorebuild hello grub configuration:</span></span>

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

9. <span data-ttu-id="70e88-188">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás, amely általában hello alapértelmezett toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="70e88-188">Ensure that hello SSH server is installed and configured toostart at boot time, which is usually hello default.</span></span> <span data-ttu-id="70e88-189">Módosítsa `/etc/ssh/sshd_config` tooinclude hello a következő sort:</span><span class="sxs-lookup"><span data-stu-id="70e88-189">Modify `/etc/ssh/sshd_config` tooinclude hello following line:</span></span>

        ClientAliveInterval 180

10. <span data-ttu-id="70e88-190">hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve.</span><span class="sxs-lookup"><span data-stu-id="70e88-190">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="70e88-191">Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-191">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

11. <span data-ttu-id="70e88-192">Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-192">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

12. <span data-ttu-id="70e88-193">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="70e88-193">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="70e88-194">hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="70e88-194">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="70e88-195">Vegye figyelembe, hogy hello helyi erőforrás egy ideiglenes lemez, és előfordulhat, hogy lehet üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="70e88-195">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="70e88-196">Telepítése után hello Azure Linux ügynök az előző lépésben hello, módosítsa a következő paramétereket a hello `/etc/waagent.conf` megfelelően:</span><span class="sxs-lookup"><span data-stu-id="70e88-196">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. <span data-ttu-id="70e88-197">Toounregister hello előfizetés tetszés szerint futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-197">If you want toounregister hello subscription, run hello following command:</span></span>

        # sudo subscription-manager unregister

14. <span data-ttu-id="70e88-198">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="70e88-198">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. <span data-ttu-id="70e88-199">Kattintson a **művelet** > **leállítása** a Hyper-V kezelőjében.</span><span class="sxs-lookup"><span data-stu-id="70e88-199">Click **Action** > **Shut Down** in Hyper-V Manager.</span></span> <span data-ttu-id="70e88-200">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="70e88-200">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>


## <a name="prepare-a-red-hat-based-virtual-machine-from-kvm"></a><span data-ttu-id="70e88-201">Készítse elő a Red Hat-alapú virtuális gépek KVM a</span><span class="sxs-lookup"><span data-stu-id="70e88-201">Prepare a Red Hat-based virtual machine from KVM</span></span>
### <a name="prepare-a-rhel-6-virtual-machine-from-kvm"></a><span data-ttu-id="70e88-202">A KVM RHEL 6 virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="70e88-202">Prepare a RHEL 6 virtual machine from KVM</span></span>

1. <span data-ttu-id="70e88-203">Töltse le az RHEL 6 hello KVM képe hello Red Hat webhelyről.</span><span class="sxs-lookup"><span data-stu-id="70e88-203">Download hello KVM image of RHEL 6 from hello Red Hat website.</span></span>

2. <span data-ttu-id="70e88-204">Legfelső szintű jelszót állíthat be.</span><span class="sxs-lookup"><span data-stu-id="70e88-204">Set a root password.</span></span>

    <span data-ttu-id="70e88-205">Egy titkosított jelszót létrehozni, és másolja a hello parancs kimenetében hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-205">Generate an encrypted password, and copy hello output of hello command:</span></span>

        # openssl passwd -1 changeme

    <span data-ttu-id="70e88-206">Állítsa be a guestfish egy gyökérszintű jelszót:</span><span class="sxs-lookup"><span data-stu-id="70e88-206">Set a root password with guestfish:</span></span>
        
        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   <span data-ttu-id="70e88-207">Változás hello második mezője hello gyökér szintű felhasználó a "!"</span><span class="sxs-lookup"><span data-stu-id="70e88-207">Change hello second field of hello root user from "!!"</span></span> <span data-ttu-id="70e88-208">toohello titkosított jelszót.</span><span class="sxs-lookup"><span data-stu-id="70e88-208">toohello encrypted password.</span></span>

3. <span data-ttu-id="70e88-209">Hozzon létre egy virtuális gép KVM hello qcow2 lemezképéről.</span><span class="sxs-lookup"><span data-stu-id="70e88-209">Create a virtual machine in KVM from hello qcow2 image.</span></span> <span data-ttu-id="70e88-210">Hello lemez típusának beállítása túl**qcow2**, és állítsa be a virtuális hálózati illesztő eszközmodell hello túl**virtio**.</span><span class="sxs-lookup"><span data-stu-id="70e88-210">Set hello disk type too**qcow2**, and set hello virtual network interface device model too**virtio**.</span></span> <span data-ttu-id="70e88-211">Ezt követően indítsa el a hello virtuális gép, és jelentkezzen be rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="70e88-211">Then, start hello virtual machine, and sign in as root.</span></span>

4. <span data-ttu-id="70e88-212">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-212">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="70e88-213">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-213">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

6. <span data-ttu-id="70e88-214">Helyezze át vagy távolítsa el hello udev szabályok tooavoid hello Ethernet-adapter statikus szabályok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="70e88-214">Move (or remove) hello udev rules tooavoid generating static rules for hello Ethernet interface.</span></span> <span data-ttu-id="70e88-215">Ezek a szabályok problémákat okozhat, ha az Azure- vagy Hyper-v egy virtuális gép klónozását</span><span class="sxs-lookup"><span data-stu-id="70e88-215">These rules cause problems when you clone a virtual machine in Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

7. <span data-ttu-id="70e88-216">Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-216">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # chkconfig network on

8. <span data-ttu-id="70e88-217">A Red Hat előfizetés tooenable hello telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-217">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # subscription-manager register --auto-attach --username=XXX --password=XXX

9. <span data-ttu-id="70e88-218">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="70e88-218">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="70e88-219">toodo ezt a konfigurációt, nyissa meg `/boot/grub/menu.lst` egy szövegszerkesztőben, és győződjön meg arról, hogy hello alapértelmezett kernel tartalmazza a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-219">toodo this configuration, open `/boot/grub/menu.lst` in a text editor, and ensure that hello default kernel includes hello following parameters:</span></span>
    
        console=ttyS0 earlyprintk=ttyS0 rootdelay=300
    
    <span data-ttu-id="70e88-220">Ez biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="70e88-220">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span>
    
    <span data-ttu-id="70e88-221">Emellett javasoljuk, hogy távolítsa el a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-221">In addition, we recommend that you remove hello following parameters:</span></span>
    
        rhgb quiet crashkernel=auto
    
    <span data-ttu-id="70e88-222">Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="70e88-222">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="70e88-223">Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="70e88-223">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="70e88-224">Vegye figyelembe, hogy ez a paraméter csökkenti hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB, amelyek kisebb virtuálisgép-méretek a problémás lehet.</span><span class="sxs-lookup"><span data-stu-id="70e88-224">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

    >[!Important]
    <span data-ttu-id="70e88-225">RHEL 6.5-ös vagy korábbi is be kell állítani hello `numa=off` kernel paraméter.</span><span class="sxs-lookup"><span data-stu-id="70e88-225">RHEL 6.5 and earlier must also set hello `numa=off` kernel parameter.</span></span> <span data-ttu-id="70e88-226">Tekintse meg a Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span><span class="sxs-lookup"><span data-stu-id="70e88-226">See Red Hat [KB 436883](https://access.redhat.com/solutions/436883).</span></span>

10. <span data-ttu-id="70e88-227">Adja hozzá a Hyper-V modulok tooinitramfs:</span><span class="sxs-lookup"><span data-stu-id="70e88-227">Add Hyper-V modules tooinitramfs:</span></span>  

    <span data-ttu-id="70e88-228">Szerkesztés `/etc/dracut.conf`, és adja hozzá a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-228">Edit `/etc/dracut.conf`, and add hello following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="70e88-229">Építse újra initramfs:</span><span class="sxs-lookup"><span data-stu-id="70e88-229">Rebuild initramfs:</span></span>

        # dracut -f -v

11. <span data-ttu-id="70e88-230">Távolítsa el a felhő inicializálás:</span><span class="sxs-lookup"><span data-stu-id="70e88-230">Uninstall cloud-init:</span></span>

        # yum remove cloud-init

12. <span data-ttu-id="70e88-231">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="70e88-231">Ensure that hello SSH server is installed and configured toostart at boot time:</span></span>

        # chkconfig sshd on

    <span data-ttu-id="70e88-232">Módosítsa az alábbi /etc/ssh/sshd_config tooinclude hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-232">Modify /etc/ssh/sshd_config tooinclude hello following lines:</span></span>

        PasswordAuthentication yes
        ClientAliveInterval 180

13. <span data-ttu-id="70e88-233">hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve.</span><span class="sxs-lookup"><span data-stu-id="70e88-233">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="70e88-234">Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-234">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

14. <span data-ttu-id="70e88-235">Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-235">Install hello Azure Linux Agent by running hello following command:</span></span>

        # yum install WALinuxAgent

        # chkconfig waagent on

15. <span data-ttu-id="70e88-236">hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="70e88-236">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="70e88-237">Vegye figyelembe, hogy hello helyi erőforrás egy ideiglenes lemez, és előfordulhat, hogy lehet üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="70e88-237">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="70e88-238">Telepítése után hello Azure Linux ügynök az előző lépésben hello, módosítsa a következő paramétereket a hello **/etc/waagent.conf** megfelelően:</span><span class="sxs-lookup"><span data-stu-id="70e88-238">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in **/etc/waagent.conf** appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. <span data-ttu-id="70e88-239">Hello előfizetés regisztrációját (ha szükséges) hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-239">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # subscription-manager unregister

17. <span data-ttu-id="70e88-240">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="70e88-240">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. <span data-ttu-id="70e88-241">Állítsa le a virtuális gép hello KVM.</span><span class="sxs-lookup"><span data-stu-id="70e88-241">Shut down hello virtual machine in KVM.</span></span>

19. <span data-ttu-id="70e88-242">Az átalakítás hello qcow2 kép toohello VHD formátummal.</span><span class="sxs-lookup"><span data-stu-id="70e88-242">Convert hello qcow2 image toohello VHD format.</span></span>

    <span data-ttu-id="70e88-243">Először alakítsa át hello tooraw képformátum:</span><span class="sxs-lookup"><span data-stu-id="70e88-243">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f qcow2 -O raw rhel-6.8.qcow2 rhel-6.8.raw

    <span data-ttu-id="70e88-244">Győződjön meg arról, hogy hello nyers lemezkép hello mérete 1 MB igazodik.</span><span class="sxs-lookup"><span data-stu-id="70e88-244">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="70e88-245">Ellenkező esetben kerekíteni hello mérete tooalign 1 MB:</span><span class="sxs-lookup"><span data-stu-id="70e88-245">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="70e88-246">Alakítsa át a nyers tooa hello rögzített méretű virtuális merevlemez:</span><span class="sxs-lookup"><span data-stu-id="70e88-246">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd


### <a name="prepare-a-rhel-7-virtual-machine-from-kvm"></a><span data-ttu-id="70e88-247">A KVM RHEL 7 virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="70e88-247">Prepare a RHEL 7 virtual machine from KVM</span></span>

1. <span data-ttu-id="70e88-248">Töltse le az RHEL 7 hello KVM képe hello Red Hat webhelyről.</span><span class="sxs-lookup"><span data-stu-id="70e88-248">Download hello KVM image of RHEL 7 from hello Red Hat website.</span></span> <span data-ttu-id="70e88-249">Ez az eljárás RHEL 7 hello példaként használja.</span><span class="sxs-lookup"><span data-stu-id="70e88-249">This procedure uses RHEL 7 as hello example.</span></span>

2. <span data-ttu-id="70e88-250">Legfelső szintű jelszót állíthat be.</span><span class="sxs-lookup"><span data-stu-id="70e88-250">Set a root password.</span></span>

    <span data-ttu-id="70e88-251">Egy titkosított jelszót létrehozni, és másolja a hello parancs kimenetében hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-251">Generate an encrypted password, and copy hello output of hello command:</span></span>

        # openssl passwd -1 changeme

    <span data-ttu-id="70e88-252">Állítsa be a guestfish egy gyökérszintű jelszót:</span><span class="sxs-lookup"><span data-stu-id="70e88-252">Set a root password with guestfish:</span></span>

        # guestfish --rw -a <image-name>
        > <fs> run
        > <fs> list-filesystems
        > <fs> mount /dev/sda1 /
        > <fs> vi /etc/shadow
        > <fs> exit

   <span data-ttu-id="70e88-253">Változás hello második mezője gyökér szintű felhasználó a "!"</span><span class="sxs-lookup"><span data-stu-id="70e88-253">Change hello second field of root user from "!!"</span></span> <span data-ttu-id="70e88-254">toohello titkosított jelszót.</span><span class="sxs-lookup"><span data-stu-id="70e88-254">toohello encrypted password.</span></span>

3. <span data-ttu-id="70e88-255">Hozzon létre egy virtuális gép KVM hello qcow2 lemezképéről.</span><span class="sxs-lookup"><span data-stu-id="70e88-255">Create a virtual machine in KVM from hello qcow2 image.</span></span> <span data-ttu-id="70e88-256">Hello lemez típusának beállítása túl**qcow2**, és állítsa be a virtuális hálózati illesztő eszközmodell hello túl**virtio**.</span><span class="sxs-lookup"><span data-stu-id="70e88-256">Set hello disk type too**qcow2**, and set hello virtual network interface device model too**virtio**.</span></span> <span data-ttu-id="70e88-257">Ezt követően indítsa el a hello virtuális gép, és jelentkezzen be rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="70e88-257">Then, start hello virtual machine, and sign in as root.</span></span>

4. <span data-ttu-id="70e88-258">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-258">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

5. <span data-ttu-id="70e88-259">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-259">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

6. <span data-ttu-id="70e88-260">Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-260">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # chkconfig network on

7. <span data-ttu-id="70e88-261">A Red Hat előfizetés tooenable telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-261">Register your Red Hat subscription tooenable installation of packages from hello RHEL repository by running hello following command:</span></span>

        # subscription-manager register --auto-attach --username=XXX --password=XXX

8. <span data-ttu-id="70e88-262">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="70e88-262">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="70e88-263">toodo ezt a konfigurációt, nyissa meg `/etc/default/grub` egy szövegszerkesztőben, és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter.</span><span class="sxs-lookup"><span data-stu-id="70e88-263">toodo this configuration, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="70e88-264">Példa:</span><span class="sxs-lookup"><span data-stu-id="70e88-264">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="70e88-265">Ez a parancs is biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="70e88-265">This command also ensures that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="70e88-266">Hello parancsot is a hálózati adapterek hello új RHEL 7 elnevezési konvenciókat kikapcsolása.</span><span class="sxs-lookup"><span data-stu-id="70e88-266">hello command also turns off hello new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="70e88-267">Emellett javasoljuk, hogy távolítsa el a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-267">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="70e88-268">Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="70e88-268">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="70e88-269">Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="70e88-269">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="70e88-270">Vegye figyelembe, hogy ez a paraméter csökkenti hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB, amelyek kisebb virtuálisgép-méretek a problémás lehet.</span><span class="sxs-lookup"><span data-stu-id="70e88-270">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

9. <span data-ttu-id="70e88-271">Miután befejezte az szerkesztési `/etc/default/grub`- ben futtassa hello következő toorebuild hello lárvajárat konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="70e88-271">After you are done editing `/etc/default/grub`, run hello following command toorebuild hello grub configuration:</span></span>

        # grub2-mkconfig -o /boot/grub2/grub.cfg

10. <span data-ttu-id="70e88-272">Vegyen fel Hyper-V-modulokkal initramfs.</span><span class="sxs-lookup"><span data-stu-id="70e88-272">Add Hyper-V modules into initramfs.</span></span>

    <span data-ttu-id="70e88-273">Szerkesztés `/etc/dracut.conf` , és adja hozzá a tartalomhoz:</span><span class="sxs-lookup"><span data-stu-id="70e88-273">Edit `/etc/dracut.conf` and add content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="70e88-274">Építse újra initramfs:</span><span class="sxs-lookup"><span data-stu-id="70e88-274">Rebuild initramfs:</span></span>

        # dracut -f -v

11. <span data-ttu-id="70e88-275">Távolítsa el a felhő inicializálás:</span><span class="sxs-lookup"><span data-stu-id="70e88-275">Uninstall cloud-init:</span></span>

        # yum remove cloud-init

12. <span data-ttu-id="70e88-276">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="70e88-276">Ensure that hello SSH server is installed and configured toostart at boot time:</span></span>

        # systemctl enable sshd

    <span data-ttu-id="70e88-277">Módosítsa az alábbi /etc/ssh/sshd_config tooinclude hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-277">Modify /etc/ssh/sshd_config tooinclude hello following lines:</span></span>

        PasswordAuthentication yes
        ClientAliveInterval 180

13. <span data-ttu-id="70e88-278">hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve.</span><span class="sxs-lookup"><span data-stu-id="70e88-278">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="70e88-279">Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-279">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

14. <span data-ttu-id="70e88-280">Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-280">Install hello Azure Linux Agent by running hello following command:</span></span>

        # yum install WALinuxAgent

    <span data-ttu-id="70e88-281">Hello waagent szolgáltatás engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="70e88-281">Enable hello waagent service:</span></span>

        # systemctl enable waagent.service

15. <span data-ttu-id="70e88-282">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="70e88-282">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="70e88-283">hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="70e88-283">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="70e88-284">Vegye figyelembe, hogy hello helyi erőforrás egy ideiglenes lemez, és előfordulhat, hogy lehet üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="70e88-284">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="70e88-285">Telepítése után hello Azure Linux ügynök az előző lépésben hello, módosítsa a következő paramétereket a hello `/etc/waagent.conf` megfelelően:</span><span class="sxs-lookup"><span data-stu-id="70e88-285">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

16. <span data-ttu-id="70e88-286">Hello előfizetés regisztrációját (ha szükséges) hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-286">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # subscription-manager unregister

17. <span data-ttu-id="70e88-287">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="70e88-287">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

18. <span data-ttu-id="70e88-288">Állítsa le a virtuális gép hello KVM.</span><span class="sxs-lookup"><span data-stu-id="70e88-288">Shut down hello virtual machine in KVM.</span></span>

19. <span data-ttu-id="70e88-289">Az átalakítás hello qcow2 kép toohello VHD formátummal.</span><span class="sxs-lookup"><span data-stu-id="70e88-289">Convert hello qcow2 image toohello VHD format.</span></span>

    <span data-ttu-id="70e88-290">Először alakítsa át hello tooraw képformátum:</span><span class="sxs-lookup"><span data-stu-id="70e88-290">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f qcow2 -O raw rhel-7.3.qcow2 rhel-7.3.raw

    <span data-ttu-id="70e88-291">Győződjön meg arról, hogy hello nyers lemezkép hello mérete 1 MB igazodik.</span><span class="sxs-lookup"><span data-stu-id="70e88-291">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="70e88-292">Ellenkező esetben kerekíteni hello mérete tooalign 1 MB:</span><span class="sxs-lookup"><span data-stu-id="70e88-292">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="70e88-293">Alakítsa át a nyers tooa hello rögzített méretű virtuális merevlemez:</span><span class="sxs-lookup"><span data-stu-id="70e88-293">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-vmware"></a><span data-ttu-id="70e88-294">A VMware Red Hat-alapú virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="70e88-294">Prepare a Red Hat-based virtual machine from VMware</span></span>
### <a name="prerequisites"></a><span data-ttu-id="70e88-295">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="70e88-295">Prerequisites</span></span>
<span data-ttu-id="70e88-296">Ez a szakasz azt feltételezi, hogy még telepítette RHEL virtuális gép VMware-ben.</span><span class="sxs-lookup"><span data-stu-id="70e88-296">This section assumes that you have already installed a RHEL virtual machine in VMware.</span></span> <span data-ttu-id="70e88-297">Hogyan tooinstall VMware, operációs rendszer: vonatkozó további információért [VMware vendég operációs rendszer telepítési útmutató](http://partnerweb.vmware.com/GOSIG/home.html).</span><span class="sxs-lookup"><span data-stu-id="70e88-297">For details about how tooinstall an operating system in VMware, see [VMware Guest Operating System Installation Guide](http://partnerweb.vmware.com/GOSIG/home.html).</span></span>

* <span data-ttu-id="70e88-298">Hello Linux operációs rendszer telepítésekor azt javasoljuk, hogy Ön normál partíció helyett használjon LVM, ez utóbbi érték gyakran hello alapértelmezett sok telepítés.</span><span class="sxs-lookup"><span data-stu-id="70e88-298">When you install hello Linux operating system, we recommend that you use standard partitions rather than LVM, which is often hello default for many installations.</span></span> <span data-ttu-id="70e88-299">LVM neve ütközik a klónozott virtuális gépet, így elkerülhető, különösen akkor, ha a hibaelhárítási legalább egyszer kell csatolt toobe tooanother virtuális gép operációsrendszer-lemez.</span><span class="sxs-lookup"><span data-stu-id="70e88-299">This will avoid LVM name conflicts with cloned virtual machine, particularly if an operating system disk ever needs toobe attached tooanother virtual machine for troubleshooting.</span></span> <span data-ttu-id="70e88-300">LVM vagy RAID használható adatlemezek Ha előnyben részesített.</span><span class="sxs-lookup"><span data-stu-id="70e88-300">LVM or RAID can be used on data disks if preferred.</span></span>
* <span data-ttu-id="70e88-301">Ne konfiguráljon egy swap partíció hello operációsrendszer-lemez.</span><span class="sxs-lookup"><span data-stu-id="70e88-301">Do not configure a swap partition on hello operating system disk.</span></span> <span data-ttu-id="70e88-302">Hello Linux ügynök toocreate hello ideiglenes erőforrás lemezen a lapozófájl konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="70e88-302">You can configure hello Linux agent toocreate a swap file on hello temporary resource disk.</span></span> <span data-ttu-id="70e88-303">További információk a találhatók hello szükséges lépésekről.</span><span class="sxs-lookup"><span data-stu-id="70e88-303">You can find more information about this in hello steps that follow.</span></span>
* <span data-ttu-id="70e88-304">Hello virtuális merevlemez létrehozásakor válassza ki a **tároló virtuális lemez egyetlen fájlként**.</span><span class="sxs-lookup"><span data-stu-id="70e88-304">When you create hello virtual hard disk, select **Store virtual disk as a single file**.</span></span>

### <a name="prepare-a-rhel-6-virtual-machine-from-vmware"></a><span data-ttu-id="70e88-305">A VMware RHEL 6 virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="70e88-305">Prepare a RHEL 6 virtual machine from VMware</span></span>
1. <span data-ttu-id="70e88-306">Az RHEL 6 NetworkManager zavarhatja hello Azure Linux ügynök.</span><span class="sxs-lookup"><span data-stu-id="70e88-306">In RHEL 6, NetworkManager can interfere with hello Azure Linux agent.</span></span> <span data-ttu-id="70e88-307">Ez a csomag eltávolítása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-307">Uninstall this package by running hello following command:</span></span>
   
        # sudo rpm -e --nodeps NetworkManager

2. <span data-ttu-id="70e88-308">Hozzon létre egy fájlt **hálózati** hello etc/sysconfig/könyvtárban, amely tartalmazza a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-308">Create a file named **network** in hello /etc/sysconfig/ directory that contains hello following text:</span></span>

        NETWORKING=yes
        HOSTNAME=localhost.localdomain

3. <span data-ttu-id="70e88-309">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-309">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no

4. <span data-ttu-id="70e88-310">Helyezze át vagy távolítsa el hello udev szabályok tooavoid hello Ethernet-adapter statikus szabályok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="70e88-310">Move (or remove) hello udev rules tooavoid generating static rules for hello Ethernet interface.</span></span> <span data-ttu-id="70e88-311">Ezek a szabályok problémákat okozhat, ha az Azure- vagy Hyper-v egy virtuális gép klónozását</span><span class="sxs-lookup"><span data-stu-id="70e88-311">These rules cause problems when you clone a virtual machine in Azure or Hyper-V:</span></span>

        # sudo ln -s /dev/null /etc/udev/rules.d/75-persistent-net-generator.rules

        # sudo rm -f /etc/udev/rules.d/70-persistent-net.rules

5. <span data-ttu-id="70e88-312">Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-312">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

6. <span data-ttu-id="70e88-313">A Red Hat előfizetés tooenable hello telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-313">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

7. <span data-ttu-id="70e88-314">hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve.</span><span class="sxs-lookup"><span data-stu-id="70e88-314">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="70e88-315">Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-315">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-6-server-extras-rpms

8. <span data-ttu-id="70e88-316">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="70e88-316">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="70e88-317">toodo a, nyissa meg `/etc/default/grub` egy szövegszerkesztőben, és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter.</span><span class="sxs-lookup"><span data-stu-id="70e88-317">toodo this, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="70e88-318">Példa:</span><span class="sxs-lookup"><span data-stu-id="70e88-318">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0"
   
   <span data-ttu-id="70e88-319">Ez biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="70e88-319">This will also ensure that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="70e88-320">Emellett javasoljuk, hogy távolítsa el a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-320">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="70e88-321">Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="70e88-321">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="70e88-322">Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="70e88-322">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="70e88-323">Vegye figyelembe, hogy ez a paraméter csökkenti hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB, amelyek kisebb virtuálisgép-méretek a problémás lehet.</span><span class="sxs-lookup"><span data-stu-id="70e88-323">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

9. <span data-ttu-id="70e88-324">Adja hozzá a Hyper-V modulok tooinitramfs:</span><span class="sxs-lookup"><span data-stu-id="70e88-324">Add Hyper-V modules tooinitramfs:</span></span>

    <span data-ttu-id="70e88-325">Szerkesztés `/etc/dracut.conf`, és adja hozzá a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-325">Edit `/etc/dracut.conf`, and add hello following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="70e88-326">Építse újra initramfs:</span><span class="sxs-lookup"><span data-stu-id="70e88-326">Rebuild initramfs:</span></span>

        # dracut -f -v

10. <span data-ttu-id="70e88-327">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás, amely általában hello alapértelmezett toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="70e88-327">Ensure that hello SSH server is installed and configured toostart at boot time, which is usually hello default.</span></span> <span data-ttu-id="70e88-328">Módosítsa `/etc/ssh/sshd_config` tooinclude hello a következő sort:</span><span class="sxs-lookup"><span data-stu-id="70e88-328">Modify `/etc/ssh/sshd_config` tooinclude hello following line:</span></span>

    <span data-ttu-id="70e88-329">ClientAliveInterval 180</span><span class="sxs-lookup"><span data-stu-id="70e88-329">ClientAliveInterval 180</span></span>

11. <span data-ttu-id="70e88-330">Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-330">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo chkconfig waagent on

12. <span data-ttu-id="70e88-331">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="70e88-331">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="70e88-332">hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="70e88-332">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="70e88-333">Vegye figyelembe, hogy hello helyi erőforrás egy ideiglenes lemez, és előfordulhat, hogy lehet üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="70e88-333">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="70e88-334">Telepítése után hello Azure Linux ügynök az előző lépésben hello, módosítsa a következő paramétereket a hello `/etc/waagent.conf` megfelelően:</span><span class="sxs-lookup"><span data-stu-id="70e88-334">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

13. <span data-ttu-id="70e88-335">Hello előfizetés regisztrációját (ha szükséges) hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-335">Unregister hello subscription (if necessary) by running hello following command:</span></span>

        # sudo subscription-manager unregister

14. <span data-ttu-id="70e88-336">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="70e88-336">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

15. <span data-ttu-id="70e88-337">Hello virtuális gép leállítása és hello VMDK fájlt tooa .vhd-fájllá alakítsa át.</span><span class="sxs-lookup"><span data-stu-id="70e88-337">Shut down hello virtual machine, and convert hello VMDK file tooa .vhd file.</span></span>

    <span data-ttu-id="70e88-338">Először alakítsa át hello tooraw képformátum:</span><span class="sxs-lookup"><span data-stu-id="70e88-338">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f vmdk -O raw rhel-6.8.vmdk rhel-6.8.raw

    <span data-ttu-id="70e88-339">Győződjön meg arról, hogy hello nyers lemezkép hello mérete 1 MB igazodik.</span><span class="sxs-lookup"><span data-stu-id="70e88-339">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="70e88-340">Ellenkező esetben kerekíteni hello mérete tooalign 1 MB:</span><span class="sxs-lookup"><span data-stu-id="70e88-340">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="70e88-341">Alakítsa át a nyers tooa hello rögzített méretű virtuális merevlemez:</span><span class="sxs-lookup"><span data-stu-id="70e88-341">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-6.8.raw rhel-6.8.vhd

### <a name="prepare-a-rhel-7-virtual-machine-from-vmware"></a><span data-ttu-id="70e88-342">A VMware RHEL 7 virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="70e88-342">Prepare a RHEL 7 virtual machine from VMware</span></span>
1. <span data-ttu-id="70e88-343">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-343">Create or edit hello `/etc/sysconfig/network` file, and add hello following text:</span></span>
   
        NETWORKING=yes
        HOSTNAME=localhost.localdomain

2. <span data-ttu-id="70e88-344">Hozzon létre vagy szerkessze a hello `/etc/sysconfig/network-scripts/ifcfg-eth0` fájlt, és adja hozzá a következő szöveg hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-344">Create or edit hello `/etc/sysconfig/network-scripts/ifcfg-eth0` file, and add hello following text:</span></span>
   
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no

3. <span data-ttu-id="70e88-345">Győződjön meg arról, hogy hello hálózati szolgáltatás rendszerindításkor megkezdődik hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-345">Ensure that hello network service will start at boot time by running hello following command:</span></span>

        # sudo chkconfig network on

4. <span data-ttu-id="70e88-346">A Red Hat előfizetés tooenable hello telepített RHEL adattárból hello csomagok regisztrálása hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-346">Register your Red Hat subscription tooenable hello installation of packages from hello RHEL repository by running hello following command:</span></span>

        # sudo subscription-manager register --auto-attach --username=XXX --password=XXX

5. <span data-ttu-id="70e88-347">Hello kernel rendszerindító sor a lárvajárat konfigurációs tooinclude kiegészítő rendszermag paraméterek módosítása az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="70e88-347">Modify hello kernel boot line in your grub configuration tooinclude additional kernel parameters for Azure.</span></span> <span data-ttu-id="70e88-348">toodo módosítást, nyissa meg `/etc/default/grub` egy szövegszerkesztőben, és a Szerkesztés hello `GRUB_CMDLINE_LINUX` paraméter.</span><span class="sxs-lookup"><span data-stu-id="70e88-348">toodo this modification, open `/etc/default/grub` in a text editor, and edit hello `GRUB_CMDLINE_LINUX` parameter.</span></span> <span data-ttu-id="70e88-349">Példa:</span><span class="sxs-lookup"><span data-stu-id="70e88-349">For example:</span></span>
   
        GRUB_CMDLINE_LINUX="rootdelay=300 console=ttyS0 earlyprintk=ttyS0 net.ifnames=0"
   
   <span data-ttu-id="70e88-350">Ez a konfiguráció is biztosítja, hogy minden konzol üzenetküldés toohello első soros port, amely segít az Azure által támogatott hibáinak feltárására.</span><span class="sxs-lookup"><span data-stu-id="70e88-350">This configuration also ensures that all console messages are sent toohello first serial port, which can assist Azure support with debugging issues.</span></span> <span data-ttu-id="70e88-351">Azt is kikapcsolja hello új RHEL 7 elnevezési konvenciói a hálózati adapterek.</span><span class="sxs-lookup"><span data-stu-id="70e88-351">It also turns off hello new RHEL 7 naming conventions for NICs.</span></span> <span data-ttu-id="70e88-352">Emellett javasoljuk, hogy távolítsa el a következő paraméterek hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-352">In addition, we recommend that you remove hello following parameters:</span></span>
   
        rhgb quiet crashkernel=auto
   
    <span data-ttu-id="70e88-353">Grafikus és a csendes rendszerindító egy felhőalapú környezetben, ahol azt szeretnénk, hogy minden hello naplók toobe küldött toohello soros port nem hasznosak.</span><span class="sxs-lookup"><span data-stu-id="70e88-353">Graphical and quiet boot are not useful in a cloud environment where we want all hello logs toobe sent toohello serial port.</span></span> <span data-ttu-id="70e88-354">Meghagyhatja hello `crashkernel` beállítás konfigurálva, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="70e88-354">You can leave hello `crashkernel` option configured if desired.</span></span> <span data-ttu-id="70e88-355">Vegye figyelembe, hogy ez a paraméter csökkenti hello rendelkezésre álló memória mennyiségétől hello virtuális gépen legalább 128 MB, amelyek kisebb virtuálisgép-méretek a problémás lehet.</span><span class="sxs-lookup"><span data-stu-id="70e88-355">Note that this parameter reduces hello amount of available memory in hello virtual machine by 128 MB or more, which might be problematic on smaller virtual machine sizes.</span></span>

6. <span data-ttu-id="70e88-356">Miután befejezte az szerkesztési `/etc/default/grub`- ben futtassa hello következő toorebuild hello lárvajárat konfigurálása:</span><span class="sxs-lookup"><span data-stu-id="70e88-356">After you are done editing `/etc/default/grub`, run hello following command toorebuild hello grub configuration:</span></span>

        # sudo grub2-mkconfig -o /boot/grub2/grub.cfg

7. <span data-ttu-id="70e88-357">Adja hozzá a Hyper-V modulok tooinitramfs.</span><span class="sxs-lookup"><span data-stu-id="70e88-357">Add Hyper-V modules tooinitramfs.</span></span>

    <span data-ttu-id="70e88-358">Szerkesztés `/etc/dracut.conf`, adja hozzá a tartalomhoz:</span><span class="sxs-lookup"><span data-stu-id="70e88-358">Edit `/etc/dracut.conf`, add content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

    <span data-ttu-id="70e88-359">Építse újra initramfs:</span><span class="sxs-lookup"><span data-stu-id="70e88-359">Rebuild initramfs:</span></span>

        # dracut -f -v

8. <span data-ttu-id="70e88-360">Győződjön meg arról, hogy hello SSH kiszolgáló telepítve van, és rendszerindítás toostart konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="70e88-360">Ensure that hello SSH server is installed and configured toostart at boot time.</span></span> <span data-ttu-id="70e88-361">Ez a beállítás általában hello alapértelmezett beállítás.</span><span class="sxs-lookup"><span data-stu-id="70e88-361">This setting is usually hello default.</span></span> <span data-ttu-id="70e88-362">Módosítsa `/etc/ssh/sshd_config` tooinclude hello a következő sort:</span><span class="sxs-lookup"><span data-stu-id="70e88-362">Modify `/etc/ssh/sshd_config` tooinclude hello following line:</span></span>

        ClientAliveInterval 180

9. <span data-ttu-id="70e88-363">hello WALinuxAgent csomag, `WALinuxAgent-<version>`, toohello Red Hat kiegészítő funkciók tárház lett leküldve.</span><span class="sxs-lookup"><span data-stu-id="70e88-363">hello WALinuxAgent package, `WALinuxAgent-<version>`, has been pushed toohello Red Hat extras repository.</span></span> <span data-ttu-id="70e88-364">Engedélyezze hello kiegészítő funkciók tárház hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-364">Enable hello extras repository by running hello following command:</span></span>

        # subscription-manager repos --enable=rhel-7-server-extras-rpms

10. <span data-ttu-id="70e88-365">Hello Azure Linux ügynök telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="70e88-365">Install hello Azure Linux Agent by running hello following command:</span></span>

        # sudo yum install WALinuxAgent

        # sudo systemctl enable waagent.service

11. <span data-ttu-id="70e88-366">Ne hozzon létre a hello operációsrendszer-lemez lapozóterület.</span><span class="sxs-lookup"><span data-stu-id="70e88-366">Do not create swap space on hello operating system disk.</span></span>

    <span data-ttu-id="70e88-367">hello Azure Linux ügynök automatikusan konfigurálhatja lapozóterület hello helyi erőforrás lemezen, amely virtuális géphez csatolt toohello Azure hello virtuális gép kiépítése után.</span><span class="sxs-lookup"><span data-stu-id="70e88-367">hello Azure Linux Agent can automatically configure swap space by using hello local resource disk that is attached toohello virtual machine after hello virtual machine is provisioned on Azure.</span></span> <span data-ttu-id="70e88-368">Vegye figyelembe, hogy hello helyi erőforrás egy ideiglenes lemez, és előfordulhat, hogy lehet üríteni, megszüntetett hello virtuális gép esetén.</span><span class="sxs-lookup"><span data-stu-id="70e88-368">Note that hello local resource disk is a temporary disk, and it might be emptied when hello virtual machine is deprovisioned.</span></span> <span data-ttu-id="70e88-369">Telepítése után hello Azure Linux ügynök az előző lépésben hello, módosítsa a következő paramétereket a hello `/etc/waagent.conf` megfelelően:</span><span class="sxs-lookup"><span data-stu-id="70e88-369">After you install hello Azure Linux Agent in hello previous step, modify hello following parameters in `/etc/waagent.conf` appropriately:</span></span>

        ResourceDisk.Format=y
        ResourceDisk.Filesystem=ext4
        ResourceDisk.MountPoint=/mnt/resource
        ResourceDisk.EnableSwap=y
        ResourceDisk.SwapSizeMB=2048    ## NOTE: set this toowhatever you need it toobe.

12. <span data-ttu-id="70e88-370">Toounregister hello előfizetés tetszés szerint futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-370">If you want toounregister hello subscription, run hello following command:</span></span>

        # sudo subscription-manager unregister

13. <span data-ttu-id="70e88-371">Futtassa a következő parancsok toodeprovision hello virtuális gép hello, és előkészíti az Azure-on történő üzembe helyezéséhez:</span><span class="sxs-lookup"><span data-stu-id="70e88-371">Run hello following commands toodeprovision hello virtual machine and prepare it for provisioning on Azure:</span></span>

        # sudo waagent -force -deprovision

        # export HISTSIZE=0

        # logout

14. <span data-ttu-id="70e88-372">Hello virtuális gép leállítása, és alakítsa át a hello VMDK-fájl toohello VHD formátumú.</span><span class="sxs-lookup"><span data-stu-id="70e88-372">Shut down hello virtual machine, and convert hello VMDK file toohello VHD format.</span></span>

    <span data-ttu-id="70e88-373">Először alakítsa át hello tooraw képformátum:</span><span class="sxs-lookup"><span data-stu-id="70e88-373">First convert hello image tooraw format:</span></span>

        # qemu-img convert -f vmdk -O raw rhel-7.3.vmdk rhel-7.3.raw

    <span data-ttu-id="70e88-374">Győződjön meg arról, hogy hello nyers lemezkép hello mérete 1 MB igazodik.</span><span class="sxs-lookup"><span data-stu-id="70e88-374">Make sure that hello size of hello raw image is aligned with 1 MB.</span></span> <span data-ttu-id="70e88-375">Ellenkező esetben kerekíteni hello mérete tooalign 1 MB:</span><span class="sxs-lookup"><span data-stu-id="70e88-375">Otherwise, round up hello size tooalign with 1 MB:</span></span>

        # MB=$((1024*1024))
        # size=$(qemu-img info -f raw --output json "rhel-6.8.raw" | \
          gawk 'match($0, /"virtual-size": ([0-9]+),/, val) {print val[1]}')

        # rounded_size=$((($size/$MB + 1)*$MB))
        # qemu-img resize rhel-6.8.raw $rounded_size

    <span data-ttu-id="70e88-376">Alakítsa át a nyers tooa hello rögzített méretű virtuális merevlemez:</span><span class="sxs-lookup"><span data-stu-id="70e88-376">Convert hello raw disk tooa fixed-sized VHD:</span></span>

        # qemu-img convert -f raw -o subformat=fixed -O vpc rhel-7.3.raw rhel-7.3.vhd

## <a name="prepare-a-red-hat-based-virtual-machine-from-an-iso-by-using-a-kickstart-file-automatically"></a><span data-ttu-id="70e88-377">Készítse elő a Red Hat-alapú virtuális gép ISO-Lemezképet a automatikusan kickstart fájl használatával</span><span class="sxs-lookup"><span data-stu-id="70e88-377">Prepare a Red Hat-based virtual machine from an ISO by using a kickstart file automatically</span></span>
### <a name="prepare-a-rhel-7-virtual-machine-from-a-kickstart-file"></a><span data-ttu-id="70e88-378">Kickstart fájlból RHEL 7 virtuális gép előkészítése</span><span class="sxs-lookup"><span data-stu-id="70e88-378">Prepare a RHEL 7 virtual machine from a kickstart file</span></span>

1.  <span data-ttu-id="70e88-379">Hozzon létre egy kickstart fájlt, amely tartalmazza a következő tartalmat hello, és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="70e88-379">Create a kickstart file that includes hello following content, and save hello file.</span></span> <span data-ttu-id="70e88-380">Kickstart telepítésével kapcsolatos részletekért lásd: hello [Kickstart a telepítési útmutató](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).</span><span class="sxs-lookup"><span data-stu-id="70e88-380">For details about kickstart installation, see hello [Kickstart Installation Guide](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Installation_Guide/chap-kickstart-installations.html).</span></span>

        # Kickstart for provisioning a RHEL 7 Azure VM

        # System authorization information
          auth --enableshadow --passalgo=sha512

        # Use graphical install
        text

        # Do not run hello Setup Agent on first boot
        firstboot --disable

        # Keyboard layouts
        keyboard --vckeymap=us --xlayouts='us'

        # System language
        lang en_US.UTF-8

        # Network information
        network  --bootproto=dhcp

        # Root password
        rootpw --plaintext "to_be_disabled"

        # System services
        services --enabled="sshd,waagent,NetworkManager"

        # System timezone
        timezone Etc/UTC --isUtc --ntpservers 0.rhel.pool.ntp.org,1.rhel.pool.ntp.org,2.rhel.pool.ntp.org,3.rhel.pool.ntp.org

        # Partition clearing information
        clearpart --all --initlabel

        # Clear hello MBR
        zerombr

        # Disk partitioning information
        part /boot --fstype="xfs" --size=500
        part / --fstyp="xfs" --size=1 --grow --asprimary

        # System bootloader configuration
        bootloader --location=mbr

        # Firewall configuration
        firewall --disabled

        # Enable SELinux
        selinux --enforcing

        # Don't configure X
        skipx

        # Power down hello machine after install
        poweroff

        %packages
        @base
        @console-internet
        chrony
        sudo
        parted
        -dracut-config-rescue

        %end

        %post --log=/var/log/anaconda/post-install.log

        #!/bin/bash

        # Register Red Hat Subscription
        subscription-manager register --username=XXX --password=XXX --auto-attach --force

        # Install latest repo update
        yum update -y

        # Enable extras repo
        subscription-manager repos --enable=rhel-7-server-extras-rpms

        # Install WALinuxAgent
        yum install -y WALinuxAgent

        # Unregister Red Hat subscription
        subscription-manager unregister

        # Enable waaagent at boot-up
        systemctl enable waagent

        # Disable hello root account
        usermod root -p '!!'

        # Configure swap in WALinuxAgent
        sed -i 's/^\(ResourceDisk\.EnableSwap\)=[Nn]$/\1=y/g' /etc/waagent.conf
        sed -i 's/^\(ResourceDisk\.SwapSizeMB\)=[0-9]*$/\1=2048/g' /etc/waagent.conf

        # Set hello cmdline
        sed -i 's/^\(GRUB_CMDLINE_LINUX\)=".*"$/\1="console=tty1 console=ttyS0 earlyprintk=ttyS0 rootdelay=300"/g' /etc/default/grub

        # Enable SSH keepalive
        sed -i 's/^#\(ClientAliveInterval\).*$/\1 180/g' /etc/ssh/sshd_config

        # Build hello grub cfg
        grub2-mkconfig -o /boot/grub2/grub.cfg

        # Configure network
        cat << EOF > /etc/sysconfig/network-scripts/ifcfg-eth0
        DEVICE=eth0
        ONBOOT=yes
        BOOTPROTO=dhcp
        TYPE=Ethernet
        USERCTL=no
        PEERDNS=yes
        IPV6INIT=no
        NM_CONTROLLED=no
        EOF

        # Deprovision and prepare for Azure
        waagent -force -deprovision

        %end

2. <span data-ttu-id="70e88-381">Helyezze a hello kickstart fájlt, ahol hello telepítési rendszer tudja-e érni.</span><span class="sxs-lookup"><span data-stu-id="70e88-381">Place hello kickstart file where hello installation system can access it.</span></span>

3. <span data-ttu-id="70e88-382">A Hyper-V kezelőjében hozzon létre egy új virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="70e88-382">In Hyper-V Manager, create a new virtual machine.</span></span> <span data-ttu-id="70e88-383">A hello **virtuális merevlemez csatlakoztatása** lapon jelölje be **virtuális merevlemez csatlakoztatása később**, és a teljes hello új virtuális gép varázsló.</span><span class="sxs-lookup"><span data-stu-id="70e88-383">On hello **Connect Virtual Hard Disk** page, select **Attach a virtual hard disk later**, and complete hello New Virtual Machine Wizard.</span></span>

4. <span data-ttu-id="70e88-384">Nyissa meg a hello virtuális gép beállításait:</span><span class="sxs-lookup"><span data-stu-id="70e88-384">Open hello virtual machine settings:</span></span>

    <span data-ttu-id="70e88-385">a.</span><span class="sxs-lookup"><span data-stu-id="70e88-385">a.</span></span>  <span data-ttu-id="70e88-386">Rendeljen a virtuális merevlemez toohello új virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="70e88-386">Attach a new virtual hard disk toohello virtual machine.</span></span> <span data-ttu-id="70e88-387">Győződjön meg arról, hogy tooselect **VHD formátumú** és **rögzített méretű**.</span><span class="sxs-lookup"><span data-stu-id="70e88-387">Make sure tooselect **VHD Format** and **Fixed Size**.</span></span>

    <span data-ttu-id="70e88-388">b.</span><span class="sxs-lookup"><span data-stu-id="70e88-388">b.</span></span>  <span data-ttu-id="70e88-389">Hello telepítési ISO toohello DVD-meghajtó csatlakoztatása.</span><span class="sxs-lookup"><span data-stu-id="70e88-389">Attach hello installation ISO toohello DVD drive.</span></span>

    <span data-ttu-id="70e88-390">c.</span><span class="sxs-lookup"><span data-stu-id="70e88-390">c.</span></span>  <span data-ttu-id="70e88-391">Állítsa be a hello BIOS tooboot CD-ről.</span><span class="sxs-lookup"><span data-stu-id="70e88-391">Set hello BIOS tooboot from CD.</span></span>

5. <span data-ttu-id="70e88-392">Hello virtuális gép elindításához.</span><span class="sxs-lookup"><span data-stu-id="70e88-392">Start hello virtual machine.</span></span> <span data-ttu-id="70e88-393">Amikor megjelenik a hello telepítési útmutatót, nyomja meg **lapon** tooconfigure hello indítási beállítások.</span><span class="sxs-lookup"><span data-stu-id="70e88-393">When hello installation guide appears, press **Tab** tooconfigure hello boot options.</span></span>

6. <span data-ttu-id="70e88-394">Adjon meg `inst.ks=<hello location of hello kickstart file>` hello indítási beállítások, és nyomja le az hello végén **Enter**.</span><span class="sxs-lookup"><span data-stu-id="70e88-394">Enter `inst.ks=<hello location of hello kickstart file>` at hello end of hello boot options, and press **Enter**.</span></span>

7. <span data-ttu-id="70e88-395">Várjon, amíg hello telepítési toofinish.</span><span class="sxs-lookup"><span data-stu-id="70e88-395">Wait for hello installation toofinish.</span></span> <span data-ttu-id="70e88-396">Amikor elkészült, a program automatikusan leáll hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="70e88-396">When it's finished, hello virtual machine will be shut down automatically.</span></span> <span data-ttu-id="70e88-397">A Linux virtuális merevlemez most készen áll a toobe feltöltött tooAzure van.</span><span class="sxs-lookup"><span data-stu-id="70e88-397">Your Linux VHD is now ready toobe uploaded tooAzure.</span></span>

## <a name="known-issues"></a><span data-ttu-id="70e88-398">Ismert problémák</span><span class="sxs-lookup"><span data-stu-id="70e88-398">Known issues</span></span>
### <a name="hello-hyper-v-driver-could-not-be-included-in-hello-initial-ram-disk-when-using-a-non-hyper-v-hypervisor"></a><span data-ttu-id="70e88-399">hello Hyper-V illesztőprogram sikerült sem szerepelhet hello kezdeti RAM-lemez nem Hyper-V hipervizort használatakor</span><span class="sxs-lookup"><span data-stu-id="70e88-399">hello Hyper-V driver could not be included in hello initial RAM disk when using a non-Hyper-V hypervisor</span></span>

<span data-ttu-id="70e88-400">Bizonyos esetekben Linux telepítők nem hello illesztőprogramokat tartalmazhatnak Hyper-V a hello kezdeti RAM-lemez (initrd vagy initramfs) kivéve, ha a Linux észleli, hogy fut-e a Hyper-V környezetben.</span><span class="sxs-lookup"><span data-stu-id="70e88-400">In some cases, Linux installers might not include hello drivers for Hyper-V in hello initial RAM disk (initrd or initramfs) unless Linux detects that it is running in a Hyper-V environment.</span></span>

<span data-ttu-id="70e88-401">Ha a Linux-lemezképet használ egy eltérő virtualizációs (Ez azt jelenti, hogy Virtualbox, Xen, stb.) a rendszer tooprepare, szükség lehet, hogy legalább hello hv_vmbus toorebuild initrd tooensure, és a hv_storvsc kernel modulok hello kezdeti RAM-lemez elérhető.</span><span class="sxs-lookup"><span data-stu-id="70e88-401">When you're using a different virtualization system (that is, Virtualbox, Xen, etc.) tooprepare your Linux image, you might need toorebuild initrd tooensure that at least hello hv_vmbus and hv_storvsc kernel modules are available on hello initial RAM disk.</span></span> <span data-ttu-id="70e88-402">Ez az egy ismert probléma legalább hello fölérendelt Red Hat terjesztési alapuló rendszereken.</span><span class="sxs-lookup"><span data-stu-id="70e88-402">This is a known issue at least on systems that are based on hello upstream Red Hat distribution.</span></span>

<span data-ttu-id="70e88-403">tooresolve probléma, adja hozzá a Hyper-V modulok tooinitramfs, és azt újjáépítenie:</span><span class="sxs-lookup"><span data-stu-id="70e88-403">tooresolve this issue, add Hyper-V modules tooinitramfs and rebuild it:</span></span>

<span data-ttu-id="70e88-404">Szerkesztés `/etc/dracut.conf`, és adja hozzá a tartalom a következő hello:</span><span class="sxs-lookup"><span data-stu-id="70e88-404">Edit `/etc/dracut.conf`, and add hello following content:</span></span>

        add_drivers+="hv_vmbus hv_netvsc hv_storvsc"

<span data-ttu-id="70e88-405">Építse újra initramfs:</span><span class="sxs-lookup"><span data-stu-id="70e88-405">Rebuild initramfs:</span></span>

        # dracut -f -v

<span data-ttu-id="70e88-406">További részletekért hello információk: [initramfs újraépítése](https://access.redhat.com/solutions/1958).</span><span class="sxs-lookup"><span data-stu-id="70e88-406">For more details, see hello information about [rebuilding initramfs](https://access.redhat.com/solutions/1958).</span></span>

## <a name="next-steps"></a><span data-ttu-id="70e88-407">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="70e88-407">Next steps</span></span>
<span data-ttu-id="70e88-408">Ön éppen most már készen áll a toouse a Red Hat Enterprise Linux virtuális merevlemez toocreate új virtuális gépeket az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="70e88-408">You're now ready toouse your Red Hat Enterprise Linux virtual hard disk toocreate new virtual machines in Azure.</span></span> <span data-ttu-id="70e88-409">Ha ez hello első alkalom, hogy feltöltendő hello .vhd fájl tooAzure, tekintse meg a 2. és 3 [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="70e88-409">If this is hello first time that you're uploading hello .vhd file tooAzure, see steps 2 and 3 in [Creating and uploading a virtual hard disk that contains hello Linux operating system](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

<span data-ttu-id="70e88-410">További információt, amelyek hello hipervizorok hitelesített toorun Red Hat Enterprise Linux, a következő témakörben: [hello Red Hat webhely](https://access.redhat.com/certified-hypervisors).</span><span class="sxs-lookup"><span data-stu-id="70e88-410">For more details about hello hypervisors that are certified toorun Red Hat Enterprise Linux, see [hello Red Hat website](https://access.redhat.com/certified-hypervisors).</span></span>
