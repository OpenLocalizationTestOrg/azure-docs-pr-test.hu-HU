---
title: "aaaConfigure szoftveres RAID Linuxot futtató virtuális gépen |} Microsoft Docs"
description: Ismerje meg, hogyan toouse mdadm tooconfigure RAID-Linux az Azure-ban.
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
ms.openlocfilehash: f06e2679d953faf88ffee9991226cdb3cc1cbdb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-software-raid-on-linux"></a><span data-ttu-id="53546-103">Szoftveres RAID konfigurálása Linuxban</span><span class="sxs-lookup"><span data-stu-id="53546-103">Configure Software RAID on Linux</span></span>
<span data-ttu-id="53546-104">Egy általános forgatókönyv-toouse szoftveres RAID Linux virtuális gépek Azure toopresent több csatolt adatok egyetlen RAID eszközként lemezterület.</span><span class="sxs-lookup"><span data-stu-id="53546-104">It's a common scenario toouse software RAID on Linux virtual machines in Azure toopresent multiple attached data disks as a single RAID device.</span></span> <span data-ttu-id="53546-105">Ez általában használt tooimprove teljesítmény kell és átviteli képest toousing csak egyetlen lemezt a továbbfejlesztett engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="53546-105">Typically this can be used tooimprove performance and allow for improved throughput compared toousing just a single disk.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="53546-106">Adatlemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="53546-106">Attaching data disks</span></span>
<span data-ttu-id="53546-107">Két vagy több üres adatlemezek szükséges tooconfigure RAID eszköz.</span><span class="sxs-lookup"><span data-stu-id="53546-107">Two or more empty data disks are needed tooconfigure a RAID device.</span></span>  <span data-ttu-id="53546-108">hello elsődleges RAID eszköz létrehozása oka, hogy a lemez IO tooimprove teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="53546-108">hello primary reason for creating a RAID device is tooimprove performance of your disk IO.</span></span>  <span data-ttu-id="53546-109">IO igényei szerint, kiválaszthatja a tooattach tárolódnak a standard szintű tárolást, a másolatot IO too500 ps / lemez vagy a prémium szintű storage IO too5000 ps lemezenként fel a lemezeket.</span><span class="sxs-lookup"><span data-stu-id="53546-109">Based on your IO needs, you can choose tooattach disks that are stored in our Standard Storage, with up too500 IO/ps per disk or our Premium storage with up too5000 IO/ps per disk.</span></span> <span data-ttu-id="53546-110">Ez a cikk nem halad részletes hogyan tooprovision, és csatlakoztassa az adatok lemezek tooa Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="53546-110">This article does not go into detail on how tooprovision and attach data disks tooa Linux virtual machine.</span></span>  <span data-ttu-id="53546-111">Lásd: hello Microsoft Azure cikk [lemezt csatlakoztatni](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) hogyan tooattach egy üres adatok lemezre tooa Linux virtuális gépet az Azure részletes útmutatást.</span><span class="sxs-lookup"><span data-stu-id="53546-111">See hello Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how tooattach an empty data disk tooa Linux virtual machine on Azure.</span></span>

## <a name="install-hello-mdadm-utility"></a><span data-ttu-id="53546-112">Hello mdadm segédprogram</span><span class="sxs-lookup"><span data-stu-id="53546-112">Install hello mdadm utility</span></span>
* <span data-ttu-id="53546-113">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="53546-113">**Ubuntu**</span></span>
```bash
sudo apt-get update
sudo apt-get install mdadm
```

* <span data-ttu-id="53546-114">**CentOS & Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="53546-114">**CentOS & Oracle Linux**</span></span>
```bash
sudo yum install mdadm
```

* <span data-ttu-id="53546-115">**SLES és openSUSE**</span><span class="sxs-lookup"><span data-stu-id="53546-115">**SLES and openSUSE**</span></span>
```bash  
zypper install mdadm
```

## <a name="create-hello-disk-partitions"></a><span data-ttu-id="53546-116">Hozzon létre hello lemezpartíció</span><span class="sxs-lookup"><span data-stu-id="53546-116">Create hello disk partitions</span></span>
<span data-ttu-id="53546-117">Ebben a példában /dev/sdc egyetlen lemezt a partíciót létrehozni azt.</span><span class="sxs-lookup"><span data-stu-id="53546-117">In this example, we create a single disk partition on /dev/sdc.</span></span> <span data-ttu-id="53546-118">hello új lemezpartíciót /dev/sdc1 neve.</span><span class="sxs-lookup"><span data-stu-id="53546-118">hello new disk partition will be called /dev/sdc1.</span></span>

1. <span data-ttu-id="53546-119">Start `fdisk` toobegin partíciók létrehozása</span><span class="sxs-lookup"><span data-stu-id="53546-119">Start `fdisk` toobegin creating partitions</span></span>

    ```bash
    sudo fdisk /dev/sdc
    Device contains neither a valid DOS partition table, nor Sun, SGI or OSF disklabel
    Building a new DOS disklabel with disk identifier 0xa34cb70c.
    Changes will remain in memory only, until you decide toowrite them.
    After that, of course, hello previous content won't be recoverable.

    WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
                    switch off hello mode (command 'c') and change display units to
                    sectors (command 'u').
    ```

2. <span data-ttu-id="53546-120">Nyomja meg az "n" hello Rákérdezés toocreate, egy  **n** új partíció:</span><span class="sxs-lookup"><span data-stu-id="53546-120">Press 'n' at hello prompt toocreate a **n**ew partition:</span></span>

    ```bash
    Command (m for help): n
    ```

3. <span data-ttu-id="53546-121">Nyomja meg az "p" toocreate egy **p**lsődleges partíció:</span><span class="sxs-lookup"><span data-stu-id="53546-121">Next, press 'p' toocreate a **p**rimary partition:</span></span>

    ```bash 
    Command action
            e   extended
            p   primary partition (1-4)
    ```

4. <span data-ttu-id="53546-122">Nyomja meg az "1" tooselect partíciószám 1:</span><span class="sxs-lookup"><span data-stu-id="53546-122">Press '1' tooselect partition number 1:</span></span>

    ```bash
    Partition number (1-4): 1
    ```

5. <span data-ttu-id="53546-123">Válassza ki a kiindulási pont hello új partíciót, vagy nyomja le az hello `<enter>` tooaccept hello alapértelmezett tooplace hello partíció hello elején szabad lemezterület hello hello meghajtón:</span><span class="sxs-lookup"><span data-stu-id="53546-123">Select hello starting point of hello new partition, or press `<enter>` tooaccept hello default tooplace hello partition at hello beginning of hello free space on hello drive:</span></span>

    ```bash   
    First cylinder (1-1305, default 1):
    Using default value 1
    ```

6. <span data-ttu-id="53546-124">Válassza ki a hello partíció, például "+10G" típusú toocreate egy 10 GB partíció hello méretét.</span><span class="sxs-lookup"><span data-stu-id="53546-124">Select hello size of hello partition, for example type '+10G' toocreate a 10 gigabyte partition.</span></span> <span data-ttu-id="53546-125">Vagy nyomja le az ENTER `<enter>` hello teljes meghajtó egyetlen partícióra létrehozása:</span><span class="sxs-lookup"><span data-stu-id="53546-125">Or, press `<enter>` create a single partition that spans hello entire drive:</span></span>

    ```bash   
    Last cylinder, +cylinders or +size{K,M,G} (1-1305, default 1305): 
    Using default value 1305
    ```

7. <span data-ttu-id="53546-126">Ezután módosítsa az Azonosítót hello és **t**hello partíció hello alapértelmezett típus azonosító "83" (Linux) tooID "fd" (Linux raid automatikus):</span><span class="sxs-lookup"><span data-stu-id="53546-126">Next, change hello ID and **t**ype of hello partition from hello default ID '83' (Linux) tooID 'fd' (Linux raid auto):</span></span>

    ```bash  
    Command (m for help): t
    Selected partition 1
    Hex code (type L toolist codes): fd
    ```

8. <span data-ttu-id="53546-127">Végezetül hello partíció tábla toohello meghajtó írása, és zárja be a fdisk:</span><span class="sxs-lookup"><span data-stu-id="53546-127">Finally, write hello partition table toohello drive and exit fdisk:</span></span>

    ```bash   
    Command (m for help): w
    hello partition table has been altered!
    ```

## <a name="create-hello-raid-array"></a><span data-ttu-id="53546-128">Hello RAID tömb létrehozása</span><span class="sxs-lookup"><span data-stu-id="53546-128">Create hello RAID array</span></span>
1. <span data-ttu-id="53546-129">a következő példa lesz "paritásos" (RAID 0. szint) három lemezeken található három különböző adatokhoz (sdc1, sdd1, sde1) hello.</span><span class="sxs-lookup"><span data-stu-id="53546-129">hello following example will "stripe" (RAID level 0) three partitions located on three separate data disks (sdc1, sdd1, sde1).</span></span>  <span data-ttu-id="53546-130">A parancs új RAID eszköz futtatása után **/dev/md127** jön létre.</span><span class="sxs-lookup"><span data-stu-id="53546-130">After running this command a new RAID device called **/dev/md127** is created.</span></span> <span data-ttu-id="53546-131">Vegye figyelembe azt is, hogy ha ezeket az adatokat lemezek azt korábban egy másik inaktív RAID tömb része lehet szükséges tooadd hello `--force` paraméter toohello `mdadm` parancs:</span><span class="sxs-lookup"><span data-stu-id="53546-131">Also note that if these data disks we previously part of another defunct RAID array it may be necessary tooadd hello `--force` parameter toohello `mdadm` command:</span></span>

    ```bash  
    sudo mdadm --create /dev/md127 --level 0 --raid-devices 3 \
        /dev/sdc1 /dev/sdd1 /dev/sde1
    ```

2. <span data-ttu-id="53546-132">Hello fájlrendszer hello új RAID-eszköz létrehozása</span><span class="sxs-lookup"><span data-stu-id="53546-132">Create hello file system on hello new RAID device</span></span>
   
    <span data-ttu-id="53546-133">a.</span><span class="sxs-lookup"><span data-stu-id="53546-133">a.</span></span> <span data-ttu-id="53546-134">**CentOS, Oracle Linux, SLES 12, openSUSE és Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="53546-134">**CentOS, Oracle Linux, SLES 12, openSUSE, and Ubuntu**</span></span>

    ```bash   
    sudo mkfs -t ext4 /dev/md127
    ```
   
    <span data-ttu-id="53546-135">b.</span><span class="sxs-lookup"><span data-stu-id="53546-135">b.</span></span> <span data-ttu-id="53546-136">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="53546-136">**SLES 11**</span></span>

    ```bash
    sudo mkfs -t ext3 /dev/md127
    ```
   
    <span data-ttu-id="53546-137">c.</span><span class="sxs-lookup"><span data-stu-id="53546-137">c.</span></span> <span data-ttu-id="53546-138">**SLES 11** - boot.md engedélyezése és mdadm.conf létrehozása</span><span class="sxs-lookup"><span data-stu-id="53546-138">**SLES 11** - enable boot.md and create mdadm.conf</span></span>

    ```bash
    sudo -i chkconfig --add boot.md
    sudo echo 'DEVICE /dev/sd*[0-9]' >> /etc/mdadm.conf
    ```
   
   > [!NOTE]
   > <span data-ttu-id="53546-139">A SUSE rendszereken módosítások végrehajtását követően újraindításra lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="53546-139">A reboot may be required after making these changes on SUSE systems.</span></span> <span data-ttu-id="53546-140">Ez a lépés *nem* SLES 12 szükséges.</span><span class="sxs-lookup"><span data-stu-id="53546-140">This step is *not* required on SLES 12.</span></span>
   > 
   > 

## <a name="add-hello-new-file-system-tooetcfstab"></a><span data-ttu-id="53546-141">Hello új fájl rendszer túl/etc/fstab hozzáadása</span><span class="sxs-lookup"><span data-stu-id="53546-141">Add hello new file system too/etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="53546-142">Helytelen a hello /etc/fstab fájl szerkesztése meghiúsulását eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="53546-142">Improperly editing hello /etc/fstab file could result in an unbootable system.</span></span> <span data-ttu-id="53546-143">Ha nem ismeri, tekintse meg a toohello terjesztési dokumentációját olvashat, hogyan tooproperly szerkesztheti a fájlt.</span><span class="sxs-lookup"><span data-stu-id="53546-143">If unsure, refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="53546-144">Ajánlott továbbá, hogy létrejött-e a fájl biztonsági másolata hello /etc/fstab szerkesztése előtt.</span><span class="sxs-lookup"><span data-stu-id="53546-144">It is also recommended that a backup of hello /etc/fstab file is created before editing.</span></span>

1. <span data-ttu-id="53546-145">Hozzon létre szükséges hello csatlakoztatási pontot az új fájlrendszer, például:</span><span class="sxs-lookup"><span data-stu-id="53546-145">Create hello desired mount point for your new file system, for example:</span></span>

    ```bash
    sudo mkdir /data
    ```
2. <span data-ttu-id="53546-146">/Etc/fstab szerkesztésekor hello **UUID** használt tooreference hello fájl hello helyett a rendszer eszköz névnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="53546-146">When editing /etc/fstab, hello **UUID** should be used tooreference hello file system rather than hello device name.</span></span>  <span data-ttu-id="53546-147">Használjon hello `blkid` segédprogram toodetermine hello UUID hello új fájlrendszer:</span><span class="sxs-lookup"><span data-stu-id="53546-147">Use hello `blkid` utility toodetermine hello UUID for hello new file system:</span></span>

    ```bash   
    sudo /sbin/blkid
    ...........
    /dev/md127: UUID="aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee" TYPE="ext4"
    ```

3. <span data-ttu-id="53546-148">Nyissa meg a /etc/fstab egy szövegszerkesztőben, és adja hozzá egy bejegyzést a hello új fájlrendszer, például:</span><span class="sxs-lookup"><span data-stu-id="53546-148">Open /etc/fstab in a text editor and add an entry for hello new file system, for example:</span></span>

    ```bash   
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults  0  2
    ```
   
    <span data-ttu-id="53546-149">Vagy a **SLES 11**:</span><span class="sxs-lookup"><span data-stu-id="53546-149">Or on **SLES 11**:</span></span>

    ```bash
    /dev/disk/by-uuid/aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext3  defaults  0  2
    ```
   
    <span data-ttu-id="53546-150">Ezt követően mentse, és zárja be a /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="53546-150">Then, save and close /etc/fstab.</span></span>

4. <span data-ttu-id="53546-151">Tesztelje, hogy hello etc/fstab bejegyzés helyességét.</span><span class="sxs-lookup"><span data-stu-id="53546-151">Test that hello /etc/fstab entry is correct:</span></span>

    ```bash  
    sudo mount -a
    ```

    <span data-ttu-id="53546-152">Ha ezt a parancsot egy hibaüzenet, ellenőrizze a hello szintaxis hello /etc/fstab fájlban.</span><span class="sxs-lookup"><span data-stu-id="53546-152">If this command results in an error message, please check hello syntax in hello /etc/fstab file.</span></span>
   
    <span data-ttu-id="53546-153">Ezután futtassa az hello `mount` parancs tooensure hello fájlrendszer van csatlakoztatva:</span><span class="sxs-lookup"><span data-stu-id="53546-153">Next run hello `mount` command tooensure hello file system is mounted:</span></span>

    ```bash   
    mount
    .................
    /dev/md127 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="53546-154">(Választható) FailSafe rendszerindító paraméterek</span><span class="sxs-lookup"><span data-stu-id="53546-154">(Optional) Failsafe Boot Parameters</span></span>
   
    <span data-ttu-id="53546-155">**fstab konfiguráció**</span><span class="sxs-lookup"><span data-stu-id="53546-155">**fstab configuration**</span></span>
   
    <span data-ttu-id="53546-156">Terjesztések tartalmaz vagy hello `nobootwait` vagy `nofail` toohello/etc/fstab fájl hozzáadott paraméterek csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="53546-156">Many distributions include either hello `nobootwait` or `nofail` mount parameters that may be added toohello /etc/fstab file.</span></span> <span data-ttu-id="53546-157">Ezek a paraméterek hibák engedélyezése, ha egy adott fájlrendszer csatlakoztatása és hello Linux rendszer toocontinue tooboot engedélyezi, akkor is, ha nem tooproperly csatlakoztatási hello RAID fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="53546-157">These parameters allow for failures when mounting a particular file system and allow hello Linux system toocontinue tooboot even if it is unable tooproperly mount hello RAID file system.</span></span> <span data-ttu-id="53546-158">További információ ezekről a paraméterekről tekintse meg a tooyour terjesztési dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="53546-158">Refer tooyour distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="53546-159">(Ubuntu). példa:</span><span class="sxs-lookup"><span data-stu-id="53546-159">Example (Ubuntu):</span></span>

    ```bash  
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,nobootwait  0  2
    ```   

    <span data-ttu-id="53546-160">**Linux rendszerindító paraméterek**</span><span class="sxs-lookup"><span data-stu-id="53546-160">**Linux boot parameters**</span></span>
   
    <span data-ttu-id="53546-161">A fenti paraméterek toohello hozzáadását, hello kernel paraméter "`bootdegraded=true`" hello rendszer tooboot lehetővé teheti, még akkor is, ha hello RAID kezeli, sérült vagy alacsonyabb, például ha egy adatmeghajtó véletlenül eltávolítják hello virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="53546-161">In addition toohello above parameters, hello kernel parameter "`bootdegraded=true`" can allow hello system tooboot even if hello RAID is perceived as damaged or degraded, for example if a data drive is inadvertently removed from hello virtual machine.</span></span> <span data-ttu-id="53546-162">Alapértelmezés szerint ez is vezethet a nem rendszerindító rendszer.</span><span class="sxs-lookup"><span data-stu-id="53546-162">By default this could also result in a non-bootable system.</span></span>
   
    <span data-ttu-id="53546-163">Tekintse meg az tooyour terjesztési dokumentációja hogyan tooproperly kernel paraméterek szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="53546-163">Please refer tooyour distribution's documentation on how tooproperly edit kernel parameters.</span></span> <span data-ttu-id="53546-164">Például terjesztések (CentOS, Oracle Linux SLES 11.) a következő paraméterek vehetők manuálisan toohello "`/boot/grub/menu.lst`" fájl.</span><span class="sxs-lookup"><span data-stu-id="53546-164">For example, in many distributions (CentOS, Oracle Linux, SLES 11) these parameters may be added manually toohello "`/boot/grub/menu.lst`" file.</span></span>  <span data-ttu-id="53546-165">Ubuntu ennek a paraméternek is hozzáadható toohello `GRUB_CMDLINE_LINUX_DEFAULT` változó a "/ etc/alapértelmezett/lárvajárat".</span><span class="sxs-lookup"><span data-stu-id="53546-165">On Ubuntu this parameter can be added toohello `GRUB_CMDLINE_LINUX_DEFAULT` variable on "/etc/default/grub".</span></span>


## <a name="trimunmap-support"></a><span data-ttu-id="53546-166">TRIM/UNMAP támogatása</span><span class="sxs-lookup"><span data-stu-id="53546-166">TRIM/UNMAP support</span></span>
<span data-ttu-id="53546-167">Egyes Linux kernelek vágást/UNMAP műveletek toodiscard támogatja a nem használt blokkok hello lemezen.</span><span class="sxs-lookup"><span data-stu-id="53546-167">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="53546-168">Ezek a műveletek elsősorban hasznosak standard tárolási tooinform Azure törölt lapok már nem érvényes, és is elvesznek.</span><span class="sxs-lookup"><span data-stu-id="53546-168">These operations are primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="53546-169">Lapok elvetése költség mentheti, ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="53546-169">Discarding pages can save cost if you create large files and then delete them.</span></span>

> [!NOTE]
> <span data-ttu-id="53546-170">RAID nem lehetséges, hogy ki elvetési parancsok, ha hello adatrészlet méretének hello tömb tooless hello alapértelmezett (512KB) van beállítva.</span><span class="sxs-lookup"><span data-stu-id="53546-170">RAID may not issue discard commands if hello chunk size for hello array is set tooless than hello default (512KB).</span></span> <span data-ttu-id="53546-171">Ennek az az oka hello megfeleltetésének törlése a gazdagép hello granularitási egyben 512KB.</span><span class="sxs-lookup"><span data-stu-id="53546-171">This is because hello unmap granularity on hello Host is also 512KB.</span></span> <span data-ttu-id="53546-172">Ha módosította-e hello tömb adatrészlet méretének keresztül mdadm tartozó `--chunk=` paramétert, majd a vágás/megfeleltetésének törlése kérelmek előfordulhat, hogy figyelmen kívül lesz hagyva hello kernel.</span><span class="sxs-lookup"><span data-stu-id="53546-172">If you modified hello array's chunk size via mdadm's `--chunk=` parameter, then TRIM/unmap requests may be ignored by hello kernel.</span></span>

<span data-ttu-id="53546-173">Két módon tooenable vágást támogatja a Linux virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="53546-173">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="53546-174">A szokásos módon olvassa el a terjesztési hello ajánlott megközelítést alkalmazva:</span><span class="sxs-lookup"><span data-stu-id="53546-174">As usual, consult your distribution for hello recommended approach:</span></span>

- <span data-ttu-id="53546-175">Használjon hello `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:</span><span class="sxs-lookup"><span data-stu-id="53546-175">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash
    UUID=aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="53546-176">Az egyes esetekben hello `discard` lehetőség is van a teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="53546-176">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="53546-177">Alternatív megoldásként futtathatja a hello `fstrim` manuálisan parancsot a parancssorból hello, vagy felveheti Ön is tooyour crontab toorun rendszeresen:</span><span class="sxs-lookup"><span data-stu-id="53546-177">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>

    <span data-ttu-id="53546-178">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="53546-178">**Ubuntu**</span></span>

    ```bash
    # sudo apt-get install util-linux
    # sudo fstrim /data
    ```

    <span data-ttu-id="53546-179">**RHEL vagy CentOS**</span><span class="sxs-lookup"><span data-stu-id="53546-179">**RHEL/CentOS**</span></span>
    ```bash
    # sudo yum install util-linux
    # sudo fstrim /data
    ```
