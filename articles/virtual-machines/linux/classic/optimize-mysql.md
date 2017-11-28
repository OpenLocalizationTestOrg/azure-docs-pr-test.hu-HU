---
title: "Linux MySQL teljesítményének optimalizálásához |} Microsoft Docs"
description: "Ismerje meg, hogyan optimalizálható a Linux operációs rendszert futtató Azure virtuális géphez (VM) futtató MySQL."
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
ms.openlocfilehash: 8f2ec884fa98e989448ac11675e71f39aa21fa7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="optimize-mysql-performance-on-azure-linux-vms"></a><span data-ttu-id="346d0-103">Az Azure Linux virtuális gépeken futó MySQL teljesítményének optimalizálása</span><span class="sxs-lookup"><span data-stu-id="346d0-103">Optimize MySQL Performance on Azure Linux VMs</span></span>
<span data-ttu-id="346d0-104">Nincsenek számos tényező befolyásolja az Azure, mind a virtuális hardver kiválasztása és szoftverkonfigurációt MySQL teljesítményére.</span><span class="sxs-lookup"><span data-stu-id="346d0-104">There are many factors that affect MySQL performance on Azure, both in virtual hardware selection and software configuration.</span></span> <span data-ttu-id="346d0-105">Ez a cikk foglalkozik, a tárolás, a rendszer és a Helyadatbázis-konfigurációk keresztül optimalizálás teljesítményét.</span><span class="sxs-lookup"><span data-stu-id="346d0-105">This article focuses on optimizing performance through storage, system, and database configurations.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="346d0-106">Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Azure Resource Manager](../../../resource-manager-deployment-model.md) és klasszikus.</span><span class="sxs-lookup"><span data-stu-id="346d0-106">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager](../../../resource-manager-deployment-model.md) and classic.</span></span> <span data-ttu-id="346d0-107">Ez a cikk a klasszikus üzembehelyezési modellt ismerteti.</span><span class="sxs-lookup"><span data-stu-id="346d0-107">This article covers using the classic deployment model.</span></span> <span data-ttu-id="346d0-108">A Microsoft azt javasolja, hogy az új telepítések esetén a Resource Manager modellt használja.</span><span class="sxs-lookup"><span data-stu-id="346d0-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="346d0-109">További információ a Linux virtuális gép optimalizálás a Resource Manager modellt: [optimalizálhatja a Linux virtuális Gépet az Azure-on](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="346d0-109">For information about Linux VM optimizations with the Resource Manager model, see [Optimize your Linux VM on Azure](../optimization.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="utilize-raid-on-an-azure-virtual-machine"></a><span data-ttu-id="346d0-110">Egy Azure virtuális gépen RAID használata</span><span class="sxs-lookup"><span data-stu-id="346d0-110">Utilize RAID on an Azure virtual machine</span></span>
<span data-ttu-id="346d0-111">Tároló, a kulcsfontosságú tényező, amely befolyásolja az adatbázis teljesítményének felhőalapú környezetben.</span><span class="sxs-lookup"><span data-stu-id="346d0-111">Storage is the key factor that affects database performance in cloud environments.</span></span> <span data-ttu-id="346d0-112">Képest egyetlen lemez, RAID keresztül párhuzamossági gyorsabb hozzáférést biztosíthat.</span><span class="sxs-lookup"><span data-stu-id="346d0-112">Compared to a single disk, RAID can provide faster access via concurrency.</span></span> <span data-ttu-id="346d0-113">További információkért lásd: [szabványos RAID-szintek](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span><span class="sxs-lookup"><span data-stu-id="346d0-113">For more information, see [Standard RAID levels](http://en.wikipedia.org/wiki/Standard_RAID_levels).</span></span>   

<span data-ttu-id="346d0-114">Lemezes i/o-átviteli és az Azure-ban i/o-válaszidő RAID javítható ki.</span><span class="sxs-lookup"><span data-stu-id="346d0-114">Disk I/O throughput and I/O response time in Azure can be improved through RAID.</span></span> <span data-ttu-id="346d0-115">Labor tesztek megjelenítése, hogy a lemez i/o-átviteli meg kell duplázni és i/o-válaszidő csökkenthető félig átlagosan RAID lemezek száma nő (a két négy, négy és nyolc, stb.) Ha.</span><span class="sxs-lookup"><span data-stu-id="346d0-115">Our lab tests show that disk I/O throughput can be doubled and I/O response time can be reduced by half on average when the number of RAID disks is doubled (from two to four, four to eight, etc.).</span></span> <span data-ttu-id="346d0-116">Lásd: [függelék](#AppendixA) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="346d0-116">See [Appendix A](#AppendixA) for details.</span></span>  

<span data-ttu-id="346d0-117">Lemezes i/o, mellett MySQL teljesítmény javítja, abban az esetben, amikor növeli a RAID szintjét.</span><span class="sxs-lookup"><span data-stu-id="346d0-117">In addition to disk I/O, MySQL performance improves when you increase the RAID level.</span></span>  <span data-ttu-id="346d0-118">Lásd: [B függelék](#AppendixB) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="346d0-118">See [Appendix B](#AppendixB) for details.</span></span>  

<span data-ttu-id="346d0-119">Érdemes figyelembe venni az adatrészlet méretének is.</span><span class="sxs-lookup"><span data-stu-id="346d0-119">You might also want to consider the chunk size.</span></span> <span data-ttu-id="346d0-120">Általában még nagyobb adattömbméretet, nyílik meg alsó terhelés, különösen a nagy írási műveleteket.</span><span class="sxs-lookup"><span data-stu-id="346d0-120">In general, when you have a larger chunk size, you get lower overhead, especially for large writes.</span></span> <span data-ttu-id="346d0-121">Azonban ha adatrészlet mérete túl nagy, adhat, amely megakadályozza, hogy kihasználja a RAID többletterhelést.</span><span class="sxs-lookup"><span data-stu-id="346d0-121">However, when the chunk size is too large, it might add additional overhead that prevents you from taking advantage of RAID.</span></span> <span data-ttu-id="346d0-122">A jelenlegi alapértelmezett méret 512 KB, ami a legáltalánosabb éles környezetek optimális bizonyult.</span><span class="sxs-lookup"><span data-stu-id="346d0-122">The current default size is 512 KB, which is proven to be optimal for most general production environments.</span></span> <span data-ttu-id="346d0-123">Lásd: [C függelék](#AppendixC) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="346d0-123">See [Appendix C](#AppendixC) for details.</span></span>   

<span data-ttu-id="346d0-124">Nincsenek korlátozások is hozzáadhat más virtuális gépek típusainak hány lemezeken.</span><span class="sxs-lookup"><span data-stu-id="346d0-124">There are limits on how many disks you can add for different virtual machine types.</span></span> <span data-ttu-id="346d0-125">Ezek a korlátozások részletes leírást talál [virtuális gép és felhő mérete](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="346d0-125">These limits are detailed in [Virtual machine and cloud service sizes for Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span> <span data-ttu-id="346d0-126">Kövesse az ebben a cikkben RAID példa négy csatolt adatlemezek szüksége lesz, bár esetén dönthet úgy állítsa be a RAID kevesebb lemezt.</span><span class="sxs-lookup"><span data-stu-id="346d0-126">You will need four attached data disks to follow the RAID example in this article, although you can choose to set up RAID with fewer disks.</span></span>  

<span data-ttu-id="346d0-127">Ez a cikk azt feltételezi, hogy már létrehozott egy Linux virtuális gép és a MYSQL telepítette és konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="346d0-127">This article assumes you have already created a Linux virtual machine and have MYSQL installed and configured.</span></span> <span data-ttu-id="346d0-128">A bevezetés további információkért lásd: MySQL telepítése az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="346d0-128">For more information on getting started, see How to install MySQL on Azure.</span></span>  

### <a name="set-up-raid-on-azure"></a><span data-ttu-id="346d0-129">Az Azure-on RAID beállítása</span><span class="sxs-lookup"><span data-stu-id="346d0-129">Set up RAID on Azure</span></span>
<span data-ttu-id="346d0-130">A következő lépések bemutatják, hogyan Azure RAID létrehozása az Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="346d0-130">The following steps show how to create RAID on Azure by using the Azure portal.</span></span> <span data-ttu-id="346d0-131">Is állíthatja be RAID Windows PowerShell-parancsfájlok használatával.</span><span class="sxs-lookup"><span data-stu-id="346d0-131">You can also set up RAID by using Windows PowerShell scripts.</span></span>
<span data-ttu-id="346d0-132">Ebben a példában azt konfigurálja RAID 0 négy lemezzel.</span><span class="sxs-lookup"><span data-stu-id="346d0-132">In this example, we will configure RAID 0 with four disks.</span></span>  

#### <a name="add-a-data-disk-to-your-virtual-machine"></a><span data-ttu-id="346d0-133">Adatlemez hozzáadása a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="346d0-133">Add a data disk to your virtual machine</span></span>
<span data-ttu-id="346d0-134">Az Azure portálon az irányítópult megnyitásához, és válassza ki a kívánt adatok lemezt szeretne felvenni a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="346d0-134">In the Azure portal, go to the dashboard and select the virtual machine to which you want to add a data disk.</span></span> <span data-ttu-id="346d0-135">Ebben a példában a virtuális gép mysqlnode1.</span><span class="sxs-lookup"><span data-stu-id="346d0-135">In this example, the virtual machine is mysqlnode1.</span></span>  

<!--![Virtual machines][1]-->

<span data-ttu-id="346d0-136">Kattintson a **lemezek** majd **új csatolása**.</span><span class="sxs-lookup"><span data-stu-id="346d0-136">Click **Disks** and then click **Attach New**.</span></span>

![Lemez hozzáadása a virtuális gépek](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-Disks-option.png)

<span data-ttu-id="346d0-138">Hozzon létre egy új 500 GB lemezterület.</span><span class="sxs-lookup"><span data-stu-id="346d0-138">Create a new 500 GB disk.</span></span> <span data-ttu-id="346d0-139">Győződjön meg arról, hogy **állomás gyorsítótár preferencia** értéke **nincs**.</span><span class="sxs-lookup"><span data-stu-id="346d0-139">Make sure that **Host Cache Preference** is set to **None**.</span></span>  <span data-ttu-id="346d0-140">Amikor végzett, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="346d0-140">When you're finished, click **OK**.</span></span>

![Üres lemez csatolása](media/optimize-mysql/virtual-machines-linux-optimize-mysql-perf-attach-empty-disk.png)


<span data-ttu-id="346d0-142">Ez hozzáadja egy üres lemez a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="346d0-142">This adds one empty disk into your virtual machine.</span></span> <span data-ttu-id="346d0-143">Ismételje meg ezt a három még többször, hogy négy adatlemezek a RAID.</span><span class="sxs-lookup"><span data-stu-id="346d0-143">Repeat this step three more times so that you have four data disks for RAID.</span></span>  

<span data-ttu-id="346d0-144">A hozzáadott meghajtó van a virtuális gép a kernel üzenet napló megnézzük látható.</span><span class="sxs-lookup"><span data-stu-id="346d0-144">You can see the added drives in the virtual machine by looking at the kernel message log.</span></span> <span data-ttu-id="346d0-145">Például ez az Ubuntu, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="346d0-145">For example, to see this on Ubuntu, use the following command:</span></span>  

    sudo grep SCSI /var/log/dmesg

#### <a name="create-raid-with-the-additional-disks"></a><span data-ttu-id="346d0-146">A további lemezek RAID létrehozása</span><span class="sxs-lookup"><span data-stu-id="346d0-146">Create RAID with the additional disks</span></span>
<span data-ttu-id="346d0-147">A következő lépések bemutatják, hogyan [szoftveres RAID Linux konfigurálása](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="346d0-147">The following steps describe how to [configure software RAID on Linux](../configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

> [!NOTE]
> <span data-ttu-id="346d0-148">Ha XFS fájlrendszert használ, RAID létrehozása után hajtható végre az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="346d0-148">If you are using the XFS file system, execute the following steps after you have created RAID.</span></span>
>
>

<span data-ttu-id="346d0-149">Debian, Ubuntu vagy Linux menta XFS telepítéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="346d0-149">To install XFS on Debian, Ubuntu, or Linux Mint, use the following command:</span></span>  

    apt-get -y install xfsprogs  

<span data-ttu-id="346d0-150">A Fedora, a CentOS, vagy az RHEL XFS telepítéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="346d0-150">To install XFS on Fedora, CentOS, or RHEL, use the following command:</span></span>  

    yum -y install xfsprogs  xfsdump


#### <a name="set-up-a-new-storage-path"></a><span data-ttu-id="346d0-151">Állítson be egy új. tárolási elérési útja</span><span class="sxs-lookup"><span data-stu-id="346d0-151">Set up a new storage path</span></span>
<span data-ttu-id="346d0-152">Az alábbi parancs segítségével állítson be egy új. tárolási elérési útja:</span><span class="sxs-lookup"><span data-stu-id="346d0-152">Use the following command to set up a new storage path:</span></span>  

    root@mysqlnode1:~# mkdir -p /RAID0/mysql

#### <a name="copy-the-original-data-to-the-new-storage-path"></a><span data-ttu-id="346d0-153">Az eredeti adatok másolása az új tárolási elérési útja</span><span class="sxs-lookup"><span data-stu-id="346d0-153">Copy the original data to the new storage path</span></span>
<span data-ttu-id="346d0-154">A következő paranccsal adatok másolása az új tárolási elérési útja:</span><span class="sxs-lookup"><span data-stu-id="346d0-154">Use the following command to copy data to the new storage path:</span></span>  

    root@mysqlnode1:~# cp -rp /var/lib/mysql/* /RAID0/mysql/

#### <a name="modify-permissions-so-mysql-can-access-read-and-write-the-data-disk"></a><span data-ttu-id="346d0-155">Módosítsa az engedélyeket, MySQL hozzáférhet (olvasási és írási) az adatok lemezre</span><span class="sxs-lookup"><span data-stu-id="346d0-155">Modify permissions so MySQL can access (read and write) the data disk</span></span>
<span data-ttu-id="346d0-156">A következő paranccsal módosítsa az engedélyeket:</span><span class="sxs-lookup"><span data-stu-id="346d0-156">Use the following command to modify permissions:</span></span>  

    root@mysqlnode1:~# chown -R mysql.mysql /RAID0/mysql && chmod -R 755 /RAID0/mysql


## <a name="adjust-the-disk-io-scheduling-algorithm"></a><span data-ttu-id="346d0-157">A lemez i/o-ütemezési algoritmus beállítása</span><span class="sxs-lookup"><span data-stu-id="346d0-157">Adjust the disk I/O scheduling algorithm</span></span>
<span data-ttu-id="346d0-158">Linux megvalósítja négy különböző ütemezése algoritmusok i/o:</span><span class="sxs-lookup"><span data-stu-id="346d0-158">Linux implements four types of I/O scheduling algorithms:</span></span>  

* <span data-ttu-id="346d0-159">NOOP algoritmus (nincs művelet)</span><span class="sxs-lookup"><span data-stu-id="346d0-159">NOOP algorithm (No Operation)</span></span>
* <span data-ttu-id="346d0-160">Határidő algoritmus (határidő)</span><span class="sxs-lookup"><span data-stu-id="346d0-160">Deadline algorithm (Deadline)</span></span>
* <span data-ttu-id="346d0-161">Teljesen valós Üzenetsor-kezelés algoritmus (CFQ)</span><span class="sxs-lookup"><span data-stu-id="346d0-161">Completely fair queuing algorithm (CFQ)</span></span>
* <span data-ttu-id="346d0-162">Keret időszak algoritmus (Anticipatory)</span><span class="sxs-lookup"><span data-stu-id="346d0-162">Budget period algorithm (Anticipatory)</span></span>  

<span data-ttu-id="346d0-163">A teljesítmény optimalizálása különböző i/o-bejegyzéstípusait különböző forgatókönyvek alapján választhatja ki.</span><span class="sxs-lookup"><span data-stu-id="346d0-163">You can select different I/O schedulers under different scenarios to optimize performance.</span></span> <span data-ttu-id="346d0-164">Teljesen elérésű környezetben nincs jelentős különbség a teljesítmény CFQ és a határidő algoritmusok között.</span><span class="sxs-lookup"><span data-stu-id="346d0-164">In a completely random access environment, there is not a significant difference between the CFQ and Deadline algorithms for performance.</span></span> <span data-ttu-id="346d0-165">Azt javasoljuk, hogy beállította a MySQL-adatbázis környezet stabilitását határideje.</span><span class="sxs-lookup"><span data-stu-id="346d0-165">We recommend that you set the MySQL database environment to Deadline for stability.</span></span> <span data-ttu-id="346d0-166">Ha nagy mennyiségű szekvenciális i/o, CFQ csökkentheti a lemezek i/o műveleteinek teljesítménye.</span><span class="sxs-lookup"><span data-stu-id="346d0-166">If there is a lot of sequential I/O, CFQ might reduce disk I/O performance.</span></span>   

<span data-ttu-id="346d0-167">Az SSD és egyéb eszközökről NOOP vagy határidő érhető el az alapértelmezett ütemező jobb teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="346d0-167">For SSD and other equipment, NOOP or Deadline can achieve better performance than the default scheduler.</span></span>   

<span data-ttu-id="346d0-168">A kernel 2.5, mielőtt az alapértelmezett ütemezés i/o-algoritmus határidő.</span><span class="sxs-lookup"><span data-stu-id="346d0-168">Prior to the kernel 2.5, the default I/O scheduling algorithm is Deadline.</span></span> <span data-ttu-id="346d0-169">A kernel 2.6.18 verziótól kezdődően CFQ vált az alapértelmezett ütemezés i/o-algoritmus.</span><span class="sxs-lookup"><span data-stu-id="346d0-169">Starting with the kernel 2.6.18, CFQ became the default I/O scheduling algorithm.</span></span>  <span data-ttu-id="346d0-170">Adja meg ezt a beállítást, kernel rendszerindítás közben, vagy dinamikusan módosíthatja ezt a beállítást, ha a rendszer.</span><span class="sxs-lookup"><span data-stu-id="346d0-170">You can specify this setting at kernel boot time or dynamically modify this setting when the system is running.</span></span>  

<span data-ttu-id="346d0-171">A következő példa bemutatja, hogyan ellenőrizze, és állítsa be az alapértelmezett ütemező a Debian terjesztési termékcsalád NOOP algoritmus.</span><span class="sxs-lookup"><span data-stu-id="346d0-171">The following example demonstrates how to check and set the default scheduler to the NOOP algorithm in the Debian distribution family.</span></span>  

### <a name="view-the-current-io-scheduler"></a><span data-ttu-id="346d0-172">Az aktuális i/o-ütemező megtekintése</span><span class="sxs-lookup"><span data-stu-id="346d0-172">View the current I/O scheduler</span></span>
<span data-ttu-id="346d0-173">A következő parancsot az ütemező megtekintése:</span><span class="sxs-lookup"><span data-stu-id="346d0-173">To view the scheduler run the following command:</span></span>  

    root@mysqlnode1:~# cat /sys/block/sda/queue/scheduler

<span data-ttu-id="346d0-174">Kimeneti, amely megadja, hogy a jelenlegi Feladatütemező következő jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="346d0-174">You will see following output, which indicates the current scheduler:</span></span>  

    noop [deadline] cfq


### <a name="change-the-current-device-devsda-of-the-io-scheduling-algorithm"></a><span data-ttu-id="346d0-175">Az aktuális eszköz (/ dev/sda) ütemezési i/o-algoritmus módosítása</span><span class="sxs-lookup"><span data-stu-id="346d0-175">Change the current device (/dev/sda) of the I/O scheduling algorithm</span></span>
<span data-ttu-id="346d0-176">A következő parancsokat az aktuális eszköz módosításához:</span><span class="sxs-lookup"><span data-stu-id="346d0-176">Run the following commands to change the current device:</span></span>  

    azureuser@mysqlnode1:~$ sudo su -
    root@mysqlnode1:~# echo "noop" >/sys/block/sda/queue/scheduler
    root@mysqlnode1:~# sed -i 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX_DEFAULT="quiet splash elevator=noop"/g' /etc/default/grub
    root@mysqlnode1:~# update-grub

> [!NOTE]
> <span data-ttu-id="346d0-177">Beállítása ez /dev/sda csak akkor nem hasznos.</span><span class="sxs-lookup"><span data-stu-id="346d0-177">Setting this for /dev/sda alone is not useful.</span></span> <span data-ttu-id="346d0-178">Kell beállítani az összes adat lemez ahol az adatbázis található.</span><span class="sxs-lookup"><span data-stu-id="346d0-178">It must be set on all data disks where the database resides.</span></span>  
>
>

<span data-ttu-id="346d0-179">A következő kimeneti, jelezve, hogy grub.cfg sikeresen újraépítették, és, hogy az alapértelmezett ütemező környezetébe NOOP kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="346d0-179">You should see the following output, indicating that grub.cfg has been rebuilt successfully and that the default scheduler has been updated to NOOP:</span></span>  

    Generating grub configuration file ...
    Found linux image: /boot/vmlinuz-3.13.0-34-generic
    Found initrd image: /boot/initrd.img-3.13.0-34-generic
    Found linux image: /boot/vmlinuz-3.13.0-32-generic
    Found initrd image: /boot/initrd.img-3.13.0-32-generic
    Found memtest86+ image: /memtest86+.elf
    Found memtest86+ image: /memtest86+.bin
    done

<span data-ttu-id="346d0-180">A Red Hat terjesztési termékcsalád szükséges csak a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="346d0-180">For the Red Hat distribution family, you need only the following command:</span></span>

    echo 'echo noop >/sys/block/sda/queue/scheduler' >> /etc/rc.local

## <a name="configure-system-file-operations-settings"></a><span data-ttu-id="346d0-181">Fájl műveletek rendszerbeállításainak konfigurálására</span><span class="sxs-lookup"><span data-stu-id="346d0-181">Configure system file operations settings</span></span>
<span data-ttu-id="346d0-182">Egy bevált gyakorlat az, hogy tiltsa le a *atime* funkciót a fájlrendszeren.</span><span class="sxs-lookup"><span data-stu-id="346d0-182">One best practice is to disable the *atime* logging feature on the file system.</span></span> <span data-ttu-id="346d0-183">Atime az utolsó hozzáférés időpontjának.</span><span class="sxs-lookup"><span data-stu-id="346d0-183">Atime is the last file access time.</span></span> <span data-ttu-id="346d0-184">Amikor egy fájl érhető el, a fájlrendszer a Timestamp típusú rögzíti a naplóban.</span><span class="sxs-lookup"><span data-stu-id="346d0-184">Whenever a file is accessed, the file system records the timestamp in the log.</span></span> <span data-ttu-id="346d0-185">Ezek az információk azonban igen ritkán alkalmazzák.</span><span class="sxs-lookup"><span data-stu-id="346d0-185">However, this information is rarely used.</span></span> <span data-ttu-id="346d0-186">Ha letiltja, ha nincs szüksége, amely csökkenti a teljes lemez hozzáférés idejét.</span><span class="sxs-lookup"><span data-stu-id="346d0-186">You can disable it if you don't need it, which will reduce overall disk access time.</span></span>  

<span data-ttu-id="346d0-187">Atime naplózás letiltásához kell módosítani a fájl rendszer konfigurációs fájl /etc/ fstab, és adja hozzá a **noatime** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="346d0-187">To disable atime logging, you need to modify the file system configuration file /etc/ fstab and add the **noatime** option.</span></span>  

<span data-ttu-id="346d0-188">Például szerkesztése a vim /etc/fstab fájl hozzáadása a noatime a következő mintában látható módon:</span><span class="sxs-lookup"><span data-stu-id="346d0-188">For example, edit the vim /etc/fstab file, adding the noatime as shown in the following sample:</span></span>  

    # CLOUD_IMG: This file was created/modified by the Cloud Image build process
    UUID=3cc98c06-d649-432d-81df-6dcd2a584d41       /        ext4   defaults,discard        0 0
    #Add the “noatime” option below to disable atime logging
    UUID="431b1e78-8226-43ec-9460-514a9adf060e"     /RAID0   xfs   defaults,nobootwait, noatime 0 0
    /dev/sdb1       /mnt    auto    defaults,nobootwait,comment=cloudconfig 0       2

<span data-ttu-id="346d0-189">Ezután csatlakoztassa újra a fájlrendszer, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="346d0-189">Then, remount the file system with the following command:</span></span>  

    mount -o remount /RAID0

<span data-ttu-id="346d0-190">A módosított teszteredmény.</span><span class="sxs-lookup"><span data-stu-id="346d0-190">Test the modified result.</span></span> <span data-ttu-id="346d0-191">Ha módosítja a fájl tesztelése, a hozzáférés időpontja nem frissül.</span><span class="sxs-lookup"><span data-stu-id="346d0-191">When you modify the test file, the access time is not updated.</span></span> <span data-ttu-id="346d0-192">Az alábbi példák bemutatják, mi a kódot a következőképpen néz módosítás előtti és utáni.</span><span class="sxs-lookup"><span data-stu-id="346d0-192">The following examples show what the code looks like before and after modification.</span></span>

<span data-ttu-id="346d0-193">Előtte:</span><span class="sxs-lookup"><span data-stu-id="346d0-193">Before:</span></span>        

![Code access módosítás előtt][5]

<span data-ttu-id="346d0-195">Utána:</span><span class="sxs-lookup"><span data-stu-id="346d0-195">After:</span></span>

![Code access módosítás után][6]

## <a name="increase-the-maximum-number-of-system-handles-for-high-concurrency"></a><span data-ttu-id="346d0-197">Kezeli a nagy egyidejű maximális számának növelése</span><span class="sxs-lookup"><span data-stu-id="346d0-197">Increase the maximum number of system handles for high concurrency</span></span>
<span data-ttu-id="346d0-198">MySQL egy olyan nagy feldolgozási adatbázis.</span><span class="sxs-lookup"><span data-stu-id="346d0-198">MySQL is a high concurrency database.</span></span> <span data-ttu-id="346d0-199">Az alapértelmezett száma párhuzamos leírók érték 1024 Linux, amely nem mindig elegendő.</span><span class="sxs-lookup"><span data-stu-id="346d0-199">The default number of concurrent handles is 1024 for Linux, which is not always sufficient.</span></span> <span data-ttu-id="346d0-200">Az alábbi lépések segítségével növelheti a maximális párhuzamos kezeli a rendszer támogatja a magas egyidejűségi beállítása pedig MySQL.</span><span class="sxs-lookup"><span data-stu-id="346d0-200">Use the following steps to increase the maximum concurrent handles of the system to support high concurrency of MySQL.</span></span>

### <a name="modify-the-limitsconf-file"></a><span data-ttu-id="346d0-201">Módosítsa a limits.conf fájlt.</span><span class="sxs-lookup"><span data-stu-id="346d0-201">Modify the limits.conf file</span></span>
<span data-ttu-id="346d0-202">Növeli a maximális megengedett egyidejű kezeli, vegye fel a következő négy sorokat a /etc/security/limits.conf fájlban.</span><span class="sxs-lookup"><span data-stu-id="346d0-202">To increase the maximum allowed concurrent handles, add the following four lines in the /etc/security/limits.conf file.</span></span> <span data-ttu-id="346d0-203">Vegye figyelembe, hogy a 65536 értékű a rendszer által támogatott maximális számát.</span><span class="sxs-lookup"><span data-stu-id="346d0-203">Note that 65536 is the maximum number that the system can support.</span></span>   

    * <span data-ttu-id="346d0-204">a 65536 értékű enyhe nofile</span><span class="sxs-lookup"><span data-stu-id="346d0-204">soft nofile 65536</span></span>
    * <span data-ttu-id="346d0-205">a 65536 értékű rögzített nofile</span><span class="sxs-lookup"><span data-stu-id="346d0-205">hard nofile 65536</span></span>
    * <span data-ttu-id="346d0-206">a 65536 értékű enyhe nproc</span><span class="sxs-lookup"><span data-stu-id="346d0-206">soft nproc 65536</span></span>
    * <span data-ttu-id="346d0-207">a 65536 értékű rögzített nproc</span><span class="sxs-lookup"><span data-stu-id="346d0-207">hard nproc 65536</span></span>

### <a name="update-the-system-for-the-new-limits"></a><span data-ttu-id="346d0-208">A rendszer az új korlátok frissítésére</span><span class="sxs-lookup"><span data-stu-id="346d0-208">Update the system for the new limits</span></span>
<span data-ttu-id="346d0-209">A rendszer frissítéséhez futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="346d0-209">To update the system, run the following commands:</span></span>  

    ulimit -SHn 65536
    ulimit -SHu 65536

### <a name="ensure-that-the-limits-are-updated-at-boot-time"></a><span data-ttu-id="346d0-210">Győződjön meg arról, hogy a korlátok rendszerindítás frissítése</span><span class="sxs-lookup"><span data-stu-id="346d0-210">Ensure that the limits are updated at boot time</span></span>
<span data-ttu-id="346d0-211">Helyezze el a következő indítási parancsok a /etc/rc.local fájl, a rendszerindítás érvénybe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="346d0-211">Put the following startup commands in the /etc/rc.local file so it will take effect at boot time.</span></span>  

    echo “ulimit -SHn 65536” >>/etc/rc.local
    echo “ulimit -SHu 65536” >>/etc/rc.local

## <a name="mysql-database-optimization"></a><span data-ttu-id="346d0-212">MySQL-adatbázis optimalizálása</span><span class="sxs-lookup"><span data-stu-id="346d0-212">MySQL database optimization</span></span>
<span data-ttu-id="346d0-213">Adja meg a MySQL az Azure-on, használhatja a helyszíni gépen azonos teljesítményének hangolása stratégia is használhat.</span><span class="sxs-lookup"><span data-stu-id="346d0-213">To configure MySQL on Azure, you can use the same performance-tuning strategy you use on an on-premises machine.</span></span>  

<span data-ttu-id="346d0-214">A fő i/o-optimalizálás szabályok a következők:</span><span class="sxs-lookup"><span data-stu-id="346d0-214">The main I/O optimization rules are:</span></span>   

* <span data-ttu-id="346d0-215">Növelje a gyorsítótár méretét.</span><span class="sxs-lookup"><span data-stu-id="346d0-215">Increase the cache size.</span></span>
* <span data-ttu-id="346d0-216">I/o-válaszidő csökkentése.</span><span class="sxs-lookup"><span data-stu-id="346d0-216">Reduce I/O response time.</span></span>  

<span data-ttu-id="346d0-217">MySQL-kiszolgáló beállításainak optimalizálása érdekében frissítheti a my.cnf fájlt, amely az alapértelmezett konfigurációs fájl a kiszolgáló és az ügyfélszámítógépek számára.</span><span class="sxs-lookup"><span data-stu-id="346d0-217">To optimize MySQL server settings, you can update the my.cnf file, which is the default configuration file for both server and client computers.</span></span>  

<span data-ttu-id="346d0-218">A következő konfigurációs elemek olyan a fő MySQL teljesítményét befolyásoló tényezők:</span><span class="sxs-lookup"><span data-stu-id="346d0-218">The following configuration items are the main factors that affect MySQL performance:</span></span>  

* <span data-ttu-id="346d0-219">**innodb_buffer_pool_size**: A pufferkészlet pufferelt adatok és az index tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="346d0-219">**innodb_buffer_pool_size**: The buffer pool contains buffered data and the index.</span></span> <span data-ttu-id="346d0-220">Ez általában 70 %-a fizikai memória van beállítva.</span><span class="sxs-lookup"><span data-stu-id="346d0-220">This is usually set to 70 percent of physical memory.</span></span>
* <span data-ttu-id="346d0-221">**innodb_log_file_size**: visszaállítási napló mérete.</span><span class="sxs-lookup"><span data-stu-id="346d0-221">**innodb_log_file_size**: This is the redo log size.</span></span> <span data-ttu-id="346d0-222">Ismét: naplók segítségével győződjön meg arról, hogy az írási műveletek gyors, megbízható és helyreállítható rendszerösszeomlás után.</span><span class="sxs-lookup"><span data-stu-id="346d0-222">You use redo logs to ensure that write operations are fast, reliable, and recoverable after a crash.</span></span> <span data-ttu-id="346d0-223">Ez 512 MB-ra, így elég hely az írási műveletek naplózása van beállítva.</span><span class="sxs-lookup"><span data-stu-id="346d0-223">This is set to 512 MB, which will give you plenty of space for logging write operations.</span></span>
* <span data-ttu-id="346d0-224">**max_connections**: néha alkalmazások ne zárja be kapcsolatok megfelelően.</span><span class="sxs-lookup"><span data-stu-id="346d0-224">**max_connections**: Sometimes applications do not close connections properly.</span></span> <span data-ttu-id="346d0-225">Nagyobb értéket ad a kiszolgáló több időt idled kapcsolatok újrahasznosítása.</span><span class="sxs-lookup"><span data-stu-id="346d0-225">A larger value will give the server more time to recycle idled connections.</span></span> <span data-ttu-id="346d0-226">Kapcsolatok maximális száma 10 000, de a javasolt legfeljebb 5000.</span><span class="sxs-lookup"><span data-stu-id="346d0-226">The maximum number of connections is 10,000, but the recommended maximum is 5,000.</span></span>
* <span data-ttu-id="346d0-227">**Innodb_file_per_table**: Ez a beállítás engedélyezi vagy letiltja a táblák külön fájlban tárolhatja InnoDB képességét.</span><span class="sxs-lookup"><span data-stu-id="346d0-227">**Innodb_file_per_table**: This setting enables or disables the ability of InnoDB to store tables in separate files.</span></span> <span data-ttu-id="346d0-228">Jelölje be a jelölőnégyzetet annak érdekében, hogy számos speciális felügyeleti műveletek hatékonyan alkalmazható.</span><span class="sxs-lookup"><span data-stu-id="346d0-228">Turn on the option to ensure that several advanced administration operations can be applied efficiently.</span></span> <span data-ttu-id="346d0-229">A teljesítmény szempontjából ez felgyorsíthatja az a tábla terület átviteli és tekintetében felügyeleti teljesítményének optimalizálásához.</span><span class="sxs-lookup"><span data-stu-id="346d0-229">From a performance point of view, it can speed up the table space transmission and optimize the debris management performance.</span></span> <span data-ttu-id="346d0-230">Ez a beállítás az ajánlott beállítás be Kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="346d0-230">The recommended setting for this option is ON.</span></span></br></br>
<span data-ttu-id="346d0-231">A MySQL 5.6 az alapértelmezett beállítás be Kapcsolva, nincs teendője.</span><span class="sxs-lookup"><span data-stu-id="346d0-231">From MySQL 5.6, the default setting is ON, so no action is required.</span></span> <span data-ttu-id="346d0-232">Régebbi verziók esetében az alapértelmezett beállítás értéke OFF.</span><span class="sxs-lookup"><span data-stu-id="346d0-232">For earlier versions, the default setting is OFF.</span></span> <span data-ttu-id="346d0-233">A úgy kell módosítani, mielőtt adatok betöltése, mert az csak az újonnan létrehozott táblák is érint.</span><span class="sxs-lookup"><span data-stu-id="346d0-233">The setting should be changed before data is loaded, because only newly created tables are affected.</span></span>
* <span data-ttu-id="346d0-234">**innodb_flush_log_at_trx_commit**: az alapértelmezett értéke 1, 0 értékre állítva hatókörű ~ 2.</span><span class="sxs-lookup"><span data-stu-id="346d0-234">**innodb_flush_log_at_trx_commit**: The default value is 1, with the scope set to 0~2.</span></span> <span data-ttu-id="346d0-235">Az alapértelmezett érték a legmegfelelőbb beállítás a különálló MySQL-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="346d0-235">The default value is the most suitable option for standalone MySQL DB.</span></span> <span data-ttu-id="346d0-236">2 lehetővé teszi, hogy a legtöbb adatintegritást és MySQL-fürt főkiszolgálójának alkalmas.</span><span class="sxs-lookup"><span data-stu-id="346d0-236">The setting of 2 enables the most data integrity and is suitable for Master in MySQL Cluster.</span></span> <span data-ttu-id="346d0-237">A 0 érték engedélyezi adatvesztés, ami hatással lehet a megbízhatóság (egyes esetekben nagyobb teljesítményű), és az alárendelt MySQL-fürt alkalmas.</span><span class="sxs-lookup"><span data-stu-id="346d0-237">The setting of 0 allows data loss, which can affect reliability (in some cases with better performance), and is suitable for Slave in MySQL Cluster.</span></span>
* <span data-ttu-id="346d0-238">**Innodb_log_buffer_size**: A napló puffer lehetővé teszi, hogy a tranzakciók anélkül, hogy a napló kiürítése lemezre, a tranzakciók véglegesítése előtt futtatásához.</span><span class="sxs-lookup"><span data-stu-id="346d0-238">**Innodb_log_buffer_size**: The log buffer allows transactions to run without having to flush the log to disk before the transactions commit.</span></span> <span data-ttu-id="346d0-239">Azonban ha nagy bináris objektum vagy szövegmezőt, a gyorsítótár gyorsan fognak használni, és gyakori lemez i/o indul.</span><span class="sxs-lookup"><span data-stu-id="346d0-239">However, if there is large binary object or text field, the cache will be consumed quickly and frequent disk I/O will be triggered.</span></span> <span data-ttu-id="346d0-240">Fontos, hogy jobban a pufferméret növelése, ha Innodb_log_waits állapot változó nem 0.</span><span class="sxs-lookup"><span data-stu-id="346d0-240">It is better increase the buffer size if Innodb_log_waits state variable is not 0.</span></span>
* <span data-ttu-id="346d0-241">**query_cache_size**: A legjobb lehetőség le kell tiltani a kezdettől.</span><span class="sxs-lookup"><span data-stu-id="346d0-241">**query_cache_size**: The best option is to disable it from the outset.</span></span> <span data-ttu-id="346d0-242">Query_cache_size értéke 0 (Ez a MySQL 5.6 az alapbeállítás), és más módszerekkel lekérdezések felgyorsítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="346d0-242">Set query_cache_size to 0 (this is the default setting in MySQL 5.6) and use other methods to speed up queries.</span></span>  

<span data-ttu-id="346d0-243">Lásd: [D függelék](#AppendixD) előtt és után az optimalizálási teljesítmény összehasonlítása.</span><span class="sxs-lookup"><span data-stu-id="346d0-243">See [Appendix D](#AppendixD) for a comparison of performance before and after the optimization.</span></span>

## <a name="turn-on-the-mysql-slow-query-log-for-analyzing-the-performance-bottleneck"></a><span data-ttu-id="346d0-244">A MySQL lassú lekérdezés naplóban elemzése a teljesítménybeli szűk keresztmetszetek bekapcsolása</span><span class="sxs-lookup"><span data-stu-id="346d0-244">Turn on the MySQL slow query log for analyzing the performance bottleneck</span></span>
<span data-ttu-id="346d0-245">A MySQL lassú lekérdezés napló segítségével azonosíthatja a lassú lekérdezések a MySQL.</span><span class="sxs-lookup"><span data-stu-id="346d0-245">The MySQL slow query log can help you identify the slow queries for MySQL.</span></span> <span data-ttu-id="346d0-246">Miután engedélyezte a MySQL lassú lekérdezés napló, használhatja a MySQL-eszközök például **mysqldumpslow** a teljesítménybeli szűk keresztmetszetek azonosításához.</span><span class="sxs-lookup"><span data-stu-id="346d0-246">After enabling the MySQL slow query log, you can use MySQL tools like **mysqldumpslow** to identify the performance bottleneck.</span></span>  

<span data-ttu-id="346d0-247">Alapértelmezés szerint ez nincs engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="346d0-247">By default, this is not enabled.</span></span> <span data-ttu-id="346d0-248">A lassú lekérdezés napló bekapcsolása, előfordulhat, hogy néhány Processzor-erőforrások felhasználását.</span><span class="sxs-lookup"><span data-stu-id="346d0-248">Turning on the slow query log might consume some CPU resources.</span></span> <span data-ttu-id="346d0-249">Javasoljuk, hogy engedélyezze a ideiglenesen szűk keresztmetszetek hibaelhárításhoz.</span><span class="sxs-lookup"><span data-stu-id="346d0-249">We recommend that you enable this temporarily for troubleshooting performance bottlenecks.</span></span> <span data-ttu-id="346d0-250">A lassú lekérdezés napló bekapcsolása:</span><span class="sxs-lookup"><span data-stu-id="346d0-250">To turn on the slow query log:</span></span>

1. <span data-ttu-id="346d0-251">Módosítsa a my.cnf fájlt adja hozzá a következő sorokat a befejezési:</span><span class="sxs-lookup"><span data-stu-id="346d0-251">Modify the my.cnf file by adding the following lines to the end:</span></span>

        long_query_time = 2
        slow_query_log = 1
        slow_query_log_file = /RAID0/mysql/mysql-slow.log

2. <span data-ttu-id="346d0-252">Indítsa újra a MySQL-kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="346d0-252">Restart the MySQL server.</span></span>

        service  mysql  restart

3. <span data-ttu-id="346d0-253">Ellenőrizze, hogy a beállítás használatával tart hatása a **megjelenítése** parancsot.</span><span class="sxs-lookup"><span data-stu-id="346d0-253">Check whether the setting is taking effect by using the **show** command.</span></span>

![Lassú lekérdezés-log ON][7]   

![Lassú lekérdezés-napló eredmények][8]

<span data-ttu-id="346d0-256">Ebben a példában láthatja, hogy a lassú lekérdezés szolgáltatás be lett kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="346d0-256">In this example, you can see that the slow query feature has been turned on.</span></span> <span data-ttu-id="346d0-257">Ezután a **mysqldumpslow** eszköz a szűk keresztmetszetek, és mint indexek hozzáadása a teljesítmény optimalizálása.</span><span class="sxs-lookup"><span data-stu-id="346d0-257">You can then use the **mysqldumpslow** tool to determine performance bottlenecks and optimize performance, such as adding indexes.</span></span>

## <a name="appendices"></a><span data-ttu-id="346d0-258">Mellékletek</span><span class="sxs-lookup"><span data-stu-id="346d0-258">Appendices</span></span>
<span data-ttu-id="346d0-259">Az alábbiakban minta teszt teljesítményadatok előállított célzott laborkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="346d0-259">The following are sample performance test data produced in a targeted lab environment.</span></span> <span data-ttu-id="346d0-260">Hangolási módszerek különböző teljesítménnyel biztosítják a teljesítmény adatok trend általános.</span><span class="sxs-lookup"><span data-stu-id="346d0-260">They provide general background on the performance data trend with different performance tuning approaches.</span></span> <span data-ttu-id="346d0-261">Az eredmények a környezet vagy a termék különböző verziói függően változhat.</span><span class="sxs-lookup"><span data-stu-id="346d0-261">The results might vary under different environment or product versions.</span></span>

### <span data-ttu-id="346d0-262"><a name="AppendixA"></a>A függelék</span><span class="sxs-lookup"><span data-stu-id="346d0-262"><a name="AppendixA"></a>Appendix A</span></span>  
<span data-ttu-id="346d0-263">**Lemez teljesítménye (IOPS) a különböző RAID-szintek**</span><span class="sxs-lookup"><span data-stu-id="346d0-263">**Disk performance (IOPS) with different RAID levels**</span></span>

![Különböző RAID-szintek IOPS lemez][9]

<span data-ttu-id="346d0-265">**Teszt parancsok**</span><span class="sxs-lookup"><span data-stu-id="346d0-265">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=5G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite

> [!NOTE]
> <span data-ttu-id="346d0-266">A szinkronizálás számítási Ez a vizsgálat elérni a RAID felső korlátja 64 szál használja.</span><span class="sxs-lookup"><span data-stu-id="346d0-266">The workload of this test uses 64 threads, trying to reach the upper limit of RAID.</span></span>
>
>

### <span data-ttu-id="346d0-267"><a name="AppendixB"></a>B függelék</span><span class="sxs-lookup"><span data-stu-id="346d0-267"><a name="AppendixB"></a>Appendix B</span></span>  
<span data-ttu-id="346d0-268">**Különböző RAID-szintek MySQL (teljesítmény) teljesítmény összehasonlítása** </span><span class="sxs-lookup"><span data-stu-id="346d0-268">**MySQL performance (throughput) comparison with different RAID levels** </span></span>  
<span data-ttu-id="346d0-269">(XFS fájlrendszer)</span><span class="sxs-lookup"><span data-stu-id="346d0-269">(XFS file system)</span></span>

![Különböző RAID-szintek MySQL teljesítmény összehasonlítása][10]  
![Különböző RAID-szintek MySQL teljesítmény összehasonlítása][11]

<span data-ttu-id="346d0-272">**Teszt parancsok**</span><span class="sxs-lookup"><span data-stu-id="346d0-272">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb

<span data-ttu-id="346d0-273">**Különböző RAID-szintek MySQL (OLTP) teljesítmény összehasonlítása**</span><span class="sxs-lookup"><span data-stu-id="346d0-273">**MySQL performance (OLTP) comparison with different RAID levels**</span></span>  
<span data-ttu-id="346d0-274">![Különböző RAID-szintek MySQL (OLTP) teljesítmény összehasonlítása][12]</span><span class="sxs-lookup"><span data-stu-id="346d0-274">![MySQL performance (OLTP) comparison with different RAID levels][12]</span></span>

<span data-ttu-id="346d0-275">**Teszt parancsok**</span><span class="sxs-lookup"><span data-stu-id="346d0-275">**Test commands**</span></span>

    time sysbench --test=oltp --db-driver=mysql --mysql-user=root --mysql-password=0ps.123  --mysql-table-engine=innodb --mysql-host=127.0.0.1 --mysql-port=3306 --mysql-socket=/var/run/mysqld/mysqld.sock --mysql-db=test --oltp-table-size=1000000 prepare

### <span data-ttu-id="346d0-276"><a name="AppendixC"></a>C függelék</span><span class="sxs-lookup"><span data-stu-id="346d0-276"><a name="AppendixC"></a>Appendix C</span></span>   
<span data-ttu-id="346d0-277">**A különböző adatrészlet mérete a lemez teljesítménye (IOPS) összehasonlítását**</span><span class="sxs-lookup"><span data-stu-id="346d0-277">**Disk performance (IOPS) comparison for different chunk sizes**</span></span>  
<span data-ttu-id="346d0-278">(XFS fájlrendszer)</span><span class="sxs-lookup"><span data-stu-id="346d0-278">(XFS file system)</span></span>

![][13]

<span data-ttu-id="346d0-279">**Teszt parancsok**</span><span class="sxs-lookup"><span data-stu-id="346d0-279">**Test commands**</span></span>  

    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=30G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite
    fio -filename=/path/test -iodepth=64 -ioengine=libaio -direct=1 -rw=randwrite -bs=4k -size=1G -numjobs=64 -runtime=30 -group_reporting -name=test-randwrite  

<span data-ttu-id="346d0-280">A fájl a teszteléshez használt értékek 30 és 1 GB-os, illetve, XFS fájlrendszer a RAID 0 (4 lemezek).</span><span class="sxs-lookup"><span data-stu-id="346d0-280">The file sizes used for this testing are 30 GB and 1 GB, respectively, with RAID 0 (4 disks) XFS file system.</span></span>

### <span data-ttu-id="346d0-281"><a name="AppendixD"></a>D függelék:</span><span class="sxs-lookup"><span data-stu-id="346d0-281"><a name="AppendixD"></a>Appendix D</span></span>  
<span data-ttu-id="346d0-282">**MySQL teljesítmény (teljesítmény) összehasonlítás előtt és után optimalizálása**</span><span class="sxs-lookup"><span data-stu-id="346d0-282">**MySQL performance (throughput) comparison before and after optimization**</span></span>  
<span data-ttu-id="346d0-283">(XFS fájlrendszer)</span><span class="sxs-lookup"><span data-stu-id="346d0-283">(XFS File System)</span></span>

![MySQL teljesítmény (teljesítmény) összehasonlítás előtt és után optimalizálása][14]

<span data-ttu-id="346d0-285">**Teszt parancsok**</span><span class="sxs-lookup"><span data-stu-id="346d0-285">**Test commands**</span></span>

    mysqlslap -p0ps.123 --concurrency=2 --iterations=1 --number-int-cols=10 --number-char-cols=10 -a --auto-generate-sql-guid-primary --number-of-queries=10000 --auto-generate-sql-load-type=write –engine=innodb,misam

<span data-ttu-id="346d0-286">**A konfigurációs beállítás az alapértelmezett és optimalizálás a következőképpen történik:**</span><span class="sxs-lookup"><span data-stu-id="346d0-286">**The configuration setting for default and optimization is as follows:**</span></span>

| <span data-ttu-id="346d0-287">Paraméterek</span><span class="sxs-lookup"><span data-stu-id="346d0-287">Parameters</span></span> | <span data-ttu-id="346d0-288">Alapértelmezett</span><span class="sxs-lookup"><span data-stu-id="346d0-288">Default</span></span> | <span data-ttu-id="346d0-289">Optimalizálás</span><span class="sxs-lookup"><span data-stu-id="346d0-289">Optimization</span></span> |
| --- | --- | --- |
| <span data-ttu-id="346d0-290">**innodb_buffer_pool_size**</span><span class="sxs-lookup"><span data-stu-id="346d0-290">**innodb_buffer_pool_size**</span></span> |<span data-ttu-id="346d0-291">None</span><span class="sxs-lookup"><span data-stu-id="346d0-291">None</span></span> |<span data-ttu-id="346d0-292">7 GB</span><span class="sxs-lookup"><span data-stu-id="346d0-292">7 GB</span></span> |
| <span data-ttu-id="346d0-293">**innodb_log_file_size**</span><span class="sxs-lookup"><span data-stu-id="346d0-293">**innodb_log_file_size**</span></span> |<span data-ttu-id="346d0-294">5 MB</span><span class="sxs-lookup"><span data-stu-id="346d0-294">5 MB</span></span> |<span data-ttu-id="346d0-295">512 MB</span><span class="sxs-lookup"><span data-stu-id="346d0-295">512 MB</span></span> |
| <span data-ttu-id="346d0-296">**max_connections**</span><span class="sxs-lookup"><span data-stu-id="346d0-296">**max_connections**</span></span> |<span data-ttu-id="346d0-297">100</span><span class="sxs-lookup"><span data-stu-id="346d0-297">100</span></span> |<span data-ttu-id="346d0-298">5000</span><span class="sxs-lookup"><span data-stu-id="346d0-298">5000</span></span> |
| <span data-ttu-id="346d0-299">**innodb_file_per_table**</span><span class="sxs-lookup"><span data-stu-id="346d0-299">**innodb_file_per_table**</span></span> |<span data-ttu-id="346d0-300">0</span><span class="sxs-lookup"><span data-stu-id="346d0-300">0</span></span> |<span data-ttu-id="346d0-301">1</span><span class="sxs-lookup"><span data-stu-id="346d0-301">1</span></span> |
| <span data-ttu-id="346d0-302">**innodb_flush_log_at_trx_commit**</span><span class="sxs-lookup"><span data-stu-id="346d0-302">**innodb_flush_log_at_trx_commit**</span></span> |<span data-ttu-id="346d0-303">1</span><span class="sxs-lookup"><span data-stu-id="346d0-303">1</span></span> |<span data-ttu-id="346d0-304">2</span><span class="sxs-lookup"><span data-stu-id="346d0-304">2</span></span> |
| <span data-ttu-id="346d0-305">**innodb_log_buffer_size**</span><span class="sxs-lookup"><span data-stu-id="346d0-305">**innodb_log_buffer_size**</span></span> |<span data-ttu-id="346d0-306">8 MB</span><span class="sxs-lookup"><span data-stu-id="346d0-306">8 MB</span></span> |<span data-ttu-id="346d0-307">128 MB</span><span class="sxs-lookup"><span data-stu-id="346d0-307">128 MB</span></span> |
| <span data-ttu-id="346d0-308">**query_cache_size**</span><span class="sxs-lookup"><span data-stu-id="346d0-308">**query_cache_size**</span></span> |<span data-ttu-id="346d0-309">16 MB</span><span class="sxs-lookup"><span data-stu-id="346d0-309">16 MB</span></span> |<span data-ttu-id="346d0-310">0</span><span class="sxs-lookup"><span data-stu-id="346d0-310">0</span></span> |

<span data-ttu-id="346d0-311">További részletes [optimalizálási konfigurációs paraméterek](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), tekintse meg a [MySQL hivatalos utasításokat](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span><span class="sxs-lookup"><span data-stu-id="346d0-311">For more detailed [optimization configuration parameters](http://dev.mysql.com/doc/refman/5.6/en/innodb-configuration.html), refer to the [MySQL official instructions](http://dev.mysql.com/doc/refman/5.6/en/innodb-parameters.html#sysvar_innodb_flush_method).</span></span>  

  <span data-ttu-id="346d0-312">**Tesztkörnyezet**</span><span class="sxs-lookup"><span data-stu-id="346d0-312">**Test environment**</span></span>  

| <span data-ttu-id="346d0-313">Hardver</span><span class="sxs-lookup"><span data-stu-id="346d0-313">Hardware</span></span> | <span data-ttu-id="346d0-314">Részletek</span><span class="sxs-lookup"><span data-stu-id="346d0-314">Details</span></span> |
| --- | --- |
| <span data-ttu-id="346d0-315">CPU</span><span class="sxs-lookup"><span data-stu-id="346d0-315">CPU</span></span> |<span data-ttu-id="346d0-316">AMD Opteron(tm) processzor 4171 Helykiszolgálójához / 4 magos</span><span class="sxs-lookup"><span data-stu-id="346d0-316">AMD Opteron(tm) Processor 4171 HE/4 cores</span></span> |
| <span data-ttu-id="346d0-317">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="346d0-317">Memory</span></span> |<span data-ttu-id="346d0-318">14 GB</span><span class="sxs-lookup"><span data-stu-id="346d0-318">14 GB</span></span> |
| <span data-ttu-id="346d0-319">Lemez</span><span class="sxs-lookup"><span data-stu-id="346d0-319">Disk</span></span> |<span data-ttu-id="346d0-320">10 GB/lemez</span><span class="sxs-lookup"><span data-stu-id="346d0-320">10 GB/disk</span></span> |
| <span data-ttu-id="346d0-321">Operációs rendszer</span><span class="sxs-lookup"><span data-stu-id="346d0-321">OS</span></span> |<span data-ttu-id="346d0-322">Ubuntu 14.04.1 LTS</span><span class="sxs-lookup"><span data-stu-id="346d0-322">Ubuntu 14.04.1 LTS</span></span> |

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

