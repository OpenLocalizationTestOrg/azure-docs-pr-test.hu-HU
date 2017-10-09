---
title: "MySQL-teljesítmény Linux aaaOptimize |} Microsoft Docs"
description: "Megtudhatja, hogyan toooptimize MySQL futó Linux operációs rendszert futtató Azure virtuális géphez (VM)."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 0c1c7fc5-a528-4d84-b65d-2df225f2233f
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: ningk
ms.openlocfilehash: 9e6458723233721e06f30b9de33635d403eefcba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a><span data-ttu-id="01e06-103">Az Azure Linux virtuális gépeken futó MySQL teljesítményének optimalizálása</span><span class="sxs-lookup"><span data-stu-id="01e06-103">Optimize MySQL Performance on Azure Linux VMs</span></span>
<span data-ttu-id="01e06-104">Nincsenek számos tényező befolyásolja az Azure, mind a virtuális hardver kiválasztása és szoftverkonfigurációt MySQL teljesítményére.</span><span class="sxs-lookup"><span data-stu-id="01e06-104">There are many factors that affect MySQL performance on Azure, both in virtual hardware selection and software configuration.</span></span> <span data-ttu-id="01e06-105">Ez a cikk foglalkozik, a tárolás, a rendszer és a Helyadatbázis-konfigurációk keresztül optimalizálás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="01e06-105">This article focuses on optimizing performance through storage, system, and database configurations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="01e06-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="01e06-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="01e06-107">Ez a cikk hello klasszikus telepítési modell használatát bemutatja.</span><span class="sxs-lookup"><span data-stu-id="01e06-107">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="01e06-108">A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="01e06-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="01e06-109">További információ a Linux virtuális gép optimalizálásokat hello Resource Manager modellt: [optimalizálhatja a Linux virtuális Gépet az Azure-on](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="01e06-109">For information about Linux VM optimizations with hello Resource Manager model, see [Optimize your Linux VM on Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="utilize-raid-on-an-azure-virtual-machine"></a><span data-ttu-id="01e06-110">Egy Azure virtuális gépen RAID használata</span><span class="sxs-lookup"><span data-stu-id="01e06-110">Utilize RAID on an Azure virtual machine</span></span>
<span data-ttu-id="01e06-111">Storage egy hello kulcsfontosságú tényező, amely befolyásolja az adatbázis teljesítményének felhőalapú környezetben.</span><span class="sxs-lookup"><span data-stu-id="01e06-111">Storage is hello key factor that affects database performance in cloud environments.</span></span> <span data-ttu-id="01e06-112">Összehasonlított tooa egyetlen lemez, RAID keresztül párhuzamossági gyorsabb hozzáférést biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="01e06-112">Compared tooa single disk, RAID can provide faster access via concurrency.</span></span> <span data-ttu-id="01e06-113">További információkért lásd: [szabványos RAID-szintek](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span><span class="sxs-lookup"><span data-stu-id="01e06-113">For more information, see [Standard RAID levels](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span></span>   

<span data-ttu-id="01e06-114">Lemezes i/o-átviteli és az Azure-ban i/o-válaszidő RAID javítható ki.</span><span class="sxs-lookup"><span data-stu-id="01e06-114">Disk I/O throughput and I/O response time in Azure can be improved through RAID.</span></span> <span data-ttu-id="01e06-115">Labor tesztek megjelenítése, hogy a lemez i/o-átviteli meg kell duplázni és i/o-válaszidő csökkenthető fele átlagosan RAID lemezek hello száma nő (a két toofour négy tooeight, stb.) Ha.</span><span class="sxs-lookup"><span data-stu-id="01e06-115">Our lab tests show that disk I/O throughput can be doubled and I/O response time can be reduced by half on average when hello number of RAID disks is doubled (from two toofour, four tooeight, etc.).</span></span> <span data-ttu-id="01e06-116">Lásd: [függelék](#AppendixA) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="01e06-116">See [Appendix A](#AppendixA) for details.</span></span>  

<span data-ttu-id="01e06-117">Továbbá toodisk i/o, MySQL teljesítmény növeli a hello milyen RAID-szinteket növelésével.</span><span class="sxs-lookup"><span data-stu-id="01e06-117">In addition toodisk I/O, MySQL performance improves when you increase hello RAID level.</span></span>  <span data-ttu-id="01e06-118">Lásd: [B függelék](#AppendixB) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="01e06-118">See [Appendix B](#AppendixB) for details.</span></span>  

<span data-ttu-id="01e06-119">Érdemes lehet tooconsider hello adatrészlet méretének.</span><span class="sxs-lookup"><span data-stu-id="01e06-119">You might also want tooconsider hello chunk size.</span></span> <span data-ttu-id="01e06-120">Általában még nagyobb adattömbméretet, nyílik meg alsó terhelés, különösen a nagy írási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="01e06-120">In general, when you have a larger chunk size, you get lower overhead, especially for large writes.</span></span> <span data-ttu-id="01e06-121">Azonban ha hello adatrészlet mérete túl nagy, adhat, amely megakadályozza, hogy kihasználja a RAID többletterhelést.</span><span class="sxs-lookup"><span data-stu-id="01e06-121">However, when hello chunk size is too large, it might add additional overhead that prevents you from taking advantage of RAID.</span></span> <span data-ttu-id="01e06-122">hello jelenlegi alapértelmezett méret 512 KB, ami bizonyítása toobe optimális legáltalánosabb éles környezetekben.</span><span class="sxs-lookup"><span data-stu-id="01e06-122">hello current default size is 512 KB, which is proven toobe optimal for most general production environments.</span></span> <span data-ttu-id="01e06-123">Lásd: [C függelék](#AppendixC) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="01e06-123">See [Appendix C](#AppendixC) for details.</span></span>   

<span data-ttu-id="01e06-124">Nincsenek korlátozások is hozzáadhat más virtuális gépek típusainak hány lemezeken.</span><span class="sxs-lookup"><span data-stu-id="01e06-124">There are limits on how many disks you can add for different virtual machine types.</span></span> <span data-ttu-id="01e06-125">Ezek a korlátozások részletes leírást talál [virtuális gép és felhő mérete](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="01e06-125">These limits are detailed in [Virtual machine and cloud service sizes for Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span> <span data-ttu-id="01e06-126">De megadható tooset mentése RAID kevesebb lemezzel kell négy csatolt adatok lemezek toofollow hello RAID példa ebben a cikkben.</span><span class="sxs-lookup"><span data-stu-id="01e06-126">You will need four attached data disks toofollow hello RAID example in this article, although you can choose tooset up RAID with fewer disks.</span></span>  

<span data-ttu-id="01e06-127">Ez a cikk azt feltételezi, hogy már létrehozott egy Linux virtuális gép és a MYSQL telepítette és konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="01e06-127">This article assumes you have already created a Linux virtual machine and have MYSQL installed and configured.</span></span> <span data-ttu-id="01e06-128">A bevezetés további információkért lásd: hogyan tooinstall MySQL az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="01e06-128">For more information on getting started, see How tooinstall MySQL on Azure.</span></span>  

### <a name="set-up-raid-on-azure"></a><span data-ttu-id="01e06-129">Az Azure-on RAID beállítása</span><span class="sxs-lookup"><span data-stu-id="01e06-129">Set up RAID on Azure</span></span>
<span data-ttu-id="01e06-130">hello következő lépések bemutatják, hogyan toocreate RAID-Azure hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="01e06-130">hello following steps show how toocreate RAID on Azure by using hello Azure portal.</span></span> <span data-ttu-id="01e06-131">Is állíthatja be RAID Windows PowerShell-parancsfájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="01e06-131">You can also set up RAID by using Windows PowerShell scripts.</span></span>
<span data-ttu-id="01e06-132">Ebben a példában azt konfigurálja RAID 0 négy lemezzel.</span><span class="sxs-lookup"><span data-stu-id="01e06-132">In this example, we will configure RAID 0 with four disks.</span></span>  

#### <a name="add-a-data-disk-tooyour-virtual-machine"></a><span data-ttu-id="01e06-133">Adatok lemez tooyour virtuális gép hozzáadása</span><span class="sxs-lookup"><span data-stu-id="01e06-133">Add a data disk tooyour virtual machine</span></span>
<span data-ttu-id="01e06-134">Az hello Azure-portálon lépjen toohello irányítópult, és válassza ki a kívánt tooadd adatlemezt hello virtuális gép toowhich.</span><span class="sxs-lookup"><span data-stu-id="01e06-134">In hello Azure portal, go toohello dashboard and select hello virtual machine toowhich you want tooadd a data disk.</span></span> <span data-ttu-id="01e06-135">Ebben a példában a hello virtuális gép olyan mysqlnode1.</span><span class="sxs-lookup"><span data-stu-id="01e06-135">In this example, hello virtual machine is mysqlnode1.</span></span>  

<!--![Virtual machines][1]-->

<span data-ttu-id="01e06-136">Kattintson a **lemezek** majd **új csatolása**.</span><span class="sxs-lookup"><span data-stu-id="01e06-136">Click **Disks** and then click **Attach New**.</span></span>

![Lemez hozzáadása a virtuális gépek](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

<span data-ttu-id="01e06-138">Hozzon létre egy új 500 GB lemezterület.</span><span class="sxs-lookup"><span data-stu-id="01e06-138">Create a new 500 GB disk.</span></span> <span data-ttu-id="01e06-139">Győződjön meg arról, hogy **állomás gyorsítótár preferencia** értéke túl**nincs**.</span><span class="sxs-lookup"><span data-stu-id="01e06-139">Make sure that **Host Cache Preference** is set too**None**.</span></span>  <span data-ttu-id="01e06-140">Amikor végzett, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="01e06-140">When you're finished, click **OK**.</span></span>

![Üres lemez csatolása](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


<span data-ttu-id="01e06-142">Ez hozzáadja egy üres lemez a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="01e06-142">This adds one empty disk into your virtual machine.</span></span> <span data-ttu-id="01e06-143">Ismételje meg ezt a három még többször, hogy négy adatlemezek a RAID.</span><span class="sxs-lookup"><span data-stu-id="01e06-143">Repeat this step three more times so that you have four data disks for RAID.</span></span>  

<span data-ttu-id="01e06-144">Megtekintheti a hozzáadott hello meghajtókat a hello virtuális gép hello kernel üzenet napló megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="01e06-144">You can see hello added drives in hello virtual machine by looking at hello kernel message log.</span></span> <span data-ttu-id="01e06-145">Például toosee Ez az Ubuntu, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="01e06-145">For example, toosee this on Ubuntu, use hello following command:</span></span>  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-hello-additional-disks"></a><span data-ttu-id="01e06-146">RAID hozzon létre hello további lemezek</span><span class="sxs-lookup"><span data-stu-id="01e06-146">Create RAID with hello additional disks</span></span>
<span data-ttu-id="01e06-147">hello következő lépések bemutatják, hogyan túl[szoftveres RAID Linux konfigurálása](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="01e06-147">hello following steps describe how too[configure software RAID on Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="01e06-148">Ha hello XFS fájlrendszert használ, a végrehajtást hello RAID létrehozása után a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="01e06-148">If you are using hello XFS file system, execute hello following steps after you have created RAID.</span></span>
>
>

<span data-ttu-id="01e06-149">tooinstall XFS Debian, Ubuntu vagy Linux menta használata hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="01e06-149">tooinstall XFS on Debian, Ubuntu, or Linux Mint, use hello following command:</span></span>  

    apt-get -y install xfsprogs  

<span data-ttu-id="01e06-150">tooinstall XFS Fedora, CentOS vagy RHEL, használjon hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="01e06-150">tooinstall XFS on Fedora, CentOS, or RHEL, use hello following command:</span></span>  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a><span data-ttu-id="01e06-151">Állítson be egy új. tárolási elérési útja</span><span class="sxs-lookup"><span data-stu-id="01e06-151">Set up a new storage path</span></span>
<span data-ttu-id="01e06-152">A következő parancs tooset be egy új. tárolási elérési útja hello használata:</span><span class="sxs-lookup"><span data-stu-id="01e06-152">Use hello following command tooset up a new storage path:</span></span>  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-hello-original-data-toohello-new-storage-path"></a><span data-ttu-id="01e06-153">Másolás hello eredeti toohello új tároló elérési útja</span><span class="sxs-lookup"><span data-stu-id="01e06-153">Copy hello original data toohello new storage path</span></span>
<span data-ttu-id="01e06-154">A következő parancs toocopy toohello új tároló elérési útja hello használata:</span><span class="sxs-lookup"><span data-stu-id="01e06-154">Use hello following command toocopy data toohello new storage path:</span></span>  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-hello-data-disk"></a><span data-ttu-id="01e06-155">Módosítsa az engedélyeket, MySQL hozzáférhet (olvasási és írási) hello adatlemez</span><span class="sxs-lookup"><span data-stu-id="01e06-155">Modify permissions so MySQL can access (read and write) hello data disk</span></span>
<span data-ttu-id="01e06-156">Használja az alábbi parancs toomodify engedélyek hello:</span><span class="sxs-lookup"><span data-stu-id="01e06-156">Use hello following command toomodify permissions:</span></span>  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-hello-disk-io-scheduling-algorithm"></a><span data-ttu-id="01e06-157">Hello lemez i/o algoritmus ütemezés módosítása</span><span class="sxs-lookup"><span data-stu-id="01e06-157">Adjust hello disk I/O scheduling algorithm</span></span>
<span data-ttu-id="01e06-158">Linux megvalósítja négy különböző ütemezése algoritmusok i/o:</span><span class="sxs-lookup"><span data-stu-id="01e06-158">Linux implements four types of I/O scheduling algorithms:</span></span>  

* <span data-ttu-id="01e06-159">NOOP algoritmus (nincs művelet)</span><span class="sxs-lookup"><span data-stu-id="01e06-159">NOOP algorithm (No Operation)</span></span>
* <span data-ttu-id="01e06-160">Határidő algoritmus (határidő)</span><span class="sxs-lookup"><span data-stu-id="01e06-160">Deadline algorithm (Deadline)</span></span>
* <span data-ttu-id="01e06-161">Teljesen valós Üzenetsor-kezelés algoritmus (CFQ)</span><span class="sxs-lookup"><span data-stu-id="01e06-161">Completely fair queuing algorithm (CFQ)</span></span>
* <span data-ttu-id="01e06-162">Keret időszak algoritmus (Anticipatory)</span><span class="sxs-lookup"><span data-stu-id="01e06-162">Budget period algorithm (Anticipatory)</span></span>  

<span data-ttu-id="01e06-163">A különböző alkalmazási helyzetek toooptimize teljesítmény különböző i/o-bejegyzéstípusait választhatja ki.</span><span class="sxs-lookup"><span data-stu-id="01e06-163">You can select different I/O schedulers under different scenarios toooptimize performance.</span></span> <span data-ttu-id="01e06-164">Teljesen elérésű környezetben nincs hello CFQ és a határidő algoritmusok teljesítményének jelentős különbsége.</span><span class="sxs-lookup"><span data-stu-id="01e06-164">In a completely random access environment, there is not a significant difference between hello CFQ and Deadline algorithms for performance.</span></span> <span data-ttu-id="01e06-165">Azt javasoljuk, hogy állítsa a hello MySQL adatbázis környezet tooDeadline stabilitását.</span><span class="sxs-lookup"><span data-stu-id="01e06-165">We recommend that you set hello MySQL database environment tooDeadline for stability.</span></span> <span data-ttu-id="01e06-166">Ha nagy mennyiségű szekvenciális i/o, CFQ csökkentheti a lemezek i/o műveleteinek teljesítménye.</span><span class="sxs-lookup"><span data-stu-id="01e06-166">If there is a lot of sequential I/O, CFQ might reduce disk I/O performance.</span></span>   

<span data-ttu-id="01e06-167">Az SSD és egyéb eszközökről NOOP vagy határidő érhető el hello alapértelmezett Feladatütemező jobb teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="01e06-167">For SSD and other equipment, NOOP or Deadline can achieve better performance than hello default scheduler.</span></span>   

<span data-ttu-id="01e06-168">Előzetes toohello kernel 2.5, hello alapértelmezett i/o algoritmus ütemezés határidő.</span><span class="sxs-lookup"><span data-stu-id="01e06-168">Prior toohello kernel 2.5, hello default I/O scheduling algorithm is Deadline.</span></span> <span data-ttu-id="01e06-169">Hello kernel 2.6.18 verziótól kezdődően a CFQ hello alapértelmezett i/o-ütemezési algoritmus inaktívvá vált.</span><span class="sxs-lookup"><span data-stu-id="01e06-169">Starting with hello kernel 2.6.18, CFQ became hello default I/O scheduling algorithm.</span></span>  <span data-ttu-id="01e06-170">Adja meg ezt a beállítást, kernel rendszerindítás közben, vagy hello rendszer futtatásakor dinamikusan módosítsa ezt a beállítást.</span><span class="sxs-lookup"><span data-stu-id="01e06-170">You can specify this setting at kernel boot time or dynamically modify this setting when hello system is running.</span></span>  

<span data-ttu-id="01e06-171">hello a következő példa bemutatja, hogyan toocheck és a készlet hello alapértelmezett Feladatütemező toohello NOOP algoritmus hello Debian terjesztési termékcsalád.</span><span class="sxs-lookup"><span data-stu-id="01e06-171">hello following example demonstrates how toocheck and set hello default scheduler toohello NOOP algorithm in hello Debian distribution family.</span></span>  

### <a name="view-hello-current-io-scheduler"></a><span data-ttu-id="01e06-172">Nézet hello aktuális i/o-ütemező</span><span class="sxs-lookup"><span data-stu-id="01e06-172">View hello current I/O scheduler</span></span>
<span data-ttu-id="01e06-173">tooview hello Feladatütemező futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="01e06-173">tooview hello scheduler run hello following command:</span></span>  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

<span data-ttu-id="01e06-174">A következő kimeneti, amely megadja, hogy a jelenlegi Feladatütemező hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="01e06-174">You will see following output, which indicates hello current scheduler:</span></span>  

    noop [deadline] cfq


### <a name="change-hello-current-device-devsda-of-hello-io-scheduling-algorithm"></a><span data-ttu-id="01e06-175">Hello aktuális eszköz (/ dev/sda) hello i/o-ütemezési algoritmus módosítása</span><span class="sxs-lookup"><span data-stu-id="01e06-175">Change hello current device (/dev/sda) of hello I/O scheduling algorithm</span></span>
<span data-ttu-id="01e06-176">Futtassa a következő parancsok toochange hello aktuális eszköz hello:</span><span class="sxs-lookup"><span data-stu-id="01e06-176">Run hello following commands toochange hello current device:</span></span>  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> <span data-ttu-id="01e06-177">Beállítása ez /dev/sda csak akkor nem hasznos.</span><span class="sxs-lookup"><span data-stu-id="01e06-177">Setting this for /dev/sda alone is not useful.</span></span> <span data-ttu-id="01e06-178">Kell beállítani az összes adat lemez ahol hello adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="01e06-178">It must be set on all data disks where hello database resides.</span></span>  
>
>

<span data-ttu-id="01e06-179">A következő kimeneti, jelezve, hogy grub.cfg sikeresen újraépítették, és adott hello alapértelmezett Feladatütemező lett frissített tooNOOP hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="01e06-179">You should see hello following output, indicating that grub.cfg has been rebuilt successfully and that hello default scheduler has been updated tooNOOP:</span></span>  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

<span data-ttu-id="01e06-180">A Red Hat terjesztési termékcsalád hello meg kell csak hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="01e06-180">For hello Red Hat distribution family, you need only hello following command:</span></span>

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a><span data-ttu-id="01e06-181">Fájl műveletek rendszerbeállításainak konfigurálására</span><span class="sxs-lookup"><span data-stu-id="01e06-181">Configure system file operations settings</span></span>
<span data-ttu-id="01e06-182">Egy ajánlott toodisable hello *atime* hello fájlrendszerben funkciót.</span><span class="sxs-lookup"><span data-stu-id="01e06-182">One best practice is toodisable hello *atime* logging feature on hello file system.</span></span> <span data-ttu-id="01e06-183">Atime hello utolsó hozzáférés időpontjának.</span><span class="sxs-lookup"><span data-stu-id="01e06-183">Atime is hello last file access time.</span></span> <span data-ttu-id="01e06-184">Amikor egy fájl megnyitásakor, hello rendszer rekordjainak hello időbélyeg hello naplóban.</span><span class="sxs-lookup"><span data-stu-id="01e06-184">Whenever a file is accessed, hello file system records hello timestamp in hello log.</span></span> <span data-ttu-id="01e06-185">Ezek az információk azonban igen ritkán alkalmazzák.</span><span class="sxs-lookup"><span data-stu-id="01e06-185">However, this information is rarely used.</span></span> <span data-ttu-id="01e06-186">Ha letiltja, ha nincs szüksége, amely csökkenti a teljes lemez hozzáférés idejét.</span><span class="sxs-lookup"><span data-stu-id="01e06-186">You can disable it if you don't need it, which will reduce overall disk access time.</span></span>  

<span data-ttu-id="01e06-187">toodisable atime naplózás szükségesek toomodify hello fájl rendszer konfigurációs fájl /etc/ fstab, és adja hozzá a hello **noatime** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="01e06-187">toodisable atime logging, you need toomodify hello file system configuration file /etc/ fstab and add hello **noatime** option.</span></span>  

<span data-ttu-id="01e06-188">Hello vim /etc/fstab fájl, hello noatime hozzáadása, ahogy az a következő minta hello például szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="01e06-188">For example, edit hello vim /etc/fstab file, adding hello noatime as shown in hello following sample:</span></span>  

    # CLOUD_IMG: This file was created/modified by hello Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add hello “noatime” option below toodisable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

<span data-ttu-id="01e06-189">Majd csatlakoztassa újra hello fájlrendszer a hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="01e06-189">Then, remount hello file system with hello following command:</span></span>  

    mount -o remount /RAID0

<span data-ttu-id="01e06-190">Teszt hello eredmény módosítani.</span><span class="sxs-lookup"><span data-stu-id="01e06-190">Test hello modified result.</span></span> <span data-ttu-id="01e06-191">Hello tesztfájl módosításakor hello hozzáférés időpontja nem frissül.</span><span class="sxs-lookup"><span data-stu-id="01e06-191">When you modify hello test file, hello access time is not updated.</span></span> <span data-ttu-id="01e06-192">hello a következő példák szemléltetik milyen hello kódot a következőképpen néz módosítás előtti és utáni.</span><span class="sxs-lookup"><span data-stu-id="01e06-192">hello following examples show what hello code looks like before and after modification.</span></span>

<span data-ttu-id="01e06-193">Előtte:</span><span class="sxs-lookup"><span data-stu-id="01e06-193">Before:</span></span>        

![Code access módosítás előtt][5]

<span data-ttu-id="01e06-195">Utána:</span><span class="sxs-lookup"><span data-stu-id="01e06-195">After:</span></span>

![Code access módosítás után][6]

## <a name="increase-hello-maximum-number-of-system-handles-for-high-concurrency"></a><span data-ttu-id="01e06-197">Növelje a nagy egyidejű kezeli hello maximális számát</span><span class="sxs-lookup"><span data-stu-id="01e06-197">Increase hello maximum number of system handles for high concurrency</span></span>
<span data-ttu-id="01e06-198">MySQL egy olyan nagy feldolgozási adatbázis.</span><span class="sxs-lookup"><span data-stu-id="01e06-198">MySQL is a high concurrency database.</span></span> <span data-ttu-id="01e06-199">egyidejű leíróinak száma hello alapértelmezett érték 1024 Linux, amely nem mindig elegendő.</span><span class="sxs-lookup"><span data-stu-id="01e06-199">hello default number of concurrent handles is 1024 for Linux, which is not always sufficient.</span></span> <span data-ttu-id="01e06-200">A következő lépéseket tooincrease hello maximális párhuzamos leírókat hello rendszer toosupport magas egyidejűségi beállítása pedig MySQL hello használata.</span><span class="sxs-lookup"><span data-stu-id="01e06-200">Use hello following steps tooincrease hello maximum concurrent handles of hello system toosupport high concurrency of MySQL.</span></span>

### <a name="modify-hello-limitsconf-file"></a><span data-ttu-id="01e06-201">Hello limits.conf fájl módosítása</span><span class="sxs-lookup"><span data-stu-id="01e06-201">Modify hello limits.conf file</span></span>
<span data-ttu-id="01e06-202">tooincrease hello maximális engedélyezett egyidejű kezeli, vegye fel a következő négy hello /etc/security/limits.conf fájl sorainak hello.</span><span class="sxs-lookup"><span data-stu-id="01e06-202">tooincrease hello maximum allowed concurrent handles, add hello following four lines in hello /etc/security/limits.conf file.</span></span> <span data-ttu-id="01e06-203">Vegye figyelembe, hogy a 65536 értékű hello rendszer által támogatott hello maximális számát.</span><span class="sxs-lookup"><span data-stu-id="01e06-203">Note that 65536 is hello maximum number that hello system can support.</span></span>   

    * <span data-ttu-id="01e06-204">a 65536 értékű enyhe nofile</span><span class="sxs-lookup"><span data-stu-id="01e06-204">soft nofile 65536</span></span>
    * <span data-ttu-id="01e06-205">a 65536 értékű rögzített nofile</span><span class="sxs-lookup"><span data-stu-id="01e06-205">hard nofile 65536</span></span>
    * <span data-ttu-id="01e06-206">a 65536 értékű enyhe nproc</span><span class="sxs-lookup"><span data-stu-id="01e06-206">soft nproc 65536</span></span>
    * <span data-ttu-id="01e06-207">a 65536 értékű rögzített nproc</span><span class="sxs-lookup"><span data-stu-id="01e06-207">hard nproc 65536</span></span>

### <a name="update-hello-system-for-hello-new-limits"></a><span data-ttu-id="01e06-208">Hello rendszert hello új korlátok</span><span class="sxs-lookup"><span data-stu-id="01e06-208">Update hello system for hello new limits</span></span>
<span data-ttu-id="01e06-209">tooupdate hello rendszer hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="01e06-209">tooupdate hello system, run hello following commands:</span></span>  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-hello-limits-are-updated-at-boot-time"></a><span data-ttu-id="01e06-210">Győződjön meg arról, hogy hello korlátok rendszerindítás frissítése</span><span class="sxs-lookup"><span data-stu-id="01e06-210">Ensure that hello limits are updated at boot time</span></span>
<span data-ttu-id="01e06-211">Helyezze el a következő indítási parancsok hello /etc/rc.local fájlban, akkor lép érvénybe rendszerindítás hello.</span><span class="sxs-lookup"><span data-stu-id="01e06-211">Put hello following startup commands in hello /etc/rc.local file so it will take effect at boot time.</span></span>  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a><span data-ttu-id="01e06-212">MySQL-adatbázis optimalizálása</span><span class="sxs-lookup"><span data-stu-id="01e06-212">MySQL database optimization</span></span>
<span data-ttu-id="01e06-213">Azure-beli MySQL tooconfigure, használhatja az a helyi számítógépen, amelyekkel azonos teljesítményének hangolása stratégia hello.</span><span class="sxs-lookup"><span data-stu-id="01e06-213">tooconfigure MySQL on Azure, you can use hello same performance-tuning strategy you use on an on-premises machine.</span></span>  

<span data-ttu-id="01e06-214">hello fő i/o-optimalizálás szabályok a következők:</span><span class="sxs-lookup"><span data-stu-id="01e06-214">hello main I/O optimization rules are:</span></span>   

* <span data-ttu-id="01e06-215">Hello gyorsítótár méretének növelése.</span><span class="sxs-lookup"><span data-stu-id="01e06-215">Increase hello cache size.</span></span>
* <span data-ttu-id="01e06-216">I/o-válaszidő csökkentése.</span><span class="sxs-lookup"><span data-stu-id="01e06-216">Reduce I/O response time.</span></span>  

<span data-ttu-id="01e06-217">toooptimize MySQL-kiszolgáló beállításait, frissítheti hello my.cnf fájl, amely hello alapértelmezett konfigurációs fájl a kiszolgáló és az ügyfélszámítógépek számára.</span><span class="sxs-lookup"><span data-stu-id="01e06-217">toooptimize MySQL server settings, you can update hello my.cnf file, which is hello default configuration file for both server and client computers.</span></span>  

<span data-ttu-id="01e06-218">hello következő konfigurációs elemek olyan hello fő tényezőt jelent a MySQL teljesítmény:</span><span class="sxs-lookup"><span data-stu-id="01e06-218">hello following configuration items are hello main factors that affect MySQL performance:</span></span>  

* <span data-ttu-id="01e06-219">**innodb_buffer_pool_size**: hello pufferkészlet pufferelt adatok és hello indexet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="01e06-219">**innodb_buffer_pool_size**: hello buffer pool contains buffered data and hello index.</span></span> <span data-ttu-id="01e06-220">Általában a fizikai memória too70 százalékos értéke.</span><span class="sxs-lookup"><span data-stu-id="01e06-220">This is usually set too70 percent of physical memory.</span></span>
* <span data-ttu-id="01e06-221">**innodb_log_file_size**: hello visszaállítási napló mérete.</span><span class="sxs-lookup"><span data-stu-id="01e06-221">**innodb_log_file_size**: This is hello redo log size.</span></span> <span data-ttu-id="01e06-222">Ismét: naplók tooensure, hogy az írási műveletek gyors, megbízható és helyreállításhoz legyenek rendszerösszeomlás után használhatja.</span><span class="sxs-lookup"><span data-stu-id="01e06-222">You use redo logs tooensure that write operations are fast, reliable, and recoverable after a crash.</span></span> <span data-ttu-id="01e06-223">Adja meg elég hely a naplózás írási műveletek too512 MB értéke.</span><span class="sxs-lookup"><span data-stu-id="01e06-223">This is set too512 MB, which will give you plenty of space for logging write operations.</span></span>
* <span data-ttu-id="01e06-224">**max_connections**: néha alkalmazások ne zárja be kapcsolatok megfelelően.</span><span class="sxs-lookup"><span data-stu-id="01e06-224">**max_connections**: Sometimes applications do not close connections properly.</span></span> <span data-ttu-id="01e06-225">Nagyobb értéket ad hello server toorecycle idled kapcsolatok több időt.</span><span class="sxs-lookup"><span data-stu-id="01e06-225">A larger value will give hello server more time toorecycle idled connections.</span></span> <span data-ttu-id="01e06-226">hello kapcsolatok maximális száma 10 000-re, de hello ajánlott legfeljebb 5 000.</span><span class="sxs-lookup"><span data-stu-id="01e06-226">hello maximum number of connections is 10,000, but hello recommended maximum is 5,000.</span></span>
* <span data-ttu-id="01e06-227">**Innodb_file_per_table**: Ez a beállítás engedélyezi vagy letiltja a InnoDB toostore táblák külön fájlban hello képességét.</span><span class="sxs-lookup"><span data-stu-id="01e06-227">**Innodb_file_per_table**: This setting enables or disables hello ability of InnoDB toostore tables in separate files.</span></span> <span data-ttu-id="01e06-228">Kapcsolja be a hello beállítás tooensure, hogy számos speciális felügyeleti műveletek hatékonyan alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="01e06-228">Turn on hello option tooensure that several advanced administration operations can be applied efficiently.</span></span> <span data-ttu-id="01e06-229">A teljesítmény szempontjából az hello tábla terület átviteli felgyorsítása és hello tekintetében felügyeleti teljesítményének optimalizálásához.</span><span class="sxs-lookup"><span data-stu-id="01e06-229">From a performance point of view, it can speed up hello table space transmission and optimize hello debris management performance.</span></span> <span data-ttu-id="01e06-230">hello ajánlott beállítás be Kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="01e06-230">hello recommended setting for this option is ON.</span></span></br></br>
<span data-ttu-id="01e06-231">A MySQL 5.6 hello alapértelmezett beállítás be Kapcsolva, nincs teendője.</span><span class="sxs-lookup"><span data-stu-id="01e06-231">From MySQL 5.6, hello default setting is ON, so no action is required.</span></span> <span data-ttu-id="01e06-232">Korábbi verzióihoz hello alapértelmezett beállítás értéke OFF.</span><span class="sxs-lookup"><span data-stu-id="01e06-232">For earlier versions, hello default setting is OFF.</span></span> <span data-ttu-id="01e06-233">hello úgy kell módosítani, mielőtt adatok betöltése, mert az csak az újonnan létrehozott táblák is érint.</span><span class="sxs-lookup"><span data-stu-id="01e06-233">hello setting should be changed before data is loaded, because only newly created tables are affected.</span></span>
* <span data-ttu-id="01e06-234">**innodb_flush_log_at_trx_commit**: hello alapértelmezett értéke 1, a hello hatókör beállítása too0 ~ 2.</span><span class="sxs-lookup"><span data-stu-id="01e06-234">**innodb_flush_log_at_trx_commit**: hello default value is 1, with hello scope set too0~2.</span></span> <span data-ttu-id="01e06-235">hello alapértelmezett értéke különálló MySQL-adatbázis hello legmegfelelőbb beállítást.</span><span class="sxs-lookup"><span data-stu-id="01e06-235">hello default value is hello most suitable option for standalone MySQL DB.</span></span> <span data-ttu-id="01e06-236">hello 2 lehetővé teszi, hogy a legtöbb adatintegritás hello és MySQL-fürt főkiszolgálójának alkalmas.</span><span class="sxs-lookup"><span data-stu-id="01e06-236">hello setting of 2 enables hello most data integrity and is suitable for Master in MySQL Cluster.</span></span> <span data-ttu-id="01e06-237">hello 0 beállítással adatvesztés, ami hatással lehet a megbízhatóság (egyes esetekben nagyobb teljesítményű), és az alárendelt MySQL-fürt alkalmas.</span><span class="sxs-lookup"><span data-stu-id="01e06-237">hello setting of 0 allows data loss, which can affect reliability (in some cases with better performance), and is suitable for Slave in MySQL Cluster.</span></span>
* <span data-ttu-id="01e06-238">**Innodb_log_buffer_size**: hello napló puffer lehetővé teszi, hogy a tranzakciók toorun anélkül, hogy tooflush hello napló toodisk hello tranzakciók véglegesítés előtt.</span><span class="sxs-lookup"><span data-stu-id="01e06-238">**Innodb_log_buffer_size**: hello log buffer allows transactions toorun without having tooflush hello log toodisk before hello transactions commit.</span></span> <span data-ttu-id="01e06-239">Azonban ha nagy bináris objektum vagy szövegmezőt, gyorsan hello gyorsítótár fognak használni, és gyakori lemez i/o indul.</span><span class="sxs-lookup"><span data-stu-id="01e06-239">However, if there is large binary object or text field, hello cache will be consumed quickly and frequent disk I/O will be triggered.</span></span> <span data-ttu-id="01e06-240">Fontos, hogy jobban hello pufferméret növelése, ha Innodb_log_waits állapot változó nem 0.</span><span class="sxs-lookup"><span data-stu-id="01e06-240">It is better increase hello buffer size if Innodb_log_waits state variable is not 0.</span></span>
* <span data-ttu-id="01e06-241">**query_cache_size**: hello legjobb lehetőség toodisable hello kezdettől azt.</span><span class="sxs-lookup"><span data-stu-id="01e06-241">**query_cache_size**: hello best option is toodisable it from hello outset.</span></span> <span data-ttu-id="01e06-242">(Ez az hello alapértelmezett beállítása a MySQL 5.6) query_cache_size too0, és fel a lekérdezést más módszerek toospeed használ.</span><span class="sxs-lookup"><span data-stu-id="01e06-242">Set query_cache_size too0 (this is hello default setting in MySQL 5.6) and use other methods toospeed up queries.</span></span>  

<span data-ttu-id="01e06-243">Lásd: [D függelék](#AppendixD) előtt és után hello optimalizálási teljesítmény összehasonlítása.</span><span class="sxs-lookup"><span data-stu-id="01e06-243">See [Appendix D](#AppendixD) for a comparison of performance before and after hello optimization.</span></span>

## <a name="turn-on-hello-mysql-slow-query-log-for-analyzing-hello-performance-bottleneck"></a><span data-ttu-id="01e06-244">Hello MySQL lassú lekérdezés napló hello teljesítménybeli szűk keresztmetszetek elemzéséhez bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="01e06-244">Turn on hello MySQL slow query log for analyzing hello performance bottleneck</span></span>
<span data-ttu-id="01e06-245">hello MySQL lassú lekérdezés naplók segíthetnek a MySQL hello lassú lekérdezések azonosítja.</span><span class="sxs-lookup"><span data-stu-id="01e06-245">hello MySQL slow query log can help you identify hello slow queries for MySQL.</span></span> <span data-ttu-id="01e06-246">Miután engedélyezte a hello MySQL lassú lekérdezés napló, használhatja a MySQL-eszközök például **mysqldumpslow** tooidentify hello teljesítménybeli szűk keresztmetszetek.</span><span class="sxs-lookup"><span data-stu-id="01e06-246">After enabling hello MySQL slow query log, you can use MySQL tools like **mysqldumpslow** tooidentify hello performance bottleneck.</span></span>  

<span data-ttu-id="01e06-247">Alapértelmezés szerint ez nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="01e06-247">By default, this is not enabled.</span></span> <span data-ttu-id="01e06-248">Hello lassú lekérdezés napló bekapcsolása, előfordulhat, hogy néhány Processzor-erőforrások felhasználását.</span><span class="sxs-lookup"><span data-stu-id="01e06-248">Turning on hello slow query log might consume some CPU resources.</span></span> <span data-ttu-id="01e06-249">Javasoljuk, hogy engedélyezze a ideiglenesen szűk keresztmetszetek hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="01e06-249">We recommend that you enable this temporarily for troubleshooting performance bottlenecks.</span></span> <span data-ttu-id="01e06-250">a hello lassú lekérdezés napló tooturn:</span><span class="sxs-lookup"><span data-stu-id="01e06-250">tooturn on hello slow query log:</span></span>

1. <span data-ttu-id="01e06-251">Módosítsa a hello my.cnf fájlt adja hozzá a következő sorokat toohello end hello:</span><span class="sxs-lookup"><span data-stu-id="01e06-251">Modify hello my.cnf file by adding hello following lines toohello end:</span></span>

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. <span data-ttu-id="01e06-252">Indítsa újra a hello MySQL-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="01e06-252">Restart hello MySQL server.</span></span>

        service  mysql  restart

3. <span data-ttu-id="01e06-253">Ellenőrizze, hogy hello beállítás érvénybe tart hello segítségével **megjelenítése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="01e06-253">Check whether hello setting is taking effect by using hello **show** command.</span></span>

![Lassú lekérdezés-log ON][7]   

![Lassú lekérdezés-napló eredmények][8]

<span data-ttu-id="01e06-256">Ebben a példában láthatja, lassú lekérdezés hello szolgáltatást be van kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="01e06-256">In this example, you can see that hello slow query feature has been turned on.</span></span> <span data-ttu-id="01e06-257">Hello segítségével **mysqldumpslow** toodetermine szűk keresztmetszetek eszközt, és a teljesítmény, mint indexek optimalizálása érdekében.</span><span class="sxs-lookup"><span data-stu-id="01e06-257">You can then use hello **mysqldumpslow** tool toodetermine performance bottlenecks and optimize performance, such as adding indexes.</span></span>

## <a name="appendices"></a><span data-ttu-id="01e06-258">Mellékletek</span><span class="sxs-lookup"><span data-stu-id="01e06-258">Appendices</span></span>
<span data-ttu-id="01e06-259">Az alábbiakban hello minta teszt teljesítményadatok előállított célzott laborkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="01e06-259">hello following are sample performance test data produced in a targeted lab environment.</span></span> <span data-ttu-id="01e06-260">Hangolási módszerek különböző teljesítménnyel adathordozóira hello teljesítmény adatok trend általános.</span><span class="sxs-lookup"><span data-stu-id="01e06-260">They provide general background on hello performance data trend with different performance tuning approaches.</span></span> <span data-ttu-id="01e06-261">a környezet vagy a termék különböző verziói hello eredmények függően változhat.</span><span class="sxs-lookup"><span data-stu-id="01e06-261">hello results might vary under different environment or product versions.</span></span>

### <span data-ttu-id="01e06-262"><a name="AppendixA"></a>A függelék</span><span class="sxs-lookup"><span data-stu-id="01e06-262"><a name="AppendixA"></a>Appendix A</span></span>  
<span data-ttu-id="01e06-263">**Lemez teljesítménye (IOPS) a különböző RAID-szintek**</span><span class="sxs-lookup"><span data-stu-id="01e06-263">**Disk performance (IOPS) with different RAID levels**</span></span>

![Különböző RAID-szintek IOPS lemez][9]

<span data-ttu-id="01e06-265">**Teszt parancsok**</span><span class="sxs-lookup"><span data-stu-id="01e06-265">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> <span data-ttu-id="01e06-266">Ebben a tesztben hello munkaterhelés 64 szál tooreach hello felső korlátja RAID közben használ.</span><span class="sxs-lookup"><span data-stu-id="01e06-266">hello workload of this test uses 64 threads, trying tooreach hello upper limit of RAID.</span></span>
>
>

### <span data-ttu-id="01e06-267"><a name="AppendixB"></a>B függelék</span><span class="sxs-lookup"><span data-stu-id="01e06-267"><a name="AppendixB"></a>Appendix B</span></span>  
<span data-ttu-id="01e06-268">**Különböző RAID-szintek MySQL (teljesítmény) teljesítmény összehasonlítása** </span><span class="sxs-lookup"><span data-stu-id="01e06-268">**MySQL performance (throughput) comparison with different RAID levels** </span></span>  
<span data-ttu-id="01e06-269">(XFS fájlrendszer)</span><span class="sxs-lookup"><span data-stu-id="01e06-269">(XFS file system)</span></span>

![Különböző RAID-szintek MySQL teljesítmény összehasonlítása][10]  
![Különböző RAID-szintek MySQL teljesítmény összehasonlítása][11]

<span data-ttu-id="01e06-272">**Teszt parancsok**</span><span class="sxs-lookup"><span data-stu-id="01e06-272">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

<span data-ttu-id="01e06-273">**Különböző RAID-szintek MySQL (OLTP) teljesítmény összehasonlítása**</span><span class="sxs-lookup"><span data-stu-id="01e06-273">**MySQL performance (OLTP) comparison with different RAID levels**</span></span>  
<span data-ttu-id="01e06-274">![Különböző RAID-szintek MySQL (OLTP) teljesítmény összehasonlítása][12]</span><span class="sxs-lookup"><span data-stu-id="01e06-274">![MySQL performance (OLTP) comparison with different RAID levels][12]</span></span>

<span data-ttu-id="01e06-275">**Teszt parancsok**</span><span class="sxs-lookup"><span data-stu-id="01e06-275">**Test commands**</span></span>

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <span data-ttu-id="01e06-276"><a name="AppendixC"></a>C függelék</span><span class="sxs-lookup"><span data-stu-id="01e06-276"><a name="AppendixC"></a>Appendix C</span></span>   
<span data-ttu-id="01e06-277">**A különböző adatrészlet mérete a lemez teljesítménye (IOPS) összehasonlítását**</span><span class="sxs-lookup"><span data-stu-id="01e06-277">**Disk performance (IOPS) comparison for different chunk sizes**</span></span>  
<span data-ttu-id="01e06-278">(XFS fájlrendszer)</span><span class="sxs-lookup"><span data-stu-id="01e06-278">(XFS file system)</span></span>

![][13]

<span data-ttu-id="01e06-279">**Teszt parancsok**</span><span class="sxs-lookup"><span data-stu-id="01e06-279">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

<span data-ttu-id="01e06-280">a teszteléshez használt hello fájlméret 30 és 1 GB-os, rendre, XFS fájlrendszer a RAID 0 (4 lemezek).</span><span class="sxs-lookup"><span data-stu-id="01e06-280">hello file sizes used for this testing are 30 GB and 1 GB, respectively, with RAID 0 (4 disks) XFS file system.</span></span>

### <span data-ttu-id="01e06-281"><a name="AppendixD"></a>D függelék:</span><span class="sxs-lookup"><span data-stu-id="01e06-281"><a name="AppendixD"></a>Appendix D</span></span>  
<span data-ttu-id="01e06-282">**MySQL teljesítmény (teljesítmény) összehasonlítás előtt és után optimalizálása**</span><span class="sxs-lookup"><span data-stu-id="01e06-282">**MySQL performance (throughput) comparison before and after optimization**</span></span>  
<span data-ttu-id="01e06-283">(XFS fájlrendszer)</span><span class="sxs-lookup"><span data-stu-id="01e06-283">(XFS File System)</span></span>

![MySQL teljesítmény (teljesítmény) összehasonlítás előtt és után optimalizálása][14]

<span data-ttu-id="01e06-285">**Teszt parancsok**</span><span class="sxs-lookup"><span data-stu-id="01e06-285">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

<span data-ttu-id="01e06-286">**hello konfigurációs beállítás az alapértelmezett és optimalizálás a következőképpen történik:**</span><span class="sxs-lookup"><span data-stu-id="01e06-286">**hello configuration setting for default and optimization is as follows:**</span></span>

| <span data-ttu-id="01e06-287">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="01e06-287">Parameters</span></span> | <span data-ttu-id="01e06-288">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="01e06-288">Default</span></span> | <span data-ttu-id="01e06-289">Optimalizálás</span><span class="sxs-lookup"><span data-stu-id="01e06-289">Optimization</span></span> |
| --- | --- | --- |
| <span data-ttu-id="01e06-290">**innodb_buffer_pool_size**</span><span class="sxs-lookup"><span data-stu-id="01e06-290">**innodb_buffer_pool_size**</span></span> |<span data-ttu-id="01e06-291">None</span><span class="sxs-lookup"><span data-stu-id="01e06-291">None</span></span> |<span data-ttu-id="01e06-292">7 GB</span><span class="sxs-lookup"><span data-stu-id="01e06-292">7 GB</span></span> |
| <span data-ttu-id="01e06-293">**innodb_log_file_size**</span><span class="sxs-lookup"><span data-stu-id="01e06-293">**innodb_log_file_size**</span></span> |<span data-ttu-id="01e06-294">5 MB</span><span class="sxs-lookup"><span data-stu-id="01e06-294">5 MB</span></span> |<span data-ttu-id="01e06-295">512 MB</span><span class="sxs-lookup"><span data-stu-id="01e06-295">512 MB</span></span> |
| <span data-ttu-id="01e06-296">**max_connections**</span><span class="sxs-lookup"><span data-stu-id="01e06-296">**max_connections**</span></span> |<span data-ttu-id="01e06-297">100</span><span class="sxs-lookup"><span data-stu-id="01e06-297">100</span></span> |<span data-ttu-id="01e06-298">5000</span><span class="sxs-lookup"><span data-stu-id="01e06-298">5000</span></span> |
| <span data-ttu-id="01e06-299">**innodb_file_per_table**</span><span class="sxs-lookup"><span data-stu-id="01e06-299">**innodb_file_per_table**</span></span> |<span data-ttu-id="01e06-300">0</span><span class="sxs-lookup"><span data-stu-id="01e06-300">0</span></span> |<span data-ttu-id="01e06-301">1</span><span class="sxs-lookup"><span data-stu-id="01e06-301">1</span></span> |
| <span data-ttu-id="01e06-302">**innodb_flush_log_at_trx_commit**</span><span class="sxs-lookup"><span data-stu-id="01e06-302">**innodb_flush_log_at_trx_commit**</span></span> |<span data-ttu-id="01e06-303">1</span><span class="sxs-lookup"><span data-stu-id="01e06-303">1</span></span> |<span data-ttu-id="01e06-304">2</span><span class="sxs-lookup"><span data-stu-id="01e06-304">2</span></span> |
| <span data-ttu-id="01e06-305">**innodb_log_buffer_size**</span><span class="sxs-lookup"><span data-stu-id="01e06-305">**innodb_log_buffer_size**</span></span> |<span data-ttu-id="01e06-306">8 MB</span><span class="sxs-lookup"><span data-stu-id="01e06-306">8 MB</span></span> |<span data-ttu-id="01e06-307">128 MB</span><span class="sxs-lookup"><span data-stu-id="01e06-307">128 MB</span></span> |
| <span data-ttu-id="01e06-308">**query_cache_size**</span><span class="sxs-lookup"><span data-stu-id="01e06-308">**query_cache_size**</span></span> |<span data-ttu-id="01e06-309">16 MB</span><span class="sxs-lookup"><span data-stu-id="01e06-309">16 MB</span></span> |<span data-ttu-id="01e06-310">0</span><span class="sxs-lookup"><span data-stu-id="01e06-310">0</span></span> |

<span data-ttu-id="01e06-311">További részletes [optimalizálási konfigurációs paraméterek](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), tekintse meg a toohello [MySQL hivatalos utasításokat](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span><span class="sxs-lookup"><span data-stu-id="01e06-311">For more detailed [optimization configuration parameters](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), refer toohello [MySQL official instructions](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span></span>  

  <span data-ttu-id="01e06-312">**Tesztkörnyezet**</span><span class="sxs-lookup"><span data-stu-id="01e06-312">**Test environment**</span></span>  

| <span data-ttu-id="01e06-313">Hardver</span><span class="sxs-lookup"><span data-stu-id="01e06-313">Hardware</span></span> | <span data-ttu-id="01e06-314">Részletek</span><span class="sxs-lookup"><span data-stu-id="01e06-314">Details</span></span> |
| --- | --- |
| <span data-ttu-id="01e06-315">CPU</span><span class="sxs-lookup"><span data-stu-id="01e06-315">CPU</span></span> |<span data-ttu-id="01e06-316">AMD Opteron(tm) processzor 4171 Helykiszolgálójához / 4 magos</span><span class="sxs-lookup"><span data-stu-id="01e06-316">AMD Opteron(tm) Processor 4171 HE/4 cores</span></span> |
| <span data-ttu-id="01e06-317">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="01e06-317">Memory</span></span> |<span data-ttu-id="01e06-318">14 GB</span><span class="sxs-lookup"><span data-stu-id="01e06-318">14 GB</span></span> |
| <span data-ttu-id="01e06-319">Lemez</span><span class="sxs-lookup"><span data-stu-id="01e06-319">Disk</span></span> |<span data-ttu-id="01e06-320">10 GB/lemez</span><span class="sxs-lookup"><span data-stu-id="01e06-320">10 GB/disk</span></span> |
| <span data-ttu-id="01e06-321">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="01e06-321">OS</span></span> |<span data-ttu-id="01e06-322">Ubuntu 14.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="01e06-322">Ubuntu 14.04.1 LTS</span></span> |

[1]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-01.png
[2]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-02.png
[3]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-03.png
[4]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-04.png
[5]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-05.png
[6]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-06.png
[7]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-07.png
[8]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-08.png
[9]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-09.png
[10]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-10.png
[11]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-11.png
[12]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-12.png
[13]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-13.png
[14]:media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-14.png

