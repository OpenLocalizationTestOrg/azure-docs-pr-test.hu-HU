---
title: "LVM konfigurálása a Linux rendszerű virtuális gép |} Microsoft Docs"
description: "Megtudhatja, hogyan LVM konfigurálása Linux az Azure-ban."
services: virtual-machines-linux
documentationcenter: na
author: szarkos
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: 7f533725-1484-479d-9472-6b3098d0aecc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: 7926627aaa3f0da935131f491d927ab5cb4b35c9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a><span data-ttu-id="a8fbd-103">A Linux virtuális gép az Azure-ban LVM konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a8fbd-103">Configure LVM on a Linux VM in Azure</span></span>
<span data-ttu-id="a8fbd-104">Ez a dokumentum bemutatja az logikai kötet Manager (LVM) konfigurálása az Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-104">This document will discuss how to configure Logical Volume Manager (LVM) in your Azure virtual machine.</span></span> <span data-ttu-id="a8fbd-105">Is megvalósítható LVM konfigurálása a virtuális géphez csatolt lemezen, alapértelmezés szerint a legtöbb felhő lemezképek nem lesz konfigurálva az operációsrendszer-lemezképet a LVM.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-105">While it is feasible to configure LVM on any disk attached to the virtual machine, by default most cloud images will not have LVM configured on the OS disk.</span></span> <span data-ttu-id="a8fbd-106">Ez a duplikált kötet csoportok problémák elkerülése érdekében, ha az operációs rendszer lemezének valaha is egy másik virtuális Géphez van csatolva az azonos terjesztési és típusa, azaz a helyreállítás során.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-106">This is to prevent problems with duplicate volume groups if the OS disk is ever attached to another VM of the same distribution and type, i.e. during a recovery scenario.</span></span> <span data-ttu-id="a8fbd-107">Ezért ajánlott, csak az adatlemezek LVM használandó.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-107">Therefore it is recommended only to use LVM on the data disks.</span></span>

## <a name="linear-vs-striped-logical-volumes"></a><span data-ttu-id="a8fbd-108">Csíkozott köteteken tárolni logikai és lineáris</span><span class="sxs-lookup"><span data-stu-id="a8fbd-108">Linear vs. striped logical volumes</span></span>
<span data-ttu-id="a8fbd-109">A fizikai lemezek számát egyesítése egyetlen tárolóköteten LVM használható.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-109">LVM can be used to combine a number of physical disks into a single storage volume.</span></span> <span data-ttu-id="a8fbd-110">Alapértelmezés szerint LVM általában létrehoz lineáris logikai kötetek, ami azt jelenti, hogy a fizikai tároló együtt összefűzendő-e.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-110">By default LVM will usually create linear logical volumes, which means that the physical storage is concatenated together.</span></span> <span data-ttu-id="a8fbd-111">Ebben az esetben olvasási/írási műveletek általában csak kapnak egyetlen lemezre.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-111">In this case read/write operations will typically only be sent to a single disk.</span></span> <span data-ttu-id="a8fbd-112">Ezzel szemben azt is létrehozhat ahol olvasási és írási elosztott több olyan lemezt, a kötet (azaz a RAID0 hasonlóan) csoportban lévő logikai csíkozott.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-112">In contrast, we can also create striped logical volumes where reads and writes are distributed to multiple disks contained in the volume group (i.e. similar to RAID0).</span></span> <span data-ttu-id="a8fbd-113">A teljesítményre vonatkozó megfontolásból valószínűleg érdemes a logikai kötetek paritásos, hogy az olvasások és írások használják a mellékelt adatok lemezek.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-113">For performance reasons it is likely you will want to stripe your logical volumes so that reads and writes utilize all your attached data disks.</span></span>

<span data-ttu-id="a8fbd-114">Ez a dokumentum azt ismerteti, hogyan több adatlemezek egyesítése egyetlen köteten csoport, majd hozza létre a logikai csíkozott kötetek.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-114">This document will describe how to combine several data disks into a single volume group, and then create a striped logical volume.</span></span> <span data-ttu-id="a8fbd-115">Az alábbi lépéseket történő együttműködésre a legtöbb terjesztéseket némileg általánosítva vannak.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-115">The steps below are somewhat generalized to work with most distributions.</span></span> <span data-ttu-id="a8fbd-116">A legtöbb esetben a segédprogramok és munkafolyamatok kezelése az Azure-on LVM nincsenek alapvetően más, mint más környezetekben.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-116">In most cases the utilities and workflows for managing LVM on Azure are not fundamentally different than other environments.</span></span> <span data-ttu-id="a8fbd-117">Szokásos módon is tekintse át a Linux forgalmazójával dokumentációk és ajánlott eljárások az adott terjesztési LVM használatával.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-117">As usual, please also consult your Linux vendor for documentation and best practices for using LVM with your particular distribution.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="a8fbd-118">Adatlemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="a8fbd-118">Attaching data disks</span></span>
<span data-ttu-id="a8fbd-119">Egy általában érdemes legalább két üres adatlemezekkel rendelkező start LVM használatakor.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-119">One will usually want to start with two or more empty data disks when using LVM.</span></span> <span data-ttu-id="a8fbd-120">IO igényei szerint, ha szeretné, csatlakoztassa a szabványos tároló, a legfeljebb 500 IO/ps / lemez vagy a prémium szintű storage, legfeljebb 5000 IO/ps lemezenként tárolt lemezeket.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-120">Based on your IO needs, you can choose to attach disks that are stored in our Standard Storage, with up to 500 IO/ps per disk or our Premium storage with up to 5000 IO/ps per disk.</span></span> <span data-ttu-id="a8fbd-121">Ez a cikk nem kerül részletes kiépíteni, és a Linux virtuális gép adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-121">This article will not go into detail on how to provision and attach data disks to a Linux virtual machine.</span></span> <span data-ttu-id="a8fbd-122">A Microsoft Azure cikke [lemezt csatlakoztatni](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) egy üres adatlemezt csatolni egy Linux virtuális gépet az Azure részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-122">Please see the Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how to attach an empty data disk to a Linux virtual machine on Azure.</span></span>

## <a name="install-the-lvm-utilities"></a><span data-ttu-id="a8fbd-123">Telepítse a LVM segédeszközöket</span><span class="sxs-lookup"><span data-stu-id="a8fbd-123">Install the LVM utilities</span></span>
* <span data-ttu-id="a8fbd-124">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="a8fbd-124">**Ubuntu**</span></span>

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* <span data-ttu-id="a8fbd-125">**RHEL, CentOS & Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="a8fbd-125">**RHEL, CentOS & Oracle Linux**</span></span>

    ```bash   
    sudo yum install lvm2
    ```

* <span data-ttu-id="a8fbd-126">**SLES 12 és openSUSE**</span><span class="sxs-lookup"><span data-stu-id="a8fbd-126">**SLES 12 and openSUSE**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

* <span data-ttu-id="a8fbd-127">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="a8fbd-127">**SLES 11**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

    <span data-ttu-id="a8fbd-128">A SLES11 is szerkeszteni kell `/etc/sysconfig/lvm` és `LVM_ACTIVATED_ON_DISCOVERED` az "Engedélyezés":</span><span class="sxs-lookup"><span data-stu-id="a8fbd-128">On SLES11 you must also edit `/etc/sysconfig/lvm` and set `LVM_ACTIVATED_ON_DISCOVERED` to "enable":</span></span>

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a><span data-ttu-id="a8fbd-129">LVM konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a8fbd-129">Configure LVM</span></span>
<span data-ttu-id="a8fbd-130">A jelen útmutató indulunk ki csatolt adatok három lemezt, amely lesz lesz az `/dev/sdc`, `/dev/sdd` és `/dev/sde`.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-130">In this guide we will assume you have attached three data disks, which we'll refer to as `/dev/sdc`, `/dev/sdd` and `/dev/sde`.</span></span> <span data-ttu-id="a8fbd-131">Előfordulhat, hogy ezek nem mindig a azonos útvonalneveket a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-131">Note that these may not always be the same path names in your VM.</span></span> <span data-ttu-id="a8fbd-132">Futtathatja a(z)`sudo fdisk -l`"vagy hasonló paranccsal listát készíthet a rendelkezésre álló lemezek.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-132">You can run '`sudo fdisk -l`' or similar command to list your available disks.</span></span>

1. <span data-ttu-id="a8fbd-133">Készítse elő a fizikai kötetekre:</span><span class="sxs-lookup"><span data-stu-id="a8fbd-133">Prepare the physical volumes:</span></span>

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. <span data-ttu-id="a8fbd-134">Hozzon létre egy kötet csoportot.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-134">Create a volume group.</span></span> <span data-ttu-id="a8fbd-135">Ebben a példában a kötet csoport hívjuk `data-vg01`:</span><span class="sxs-lookup"><span data-stu-id="a8fbd-135">In this example we are calling the volume group `data-vg01`:</span></span>

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. <span data-ttu-id="a8fbd-136">A logikai kötetek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-136">Create the logical volume(s).</span></span> <span data-ttu-id="a8fbd-137">A parancs azt alább hoz létre a következő nevű egyetlen logikai kötet `data-lv01` span a teljes kötet csoport, de vegye figyelembe, hogy azt is megvalósítható a több logikai köteteket a kötet csoport létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-137">The command below we will create a single logical volume called `data-lv01` to span the entire volume group, but note that it is also feasible to create multiple logical volumes in the volume group.</span></span>

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. <span data-ttu-id="a8fbd-138">A logikai kötet formázása</span><span class="sxs-lookup"><span data-stu-id="a8fbd-138">Format the logical volume</span></span>

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > <span data-ttu-id="a8fbd-139">SLES11 használatával `-t ext3` ext4 helyett.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-139">With SLES11 use `-t ext3` instead of ext4.</span></span> <span data-ttu-id="a8fbd-140">SLES11 csak olvasási hozzáférést ext4 fájlrendszerek támogatja.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-140">SLES11 only supports read-only access to ext4 filesystems.</span></span>

## <a name="add-the-new-file-system-to-etcfstab"></a><span data-ttu-id="a8fbd-141">Az új fájlrendszer /etc/fstab hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a8fbd-141">Add the new file system to /etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="a8fbd-142">Nem megfelelő módosítása a `/etc/fstab` fájl meghiúsulását eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-142">Improperly editing the `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="a8fbd-143">Ha nem ismeri, olvassa el a telepítési dokumentációban talál információkat arról a fájl megfelelően szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-143">If unsure, please refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="a8fbd-144">Is javasolt biztonsági másolatot a `/etc/fstab` fájl szerkesztése előtt jön létre.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-144">It is also recommended that a backup of the `/etc/fstab` file is created before editing.</span></span>

1. <span data-ttu-id="a8fbd-145">Létrehozhat például az új fájlrendszer, a megfelelő csatlakozási pont:</span><span class="sxs-lookup"><span data-stu-id="a8fbd-145">Create the desired mount point for your new file system, for example:</span></span>

    ```bash  
    sudo mkdir /data
    ```

2. <span data-ttu-id="a8fbd-146">Keresse meg a logikai elérési útjával</span><span class="sxs-lookup"><span data-stu-id="a8fbd-146">Locate the logical volume path</span></span>

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. <span data-ttu-id="a8fbd-147">Nyissa meg `/etc/fstab` egy szövegszerkesztőben, és adja hozzá a bejegyzést az új fájlrendszer, például:</span><span class="sxs-lookup"><span data-stu-id="a8fbd-147">Open `/etc/fstab` in a text editor and add an entry for the new file system, for example:</span></span>

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    <span data-ttu-id="a8fbd-148">Ezt követően mentse és zárja be `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-148">Then, save and close `/etc/fstab`.</span></span>

4. <span data-ttu-id="a8fbd-149">Tesztelhető, hogy a `/etc/fstab` bejegyzés helyességét:</span><span class="sxs-lookup"><span data-stu-id="a8fbd-149">Test that the `/etc/fstab` entry is correct:</span></span>

    ```bash    
    sudo mount -a
    ```

    <span data-ttu-id="a8fbd-150">Ha ezt a parancsot egy hibaüzenet ellenőrizze a szintaxist a `/etc/fstab` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-150">If this command results in an error message please check the syntax in the `/etc/fstab` file.</span></span>
   
    <span data-ttu-id="a8fbd-151">Ezután futtassa a `mount` parancsot annak biztosítására, hogy a fájlrendszer van csatlakoztatva:</span><span class="sxs-lookup"><span data-stu-id="a8fbd-151">Next run the `mount` command to ensure the file system is mounted:</span></span>

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="a8fbd-152">(Választható) A FailSafe rendszerindító paraméterek`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="a8fbd-152">(Optional) Failsafe boot parameters in `/etc/fstab`</span></span>
   
    <span data-ttu-id="a8fbd-153">Terjesztések valamelyikét tartalmazza a `nobootwait` vagy `nofail` csatlakoztatási fel paraméterek a `/etc/fstab` fájlt.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-153">Many distributions include either the `nobootwait` or `nofail` mount parameters that may be added to the `/etc/fstab` file.</span></span> <span data-ttu-id="a8fbd-154">Ezek a paraméterek hibák engedélyezése, ha egy adott fájlrendszer csatlakoztatása és a Linux rendszer akkor is, ha az nem csatolható fel a RAID fájlrendszer megfelelően elindulni.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-154">These parameters allow for failures when mounting a particular file system and allow the Linux system to continue to boot even if it is unable to properly mount the RAID file system.</span></span> <span data-ttu-id="a8fbd-155">További információt ezekről a paraméterekről a terjesztési dokumentációjában tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-155">Please refer to your distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="a8fbd-156">(Ubuntu). példa:</span><span class="sxs-lookup"><span data-stu-id="a8fbd-156">Example (Ubuntu):</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a><span data-ttu-id="a8fbd-157">TRIM/UNMAP támogatása</span><span class="sxs-lookup"><span data-stu-id="a8fbd-157">TRIM/UNMAP support</span></span>
<span data-ttu-id="a8fbd-158">Egyes Linux kernelek támogatja a vágás/UNMAP műveleteket elveti a nem használt blokkok a lemezen.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-158">Some Linux kernels support TRIM/UNMAP operations to discard unused blocks on the disk.</span></span> <span data-ttu-id="a8fbd-159">Ezek a műveletek elsősorban hasznosak standard tárolási tájékoztatja Azure törölt lapok már nem érvényesek, és is elvesznek.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-159">These operations are primarily useful in standard storage to inform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="a8fbd-160">Lapok elvetése költség mentheti, ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-160">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="a8fbd-161">Két módon vágást engedélyezése támogatja a Linux virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-161">There are two ways to enable TRIM support in your Linux VM.</span></span> <span data-ttu-id="a8fbd-162">A szokásos módon olvassa el a terjesztési az ajánlott módszer:</span><span class="sxs-lookup"><span data-stu-id="a8fbd-162">As usual, consult your distribution for the recommended approach:</span></span>

- <span data-ttu-id="a8fbd-163">Használja a `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:</span><span class="sxs-lookup"><span data-stu-id="a8fbd-163">Use the `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="a8fbd-164">Bizonyos esetekben a `discard` lehetőség is van a teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="a8fbd-164">In some cases the `discard` option may have performance implications.</span></span> <span data-ttu-id="a8fbd-165">Alternatív megoldásként futtathatja a `fstrim` manuálisan parancsot a parancssorból, vagy adja hozzá a crontab rendszeresen futtatásához:</span><span class="sxs-lookup"><span data-stu-id="a8fbd-165">Alternatively, you can run the `fstrim` command manually from the command line, or add it to your crontab to run regularly:</span></span>

    <span data-ttu-id="a8fbd-166">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="a8fbd-166">**Ubuntu**</span></span>

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    <span data-ttu-id="a8fbd-167">**RHEL vagy CentOS**</span><span class="sxs-lookup"><span data-stu-id="a8fbd-167">**RHEL/CentOS**</span></span>

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
