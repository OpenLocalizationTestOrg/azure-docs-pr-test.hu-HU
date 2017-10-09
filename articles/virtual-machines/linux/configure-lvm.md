---
title: "a Linux rendszerű virtuális gép LVM aaaConfigure |} Microsoft Docs"
description: Megtudhatja, hogyan tooconfigure LVM Linux az Azure-ban.
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
ms.openlocfilehash: 8daf792d87c6bb3d91a2eddcd01cfab34fd28cff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-lvm-on-a-linux-vm-in-azure"></a><span data-ttu-id="c9919-103">A Linux virtuális gép az Azure-ban LVM konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c9919-103">Configure LVM on a Linux VM in Azure</span></span>
<span data-ttu-id="c9919-104">Ez a dokumentum bemutatja, hogyan lehet hogyan tooconfigure logikai kötet Manager (LVM) az Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="c9919-104">This document will discuss how tooconfigure Logical Volume Manager (LVM) in your Azure virtual machine.</span></span> <span data-ttu-id="c9919-105">Bár ez megvalósítható tooconfigure lemezen LVM csatolt toohello virtuális gép, alapértelmezés szerint a legtöbb felhő lemezképek nem fognak LVM konfigurált hello operációsrendszer-lemezzel.</span><span class="sxs-lookup"><span data-stu-id="c9919-105">While it is feasible tooconfigure LVM on any disk attached toohello virtual machine, by default most cloud images will not have LVM configured on hello OS disk.</span></span> <span data-ttu-id="c9919-106">Ez az ismétlődő kötet csoportok tooprevent problémákat, ha az operációsrendszer-lemez van legalább egyszer hello csatolt tooanother hello a virtuális gép ugyanahhoz felosztáshoz és a típus, azaz a helyreállítás során.</span><span class="sxs-lookup"><span data-stu-id="c9919-106">This is tooprevent problems with duplicate volume groups if hello OS disk is ever attached tooanother VM of hello same distribution and type, i.e. during a recovery scenario.</span></span> <span data-ttu-id="c9919-107">Ezért ajánlott csak toouse LVM hello adatok lemezeken.</span><span class="sxs-lookup"><span data-stu-id="c9919-107">Therefore it is recommended only toouse LVM on hello data disks.</span></span>

## <a name="linear-vs-striped-logical-volumes"></a><span data-ttu-id="c9919-108">Csíkozott köteteken tárolni logikai és lineáris</span><span class="sxs-lookup"><span data-stu-id="c9919-108">Linear vs. striped logical volumes</span></span>
<span data-ttu-id="c9919-109">LVM lehet használt toocombine egy egyetlen tárolókötethez való fizikai lemezek számát.</span><span class="sxs-lookup"><span data-stu-id="c9919-109">LVM can be used toocombine a number of physical disks into a single storage volume.</span></span> <span data-ttu-id="c9919-110">Alapértelmezés szerint LVM általában létrehoz lineáris logikai kötetek, ami azt jelenti, hogy együtt össze van-e fűzve hello fizikai tároló.</span><span class="sxs-lookup"><span data-stu-id="c9919-110">By default LVM will usually create linear logical volumes, which means that hello physical storage is concatenated together.</span></span> <span data-ttu-id="c9919-111">Ebben az esetben olvasási/írási műveletek általában csak küld tooa egy lemezt.</span><span class="sxs-lookup"><span data-stu-id="c9919-111">In this case read/write operations will typically only be sent tooa single disk.</span></span> <span data-ttu-id="c9919-112">Ezzel szemben is létrehozhatunk olyan logikai csíkozott olvasási és írási esetén elosztott toomultiple lemezek hello kötet csoportból (azaz hasonló tooRAID0).</span><span class="sxs-lookup"><span data-stu-id="c9919-112">In contrast, we can also create striped logical volumes where reads and writes are distributed toomultiple disks contained in hello volume group (i.e. similar tooRAID0).</span></span> <span data-ttu-id="c9919-113">Valószínűleg teljesítmény érdekében érdemes toostripe a logikai köteteket, hogy az olvasások és írások használják a mellékelt adatok lemezek.</span><span class="sxs-lookup"><span data-stu-id="c9919-113">For performance reasons it is likely you will want toostripe your logical volumes so that reads and writes utilize all your attached data disks.</span></span>

<span data-ttu-id="c9919-114">Ez a dokumentum ismerteti, hogyan toocombine több adat lemezek kötetéről csoportba, és majd a csíkozott logikai kötet létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c9919-114">This document will describe how toocombine several data disks into a single volume group, and then create a striped logical volume.</span></span> <span data-ttu-id="c9919-115">hello az alábbi lépésekre némileg általánosított toowork a legtöbb terjesztéseket.</span><span class="sxs-lookup"><span data-stu-id="c9919-115">hello steps below are somewhat generalized toowork with most distributions.</span></span> <span data-ttu-id="c9919-116">A legtöbb esetben hello segédprogramok és munkafolyamatok kezelése az Azure-on LVM nincsenek alapvetően más, mint más környezetekben.</span><span class="sxs-lookup"><span data-stu-id="c9919-116">In most cases hello utilities and workflows for managing LVM on Azure are not fundamentally different than other environments.</span></span> <span data-ttu-id="c9919-117">Szokásos módon is tekintse át a Linux forgalmazójával dokumentációk és ajánlott eljárások az adott terjesztési LVM használatával.</span><span class="sxs-lookup"><span data-stu-id="c9919-117">As usual, please also consult your Linux vendor for documentation and best practices for using LVM with your particular distribution.</span></span>

## <a name="attaching-data-disks"></a><span data-ttu-id="c9919-118">Adatlemez csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="c9919-118">Attaching data disks</span></span>
<span data-ttu-id="c9919-119">Egy általában érdemes toostart két vagy több üres adatlemezekkel rendelkező LVM használatakor.</span><span class="sxs-lookup"><span data-stu-id="c9919-119">One will usually want toostart with two or more empty data disks when using LVM.</span></span> <span data-ttu-id="c9919-120">IO igényei szerint, kiválaszthatja a tooattach tárolódnak a standard szintű tárolást, a másolatot IO too500 ps / lemez vagy a prémium szintű storage IO too5000 ps lemezenként fel a lemezeket.</span><span class="sxs-lookup"><span data-stu-id="c9919-120">Based on your IO needs, you can choose tooattach disks that are stored in our Standard Storage, with up too500 IO/ps per disk or our Premium storage with up too5000 IO/ps per disk.</span></span> <span data-ttu-id="c9919-121">Ez a cikk nem kerül részletes hogyan tooprovision, és csatlakoztassa az adatok lemezek tooa Linux virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c9919-121">This article will not go into detail on how tooprovision and attach data disks tooa Linux virtual machine.</span></span> <span data-ttu-id="c9919-122">Tekintse meg a Microsoft Azure-cikk hello [lemezt csatlakoztatni](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) részletes útmutató hogyan tooattach egy üres adatok lemezre tooa Linux virtuális gépet az Azure.</span><span class="sxs-lookup"><span data-stu-id="c9919-122">Please see hello Microsoft Azure article [attach a disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) for detailed instructions on how tooattach an empty data disk tooa Linux virtual machine on Azure.</span></span>

## <a name="install-hello-lvm-utilities"></a><span data-ttu-id="c9919-123">Hello LVM segédeszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="c9919-123">Install hello LVM utilities</span></span>
* <span data-ttu-id="c9919-124">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="c9919-124">**Ubuntu**</span></span>

    ```bash  
    sudo apt-get update
    sudo apt-get install lvm2
    ```

* <span data-ttu-id="c9919-125">**RHEL, CentOS & Oracle Linux**</span><span class="sxs-lookup"><span data-stu-id="c9919-125">**RHEL, CentOS & Oracle Linux**</span></span>

    ```bash   
    sudo yum install lvm2
    ```

* <span data-ttu-id="c9919-126">**SLES 12 és openSUSE**</span><span class="sxs-lookup"><span data-stu-id="c9919-126">**SLES 12 and openSUSE**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

* <span data-ttu-id="c9919-127">**SLES 11**</span><span class="sxs-lookup"><span data-stu-id="c9919-127">**SLES 11**</span></span>

    ```bash   
    sudo zypper install lvm2
    ```

    <span data-ttu-id="c9919-128">A SLES11 is szerkeszteni kell `/etc/sysconfig/lvm` és `LVM_ACTIVATED_ON_DISCOVERED` túl "Engedélyezés":</span><span class="sxs-lookup"><span data-stu-id="c9919-128">On SLES11 you must also edit `/etc/sysconfig/lvm` and set `LVM_ACTIVATED_ON_DISCOVERED` too"enable":</span></span>

    ```sh   
    LVM_ACTIVATED_ON_DISCOVERED="enable" 
    ```

## <a name="configure-lvm"></a><span data-ttu-id="c9919-129">LVM konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c9919-129">Configure LVM</span></span>
<span data-ttu-id="c9919-130">A jelen útmutató indulunk ki csatolt adatok három lemezt, amely kifejezés tooas `/dev/sdc`, `/dev/sdd` és `/dev/sde`.</span><span class="sxs-lookup"><span data-stu-id="c9919-130">In this guide we will assume you have attached three data disks, which we'll refer tooas `/dev/sdc`, `/dev/sdd` and `/dev/sde`.</span></span> <span data-ttu-id="c9919-131">Vegye figyelembe, hogy ezeket nem lehet a virtuális gép ugyanazon útvonalneveket hello legyen.</span><span class="sxs-lookup"><span data-stu-id="c9919-131">Note that these may not always be hello same path names in your VM.</span></span> <span data-ttu-id="c9919-132">Futtathatja a(z)`sudo fdisk -l`"vagy hasonló parancs toolist a rendelkezésre álló lemezek.</span><span class="sxs-lookup"><span data-stu-id="c9919-132">You can run '`sudo fdisk -l`' or similar command toolist your available disks.</span></span>

1. <span data-ttu-id="c9919-133">Készítse elő a hello fizikai kötetekre:</span><span class="sxs-lookup"><span data-stu-id="c9919-133">Prepare hello physical volumes:</span></span>

    ```bash    
    sudo pvcreate /dev/sd[cde]
    Physical volume "/dev/sdc" successfully created
    Physical volume "/dev/sdd" successfully created
    Physical volume "/dev/sde" successfully created
    ```

2. <span data-ttu-id="c9919-134">Hozzon létre egy kötet csoportot.</span><span class="sxs-lookup"><span data-stu-id="c9919-134">Create a volume group.</span></span> <span data-ttu-id="c9919-135">Ebben a példában hívjuk hello kötet csoport `data-vg01`:</span><span class="sxs-lookup"><span data-stu-id="c9919-135">In this example we are calling hello volume group `data-vg01`:</span></span>

    ```bash    
    sudo vgcreate data-vg01 /dev/sd[cde]
    Volume group "data-vg01" successfully created
    ```

3. <span data-ttu-id="c9919-136">Hello logikai kötetek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c9919-136">Create hello logical volume(s).</span></span> <span data-ttu-id="c9919-137">hello azt az alábbi parancs létrehoz nevű egyetlen logikai kötet `data-lv01` toospan hello teljes kötet csoport, de ne feledje, hogy azt is megvalósítható toocreate több logikai kötet hello kötet csoportban.</span><span class="sxs-lookup"><span data-stu-id="c9919-137">hello command below we will create a single logical volume called `data-lv01` toospan hello entire volume group, but note that it is also feasible toocreate multiple logical volumes in hello volume group.</span></span>

    ```bash   
    sudo lvcreate --extents 100%FREE --stripes 3 --name data-lv01 data-vg01
    Logical volume "data-lv01" created.
    ```

4. <span data-ttu-id="c9919-138">Kötet formázásának hello logikai</span><span class="sxs-lookup"><span data-stu-id="c9919-138">Format hello logical volume</span></span>

    ```bash  
    sudo mkfs -t ext4 /dev/data-vg01/data-lv01
    ```
   
   > [!NOTE]
   > <span data-ttu-id="c9919-139">SLES11 használatával `-t ext3` ext4 helyett.</span><span class="sxs-lookup"><span data-stu-id="c9919-139">With SLES11 use `-t ext3` instead of ext4.</span></span> <span data-ttu-id="c9919-140">SLES11 csak olvasási hozzáféréssel tooext4 fájlrendszerek támogatja.</span><span class="sxs-lookup"><span data-stu-id="c9919-140">SLES11 only supports read-only access tooext4 filesystems.</span></span>

## <a name="add-hello-new-file-system-tooetcfstab"></a><span data-ttu-id="c9919-141">Hello új fájl rendszer túl/etc/fstab hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c9919-141">Add hello new file system too/etc/fstab</span></span>
> [!IMPORTANT]
> <span data-ttu-id="c9919-142">Nem megfelelően szerkesztése hello `/etc/fstab` fájl meghiúsulását eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="c9919-142">Improperly editing hello `/etc/fstab` file could result in an unbootable system.</span></span> <span data-ttu-id="c9919-143">Ha nem ismeri, tekintse meg toohello terjesztési dokumentációjában olvashat, hogyan tooproperly szerkesztheti a fájlt.</span><span class="sxs-lookup"><span data-stu-id="c9919-143">If unsure, please refer toohello distribution's documentation for information on how tooproperly edit this file.</span></span> <span data-ttu-id="c9919-144">Is ajánlott, amelyek a biztonsági másolatának hello `/etc/fstab` fájl szerkesztése előtt jön létre.</span><span class="sxs-lookup"><span data-stu-id="c9919-144">It is also recommended that a backup of hello `/etc/fstab` file is created before editing.</span></span>

1. <span data-ttu-id="c9919-145">Hozzon létre szükséges hello csatlakoztatási pontot az új fájlrendszer, például:</span><span class="sxs-lookup"><span data-stu-id="c9919-145">Create hello desired mount point for your new file system, for example:</span></span>

    ```bash  
    sudo mkdir /data
    ```

2. <span data-ttu-id="c9919-146">Keresse meg a hello logikai kötet elérési útja</span><span class="sxs-lookup"><span data-stu-id="c9919-146">Locate hello logical volume path</span></span>

    ```bash    
    lvdisplay
    --- Logical volume ---
    LV Path                /dev/data-vg01/data-lv01
    ....
    ```

3. <span data-ttu-id="c9919-147">Nyissa meg `/etc/fstab` egy szövegszerkesztőben, és adja hozzá egy bejegyzést a hello új fájlrendszer, például:</span><span class="sxs-lookup"><span data-stu-id="c9919-147">Open `/etc/fstab` in a text editor and add an entry for hello new file system, for example:</span></span>

    ```bash    
    /dev/data-vg01/data-lv01  /data  ext4  defaults  0  2
    ```   
    <span data-ttu-id="c9919-148">Ezt követően mentse és zárja be `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="c9919-148">Then, save and close `/etc/fstab`.</span></span>

4. <span data-ttu-id="c9919-149">Tesztelje, hogy hello `/etc/fstab` bejegyzés helyességét:</span><span class="sxs-lookup"><span data-stu-id="c9919-149">Test that hello `/etc/fstab` entry is correct:</span></span>

    ```bash    
    sudo mount -a
    ```

    <span data-ttu-id="c9919-150">Ha ezt a parancsot egy hibaüzenet ellenőrizze hello hello szintaxist `/etc/fstab` fájlt.</span><span class="sxs-lookup"><span data-stu-id="c9919-150">If this command results in an error message please check hello syntax in hello `/etc/fstab` file.</span></span>
   
    <span data-ttu-id="c9919-151">Ezután futtassa az hello `mount` parancs tooensure hello fájlrendszer van csatlakoztatva:</span><span class="sxs-lookup"><span data-stu-id="c9919-151">Next run hello `mount` command tooensure hello file system is mounted:</span></span>

    ```bash    
    mount
    ......
    /dev/mapper/data--vg01-data--lv01 on /data type ext4 (rw)
    ```

5. <span data-ttu-id="c9919-152">(Választható) A FailSafe rendszerindító paraméterek`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="c9919-152">(Optional) Failsafe boot parameters in `/etc/fstab`</span></span>
   
    <span data-ttu-id="c9919-153">Terjesztések tartalmaz vagy hello `nobootwait` vagy `nofail` toohello hozzáadott paraméterek csatlakoztatási `/etc/fstab` fájlt.</span><span class="sxs-lookup"><span data-stu-id="c9919-153">Many distributions include either hello `nobootwait` or `nofail` mount parameters that may be added toohello `/etc/fstab` file.</span></span> <span data-ttu-id="c9919-154">Ezek a paraméterek hibák engedélyezése, ha egy adott fájlrendszer csatlakoztatása és hello Linux rendszer toocontinue tooboot engedélyezi, akkor is, ha nem tooproperly csatlakoztatási hello RAID fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="c9919-154">These parameters allow for failures when mounting a particular file system and allow hello Linux system toocontinue tooboot even if it is unable tooproperly mount hello RAID file system.</span></span> <span data-ttu-id="c9919-155">További információ ezekről a paraméterekről tooyour terjesztési dokumentációjában tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="c9919-155">Please refer tooyour distribution's documentation for more information on these parameters.</span></span>
   
    <span data-ttu-id="c9919-156">(Ubuntu). példa:</span><span class="sxs-lookup"><span data-stu-id="c9919-156">Example (Ubuntu):</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,nobootwait  0  2
    ```

## <a name="trimunmap-support"></a><span data-ttu-id="c9919-157">TRIM/UNMAP támogatása</span><span class="sxs-lookup"><span data-stu-id="c9919-157">TRIM/UNMAP support</span></span>
<span data-ttu-id="c9919-158">Egyes Linux kernelek vágást/UNMAP műveletek toodiscard támogatja a nem használt blokkok hello lemezen.</span><span class="sxs-lookup"><span data-stu-id="c9919-158">Some Linux kernels support TRIM/UNMAP operations toodiscard unused blocks on hello disk.</span></span> <span data-ttu-id="c9919-159">Ezek a műveletek elsősorban hasznosak standard tárolási tooinform Azure törölt lapok már nem érvényes, és is elvesznek.</span><span class="sxs-lookup"><span data-stu-id="c9919-159">These operations are primarily useful in standard storage tooinform Azure that deleted pages are no longer valid and can be discarded.</span></span> <span data-ttu-id="c9919-160">Lapok elvetése költség mentheti, ha nagy fájlok létrehozása, majd törli őket.</span><span class="sxs-lookup"><span data-stu-id="c9919-160">Discarding pages can save cost if you create large files and then delete them.</span></span>

<span data-ttu-id="c9919-161">Két módon tooenable vágást támogatja a Linux virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="c9919-161">There are two ways tooenable TRIM support in your Linux VM.</span></span> <span data-ttu-id="c9919-162">A szokásos módon olvassa el a terjesztési hello ajánlott megközelítést alkalmazva:</span><span class="sxs-lookup"><span data-stu-id="c9919-162">As usual, consult your distribution for hello recommended approach:</span></span>

- <span data-ttu-id="c9919-163">Használjon hello `discard` lehetőség a csatlakoztatási `/etc/fstab`, például:</span><span class="sxs-lookup"><span data-stu-id="c9919-163">Use hello `discard` mount option in `/etc/fstab`, for example:</span></span>

    ```bash 
    /dev/data-vg01/data-lv01  /data  ext4  defaults,discard  0  2
    ```

- <span data-ttu-id="c9919-164">Az egyes esetekben hello `discard` lehetőség is van a teljesítményre.</span><span class="sxs-lookup"><span data-stu-id="c9919-164">In some cases hello `discard` option may have performance implications.</span></span> <span data-ttu-id="c9919-165">Alternatív megoldásként futtathatja a hello `fstrim` manuálisan parancsot a parancssorból hello, vagy felveheti Ön is tooyour crontab toorun rendszeresen:</span><span class="sxs-lookup"><span data-stu-id="c9919-165">Alternatively, you can run hello `fstrim` command manually from hello command line, or add it tooyour crontab toorun regularly:</span></span>

    <span data-ttu-id="c9919-166">**Ubuntu**</span><span class="sxs-lookup"><span data-stu-id="c9919-166">**Ubuntu**</span></span>

    ```bash 
    # sudo apt-get install util-linux
    # sudo fstrim /datadrive
    ```

    <span data-ttu-id="c9919-167">**RHEL vagy CentOS**</span><span class="sxs-lookup"><span data-stu-id="c9919-167">**RHEL/CentOS**</span></span>

    ```bash 
    # sudo yum install util-linux
    # sudo fstrim /datadrive
    ```
