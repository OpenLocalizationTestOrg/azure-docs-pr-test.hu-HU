---
title: "Szoftveres RAID Linuxot futtató virtuális gépen konfigurálása |} Microsoft Docs"
description: "Útmutató mdadm Linux RAID konfigurálása az Azure-ban."
services: virtual-machines-linux
documentationcenter: na
author: rickstercdn
manager: timlt
editor: tysonn
tag: azure-service-management,azure-resource-manager
ms.assetid: f3cb2786-bda6-4d2c-9aaf-2db80f490feb
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: rclaus
ms.openlocfilehash: 12f540a700fbf85e579e8aadc9f6def039299ff7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="configure-software-raid-on-linux"></a><span data-ttu-id="035f9-103">Szoftveres RAID konfigurálása Linuxban</span><span class="sxs-lookup"><span data-stu-id="035f9-103">Configure Software RAID on Linux</span></span>
<span data-ttu-id="035f9-104">Egy általános forgatókönyv szoftveres RAID Linux virtuális gépeken használatára az Azure-ban van több csatolt adatlemezek egyetlen RAID eszközként is.</span><span class="sxs-lookup"><span data-stu-id="035f9-104">It's a common scenario to use software RAID on Linux virtual machines in Azure to present multiple attached data disks as a single RAID device.</span></span> <span data-ttu-id="035f9-105">Általában ez is használható a teljesítmény javítása, és nagyobb átviteli sebesség eléréséhez csak egyetlen lemez használatával engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="035f9-105">Typically this can be used to improve performance and allow for improved throughput compared to using just a single disk.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="035f9-106">Adatlemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="035f9-106">Attaching data disks</span></span>
<span data-ttu-id="035f9-107">Két vagy több üres adatlemezek RAID eszköz beállítása van szükség.</span><span class="sxs-lookup"><span data-stu-id="035f9-107">Two or more empty data disks are needed to configure a RAID device.</span></span>  <span data-ttu-id="035f9-108">Az elsődleges RAID eszköz létrehozása oka, hogy a lemez IO teljesítmény javítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="035f9-108">The primary reason for creating a RAID device is to improve performance of your disk IO.</span></span>  <span data-ttu-id="035f9-109">IO igényei szerint, ha szeretné, csatlakoztassa a szabványos tároló, a legfeljebb 500 IO/ps / lemez vagy a prémium szintű storage, legfeljebb 5000 IO/ps lemezenként tárolt lemezeket.</span><span class="sxs-lookup"><span data-stu-id="035f9-109">Based on your IO needs, you can choose to attach disks that are stored in our Standard Storage, with up to 500 IO/ps per disk or our Premium storage with up to 5000 IO/ps per disk.</span></span> <span data-ttu-id="035f9-110">Ez a cikk nem halad részletes kiépíteni, és a Linux virtuális gép adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="035f9-110">This article does not go into detail on how to provision and attach data disks to a Linux virtual machine.</span></span>  <span data-ttu-id="035f9-111">A Microsoft Azure cikke [lemezt csatlakoztatni](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) egy üres adatlemezt csatolni egy Linux virtuális gépet az Azure részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="035f9-111">See the Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how to attach an empty data disk to a Linux virtual machine on Azure.</span></span>

## <a name="install-the-mdadm-utility"></a><span data-ttu-id="035f9-112">Telepítse a mdadm segédprogram</span><span class="sxs-lookup"><span data-stu-id="035f9-112">Install the mdadm utility</span></span>
* <span data-ttu-id="035f9-113">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="035f9-113">**Ubuntu**</span></span>
```bash
sudo apt-get update
sudo apt-get install mdadm
```

* <span data-ttu-id="035f9-114">**CentOS & Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="035f9-114">**CentOS & Oracle Linux**</span></span>
```bash
sudo yum install mdadm
```

* <span data-ttu-id="035f9-115">**SLES és openSUSE**</span><span class="sxs-lookup"><span data-stu-id="035f9-115">**SLES and openSUSE**</span></span>
```bash  
zypper install mdadm
```

## <a name="create-the-disk-partitions"></a><span data-ttu-id="035f9-116">Hozzon létre a lemezpartíció</span><span class="sxs-lookup"><span data-stu-id="035f9-116">Create the disk partitions</span></span>
<span data-ttu-id="035f9-117">Ebben a példában /dev/sdc egyetlen lemezt a partíciót létrehozni azt.</span><span class="sxs-lookup"><span data-stu-id="035f9-117">In this example, we create a single disk partition on /dev/sdc.</span></span> <span data-ttu-id="035f9-118">Új tároló lemezpartíción /dev/sdc1 neve.</span><span class="sxs-lookup"><span data-stu-id="035f9-118">The new disk partition will be called /dev/sdc1.</span></span>

1. <span data-ttu-id="035f9-119">Start `fdisk` partíciók létrehozásának megkezdéséhez</span><span class="sxs-lookup"><span data-stu-id="035f9-119">Start `fdisk` to begin creating partitions</span></span>

    ```bash
    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa34cb70c.
    Changes will remain in memory only, until you decide to write them.
    After that, of course, the previous content won't be recoverable.

    WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                    switch off the mode (command 'c') and change display units to
                    sectors (command 'u').
    ```

2. <span data-ttu-id="035f9-120">Nyomja meg az "n" létrehozásához a parancssorban a  **n** új partíció:</span><span class="sxs-lookup"><span data-stu-id="035f9-120">Press 'n' at the prompt to create a **n**ew partition:</span></span>

    ```bash
    Command (m for help): n
    ```

3. <span data-ttu-id="035f9-121">Nyomja meg az "p" létrehozásához egy **p**lsődleges partíció:</span><span class="sxs-lookup"><span data-stu-id="035f9-121">Next, press 'p' to create a **p**rimary partition:</span></span>

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. <span data-ttu-id="035f9-122">Nyomja meg az "1" partíciószám 1 kiválasztásához:</span><span class="sxs-lookup"><span data-stu-id="035f9-122">Press '1' to select partition number 1:</span></span>

    ```bash
    Partition number (1-4): 1
    ```

5. <span data-ttu-id="035f9-123">Válassza ki az új partíciót, vagy nyomja meg a kiindulási pont `<enter>` elfogadja az alapértelmezett helyezhető el a partíció a szabad lemezterület a meghajtón elején:</span><span class="sxs-lookup"><span data-stu-id="035f9-123">Select the starting point of the new partition, or press `<enter>` to accept the default to place the partition at the beginning of the free space on the drive:</span></span>

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. <span data-ttu-id="035f9-124">Válassza ki a partíció méretét, írja be például a "+10G" 10 gigabájt partíció létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="035f9-124">Select the size of the partition, for example type '+10G' to create a 10 gigabyte partition.</span></span> <span data-ttu-id="035f9-125">Vagy nyomja le az ENTER `<enter>` létrehoz egy olyan partíciót a teljes meghajtót is:</span><span class="sxs-lookup"><span data-stu-id="035f9-125">Or, press `<enter>` create a single partition that spans the entire drive:</span></span>

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. <span data-ttu-id="035f9-126">Ezután módosítsa az Azonosítót és **t**típusa a partíció a "83" alapértelmezett-azonosító (Linux) azonosító "fd" (Linux raid automatikus):</span><span class="sxs-lookup"><span data-stu-id="035f9-126">Next, change the ID and **t**ype of the partition from the default ID '83' (Linux) to ID 'fd' (Linux raid auto):</span></span>

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L to list codes): fd
    ```

8. <span data-ttu-id="035f9-127">Végezetül a partíciós táblán írni a meghajtót, és zárja be a fdisk:</span><span class="sxs-lookup"><span data-stu-id="035f9-127">Finally, write the partition table to the drive and exit fdisk:</span></span>

    ```bash   
    Command (m for help): w
    The partition table has been altered!
    ```

## <a name="create-the-raid-array"></a><span data-ttu-id="035f9-128">A RAID tömb létrehozása</span><span class="sxs-lookup"><span data-stu-id="035f9-128">Create the RAID array</span></span>
1. <span data-ttu-id="035f9-129">A következő példa fog "paritásos" (RAID 0. szint) három lemezeken található három különböző adatokhoz (sdc1, sdd1, sde1).</span><span class="sxs-lookup"><span data-stu-id="035f9-129">The following example will "stripe" (RAID level 0) three partitions located on three separate data disks (sdc1, sdd1, sde1).</span></span>  <span data-ttu-id="035f9-130">A parancs új RAID eszköz futtatása után **/dev/md127** jön létre.</span><span class="sxs-lookup"><span data-stu-id="035f9-130">After running this command a new RAID device called **/dev/md127** is created.</span></span> <span data-ttu-id="035f9-131">Vegye figyelembe azt is, hogy ha ezeket az adatokat lemezek azt korábban egy másik inaktív RAID tömb része lehet kell hozzáadnia a `--force` paramétert a `mdadm` parancs:</span><span class="sxs-lookup"><span data-stu-id="035f9-131">Also note that if these data disks we previously part of another defunct RAID array it may be necessary to add the `--force` parameter to the `mdadm` command:</span></span>

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. <span data-ttu-id="035f9-132">A fájlrendszer létrehozása az új RAID-eszközön</span><span class="sxs-lookup"><span data-stu-id="035f9-132">Create the file system on the new RAID device</span></span>
   
    <span data-ttu-id="035f9-133">a.</span><span class="sxs-lookup"><span data-stu-id="035f9-133">a.</span></span> <span data-ttu-id="035f9-134">**CentOS, Oracle Linux, SLES 12, openSUSE és Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="035f9-134">**CentOS, Oracle Linux, SLES 12, openSUSE, and Ubuntu**</span></span>

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    <span data-ttu-id="035f9-135">b.</span><span class="sxs-lookup"><span data-stu-id="035f9-135">b.</span></span> <span data-ttu-id="035f9-136">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="035f9-136">**SLES 11**</span></span>

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    <span data-ttu-id="035f9-137">c.</span><span class="sxs-lookup"><span data-stu-id="035f9-137">c.</span></span> <span data-ttu-id="035f9-138">**SLES 11** - boot.md engedélyezése és mdadm.conf létrehozása</span><span class="sxs-lookup"><span data-stu-id="035f9-138">**SLES 11** - enable boot.md and create mdadm.conf</span></span>

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > <span data-ttu-id="035f9-139">A SUSE rendszereken módosítások végrehajtását követően újraindításra lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="035f9-139">A reboot may be required after making these changes on SUSE systems.</span></span> <span data-ttu-id="035f9-140">Ez a lépés *nem* SLES 12 szükséges.</span><span class="sxs-lookup"><span data-stu-id="035f9-140">This step is *not* required on SLES 12.</span></span>
   > 
   > 

## <a name="add-the-new-file-system-to-etcfstab"></a><span data-ttu-id="035f9-141">Az új fájlrendszer /etc/fstab hozzáadása</span><span class="sxs-lookup"><span data-stu-id="035f9-141">Add the new file system to /etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="035f9-142">Helytelen a az /etc/fstab fájl szerkesztése meghiúsulását eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="035f9-142">Improperly editing the /etc/fstab file could result in an unbootable system.</span></span> <span data-ttu-id="035f9-143">Ha nem ismeri, tekintse meg a telepítési dokumentációban talál információkat arról a fájl megfelelően szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="035f9-143">If unsure, refer to the distribution's documentation for information on how to properly edit this file.</span></span> <span data-ttu-id="035f9-144">Ajánlott továbbá, hogy létrejött-e a fájl biztonsági másolata /etc/fstab szerkesztése előtt.</span><span class="sxs-lookup"><span data-stu-id="035f9-144">It is also recommended that a backup of the /etc/fstab file is created before editing.</span></span>

1. <span data-ttu-id="035f9-145">Létrehozhat például az új fájlrendszer, a megfelelő csatlakozási pont:</span><span class="sxs-lookup"><span data-stu-id="035f9-145">Create the desired mount point for your new file system, for example:</span></span>

    ```bash
    sudo mkdir /data
    ```
2. <span data-ttu-id="035f9-146">/Etc/fstab, szerkesztésekor a **UUID** használatával hivatkozik, az eszköz neve helyett a fájlrendszerben.</span><span class="sxs-lookup"><span data-stu-id="035f9-146">When editing /etc/fstab, the **UUID** should be used to reference the file system rather than the device name.</span></span>  <span data-ttu-id="035f9-147">Használja a `blkid` segítségével határozza meg, az új fájlrendszer UUID:</span><span class="sxs-lookup"><span data-stu-id="035f9-147">Use the `blkid` utility to determine the UUID for the new file system:</span></span>

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. <span data-ttu-id="035f9-148">Nyissa meg a /etc/fstab egy szövegszerkesztőben, és adja hozzá a bejegyzést az új fájlrendszer, például:</span><span class="sxs-lookup"><span data-stu-id="035f9-148">Open /etc/fstab in a text editor and add an entry for the new file system, for example:</span></span>

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    <span data-ttu-id="035f9-149">Vagy a **SLES 11**:</span><span class="sxs-lookup"><span data-stu-id="035f9-149">Or on **SLES 11**:</span></span>

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    <span data-ttu-id="035f9-150">Ezt követően mentse, és zárja be a /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="035f9-150">Then, save and close /etc/fstab.</span></span>

4. <span data-ttu-id="035f9-151">Tesztelje, hogy helyesek-e az/etc/fstab bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="035f9-151">Test that the /etc/fstab entry is correct:</span></span>

    ```bash  
    sudo mount -a
    ```

    <span data-ttu-id="035f9-152">Ha ezt a parancsot egy hibaüzenet, ellenőrizze a szintaxist a /etc/fstab fájlban.</span><span class="sxs-lookup"><span data-stu-id="035f9-152">If this command results in an error message, please check the syntax in the /etc/fstab file.</span></span>
   
    <span data-ttu-id="035f9-153">Ezután futtassa a `mount` parancsot annak biztosítására, hogy a fájlrendszer van csatlakoztatva:</span><span class="sxs-lookup"><span data-stu-id="035f9-153">Next run the `mount` command to ensure the file system is mounted:</span></span>

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="035f9-154">(Választható) FailSafe rendszerindító paraméterek</span><span class="sxs-lookup"><span data-stu-id="035f9-154">(Optional) Failsafe Boot Parameters</span></span>
   
    <span data-ttu-id="035f9-155">**fstab konfiguráció**</span><span class="sxs-lookup"><span data-stu-id="035f9-155">**fstab configuration**</span></span>
   
    <span data-ttu-id="035f9-156">Terjesztések valamelyikét tartalmazza a `nobootwait` vagy `nofail` csatlakoztatási paramétereket, előfordulhat, hogy hozzá lehet adni a/etc/fstab fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="035f9-156">Many distributions include either the `nobootwait` or `nofail` mount parameters that may be added to the /etc/fstab file.</span></span> <span data-ttu-id="035f9-157">Ezek a paraméterek hibák engedélyezése, ha egy adott fájlrendszer csatlakoztatása és a Linux rendszer akkor is, ha az nem csatolható fel a RAID fájlrendszer megfelelően elindulni.</span><span class="sxs-lookup"><span data-stu-id="035f9-157">These parameters allow for failures when mounting a particular file system and allow the Linux system to continue to boot even if it is unable to properly mount the RAID file system.</span></span> <span data-ttu-id="035f9-158">Tekintse meg a terjesztési dokumentációjában talál további információt ezekről a paraméterekről.</span><span class="sxs-lookup"><span data-stu-id="035f9-158">Refer to your distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="035f9-159">(Ubuntu). példa:</span><span class="sxs-lookup"><span data-stu-id="035f9-159">Example (Ubuntu):</span></span>

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    <span data-ttu-id="035f9-160">**Linux rendszerindító paraméterek**</span><span class="sxs-lookup"><span data-stu-id="035f9-160">**Linux boot parameters**</span></span>
   
    <span data-ttu-id="035f9-161">A fenti paraméterek, a kernel paraméter mellett "`bootdegraded=true`" megtehetik, hogy a rendszer akkor is, ha a RAID érzékelt sérült vagy csökkent, a példaként, ha egy adatmeghajtó véletlenül eltávolítják a virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="035f9-161">In addition to the above parameters, the kernel parameter "`bootdegraded=true`" can allow the system to boot even if the RAID is perceived as damaged or degraded, for example if a data drive is inadvertently removed from the virtual machine.</span></span> <span data-ttu-id="035f9-162">Alapértelmezés szerint ez is vezethet a nem rendszerindító rendszer.</span><span class="sxs-lookup"><span data-stu-id="035f9-162">By default this could also result in a non-bootable system.</span></span>
   
    <span data-ttu-id="035f9-163">Tekintse meg a terjesztési dokumentációja megfelelően a kernel paraméterek szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="035f9-163">Please refer to your distribution's documentation on how to properly edit kernel parameters.</span></span> <span data-ttu-id="035f9-164">Például terjesztések (CentOS, Oracle Linux SLES 11.) a következő paraméterek vehetők manuálisan a "`/boot/grub/menu.lst`" fájl.</span><span class="sxs-lookup"><span data-stu-id="035f9-164">For example, in many distributions (CentOS, Oracle Linux, SLES 11) these parameters may be added manually to the "`/boot/grub/menu.lst`" file.</span></span>  <span data-ttu-id="035f9-165">Ubuntu az ezzel a paraméterrel adhatók hozzá a `GRUB_CMDLINE_LINUX_DEFAULT` változó a "/ etc/alapértelmezett/lárvajárat".</span><span class="sxs-lookup"><span data-stu-id="035f9-165">On Ubuntu this parameter can be added to the `GRUB_CMDLINE_LINUX_DEFAULT` variable on "/etc/default/grub".</span></span>


## <a name="trimunmap-support"></a><span data-ttu-id="035f9-166">TRIM/UNMAP támogatása</span><span class="sxs-lookup"><span data-stu-id="035f9-166">TRIM/UNMAP support</span></span>
<span data-ttu-id="035f9-167">Egyes Linux kernelek támogatja a vágás/UNMAP műveleteket elveti a nem használt blokkok a lemezen.</span><span class="sxs-lookup"><span data-stu-id="035f9-167">Some Linux kernels support TRIM/UNMAP operations to discard unused blocks on the disk.</span></span> <span data-ttu-id="035f9-168">Ezek a műveletek elsősorban hasznosak standard tárolási tájékoztatja Azure törölt lapok már nem érvényesek, és is elvesznek.</span><span class="sxs-lookup"><span data-stu-id="035f9-168">These operations are primarily useful in standard storage to inform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="035f9-169">Lapok elvetése költség mentheti, ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="035f9-169">Discarding pages can save cost if you create large files and then delete them.</span></span>

> [!NOTE]
> <span data-ttu-id="035f9-170">RAID nem lehetséges, hogy ki elvetési parancsok, ha a tömb az adatrészlet méretének értéke kisebb, mint az alapértelmezett (512KB).</span><span class="sxs-lookup"><span data-stu-id="035f9-170">RAID may not issue discard commands if the chunk size for the array is set to less than the default (512KB).</span></span> <span data-ttu-id="035f9-171">Ez azért, mert a gazdagépen unmap granularitása is 512KB.</span><span class="sxs-lookup"><span data-stu-id="035f9-171">This is because the unmap granularity on the Host is also 512KB.</span></span> <span data-ttu-id="035f9-172">Ha a tömb adatrészlet méretének keresztül mdadm által módosított `--chunk=` paramétert, majd a vágás/megfeleltetésének törlése kérelmek előfordulhat, hogy figyelmen kívül lesz hagyva a rendszermag által.</span><span class="sxs-lookup"><span data-stu-id="035f9-172">If you modified the array's chunk size via mdadm's `--chunk=` parameter, then TRIM/unmap requests may be ignored by the kernel.</span></span>

<span data-ttu-id="035f9-173">Két módon vágást engedélyezése támogatja a Linux virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="035f9-173">There are two ways to enable TRIM support in your Linux VM.</span></span> <span data-ttu-id="035f9-174">A szokásos módon olvassa el a terjesztési az ajánlott módszer:</span><span class="sxs-lookup"><span data-stu-id="035f9-174">As usual, consult your distribution for the recommended approach:</span></span>

- <span data-ttu-id="035f9-175">Használja a `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:</span><span class="sxs-lookup"><span data-stu-id="035f9-175">Use the `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="035f9-176">Bizonyos esetekben a `discard` lehetőség is van a teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="035f9-176">In some cases the `discard` option may have performance implications.</span></span> <span data-ttu-id="035f9-177">Alternatív megoldásként futtathatja a `fstrim` manuálisan parancsot a parancssorból, vagy adja hozzá a crontab rendszeresen futtatásához:</span><span class="sxs-lookup"><span data-stu-id="035f9-177">Alternatively, you can run the `fstrim` command manually from the command line, or add it to your crontab to run regularly:</span></span>

    <span data-ttu-id="035f9-178">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="035f9-178">**Ubuntu**</span></span>

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    <span data-ttu-id="035f9-179">**RHEL vagy CentOS**</span><span class="sxs-lookup"><span data-stu-id="035f9-179">**RHEL/CentOS**</span></span>
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
